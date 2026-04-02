# WhatsApp Cloud API — Webhook Setup Guide

## Prerequisites
- Meta Business Account (free)
- Facebook Developer Account (free)
- A phone number NOT currently registered on personal WhatsApp

---

## Step 1 — Create a Meta App

1. Go to developers.facebook.com → My Apps → Create App
2. Select **Business** type
3. Name the app: `{BUSINESS_NAME} Bot`
4. Add the **WhatsApp** product to the app

---

## Step 2 — Configure WhatsApp Business

1. In the app dashboard → WhatsApp → Getting Started
2. Add your phone number (verify via SMS/call)
3. Note down:
   - `PHONE_NUMBER_ID`
   - `WHATSAPP_BUSINESS_ACCOUNT_ID`
   - `ACCESS_TOKEN` (temporary — generate a permanent one below)

### Generate a Permanent Access Token
1. Meta Business Suite → Settings → System Users → Add System User
2. Assign Admin role
3. Generate token with scopes: `whatsapp_business_messaging`, `whatsapp_business_management`
4. Save this token — store in Make.com as a connection credential

---

## Step 3 — Configure Webhook in Make.com

1. In Make.com, create a new scenario
2. Add a **Webhooks** module → Custom Webhook
3. Copy the generated webhook URL
4. In Meta Developer App → WhatsApp → Configuration → Webhook:
   - Paste webhook URL
   - Set Verify Token: `whatsapp_verify_{BUSINESS_NAME_LOWERCASE}`
   - Subscribe to: `messages`
5. Click Verify and Save

---

## Step 4 — Verify Webhook Handshake

Make.com webhook module handles GET verification automatically.
If verification fails, ensure the scenario is **active** before clicking verify in Meta.

---

## Step 5 — Send a Test Message

1. In Meta Developer → WhatsApp → API Setup
2. Use the test number to send a message to your registered number
3. Check Make.com → Scenario History for the incoming payload

**Expected webhook payload structure:**
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "WHATSAPP_BUSINESS_ACCOUNT_ID",
    "changes": [{
      "value": {
        "messaging_product": "whatsapp",
        "metadata": {
          "display_phone_number": "PHONE_NUMBER",
          "phone_number_id": "PHONE_NUMBER_ID"
        },
        "contacts": [{
          "profile": { "name": "CUSTOMER_NAME" },
          "wa_id": "CUSTOMER_PHONE"
        }],
        "messages": [{
          "from": "CUSTOMER_PHONE",
          "id": "MESSAGE_ID",
          "timestamp": "UNIX_TIMESTAMP",
          "text": { "body": "MESSAGE_TEXT" },
          "type": "text"
        }]
      },
      "field": "messages"
    }]
  }]
}
```

---

## Step 6 — Send Outbound Message (Test)

Use this cURL to test outbound sending manually:

```bash
curl -X POST \
  https://graph.facebook.com/v18.0/{{PHONE_NUMBER_ID}}/messages \
  -H "Authorization: Bearer {{ACCESS_TOKEN}}" \
  -H "Content-Type: application/json" \
  -d '{
    "messaging_product": "whatsapp",
    "to": "{{CUSTOMER_PHONE}}",
    "type": "text",
    "text": { "body": "Hello! This is a test message from {{BUSINESS_NAME}}." }
  }'
```

---

## Rate Limits & Quotas

| Tier | Conversations/day | Notes |
|------|------------------|-------|
| Free | 1,000/month | First 1k business-initiated free |
| Paid | Unlimited | ~$0.005–0.08 per conversation (varies by country) |

Free tier is sufficient for most local businesses under 50 active customers/month.
