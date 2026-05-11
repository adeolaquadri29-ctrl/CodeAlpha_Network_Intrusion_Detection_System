🧪 Attack Simulation – NIDS Testing

This document describes how attacks were simulated and detected in the system.

⸻

🎯 Target Machine

* IP Address: 192.168.0.122 (Ubuntu - Defender)

⚔️ Attacker Machine

* Kali Linux

⸻

🔹 1. Nmap SYN Scan
nmap -sS 192.168.0.122

Result:

* Snort detected SYN scan activity
* Alert triggered: “Possible Nmap SYN Scan”

⸻

🔹 2. ICMP Ping Flood
ping 192.168.0.122

Result:

* Snort detected ICMP traffic
* Alert triggered: “ICMP Ping Detected”

⸻

🔹 3. SSH Brute Force Attempt
hydra -L users.txt -P passwords.txt ssh://192.168.0.122

Result:

* Snort detected SSH connection attempts
* Alert triggered: “SSH Connection Attempt”

⸻

📊 Observations

* Attacker IP: 192.168.0.117
* Target IP: 192.168.0.122

Timeline:

* 15:23 → Nmap Scan + SSH Attempt
* 15:26 → ICMP Ping

⸻

⚙️ Response

* Auto-block script detected attacker IP
* Firewall rule applied using iptables
* Attacker IP was blocked

⸻

📈 Outcome

* All attacks were successfully detected
* Logs were forwarded to Elasticsearch
* Data was visualized in Kibana

⸻

✅ Conclusion

The system effectively detects, logs, and responds to network-based attacks in real time.
