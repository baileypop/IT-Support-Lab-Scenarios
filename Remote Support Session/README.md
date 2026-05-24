# Remote Support Session  

Author: Bailey Popovic  
Domain: Remote Assistance - Help Desk Support Workflow  
Environment: Windows 11 Client, Quick Assist (built-in Windows tool)  


## Objective  
Run a documented remote support session using Quick Assist, diagnose a software installation failure on the remote machine, resolve it by obtaining a clean installer from the official source, and close the session with a full ticket trail from open to resolution.  

## Business Scenario  

**Ticket #079 | Remote Application Installation Failure**  
Sam Ramos, a remote employee, reports that they cannot install an application on their machine. The installer is failing with an error message and they have already attempted the installation twice without success. The issue is blocking them from completing a shift-critical task. IT support has been asked to connect remotely, investigate the failure, fix it, and confirm the application is working before closing the session.  

## Environment and Tools Used  

| Component | Detail |
| --- | --- |
| Technician Machine | Windows 11 - Bailey Popovic |
| Remote User Machine | Windows 11 - Sam Ramos |
| Remote Tool | Quick Assist (built in to Windows 11) |
| Application Being Installed | 7-Zip (used to replicate the access denied install scenario) |
| Root Cause | Corrupted installer file - NSIS Error |
| Fix Applied | Clean installer downloaded from official source |

## Steps Performed  

### Phase 1 - Create Ticket  
#### Step 1.1 - Log the initial ticket  

Before connecting to the remote machine, the ticket is created with all required fields.  

<img width="704" height="300" alt="ticket 079" src="https://github.com/user-attachments/assets/ccf8931d-a517-4e52-bb65-0c816d5b5c94" />  

Creating the ticket before the session establishes the start time, captures the user's own description of the problem before any investigation begins, and creates a record that exists independently of the technicians memory. If the session is interrupted or handed over, another technician can pick up the ticket without losing context.  

### Phase 2 - Establish the Remote Session  
#### Step 2.1 - Generate a Security Code in Quick Assist (Technician Side)  

Open **Quick Assist** on the technician machine and click **Help someone**. After signing in with a Microsoft account, Quick Assist generates a six-character security code that expires within 10 minutes. Code shared verbally.  

<img width="459" height="566" alt="2 1" src="https://github.com/user-attachments/assets/2fd51d8e-0633-4e08-8730-96332340bf88" />  

#### Step 2.2 - User Enters the Security Code (User Side)  

On the user's machine, Quick Assist opens to the **Get help** screen. The user types the security code provided by the technician and clicks **Submit**.  

#### Step 2.3 - User Allows Screen Sharing  

Quick Assist presents the user with an Allow screen sharing confirmation. The user checks the security acknowledgement checkbox and clicks **Allow**.  

<img width="463" height="687" alt="2 3" src="https://github.com/user-attachments/assets/de76fd9d-1bf4-46c1-87fc-53b377c99698" />  

#### Step 2.4 - Connection Established: Technician View  

The technician's Quick Assist window now shows the remote desktop. The user mode shows **Administrator**, confirming the session has elevated rights. The remote screen is fully visible inside the Quick Assist frame.  
<img width="1059" height="906" alt="2 4" src="https://github.com/user-attachments/assets/bf8c4423-f15e-44c3-b86b-4581677ca6b5" />
  


#### Step 2.5 - Connection Confirmed: Client View  

On the user's machine, a Quick Assist banner appears at the top of the screen reading **Screen sharing on**, confirming to the user that the session is active. The user can end the session at any time using the Leave button.  

<img width="1025" height="719" alt="2 5" src="https://github.com/user-attachments/assets/94d0304d-cd45-41a9-b7b6-c9b0b784666d" />  


### Phase 3 - Investigate the fault  

#### Step 3.1 - Add Investigation Note to the Ticket  

With the session active and the problem confirmed on screen, the technician updates the ticket with an investigation note including the timestamp, what was observed, and the user's exact description of the error.  

<img width="817" height="357" alt="3 1" src="https://github.com/user-attachments/assets/aa7d01d2-22d7-4ba5-b667-336a975d39dd" />  

#### Step 3.2 - Identify the Root Cause: NSIS Error  

On the remote machine, the technician navigates to the Downloads folder and attempts to run the installer the user had been using. The installer returns an **NSIS Error**: the installer file is corrupted or incomplete.  
<img width="412" height="251" alt="3 2" src="https://github.com/user-attachments/assets/6f51f531-659e-4c02-9255-5b8e1bf5b48f" />  

The NSIS error confirms the installer file itself ist he problem not the user's permissions or UAC settings. This rules out the Access denied description the user gave - they were reading a different error as the NSIS error preceded the access denied message. The fix is to obtain a clean copy of the installer from a verified source.  

### Phase 4 - Fix the Issue  

#### Step 4.1 - Download a Clean Installer from the Official Source  

While still connected via Quick Assist, the technician opens the browser on the remote machine and navigfates to the official download page at **7-zip.org**. The correct 64-bit Windows installer is downloaded directly to the remote machine.  

<img width="1267" height="665" alt="4 1" src="https://github.com/user-attachments/assets/b7f78277-7600-4eb7-856f-70c047a6af21" />  

#### Step 4.2 - Install the Application and Confirm It Works  

The clean install runs without errors. The application install successfully and opens on the remote machine, confirming it is fully functional.  

<img width="751" height="511" alt="4 2" src="https://github.com/user-attachments/assets/108227e3-4d26-4a11-a886-d26d08203b3e" />  


### Phase 5 - End the Session and Close the Ticket  

#### Step 5.1 - End the Quick Assist Session  

Once the user confirms the application is working, the technician ends the Quick Assist session. Quick Assist shows the **Screen sharing has ended** confirmation on the technician side.  

<img width="953" height="795" alt="5 1" src="https://github.com/user-attachments/assets/a4e1f9ed-ad8b-4b7f-8b5e-d7313a059f84" />  

#### Step 5.2 - Update and Close the Ticket  

Return to the ticket and add the resolution note with timestamp, root cause confirmed, action taken, and user confirmation. Change the status from Open to Resolved and record the close time.

## Key Learning Points:  

1. The ticket should be open before the session starts.
2. Investigate the actual error before trying fixes.
3. Always download from the official source.
4. Update the ticket during the session, not after.
5. End the session clearly and confirm with the user.


## Troubleshooting Reference  

| Symptom | Cause | Resolution |
| --- | --- | --- |
| NSIS Error on installer launch | Installer file corrupted during download | Delete file and download a fresh copy from the official vendor site |
| Acess denied during installation | Installer not running with admin rights | Right-click installer and select Run as Administrator |
| Quick Assist code not accepted | Code has expired - 10 minute limit | Generate a new code in Quick Assist and share again |
| Remote desktop not visible after connection | User did not click Allow on the consent screen | Ask user to check for the Allow screen sharing prompt |
| Application installs but does not open | Conflicting old version or missing dependency | Check Apps and Features for an existing version- Uninstall first if found |
| Session drops mid-investigation | Network interruption | Reconnect via Quick Assist with a new code- update ticket with interruption note |
| User cannot find Quick Assist | Search bar not returning results | Press Win + R and type quickassist- or open it from the Microsoft Store |













