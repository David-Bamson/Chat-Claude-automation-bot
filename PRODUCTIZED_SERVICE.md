# WhatsApp Automation — Productized Service Blueprint
### Designed for repeatable, low-effort deployment across multiple clients

---

## THE CORE PRINCIPLE

**80% standardised. 20% customised.**

The system is a pre-built product that you sell repeatedly.
Each new client only requires you to change a small, defined set of variables — nothing more.

---

## WHAT IS STANDARDISED (Never Changes Per Client)

These components are built once and reused for every deployment:

| Component | What It Is | How It's Reused |
|-----------|-----------|-----------------|
| Make.com scenario | The entire automation logic | Import blueprint → update variables |
| AI prompt structure | Question flow, booking steps, handoff triggers | Only the business details change |
| WhatsApp templates | Pre-approved message formats | Same templates, different business name |
| Airtable base | CRM structure, fields, views | Duplicate from master template |
| Error handling | Fallback messages, retry logic, owner alerts | Identical across all clients |
| Reminder flow | 24h + 2h reminders, no-show follow-up | Same logic, same timing |
| Follow-up flow | Thank-you + rebook prompt | Same delays, same structure |
| Onboarding process | Step-by-step setup guide | Same 8-step checklist every time |

---

## WHAT IS CUSTOMISED (Client-Specific Variables Only)

Everything that changes lives in ONE file: `config/{business-name}.json`

```
CHANGE THESE PER CLIENT:
├── Business name, type, location
├── Services list + prices
├── Opening hours
├── Owner name + phone
├── WhatsApp Business number
├── Google Calendar ID
├── Airtable Base ID
└── AI persona tone (friendly / professional / casual)

THAT'S IT. NOTHING ELSE CHANGES.
```

---

## DEPLOYMENT TIME TARGET

| Task | Time |
|------|------|
| Collect client info (form) | 0 min (async — client fills it) |
| Fill config file | 5 min |
| Meta/WhatsApp API setup | 20 min |
| Clone Airtable base | 5 min |
| Import + configure Make.com | 10 min |
| Connect webhook | 5 min |
| Submit WhatsApp templates | 10 min |
| End-to-end testing | 15 min |
| Client handover | 10 min |
| **TOTAL** | **80 minutes** |

After 5 deployments, this drops to under 60 minutes.
After 20 deployments, under 45 minutes.

---

## PRODUCT TIERS (What You Sell)

### Tier 1 — Starter ($49 setup + $29/month)
**Best for:** Businesses with simple, single-service operations (e.g. barber, cleaner)

Includes:
- WhatsApp bot (instant replies + booking flow)
- Airtable CRM (lead capture)
- Google Calendar integration
- 24h + 2h reminders
- Basic AI responses
- Up to 500 conversations/month

**Your cost:** ~$10–12/month
**Your margin:** ~60%

---

### Tier 2 — Growth ($99 setup + $59/month)
**Best for:** Multi-service businesses with repeat customers (salon, bakery, vendor)

Everything in Starter, plus:
- Post-service thank-you messages
- 7-day rebook prompts
- Daily owner summary via WhatsApp
- Human handoff detection
- Up to 2,000 conversations/month

**Your cost:** ~$15–18/month
**Your margin:** ~70%

---

### Tier 3 — Pro ($149 setup + $99/month)
**Best for:** Busy businesses with staff, high volume, brand-conscious owners

Everything in Growth, plus:
- Custom AI persona (tone, name, language)
- 2 WhatsApp numbers (e.g. bookings + orders on separate lines)
- Weekly performance report
- Priority support
- Unlimited conversations

**Your cost:** ~$25–35/month
**Your margin:** ~70–75%

---

## MINIMAL MAINTENANCE MODEL

Once a client is deployed, your ongoing effort is:

| Task | Frequency | Time |
|------|-----------|------|
| Monitor Make.com for errors | Weekly | 5 min |
| Review AI reply quality (first month only) | Weekly | 10 min |
| Template resubmission if needed | Rare | 15 min |
| Client check-in | Monthly | 10 min |
| Upgrade/change requests | On demand | 15–30 min |

**Total ongoing time per client: ~30 min/month**

At 20 clients: ~10 hours/month maintenance
At 50 clients: ~25 hours/month (hire a VA at this point)

---

## REUSABILITY MATRIX

| System Part | Reusable? | Effort to Customise |
|-------------|-----------|---------------------|
| Make.com blueprint | Yes — import once | 10 min (update variables) |
| AI system prompt | Yes — template | 5 min (fill in business details) |
| WhatsApp templates | Yes — resubmit per account | 10 min |
| Airtable base | Yes — duplicate | 2 min |
| Webhook setup | Yes — same process | 20 min |
| Error handling | Yes — built into blueprint | 0 min |
| Config file | Per-client | 5 min |

---

## COST BREAKDOWN PER CLIENT

### Starter Tier
| Item | Cost |
|------|------|
| Make.com (Core plan, shared across clients) | ~$3/client |
| Claude Haiku API (500 conversations × ~0.003) | ~$1.50 |
| Airtable (Free tier per base) | $0 |
| WhatsApp Cloud API (first 1k/month free) | $0 |
| Google Calendar API | $0 |
| **Total** | **~$4.50/month** |

Charge $29/month → **gross margin ~85%**

### Growth Tier
| Item | Cost |
|------|------|
| Make.com | ~$5/client |
| Claude Haiku API (2,000 conversations) | ~$6 |
| Airtable | $0 |
| WhatsApp (over 1k free tier: ~500 paid at $0.005) | ~$2.50 |
| **Total** | **~$13.50/month** |

Charge $59/month → **gross margin ~77%**

---

## SCALING WITHOUT BREAKING

### 1–10 clients
- Manage manually from Make.com + Airtable
- One shared Make.com Team plan
- Track clients in a simple Airtable table

### 10–30 clients
- Create a master "Agency Dashboard" Airtable base to track all clients
- Standardise API keys in Make.com connections (one Claude key, rotate if needed)
- Use a naming convention: `{CLIENT_NAME}_SCENARIO`, `{CLIENT_NAME}_CRM`

### 30–100 clients
- Hire a VA to handle onboarding (onboarding guide is the SOP)
- Build a simple client intake form (Typeform → Make.com → auto-creates client record)
- Consider building a lightweight admin panel (Softr on top of Airtable)

### 100+ clients
- Move to a multi-tenant backend (Node.js + Supabase)
- Central webhook router: one URL, routes by phone number to correct client config
- Self-service client portal (client updates own hours, services without contacting you)
- Move WhatsApp infrastructure to a BSP (Business Solution Provider) for volume pricing

---

## SALES PROCESS (Productized)

### Step 1 — Lead Qualification (5 min call)
Ask:
- Do you use WhatsApp for customer enquiries?
- Do you miss messages or get overwhelmed?
- Do you currently do bookings manually?

If YES to 2/3 → they're a buyer.

### Step 2 — Demo (Show, Don't Tell)
Send them a WhatsApp message to a demo number.
Let them experience the bot as a customer.
They'll sell themselves.

### Step 3 — Close
Present the tier table.
For most local businesses: recommend Growth ($99 setup + $59/month).
Offer a 14-day money-back guarantee to reduce friction.

### Step 4 — Onboard
Send intake form. Schedule 90-min setup call.
Follow onboarding guide. Done.

---

## KEY CONSTRAINTS & GUARDRAILS

- **Never promise 100% automation** — some messages will always need the owner
- **Always set up human handoff** — customers expect a real person for complaints
- **WhatsApp templates must be approved before launch** — never promise same-day go-live
- **One phone number = one WhatsApp Business account** — clients must have a dedicated SIM
- **Make.com free plan won't work** — minimum Core plan ($9/month) required for webhooks

---

## GROWTH MOATS (Why Clients Stay)

1. **Switching cost** — all their customer data is in your Airtable setup
2. **Trained AI** — prompt is tuned to their business over time
3. **Results** — they start seeing bookings come in passively within days
4. **Low price** — $29–$99/month is trivial for a business making $3k–$30k/month
5. **Dependency** — they'll never want to go back to manual WhatsApp management
