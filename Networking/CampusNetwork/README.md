# CampusNetwork

**Status:** In Progress

A multi-building university campus network, designed and simulated in Cisco Packet Tracer. The project covers VLAN segmentation, wireless and wired access, inter-VLAN routing, DHCP, and access control across seven buildings with different connectivity requirements.

## Objective

Design a network that reflects how a real university campus is actually built — multiple buildings, each with different access needs (wired labs, staff Wi-Fi, student Wi-Fi, restricted admin access), all routed through a central core rather than isolated per building.

This project is focused on building a solid understanding of:

- **Subnetting & IP addressing** across multiple segments
- **VLANs** for logical separation of traffic within and across buildings
- **Inter-VLAN routing** via a core router/Layer 3 switch
- **DHCP** scoped per VLAN
- **Wireless configuration** — separate SSIDs and security levels per user group
- **Access Control Lists (ACLs)** to restrict traffic between segments
- **ARP behavior, switch MAC address tables, and broadcast domain boundaries**

## Scenario

The university has 7 buildings, each with its own access requirements:

| Building | Type | Connectivity |
|---|---|---|
| Library | Academic/Shared | Staff Wi-Fi (restricted) + Student Wi-Fi (open) |
| Academic Block A | Classrooms + Computer Lab | Wired Ethernet (Lab) + Student Wi-Fi |
| Academic Block B | Classrooms | Student Wi-Fi only |
| Academic Block C | Classrooms + Faculty offices | Student Wi-Fi + Faculty Wi-Fi (restricted) |
| Admin Block | Administration | Wired only, restricted (finance, records, HR) |
| Hostel Block | Student residence | Student Wi-Fi (isolated from academic VLANs) |
| Sports Complex | Recreational | Guest Wi-Fi (open, internet-only) |

## Topology

```
                                     ┌─────────────────────┐
                                     │   Core Router /       │
                                     │   L3 Distribution     │
                                     │   Switch (Backbone)   │
                                     └───────────┬────────────┘
        ┌───────────┬──────────────┬─────────────┼──────────────┬──────────────┬──────────────┐
        │            │              │             │              │              │
   ┌────▼───┐   ┌────▼───┐    ┌─────▼────┐  ┌─────▼─────┐  ┌─────▼─────┐  ┌─────▼─────┐  ┌─────▼─────┐
   │Library  │   │Block A │    │ Block B  │  │ Block C   │  │ Admin      │  │ Hostel     │  │ Sports     │
   │Switch+WAP│  │Switch+WAP│  │Switch+WAP│  │Switch+WAP │  │ Switch     │  │Switch+WAP  │  │ WAP only   │
   └─────────┘   └─────────┘   └──────────┘  └───────────┘  └────────────┘  └────────────┘  └────────────┘
```

Each building connects back to the core over a trunk link carrying only the VLANs relevant to that building.

## VLAN Plan

| VLAN ID | Name | Subnet | Building | Access Type |
|---|---|---|---|---|
| 10 | Library-Staff | 192.168.10.0/24 | Library | Wi-Fi, restricted |
| 20 | Library-Student | 192.168.20.0/24 | Library | Wi-Fi, open |
| 30 | BlockA-Lab | 192.168.30.0/24 | Block A | Wired only |
| 40 | BlockA-Student | 192.168.40.0/24 | Block A | Wi-Fi, open |
| 50 | BlockB-Student | 192.168.50.0/24 | Block B | Wi-Fi, open |
| 60 | BlockC-Student | 192.168.60.0/24 | Block C | Wi-Fi, open |
| 70 | BlockC-Faculty | 192.168.70.0/24 | Block C | Wi-Fi, restricted |
| 80 | Admin | 192.168.80.0/24 | Admin Block | Wired, restricted |
| 90 | Hostel-Student | 192.168.90.0/24 | Hostel | Wi-Fi, open |
| 100 | Guest-Sports | 192.168.100.0/24 | Sports Complex | Wi-Fi, open/isolated |
| 999 | Mgmt/Backbone | 10.10.0.0/24 | Core | Wired, admin-only |

Full breakdown (host ranges, gateways, DHCP pools) is in [`docs/ip-addressing-plan.md`](./docs/ip-addressing-plan.md).

## Access Control Summary

| Rule | Behavior |
|---|---|
| Guest-Sports (VLAN 100) | Internet only — no access to any other VLAN |
| Hostel-Student (VLAN 90) | No access to Admin, Library-Staff, or Block C-Faculty |
| Student VLANs (20/40/50/60/90) | No access to Admin (80), Faculty (70), or Library-Staff (10) |
| Admin (VLAN 80) | Fully restricted — no inbound access from any student/guest VLAN |
| Mgmt (VLAN 999) | Reachable only from designated admin devices |

Full ACL definitions are in [`configs/acl/`](./configs/acl/).

## Folder Structure

```
CampusNetwork/
├── README.md                      # This file
├── topology/
│   └── campus-network.pkt         # Main Packet Tracer file
├── docs/
│   ├── ip-addressing-plan.md      # Full subnet/host breakdown
│   ├── build-notes.md             # Build log and observations
│   └── verification-tests.md      # Ping/ARP/DHCP test results
├── configs/
│   ├── router/
│   │   └── core-router.txt        # Router subinterfaces, DHCP pools
│   ├── switches/
│   │   ├── library-switch.txt
│   │   ├── blockA-switch.txt
│   │   ├── blockB-switch.txt
│   │   ├── blockC-switch.txt
│   │   ├── admin-switch.txt
│   │   └── hostel-switch.txt
│   ├── wireless/
│   │   └── wap-ssid-config.txt    # SSID + security settings per building
│   └── acl/
│       └── inter-vlan-acls.txt    # ACL rules between VLANs
└── screenshots/
    ├── mac-address-table.png
    ├── arp-table.png
    └── dhcp-verification.png
```

## Build Order

1. Physical topology — place routers, switches, WAPs, end devices per building
2. IP addressing plan — document subnets before configuring anything
3. VLANs on each building's switch
4. Core router — subinterfaces (router-on-a-stick or L3 SVIs) + DHCP pools
5. Wireless — SSIDs and security per building/user group
6. ACLs — restrict cross-VLAN access per the rules above
7. Test and document — ARP tables, MAC address tables, DHCP leases, ping tests (allowed/blocked)

## Status

- [x] Building/VLAN plan finalized
- [x] Physical topology laid out
- [ ] Switch VLAN configuration
- [ ] Core router inter-VLAN routing + DHCP
- [ ] Wireless SSID/security setup
- [ ] ACL implementation
- [ ] Verification testing and screenshots
- [ ] Final write-up

## Tools Used

- Cisco Packet Tracer

---

Part of the [Projects Portfolio](../../) repository.
