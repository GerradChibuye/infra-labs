# Configuration Capture Guide

Capture configuration evidence from the actual saved Packet Tracer devices. Avoid reconstructing full configurations from memory.

## Capture these outputs

### R1-BLACKOUT

```text
show ip interface brief
show ip route
show running-config
```

Review for:
- `G0/0` and subinterfaces `.10`, `.20`, `.30`, `.40`, `.50`, `.99`.
- Correct gateway address and mask per VLAN.
- 802.1Q encapsulation VLAN IDs.
- DHCP pools and excluded addresses.

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

Review for:
- VLANs 10, 20, 30, 40, 50, 99.
- Router-facing trunk Fa0/1.
- SW3-facing trunk Fa0/3.
- EtherChannel Po1 with Fa0/2 and Fa0/5 bundled.
- Root-bridge priority for all six VLANs.
- Management SVI `192.168.11.34/27`.
- SSH-only VTY configuration.

### SW2-ACCESS

```text
show vlan brief
show interfaces trunk
show etherchannel summary
show port-security
show port-security address
show interfaces status
show ip interface brief
show running-config
```

Review for:
- Po1 with Fa0/1 and Fa0/4 bundled.
- PC0 Fa0/2 in VLAN 10 and PC1 Fa0/3 in VLAN 20.
- Sticky secure MAC addresses after client traffic.
- Fa0/5 shut down.
- Management SVI `192.168.11.35/27`.

### SW3-ACCESS

```text
show vlan brief
show interfaces trunk
show spanning-tree vlan 10
show port-security
show port-security address
show interfaces status
show ip interface brief
show running-config
```

Review for:
- Fa0/1 trunk to SW1.
- PC2 Fa0/2 in VLAN 30 and PC3 Fa0/3 in VLAN 40.
- Sticky secure MAC addresses.
- Fa0/4–Fa0/24 shut down, as configured in the lab.
- Management SVI `192.168.11.36/27`.

## Secret handling

Before committing output or configuration:
- Replace local passwords and `enable secret` values with placeholders.
- Do not commit private keys or credential files.
- Keep the full working Packet Tracer file private if it contains any secrets you do not intend to publish.
- Prefer sanitized excerpts for public documentation.

## Capture quality

Prefer readable screenshots that show the device prompt, command and complete relevant output. Name files according to `verification.md`. Include at least:
1. Whole topology.
2. VLAN and addressing evidence.
3. Inter-VLAN ping evidence.
4. EtherChannel normal and failure states.
5. STP root evidence.
6. SSH session.
7. Port-security MAC learning.
8. The VLAN 20 trunk allow-list fault and correction, if you recreate it for a demonstration.
