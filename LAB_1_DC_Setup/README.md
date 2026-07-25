# Lab 1: Domain Controller Setup

## Objective
Install Windows Server 2022 and promote to Active Directory Domain Controller.

## Steps Completed
1. Launched t3.micro EC2 instance (40GB storage)
2. Set static private IP: 10.0.0.10
3. Installed Active Directory Domain Services role
4. Promoted server to Domain Controller (corp.local forest)
5. Verified AD installation with PowerShell

## Deliverables
- DC fully operational
- Domain: corp.local
- DSRM password configured

## Screenshots

Here's the Get-ADDomain output confirming the DC is set up:

<img width="1026" height="684" alt="dc_serv_IP2" src="https://github.com/user-attachments/assets/e8ee3be4-46bc-4ebf-93ae-64eb28d72fbe" />
<img width="1364" height="819" alt="Screenshot 2026-07-24 at 5 35 32 PM" src="https://github.com/user-attachments/assets/80f51b75-3488-4ffd-a924-d33f6eaacaa5" />
<img width="995" height="739" alt="get-addomain" src="https://github.com/user-attachments/assets/96bb9d83-9cf6-4be3-a5b8-1e807ed58360" />




