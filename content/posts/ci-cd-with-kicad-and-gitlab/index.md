---
weight: 2
title: "CI/CD with KiCad and Gitlab"
date: 2023-05-12T17:37:16+02:00
lastmod: 2023-05-12T17:37:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: ""
draft: false
enableEmoji: true

featuredImage: "rendered-esp32-base"
resources:
  - name: rendered-esp32-base
    src: render.png
  - name: gitlab-pipeline
    src: gitlab-pipeline.png
  - name: kicad-pref
    src: kicad-pref.png
  - name: kicad-part
    src: kicad-part.png




tags: ["PCB", "KiCad", "EE", "Gitlab", "JLCPCB", "LCSC"]
categories: ["projects"]

toc:
  auto: false
math:
  enable: true


---
(Blender Render of KiCad Exported PCB)


<!--more-->


## Introduction

I make many small projects and usually order PCBs at some place. Generating Gerber files and checking everything is in order before ordering can be tedious. Additionally the more complex the board the more parts I need and most likely I will also need a reference in order to place the parts. 

So I decided to setup a gitlab pipeline to automate the whole process.

After I commit into the master the gitlab pipeline will run tests on the schematic and on the PCB. If those pass it will generate the Gerbers I need for the specific fab I use (JLCPCB in this case) and also generate a BOM which I can use to order parts at LCSC. Additionally it will generate a fancy [KiCad Interactive HTML BOM](https://openscopeproject.org/InteractiveHtmlBomDemo/), position file for JLCPCB (if you want them to do assembly) and a [PDF of the schematic](https://github.com/sschueller/sschueller.github.io/raw/master/content/posts/ci-cd-with-kicad-and-gitlab/shades-schematic-1.pdf)
. All these artifacts are commited back into the master and a version is applied. I also have a CHANGELOG.md which is automatically filled from commit messages (I used conventional commits to format my commit messages)

## Git

In order for this to work you need to keep your KiCad project in a git repository. To do this I use the following ```.gitignore``` (KiCad makes tons of files but you don't need to commit all of them) and file structure.

**Folder Structure**
```bash
├── ci/               # Files specific to CI (colors for PDF etc.)
├── datasheets/       # Data sheets of items I used in this project
├── Fabrication/      # Fabrication output
├── footprints/       # KiCad footprints (only for this project)
├── library/          # KiCad libraries (only for this project)
├── packages3d/       # KiCad 3d packages (only for this project)
├── VERSION.txt       # version (Auto updated)
├── CHANGELOG.md      # changelog (Auto generated)
├── README.md         
├── output.kibot.yaml # KiBot output configuration, gerbers, BOM etc. 
├── test.kibot.yaml   # KiBot Tests
├── .gitignore
└── .gitlab-ci.yml    # Gitlab CI file
```

**.gitignore**
```bash
# For PCBs designed using KiCad: http://www.kicad-pcb.org/
# Format documentation: http://kicad-pcb.org/help/file-formats/

# Temporary files
*.000
*.bak
*.bck
*.kicad_pcb-bak
*.kicad_sch-bak
*.kicad_prl
*.sch-bak
*~
_autosave-*
*.tmp
*-save.pro
*-save.kicad_pcb
fp-info-cache
*-backups/

# Netlist files (exported from Eeschema)
*.net

# Autorouter files (exported from Pcbnew)
*.dsn
*.ses
*.rules

# Exported BOM files
*.xml
*.csv
export-*

# Rendered output
3d/Render/*
```

## KiCad Setup

When adding components, add "LCSC" to overall schematic (BOM export will fail if added to each part separately (join issue)) and the part number in each part to the part in order to use the JLCPCB assembly service. Only parts with LCSC column are exported in the JLCPCB BOMs.

![KiCad Schematic Preferences](kicad-pref)

Now when adding parts fill in the LCSC field.

![KiCad Part Edit](kicad-part)



## GitLab Setup

![](gitlab-pipeline)

I use a general gitlab setup with docker runners and [semantic releases](https://levelup.gitconnected.com/semantic-versioning-and-release-automation-on-gitlab-9ba16af0c21)

### Gitlab CI

.gitlab-ci.yml
```yaml
stages:
  - fetch-version
  - testing
  - gen_fab
  - publish

#https://levelup.gitconnected.com/semantic-versioning-and-release-automation-on-gitlab-9ba16af0c21
fetch-semantic-version:
  image: node:18
  stage: fetch-version
  only:
    refs:
    - master
    - alpha
    - /^(([0-9]+)\.)?([0-9]+)\.x/ # This matches maintenance branches
    - /^([0-9]+)\.([0-9]+)\.([0-9]+)(?:-([0-9A-Za-z-]+(?:\.[0-9A-Za-z-]+)*))?(?:\+[0-9A-Za-z-]+)?$/ # This matches pre-releases
  script:
    - npm install @semantic-release/gitlab @semantic-release/exec @semantic-release/changelog @semantic-release/git -D
    - npx semantic-release --generate-notes false --dry-run
  artifacts:
    paths:
    - VERSION.txt
  tags:
    - docker

generate-non-semantic-version:
  stage: fetch-version
  except:
    refs:
    - master
    - alpha
    - /^(([0-9]+)\.)?([0-9]+)\.x/ # This matches maintenance branches
    - /^([0-9]+)\.([0-9]+)\.([0-9]+)(?:-([0-9A-Za-z-]+(?:\.[0-9A-Za-z-]+)*))?(?:\+[0-9A-Za-z-]+)?$/ # This matches pre-releases
  script:
    - echo build-$CI_PIPELINE_ID > VERSION.txt
  artifacts:
    paths:
    - VERSION.txt
  tags:
    - docker

tests:
  image: ghcr.io/inti-cmnb/kicad7_auto:1.6.2
  stage: testing
  script:
    - "[ -f *.kicad_pcb ] && kibot -c test.kibot.yaml"
  tags:
    - docker

pcb_outputs:
  image: ghcr.io/inti-cmnb/kicad7_auto:1.6.2
  stage: gen_fab
  script:
    - "[ -f *.kicad_pcb ] && kibot -c output.kibot.yaml"
  only:
    refs:
    - master
    - alpha
    # This matches maintenance branches
    - /^(([0-9]+)\.)?([0-9]+)\.x/
    # This matches pre-releases
    - /^([0-9]+)\.([0-9]+)\.([0-9]+)(?:-([0-9A-Za-z-]+(?:\.[0-9A-Za-z-]+)*))?(?:\+[0-9A-Za-z-]+)?$/ 
  artifacts:
    when: always
    paths:
      - Fabrication/
  tags:
    - docker

release:
  stage: publish
  image: node:18
  only:
    refs:
    - master
    - alpha
    # This matches maintenance branches
    - /^(([0-9]+)\.)?([0-9]+)\.x/
    # This matches pre-releases
    - /^([0-9]+)\.([0-9]+)\.([0-9]+)(?:-([0-9A-Za-z-]+(?:\.[0-9A-Za-z-]+)*))?(?:\+[0-9A-Za-z-]+)?$/ 
  script:
    - npm install @semantic-release/gitlab @semantic-release/exec @semantic-release/changelog @semantic-release/git -D
    - npx semantic-release
  tags:
    - docker
``` 


### KiBot

[KiBot](https://github.com/INTI-CMNB/KiBot) lets you automate almost anything that KiCad can do. I use it with docker to be able to run it in gitlab.

Here is the test and output configrations I use which are specific for JLCPCB but can be adjusted to almost any fab. There are also many [examples](https://github.com/INTI-CMNB/KiBot/tree/master/docs/samples) on the KiBot github for other fabs.

test.kibot.yaml
```yaml
kibot:
  version: 1

preflight:
  run_erc: true
  update_xml: false
  run_drc: true
  check_zone_fills: true
  ignore_unconnected: false
```

output.kibot.yaml 
```yaml
# Gerber and drill files for JLCPCB, without stencil
# URL: https://jlcpcb.com/
kibot:
  version: 1

globals:
  resources_dir: ci

filters:
  - name: only_jlc_parts
    comment: 'Only parts with JLC (LCSC) code'
    type: generic
    include_only:
      - column: 'LCSC'
        regex: '^C\d+'

variants:
  - name: rotated
    comment: 'Just a place holder for the rotation filter'
    type: kibom
    variant: rotated
    pre_transform: _rot_footprint

# JLCPCB Gerber Output
outputs:
  - name: JLCPCB_gerbers
    comment: Gerbers compatible with JLCPCB
    type: gerber
    dir: JLCPCB
    options: &gerber_options
      exclude_edge_layer: true
      exclude_pads_from_silkscreen: true
      plot_sheet_reference: false
      plot_footprint_refs: true
      plot_footprint_values: false
      force_plot_invisible_refs_vals: false
      tent_vias: true
      use_protel_extensions: true
      create_gerber_job_file: false
      disable_aperture_macros: true
      gerber_precision: 4.6
      use_gerber_x2_attributes: false
      use_gerber_net_attributes: false
      line_width: 0.1
      subtract_mask_from_silk: true
    layers:
      # Note: a more generic approach is to use 'copper' but then the filenames
      # are slightly different.
      - F.Cu
      - B.Cu
      - F.Paste
      - B.Paste
      - F.SilkS
      - B.SilkS
      - F.Mask
      - B.Mask
      - Edge.Cuts

# JLCPCB drill files
  - name: JLCPCB_drill
    comment: Drill files compatible with JLCPCB
    type: excellon
    dir: JLCPCB
    options:
      pth_and_npth_single_file: false
      pth_id: '-PTH'
      npth_id: '-NPTH'
      metric_units: false
      output: "%f%i.%x"

# zip all JLCPCB gerber and drill files together
  - name: JLCPCB
    comment: ZIP file for JLCPCB
    type: compress
    dir: Fabrication/JLCPCB
    options:
      files:
        - from_output: JLCPCB_gerbers
          dest: /
        - from_output: JLCPCB_drill
          dest: /

# html ibom
  - name: ibom
    comment: Interactive BOM
    type: ibom 
    dir: Fabrication/ibom
    options:
      dark_mode: true
      name_format: 'index'

# JLCPCB assembly positions of components
  - name: 'JLCPCB_position'
    comment: "Pick and place file, JLCPCB style"
    type: position
    dir: Fabrication/JLCPCB-BOM
    options:
      variant: rotated
      output: '%f_cpl_jlc.%x'
      format: CSV
      units: millimeters
      separate_files_for_front_and_back: false
      only_smd: true
      columns:
        - id: Ref
          name: Designator
        - Val
        - Package
        - id: PosX
          name: "Mid X"
        - id: PosY
          name: "Mid Y"
        - id: Rot
          name: Rotation
        - id: Side
          name: Layer

# JLCPCB Bom for assembly or for LCSC order
  - name: 'JLCPCB_bom'
    comment: "BoM for JLCPCB"
    type: bom
    dir: Fabrication/JLCPCB-BOM    
    options:
      output: '%f_%i_jlc.%x'
      exclude_filter: 'only_jlc_parts'
      ref_separator: ','
      columns:
        - field: Value
          name: Comment
        - field: References
          name: Designator
        - Footprint
        - field: 'LCSC'
          name: 'LCSC part number'
        - field: 'Quantity Per PCB'
          name: 'QTY'
      csv:
        hide_pcb_info: true
        hide_stats_info: true
        quote_all: true

# PDF of Schematic with dracula theme
  - name: 'SchPrint'
    comment: "Print schematic PDF"
    type: pdf_sch_print
    dir: Fabrication/PDFs
    options:
      color_theme: dracula
      background_color: true

```


## Fabrication

After the pipeline runs successfully I have the following files in my repositories Fabrication folder:

```bash
├── ibom
│   └── myproject-ibom.html      # interactive BOM
├── JLCPCB
│   └── myproject-JLCPCB.zip     # gerbers for PCB order
├── JLCPCB-BOM
│   ├── myproject_bom_jlc.csv    # BOM for assembly or LCDC order
│   └── myproject_cpl_jlc.csv    # Assembly position file
└── PDFs
    └── myproject-schematic.pdf  # PDF of schematic
```

## TODO

- I have not used JLCPCB's assembly service with this export so it may not be correct
- The DRC appears to be using the KiCad defaults and not the options I set in KiCad, I may need to pass the designs rules to KiBot somehow.
- A PDF of the PCB would also be useful but requires defining which layers etc. in the config above.
- I would like the interactive BOM be upload to a gitlab page
- 3d export?

## Eye-Candy

This is an export from KiCad into Blender which textures applied as well as animated. 

{{< youtube svU7NyfJJKY >}}
