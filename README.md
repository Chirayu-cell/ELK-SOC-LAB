# Windows SOC Monitoring Lab — ELK Stack

Build notes and detection work from a Windows-focused SOC monitoring lab I ran
using Elasticsearch, Kibana, Elastic Agent, Sysmon, and Elastic Endpoint
Security.

**What this repo is:** the configuration, detection logic, and findings from a
homelab I built to learn Windows telemetry pipelines and detection engineering.
It is a record of work, not a deployable product — you can't `git clone` this
and get a running SOC. What you can do is read the detections and the tuning
notes.

---

## Lab topology

| Component | Role |
|---|---|
| Windows 10 VM | Monitored endpoint — Sysmon, Windows Security, PowerShell operational logs |
| Elastic Agent (Fleet-managed) | Log shipping and endpoint security |
| Elasticsearch + Kibana | Storage, search, dashboards, detection rules |
| Attacker host | Generates the simulated activity below |

![Lab overview](screenshots/overview.jpg)

---

## Techniques simulated

| ATT&CK ID | Technique | Signal used |
|---|---|---|
| T1110 | Brute Force | Event ID 4625 volume/velocity per source |
| T1059.001 | PowerShell | Encoded command execution, `-enc` / `-EncodedCommand` |
| T1059.003 | Windows Command Shell | `cmd.exe` spawned from unusual parents |
| T1204 | User Execution | EICAR test file execution |
| T1105 | Ingress Tool Transfer | Outbound transfer via LOLBins |

---

## What I actually learned

Two things worth writing down, both of which cost me time:

**4625 volume alone is a bad brute-force detection.** A threshold on failed
logons fires constantly on a machine where a service account has a stale
cached credential. What made it usable was correlating on *distinct target
accounts per source* rather than raw failure count — one account failing 50
times is a stuck credential, 50 accounts failing once each is spraying.

**Sysmon's default config is too noisy to hunt in.** Without filtering, normal
workstation activity buries anything interesting. Tuning the config down to the
event IDs that carry detection value (1, 3, 7, 8, 11, 13) is the difference
between a searchable index and an expensive one.

---

## Repo contents

```
├── configs/
│   ├── sysmon-config.xml       # Sysmon config used in this lab
│   ├── fleet-policy.md         # Elastic Agent integration set + notes
│   └── detection-rules.ndjson  # Kibana detection rules (exported)
├── docs/
│   ├── methodology.md          # How the lab was built, in order
│   ├── attack-scenarios.md     # Each simulation and expected telemetry
│   └── queries.spl             # Hunting queries
└── screenshots/
```

## Limitations

Single Windows endpoint, no domain controller, no network segmentation. The
detections here are tuned against one machine's baseline and would need
rework against real enterprise volume. Endpoint Security was run in detect-only
mode throughout.

© 2025 Chirayu Paliwal
