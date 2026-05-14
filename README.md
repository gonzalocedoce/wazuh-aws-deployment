Single-node Wazuh All-in-One deployment using AWS AMI Linux 2023  with tailored security groups.. Configured as a centralized XDR and SIEM platform for threat detection, vulnerability scanning, file monitoring, and compliance. Currently monitoring Windows agents, ready to scale with additional Linux workloads. Documented for easy replication.
# Wazuh Deployment on AWS

Wazuh XDR/SIEM deployment on Amazon Linux 2023.

## Architecture

<img width="371" height="372" alt="image" src="https://github.com/user-attachments/assets/e16cf0e6-8bf3-4ea8-84ed-c462a1bd2257" />


- Single-node All-in-One deployment
- Amazon Linux 2023
- Wazuh version 4.14.5

## Quick Access
- Dashboard URL: `https://<PUBLIC-IP>`
- Username and password provided by the AMI. 

<img width="1910" height="932" alt="image" src="https://github.com/user-attachments/assets/7661c211-deff-4829-94fb-e660d27e748c" />

<img width="1902" height="585" alt="image" src="https://github.com/user-attachments/assets/82fbfc11-414e-4118-97fd-7f1eb79c7c6b" />
<img width="1902" height="585" alt="image" src="https://github.com/user-attachments/assets/e7fc4af4-3f0f-4ea8-8929-cb5e70d28d47" />


## Infrastructure

- **Instance Type**: t3.large / c5a.xlarge
- **AMI**: Wazuh All-in-One or Amazon Linux 2023
- **Key Pair**: `EC2 Key Pair`

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

Wazuh agent :<img width="331" height="284" alt="image" src="https://github.com/user-attachments/assets/4a246dfd-92dd-4d16-bdc3-ef8192f07d42" />
In case it get disconnected you can start the procesess from here and change the Manager IP if it changes.
