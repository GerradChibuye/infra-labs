# Operation Blackout — Configuration Reference Guide

## Overview

This directory documents the principal configuration components of the Operation Blackout enterprise campus network.

The configuration references cover the router and three switches, including VLAN segmentation, inter-VLAN routing, DHCP, trunking, LACP EtherChannel, Spanning Tree Protocol, SSH management, and access-port security.

The references summarize relevant configuration excerpts and verified design details. They are intended to support technical review and understanding of the project.

## Device Configuration References

| Device | Primary Responsibilities | Reference |
|---|---|---|
| R1-BLACKOUT | Inter-VLAN routing and DHCP | [R1-BLACKOUT](R1-BLACKOUT.md) |
| SW1-CORE | Core switching, trunking, EtherChannel, STP root selection | [SW1-CORE](SW1-CORE.md) |
| SW2-ACCESS | Administration and Finance access, EtherChannel, port security | [SW2-ACCESS](SW2-ACCESS.md) |
| SW3-ACCESS | IT and Staff access, trunking, port security | [SW3-ACCESS](SW3-ACCESS.md) |

## Configuration Principles

The implementation follows these design principles:

- Departmental traffic is separated into dedicated VLANs.
- Router subinterfaces provide the default gateways for the configured VLANs.
- DHCP supplies addressing to departmental clients.
- Trunk links carry the VLANs required by the topology.
- LACP combines two physical links between the core and an access switch.
- PVST is used for Layer 2 loop prevention and root-bridge selection.
- SSH provides remote management access to the switches.
- Port security restricts the number of learned MAC addresses on selected end-user ports.
- Selected unused access ports are administratively disabled.

## Scope and Accuracy

These documents are sanitized configuration references, not complete device backups or import-ready configurations. Device credentials, password hashes, and unnecessary learned MAC addresses are omitted.

The Cisco Packet Tracer project file remains the primary artifact representing the implemented network.
