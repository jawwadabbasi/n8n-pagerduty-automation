# n8n-pagerduty-automation

This repository contains n8n workflows that integrate with PagerDuty to automate on-call operations, scheduling visibility, and future incident‑resolution workflows using automation and AI.

## Project Overview

This project currently includes a workflow that:

- Retrieves on-call schedules from multiple PagerDuty teams.
- Normalizes those schedules into weekday-day, weekday-night, and weekend categories.
- Sends the summarized schedule to a designated communication channel such as Telegram.
- Runs automatically on a weekly cadence using n8n's Schedule Trigger.

This workflow supports multiple teams, including:
- DevOps team
- VI team
- Team Lead rotation
- Any additional PagerDuty schedule IDs you add

All PagerDuty calls are performed via the official oncalls API.

## How It Works

### 1. Time Window Calculation
A Code node calculates the `since` and `until` parameters based on the current time in America/New_York.  
The logic fetches on-call schedules from:
- Friday 5:00 PM (start of after‑hours period)
- Until next Friday 7:59:59 AM (end of after‑hours period)

### 2. PagerDuty API Calls
Three HTTP Request nodes pull schedule data using the PagerDuty API:

- DevOps team schedule
- VI team schedule
- Team Lead schedule

All nodes request:
- Expanded user information
- Contact methods
- On-call rotation details

### 3. Schedule Classification
A JavaScript Code node analyzes each on-call user across the entire time range and classifies availability into:
- weekday_day (8 AM–5 PM)
- weekday_night (5 PM–8 AM)
- weekend (Saturday and Sunday)

### 4. Contact Method Lookup
The Team Lead schedule output contains a link to the contact method API.  
The workflow automatically extracts the phone number using that link.

### 5. Merge and Notification
All computed data is merged into a single payload and sent via Telegram messaging.

## Upcoming Additions

This repository will expand to include advanced automation around PagerDuty:

### 1. Auto-resolving incidents with AI Agents
- Ingest incident details
- Use an LLM to classify issue type and likely root cause
- Trigger scripts, health checks, or attempted remediations
- Automatically resolve or escalate incidents based on confidence scoring

### 2. Automated triage of incoming incidents
- Parse incident summaries and context
- Apply AI reasoning to generate actionable runbooks
- Notify responsible teams with recommended steps

### 3. PagerDuty outbound automation
- Create incidents from n8n
- Modify schedules
- Update incident notes automatically

### 4. Multi-environment integrations
- Integrate with internal systems like DNS, Kubernetes, GitLab, or monitoring platforms
- Check health of dependencies before triggering escalations

## Requirements

- n8n running locally or in your orchestration environment
- PagerDuty API token
- PagerDuty schedule IDs
- Telegram bot and chat ID (optional for notifications)

## Setup

1. Import the workflow JSON file into n8n.
2. Add your PagerDuty API token to all HTTP Request nodes.
3. Add your PagerDuty schedule IDs.
4. Add Telegram credentials if using messaging.
5. Enable the workflow.

## Repository Goals

The goal of this repository is to build production-ready PagerDuty automation patterns using n8n, including:

- Schedule visibility automation
- Automated incident remediation
- AI-augmented operations
- Cross-system orchestration
- Self-healing pipelines

More workflow templates will be added over time.

---

## License

This project is licensed under the **MIT License** — free for personal and commercial use.

---

## Author
**Jawwad Ahmed Abbasi**  
Senior Software Developer  
[GitHub](https://github.com/jawwadabbasi) | [YouTube](https://www.youtube.com/@jawwad_abbasi)