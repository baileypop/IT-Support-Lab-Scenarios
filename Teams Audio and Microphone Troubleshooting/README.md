# Teams Audio and Microphone Troubleshooting

**Author:** Bailey Popovic
<br>
**Domain:** Microsoft 365 - Teams Audio Support
<br>
**Environment:** Windows 11 Client, Microsoft Teams
<br>
<br>

## Objective  
Diagnose and resolve a Teams microphone failure by identifying where the audio fault exists, simulate the problem by disabling the microphone, verify the privacy settings that control application access to the microphone, and confirm the audio is working in a live Teams call.  

## Business Scenario
**Ticket #062 - Teams Audio Fault - Remote User Cannot Be Heard in Meetings**  

  A remote employee reports they can hear other participants in Microsoft Teams meetings but nobody can hear them. They have joined the call correctly and their Teams status shows as connected. The issue has been present since their last Windows update. IT Support has been asked to investigate whether the fault is in the Teams app configuration, Windows audio settings, or Windows privacy settings that control microphone access.  
  
## Environment and Tools Used  

| Component | Detail |
|-----------|--------|
| Client OS | Windows 11 |
| Application | Microsoft Teams |
| Audio Hardware | High Definition Audio Device - integrated |
| Default Output Device | Speakers - high definition audio device |
| Default Input Device | Microphone - high definition audio device |
| Tools Used | Teams Settings, Windows Sound Settings, Sound Recording tab, Privacy and Security settings |

## Audio Troubleshooting Stack  
When a user cannot be heard in Teams, the fault is usually in one of three places.  
```
Layer 3 - Teams Application Level
  Settings -> Devices -> Microphone
  "Does Teams know which microphone to use?"
    | Wrong device selected or no device shown -> change selection
    | Correct device shown -> move down

Layer 2  - Windows Audio Device Level
  Settings -> System -> Sound -> More sound settings -> Recording tab
  "Is the microphone device enabled in Windows?"
    | Disabled -> enable it
    | Not set as default -> set as default
    | Enabled and default -> move down

Layer 1 - Windows Privacy Level
  Settings -> Privacy & Security -> Microphone
  "Is Teams allowed to access the microphone at all?"
      | Teams toggle Off -> turn on
      | Teams toggle On -> audio should work
```

A Teams microphone issue is almost always a Windows audio or privacy configuration. It is rarely a Teams problem itself but presents itself as one because it needs microphone access to function.  


## Steps Performed  

### Phase 1 - Baseline Configuration  
#### Step 1.1 - Check Windows Sound Settings  

Open **Settings -> System -> Sound** and confirm both the output and input devices are correctly assigned. This just shows the working state before any changes are made and confirms the hardware is present.  

 
<img width="677" height="682" alt="baseline" src="https://github.com/user-attachments/assets/6bf9ae88-09a8-42d6-bd97-884213474b6c" />  



**Step 1.2 - Check Teams Device Settings**  

Open Teams, click the three dots at the top right, go to Settings -> Devices and confirm the Speaker and Microphone dropdowns show to correct devices.  


<img width="974" height="566" alt="1 2 check teams device settings" src="https://github.com/user-attachments/assets/b89bc7ce-9736-4e40-9291-08b27f524e37" />



Teams has the correct devices selected. Speaker is set to Speakers (High Definition Audio Device) and Microphone is set to Microphone (High Definition Audio Device). At baseline, if the user were on a call right now, audio would be working in both directions.  

### Phase 2 - Simulate the Fault (Disable the Microphone)  
#### Step 2.1 - Disable the Microphone in Windows Recording Devices 

Navigate to **Settings -> System -> Sound -> More sound settings**. Click the **Recording** tab. Right-click the Microphone option and select **Disable**. Click OK.  



<img width="663" height="677" alt="2 1 disable microphone" src="https://github.com/user-attachments/assets/e47dff55-634d-4c3b-b3bb-73ee85e0eebc" />


Teams will no longer be able to access the microphone regardless of what is selected in Teams settings. This represents the fault state.  

Simulates:  
- A user or policy has accidentally disabled the device
- A Windows update has changed the device state
- The device has been set to disabled by a group policy applied to the machine

#### Step 2.2 - What the User Experiences During This Fault

  - Other participants cannot hear them
  - Teams may show the microphone as available in the Devices settings but produces no audio
  - The mic button in the call toolbar appears functional but produces no input
  - There is no error message or warning visible to the user
 

<img width="1001" height="704" alt="2 2 user experience" src="https://github.com/user-attachments/assets/8589a8dd-c90d-404c-8abf-2f1d87dfb34e" />


### Phase 3 - Re-enable the Microphone and Validate  

#### Step 3.1 - Re-enable the microphone in Windows Recording  

Return to **Settings -> Sound -> More sound settings -> Recording tab.** Right-click the microphone entry and select **Enable**. Confirm it returns to the active state and set as default if needed. Click OK.  


<img width="751" height="676" alt="3 1 reenable microphone" src="https://github.com/user-attachments/assets/23117f29-e2e0-4fbb-9850-d7d5f3754b59" />


#### Step 3.2 - Confirm Teams Detects the Device  

Return to Teams Settings -> Devices and confirm the microphone dropdown still shows the correct device. If Teams had detected the device disappearing during the during the fault, it may have cleared the selection or defaulted to another device. Verify and correct if needed.  

#### Step 3.3 - Verify Microphone Access in Privacy and Security  

Navigate to **Settings -> Privacy & Security -> Microphone.** Confirm that **Microphone access** is toggled On at the system level and that **Microsoft Teams** specifically shows as On in the per-app list.  


<img width="754" height="680" alt="3 3 mic access in privacy and sec" src="https://github.com/user-attachments/assets/79b4435a-aef9-462a-8223-93432b80e0d3" />


#### Step 3.4 - Re-Confirm Audio  

The call toolbar confirms the audio is active when the Mic button is visible and interactive.  


<img width="995" height="705" alt="3 4 reconfirm audio" src="https://github.com/user-attachments/assets/239ff637-b368-47bd-9ab3-8900efa9c2fc" />



## Troubleshooting Reference  

| Symptom | Cause | Resolution |
| --- | --- | --- |
|User cannot be heard, microphone button appears normal | Microphone disabled at Windows Recording device level | Sound settings -> More sound settings -> Recording -> Enable microphone |
| Teams shows no microphone device in dropdown | Microphone not present or not recognized by Windows | Check Device Manager for audio device, check Recording tab |
| Teams microphone dropdownshows correct device but still no audio | Privacy setting blocking Teams microphone access | Settings -> Privacy and Security -> Microphone -> Turn Teams 'On' |
| Microphone works in other apps but not Teams | Teams has wrong device selected or privacy toggle 'Off' | Check Teams Settings -> Devices and Privacy settings |
|Microphone works after call starts but cuts out | Auto-adjust sensitivity causing drop-outs | Teams settings -> Devices -> turn off Automatically Adjust Mic Sensitivity |
| User hears themselves echoing | Speaker output feedback into microphone input | Ask user to use headset or reduce speaker volume. Check noise suppression setting |
|Audio works in test but not in live calls | Network issues affecting real-time audio | Check bandwidth and firewall rules for Teams media ports |







