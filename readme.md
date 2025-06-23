# 🔍 Local Network Scanning using Nmap & Wireshark

This project is a part of my internship task. It involves scanning my local Wi-Fi network using **Nmap**, capturing packets using **Wireshark**, and analyzing the results to identify active devices, open ports, running services, and potential security risks.

---

## 🧰 Tools Used

- **Nmap** – For network scanning, port detection, OS and service fingerprinting
- **Wireshark** – For monitoring and analyzing packet-level network activity
- **Linux** – Operating system used for scanning and analysis

---

## 📝 Steps Performed

1. Install Nmap
Installed Nmap using APT:
```bash
sudo apt update
sudo apt install nmap

2. Find Your Local IP Range

Used the following command to determine the IP range:

ip a

    Identified device IP: 192.168.29.x

    So the full subnet used for scanning was: 192.168.29.0/24

3. Perform a TCP SYN Scan

Ran a basic Nmap SYN scan:

nmap -sS 192.168.29.112/24 -oN scan-results.txt

    -sS performs a TCP SYN scan (stealthy and fast)

    -oN saves output to scan-results.txt

✅ 4. Note Down IPs and Open Ports

Nmap returned results for 7 active hosts. Here's a summary:
IP Address	Open Ports	MAC Vendor / Device Info
192.168.29.1	80, 443, 1900, 7443, 8080, 8443	Servercom (India) Pvt. Ltd (Router)
192.168.29.13	None (all ports closed)	Unknown
192.168.29.43	None (all ports closed)	Xiaomi Communications
192.168.29.45	2869, 9080	Earda Technologies (Android device)
192.168.29.123	None	Unknown
192.168.29.214	5060, 44176 (filtered)	Unknown
192.168.29.112	None (all ports closed)	Likely the scanning device itself

All details are saved in scan-results.txt.
✅ 5. Capture Packet Traffic using Wireshark

    Opened Wireshark and selected the active interface

    Started packet capture before running the Nmap scan

    Applied filters to view SYN packets and web traffic:

tcp.flags.syn == 1 && tcp.flags.ack == 0
tcp.port == 80 || tcp.port == 443 || tcp.flags.syn == 1

Saved the full packet capture as:

    nmap-wireshark-capture.pcapng

6. Analyze Services and Ports

    Common services like HTTP, HTTPS, UPNP, and proxy were found on the router.

    Port 2869 is used for ICS service, and 9080 often hosts custom APIs.

    Devices exposing web interfaces (like ports 80, 443, 8080) should be checked for vulnerabilities.

7. Identify Potential Security Risks
Observation	Risk Level	Recommendation
Open port 8080 and 8443 on router	
  - High	Disable unused web interfaces
UPNP on port 1900	
  - Medium	Disable UPNP in router settings if unused
Unknown services on Android device	
  - Medium	Review apps/services using ports
Multiple devices with unknown MACs
