# ✅ WhatsApp Business AI Agent Integration — VERIFIED

**Research Date:** January 13, 2026  
**Source:** Perplexity Comet Deep Research  
**Status:** READY FOR IMPLEMENTATION

---

## 🚨 CRITICAL: Meta Policy Status

### Ban Confirmed — But We're COMPLIANT ✅

**Effective January 15, 2026**, Meta prohibits general-purpose AI chatbots on WhatsApp Business API.

| Category | Status | Our Use Case |
|----------|--------|--------------|
| General-purpose AI assistants (ChatGPT, Perplexity bots) | ❌ Banned | N/A |
| Open-ended conversation bots | ❌ Banned | N/A |
| **Customer support AI** | ✅ Allowed | ✅ Our use |
| **Lead qualification bots** | ✅ Allowed | ✅ Our use |
| Booking/appointment confirmations | ✅ Allowed | ✅ Our use |

**Why We're Compliant:**
- AI serves specific business function (applicant qualification)
- Conversations are business-led with defined outcomes
- AI role is ancillary to coaching service
- Human escalation paths included

---

## 💰 Pricing (July 2025 Model)

| Message Type | Cost |
|--------------|------|
| **Service conversations (user-initiated)** | **FREE** ✅ |
| Marketing templates | $0.05-0.15/msg |
| Utility templates | $0.02-0.08/msg |

**Key Insight:** Since applicants initiate via our deep link, ALL qualification conversations are **FREE service conversations**.

---

## 📊 Rate Limits

| Business Status | Daily Limit |
|-----------------|-------------|
| Unverified | 250 conversations |
| Verified (new) | 1,000 → 2,000 |
| Established | 10,000 → 100,000 → Unlimited |

**API Rate:** 80 messages/second (Cloud API default)

For 100-500 conversations/month: **Well under limits** ✅

---

## 🔗 WhatsApp Deep Link Format

```
https://wa.me/{PHONE_NUMBER}?text={URL_ENCODED_MESSAGE}
```

### Implementation (Already Done in V2):
```javascript
const applicantId = generateApplicantId(); // BAMBU-2026-XXXXXX
const message = `Hey Bambu team! I just submitted my Academy application.

My Application ID: ${applicantId}
Name: ${firstName} ${lastName}
Email: ${email}

Excited to chat!`;
const deepLink = `https://wa.me/6281244997721?text=${encodeURIComponent(message)}`;
```

---

## 🏗️ Architecture (Confirmed)

```
┌──────────────────────────────────────────────────────────────────────────┐
│                        BAMBU WHATSAPP AI AGENT                           │
├──────────────────────────────────────────────────────────────────────────┤
│  WhatsApp Trigger → Extract ID → MongoDB Lookup → OpenRouter AI → Reply  │
│         ↓                ↓              ↓              ↓           ↓     │
│  Incoming Msg    Regex Parse    Applicant Data    Claude 3.5   WhatsApp  │
│                                                                Response  │
│                          ↓                                               │
│                    Slack Notification (#all-team-frieds)                 │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## 📋 Setup Checklist

### Phase 1: Infrastructure (Days 1-3)
- [ ] Verify business in Meta Business Manager
- [ ] Create Meta Developer app (WhatsApp type)
- [ ] Decide: Will's existing number OR Twilio virtual number
- [ ] Register phone number with WhatsApp API
- [ ] Generate permanent access token via System User

### Phase 2: n8n Core Workflow (Days 3-5)
- [ ] Configure WhatsApp Business Cloud credential
- [ ] Create WhatsApp Trigger node
- [ ] Build applicant ID extraction function
- [ ] Connect MongoDB for applicant lookup/upsert
- [ ] Build context builder for AI

### Phase 3: AI Integration (Days 5-7)
- [ ] Configure OpenRouter credential (custom base URL)
- [ ] Build AI request node with system prompt
- [ ] Add response parsing + WhatsApp reply
- [ ] Implement stage progression logic

### Phase 4: Notifications (Days 7-8)
- [ ] Slack notification node
- [ ] Human escalation triggers
- [ ] Error handling + retries

### Phase 5: Testing (Days 8-10)
- [ ] End-to-end flow testing
- [ ] Edge case handling
- [ ] Prompt refinement

**Total MVP:** ~31.5 hours + verification wait

---

## ⚠️ Phone Number Decision Required

**Issue:** Will's WhatsApp Business number cannot be used with Cloud API if already registered on WhatsApp app.

**Options:**
1. **Deregister from WhatsApp app** — Loses chat history
2. **Get Twilio virtual number** — ~$1-2/month, recommended

**Recommendation:** Use Twilio virtual number for API, keep Will's number for manual chats.

---

## 🔐 Token Generation Steps

1. Go to business.facebook.com → Business Settings
2. Create System User with Admin role
3. Assign Assets → Select WhatsApp app → Full Control
4. Generate Token:
   - Expiration: **Never**
   - Permissions: WhatsApp Business Management + Messaging
5. **COPY IMMEDIATELY** — shown only once!

---

## 📊 MongoDB Schema (Verified)

```javascript
{
  "_id": ObjectId,
  "applicantId": "BAMBU-2026-XXXXXX",
  "whatsappNumber": "+6281234567890",
  "email": "applicant@email.com",
  "status": "qualifying", // pending | qualifying | qualified | scheduled | enrolled
  "stage": 1, // 1-5 conversation stages
  "qualificationScore": 0-100,
  "responses": {
    "fitnessGoals": "...",
    "currentLevel": "...",
    "weeklyAvailability": "...",
    "budgetConfirmed": false
  },
  "conversationHistory": [
    { "role": "user", "content": "...", "timestamp": ISODate }
  ],
  "research": {
    "instagram": { "handle": "@...", "followers": 0 }
  },
  "createdAt": ISODate,
  "updatedAt": ISODate
}
```

---

## 📚 Sources

- Meta WhatsApp Business Platform documentation
- n8n WhatsApp Business Cloud integration docs
- OpenRouter API documentation
- MongoDB chat schema best practices
- Industry compliance analysis

---

*Research verified by Perplexity Comet — January 13, 2026*

