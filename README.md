# Configuring Single-Area OSPF
## Objective

This lab covers the configuration and verification of Open Shortest Path First (OSPF) in a single-area design. All routers operate in **Area 0**, demonstrating how OSPF forms neighbor adjacencies, builds the link-state database, and exchanges routes. The goal is to provide a clear, hands-on understanding of basic OSPF operation and end-to-end connectivity within a simple multi-router network.

## Topology
<img width="1461" height="725" alt="Screenshot 2025-12-11 at 2 13 53 PM" src="https://github.com/user-attachments/assets/f90cc05f-4a98-418f-826f-a9e7f663a31e" />



## What is OSPF

Open Shortest Path First (OSPF) is a link-state routing protocol used in IP networks to dynamically determine the best path for traffic. It operates within a single autonomous system and uses the Dijkstra Shortest Path First algorithm to calculate optimal routes based on link-state information.

## Configuring Loopback Interfaces

Loopback interfaces are commonly used in OSPF because they provide a **stable and reliable** address for router identification and reachability. Unlike physical interfaces, loopbacks do not go down unless manually shutdown.

Router Loopback IP Addresses

| Router 1 | Router 2 | Router 3 | Router 4 |
| --- | --- | --- | --- |
| 1.1.1.1/32 | 2.2.2.2/32 | 3.3.3.3/32 | 4.4.4.4/32 |

### Router 1 Configuration

<img width="850" height="631" alt="Screenshot 2025-12-11 at 2 30 27 PM" src="https://github.com/user-attachments/assets/e28f7466-0818-4196-a5e2-a76ff22004b7" />

### Router 2 Configuration

<img width="850" height="631" alt="Screenshot 2025-12-11 at 2 39 25 PM" src="https://github.com/user-attachments/assets/6bc8ba16-a54d-4680-a986-8bd7eac83708" />

### Router 3 Configuration

<img width="850" height="631" alt="Screenshot 2025-12-12 at 9 08 36 AM" src="https://github.com/user-attachments/assets/3fc0ac5e-4868-4fee-8c21-56bc742af9ce" />

### Router 4

<img width="850" height="631" alt="Screenshot 2025-12-12 at 9 13 36 AM" src="https://github.com/user-attachments/assets/0515350e-c589-47d3-9694-7393f0e58c9b" />


## Configuring OSPF on Routers

<img width="1461" height="725" alt="Screenshot 2025-12-11 at 2 13 53 PM" src="https://github.com/user-attachments/assets/f90cc05f-4a98-418f-826f-a9e7f663a31e" />

Referring to the topology above, each router’s physical and loopback interfaces must be included in the appropriate OSPF network statements so they participate in the OSPF process. When an interface is activated under OSPF, the router forms adjacencies with neighboring routers, generates Link-State Advertisements (LSAs), and floods those LSAs throughout the area. This allows every router to build a synchronized link-state database and run the SPF algorithm to calculate the optimal paths across the network.

### Router 1

<img width="850" height="631" alt="Screenshot 2025-12-12 at 9 43 53 AM" src="https://github.com/user-attachments/assets/385409da-c6fe-4181-9acf-f27b5443269e" />

### Router 2

<img width="850" height="631" alt="Screenshot 2025-12-12 at 9 52 16 AM" src="https://github.com/user-attachments/assets/1082f9db-9334-49b0-b1ae-776ec3c77617" />

### Router 3

<img width="850" height="631" alt="Screenshot 2025-12-12 at 9 54 59 AM" src="https://github.com/user-attachments/assets/f21559e6-09d3-4862-8c00-135def58d055" />

### Router 4

<img width="850" height="631" alt="Screenshot 2025-12-12 at 9 56 43 AM" src="https://github.com/user-attachments/assets/1b6aebde-cde8-4a32-acaa-08a885062b3b" />

## Checking Routers Neighbor Table and Routing Table

Verifying the neighbor table confirms that OSPF has successfully discovered and formed adjacencies with other routers on the connected networks.
Because OSPF relies on synchronized LSDBs (Link-State Databases) to run SPF, adjacency formation is a critical early step in the OSPF process.

If neighbors are not present in this table, the router cannot exchange LSAs, cannot compute correct routes, and the OSPF domain will not converge properly.

Once adjacency formation and LSA exchange are complete, the router should run SPF and install the best OSPF routes into the routing table and loopback interfaces.
If neighbors appear in the table but routes do not, the issue lies with LSAs, area configuration, filtering, or SPF calculations. The routing table will be the same across all routers since this is single area OSPF.

### Router 1 Neighbor Table and Routing Table

<img width="850" height="631" alt="Screenshot 2025-12-12 at 9 59 56 AM" src="https://github.com/user-attachments/assets/a5beef29-5226-4a5b-9cf3-8dd43f7617f5" />

<img width="850" height="631" alt="Screenshot 2025-12-12 at 10 01 51 AM" src="https://github.com/user-attachments/assets/a4a72e3c-e3eb-4811-9f1e-9f3a728a15d7" />

### Router 2 Neighbor Table

<img width="850" height="631" alt="Screenshot 2025-12-12 at 10 05 37 AM" src="https://github.com/user-attachments/assets/902f0e2e-7dd0-47b7-b2d6-3615114fafdb" />

### Router 3 Neighbor Table

<img width="850" height="631" alt="Screenshot 2025-12-12 at 10 08 37 AM" src="https://github.com/user-attachments/assets/79a00555-1050-4681-a9ba-2a477261e6d0" />

### Router 4 Neighbor Table

<img width="850" height="631" alt="Screenshot 2025-12-12 at 10 09 38 AM" src="https://github.com/user-attachments/assets/2aff40a0-7d0c-4c63-9c2c-c6a2411134de" />

## Test Connectivity

After confirming neighbors and routing tables, the final step is validating end-to-end IP reachability.

Connectivity tests ensure that:

- OSPF adjacencies are functioning
- LSAs are being exchanged
- SPF has calculated valid routes
- Routes are correctly installed and being used for forwarding

Ping test between directly connected network on Router 1 and Router 2

<img width="850" height="451" alt="Screenshot 2025-12-12 at 10 13 23 AM" src="https://github.com/user-attachments/assets/7a4fa248-aa2b-4698-83f1-327aa6968af3" />

Ping test between Router 1 and Router 4 Loopback Interface

<img width="850" height="451" alt="Screenshot 2025-12-12 at 10 15 32 AM" src="https://github.com/user-attachments/assets/d3d72aae-c369-4a25-b891-b52f75306463" />
