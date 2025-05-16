---
weight: 2
title: "Upgrading my 25gbit internet router to VyOS"
date: 2025-05-16T16:37:16+02:00
lastmod: 2025-05-16T16:37:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: ""
draft: false
enableEmoji: true

featuredImage: "ms-01"


tags: ["VyOS", "router", "firewall", "FTTH", "Network"]
categories: ["projects"]

toc:
  auto: false
math:
  enable: true


resources:
  - name: ai-argument
    src: IMG_20250426_232730_329.jpg
  - name: ms-01
    src: P_20250516_210518_1.jpg
  - name: interfaces
    src: interfaces.png
  - name: vlans
    src: vlans.png

---



It has been a while since I setup my [original router for my 25gbit internet connection](/posts/wiring-a-home-with-fiber). I decided it was time to upgrade but since I have some services running I did not want to be down for too long and decided to purchase some new hardware which would allow me to experiment with [VyOS](https://vyos.io/) without effecting my current setup.

<!--more-->

There has been a lot of wirwin around the [MS-01](https://minisforumpc.eu/en/products/ms-01) since it came out about a year ago so I decided to purchase one. Additionly I purchased a used [Mellanox SFP28](https://www.nvidia.com/en-in/networking/ethernet/connectx-4-lx/) card from aliexpress.

The following post contains specific types of configurations I needed and how I set those up.


{{< admonition about "Init7 Discount" true >}}
If you find this article useful and you are considering [Init7](https://www.init7.net/en/init7-empfehlen/) as your provider you can use my referral code "20700408098" to get CHF 111.- off hardware.
{{< /admonition >}}

## The Plan

My plan was to install proxmox on the MS-01 just like on the "old" server/router and run VyOS on as a VM. Primary reason for this is to make upgrading and backup easier. 

## Interface configuration

### Physical interfaces

![MS-01 Interfaces](interfaces)

I use PCI passthrough to the VyOS VM for the onboard SFP+ and the Mellonox SFP28 PCIe card. One onboard 2.5G is bridged in via proxmox as well as a bridge with not physical NICs to connect VMs running on MS-01 with VyOS.

### VLANs and bridges

![VyOS VLANs](vlans)

The only reason I am even using VLANs is because one of the WANS is located at another part of the house and I did not want to use more than one fiber (would require more SFP+ modules etc.) to get that WAN to the router.

I have several VLANs at this time which are:
 
- 100 LAN
- 150 Swisscom WAN
- 200 DMZ (This one did not exist in the old setup, I used it to connect a trunk between the two instances of proxmox I now have)
- 900 Managment

I decided to use bridges in VyOS to connect VLANs and physical interfaces together as well as apply firewall/routing rules


## VyOS Setup

> Many thanks to [John Howard](https://www.problemofnetwork.com/posts/updating-my-fiber7-vyos-config-to-1dot5/) for the example vyos setup he has posted which got me going in the right direction.

I am new to VyOS so here are a few things that I "learned" setting up my VM. 

{{< admonition info "VyOS Configuration" true >}}
If you get stuck and your configuration looks good yet it doesn't work I highly suggest you reset your vyos. I have had a situation were my configuration commit fine but it didn't work as expected. Only once I reset and tried to load the full configuration I was given an error which lead me to fix my issue.

I also recommend to use the quarterly LTS version and not the rolling release which may have issues.
{{< /admonition >}}


### Initial Setup

I configured the VyOS with 4 sockets and 4 cores as well as 4GB of ram (probably overkill at this time). After booting the live enviroment of the ISO I ran the setup process to install vyos to disk.

In order to make my life easier and not having to type my entire config into the kvm shell I added a `virtiofs` file system. This allows me to edit the file via a SSH session to the proxmox server and then load it into VyOS until I have SSH access directly into the VyOS VM. 

I only need to enter `sudo mount -t virtiofs vyos-config /mnt` in VyOS via the shell and then run `load /mnt/myconfig` in the `configure` mode. 

I also added it to the fstab so it is mounted on reboot:

```
vyos-config /mnt virtiofs rw,relatime 0 0
```


### Interfaces

I re-mapped my interfaces to match the order I wanted and if for any reason VyOS detects them in a different order after an update I am not affected. 

In order for this to work I had to copy out the auto generated `/config/config.boot`, edited the mac addresses, names and then copied it back. After a restart the interfaces were then correctly assigned as I wanted them.

```
interfaces {
    ethernet eth0 {
        hw-id 58:47:ca:xx:xx:xx
        offload {
            gro
            gso
            sg
            tso
        }
    }
    ...
}
...
```


### VLANs / Bridges

I setup several bridges (non VLAN aware) which connected the VLANs together. 

For example the br200 which is my DMZ connected the trunk port going to my "old server" as well as the trunk going to proxmox iteslf on which vyos is running so I can connect service running on there as well.

```shell
interfaces {
    bridge br200 {
        address "10.20.10.1/24"
        description "Bridge for VLAN 200 (DMZ1)"
        ip
        member {
            interface eth4.200 {
            }
            interface eth6.200 {
            }
        }
    }
    ethernet eth4 {
        description "Trunk Port (VLAN 100 & 150)"
        hw-id "50:6b:4b:xx:xx:xx"
        offload {
            gro
            gso
            sg
            tso
        }
        vif 100 {
            description "LAN (VLAN 100)"
        }
        vif 150 {
            description "Swisscom WAN (VLAN 150)"
        }
        vif 200 {
            description "DMZ1 (VLAN 200)"
        }
        vif 900 {
            description "MGMT (MGMT 900)"
        }
    }
    ethernet eth6 {
        description "Proxmox Trunk"
        hw-id "bc:24:11:xx:xx:xx"
        offload {
            gro
            gso
            sg
            tso
        }
        vif 100 {
            description "LAN (VLAN 100)"
        }        
        vif 200 {
            description "DMZ1 (VLAN 200)"
        }
    }
...
```

#### VLANs Routing Issue

Initaliy I ran into an issue where I could not route traffic between for example `eth4.200` and `eth6.200` but traffic did go from either one of those to vyos. After several days arguing with ChatGPT to Deepseek I found my mistake (without any AI being even close) which was my load-balancing rule, it was missing the exclusion for br200...


```shell
set load-balancing wan rule 12 destination address '10.20.10.0/24'
set load-balancing wan rule 12 exclude
set load-balancing wan rule 12 inbound-interface 'br200'
```

![Me after arguing with AIs for days](ai-argument)


### Backup

For backup at this time I have two cronjobs in the VyOS VM to export the structure config and the commands config. These also go into the `virtiofs` mount.

```shell
0 0 * * * /opt/vyatta/bin/vyatta-op-cmd-wrapper show configuration commands > /mnt/backups/config.commands.$(date --iso-8601=seconds)
0 0 * * * cp /config/config.boot /mnt/backups/vyos-config.$(date --iso-8601=seconds)
```

The second export can be loaded into vyos via `load` command

### DDNS

I do not have a fixed IP (althought it doesn't really change very often) so I also send updates to a external DDNS/DNS server of mine.

```shell
set service dns dynamic name dedyn description 'Dynamic dns service'
set service dns dynamic name dedyn username 'myusername'
set service dns dynamic name dedyn password 'mypassword'
set service dns dynamic name dedyn host-name 'myhostname'
set service dns dynamic name dedyn protocol 'dyndns2'
set service dns dynamic name dedyn server 'my ddns server'
set service dns dynamic name dedyn address interface 'eth2'
set service dns dynamic interval 300
```


### DMZ (Hosted sites)

For the services I am hosting directly from my connection like Matrix server and Mastadon I have a DMZ setup. All the services are behind a nginx reverse proxy which also deals with SSL certificates.

I have a NAT rule going to the proxy as well as the approriate firewall settings

```shell
set nat destination rule 10443 description 'HTTPS to Ingress'
set nat destination rule 10443 destination port '443'
set nat destination rule 10443 inbound-interface name 'eth2'
set nat destination rule 10443 protocol 'tcp_udp'
set nat destination rule 10443 translation address '10.20.10.200'
set nat destination rule 10443 translation port '443'
```

#### Special routing

For these sites to work from within my LAN there are two options. 

The first one would be to use Hairpin NAT but unlike [opnsense](https://docs.opnsense.org/manual/how-tos/nat_reflection.html) it does not work as well especially if the port is 443 or 80 as in my case. 

For this reason I decided to use Split DNS which is just defining the interal IP of a service on my local DNS server which I use on all my clients.

So for example:
```shell
set system static-host-mapping host-name mywebsite.com inet '10.20.10.99'
```

This did cause an issue with my gitlab server when using SSH as it would send traffic to my proxy and not my actuall gitlab server. 

To solve this I setup a "SSH Proxy" on my nginx proxy like so:

```shell
stream {
  upstream ssh {
        # the gitlab server
        server 10.20.10.101:1234;
  }
  server {
        # where ssh listens
        listen 1234;
        proxy_pass ssh;
  }
}
```

### WAN failover

I have 3 WANs which I have specific order in which they should be used. The following will use the Init7 primarily and failover to the slower connections in order.

```shell
set load-balancing wan enable-local-traffic
set load-balancing wan flush-connections
set load-balancing wan sticky-connections inbound

# init 7
set load-balancing wan interface-health eth2 nexthop 'dhcp'
set load-balancing wan interface-health eth2 test 1 target '8.8.8.8'
set load-balancing wan interface-health eth2 test 1 type 'ping'

# Swisscom
set load-balancing wan interface-health br150 nexthop 'dhcp'
set load-balancing wan interface-health br150 test 1 target '8.8.8.8'
set load-balancing wan interface-health br150 test 1 type 'ping'

# Yallo
set load-balancing wan interface-health eth5 nexthop 'dhcp'
set load-balancing wan interface-health eth5 test 1 target '192.168.8.1'
set load-balancing wan interface-health eth5 test 1 type 'ping'

# rule for LAN
set load-balancing wan rule 20 failover
set load-balancing wan rule 20 inbound-interface 'br100'
set load-balancing wan rule 20 interface br150 weight '100'
set load-balancing wan rule 20 interface eth2 weight '150'
set load-balancing wan rule 20 interface eth5 weight '50'
set load-balancing wan rule 20 protocol 'all'

# rule for DMZ
set load-balancing wan rule 30 failover
set load-balancing wan rule 30 inbound-interface 'br200'
set load-balancing wan rule 30 interface br150 weight '100'
set load-balancing wan rule 30 interface eth2 weight '150'
set load-balancing wan rule 30 interface eth5 weight '50'
set load-balancing wan rule 30 protocol 'all'

# to prevent local traffic from going through the load-balancer the following are needed
# if these are missing inter brige routing will not work
set load-balancing wan rule 10 destination address '10.10.10.0/24'
set load-balancing wan rule 10 exclude
set load-balancing wan rule 10 inbound-interface 'br100'

set load-balancing wan rule 11 destination address '10.20.10.0/24'
set load-balancing wan rule 11 exclude
set load-balancing wan rule 11 inbound-interface 'br100'

set load-balancing wan rule 12 destination address '10.20.10.0/24'
set load-balancing wan rule 12 exclude
set load-balancing wan rule 12 inbound-interface 'br200'

set load-balancing wan rule 13 destination address '10.99.10.0/24'
set load-balancing wan rule 13 exclude
set load-balancing wan rule 13 inbound-interface 'br900'
```



### Routing / Firewall

As I mentioned above, I used [John's blog](https://www.problemofnetwork.com/posts/updating-my-fiber7-vyos-config-to-1dot5/) as a starting point and added a DMZ zone for my hosted services. I also applied my firewall rules to bridges instead of directly the interfaces. Thank you John for the great write up.



## Wireguard

Standard road-warior setup for laptops and phones as well as some remote fixed locations.

```shell
set interfaces wireguard wg1 address '10.25.30.1/24'
set interfaces wireguard wg1 description 'HomeWireGuard'
set interfaces wireguard wg1 peer mobile_client allowed-ips '10.25.30.4/32'
set interfaces wireguard wg1 peer mobile_client public-key 'clientpubkey'
set interfaces wireguard wg1 port '132456'
set interfaces wireguard wg1 private-key 'myprivkey'
```

#### NAT

```shell
set nat destination rule 21940 description 'WireGuard VPN'
set nat destination rule 21940 destination port '132456'
set nat destination rule 21940 inbound-interface name 'eth2'
set nat destination rule 21940 protocol 'udp'
set nat destination rule 21940 translation address '10.25.30.1'
```

#### Firewall

```shell
set firewall ipv4 name lan-wg-v4 description 'LAN to WG IPv4'
set firewall ipv4 name lan-wg-v4 default-action 'drop'
set firewall ipv4 name lan-wg-v4 default-log
set firewall ipv4 name lan-wg-v4 rule 1 action 'accept'
set firewall ipv4 name lan-wg-v4 rule 1 state 'established'
set firewall ipv4 name lan-wg-v4 rule 1 state 'related'
set firewall ipv4 name lan-wg-v4 rule 2 action 'drop'
set firewall ipv4 name lan-wg-v4 rule 2 state 'invalid'

set firewall ipv4 name local-wg-v4 description 'This Router to wg IPv4'
set firewall ipv4 name local-wg-v4 default-action 'drop'
set firewall ipv4 name local-wg-v4 default-log
set firewall ipv4 name local-wg-v4 rule 1 action 'accept'

set firewall ipv4 name wan-wg-v4 description 'WAN to WG IPv4'
set firewall ipv4 name wan-wg-v4 default-action 'drop'
set firewall ipv4 name wan-wg-v4 default-log
set firewall ipv4 name wan-wg-v4 rule 1 action 'accept'
set firewall ipv4 name wan-wg-v4 rule 1 state 'related'
set firewall ipv4 name wan-wg-v4 rule 1 state 'established'
set firewall ipv4 name wan-wg-v4 rule 2 action 'drop'
set firewall ipv4 name wan-wg-v4 rule 2 state 'invalid'
set firewall ipv4 name wg-lan-v4 rule 10 action 'accept'
set firewall ipv4 name wg-lan-v4 rule 10 description 'wg to server in LAN'
set firewall ipv4 name wg-lan-v4 rule 10 destination group address-group 'SERVER_IN_LAN'
set firewall ipv4 name wg-lan-v4 rule 10 protocol 'tcp_udp'
set firewall ipv4 name wg-lan-v4 rule 10 source group address-group 'WG_SERVER_ACCESS'

set firewall ipv4 name wg-local-v4 description 'WG to This Router IPv4'
set firewall ipv4 name wg-local-v4 default-action 'drop'
set firewall ipv4 name wg-local-v4 default-log
set firewall ipv4 name wg-local-v4 rule 1 action 'accept'
set firewall ipv4 name wg-local-v4 rule 1 state 'established'
set firewall ipv4 name wg-local-v4 rule 1 state 'related'
set firewall ipv4 name wg-local-v4 rule 2 action 'drop'
set firewall ipv4 name wg-local-v4 rule 2 state 'invalid'
set firewall ipv4 name wg-local-v4 rule 3 action 'accept'
set firewall ipv4 name wg-local-v4 rule 3 description 'explicit allow DNS'
set firewall ipv4 name wg-local-v4 rule 3 destination port '53'
set firewall ipv4 name wg-local-v4 rule 3 protocol 'tcp_udp'

set firewall ipv4 name wg-wan-v4 description 'WG to WAN IPv4'
set firewall ipv4 name wg-wan-v4 default-action 'drop'
set firewall ipv4 name wg-wan-v4 default-log
set firewall ipv4 name wg-wan-v4 rule 1 action 'accept'

set firewall zone lan from wg firewall name 'wg-lan-v4'
set firewall zone local from wg firewall name 'wg-local-v4'
set firewall zone wan from wg firewall name 'wg-wan-v4'

set firewall zone wg default-action 'drop'
set firewall zone wg from lan firewall name 'lan-wg-v4'
set firewall zone wg from local firewall name 'local-wg-v4'
set firewall zone wg from wan firewall name 'wan-wg-v4'
set firewall zone wg interface 'wg1'
```


#### Failover rule for traffic going back out
```shell
set load-balancing wan rule 40 failover
set load-balancing wan rule 40 inbound-interface 'wg1'
set load-balancing wan rule 40 interface br150 weight '100'
set load-balancing wan rule 40 interface eth2 weight '150'
set load-balancing wan rule 40 interface eth5 weight '50'
set load-balancing wan rule 40 protocol 'all'
```



### TODO: 

#### Special routing

Like under opnsense I would like to be able to connect to my WAN2 and WAN3 routers even if the current WAN is Init7.

I set the following static routes but it doesn't work. I am still trying to figure out what I am missing as I see the traffic for 192.168.8.1 go out the Init7 Link when it should not.

Yallo Router
```shell
set protocols static route 192.168.8.1/32 interface eth5
```

Swisscom Router
```shell
set protocols static route 192.168.1.1/32 interface br150
```
#### IPv6

At some point...

## Performance

The perfornamce is much better than with opnsense and also a reason why I switched over to VyOS.

This is from my proxy which is running under proxmox on my older server via trunk (SFP28) to the MS-01 routed through VyoS to the internet.

[![Speedtest](https://www.speedtest.net/result/c/f11e8bde-e5a3-4fe7-9c-13-f0ef236d0566.png)](https://www.speedtest.net/result/c/f11e8bde-e5a3-4fe7-9c-13-f0ef236d0566)

