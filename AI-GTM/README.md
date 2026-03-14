# 🚀 AI-Powered GTM Engine

> **Replace a $300K+ sales team with AI automation for ~$100/month.**

An end-to-end AI-driven Go-To-Market engine built with **n8n**, **LLM APIs**, and **Salesforce**. Fully autonomous — finds leads, enriches them, writes outbound sequences, generates content, and orchestrates your CRM. All running 24/7.

---

## 📊 Results

| Metric | Output |
|--------|--------|
| **Qualified Leads** | 80-200/month |
| **Enrichment Rate** | 90%+ |
| **Cost Per Lead** | < $2 |
| **Outbound Sequences** | 15-30/week (AI-written, personalized) |
| **LinkedIn Content** | 7 posts/week + daily competitor intel |
| **Time to First Contact** | < 24 hours |
| **Traditional Cost** | $300K-$430K/year |
| **This Engine** | ~$1,200/year in tools + 1 person |

---

## 🔧 5 Workflows

### 1. 🔍 Lead Identification Pipeline
**`asteris-google-maps-lead-finder.json`**

Automatically discovers and captures leads from Google Maps.

- **Apify Google Maps Scraper** → searches by geography, category, rating
- **Jina Reader** → extracts website content
- **Groq AI (LLaMA 3.3 70B)** → identifies practice details
- Auto-creates leads in **Salesforce** with duplicate detection
- Sends new lead alerts to **Slack**

**Output:** 20-50 new leads per week, automatically captured in your CRM.

---

### 2. 🧠 AI Lead Enrichment Engine
**`asteris-lead-enrichment.json`**

Transforms raw leads into sales-ready profiles with AI-powered analysis.

- **Website Crawler** → scans up to 10 pages (About, Team, Contact)
- **AI Decision Maker Agent** → finds the owner/director, their email, phone, LinkedIn
- **ICP Scoring Agent** → scores 0-100 across 4 dimensions:

| Dimension | Max Score | What It Measures |
|-----------|----------|-----------------|
| ICP Match | 35 | Industry focus, company size, geography |
| Tech Pain | 30 | Legacy systems, missing capabilities |
| Intent | 25 | Hiring signals, new equipment, social activity |
| Authority | 10 | Decision-maker level (owner vs. staff) |

- Auto-tiers: 🔥 **Hot** (≥70) · 🟡 **Warm** (40-69) · 🔵 **Cold** (<40)
- Updates Salesforce + Slack notification

**Output:** Every lead scored, enriched, and ready for outreach.

---

### 3. ✉️ AI Outbound Sequencer
**`asteris-outbound-sequencer.json`**

Generates hyper-personalized outbound email sequences for qualified leads.

- **LinkedIn Profile Scraper** → via DuckDuckGo + Jina Reader
- **AI Outreach Agent** → writes a full 4-touch email sequence:
  - **Day 1:** Pattern interrupt — references specific details, names a pain point
  - **Day 3:** Case study — relevant success story with real numbers
  - **Day 7:** Angle shift — different feature, provocative question
  - **Day 14:** Break-up — respect their time, final value prop
- Plus a **LinkedIn DM** for multi-channel outreach
- Product-specific messaging based on lead's fit
- Posted to **Slack** for team review before sending

**Output:** 15-30 personalized outbound sequences per week, ready to send.

---

### 4. 📢 Content Engine & Competitor Intel
**`asteris-content-engine.json`**

Generates daily thought leadership content and competitive intelligence.

- **DuckDuckGo + Jina** → scrapes LinkedIn for industry pain points and competitor mentions
- **AI Content Writer** → punchy, practitioner-to-practitioner tone (not corporate fluff)
- **AI Competitor Analyst** → tracks what competitors are doing, customer complaints, opportunities
- Delivers to **Slack** daily:
  - 📝 Ready-to-post LinkedIn content
  - 🔍 Competitor intelligence brief with battlecards

**Output:** 7 LinkedIn posts/week + daily competitor intelligence briefs.

---

### 5. 🌐 Web-to-Lead CRM Orchestrator
**`asteris-crm-orchestrator.json`**

Captures inbound interest and routes it intelligently.

- **Branded Demo Request Form** → industry-specific fields (practice type, product interest, current system, pain points)
- **AI Intent Scoring** → scores 1-10 based on form responses
- **Auto Salesforce Lead Creation** → full lead record with AI analysis
- **Intelligent Slack Routing** → high-intent leads (8+) trigger urgent alerts with:
  - Competitor displacement strategy
  - Recommended talking points
  - Suggested demo angle

**Output:** Every inbound request scored, logged, and routed in real-time.

---

## 🔄 Full Cycle Flow

```
GOOGLE MAPS
    │
    ▼
┌─────────────────────────────────┐
│  1. LEAD PIPELINE               │
│  Scrape → Capture → Salesforce  │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  2. AI ENRICHMENT               │
│  Crawl → AI DM Finder →        │
│  ICP Score → Hot/Warm/Cold      │
└────────┬──────────┬─────────────┘
    Hot/Warm       Cold
         │          (parked)
         ▼
┌─────────────────────────────────┐
│  3. OUTBOUND SEQUENCER          │
│  LinkedIn Intel → AI Emails →   │
│  4-Touch Sequence → Slack       │
└─────────────────────────────────┘

    RUNNING IN PARALLEL:

┌─────────────────────────────────┐
│  4. CONTENT ENGINE              │
│  Daily LinkedIn + Competitor    │
└─────────────────────────────────┘

┌─────────────────────────────────┐
│  5. WEB-TO-LEAD                 │
│  Inbound Form → AI Score →     │
│  Slack Alert                    │
└─────────────────────────────────┘

         ALL FEEDS INTO
              │
              ▼
     ┌─────────────────┐
     │   SALESFORCE     │
     │   DASHBOARDS     │
     └─────────────────┘
```

---

## 🛠 Tech Stack

| Tool | Role | Cost |
|------|------|------|
| **n8n** (self-hosted) | Workflow automation engine | Free |
| **Groq AI** (LLaMA 3.3 70B) | Lead scoring, content, enrichment | Free–$50/mo |
| **Apify** | Google Maps scraping, website crawling | ~$49/mo |
| **Jina Reader** | Website content extraction | Free |
| **Salesforce** | CRM, lead management, dashboards | Existing |
| **Slack** | Team notifications, content delivery | Existing |

**Total: ~$100/month** vs. $300K-$430K/year for a traditional sales team.

---

## 🚀 Setup

1. Import workflows into your n8n instance
2. Configure credentials:
   - Salesforce OAuth
   - Groq API key
   - Apify API token
   - Slack Bot token
3. Activate workflows
4. Leads start flowing automatically

---

## 📈 Phase 2 Roadmap

| Enhancement | Tools |
|-------------|-------|
| Auto email sending with tracking | Instantly.ai, Smartlead |
| Multi-channel sequences (LinkedIn + email + phone) | Apollo, La Growth Machine |
| Buyer intent data | Bombora, G2, 6sense |
| Email verification | ZeroBounce, NeverBounce |
| A/B testing engine | Built-in n8n logic |
| Meeting booking | Calendly, Cal.com |

---

## 📄 License

MIT License — see [LICENSE](../LICENSE) for details.

---

**Built by [Deepak Raj](https://github.com/deepakaju96-cmyk)** — AI GTM Engineer
