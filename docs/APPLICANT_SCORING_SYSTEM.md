# Bambu Academy Applicant Scoring System v1.0

## Overview
This document defines the scoring methodology for evaluating Bambu Training Academy applicants. The system uses 6 dimensions scored 0-100, designed to identify high-potential coaches who will succeed in the program and represent Bambu's brand.

---

## 1. Scoring Dimensions (6 Total)

### 1.1 Coaching Experience (CE_Score) — 0-100
**What it measures:** Current coaching role, years of experience, client volume, coaching diversity

**Signals:**
- Current job title (Trainer, Coach, Head Coach, Gym Owner)
- Years actively coaching
- Client base size (1:1 clients, group classes, online clients)
- Coaching domains (CrossFit, Olympic Lifting, HYROX, General Fitness, etc.)
- Certifications held (CrossFit L1/L2, Burgener, NASM, etc.)

**Scoring Formula:**
```
Title_Norm: Gym Owner/Head Coach=1.0, Senior Coach=0.8, Coach/Trainer=0.6, Assistant/New=0.4
Years_Norm: 0-1yr=0.2, 1-3yr=0.4, 3-5yr=0.6, 5-10yr=0.8, 10+yr=1.0
ClientBase_Norm: <10=0.3, 10-30=0.5, 30-50=0.7, 50-100=0.85, 100+=1.0
Diversity_Norm: 1 domain=0.4, 2 domains=0.6, 3+=0.8

CE_Score = round(100 × (0.30×Title + 0.25×Years + 0.25×ClientBase + 0.20×Diversity))
```

### 1.2 Technical Foundation (TF_Score) — 0-100
**What it measures:** Movement knowledge, biomechanics understanding, programming ability

**Signals:**
- Formal certifications (CrossFit, USAW, Burgener, NSCA, etc.)
- Educational background (Kinesiology, Exercise Science, Sports Science)
- Competition experience as athlete
- Programming knowledge evidence
- Movement quality (if video available)

**Scoring Formula:**
```
Certs_Norm: 0 certs=0.2, 1-2=0.4, 3-4=0.6, 5+=0.8, Elite certs (Burgener/USAW Advanced)=1.0
Education_Norm: None=0.2, Related field=0.6, Degree in Ex Sci/Kinesiology=1.0
Athlete_Norm: No competition=0.3, Local=0.5, Regional=0.7, National+=0.9
Programming_Norm: Evidence of writing programs=0.3-1.0 based on sophistication

TF_Score = round(100 × (0.35×Certs + 0.25×Education + 0.20×Athlete + 0.20×Programming))
```

### 1.3 Business Acumen (BA_Score) — 0-100
**What it measures:** Entrepreneurial mindset, business ownership, marketing awareness

**Signals:**
- Business ownership (Gym owner, Online coaching business, PT Academy)
- Social media presence and follower count
- Evidence of marketing/branding efforts
- Pricing/packaging sophistication
- Revenue signals (if discoverable)

**Scoring Formula:**
```
Ownership_Norm: Employee=0.3, Independent contractor=0.5, Business owner=0.8, Multi-location/Academy=1.0
Social_Norm: <500 followers=0.2, 500-2K=0.4, 2K-10K=0.6, 10K-50K=0.8, 50K+=1.0
Marketing_Norm: None visible=0.2, Basic presence=0.5, Active content=0.7, Professional brand=1.0
Revenue_Norm: Part-time=0.3, Full-time coach=0.6, Multiple revenue streams=0.8, Scaled business=1.0

BA_Score = round(100 × (0.35×Ownership + 0.25×Social + 0.20×Marketing + 0.20×Revenue))
```

### 1.4 Growth Mindset (GM_Score) — 0-100
**What it measures:** Learning orientation, self-improvement commitment, coachability

**Signals:**
- Previous education investments (courses, certifications, workshops)
- Content consumption (follows industry leaders, engages with educational content)
- Self-awareness in application (acknowledges gaps, seeks growth)
- Responsiveness and engagement quality
- Humility signals vs. ego signals

**Scoring Formula:**
```
Education_Investment: No history=0.2, Some courses=0.5, Regular investment=0.8, Heavy investor=1.0
Engagement_Quality: Generic comments=0.3, Thoughtful engagement=0.6, Asks questions=0.8, Seeks mentorship=1.0
Self_Awareness: Defensive/ego=0.2, Neutral=0.5, Acknowledges gaps=0.8, Actively seeks growth=1.0
Coachability: Unknown=0.5, Evidence of taking feedback=0.8, Demonstrated behavior change=1.0

GM_Score = round(100 × (0.30×Education_Investment + 0.30×Engagement_Quality + 0.25×Self_Awareness + 0.15×Coachability))
```

### 1.5 Financial Readiness (FR_Score) — 0-100
**What it measures:** Ability to invest $6,999, payment stability

**Signals:**
- Business ownership (proxy for income stability)
- Location/market (high-cost vs. low-cost regions)
- Employment stability signals
- Explicit statements about budget/payment plan needs
- Social proof of lifestyle (travel, equipment, gym membership)

**Scoring Formula:**
```
Income_Proxy: Part-time/unemployed=0.3, Employee=0.5, Senior role=0.7, Business owner=0.9, Multi-business=1.0
Market_Tier: Developing market=0.5, Mid-tier market=0.7, High-income market (US/UK/AU/EU)=1.0
Payment_Signal: Needs payment plan=0.5, Flexible=0.7, Can pay in full=1.0
Stability: Unclear=0.5, Stable employment=0.7, Established business=1.0

FR_Score = round(100 × (0.35×Income_Proxy + 0.25×Market_Tier + 0.25×Payment_Signal + 0.15×Stability))
```

### 1.6 Commitment Signal (CS_Score) — 0-100
**What it measures:** Explicit interest level, application quality, availability

**Signals:**
- Application completeness and quality
- Explicit buying intent signals ("Where do I sign up?", "Book a call")
- Response speed and engagement
- Availability for March cohort
- Referral source (organic vs. cold)

**Scoring Formula:**
```
Intent_Signal: 
  - Explicit ask to sign up = 1.0
  - Asks to book a call = 0.9
  - Asks detailed questions = 0.7
  - Positive engagement (emoji/comment) = 0.5
  - Passive follower = 0.3

Application_Quality: Incomplete=0.3, Basic=0.5, Detailed=0.7, Exceptional=1.0
Response_Speed: No response=0.2, Slow (>48h)=0.4, Normal (24-48h)=0.6, Fast (<24h)=0.8, Immediate=1.0
Availability: Unknown=0.5, Maybe March=0.7, Confirmed March=1.0
Source: Cold outreach=0.4, Warm (engaged with content)=0.7, Hot (explicit interest)=1.0

CS_Score = round(100 × (0.35×Intent_Signal + 0.20×Application_Quality + 0.15×Response_Speed + 0.15×Availability + 0.15×Source))
```

---

## 2. Overall Applicant Score

### 2.1 Weighted Composite Score
```
Overall_Score = round(
  0.20 × CE_Score +      // Coaching Experience
  0.15 × TF_Score +      // Technical Foundation
  0.15 × BA_Score +      // Business Acumen
  0.15 × GM_Score +      // Growth Mindset
  0.15 × FR_Score +      // Financial Readiness
  0.20 × CS_Score        // Commitment Signal
)
```

### 2.2 Tier Classification
| Tier | Score Range | Action |
|------|-------------|--------|
| **TIER 1 - HOT** | 75-100 | Immediate call scheduling |
| **TIER 2 - WARM** | 55-74 | Nurture sequence, follow-up DM |
| **TIER 3 - COOL** | 35-54 | Add to email list, content nurture |
| **TIER 4 - COLD** | 0-34 | Not a fit for this cohort |

### 2.3 Red Flags (Automatic Penalties)
- **Ego/Arrogance signals**: -15 points
- **Negative online presence**: -20 points
- **Competing program affiliation**: -10 points (evaluate case-by-case)
- **Inconsistent information**: -10 points
- **No response after 2 follow-ups**: -25 points

### 2.4 Green Flags (Bonuses)
- **Gym owner with staff to train**: +10 points
- **Runs own coaching academy**: +15 points
- **Burgener Strength connection**: +10 points
- **Personal referral from Will/Carrie**: +20 points
- **CrossFit Games athlete**: +10 points
- **Large social following (50K+)**: +5 points

---

## 3. Data Collection Requirements

### 3.1 From Application Form
- Full name
- Email
- WhatsApp number
- Current coaching role
- Years of experience
- Primary coaching domain
- What they want to improve
- How they heard about Academy

### 3.2 From Research (MCP Tools)
- Instagram profile metrics
- LinkedIn career history
- Business/gym website
- Certifications visible online
- Content engagement patterns
- Geographic location
- Estimated business scale

### 3.3 From WhatsApp Conversation
- Response quality and speed
- Questions asked
- Payment plan discussion
- Availability confirmation
- Enthusiasm level
- Red/green flag signals

---

## 4. Score Visualization

### 4.1 Radar Chart (6 axes)
```
        CE (Coaching Experience)
              ▲
             /|\
            / | \
    GM ◄───●──┼──●───► TF
   (Growth)   |    (Technical)
            \ | /
             \|/
              ▼
        CS (Commitment)
       /           \
      /             \
   BA ◄─────────────► FR
(Business)      (Financial)
```

### 4.2 Score Card Display
```
┌─────────────────────────────────────┐
│ BEN CRAWSHAW            TIER 1 HOT │
│ @ben_crawsh                        │
├─────────────────────────────────────┤
│ Overall: 87/100  ████████████░░ 87%│
├─────────────────────────────────────┤
│ CE: 82 ████████░░  Coaching Exp    │
│ TF: 78 ███████░░░  Technical       │
│ BA: 95 █████████░  Business        │
│ GM: 85 ████████░░  Growth Mindset  │
│ FR: 88 ████████░░  Financial       │
│ CS: 92 █████████░  Commitment      │
├─────────────────────────────────────┤
│ 🏢 BC FITNESS HQ (Owner)           │
│ 📍 Carlisle, UK                    │
│ 💬 "where do I sign up?😍"         │
└─────────────────────────────────────┘
```

---

## 5. Integration Points

### 5.1 Application Flow
1. Form submission → Create applicant record
2. AI Agent research → Enrich profile data
3. Scoring engine → Calculate 6 scores
4. Tier assignment → Route to appropriate workflow
5. Dashboard update → Display in CRM

### 5.2 WhatsApp Integration
- Pre-call: AI agent handles initial qualification
- During call: Real-time score updates based on conversation
- Post-call: Final score locked, notes added

### 5.3 Dashboard Features
- Sortable by any score dimension
- Filter by tier
- Search by name/location
- Export to CSV
- WhatsApp chat history modal

---

## 6. Example Scores

### Ben Crawshaw (TIER 1)
```json
{
  "name": "Ben Crawshaw",
  "handle": "@ben_crawsh",
  "scores": {
    "coaching_experience": 82,
    "technical_foundation": 78,
    "business_acumen": 95,
    "growth_mindset": 85,
    "financial_readiness": 88,
    "commitment_signal": 92
  },
  "overall": 87,
  "tier": 1,
  "signals": {
    "positive": ["Owns PT Academy", "Explicit signup request", "2,989 followers", "UK market"],
    "negative": []
  }
}
```

### Neridah Lock (TIER 1)
```json
{
  "name": "Neridah Lock",
  "handle": "@foulmouthedtattooedgirl",
  "scores": {
    "coaching_experience": 75,
    "technical_foundation": 72,
    "business_acumen": 88,
    "growth_mindset": 90,
    "financial_readiness": 78,
    "commitment_signal": 95
  },
  "overall": 83,
  "tier": 1,
  "signals": {
    "positive": ["Building gym", "Requested call explicitly", "HYROX community", "Australia market"],
    "negative": []
  }
}
```

---

*Version 1.0 — January 2026*
*Bambu Training Academy*

