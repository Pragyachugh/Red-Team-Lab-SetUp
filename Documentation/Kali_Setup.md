# Kali Networking — 4-NIC Lab Setup (VirtualBox)

This document explains how I wired **Kali** to four separate networks in VirtualBox and configured each interface in Kali (both GUI and CLI). It includes IP plans, routing/metrics, verification, and troubleshooting.

---

## 1) Create the lab networks in VirtualBox (once)

**VirtualBox → Tools → Network**

### A. NAT Network(s) (used for “zones”)

Create as many as you need. Example plan I used:

| Name            | IPv4 CIDR         | DHCP | Internet  |
| --------------- | ----------------- | ---- | --------- |
| `External-Zone` | `172.16.10.0/24`  | Off  | Yes (NAT) |
| `DMZ-Zone`      | `172.16.100.0/24` | Off  | Yes (NAT) |
| `Internal-Zone` | `172.16.200.0/24` | Off  | Yes (NAT) |

> Notes
> • NAT Network = VirtualBox’s router/NAT. VMs inside can talk to each other; outbound internet is NATed.
> • I keep **DHCP off** so I can assign static lab IPs.
> • If you want these zones *isolated from the Internet*, disable “Supports DHCP” and “Supports DHCP/Internet via NAT” (depends on VB version wording) and don’t set a default route on Kali for those adapters.

### B. Host-Only Network (management path)

Still in **Tools → Network → Host-Only Networks**:

* Create `vboxnet0` (default).
* Typical host IP: `192.168.56.1/24`, with DHCP **off** (I use static IPs).

---

## 2) Attach **four** adapters to the Kali VM

**Kali VM → Settings → Network**

Enable **Adapter 1–4** and map them:

| Adapter | Attached to       | Name to pick                    | Advanced                                                                         |
| ------: | ----------------- | ------------------------------- | -------------------------------------------------------------------------------- |
|       1 | NAT               | (built-in NAT)                  | Adapter Type: *Intel PRO/1000 MT*; Promiscuous: **Allow All**; Cable Connected ✔ |
|       2 | Host-only Adapter | `vboxnet0`                      | Same advanced settings                                                           |
|       3 | NAT Network       | `External-Zone`                 | Same advanced settings                                                           |
|       4 | NAT Network       | `DMZ-Zone` (or `Internal-Zone`) | Same advanced settings                                                           |

> Tip: Promiscuous “Allow All” lets Kali sniff traffic in the lab.

Boot Kali.

---

## 3) Name each connection in Kali (so it’s not “Wired connection 1/2/3/4”)

You can use the GUI (**top-right network icon → Wired Settings**) or the CLI (recommended for repeatability).

First, see what NetworkManager created:

```bash
nmcli con show
```

You’ll see “Wired connection 1”, “2”, “3”, “4”. Rename them:

```bash
sudo nmcli con modify "Wired connection 1" connection.id NAT-Internet
sudo nmcli con modify "Wired connection 2" connection.id HostOnly-56
sudo nmcli con modify "Wired connection 3" connection.id External-172.16.10
sudo nmcli con modify "Wired connection 4" connection.id DMZ-172.16.100
```

> If your numbering is different (e.g., the one that’s connected right now is “3”), just rename accordingly. The names don’t have to match mine—pick something meaningful.

---

## 4) Assign IPs + routes to each NIC

Decide a static IP for Kali in each network. Example:

| Connection name      | Interface (typical) | IP (Kali)          | Gateway        | DNS          | Default Route?              |
| -------------------- | ------------------- | ------------------ | -------------- | ------------ | --------------------------- |
| `NAT-Internet`       | `enp0s3`            | DHCP (auto)        | from VB NAT    | from DHCP    | **Yes**                     |
| `HostOnly-56`        | `enp0s8`            | `192.168.56.10/24` | *(none)*       | *(none)*     | **No**                      |
| `External-172.16.10` | `enp0s9`            | `172.16.10.10/24`  | `172.16.10.1`  | *(optional)* | **No** (or Yes if you want) |
| `DMZ-172.16.100`     | `enp0s10`           | `172.16.100.10/24` | `172.16.100.1` | *(optional)* | **No**                      |

> Gateways `172.16.x.1` should be the router/firewall interface for that zone (e.g., pfSense).
> Keeping only **one** default route (on `NAT-Internet`) avoids route confusion; you can still reach the other subnets because Kali has directly-connected routes for them.

### Apply with `nmcli`

```bash
# 1) Internet via NAT (DHCP + lowest metric = preferred default)
sudo nmcli con modify NAT-Internet ipv4.method auto ipv4.route-metric 50
sudo nmcli con up NAT-Internet

# 2) Host-only (no default route)
sudo nmcli con modify HostOnly-56 ipv4.method manual ipv4.addresses 192.168.56.10/24 ipv4.never-default yes
sudo nmcli con up HostOnly-56

# 3) External zone (static IP, no default route)
sudo nmcli con modify External-172.16.10 ipv4.method manual ipv4.addresses 172.16.10.10/24 ipv4.gateway 172.16.10.1 ipv4.route-metric 200 ipv4.dns "8.8.8.8"
sudo nmcli con up External-172.16.10

# 4) DMZ zone (static IP, no default route)
sudo nmcli con modify DMZ-172.16.100 ipv4.method manual ipv4.addresses 172.16.100.10/24 ipv4.gateway 172.16.100.1 ipv4.route-metric 300
sudo nmcli con up DMZ-172.16.100
```

> `ipv4.never-default yes` = never add a default route for that connection.
> `ipv4.route-metric` controls route priority. Lower = preferred.

---

## 5) Verify

```bash
# Interfaces & IPs
ip -br a

# Routing table (should show ONE default via NAT)
ip route

# Connectivity
ping -c 3 8.8.8.8                 # Internet via NAT
curl ifconfig.me                  # See public IP via NAT

ping -c 3 192.168.56.1            # Host-only gateway (host)
ping -c 3 172.16.10.1             # External zone router (e.g., pfSense)
ping -c 3 172.16.100.1            # DMZ router
```

If you set DNS on a non-NAT interface, make sure the **“DNS priority/metric”** isn’t hijacking resolution; you can keep DNS coming from `NAT-Internet` or set a custom `/etc/resolv.conf` using resolvconf/NetworkManager.

---

## 6) GUI-only steps (if you prefer clicks)

1. Click the **network icon** (top-right) → **Wired Settings**.
2. For each connection:

   * Rename it (e.g., *NAT-Internet*, *HostOnly-56*, etc.).
   * **IPv4** tab → choose **Automatic (DHCP)** for NAT, **Manual** for others.
   * Enter Address/Netmask/Gateway as per the table.
   * **Routes…** → tick **“Use this connection only for resources on its network”** for host-only/DMZ/external (this is the GUI equivalent of `never-default`).
   * Save → toggle the connection **Off/On** to apply.

---

## 7) Optional: pick which interface owns the default route

If you want the **External-Zone** to be Kali’s default route instead of NAT:

```bash
sudo nmcli con modify NAT-Internet ipv4.never-default yes
sudo nmcli con modify External-172.16.10 ipv4.never-default no ipv4.route-metric 50
sudo nmcli con up External-172.16.10
```

Check `ip route` again and validate internet access still works (it will if that zone ultimately NATs to the Internet via pfSense/VirtualBox).

---

## 8) Snapshots

Once everything works:

* VirtualBox → **Take Snapshot** on Kali (e.g., `Kali-4NIC-initial`) to return here anytime.

---

## 9) Troubleshooting

* **Duplicate “Wired connection 1/2/3/4” keep appearing**
  Delete stale profiles, then reconnect:

  ```bash
  nmcli con show
  sudo nmcli con delete "Wired connection 1"
  sudo nmcli con delete "Wired connection 2"
  # …delete any extras you don't use
  ```

* **No internet**

  * Ensure only **one** default route (`ip route`).
  * Verify the connection with the default route has the **lowest metric**.
  * If using a NAT Network as default, the gateway is usually the `.1` of that NAT Network.

* **Interface shows “unmanaged”**

  ```bash
  sudo systemctl enable --now NetworkManager
  ```

  And ensure `/etc/NetworkManager/NetworkManager.conf` doesn’t exclude your NICs.

* **Can’t see traffic in Wireshark**

  * In VirtualBox, NIC **Promiscuous Mode = Allow All**.
  * Use the correct interface (e.g., `enp0s9` for External zone).

---

## 10) What this gives you

* Internet access from Kali (updates, tool installs).
* A **Host-Only** management lane to your host.
* Direct presence in **External** and **DMZ/Internal** zones for offensive testing.
* Clean routing so traffic goes where *you* intend.
