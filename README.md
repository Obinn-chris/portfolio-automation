# Chris Mbanta — GTM Engineering Portfolio

I'm a GTM Engineer who builds automation systems that turn manual sales operations work into repeatable, data-accurate processes. This repository contains workflows I built to automate outbound reporting, win/loss analysis, and ICP intelligence — work that previously consumed 30+ hours of manual effort every two weeks.

---

## Background

**At FullFunnel** (B2B outbound sales agency), I ran GTM Engineering across multiple clients including InsightSoftware, Franklin Alliance, Aprika, Unify, and Inspire — building and maintaining the full automation stack for their outbound programs.

**At GrowSurely**, I built and ran outbound campaigns across multiple industries — recruiting, manufacturing, SaaS, and more — managing millions of rows in Clay across dozens of campaigns.

**Stack I work in:** Clay · n8n · HubSpot · Salesforce · HeyReach · Instantly · Lemlist · Apollo · Gong · Gamma · LucidChart · Claude Code · Instant Data Scraper

---

## Workflows in This Repo

### 1. Outbound Reporting — v1 (Original)
**File:** `outbound-reporting-v1-original.json`

The first version of an automated bi-weekly outbound reporting workflow. Pulls campaign data from Instantly (email) and HeyReach (LinkedIn), processes the metrics, and delivers a report to Slack and Google Drive.

**Problem discovered during use:** Open rate figures were consistently inflated. After digging into the Instantly API, I found the root cause — `emails_sent_count` returns total emails across all sequence steps (e.g. 145), but the correct denominator for open rate is `sequences_started` (unique people contacted, e.g. 46). This mismatch caused numbers that required 20+ hours of manual correction every reporting cycle.

---

### 2. Outbound Reporting — v6 (Final)
**File:** `outbound-reporting-v6-final.json`

Complete rebuild after identifying the data bug and scaling requirements. Went from single-campaign to fully dynamic multi-campaign architecture.

**What changed from v1 to v6:**
- Fixed the open rate calculation — denominator now uses `sequences_started`, not `emails_sent_count`
- Multi-campaign support — workflow runs across all active campaigns dynamically, no manual configuration per campaign
- AI-generated narrative using Google Gemini 2.5 Pro — produces a human-readable performance summary with diagnosis and recommendations
- Switched from passing data through an HTTP API before Gamma to passing it directly to Gamma — simplifying the pipeline and removing a failure point
- Delivers to Gamma (formatted report doc), Google Drive (archive), and Slack (team notification)
- Reduced manual correction time from ~30 hours/fortnight to near zero

**Real output example — Constellation client (April 27 – May 8, 2026):**
The workflow automatically pulled and unified data across 8 campaigns (4 Instantly email, 4 HeyReach LinkedIn), identified that zero replies were generated despite healthy deliverability signals, diagnosed the specific technical and messaging gaps per campaign, and delivered a structured report with prioritized action items — all without manual input.

**Stack:** n8n · Instantly API · HeyReach API · Google Gemini 2.5 Pro · Google Drive · Slack · Gamma

---

### 3. Win/Loss Analysis — ISW Certent Equity Management
**File:** `win-loss-analysis-workflow.json`

Automated win/loss analysis pipeline built for InsightSoftware's Certent Equity Management product line. Pulls closed opportunity data from HubSpot/Salesforce, enriches it, runs statistical analysis, and generates a full ICP report for the sales leadership team.

**Real output — ISW Certent dataset:**
- Analyzed 221 standard opportunities (73 Won, 148 Lost) with a 33.0% overall conversion rate and $346.7k total ARR won
- Identified that public companies convert at 43.1% vs 33.0% overall, with $21.6k avg ARR
- Pinpointed top geographies: California, Texas, NJ, MD, TN, WA
- Top converting industries: Banking, Medical Equipment Manufacturing, Biotechnology, Oil & Gas
- Surfaced that existing customer expansion converts at 50.9% — highest of any segment
- Delivered a scored ICP definition used to reprioritize outbound targeting

**Stack:** n8n · HubSpot · Salesforce · Gong · Clay · Google Sheets · Gamma

---

### 4. ICP Intelligence Workflow
**File:** `icp-intelligence-workflow.json`

Automated ICP scoring and enrichment workflow. Takes raw prospect data, enriches it against defined fit criteria, scores each account, and outputs a prioritized target list for the outbound team.

**Stack:** n8n · Clay · Google Gemini · Apollo

---

## What This Work Demonstrates

- **Data debugging** — traced a reporting bug to specific API behavior and redesigned the aggregation logic to fix it
- **Systems thinking** — scaled a single-campaign workflow into a dynamic multi-campaign architecture
- **Full-stack GTM** — connected data sources (CRM, outreach platforms, enrichment tools) into unified automated pipelines
- **Business impact** — analysis outputs directly informed ICP prioritization and targeting decisions for clients
