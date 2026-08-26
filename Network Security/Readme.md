Network Security Lab — Cisco Packet Tracer

A Cisco Packet Tracer network security project demonstrating VLAN segmentation, inter-VLAN routing, DHCP, secure remote administration with SSH, switch port security, unused-port hardening, and basic ACL-based traffic filtering.

Project Overview

This project builds a segmented enterprise-style network using one Cisco router and two Cisco switches. The network separates devices into three security zones:

VLAN 10 — ADMIN

VLAN 20 — EMPLOYEE

VLAN 30 — GUESTS

The project focuses on practical Layer 2 and Layer 3 security controls rather than only basic connectivity.

Security objectives

The lab demonstrates:

VLAN-based network segmentation

Inter-VLAN routing using Router-on-a-Stick

DHCP for each VLAN

SSH instead of Telnet for switch management

Local username/password authentication

Port security with sticky MAC addresses

Shutdown of unused switch ports

ACLs that restrict access between security zones

Verification and testing of the implemented controls

Topology

                           +----------------+
                           |      R1        |
                           |  Cisco 2911    |
                           +----------------+
                                  |
                                  | G0/0
                                  | 802.1Q Trunk
                                  |
                           +-------------+
                           |     S1      |
                           |   2960      |
                           +-------------+
                            | | | |  |
                            | | | |  +------ AP1 (VLAN 30)
                            | | | |
                            | | | +--------- PC4 (VLAN 20)
                            | | +----------- PC3 (VLAN 20)
                            | +------------- PC2 (VLAN 10)
                            +--------------- PC1 (VLAN 10)
                                  ||
                                  || Gi0/2 <-> Gi0/2
                                  || 802.1Q Trunk
                                  ||
                           +-------------+
                           |     S2      |
                           |   2960      |
                           +-------------+
                            | | | |  |
                            | | | |  +------ AP2 (VLAN 30)
                            | | | |
                            | | | +--------- PC8 (VLAN 20)
                            | | +----------- PC7 (VLAN 20)
                            | +------------- PC6 (VLAN 10)
                            +--------------- PC5 (VLAN 10)

Physical connections

S1

Interface

Connected device

Purpose

Fa0/1

PC1

VLAN 10 — ADMIN

Fa0/2

PC2

VLAN 10 — ADMIN

Fa0/3

PC3

VLAN 20 — EMPLOYEE

Fa0/4

PC4

VLAN 20 — EMPLOYEE

Fa0/5

AP1

VLAN 30 — GUESTS

Gi0/1

R1 Gi0/0

802.1Q trunk

Gi0/2

S2 Gi0/2

802.1Q trunk

S2

Interface

Connected device

Purpose

Fa0/1

PC5

VLAN 10 — ADMIN

Fa0/2

PC6

VLAN 10 — ADMIN

Fa0/3

PC7

VLAN 20 — EMPLOYEE

Fa0/4

PC8

VLAN 20 — EMPLOYEE

Fa0/5

AP2

VLAN 30 — GUESTS

Gi0/2

S1 Gi0/2

802.1Q trunk

The second router-to-switch link was removed so that the router uses a single Router-on-a-Stick trunk through S1. VLAN traffic reaches S2 over the S1–S2 trunk.

VLAN and IP Addressing Plan

VLAN

Name

Network

Default Gateway

Purpose

10

ADMIN

192.168.10.0/24

192.168.10.1

Administrative devices

20

EMPLOYEE

192.168.20.0/24

192.168.20.1

Employee devices

30

GUESTS

192.168.30.0/24

192.168.30.1

Guest and wireless devices

Device addressing

Client devices use DHCP from R1.

Typical DHCP ranges are:

VLAN 10: 192.168.10.11 onward

VLAN 20: 192.168.20.11 onward

VLAN 30: 192.168.30.11 onward

Gateway addresses are reserved for the router.

Technologies Used

Cisco Packet Tracer

Cisco IOS

Cisco 2911 Router

Cisco 2960 Switches

IEEE 802.1Q VLAN trunking

Router-on-a-Stick

DHCP

SSH v2

Local authentication

Port Security

Sticky MAC addresses

Extended ACLs

Spanning Tree Protocol (PVST)

1. VLAN Segmentation

Three VLANs were created on both switches:

VLAN 10
Name: ADMIN

VLAN 20
Name: EMPLOYEE

VLAN 30
Name: GUESTS

Access-port assignment

S1

Fa0/1-2 → VLAN 10
Fa0/3-4 → VLAN 20
Fa0/5   → VLAN 30

S2

Fa0/1-2 → VLAN 10
Fa0/3-4 → VLAN 20
Fa0/5   → VLAN 30

The access points are placed in the GUESTS VLAN so that wireless guest clients remain separated from administrative and employee networks.

Verification

show vlan brief

Expected result:

VLAN 10 → Fa0/1, Fa0/2
VLAN 20 → Fa0/3, Fa0/4
VLAN 30 → Fa0/5

2. Trunk Configuration

The following links carry multiple VLANs:

R1 Gi0/0 ↔ S1 Gi0/1
S1 Gi0/2 ↔ S2 Gi0/2

Both are configured as 802.1Q trunks.

Example

interface gigabitEthernet 0/1
 switchport mode trunk

Verification

show interfaces trunk

The S1–S2 trunk was verified to carry:

VLAN 10
VLAN 20
VLAN 30

and to be in the trunking state.

3. Router-on-a-Stick

R1 performs inter-VLAN routing using subinterfaces on GigabitEthernet0/0.

Router configuration

interface GigabitEthernet0/0
 no ip address
 no shutdown

interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0

Result

The router provides the default gateway for each VLAN:

VLAN 10 → 192.168.10.1
VLAN 20 → 192.168.20.1
VLAN 30 → 192.168.30.1

Verification

show ip interface brief
show ip route

Expected routing table entries include:

192.168.10.0/24
192.168.20.0/24
192.168.30.0/24

4. DHCP

R1 provides DHCP services for all three VLANs.

ADMIN

ip dhcp pool ADMIN
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8

EMPLOYEE

ip dhcp pool EMPLOYEE
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8

GUESTS

ip dhcp pool GUESTS
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 8.8.8.8

Gateway addresses and selected low addresses are excluded from DHCP allocation.

Verification

show ip dhcp pool
show ip dhcp binding

Clients were successfully assigned addresses from their appropriate VLAN subnet.

5. SSH Instead of Telnet

SSH was implemented for secure switch management.

Telnet was intentionally disabled on the VTY lines.

SSH configuration

Example configuration used on the switches:

hostname S1
ip domain-name networksecurity.local

username admin privilege 15 secret <SSH_PASSWORD>

crypto key generate rsa
ip ssh version 2

line vty 0 15
 login local
 transport input ssh

enable secret <ENABLE_SECRET>

The same approach was applied to S2.

Management IPs

S1 VLAN 10 → 192.168.10.2
S2 VLAN 10 → 192.168.10.3

Default gateway for both switches:

192.168.10.1

Verification

show ip ssh
show ip interface brief

SSH connections were successfully tested from a VLAN 10 client.

Example:

ssh -l admin 192.168.10.2
ssh -l admin 192.168.10.3

Telnet was not permitted because the VTY lines use:

transport input ssh

6. Port Security

Port security was enabled on PC-facing access ports.

Configured interfaces:

S1 Fa0/1-4
S2 Fa0/1-4

Configuration

interface range fastEthernet 0/1-4
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation shutdown

Security behavior

Each port allows one learned MAC address.

Sticky learning automatically records the connected device's MAC address.

If a different MAC address is detected on the protected port, the violation action is:

Shutdown

Verification

show port-security
show port-security address
show port-security interface fa0/1

Both switches successfully learned secure sticky MAC addresses for their PC ports.

Example expected state:

Port Security       : Enabled
Port Status         : Secure-up
Maximum MAC         : 1
Sticky MAC          : 1
Violation Mode      : Shutdown

7. Unused Port Hardening

Unused FastEthernet ports were administratively disabled.

For example:

interface range fastEthernet 0/6-24
 shutdown

This was configured on both switches.

Unused interfaces are disabled to reduce the chance of an unauthorized device being connected to an unused physical port.

Verification

show interfaces status

Expected state:

Fa0/6–Fa0/24 → disabled

Only the ports actively used by PCs, APs, and trunks remain enabled.

8. Password and Access Security

Local authentication is used for SSH management.

The switches use:

username admin privilege 15 secret <SSH_PASSWORD>

Privileged EXEC access is protected with:

enable secret <ENABLE_SECRET>

The exact credentials should not be committed to GitHub. Use placeholders in documentation and keep real passwords private.

9. Access Control Lists

ACLs are used to control traffic between security zones.

ACL 110 — EMPLOYEE to ADMIN

The security policy is designed to prevent Employee-originated traffic from reaching the Admin network.

Conceptually:

EMPLOYEE (192.168.20.0/24)
            ↓
       BLOCK ADMIN
            ↓
ADMIN (192.168.10.0/24)

The ACL is applied to the VLAN 20 router subinterface.

ACL 120 — GUEST isolation

Guest traffic is restricted from reaching both internal VLANs.

GUESTS (192.168.30.0/24)
          ↓
    BLOCK ADMIN
          ↓
    BLOCK EMPLOYEE

The relevant ACL rules are:

access-list 120 deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
access-list 120 deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 120 permit ip any any

Verification

show access-lists

ACL counters were observed increasing when blocked traffic was generated, confirming that the rules were actively processing traffic.

Note: ACLs are stateless. When designing a production policy, direction and return traffic must be considered carefully. This lab focuses on demonstrating basic VLAN-to-VLAN filtering and ACL behavior.

10. Security Policy

The intended segmentation policy is:

Source

Destination

Policy

ADMIN

EMPLOYEE

Allowed

ADMIN

GUESTS

Allowed unless further restricted

EMPLOYEE

ADMIN

Restricted

EMPLOYEE

EMPLOYEE

Allowed

GUESTS

ADMIN

Blocked

GUESTS

EMPLOYEE

Blocked

GUESTS

GUESTS

Allowed

This creates a basic trust hierarchy:

ADMIN
  |
  | higher trust
  v
EMPLOYEE
  |
  | restricted
  v
GUESTS

11. Verification and Testing

The following commands were used during validation.

Router

Interface status

show ip interface brief

Routing table

show ip route

DHCP

show ip dhcp pool
show ip dhcp binding

ACLs

show access-lists

Switches

VLANs

show vlan brief

Trunks

show interfaces trunk

Port security

show port-security address
show port-security interface fa0/1

Interface status

show interfaces status

SSH

show ip ssh

Saved configuration

show startup-config

12. Test Results

The following functionality was verified during the lab:

VLAN 10, VLAN 20, and VLAN 30 were created and assigned correctly.

DHCP successfully provided addresses to clients.

Router-on-a-Stick subinterfaces became up/up.

VLAN gateway connectivity was successfully tested.

The S1–S2 802.1Q trunk carried VLANs 10, 20, and 30.

Port security successfully learned sticky MAC addresses.

Unused switch ports were disabled.

SSH access to S1 was successfully tested.

SSH access to S2 was successfully tested after generating RSA keys on S2.

Telnet was not accepted on the SSH-only VTY configuration.

ACL counters confirmed that restricted traffic matched the configured deny rules.

13. Recommended Repository Structure

network-security-project/
│
├── Network_Security_Project.pkt
├── README.md
│
└── screenshots/
    ├── topology.png
    ├── vlan-configuration.png
    ├── trunk-configuration.png
    ├── router-on-a-stick.png
    ├── dhcp.png
    ├── port-security.png
    ├── unused-ports.png
    ├── ssh-success.png
    ├── telnet-blocked.png
    └── acl-testing.png

Suggested screenshots

Use focused screenshots instead of one extremely long terminal screenshot.

Recommended evidence:

Topology: complete Packet Tracer topology

VLAN: show vlan brief

Trunk: show interfaces trunk

Routing: show ip interface brief and/or show ip route

DHCP: show ip dhcp pool / show ip dhcp binding

Port security: show port-security address

Unused ports: show interfaces status

SSH: successful ssh -l admin ... login

Telnet: failed Telnet attempt

ACL: show access-lists with match counters

14. Configuration Persistence

After completing the configuration, save the running configuration on R1, S1, and S2:

copy running-config startup-config

When prompted:

Destination filename [startup-config]?

press Enter.

Verify with:

show startup-config

The .pkt file should also be committed to the repository so the complete topology and configuration can be opened in Cisco Packet Tracer.

15. Learning Outcomes

This project provided practical experience with:

VLAN creation and segmentation

Access and trunk ports

802.1Q trunking

Router-on-a-Stick

IPv4 addressing

DHCP configuration

Inter-VLAN routing

Cisco IOS command-line configuration

SSH-based network administration

Local authentication

Port security and sticky MAC learning

Unused-port hardening

ACL configuration

Network security testing

Troubleshooting Layer 2 and Layer 3 connectivity

16. Future Improvements

Possible extensions for this lab include:

DHCP snooping

Dynamic ARP Inspection

IP Source Guard

BPDU Guard and PortFast

Stronger password policy

More granular extended ACLs

Dedicated management VLAN

Wireless authentication and encryption

Syslog and centralized logging

NTP

SNMP monitoring

Intrusion Detection/Prevention integration

Network device configuration backups

Conclusion

This project demonstrates a practical baseline security configuration for a small enterprise network. The network is segmented into separate administrative, employee, and guest zones, while security controls are applied at both the switch and router levels.

The final design combines:

VLAN Segmentation
        +
Router-on-a-Stick
        +
DHCP
        +
SSH Management
        +
Port Security
        +
Unused-Port Hardening
        +
ACL Filtering
        =
Secure Cisco Network Security Lab

The project can be opened in Cisco Packet Tracer using the included .pkt file and reviewed using the verification commands documented above.
