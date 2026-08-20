# Enterprise Network Design & Configuration

## 📌 Project Overview

This project is a **Cisco Packet Tracer enterprise network simulation** designed to demonstrate the configuration of a small enterprise network with wired and wireless clients.

The network includes an **ISP connection, Edge Router, Core Layer-3 Switch, three Access Switches, Access Points, wired PCs, and wireless devices**.

The project focuses on practical networking concepts including:

* Network topology design
* Router configuration
* Layer-3 switching
* VLAN configuration
* 802.1Q trunking
* Inter-VLAN routing
* DHCP
* Wireless networking
* Default gateways
* IP addressing
* Connectivity testing

---

## 🎯 Project Objectives

The main objectives of this project were to:

1. Design an enterprise-style network in Cisco Packet Tracer.
2. Establish connectivity between the enterprise network and an ISP.
3. Configure an Edge Router and Core Layer-3 Switch.
4. Connect multiple Access Switches to the Core Switch using trunk links.
5. Separate wireless and wired devices using VLANs.
6. Configure DHCP for automatic IP address assignment.
7. Provide inter-VLAN routing through the Core Layer-3 Switch.
8. Configure Access Points for wireless connectivity.
9. Test connectivity between different parts of the network.

---

## 🏗️ Network Topology

The final network follows this structure:

```text
                         PT Cloud
                            |
                       ISP Router
                            |
                       Edge Router
                            |
                     Core L3 Switch
                     /      |      \
                  Trunk    Trunk    Trunk
                   /         |         \
              Switch 1    Switch 2    Switch 3
               /    \       /   \       /    \
             AP     PC     AP    PC    AP     PC
             |      |      |     |     |      |
         Wireless  Wired Wireless Wired Wireless Wired
         VLAN 10  VLAN 20 VLAN 10 VLAN 20 VLAN 10 VLAN 20
```

### Network Flow

```text
Internet/Cloud
      ↓
ISP Router
      ↓
Edge Router
      ↓
Core Layer-3 Switch
      ↓
 ┌───────────────┐
 │               │
VLAN 10         VLAN 20
Wireless        Wired
 │               │
APs              PCs
 │
Wireless Devices
```

---

# 🌐 IP Addressing

## WAN: ISP Router ↔ Edge Router

| Device      | Interface           | IP Address     |
| ----------- | ------------------- | -------------- |
| ISP Router  | GigabitEthernet 0/1 | `198.51.100.1` |
| Edge Router | GigabitEthernet 0/1 | `198.51.100.2` |

The connection uses the `198.51.100.0` network.

---

## Edge Router ↔ Core Layer-3 Switch

| Device         | Interface           | IP Address |
| -------------- | ------------------- | ---------- |
| Edge Router    | GigabitEthernet 0/0 | `10.0.0.1` |
| Core L3 Switch | GigabitEthernet 0/1 | `10.0.0.2` |

The Edge Router and Core Switch use the `10.0.0.0/24` network for their connection.

---

# 🔀 VLAN Configuration

Two VLANs were created to separate wireless and wired devices.

| VLAN    | Name               | Purpose          | Network           | Gateway        |
| ------- | ------------------ | ---------------- | ----------------- | -------------- |
| VLAN 10 | `wireless-devices` | Wireless clients | `192.168.10.0/24` | `192.168.10.1` |
| VLAN 20 | `WIRED`            | Wired clients    | `192.168.20.0/24` | `192.168.20.1` |

### VLAN 10 — Wireless

Wireless devices connect through Access Points and are assigned to **VLAN 10**.

```text
VLAN 10
Network: 192.168.10.0/24
Gateway: 192.168.10.1
```

### VLAN 20 — Wired

Wired PCs are connected to access ports configured for **VLAN 20**.

```text
VLAN 20
Network: 192.168.20.0/24
Gateway: 192.168.20.1
```

---

# 🔗 Trunk Configuration

Each Access Switch has a **single physical connection** to the Core Layer-3 Switch.

The same physical connection carries both VLAN 10 and VLAN 20 using **802.1Q trunking**.

### Core Switch Connections

```text
Core Fa0/1 → Switch 3 Fa0/24
Core Fa0/2 → Switch 1 Fa0/24
Core Fa0/3 → Switch 2 Fa0/24
```

All these links are configured as trunk links.

```text
Core Switch
     |
     | 802.1Q Trunk
     |
Access Switch
```

The trunk carries:

```text
VLAN 10 → Wireless traffic
VLAN 20 → Wired traffic
```

This avoids the need for separate physical cables for each VLAN.

---

# 📡 Wireless Network

Three Access Points are connected to the Access Switches.

The Access Point ports are configured as access ports belonging to **VLAN 10**.

Example:

```text
interface fastEthernet 0/10
switchport mode access
switchport access vlan 10
no shutdown
```

Wireless devices connect to the Access Points and receive their network configuration through DHCP.

---

# 🖥️ Wired Network

Wired PCs are connected to unused FastEthernet ports on the Access Switches.

The ports connected to wired PCs are configured as access ports in **VLAN 20**.

Example:

```text
interface fastEthernet 0/1
switchport mode access
switchport access vlan 20
no shutdown
```

---

# 🧩 DHCP Configuration

The Core Layer-3 Switch acts as the **DHCP server**.

This allows both wireless and wired devices to automatically obtain their network configuration.

## Wireless DHCP Pool

```text
ip dhcp excluded-address 192.168.10.1 192.168.10.20

ip dhcp pool WIRELESS
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
```

Wireless devices receive addresses from:

```text
192.168.10.0/24
```

---

## Wired DHCP Pool

```text
ip dhcp excluded-address 192.168.20.1 192.168.20.20

ip dhcp pool WIRED
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 8.8.8.8
```

Wired devices receive addresses from:

```text
192.168.20.0/24
```

---

# 🚦 Inter-VLAN Routing

The Core Layer-3 Switch performs routing between VLANs using Switch Virtual Interfaces (SVIs).

## VLAN 10 SVI

```text
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown
```

## VLAN 20 SVI

```text
interface vlan 20
ip address 192.168.20.1 255.255.255.0
no shutdown
```

Layer-3 routing is enabled with:

```text
ip routing
```

Therefore, the Core Switch can route traffic between:

```text
192.168.10.0/24
        ↕
192.168.20.0/24
```

---

# ⚙️ Important Interfaces

## Edge Router

```text
GigabitEthernet0/0 → Core L3 Switch
IP: 10.0.0.1

GigabitEthernet0/1 → ISP Router
IP: 198.51.100.2
```

## Core L3 Switch

```text
GigabitEthernet0/1 → Edge Router
IP: 10.0.0.2

FastEthernet0/1 → Switch 3
FastEthernet0/2 → Switch 1
FastEthernet0/3 → Switch 2
```

The three FastEthernet connections are configured as **trunks**.

---

# 🧪 Testing & Verification

The network was tested at multiple levels.

### 1. ISP Router ↔ Edge Router

```text
ping 198.51.100.1
```

Successful ✅

---

### 2. Edge Router ↔ Core Switch

```text
ping 10.0.0.2
```

Successful ✅

---

### 3. Wireless Device ↔ Wireless Gateway

```text
ping 192.168.10.1
```

Successful ✅

---

### 4. Wireless Device ↔ Edge Router

```text
ping 10.0.0.1
```

Successful ✅

---

### 5. Wired Device ↔ Wired Gateway

```text
ping 192.168.20.1
```

Successful ✅

---

### 6. DHCP Verification

Wireless and wired devices successfully received IP addresses automatically through DHCP.

Expected wireless configuration:

```text
IP Address:      192.168.10.x
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

Expected wired configuration:

```text
IP Address:      192.168.20.x
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
```

---

# 🔍 Useful Verification Commands

The following Cisco IOS commands were useful during the project:

### Check interfaces

```text
show ip interface brief
```

### Check VLANs

```text
show vlan brief
```

### Check trunk links

```text
show interfaces trunk
```

### Check DHCP pools

```text
show ip dhcp pool
```

### Check DHCP-assigned addresses

```text
show ip dhcp binding
```

### Check connected Cisco devices

```text
show cdp neighbors
```

### Test connectivity

```text
ping <IP-address>
```

---

# 🛠️ Technologies & Tools

* **Cisco Packet Tracer**
* Cisco IOS
* Routers
* Layer-3 Switch
* Layer-2 Switches
* Access Points
* Wired PCs
* Wireless Devices
* IPv4
* VLAN
* 802.1Q Trunking
* DHCP
* Inter-VLAN Routing
* ICMP/Ping

---

# 📚 Networking Concepts Demonstrated

This project provided practical implementation of:

### VLANs

Logical segmentation of the network into separate broadcast domains.

### Access Ports

Used to connect end devices such as PCs and Access Points to a specific VLAN.

### Trunk Ports

Used between the Core Switch and Access Switches to carry multiple VLANs over a single physical connection.

### Layer-3 Switching

The Core Switch performs routing between VLANs using SVIs.

### DHCP

Automatically provides IP addresses and other network configuration to clients.

### Default Gateway

Allows devices to communicate outside their local subnet.

### Routing

The Edge Router provides connectivity between the internal enterprise network and the ISP network.

### Wireless Networking

Access Points provide wireless connectivity to client devices while placing them into the configured wireless VLAN.

---

# 🎯 Final Result

The completed Packet Tracer project successfully demonstrates an enterprise network where:

```text
                 ISP / Internet
                       |
                  ISP Router
                       |
                  Edge Router
                       |
                Core L3 Switch
                 /          \
           VLAN 10          VLAN 20
          Wireless           Wired
             |                 |
            APs                PCs
             |                 |
      Wireless Devices     Wired Devices
```

Both wired and wireless clients receive their IP configuration automatically through **DHCP**, while the Core Layer-3 Switch handles **VLAN gateways and inter-VLAN routing**.

The network was tested using `ping` and all major connectivity tests were successful. ✅

---

