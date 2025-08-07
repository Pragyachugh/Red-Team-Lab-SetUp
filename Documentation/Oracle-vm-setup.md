# 🛠️ Virtualization Foundations: Setting Up Oracle VM for a Cybersecurity Lab

Creating a controlled, isolated, and reproducible environment is the first step to learning offensive security safely. I chose [Oracle VM VirtualBox](https://www.virtualbox.org/) for its flexibility and community support.

## 🔽 Installing Oracle VM VirtualBox

1. Head to the official site: https://www.virtualbox.org/
2. Download the latest version for your host OS (Windows, macOS, Linux).
3. Run the installer with default settings unless you have specific needs.
4. (Optional but recommended) Also download and install the **Extension Pack** from the same page for USB, RDP, and other enhancements.

## ⚙️ Initial Configuration Tips

Here are some key configuration steps I took to avoid common pitfalls:

### ✅ Virtualization in BIOS
- Ensure virtualization is **enabled** in BIOS/UEFI (Intel VT-x or AMD-V).
- Without this, your VMs may not boot or behave unpredictably.

### 🧠 RAM and CPU Allocation
- I set each VM to use:
  - 2–4 GB RAM (depending on purpose)
  - 2 CPU cores minimum
- Make sure your host has **enough resources** before powering on multiple machines at once.

### 🖧 Network Adapter Setup
Proper network planning saves you from future pain.

I created **three VirtualBox host-only adapters**:
1. for LAN/Internal traffic (shared by all VMs)
2. for DMZ (for vulnerable machines)
3. for isolated tools like pfSense or domain controller segments

I'll break these down more in a later networking-focused post.

### 💾 Storage Management
- Changed VM disk format to `VDI` and enabled **dynamic allocation** to save host storage.
- Stored all VMs on an external SSD to avoid crowding my system drive.

### 🎨 Personalization
- Enabled **Mini toolbar in full screen** (VirtualBox settings).
- Disabled audio and USB unless needed (to reduce overhead).

## 🚨 Lessons Learned

- Don’t skip BIOS virtualization — wasted hours wondering why VMs booted weirdly!
- Allocate **just enough** RAM to Kali and Windows machines — performance matters when multitasking.
- Label each adapter and VM **clearly** — this helps once you hit 6+ machines.
- I reinstalled the whole setup *three times* before I got it right — out of frustration, confusion, and missing network configs 🤦🏻‍♀️

## 🧰 Tools I Used So Far
- Oracle VirtualBox
- VirtualBox Extension Pack
- (Upcoming) pfSense, Kali, Metasploitable3, etc.

---

> This post is part of an ongoing documentation of my self-built offensive security lab. The goal is to provide practical, no-fluff insights to help others build and break things — safely.

