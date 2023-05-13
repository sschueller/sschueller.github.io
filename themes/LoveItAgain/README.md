# LoveItAgain Theme (LoveIt Theme Forked) | Hugo

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/thomas-louvigne/LoveItAgain?style=flat-square)](https://github.com/thomas-louvigne/LoveItAgain/releases)
[![Hugo](https://img.shields.io/badge/Hugo-%5E0.62.0-ff4088?style=flat-square&logo=hugo)](https://gohugo.io/)
[![License](https://img.shields.io/github/license/thomas-louvigne/LoveItAgain?style=flat-square)](https://github.com/thomas-louvigne/LoveItAgain/blob/master/LICENSE)

- **LoveItAgain** is a **clean**, **elegant** but **advanced** blog theme for [Hugo](https://gohugo.io/).
- **LoveItAgain** is a fork of [**LoveIt**](https://github.com/dillonzq/LoveIt), an hugo theme from [dillonzq](https://github.com/dillonzq/)
- It is based on the original [LeaveIt Theme](https://github.com/liuzc/LeaveIt) and [KeepIt Theme](https://github.com/Fastbyte01/KeepIt).

![Hugo Theme LoveIt](https://github.com/dillonzq/LoveIt/raw/master/images/Apple-Devices-Preview.png)

## Difference with the original LoveIt theme
- Extra Padding for social links in the footer : MR https://github.com/dillonzq/LoveIt/pull/798
- Remove Algolia search Enging beacause of security reason : MR https://github.com/dillonzq/LoveIt/issues/818
- Add puppet language : MR https://github.com/dillonzq/LoveIt/pull/800
- Add google Ads : MR https://github.com/dillonzq/LoveIt/pull/816
- Update npm package: husky, core-js, browserify & babel
- Add Ukranian translation : MR https://github.com/dillonzq/LoveIt/pull/805
- Update French translation : MR https://github.com/dillonzq/LoveIt/pull/719 and https://github.com/dillonzq/LoveIt/pull/809
- Add Social :
  - Buy Me a Coffee & Leetcode : MR https://github.com/dillonzq/LoveIt/pull/821
  - Tiktok : MR https://github.com/dillonzq/LoveIt/pull/778/
  - Add ko-fi : (Internal MR)
- Bump to 2.2.3 of json5 depedency : MR https://github.com/dillonzq/LoveIt/pull/785
- Update README :
  - Remove Badges (CircleCi / Sonar / Stargazer)
  - Remove Sponsor
  - Add this section to evalute the change
- Update documentation / exempleSite

## TODO List :
- Check all the documentation
- Remove or translate all chiness documentation (i can't do it)
- Change image of the README
- Finish removing all line of code `algolia`
- Test `lunr` search (don't works for me but neither with `LoveIt`)

## [Old : Before fork] [Demo Site](https://hugoloveit.com/)

To see this theme in action, here is a live [demo site](https://hugoloveit.com/) which is rendered with **LoveItAgain** theme.

## Why choose LoveItAgain ?

Compared to the original LeaveIt theme and the KeepIt theme, the LoveItAgain theme has the following modifications.

* Custom **Header**
* Custom **CSS Style**
* A new **home page**, compatible with the latest version of Hugo
* A lot of **style detail adjustments,** including color, font size, margins, code preview style
* More readable **dark mode**
* Some beautiful **CSS animations**
* Easy-to-use and self-expanding **table of contents**
* More **social links**, **share sites** and **comment system**
* **Search** supported by [Lunr.js](https://lunrjs.com/) (Algolia is no more suported for security reason)
* **Copy code** to clipboard with one click
* Extended Markdown syntax for **[Font Awesome](https://fontawesome.com/) icons**
* Extended Markdown syntax for **ruby annotation**
* Extended Markdown syntax for **fraction**
* **Mathematical formula** supported by [KaTeX](https://katex.org/)
* **Diagram syntax** shortcode supported by [mermaid](https://github.com/mermaid-js/mermaid)
* **Interactive data visualization** shortcode supported by [ECharts](https://echarts.apache.org/)
* **Mapbox** shortcode supported by [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js)
* Embedded **music player** supported by [APlayer](https://github.com/MoePlayer/APlayer) and [MetingJS](https://github.com/metowolf/MetingJS)
* **Bilibili** player supported
* Kinds of **admonitions** shortcode supported
* Custom style shortcodes supported
* **CDN** for all third-party libraries supported
* ...

In short,
if you prefer the design language and freedom of the LoveItAgain theme,
if you want to use the extended Font Awesome icons conveniently,
if you want to embed mathematical formulas, flowcharts, music or Bilibili videos in your posts,
the LoveItAgain theme may be more suitable for you.

I hope you will LoveItAgain ❤️!

## Features

### Performance and SEO

* Optimized for **performance**: 99/100 on mobile and 100/100 on desktop in [Google PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights)
* Optimized SEO performance with a correct **SEO SCHEMA** based on JSON-LD
* **[Google Analytics](https://analytics.google.com/analytics)** supported
* **[Fathom Analytics](https://usefathom.com/)** supported
* **[Plausible Analytics](https://plausible.io/)** supported
* **[Yandex Metrica](https://metrica.yandex.com/)** supported
* Search engine **verification** supported (Google, Bind, Yandex and Baidu)
* **CDN** for third-party libraries supported
* Automatically converted images with **Lazy Load** by [lazysizes](https://github.com/aFarkas/lazysizes)

### Appearance and Layout

* **Desktop/Mobile Responsive** layout
* **Light/Dark** mode
* Globally consistent **design language**
* **Pagination** supported
* Easy-to-use and self-expanding **table of contents**
* **Multilanguage** supported and i18n ready
* Beautiful **CSS animation**

### Social and Comment Systems

* **Gravatar** supported by [Gravatar](https://gravatar.com)
* Local **Avatar** supported
* Up to **76** social links supported
* Up to **24** share sites supported
* **Disqus** comment system supported by [Disqus](https://disqus.com)
* **Gitalk** comment system supported by [Gitalk](https://github.com/gitalk/gitalk)
* **Valine** comment system supported by [Valine](https://valine.js.org/)
* **Facebook comments** system supported by [Facebook](https://developers.facebook.com/docs/plugins/comments/)
* **Telegram comments** system supported by [Telegram Comments](https://comments.app/)
* **Commento** comment system supported by [Commento](https://commento.io/)
* **utterances** comment system supported by [utterances](https://utteranc.es/)
* **giscus** comment system supported by [giscus](https://giscus.app/)

### Extended Features

* **Search** supported by [Lunr.js](https://lunrjs.com/)
* **Twemoji** supported
* Automatically **highlighting** code
* **Copy code** to clipboard with one click
* **Images gallery** supported by [lightGallery](https://github.com/sachinchoolur/lightgallery)
* Extended Markdown syntax for **[Font Awesome](https://fontawesome.com/) icons**
* Extended Markdown syntax for **ruby annotation**
* Extended Markdown syntax for **fraction**
* **Mathematical formula** supported by [KaTeX](https://katex.org/)
* **Diagrams** shortcode supported by [mermaid](https://github.com/mermaid-js/mermaid)
* **Interactive data visualization** shortcode supported by [ECharts](https://echarts.apache.org/)
* **Mapbox** shortcode supported by [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js)
* **Music player** shortcode supported by [APlayer](https://github.com/MoePlayer/APlayer) and [MetingJS](https://github.com/metowolf/MetingJS)
* **Bilibili player** shortcode
* Kinds of **admonitions** shortcode
* **Custom style** shortcode
* **Custom script** shortcode
* **Animated typing** supported by [TypeIt](https://typeitjs.com/)
* **Cookie consent banner** supported by [cookieconsent](https://github.com/osano/cookieconsent)
* **Person** shortcode
* **Google Ads** shortcode
* ...

## [Documentation](https://hugoloveit.com/categories/documentation/)

Build Documentation Locally:

```bash
hugo server --source=exampleSite
```

## Multilingual and i18n

LoveItAgain supports the following languages:

* English
* Simplified Chinese
* Traditional Chinese
* French
* Polish
* Brazilian Portuguese
* Italian
* Spanish
* German
* Serbian
* Russian
* Romanian
* Vietnamese
* Arabic
* Catalan
* Thai
* Telugu
* Indonesian
* Turkish
* Korean
* Hindi
* Ukrainian

* [Contribute with a new language](https://github.com/thomas-louvigne/LoveItAgain/pulls)

[Languages Compatibility](https://hugoloveit.com/theme-documentation-basics/#language-compatibility)


## Questions, ideas, bugs, pull requests

All feedback is welcome! Head over to the [issue tracker](https://github.com/thomas-louvigne/LoveItAgain/issues).

## License

LoveItAgain is licensed under the **GPL v3** license. Check the [LICENSE file](https://github.com/thomas-louvigne/LoveItAgain/blob/master/LICENSE) for details.

## Special Thanks

Thanks to the authors of following resources included in the theme:

* [normalize.css](https://github.com/necolas/normalize.css)
* [Font Awesome](https://fontawesome.com/)
* [Simple Icons](https://github.com/simple-icons/simple-icons)
* [Animate.css](https://daneden.github.io/animate.css/)
* [Lunr.js](https://lunrjs.com/)
* [lazysizes](https://github.com/aFarkas/lazysizes)
* [object-fit-images](https://github.com/fregante/object-fit-images)
* [Twemoji](https://github.com/twitter/twemoji)
* [emoji-data](https://github.com/iamcal/emoji-data)
* [lightGallery](https://github.com/sachinchoolur/lightgallery)
* [clipboard.js](https://github.com/zenorocha/clipboard.js)
* [Sharer.js](https://github.com/ellisonleao/sharer.js)
* [TypeIt](https://typeitjs.com/)
* [KaTeX](https://katex.org/)
* [mermaid](https://github.com/mermaid-js/mermaid)
* [ECharts](https://echarts.apache.org/)
* [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js)
* [APlayer](https://github.com/MoePlayer/APlayer)
* [MetingJS](https://github.com/metowolf/MetingJS)
* [Gitalk](https://github.com/gitalk/gitalk)
* [Valine](https://valine.js.org/)
* [cookieconsent](https://github.com/osano/cookieconsent)

## Author

[Dillon](https://dillonzq.com)

Forked by [Thomas Louvigne](https://thomas-louvigne.com)
Thanks! ❤️
