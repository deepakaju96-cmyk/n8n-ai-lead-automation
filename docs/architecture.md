# Architecture Deep Dive

## System Design

This automation system follows a **hub-and-spoke architecture** where the Lead Capture workflow acts as the primary hub, with the Opp-won Monitor as a secondary entry point, both feeding into the shared Contract Sender sub-flow.

## Workflow 1: Lead Capture & Enrichment

### Node-by-Node Breakdown

#### 1. Form Trigger (`n8n-nodes-base.formTrigger`)
- **Purpose:** Public-facing web form for lead capture
- **Fields:** Full Name*, Email*, Company Name*, Phone, Website, Industry, Company Size, Notes
- **Output:** JSON object with all form field values + `submittedAt` timestamp

#### 2. Fetch All Leads (`n8n-nodes-base.googleSheets`)
- **Purpose:** Reads all existing leads from the master spreadsheet
- **Config:** `continueOnFail: true` — handles empty sheets gracefully
- **Config:** `alwaysOutputData: true` — ensures downstream nodes execute even on empty sheet

#### 3. Check Duplicate (`n8n-nodes-base.code`)
- **Purpose:** Custom deduplication engine
- **Logic:**
  - Iterates through all existing leads
  - Checks for email match (case-insensitive)
  - Checks for name + company match (case-insensitive)
  - Returns `duplicateReason` string (empty = new lead)

#### 4. Is New Lead? (`n8n-nodes-base.if`)
- **Purpose:** Routes based on deduplication result
- **True branch:** `duplicateReason` is empty → proceed with enrichment
- **False branch:** Duplicate found → flow stops (no further processing)

#### 5. Research Company (`@n8n/n8n-nodes-langchain.googleGemini`)
- **Purpose:** AI-powered company research using Gemini 2.5 Flash
- **Features:**
  - Web search enabled (`builtInTools.googleSearch: true`)
  - Structured JSON output (website, phone, industry, size, about)
  - Uses form-provided data as seed information
- **Model:** `models/gemini-2.5-flash`

#### 6. Parse AI Response (`n8n-nodes-base.code`)
- **Purpose:** Extracts structured data from AI response
- **Error handling:** Strips markdown code blocks, finds JSON in response
- **Merge strategy:** Form values take priority over AI-discovered values

#### 7. Append to Sheet (`n8n-nodes-base.googleSheets`)
- **Purpose:** Writes the fully enriched lead record
- **Columns:** Full Name, Email, Company Name, Phone, Website, Industry, Company Size, About, Notes, Submitted At

#### 8. Notify Team on Slack (`n8n-nodes-base.slack`)
- **Purpose:** Real-time team notification with rich formatting
- **Format:** Emoji-enhanced message with all lead details + AI company summary

#### 9. Send Thank You Email (`n8n-nodes-base.gmail`)
- **Purpose:** Automated personalized response to the lead
- **Personalization:** Uses lead's name and company name

#### 10. Call Contract Sender (`n8n-nodes-base.executeWorkflow`)
- **Purpose:** Triggers contract generation sub-flow
- **Runs in parallel** with Slack notification

---

## Workflow 2: Opp-won Monitor

### Purpose
Watches for leads that have progressed to "Opp-won" stage and automatically triggers contract sending.

### Schedule
Runs every **5 minutes** to minimize delay between opportunity close and contract delivery.

### Flow
1. **Schedule Trigger** → Fires every 5 minutes
2. **Google Sheets Read** → Filters for `Stage = "Opp-won"`
3. **Execute Workflow** → Calls Contract Sender sub-flow for each match

---

## Workflow 3: Contract Sender (Sub-flow)

### Purpose
Shared contract generation and delivery workflow. Called internally by both Lead Capture and Opp-won Monitor.

### Design Decision
Built as a sub-flow to:
- Avoid code duplication
- Enable independent updates to contract logic
- Keep parent workflows focused on their primary purpose

---

## Error Handling Strategy

| Scenario | Handling |
|----------|----------|
| Empty Google Sheet | `continueOnFail: true` on Fetch node |
| AI returns invalid JSON | Try/catch in Parse node with fallback |
| Duplicate lead submitted | Blocked at If node, no further processing |
| Gmail/Slack API failure | n8n native retry mechanism |
