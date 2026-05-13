# wazuh-aws-deployment
Single-node Wazuh All-in-One deployment using AWS AMI Linux 2023  with tailored security groups.. Configured as a centralized XDR and SIEM platform for threat detection, vulnerability scanning, file monitoring, and compliance. Currently monitoring Windows agents, ready to scale with additional Linux workloads. Documented for easy replication.
# Wazuh Deployment on AWS

Wazuh XDR/SIEM deployment on Amazon Linux 2023.

## Architecture
┌─────────────────────────────────────────┐
│           Cloud Instance (VM)           │
│                                         │
│  ┌──────────────────────────────────┐   │
│  │        Wazuh Manager             │   │
│  │  - API: port 55000               │   │
│  │  - Agent communication: 1514     │   │
│  │  - Syslog: 514                   │   │
│  └──────────────────────────────────┘   │
└─────────────────────────────────────────┘
         ▲                    ▲
         │                    │
┌────────────────┐   ┌─────────────────────┐
│  Linux Agent   │   │   Windows Agent     │
│  (Ubuntu/CentOS│   │   (Windows 10/Srv)  │
└────────────────┘   └─────────────────────┘
- Single-node All-in-One deployment
- Amazon Linux 2023
- Wazuh version 4.14.5

## Quick Access
- Dashboard URL: `https://<PUBLIC-IP>`
- Username: `admin`

## Infrastructure

- **Instance Type**: t3.large / c5a.xlarge
- **AMI**: Wazuh All-in-One or Amazon Linux 2023
- **Key Pair**: `wazuh-server-key.pem`

## Security Groups

| Port | Protocol | Source          | Purpose              |
|------|----------|-----------------|----------------------|
| 22   | TCP      | My-IP           | SSH                  |
| 443  | TCP      | My-IP           | Dashboard            |
| 1514 | TCP      | Agents          | Agent registration   |
| 1515 | TCP      | Agents          | Agent communication  |

## Agent Deployment Examples

### Windows Agent
```powershell
msiexec.exe /i wazuh-agent.msi /q WAZUH_MANAGER="54.XXX.XXX.XXX" WAZUH_AGENT_NAME="Gonzalotest"
