# Webflow Academy V2 Page Creation — Perplexity Comet

**Assigned To:** Perplexity Comet (Browser Automation Agent)  
**Coordinator:** Cursor Agent + Alexey  
**Date:** January 13, 2026  
**Priority:** HIGH

---

## 🎯 MISSION

Create a new Academy V2 page in Webflow WITHOUT affecting the current live website. This page will be the new application funnel for Bambu Academy March 2026 cohort.

---

## ⚠️ CRITICAL SAFETY RULES

1. **NEVER modify existing published pages**
2. **NEVER delete any content**
3. **ALWAYS work in draft/staging mode**
4. **CREATE NEW pages only** — use slug `/academy-v2` or `/academy/apply`
5. **Test in preview mode** before any publish action
6. **Document every action** you take

---

## 📋 PHASE 1: RESEARCH & PREPARATION

### Step 1.1: Webflow Environment Research

Before making any changes, research and document:

```json
{
  "webflowCapabilities": {
    "canCreatePages": true/false,
    "canDuplicatePages": true/false,
    "canAddCustomCode": true/false,
    "canCreateForms": true/false,
    "hasAPIAccess": true/false,
    "hasStagingEnvironment": true/false
  },
  "currentAcademyPage": {
    "exists": true/false,
    "url": "...",
    "canDuplicate": true/false
  },
  "recommendedApproach": "duplicate existing / create from scratch / use template"
}
```

### Step 1.2: API/Integration Research

Research how to connect Webflow to external systems:

1. **Form Submissions → Webhook**
   - Can Webflow forms trigger webhooks?
   - What's the native integration method?
   - Can we use Zapier/Make as middleware?

2. **Custom Code Capabilities**
   - Can we embed JavaScript in pages?
   - Can we add custom form handlers?
   - Can we inject our WhatsApp redirect logic?

3. **API Access for Cursor Agent**
   - Does Webflow have a REST API?
   - Can Cursor connect directly via API?
   - What authentication is needed?

**Output this research:**
```json
{
  "integrationOptions": {
    "webhooks": {
      "native": true/false,
      "method": "Webflow Logic / Zapier / Make / Custom",
      "webhookUrl": "where to configure"
    },
    "customCode": {
      "allowed": true/false,
      "location": "page settings / embed block / site-wide",
      "limitations": "..."
    },
    "api": {
      "available": true/false,
      "docsUrl": "...",
      "authMethod": "API key / OAuth",
      "cursorCanConnect": true/false
    }
  }
}
```

---

## 📋 PHASE 2: PAGE CREATION

### Step 2.1: Create New Page

**Method A: Duplicate Existing Academy Page**
1. Find current Academy page in Webflow
2. Duplicate it (right-click → Duplicate)
3. Rename to "Academy V2" or "Academy Application"
4. Change slug to `/academy-v2`
5. Keep as DRAFT (do not publish)

**Method B: Create From Scratch**
1. Create new blank page
2. Name: "Academy V2"
3. Slug: `/academy-v2`
4. Keep as DRAFT

### Step 2.2: Page Structure

Create these sections (matching our GitHub Pages version):

```
1. HERO SECTION
   - Background: Video or gradient
   - Headline: "Become an Elite Coach"
   - Subheadline: "4-Week Intensive Training in Bali"
   - CTA Button: "Apply Now" → scrolls to form

2. WHO IS THIS FOR
   - 4 cards with icons
   - Aspiring coaches, Career changers, Gym owners, Fitness enthusiasts

3. THE PROCESS
   - 4-step visual timeline
   - Apply → Interview → Train → Certify

4. WHAT YOU'LL MASTER
   - 9 skill cards with icons
   - Body Mechanics, Nutrition, etc.

5. PAYMENT OPTIONS
   - Early Bird: $4,299 (highlight this)
   - Standard: $6,999
   - Note: "Payment plans available upon request"

6. CAREER LADDER
   - Timeline showing progression
   - Junior → Senior → Elite → Build Your Own Path

7. APPLICATION FORM
   - Fields: First Name, Last Name, Email, WhatsApp, Location, Current Role, Why Apply
   - Submit button
   - Form name: "academy-v2-application"

8. FAQ SECTION
   - Accordion style
   - Common questions about the program

9. FOOTER
   - Standard Bambu footer
```

### Step 2.3: Form Configuration

**Form Settings:**
```json
{
  "formName": "academy-v2-application",
  "fields": [
    {"name": "first_name", "type": "text", "required": true},
    {"name": "last_name", "type": "text", "required": true},
    {"name": "email", "type": "email", "required": true},
    {"name": "whatsapp", "type": "tel", "required": true},
    {"name": "location", "type": "text", "required": true},
    {"name": "current_role", "type": "select", "options": ["Aspiring coach", "Currently coaching", "Gym owner", "Fitness enthusiast", "Career changer", "Other"]},
    {"name": "why_apply", "type": "textarea", "required": true}
  ],
  "submitAction": "webhook",
  "webhookUrl": "https://alexbauer.app.n8n.cloud/webhook/bambu-webflow-application",
  "successMessage": "Application received! Redirecting to WhatsApp...",
  "redirectUrl": "custom JavaScript handles this"
}
```

### Step 2.4: Custom Code Integration

**Add this to Page Custom Code (before </body>):**

```html
<script>
// Bambu Academy V2 - Application Handler
(function() {
  const form = document.querySelector('form[data-name="academy-v2-application"]');
  if (!form) return;
  
  form.addEventListener('submit', function(e) {
    // Generate unique applicant ID
    const timestamp = Date.now().toString(36);
    const random = Math.random().toString(36).substr(2, 4);
    const applicantId = 'BAMBU-2026-' + (timestamp + random).toUpperCase().substr(0, 6);
    
    // Store in localStorage
    const formData = new FormData(form);
    const applicantData = {
      applicant_id: applicantId,
      first_name: formData.get('first_name'),
      last_name: formData.get('last_name'),
      email: formData.get('email'),
      whatsapp: formData.get('whatsapp'),
      location: formData.get('location'),
      current_role: formData.get('current_role'),
      why_apply: formData.get('why_apply'),
      submitted_at: new Date().toISOString(),
      source: 'webflow'
    };
    
    localStorage.setItem('bambu_application', JSON.stringify(applicantData));
    
    // After Webflow processes the form, redirect to thank you flow
    setTimeout(function() {
      const whatsappNumber = '6281244997721';
      const message = encodeURIComponent(
        `Hi! I just applied to Bambu Academy 🌴\n\nMy Application ID: ${applicantId}\nName: ${applicantData.first_name} ${applicantData.last_name}\n\nExcited to learn more!`
      );
      
      // Open WhatsApp in new tab
      window.open(`https://wa.me/${whatsappNumber}?text=${message}`, '_blank');
      
      // Redirect to profile page
      window.location.href = `/academy/profile?id=${applicantId}`;
    }, 1500);
  });
})();
</script>
```

---

## 📋 PHASE 3: WEBHOOK SETUP

### Option A: Webflow Native Logic (if available)
1. Go to Site Settings → Logic
2. Create new flow triggered by form submission
3. Add HTTP Request action
4. Configure webhook to n8n

### Option B: Zapier/Make Integration
1. Connect Webflow to Zapier
2. Trigger: New Form Submission
3. Action: Webhook to n8n URL

### Option C: Custom Webhook (via form action)
1. Change form action to webhook URL
2. Handle CORS if needed

**Webhook URL:** `https://alexbauer.app.n8n.cloud/webhook/bambu-webflow-application`

**Payload format:**
```json
{
  "source": "webflow",
  "applicant_id": "BAMBU-2026-XXXXXX",
  "first_name": "...",
  "last_name": "...",
  "email": "...",
  "whatsapp": "...",
  "location": "...",
  "current_role": "...",
  "why_apply": "...",
  "submitted_at": "ISO timestamp"
}
```

---

## 📋 PHASE 4: PROFILE PAGE SETUP

### Create Profile Page

1. Create new page: "Academy Profile"
2. Slug: `/academy/profile`
3. Keep as DRAFT

**Page structure:**
```
- Header with Bambu logo
- "Your Application" heading
- Application details card (populated via JavaScript)
- "AI Analysis" section with loader
- Refresh button to check for updates
- WhatsApp contact button
```

**Custom code for profile page:**
```html
<script>
// Profile Page - Fetch from MongoDB via API
(function() {
  const urlParams = new URLSearchParams(window.location.search);
  const applicantId = urlParams.get('id');
  
  if (!applicantId) {
    document.body.innerHTML = '<h1>No application found</h1>';
    return;
  }
  
  // First, show localStorage data immediately
  const localData = JSON.parse(localStorage.getItem('bambu_application') || '{}');
  if (localData.applicant_id === applicantId) {
    renderApplication(localData);
  }
  
  // Then fetch from MongoDB for AI analysis
  fetchProfile(applicantId);
  
  function fetchProfile(id) {
    fetch(`https://alexbauer.app.n8n.cloud/webhook/bambu-get-profile?id=${id}`)
      .then(r => r.json())
      .then(data => {
        if (data.ai_analysis) {
          renderAIAnalysis(data.ai_analysis);
        } else {
          showAnalysisPending();
        }
      })
      .catch(err => showAnalysisPending());
  }
  
  function renderApplication(data) {
    // Populate application details
  }
  
  function renderAIAnalysis(analysis) {
    // Show AI-generated profile insights
  }
  
  function showAnalysisPending() {
    // Show loader with "Analysis in progress..."
  }
})();
</script>
```

---

## 📤 OUTPUT FORMAT

After completing each phase, return:

```json
{
  "phase": 1,
  "status": "completed/in_progress/blocked",
  "actions_taken": [
    "Researched Webflow capabilities",
    "Documented API options"
  ],
  "screenshots": [
    {"name": "webflow-dashboard", "description": "..."}
  ],
  "blockers": [],
  "next_steps": [],
  "questions_for_alexey": [],
  "questions_for_cursor": []
}
```

---

## 🔄 COORDINATION WORKFLOW

### Who Does What:

| Task | Executor | Notes |
|------|----------|-------|
| Webflow page creation | Comet | Browser automation |
| Custom code writing | Cursor | Provides code snippets |
| Webhook setup | Cursor + n8n | API configuration |
| MongoDB schema | Cursor | Database design |
| Testing | Alexey | Manual verification |
| Publishing | Alexey | Final approval needed |

### Communication Flow:

```
1. Comet researches Webflow → Returns JSON to Cursor
2. Cursor analyzes → Provides code/instructions
3. Comet implements in Webflow → Screenshots to Alexey
4. Alexey reviews → Approves or requests changes
5. Cursor updates n8n/MongoDB → Tests webhook
6. Alexey publishes page → Live!
```

### API Connection Research:

**For Cursor to connect directly to Webflow:**
1. Check if Webflow API is enabled for the site
2. Get API token from Site Settings → Integrations
3. API Docs: https://developers.webflow.com/
4. Cursor can use HTTP requests to:
   - List pages
   - Update page content
   - Manage CMS items
   - Trigger publishes (with caution)

**Return API access info:**
```json
{
  "webflowApi": {
    "enabled": true/false,
    "apiToken": "REDACTED - tell Alexey to share securely",
    "siteId": "...",
    "capabilities": ["read_pages", "write_pages", "publish", "forms"]
  }
}
```

---

## ⏱️ ESTIMATED TIMELINE

| Phase | Duration | Dependencies |
|-------|----------|--------------|
| Research | 30 min | Webflow access |
| Page Creation | 1-2 hours | Research complete |
| Form Setup | 30 min | Page created |
| Custom Code | 30 min | Cursor provides code |
| Webhook Config | 30 min | n8n ready |
| Testing | 1 hour | All above complete |
| **Total** | **4-5 hours** | |

---

## 🚨 ESCALATION

If you encounter any of these, STOP and report to Alexey:
- Cannot create new pages
- Accidentally modified live content
- Form submissions not working
- API access denied
- Any error messages

---

*Task created by Cursor Agent — Bambu Academy Project*
*Coordination: Cursor ↔ Comet ↔ Alexey*


