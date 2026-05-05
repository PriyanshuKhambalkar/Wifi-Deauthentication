<div align="center">

```
██╗    ██╗██╗      ███████╗██╗    ██████╗ ███████╗ █████╗ ██╗   ██╗████████╗██╗  ██╗
██║    ██║██║      ██╔════╝██║    ██╔══██╗██╔════╝██╔══██╗██║   ██║╚══██╔══╝██║  ██║
██║ █╗ ██║██║█████╗█████╗  ██║    ██║  ██║█████╗  ███████║██║   ██║   ██║   ███████║
██║███╗██║██║╚════╝██╔══╝  ██║    ██║  ██║██╔══╝  ██╔══██║██║   ██║   ██║   ██╔══██║
╚███╔███╔╝██║      ██║     ██║    ██████╔╝███████╗██║  ██║╚██████╔╝   ██║   ██║  ██║
 ╚══╝╚══╝ ╚═╝      ╚═╝     ╚═╝    ╚═════╝ ╚══════╝╚═╝  ╚═╝ ╚═════╝    ╚═╝   ╚═╝  ╚═╝
```

### Wi-Fi Deauthentication Attack — Educational Demonstration

*Understanding wireless network disruption through 802.11 deauthentication frames.*

<br/>

<img src="https://img.shields.io/badge/Python-3.x-3776ab?style=for-the-badge&logo=python&logoColor=white&labelColor=1a1a2e"/>
<img src="https://img.shields.io/badge/Scapy-Packet%20Crafting-ef4444?style=for-the-badge&logoColor=white&labelColor=1a1a2e"/>
<img src="https://img.shields.io/badge/aircrack--ng-Wireless%20Tools-10b981?style=for-the-badge&logoColor=white&labelColor=1a1a2e"/>
<img src="https://img.shields.io/badge/Kali%20Linux-Required-557c94?style=for-the-badge&logo=kalilinux&logoColor=white&labelColor=1a1a2e"/>
<img src="https://img.shields.io/badge/Purpose-Educational%20Only-f59e0b?style=for-the-badge&logoColor=white&labelColor=1a1a2e"/>

<br/><br/>

> ⚠️ **IMPORTANT DISCLAIMER**
> This project is strictly for **educational and authorized research purposes only.**
> Performing deauthentication attacks on networks you do not own or have explicit
> written permission to test is **illegal** under computer fraud and wireless
> communication laws in most countries. The author bears no responsibility for misuse.

<br/>

</div>

---

## 📖 What is This Project?

**Wi-Fi Deauthentication** is an educational Python project that demonstrates how the 802.11 deauthentication attack works at the packet level. This attack exploits a fundamental design limitation in the original Wi-Fi protocol — management frames (including deauthentication frames) are not authenticated, meaning any device can craft and send them.

This project is designed to help cybersecurity students understand wireless network security concepts, Wi-Fi protocol vulnerabilities, and why modern networks use Protected Management Frames (PMF / 802.11w) as a defence. Understanding how attacks work is the first step toward building better defences.

---

## ✨ Features

- 📡 **Deauth Packet Crafting** — Sends 802.11 deauthentication frames to a target
- 🎯 **Targeted Disconnection** — Disconnect a specific client or all clients from an AP
- 📋 **Network Scanning** — Discover nearby networks and connected clients
- 🔁 **Continuous Mode** — Send repeated deauth frames until stopped
- 🖥️ **Monitor Mode Support** — Works with any wireless adapter supporting monitor mode
- 📊 **On-Screen Feedback** — Real-time output showing packets sent

---

## 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| Python 3.x | Core scripting language |
| Scapy | 802.11 packet crafting and injection |
| aircrack-ng suite | Monitor mode, network discovery |
| aireplay-ng | Deauthentication frame transmission |
| airodump-ng | Wireless network and client scanning |

---

## 📁 Project Structure

```
Wifi-Deauthentication/
│
├── deauth.py           # Main script — packet crafting and injection logic
├── requirements.txt    # Python dependencies
├── README.md           # Project documentation
└── LICENSE             # License file
```

---

## 🧠 How It Works

```
802.11 Protocol Background
─────────────────────────
Normal flow:     Client ←──── Association ────→ Access Point
                 Client ←──── Authentication ──→ Access Point

Deauth attack:   Attacker ──── Deauth Frame ──→ Client (spoofed as AP)
                              (unauthenticated — no signature required)
                 Client disconnects ✓
```

**Step-by-step execution:**

```
Script starts
      │
      ▼
Wireless adapter set to Monitor Mode
      │
      ▼
airodump-ng scans for nearby networks
      │
      ▼
Target BSSID (AP MAC) and client MAC selected
      │
      ▼
Scapy crafts 802.11 deauth frames
      │  (Source: spoofed as AP MAC)
      │  (Destination: client MAC or broadcast)
      ▼
Frames injected at selected rate
      │
      ▼
Target client disconnected from AP
      │
      ▼
Process repeats until stopped (Ctrl+C)
```

The attack works because the original IEEE 802.11 standard did not require management frames to be cryptographically signed. Modern networks using **WPA3** or **802.11w (PMF)** are protected against this attack.

---

## ⚙️ Requirements

**Operating System:**
- Kali Linux (recommended)
- Ubuntu, Parrot OS, or any Debian-based Linux

**Hardware:**
- Wireless adapter supporting **monitor mode** and **packet injection**
- Recommended chipsets: Atheros AR9271, Ralink RT3070, Realtek RTL8812AU

**Software:**
```bash
# Update system
sudo apt update

# Install aircrack-ng suite
sudo apt install aircrack-ng -y

# Install Python dependencies
pip install scapy
```

---

## ▶️ Usage

> ⚠️ **Only run on networks you own or have written permission to test.**

**Step 1 — Identify your wireless interface:**
```bash
iwconfig
# Note your interface name — usually wlan0 or wlan1
```

**Step 2 — Enable monitor mode:**
```bash
sudo airmon-ng start wlan0
# Interface is now wlan0mon
```

**Step 3 — Scan for nearby networks:**
```bash
sudo airodump-ng wlan0mon
# Note the BSSID and channel of your target network
```

**Step 4 — Scan for clients on target network:**
```bash
sudo airodump-ng --bssid <TARGET_BSSID> -c <CHANNEL> wlan0mon
# Note the client MAC address
```

**Step 5 — Run the deauth script:**
```bash
sudo python3 deauth.py
```

**Step 6 — Stop the attack:**
```bash
Ctrl+C
```

**Step 7 — Restore managed mode after testing:**
```bash
sudo airmon-ng stop wlan0mon
sudo service NetworkManager restart
```

---

## 🔐 Ethical and Legal Notice

```
┌──────────────────────────────────────────────────────────────┐
│                    ⚠️  LEGAL WARNING                          │
│                                                               │
│  Deauthentication attacks on unauthorized networks may        │
│  violate:                                                     │
│                                                               │
│  🇮🇳 India    — IT Act 2000, Section 66               │
│  🇺🇸 USA      — Computer Fraud and Abuse Act (CFAA)   │
│  🇬🇧 UK       — Computer Misuse Act 1990              │
│  🌍 Most      — Similar national cybercrime laws      │
│     countries                                                 │
│                                                               │
│  ✅  Authorized use only:                                     │
│      • Your own home or lab network                           │
│      • Written permission from network owner                  │
│      • Controlled academic / research environment             │
│                                                               │
│  The author provides this solely for education.               │
│  Misuse is entirely the responsibility of the user.           │
└──────────────────────────────────────────────────────────────┘
```

---

## 🛡️ Defence Against This Attack

Understanding the attack also helps understand the defence:

| Defence | Description |
|---------|-------------|
| **802.11w (PMF)** | Protected Management Frames — cryptographically signs deauth frames |
| **WPA3** | Includes PMF by default — immune to this attack |
| **IDS/IPS** | Tools like Kismet detect deauth flood patterns |
| **Client isolation** | Reduces blast radius of successful attacks |

---

## 🔮 Future Enhancements

- [ ] Interactive network selection menu
- [ ] Auto-detect monitor mode capable interface
- [ ] Packet count and timing control via CLI arguments
- [ ] Detection mode — identify deauth attacks on local network
- [ ] Support for targeted vs broadcast deauth toggle

---

## 🤝 Contributing

Contributions for educational improvements are welcome.

```bash
# Fork the repository
git fork https://github.com/PriyanshuKhambalkar/Wifi-Deauthentication.git

# Create a feature branch
git checkout -b feature/your-improvement

# Commit and push
git commit -m "Add: your improvement"
git push origin feature/your-improvement
```

---

## 📚 Learning Resources

- [IEEE 802.11 Standard — Management Frames](https://standards.ieee.org/ieee/802.11)
- [802.11w Protected Management Frames](https://www.wi-fi.org/discover-wi-fi/wi-fi-certified-protected-management-frames)
- [Aircrack-ng Documentation](https://www.aircrack-ng.org/documentation.html)
- [Scapy 802.11 Docs](https://scapy.readthedocs.io/en/latest/layers/wireless.html)

---

## 📜 License

This project is licensed under the [MIT License](https://github.com/PriyanshuKhambalkar/Wifi-Deauthentication/blob/5d998aff775c9bb5f4e8c9e21dedd050b6221344/LICENSE) - see the LICENSE file for details.<br/> 
For any other use, please get in touch with the author.

---

## 👤 Author

**Priyanshu Khambalkar**


---

<div align="center">

*Built with Python · Scapy · aircrack-ng · For Authorized Testing Only*

⭐ **Star this repo if it helped your wireless security learning**

</div>
