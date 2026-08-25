# Verification Tests

Documented results for the checks that prove the network is behaving as designed.

## 1. Connectivity Within a VLAN

| From | To | Expected | Result |
|---|---|---|---|
| BlockA-Lab PC1 | BlockA-Lab PC2 | Success | |
| Library-Student device | Library-Student device | Success | |

## 2. Connectivity Across VLANs (Should Be Blocked)

| From | To | Expected | Result |
|---|---|---|---|
| Hostel-Student | Admin | Blocked | |
| Guest-Sports | Any internal VLAN | Blocked | |
| BlockB-Student | BlockC-Faculty | Blocked | |

## 3. Connectivity Across VLANs (Should Be Allowed, e.g. Internet/Shared Services)

| From | To | Expected | Result |
|---|---|---|---|
| Any Student VLAN | Internet (simulated) | Success | |

## 4. DHCP Verification

| VLAN | Device | Expected IP Range | Result |
|---|---|---|---|
| 40 (BlockA-Student) | Laptop1 | 192.168.40.11–.254 | |
| 90 (Hostel-Student) | Laptop2 | 192.168.90.11–.254 | |

## 5. ARP Table Checks

Run `arp -a` on end devices after communication and confirm entries.

| Device | Observation |
|---|---|
| | |

## 6. Switch MAC Address Table

Run `show mac address-table` on each access switch after devices communicate.

| Switch | Observation |
|---|---|
| Library-Switch | |
| BlockA-Switch | |

## 7. Broadcast Domain Isolation

Confirm an ARP broadcast (`FF:FF:FF:FF:FF:FF`) sent in one VLAN does not appear in another VLAN's traffic capture.

| Test | Result |
|---|---|
| ARP broadcast in VLAN 40 not seen in VLAN 50 | |
