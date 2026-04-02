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

## B. TWO VERSIONS — START WITH THE MVP

This system has two versions. Start with the MVP. Upgrade only when the client is seeing consistent value.

| | MVP | Full System |
|-|-----|-------------|
| **Setup time** | Under 2 hours | 80–90 minutes (after MVP is running) |
| **Monthly cost** | $0 | ~$14–35/month |
| **Tools** | 3 | 5 |
| **AI replies** | No — fixed menu | Yes — Claude AI |
| **Calendar sync** | No — Sheet only | Yes — Google Calendar |
| **Reminders** | No | Yes — 24h + 2h |
| **Follow-ups** | No | Yes — 4h + 7 days |
| **Owner summary** | No | Yes — daily WhatsApp |
| **Sell at** | $49 setup, $0/month | $49–149 setup, $29–99/month |

---

## C. MVP — SYSTEM OVERVIEW

**The 3 tools:**

| Tool | Purpose | Cost |
|------|---------|------|
| WhatsApp Cloud API (Meta) | Receive + send messages | Free |
| Make.com | Run the automation | Free (up to 1,000 ops/month) |
| Google Sheets | Store leads + bookings | Free |

**Total monthly cost: $0**

**MVP Flow:**
```
Customer messages the business WhatsApp
        ↓
Bot replies instantly with a greeting + numbered menu
        ↓
Customer picks: 1 (Book) or 2 (Ask a Question)
        ↓
Bot collects: Name → Service → Date & Time
        ↓
Booking saved to Google Sheets
        ↓
Customer receives confirmation message
        ↓
Owner sees the booking in their Sheet
```

---

## D. MVP — STEP-BY-STEP WORKFLOW

### STEP 1 — Customer writes anything
**Bot replies:**
```
Hi! Welcome to {BUSINESS_NAME} 👋

How can I help you today? Reply with a number:

1️⃣ Book an appointment
2️⃣ Ask a question
```

---

### STEP 2A — Customer replies "1" (Book)
**Bot replies:**
```
Great! What service would you like?

1️⃣ {SERVICE_1} — {PRICE_1}
2️⃣ {SERVICE_2} — {PRICE_2}
3️⃣ {SERVICE_3} — {PRICE_3}
```

---

### STEP 3 — Customer picks a service
**Bot replies:**
```
Perfect! What's your name?
```

---

### STEP 4 — Customer gives name
**Bot replies:**
```
Thanks {NAME}! What date and time works for you?

We're open: {DAYS}: {HOURS}

Just reply with your preferred date and time (e.g. "Friday 3pm")
```

---

### STEP 5 — Customer gives date and time
**Bot replies:**
```
You're all set, {NAME}! ✅

📋 Service: {SERVICE}
📅 Date: {DATE}
⏰ Time: {TIME}
📍 {BUSINESS_NAME}

We'll see you then! Message us if anything changes.
```

**Simultaneously:** Row added to Google Sheet.

---

### STEP 2B — Customer replies "2" (Question)
**Bot replies:**
```
Of course! What would you like to know?
You can ask about our services, prices, or location.
Or reply BOOK any time to make a booking.
```

Any follow-up → bot replies:
```
Thanks! {OWNER_NAME} will get back to you shortly.
```

Owner replies manually from their WhatsApp.

---

### GOOGLE SHEET STRUCTURE

| Column | Stores |
|--------|--------|
| A — Timestamp | When booking was made |
| B — Customer Phone | Their WhatsApp number |
| C — Customer Name | Collected in flow |
| D — Service | What they booked |
| E — Date | Preferred date |
| F — Time | Preferred time |
| G — Status | New / Confirmed / Completed |

---

### MAKE.COM SCENARIO — 4 MODULES

```
[1] Webhook → receives WhatsApp message
        ↓
[2] Router → checks conversation step (via Data Store)
        ↓
[3] WhatsApp → sends next message in flow
        ↓
[4] Google Sheets → logs booking when complete
```

Conversation state tracked per customer using Make.com's built-in Data Store (key: phone number, value: current step).

---

## E. MVP SETUP CHECKLIST (Under 2 Hours)

### Phase 1 — WhatsApp API (30 min)
```
[ ] Create Meta Developer account (developers.facebook.com)
[ ] New App → Business type → add WhatsApp product
[ ] Register phone number (must not be on personal WhatsApp)
[ ] Generate permanent System User access token
[ ] Save: PHONE_NUMBER_ID + ACCESS_TOKEN
```

### Phase 2 — Google Sheet (5 min)
```
[ ] Create new Google Sheet: "{BUSINESS_NAME} — Bookings"
[ ] Add headers: Timestamp, Phone, Name, Service, Date, Time, Status
[ ] Share with client
```

### Phase 3 — Make.com (45 min)
```
[ ] Create free Make.com account
[ ] New scenario → Webhook module → copy webhook URL
[ ] In Meta Developer → WhatsApp → Webhook → paste URL → verify
[ ] Build router with step conditions
[ ] Add WhatsApp HTTP module for each reply
[ ] Add Google Sheets module to log bookings
[ ] Add Data Store module to track conversation step
[ ] Activate scenario
```

### Phase 4 — Test (20 min)
```
[ ] Send "Hi" to the number
[ ] Complete full booking flow as a customer
[ ] Confirm row appears in Google Sheet
[ ] Send "2" — confirm owner receives the question
[ ] Go live
```

---

## F. MVP — VARIABLES TO FILL PER CLIENT (10 Variables, 5 Minutes)

```
{BUSINESS_NAME}    e.g. "Dave's Barbershop"
{OWNER_NAME}       e.g. "Dave"
{SERVICE_1}        e.g. "Haircut"
{PRICE_1}          e.g. "$15"
{SERVICE_2}        e.g. "Beard Trim"
{PRICE_2}          e.g. "$10"
{SERVICE_3}        e.g. "Haircut + Beard"
{PRICE_3}          e.g. "$20"
{DAYS}             e.g. "Mon–Sat"
{HOURS}            e.g. "9am – 6pm"
```

---

## G. FULL SYSTEM — UPGRADE PATH

Once the MVP is live and the client is seeing bookings, add these one at a time.
Each is a small addition to the existing Make.com scenario — nothing gets rebuilt.

| Upgrade | What It Adds | Trigger to Upgrade |
|---------|-------------|-------------------|
| + Google Calendar | Auto-creates events, prevents double bookings | Client starts getting 10+ bookings/week |
| + Airtable CRM | Better lead tracking, statuses, views | Google Sheet feels too basic |
| + Claude AI | Natural replies instead of fixed menu | Client wants smarter conversations |
| + Reminders | 24h + 2h appointment reminders | No-shows become a problem |
| + Follow-ups | Thank-you + 7-day rebook prompt | Client wants repeat business automated |
| + Daily summary | Owner WhatsApp briefing every morning | Client has 20+ bookings/week |

---

## H. FULL SYSTEM — TECH STACK

| Layer | Tool | Why |
|-------|------|-----|
| Messaging | WhatsApp Cloud API (Meta) | Free tier, official, scales to millions |
| Automation | Make.com | Visual, no-code, handles branching logic, mobile-friendly |
| AI Responses | Claude API (Haiku model) | Fast, cheap (~$0.002/conversation), context-aware |
| Database | Airtable | No-code CRM, mobile-accessible, duplicatable |
| Calendar | Google Calendar API | Free, universal, easy owner sharing |

---

## I. FULL SYSTEM — AI REPLY PROMPT

Replace the fixed numbered menu with this Claude system prompt:

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
If the customer says "human", "urgent", or "call me" — say:
"I'm connecting you with {OWNER_NAME} now. They'll be with you shortly."
```

---

## J. FULL SYSTEM — FAILURE HANDLING

| Failure | Detection | Fallback |
|---------|-----------|----------|
| WhatsApp API down | HTTP error in Make.com | Queue → retry every 5 min × 3, then alert owner |
| Claude API timeout | Empty/error response | Send pre-written fallback message, flag in Airtable |
| Double booking | Google Calendar conflict | Never confirm — offer next 3 available slots |
| Airtable write fails | HTTP 422/500 | Store in Make.com Data Store, retry after 10 min |
| Unrecognised message | No clear intent | "Let me connect you with {OWNER_NAME} directly." |
| Scenario error | Make.com built-in alert | Pause + notify owner, queue incoming messages |

---

## K. SCALING STRATEGY

### 1–100 Users (Single Business, MVP)
- Stack: WhatsApp free tier + Make.com free + Google Sheets
- Cost: $0/month
- Action: Set up once, check weekly

### 1–100 Users (Single Business, Full System)
- Stack: Make.com Core + Airtable Free + Claude Haiku
- Cost: ~$14–18/month
- Action: Set up once, review monthly

### 100–1,000 Users (Multi-Client SaaS)
- Each client: own WhatsApp number + cloned Make.com scenario + duplicated Airtable base
- Add: client intake form → auto-configures system
- Add: Make.com Team plan, shared Claude API key with per-client tracking
- Cost: ~$80–150/month base + per-client margin

### 1,000–10,000+ Users (Platform Scale)
- Central webhook router in Node.js (Railway/Render)
- Client configs in PostgreSQL (Supabase)
- Replace Make.com with Node.js + BullMQ job queue
- Move WhatsApp to a BSP for volume pricing
- Add Sentry + PostHog
- Cost: ~$300–800/month infrastructure

---

## L. MONETISATION MODEL

### Pricing

| Tier | Setup | Monthly | What's Running |
|------|-------|---------|----------------|
| MVP | $49 | $0 | Fixed menu + Google Sheets |
| Starter | $49 | $29 | MVP + Airtable CRM + basic AI |
| Growth | $99 | $59 | Starter + reminders + follow-ups + daily summary |
| Pro | $149 | $99 | Growth + custom AI persona + 2 numbers + reports |

### Unit Economics (Growth Tier)
- Claude Haiku (2,000 conversations): ~$6/month
- Make.com share: ~$5/month
- WhatsApp (over free tier): ~$2.50/month
- Airtable: $0
- **Total cost: ~$13.50/month | Revenue: $59/month | Margin: ~77%**

### Revenue Strategy
1. Sell the MVP at $49 setup, $0/month — low barrier, fast yes
2. After 30 days, show them the booking data, pitch the upgrade
3. Most clients move to Growth within 60 days
4. Setup fee covers your time; monthly covers costs + support + profit

---

## M. PRODUCTIZED SERVICE — 80/20 SPLIT

### What NEVER Changes Per Client (Built Once, Reused Forever)

| Component | How It's Reused |
|-----------|----------------|
| Make.com blueprint | Import → update variables |
| Message flow + booking steps | Fill in business details only |
| WhatsApp message templates | Resubmit per account (same text) |
| Google Sheet structure | Duplicate from master |
| Airtable base (full system) | Duplicate from master |
| Error handling + fallback logic | Identical across all clients |
| Onboarding checklist | Same process every time |

### What CHANGES Per Client (One Config File — 5 Minutes)

```
BUSINESS_NAME
BUSINESS_TYPE
LOCATION
SERVICES + PRICES (up to 5)
OPENING HOURS
OWNER_NAME
OWNER_PHONE
WHATSAPP_BUSINESS_NUMBER
```

---

## N. ONGOING MAINTENANCE PER CLIENT

| Task | Frequency | Time |
|------|-----------|------|
| Check Make.com for errors | Weekly | 5 min |
| Review reply quality (month 1 only) | Weekly | 10 min |
| Client check-in | Monthly | 10 min |
| Change requests (new hours, services) | On demand | 15 min |
| **Total per month** | | **~30 min** |

At 20 clients: ~10 hours/month
At 50 clients: hire a VA — onboarding guide is their SOP

---

## O. SALES PROCESS

**Qualify (5 minutes):**
- Do you use WhatsApp for customer enquiries?
- Do you miss messages or feel overwhelmed?
- Do you manage bookings manually?

Two yes answers = buyer.

**Demo:** Let them message a live demo number. They go through the booking flow as a customer. 90 seconds. They sell themselves.

**Close:** Start with MVP ($49, $0/month). Remove all friction. Once they see it working, upsell to Growth.

**Onboard:** Send 10-variable intake form → 2-hour setup call → live.

---

## P. AUTOMATION ROADMAP (Future Upgrades)

### Near-term
- Automated client onboarding (intake form → system self-configures)
- Google/Facebook review requests post-service
- Payment links via Paystack / Flutterwave / WhatsApp Pay

### Mid-term
- Auto-detect customer language → reply in same language
- Loyalty tracking (Nth visit = automatic discount message)
- AI-generated weekly performance reports for owner

### Long-term
- Voice note transcription + AI response
- Predictive scheduling (slow-day promo messages auto-sent)
- Staff scheduling integration (only show slots when staff is available)

---

## KEY CONSTRAINTS TO SET WITH CLIENTS

- One dedicated phone number required (cannot use their personal WhatsApp)
- WhatsApp outbound templates need 24–48h Meta approval before first send
- Bot handles ~80% of conversations — some will always need the owner
- Human handoff is always available — customers can trigger it anytime
- MVP has no conflict checking — owner confirms manually from the Sheet
