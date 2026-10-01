# FactSarkar

A fact-checking web app. Type in a claim and it looks the claim up in a database of published fact-checks, ranks the matches by how closely they fit, and has an AI analyst explain what the evidence says and give a one-line verdict.

> This is a fork of [AdvaitSamant/TruthScope](https://github.com/AdvaitSamant/TruthScope), built as a team project. Inside the app the product is branded **FactScope**.

## What it does

- **Looks up existing fact-checks.** Queries the [Google Fact Check Tools API](https://developers.google.com/fact-check/tools/api), which collects reviews from fact-checking publishers, and shows each match with the claimant, the publisher, their rating and a link to the source.
- **Ranks results by relevance.** Scores every match against your claim with a sentence-embedding model (`all-MiniLM-L6-v2`) and sorts by similarity, shown as a match percentage.
- **Explains the evidence.** "Vera", an AI analyst running on an LLM through [OpenRouter](https://openrouter.ai), reads the matches, writes a short analysis, then gives a one-sentence verdict: true, false, misleading or unverified.
- **Works in eight languages.** English, Hindi, Marathi, French, German, Spanish, Chinese (Simplified) and Japanese, translated on the fly.
- **Exports PDF reports.** Download a report for a single check, or one covering every check in your session.

## How it works

```
your claim
   │
   ├─► Google Fact Check Tools API ──► published fact-checks
   │                                        │
   ├─► sentence-transformers ──────────────►│ ranked by similarity
   │                                        ▼
   └─► LLM via OpenRouter ("Vera") ──► analysis + verdict ──► PDF report
```

Everything lives in `app.py`, a single Streamlit app with a landing page and the fact-check page.

## Getting started

You need Python 3.9+ and two API keys:

- a **Google Fact Check Tools API** key, from the [Google Cloud console](https://console.cloud.google.com/apis/library/factchecktools.googleapis.com)
- an **OpenRouter** API key, from [openrouter.ai/keys](https://openrouter.ai/keys)

1. Clone the repo and install the dependencies:

   ```bash
   git clone https://github.com/PiyushB046/FactSarkar.git
   cd FactSarkar
   pip install -r requirements.txt
   ```

2. Create `.streamlit/secrets.toml` with your keys. This file is git-ignored; never commit it.

   ```toml
   GOOGLE_FACT_CHECK_API_KEY = "your-google-key"
   LLM_API = "your-openrouter-key"
   ```

3. Run the app:

   ```bash
   streamlit run app.py
   ```

The first run downloads the embedding model, so it takes a little longer.

## Limitations

- It can only find claims that a fact-checking publisher has already reviewed. New or niche claims often return no matches, and the AI verdict then has no evidence to lean on.
- The AI analysis is a summary of the matches, not an independent investigation. Check the linked sources before relying on a verdict.

## Tech stack

Python · Streamlit · sentence-transformers · Google Fact Check Tools API · OpenRouter · deep-translator · ReportLab

## License

MIT. See [LICENSE](LICENSE).
