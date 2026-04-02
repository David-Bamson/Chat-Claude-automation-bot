# WhatsApp Automation MVP
### Auto Reply + Lead Capture + Basic Booking — Set Up in Under 2 Hours

---

## THE ONLY 3 TOOLS YOU NEED

| Tool | Purpose | Cost |
|------|---------|------|
| **WhatsApp Cloud API** (Meta) | Receive + send messages | Free |
| **Make.com** | Run the automation | Free (up to 1,000 ops/month) |
| **Google Sheets** | Store leads + bookings | Free |

**Total monthly cost: $0 to start.**
Upgrade only when the free tiers run out.

---

## WHAT THE MVP DOES

```
Customer messages the business WhatsApp
        ↓
Bot replies instantly with a greeting + menu
        ↓
Customer picks: Book / Ask a Question
        ↓
Bot collects: Name → Service → Date & Time
        ↓
Booking saved to Google Sheets
        ↓
Customer gets a confirmation message
        ↓
Owner sees the booking in their Google Sheet
```

That's it. No AI. No calendar sync. No reminders. Just the core loop — working, live, in 2 hours.

---

## WHAT IS REMOVED FROM THE FULL SYSTEM (AND WHY)

| Removed | Reason |
|---------|--------|
| Claude AI responses | Adds cost + complexity. Fixed menu works fine at MVP stage. |
| Google Calendar sync | Nice to have. Owner can add manually from the Sheet. |
| Automated reminders | Not needed until bookings are flowing consistently |
| Post-service follow-ups | Phase 2 |
| Owner daily summary | They can open the Sheet |
| Error retry logic | Over-engineered for < 50 customers/month |
| Multi-tier routing | One simple flow handles 90% of conversations |

---

## THE MVP FLOW (Step by Step)

### Message 1 — Customer writes anything
Bot replies:

```
Hi! Welcome to {BUSINESS_NAME} 👋

How can I help you today?
Reply with a number:

1️⃣ Book an appointment
2️⃣ Ask a question
```

---

### Message 2 — Customer replies "1" (Book)
Bot replies:

```
Great! Let's get you booked.

What service would you like?
Reply with a number:

1️⃣ {SERVICE_1} — {PRICE_1}
2️⃣ {SERVICE_2} — {PRICE_2}
3️⃣ {SERVICE_3} — {PRICE_3}
```

---

### Message 3 — Customer picks a service
Bot replies:

```
Perfect! What's your name?
```

---

### Message 4 — Customer gives name
Bot replies:

```
Thanks {NAME}! What date and time works for you?

We're open:
{DAYS}: {HOURS}

Just reply with your preferred date and time
(e.g. "Friday 3pm")
```

---

### Message 5 — Customer gives date/time
Bot replies:

```
You're all set, {NAME}! ✅

Here's your booking:
📋 Service: {SERVICE}
📅 Date: {DATE}
⏰ Time: {TIME}
📍 {BUSINESS_NAME}

We'll see you then! If anything changes, just message us here.
```

**Simultaneously:** Row added to Google Sheet with all details.

---

### Message 2 (alternate) — Customer replies "2" (Question)
Bot replies:

```
Of course! What would you like to know?

You can ask about our services, prices, or location.
Or reply BOOK any time to make a booking.
```

Any follow-up message → bot replies:

```
Thanks for your message! {OWNER_NAME} will get back to you shortly.
```

Owner sees the message in their WhatsApp and replies manually.

---

## GOOGLE SHEET STRUCTURE

One sheet. Seven columns.

| Column | What It Stores |
|--------|---------------|
| A — Timestamp | When the booking was made |
| B — Customer Phone | Their WhatsApp number |
| C — Customer Name | Collected during flow |
| D — Service | What they booked |
| E — Date | Preferred date |
| F — Time | Preferred time |
| G — Status | New / Confirmed / Completed |

Owner manages status manually by updating column G.

---

## MAKE.COM SCENARIO STRUCTURE

4 modules. That's all.

```
[1] Webhook — receives WhatsApp message
        ↓
[2] Router — checks conversation state
    (what step is the customer on?)
        ↓
[3] WhatsApp — sends the next message in the flow
        ↓
[4] Google Sheets — adds row when booking complete
```

Conversation state is tracked using Make.com's built-in Data Store.
Key: customer phone number. Value: current step (1, 2, 3, 4, or "done").

---

## SETUP CHECKLIST (Under 2 Hours)

### Phase 1 — WhatsApp API (30 min)
```
[ ] Create a free Meta Developer account (developers.facebook.com)
[ ] Create a new App → choose Business type
[ ] Add WhatsApp product to the app
[ ] Register a phone number (needs a SIM not on personal WhatsApp)
[ ] Generate a permanent System User access token
[ ] Save: PHONE_NUMBER_ID and ACCESS_TOKEN
```

### Phase 2 — Google Sheet (5 min)
```
[ ] Create a new Google Sheet named "{BUSINESS_NAME} — Bookings"
[ ] Add column headers: Timestamp, Phone, Name, Service, Date, Time, Status
[ ] Share the sheet with yourself (or the client)
```

### Phase 3 — Make.com (45 min)
```
[ ] Create a free Make.com account
[ ] New scenario → add Webhook module → copy webhook URL
[ ] In Meta Developer app → WhatsApp → Webhook → paste URL → verify
[ ] Add Router module with conditions for each conversation step
[ ] Add WhatsApp HTTP module for each reply (use Meta Graph API)
[ ] Add Google Sheets module to log completed bookings
[ ] Add Data Store module to track conversation step per customer
[ ] Turn scenario ON
```

### Phase 4 — Test (20 min)
```
[ ] Send "Hi" to the WhatsApp number
[ ] Go through the full booking flow as a customer
[ ] Confirm the row appears in Google Sheets
[ ] Send "2" (question flow) — confirm owner receives it
[ ] Done
```

---

## VARIABLES TO FILL IN (Per Client)

```
{BUSINESS_NAME}     → e.g. "Dave's Barbershop"
{OWNER_NAME}        → e.g. "Dave"
{SERVICE_1}         → e.g. "Haircut"
{PRICE_1}           → e.g. "$15"
{SERVICE_2}         → e.g. "Beard Trim"
{PRICE_2}           → e.g. "$10"
{SERVICE_3}         → e.g. "Haircut + Beard"
{PRICE_3}           → e.g. "$20"
{DAYS}              → e.g. "Mon–Sat"
{HOURS}             → e.g. "9am – 6pm"
```

10 variables. 5 minutes to fill.

---

## HOW TO SELL THE MVP

**One sentence pitch:**
> "I set up a WhatsApp bot for your business that replies to customers instantly, takes their booking, and saves it to a spreadsheet — all while you sleep."

**What to demo:**
Pull out your phone. Message a live demo number. Walk them through the booking flow in real time. Takes 90 seconds. They see it. They want it.

**Pricing for MVP:**
- Setup: $49 (your time to configure)
- Monthly: $0 (everything is on free tiers)

Once they're seeing value (bookings coming in consistently), upsell them to the full system at $59/month — adding reminders, AI replies, and calendar sync.

---

## MVP → FULL SYSTEM UPGRADE PATH

Add these one at a time, only when the client asks or needs them:

| Upgrade | What It Adds | Extra Cost |
|---------|-------------|------------|
| + Claude AI | Natural replies instead of fixed menu | ~$5/month |
| + Google Calendar | Auto-creates calendar events | Free |
| + Reminders | 24h + 2h appointment reminders | Included in Make.com |
| + Follow-ups | Thank-you + rebook messages | Included in Make.com |
| + Daily summary | Owner morning WhatsApp briefing | Included in Make.com |

Each upgrade is a small addition to the existing Make.com scenario.
Nothing needs to be rebuilt. The MVP is the foundation.

---

## LIMITATIONS TO SET WITH CLIENTS UPFRONT

- Bot handles bookings only — complex questions go to the owner
- No automatic conflict checking — owner confirms bookings manually from the Sheet
- No calendar sync until the upgrade
- WhatsApp templates need Meta approval (24–48h) for outbound messages
- One phone number required — cannot use their personal WhatsApp number
