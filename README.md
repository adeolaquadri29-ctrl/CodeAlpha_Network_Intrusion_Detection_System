# CodeAlpha_Network_Intrusion_Detection_System
This project was developed as part of my Cybersecurity Internship with CodeAlpha. It demonstrates the implementation of a Network Intrusion Detection System (NIDS) with real-time attack monitoring and visualization.

🚨 CodeAlpha Network Intrusion Detection System (NIDS)

📌 Project Overview

This project was developed as the final task during my Cybersecurity Internship with CodeAlpha.

It demonstrates a complete Network Intrusion Detection and Response System capable of detecting, monitoring, and responding to suspicious network activity in real time, while visualizing threats using a centralized dashboard.

⸻

🧠 Objectives

* Set up a Network Intrusion Detection System (NIDS)
* Configure rules and alerts to detect malicious activity
* Continuously monitor network traffic
* Implement automated response mechanisms
* Visualize detected attacks using dashboards

⸻

🛠️ Tools & Technologies

* Snort (Intrusion Detection Engine)
* Elasticsearch (Log Storage & Search Engine)
* Kibana (Visualization Dashboard)
* Filebeat (Log Forwarding)
* Docker (Containerization)
* Linux (Kali/Ubuntu VM Environment)

⸻

🧱 System Architecture

Snort → Filebeat → Elasticsearch → Kibana

This architecture simulates a Security Operations Center (SOC) pipeline:

* Snort → detects threats
* Auto-block script → blocks attacker IPs
* Filebeat → forwards logs
* Elasticsearch → stores data
* Kibana → visualizes attacks

⸻

🔍 Detection & Monitoring

Snort Configuration

* Promiscuous mode enabled (bridged network)
* HOME_NET configured as: 192.168.0.0/24
* Custom and default rules applied

Example Detection Rules
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping Detected"; sid:1000001; rev:1;)

alert tcp any any -> $HOME_NET 22 (msg:"SSH Connection Attempt"; sid:1000002; rev:1;)

alert tcp any any -> $HOME_NET any (flags:S; msg:"Possible Nmap SYN Scan"; sid:1000003; rev:1;)

alert tcp any any -> $HOME_NET 80 (msg:"HTTP Traffic Detected"; sid:1000004; rev:1;)

Continuous Monitoring

Logs are generated and monitored in real time: tail -f /var/log/snort/alert

⚙️ Automated Response Mechanism

An automated response system was implemented to block malicious IP addresses in real time.

How it works:

* Extract attacker IP from Snort alerts
* Apply firewall rule using iptables
* Block further communication

⚠️ This feature was disabled after testing to allow VM communication for other lab activities.

⸻

📡 Log Forwarding (Filebeat)

Snort logs are forwarded to Elasticsearch using Filebeat.

Configuration: filebeat.inputs:

  - type: log

    enabled: true

    paths:

      - /var/log/snort/alert

output.elasticsearch:

  hosts: ["http://localhost:9200"]

📊 Visualization (Kibana)

Kibana is used to analyze and visualize detected threats.

Features:

* Real-time attack monitoring
* Source IP tracking
* Attack timelines
* Frequency analysis

Data View Configuration:

* Index Pattern: filebeat-*
* Timestamp Field: @timestamp

⸻

🧪 Attack Simulation

The system was tested using real attack scenarios:

🔹 Nmap SYN Scan
nmap -sS 192.168.0.122
🔹 ICMP Ping Flood 
ping 192.168.0.122
🔹 SSH Brute Force Attempt
hydra -L users.txt -P passwords.txt ssh://192.168.0.122

📈 Detection Results

* Attacker IP: 192.168.0.117
* Target IP: 192.168.0.122

Timeline:

* 15:23 → Nmap SYN Scan & SSH attempt detected
* 15:26 → ICMP Ping detected

Outcome:

* Snort successfully detected all attacks
* Logs were forwarded via Filebeat
* Data was indexed in Elasticsearch
* Attacks were visualized in Kibana dashboards

⸻

📸 Screenshots

📂 25 screenshots available in the /screenshots directory

⸻

🎥 Project Demonstration

A full explanation video is available on LinkedIn:

👉 https://www.linkedin.com/in/champion-adeola-quadri-a3a182399

⸻

📚 Learning Outcomes

* Built a complete IDS + SIEM pipeline
* Gained hands-on experience with SOC tools
* Performed real attack detection and analysis
* Implemented automated incident response
* Developed log analysis and visualization skills

⸻

🏁 Conclusion

This project demonstrates how a real-world intrusion detection system operates by combining:

* Detection (Snort)
* Response (iptables automation)
* Monitoring (Filebeat + Elasticsearch)
* Visualization (Kibana)

It reflects a practical implementation of a SOC-based threat detection workflow.
