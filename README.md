# Equity SEO — Team Docs & SOPs

This repo holds the Standard Operating Procedures (SOPs) and working notes for the
Equity SEO cleanup project — the effort to fix "AI Readiness" errors flagged by our
SEO/AI-readiness tool (the one wired up to our Google Cloud project, Google Search
Console, and Google Analytics 4).

**Who this is for:** you're the project lead, still learning the role, and your job
right now is to (1) delegate work the team can execute *without* needing you to
explain it live, and (2) keep researching new error types until you understand them
well enough to write the next SOP yourself.

## How this repo is organized

```
docs/
  sop/         Ready-to-delegate procedures — your team works from these.
  learning/    Your own research notes on errors not yet turned into an SOP.
```

- **`docs/sop/`** — Each file is a finished SOP: what the error means, why it
  matters, exact steps, a validation step, and a tracker so work is visible.
  Hand these to your team as-is.
- **`docs/learning/`** — Not for the team yet. This is where you build understanding
  before you write the next SOP. It has a repeatable method so "explore an error"
  always ends in something you could hand off.

## Current status

| Error | Status | Doc |
|---|---|---|
| FAQ content without FAQPage schema | 🟢 **Ready to delegate — fix first** (128 pages) | [`docs/sop/01-faq-schema-markup.md`](docs/sop/01-faq-schema-markup.md) |
| Page does not answer its own headline | 🔍 You're researching this | [`docs/learning/ai-readiness-error-notes.md`](docs/learning/ai-readiness-error-notes.md) |
| Pages open with the same words | 🔍 You're researching this | [`docs/learning/ai-readiness-error-notes.md`](docs/learning/ai-readiness-error-notes.md) |
| No concrete figures to quote | 🔍 You're researching this | [`docs/learning/ai-readiness-error-notes.md`](docs/learning/ai-readiness-error-notes.md) |

## What "AI Readiness" means here

Traditional SEO optimizes for ranking in a list of blue links. AI Readiness (also
called **AEO — Answer Engine Optimization**) optimizes for a *different* moment:
an AI system (Google AI Overviews, ChatGPT, Perplexity, a voice assistant) reading
your page and deciding whether to **quote, cite, or summarize it directly** as the
answer. That's a stricter bar — the AI has to be able to (a) find the exact
answer, (b) trust it's accurate, and (c) lift it cleanly without extra work. Most
of the errors this tool flags are different ways a page fails that bar. Keep that
one idea in mind — it makes every individual error make sense.
