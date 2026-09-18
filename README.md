<div align="center">

```
 █████╗ ██╗  ██████╗ █████╗ ██╗  ██╗ █████╗ ███████╗██████╗
██╔══██╗██║  ██╔══██╗██╔══██╗██║ ██╔╝██╔══██╗██╔════╝██╔══██╗
███████║██║  ██║  ██║███████║█████╔╝ ███████║█████╗  ██████╔╝
██╔══██║██║  ██║  ██║██╔══██║██╔═██╗ ██╔══██║██╔══╝  ██╔══██╗
██║  ██║██║  ██████╔╝██║  ██║██║  ██╗██║  ██║███████╗██║  ██║
╚═╝  ╚═╝╚═╝  ╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝

 ██████╗ ███████╗██╗███╗   ██╗████████╗██╗  ██╗
██╔═══██╗██╔════╝██║████╗  ██║╚══██╔══╝██║  ██║
██║   ██║███████╗██║██╔██╗ ██║   ██║   ███████║
██║   ██║╚════██║██║██║╚██╗██║   ██║   ██╔══██║
╚██████╔╝███████║██║██║ ╚████║   ██║   ██║  ██║
 ╚═════╝ ╚══════╝╚═╝╚═╝  ╚═══╝   ╚═╝   ╚═╝  ╚═╝
```

**Open Source Intelligence & Threat Hunter Framework**

[![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.136-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.57-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](https://streamlit.io)
[![License](https://img.shields.io/badge/License-MIT-34c759?style=for-the-badge)](LICENSE)
[![CI](https://img.shields.io/github/actions/workflow/status/ernestolopez/osinth/ci.yml?style=for-the-badge&label=CI)](../../actions)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)](Dockerfile)

*Professional-grade OSINT & threat intelligence platform for SOC teams, security researchers, and analysts.*

[Features](#-features) · [Install](#-installation) · [CLI](#-cli-usage) · [API](#-api) · [Plugins](#-plugins) · [Roadmap](#-roadmap)

</div>

---

## 🛡️ What is OSINTH?

OSINTH is a **modular, event-driven cyber threat intelligence platform** that aggregates OSINT from multiple sources, correlates indicators of compromise, and automates the investigative workflow — all from a single framework.

Built for:
- 🏢 **SOC teams** needing automated threat enrichment pipelines
- 🔍 **OSINT analysts** investigating IPs, domains, emails, hashes
- 🧪 **Security researchers** correlating threat actors and campaigns
- 🤖 **Automation engineers** building detection and response workflows

---

## ✨ Features

### 🔍 OSINT Intelligence Sources

| Source | What it gives you |
|--------|-------------------|
| **VirusTotal** | Hash/URL/IP/domain reputation, scan results, detection ratio |
| **Shodan** | Open ports, banners, CVEs, exposed services, geolocation |
| **AbuseIPDB** | IP abuse reports, confidence score, ISP, usage type |
| **AlienVault OTX** | Threat pulses, malware families, attack techniques |
| **MalwareBazaar** | Malware samples, hashes, YARA signatures |
| **URLhaus** | Malicious URLs, phishing, drive-by downloads |
| **Whois / DNS** | Domain registration, NS records, MX, subdomains |
| **Geolocation** | IP → country, city, ASN, org, coordinates |

### 🤖 AI-Powered Analysis
- **Ollama LLM** integration for natural language threat reports
- **Phishing classifier** — brand spoofing, urgency detection, URL scoring
- **IOC classifier** — auto-detects type (IP/domain/hash/email/URL) + risk score
- **Threat classifier** — 17 rule categories + AI enrichment (ransomware, RAT, C2…)
- **Anomaly detector** — EMA-based statistical baseline per metric
- **Correlation engine** — clusters related events into campaigns

### 📡 Live Detection Pipeline

```
Raw Event → DetectionRules → CorrelationEngine → AnomalyDetector
     │                                                   │
     ▼                                                   ▼
 EventBus ──► AlertManager ──► Telegram / Discord    IOCMonitor
     │
     ▼
 ForensicCollector ──► SHA-256 evidence chain
```

### 🚨 Automated Response
- Telegram & Discord alerts with deduplication and rate limiting
- AutoResponse — pluggable actions triggered on threat detection
- Continuous Monitor — workers + scheduler + watchdog in one orchestrator
- WebSocket — live event streaming to dashboard / external consumers

---

## 📐 Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         OSINTH v2.0                             │
├──────────────┬──────────────────┬──────────────┬───────────────┤
│  Dashboard   │    REST API      │    CLI       │  Scheduler    │
│ (Streamlit)  │  (FastAPI+WS)    │  (Typer)     │ (APScheduler) │
├──────────────┴──────────────────┴──────────────┴───────────────┤
│                       Core Engine                               │
│  Scanner · AIEngine · RiskEngine · SearchEngine(FTS5)          │
│  Security · RBAC · AuditTrail · Metrics(Prometheus)            │
├──────────────────────────┬──────────────────────────────────────┤
│     Automation Layer     │         OSINT Modules               │
│  EventBus · WorkerPool   │  ThreatIntel · Domain · IP          │
│  LiveDetector            │  Email · Geo · Network              │
│  CorrelationEngine       │  Social · Telegram · Phishing       │
│  AnomalyDetector         ├──────────────────────────────────────┤
│  IOCMonitor              │         AI / ML                     │
│  ForensicCollector       │  ThreatClassifier · PhishingClassifier│
│  AlertManager            │  IOCClassifier · CorrelationAI      │
├──────────────────────────┴──────────────────────────────────────┤
│                        Data Layer                               │
│  SQLite (SQLAlchemy) · SQLite FTS5 · ThreatGraph · Cache       │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Installation

### Requirements
- Python 3.12+
- [Ollama](https://ollama.ai) (optional — for AI features)

### Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/ernestolopez/osinth.git
cd osinth

# 2. Create virtual environment
python -m venv .venv
.venv\Scripts\activate          # Windows
source .venv/bin/activate       # Linux / macOS

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure environment
cp .env.example .env
# Edit .env with your API keys  ← or use Settings tab in dashboard
# (All keys are OPTIONAL and have free tiers — links are in .env.example)

# 5. Verify the installation
python check_install.py
# Prints a clear report: Python, dependencies, API keys, DB, Ollama, folders.
# 0 fallas = ready to run. Warnings are optional features only.

# 6. Launch dashboard
streamlit run ui/dashboard.py
# → http://localhost:8501
```

> 💡 **Tip:** Some advanced features (Maigret, YARA, Office-macro analysis) use
> optional dependencies already listed in `requirements.txt`. If any is missing,
> the app degrades gracefully and `check_install.py` tells you exactly what to
> install — nothing ever crashes.

### Docker

```bash
docker build -t osinth:latest .
docker run -p 8501:8501 -p 8000:8000 --env-file .env osinth:latest
# Dashboard  → http://localhost:8501
# API        → http://localhost:8000
# API Docs   → http://localhost:8000/docs
```

---

## 💻 CLI Usage

```bash
# Scan targets
python cli.py scan target 185.220.101.1
python cli.py scan target evil-domain.com --verbose --output result.json
python cli.py scan batch targets.txt --workers 8

# IOC management
python cli.py ioc add 185.220.101.1 --type ip --tags tor,c2
python cli.py ioc classify d41d8cd98f00b204e9800998ecf8427e

# Reports
python cli.py report generate --format pdf --output report.pdf --days 7

# System
python cli.py health
python cli.py version
python cli.py api start --port 8000 --reload
python cli.py dashboard --port 8501
```

### Example scan output

```
┌──────────────────┬────────────────────────────────┐
│ Field            │ Value                          │
├──────────────────┼────────────────────────────────┤
│ target           │ 185.220.101.1                  │
│ risk_level       │ CRITICAL                       │
│ risk_score       │ 87.5 / 100                     │
│ threat_category  │ malware_c2                     │
│ country          │ DE · Tor Project               │
│ abuse_score      │ 100 / 100                      │
│ vt_detections    │ 47 / 94 engines                │
│ shodan_ports     │ 9001, 9030, 443                │
│ mitre_ttps       │ T1071, T1090, T1041            │
└──────────────────┴────────────────────────────────┘
```

---

## 🌐 API

**Base URL:** `http://localhost:8000` · **Docs:** `/docs` · **Metrics:** `/metrics`

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/v1/scan` | Async scan (queued background job) |
| `POST` | `/api/v1/scan/sync` | Sync scan — waits for result |
| `GET`  | `/api/v1/threats` | List threats with filters |
| `GET`  | `/api/v1/threatintel/{target}` | Full multi-source ThreatIntel |
| `POST` | `/api/v1/ioc/watch` | Add IOC to live monitor |
| `POST` | `/api/v1/ioc/classify` | Classify & score an IOC |
| `GET`  | `/api/v1/ioc/list` | List all watched IOCs |
| `POST` | `/api/v1/alert` | Send manual alert |
| `POST` | `/api/v1/ingest` | Ingest CSV / JSON / IOC feed |
| `GET`  | `/api/v1/monitor/health` | System health & stats |
| `WS`   | `/ws/events` | Live event stream (filterable) |
| `WS`   | `/ws/monitor` | Live health stream (5s interval) |

```bash
# Quick examples
curl -X POST http://localhost:8000/api/v1/scan/sync \
  -H "Content-Type: application/json" \
  -d '{"target": "evil.com"}'

# WebSocket live feed
wscat -c "ws://localhost:8000/ws/events?min_severity=high&types=THREAT_DETECTED"
```

---

## 🔧 Configuration

```bash
# Threat Intel APIs (get free keys at each provider)
VT_API=...             # virustotal.com
SHODAN_API=...         # shodan.io
ABUSEIPDB_KEY=...      # abuseipdb.com
ALIENVAULT_KEY=...     # otx.alienvault.com

# Alerts
TELEGRAM_BOT_TOKEN=... # t.me/BotFather
TELEGRAM_CHAT_ID=...
DISCORD_WEBHOOK_URL=...

# AI
OLLAMA_HOST=http://localhost:11434
OLLAMA_MODEL=llama3

ENV=production          # development | staging | production
```

> API keys can also be configured from the **⚙️ Settings** tab in the dashboard — no file editing needed.

---

## 🧩 Plugins

```python
# plugins/my_plugin.py
from plugins.base import BasePlugin, PluginResult

class MyPlugin(BasePlugin):
    name        = "my_plugin"
    version     = "1.0.0"
    description = "Custom enrichment plugin"

    def run(self, target: str, context: dict) -> PluginResult:
        return PluginResult(
            plugin=self.name,
            data={"field": "value"},
            risk_contribution=10.0,
        )
```

Built-in plugins: `vt_plugin` · `shodan_plugin` · `telegram_plugin` · `discord_plugin` · `phishing_plugin` · `twitter_plugin`

---

## 📁 Project Structure

```
osinth/
├── ai/                  # Classifiers + LLM prompts
├── api/                 # FastAPI REST + WebSocket + Auth
├── automation/          # EventBus, Workers, Alerts, Forensics
├── core/                # Scanner, AI, Risk, Security, RBAC, Audit
├── modules/             # OSINT modules (ThreatIntel, Domain, IP…)
├── plugins/             # Hot-loadable enrichment plugins
├── ui/                  # Streamlit dashboard + theme + pages
├── database/            # SQLAlchemy models + DB manager
├── tests/               # Unit + integration + API tests
├── cli.py               # Typer CLI entry point
├── config.py            # Pydantic-settings typed config
├── Dockerfile
├── pyproject.toml       # Build system + Ruff + Black + Pytest
└── .github/workflows/   # CI (lint → test → build → docker)
```

---

## 🗺️ Roadmap

| Version | Target | Features |
|---------|--------|----------|
| **v2.1** | Q3 2026 | Elasticsearch indexing · MITRE ATT&CK mapping · STIX/TAXII export · Attack graph D3.js |
| **v2.2** | Q4 2026 | Multi-tenant dashboard · Plugin marketplace · ML threat prediction · PCAP deep analysis |
| **v3.0** | 2027 | Distributed workers (Redis+Celery) · GraphQL API · Kubernetes · Enterprise SSO |

---

## 🤝 Contributing

```bash
git checkout -b feature/my-feature
pytest tests/ -v
ruff check . && black --check .
# Submit pull request
```

---

## 👤 Author

**Ernesto Lopez** — Cybersecurity Researcher · OSINT Analyst · Threat Hunter · Full-Stack Developer

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

<div align="center">

**⭐ Star this repo if OSINTH is useful to you ⭐**

*Built for the security community*

</div>
