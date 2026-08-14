# 📬 Smart Inbox Gatekeeper & Action-Item Extractor

A production-ready multi-agent AI system built with the [Strands Agents SDK](https://strandsagents.com/) that monitors Gmail and Microsoft Outlook, filters inbox noise, extracts actionable tasks, and delivers a concise daily executive briefing.

## Architecture

```
┌─────────────┐     ┌─────────────┐
│   Gmail     │     │   Outlook   │
│   OAuth2    │     │  Graph API  │
└──────┬──────┘     └──────┬──────┘
       │                   │
       └─────────┬─────────┘
                 │
        ┌────────▼────────┐
        │   Orchestrator  │
        │   (Scheduler)   │
        └────────┬────────┘
                 │
    ┌────────────▼────────────┐
    │  Agent 1: Spam Filter   │
    │  (Classification)       │
    └────────────┬────────────┘
                 │ IMPORTANT only
    ┌────────────▼────────────┐
    │  Agent 2: Action-Item   │
    │  Extractor              │
    └────────────┬────────────┘
                 │
    ┌────────────▼────────────┐
    │     SQLite Storage      │
    └────────────┬────────────┘
                 │ Daily (cron)
    ┌────────────▼────────────┐
    │  Agent 3: Briefing      │
    │  Executive              │
    └────────────┬────────────┘
                 │
    ┌────────────▼────────────┐
    │  Delivery: Email/Slack  │
    │  /Teams                 │
    └─────────────────────────┘
```

## Features

- **3 Specialized AI Agents** working as a coordinated pipeline
- **Gmail + Outlook** integration with OAuth2
- **7-category email classification** with confidence scoring
- **Structured action-item extraction** (tasks, deadlines, priorities, meetings)
- **Daily executive briefing** delivered via email, Slack, or Teams
- **VIP contact handling** with automatic priority boost
- **Follow-up detection** for reminders and unanswered requests
- **Duplicate prevention** and 90-day data retention
- **Production observability** with structured logging and metrics
- **Retry mechanisms** with exponential backoff
- **Dockerized deployment** with health checks

## Quick Start

### 1. Prerequisites

- Python 3.11+
- AWS account (for Bedrock model access)
- Gmail OAuth2 credentials and/or Microsoft Azure app registration

### 2. Installation

```bash
pip install -e .
```

### 3. Configuration

```bash
cp .env.example .env
# Edit .env with your credentials
```

### 4. Run

```bash
# Continuous monitoring (polls every 5 minutes, briefing at configured time)
python main.py

# Process emails once and exit
python main.py --once

# Generate briefing immediately
python main.py --briefing

# Run data cleanup
python main.py --cleanup
```

### 5. Docker

```bash
docker-compose up -d
```

## Agent Details

### Agent 1: Spam & Noise Filter

Classifies every email into one of 7 categories:
- `IMPORTANT` - Requires user attention
- `NEWSLETTER` - Content digests, blog updates
- `PROMOTIONAL` - Marketing, sales, discounts
- `RECEIPT` - Order confirmations, payments
- `SOCIAL_NOTIFICATION` - Social media alerts
- `SYSTEM_ALERT` - Automated system notifications
- `SPAM` - Unsolicited bulk mail

### Agent 2: Action-Item Extractor

Extracts structured intelligence from important emails:
- Summary, core request, tasks, questions
- Deadlines with normalized dates
- Priority (HIGH/MEDIUM/LOW)
- Meeting requests and details
- Follow-up and response detection

### Agent 3: Briefing Executive

Generates a daily digest with:
- Attention summary (counts and most urgent item)
- Action items table (sender, request, next step, priority, deadline)
- Meeting requests section
- Approaching deadlines
- Follow-ups needed

## Project Structure

```
├── main.py                    # Application entry point
├── pyproject.toml             # Dependencies and project config
├── Dockerfile                 # Production container
├── docker-compose.yml         # Container orchestration
├── .env.example               # Configuration template
├── src/
│   ├── config.py              # Settings management
│   ├── models.py              # Pydantic data models
│   ├── orchestrator.py        # Pipeline coordination
│   ├── scheduler.py           # Job scheduling (APScheduler)
│   ├── logging_config.py      # Structured logging setup
│   ├── agents/
│   │   ├── spam_filter.py     # Agent 1: Classification
│   │   ├── action_extractor.py # Agent 2: Extraction
│   │   └── briefing_executive.py # Agent 3: Briefing
│   ├── database/
│   │   ├── schema.py          # SQLAlchemy ORM models
│   │   └── repository.py     # Data access layer
│   └── integrations/
│       ├── gmail_client.py    # Gmail API client
│       ├── outlook_client.py  # Microsoft Graph client
│       └── notifications.py   # Email/Slack/Teams delivery
└── tests/
    ├── test_models.py         # Model validation tests
    ├── test_repository.py     # Database layer tests
    └── test_orchestrator.py   # Pipeline integration tests
```

## Configuration Reference

| Variable | Description | Default |
|----------|-------------|---------|
| `LLM_API_KEY` | API key for the LLM endpoint | - |
| `LLM_BASE_URL` | OpenAI-compatible chat completions URL | - |
| `LLM_MODEL_ID` | Model identifier (sonnet, haiku, nova-pro, etc.) | `sonnet` |
| `GMAIL_CLIENT_ID` | Gmail OAuth2 client ID | - |
| `OUTLOOK_CLIENT_ID` | Microsoft app client ID | - |
| `BRIEFING_SCHEDULE_HOUR` | Hour to send daily briefing (24h) | `7` |
| `BRIEFING_TIMEZONE` | Timezone for scheduling | `America/New_York` |
| `VIP_CONTACTS` | Comma-separated VIP emails | - |
| `DATA_RETENTION_DAYS` | Days to keep email data | `90` |
| `SLACK_WEBHOOK_URL` | Slack incoming webhook | - |
| `TEAMS_WEBHOOK_URL` | Teams incoming webhook | - |

## Testing

```bash
pip install -e ".[dev]"
pytest -v
```


## Author

**NagaVenkatesh Arigala** — AI/GenAI Engineer
[LinkedIn](https://www.linkedin.com/in/nv-arigala0801/) · [GitHub](https://github.com/Venki0987)

---

## 📂 About this repository

This is a **documentation and architecture showcase**. It covers the problem, the system design, the agent topology, and the engineering decisions behind the project.

**The source code is held in a private repository.** I'm glad to walk through the implementation in a technical conversation or screen-share — reach out via the links below.

All rights reserved — see [LICENSE](LICENSE).