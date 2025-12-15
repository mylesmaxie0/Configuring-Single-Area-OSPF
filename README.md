# Single-Area OSPF Area 0

## 1. Lab Overview

This lab documents the configuration and verification of **Single-Area OSPF (Area 0)** with the use of **passive interfaces**. The goal is to demonstrate how OSPF advertises networks while selectively suppressing neighbor formation and hello packets on LAN-facing interfaces.


---

## 2. Objectives 
- Configure OSPF using network statements
- Identify which interfaces should be passive and why
- Verify OSPF neighbors, interfaces, and routes
- Troubleshoot common OSPF misconfigurations

---

## 3. Network Topology

<img width="853" height="462" alt="Screenshot 2025-12-15 at 8 46 31 AM" src="https://github.com/user-attachments/assets/8d79da3b-39da-42f7-9c5b-f5f5c98f1c24" />


### Design Notes

- All routers participate in **Area 0**
- Only router-to-router links form OSPF adjacencies
- LAN interfaces are configured as **passive interfaces**

---

## 4. IP Addressing Plan

| Router | Interface | IP Address | Subnet Mask |
|------|----------|------------|-------------|
| R1 | G0/0 | 192.168.1.1 | 255.255.255.0 |
| R1 | G0/1 | 10.0.12.1 | 255.255.255.252 |
| R2 | G0/0 | 10.0.12.2 | 255.255.255.252 |
| R2 | G0/1 | 10.0.23.1 | 255.255.255.252 |
| R3 | G0/0 | 10.0.23.2 | 255.255.255.252 |
| R3 | G0/1 | 192.168.3.1 | 255.255.255.0 |

---

## 5. OSPF Concepts Used in This Lab

### Single-Area OSPF

- All routers are in **Area 0**
- Simplifies design and troubleshooting
- No ABRs or inter-area routing

### Passive Interfaces

A **passive interface**:
- Does **not** send or receive OSPF hello packets
- Does **not** form OSPF neighbor adjacencies
- **Still advertises** the connected network into OSPF

---

## Verify Base Connectivity and IP Addressing
Before enabling OSPF, ensure all interfaces are correctly addressed and operational.

### Actions
- Configure IP addresses on all router interfaces
- Ensure interfaces are administratively enabled

#### Router 1 
<img width="850" height="266" alt="Screenshot 2025-12-15 at 9 00 55 AM" src="https://github.com/user-attachments/assets/658213ac-fefd-49c4-bb87-fa121b273466" />

#### Router 2
<img width="850" height="266" alt="Screenshot 2025-12-15 at 9 04 25 AM" src="https://github.com/user-attachments/assets/b0b5f0c1-99aa-484f-8878-aabd29cc4572" />

#### Router 3
<img width="850" height="266" alt="Screenshot 2025-12-15 at 9 07 06 AM" src="https://github.com/user-attachments/assets/672eaf9b-409c-4d9d-93a1-693f70c6da64" />

## Enabling OSPF Routing Process
OSPF must be enabled locally on each router before networks can be advertised.

### Actions 
- Create OSPF process 1 on each router

### What This Does
- Starts on OSPF routing process
- Allows the router to participate in OSPF
- Does not advertise any networks by itself

#### Router 1
<img width="850" height="266" alt="Screenshot 2025-12-15 at 10 21 17 AM" src="https://github.com/user-attachments/assets/6d684510-f60d-4704-9b27-31d82e4c9fea" />

#### Router 2
<img width="850" height="266" alt="Screenshot 2025-12-15 at 10 24 05 AM" src="https://github.com/user-attachments/assets/b539baa6-0201-4761-a87b-b92092b34b4c" />

#### Router 3
<img width="850" height="266" alt="Screenshot 2025-12-15 at 10 24 50 AM" src="https://github.com/user-attachments/assets/5499da3d-1ced-459a-adb7-c4c240794b0d" />

## Advertise Router-to-Router Links into Area 0
Only router-facing links should form OSPF adjacencies.

### Actions 
- Add network statements for point-to-point links

#### Router 1 Configuration
<img width="850" height="266" alt="Screenshot 2025-12-15 at 10 29 37 AM" src="https://github.com/user-attachments/assets/fd57b11e-fff2-481f-82e8-7851180dd211" />

##### Explanation
- Enables OSPF on the R1–R2 point-to-point link
- 0.0.0.3 matches the /30 subnet

#### Router 2 Configuration
<img width="850" height="266" alt="Screenshot 2025-12-15 at 10 33 36 AM" src="https://github.com/user-attachments/assets/8080d35f-9496-4972-8739-d1625f2f7e0e" />

##### Explanation
- Enables OSPF on both router-facing interfaces
- Allows R2 to form adjacencies with:
- R1 on 10.0.12.0/30
- R3 on 10.0.23.0/30

#### Router 3 Configuration
<img width="850" height="266" alt="Screenshot 2025-12-15 at 10 36 54 AM" src="https://github.com/user-attachments/assets/2e8e6d64-643f-4669-bda3-d15f51f59546" />

##### Explanation
- Enables OSPF on the R3–R2 point-to-point link
- Allows R3 to:
- Discover R2 via OSPF hellos
- Form an adjacency

## Advertise LAN Networks into OSPF
LAN networks must be reachable by other routers, but should not form neighbors.

### Actions
- Add network statements for LAN subnets

### What This Does
- Injects LAN networks into OSPF
- Allows other routers to install routes to these networks
- Does not yet prevent hello packets

#### Router 1 Configuration (LAN: 192.168.1.0/24)
<img width="850" height="266" alt="Screenshot 2025-12-15 at 10 44 33 AM" src="https://github.com/user-attachments/assets/a24a4748-5fa2-48d4-a2b1-e37497ecbfa4" />

##### Explanation
- Advertises the R1 LAN subnet into OSPF Area 0
- Allows all other routers to install a route to 192.168.1.0/24
- OSPF is enabled on the LAN interface, but neighbor behavior is not yet restricted


#### Router 3 Configuration (LAN: 192.168.3.0/24)
<img width="850" height="266" alt="Screenshot 2025-12-15 at 10 47 14 AM" src="https://github.com/user-attachments/assets/bdd06afa-0ff4-4c06-b12b-eda899729c07" />

## Configure Passive Interfaces on LAN Ports
LAN-facing interfaces should not participate in OSPF neighbor discovery.
Passive interfaces suppress hello packets while still allowing route advertisement.

### Actions
- Identify all LAN-facing interfaces
- Mark them as passive

### What This Does
- Stops OSPF hello packets on those interfaces
- Prevents adjacency formation with end devices
- Still advertises the connected network via LSAs

#### Router 1 Configuration (LAN Interface)
<img width="850" height="266" alt="Screenshot 2025-12-15 at 11 00 56 AM" src="https://github.com/user-attachments/assets/62bec40a-55a4-4486-b54c-ccd5e162eb9c" />


##### Explanation
- Suppresses OSPF hello packets on GigabitEthernet0/0
- Prevents OSPF adjacency formation with end devices
- Continues to advertise 192.168.1.0/24 into Area 0


#### Router 3 Configuration (LAN Interface)
<img width="850" height="266" alt="Screenshot 2025-12-15 at 10 58 59 AM" src="https://github.com/user-attachments/assets/5acd94a9-7a15-46a1-8500-272bf3d2b6eb" />

##### Explanation
- Suppresses OSPF hello packets on GigabitEthernet0/1
- Prevents unintended neighbor formation
- Continues to advertise 192.168.3.0/24 into Area 0

## Verify OSPF Neighbor Adjacencies

#### Expected Outcome
- Neighbors appear only on router-to-router links
- No neighbors listed for LAN interfaces

#### Router 1  Neighbor Table

