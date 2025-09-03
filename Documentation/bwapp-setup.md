🐞 Setting Up BWAPP (Buggy Web Application Project)

After setting up Oracle VirtualBox and Kali Linux, the first vulnerable machine I added to my lab is BWAPP (Buggy Web Application Project).

BWAPP is one of the best starting points for learning web application security because it has more than 100+ vulnerabilities to practice on — from SQL Injection to XSS and beyond.

This post documents how I installed and configured BWAPP in my offensive security lab.

📥 Step 1: Download BWAPP VM

SourceForge link: BWAPP OVA Download

File type: .ova (ready-made appliance for VirtualBox)

Approx size: ~500 MB

💡 Tip: If your download is slow or fails, try a mirror from SourceForge or use a download manager.

🖥️ Step 2: Import into VirtualBox

Open VirtualBox → File → Import Appliance.

Select the .ova file you downloaded.

Accept the default settings (you can tweak CPU/RAM later if needed).

RAM: 1024 MB (minimum, enough for lab use).

CPU: 1 core.

Import → Wait for it to finish.

✅ BWAPP will now appear in your list of VirtualBox machines.

🌐 Step 3: Network Configuration

Since I’m building a lab where Kali (attacker) needs to talk to BWAPP (target), I connected BWAPP to my Internal Network.

Adapter 1 → NAT (optional, internet access).

Adapter 2 → Internal Network → Lab_DMZ.

This way:

BWAPP can talk to Kali.

Host system remains isolated.

Traffic stays inside the lab.

▶️ Step 4: Start BWAPP

Start the VM.

Login screen:

Username: bee

Password: bug

Once logged in, BWAPP will auto-launch the LAMP stack (Linux + Apache + MySQL + PHP).

🌍 Step 5: Accessing BWAPP

Inside BWAPP VM → open terminal:

ifconfig
# or
ip a


Copy the IP (likely 192.168.x.x).

From Kali Browser → Visit:

http://<BWAPP_IP>/bWAPP/login.php


✅ You should see the BWAPP login page.

⚠️ Expected Issues & Fixes

Here are problems I ran into (and ones you might too):

❌ BWAPP not getting IP

Fix: Check VirtualBox → Settings → Network → Adapter 2 is on Lab_DMZ.

❌ Page won’t load in browser

Fix: Start Apache & MySQL manually:

service apache2 start
service mysql start


❌ Login doesn’t work

Fix: Reset the MySQL DB from inside BWAPP:

Visit: http://<BWAPP_IP>/bWAPP/install.php

Run the setup script again.

❌ Can’t reach from Kali

Fix: Test with ping <BWAPP_IP> from Kali.

If ping fails → recheck network mode (Internal Network must match).

🧪 Step 6: Validate Setup

Ping test works ✅

Browser shows BWAPP login page ✅

You can log in with:

Username: bee

Password: bug ✅

🎯 Why BWAPP First?

I started with BWAPP because:

It’s lightweight.

Focused only on web vulnerabilities.

Provides an easy win — a working target to attack quickly.

This keeps motivation high before moving to heavier setups like Metasploitable3 or Windows Server.

🔮 Next Up

Now that BWAPP is running, my next step is to configure Metasploitable3 — a more complex, Windows-based vulnerable machine that simulates real enterprise environments.
