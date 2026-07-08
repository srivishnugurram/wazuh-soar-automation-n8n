# Wazuh + n8n SOAR Pipeline — FIM Alert Automation

An end-to-end SOAR pipeline that detects file integrity changes, enriches alerts with VirusTotal threat intelligence, and delivers real-time push notifications via ntfy.

---

## Architecture
Ubuntu Agent (FIM)
↓  rule 100201 triggered
Wazuh Manager
↓  custom-n8n integration script
n8n Webhook (WSL/Docker)
↓  Edit Fields node
VirusTotal API (hash lookup)
↓  HTTP Request node
ntfy.sh Push Notification
## Tech Stack

| Component | Role |
|-----------|------|
| Wazuh 4.x | SIEM — FIM monitoring + alert generation |
| n8n (Docker/WSL) | SOAR workflow orchestration |
| VirusTotal API | Threat intelligence enrichment |
| ntfy.sh | Push notification delivery |
| Ubuntu 22.04 | Wazuh agent (monitored endpoint) |

---

## Project Structure
wazuh-n8n-soar-fim/
├── README.md
├── scripts/
│   └── custom-n8n
├── configs/
│   ├── wazuh/
│   │   ├── ossec-integration.xml
│   │   └── local_rules.xml
│   └── n8n/
│       └── docker-compose.yml
└── docs/
└── architecture.md
