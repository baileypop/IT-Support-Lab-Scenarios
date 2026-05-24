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


#### Step 2.2 - Navigate to Account Lockout Policy  

In the Group Policy Management console, navigate to:  
```
Forest: mylab.local  
    └── Domains  
          └── mylab.local   
                └── Default Domain Policy [Right-click -> Edit]
```  
Then, in the Group Policy Management Editor:  

```
Computer Configuration  
  └── Policies  
        └── Windows Settings  
              └── Security Settings  
                    └── Account Policies  
                          └── Account Lockout Policy
```  
The final configured policy values are shown below:  

<img width="631" height="534" alt="2 2" src="https://github.com/user-attachments/assets/b7a334fc-a18d-4202-9f45-692f8f87758b" />  

#### Step 2.3 - Set Account Lockout Threshold  

Double-click **Account lockout threshold** and set it to **3 invalid lockout attempts**.  

<img width="414" height="506" alt="2 3" src="https://github.com/user-attachments/assets/5e4e4661-d905-496e-a72b-a6b417b72a8f" />  

In a real environment 5-10 attempts is more user friendly, but I use 3 here to make the lab quick and repeatable.  



#### Step 2.4 - Set Account Lockout Duration  

Set the lockout duration to **15 minutes**. This means a locked out account auto-unlocks after 15 minutes without admin intervention (useful for lower-priority lockouts).  

<img width="417" height="509" alt="2 4" src="https://github.com/user-attachments/assets/00efe093-da6d-420c-af90-a750f593c9a6" />  

#### Step 2.5 - Set Reset Account Lockout Counter After  

Set the counter reset to **15 minutes**. This resets the failed attempt count if no further failures occur within the window.

<img width="417" height="507" alt="2 5" src="https://github.com/user-attachments/assets/ce9ce80b-f18b-444e-aeb8-b3e019995742" />  

#### Step 2.6 - Force Group Policy Update via CMD  

Open Command Prompt as Administrator.  

Run the following command to push the new policy to the domain immediately without waiting for the standard refresh cycle:  
```gpupdate /force```  

<img width="977" height="239" alt="2 6" src="https://github.com/user-attachments/assets/8794ae47-9fae-40ae-b23c-bfe35d94a44e" />  

The command output confirms the policy refresh is processing:  

<img width="973" height="612" alt="2 6 1" src="https://github.com/user-attachments/assets/49cef2f4-5fde-45ef-8b0e-3a9f60060812" />  
**Account Lockout Policy is now active across the domain.**  

### Phase 3 - Trigger the Account Lockout on the Client  
**Goal**: Reproduce the business scenario by entering the wrong password on the Windows 11 client until the account locks.  

#### Step 3.1 - Attempt Login with Wrong Password (x3)  

On the Windows 11 client, attempt to sign in as ``bblack@mylab.local`` using an incorrect password three times in a row.  

After the 3rd failed attempt, Windows displays the lockout message:  

<img width="1012" height="695" alt="3 1" src="https://github.com/user-attachments/assets/849e2481-7190-483a-aa73-ff0a3c23db7d" />
  

### Phase 4 - Investigate and Resolve on the Domain Controller  
**Goal:** As the help desk/IT admin, locate the affected account, confirm the lock state, reset the password, and unlock the account.  

#### Step 4.1 - Locate the User in ADUC  

Back on the Domain Controller, open ADUC and navigate to the **Users** container. Locate **Bill Black** in the user list.  

<img width="753" height="529" alt="4 1" src="https://github.com/user-attachments/assets/c21f13a7-5f8c-43c5-b225-5bc0152cc6b4" />  

#### Step 4.2 - Open User Properties to Verify Lock State  

Right-click **Bill Black** -> **Properties**.  

<img width="750" height="526" alt="4 2" src="https://github.com/user-attachments/assets/12ff33b4-9934-41a2-8db1-05087fba029b" />
  
Navigate to the **Account** tab. You will see the following message:  
"Unlock account. This account is currently locked out on this Active Directory Domain Controller."  

The **Unlock account** checkbox is visible, confirming the account is locked.  
<img width="409" height="537" alt="4 2 1" src="https://github.com/user-attachments/assets/441f3d1f-91dd-4c11-ae35-ba409e910f35" />  

#### Step 4.3 - Initiate Password Reset  

Close Properites. Right-click **Bill Black** again and select **Reset Password**.  














