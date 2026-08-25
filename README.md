# Networking Lab

A collection of practical **computer networking projects built with Cisco Packet Tracer**. This repository contains hands-on implementations of networking concepts learned through Cisco Networking Basics and further explored through practical network configuration, routing, enterprise networking, and security projects.

The purpose of this repository is to apply networking concepts in realistic simulated environments and develop practical experience with Cisco routers, switches, IP addressing, VLANs, routing, and network security.

---

## Projects

### 1. Inter-VLAN Routing

A VLAN-based network designed to demonstrate communication between different VLANs using Inter-VLAN Routing.

#### Concepts Implemented

* VLAN creation and configuration
* VLAN naming
* Access ports
* Trunk ports
* 802.1Q
* IP addressing
* Default gateways
* Router-on-a-Stick
* Inter-VLAN communication
* Network segmentation
* Connectivity testing

#### Network Segmentation

The network contains multiple VLANs with separate groups of end devices.

| VLAN    | Devices / Purpose     |
| ------- | --------------------- |
| VLAN 10 | PCs                   |
| VLAN 20 | PCs / Laptops         |
| VLAN 30 | PCs / Network devices |

Devices within the same VLAN can communicate at Layer 2, while communication between different VLANs is handled by the router.

#### Verification

Connectivity was tested using `ping` between devices in different VLANs to verify that Inter-VLAN Routing was functioning correctly.

---

### 2. Enterprise Network

A simulated enterprise network demonstrating how an internal organization can connect to an external ISP network through an edge router.

#### Network Components

* Enterprise/Edge Router
* ISP Router
* Cisco Switches
* PCs
* Laptops
* LAN connections
* WAN connection

#### Concepts Implemented

* Enterprise network topology
* LAN and WAN
* Router-to-router communication
* IP addressing
* Edge router configuration
* ISP connectivity
* Default gateway configuration
* WAN addressing
* Network segmentation
* Connectivity testing

#### Simplified Topology

```text
                ISP Network
                     |
                 ISP Router
                     |
                  WAN Link
                     |
                Edge Router
                     |
                Enterprise LAN
                     |
                  Switch
                /   |   \
              PC   PC   PC
```

This project demonstrates the basic architecture of an enterprise network communicating with an ISP through an edge router.

---

### 3. Network Security

A Cisco Packet Tracer project focused on applying basic security practices to network infrastructure.

#### Concepts Implemented

* Cisco device security
* Password configuration
* Privileged EXEC security
* Console access security
* Secure device configuration
* Access control concepts
* Network segmentation
* Basic network hardening

The project demonstrates how network devices can be configured to reduce unauthorized access and improve the security of the network infrastructure.

---

# Skills Demonstrated

These projects provide practical experience with:

### Networking

* LAN and WAN
* Network topologies
* IPv4 addressing
* Subnetting
* Default gateways
* MAC addresses
* ARP
* ICMP
* Ethernet

### Switching

* Cisco switches
* VLANs
* Access ports
* Trunk ports
* 802.1Q
* Broadcast domains
* Network segmentation

### Routing

* Cisco routers
* Inter-VLAN Routing
* Router-on-a-Stick
* Default routing concepts
* Router-to-router communication
* WAN connectivity
* Edge router configuration

### Network Services

* DHCP concepts
* DNS concepts
* HTTP/HTTPS concepts
* FTP concepts
* ICMP

### Security

* Cisco device hardening
* Password protection
* Secure administrative access
* Network segmentation
* Basic access control

### Troubleshooting

* Connectivity testing
* Interface verification
* VLAN verification
* Trunk verification
* Configuration verification
* Packet Tracer simulation and testing

---

# Cisco IOS Commands Practiced

Some of the commands used throughout the projects include:

```text
enable
configure terminal
hostname
interface
ip address
no shutdown
description
vlan
name
switchport mode access
switchport access vlan
switchport mode trunk
encapsulation dot1Q
show running-config
show ip interface brief
show vlan brief
show interfaces trunk
show mac address-table
ping
traceroute
```

---

# Tools

* **Cisco Packet Tracer**
* Cisco IOS CLI
* IPv4 Networking
* Ethernet
* TCP/IP

---

# Learning Foundation

These projects were developed while studying **Cisco Networking Basics**, which provided the foundational knowledge required to understand and configure the networks.

The course covered fundamental concepts such as:

* Network devices
* IP addressing
* Ethernet
* TCP/IP
* OSI model
* Network communication
* Basic Cisco device configuration
* Network troubleshooting

The projects in this repository extend those fundamentals into practical network implementations.

---

# Project Structure

```text
networking-lab/
│
├── inter-vlan-routing/
│   ├── *.pkt
│   └── README.md
│
├── enterprise-network/
│   ├── *.pkt
│   └── README.md
│
├── network-security/
│   ├── *.pkt
│   └── README.md
│
└── README.md
```

Each project contains its Cisco Packet Tracer topology and project-specific documentation.

---

# Future Projects

More networking projects will be added as I continue developing my networking skills.

Planned areas include:

* OSPF
* NAT/PAT
* ACLs
* DHCP Server
* DNS Server
* VPN
* Advanced Network Security
* Larger Enterprise Network Designs

---

# Objective

The objective of this repository is to build practical networking experience by designing, configuring, testing, and troubleshooting network topologies in Cisco Packet Tracer.

It serves as a practical project portfolio demonstrating my progression from **networking fundamentals to routing, enterprise networking, and network security**.
