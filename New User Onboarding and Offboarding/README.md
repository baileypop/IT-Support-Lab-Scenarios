# New User Onboarding and Offboarding  

Author: Bailey Popovic  

Domain: Windows Server - Active Directory, Device Management, Software Deployment, Access Control  

Environment: Windows Server 2022 (Domain Controller), Windows 11 Client VM, Active Directory Users and Computers, [domain] domain  

## Objective  

Run a complete end-to-end support workflow for a new starter and a departing employee.  
Provision a user account in Active Directory, create a department security group, join a device to the domain, install required software, configure email access, map a shared network drive, and produce a Day One handover note.  
Then execute a full offboarding workflow covering account disabling, group removal, password reset, device collection, and compliance documentation.  

## Business Scenario  

**Onboarding:** HR has submitted a request for Danita Widmar, a new Finance Department employee starting on June 15, 2026. She needs a domain account, department group membership, a laptop joined to the domain, Microsoft Office, Teams, and Google Chrome installed, and access to the Finance shared drive. The device must be ready before her first day.

**Offboarding:** Two weeks later, Danita Widmar resigns. Her last day is June 30th. Her manager, Esther Jordan, submits an offboarding request on June 29th requiring account disabling, group membership removal, password reset, laptop and headset collection, and a data wipe before the device is redeployed.  


## Environment and Tools Used  

| Component | Detail |
| --- | --- |
| Domain Controller OS | Windows Server 2022 |
| Domain Name | [domain] |
| Client Machine | WIN11_CLIENT01 |
| Tool | Active Directory Users and Computers (ADUC) |
| User Account | danita.widmar@[domain] |
| Department Group | Finance_Department (Global, Security) |
| Software Installed | Microsoft Teams, Office, Google Chrome, Microsoft 365 | 
| Shared Drive | \[dc_ip]\Finance_Share - mapped as X: |
| Email Workflow | Microsoft 365 license assignment (documented workflow) |  

## Onboarding and Offboarding Process Map  

Every step in both workflows maps to a security or access control requirement. Skipping any step either creates a gap in access provisioning or leaves a security exposure when a user departs.  


```
ONBOARDING WORKFLOW
  Step 1 — Receive and log onboarding ticket
       ↓
  Step 2 — Create AD user account (identity layer)
       ↓
  Step 3 — Create department group and assign membership (access layer)
       ↓
  Step 4 — Join device to domain (device layer)
       ↓
  Step 5 — Document email access workflow (communication layer)
       ↓
  Step 6 — Install required software (productivity layer)
       ↓
  Step 7 — Create shared folder and map network drive (resource access layer)
       ↓
  Step 8 — Produce Day One handover note (handover layer)
       ↓
  Onboarding complete — user ready for first day

OFFBOARDING WORKFLOW
  Step 1 — Receive and log offboarding ticket (with manager authorisation)
       ↓
  Step 2 — Disable AD account (blocks all authentication immediately)
       ↓
  Step 3 — Remove group memberships (removes resource access)
       ↓
  Step 4 — Reset password (prevents account reactivation by ex-user)
       ↓
  Step 5 — Collect device and complete hardware checklist (physical security)
       ↓
  Step 6 — Confirm data wipe required and schedule reimaging (data security)
       ↓
  Offboarding complete — access fully removed, device secured
```











