# WhatsApp Business Automation System

A reusable, productized automation system for small and medium-sized local businesses
(barbers, salons, bakeries, vendors, service providers) that use WhatsApp as their
primary customer channel.

## What This System Does

- Instant AI-powered replies to every customer message (< 3 seconds)
- Automated booking flow (collects name, service, date, time)
- Google Calendar integration (no double bookings)
- Airtable CRM (every lead captured automatically)
- Appointment reminders (24h + 2h before)
- Post-service follow-ups (thank-you + rebook prompt)
- Daily summary sent to owner via WhatsApp
- Human handoff when customers request it
- Full error handling and fallback at every step

## Quick Start

1. Fill in `config/business-config-template.json` with client details
2. Follow `docs/onboarding-guide.md` (80 minutes end-to-end)
3. Go live

## Repository Structure

```
├── SYSTEM_DESIGN.md              Full system architecture
├── PRODUCTIZED_SERVICE.md        Productization strategy + pricing
├── README.md                     This file
├── config/
│   └── business-config-template.json   Client variables (fill per client)
├── ai-prompts/
│   └── system-prompt-template.md       Claude system prompt + intent/slot prompts
├── make-com/
│   └── scenario-template.json          Make.com blueprint (importable)
├── airtable/
│   └── base-schema.json                Airtable base structure
├── whatsapp/
│   ├── message-templates.md            Pre-approved WhatsApp templates
│   └── webhook-setup.md                WhatsApp Cloud API setup guide
└── docs/
    ├── onboarding-guide.md             Step-by-step client setup (80 min)
    └── troubleshooting.md              Common issues + fixes
```

## Tech Stack

| Tool | Purpose |
|------|---------|
| WhatsApp Cloud API (Meta) | Customer messaging channel |
| Make.com | Automation orchestration (no-code) |
| Claude API (Haiku model) | AI response generation |
| Airtable | CRM + lead storage |
| Google Calendar | Booking management |

## Pricing Tiers

| Tier | Setup | Monthly | Target |
|------|-------|---------|--------|
| Starter | $49 | $29 | Single-service businesses |
| Growth | $99 | $59 | Multi-service, repeat customers |
| Pro | $149 | $99 | High-volume, brand-conscious |

## Key Files to Read First

1. `SYSTEM_DESIGN.md` — full architecture, workflow, and scaling strategy
2. `PRODUCTIZED_SERVICE.md` — what to standardise, what to customise, cost breakdown
3. `docs/onboarding-guide.md` — deploy a new client in 80 minutes
