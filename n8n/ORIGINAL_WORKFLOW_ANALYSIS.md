# Original n8n Workflow Analysis

**Workflow:** Manychat Chat Response Agent V3  
**ID:** `uEXIt8AfdrzJ9MvG`  
**Analysis Date:** January 13, 2026

---

## 📊 Workflow Overview

The original workflow is a sophisticated chat response system for Manychat/PartnerStack partner communications. It features:

- **2-Agent Architecture:** Main response agent + Response parser/validator
- **5 KB Sub-Agents:** Specialized knowledge base agents for different topics
- **MongoDB Integration:** Chat history, partner data, application data
- **HTTP Integrations:** PartnerStack API for partner management
- **Routing System:** JSON-based routing to appropriate KB agents

---

## 🔗 Node Inventory

### Trigger Nodes
| Node | Type | Purpose |
|------|------|---------|
| Webhook | Webhook Trigger | External trigger |
| When Executed by Another Workflow | Execute Workflow Trigger | Internal call |

### Core Processing
| Node | Type | Purpose |
|------|------|---------|
| Parse agent input data | Set | Extract message data |
| Aggregate messages | Aggregate | Combine messages |
| #1 Chat Response Agent1 | AI Agent | Generate responses |
| #2 Response Parser1 | AI Agent | Validate & parse |

### MongoDB Operations
| Node | Collection | Operation |
|------|------------|-----------|
| Get message history | messages | find |
| Get application | applications | findOne |
| Get partner | partners | findOne |
| Get chat | chats | findOne |
| Insert AI Message1 | messages | insertOne |
| Update Chat Status1 | chats | updateOne |
| Insert applicant to Mongo | applicants | insertOne |
| Insert applicant to Mongo2 | messages | insertMany |

### KB Agents (5)
| Agent | Purpose | Model |
|-------|---------|-------|
| 1_KB_Product_Info2 | Product knowledge | OpenRouter |
| 2_KB_Success_Stories1 | Success stories | OpenRouter |
| 3_KB_Partner_Support1 | Partner support | OpenRouter |
| 4_KB_Platform_Guides1 | Platform guides | OpenRouter |
| 5_KB_Partner_Research | Partner research | OpenRouter + Firecrawl |

### Routing Logic
| Node | Condition |
|------|-----------|
| Direct | Route to direct response |
| Product info | Route to product KB |
| Success stories | Route to stories KB |
| Partner Support | Route to support KB |
| Platform Guides | Route to guides KB |
| Partner Research | Route to research KB |

### External APIs
| Node | Service | Purpose |
|------|---------|---------|
| Get ps token | Data Table | PartnerStack auth |
| Read1 | HTTP Request | Mark messages read |
| Get stats | HTTP Request | Partner statistics |
| Get chat from PS | HTTP Request | Fetch chat data |
| Get messages | HTTP Request | Fetch messages |
| Send message | WhatsApp | Send response |

---

## 🧠 Main Agent Prompt Structure

### Role Definition
```
Role: Manychat Chat Response Agent V2
Description: Generating responses for Manychat partners/applicants
Architecture: V2 with JSON-based routing
```

### Key Features
1. **Scenario Detection:** Inbound message vs Proactive outreach
2. **Routing Flags:** Direct response or KB agent routing
3. **Quality Scoring:** Response quality metrics
4. **Personalization:** Based on partner profile

### Output Schema
```json
{
  "routing": {
    "route_to_kb": true/false,
    "kb_agent": "product_info|success_stories|etc",
    "direct_response": true/false
  },
  "draft_response": "...",
  "ai_metadata": {
    "intent": "...",
    "sentiment": "...",
    "confidence": 0.0-1.0
  },
  "personalization": { ... },
  "debug_notes": "..."
}
```

---

## 🔄 Response Parser Structure

### Validation Pipeline
1. Scenario detection
2. JSON parsing
3. KB agent response merging
4. Structure validation
5. Quality validation
6. Score generation
7. Metadata building

### Output Schema
```json
{
  "final_message": "...",
  "validation_status": "valid|invalid",
  "quality_score": 0-100,
  "issues_found": [],
  "quality_analysis": { ... },
  "manager_notes": "..."
}
```

---

## 🎯 Adaptation Strategy for Bambu

### What to Keep
1. **2-Agent Architecture** — Main agent + Parser pattern works well
2. **MongoDB Integration** — Same pattern, different collections
3. **JSON Output Schema** — Adapt for qualification stages
4. **Quality Scoring** — Apply to response quality
5. **WhatsApp Send Node** — Direct reuse

### What to Remove
1. **All 5 KB Agents** — Not needed for single-purpose qualification
2. **PartnerStack Integration** — Replace with internal application data
3. **Complex Routing** — Simplify to stage-based flow
4. **Data Table Lookups** — Not needed
5. **External Workflow Calls** — Simplify

### What to Add
1. **Applicant ID Extraction** — BAMBU-2026-XXXXXX parsing
2. **Stage Progression** — 5-stage qualification flow
3. **Slack Notifications** — Team alerts for qualified leads
4. **Escalation Logic** — Human handoff triggers
5. **Will's Voice Enforcement** — Style validation in parser

### Simplification Summary
| Original | Bambu Adaptation |
|----------|------------------|
| 2 main agents + 5 KB agents | 1 main agent + 1 parser |
| Complex JSON routing | Simple stage progression |
| Multiple external APIs | MongoDB + WhatsApp only |
| ~35 nodes | ~15 nodes |
| Partner/applicant dual flow | Applicant only |

---

## 📋 Node-by-Node Mapping

| Original Node | Bambu Equivalent | Changes |
|---------------|------------------|---------|
| Webhook | WhatsApp Trigger | Different trigger type |
| Parse agent input data | Parse Message & Extract ID | Add ID extraction |
| Get message history | Get Chat History | Same pattern |
| Get partner | Get Applicant | Different collection |
| #1 Chat Response Agent | Bambu AI Agent | New prompt |
| #2 Response Parser | Parse AI Response | Simplified |
| Insert AI Message1 | Insert AI Message | Same pattern |
| Update Chat Status1 | Update Applicant | Different fields |
| Send message | Send WhatsApp Reply | Same |
| (none) | Slack Notifications | New |
| (none) | Check Escalation | New |

---

## ⚙️ Configuration Migration

### MongoDB Collections
```
Original → Bambu
partners → applicants
messages → messages (same)
chats → (embedded in applicants)
applications → (merged into applicants)
```

### Environment Variables Needed
```
MONGODB_URI=mongodb+srv://...
WHATSAPP_PHONE_NUMBER_ID=...
WHATSAPP_ACCESS_TOKEN=...
OPENROUTER_API_KEY=...
SLACK_BOT_TOKEN=...
```

### Credential IDs to Configure
1. `MONGO_CREDENTIAL_ID` — MongoDB connection
2. `OPENROUTER_CREDENTIAL_ID` — OpenRouter API
3. `WHATSAPP_CREDENTIAL_ID` — WhatsApp Business Cloud
4. `SLACK_CREDENTIAL_ID` — Slack Bot

---

## 🚀 Implementation Recommendations

### Phase 1: Core Flow (Day 1-2)
1. Import `bambu_whatsapp_workflow.json` to n8n
2. Configure MongoDB credential
3. Configure OpenRouter credential
4. Test message parsing with manual trigger

### Phase 2: WhatsApp Integration (Day 3-4)
1. Set up WhatsApp Business Cloud API
2. Configure webhook URL in Meta Developer
3. Test incoming message trigger
4. Test outgoing message send

### Phase 3: AI Agent Tuning (Day 5-6)
1. Load `agent_system_prompt.md` into agent node
2. Test with simulated conversations
3. Refine prompt based on outputs
4. Add stage progression logic

### Phase 4: Notifications (Day 7)
1. Configure Slack credential
2. Set up #all-team-friends channel
3. Test escalation alerts
4. Test qualified lead notifications

### Phase 5: Production (Day 8-10)
1. End-to-end testing
2. Edge case handling
3. Monitoring setup
4. Go live

---

## 📝 Additional Notes

### Original Workflow Strengths
- Robust error handling
- Comprehensive logging
- Flexible routing system
- Quality scoring

### Adaptations to Maintain Quality
- Keep response quality validation
- Maintain conversation history depth
- Preserve error fallback patterns
- Add Bambu-specific escalation triggers

---

*Analysis by Cursor Agent — Bambu Academy Project*

