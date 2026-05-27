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

**The DNS server on this client must point to the domain controller IP address for the domain join to resolve correctly.**  


#### Step 4.2 - Confirm Domain Join in System Properties  

After the restart, open System Properties and confirm the machine shows the full computer name and domain.  

<img width="407" height="465" alt="4 2" src="https://github.com/user-attachments/assets/0b59249b-58c0-417b-a698-cc7ce15ba92b" />  


#### Step 4.3 - Verify Device Appears in ADUC Computers Container  

Return to Active Directory Users and Computers on the domain controller. Navigate to the Computers container and confirm WIN11_CLIENT01 appears as a Computer object.  

<img width="753" height="529" alt="4 3" src="https://github.com/user-attachments/assets/7f5c03d4-e96a-4814-8518-c5a2c68bf935" />  

### Phase 5: Configure Email Access  

#### Step 5.1 - Document the Microsoft 365 Email Provisioning Workflow  

Email provisioning in Microsoft 365 requires actions in the M365 admin centre rather than in Active Directory. The workflow was documented but not executed in this lab VM because a live M365 tenant was not available in the test environment.  

| Step | Action | Performed by |
| --- | --- | ---|
| 1 | IT submits license request to M365 admin | IT Support |
| 2 | Admin assigns Microsoft 365 license to danita.widmar@[domain] | M365 Admin |
| 3 | Exchange Online mailbox created automatically | automatic |
| 4 | IT verifies access via Outlook or OWA | IT Support |
| 5 | Confirmation sent to manager Esther Jordan | IT Support |  


### Phase 6: Install Required Software  

#### Step 6.1 - Install Microsoft Teams, WPS Office, and Supporting Applications  

Install all applications listed on the onboarding ticket. Verify each installation in Settings > Apps > Installed Apps.

### Phase 7: Create Shared Folder and Map Network Drive  

#### Step 7.1 - Confirm Finance_Share Folder is Shared on the File Server  

On the file server, navigate to the Finance_Share folder properties and confirm it is shared with the correct network path.  

<img width="362" height="477" alt="7 1" src="https://github.com/user-attachments/assets/a6161261-a196-435f-88f1-0be0ba096f55" />  


#### Step 7.2 - Map the Network Drive on the Client Machine  

On WIN11_CLIENT01, map the Finance_Share as a persistent network drive. The drive is mapped to X: from \[dc_ip]\Finance_Share.  

<img width="785" height="592" alt="7 2" src="https://github.com/user-attachments/assets/bb44952c-1ace-4001-ac88-b3e1912a9221" />  


### Phase 8: Produce the Day One Handover Note  

#### Step 8.1 - Create the Handover Document for the New User  

Before the user's first day, produce a handover note summarizing everything that has been provisioned. This document is given to the user and a copy is attached to the ticket.  

<img width="493" height="634" alt="8 1" src="https://github.com/user-attachments/assets/84db03aa-94c1-4fc1-b649-8656e2171b66" />  

## Offboarding  

### Phase 9: Receive and Log the Offboarding Request  

#### Step 9.1 - Review the Offboarding Ticket

The offboarding request is submitted by Esther Jordan (manager) on June 29 2026. Last day of employment is June 30 2026. All actions must be completed by end of business on the last day.  

<img width="477" height="485" alt="9 1" src="https://github.com/user-attachments/assets/c7f439e7-ef3d-4320-974b-5f52c1e0720a" />  

### Phase 10: Disable the Active Directory Account  

#### Step 10.1 - Disable Danita Widmar's Account in ADUC  

Right-click on Danita Widmar's account in ADUC and select Disable Account. Confirm the action when prompted.  

<img width="308" height="149" alt="10 1" src="https://github.com/user-attachments/assets/d3c74b92-30a7-4a68-8bb5-fcc044c8e985" />  


### Phase 11: Remove Group Memberships  

#### Step 11.1 - Remove Danita Widmar from Finance_Department Group  

Open the Finance_Department group properties. Navigate to the Members tab. Select Maria Shima and click Remove. Confirm when prompted.

<img width="395" height="448" alt="11 1" src="https://github.com/user-attachments/assets/b5af621f-fc89-40ec-a6b0-5b84c004cace" />  


### Phase 12: Reset the Password  

#### Step 12.1 - Reset Password and Configure Account Options  

Open Danita Widmar's account properties in ADUC and navigate to the Account tab. Set a strong temporary password and ensure the User must change password at next logon checkbox is ticked. This prevents the account from being reactivated with the original credentials if it is accidentally re-enabled in the future.

<img width="412" height="538" alt="12 1" src="https://github.com/user-attachments/assets/3d627394-829e-41fe-90bb-423158b3919f" />  


### Phase 13: Collect the Device and Complete the Hardware Checklist

#### Step 13.1 - Complete the Device Collection Checklist

Before the device is wiped and redeployed, complete a physical hardware checklist confirming what was returned, the condition of each item, and who collected it.  

<img width="510" height="571" alt="13 1" src="https://github.com/user-attachments/assets/0d3afdb5-c326-46ae-9740-54294a3abb83" />  


## Troubleshooting Reference  

| Situation | Action | Common Mistake |
| --- | --- | --- |
| New user cannot log in on first day | Confirm account is enabled, password not expired, and device is domain joined | Checking the laptop before checking the AD account |
| Domain join fails during device setup | Confirm DNS on the client points to the domain controller, not a public DNS server | Attempting to join domain with public DNS (8.8.8.8) configured |
| User cannot access Finance_Share | Confirm Finance_Department group membership and NTFS permissions on the share | Checking only the share permissions, ignoring NTFS permissions |
| Offboarding request missing manager authorization | Return the ticket to the requester — do not disable the account without confirmed authorization | Disabling account based on verbal instruction without written authorization |
| Account disabled but user still has active sessions | Force sign-out via Active Directory or M365 admin centre - disabling AD blocks new logins but does not terminate active sessions | Assuming account disable ends all active sessions immediately |
| Device not visible in ADUC after domain join | Confirm the domain join completed and a restart occurred - computers appear in ADUC after first reboot | Looking in ADUC before the client machine has rebooted |
| Shared drive not accessible after mapping | Confirm group membership is active and NTFS permissions include the security group — mapping a drive without correct permissions shows the drive but denies access | Mapping the drive letter correctly but assigning permissions to the individual user instead of the group |  







