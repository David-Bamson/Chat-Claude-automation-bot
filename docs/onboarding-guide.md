# Client Onboarding Guide
### Deploy a new business in under 90 minutes

---

## Pre-Onboarding Checklist (Collect from client)

Send client this form (Typeform or Google Form) before the session:

```
1. Business name
2. Business type (barber / salon / bakery / other)
3. Location / address
4. Services offered + prices
5. Opening hours
6. Owner's name
7. Owner's WhatsApp number (personal — for alerts)
8. A phone number to use for the WhatsApp Business account
   (must not be registered on personal WhatsApp)
9. Google account email (for calendar access)
10. Preferred language for customer replies
```

---

## Onboarding Steps

### Step 1 — Fill the Config File (5 mins)
1. Open `config/business-config-template.json`
2. Duplicate it → rename to `config/{business-name}.json`
3. Fill in all `{{VARIABLES}}` with client's info
4. Save

---

### Step 2 — Set Up Meta & WhatsApp API (20 mins)
Follow `whatsapp/webhook-setup.md` completely.

Collect and save:
- `PHONE_NUMBER_ID`
- `ACCESS_TOKEN` (permanent)
- `WHATSAPP_BUSINESS_ACCOUNT_ID`

---

### Step 3 — Clone Airtable Base (5 mins)
1. Open the master Airtable template base
2. Click Share → Duplicate Base
3. Rename to `{BUSINESS_NAME} — CRM`
4. Share with client's email (Editor access)
5. Copy the `Base ID` from the URL → add to config file

**Airtable base URL format:**
`https://airtable.com/appXXXXXXXXXX/...`
The `appXXXXXXXXXX` part is the Base ID.

---

### Step 4 — Import Make.com Scenario (10 mins)
1. Log into Make.com
2. Create New Scenario → Import Blueprint
3. Upload `make-com/scenario-template.json`
4. Update all connection credentials:
   - WhatsApp: paste `PHONE_NUMBER_ID` + `ACCESS_TOKEN`
   - Airtable: connect account → select cloned base
   - Google Calendar: connect client's Google account
   - Claude API: use shared API key (or client's own)
5. Update all `{{VARIABLES}}` in the scenario text nodes
6. Activate the scenario

---

### Step 5 — Connect Webhook to Meta (5 mins)
1. Copy webhook URL from Make.com scenario
2. Paste into Meta Developer App → WhatsApp → Webhook
3. Verify (scenario must be active)
4. Subscribe to `messages` field

---

### Step 6 — Submit WhatsApp Templates (10 mins)
1. Go to Meta Business Suite → WhatsApp Manager → Message Templates
2. Submit templates from `whatsapp/message-templates.md`
3. Start with UTILITY templates (approved faster)
4. Note: MARKETING templates take 24–48 hours

---

### Step 7 — End-to-End Test (15 mins)

Use a test phone to simulate a full customer journey:

```
Test 1 — Booking flow
[ ] Send: "Hi, I want to book a haircut"
[ ] Bot replies with greeting + asks for details
[ ] Send: name, service, date, time
[ ] Bot confirms booking
[ ] Check: Google Calendar event created
[ ] Check: Airtable record created with status = Confirmed

Test 2 — Unknown message
[ ] Send: "Do you do home visits?"
[ ] Bot gives helpful reply or escalates to owner

Test 3 — Human handoff
[ ] Send: "I need to speak to someone"
[ ] Bot sends handoff message
[ ] Owner receives alert on personal WhatsApp

Test 4 — Reminder (manual trigger)
[ ] Manually trigger reminder scenario in Make.com
[ ] Confirm reminder message is received
```

---

### Step 8 — Hand Over to Client (10 mins)

Send client:
1. Link to their Airtable CRM base
2. WhatsApp Business number is now live
3. Brief voice note explaining:
   - How to view today's bookings in Airtable
   - What to do if they want to reply manually (just reply in WhatsApp — bot pauses for 24hr window)
   - How to contact you for support

---

## Post-Onboarding (Ongoing)

| When | Action |
|------|--------|
| Day 3 | Check Airtable for any failed records |
| Day 7 | Review Make.com scenario history for errors |
| Month 1 | Review AI response quality — adjust system prompt if needed |
| Month 3 | Review if client is ready to upgrade tier |

---

## Common Setup Errors

| Error | Fix |
|-------|-----|
| Webhook verification fails | Make sure scenario is active before clicking verify in Meta |
| Messages not appearing in Make.com | Check webhook is subscribed to `messages` field |
| Calendar events not created | Re-authenticate Google Calendar in Make.com |
| Airtable records not saving | Check Base ID and Table Name match exactly |
| Bot sends duplicate replies | Add message ID deduplication check in Make.com |
