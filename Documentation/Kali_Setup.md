Before starting further... exclude the virtual box folder from WINDOWS SECURITY i.e MICROSOFT DEFENDER ANTIVIRUS SCAN 

KALI LAB SETUP
1. Download .iso image from official kali website that supports your system.
2. In your virtual machine, go to machine > new > add the downloaded iso image > select linux debian operating system > complete the hardware requirememts according to your system
3. Right click on the added machine and open settings
4. In settings > Advanced Settings :
   - Set Shared clipboard and Drag n drop to bidirectional
   - Enable 3D acceleration
5. Setup Networks :
   - Adapter 1 : Set it to Bridged adapter
   - Adapter 2 : Set it to NAT network and name External zone
   - Adapter 3 : Set it to NAT network and name DMZ zone
   - Adapter 4 : Set it to NAT network and name Internal zone
     Reference : [Images/kali network setup.jpg]
6. Start the machine.

*The machine might not start if the .iso file is not connected properly so then you will have to
  -close the machine
  -open machine settings > storage 
  - attach disk file (.ios file) if the space shows empty
  - restart
This will start the machine.

7. Enter the desired information and you are well good to go just enter your login id password and that's it.

   
