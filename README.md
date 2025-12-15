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
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 10 36 PM" src="https://github.com/user-attachments/assets/a399a2e3-98d9-40b3-b8ca-8395cd59cba8" />


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

## Configure a Loopback Interface
Adding loopback interfaces for each router is a good practice in OSPF, mainly because it provides a stable Router ID that doesn’t depend on physical interfaces. 

##### Router 1
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 18 27 PM" src="https://github.com/user-attachments/assets/862e8549-8efc-41ba-b611-d07327c8c3d4" />
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 20 01 PM" src="https://github.com/user-attachments/assets/8ce9f209-e436-4ec1-a708-00af5a9147c7" />

##### Router 2
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 22 05 PM" src="https://github.com/user-attachments/assets/77948030-8e64-4072-aad3-c3964dd2aaa9" />
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 22 59 PM" src="https://github.com/user-attachments/assets/217ac0b9-b38a-48d6-b266-cb072ee78aaf" />

##### Router 3
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 24 38 PM" src="https://github.com/user-attachments/assets/0b5ec54b-6025-4e63-a9bb-340fb7ca69e8" />
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 25 48 PM" src="https://github.com/user-attachments/assets/39be69ca-f97b-46d7-8885-21eba262bee6" />

### Configure OSPF to Use the Loopback as Router ID
OSPF picks the highest IP on an active interface by default if you don’t configure a router ID. Loopbacks ensure it’s stable.

#### Router 1
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 30 46 PM" src="https://github.com/user-attachments/assets/becd99a1-c0f5-4cbe-9627-f36b7a3d3090" />

#### Router 2
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 34 32 PM" src="https://github.com/user-attachments/assets/229130a7-182b-4ad3-8396-8654ee09c897" />

#### Router 3
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 36 25 PM" src="https://github.com/user-attachments/assets/67d9fc08-c369-40d3-a072-574222c4b305" />

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

##### Router 1 Configuration (LAN Interface)
<img width="850" height="266" alt="Screenshot 2025-12-15 at 11 00 56 AM" src="https://github.com/user-attachments/assets/62bec40a-55a4-4486-b54c-ccd5e162eb9c" />


##### Explanation
- Suppresses OSPF hello packets on GigabitEthernet0/0
- Prevents OSPF adjacency formation with end devices
- Continues to advertise 192.168.1.0/24 into Area 0


##### Router 3 Configuration (LAN Interface)
<img width="850" height="266" alt="Screenshot 2025-12-15 at 10 58 59 AM" src="https://github.com/user-attachments/assets/5acd94a9-7a15-46a1-8500-272bf3d2b6eb" />

##### Explanation
- Suppresses OSPF hello packets on GigabitEthernet0/1
- Prevents unintended neighbor formation
- Continues to advertise 192.168.3.0/24 into Area 0

## Verify OSPF Neighbor Adjacencies

#### Expected Outcome
- Neighbors appear only on router-to-router links
- No neighbors listed for LAN interfaces

##### Router 1  Neighbor Table
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 38 03 PM" src="https://github.com/user-attachments/assets/c64a176c-ba3f-4a08-8de6-18953b0a1ad7" />

As shown, Router 1 has form ajacencies with Router 2, which has a Router ID of 2.2.2.2. 

#### Router 2  Neighbor Table
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 48 05 PM" src="https://github.com/user-attachments/assets/bbc8115c-be71-4a0b-aa47-76d56de40b8d" />

Router 2 formed adjacencies with Router 1 (1.1.1.1) and Router 3 (3.3.3.3).

##### Router 3  Neighbor Table
<img width="850" height="266" alt="Screenshot 2025-12-15 at 12 50 59 PM" src="https://github.com/user-attachments/assets/a0cdccac-7c4d-4754-86d5-9701f3ab03cd" />


Router 3 formed adjacencies with Router 2 (2.2.2.2).

## Verify OSPF Routes in the Routing Table

#### What This Confirms
- Routes learned dynamically via OSPF are marked with O
- Administrative Distance is 110
- Metrics are cumulative OSPF costs
- Remote LAN networks are reachable

##### Router 1 Routing Table
<img width="850" height="329" alt="Screenshot 2025-12-15 at 12 58 49 PM" src="https://github.com/user-attachments/assets/31aad68a-9e2a-41f1-abb2-8a4597637432" />

##### Router 2 Routing Table
<img width="850" height="329" alt="Screenshot 2025-12-15 at 12 58 49 PM" src="https://github.com/user-attachments/assets/8f166e45-2cb3-4631-89f2-ae83c37b4787" />

##### Router 3 Routing Table
<img width="850" height="329" alt="Screenshot 2025-12-15 at 12 58 49 PM" src="https://github.com/user-attachments/assets/60552b51-a5ba-46ea-8d24-852e55bf0ba7" />

## End-to-End Connectivity Verification

#### What This Confirms	
- Full end-to-end routing is functional
- OSPF is successfully providing reachability
- Passive interfaces did not prevent route advertisement

#### To validate correct OSPF operation, initiate a ping test from LAN-A (PC1) to LAN-B (PC2) and verify successful ICMP reachability across the OSPF domain.
<img width="839" height="417" alt="Screenshot 2025-12-15 at 1 10 17 PM" src="https://github.com/user-attachments/assets/702882ba-ba48-4ed0-997f-13b6db6b16c8" />

##### PC1 - LAN A IP Settings
The default gateway on PC1’s network adapter is configured as 192.168.1.1, which directs outbound traffic to Router 1’s LAN interface for routing beyond the local subnet.
<img width="855" height="457" alt="Screenshot 2025-12-15 at 1 17 34 PM" src="https://github.com/user-attachments/assets/5b485ebe-aaff-43ea-ab4a-8342bdeeafed" />

##### PC2 - LAN B IP Settings
The default gateway on PC2’s network adapter is configured as 192.168.3.1, which directs outbound traffic to Router 2’s LAN interface for routing beyond the local subnet.
<img width="861" height="457" alt="Screenshot 2025-12-15 at 1 21 32 PM" src="https://github.com/user-attachments/assets/0d4df2a1-9520-4065-9be8-1f8d73cd1635" />

### Pinging PC1 -> PC2
The initial ping requests may fail while the ARP process resolves the destination MAC address. Once ARP resolution is complete, subsequent ping tests should receive all packets successfully.
<img width="861" height="393" alt="Screenshot 2025-12-15 at 1 25 47 PM" src="https://github.com/user-attachments/assets/2ce6e926-2964-4964-8b16-b507e7e0bddb" />

## Summary
This lab demonstrates the configuration and verification of Single-Area OSPF (Area 0) with the use of passive interfaces. Router-to-router links are enabled for OSPF adjacency formation, while LAN-facing interfaces are configured as passive to prevent unnecessary routing updates. End-to-end connectivity is validated through routing table inspection and ICMP testing between LAN segments.

