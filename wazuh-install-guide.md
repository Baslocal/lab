# Wazuh Installation Guide

## Server Installation

The server script performs an all-in-one Wazuh server installation. It automatically detects your server's IP address and sets up all required components.

```bash
#!/bin/bash

# Create log file with timestamp
LOG_FILE="/var/log/wazuh-install-$(date +%Y%m%d_%H%M%S).log"

# Get server IP
SERVER_IP=$(hostname -I | awk '{print $1}')

echo "Starting Wazuh installation - $(date)" | tee -a $LOG_FILE

# Download Wazuh installation script
echo "Downloading Wazuh installation script" | tee -a $LOG_FILE
curl -sO https://packages.wazuh.com/4.5/wazuh-install.sh | tee -a $LOG_FILE
chmod +x wazuh-install.sh

# Create config file
cat > /root/config.yml << EOF
nodes:
  # Wazuh manager node
  manager:
    node_name: wazuh-1
    node_type: server
    node_ip: $SERVER_IP
    installation_type: "all-in-one"
EOF

echo "Created config.yml with IP: $SERVER_IP" | tee -a $LOG_FILE

# Run Wazuh installation commands
bash wazuh-install.sh -g -i | tee -a $LOG_FILE
bash wazuh-install.sh -a -i | tee -a $LOG_FILE

echo "Installation completed - $(date)" | tee -a $LOG_FILE
```

## Agent Installation

Once your server is running, you can install agents on your devices. Simply run the appropriate command for your operating system and provide your Wazuh server's IP address when prompted.

### For Linux Systems
```bash
read -p "Enter the Wazuh Manager IP address: " WAZUH_MANAGER

curl -so wazuh-agent.deb https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.8.1-1_amd64.deb && \
sudo apt-get install -y lsb-release && \
sudo WAZUH_MANAGER="$WAZUH_MANAGER" WAZUH_AGENT_NAME="$(hostname)" dpkg -i ./wazuh-agent.deb && \
echo '<ossec_config><client><server><address>'$WAZUH_MANAGER'</address></server></client></ossec_config>' | sudo tee /var/ossec/etc/ossec.conf > /dev/null && \
sudo systemctl daemon-reload && \
sudo systemctl enable wazuh-agent && \
sudo systemctl start wazuh-agent
```

### For Regular Windows
```powershell
$WAZUH_MANAGER = Read-Host -Prompt "Enter the Wazuh Manager IP address"

Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.3-1.msi -OutFile ${env:tmp}\wazuh-agent.msi
msiexec.exe /i ${env:tmp}\wazuh-agent.msi /q WAZUH_MANAGER=$WAZUH_MANAGER WAZUH_AGENT_NAME='Windows' WAZUH_REGISTRATION_SERVER=$WAZUH_MANAGER
echo '<?xml version="1.0" encoding="utf-8"?><ossec_config><client><server><address>' + $WAZUH_MANAGER + '</address></server></client></ossec_config>' > "C:\Program Files (x86)\ossec-agent\ossec.conf"
NET START Wazuh
```

### For Windows Server
```powershell
$WAZUH_MANAGER = Read-Host -Prompt "Enter the Wazuh Manager IP address"

msiexec.exe /i wazuh-agent-4.5.4-1.msi /q WAZUH_MANAGER=$WAZUH_MANAGER WAZUH_REGISTRATION_SERVER=$WAZUH_MANAGER WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='Windowsserver'
echo '<?xml version="1.0" encoding="utf-8"?><ossec_config><client><server><address>' + $WAZUH_MANAGER + '</address></server></client></ossec_config>' > "C:\Program Files (x86)\ossec-agent\ossec.conf"
NET START Wazuh
```

### For Mac with Intel Processor
```bash
read -p "Enter the Wazuh Manager IP address: " WAZUH_MANAGER

curl -so wazuh-agent.pkg https://packages.wazuh.com/4.x/macos/wazuh-agent-4.5.4-1.intel64.pkg && \
sudo echo "WAZUH_MANAGER='$WAZUH_MANAGER' && WAZUH_AGENT_GROUP='default' && WAZUH_AGENT_NAME='$(hostname)'" > /tmp/wazuh_envs && \
sudo installer -pkg ./wazuh-agent.pkg -target / && \
echo '<ossec_config><client><server><address>'$WAZUH_MANAGER'</address></server></client></ossec_config>' | sudo tee /Library/Ossec/etc/ossec.conf && \
sudo /Library/Ossec/bin/wazuh-control start
```

### For Mac with M1/M2 Chip
```bash
read -p "Enter the Wazuh Manager IP address: " WAZUH_MANAGER

curl -so wazuh-agent.pkg https://packages.wazuh.com/4.x/macos/wazuh-agent-4.5.4-1.arm64.pkg && \
sudo echo "WAZUH_MANAGER='$WAZUH_MANAGER' && WAZUH_AGENT_GROUP='default' && WAZUH_AGENT_NAME='$(hostname)'" > /tmp/wazuh_envs && \
sudo installer -pkg ./wazuh-agent.pkg -target / && \
echo '<ossec_config><client><server><address>'$WAZUH_MANAGER'</address></server></client></ossec_config>' | sudo tee /Library/Ossec/etc/ossec.conf && \
sudo /Library/Ossec/bin/wazuh-control start
```
