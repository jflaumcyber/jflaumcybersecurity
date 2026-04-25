# 🏠 Home Lab Setup

A walkthrough of my personal cybersecurity home lab, including a Wazuh SIEM deployment and virtual machine installations for Kali Linux and Ubuntu.

---

## 🛡️ Wazuh SIEM — Single Node Installation

[Wazuh](https://wazuh.com/) is an open-source security information and event management (SIEM) platform used for threat detection, integrity monitoring, and incident response.

### Installation

**1. Run the install script:**
```bash
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
```

**2.** When the script finishes, note down the **admin password** shown in the terminal.

**3.** Open a browser and navigate to the IP address or DNS name of your Wazuh VM. The Wazuh dashboard should load, giving you access to your SIEM environment.

**4.** Log in with username `admin` and the password from your terminal output.

---

### Setting Up Decoders and Rules for Syslog Integration

A GitHub repository with decoders and rules tailored for syslog forwarding is available at:

```
https://github.com/object1st/wazuh-examples.git
```

#### File Placement

Instead of modifying the default files, create separate files for better organization.

**Decoder files** → place in `/var/ossec/etc/decoders/`:
- `honeypot_application_decoders.xml`
- `ootbi_working_decoders.xml`

**Rules files** → place in `/var/ossec/etc/rules/`:
- `honeypot_application_rules.xml`
- `ootbi_final_rules.xml`

---

### Configuring Wazuh to Receive Syslog Messages

**1.** Add the following block to `/var/ossec/etc/ossec.conf` under `</ossec_config>`:

```xml
<remote>
    <connection>syslog</connection>
    <port>514</port>
    <protocol>udp</protocol>
    <allowed-ips>192.168.0.0/24</allowed-ips>
    <allowed-ips>127.0.0.1</allowed-ips>
</remote>
```

**2.** Update `allowed-ips` to match your subnet.

**3.** Restart the Wazuh manager:
```bash
systemctl restart wazuh-manager
```

**4.** Verify the port is listening:
```bash
ss -tunlp
```

---

## 🐉 Kali Linux VM — VirtualBox Installation

[Kali Linux](https://www.kali.org/) is a Debian-based distribution built for penetration testing and security research.

> **Note:** You may need to enable virtualization in your BIOS/UEFI (e.g., Intel VT-x / AMD-V) before proceeding.

### Creating the VM

1. Open VirtualBox and click **Machine → New**.
2. **Name and OS:** Name the VM (e.g., `kali-linux-2026.1-vbox-amd64`). Set **Type** to `Linux` and **Version** to `Debian (64-bit)`.
3. **Memory:** Set RAM to at least `2048 MB (2 GB)`. More is better for running security tools.
4. **Hard Disk:** Select **Create a virtual hard disk now** → **VDI** format → **Dynamically allocated** → `80 GB`.
5. Click **Create** to complete the wizard.

### Configuring Settings

Open **Settings** for the VM and apply the following:

| Section | Setting |
|---|---|
| General → Advanced | Shared Clipboard: **Bidirectional**, Drag'n'Drop: **Bidirectional** |
| System → Motherboard | Boot Order: Hard Disk (1st), Optical (2nd) |
| System → Processor | Processors: **2**, Enable PAE/NX: ✅ |
| Display → Screen | Video Memory: **128 MB**, Accelerated 3D: ❌ Disabled |

### Starting and Installing

1. Press **Start**. When prompted, click the folder icon to mount your Kali ISO.
2. In the **Optical Disk Selector**, click **Add**, navigate to your downloaded ISO, select it, and press **Choose**.
3. Click **Start** and proceed through the [Kali Linux installation wizard](https://www.kali.org/docs/installation/hard-disk-install/).

> The install wizard should automatically detect it's inside a VM and install VirtualBox Guest Additions for a better experience.

### Expanding Storage (VirtualBox 7.0+)

1. Power off the VM.
2. Go to **File → Tools → Virtual Media Manager**.
3. Select your `.vdi` file and adjust the size slider, then click **Apply**.
4. Boot Kali and use `gparted` to extend the partition to use the new space.

---

## 🐧 Ubuntu VM — VirtualBox Installation

[Ubuntu](https://ubuntu.com/) is a beginner-friendly Linux distribution great for general-purpose use and development.

### Step 1 — Download Ubuntu

Download the latest Ubuntu Desktop ISO from: [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)

### Step 2 — Install VirtualBox

Download and install VirtualBox from: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

### Step 3 — Create a New VM

Click **New** and fill in the details:
- **Name:** Include "Ubuntu" so the Type and Version auto-fill.
- **ISO Image:** Browse to your downloaded Ubuntu ISO.
- Leave "Skip Unattended Installation" unchecked for an automated setup.

### Step 4 — Create a User Profile

Set your credentials during setup. The defaults are:
- **Username:** `vboxuser`
- **Password:** `changeme`

> ⚠️ **Change these** — the defaults create a user without sudo access.

Enable **Guest Additions** for quality-of-life features like dynamic screen resizing.

### Step 5 — Define Resources

| Resource | Recommended |
|---|---|
| RAM | 8 GB (4 GB minimum) |
| CPUs | 4 |
| Hard Disk | 25 GB minimum |

Stay in the **green zones** of each slider to avoid starving your host OS.

### Step 6 — Start and Install

Click **Start**. The unattended installer will run automatically — don't interact with the initial boot prompt. The machine will reboot once installation completes.

Log in with your username and password (default: `changeme` if unchanged).

### Post-Install Updates

```bash
sudo apt update && sudo apt upgrade -y
sudo snap refresh
```
