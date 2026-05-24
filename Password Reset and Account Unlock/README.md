# Password Reset and Account Unlock  

Author: Bailey Popovic  
Domain: Active Directory Administration  
Environment: Windows Server 2022 + Windows 11 Client (VM)  

## Objective  

Reset a forgotten password for a domain user account, unlock a locked-out Active Directory account and enforce a mandatory password change at the user's next sign in. All within Active Directory Domain Services environment.  


## Business Scenario  

**Ticket #089 | Priority: HIGH | Reported 11:02 AM**  

A user (Bill Black, login: ``bblack@mylab.local``) reports they cannot log in to their workstation ahead of a critical 11:30 AM meeting. Multiple failed password attempts have triggered an account lockout. The user needs access restored immediately.  

## Environemt and Tools Used  

| Component | Details |
| --- | --- |  
| Domain Controller OS | Windows Server 2022 Evaluation |
| Client OS | Windows 11 |
| Domain | ``mylab.local`` |
| Tool: ADUC | Active Directory Users and Computers |
| Tool: GPMC | Group Policy Management Console |
| Tool: CMD | Command Prompt (Administrator) - ``gpupdate /force`` |
| Deployment | VirtualBox local lab |  


