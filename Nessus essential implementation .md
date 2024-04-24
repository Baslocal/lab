Objective:



To perform a credentialed patch audit of my Homelab environment to identify and report missing patch for Windows and Linux macOS systems.
-Note this was done in my Homelab on my VMs
 
Things needs
-Service accounts for the credentialed scan both Windows and Linux
-VMs or PCs
-Nessus essential 
-Scope for the scan (IP range)
Steps
1.     Created a new scan in Nessus.
a.     Went to Policies > New Policy.
b.     Selected Credentialed Patch Audit as the policy type.
c.     Entered a name for the policy.

![unknown](https://github.com/Baslocal/lab/assets/155853534/e301a90c-fca1-470e-9d33-c95223f55230)
2.     Configured the scan settings.
A.    Selected the Scan Targets tab. Added the IP range for the scan Windows and Linux systems it wanted to target
B.    Configured the credentials for Linux and Windows
C.     Selected the Credentials tab.

![unknown](https://github.com/Baslocal/lab/assets/155853534/58f7e8c5-644d-46b1-9828-9988a405818c)
Before
![unknown](https://github.com/Baslocal/lab/assets/155853534/a4ceb6ec-3f3a-4cfa-b328-e8792a1e4495)

After
![unknown](https://github.com/Baslocal/lab/assets/155853534/6a54a882-8dc8-4c9a-b57d-96f41321655a)
Remediation plan
To address the identified vulnerabilities, I prioritized remediating those classified as "High" severity.
Windows
-Check for update and apply missing updates
Linux
-Most of my Linux VMs and containers are command line driven usually the command that done to patch them is sudo apt-get update and sudo apt-get upgrade
decided to use this command (apt update && sudo apt full-upgrade -yy) 
MacOS -Check for update
 
