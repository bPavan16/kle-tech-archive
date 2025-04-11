# OSPF

## The configuration for **Device 1** and **Device 2**, focusing on setting IP addresses, VLANs, OSPF routing, and Layer 3 interfaces. These configurations are meant for Juniper devices using Junos OS.


### Device 1:
```
Set ip; Set members; Set ip; set ospf; Set l id

1.	set interfaces ge-0/0/0 unit 0 family inet address 10.10.10.1/24
2.	set interfaces ge-0/0/1 unit 0 family inet address 10.10.10.2/24
3.	set interfaces ge-0/0/2 unit 0 family ethernet-switching vlan members VOICE
4.	set interfaces vlan unit 10 family inet address 192.168.1.1/24
5.	set protocols ospf area 0.0.0.10 interface ge-0/0/0.0 metric 5
6.	set protocols ospf area 0.0.0.10 interface ge-0/0/1.0 metric 10
7.	set protocols ospf area 0.0.0.10 interface vlan.10
8.	set vlans VOICE vlan-id 10
9.	set vlans VOICE l3-interface vlan.10
```

### Device 2:

```
1.	set interfaces ge-0/0/0 unit 0 family inet address 10.10.10.8/24
2.	set interfaces ge-0/0/1 unit 0 family inet address 10.10.10.7/24
3.	set interfaces ge-0/0/2 unit 0 family ethernet-switching vlan members DATA
4.	set interfaces vlan unit 20 family inet address 172.16.1.1/24
5.	set protocols ospf area 0.0.0.10 interface ge-0/0/0.0 metric 5
6.	set protocols ospf area 0.0.0.10 interface ge-0/0/1.0 metric 10
7.	set protocols ospf area 0.0.0.10 interface vlan.20
```



---

## 🔧 **Device 1 Configuration Breakdown**

### 🖧 1 & 2. IP Addresses on Physical Interfaces
```bash
set interfaces ge-0/0/0 unit 0 family inet address 10.10.10.1/24
set interfaces ge-0/0/1 unit 0 family inet address 10.10.10.2/24
```
- Assigns IPv4 addresses to the physical interfaces:
  - `ge-0/0/0.0` → 10.10.10.1/24
  - `ge-0/0/1.0` → 10.10.10.2/24

These interfaces are used for point-to-point communication, likely forming OSPF adjacencies.

---

### 🔁 3. VLAN Membership for Access Port
```bash
set interfaces ge-0/0/2 unit 0 family ethernet-switching vlan members VOICE
```
- Configures interface `ge-0/0/2` as an access port in VLAN named `VOICE`.

---

### 🌐 4. L3 VLAN Interface Configuration
```bash
set interfaces vlan unit 10 family inet address 192.168.1.1/24
```
- Creates a Layer 3 interface for VLAN 10.
- The SVI (Switched Virtual Interface) `vlan.10` is assigned the IP 192.168.1.1/24.

---

### 🔁 5-7. OSPF Configuration
```bash
set protocols ospf area 0.0.0.10 interface ge-0/0/0.0 metric 5
set protocols ospf area 0.0.0.10 interface ge-0/0/1.0 metric 10
set protocols ospf area 0.0.0.10 interface vlan.10
```
- Adds interfaces to OSPF Area `0.0.0.10`
- `ge-0/0/0.0` with lower cost (metric 5)
- `ge-0/0/1.0` as backup or less preferred (metric 10)
- `vlan.10` is included for routing traffic to the VOICE subnet.

---

### 🧱 8-9. VLAN Configuration with Routing
```bash
set vlans VOICE vlan-id 10
set vlans VOICE l3-interface vlan.10
```
- Creates VLAN `VOICE` with ID 10.
- Maps the VLAN to its Layer 3 interface `vlan.10`.

---

## 🔧 **Device 2 Configuration Breakdown**

### 🖧 1 & 2. IP Addresses on Physical Interfaces
```bash
set interfaces ge-0/0/0 unit 0 family inet address 10.10.10.8/24
set interfaces ge-0/0/1 unit 0 family inet address 10.10.10.7/24
```
- `ge-0/0/0.0` → 10.10.10.8/24
- `ge-0/0/1.0` → 10.10.10.7/24

Likely peer IPs for Device 1, forming OSPF adjacencies.

---

### 🔁 3. VLAN Membership
```bash
set interfaces ge-0/0/2 unit 0 family ethernet-switching vlan members DATA
```
- Access port on VLAN `DATA`.

---

### 🌐 4. VLAN L3 Interface
```bash
set interfaces vlan unit 20 family inet address 172.16.1.1/24
```
- Assigns an IP to `vlan.20`, acting as gateway for VLAN 20 (DATA network).

---

### 🔁 5-7. OSPF Configuration
```bash
set protocols ospf area 0.0.0.10 interface ge-0/0/0.0 metric 5
set protocols ospf area 0.0.0.10 interface ge-0/0/1.0 metric 10
set protocols ospf area 0.0.0.10 interface vlan.20
```
- Similar to Device 1:
  - Primary and backup interfaces for routing.
  - `vlan.20` is advertised via OSPF.

---

## 🧠 Summary of Key Concepts

| Concept | Device 1 | Device 2 |
|--------|----------|----------|
| **IP Config** | 10.10.10.1, 10.10.10.2, 192.168.1.1 | 10.10.10.8, 10.10.10.7, 172.16.1.1 |
| **VLANs** | VOICE (ID 10) | DATA (ID 20) |
| **OSPF Area** | 0.0.0.10 | 0.0.0.10 |
| **OSPF Metrics** | 5 (primary), 10 (backup) | Same |
| **L3 VLAN Interface** | vlan.10 | vlan.20 |

---

## 🧪 Practical Use Case

This setup supports:
- Inter-VLAN routing via OSPF.
- Redundant routing paths between devices.
- Segmentation using VLANs for VOICE and DATA.

---

