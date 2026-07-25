## Lab 3: Group Policy Configuration

Created GPO "Sales-Password-Policy" on Domain Controller:
- Minimum password length: 12 characters
- Password expiration: 90 days
- Password history: 5 passwords

Linked to Sales OU for enforcement.

<img width="923" height="525" alt="Screenshot 2026-07-24 at 9 26 27 PM" src="https://github.com/user-attachments/assets/2f772de9-17cf-4196-910c-77b149ae7995" />


In production, domain-joined clients would receive these policies via gpupdate /force 
and verify via gpresult /h report.html.
