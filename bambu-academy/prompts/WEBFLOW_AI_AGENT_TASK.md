# Webflow AI Agent Task — Perplexity Comet

**Assigned To:** Perplexity Comet (Browser Automation Agent)  
**Assigned By:** Cursor Agent (Bambu Project)  
**Date:** January 13, 2026

---

## 🎯 MISSION

You are tasked with exploring and documenting the Bambu Fitness Webflow site to help us understand its structure and identify what changes need to be made for the Academy launch.

---

## 📋 TASKS

### Task 1: Webflow Site Audit

**Objective:** Document the current Bambu Webflow site structure

**Steps:**
1. Log into Webflow using credentials provided
2. Navigate to the Bambu Fitness project
3. Document the following:

```json
{
  "siteStructure": {
    "pages": [
      {
        "name": "Page Name",
        "slug": "/page-slug",
        "status": "published/draft",
        "lastUpdated": "date",
        "type": "static/CMS/collection"
      }
    ],
    "collections": [
      {
        "name": "Collection Name",
        "itemCount": 0,
        "fields": ["field1", "field2"]
      }
    ],
    "cmsItems": {
      "coaches": [],
      "programs": [],
      "testimonials": []
    }
  }
}
```

4. Take screenshots of:
   - Site dashboard/overview
   - Page list
   - Current Academy page (if exists)
   - CMS collections
   - Site settings

---

### Task 2: Academy Page Analysis

**Objective:** Analyze the current Academy page in Webflow

**Find and document:**
1. Current Academy page URL and status
2. Section breakdown (hero, content blocks, CTAs)
3. Forms and their configurations
4. Integrations (email, analytics, etc.)
5. Custom code blocks if any

**Output format:**
```json
{
  "academyPage": {
    "url": "https://bambufitnessbali.com/academy",
    "webflowUrl": "Webflow editor URL",
    "status": "published/draft",
    "sections": [
      {
        "name": "Hero",
        "components": ["heading", "image", "cta-button"],
        "customCode": false
      }
    ],
    "forms": [
      {
        "name": "Application Form",
        "fields": [],
        "action": "webflow/zapier/custom"
      }
    ],
    "integrations": []
  }
}
```

---

### Task 3: Required Changes Identification

**Objective:** List what needs to change for Academy V2

Based on our new funnel, identify:

1. **Content Updates Needed:**
   - Pricing: Early Bird $4,299 / Standard $6,999
   - Remove monthly payment option from display
   - Update career ladder section
   - Add 4-week curriculum details

2. **Form Updates:**
   - Add applicant ID generation
   - Add WhatsApp redirect after submission
   - Update form fields to match our schema

3. **New Sections Needed:**
   - "Who Is This For" section
   - "The Process" (4 steps visual)
   - Payment options cards
   - Career ladder timeline
   - Skills/outcomes grid

4. **Technical Changes:**
   - WhatsApp deep link integration
   - Form submission handler
   - Application ID system

---

### Task 4: Integration Check

**Objective:** Document current integrations and recommend additions

**Check for:**
1. Current form handlers (native Webflow, Zapier, Make, etc.)
2. Analytics setup (GA4, Meta Pixel, etc.)
3. Email integrations
4. CRM connections
5. Custom JavaScript/CSS

**Return:**
```json
{
  "currentIntegrations": [
    {
      "type": "form",
      "provider": "Webflow native",
      "configuration": "..."
    }
  ],
  "recommendedAdditions": [
    {
      "integration": "WhatsApp Business API",
      "purpose": "Post-application redirect",
      "implementation": "Custom code embed"
    }
  ]
}
```

---

## 📤 OUTPUT FORMAT

Return a comprehensive JSON report:

```json
{
  "auditDate": "2026-01-13",
  "siteUrl": "https://bambufitnessbali.com",
  "webflowProjectId": "...",
  
  "siteStructure": { ... },
  "academyPage": { ... },
  "requiredChanges": {
    "content": [],
    "forms": [],
    "sections": [],
    "technical": []
  },
  "integrations": { ... },
  
  "recommendations": [
    {
      "priority": "high/medium/low",
      "change": "Description",
      "effort": "hours estimate",
      "notes": "..."
    }
  ],
  
  "screenshots": [
    {
      "name": "dashboard",
      "description": "Webflow dashboard overview"
    }
  ],
  
  "accessNotes": "Any issues or notes about access"
}
```

---

## ⚠️ IMPORTANT NOTES

1. **Do NOT make any changes** — this is audit only
2. Document everything you see, even if seems unrelated
3. Note any errors or access issues
4. If you find custom code, copy it verbatim
5. Check for staging/dev versions of Academy page

---

## 🔑 ACCESS

Webflow credentials will be provided by Alexey.

**Required access level:** Editor or Designer role

---

## 🔄 RETURN TO CURSOR

After completing this audit, return the JSON output to the Cursor agent for:
1. Planning specific Webflow changes
2. Creating implementation tickets
3. Coordinating with n8n workflow setup
4. Syncing with GitHub Pages version

---

*Task created by Cursor Agent — Bambu Academy Project*

