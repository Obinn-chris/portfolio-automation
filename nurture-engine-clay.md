# Nurture Engine — Built with Clay

## What It Does

Auto-generates hyper-personalized nurture emails for warm leads, triggered by real buying signals (hiring, funding, news), with a fitment score and one-pager — ready to send.

Pulls contacts from HubSpot, enriches them with live company and person data, scores them against FullFunnel's ICP, selects the most relevant content (YouTube or blog), and writes a personalized email — all automatically.

**Stack:** Clay · HubSpot · Claygent · Instant Data Scraper · YouTube · Blog content library · GPT-4o · GPT-5.4

---

## How It Works — Step by Step

### 1. Data Import
- Pulls contacts directly from HubSpot CRM into Clay. Each row = one lead.
- Extracts: First Name, Last Name, Email, Phone, Company, Job Title, LinkedIn URL.

### 2. Company Name Normalization
- AI (GPT-4o) strips legal suffixes and regional descriptors from raw company names.
- Example: "DC Norris North America LLC" → "DC Norris"
- Ensures clean names for all downstream enrichment lookups.

### 3. LinkedIn & Employment Verification
- If LinkedIn URL is missing from HubSpot, Claygent web-searches and finds it using name + company domain.
- Claygent then visits the LinkedIn profile and checks the Experience section to confirm the person is still at that company.
- **This is a gate** — all downstream enrichment only runs if employment is confirmed (YES).

### 4. Company Enrichment
- Finds the company domain via Google.
- Clay's company enrichment pulls: LinkedIn URL, company size, industry, description, employee count, revenue, and location.

### 5. Hiring Signals
- Searches LinkedIn for active job postings at the company filtered by GTM/Sales/Marketing/RevOps roles posted in the last 30 days (US only).
- Outputs: total job count + list of relevant job titles being hired for.

### 6. News & Major Development Signals
- Claygent searches public sources (company site, LinkedIn, Twitter/X, news) for the most recent major development: funding, product launch, partnership, acquisition, leadership change, or expansion.
- A date check confirms the news is within the last 90 days — if not, the field is blanked out so stale news never reaches the email.

### 7. Lead Company Profile
- Claygent scrapes the lead's company website to extract what they offer and who their target customers are.
- Used as context for fitment scoring and email generation.

### 8. Fitment Scoring & One-Pager
- AI (GPT-5.4) takes all enriched data — company profile, hiring signals, major news, lead's offerings, FullFunnel's services and ICP — and generates:
  - A **Fitment Score** (how well the lead matches FullFunnel's ICP)
  - A **Status label** (Hot / Warm / Cold)
  - A formatted **One-Pager** account summary for the sales team

### 9. LinkedIn Posts
- Fetches the lead's 5 most recent LinkedIn posts for additional personalization context.

### 10. Event Categorization
- AI (GPT-4o) classifies the company's current moment into one of: Hiring · Funding · Expansion · Acquisition · Product Launch · No Meaningful Event
- This category drives which content gets selected in the next step.

### 11. Content Selection
- Based on the event category and one-pager, AI (GPT-5.4) selects the single most relevant content URL from the connected content libraries (YouTube videos or blog posts).
- Falls back to generic GTM/RevOps content if no specific match is found.
- Content libraries are maintained in two separate Clay tables: YouTube content and Blog content.

### 12. Email Generation
- AI (GPT-5.4) writes a 70–110 word nurture email + 2–5 word subject line.
- Uses the strongest available signal in priority order: Hiring signal > Major news > Generic GTM pattern.
- Ends with the selected content URL. No hard CTA after the link.

### 13. Export
- Lead's LinkedIn posts are sent to a separate Clay table ("LinkedIn Post") for further processing.
- Final email + subject line is ready to push to outreach sequence.

---

## Architecture — Three Tables

| Table | Purpose |
|-------|---------|
| **Nurture Engine** (main) | Full enrichment, scoring, and email generation pipeline |
| **LinkedIn Post** | Receives and stores LinkedIn post data from the main table |
| **Content Library** | YouTube videos, blog posts, and fallback GTM content used for email personalization |
