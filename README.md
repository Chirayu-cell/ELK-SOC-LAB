# 🖥️ Windows SOC Monitoring & Threat Hunting Lab (ELK Stack)

A full Windows-focused SOC monitoring and detection engineering project built using:

- **Elasticsearch**
- **Kibana**
- **Elastic Agent**
- **Sysmon**
- **Elastic Endpoint Security**

## 🔥 Key Features
- Integrated ingestion of Windows Event Logs, Sysmon logs, PowerShell logs, and Endpoint Security alerts.
- Dashboards tracking:
  - Failed logons  
  - Suspicious PowerShell  
  - Sysmon process activity  
  - Malware alerts  
- Custom detection rules & MITRE ATT&CK mapping.

## 🧪 Techniques Simulated
- T1110 — Brute Force (Event ID 4625 spikes)
- T1059 — PowerShell execution / encoded commands
- T1059.003 — Windows Command Shell
- T1204 — User Execution (EICAR test)
- T1105 — Ingress Tool Transfer

## 📊 Dashboard (screenshot in `/screenshots`)
Contains:
- Failed logons
- PowerShell command usage
- Sysmon process trends
- Malware alerts
- MITRE ATT&CK coverage table

## 📁 Repo Structure
```
windows-elk-soc-lab/
├── README.md
├── screenshots/
│   └── overview.jpg
├── configs/
│   ├── sysmon-config.xml
│   ├── fleet-policy.md
│   └── detection-rules.ndjson
└── docs/
    ├── methodology.md
    ├── attack-scenarios.md
    └── queries.spl
```

## 📚 What I Learned
- Building full Windows telemetry pipelines  
- Using Kibana Lens, Timelines, and Detection Rules  
- Threat hunting with Sysmon process chains  
- IR workflow: detect → triage → analyse → contain → remediate  

© 2025 Chirayu Paliwal
