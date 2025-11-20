Setting Up BWAPP (Buggy Web Application Project)

After setting up Oracle VirtualBox and Kali Linux, the first vulnerable machine I added to my lab is BWAPP (Buggy Web Application Project).
BWAPP is one of the best starting points for learning web application security because it has more than 100+ vulnerabilities to practice on — from SQL Injection to XSS and beyond.
This post documents how I installed and configured BWAPP in my offensive security lab.

Step 1: Create new machine in VirtualBox
Open VirtualBox → Machine → New.
Name the machine BWAPP Vulnerable WebAPP - DMZ.
Select the location of machine in DMZ Folder in Virtual Machine.
Create a new folder naming Vulnerable WebApp and select it.
Select the downloaded iso image of ubuntu and the system will automatically detect the machine type.
Now complete the installation with the required hardware arrangements.

BWAPP will now appear in your list of VirtualBox machines but this is an ubuntu system in which BWAPP needs to be installed.

Step 2: Network Configuration
Since I’m building a lab where Kali (attacker) needs to talk to BWAPP (target), I connected BWAPP to my Internal Network.

Adapter 1 → Bridged Network (for internet access)
Adapter 2 → NAT Network → DMZ-Zone
Adapter 3 → NAT Network → External-Zone
Adapter 4 → NAT Network → Internal-Zone
This way:
BWAPP can talk to Kali.
Host system remains isolated.
Traffic stays inside the lab.

Steo 3: Start Ubuntu and complete its setup with all the upgrades and insert guest addition cd image.
terminal > cd [path of guest addition image] > ls > sudo ./VBoxLinuxAdditions.run

Step 4: Click on Top right corner > go to wired settings > Configure the ipv4 addresses manually for external, internal, and DMZ Zone
Finally reboot the machine for all the configurations to take place safely without error. 

Step 5: create a snapshot of the current state of the machine as the changes we would make may potentially harm the system.

Step 6: download mysql-server from terminal [command : sudo apt install mysql-server]
To initiate mysql everytime you switch on the machine [command : sudo systemctl enable mysql]
Create a user for BWAPP [command: CREATE USER 'user'@'localhost' IDENTIFIED BY 'xxxxxxx';]
create a database for bWAPP[command: CREATE DATABASE bWAPP;]
[command: grant all privileges on bWAPP.* to 'user'@'localhost';]
[command: FLUSH PRIVILEGES;]

Step 7: download apache webserver
[command: sudo apt install apache2]
[command: sudo systemctl enable apache2]
[command: sudo apt install php libapache2-mod-php]

Step 8: Install BWAPP from sourceforge original site and move it to desktop after downloading for convinient use.
[command: Desktop/]
[command: ls]
[command: sudo unzip -d /var/www/html bWAPPv2.2.zip]
[command: sudo chmod -R 777 bWAPP]
[command: cd bWAPP/admin/]
[command: sudo nano settings.php] Change the username and password to the one that we have set.
On checking bWAPP at "http://localhost/bWAPP/install.php" we encountered an error that the page was loading , on checking "cat /var/log/apache2/error.log" we found that some error has been occured that was , we forgot to install php-mysql
[command: sudo apt install php-mysql]
Restart apache2 and check the bWAPP site again and now it works perfectly fine.
Now check the login page with username:bee and password:bug.

Step 9: BWAPP IS COMPLETELY INSTALLED AND READY TO USE
Reference image : ![my work](Images/BWAPP installation.jpg)

Step 10: Remove the snapshot

QUESTION TIMEEE : Why BWAPP First?
I started with BWAPP because:
It’s lightweight.
Focused only on web vulnerabilities.
Provides an easy win — a working target to attack quickly.
This keeps motivation high before moving to heavier setups like Metasploitable3 or Windows Server.

🔮 Next Up

Now that BWAPP is running, my next step is to configure Metasploitable3 — a more complex, Windows-based vulnerable machine that simulates real enterprise environments.
