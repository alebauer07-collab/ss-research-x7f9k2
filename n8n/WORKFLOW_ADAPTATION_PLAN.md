# n8n WhatsApp Workflow Adaptation Plan

**Source:** Manychat Chat Response Agent V3  
**Target:** Bambu Academy WhatsApp AI Agent  
**Date:** January 13, 2026

---

## 📊 Source Workflow Analysis

The existing workflow (`uEXIt8AfdrzJ9MvG`) is designed for Manychat/PartnerStack partner communication. Key architecture:

### Current Flow:
```
Webhook Trigger
    ↓
Parse Agent Input → Get Message History (MongoDB) → Get Partner Data
    ↓
#1 Chat Response Agent (OpenRouter - Claude) 
    ↓
KB Agent Routing (5 specialized agents)
    ↓
#2 Response Parser (Validation + Quality)
    ↓
Insert AI Message (MongoDB) → Update Chat Status
```

### Nodes to Keep (Adapt):
| Original Node | Bambu Adaptation |
|---------------|------------------|
| Webhook | WhatsApp Business Cloud Trigger |
| Parse agent input data | Parse WhatsApp message + Extract Applicant ID |
| Get message history (MongoDB) | Get conversation history from `messages` collection |
| Get partner (MongoDB) | Get applicant from `applicants` collection |
| #1 Chat Response Agent | Bambu Qualification Agent (new prompt) |
| #2 Response Parser | Response Validator (simplified) |
| Insert AI Message | Insert message to `messages` collection |
| Send message (WhatsApp) | WhatsApp Business Cloud Reply |

### Nodes to Remove:
- All PartnerStack HTTP requests
- All KB Agents (Product Info, Success Stories, Partner Support, Platform Guides, Partner Research)
- Data table nodes (ps token)
- Check if analysis exist (not needed for Bambu flow)

### Nodes to Add:
- **Slack Notification** - Alert team for qualified leads
- **Applicant ID Extractor** - Regex parse `BAMBU-2026-XXXXXX`
- **Stage Progression Logic** - Move applicants through qualification stages
- **Escalation Check** - Trigger human handoff when needed

---

## 🔄 Adapted Workflow Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    BAMBU ACADEMY WHATSAPP AI WORKFLOW                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  [1] WhatsApp Trigger ──────────────────────────────────────────────────►   │
│           │                                                                  │
│           ▼                                                                  │
│  [2] Parse Message & Extract ID ─────────────────────────────────────────►  │
│           │                                                                  │
│           ├──► [3a] Get Applicant (MongoDB)                                  │
│           │         │                                                        │
│           │         ▼                                                        │
│           └──► [3b] Get Chat History (MongoDB)                               │
│                     │                                                        │
│                     ▼                                                        │
│           [4] Build Context ─────────────────────────────────────────────►  │
│                     │                                                        │
│                     ▼                                                        │
│           [5] Bambu AI Agent (OpenRouter - Claude 3.5 Sonnet) ───────────►  │
│                     │                                                        │
│                     ▼                                                        │
│           [6] Response Parser & Validator ───────────────────────────────►  │
│                     │                                                        │
│                     ├──► [7a] Insert Message (MongoDB)                       │
│                     │                                                        │
│                     ├──► [7b] Update Applicant Stage (MongoDB)               │
│                     │                                                        │
│                     ├──► [7c] Send WhatsApp Reply                            │
│                     │                                                        │
│                     └──► [7d] Check Escalation ───► Slack Notification       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 📝 Key Prompt Adaptations

### Original Agent Prompt (Summary):
- Role: Manychat Chat Response Agent V2
- Purpose: Responding to Manychat partners/applicants
- Features: JSON-based routing to 5 KB agents, scenario detection, quality scoring

### Bambu Agent Prompt (New):
- Role: Bambu Academy Qualification Agent
- Purpose: Qualifying fitness coaching applicants via WhatsApp
- Features: Single-agent design (no KB routing needed), 5-stage qualification flow, Will's communication style

### Critical Prompt Changes:

1. **Identity Section:**
```
BEFORE: "You are Manychat Chat Response Agent V2..."
AFTER: "You are the Bambu Academy WhatsApp Qualification Agent..."
```

2. **Communication Style:**
- Remove: Corporate tone, technical partner support language
- Add: Will Henke's voice (direct, warm, confident, no corporate speak)

3. **Output Schema:**
- Keep: JSON structure with routing flags
- Adapt: Replace KB routing with stage progression
- Add: Escalation flags, qualification score updates

4. **Knowledge Base:**
- Remove: Product Info, Success Stories, Partner Support, Platform Guides
- Add: Academy curriculum, pricing ($4,299 early bird / $6,999 standard), career paths, program details

---

## 🔧 Node Configuration Changes

### Node: WhatsApp Trigger
```json
{
  "name": "WhatsApp Trigger",
  "type": "n8n-nodes-base.whatsAppTrigger",
  "typeVersion": 1,
  "position": [250, 300],
  "webhookId": "bambu-whatsapp-webhook",
  "parameters": {
    "updates": ["messages"]
  }
}
```

### Node: Parse Message & Extract ID
```javascript
// Extract applicant ID from message
const message = $json.messages?.[0]?.text?.body || '';
const idMatch = message.match(/BAMBU-2026-\d{6}/);
const applicantId = idMatch ? idMatch[0] : null;

// Extract phone number
const from = $json.messages?.[0]?.from || '';

return {
  applicantId,
  phone: from,
  messageBody: message,
  timestamp: new Date().toISOString()
};
```

### Node: MongoDB - Get Applicant
```json
{
  "collection": "applicants",
  "operation": "findOne",
  "query": {
    "applicantId": "={{ $json.applicantId }}"
  }
}
```

### Node: MongoDB - Get Chat History
```json
{
  "collection": "messages",
  "operation": "find",
  "query": {
    "applicantId": "={{ $json.applicantId }}"
  },
  "options": {
    "sort": { "timestamp": 1 },
    "limit": 20
  }
}
```

### Node: Slack Notification (Qualified Lead)
```json
{
  "name": "Notify Team - Qualified Lead",
  "type": "n8n-nodes-base.slack",
  "parameters": {
    "channel": "#all-team-friends",
    "text": "🎯 *New Qualified Lead!*\n\n*Name:* {{ $json.applicant.name }}\n*Score:* {{ $json.qualificationScore }}/100\n*Stage:* {{ $json.stage }}\n*ID:* {{ $json.applicantId }}\n\n<https://wa.me/{{ $json.phone }}|Open WhatsApp Chat>"
  }
}
```

---

## 📄 Files to Create

1. `bambu_whatsapp_workflow.json` - Full n8n workflow export
2. `agent_system_prompt.md` - Main AI agent prompt
3. `parser_prompt.md` - Response parser prompt
4. `mongodb_schemas.js` - Collection schemas

---

## ⚠️ Important Adaptations

### Remove from Original:
1. All PartnerStack API calls
2. KB Agent routing (5 agents) - simplify to single agent
3. Partner analysis workflow call
4. Data table lookups
5. Complex V2 routing architecture

### Add for Bambu:
1. Applicant ID extraction (BAMBU-2026-XXXXXX format)
2. 5-stage qualification flow logic
3. Will's communication style enforcement
4. Slack notifications for qualified leads
5. Escalation triggers for special cases
6. Updated MongoDB schema for applicants + messages

### Simplifications:
- Original: 2-agent architecture (#1 Agent + #2 Parser) with 5 KB sub-agents
- Bambu: 1 main agent + 1 lightweight parser (no KB routing needed)
- Original: Complex JSON routing with multiple scenarios
- Bambu: Simple stage-based progression

---

## 🚀 Implementation Order

1. **Phase 1:** Create MongoDB collections (done ✅)
2. **Phase 2:** Set up WhatsApp Business Cloud API credential
3. **Phase 3:** Build core workflow skeleton
4. **Phase 4:** Implement Bambu AI agent with new prompt
5. **Phase 5:** Add response parser and validation
6. **Phase 6:** Connect Slack notifications
7. **Phase 7:** Test end-to-end with simulated messages
8. **Phase 8:** Deploy and monitor

---

*This plan adapts the existing Manychat workflow architecture for Bambu Academy's specific needs while maintaining proven patterns for reliability.*

