# Multi-Domain Hybrid Routing Lab (Static, OSPF, & EIGRP)

A GNS3 network topology demonstrating end-to-end enterprise connectivity across three distinct routing domains—**Static Routing**, **OSPF**, and **EIGRP**—integrated via route redistribution and VLAN segmentation[cite: 1].

## 📖 Project Overview

This lab simulates a multi-site corporate network divided into two core regions connected over a central WAN transit link[cite: 1]:
* **Set A (Left Site — Static Routing):** Consists of routers `R1`, `R2`, and `R3` arranged in a triangular full-mesh topology, utilizing static routes for internal reachability[cite: 1].
* **Core Transit (OSPF):** Connects Autonomous System Boundary Routers (ASBRs) `R3` and `R5` across a point-to-point `/30` backbone link running OSPF[cite: 1].
* **Set B (Right Site — Dynamic EIGRP):** Consists of routers `R4`, `R5`, and `R6` in a triangular topology running EIGRP for dynamic internal path selection[cite: 1].

---

## 🗺️ Network Topology

> *Place your topology screenshot in an `images/` directory and link it below:*
> `![Network Topology](images/topology.png)`

### Architecture Highlights
* **Routers:** 6 Cisco IOS routers (`R1` – `R6`)[cite: 1]
* **Switches:** 6 EtherSwitch modules (`ESW1` – `ESW6`) providing Layer 2 VLAN access[cite: 1]
* **Endpoints:** 4 Virtual PCs (`PC1` – `PC4`) assigned to dedicated access VLANs[cite: 1]
* **Inter-Router Links:** Point-to-point `/30` subnets from the `10.0.45.0/24` address block[cite: 1]

---

## 📊 IP Addressing & Interface Table

### Point-to-Point Router Links (`/30`)

| Subnet | Router A | Interface | IP Address | Router B | Interface | IP Address | Routing Domain |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `10.0.45.0/30`[cite: 1] | **R1**[cite: 1] | `g2/0`[cite: 1] | `10.0.45.1`[cite: 1] | **R3**[cite: 1] | `g2/0`[cite: 1] | `10.0.45.2`[cite: 1] | Static (Set A)[cite: 1] |
| `10.0.45.4/30`[cite: 1] | **R3**[cite: 1] | `g1/0`[cite: 1] | `10.0.45.5`[cite: 1] | **R2**[cite: 1] | `g2/0`[cite: 1] | `10.0.45.6`[cite: 1] | Static (Set A)[cite: 1] |
| `10.0.45.8/30`[cite: 1] | **R2**[cite: 1] | `g1/0`[cite: 1] | `10.0.45.9`[cite: 1] | **R1**[cite: 1] | `g1/0`[cite: 1] | `10.0.45.10`[cite: 1] | Static (Set A)[cite: 1] |
| `10.0.45.12/30`[cite: 1] | **R5**[cite: 1] | `g1/0`[cite: 1] | `10.0.45.13`[cite: 1] | **R4**[cite: 1] | `g1/0`[cite: 1] | `10.0.45.14`[cite: 1] | EIGRP (Set B)[cite: 1] |
| `10.0.45.16/30`[cite: 1] | **R5**[cite: 1] | `g2/0`[cite: 1] | `10.0.45.17`[cite: 1] | **R6**[cite: 1] | `g2/0`[cite: 1] | `10.0.45.18`[cite: 1] | EIGRP (Set B)[cite: 1] |
| `10.0.45.20/30`[cite: 1] | **R6**[cite: 1] | `g1/0`[cite: 1] | `10.0.45.21`[cite: 1] | **R4**[cite: 1] | `g2/0`[cite: 1] | `10.0.45.22`[cite: 1] | EIGRP (Set B)[cite: 1] |
| `10.0.45.24/30`[cite: 1] | **R3**[cite: 1] | `g6/0`[cite: 1] | `10.0.45.25`[cite: 1] | **R5**[cite: 1] | `g6/0`[cite: 1] | `10.0.45.26`[cite: 1] | OSPF Backbone[cite: 1] |

### LAN Subnets & VLAN Assignments

| Network | VLAN | Gateway Router | Router Interface | Switch | Uplink Port | Access Port | End Host |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `192.168.1.0/24`[cite: 1] | VLAN 10[cite: 1] | **R1**[cite: 1] | `g3/0`[cite: 1] | `ESW5`[cite: 1] | `f1/15`[cite: 1] | — | — |
| `192.168.2.0/24`[cite: 1] | VLAN 20[cite: 1] | **R3**[cite: 1] | `g5/0`[cite: 1] | `ESW2`[cite: 1] | `f1/15`[cite: 1] | `f1/14`[cite: 1] | **PC2**[cite: 1] |
| `192.168.3.0/24`[cite: 1] | VLAN 30[cite: 1] | **R2**[cite: 1] | `g5/0`[cite: 1] | `ESW1`[cite: 1] | `f1/15`[cite: 1] | `f1/14`[cite: 1] | **PC1**[cite: 1] |
| `192.168.4.0/24`[cite: 1] | VLAN 40[cite: 1] | **R5**[cite: 1] | `g5/0`[cite: 1] | `ESW3`[cite: 1] | `f1/15`[cite: 1] | `f1/14`[cite: 1] | **PC3**[cite: 1] |
| `192.168.5.0/24`[cite: 1] | VLAN 50[cite: 1] | **R4**[cite: 1] | `g3/0`[cite: 1] | `ESW6`[cite: 1] | `f1/15`[cite: 1] | — | — |
| `192.168.6.0/24`[cite: 1] | VLAN 60[cite: 1] | **R6**[cite: 1] | `g5/0`[cite: 1] | `ESW4`[cite: 1] | `f1/15`[cite: 1] | `f1/14`[cite: 1] | **PC4** *(VLAN 50)*[cite: 1] |

*(Note: Verify VLAN alignment on the `R6`–`ESW4`–`PC4` segment, as the gateway subnet is labeled `VLAN 60` while `PC4` is labeled `VLAN 50`[cite: 1].)*

---

## ⚙️ Key Technologies & Implementation Details

1. **Static Routing (Set A: `R1`, `R2`, `R3`)**[cite: 1]
   * Configured with explicit next-hop static routes for all `192.168.1.0/24`–`192.168.3.0/24` LANs and point-to-point transit links[cite: 1].
   * `R1` and `R2` use default or summary routes pointing toward `R3` (`10.0.45.2` and `10.0.45.5`) to reach OSPF and EIGRP networks[cite: 1].
2. **OSPF Transit Backbone (`R3` ↔ `R5`)**[cite: 1]
   * Runs single-area OSPF (`Area 0`) across `10.0.45.24/30`[cite: 1].
   * `R3` redistributes static and connected routes into OSPF; `R5` performs mutual redistribution between OSPF and EIGRP.
3. **Dynamic EIGRP (Set B: `R4`, `R5`, `R6`)**[cite: 1]
   * Configured with `no auto-summary` across all internal `10.0.45.12/30`–`10.0.45.20/30` links and local LAN interfaces[cite: 1].
   * `R5` injects external OSPF/Static routes into EIGRP using appropriate seed metrics (`bandwidth`, `delay`, `reliability`, `load`, `MTU`).
4. **VLAN & Trunking Configuration (`ESW1` – `ESW6`)**[cite: 1]
   * Switch uplinks (`f1/15`) to router interfaces (`g3/0` / `g5/0`) operate as 802.1Q trunks (or access ports if routed directly on physical interfaces)[cite: 1].
   * Host-facing ports (`f1/14`) are configured as `switchport mode access` assigned to their respective VLANs[cite: 1].

---

## 🚀 Getting Started

### Prerequisites
* **GNS3** (v2.2 or later)
* **Cisco IOS Image** supporting GigabitEthernet interfaces (e.g., `c7200` or `vIOS-L2/L3`) and EtherSwitch module (`NM-16ESW`)
* **VPCS** (Virtual PC Simulator) for endpoint testing[cite: 1]

### Installation & Setup
1. Clone this repository:
   ```bash
   git clone [https://github.com/](https://github.com/)<your-username>/<repo-name>.git
   cd <repo-name>
