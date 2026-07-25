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
