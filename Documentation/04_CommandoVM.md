**INSTALLATION PROCESS FOR COMMANDO VM** REQUIRES DOWNLOADING WINDOWS 11 OR 10 BUT IT WORKS BETTER ON WINDOWS 10 SO WE WOULD DOWNLOAD WINDOWS 10 IN VM.

WINDOWS 10 DOWNLOAD:
- Download media creation tool for windows 10.
- Run the executable file.
- Choose the options that suit your requirements.
- Download the .iso image in your desired location.

NOW, VM SETUP :
- Create new machine.
- Add the desired .iso image and name in Windows 10 for now.
- Add the machine in extenal zone folder of the virtual machines.
- Keep storage upto 90-100 gb preferably because commando vm is a heavy file.
- Finish the setup and do the changes in advanced settings like enable 3D acceleration , allow bidirectional movements, enable only 1 network adapter for now i.e. bridged adapter for internet access.
- Finally you're ready to start the machine.

Inside Windows 10:
- Clean up the desktop as per your preferences. I removed microsoft edge, search bar, etc. from taskbar. You can clean it as per your preference.
- Now update windows from settings > update and security.
- Insert Guest Addition CD Image in your setup.
- In File Explorer > go to This PC > Then Click on virtual box guest addition drive.
- From here install 64 bit guest addition application of virtualBox.
- In control Panel, disable windows defender wall.
- Open Windows security, then virus and threat protection and from here **exclude** your **desktop** from protection.
- Pin control panel and settings to taskbar
- Now shutdown the machine and copy the machine to a spare machine folder in virtual box folder because we need a spare windows 10 machine and a machine for commando VM
- Now rename the machine to CommandoVM, then in storage : remove the iso image, enable all the 4 network adapters with external, dmz and internal zones respectively.

  Now Start the CommandoVM machine:
  - We will be configuring our network adapters now:
  - Go to control Panel > Network and Sharing Center > Change adapter settings
  - - Adapter 1: Bridged Adapter (remains the default)
    - Adapter 2 (External Zone): Static IP 172.16.10.20, Subnet Mask 255.255.255.0, Gateway 172.16.1.1, DNS 8.8.8.8
    - Adapter 3 (DMZ Zone): Static IP 172.16.100.20, Subnet Mask 255.255.255.0 (default gateway is left out)
    - Adapter 4 (Internal Zone): Static IP 172.16.200.20, Subnet Mask 255.255.255.0 (default gateway is left out)
  - The machine is started, and connectivity is verified by successfully pinging the Kali Linux machine (e.g., at 172.16.10.1), confirming the external zone is correctly set up.
 
  CommandoVM Installation procedure:
  - Download Repository: The Commando VM GitHub repository is downloaded as a zip file, extracted, and placed on the desktop.
  - Turn tamper protection off from windows security.
  - Group Policy Editor (gpedit.msc) is accessed by right clicking on start menu > run and make the following changes in computer configuration.
  - In Computer Configuration > Administrative Templates > Windows Components > Microsoft Defender Antivirus > Real-time protection, the setting "Turn off real-time protection" is enabled. The machine is then rebooted for the policy to take effect.
  - Disable Protection (Stage 3 - Group Policy): After reboot, Group Policy Editor is accessed again, and under Microsoft Defender Antivirus, the setting "Turn off Windows Defender Antivirus" is enabled. The machine is rebooted a second time.
  - Prepare PowerShell: PowerShell is opened as an administrator.
  -  - The user navigates to the extracted Commando VM folder.
     - The execution policy is set to unrestricted: Set-ExecutionPolicy Unrestricted -Force
     - The execution policy is set to unrestricted: Set-ExecutionPolicy Unrestricted -Force
  - Run Installer: The graphical user interface (GUI) installation script is executed: .\install.ps1
  - Select Installation Type: The Full installation option is chosen, which installs all suitable tools for Commando VM (including components like Windows Terminal and Microsoft Office).
  - Start Installation: The user provides the password ("Password123") to allow the installer to handle automatic reboots during the process.
  - The installation runs for about 30 to 90 minutes. Once completed, Commando VM and all its tools (like Visual Studio) are available on the machine.
 
  
