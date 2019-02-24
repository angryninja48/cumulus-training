class: center, middle, title
<br>

# Configuring Routing

<br>
---

# Routing Architecture

- FRR (Free Range Routing) is used on Cumulus Linux to provide the routing protocols needed for dynamic routing.

- FRR is a fork of Quagga

- Zebra os the routing manager.

- switchd is a daemon that runs on all Cumulus Linux switches and is essentially the glue between the hardware and software.


.center[
<img src="slides/images/routing_arch.png" style="alight:bottom;width:60%;height:60%;">
]

---

# Enabling FRR

- FRRouting does not start by default in Cumulus Linux

- Need to enable the appropriate routing protocols (Zebra always needs to be enabled)

- Edit **/etc/frr/daemons** file and enable what is needed.

```bash
cumulus@leaf03:~$ cat /etc/frr/daemons
zebra=yes
bgpd=yes
ospfd=no
ospf6d=no
ripd=no
ripngd=no
isisd=no
pimd=no
ldpd=no
nhrpd=no
eigrpd=no
babeld=no
sharpd=no
pbrd=no
```
---
# Enabling FRR

- Enable and start the frr daemon

```bash
cumulus@leaf03:~$ sudo systemctl enable frr.service
cumulus@leaf03:~$ sudo systemctl start frr.service
```

- Check daemon has started

```bash
cumulus@leaf03:~$ sudo systemctl status frr
● frr.service - FRRouting
   Loaded: loaded (/lib/systemd/system/frr.service; enabled)
   Active: active (running) since Sun 2019-02-24 08:36:17 UTC; 2s ago
```

---
# Configuring Routing

- Using integrated shell (Similar to configuring Cisco)

```bash
cumulus@leaf03:~$ sudo vtysh

Hello, this is FRRouting (version 4.0+cl3u9).
Copyright 1996-2005 Kunihiro Ishiguro, et al.

leaf03#
```

- Configuring BGP

```bash
leaf03# config t
leaf03(config)# router bgp 65000
leaf03(config-router)# neighbor 10.1.1.1 remote-as 65001
leaf03(config-router)# exit
leaf03(config)# exit
leaf03# wr mem
Note: this version of vtysh never writes vtysh.conf
Building Configuration...
Integrated configuration saved to /etc/frr//frr.conf
[OK]
```

---
# Configuring Routing

- Edit the file /etc/frr/frr.conf

```bash
cumulus@leaf03:~$ sudo cat /etc/frr/frr.conf
frr version 4.0+cl3u9
frr defaults datacenter
hostname leaf03
username cumulus nopassword
!
service integrated-vtysh-config
!
log syslog informational
!
router bgp 65000
 neighbor 10.1.1.1 remote-as 65001
!
line vty
!
```

- Reload configuration

```bash
cumulus@leaf03:~$ sudo systemctl reload frr.service
```

---

# BGP Unnumbered

- Is awesome

- Advertises an IPv4 route with an IPv6 next hop using extended next hop encoding - Defined by [RFC 5549](https://tools.ietf.org/html/rfc5549)

- Works by advertising the link-local addressing, Thus IPv6 neighbour discovery must be enabled

```bash
interface swp1
 ipv6 nd ra-interval 5
 no ipv6 nd suppress-ra
!
router bgp 65000
 bgp router-id 10.0.0.21
 neighbor fabric peer-group
 neighbor fabric remote-as external
 neighbor fabric description Internal Fabric Network
 neighbor fabric capability extended-nexthop
 neighbor swp1 interface peer-group fabric
```
