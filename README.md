# ReviewPilot Lite

An n8n workflow that turns a competitor's customer reviews into a business-ready
intelligence report: exploitable weaknesses, product gaps, and table stakes.

## What it does

- Client submits a competitor product URL via a form
- Scrapes 2–4 star reviews (mock scraper included; swappable for Apify/Firecrawl)
- Removes fake, short, and duplicate reviews
- Runs aspect-based sentiment analysis with an LLM (Groq / OpenAI-compatible)
- Merges multi-batch insights with summed mention counts
- Appends a client-ready report to Google Sheets
- Fails loudly (quality gate) when there is not enough clean data

## Workflow file

Import into n8n:

    workflow/reviewpilot-lite.json

## Required credentials

- Groq (or any OpenAI-compatible) API key — used in the AI Aspect Analysis header
- Google Sheets OAuth2 — used by the Append node

After importing, replace these placeholders:

- `Bearer REPLACE_WITH_GROQ_KEY`
- `REPLACE_WITH_SHEET_ID`

## Google Sheet setup

Create a sheet with tab `Insights` and this header row:

    Timestamp | Competitor | ProductURL | Platform | InsightType | Feature |
    Insight | MentionCount | Severity | Evidence | RecommendedAction | RunID

## Node map

Form Trigger → Set Inputs → Scrape Reviews → Filter Reviews →
Create Review Batches → Build AI Request → AI Aspect Analysis →
Normalize AI Output → Parse AI JSON → Aggregate Insights →
Format Output Rows → Append to Google Sheets

## Prompts

See `prompts/aspect-based-sentiment.md`.

## Legal note

Ensure you have the right to collect and process review data. Comply with
platform terms and applicable privacy laws. Store no reviewer personal data.