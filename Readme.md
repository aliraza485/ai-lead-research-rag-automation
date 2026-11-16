# AI Lead Intake, Automated Web Research & Slack RAG Assistant

An end-to-end AI automation pipeline built with **n8n**, **OpenAI (`gpt-4o-mini` & `text-embedding-3-small`)**, **Jina Reader**, **Google Sheets**, and **Slack**.

## Architecture Overview

1. **Lead Intake & Automated Research (`01_lead_intake_and_research.json`)**
   - Captures lead submissions (`Full Name`, `Business Name`, `Business Size`, `Website URL`) via an HTML frontend connected to an n8n Webhook.
   - Scrapes and extracts clean markdown content from the target company's website using **Jina Reader**.
   - Uses an **OpenAI AI Agent** to analyze the company, extract core services, and generate an executive summary.
   - Logs structured lead records with ISO timestamps into **Google Sheets**.
   - Generates vector embeddings (`text-embedding-3-small`) and indexes the company profile into a **Vector Store** for semantic search.
   - Sends a formatted real-time lead alert to a dedicated **Slack** channel.

2. **Interactive Slack RAG Bot (`02_lead_rag_slack_bot.json`)**
   - Listens for `@bot` mentions across Slack threads (`app_mention`).
   - Combines **Vector Store RAG retrieval** (for semantic company knowledge queries) with **Google Sheets tool calling** (for date-filtered daily and monthly lead summaries).
   - Formats responses cleanly into native Slack `mrkdwn` inside threaded replies.

## Tech Stack
- **Orchestration:** n8n Cloud
- **LLMs & Embeddings:** OpenAI (`gpt-4o-mini`, `text-embedding-3-small`)
- **Web Extraction:** Jina Reader API
- **Storage & Knowledge Base:** Google Sheets + Vector Store
- **Interface & Notifications:** HTML/JS Frontend + Slack API

## Setup Instructions
1. Import both JSON files from `/workflows` into your n8n instance.
2. Configure credentials for **OpenAI**, **Google Sheets**, and **Slack**.
3. Update the Google Sheet ID and Vector Store memory key across both workflows.
4. Copy the Production Webhook URL from Workflow 1 into `frontend/index.html`.
5. Publish both workflows and test via the web form and Slack `@mention`.