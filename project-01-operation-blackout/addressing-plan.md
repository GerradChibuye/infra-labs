# Operation Blackout — IP Addressing Plan

## 1. Overview

The network uses IPv4 subnetting to separate four departments, a server network, and a management network. The departmental networks use `/26` subnets, while the server and management networks use `/27` subnets.

## 2. VLAN and Subnet Allocation

| VLAN | Name | Network | Subnet Mask | Usable Host Range | Default Gateway | Broadcast |
|---:|---|---|---|---|---|---|
| 10 | ADMIN | `192.168.10.0/26` | `255.255.255.192` | `192.168.10.1–192.168.10.62` | `192.168.10.1` | `192.168.10.63` |
| 20 | FINANCE | `192.168.10.64/26` | `255.255.255.192` | `192.168.10.65–192.168.10.126` | `192.168.10.65` | `192.168.10.127` |
| 30 | IT | `192.168.10.128/26` | `255.255.255.192` | `192.168.10.129–192.168.10.190` | `192.168.10.129` | `192.168.10.191` |
| 40 | STAFF | `192.168.10.192/26` | `255.255.255.192` | `192.168.10.193–192.168.10.254` | `192.168.10.193` | `192.168.10.255` |
| 50 | SERVERS | `192.168.11.0/27` | `255.255.255.224` | `192.168.11.1–192.168.11.30` | `192.168.11.1` | `192.168.11.31` |
| 99 | MANAGEMENT | `192.168.11.32/27` | `255.255.255.224` | `192.168.11.33–192.168.11.62` | `192.168.11.33` | `192.168.11.63` |

The usable ranges above include the gateway addresses.

## 3. Router Subinterfaces

| Router Interface | VLAN | IPv4 Address | Purpose |
|---|---:|---|---|
| `G0/0.10` | 10 | `192.168.10.1/26` | ADMIN gateway |
| `G0/0.20` | 20 | `192.168.10.65/26` | FINANCE gateway |
| `G0/0.30` | 30 | `192.168.10.129/26` | IT gateway |
| `G0/0.40` | 40 | `192.168.10.193/26` | STAFF gateway |
| `G0/0.50` | 50 | `192.168.11.1/27` | SERVERS gateway |
| `G0/0.99` | 99 | `192.168.11.33/27` | MANAGEMENT gateway |

The physical interface `G0/0` carries the VLAN-tagged traffic. Each subinterface uses IEEE 802.1Q encapsulation.

## 4. End-Device Addressing

### DHCP Clients

| Device | Department | VLAN | Verified IPv4 Address |
|---|---|---:|---|
| PC0 | ADMIN | 10 | `192.168.10.11` |
| PC1 | FINANCE | 20 | `192.168.10.75` |
| PC2 | IT | 30 | `192.168.10.139` |
| PC3 | STAFF | 40 | `192.168.10.203` |

The router provides DHCP services for VLANs 10, 20, 30, and 40. The first ten addresses in each `/26` subnet are excluded from DHCP allocation, protecting the gateway and reserved address space.

The DHCP pools also advertise `8.8.8.8` as the DNS server. This configuration does not, by itself, demonstrate that Internet connectivity or external DNS resolution is available in the simulation.

### Server

| Device | VLAN | IPv4 Address | Mask | Default Gateway |
|---|---:|---|---|---|
| Server0 | 50 | `192.168.11.10` | `255.255.255.224` | `192.168.11.1` |

### Switch Management

| Device | Management VLAN | IPv4 Address | Mask | Default Gateway |
|---|---:|---|---|---|
| SW1-CORE | 99 | `192.168.11.34` | `255.255.255.224` | `192.168.11.33` |
| SW2-ACCESS | 99 | `192.168.11.35` | `255.255.255.224` | `192.168.11.33` |
| SW3-ACCESS | 99 | `192.168.11.36` | `255.255.255.224` | `192.168.11.33` |

## 5. Design Notes

- VLANs create separate Layer 2 broadcast domains.
- Router subinterfaces provide inter-VLAN routing.
- VLAN 50 separates server traffic from departmental client traffic.
- VLAN 99 provides a dedicated management subnet for the switches.
- DHCP is configured only for the four departmental VLANs.
- The addressing plan is designed for this simulated lab and may need adjustment before production use.

## 6. Verification

Address assignments were checked on the end devices, and connectivity was tested between departmental clients, Server0, and the switch management interfaces.

See [Verification Results](verification.md) for the recorded test outcomes.
