# Architecture

## Data flow

Trigger → Scrape → Clean → Batch → LLM analysis → Parse → Aggregate → Format → Sheet

## Design decisions

- 2–4 star reviews only: most honest, detailed feedback
- Code-based filter: length, spam phrases, duplicates (no LLM cost)
- Batching: keeps LLM context focused; aggregator merges results
- Quality gate inside Filter Reviews: n8n skips nodes with 0 input items,
  so the gate must live in a node that always executes
- RunID column: every execution is traceable in the client sheet

## Swap points

- Scrape Reviews: replace mock data with Apify/Firecrawl call (same schema)
- AI Aspect Analysis: any OpenAI-compatible endpoint (Groq, OpenAI, OpenRouter)
- Output: Google Sheets node can be replaced with Notion / email / dashboard