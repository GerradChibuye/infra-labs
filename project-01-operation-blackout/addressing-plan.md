# VLAN and Addressing Plan

## VLAN subnets

| VLAN | Name | Network/prefix | Mask | Usable host range | Gateway |
|---:|---|---|---|---|---|
| 10 | ADMIN | `192.168.10.0/26` | `255.255.255.192` | `192.168.10.1–192.168.10.62` | `192.168.10.1` |
| 20 | FINANCE | `192.168.10.64/26` | `255.255.255.192` | `192.168.10.65–192.168.10.126` | `192.168.10.65` |
| 30 | IT | `192.168.10.128/26` | `255.255.255.192` | `192.168.10.129–192.168.10.190` | `192.168.10.129` |
| 40 | STAFF | `192.168.10.192/26` | `255.255.255.192` | `192.168.10.193–192.168.10.254` | `192.168.10.193` |
| 50 | SERVERS | `192.168.11.0/27` | `255.255.255.224` | `192.168.11.1–192.168.11.30` | `192.168.11.1` |
| 99 | MANAGEMENT | `192.168.11.32/27` | `255.255.255.224` | `192.168.11.33–192.168.11.62` | `192.168.11.33` |

## Known assignments

| Device | Address/prefix | Default gateway | Method |
|---|---|---|---|
| PC0 | `192.168.10.11/26` | `192.168.10.1` | DHCP |
| PC1 | `192.168.10.75/26` | `192.168.10.65` | DHCP |
| PC2 | `192.168.10.139/26` | `192.168.10.129` | DHCP |
| PC3 | `192.168.10.203/26` | `192.168.10.193` | DHCP |
| Server0 | `192.168.11.10/27` | `192.168.11.1` | Static |
| SW1-CORE SVI | `192.168.11.34/27` | `192.168.11.33` | Static |
| SW2-ACCESS SVI | `192.168.11.35/27` | `192.168.11.33` | Static |
| SW3-ACCESS SVI | `192.168.11.36/27` | `192.168.11.33` | Static |

## DHCP notes

The router provides DHCP to VLANs 10, 20, 30 and 40. Addresses `.1` through `.10` are excluded in each `/26` subnet, preserving low addresses for gateways and possible infrastructure. The known leases above were observed in the lab.

VLAN 50 and VLAN 99 use static addressing in this implementation. No DHCP service for these VLANs is claimed.
