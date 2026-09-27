# Website Opportunity Scanner

A prototype tool that scores a small business website against 7 common weaknesses and flags how much it would benefit from a rebuild.

## What it does

Paste in a website URL and the app sends it to an LLM (via [OpenRouter](https://openrouter.ai)), which scores the site 0–10 on seven signals known to hurt customer acquisition. The scores are totaled into an opportunity tier — Low, Medium, or High — along with a short summary and a recommendation for what a rebuild should focus on.

This is Level 2 of the internship project: a working prototype of the "Website Opportunity Scanner" concept developed during Stage 1–4 research.

## The 7 signals

| Signal | What it flags |
|---|---|
| Unclear value proposition | Homepage doesn't say what the business does within 5 seconds |
| No clear call-to-action | No visible Book Now / Get Quote / Call Us / Contact button |
| Weak trust signals | No reviews, testimonials, case studies, or certifications |
| Poor mobile layout | Text too small, buttons hard to tap, layout breaks on phone |
| Confusing service structure | Too many menu items, unclear navigation, services buried |
| No contact information | No phone, email, address, or contact form visible |
| Slow loading speed | Heavy images, plugins, or slow hosting delay the page |

Each is scored 0 (no issue) to 10 (severe issue). The total (out of 70) maps to a tier:

- **0–24** — Low opportunity
- **25–49** — Medium opportunity
- **50–70** — High opportunity

## How it works

```
React App  →  OpenRouter API Gateway  →  GPT-OSS LLM
```

- The **system prompt** describes the rubric and the exact JSON shape the response must follow.
- The **user prompt** is just the URL being scanned.
- The response is parsed and clamped into safe values before it touches the UI, so a malformed response can't break the table.

## Tech stack

- [Create React App](https://create-react-app.dev/)
- [OpenRouter](https://openrouter.ai) as the LLM API gateway
- `openai/gpt-oss-20b` for scoring
- No CSS framework — everything is plain inline styles in `src/App.js`

## Getting started

1. Clone the repo and install dependencies:
   ```
   npm install
   ```
2. Create a file named `.env.local` in the project root with your own OpenRouter API key:
   ```
   REACT_APP_OPENROUTER_API_KEY=your_key_here
   ```
   Get a key at [openrouter.ai/settings/keys](https://openrouter.ai/settings/keys). This file is already excluded from git via `.gitignore` — never commit it.
3. Start the dev server:
   ```
   npm start
   ```
4. Paste a business URL into the app and click Scan.

## A note on the API key

Because this is a client-only React app, the API key is bundled into the JavaScript sent to the browser — it's kept out of GitHub, but not out of the deployed app itself. Anyone using the hosted version could find it in browser dev tools. For a prototype this is an accepted tradeoff; a production version would route the OpenRouter call through a small backend instead.

## Project structure

Everything lives in a single file, `src/App.js` — the rubric data, the OpenRouter call, and all styling. This was a deliberate choice for a fast-moving prototype; it would be split into components for a production build.
