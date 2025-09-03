🛠️ Setting Up Metasploitable3

After BWAPP, the next vulnerable machine I added to my lab was Metasploitable3.
Unlike Metasploitable2 (which is a simple Linux VM), Metasploitable3 is a Windows Server 2008 (or Ubuntu) environment preloaded with misconfigurations and exploitable services.

This makes it an enterprise-style target — perfect for practicing privilege escalation, exploitation frameworks (like Metasploit), and post-exploitation techniques.

📥 Step 1: Downloading Metasploitable3

Metasploitable3 is not a direct .ova download. Instead, you have to build the VM yourself using scripts.

Official repo: Rapid7 Metasploitable3

Clone or download the repository:

git clone https://github.com/rapid7/metasploitable3.git
cd metasploitable3


Inside, you’ll find Vagrant + Packer scripts to build either:

metasploitable3-win2k8 (Windows Server 2008)

metasploitable3-ubuntu (Ubuntu 14.04)

⚙️ Step 2: Requirements

Before building, install:

Vagrant

VirtualBox

Packer

💡 Tip: Ensure VirtualBox and Vagrant are added to your PATH, otherwise commands will fail.

🛠️ Step 3: Building the VM

Example for Windows Server 2008 build:

cd metasploitable3
packer build windows_2008.json
vagrant up win2k8


Expected build time: 30–60 mins (depends on internet + hardware).

⚠️ Expected Errors & Fixes

Metasploitable3 is known for breaking, so here’s a survival list:

❌ Packer fails at Windows ISO download

Fix: Download ISO manually → place it in the iso/ folder → update the script path.

❌ “Vagrant not found” error

Fix: Reinstall Vagrant and add it to PATH. Restart shell.

❌ VM stuck on boot

Fix: Check VirtualBox settings:

RAM: at least 2GB.

Enable PAE/NX + VT-x in System → Acceleration.

❌ Network unreachable

Fix: Attach Adapter 2 to the same Internal Network (Lab_DMZ) as Kali.

Get IP with:

ipconfig


Then test from Kali with:

ping <Metasploitable3-IP>


❌ RDP/WinRM not working

Fix: Ensure Windows Firewall is disabled inside VM (sometimes defaults block).

🌍 Step 4: Accessing Metasploitable3

Once running:

Username: vagrant

Password: vagrant

Admin user: administrator (password varies; check Vagrantfile or repo notes).

Services exposed (examples):

SMB (445)

RDP (3389)

WinRM (5985)

MySQL (3306)

Apache Tomcat (8282)

✅ From Kali:

nmap -sV <Metasploitable3-IP>


You should see dozens of vulnerable services open.

🧪 Step 5: Validate Setup

VM boots to Windows desktop ✅

IP assigned and ping works ✅

Nmap shows services ✅

Can login via vagrant credentials ✅

🎯 Why Metasploitable3?

Realistic Windows Server environment.

Simulates corporate misconfigurations.

Perfect for privilege escalation practice.

Bridges the gap between beginner labs (like BWAPP) and real Active Directory labs.

🔮 Next Up

Now that Metasploitable3 is alive in my lab, my next step will be to configure pfSense Firewall — to simulate segmentation between external, DMZ, and internal zones.
