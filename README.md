# 🤖 AI Feedback Analysis System

> Collect → Classify → Respond → Report. Zero manual work.

![n8n](https://img.shields.io/badge/built%20with-n8n-EA4B71?style=flat-square)
![AI](https://img.shields.io/badge/AI-OpenRouter%20%7C%20Nemotron-6366f1?style=flat-square)
![Sheets](https://img.shields.io/badge/storage-Google%20Sheets-34A853?style=flat-square)
![Gmail](https://img.shields.io/badge/email-Gmail-EA4335?style=flat-square)
![Status](https://img.shields.io/badge/status-working-22c55e?style=flat-square)

---

## What it does

An end-to-end automation that handles customer feedback from submission to response — no human in the loop.

- **Collects** feedback via a hosted web form (Name, Email, Feedback)
- **Classifies** it into one of 4 categories using AI: `Complaint` · `Compliment` · `Query` · `Feature Request`
- **Scores sentiment** from `-1.0` (very frustrated) to `+1.0` (very happy)
- **Assigns priority** automatically: `High` / `Medium` / `Low`
- **Sends a personalised reply** email tailored to the category
- **Alerts on Slack** for High Priority Complaints (optional, can be enabled)
- **Every Monday at 9 AM**, generates and emails a full AI-written weekly summary report

---

## Architecture

```
Feedback Form (n8n trigger)
       │
       ▼
AI Classifier (OpenRouter · Nemotron-120B)
       │
       ▼
Parse & Build Smart Reply (JS · category + sentiment + priority + email)
       │
       ▼
Route by Category (Switch: Complaint / Compliment / Query / Feature Request)
       │
       ▼
Write to Google Sheets ──► Send Smart Reply Email (Gmail)
                      └───► Is High Priority Complaint? ──► Slack Alert (disabled by default)

── Weekly path ──────────────────────────────────────────────────────────
Every Monday 9AM ──► Read All Feedback ──► Aggregate Last 7 Days
                                                    │
                                                    ▼
                                         AI Weekly Insight (OpenRouter)
                                                    │
                                                    ▼
                                          Build Weekly Email ──► Send Report (Gmail)
```

---

## Tech stack

| Layer | Tool |
|---|---|
| Automation | [n8n](https://n8n.io) (self-hosted or cloud) |
| AI model | nvidia/nemotron-3-super-120b via [OpenRouter](https://openrouter.ai) |
| Storage | Google Sheets |
| Email | Gmail (OAuth2) |
| Alerts | Slack (optional) |

