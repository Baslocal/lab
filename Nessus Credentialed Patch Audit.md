Objective:
To perform a credentialed patch audit of my Homelab environment to identify and report missing patches for Windows and Linux systems macOS.
-Note this was done in my Homelab on my VMs


Things needs
-Service accounts for the credentialed scan both Windows and Linux
-VMs or PCs
-Nessus essential 
-Scope for the scan (IP range)


Remediation plan
My plan for the remediating the listed vulnerabilities was targetting the Highs 

Windows
-Check for update and apply missing updates

Linux
-Most of my Linux VMs or containers are command line driven usually the command that done to patch them is sudo apt-get update and sudo apt-get upgrade
Decided to use this command  (apt update && sudo apt full-upgrade -yy) 

MacOS
-Check for update

Steps

1. Created a new scan policy in Nessus.
2. a. Went to Policies > New Policy.
3. b. Selected Credentialed Patch Audit as the policy type.
4. c. Entered a name for the policy.


![Pasted Graphic 2](https://github.com/Baslocal/lab/assets/155853534/0546eab2-9007-4d2e-9086-1b9902df8a25)
1. Configured the scan policy settings.
2. a. Selected the Scan Targets tab.
3. b. Added the IP range for the scan Windows and Linux systems it wanted to target.

![Screenshot 2023-11-25 at 12 03 29 PM](https://github.com/Baslocal/lab/assets/155853534/38422a24-187d-4408-9a71-26ee42952bd4)
1. Configured the credentials for  Linux and Windows
2. a. Selected the Credentials tab.



Before
![Pasted Graphic 1](https://github.com/Baslocal/lab/assets/155853534/6f1bd652-7790-40f9-81a6-da3d8ff31da7)

After
![Pasted Graphic](https://github.com/Baslocal/lab/assets/155853534/94b613b0-1399-4c34-a277-a571ea0284b8)
