# Troubleshooting Guide

---

## Bot Not Responding to Messages

**Check 1:** Is the Make.com scenario active?
- Go to Make.com → Scenarios → confirm toggle is ON

**Check 2:** Is the webhook connected?
- Meta Developer → WhatsApp → Configuration → Webhook should show "Connected"
- If not, re-verify using the webhook URL from Make.com

**Check 3:** Is the Access Token expired?
- Temporary tokens expire after 24 hours
- Always use a System User permanent token (see webhook-setup.md Step 2)

**Check 4:** Check Make.com scenario history
- Go to Scenario → History tab → look for red (failed) executions
- Click the failed run to see which module failed and why

---

## Bot Sends Duplicate Messages

**Cause:** Webhook fires twice (Meta occasionally sends duplicate events)

**Fix:** Add a deduplication filter in Make.com:
1. After the webhook trigger, add a Filter
2. Condition: `message_id` NOT IN Make.com Data Store (last 100 IDs)
3. If passes: proceed + store message_id
4. If fails: stop scenario (duplicate)

---

## Airtable Records Not Being Created

**Check 1:** Verify Airtable connection is authenticated in Make.com
**Check 2:** Confirm Base ID matches exactly (from Airtable URL)
**Check 3:** Confirm table name is exactly `Leads` (case-sensitive)
**Check 4:** Check Airtable field names match what Make.com is sending

---

## Google Calendar Events Not Creating

**Check 1:** Re-authenticate Google Calendar in Make.com
- Connections → Google Calendar → Reauthorise
**Check 2:** Confirm the correct calendar is selected (not a shared/read-only calendar)
**Check 3:** Check the date/time format — Google Calendar requires ISO 8601: `2024-03-15T14:30:00+01:00`

---

## AI Replies Are Off-Brand or Inaccurate

**Fix:** Review and update the system prompt in `ai-prompts/system-prompt-template.md`
- Be more specific about services and pricing
- Add examples of ideal replies in the prompt
- Reduce temperature in Claude API call (set to 0.3–0.5 for more consistent replies)

---

## Customer Didn't Receive Reminder

**Check 1:** Is the reminder scenario scheduled and active in Make.com?
**Check 2:** Is the appointment date/time correctly stored in Airtable?
**Check 3:** Has the WhatsApp reminder template been approved by Meta?
- Unnapproved templates fail silently — check Meta Business Manager → Templates

---

## Make.com Scenario Keeps Failing

1. Open the failed execution in Make.com History
2. Click the red module to see the error
3. Most common errors:

| Error | Fix |
|-------|-----|
| `401 Unauthorized` | Re-authenticate the connection |
| `429 Too Many Requests` | Add a rate limiter or increase interval |
| `422 Unprocessable Entity` | Check field names and data types |
| `Connection timeout` | Retry logic — add error handler with sleep + retry |

---

## Owner Not Receiving Daily Summary

**Check 1:** Is the daily summary scenario scheduled? (Make.com scheduler → every day at 8am)
**Check 2:** Is the owner's phone number in international format? (`+447...` not `07...`)
**Check 3:** Has the `owner_daily_summary` template been approved by Meta?

---

## Client Wants to Add a New Service

1. Update `config/{business-name}.json` → add to `services` array
2. Update the system prompt in Make.com → edit the `SERVICES_LIST` text
3. No other changes needed — the AI will automatically handle the new service

---

## Client Wants to Change Opening Hours

1. Update `config/{business-name}.json` → `hours` object
2. Update the system prompt in Make.com → edit the `HOURS_SUMMARY` text
3. If using automated slot-checking: update the hour constraints in the calendar check module
