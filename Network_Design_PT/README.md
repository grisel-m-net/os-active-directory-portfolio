# Network Design 
This Cisco Packet Tracer topology models the network infrastructure supporting 
the Windows Server Active Directory lab environment.

## VLAN Details

| VLAN | Name | Purpose | Subnet | Devices |
|------|------|---------|--------|---------|
| 10 | Management | Domain Controller, DNS | 172.31.10.0/24 | DC-Server |
| 20 | Users | Client machines | 172.31.20.0/24 | Clients |

Vlan 10 Switch Configuration: 

```
Switch Ports Model              SW Version            SW Image
------ ----- -----              ----------            ----------
*    1 26    WS-C2960-24TT-L    15.0(2)SE4            C2960-LANBASEK9-M

Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2013 by Cisco Systems, Inc.
Compiled Wed 26-Jun-13 02:49 by mnguyen



Press RETURN to get started!



Switch>enable
Switch#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#name management
                ^
% Invalid input detected at '^' marker.
	
Switch(config)#vlan 10
Switch(config-vlan)#name managemenr
Switch(config-vlan)#exit
Switch(config)#interface range fastethernet 0/1-23
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport mode access vlan 10
                                               ^
% Invalid input detected at '^' marker.
	
Switch(config-if-range)#switchport access vlan 10
Switch(config-if-range)#exit
Switch(config)#interface fastethernet 0/24
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 10,20
Switch(config-if)#exit
Switch(config)#exit
Switch#
%SYS-5-CONFIG_I: Configured from console by console
write memory
Building configuration...
[OK]
Switch#
```
Vlan 20 Switch Configuration: 
```
Switch Ports Model              SW Version            SW Image
------ ----- -----              ----------            ----------
*    1 26    WS-C2960-24TT-L    15.0(2)SE4            C2960-LANBASEK9-M

Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2013 by Cisco Systems, Inc.
Compiled Wed 26-Jun-13 02:49 by mnguyen



Press RETURN to get started!



Switch>enable
Switch#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#vlan 20
Switch(config-vlan)#name Users
Switch(config-vlan)#exit
Switch(config)#interfeace range fastethernet 0/1-23
                     ^
% Invalid input detected at '^' marker.
	
Switch(config)#interface range fastethernet 0/1-23
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 20
Switch(config-if-range)#exit
Switch(config)#interface fastethernet 0/24
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 10,20
Switch(config-if)#exit
Switch(config)#exit
Switch#
%SYS-5-CONFIG_I: Configured from console by console
write memory
Building configuration...
[OK]
Switch#
```



## Design Draft

- **Sub-interface routing** on core router enables inter-VLAN routing
- **Management VLAN isolated** to reduce exposure of DC and DNS services
- **DHCP Relay** configured on VLAN 20 to relay client requests to DC DHCP
- **Trunk ports** allow tagged VLAN traffic between switches and router

## Address Scheme

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

## Mapping Topology to Real AD Lab

In the actual AWS environment:
- DC Server runs Windows Server 2022 with Active Directory
- Clients join corp.local domain
- DHCP relay ensures clients can locate DC across network segments
- VLAN separation models security best practice (admin/user separation)
