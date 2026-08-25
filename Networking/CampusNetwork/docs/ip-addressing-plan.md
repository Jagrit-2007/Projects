# IP Addressing Plan

## Subnet Breakdown

| VLAN | Name | Network | Subnet Mask | Usable Range | Gateway | Broadcast |
|---|---|---|---|---|---|---|
| 10 | Library-Staff | 192.168.10.0/24 | 255.255.255.0 | .1 – .254 | 192.168.10.1 | 192.168.10.255 |
| 20 | Library-Student | 192.168.20.0/24 | 255.255.255.0 | .1 – .254 | 192.168.20.1 | 192.168.20.255 |
| 30 | BlockA-Lab | 192.168.30.0/24 | 255.255.255.0 | .1 – .254 | 192.168.30.1 | 192.168.30.255 |
| 40 | BlockA-Student | 192.168.40.0/24 | 255.255.255.0 | .1 – .254 | 192.168.40.1 | 192.168.40.255 |
| 50 | BlockB-Student | 192.168.50.0/24 | 255.255.255.0 | .1 – .254 | 192.168.50.1 | 192.168.50.255 |
| 60 | BlockC-Student | 192.168.60.0/24 | 255.255.255.0 | .1 – .254 | 192.168.60.1 | 192.168.60.255 |
| 70 | BlockC-Faculty | 192.168.70.0/24 | 255.255.255.0 | .1 – .254 | 192.168.70.1 | 192.168.70.255 |
| 80 | Admin | 192.168.80.0/24 | 255.255.255.0 | .1 – .254 | 192.168.80.1 | 192.168.80.255 |
| 90 | Hostel-Student | 192.168.90.0/24 | 255.255.255.0 | .1 – .254 | 192.168.90.1 | 192.168.90.255 |
| 100 | Guest-Sports | 192.168.100.0/24 | 255.255.255.0 | .1 – .254 | 192.168.100.1 | 192.168.100.255 |
| 999 | Mgmt/Backbone | 10.10.0.0/24 | 255.255.255.0 | .1 – .254 | 10.10.0.1 | 10.10.0.255 |

## DHCP Pool Reservations

Reserve the low end of each range for static assignments (gateways, WAPs, switch management), and DHCP-serve the rest.

| VLAN | Static Reserved | DHCP Pool |
|---|---|---|
| 10 | .1 – .10 | .11 – .254 |
| 20 | .1 – .10 | .11 – .254 |
| 30 | .1 – .10 | .11 – .254 (or fully static, since it's a fixed lab) |
| 40 | .1 – .10 | .11 – .254 |
| 50 | .1 – .10 | .11 – .254 |
| 60 | .1 – .10 | .11 – .254 |
| 70 | .1 – .10 | .11 – .254 |
| 80 | .1 – .20 | Static only (Admin devices should not use DHCP) |
| 90 | .1 – .10 | .11 – .254 |
| 100 | .1 – .10 | .11 – .254 |
| 999 | All static | N/A |

## Notes

- All subnets use a `/24` for simplicity. If you want to practice VLSM, this is a good place to subnet further (e.g. the Admin block likely doesn't need 254 hosts).
- The Mgmt/Backbone VLAN (999) is on a separate `10.10.0.0/24` range, intentionally distinct from the `192.168.x.0/24` scheme, to make it obvious at a glance when you're looking at infrastructure traffic vs. user traffic.
- Update this table as the DHCP pools are actually configured in Packet Tracer — this is the plan, not necessarily the final state.
