# ✈️ Federal Flight Agency - Enterprise Network Architecture

An advanced, highly available enterprise network simulation designed for the **Federal Flight Agency** using Cisco Packet Tracer. This project demonstrates real-world implementation of VLSM, dynamic routing (RIPv2), DHCP Relay, and fault-tolerant backup links.

## 📌 Project Overview
The Federal Flight Agency network is divided into 5 distinct zones, each isolated with specific subnetting rules to optimize traffic and enhance security. The core architecture relies on a centralized HQ router acting as the primary hub, with a fully redundant backbone switch network ensuring 100% uptime for critical departments.

## 🖼️ Network Topology
![Federal Flight Agency Topology](topology.jpg)

## 🛠️ Key Technologies & Protocols Implemented
* **Routing:** RIPv2 (with `no auto-summary`), Static Routing, Floating Static Routing.
* **Subnetting:** Variable Length Subnet Mask (VLSM) based on a `/19` root block.
* **IP Services:** DHCP Server, DHCP Relay Agent (`ip helper-address`), DNS.
* **High Availability:** Redundant LAN backbone with custom Administrative Distance (AD).
* **Optimization:** Route Redistribution (`redistribute static`), `passive-interface` configurations to secure LAN segments.

## 📊 IP Addressing Scheme (VLSM)
The entire network is derived from the root block **`10.1.0.0/19`**. The subnets were allocated systematically based on the department size to prevent IP wastage:

| Department / Zone | Required IPs | Allocated Subnet | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **Command Center (HQ)** | 2000 | `10.1.0.0/21` | `255.255.248.0` | `10.1.0.1` |
| **Simulation Lab** | 1000 | `10.1.8.0/22` | `255.255.252.0` | `10.1.8.1` |
| **Hangar** | 1000 | `10.1.12.0/22` | `255.255.252.0` | `10.1.12.1` |
| **AI Navigation** | 500 | `10.1.16.0/23` | `255.255.254.0` | `10.1.16.1` |
| **Emergency Node** | 100 | `10.1.24.0/25` | `255.255.255.128`| `10.1.24.1` |

## 🚀 Core Architectural Features

### 1. Fault-Tolerant Routing (High Availability)
To ensure continuous communication between the **AI Unit** and **HQ**, a primary serial WAN link is used. However, a **Floating Static Route** is configured with an **Administrative Distance (AD) of 66** via the `Simulation Core (SimCore)` backbone switch. 
* If the primary serial link goes down, the AD 66 route instantly activates, redirecting traffic through the backbone switch with zero downtime.

### 2. Decentralized & Centralized DHCP Services
* **Router-based DHCP:** The AI, Hangar, and Emergency zones utilize their respective local routers as DHCP servers via the `ip dhcp pool` command.
* **DHCP Relay Agent:** The Simulation Lab houses a large number of end devices, requiring a dedicated physical DHCP server. The Sim Router is configured with the `ip helper-address` command to forward broadcast DHCP requests across network boundaries securely.

### 3. Network Optimization
* `no auto-summary` is strictly enforced in RIPv2 to prevent classful routing conflicts across the VLSM design.
* `passive-interface` is applied on all LAN-facing gigabit ports to prevent unnecessary routing broadcasts, thereby saving bandwidth and preventing topology spoofing.

## ⚙️ How to Test the Project
1. Download the `Federal_Flight_Agency.pkt` file from this repository.
2. Open it using **Cisco Packet Tracer**.
3. **Verify DHCP:** Check any PC in the Simulation Lab or Hangar; they should successfully receive an IP via DHCP.
4. **Verify Connectivity:** Ping from the Emergency Node to the Main Web Server in HQ.
5. **Test High Availability:** * Run a continuous ping from the AI router to HQ.
   * Delete the primary red serial cable between them.
   * Notice that the ping continues successfully as the traffic dynamically shifts to the Backbone Switch using the AD 66 route!

---
*Designed and configured by [Asif Bin Mahmood]*
