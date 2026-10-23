# Verification and Test Plan

Record actual output, date, and screenshot filename for each test before publishing. A test marked “operator reported pass” is based on the lab operator's confirmation; attach the corresponding evidence if available.

| ID | Test | Method | Expected result | Current report |
|---|---|---|---|---|
| T01 | Client DHCP | Check PC IP configuration | Each client receives the planned subnet, gateway and expected lease | Pass |
| T02 | Local gateway | Ping each client's own gateway | Replies received | Pass, per prior lab checks |
| T03 | Inter-VLAN routing | From PC0 ping PC1, PC2 and PC3 | Replies received | Pass |
| T04 | Server reachability | From PC0 ping `192.168.11.10` | Replies received | Pass |
| T05 | Switch management | From PC0 ping `.34`, `.35`, `.36` in `192.168.11.32/27` | Replies received | Pass |
| T06 | EtherChannel state | `show etherchannel summary` on SW1 and SW2 | `Po1(SU)` and both member links `(P)` after restoration | Pass |
| T07 | EtherChannel failure | Disconnect one member, inspect channel, ping endpoint, restore cable | Po1 stays up; remaining member forwards; both members rebundle after restoration | Pass, operator confirmed |
| T08 | STP VLAN 10 | `show spanning-tree vlan 10` on all switches | SW1 is root; SW2 root port Po1; SW3 root port Fa0/1 | Directly observed and passed |
| T09 | STP remaining VLANs | `show spanning-tree vlan 20`, `30`, `40`, `50`, `99` on SW1 and selected access switches | Root ID corresponds to SW1 for each VLAN | Operator reports all six VLANs pass; retain outputs for evidence |
| T10 | Trunk allow-list | `show interfaces trunk` on SW1/SW2/SW3 | Required VLANs allowed, active and forwarding on each required trunk | Pass, operator confirmed |
| T11 | SSH | SSH from PC to each switch management address | Authenticated CLI; Telnet not permitted on configured VTY lines | Pass |
| T12 | Port security | `show port-security` and `show port-security address` on SW2/SW3 | Maximum one MAC on each PC port; sticky addresses learned; zero violations in normal use | SW3 directly observed; operator reports all four ports configured |
| T13 | Unused ports | `show interfaces status` | Confirmed unused ports disabled; used links remain connected | SW2/SW3 directly inspected |
| T14 | Save configuration | `write memory` on each device | Save operation returns `[OK]` | Pass |

## Suggested final evidence commands

### Router

```text
show ip interface brief
show ip route
show running-config
```

### SW1-CORE

```text
show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40
show spanning-tree vlan 50
show spanning-tree vlan 99
show ip interface brief
show running-config
```

### SW2-ACCESS

```text
show vlan brief
show interfaces trunk
show etherchannel summary
show port-security
show port-security address
show interfaces status
show ip interface brief
```

### SW3-ACCESS

```text
show vlan brief
show interfaces trunk
show spanning-tree vlan 10
show port-security
show port-security address
show interfaces status
show ip interface brief
```

Do not publish passwords, password hashes, private keys, or other credentials from `show running-config`. Redact secrets before committing configuration output.

## Evidence naming suggestion

```text
evidence/
├── 01-topology.png
├── 02-vlan-brief-sw1.png
├── 03-router-subinterfaces.png
├── 04-dhcp-leases.png
├── 05-inter-vlan-ping.png
├── 06-etherchannel-normal.png
├── 07-etherchannel-member-failure.png
├── 08-etherchannel-restored.png
├── 09-stp-root-vlan10.png
├── 10-trunk-allowed-vlans.png
├── 11-ssh-management.png
└── 12-port-security.png
```
