# R1-BLACKOUT — Configuration Reference

## Role

R1-BLACKOUT provides router-on-a-stick inter-VLAN routing and DHCP for the four departmental networks.

## Physical Interface

| Interface | Purpose |
|---|---|
| `GigabitEthernet0/0` | 802.1Q trunk to SW1-CORE |
| `GigabitEthernet0/1` | Administratively shut down |
| `GigabitEthernet0/2` | Administratively shut down |

The parent interface `G0/0` has no IPv4 address. VLAN gateways are assigned to subinterfaces.

## Subinterfaces

```text
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.192

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.10.65 255.255.255.192

interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.10.129 255.255.255.192

interface GigabitEthernet0/0.40
 encapsulation dot1Q 40
 ip address 192.168.10.193 255.255.255.192

interface GigabitEthernet0/0.50
 encapsulation dot1Q 50
 ip address 192.168.11.1 255.255.255.224

interface GigabitEthernet0/0.99
 encapsulation dot1Q 99
 ip address 192.168.11.33 255.255.255.224
```

## DHCP

The router has DHCP pools for ADMIN, FINANCE, IT, and STAFF.

```text
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.10.65 192.168.10.74
ip dhcp excluded-address 192.168.10.129 192.168.10.138
ip dhcp excluded-address 192.168.10.193 192.168.10.202

ip dhcp pool ADMIN
 network 192.168.10.0 255.255.255.192
 default-router 192.168.10.1
 dns-server 8.8.8.8

ip dhcp pool FINANCE
 network 192.168.10.64 255.255.255.192
 default-router 192.168.10.65
 dns-server 8.8.8.8

ip dhcp pool IT
 network 192.168.10.128 255.255.255.192
 default-router 192.168.10.129
 dns-server 8.8.8.8

ip dhcp pool STAFF
 network 192.168.10.192 255.255.255.192
 default-router 192.168.10.193
 dns-server 8.8.8.8
```

## Verification Commands

```text
show ip interface brief
show ip route
show ip dhcp pool
show ip dhcp binding
show running-config
```

## Security Note

The captured router configuration did not establish SSH-only VTY access. Do not describe the router as SSH-hardened without separately verifying and documenting its management configuration.

This file omits irrelevant defaults and credentials. Consult the Packet Tracer project for the complete running configuration.
