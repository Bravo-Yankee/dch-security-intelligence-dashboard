# DCH Security Intelligence Platform

Live operational dashboards for **Zone D, Dubai Creek Harbour** — built for WeOne Security to give supervisors a real-time view of security incidents and Holiday Home (short-term guest) activity across all 14 residential communities, without needing to dig through WhatsApp threads or raw spreadsheets.

🔗 **Live:** hosted as a static PWA via GitHub Pages
🏢 **Coverage:** 14 communities · 6,566 apartments · 101 villas · 527 serviced apartments · 8,008 CCTV cameras · 44 ANPR cameras

---

## What's in this repo

| File | Purpose |
|---|---|
| `index.html` | **Security Command Center** — live incident feed, shift handover reports, community zone breakdown, critical/compliance alerting |
| `holiday-home.html` | **Holiday Homes Tracker (HHO)** — guest check-in/check-out monitoring, community occupancy, compliance screening |
| `logo.png` | Platform logo |

Both are self-contained, dependency-light HTML files — no build step, no framework. They run entirely client-side and pull live data from a Google Sheets CSV export, refreshed automatically.

---

## How it works

```
WhatsApp report  →  n8n workflow (Railway)  →  AI classification  →  Google Sheets  →  Dashboard (GitHub Pages)
```

1. **Intake** — Community supervisors send free-text reports/guest check-ins via WhatsApp (Evolution API).
2. **Automation** — An n8n workflow (hosted on Railway, PostgreSQL + Redis) parses each message and routes it to the correct pipeline (security incident vs. HHO guest event).
3. **AI classification** — Each message is structured into JSON (incident type, severity, zone, guest details, compliance flags, etc.) using a 4-model failover cascade:
   `Groq llama-3.1-8b-instant → Groq llama-3.3-70b-versatile → Groq gemma2-9b-it → OpenRouter (auto)`
   This protects against any single model's daily token quota silently dropping reports.
4. **Storage** — Structured records are logged to Google Sheets, which also doubles as the audit trail.
5. **Dashboard** — Both HTML files fetch the sheet as CSV on a polling interval and render it live — KPIs, activity feeds, community breakdowns, and auto-generated shift handover reports (07:00 / 19:00 Dubai time).
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
- **Automation:** [n8n](https://n8n.io) (self-hosted on Railway)
- **AI classification:** Groq (Llama 3.1/3.3, Gemma 2) with OpenRouter fallback
- **Messaging:** Evolution API (WhatsApp)
- **Data store:** Google Sheets (CSV export endpoint)
- **Animation:** [lottie-web](https://github.com/airbnb/lottie-web) for footer micro-interactions

---

## Security notes

This public repository contains the **sanitized** version of the dashboard — no live API keys, credentials, or the internal n8n workflow-editing tab are exposed here. The automation workflow and its credentials are managed separately and are not part of this static site.

---

## License

Internal tool — WeOne Security / Dubai Creek Harbour, Zone D.
