# Network Design 
This Cisco Packet Tracer topology models the network infrastructure supporting 
the Windows Server Active Directory lab environment.

<img width="987" height="500" alt="Screenshot 2026-07-25 at 5 59 29 PM" src="https://github.com/user-attachments/assets/6cbed739-f3bf-4e0c-9faf-afae3ce9acfc" />


## VLAN Details

| VLAN | Name | Purpose | Subnet | Devices |
|------|------|---------|--------|---------|
| 10 | Management | Domain Controller, DNS | 172.31.10.0/24 | DC-Server |
| 20 | Users | Client machines | 172.31.20.0/24 | Clients |

**Vlan 10 Switch Configuration:**

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

**Vlan 20 Switch Configuration:**
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

**Router Sub-Interface Configuration:**
```
Router>enable
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface fastethernet 0/0.10
%Invalid interface type and number
Router(config)#interface fastethernet 0/0.10
%Invalid interface type and number
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

Router#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     unassigned      YES unset  administratively down down 
GigabitEthernet0/1     unassigned      YES unset  administratively down down 
GigabitEthernet0/2     unassigned      YES unset  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface GigabitEthernet0/0.10
Router(config-subif)#encapsulation dot1Q 10
Router(config-subif)#ip address 172.31.10.1 255.255.255.0
Router(config-subif)#no shutdown
Router(config-subif)#exit
Router(config)#


Router con0 is now available


Press RETURN to get started.


Router>enable
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface Gigabitethernet 0/0.20
Router(config-subif)#encapsulation dot1Q 20
Router(config-subif)#ip adress 172.31.20.1 255.255.255.0
                          ^
% Invalid input detected at '^' marker.
	
Router(config-subif)#ip address 172.31.20.1 255.255.255.0
Router(config-subif)#no shutdown
Router(config-subif)#exit
Router(config)#interface gigabitethernet 0/0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to down

%LINK-3-UPDOWN: Interface GigabitEthernet0/0.10, changed state to down

%LINK-3-UPDOWN: Interface GigabitEthernet0/0.20, changed state to down
exit
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

Router#write memory
Building configuration...
[OK]
Router#
```

**DC-Server Configuration:**

<img width="698" height="477" alt="Screenshot 2026-07-25 at 5 37 54 PM" src="https://github.com/user-attachments/assets/a7296d35-e962-4313-b60d-c6cfb958cf24" />

**DHCP Relay Configuration:**

```
Router>enable
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitethernet 0/0.20
Router(config-subif)#ip helper-address 172.31.10.2
Router(config-subif)#exit
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console
write memory
Building configuration...
[OK]
Router#
```


## Design Draft

- **Sub-interface routing** on core router enables inter-VLAN routing
- **Management VLAN isolated** to reduce exposure of DC and DNS services
- **DHCP Relay** configured on VLAN 20 to relay client requests to DC DHCP
- **Trunk ports** allow tagged VLAN traffic between switches and router

## Address Scheme

All addressing matches AWS EC2 private IP scheme (172.31.x.x):
- Router: 172.31.10.1/24 (gateway)
- DC (Management VLAN): 172.31.10.2
- Clients (User VLAN): 172.31.20.10–20.11

## Network Validation

✓ All devices reachable via ping
✓ VLAN isolation verified (cross-VLAN traffic blocked by default)
✓ DHCP relay functional (clients receive addresses from DC)
✓ Routing table shows both VLANs

## Files

- `network_topology.pkt` — Cisco Packet Tracer topology

## Mapping Topology to Real AD Lab

In the actual AWS environment:
- DC Server runs Windows Server 2022 with Active Directory
- Clients join corp.local domain
- DHCP relay ensures clients can locate DC across network segments
- VLAN separation models security best practice (admin/user separation)
