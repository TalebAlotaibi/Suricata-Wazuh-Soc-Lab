# 🛡️ SOC Lab | Suricata IDS + Wazuh SIEM for Real-Time Threat Detection & Monitoring

![SOC Lab](https://img.shields.io/badge/SOC-Lab-blue)
![Suricata](https://img.shields.io/badge/IDS-Suricata-red)
![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-green)

---
## 🎯 Project Objective
To simulate a real-world SOC environment for detecting, analyzing, and responding to network-based attacks using IDS and SIEM technologies.

---
## 🏗️ Architecture Overview

- Adversary Simulation: Kali Linux (Attack Emulation)
- Monitoring: Suricata IDS
- SIEM: Wazuh (Security Monitoring, Log Analysis & Alerting)
- Log Source: Suricata EVE JSON logs
- Host: Ubuntu Server

## 🌐 Environment
This lab is built and tested in a virtualized environment using VirtualBox/VMware.

## 🔧 Technologies Used

- 🛡️ Suricata 6.0.8 – Network IDS/IPS engine  
- 📊 Wazuh 4.7 – SIEM & log analysis platform  
- 🐧 Ubuntu – Host operating system  
- 💀 Kali Linux – Attack simulation environment  
- ⚙️ Custom scripts – Automation and rule setup

---

## 🔄 Detection Flow

1. Attacker generates malicious traffic
2. Suricata detects anomalies via rules
3. Logs are written to eve.json
4. Wazuh agent collects logs
5. Alerts appear in Wazuh dashboard

## 🧪 Security Use Cases

- SSH brute-force detection  
- Port scanning detection  
- Suspicious traffic monitoring

---

## ⚙️ Setup Instructions

### 1. 📦 Install Dependencies

```bash
sudo bash dependancies.sh
```

This script installs all required packages before compiling Suricata.

---

### 2. 🐍 Download & Compile Suricata 6.0.8

```bash
wget https://www.openinfosecfoundation.org/download/suricata-6.0.8.tar.gz
tar xzvf suricata-6.0.8.tar.gz
cd suricata-6.0.8

./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
--enable-nfqueue --enable-geoip --enable-lua --enable-hiredis

make
sudo make install
sudo make install-conf
sudo make install-rules
```

---

### 3. 👤 Create Suricata User & Directories

```bash
sudo useradd -r -s /sbin/nologin suricata
sudo mkdir -p /var/log/suricata
sudo chown -R suricata:suricata /var/log/suricata/
```

Make sure to assign proper file permissions to directories you create or edit.

---

### 4. ⚖️ Download Suricata Rules

```bash
sudo bash rules.sh
```

This script automates the rule download process.

---

### 5. 🧩 Configure Suricata Service

Create a systemd service file:

```bash
sudo nano /etc/systemd/system/suricata.service
```

Paste the following:

```ini
[Unit]
Description=Suricata IDS/IPS/NSM Engine
After=network.target

[Service]
User=suricata
Group=suricata
ExecStart=/usr/bin/suricata -c /etc/suricata/suricata.yaml -i eth0
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

> 🔁 Replace `eth0` with your actual network interface (e.g., `enp0s3`)

---

### 6. 📄 Configuration File

Copy `suricata.yaml` to:

```bash
/etc/suricata/suricata.yaml
```

Update the following:

- `HOME_NET` to match your subnet (e.g., `192.168.1.0/24`)
- Interface field (`eth0` or whatever yours is)

---

### 7. 🚀 Start Suricata

```bash
sudo systemctl daemon-reload
sudo systemctl enable suricata
sudo systemctl start suricata
```

You can check status with:

```bash
sudo systemctl status suricata
```

---

## 🔗 Wazuh Agent Integration

To connect Suricata logs with the Wazuh agent:

1. Copy your `wazuh-agent-configs` into:

```bash
/var/ossec/etc/ossec.conf
```

2. Restart the agent:

```bash
sudo systemctl restart wazuh-agent
```

> ⚠️ Add decoding rules and log paths if needed for Suricata log parsing (e.g., `/var/log/suricata/eve.json`)

---

## ✅ What's Next

- Detect brute force attacks using Suricata rules  
- Map alerts to MITRE ATT&CK techniques for threat classification 
- Create Wazuh dashboards for incident visualization  
- Integrate TheHive for incident response workflow  

---
## 📌 Skills Demonstrated

- Network traffic analysis  
- IDS/IPS rule tuning and configuration 
- SIEM log correlation  
- Threat detection engineering

---

## 📄 Documentation

- Full implementation report:  
  [Download PDF](./Wazuh_Suricata_Implementation_Summary.pdf)

  ---
  
## 🧠 Author

**Taleb Alotaibi**  
Cybersecurity Analyst

📫 [LinkedIn](https://www.linkedin.com/in/talebalotaibi/)

---

## 💬 Want feedback or want to collaborate?

Feel free to open an issue or connect with me!










