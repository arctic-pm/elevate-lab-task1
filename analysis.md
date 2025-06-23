# 📊 Network Scan Analysis

This document summarizes the findings from the Nmap TCP SYN scan and Wireshark packet capture on the local Wi-Fi network (`192.168.29.0/24`). It includes an explanation of detected services, their typical usage, and potential security concerns.

---

## 🔍 Summary of Devices Found

| IP Address       | MAC Vendor                 | Open Ports                 | Device Type (Guessed)       |
|------------------|----------------------------|----------------------------|-----------------------------|
| 192.168.29.1     | Servercom (India) Pvt Ltd  | 80, 443, 1900, 7443, 8080, 8443 | Router                     |
| 192.168.29.13    | Unknown                    | None                       | Likely mobile or IoT device|
| 192.168.29.43    | Xiaomi Communications      | None                       | Mobile device (Xiaomi)      |
| 192.168.29.45    | Earda Technologies         | 2869, 9080                 | Android phone or TV device |
| 192.168.29.123   | Unknown                    | None                       | Possibly Windows device     |
| 192.168.29.214   | Unknown                    | 5060 (filtered), 44176 (filtered) | Possibly VoIP device       |
| 192.168.29.112   | (My system)                | None                       | Scanner/Host machine       |

---

## 🔌 Port and Service Analysis

| Port | Service        | Description                                         | Risk Level | Notes                             |
|------|----------------|-----------------------------------------------------|------------|-----------------------------------|
| 80   | HTTP           | Web interface (unencrypted)                        | 🟠 Medium  | Found on router                   |
| 443  | HTTPS          | Secure web interface                               | 🟢 Low     | Secure but should be monitored   |
| 1900 | UPNP           | Universal Plug and Play for auto-discovery         | 🔴 High    | Commonly exploited in IoT attacks|
| 7443 | Oracleas-https | Alternate secure web service                       | 🟠 Medium  | May indicate remote admin portal |
| 8080 | HTTP-Proxy     | Alternate HTTP port, used by services/admin panels | 🔴 High    | Should not be open to public     |
| 8443 | HTTPS-Alt      | Alternate HTTPS port                               | 🟠 Medium  | Confirm it’s secured             |
| 2869 | ICSLAP         | Windows Internet Connection Sharing                | 🟠 Medium  | Should be disabled if unused     |
| 9080 | GLRPC (Custom) | Unknown custom service (on Android device)         | 🟠 Medium  | Review device permissions        |
| 5060 | SIP            | Voice-over-IP signaling                            | 🟠 Medium  | Filtered, may be legitimate use  |

---

## 📎 Wireshark Packet Observations

### Applied Filters:
- `tcp.flags.syn == 1 && tcp.flags.ack == 0` → Showed SYN packets from scan
- `tcp.port == 80 || tcp.port == 443 || tcp.flags.syn == 1` → Highlighted port scan patterns

### Key Takeaways:
- Nmap scan generated many SYN packets, confirming scan type
- Services on port 80/443 were probed
- All packets were seen targeting LAN devices — no outbound scan detected

---

## ⚠️ Security Recommendations

- **Disable unused ports** on router and Android devices
- **Turn off UPNP (1900)** in the router admin settings
- Ensure **HTTPS (443, 8443)** is used instead of HTTP (80)
- Review any **unknown open ports** on devices like 2869, 9080
- Monitor for **new/unknown MAC addresses** connecting to the network

---

## ✅ Conclusion

This analysis demonstrates how open ports and unmonitored services can pose risks on a local network. Using Nmap and Wireshark together provides a powerful way to:
- Detect live devices
- Understand what services are running
- Spot unnecessary exposures
- Build awareness of network hygiene


