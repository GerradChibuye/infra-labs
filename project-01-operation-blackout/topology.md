# Topology and Interface Map

## Devices

| Device | Model | Role |
|---|---|---|
| R1-BLACKOUT | Cisco 2911 | Inter-VLAN routing and DHCP |
| SW1-CORE | Cisco 2960 | Core/distribution switch; intended STP root |
| SW2-ACCESS | Cisco 2960 | ADMIN and FINANCE access |
| SW3-ACCESS | Cisco 2960 | IT and STAFF access |
| Server0 | Packet Tracer server | Server VLAN endpoint |
| PC0–PC3 | Packet Tracer PCs | Department endpoints |

## Physical connections

| Endpoint A | Endpoint B | Purpose |
|---|---|---|
| R1 `G0/0` | SW1 `Fa0/1` | 802.1Q router-on-a-stick trunk |
| SW1 `Fa0/2` | SW2 `Fa0/1` | LACP EtherChannel member |
| SW1 `Fa0/5` | SW2 `Fa0/4` | LACP EtherChannel member |
| SW1 `Fa0/3` | SW3 `Fa0/1` | 802.1Q trunk |
| SW1 `Fa0/4` | Server0 `FastEthernet0` | Access VLAN 50 |
| SW2 `Fa0/2` | PC0 | Access VLAN 10 |
| SW2 `Fa0/3` | PC1 | Access VLAN 20 |
| SW3 `Fa0/2` | PC2 | Access VLAN 30 |
| SW3 `Fa0/3` | PC3 | Access VLAN 40 |

## Logical design

- R1 provides one default gateway per VLAN using subinterfaces on `G0/0`.
- SW1, SW2 and SW3 carry the six project VLANs where required.
- SW1 and SW2 aggregate two physical links into `Port-channel1` using LACP.
- SW1 is configured as root primary for VLANs 10, 20, 30, 40, 50 and 99.
- Switch management uses VLAN 99.

## Notes

Keep physical topology and interface labels consistent with the saved `.pkt` file. If the topology is later changed, update this table and the diagram together.
