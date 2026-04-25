# 🦈 Wireshark Labs

A series of hands-on network analysis labs completed during **Malware Analysis (ITD-3653)** at OSU-IT. All labs received a grade of **100/100**.

---

## Lab 4 — Analyzing HTTP Traffic

**Course:** Fall 2024 Malware Analysis (ITD-3653) | **Grade:** 100/100

### Overview
HTTP (Hypertext Transfer Protocol) is the backbone of the World Wide Web. In this lab, Wireshark was used to analyze a pre-captured `.pcap` file to identify hosts, users, and suspicious activity hidden within HTTP traffic.

### Lab Steps

**1.** Download the PCAP file from:
- Tutorial reference: [Palo Alto Unit 42 – Decrypting HTTPS Traffic](https://unit42.paloaltonetworks.com/wireshark-tutorial-decrypting-https-traffic/)
- PCAP source: [malware-traffic-analysis.net (2019-11-12)](https://www.malware-traffic-analysis.net/2019/11/12/index.html)

**2.** Launch Wireshark and open the unzipped PCAP file.

**3.** Answer the following analysis questions using Wireshark filters and inspection:

| Question | Answer |
|---|---|
| OS/device at 10.11.11.94? | Identified via HTTP User-Agent |
| OS/device at 10.11.11.121? | Identified via HTTP User-Agent |
| Vendor for MAC at 10.11.11.145? | Identified via OUI lookup |
| OS/device at 10.11.11.179? | Identified via HTTP User-Agent |
| Windows version at 10.11.11.195? | Identified via NBNS/HTTP |
| User account at 10.11.11.200? | Identified via Kerberos/SMB |
| OS/device at 10.11.11.217? | Identified via HTTP User-Agent |
| Which host downloaded a Windows executable over HTTP? | Identified by filtering `http` and looking for `.exe` transfers |
| URL that returned the executable? | Found in HTTP object export |
| SHA256 of the executable? | `8d5d36c8ffb0a9c81b145aa40c1ff3475702fb0b5f9e08e0577bdc405087e635` |
| VirusTotal detection rate? | Checked at [virustotal.com](https://virustotal.com) |
| Public IPs connected to over TCP after download? | Identified via TCP stream analysis |
| Hostname and user on that IP? | `Tucker-Win7-PC` / `candice.tucker` |

---

## Lab 5 — Identifying Hosts and Users

**Course:** Fall 2024 Malware Analysis (ITD-3653) | **Grade:** 100/100

### Overview
This lab focused on using Wireshark to map network activity back to specific devices and user accounts — a critical skill for network forensics and incident response.

Reference: [Palo Alto Unit 42 – Identifying Hosts and Users](https://unit42.paloaltonetworks.com/using-wireshark-identifying-hosts-and-users/)

### Lab Steps

1. **Launch Wireshark** and select a network interface (Wi-Fi or Ethernet).
2. **Start capturing packets** — observe source/destination addresses, protocols, and lengths in the main window.
3. **Identify Source and Destination IPs** using the `Source` and `Destination` columns.
4. **Group packets by IP** — right-click a packet → *Apply as Filter → Selected → A → B* to isolate traffic from a specific host.
5. **Analyze traffic by host** — examine patterns, protocols used, and timestamps to build a picture of device behavior.

**Key Identifiers Used:**
- IP addresses
- MAC addresses (OUI vendor lookup)
- HTTP User-Agent strings
- Kerberos authentication packets
- NBNS (NetBIOS Name Service) broadcasts

---

## Lab 7 — Changing Column Display

**Course:** Fall 2024 Malware Analysis (ITD-3653) | **Grade:** 100/100

### Overview
This lab covered customizing Wireshark's column layout to streamline packet analysis workflows.

Reference: [Palo Alto Unit 42 – Customizing Wireshark Column Display](https://unit42.paloaltonetworks.com/unit42-customizing-wireshark-changing-column-display/)

### Summary
By right-clicking on the column headers in Wireshark, you can add, remove, or rearrange columns to surface the most relevant data — such as source/destination addresses, protocols, and packet sizes. Tailoring the display to the specific investigation makes it significantly easier to focus during network troubleshooting or malware analysis sessions.

---

## Lab 8 — Decrypting HTTPS Traffic

**Course:** Fall 2024 Malware Analysis (ITD-3653) | **Grade:** 100/100

### Overview
HTTPS encrypts web traffic using TLS/SSL — but with the right key log file, Wireshark can decrypt it for analysis. This is essential for investigating malware that uses HTTPS to blend in with normal traffic.

Reference: [Palo Alto Unit 42 – Decrypting HTTPS Traffic](https://unit42.paloaltonetworks.com/wireshark-tutorial-decrypting-https-traffic/)

### Summary
Using a pre-session key log file (SSLKEYLOGFILE) captured during the original recording, Wireshark 3.x can decrypt HTTPS traffic to reveal the underlying HTTP requests and responses. The lab focused on analyzing a **Dridex malware** infection, examining what the compromised host communicated after the initial infection. Customizing the Wireshark display for this analysis was also covered, reinforcing the skills from Lab 7.

**Key Takeaways:**
- HTTPS is used by both legitimate sites and malware — it does not equal safety
- The SSLKEYLOGFILE environment variable can be set on a test machine to capture TLS session keys
- Wireshark's TLS dissector can load this key log to decrypt traffic retrospectively

---

## Lab 9 — Identifying Hosts and Users (Advanced)

**Course:** Fall 2024 Malware Analysis (ITD-3653) | **Grade:** 100/100

### Overview
Building on Lab 5, this lab deepened the technique of mapping network activity to specific devices and accounts using more advanced Wireshark filtering.

Reference: [Palo Alto Unit 42 – Identifying Hosts and Users](https://unit42.paloaltonetworks.com/using-wireshark-identifying-hosts-and-users/)

### Summary
By capturing and examining data packets — including IP addresses, MAC addresses, and user-agent strings — it's possible to track the activity of specific devices and users on a network. Applying targeted Wireshark filters helped isolate traffic from individual devices and applications, providing a clearer picture of network behavior and exposing potential security concerns. These skills are directly applicable to blue team monitoring and incident response.
