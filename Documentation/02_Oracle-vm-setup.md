SETTING UP ORACLE VM VIRTUAL BOX 

Creating a controlled, isolated, and reproducible environment is the first step to learning offensive security safely. I chose [Oracle VM VirtualBox](https://www.virtualbox.org/) for its flexibility and community support.

INSTALLATION:
1. https://www.virtualbox.org/
2. Download the latest version for your host OS.
3. Run the installer with default settings but exclude "VirtualBox Python Support" because we don't need that.
4. Now after opening the application go to file > preferences > Expert
5. Next is Click on the Sidebar Toggle Icon (three stacked lines with dots) in the top-right corner of the "Tools" panel.
6. Go to Networks and setup all the NAT network adapters
7. We will be creating 3 network adapters:
   -External Zone: Statically assign IPv4 prefixes and disable DHCP
   -Internal Zone: Statically assign IPv4 prefixes and disable DHCP
   -DMZ Zone: Statically assign IPv4 prefixes and disable DHCP

Reference image : ![my work](Images/VM setup.jpg)

Here we complete a basic setup of our Virtual Box 
