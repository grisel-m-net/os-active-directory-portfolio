# Active Directory Infrastructure Lab

Full stack nfrastructure project demonstrating network design, Windows Server deployment, 
Active Directory administration, and security policy configuration. Combines Cisco Packet Tracer 
network modeling with real AWS EC2 Active Directory implementation.

## Network Architecture

**VLAN Design:**
- **VLAN 10 (Management):** Domain Controller, DNS services (172.31.10.0/24)
- **VLAN 20 (Users):** Client machines, workstations (172.31.20.0/24)
- **DHCP Relay:** Clients in VLAN 20 receive addresses from DC via relay

## Labs

- **Network Design:** Cisco Packet Tracer topology with VLAN segmentation, inter-VLAN routing, and DHCP relay
- **Lab 1:** Windows Server 2022 installation and promotion to Domain Controller
- **Lab 2:** Create Organizational Units, users, and security groups
- **Lab 3:** Create and apply Group Policy for password security

## Skills Demonstrated

- **Networking:** VLAN design, sub-interface routing, DHCP relay, trunk ports
- **Infrastructure:** OS installation, EC2 configuration
- **Active Directory:** DC promotion, OU structure, user/group management
- **Security:** Group Policy Objects (GPO), password policies, access control
- **Troubleshooting:** Connectivity validation, DNS verification, policy application

## Architecture

**Network Layer (Packet Tracer):**
- Core Router with sub-interface routing (172.31.1.1)
- VLAN 10 Switch (Management) with DC (172.31.10.2)
- VLAN 20 Switch (Users) with clients (172.31.20.10–11)

**Infrastructure (AWS EC2):**
- DC Server (172.31.10.2): Domain Controller running Windows Server 2022
- Client (172.31.20.10): Windows Server 2022 domain-joined client
- Domain: corp.local
- Users: 3 (jsmith, jdoe, tadmin)
- Groups: 2 (SalesTeam, ITTeam)
- OUs: Sales, IT, Groups


<img width="1606" height="289" alt="Screenshot 2026-07-24 at 6 58 28 PM" src="https://github.com/user-attachments/assets/d6ee4bdd-efc4-4ea5-90c3-27101b87fd4c" />
