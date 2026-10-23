# SW1-CORE — Configuration Reference

## Role

SW1-CORE is the central switch. It connects to the router, both access switches, and Server0. It terminates the LACP EtherChannel to SW2-ACCESS and is configured as the preferred STP root for the six project VLANs.

## VLANs and Management

VLANs used: 10, 20, 30, 40, 50, and 99.

| Setting | Value |
|---|---|
| Management SVI | VLAN 99 |
| Management IPv4 | `192.168.11.34/27` |
| Default gateway | `192.168.11.33` |
| STP mode | PVST |
| SSH version | 2 |
| Domain name | `blackout.local` |

## Port Roles

| Interface | Purpose |
|---|---|
| `Fa0/1` | Trunk to R1-BLACKOUT |
| `Fa0/2` | LACP member to SW2-ACCESS |
| `Fa0/3` | Trunk to SW3-ACCESS |
| `Fa0/4` | Server0 connection, assigned to VLAN 50 |
| `Fa0/5` | LACP member to SW2-ACCESS |
| `Port-channel1` | LACP trunk to SW2-ACCESS |
| `Vlan99` | Management SVI |

## EtherChannel and Trunks

```text
interface Port-channel1
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk

interface FastEthernet0/2
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
 channel-group 1 mode active

interface FastEthernet0/5
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
 channel-group 1 mode active
```

The router-facing and SW3-facing trunk links also permit VLANs 10, 20, 30, 40, 50, and 99.

## Spanning Tree

```text
spanning-tree mode pvst
spanning-tree vlan 10,20,30,40,50,99 priority 24576
```

This sets the configured bridge priority to 24576 before the extended system ID is included in the displayed priority. VLAN 10 was directly verified to use SW1-CORE as root. The project verification notes record the subsequent checks for all six VLANs.

## Management SVI

```text
interface Vlan99
 ip address 192.168.11.34 255.255.255.224

ip default-gateway 192.168.11.33
```

## SSH VTY Lines

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

A local management account is configured on the switch. Its username and password hash are intentionally omitted here.

## Verification Commands

```text
show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree vlan 10
show ip interface brief
show port-security
show running-config
```

The Server0 interface is documented as assigned to VLAN 50. In the captured running configuration, an explicit `switchport mode access` line was not present for `Fa0/4`; verify the live configuration before describing that interface as explicitly forced to access mode.
