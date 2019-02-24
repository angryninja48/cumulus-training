class: center, middle, title
<br>

# Configuring Interfaces

<br>
---

# Interface Naming

.center[
<img src="slides/images/interface_names.png" style="alight:middle;width:100%;height:100%;">
]

---

# Configuring Interfaces

- Edit /etc/network/interfaces

```bash
cumulus@leaf03:~$ cat /etc/network/interfaces
auto lo
iface lo inet loopback
auto eth0
iface eth0 inet
  address 192.168.0.13/16
  gateway 192.168.0.254
```

- Define an interface as a stanza

```bash
auto swp1
iface swp1
   link-speed 1000
   link-duplex full
   mtu 9216
```

- With an IP Address

```bash
auto swp1
iface swp1
   address 10.1.1.1/30
```

---

# Apply the new config

| Command          | Action           |
| -------------    |------------------|
| **sudo ifreload -a**             | Restarts all interfaces labeled "Auto". Only disruptive to modified interfaces |
|                                 |       |
| **sudo service networking restart** | Restarts all interfaces labeled "Auto". Disruptive to all interfaces |
|                                 |       |
| **sudo ifup <swpX>**               | Restarts the specific interface. Disruptive to the specific interface  |

---

# Configuring a Bond

- Supports LACP

```bash
auto swp3
iface swp3

auto swp4
iface swp4

auto bond0
iface bond0
  bond-slaves swp3 swp4
```

---

# Bridging

- Cumulus uses "VLAN Aware bridging"

- Bridges = Vlan Groups for layer 2 ports

- Runs a single instance of STP

- Vlans are tagged or untagged

- Edit /etc/network/interfaces

```bash
auto bridge
iface bridge
    bridge-ports swp1 swp2 bond0
    bridge-vids 200
    bridge-vlan-aware yes
```

---

# Typical Vlan Config

### Pruning

- By default the switchport will not prune any vlans and will inherit all Vlans from the bridge

```bash
auto swp1
iface swp1
    bridge-vids 200
```

### Access port
```bash
auto swp2
iface swp2
    bridge-access 200
```
---
### Untagged Vlan

```bash
 auto swp2
 iface swp2
     bridge-pvid 200
     bridge-vids 200
```

### Confiure an SVI
```bash
auto vlan200
iface vlan200
    address 1.1.1.1/24
    vlan-id 200
    vlan-raw-device bridge
```
