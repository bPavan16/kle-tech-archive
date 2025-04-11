# DHCP

## The configuration steps for setting up **DHCP, VLANs, and remote access (SSH/HTTP)** on a Juniper device using **Junos OS**. This setup allows devices connected to `DATA` and `VOICE` VLANs to automatically receive IP addresses from DHCP pools.


```
1.	set system services ssh
2.	set system services web-management http
3.	set interfaces ge-0/0/2 unit 0 family ethernet-switching vlan members DATA
4.	set interfaces ge-0/0/5 unit 0 family ethernet-switching vlan members VOICE
5.	set interfaces vlan unit 10 family inet address 192.168.3.1/24
6.	set interfaces vlan unit 20 family inet address 192.168.4.1/24
7.	set vlans DATA vlan-id 10
8.	set vlans DATA l3-interface vlan.10
9.	set vlans VOICE vlan-id 20
10.	set vlans VOICE l3-interface vlan.20
11.	set system services dhcp pool 192.168.3.0/24 address-range low 192.168.3.2
12.	set system services dhcp pool 192.168.3.0/24 address-range high 192.168.3.10
13.	set system services dhcp pool 192.168.3.0/24 router 192.168.3.1
14.	set system services dhcp pool 192.168.4.0/24 address-range low 192.168.4.2
15.	set system services dhcp pool 192.168.4.0/24 address-range high 192.168.4.10
16.	set system services dhcp pool 192.168.4.0/24 router 192.168.4.1
```


## 🧠 Objective

- Enable **remote management** (SSH & HTTP).
- Assign **access ports** to VLANs (DATA & VOICE).
- Configure **L3 interfaces (SVIs)** for each VLAN.
- Create **DHCP pools** for automatic IP address assignment.

---

## 🔧 Step-by-Step Breakdown

### 🔐 1. Enable SSH
```bash
set system services ssh
```
- Allows secure remote terminal access (CLI) to the device over port 22.
- SSH is essential for configuration and monitoring from remote terminals.

---

### 🌐 2. Enable HTTP Web GUI
```bash
set system services web-management http
```
- Enables **web-based GUI** access to the device.
- Access is typically via `http://<device-ip>` on supported platforms.

---

### 🧷 3 & 4. Assign Switch Ports to VLANs
```bash
set interfaces ge-0/0/2 unit 0 family ethernet-switching vlan members DATA
set interfaces ge-0/0/5 unit 0 family ethernet-switching vlan members VOICE
```
- These commands configure two interfaces as **access ports**:
  - `ge-0/0/2` joins VLAN `DATA`
  - `ge-0/0/5` joins VLAN `VOICE`
- Used for connecting end devices like PCs and IP Phones.

---

### 🌍 5 & 6. Configure Layer 3 Interfaces (SVIs)
```bash
set interfaces vlan unit 10 family inet address 192.168.3.1/24
set interfaces vlan unit 20 family inet address 192.168.4.1/24
```
- Sets up **logical L3 interfaces** for VLANs:
  - `vlan.10` → 192.168.3.1/24 (for DATA)
  - `vlan.20` → 192.168.4.1/24 (for VOICE)
- These act as **default gateways** for hosts in each subnet.

---

### 🆔 7–10. VLAN Definitions and Layer 3 Mapping
```bash
set vlans DATA vlan-id 10
set vlans DATA l3-interface vlan.10

set vlans VOICE vlan-id 20
set vlans VOICE l3-interface vlan.20
```
- Creates the VLANs:
  - `DATA` → VLAN ID 10, linked to `vlan.10`
  - `VOICE` → VLAN ID 20, linked to `vlan.20`
- Maps VLANs to their respective **L3 interfaces** to allow inter-VLAN routing and DHCP gateway access.

---

### 📡 11–13. Configure DHCP Pool for DATA VLAN
```bash
set system services dhcp pool 192.168.3.0/24 address-range low 192.168.3.2
set system services dhcp pool 192.168.3.0/24 address-range high 192.168.3.10
set system services dhcp pool 192.168.3.0/24 router 192.168.3.1
```
- DHCP Server for DATA subnet:
  - Range: `192.168.3.2` to `192.168.3.10`
  - Default Gateway: `192.168.3.1` (the L3 interface)

---

### 📡 14–16. Configure DHCP Pool for VOICE VLAN
```bash
set system services dhcp pool 192.168.4.0/24 address-range low 192.168.4.2
set system services dhcp pool 192.168.4.0/24 address-range high 192.168.4.10
set system services dhcp pool 192.168.4.0/24 router 192.168.4.1
```
- DHCP Server for VOICE subnet:
  - Range: `192.168.4.2` to `192.168.4.10`
  - Default Gateway: `192.168.4.1`

---

## 🧪 Resulting Network Overview

| VLAN | ID | Subnet | Gateway | DHCP Range |
|------|----|--------|---------|------------|
| DATA | 10 | 192.168.3.0/24 | 192.168.3.1 | 192.168.3.2 - 192.168.3.10 |
| VOICE | 20 | 192.168.4.0/24 | 192.168.4.1 | 192.168.4.2 - 192.168.4.10 |

- Devices on `ge-0/0/2` get IPs from `192.168.3.0/24`
- Devices on `ge-0/0/5` get IPs from `192.168.4.0/24`
- Both can be accessed via SSH or HTTP if connected properly.

---

## ✅ Use Case

- **Office Network**: DATA for computers, VOICE for phones.
- DHCP reduces manual IP configuration.
- SSH/HTTP allows centralized and remote configuration management.

---

Would you like me to provide this in a downloadable README format or generate a diagram of the network?
