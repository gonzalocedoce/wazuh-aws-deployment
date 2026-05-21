# 🛡️ Wazuh All-in-One Deployment on AWS

> Single-node Wazuh XDR/SIEM platform deployed on Amazon Linux 2023. Configured for threat detection, vulnerability scanning, file integrity monitoring, and compliance. Currently monitoring Windows agents — ready to scale with additional Linux workloads.

![Wazuh](https://img.shields.io/badge/Wazuh-4.x-0080FF?logoColor=white)
![AWS](https://img.shields.io/badge/AWS-Amazon%20Linux%202023-FF9900?logo=amazonaws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

---

## 📐 Architecture

<img width="371" height="372" alt="Wazuh All-in-One Architecture" src="https://github.com/user-attachments/assets/e16cf0e6-8bf3-4ea8-84ed-c462a1bd2257" />

*Single-node deployment: Wazuh Manager + Indexer + Dashboard on one EC2 instance, with Windows agents reporting remotely.*

- Single-node All-in-One deployment (Manager + Indexer + Dashboard)
- Amazon Linux 2023
- Wazuh version 4.x

---

## ☁️ Infrastructure

| Component | Details |
|---|---|
| **Instance Type** | t3.large / c5a.xlarge |
| **AMI** | Amazon Linux 2023 |
| **Minimum RAM** | 8 GB |
| **Storage** | 50 GB+ recommended |
| **Key Pair** | `<your-key-pair>.pem` |

---

## 🔒 Security Groups

| Port | Protocol | Source | Purpose |
|------|----------|--------|---------|
| 22 | TCP | My IP | SSH access |
| 443 | TCP | My IP | Wazuh Dashboard |
| 1514 | TCP/UDP | Agent IPs | Agent communication |
| 1515 | TCP | Agent IPs | Agent enrollment |
| 55000 | TCP | My IP | Wazuh API |

---

## 🚀 Deployment

### 1. Launch EC2 instance

Launch an Amazon Linux 2023 instance with the specs above. Apply the security group rules before proceeding.

### 2. Install Wazuh All-in-One

SSH into the instance and run:

```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

> Save the credentials printed at the end of installation — you'll need them to log into the Dashboard.

### 3. Verify services

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

---

## 🖥️ Agent Deployment

### Windows Agent

Run as Administrator in PowerShell (replace with your instance's public IP):

```powershell
msiexec.exe /i wazuh-agent.msi /q `
  WAZUH_MANAGER="<PUBLIC-IP>" `
  WAZUH_AGENT_NAME="<AGENT-NAME>"

NET START WazuhSvc
```

### Linux Agent

```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-agent-install.sh
sudo WAZUH_MANAGER='<PUBLIC-IP>' bash ./wazuh-agent-install.sh

sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

## 🔍 Quick Access

| Resource | URL |
|---|---|
| Dashboard | `https://<PUBLIC-IP>` |
| API | `https://<PUBLIC-IP>:55000` |
| Username | `admin` |
| Password | *Generated during install — save it* |

---

## ✅ Verification

Confirm agents are connected from the manager:

```bash
sudo /var/ossec/bin/agent_control -l
```

Check live alerts:

```bash
sudo tail -f /var/ossec/logs/alerts/alerts.json
```

---

## 🛠️ Troubleshooting

| Issue | Solution |
|---|---|
| Agent shows disconnected | Verify ports 1514/1515 are open in security group |
| Dashboard unreachable | Check port 443 is open and `wazuh-dashboard` service is running |
| Agent enrollment fails | Confirm `WAZUH_MANAGER` IP is correct and reachable |
| Manager won't start | Check logs at `/var/ossec/logs/ossec.log` |
| Forgot dashboard password | Re-run the Wazuh passwords tool: `sudo /usr/share/wazuh-indexer/plugins/opensearch-security/tools/wazuh-passwords-tool.sh` |

---

## 📚 References

- [Wazuh Documentation](https://documentation.wazuh.com)
- [Wazuh Installation Guide](https://documentation.wazuh.com/current/installation-guide/index.html)
- [Wazuh Agent Enrollment](https://documentation.wazuh.com/current/user-manual/agent-enrollment/index.html)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)

---

## 📝 License

This project is for educational and portfolio purposes only.
