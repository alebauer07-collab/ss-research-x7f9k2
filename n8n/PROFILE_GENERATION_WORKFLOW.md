# n8n Workflow: Applicant Profile Generation

**Purpose:** Generate AI analysis for applicants and store in MongoDB  
**Trigger:** Webhook from Academy Application page  
**Output:** JSON profile stored in MongoDB, accessible via profile page

---

## 📊 MONGODB SCHEMA

### Collection: `applicants`

```json
{
  "_id": "ObjectId",
  "applicant_id": "BAMBU-2026-XXXXXX",
  "status": "pending | researching | analyzed | qualified | declined",
  "source": "github_pages | webflow",
  
  "application": {
    "first_name": "string",
    "last_name": "string",
    "email": "string",
    "whatsapp": "string",
    "location": "string",
    "current_role": "string",
    "experience": "string",
    "why_apply": "string",
    "payment_preference": "string",
    "submitted_at": "ISO timestamp"
  },
  
  "ai_analysis": {
    "overall_score": 0-100,
    "tier": "STAR | STRONG | GOOD | MEDIUM",
    "scores": {
      "coaching_experience": 0-10,
      "technical_foundation": 0-10,
      "business_acumen": 0-10,
      "growth_mindset": 0-10,
      "financial_readiness": 0-10,
      "commitment_signal": 0-10
    },
    "insights": [
      "Insight 1",
      "Insight 2",
      "Insight 3"
    ],
    "recommendation": "string",
    "research_summary": "string",
    "analyzed_at": "ISO timestamp"
  },
  
  "research_data": {
    "instagram": {
      "handle": "string",
      "followers": 0,
      "posts": 0,
      "bio": "string",
      "is_coach": true/false
    },
    "linkedin": {
      "url": "string",
      "title": "string",
      "experience": []
    },
    "other_sources": []
  },
  
  "whatsapp_history": [
    {
      "direction": "inbound | outbound",
      "message": "string",
      "timestamp": "ISO timestamp"
    }
  ],
  
  "funnel_progress": {
    "step1_completed": true,
    "step2_completed": false,
    "step3_completed": false,
    "whatsapp_clicked_at": null,
    "profile_viewed_at": null
  },
  
  "created_at": "ISO timestamp",
  "updated_at": "ISO timestamp"
}
```

---

## 🔄 WORKFLOW: Application Submitted

### Trigger: Webhook
**URL:** `https://alexbauer.app.n8n.cloud/webhook/bambu-application-submitted`

**Expected Payload:**
```json
{
  "event": "application_submitted",
  "applicantId": "BAMBU-2026-XXXXXX",
  "formData": {
    "first_name": "...",
    "last_name": "...",
    "email": "...",
    "phone": "...",
    "location": "...",
    "role": "...",
    "experience": "...",
    "why_apply": "...",
    "payment_option": "..."
  }
}
```

### Workflow Steps:

```
1. [Webhook Trigger] → Receive application data

2. [Set Node] → Prepare MongoDB document
   - Map formData to application schema
   - Set status = "pending"
   - Set created_at = now()

3. [MongoDB Insert] → Insert into `applicants` collection
   - Database: bambu_academy
   - Collection: applicants
   - Document: prepared data

4. [AI Agent] → Research & Analyze applicant
   - Use OpenRouter (Claude/GPT)
   - Research Instagram, LinkedIn
   - Generate scores and insights
   - Output JSON analysis

5. [MongoDB Update] → Update applicant with AI analysis
   - Filter: { applicant_id: $applicantId }
   - Update: { $set: { ai_analysis: $analysis, status: "analyzed" } }

6. [Slack Notification] → Notify team (if qualified)
   - Channel: #bambu-leads
   - Message: New applicant + tier + score

7. [Respond to Webhook] → Return success
```

---

## 🔄 WORKFLOW: Get Profile

### Trigger: Webhook (GET)
**URL:** `https://alexbauer.app.n8n.cloud/webhook/bambu-get-profile?id=BAMBU-2026-XXXXXX`

### Workflow Steps:

```
1. [Webhook Trigger] → Receive applicant ID from query param

2. [MongoDB Find] → Query applicant
   - Database: bambu_academy
   - Collection: applicants
   - Filter: { applicant_id: $id }

3. [Code Node] → Format response
   - If found: return { application, ai_analysis }
   - If not found: return { error: "Not found" }

4. [Respond to Webhook] → Return JSON
```

---

## 🤖 AI AGENT PROMPT (for Research Node)

```markdown
# Bambu Academy Applicant Research Agent

You are researching a potential applicant for Bambu Academy, an elite fitness coaching certification program in Bali.

## APPLICANT DATA
Name: {{ $json.first_name }} {{ $json.last_name }}
Email: {{ $json.email }}
Location: {{ $json.location }}
Current Role: {{ $json.role }}
Experience: {{ $json.experience }}
Why They Want to Join: {{ $json.why_apply }}

## YOUR TASK

1. **Research** (if possible):
   - Search for their Instagram (fitness-related accounts)
   - Search for LinkedIn profile
   - Look for any coaching/fitness business presence

2. **Score** on these 6 dimensions (0-10 each):
   - **Coaching Experience (CE)**: Current coaching activity, client work, certifications
   - **Technical Foundation (TF)**: Understanding of training, nutrition, anatomy
   - **Business Acumen (BA)**: Entrepreneurial mindset, marketing awareness
   - **Growth Mindset (GM)**: Willingness to learn, coachability, ambition
   - **Financial Readiness (FR)**: Ability to invest $4,299-$6,999
   - **Commitment Signal (CS)**: Seriousness of application, detail in responses

3. **Generate Insights**:
   - 3-5 key observations about this applicant
   - What makes them a good/bad fit
   - Potential red flags or green flags

4. **Recommendation**:
   - One paragraph summary of whether to accept/interview/decline

## OUTPUT FORMAT (JSON)

{
  "overall_score": 75,
  "tier": "STRONG",
  "scores": {
    "coaching_experience": 7,
    "technical_foundation": 8,
    "business_acumen": 6,
    "growth_mindset": 9,
    "financial_readiness": 7,
    "commitment_signal": 8
  },
  "insights": [
    "Has been coaching for 3 years with solid client base",
    "Shows strong growth mindset in application response",
    "Location in USA means travel investment needed"
  ],
  "recommendation": "Strong candidate with solid foundation. The detailed application response shows genuine interest and growth mindset. Recommend scheduling a call to discuss program fit and travel logistics.",
  "research_summary": "Found Instagram @handle with 5K followers, posts about fitness coaching. LinkedIn shows 3 years as personal trainer at XYZ Gym."
}

## TIER CLASSIFICATION

- **STAR** (85-100): Exceptional candidate, fast-track
- **STRONG** (70-84): Great fit, priority interview
- **GOOD** (55-69): Potential fit, standard process
- **MEDIUM** (40-54): Needs more info, follow-up required
- **LOW** (<40): Not a fit, polite decline
```

---

## 🔄 WORKFLOW: WhatsApp Clicked

### Trigger: Webhook
**URL:** `https://alexbauer.app.n8n.cloud/webhook/bambu-whatsapp-clicked`

**Payload:**
```json
{
  "event": "whatsapp_clicked",
  "applicantId": "BAMBU-2026-XXXXXX"
}
```

### Workflow:
```
1. [Webhook Trigger]
2. [MongoDB Update] → Update funnel progress
   - Filter: { applicant_id: $applicantId }
   - Update: { $set: { "funnel_progress.step2_completed": true, "funnel_progress.whatsapp_clicked_at": now() } }
3. [Respond] → { success: true }
```

---

## 📋 WORKFLOW JSON (Import to n8n)

See `bambu_profile_workflow.json` for the complete importable workflow.

---

## 🔗 WEBHOOK URLS SUMMARY

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/webhook/bambu-application-submitted` | POST | New application |
| `/webhook/bambu-get-profile` | GET | Fetch profile by ID |
| `/webhook/bambu-whatsapp-clicked` | POST | Track WhatsApp click |
| `/webhook/bambu-step2-completed` | POST | Track Step 2 completion |

---

## 🧪 TESTING

1. Submit test application on GitHub Pages
2. Check MongoDB for new document
3. Wait ~5 minutes for AI analysis
4. Visit profile page with applicant ID
5. Verify analysis displays correctly

---

*Created by Cursor Agent — Bambu Academy Project*

