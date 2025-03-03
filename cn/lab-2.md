Here is the full Markdown content:

# Juniper VLAN Configuration Explanation

## 1. VLAN Configuration
```shell
set vlans DATA vlan-id 10
set vlans VOICE vlan-id 20

Creates VLAN 10 with the name DATA.

Creates VLAN 20 with the name VOICE.



---

2. Assign VLANs to Physical Interfaces

set interfaces ge-0/0/2 unit 0 family ethernet-switching vlan members DATA

Assigns the physical interface ge-0/0/2 to VLAN 10 (DATA) in the Ethernet-switching family.


set interfaces ge-0/0/5 unit 0 family ethernet-switching vlan members VOICE

Assigns the physical interface ge-0/0/5 to VLAN 20 (VOICE).



---

3. Assign IP Addresses to VLAN Interfaces

set interfaces vlan unit 10 family inet address 72.168.3.1/24

Assigns the IP address 72.168.3.1/24 to the VLAN 10 (DATA) interface.

This is a Layer 3 (L3) configuration, meaning VLAN 10 has an IP gateway.


set interfaces vlan unit 20 family inet address 192.168.4.1/24

Assigns the IP address 192.168.4.1/24 to the VLAN 20 (VOICE) interface.

Provides an L3 gateway for devices in VLAN 20.



---

4. Bind VLANs to VLAN Interfaces

set vlans DATA l3-interface vlan.10
set vlans VOICE l3-interface vlan.20

Binds VLAN 10 to its logical VLAN interface (vlan.10).

Binds VLAN 20 to vlan.20.

Ensures proper L3 communication for VLAN routing.



---

Summary

VLAN 10 (DATA) and VLAN 20 (VOICE) are created.

ge-0/0/2 is assigned to VLAN 10, and ge-0/0/5 is assigned to VLAN 20.

IP addresses are assigned to the VLAN interfaces for L3 routing.

VLANs are linked to their respective VLAN interfaces for proper routing.


This setup allows:

VLAN 10 (DATA) to communicate through 72.168.3.1.

VLAN 20 (VOICE) to use 192.168.4.1 as their gateway.


Let me know if you need further clarification!

Let me know if you need any modifications!

