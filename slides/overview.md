class: center, middle, title
<br>

# Cumulus & Ansible

<br>

Jon Baker

jonbaker85@gmail.com

http://angrynet.ninja

<br>
<br>
---

# Agenda

The primary objective of this session is to give you some basic cumulus and ansible knowledge

- Cumulus Overview

- Cumulus Switch Basics

- Configuring Interfaces

- Configuring Routing (BGP + EVPN)

- Using NCLU

- Driving it with Ansible

---

# Before we begin
### Cumulus in the cloud

- Sign up for a Cumulus Account and confirm email address

- After activation click on “Cumulus in the Cloud” and then “getting started”

- Create a “Blank Slate”

- Wait for provisioning to finish – Takes between 10 - 15mins

---

.center[
<img src="slides/images/citc.png" style="alight:middle;width:770px;height:490px;">
]

---

# Quick Overview

- Cumulus Linux is a network operating system designed to run on over 70+ switch hardware platforms

* Switch platforms include:
  * Edgecore
  * Mellanox (Spectrum)
  * Dell
  * HP
  * Many more


- It is Debian Linux based meaning you get full access to everything that comes with having a full Linux system

- All Linux based tools run natively

---

.center[
<img src="slides/images/v_switch.png" style="alight:middle;width:770px;height:490px;">
]


---

.center[
<img src="slides/images/c_switch.png" style="alight:middle;width:770px;height:490px;">
]
