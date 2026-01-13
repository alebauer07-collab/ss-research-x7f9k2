# 🔍 Perplexity Comet Task: WhatsApp Integration Verification

## Task Overview
Verify and expand on the WhatsApp Business AI Agent integration research. Double-check solutions and provide implementation guidance.

## Assigned To
- **Primary Executor:** Perplexity Comet
- **Supervisor:** Alexey

---

## Research Questions

### 1. Meta WhatsApp Business API Policies (January 2026)
- What are the current Meta policies for AI chatbots on WhatsApp Business API?
- Are there any restrictions on external AI agents (OpenRouter, Claude, etc.)?
- What happened with the rumored January 2026 AI agent ban?
- What compliance requirements exist for fitness/coaching industry?

### 2. n8n + WhatsApp Business Cloud Integration
- Step-by-step setup guide for connecting n8n to WhatsApp Business Cloud API
- How to configure webhooks for incoming messages
- How to handle message templates vs. session messages
- Rate limits and best practices

### 3. Applicant ID System
- Best practices for generating unique applicant IDs
- WhatsApp deep link format: `https://wa.me/PHONE?text=PREFILLED_MESSAGE`
- How to include applicant ID in pre-filled WhatsApp message
- URL encoding requirements for special characters

### 4. AI Agent Architecture
- Can we use OpenRouter models with n8n for WhatsApp responses?
- How to maintain conversation context across messages?
- How to trigger external research (Firecrawl, Perplexity) from n8n?
- Memory/context window considerations for long conversations

### 5. MongoDB Integration
- Best practices for storing WhatsApp conversation history
- How to update applicant status in real-time
- Webhook patterns for n8n → MongoDB updates

### 6. Alternative Solutions
- Are there better alternatives to n8n + Meta Cloud API?
- What about Twilio vs. direct Meta API?
- Any no-code solutions worth considering (WATI, Respond.io)?
- Cost comparison for 100-500 conversations/month

---

## Expected Output Format

```markdown
## 1. Meta Policy Status
[Current policy details and compliance requirements]

## 2. n8n Setup Guide
[Step-by-step instructions with code snippets]

## 3. Applicant ID Implementation
[Technical specification with examples]

## 4. AI Agent Configuration
[Architecture recommendations with n8n node configurations]

## 5. MongoDB Schema Recommendations
[Optimized schema for our use case]

## 6. Alternative Analysis
[Pros/cons comparison table]

## 7. Implementation Timeline
[Estimated hours for each component]

## 8. Potential Issues & Mitigations
[Risk assessment and solutions]
```

---

## Context for Perplexity

### Our Setup:
- **n8n:** Self-hosted instance, already running other workflows
- **OpenRouter:** Budget available, prefer Claude models
- **MongoDB:** MCP access available
- **Slack:** Connected to #all-team-frieds channel
- **Use Case:** Fitness coaching academy applicant qualification
- **Volume:** ~100-500 conversations/month expected
- **WhatsApp:** Business number already registered on Will's phone

### What We've Already Decided:
- Using n8n as the orchestration layer
- Meta WhatsApp Business Cloud API (not Twilio)
- MongoDB for applicant storage
- Slack for team notifications
- OpenRouter for AI responses

### What We Need Verified:
- Is this architecture compliant with Meta policies?
- Are there any gotchas we're missing?
- What's the fastest path to MVP?

---

## Deliverables

1. **Verification Report** - Confirm or correct our research findings
2. **Setup Checklist** - Actionable steps for implementation
3. **Code Snippets** - n8n workflow JSON or node configurations
4. **Risk Assessment** - What could go wrong and how to prevent it

---

*Task Created: January 13, 2026*
*For: Bambu Academy WhatsApp AI Agent*

