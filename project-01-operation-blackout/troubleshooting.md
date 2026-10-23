# Operation Blackout — Troubleshooting Case Study

## Incident: VLAN 20 Connectivity Failure

### 1. Symptom

PC0 in VLAN 10 could not successfully ping PC1 in VLAN 20. Other connectivity tests from PC0 were working.

This suggested that the problem was not necessarily a complete router or network outage. The investigation focused on the path required for communication between VLAN 10 and VLAN 20.

### 2. Investigation

The switches' trunk configuration was inspected using:

```text
show interfaces trunk
```

The allowed VLAN list on SW1-CORE's Port-channel1 contained:

```text
10,29-30,40,50,99
```

VLAN 20 was missing from the list. Because PC1's traffic traversed the EtherChannel trunk, VLAN 20 traffic could not pass across that link as intended.

### 3. Root Cause

The allowed VLAN list on Port-channel1 incorrectly permitted VLANs 29 and 30 rather than VLANs 20 and 30.

This was a Layer 2 trunk configuration fault, not an IP addressing error on PC0 or PC1.

### 4. Corrective Action

On SW1-CORE, the Port-channel1 allowed VLAN list was corrected:

```text
configure terminal
interface port-channel 1
 switchport trunk allowed vlan 10,20,30,40,50,99
end
```

The corresponding trunk configuration on the peer switch was also checked.

### 5. Verification

After the correction:

- PC0 could successfully communicate with PC1.
- Previously working connectivity to PC2, PC3, Server0, and switch management addresses remained available.
- The allowed VLAN list matched the intended VLAN plan.

### 6. Lessons Learned

1. A device can communicate with some destinations while failing to reach another VLAN because trunk policies may permit only a subset of VLANs.
2. `show interfaces trunk` is useful for verifying trunk status, encapsulation, and allowed VLAN membership.
3. The allowed VLAN list should be checked at both ends of a trunk or EtherChannel.
4. After a correction, verify both the affected flow and existing working paths to detect regressions.
5. VLAN identifiers must be checked carefully. A small typing error can create a network fault that resembles a routing problem.

## Additional Resilience Test: EtherChannel Member Failure

A separate test disconnected one physical member of the LACP EtherChannel between SW1-CORE and SW2-ACCESS.

Port-channel1 remained operational through the surviving member. After the disconnected link was restored, both physical links were bundled again.

This test demonstrates the benefit of link aggregation while also highlighting that EtherChannel does not protect against every possible device or path failure.
