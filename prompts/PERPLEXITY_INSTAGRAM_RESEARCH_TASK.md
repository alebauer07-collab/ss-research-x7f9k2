# 🔍 Perplexity Comet Task: Instagram Commenter Research

## Task Overview
Research Will Henke's (@wjhenke) Instagram commenters to identify potential Bambu Academy applicants.

## Assigned To
- **Primary Executor:** Perplexity Comet (Browser Automation Agent)
- **Supervisor:** Alexey

## Task Parameters
- **Target Account:** @wjhenke (Will Henke's Instagram)
- **Research Scope:** Recent 10-15 posts
- **Target Output:** 100+ potential applicants
- **Batch Size:** 10-15 profiles per iteration (no stoppages)

---

## Research Workflow

### Phase 1: Comment Collection
1. Navigate to @wjhenke Instagram profile
2. Open each of the 10-15 most recent posts
3. Expand all comments on each post
4. Extract all unique commenters (username, comment text, engagement type)
5. Deduplicate across posts

### Phase 2: Profile Research (Per Batch of 10-15)
For each commenter, research and extract:

#### Basic Info
- Full name
- Instagram handle
- Profile bio
- Location (if available)
- Follower/following count
- Profile type (personal/business/creator)

#### Fitness Industry Signals
- [ ] Is this person a fitness professional?
- [ ] Do they mention coaching, training, or fitness in bio?
- [ ] Do they have a gym/studio tagged?
- [ ] Do they post fitness content?
- [ ] Are they certified (any certifications mentioned)?

#### Engagement Quality
- What did they comment on Will's posts?
- How engaged are they (multiple comments, likes)?
- Do they ask questions about coaching/training?

### Phase 3: Quick Scoring (1-10 scale)
Score each profile on:

| Parameter | Description | Score |
|-----------|-------------|-------|
| **Fitness Background** | Evidence of fitness industry involvement | 1-10 |
| **Coaching Interest** | Signs they want to become/improve as coach | 1-10 |
| **Engagement Level** | How actively they engage with Will's content | 1-10 |
| **Professional Presence** | Quality of their own Instagram presence | 1-10 |
| **Location Fit** | Can they realistically attend Bali program | 1-10 |

**Total Score:** Sum of all parameters (max 50)

### Phase 4: Output Format
Generate JSON for each researched profile:

```json
{
  "instagram_handle": "@username",
  "full_name": "First Last",
  "location": "City, Country",
  "bio_summary": "Brief summary of their bio",
  "profile_url": "https://instagram.com/username",
  "follower_count": 1234,
  "is_fitness_professional": true,
  "fitness_signals": [
    "Personal trainer",
    "Posts workout content",
    "Tagged at XYZ Gym"
  ],
  "engagement_with_will": {
    "comment_count": 3,
    "sample_comments": [
      "Great post about coaching!",
      "Would love to learn more about this"
    ]
  },
  "scoring": {
    "fitness_background": 8,
    "coaching_interest": 7,
    "engagement_level": 6,
    "professional_presence": 7,
    "location_fit": 5,
    "total": 33
  },
  "recommendation": "HIGH_POTENTIAL | MEDIUM_POTENTIAL | LOW_POTENTIAL",
  "notes": "Any additional observations"
}
```

---

## Iteration Instructions

### Per Iteration (10-15 profiles):
1. Research profiles without stopping
2. Generate JSON for each
3. Compile into batch array
4. Continue to next batch immediately
5. No human approval needed between batches

### Completion Criteria:
- Minimum 100 profiles researched
- All profiles scored
- JSON output ready for Cursor agent processing

---

## Output Delivery
Return results to Cursor chat as:
1. Summary statistics (total researched, score distribution)
2. Top 20 highest-scoring profiles (detailed)
3. Full JSON array of all profiles

---

## Notes for Perplexity Comet
- Use browser automation for Instagram navigation
- Respect rate limits (don't scrape too fast)
- Focus on quality over speed
- Flag any accounts that seem like bots/spam
- Prioritize accounts with fitness-related content

---

*Task Created: January 13, 2026*
*For: Bambu Academy Applicant Pipeline*

