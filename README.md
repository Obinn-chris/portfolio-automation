# Outbound Sales Automation Portfolio

Built at FullFunnel — a B2B outbound sales agency. These workflows automate reporting and intelligence tasks that previously required 30+ hours of manual work per fortnight.

---

## Workflows

### 1. Outbound Reporting — v1 (Original)
**File:** `outbound-reporting-v1-original.json`

Single-campaign reporting workflow. Pulls data from Instantly (email outreach) and HeyReach (LinkedIn outreach), processes campaign metrics, and generates a formatted report delivered to Slack and Google Drive.

**Limitations discovered:** Open rate calculations were incorrect — the Instantly API returns `emails_sent_count` as total emails across all sequence steps, but the correct denominator for open rate is `sequences_started` (unique people contacted). This caused inflated numbers that required manual correction every reporting cycle.

---

### 2. Outbound Reporting — v6 (Final)
**File:** `outbound-reporting-v6-final.json`

Complete rebuild after identifying the root cause of the number bug and adding multi-campaign support.

**Improvements over v1:**
- Fixed open rate calculation — now uses `sequences_started` as the denominator, not `emails_sent_count`
- Multi-campaign support — runs across all active campaigns dynamically, no manual setup per campaign
- AI-generated narrative report using Google Gemini 2.5 Pro — outputs a human-readable performance summary
- Outputs to Gamma (report doc), Google Drive (archive), and Slack (team notification)
- Reduced manual correction time from ~30 hours/fortnight to near zero

**Stack:** n8n · Instantly API · HeyReach API · Google Gemini 2.5 Pro · Google Drive · Slack

---

### 3. Win/Loss Analysis — Closed Opportunities
**File:** `win-loss-analysis-workflow.json`

Automated win/loss reporting workflow for closed opportunities. Pulls CRM data (HubSpot), segments by outcome, and generates structured analysis for sales leadership review.

**Stack:** n8n · HubSpot · Google Sheets

---

### 4. ICP Intelligence Report
**File:** `icp-intelligence-workflow.json`

Ideal Customer Profile (ICP) intelligence workflow. Enriches and analyzes prospect data to score and prioritize leads based on fit criteria defined by the sales team.

**Stack:** n8n · Clay · Google Gemini

---

## Context

All workflows built using n8n (no-code automation platform). The progression from v1 to v6 on the outbound reporting workflow represents a full debugging and rebuild cycle — identifying a data accuracy bug, tracing it to the source API behavior, redesigning the aggregation logic, and scaling from single to multi-campaign architecture.
