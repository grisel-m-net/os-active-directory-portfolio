# Network Design & Validation

This Cisco Packet Tracer topology models the network infrastructure supporting 
the Windows Server Active Directory lab environment.

## VLAN Design

| VLAN | Name | Purpose | Subnet | Devices |
|------|------|---------|--------|---------|
| 10 | Management | Domain Controller, DNS | 172.31.10.0/24 | DC-Server |
| 20 | Users | Client machines | 172.31.20.0/24 | Clients |

## Key Design Decisions

- **Sub-interface routing** on core router enables inter-VLAN routing
- **Management VLAN isolated** to reduce exposure of DC and DNS services
- **DHCP Relay** configured on VLAN 20 to relay client requests to DC DHCP
- **Trunk ports** allow tagged VLAN traffic between switches and router

## Addressing Strategy

All addressing matches AWS EC2 private IP scheme (172.31.x.x):
- Router: 172.31.1.1 (gateway)
- DC (Management VLAN): 172.31.10.2
- Clients (User VLAN): 172.31.20.10–20.11

## Network Validation

✓ All devices reachable via ping
✓ VLAN isolation verified (cross-VLAN traffic blocked by default)
✓ DHCP relay functional (clients receive addresses from DC)
✓ Routing table shows both VLANs

## Files

- `network_topology.pkt` — Cisco Packet Tracer topology
- `network_topology.png` — Network diagram export

## Mapping to Real AD Lab

In the actual AWS environment:
- DC Server runs Windows Server 2022 with Active Directory
- Clients join corp.local domain
- DHCP relay ensures clients can locate DC across network segments
- VLAN separation models security best practice (admin/user separation)
