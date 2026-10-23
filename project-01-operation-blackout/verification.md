# Operation Blackout — Verification Results

## 1. Purpose

This document records the operational checks performed on the Operation Blackout campus network in Cisco Packet Tracer.

The checks cover address assignment, routing, switching, management access, redundancy behavior, and access-port security.

## 2. Verification Summary

| Test | Expected Result | Recorded Outcome |
|---|---|---|
| DHCP on PC0–PC3 | Each client receives an address from its departmental subnet | Passed |
| Inter-VLAN connectivity | Clients in different VLANs can communicate through R1-BLACKOUT | Passed |
| Server reachability | Departmental clients can reach Server0 | Passed |
| Switch management reachability | Client can reach the three switch management addresses | Passed |
| EtherChannel formation | Port-channel1 is operational and both members are bundled | Passed |
| EtherChannel member interruption | Port-channel remains operational when one member is disconnected | Passed |
| EtherChannel restoration | Both member links rejoin the bundle after reconnection | Passed |
| STP root selection | SW1-CORE is configured as preferred root for the six VLANs | Configuration applied; VLAN 10 root status directly observed |
| Trunk VLAN restrictions | Required VLANs are permitted on configured trunk links | Passed according to lab checks |
| SSH management | SSH access works on the switches | Passed according to lab checks |
| Port security | Protected access ports learn one client MAC and use the `restrict` violation action | Passed according to lab checks |

## 3. DHCP Address Verification

The following leases were observed:

| Client | VLAN | Address |
|---|---:|---|
| PC0 | 10 | `192.168.10.11` |
| PC1 | 20 | `192.168.10.75` |
| PC2 | 30 | `192.168.10.139` |
| PC3 | 40 | `192.168.10.203` |

Each address belongs to the intended subnet.

## 4. Inter-VLAN and Server Connectivity

Connectivity was tested from PC0 to PC1, PC2, PC3, Server0, and the management addresses of SW1-CORE, SW2-ACCESS, and SW3-ACCESS.

The tests succeeded after the VLAN 20 trunking fault described in the troubleshooting case study was corrected.

## 5. EtherChannel Resilience Test

The LACP bundle between SW1-CORE and SW2-ACCESS was first verified as operational with both physical members bundled.

One member link was then disconnected in Packet Tracer. Port-channel1 remained operational through the remaining member. After the cable was restored, both members returned to the bundle.

This demonstrates link-level resilience within the simulated EtherChannel. It does not provide protection against failure of either switch or the entire port-channel path.

## 6. Spanning Tree Verification

SW1-CORE was configured with a preferred root priority for VLANs 10, 20, 30, 40, 50, and 99.

The directly observed VLAN 10 output showed SW1-CORE as the root bridge. The user subsequently reported that the final root-selection checks passed for all six VLANs.

## 7. SSH and Port Security

SSH version 2 and local authentication were configured on the switches. The VTY lines were configured to accept SSH.

Port security was applied to the four departmental access ports with sticky MAC learning and the `restrict` violation action. The observed SW3 port-security summary showed a maximum of one address per protected port and zero violations at the time of inspection.

## 8. Evidence

Place screenshots in this directory and reference their actual filenames here. Useful evidence includes:

- DHCP address information from the four clients.
- Successful inter-VLAN and server pings.
- `show etherchannel summary` output.
- EtherChannel status during a member-link interruption.
- `show spanning-tree vlan 10` output.
- STP root verification for the remaining VLANs.
- `show interfaces trunk` output.
- Successful SSH management access.
- `show port-security` or `show port-security interface` output.
- The corrected VLAN 20 trunk configuration.

Do not claim a screenshot exists unless it has actually been uploaded.

## 9. Conclusion

The tests demonstrate that the simulated network provides departmental VLAN segmentation, inter-VLAN routing, DHCP, server and switch management reachability, LACP EtherChannel resilience, STP root selection, SSH management, and access-port security.

The documented VLAN 20 fault also demonstrates the importance of verifying trunk VLAN membership during troubleshooting.
