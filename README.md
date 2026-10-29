<div align="center">

# 🔐 Cybersecurity Lab Environment Setup

### Building an isolated virtual lab for penetration testing and ethical hacking practice

<p>
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ver-VirtualBox%20v7.2-0070C0?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Target-Windows%2010-0078D6?style=flat-square&labelColor=000000&logo=windows&logoColor=white" />
  <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Skill-Linux%20Networking-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" />
</p>

**NetworkWalks Internship · Week 01 Project**

*A two-node virtual cybersecurity laboratory built with Oracle VirtualBox, Kali Linux, and Windows 10<br>for controlled reconnaissance, vulnerability assessment, and authorized penetration-testing practice.*

[Overview](#-project-overview) · [Architecture](#%EF%B8%8F-lab-architecture) · [Setup](#-step-by-step-implementation) · [Verification](#-verification--testing) · [Troubleshooting](#-encountered-issues--engineering-solutions) · [Takeaways](#-key-takeaways)

</div>

---

## 📌 Project Overview

This project documents the installation, configuration, and verification of a **multi-node virtual cybersecurity and penetration-testing lab** built on Oracle VirtualBox.

A Kali Linux attacker machine and a Windows 10 target machine share a private NAT Network. The two VMs can reach each other directly, stay separate from the host's local network, and keep outbound access for security updates and domain-name testing.

### At a Glance

| | |
| :--- | :--- |
| **Attacker** | Kali Linux 2026.2 · `10.0.0.2/24` · 4 GB RAM |
| **Target** | Windows 10 · `10.0.0.4/24` · 4 GB RAM |
| **Network** | VirtualBox NAT Network `NatNetwork` · `10.0.0.0/24` |
| **Host** | Windows 11 (64-bit) · 16 GB RAM |
| **Recovery** | Clean baseline snapshot on both VMs |
| **Resolved** | NetworkManager profile error · DNS failure · Guest Additions |

---

## 🛡️ Purpose of the Lab

The lab provides an isolated, controlled environment for cybersecurity learning and authorized security testing, including:

- 🔍 Network reconnaissance and port scanning
- 🧪 Vulnerability assessment
- 📡 Packet analysis
- 🎯 Controlled exploitation practice against the Windows 10 target

> [!WARNING]
> This laboratory must only be used on systems that you own or have explicit permission to test. Do not use it against unauthorized systems.

---

## 🎯 Objectives

- [x] Deploy and configure **Oracle VirtualBox 7.2** on a Windows 11 host.
- [x] Deploy **Kali Linux 2026.2** as the dedicated penetration-testing workstation.
- [x] Deploy **Windows 10** as an internal target node.
- [x] Configure a unified **NAT Network** (`10.0.0.0/24`) for inter-VM communication.
- [x] Troubleshoot IP assignment, duplicate address detection (DAD), and DNS resolution.
- [x] Configure Guest Additions for clipboard and shared-folder support.
- [x] Document baseline testing and recovery snapshots.

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Specification |
| :--- | :--- |
| 🖥️ Host System | Windows 11 (64-bit) |
| 🧠 Host RAM | 16 GB |
| 🧰 Hypervisor | Oracle VirtualBox 7.2 |
| 🌐 Virtual Network | NAT Network (`NatNetwork`) |
| 📡 Network Subnet | `10.0.0.0/24` |
| 🚪 Default Gateway | `10.0.0.1` |
| 🌍 Primary DNS | `8.8.8.8` (Google Public DNS) |
| 🌍 Secondary DNS | `1.1.1.1` (Cloudflare DNS) |
| 🐉 Attacker VM (Kali) | `10.0.0.2/24` · 4096 MB RAM |
| 🎯 Target VM (Windows 10) | `10.0.0.4/24` · 4096 MB RAM |

---

## 🏗️ Lab Architecture

<div align="center">
  <img width="780" alt="Cybersecurity Lab Network Architecture" src="https://github.com/user-attachments/assets/34e43ada-0587-4e22-82b2-f758d1d00b12" />
  <br>
  <sub><i>Lab network architecture</i></sub>
</div>

<details>
<summary><b>View text diagram</b></summary>

```text
            Windows 11 Host (16 GB RAM)
                       │
             Oracle VirtualBox 7.2
                       │
        ┌──────────────┴──────────────┐
        │   NAT Network: NatNetwork   │
        │   10.0.0.0/24  GW 10.0.0.1  │
        └──────┬───────────────┬──────┘
               │               │
      Kali Linux 2026.2     Windows 10
      Attacker              Target
      10.0.0.2/24           10.0.0.4/24
```

</details>

---

## 🪜 Step-by-Step Implementation

### Step 1 · VirtualBox NAT Network Configuration

A custom NAT Network was created so the Kali attacker VM and Windows 10 target VM can communicate while staying separate from the local home network.

| Setting | Value |
| :--- | :--- |
| Network Name | `NatNetwork` |
| Network CIDR | `10.0.0.0/24` |
| DHCP | Enabled |
| IPv6 | Disabled |

<div align="center">
  <img width="359" height="332" alt="VirtualBox NAT Network settings" src="https://github.com/user-attachments/assets/507f86e6-0128-405b-9122-fde5d270577f" />
  <br>
  <sub><i>NAT Network settings in VirtualBox</i></sub>
</div>

---

### Step 2 · Kali Linux Workstation Setup

- Imported Kali Linux (`2026.2-virtualbox-amd64`) with **4096 MB RAM**.
- Attached Network Adapter 1 to `NAT Network` (`NatNetwork`).
- Configured static DNS and verified adapter status with `nmcli` and `ip -br a`.

<div align="center">
  <img width="386" height="253" alt="Kali Linux attached to NatNetwork" src="https://github.com/user-attachments/assets/346a20df-a6af-4b64-9a26-ca53a01b5523" />
  <br>
  <sub><i>Kali Linux attached to NatNetwork</i></sub>
</div>

---

### Step 3 · Windows 10 Target VM Configuration

- Deployed a Windows 10 virtual machine with **4096 MB RAM**, attached to `NatNetwork`.
- Verified IP assignment (`10.0.0.4`) and the default gateway (`10.0.0.1`).
- Installed **VirtualBox Windows Guest Additions** (`VBoxWindowsAdditions.exe`) to enable shared clipboard and folder sharing.

---

### Step 4 · Baseline Recovery Snapshots

After bidirectional connectivity and DNS resolution were confirmed on both systems, a clean snapshot was taken of each VM:

| VM | Snapshot Name |
| :--- | :--- |
| 🐉 Kali Linux | `Clean Baseline - Network Ready` |
| 🎯 Windows 10 | `Clean Baseline - Additions Installed` |

> [!TIP]
> Restore these snapshots before and after running security tools to return to a known-good state.

---

## 🔎 Verification & Testing

| Test | Command | Result |
| :--- | :--- | :--- |
| 🐉 Kali interface check | `ip -br a` | ✅ `eth0 UP 10.0.0.2/24` |
| 🐉 Kali gateway ping | `ping -c 3 10.0.0.1` | ✅ 3 packets received, 0% loss |
| 🐉 Kali internet reachability | `ping -c 3 8.8.8.8` | ✅ 3 packets received, 0% loss |
| 🐉 Kali DNS resolution | `ping -c 3 google.com` | ✅ Resolved and responsive |
| 🎯 Windows 10 IP check | `ipconfig` | ✅ IPv4 `10.0.0.4` |
| 🎯 Windows 10 internet reachability | `ping 8.8.8.8` | ✅ 4 packets received, 0% loss |
| 🎯 Windows 10 DNS test | `ping google.com` | ✅ Resolved and responsive |

---

## 🐞 Encountered Issues & Engineering Solutions

### Issue 1 · NetworkManager Profile Mismatch (`unknown connection`)

- **Problem:** Kali returned `Error: unknown connection 'Wired connection 1'` when applying IPv4 changes.
- **Root cause:** Profile name inconsistency in NetworkManager.
- **Solution:** Found the profile's unique identifier with `nmcli connection show` and applied the changes through its **UUID**:

```bash
sudo nmcli connection modify 275787cd-7eff-439c-be7f-948d52ce71ed ipv4.dad-timeout 0
sudo nmcli connection up 275787cd-7eff-439c-be7f-948d52ce71ed
```

> [!NOTE]
> The UUID above belongs to my VM. Run `nmcli connection show` and use your own.

<div align="center">
  <img width="220" height="177" alt="Issue 1 screenshot" src="https://github.com/user-attachments/assets/a784c0bc-a520-4c13-acaf-16d59b22db2c" />
  &nbsp;&nbsp;
  <img width="182" height="136" alt="Issue 1 troubleshooting screenshot" src="https://github.com/user-attachments/assets/693e33b4-c1ba-4766-9ba1-3f3b57ff1ba5" />
  <br>
  <sub><i>Screenshots from the NetworkManager troubleshooting</i></sub>
</div>

---

### Issue 2 · Ping Worked, but Browsing Failed (DNS Resolution)

- **Problem:** Both VMs could ping `8.8.8.8`, yet browsers returned `Server Not Found` and `ping google.com` could not find the host.
- **Root cause:** The VirtualBox DHCP lease did not provide functional public DNS resolvers.
- **Solution:**

**Kali Linux:**

```bash
sudo nmcli connection modify 275787cd-7eff-439c-be7f-948d52ce71ed ipv4.dns "8.8.8.8 1.1.1.1"
sudo nmcli connection up 275787cd-7eff-439c-be7f-948d52ce71ed
```

**Windows 10:** Open `ncpa.cpl` → IPv4 Properties, then set Preferred DNS to `8.8.8.8` and Alternate DNS to `1.1.1.1`.

---

### Issue 3 · Inactive Guest Additions & Clipboard Lock

- **Problem:** Bidirectional copy-paste between the host and the Windows 10 VM did not work.
- **Root cause:** VirtualBox Guest Additions drivers were not installed on the Windows 10 VM.
- **Solution:** Mounted the Guest Additions image via **Devices → Insert Guest Additions CD image...**, ran `VBoxWindowsAdditions.exe`, completed the wizard, and rebooted the VM.

---

## 💡 Key Takeaways

1. **Linux network administration (`nmcli`):** When NetworkManager throws profile errors such as `unknown connection`, profiles can be targeted reliably by UUID. The same tool sets Duplicate Address Detection (`ipv4.dad-timeout 0`) and public DNS servers.
2. **NAT vs. NAT Network:** A standard NAT adapter lets a single VM reach out to the internet only. A **NAT Network** creates a shared internal network where attacker and target VMs can talk to each other while keeping outbound routing.
3. **Layer 3 vs. Layer 7:** Separating IP routing problems (pinging `8.8.8.8`) from application-layer problems (DNS lookup) is essential when diagnosing connection failures on Linux and Windows.
4. **Guest Additions:** Clipboard and folder sharing only work once the Guest Additions drivers are installed inside the guest OS.
5. **Resilient baselines:** Clean snapshots taken before running security tools allow fast recovery from broken configurations or exploits.

---

## 🔐 Security & Ethical Use

This laboratory is intended strictly for education and authorized security testing.

---

## 🔗 Tools & Resources

| Tool | Link |
| :--- | :--- |
| Oracle VirtualBox | [Downloads](https://www.virtualbox.org/wiki/Downloads) · [User Manual](https://www.virtualbox.org/manual/) |
| Kali Linux | [Official download page](https://www.kali.org/get-kali/) |
| 7-Zip | [Official download page](https://www.7-zip.org/download.html) |
| Nmap | [nmap.org](https://nmap.org/) |
| Wireshark | [wireshark.org](https://www.wireshark.org/) |

---

## 👤 Author

<div align="center">

**Najeem Omokunmi Atanda**<br>
*IT Specialist / Aspiring Penetration Tester*<br>
**Devarotech**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Najeem%20Omokunmi%20Atanda-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/omokunmi-atanda-najeem-13443328a/)
[![GitHub](https://github.com/Devarotech/Cybersecurity-Lab-Setup/blob/main/README.md)

</div>

---

## 📌 Project Information

| | |
| :--- | :--- |
| **Programme** | NetworkWalks Cybersecurity Internship |
| **Project** | Cybersecurity and Penetration-Testing Lab Setup |
| **Week** | 01 |

<div align="center">
<sub>Built for learning · Documented for others · Use responsibly</sub>
</div>
