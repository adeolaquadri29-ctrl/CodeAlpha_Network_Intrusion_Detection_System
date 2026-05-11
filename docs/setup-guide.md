🛠️ Setup Guide – Network Intrusion Detection System

This guide explains how to set up the complete NIDS lab environment.

⸻

🖥️ Environment Setup

Machines Used:

* Ubuntu VM (Defender Machine)
* Kali Linux VM (Attacker Machine)

Both machines are configured in Bridged Network Mode to allow communication.

⸻

🔐 Step 1: Install and Configure Snort

Install Snort on Ubuntu:
Sudo apt update 
Sudo apt install snort -y 

Configure Network:

Edit Snort configuration:
sudo nano /etc/snort/snort.conf

Set: 
ipvar HOME_NET 192.168.0.0/24

📡 Step 2: Run Snort in IDS Mode
sudo snort -A console -q -c /etc/snort/snort.conf -i enp0s3

📦 Step 3: Install Filebeat (Log Forwarding)
sudo apt install filebeat -y 

Edit configuration 
sudo nano /etc/filebeat/filebeat.yml
Add:
filebeat.inputs:

  - type: log

    enabled: true

    paths:

      - /var/log/snort/alert

output.elasticsearch:

  hosts: ["http://localhost:9200"]

Start Filebeat:
sudo systemctl start filebeat
sudo systemctl enable filebeat

🐳 Step 4: Install Docker
sudo apt install docker.io -y

sudo systemctl start docker

📊 Step 5: Run Elasticsearch (Log Storage)
docker run -d --name elasticsearch \

  -p 9200:9200 \

  -e "discovery.type=single-node" \

  -e "xpack.security.enabled=false" \

  docker.elastic.co/elasticsearch/elasticsearch:8.12.0

📊 Step 6: Run Kibana (Visualization)
docker run -d --name kibana \

  -p 5601:5601 \

  --link elasticsearch \

  -e "NODE_OPTIONS=--max-old-space-size=256" \

  docker.elastic.co/kibana/kibana:8.12.0

Access Kibana:
http://localhost:5601

⚙️ Step 7: Automated Response (Auto-block)

Create script:
 #!/bin/bash

LOG="/var/log/snort/snort.alert.fast"

tail -Fn0 $LOG | while read line

do

    IP=$(echo "$line" | awk '{for(i=1;i<=NF;i++) if ($i ~ /^[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+$/) print $i}' | head -1)

    # Skip invalid IPs

    if [[ -z "$IP" || "$IP" == "0.0.0.0" ]]; then

        continue

    fi

    # Avoid duplicate blocking

    if iptables -L INPUT -n | grep -q "$IP"; then

        continue

    fi

    echo "[!] Detected attack from $IP"

    iptables -A INPUT -s $IP -j DROP

done

Run script: 
bash autoblock.sh

✅ Verification

* Snort detects attacks
* Filebeat forwards logs
* Elasticsearch stores logs
* Kibana displays logs

This confirms the system is working successfully.
