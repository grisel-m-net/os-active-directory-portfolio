# Lab 2: Active Directory Users & Groups

## Objective
Create OUs, users, and security groups with proper membership.

## Configuration
- OUs: Sales, IT, Groups
- Users: 3 total (jsmith, jdoe, tadmin)
- Groups: SalesTeam (2 members), ITTeam (1 member)

## Inventory
| User | OU | Group | Role |
|------|----|----|------|
| jsmith | Sales | SalesTeam | User |
| jdoe | Sales | SalesTeam | User |
| tadmin | IT | ITTeam | Domain Admin |

## Verification
All users can authenticate with domain credentials (corp\username).

## Screenshots 

users, groups, OUs

<img width="877" height="569" alt="Screenshot 2026-07-24 at 6 16 55 PM" src="https://github.com/user-attachments/assets/195e145f-c089-42e9-8eb5-26d82f522cf1" />
<img width="870" height="549" alt="Screenshot 2026-07-24 at 6 17 02 PM" src="https://github.com/user-attachments/assets/87ccd19f-37ba-4667-b58f-97c2b124c42b" />
<img width="862" height="548" alt="Screenshot 2026-07-24 at 6 17 22 PM" src="https://github.com/user-attachments/assets/455c88cd-e2c7-4150-8ce5-bec83151ae2e" />
<img width="844" height="541" alt="Screenshot 2026-07-24 at 6 17 30 PM" src="https://github.com/user-attachments/assets/f8911e97-adc4-4c81-a1c7-3d2f5c9b98e6" />
