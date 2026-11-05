<div align="center">

# 🔐 Cybersecurity Lab Environment Setup

**Building an isolated virtual lab for penetration testing and ethical hacking practice**

![Skill](https://img.shields.io/badge/Skill-Cybersecurity-red)
![VirtualBox](https://img.shields.io/badge/VirtualBox-v7.x-blue)
![Kali](https://img.shields.io/badge/Kali%20Linux-2026.2-black)
![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-teal)
![Practice](https://img.shields.io/badge/Skill-Penetration%20Testing-red)
![Virtualization](https://img.shields.io/badge/Skill-Virtualization-red)
![GitHub](https://img.shields.io/badge/GitHub-Project-lightgrey)
![NetworkWalks](https://img.shields.io/badge/NetworkWalks-Batch%20B084E-orange)

</div>

---

## 📌 Project Overview

This project is **Week 1, Project Module 1 (WK1-PM1)** of the Cybersecurity Internship Program (Batch B084E).

The goal is to set up a **virtual cybersecurity and penetration-testing laboratory** using **VirtualBox** and **Kali Linux**.

The lab gives me a controlled, isolated environment where security tools, network scanning, reconnaissance and vulnerability assessment can be practised **safely and repeatedly**, without touching any real systems.

The lab runs on a private virtual network (`10.0.0.0/24`) so more machines (Windows, Android, etc.) can be added later and used as targets for authorized security testing.

> ⚠️ **Legal note:** Everything in this repository was done on my **own laptop and my own virtual lab**. No real or third-party systems were involved.

---

## 🎯 Task Requirements

| # | Requirement | Status |
|---|-------------|--------|
| 1 | Use VirtualBox as the base (latest recommended version) | ✅ |
| 2 | Set up Kali Linux as the attacking / hacker machine | ✅ |
| 3 | Set up the network in subnet `10.0.0.0/24` | ✅ |
| 4 | Use a **NAT Network** with `10.0.0.0/24` | ✅ |
| 5 | Clipboard & drag/drop enabled in VM settings | ✅ |
| 6 | Shared folder enabled (host `Downloads` folder shared) | ✅ |
| 7 | Kali Linux IP address is `10.0.0.2/24` | ✅ |
| 8 | Kali Linux has full Internet access | ✅ |
| 9 | Snapshot of the VM taken | ✅ |

---

### Planned IP Addressing (all on `NatNetwork`, range `10.0.0.2 – 10.0.0.99 /24`)

| Machine | IP Address | Role | Status |
|---------|-----------|------|--------|
| Kali Linux | `10.0.0.2/24` | Attacker | ✅ Done |
| Windows 7 | `10.0.0.7/24` | Target | Phase 2 (optional) |
| Android 9x | `10.0.0.9/24` | Target | Phase 2 (optional) |
| Windows 10 | `10.0.0.10/24` | Target | Phase 2 (optional) |
| Windows 11 | `10.0.0.11/24` | Target | Phase 2 (optional) |
| Windows Server 2016 | `10.0.0.16/24` | Target | Phase 2 (optional) |

---

## 💻 Host System

| Item | Details |
|------|---------|
| Main OS | Windows 10 |
| RAM | 8 GB |
| Storage |, 256 GB SSD  |
| Processor |  Core  i5  |
| Virtualization | Hypervisor: Oracle VirtualBox 7. |

> Recommended minimum: 8 GB RAM, 256 GB SSD, Core i3/i5 processor.

---

## 🛠️ Step-by-Step Setup (Phase 1)

### Step 1 Download & install 7-Zip

7-Zip is needed to extract the compressed Kali Linux VM archive.

- Download: https://7-zip.org/download.html
- Run the installer and finish with the default options.

---

### Step 2 Download & install VirtualBox

- Download the latest version for your host OS: https://virtualbox.org/wiki/Downloads
- Install it with the default options (accept the network driver prompts).
- Open VirtualBox and confirm it launches properly.

---

### Step 3 Create the NAT Network (`10.0.0.0/24`)

1. In VirtualBox go to **File → Tools → Network** (shortcut `Ctrl + H`).
2. Open the **NAT Networks** tab and click **Create**.
3. Configure the new network:

| Setting | Value |
|---------|-------|
| Name | `NatNetwork` |
| IPv4 Prefix | `10.0.0.0/24` |
| Enable DHCP | ✅ Checked |
| Enable IPv6 | ❌ Unchecked |

4. Click **Apply**.
---

### Step 4 Download & import the Kali Linux VM

1. Go to https://kali.org/get-kali and choose **Virtual Machines → VirtualBox (64-bit)**.
2. Download the archive and extract it with 7-Zip.
3. Double-click the `.vbox` file (or use **Machine → Add**) to import it into VirtualBox.
4. Confirm the VM appears in the VirtualBox Manager list.

---

### Step 5 Configure Kali VM settings

#### 5.1 Network adapter

Open the Kali VM **Settings → Network → Adapter 1**:

| Setting | Value |
|---------|-------|
| Enable Network Adapter | ✅ |
| Attached to | `NAT Network` |
| Name | `NatNetwork` |
| Adapter Type | Intel PRO/1000 MT Desktop (82540EM) |
| Promiscuous Mode | `Allow All` |
| Cable Connected | ✅ |

#### 5.2 Clipboard & drag/drop

Open **Settings → General → Advanced**:

- **Shared Clipboard:** `Bidirectional`
- **Drag'n'Drop:** `Bidirectional`

#### 5.3 Shared folder (host `Downloads`)

Open **Settings → Shared Folders → Add**:

| Setting | Value |
|---------|-------|
| Folder Path | Host `Downloads` folder |
| Folder Name | `Downloads` |
| Read-only | ❌ Unchecked (Full access) |
| Auto-mount | ✅ Checked |
| Make Permanent | ✅ Checked |

Inside Kali, to access the shared folder (usually mounted at `/media/sf_Downloads`), add your user to the `vboxsf` group and reboot:

```bash
sudo usermod -aG vboxsf $USER
sudo reboot
```
---

### Step 6 Set the static IP on Kali (`10.0.0.2/24`)

1. Start the Kali VM and log in.
2. Click the network icon in the top-right panel → **Edit Connections…**
3. Select **Wired connection 1** → **IPv4 Settings** and set:

| Setting | Value |
|---------|-------|
| Method | `Manual` |
| Address | `10.0.0.2` |
| Netmask | `24` |
| Gateway | `10.0.0.1` |
| DNS servers | `8.8.8.8` *(use `10.0.0.1` if Internet gives issues)* |

4. Click **Save**, then disconnect/reconnect the network.

---

### Step 7 Verify the configuration

Run these commands in the Kali terminal:

```bash
# 1. Confirm the IP address
ip a

# 2. Confirm the default gateway
ip route

# 3. Test the virtual network gateway
ping -c 4 10.0.0.1

# 4. Test Internet connectivity (by IP)
ping -c 4 8.8.8.8

# 5. Test DNS resolution
ping -c 4 google.com
```

**Expected result:** Kali shows `10.0.0.2/24`, and all pings succeed with 0% packet loss.

---

### Step 8 Take a snapshot

Taking a snapshot lets me restore the VM to this clean, working state at any time.

1. Shut down Kali (or take a live snapshot).
2. In VirtualBox Manager select the Kali VM → **Snapshots** tab → **Take**.
3. Name it something clear, e.g. `Fresh Kali - Lab Setup Complete`.

---

### 📝 My own troubleshooting experience

> **Problem I faced:** No problem i faced

---

## ✅ Final Result

- VirtualBox installed and working
- Custom `NatNetwork` created on `10.0.0.0/24`
- Kali Linux imported and attached to `NatNetwork`
- Static IP `10.0.0.2/24` configured
- Full Internet access confirmed
- Clipboard, drag/drop and shared folder working
- Clean snapshot saved

---
![Uploading kali post.png…]()


## 📚 What I Learned

- How to build an isolated, safe virtual lab for security practice
- How NAT Networks differ from a plain NAT adapter and why they let VMs talk to each other
- How subnetting (`/24`) and static IP configuration work in practice
- How to troubleshoot network problems with `ip`, `ping` and `nmcli`
- Why snapshots are important before experimenting

---

## ⚖️ Legal Disclaimer

This project is for **education and research purposes only**. The aim is to understand how attacks work so systems can be defended better.

Hacking is only legal when:

- You test a device or network that you **own**, or your own **lab environment**
- You have **written and documented permission** from the owner
- You work as a security professional under a **signed agreement with an agreed scope**

Everything outside these cases is illegal. All work in this repository was done only in my own virtual lab.

---

## 👤 Author

**Name:** BENEDICTO RAIMUND MKORONGO
**Program:** Cybersecurity Internship — Batch B084E
**Project:** Week 1 · PM1 · Cybersecurity Lab Setup
**Instructor:** Waqas Karim (CCIE)
**LinkedIn:** www.linkedin.com/in/benedicto-mkorongo-b7a5b9446

---

<div align="center">

**#Networkwalks #Cybersecurity #EthicalHacking #KaliLinux #VirtualBox**

</div>
