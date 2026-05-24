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


## Steps Performed  

### Phase 1: Create the Test User in Active Directory  
**Goal:** Create the domain user ``bblack`` (Bill Black) who will be used throughout this lab.  

#### Step 1.1 - Open Active Directory Users and Computers (ADUC)  

Press the ``Windows`` key, type **Active Directory** and select **Active Directory Users and Computers** from the search results.  


#### Step 1.2 - Browse the Domain Structure  

Expand the ``mylab.local`` domain node in the left panel to see the default containers: Builtin, Computers, Domain Controllers, Users, etc.  

<img width="754" height="529" alt="1 2" src="https://github.com/user-attachments/assets/a5f8c48d-1b57-482f-9c0b-34ca87066b10" />  

#### Step 1.3 - Create a New User  

Right-click the **Users** container -> **New** -> **User**.  

<img width="861" height="565" alt="1 3" src="https://github.com/user-attachments/assets/84750cab-0592-4478-bf15-efe62038d43c" />  
#### Step 1.4 Fill in User Details  

Enter the Following details in the New Object - User wizard:  
| Field | Value |
| --- | --- |
| First name | ``Bill`` |
| Last name | ``Black`` |
| User logon name | ``bblack`` |
| Domain | ``@mylab.local`` |  

Click **Next**.  

<img width="436" height="378" alt="1 4" src="https://github.com/user-attachments/assets/f42fa7cd-7d7f-4ef2-80c0-fa796565d740" />  

#### Step 1.5 - Set Initial Password  

Set an initial password. Leave "**User must change password at next logon**" unchecked for now, this will be enforced later via the Reset Password dialog after the lockout.  

Click **Next**.  

<img width="438" height="378" alt="1 5" src="https://github.com/user-attachments/assets/120e6841-57cf-4207-bd86-095336088998" />


#### Step 1.6 - Confirm and Finish  

Review the summary screen. Confirm the user ``bblack@mylab.local`` will be created and click **Finish**.  

<img width="436" height="379" alt="1 6" src="https://github.com/user-attachments/assets/db6e6c7b-664a-4181-ad64-6fba4bf201b2" />

User ``bblack`` (Bill Black) has been created in the ``mylab.local`` domain.  


### Phase 2: Configure Account Lockout Policy via GPO  

Goal: Set a Group Policy that locks accounts after 3 failed login attempts, so we can reliably trigger a lockout for the lab scenario.  

#### Step 2.1 - Open Group Policy Management  

Press ``Windows``, type **Group Policy Management** and open it. 

<img width="758" height="635" alt="2 1" src="https://github.com/user-attachments/assets/18ccb19e-e91a-430c-87dd-6a556722fa00" />




