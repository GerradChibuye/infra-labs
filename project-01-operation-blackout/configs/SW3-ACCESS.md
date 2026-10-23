# SW3-ACCESS — Configuration Reference

## Role

SW3-ACCESS connects the IT and Staff clients. It uplinks to SW1-CORE using an 802.1Q trunk.

## Management

| Setting | Value |
|---|---|
| Management VLAN | 99 |
| Management IPv4 | `192.168.11.36/27` |
| Default gateway | `192.168.11.33` |
| SSH version | 2 |
| Domain name | `blackout.local` |
| STP mode | PVST |

## Port Roles

| Interface | Purpose |
|---|---|
| `Fa0/1` | Trunk to SW1-CORE |
| `Fa0/2` | IT client, VLAN 30 |
| `Fa0/3` | Staff client, VLAN 40 |
| `Fa0/4–Fa0/24` | Administratively shut down |
| `Vlan99` | Management SVI |

## Uplink Trunk

```text
interface FastEthernet0/1
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
```

## Protected Access Ports

The intended policy for `Fa0/2` and `Fa0/3` is:

```text
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation restrict
```

Port-security output showed one permitted MAC address per protected port and zero violations at the time of inspection. Learned MAC addresses are intentionally omitted from this public reference.

## Management SVI

```text
interface Vlan99
 ip address 192.168.11.36 255.255.255.224

ip default-gateway 192.168.11.33
```

## SSH

```text
ip ssh version 2
ip domain-name blackout.local

line vty 0 4
 login local
 transport input ssh

line vty 5 15
 login local
 transport input ssh
```

The local management account and credentials are not reproduced in this document.

## Verification Commands

```text
show vlan brief
show interfaces trunk
show spanning-tree vlan 10
show port-security
show port-security interface fastEthernet 0/2
show port-security interface fastEthernet 0/3
show ip interface brief
```

## Unused Ports

`Fa0/4` through `Fa0/24` are configured with `shutdown` in the captured configuration. GigabitEthernet interfaces were not shown as administratively shut down, so this document does not claim every unused interface on the switch is disabled.
