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

## Onboarding Workflow  

### Phase 1: Receive and Log Onboarding Request  
#### Step 1.1 - Review the onboarding ticket  

The onboarding ticket is received from HR with all the required info. Before any provisioning, I have confirmed the ticket contains a start date, department, manager name, required applications, and hardware requirements.  

<img width="482" height="391" alt="1 1" src="https://github.com/user-attachments/assets/f9edec8e-df0f-46f8-9f37-e632ea79b765" />  

### Phase 2: Create the Active Directory User Account

#### Step 2.1 - Create the User Account in ADUC

Open Active Directory Users and Computers on the domain controller. Navigate to the appropriate Organisational Unit (in this lab, [domain]/Users). Right-click and select New > User. Complete all required fields following the organization's naming convention.  

<img width="434" height="374" alt="2 1" src="https://github.com/user-attachments/assets/3497231e-78e0-4ff3-864e-03f89425467e" />  



### Phase 3: Create the Department Security Group and Assign Membership  

#### Step 3.1 - Create the Finance_Department Security Group  

Navigate to the Users container in ADUC. Right-click and select New > Group. Create a Global Security group named Finance_Department.  

<img width="432" height="374" alt="3 1" src="https://github.com/user-attachments/assets/021b2682-938f-40bc-84b8-6ca01bacbe0d" />


#### Step 3.2 - Add Danita Widmar to the Finance_Department Group  

Open Danita Widmar's account properties and navigate to the Member Of tab. Click Add and search for Finance_Department.  

<img width="456" height="248" alt="3 2" src="https://github.com/user-attachments/assets/4ad4f14d-2ff6-4b84-a037-3a9324ab0f0d" />  


#### Step 3.3 - Confirm Group Membership  

Return to the Member Of tab on Danita Widmar's account properties and confirm both group memberships are present: Domain Users (default) and Finance_Department (newly assigned).  

<img width="407" height="536" alt="3 3" src="https://github.com/user-attachments/assets/c198e495-8820-4aae-8a5c-b976efb05bc0" />  


### Phase 4: Join the Device to the Domain  

#### Step 4.1 - Configure Domain Join on WIN11_CLIENT01 


On the Windows 11 client machine, open System Properties (sysdm.cpl) and click Change. Select Domain and enter [domain]. Provide domain administrator credentials when prompted.

<img width="322" height="388" alt="4 1" src="https://github.com/user-attachments/assets/5b932349-5ecb-47bf-bc02-462c1b050b60" />  


#### Step 4.2 - Confirm Domain Join in System Properties  

After the restart, open System Properties and confirm the machine shows the full computer name and domain.  

<img width="407" height="465" alt="4 2" src="https://github.com/user-attachments/assets/0b59249b-58c0-417b-a698-cc7ce15ba92b" />  












