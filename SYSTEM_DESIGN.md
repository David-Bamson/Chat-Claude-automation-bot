# WhatsApp Business Automation System
### For Small & Medium Local Businesses (Barbers, Salons, Bakeries, Vendors, Service Providers)

---

## A. BUSINESS CONTEXT

### Business Type
Small and medium-sized local businesses that use WhatsApp as their primary customer communication channel.

**Target verticals:**
- Barbers & hair salons
- Nail studios & beauty parlours
- Bakeries & food vendors
- Home service providers (cleaners, plumbers, tutors)
- Clothing vendors & tailors

### Core Problem
These businesses receive high volumes of WhatsApp messages daily but have no system to:
- Respond instantly (customers leave if ignored for >5 mins)
- Capture lead data consistently
- Manage bookings without manual back-and-forth
- Follow up on abandoned conversations
- Store records for repeat customers

The owner handles everything manually from one phone, causing missed leads, double-bookings, and lost revenue.

### Target Users
- **Primary:** Business owner (operates from mobile, non-technical)
- **Secondary:** Customers (any WhatsApp user)
- **Tertiary:** Staff members who may need booking visibility

---

## B. SYSTEM OVERVIEW

### Full Workflow Summary
```
Customer sends WhatsApp message
        ↓
WhatsApp Business API receives message
        ↓
Automation platform (Make.com) triggers workflow
        ↓
AI (Claude/GPT) generates context-aware reply
        ↓
Lead data extracted & saved to Airtable/Google Sheets
        ↓
Booking confirmed → Calendar event created
        ↓
Customer receives confirmation + reminder messages
        ↓
Post-service follow-up sent automatically
        ↓
Owner sees clean dashboard of all bookings & leads
```

### Data Flow
```
[Customer WhatsApp]
        │
        ▼
[WhatsApp Cloud API] ←→ [Meta Business Account]
        │
        ▼
[Make.com Webhook] — receives message payload
        │
        ├──→ [AI Response Engine] → reply sent back
        │
        ├──→ [Lead Capture Module] → Airtable/Sheets
        │
        ├──→ [Booking Engine] → Google Calendar
        │
        └──→ [Follow-up Scheduler] → timed messages
```

---

## C. TECH STACK

| Layer | Tool | Why |
|---|---|---|
| **Messaging** | WhatsApp Cloud API (Meta) | Free tier available, official API, scales to millions |
| **Automation** | Make.com (formerly Integromat) | Visual, no-code, mobile-friendly, handles complex logic |
| **AI Responses** | Claude API (claude-haiku-4-5) | Fast, cheap, context-aware replies |
| **Database** | Airtable | No-code, mobile-friendly, visual CRM |
| **Calendar** | Google Calendar API | Free, universal, easy sharing with owner |
| **Notifications** | WhatsApp Cloud API (outbound) | Same channel, no extra tool |
| **Dashboard** | Airtable + Make.com dashboard | Zero extra cost, mobile accessible |
| **Template Store** | GitHub (this repo) | Version control for workflow templates |

### Why not Twilio or other SMS platforms?
WhatsApp Cloud API is free for the first 1,000 conversations/month per phone number. Twilio charges per message. For local businesses, WhatsApp is already the preferred channel — no behaviour change needed.

### Why Make.com over Zapier?
Make.com handles multi-step conditional logic, loops, and error handling natively. Zapier is linear. This system requires branching (booking vs. enquiry vs. order) — Make.com handles that cleanly.

---

## D. STEP-BY-STEP WORKFLOW

### TRIGGER
Customer sends any message to the business WhatsApp number.

---

### STEP 1 — Receive & Parse Message
**Trigger:** Webhook fires in Make.com when message arrives via WhatsApp Cloud API

**Action:**
- Extract: `customer_phone`, `customer_name` (if available), `message_body`, `timestamp`
- Detect message intent via keyword matching OR AI classification:
  - `booking` → route to Booking Flow
  - `order` → route to Order Flow
  - `enquiry` → route to General Enquiry Flow
  - `unknown` → route to AI Freeform Response

**Result:** Message classified and routed correctly

---

### STEP 2 — AI Response Generation
**Trigger:** Classified message passed to Claude API

**Action:**
- Send message + business context (name, services, hours, pricing) to Claude
- Use a system prompt template (customisable per business)
- Claude returns a natural, human-sounding reply

**System Prompt Template:**
```
You are a friendly assistant for {BUSINESS_NAME}, a {BUSINESS_TYPE} located in {LOCATION}.
Services offered: {SERVICES_LIST}
Operating hours: {HOURS}
Pricing: {PRICING_SUMMARY}

Your job is to:
1. Greet the customer warmly
2. Answer their question accurately
3. Guide them toward booking or placing an order
4. Ask for: their name, preferred service, and preferred date/time

Always reply in a conversational, helpful tone. Keep replies under 3 sentences unless more detail is needed.
```

**Result:** Personalised reply drafted

---

### STEP 3 — Send Reply to Customer
**Trigger:** AI response ready

**Action:**
- WhatsApp Cloud API sends message back to customer's number
- If booking flow: send interactive button message (Book Now / Ask a Question)
- If order flow: send product list or menu template message

**Result:** Customer receives instant reply (typically < 3 seconds)

---

### STEP 4 — Lead Capture
**Trigger:** Any conversation where name, service, or time is mentioned

**Action:**
- Extract structured data using AI or regex:
  - `name`, `service_requested`, `preferred_date`, `preferred_time`, `phone`
- Check Airtable: does this phone number already exist?
  - YES → update existing record
  - NO → create new lead record

**Airtable Fields:**
```
- Phone Number (unique ID)
- Customer Name
- Service Requested
- Preferred Date
- Preferred Time
- Status [New Lead / Confirmed / Completed / No-Show / Follow-up Pending]
- Source [WhatsApp]
- Created At
- Last Contacted
- Notes
```

**Result:** Lead captured without owner lifting a finger

---

### STEP 5 — Booking Confirmation
**Trigger:** Customer confirms date and time

**Action:**
1. Check Google Calendar for conflicts at requested slot
2. If slot available:
   - Create Google Calendar event (title: customer name + service)
   - Update Airtable record status → `Confirmed`
   - Send customer confirmation message with date/time summary
3. If slot unavailable:
   - AI offers next 3 available slots
   - Customer selects → loop back to step 5

**Result:** Booking locked in, calendar updated, customer confirmed

---

### STEP 6 — Automated Reminders
**Trigger:** Scheduled time-based trigger (Make.com scheduler)

**Action:**
- 24 hours before appointment: send reminder message
- 2 hours before appointment: send final reminder
- Template:

```
Hi {NAME}, just a reminder about your {SERVICE} appointment tomorrow at {TIME}.
Reply YES to confirm or NO to reschedule. See you soon! 💈
```

- If customer replies NO → trigger reschedule flow
- If no reply → flag in Airtable for owner review

**Result:** No-shows reduced, owner notified of issues

---

### STEP 7 — Post-Service Follow-up
**Trigger:** Appointment status changed to `Completed` in Airtable (manual or auto via time trigger)

**Action:**
- 4 hours after appointment: send thank-you message
- 7 days later: send re-booking prompt

```
Hi {NAME}, it was great having you at {BUSINESS_NAME}!
We'd love to see you again — reply BOOK to schedule your next {SERVICE}. 🙌
```

**Result:** Repeat bookings generated passively

---

### STEP 8 — Owner Dashboard
**Trigger:** Real-time / on-demand

**Action:**
- Airtable view shows: Today's bookings, Pending leads, Follow-ups due
- Make.com sends owner a daily WhatsApp summary every morning:

```
Good morning! Here's your day:
📅 Bookings today: 4
🔔 Pending leads: 2
📌 Follow-ups due: 1
View full dashboard: [Airtable link]
```

**Result:** Owner informed without logging into multiple tools

---

## E. FAILURE HANDLING

### Failure Point 1: WhatsApp API is down or rate-limited
- **Detection:** Make.com scenario returns HTTP error
- **Fallback:** Store message in Make.com data store queue → retry every 5 minutes up to 3 times
- **Owner alert:** If undelivered after 15 mins, WhatsApp the owner's personal number

---

### Failure Point 2: AI response fails (Claude API timeout/error)
- **Detection:** HTTP error or empty response from Claude
- **Fallback:** Send pre-written default response:
  ```
  Hi! Thanks for reaching out to {BUSINESS_NAME}. We'll get back to you shortly.
  For urgent enquiries, call us on {PHONE}.
  ```
- **Recovery:** Flag conversation in Airtable for manual follow-up

---

### Failure Point 3: Double booking (calendar conflict not caught)
- **Detection:** Google Calendar API returns conflict
- **Fallback:** Never write conflicting event — always check before confirming
- **Buffer rule:** Build 15-min buffer between appointments in calendar config
- **Recovery:** If duplicate detected post-creation, auto-message customer to reschedule

---

### Failure Point 4: Airtable write fails
- **Detection:** HTTP 422/500 from Airtable API
- **Fallback:** Make.com stores failed record in internal data store
- **Recovery:** Retry after 10 minutes, alert owner if still failing after 3 attempts

---

### Failure Point 5: Customer sends unrecognised message
- **Detection:** AI confidence score low OR intent = `unknown`
- **Fallback:** Send handoff message:
  ```
  I want to make sure I help you correctly — let me connect you with {OWNER_NAME} directly.
  ```
- **Action:** Notify owner via WhatsApp with customer's message for manual reply

---

### Failure Point 6: Make.com scenario errors / broken workflow
- **Detection:** Make.com built-in error alerting
- **Fallback:** Make.com error handler module routes to owner notification
- **Recovery:** Scenario pauses, owner gets notified, unfailed messages are queued

---

## F. SCALING STRATEGY

### Phase 1 — 1 to 100 Users (Single Business)
- **Stack:** WhatsApp Cloud API free tier + Make.com Core plan + Airtable Free
- **Ops cost:** ~$15/month (Make.com plan)
- **Capacity:** 1,000 conversations/month (free WhatsApp tier)
- **Owner action required:** Set up once, monitor weekly

---

### Phase 2 — 100 to 1,000 Users (Multi-Business Deployment)
- **Model shift:** System becomes a SaaS template — each business gets its own:
  - WhatsApp number (via Meta Business API)
  - Make.com sub-account or cloned scenario
  - Airtable base (from template)
- **Introduce:** Onboarding automation — business fills a form → system auto-configures
- **Stack additions:**
  - Softr or Glide for a self-service client portal
  - Make.com Team plan for multiple scenarios
  - Shared Claude API key with per-client usage tracking
- **Ops cost:** ~$80–150/month base + per-client margin

---

### Phase 3 — 1,000 to 10,000+ Users (Platform Scale)
- **Architecture shift:** Build a lightweight backend (Node.js on Railway/Render)
  - Centralised webhook router (one endpoint, routes by phone number to correct client config)
  - Client config stored in PostgreSQL (Supabase)
  - Claude API calls batched and cached where possible
- **Replace:** Make.com with custom Node.js + BullMQ job queue for reliability
- **WhatsApp:** Move to WhatsApp Business Solution Provider (BSP) for volume discounts
- **Monitoring:** Add Sentry for error tracking, PostHog for analytics
- **Ops cost:** ~$300–800/month infrastructure, scale with revenue

---

## G. MONETISATION MODEL

### Target Customer
Local business owners paying to save time and increase bookings.

### Pricing Strategy

| Tier | Price | Includes |
|---|---|---|
| **Starter** | $49 setup + $29/month | 1 number, 500 conversations/mo, basic flow, Airtable CRM |
| **Growth** | $99 setup + $59/month | 1 number, 2,000 conversations/mo, AI replies, reminders, follow-ups |
| **Pro** | $149 setup + $99/month | 2 numbers, unlimited conversations, custom AI persona, analytics |
| **Agency** | Custom | White-label, multi-client dashboard, priority support |

### Revenue Mechanics
- **Setup fee:** Covers your time to onboard and configure
- **Monthly retainer:** Covers API costs + platform fees + support
- **Upsells:** Extra phone numbers, custom AI training, SMS fallback, staff access

### Unit Economics (Growth Tier Example)
- Claude API cost per conversation: ~$0.002 (Haiku model)
- 2,000 conversations = ~$4/month in AI costs
- Make.com cost share: ~$5/month
- Airtable cost share: ~$3/month
- **Total cost per client: ~$12/month**
- **Revenue per client: $59/month**
- **Gross margin: ~80%**

---

## H. AUTOMATION OPPORTUNITIES (Future Roadmap)

### Near-term (0–3 months)
- [ ] Automated onboarding: business fills Typeform → system configures itself
- [ ] AI-generated weekly reports sent to owner via WhatsApp
- [ ] Automated review requests (Google / Facebook) post-service

### Mid-term (3–6 months)
- [ ] Payment collection via WhatsApp Pay or Paystack/Flutterwave link
- [ ] Multi-language support (auto-detect customer language, reply accordingly)
- [ ] Loyalty tracking (Nth visit = auto discount message)

### Long-term (6–12 months)
- [ ] Voice note transcription + AI response (handle voice messages)
- [ ] AI learns each business's most common questions over time
- [ ] Predictive scheduling (suggest slow-day promotions automatically)
- [ ] Staff scheduling integration (only offer slots when staff is available)

---

## DEPLOYMENT CHECKLIST (Per Business)

```
[ ] Create Meta Business Account + WhatsApp Business API number
[ ] Clone Make.com scenario template
[ ] Fill in business config variables:
    - BUSINESS_NAME
    - BUSINESS_TYPE
    - LOCATION
    - SERVICES_LIST
    - HOURS
    - PRICING_SUMMARY
    - OWNER_PHONE
[ ] Clone Airtable base from template
[ ] Connect Google Calendar
[ ] Test full flow (send test message, confirm booking, check calendar)
[ ] Set live — go
```

**Estimated setup time per business: 45–90 minutes**

---

## REPOSITORY STRUCTURE

```
/
├── SYSTEM_DESIGN.md          ← This document
├── make-com/
│   ├── scenario-template.json    ← Importable Make.com blueprint
│   └── error-handler.json        ← Error handling module
├── ai-prompts/
│   ├── system-prompt-template.md ← Customisable AI persona
│   └── intent-classifier.md      ← Intent detection prompt
├── airtable/
│   └── base-schema.json          ← Airtable base structure
├── whatsapp/
│   ├── message-templates.md      ← Approved WhatsApp templates
│   └── webhook-setup.md          ← API webhook configuration guide
├── docs/
│   ├── onboarding-guide.md       ← Step-by-step client setup
│   └── troubleshooting.md        ← Common issues + fixes
└── config/
    └── business-config-template.json  ← Variables per deployment
```
