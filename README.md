# CCNA Network Infrastructure Lab

## Project Overview

This project is a hands-on Cisco networking lab built using Cisco Packet Tracer.

The lab demonstrates practical CCNA-level networking concepts including VLANs, trunking, inter-VLAN routing, DHCP, static routing, NAT/PAT, and network troubleshooting.

The project was designed and configured from scratch to simulate a small enterprise network environment.

---

## Lab Objectives

* Design and implement a multi-VLAN network.
* Configure VLANs and trunk links.
* Configure Router-on-a-Stick for inter-VLAN routing.
* Configure DHCP services for multiple VLANs.
* Configure static routing between the internal network and ISP.
* Configure NAT/PAT for internal network access.
* Troubleshoot VLAN, trunk, DHCP, and interface connectivity issues.
* Verify network connectivity using Cisco IOS commands.

---

## Network Topology

The lab consists of:

* 1 Router (R1)
* 1 ISP Router
* 2 Switches (SW1 and SW2)
* 4 PCs:

  * PC-IT
  * PC-HR
  * PC-Sales
  * PC-Server

The internal network is divided into multiple VLANs.

---

## VLAN and IP Addressing

| VLAN    | Department | Network          | Gateway       |
| ------- | ---------- | ---------------- | ------------- |
| VLAN 10 | IT         | 192.168.10.0/27  | 192.168.10.1  |
| VLAN 20 | HR         | 192.168.10.32/27 | 192.168.10.33 |
| VLAN 30 | Sales      | 192.168.10.64/27 | 192.168.10.65 |
| VLAN 40 | Servers    | 192.168.10.96/28 | 192.168.10.97 |

The connection between R1 and the ISP uses:

* R1: 10.0.0.1/30
* ISP: 10.0.0.2/30

---

## Technologies Used

* Cisco Packet Tracer
* Cisco IOS
* VLANs
* 802.1Q Trunking
* Router-on-a-Stick
* DHCP
* Static Routing
* NAT/PAT
* IPv4
* Subnetting
* Network Troubleshooting

---

## VLAN Configuration

Four VLANs were created to separate departments and services:

* VLAN 10 — IT
* VLAN 20 — HR
* VLAN 30 — Sales
* VLAN 40 — Servers

Access ports were assigned to their corresponding VLANs.

The connection between SW1 and SW2 was configured as a trunk carrying the required VLANs.

---

## Inter-VLAN Routing

Router-on-a-Stick was implemented on R1 using 802.1Q subinterfaces.

The router provides the default gateway for each VLAN:

```text
G0/0/0.10 → 192.168.10.1
G0/0/0.20 → 192.168.10.33
G0/0/0.30 → 192.168.10.65
G0/0/0.40 → 192.168.10.97
```

This allows devices in different VLANs to communicate through R1.

---

## DHCP Configuration

DHCP was configured on R1 to automatically assign IPv4 addresses to the VLANs.

For VLAN 10, the DHCP pool uses:

```text
Network: 192.168.10.0/27
Default Gateway: 192.168.10.1
DNS Server: 8.8.8.8
```

Excluded addresses were configured to reserve the gateway and other required addresses.

DHCP operation was verified using:

```text
show ip dhcp binding
```

---

## Static Routing

A default route was configured on R1 toward the ISP:

```text
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

A return route was configured on the ISP router for the internal network:

```text
ip route 192.168.10.0 255.255.255.0 10.0.0.1
```

Routing was verified using:

```text
show ip route
```

---

## NAT/PAT Configuration

NAT overload (PAT) was configured on R1 to translate internal private IP addresses when communicating through the ISP connection.

The NAT configuration uses:

```text
access-list 1 permit 192.168.10.0 0.0.0.255
```

and:

```text
ip nat inside source list 1 interface GigabitEthernet0/0/1 overload
```

NAT operation was verified using:

```text
show ip nat translations
```

---

## Troubleshooting

Several network problems were intentionally introduced and then diagnosed and corrected.

### Trunk/VLAN Troubleshooting

The trunk was temporarily configured to allow only VLANs 10 and 20.

As a result, VLAN 30 traffic could not pass through the trunk and PC-Sales could not reach its default gateway.

The problem was identified using:

```text
show interfaces trunk
```

The trunk was then restored to allow:

```text
10,20,30,40
```

### DHCP Troubleshooting

A DHCP issue was intentionally introduced by excluding too many addresses.

The DHCP configuration was inspected and the incorrect exclusion was removed.

The VLAN 10 DHCP pool was restored and PC-IT successfully received:

```text
192.168.10.10
```

### Interface Troubleshooting

The PC-IT switch port was intentionally shut down.

The problem was identified using:

```text
show interfaces fa0/3 status
```

The interface was restored using:

```text
no shutdown
```

The interface returned to:

```text
connected
```

---

## Verification

The following commands were used to verify the network:

```text
ipconfig
show ip dhcp binding
show ip interface brief
show ip nat translations
show ip route
```

These commands were used to verify:

* Client IP configuration
* DHCP address allocation
* Interface and subinterface status
* NAT translations
* Routing information

---

## Screenshots

The `screenshots` folder contains verification evidence from the lab:

* `ipconfig`
* `show ip dhcp binding`
* `show ip interface brief`
* `show ip nat translations`
* `show ip route`

---

## Skills Demonstrated

* VLAN Configuration
* Trunk Configuration
* Router-on-a-Stick
* Inter-VLAN Routing
* DHCP Configuration
* Static Routing
* NAT/PAT
* IPv4 Addressing
* Subnetting
* Cisco IOS CLI
* Network Troubleshooting
* Connectivity Verification
* Packet Tracer Lab Design

---

## Project Files

```text
CCNA-Network-Infrastructure-Lab/
│
├── networking-lab.pkt
├── README.md
│
└── screenshots/
    ├── ipconfig
    ├── dhcp-binding
    ├── ip-interface-brief
    ├── nat-translations
    └── ip-route
```

## Conclusion

This project demonstrates practical implementation and troubleshooting of a small enterprise Cisco network using CCNA-level technologies.

The lab provides hands-on experience with network segmentation, routing, DHCP, NAT/PAT, and systematic troubleshooting using Cisco IOS commands.
