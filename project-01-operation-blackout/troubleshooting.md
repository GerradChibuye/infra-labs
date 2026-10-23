# Troubleshooting Record

## Incident: VLAN 20 traffic did not cross the core-to-access EtherChannel

### Symptom

PC0 in VLAN 10 could reach some remote VLANs and the server, but PC0 could not ping PC1 in VLAN 20. PC1 could ping its own gateway, while the router could not successfully ping PC1.

### Investigation

The investigation checked the relevant layers rather than assuming that a failed ping meant routing was broken:

1. Verified PC1's address and VLAN 20 gateway.
2. Checked the PC1 access port and confirmed it was up in VLAN 20.
3. Checked the router's VLAN 20 subinterface and connected route.
4. Checked ARP/MAC learning and the EtherChannel state.
5. Inspected the trunk VLAN allow-list on SW1.

The EtherChannel was operational, but SW1's `Port-channel1` allowed VLAN list contained a typo:

```text
10,29-30,40,50,99
```

That list excluded VLAN 20.

### Root cause

The logical trunk did not permit VLAN 20 traffic between SW1-CORE and SW2-ACCESS. Having VLAN 20 configured on the switches and router was not sufficient: the trunk's allowed list also had to carry it.

### Correction

On SW1-CORE:

```text
configure terminal
interface port-channel 1
 switchport trunk allowed vlan 10,20,30,40,50,99
end
```

### Verification

The operator confirmed that VLAN 20 appeared in the allowed, active and forwarding VLAN list on Po1 and that connectivity was restored.

### Lesson learned

Troubleshoot VLAN failures across multiple layers: endpoint addressing, access-port VLAN, trunk state and allowed VLANs, Layer 2 MAC learning, router subinterface, and IP routing. An interface can be up while a specific VLAN is silently excluded.

---

## Controlled test: EtherChannel member failure

### Procedure

1. Save the known-good configuration and Packet Tracer file.
2. Disconnect only the SW1 `Fa0/5` ↔ SW2 `Fa0/4` cable.
3. Check `show etherchannel summary` on both switches.
4. Check `show interfaces port-channel 1` on SW1.
5. Ping PC1 and Server0 from PC0.
6. Restore the same cable.
7. Confirm both physical interfaces are bundled again.

### Observed state during failure

- SW1: `Po1(SU)`, `Fa0/2(P)`, `Fa0/5(D)`.
- SW2: `Po1(SU)`, `Fa0/1(P)`, `Fa0/4(D)`.
- SW1 reported Port-channel1 up/up, with Fa0/2 as the remaining member.

The operator confirmed that connectivity worked. The cable was restored after the test.

### Lesson learned

LACP EtherChannel provides link redundancy by treating multiple physical links as one logical link. The remaining member can keep the logical bundle operational after one member fails. This does not imply protection against every failure type; both members still share the same switches and logical bundle.
