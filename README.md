# 🎫 AI Ticket Triage for Customer Success
Automated support ticket classification, churn alerting & VoC logging — powered by n8n + GPT-4o

## 📌 Overview
This n8n workflow automates the triage of incoming customer support emails for a Customer Success team. Every 5 minutes, it monitors a Gmail inbox, uses GPT-4o to intelligently classify each ticket, logs structured data to a Google Sheet for VoC analysis, and instantly fires a churn-risk alert email when a high-risk ticket is detected.
Built to reduce manual triage time, surface churn signals early, and feed clean, structured data into a VoC reporting pipeline — without a single human having to read and tag a ticket.

## 🧠 The Business Problem It Solves
In high-volume CS environments, support tickets arrive faster than teams can manually prioritise them. Critical signals — a frustrated enterprise client, a billing issue escalating to churn, an access problem blocking adoption — can get buried in the queue.

### This workflow ensures:

No high-risk ticket goes unnoticed — churn alerts fire in real time
Every ticket is tagged and logged — enabling trend analysis and VoC reporting
CSMs spend time on relationships, not triage — AI handles the classification layer


#### ⚙️ Workflow Architecture
Gmail Trigger (every 5 min)
        │
        ▼
Clean Email Data
(extract subject, body, sender, timestamp)
        │
        ▼
AI Classification (GPT-4o-mini)
(category, sentiment, urgency, churn_risk, summary, product_area)
        │
        ▼
Parse JSON Response
        │
        ├──────────────────────────┐
        ▼                          ▼
Prepare VoC Data            Check Churn Risk
        │                  (high churn OR negative
        ▼                   sentiment OR high urgency)
VoC Log                            │
(Google Sheets)                    ▼
                         Send Churn Alert Email
                         (to assigned CSM)


### 🔍 AI Classification Fields
Each incoming ticket is analysed and tagged across 6 dimensions:
FieldOptionscategoryonboarding, billing, bug, feature_request, account_access, othersentimentpositive, neutral, negativeurgencylow, medium, highchurn_risklow, medium, highproduct_arealogin, billing, reporting, etc.summary1–2 sentence AI-generated summary of the issue


### 🚨 Churn Alert Logic
An alert email is triggered to the assigned CSM when any of the following conditions are met:

churn_risk = high
sentiment = negative
urgency = high

This OR-based logic ensures no at-risk ticket slips through, even if only one signal is present.


### 📊 VoC Logging
Every processed ticket — regardless of risk level — is appended to a Google Sheet with the following fields:
date | from | subject | category | sentiment | urgency | churn_risk | product_area | summary
This creates a continuously updated VoC dataset that can be used for:

Trend analysis (which product areas generate the most tickets?)
Sentiment tracking over time
Churn risk monitoring across the account base
Reporting to leadership on service quality KPIs


### 🛠️ Tech Stack
ToolRolen8nWorkflow automation platformGmail (OAuth2)Email trigger + churn alert deliveryGPT-4o-miniAI classification engineGoogle SheetsVoC data logging


### 🚀 How to Use This Workflow

Import the JSON — In n8n, go to Workflows → Import → paste or upload AI_Tix_Triage_for_CS.json
Connect your credentials — Link your Gmail OAuth2 account and Google Sheets account
Set your Google Sheet — Point the VoC Log node to your target spreadsheet and sheet tab
Set the CSM alert email — Update the Send Churn Alert Email node with the recipient address
Activate the workflow — Toggle it on and it will begin polling every 5 minutes


### 💡 Potential Extensions

Slack integration — Route churn alerts to a dedicated #churn-risk Slack channel instead of (or in addition to) email
Salesforce logging — Auto-create or update a case record in Salesforce when a high-risk ticket is detected
Sentiment trend dashboard — Connect the Google Sheet to Power BI or Looker Studio for live VoC reporting
Priority routing — Add branching logic to route tickets to different CSMs based on category or account tier
Weekly digest — Schedule a weekly summary email of ticket trends pulled from the VoC log


### 👤 About the Author
Jon Sholl — Customer Success Operations Manager based in Singapore, with 10+ years managing enterprise accounts and post-sales operations. This workflow was built to demonstrate how AI-powered automation can reduce manual triage overhead, surface churn signals earlier, and enable data-driven CS operations.

🌐 [Portfolio](https://jsholl.base44.app/)
💼 [LinkedIn](https://www.linkedin.com/in/jonasholl)
📧 jonsholl@gmail.com


📄 License
MIT — free to use, adapt, and build on.
