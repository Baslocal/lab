# Wazuh Agent Installation Guide

This guide provides instructions for installing Wazuh agents across different operating systems. The scripts automate the installation process and require only the Wazuh Manager IP address as input.

## Prerequisites

- Administrative privileges on the target system
- Network connectivity to the Wazuh Manager
- The IP address of your Wazuh Manager

## Linux Installation (Ubuntu/Debian)
### Version 4.8.1

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

## Windows Installation

### Regular Windows (Version 4.7.3)

```powershell
$WAZUH_MANAGER = Read-Host -Prompt "Enter the Wazuh Manager IP address"

Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.3-1.msi -OutFile ${env:tmp}\wazuh-agent.msi
msiexec.exe /i ${env:tmp}\wazuh-agent.msi /q WAZUH_MANAGER=$WAZUH_MANAGER WAZUH_AGENT_NAME='Windows' WAZUH_REGISTRATION_SERVER=$WAZUH_MANAGER
echo '<?xml version="1.0" encoding="utf-8"?><ossec_config><client><server><address>' + $WAZUH_MANAGER + '</address></server></client></ossec_config>' > "C:\Program Files (x86)\ossec-agent\ossec.conf"
NET START Wazuh
```

### Windows Server (Version 4.5.4)

```powershell
$WAZUH_MANAGER = Read-Host -Prompt "Enter the Wazuh Manager IP address"

msiexec.exe /i wazuh-agent-4.5.4-1.msi /q WAZUH_MANAGER=$WAZUH_MANAGER WAZUH_REGISTRATION_SERVER=$WAZUH_MANAGER WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='Windowsserver'
echo '<?xml version="1.0" encoding="utf-8"?><ossec_config><client><server><address>' + $WAZUH_MANAGER + '</address></server></client></ossec_config>' > "C:\Program Files (x86)\ossec-agent\ossec.conf"
NET START Wazuh
```

## macOS Installation

### Intel-based Mac (Version 4.5.4)

```bash
read -p "Enter the Wazuh Manager IP address: " WAZUH_MANAGER

curl -so wazuh-agent.pkg https://packages.wazuh.com/4.x/macos/wazuh-agent-4.5.4-1.intel64.pkg && \
sudo echo "WAZUH_MANAGER='$WAZUH_MANAGER' && WAZUH_AGENT_GROUP='default' && WAZUH_AGENT_NAME='$(hostname)'" > /tmp/wazuh_envs && \
sudo installer -pkg ./wazuh-agent.pkg -target / && \
echo '<ossec_config><client><server><address>'$WAZUH_MANAGER'</address></server></client></ossec_config>' | sudo tee /Library/Ossec/etc/ossec.conf && \
sudo /Library/Ossec/bin/wazuh-control start
```

### Apple Silicon (M1/M2) Mac (Version 4.5.4)

```bash
read -p "Enter the Wazuh Manager IP address: " WAZUH_MANAGER

curl -so wazuh-agent.pkg https://packages.wazuh.com/4.x/macos/wazuh-agent-4.5.4-1.arm64.pkg && \
sudo echo "WAZUH_MANAGER='$WAZUH_MANAGER' && WAZUH_AGENT_GROUP='default' && WAZUH_AGENT_NAME='$(hostname)'" > /tmp/wazuh_envs && \
sudo installer -pkg ./wazuh-agent.pkg -target / && \
echo '<ossec_config><client><server><address>'$WAZUH_MANAGER'</address></server></client></ossec_config>' | sudo tee /Library/Ossec/etc/ossec.conf && \
sudo /Library/Ossec/bin/wazuh-control start
```

## Post-Installation Verification

After installing the agent, verify the installation by checking:

1. Agent service status
2. Connection to the Wazuh Manager
3. Agent registration status in the Wazuh Manager


