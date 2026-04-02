# WhatsApp Message Templates

All templates below must be submitted to Meta for approval before use in outbound (business-initiated) messages.
Inbound replies (customer-initiated conversations) can use freeform text within the 24-hour window.

Replace `{{VARIABLES}}` per client config.

---

## TEMPLATE 1 — Booking Confirmation
**Template Name:** `booking_confirmation`
**Category:** UTILITY

```
Hi {{1}}, your {{2}} appointment at {{BUSINESS_NAME}} is confirmed for {{3}} at {{4}}.

Reply YES to confirm or RESCHEDULE to change your time.

See you soon!
```

Variables: `[customer_name, service, date, time]`

---

## TEMPLATE 2 — 24-Hour Reminder
**Template Name:** `appointment_reminder_24h`
**Category:** UTILITY

```
Hi {{1}}, reminder: your {{2}} is tomorrow at {{3}} at {{BUSINESS_NAME}}.

Reply YES to confirm or NO to reschedule.
```

Variables: `[customer_name, service, time]`

---

## TEMPLATE 3 — 2-Hour Reminder
**Template Name:** `appointment_reminder_2h`
**Category:** UTILITY

```
See you soon, {{1}}! Your {{2}} appointment at {{BUSINESS_NAME}} is in 2 hours ({{3}}).

📍 {{LOCATION}}
```

Variables: `[customer_name, service, time]`

---

## TEMPLATE 4 — Post-Service Thank You
**Template Name:** `post_service_thankyou`
**Category:** MARKETING

```
Hi {{1}}, thank you for visiting {{BUSINESS_NAME}} today!

We hope you loved your {{2}}. We'd love to see you again — reply BOOK whenever you're ready. 🙌
```

Variables: `[customer_name, service]`

---

## TEMPLATE 5 — Re-booking Prompt (7-day follow-up)
**Template Name:** `rebook_prompt`
**Category:** MARKETING

```
Hi {{1}}, it's been a week since your last visit at {{BUSINESS_NAME}}!

Ready to book your next {{2}}? Reply BOOK and we'll sort you out. 💈
```

Variables: `[customer_name, service]`

---

## TEMPLATE 6 — Owner Daily Summary
**Template Name:** `owner_daily_summary`
**Category:** UTILITY
**Send to:** Owner phone number only

```
Good morning, {{1}}! Here's your summary for today:

📅 Bookings: {{2}}
🔔 New leads: {{3}}
📌 Follow-ups due: {{4}}

View dashboard: {{5}}
```

Variables: `[owner_name, bookings_count, leads_count, followups_count, airtable_link]`

---

## TEMPLATE 7 — Fallback / Handoff
**Template Name:** `human_handoff`
**Category:** UTILITY

```
Hi {{1}}, thanks for your message!

I'm connecting you with {{OWNER_NAME}} directly — they'll be with you shortly.

For urgent matters, you can also call: {{OWNER_PHONE}}
```

Variables: `[customer_name]`

---

## TEMPLATE 8 — No-Show Follow-up
**Template Name:** `no_show_followup`
**Category:** UTILITY

```
Hi {{1}}, we missed you today at {{BUSINESS_NAME}}!

Would you like to rebook your {{2}} appointment? Reply REBOOK and we'll find you the next available slot.
```

Variables: `[customer_name, service]`

---

## Notes for Deployment
- Submit all MARKETING templates to Meta at least 48 hours before first use
- UTILITY templates are typically approved within a few hours
- Never modify template text after approval — submit a new one
- Keep variable count low — Meta rejects templates with >4 variables in some regions
