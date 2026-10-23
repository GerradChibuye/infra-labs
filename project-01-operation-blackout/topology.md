# Operation Blackout — Network Topology

## 1. Logical Design

Operation Blackout uses a three-tier-inspired campus layout with a central switching point, two access switches, and a router providing inter-VLAN routing.

The topology is simulated in Cisco Packet Tracer.

## 2. Devices and Roles

| Device | Role |
|---|---|
| R1-BLACKOUT | Router-on-a-stick and DHCP server |
| SW1-CORE | Core switch, VLAN trunking, EtherChannel termination, preferred STP root |
| SW2-ACCESS | Access switch for Administration and Finance clients |
| SW3-ACCESS | Access switch for IT and Staff clients |
| PC0 | Administration client, VLAN 10 |
| PC1 | Finance client, VLAN 20 |
| PC2 | IT client, VLAN 30 |
| PC3 | Staff client, VLAN 40 |
| Server0 | Server endpoint, VLAN 50 |

## 3. Physical and Logical Links

| Device A / Interface | Device B / Interface | Link Purpose |
|---|---|---|
| R1-BLACKOUT `G0/0` | SW1-CORE `Fa0/1` | Router-to-switch VLAN trunk |
| SW1-CORE `Fa0/2` | SW2-ACCESS `Fa0/1` | EtherChannel member |
| SW1-CORE `Fa0/5` | SW2-ACCESS `Fa0/4` | EtherChannel member |
| SW1-CORE `Fa0/3` | SW3-ACCESS `Fa0/1` | VLAN trunk |
| SW1-CORE `Fa0/4` | Server0 `FastEthernet0` | Server access link, VLAN 50 |
| SW2-ACCESS `Fa0/2` | PC0 | Administration access port, VLAN 10 |
| SW2-ACCESS `Fa0/3` | PC1 | Finance access port, VLAN 20 |
| SW3-ACCESS `Fa0/2` | PC2 | IT access port, VLAN 30 |
| SW3-ACCESS `Fa0/3` | PC3 | Staff access port, VLAN 40 |

## 4. EtherChannel

SW1-CORE and SW2-ACCESS aggregate two physical Ethernet links into `Port-channel1` using LACP. The port-channel operates as a trunk carrying VLANs 10, 20, 30, 40, 50, and 99.

The two physical links are configured as LACP active members on both switches.

## 5. Inter-VLAN Routing

R1-BLACKOUT connects to SW1-CORE through a single physical interface with 802.1Q subinterfaces. Each subinterface serves as the default gateway for its associated VLAN.

This design is commonly called router-on-a-stick.

## 6. Spanning Tree

PVST is enabled on the switches. SW1-CORE is configured with a preferred root priority for the six configured VLANs.

The intended result is for SW1-CORE to serve as the root bridge while the access switches select their upstream paths toward the core.

## 7. Topology Diagram

```text
                         R1-BLACKOUT
                         G0/0 trunk
                              |
                              |
                         Fa0/1 trunk
                          SW1-CORE
                         /    |     \
                        /     |      \
                 Port-channel1|      Fa0/3 trunk
                  LACP trunk  |           |
                    /         |       SW3-ACCESS
                   /          |        /       \
             SW2-ACCESS    Server0   PC2       PC3
               /    \      VLAN 50  VLAN 30   VLAN 40
             PC0    PC1
           VLAN 10 VLAN 20
```

The diagram is logical and does not show every individual port label. Refer to the link table and Packet Tracer file for exact connections.

## 8. Related Documentation

- [Addressing Plan](addressing-plan.md)
- [Verification Results](verification.md)
- [Troubleshooting Case Study](troubleshooting.md)
