class: center, middle, title
<br>

# Cumulus Basics

<br>

---

# Configuring the Management Interface

- Eth0 is the managemenet interface

- Eth0 set to DHCP by default

- Modify /etc/network/interfaces *(No need for iface eth0 inet)*

```bash
auto eth0
iface eth0 inet
  address 192.168.0.13/16
  gateway 192.168.0.254
```

- Apply the config

```bash
cumulus@leaf03:~$ sudo ifreload -a
```

---
# Changing the Hostname

- Edit /etc/hostname and modify the hostname

- Assign the new hostname to the 127.0.1.1 address in the /etc/hosts file

```bash
127.0.0.1       localhost
::1             localhost ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters
127.0.1.1  leaf03
127.0.1.1  leaf03
```

- **sudo sysctl kernel.hostname=leaf03** / Or reboot the machine

- Check the hostname with **"hostname –f"**

```bash
cumulus@leaf03:~$ hostname -f
leaf03
```
---

# Set the Time Zone

- Edit the /etc/timezone file

```bash
cumulus@leaf03:~$ cat /etc/timezone
Etc/UTC
```
- Apply the config

```bash
cumulus@leaf03:~$ sudo dpkg-reconfigure --frontend noninteractive tzdata
```

---

# Set NTP

- Add / Remove / Edit servers in /etc/ntp.conf

```bash
cat /etc/ntp.conf | grep server
server 0.cumulusnetworks.pool.ntp.org iburst
server 1.cumulusnetworks.pool.ntp.org iburst
server 2.cumulusnetworks.pool.ntp.org iburst
server 3.cumulusnetworks.pool.ntp.org iburst
```
- Apply config

```bash
cumulus@leaf03:~$ sudo ntpd –q
```

- Check NTP status

```bash
cumulus@leaf03:~$ ntpq -pn
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
+50.116.52.97    192.5.41.41      2 u   22   64  377   29.396  -24.282  47.941
-204.11.201.12   216.218.192.202  2 u   26   64  377   50.852   12.857  76.866
+54.242.183.158  128.10.19.24     2 u    8   64  377   31.029  -54.084  38.964
*198.58.105.63   127.67.113.92    2 u   17   64  377   17.355  -32.409  46.771
```
