# WhatsApp Business Automation System — Full Breakdown
### For Small & Medium Local Businesses (Barbers, Salons, Bakeries, Vendors, Service Providers)

---

## A. BUSINESS CONTEXT

**Business Type:** Small and medium-sized local businesses using WhatsApp as their primary customer channel.

**Target verticals:** Barbers, hair salons, nail studios, bakeries, food vendors, home service providers, clothing vendors, tailors.

**Core Problem:** Business owners receive high volumes of WhatsApp messages daily with no system to respond instantly, capture lead data, manage bookings, follow up, or store records. Everything is done manually from one phone — causing missed leads, double bookings, and lost revenue.

**Target Users:**
- Primary: Business owner (mobile-first, non-technical)
- Secondary: Customers (any WhatsApp user)
- Tertiary: Staff needing booking visibility

---

## B. SYSTEM OVERVIEW

```
Customer sends WhatsApp message
        ↓
WhatsApp Business API receives message
        ↓
Make.com webhook triggers automation
        ↓
Claude AI classifies intent + generates reply
        ↓
Reply sent back to customer (< 3 seconds)
        ↓
Lead data extracted → saved to Airtable CRM
        ↓
Booking confirmed → Google Calendar event created
        ↓
Reminder messages sent 24h + 2h before appointment
        ↓
Post-service thank-you + rebook prompt sent automatically
        ↓
Owner receives daily WhatsApp summary each morning
```

**Data Flow:**
```
[Customer WhatsApp]
        │
        ▼
[WhatsApp Cloud API] ←→ [Meta Business Account]
        │
        ▼
[Make.com Webhook] — receives message payload
        │
        ├──→ [Claude AI] → intent classified → reply generated → sent back
        │
        ├──→ [Airtable] → lead captured / updated
        │
        ├──→ [Google Calendar] → booking created
        │
        └──→ [Scheduler] → reminders + follow-ups queued
```

---

## C. TECH STACK

| Layer | Tool | Why |
|-------|------|-----|
| Messaging | WhatsApp Cloud API (Meta) | Free tier, official API, scales to millions |
| Automation | Make.com | Visual, no-code, handles branching logic, mobile-friendly |
| AI Responses | Claude API (Haiku model) | Fast, cheap (~$0.002/conversation), context-aware |
| Database | Airtable | No-code CRM, mobile-accessible, duplicatable |
| Calendar | Google Calendar API | Free, universal, easy owner sharing |
| Notifications | WhatsApp Cloud API (outbound) | Same channel, no extra tool |
| Templates | GitHub | Version control for reusable workflow files |

---

## D. STEP-BY-STEP WORKFLOW

### STEP 1 — Receive & Parse Message
**Trigger:** Customer sends WhatsApp message → webhook fires in Make.com
**Action:** Extract `customer_phone`, `customer_name`, `message_body`, `timestamp` → classify intent (booking / order / enquiry / unknown)
**Result:** Message routed to the correct flow

---

### STEP 2 — AI Reply Generation
**Trigger:** Classified message sent to Claude API with business context
**Action:** Claude generates a natural, on-brand reply using this system prompt:

```
You are a friendly assistant for {BUSINESS_NAME}, a {BUSINESS_TYPE} in {LOCATION}.
Services: {SERVICES_LIST}
Hours: {HOURS}
Pricing: {PRICING_SUMMARY}

Your job:
1. Greet the customer warmly
2. Answer their question accurately
3. Guide them toward booking or ordering
4. Collect: name, service, preferred date and time

Keep replies under 3 sentences. Never make up services or prices.
If the customer says "human", "urgent", or "call me" — immediately say:
"I'm connecting you with {OWNER_NAME} now. They'll be with you shortly."
```

**Result:** Personalised reply ready in < 1 second

---

### STEP 3 — Send Reply
**Trigger:** AI response ready
**Action:** WhatsApp Cloud API sends reply to customer
**Result:** Customer receives instant response (typically < 3 seconds total)

---

### STEP 4 — Lead Capture
**Trigger:** Name, service, or time detected in conversation
**Action:** Extract structured data → check Airtable for existing phone number → create or update record

**Airtable fields captured:**
- Phone Number (unique ID), Customer Name, Service Requested
- Preferred Date, Preferred Time, Status, Source, Created At, Notes

**Result:** Lead saved automatically, zero manual effort from owner

---

### STEP 5 — Booking Confirmation
**Trigger:** Customer confirms date and time
**Action:**
1. Check Google Calendar for conflicts at requested slot
2. If available → create calendar event → update Airtable status to `Confirmed` → send confirmation message
3. If unavailable → AI offers next 3 open slots → customer selects → repeat

**Result:** Booking locked in, calendar updated, customer confirmed

---

### STEP 6 — Automated Reminders
**Trigger:** Scheduled time-based trigger in Make.com

**24h reminder:**
```
Hi {NAME}, reminder: your {SERVICE} is tomorrow at {TIME} at {BUSINESS_NAME}.
Reply YES to confirm or NO to reschedule.
```

**2h reminder:**
```
See you soon, {NAME}! Your {SERVICE} at {BUSINESS_NAME} is in 2 hours ({TIME}).
```

- If customer replies NO → trigger reschedule flow
- If no reply → flag in Airtable for owner review

**Result:** No-shows reduced significantly

---

### STEP 7 — Post-Service Follow-up
**Trigger:** Appointment marked `Completed` (time-based or manual)

**4 hours after service:**
```
Hi {NAME}, thank you for visiting {BUSINESS_NAME}! Hope you loved your {SERVICE}.
Reply BOOK whenever you're ready to come back.
```

**7 days later:**
```
Hi {NAME}, it's been a week! Ready to book your next {SERVICE}?
Reply BOOK and we'll sort you out.
```

**Result:** Repeat bookings generated passively

---

### STEP 8 — Owner Daily Summary
**Trigger:** Every morning at 8am (Make.com scheduler)

**Message sent to owner's personal WhatsApp:**
```
Good morning! Here's your day:
📅 Bookings today: 4
🔔 New leads: 2
📌 Follow-ups due: 1
View dashboard: [Airtable link]
```

**Result:** Owner stays informed without logging into multiple tools

---

## E. FAILURE HANDLING

| Failure Point | Detection | Fallback | Recovery |
|---------------|-----------|----------|----------|
| WhatsApp API down | HTTP error in Make.com | Queue message → retry every 5 min (3 attempts) | Alert owner if undelivered after 15 min |
| Claude API timeout | Empty/error response | Send pre-written fallback: "Thanks for reaching out! We'll be with you shortly." | Flag in Airtable for manual follow-up |
| Double booking | Google Calendar conflict detected | Never confirm — offer next 3 available slots instead | Auto-message customer if duplicate found post-creation |
| Airtable write fails | HTTP 422/500 from Airtable | Store in Make.com data store | Retry after 10 min, alert owner after 3 failures |
| Unrecognised message | AI confidence low | "Let me connect you with {OWNER_NAME} directly." | Owner notified on personal WhatsApp with customer's message |
| Make.com scenario error | Built-in error handler fires | Route to owner notification module | Scenario pauses, messages queued, owner alerted |

---

## F. SCALING STRATEGY

### 1–100 Users (Single Business)
- Stack: WhatsApp Cloud API free tier + Make.com Core + Airtable Free
- Cost: ~$15/month
- Capacity: 1,000 conversations/month (free WhatsApp tier)
- Owner action: Set up once, review weekly

### 100–1,000 Users (Multi-Business SaaS)
- Each client gets: their own WhatsApp number, cloned Make.com scenario, duplicated Airtable base
- Add: Softr/Glide client portal, Make.com Team plan, shared Claude API key with per-client usage tracking
- Add: Automated onboarding — client fills intake form → system configures itself
- Cost: ~$80–150/month base + per-client margin

### 1,000–10,000+ Users (Platform Scale)
- Build lightweight backend: Node.js on Railway/Render
- Central webhook router (one endpoint, routes by phone number to correct client config)
- Client config in PostgreSQL (Supabase)
- Replace Make.com with Node.js + BullMQ job queue
- Move WhatsApp to a BSP (Business Solution Provider) for volume pricing
- Add Sentry (errors) + PostHog (analytics)
- Cost: ~$300–800/month infrastructure

---

## G. MONETISATION MODEL

### Pricing Tiers

| Tier | Setup Fee | Monthly | Includes |
|------|-----------|---------|----------|
| Starter | $49 | $29 | 1 number, 500 conversations/mo, basic flow, Airtable CRM |
| Growth | $99 | $59 | 1 number, 2,000 conversations/mo, AI replies, reminders, follow-ups, daily summary |
| Pro | $149 | $99 | 2 numbers, unlimited conversations, custom AI persona, weekly reports, priority support |

### Unit Economics (Growth Tier)
- Claude Haiku API (2,000 conversations): ~$6/month
- Make.com cost share: ~$5/month
- WhatsApp (over free tier): ~$2.50/month
- Airtable: $0/month
- **Total cost per client: ~$13.50/month**
- **Revenue: $59/month**
- **Gross margin: ~77%**

### Revenue Mechanics
- Setup fee covers your onboarding time
- Monthly retainer covers API costs + platform fees + support
- Upsells: extra numbers, custom AI training, SMS fallback, staff access

---

## H. AUTOMATION OPPORTUNITIES (Roadmap)

### Near-term (0–3 months)
- Automated client onboarding: intake form → system configures itself
- AI-generated weekly performance reports sent to owner
- Automated Google/Facebook review requests post-service

### Mid-term (3–6 months)
- Payment links via Paystack/Flutterwave/WhatsApp Pay
- Auto-detect customer language → reply in same language
- Loyalty tracking (Nth visit triggers automatic discount message)

### Long-term (6–12 months)
- Voice note transcription + AI response
- AI learns each business's most common questions over time
- Predictive scheduling (slow-day promo messages auto-sent)
- Staff scheduling integration (only show slots when staff is available)

---

## PRODUCTIZED SERVICE — 80/20 SPLIT

### What NEVER Changes Per Client (80% — Built Once)

| Component | Reuse Method |
|-----------|-------------|
| Make.com automation blueprint | Import → update variables |
| AI prompt structure + booking flow | Fill in business details only |
| WhatsApp message templates | Resubmit per account (same text) |
| Airtable base structure + views | Duplicate from master |
| Error handling + fallback logic | Identical across all clients |
| Reminder timing (24h + 2h) | Same for every business |
| Follow-up delays (4h + 7d) | Same for every business |
| Onboarding process | Same 8-step checklist every time |

---

### What CHANGES Per Client (20% — One Config File)

```
BUSINESS_NAME
BUSINESS_TYPE
LOCATION
SERVICES_LIST + PRICES
OPENING_HOURS
OWNER_NAME
OWNER_PHONE
WHATSAPP_BUSINESS_NUMBER
GOOGLE_CALENDAR_ID
AIRTABLE_BASE_ID
AI_PERSONA_TONE (friendly / professional / casual)
```

**That's it. Nothing else changes.**

---

## DEPLOYMENT CHECKLIST (Per Client — 80 Minutes)

```
[ ] Client fills intake form (name, services, hours, phone number)
[ ] Fill config file with client details (5 min)
[ ] Create Meta Business Account + WhatsApp API number (20 min)
[ ] Clone Airtable base from master template (5 min)
[ ] Import Make.com scenario blueprint + update variables (10 min)
[ ] Connect webhook to Meta Developer app (5 min)
[ ] Submit WhatsApp message templates to Meta (10 min)
[ ] Run end-to-end test (send message → confirm booking → check calendar) (15 min)
[ ] Send client their Airtable link + brief handover voice note (10 min)
```

---

## ONGOING MAINTENANCE PER CLIENT

| Task | Frequency | Time |
|------|-----------|------|
| Check Make.com for errors | Weekly | 5 min |
| Review AI reply quality (month 1 only) | Weekly | 10 min |
| Client check-in | Monthly | 10 min |
| Change requests (hours, services, etc.) | On demand | 15 min |
| **Total** | **Per month** | **~30 min** |

At 20 clients: ~10 hours/month
At 50 clients: hire a VA using the onboarding guide as their SOP

---

## SALES PROCESS

**Qualify (5-minute call):**
- Do you use WhatsApp for customer enquiries? → Yes
- Do you miss messages or feel overwhelmed? → Yes
- Do you manage bookings manually? → Yes

**Demo:** Let them message a live demo number. They experience the bot as a customer. They sell themselves.

**Close:** Present the tier table. Most local businesses → Growth ($99 setup + $59/month). Offer 14-day money-back guarantee.

**Onboard:** Send intake form → 90-minute setup session → live.

---

## WHY CLIENTS STAY (Retention Moats)

1. All customer data lives in your Airtable setup (switching cost)
2. AI prompt is tuned to their specific business over time
3. Passive bookings start coming in within days (results)
4. $29–$99/month is trivial vs. the revenue it generates
5. They never want to go back to manual WhatsApp management

---

## KEY CONSTRAINTS TO SET WITH CLIENTS

- One dedicated phone number required (not a personal WhatsApp number)
- WhatsApp templates need 24–48h Meta approval before first outbound send
- System handles ~80% of conversations — some will always need the owner
- Human handoff is always available (customers can trigger it anytime)
