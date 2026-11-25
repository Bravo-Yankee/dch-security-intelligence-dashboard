# DCH Security Intelligence Platform

Live operational dashboards for **Zone D, Dubai Creek Harbour** — built for WeOne Security to give supervisors a real-time view of security incidents and Holiday Home (short-term guest) activity across all 14 residential communities, without needing to dig through WhatsApp threads or raw spreadsheets.

🔗 **Live:** https://dash.freshplug.org (sign-in required). The pages in this repo are now redirects to that site; the dashboards themselves are served from the DCH server, not from GitHub Pages.
🏢 **Coverage:** 14 communities · 6,566 apartments · 101 villas · 527 serviced apartments · 8,008 CCTV cameras · 44 ANPR cameras

---

## What's in this repo

| File | Purpose |
|---|---|
| `index.html` | **Landing page** — entry point linking to the dashboards below |
| `security-ops.html` | **Security Operations Live Feed** — incident feed, zone heatmaps, shift handover reports, alerting, on-demand analysis report |
| `holiday-home.html` | **Holiday Homes Tracker (HHO)** — guest check-in/check-out monitoring, community occupancy, compliance screening |
| `resident-database.html` | **Resident Database** — move-in/move-out log and resident record lookup |
| `sira-compliance.html` | **SIRA Compliance Tracker** |
| `duty-reference.html` | **Vertical Duty Reference** — condensed vertical and Master Community rules |
| `manifest-*.json`, `icons/` | PWA manifests and icons |

The pages are self-contained, dependency-light HTML files — no build step, no framework. They run client-side and refresh their data automatically.

---

## How it works

```
WhatsApp report  →  n8n workflow (self-hosted)  →  AI classification  →  Google Sheets  →  Dashboard (GitHub Pages)
```

1. **Intake** — Community supervisors send free-text reports/guest check-ins via WhatsApp (Evolution API).
2. **Automation** — A self-hosted n8n workflow parses each message and routes it to the correct pipeline (security incident vs. HHO guest event).
3. **AI classification** — Each message is structured into JSON (incident type, severity, zone, guest details, compliance flags, etc.) using a multi-model failover cascade. This protects against any single model's daily token quota silently dropping reports.
4. **Storage** — Structured records are logged to Google Sheets, which also doubles as the audit trail.
5. **Dashboard** — The dashboard pages fetch the sheet as CSV on a polling interval and render it live — KPIs, activity feeds, community breakdowns, and auto-generated shift handover reports (07:00 / 19:00 Dubai time).
6. **Alerting** — Critical incidents and compliance-flagged guest check-ins (e.g. nationality watch-list matches) trigger automatic WhatsApp alerts to supervisors.

---

## Features

**Security Command Center (`index.html`)**
- Live incident feed with severity/zone tagging
- Auto-generated shift handover report, copy-to-clipboard
- Per-zone incident breakdown
- Critical incident WhatsApp alerting

**Holiday Homes Tracker (`holiday-home.html`)**
- Community Activity Map with photo cards per community
- Occupancy by Community (live tally, sortable)
- Latest Activity Stream (newest-first guest check-ins/check-outs)
- Check-In / Check-Out logs with search, community filter, and sort by date/month/year
- Compliance screening against nationality/passport data
- Active Guests and HHO Operators views

---

## Tech stack

- **Frontend:** Vanilla HTML/CSS/JS, no build tooling — runs directly on GitHub Pages
- **Automation:** [n8n](https://n8n.io) (self-hosted)
- **AI classification:** multi-provider LLM failover
- **Messaging:** Evolution API (WhatsApp)
- **Data store:** Google Sheets (CSV export endpoint)
- **Animation:** [lottie-web](https://github.com/airbnb/lottie-web) for footer micro-interactions

---

## Security notes

This repository contains only the static front end. The automation workflow, its diagram, its credentials and any API keys are managed separately and are deliberately **not** published here. Do not commit keys, passwords, workflow exports or any personal data to this repository.

---

## License

Internal tool — WeOne Security / Dubai Creek Harbour, Zone D.
