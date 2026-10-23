# SW2-ACCESS — Configuration Reference

## Role

SW2-ACCESS connects the Administration and Finance clients. It uplinks to SW1-CORE using a two-member LACP EtherChannel.

## Management

| Setting | Value |
|---|---|
| Management VLAN | 99 |
| Management IPv4 | `192.168.11.35/27` |
| Default gateway | `192.168.11.33` |
| SSH version | 2 |
| Domain name | `blackout.local` |
| STP mode | PVST |

## Port Roles

| Interface | Purpose |
|---|---|
| `Fa0/1` | LACP member to SW1-CORE |
| `Fa0/2` | ADMIN client, VLAN 10 |
| `Fa0/3` | FINANCE client, VLAN 20 |
| `Fa0/4` | LACP member to SW1-CORE |
| `Fa0/5` | Unused access port, administratively shut down |
| `Port-channel1` | Trunk to SW1-CORE |
| `Vlan99` | Management SVI |

## EtherChannel

```text
interface Port-channel1
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk

interface FastEthernet0/1
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
 channel-group 1 mode active

interface FastEthernet0/4
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
 channel-group 1 mode active
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

The captured configuration showed sticky MAC learning and the `restrict` violation action. Port-security inspection showed a maximum of one address per protected port. Learned MAC addresses are deliberately omitted from this document.

## Management SVI

```text
interface Vlan99
 ip address 192.168.11.35 255.255.255.224

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

The switch uses a local management account. Credentials and password hashes are not published.

## Verification Commands

```text
show vlan brief
show interfaces trunk
show etherchannel summary
show port-security
show port-security interface fastEthernet 0/2
show port-security interface fastEthernet 0/3
show ip interface brief
```
