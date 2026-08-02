# Multi-Channel AI Receptionist

An AI receptionist built in n8n that handles customer messages across
WhatsApp and Telegram simultaneously — responds intelligently, captures
leads, books appointments, and escalates when needed. All without human
involvement.

---

## The Problem

Small businesses lose leads every day because no one is available to
respond outside working hours. Customers message on WhatsApp or Telegram
and get silence. This system ensures every message gets an instant,
intelligent response — 24/7.

---

## What It Does

- Listens for incoming messages on **WhatsApp (Twilio)** and **Telegram**
  at the same time
- Merges both channels into a single AI Agent so logic is written once
- AI Agent decides what to do based on the message:
  - Searches knowledge base and replies with accurate information
  - Captures lead details into Google Sheets
  - Books appointments into Google Sheets
  - Escalates to a human when the query is beyond its scope
- **Channel Router** sends the reply back to the correct platform
  (WhatsApp or Telegram)

---

## Architecture
Telegram Trigger
       │
       ▼
Extract Message Data1 ──┐
                        ▼
                      Merge ──▶ Receptionist Agent ──▶ Channel Router
                        ▲              │                      │
Extract Message Data ───┘              │                 True ──▶ Send WhatsApp Reply
       ▲                               │                False ──▶ Send Telegram Message
       │                    ┌──────────┴──────────┐
Twilio Trigger         OpenRouter            Simple Memory
                       Chat Model
                                    Tools:
                                    - escalate_to_human
                                    - search_knowledge_base
                                    - capture_lead
                                    - book_appointment


## Agent Tools

| Tool | What It Does |
|---|---|
| `search_knowledge_base` | Fetches answers from Google Docs knowledge base |
| `capture_lead` | Appends lead details to Google Sheets |
| `book_appointment` | Logs appointment request to Google Sheets |
| `escalate_to_human` | Flags complex queries for human follow-up |

---

## Stack

| Layer | Tool |
|---|---|
| Automation | n8n (self-hosted) |
| AI/LLM | OpenRouter Chat Model |
| Memory | Simple Memory (in-context) |
| WhatsApp | Twilio |
| Messaging | Telegram |
| Knowledge Base | Google Docs |
| Data Logging | Google Sheets |

---

## Setup Instructions

1. Import `workflow.json` into your n8n instance
2. Add credentials:
   - Twilio Account SID + Auth Token
   - Telegram Bot Token
   - OpenRouter API Key
   - Google Sheets OAuth
   - Google Docs OAuth
3. Update your Google Docs knowledge base with your business FAQs
4. Set your Google Sheets ID in the capture_lead and book_appointment
   tool nodes
5. Activate the workflow
6. Send a test message to your Twilio WhatsApp number and your
   Telegram bot

---

## Files

| File | Description |
|---|---|
| `workflow.json` | Importable n8n workflow — all credentials removed |

---

Built by [Habeebullah Sulyman]—
AI Automation Specialist
