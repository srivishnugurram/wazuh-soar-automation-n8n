# Architecture — Wazuh + n8n SOAR FIM Pipeline

## Data Flow
┌─────────────────────────────────┐
│      Ubuntu Agent (VM)          │
│  /root monitored by Wazuh FIM   │
│  realtime=yes, check_all=yes    │
│  File change → syscheck event   │
└──────────────┬──────────────────┘
│ agent → manager (1514/tcp)
▼
┌─────────────────────────────────┐
│      Wazuh Manager (VM)         │
│  Rule 100201 fires              │
│  Calls custom-n8n script        │
│  Script POSTs JSON to hook_url  │
└──────────────┬───────│ HTTP POST → WSL_IP:5678
▼
┌─────────────────────────────────┐
│   n8n (Docker inside WSL2)      │
│  Node 1: Webhook                │
│  Node 2: Edit Fields            │
│  Node 3: VirusTotal hash lookup │
│  Node 4: ntfy push notification │
└──────────────┬──────────────────┘
│ HTTPS POST
▼
┌─────────────────────────────────┐
│           ntfy.sh               │
│  Push notification to phone     │
└─────────────────────────────────┘
## Network Notes

- WSL2 uses NAT — its IP is only reachable from the Windows host
- WSL IP changes on Windows reboot — always re-check with `hostname -I`
- n8n production webhook is live once workflow is Published (Active toggle)
- VirusTotal free tier: 4 requests/minute, 500/day

## MITRE ATT&CK Coverage

| Technique | ID | Detected By |
|-----------|----|-------------|
| Data Manipulation | T1565 | FIM rule 100201 |
| File and Directory Discovery | T1083 | FIM rule 100201 |
