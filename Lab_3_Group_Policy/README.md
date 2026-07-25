# Lab 3: Group Policy & Security

## Objective
Create and apply Group Policy Object for password security.

## GPO Configuration
- Name: Sales-Password-Policy
- Applied to: Sales OU
- Policy settings:
  - Minimum password length: 12 characters
  - Password expiration: 90 days
  - Password history: 5 passwords

## Verification
- GPO linked to Sales OU
- Client (win-client) domain-joined
- gpupdate /force applied successfully
- GPO report confirms policies applied
