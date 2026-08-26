# CCNA Lab 03 — Inter-VLAN Routing using Router-on-a-Stick

## 📌 Lab Overview

This lab was performed using **Cisco Packet Tracer** to understand communication between devices located in different VLANs using **Inter-VLAN Routing**.

Two VLANs were created on a Cisco switch:

- VLAN 10 — STUDENTS
- VLAN 20 — STAFF

A Cisco router was configured using the **Router-on-a-Stick** method. Router subinterfaces were created for each VLAN, allowing devices in different VLANs to communicate with each other.

The lab also demonstrates VLAN creation, access port assignment, VLAN separation, trunk configuration, router subinterfaces, inter-VLAN communication, and packet path verification.

---

## 🎯 Objectives

- Create and configure VLANs on a Cisco switch
- Assign switch ports to different VLANs
- Verify communication separation between VLANs
- Configure a trunk connection between the switch and router
- Configure router subinterfaces using Router-on-a-Stick
- Configure Default Gateway for devices in each VLAN
- Verify inter-VLAN communication using `ping`
- Verify the packet path using `tracert`
- Verify router subinterfaces and switch trunk configuration

---

## 🖥️ Network Topology

```text
PC0                    PC1
 |                      |
 | Fa0/1                | Fa0/2
 | VLAN 10              | VLAN 20
 +---------- Switch0 ---+
              |
              | Trunk
              |
           Router1
              |
        GigabitEthernet0/0
           /          \
      G0/0.10        G0/0.20
    VLAN 10 IP      VLAN 20 IP
    192.168.10.1    192.168.20.1

PC0: 192.168.10.10
PC1: 192.168.20.10
```

---

## 🌐 VLAN and IP Addressing

### VLAN 10 — STUDENTS

| Device | Interface | VLAN | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|---|
| PC0 | FastEthernet0 | 10 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| Router1 | GigabitEthernet0/0.10 | 10 | 192.168.10.1 | 255.255.255.0 | — |

### VLAN 20 — STAFF

| Device | Interface | VLAN | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|---|
| PC1 | FastEthernet0 | 20 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| Router1 | GigabitEthernet0/0.20 | 20 | 192.168.20.1 | 255.255.255.0 | — |

---

## 🔀 VLAN Configuration

Two VLANs were created on Switch0:

```text
VLAN 10 — STUDENTS
VLAN 20 — STAFF
```

The PC ports were assigned as access ports:

```text
PC0 → VLAN 10
PC1 → VLAN 20
```

This separates the two PCs into different Layer 2 broadcast domains.

---

## 🚫 VLAN Separation Test

Before configuring Inter-VLAN Routing, communication between PC0 and PC1 was tested.

A ping attempt between the two VLANs failed because:

- PC0 belongs to VLAN 10
- PC1 belongs to VLAN 20
- Different VLANs cannot communicate at Layer 2 without a Layer 3 routing device

This confirms that VLAN segmentation was working correctly.

---

## 🔌 Router-on-a-Stick Configuration

A single physical router interface was connected to the switch using a trunk link.

The physical interface was enabled:

```text
interface gigabitEthernet 0/0
no shutdown
```

Two router subinterfaces were then configured.

### GigabitEthernet0/0.10

```text
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

### GigabitEthernet0/0.20

```text
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

The router subinterfaces act as the Default Gateways for VLAN 10 and VLAN 20.

---

## 🔗 Switch Trunk Configuration

The switch port connected to Router1 was configured as a trunk.

The trunk allows traffic from multiple VLANs to travel over a single physical link.

The trunk was verified using:

```text
show interfaces trunk
```

The verification confirmed that VLAN 10 and VLAN 20 were being carried through the trunk connection.

---

## 🧪 Inter-VLAN Routing Test

After configuring the router subinterfaces and switch trunk, PC0 was able to communicate with PC1.

From PC0:

```text
ping 192.168.20.10
```

The ping was successful, confirming that Router1 was successfully routing traffic between VLAN 10 and VLAN 20.

Communication path:

```text
PC0 (VLAN 10)
      |
      v
   Switch0
      |
      | Trunk (VLAN 10 tagged)
      v
   Router1
   G0/0.10
      |
      | Routing
      v
   G0/0.20
      |
      | Trunk (VLAN 20 tagged)
      v
   Switch0
      |
      v
PC1 (VLAN 20)
```

---

## 🛣️ Path Verification

The packet path was verified from PC0 using:

```text
tracert 192.168.20.10
```

The traceroute confirmed that traffic passed through the router before reaching the destination in VLAN 20.

This demonstrates successful Layer 3 routing between the two VLANs.

---

## 📋 Router Interface Verification

Router subinterfaces were verified using:

```text
show ip interface brief
```

The output confirmed:

```text
GigabitEthernet0/0      → up/up
GigabitEthernet0/0.10   → 192.168.10.1, up/up
GigabitEthernet0/0.20   → 192.168.20.1, up/up
```

Both subinterfaces were operational and configured with the correct IP addresses.

---

## 📸 Lab Evidence

All practical screenshots are stored in the `Screenshot/` directory.

1. **01-lab-topology.png** — Complete network topology
2. **02-vlan-creation-and-port-assignment.png** — VLAN creation and switch port assignment
3. **03-vlan-separation-test.png** — Communication failure between VLANs before routing
4. **04-router-subinterface-configuration.png** — Router-on-a-Stick subinterface configuration
5. **05-switch-trunk-verification.png** — Switch trunk verification
6. **06-inter-vlan-routing-success.png** — Successful communication between VLAN 10 and VLAN 20
7. **07-tracert-inter-vlan-routing.png** — Packet path verification using traceroute
8. **08-router-subinterface-verification.png** — Router subinterface status verification

---

## 🧠 Key Concepts Learned

- VLAN
- VLAN segmentation
- Access ports
- Trunk ports
- IEEE 802.1Q
- Router-on-a-Stick
- Router subinterfaces
- Inter-VLAN Routing
- Default Gateway
- Layer 2 and Layer 3 communication
- IPv4 addressing
- ICMP Ping
- Traceroute
- VLAN isolation

---

## 🛠️ Software Used

- Cisco Packet Tracer

---

## 📁 Lab Files

```text
Lab-03-Inter-VLAN-Routing/
│
├── LAB-03.pkt
├── README.md
│
└── Screenshot/
    ├── 01-lab-topology.png
    ├── 02-vlan-creation-and-port-assignment.png
    ├── 03-vlan-separation-test.png
    ├── 04-router-subinterface-configuration.png
    ├── 05-switch-trunk-verification.png
    ├── 06-inter-vlan-routing-success.png
    ├── 07-tracert-inter-vlan-routing.png
    └── 08-router-subinterface-verification.png
```

---

## ✅ Lab Result

The lab successfully demonstrated **Inter-VLAN Routing using the Router-on-a-Stick method**.

The practical verified:

- VLAN creation
- VLAN port assignment
- VLAN isolation
- Switch trunk configuration
- IEEE 802.1Q encapsulation
- Router subinterface configuration
- Default Gateway operation
- Inter-VLAN communication
- Successful ICMP connectivity
- Packet path verification using `tracert`
- Router interface verification

---

## 👤 Author

**Sagen**
