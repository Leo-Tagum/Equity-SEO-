# SOP #1: Add FAQPage Schema to FAQ Content

| | |
|---|---|
| **Status** | 🟢 Ready to delegate — fix this first |
| **Scope** | 128 pages flagged by the AI Readiness tool as "FAQ content without FAQPage schema" |
| **Owner (assigns work, unblocks issues)** | Project Lead |
| **Executors** | Team members (assigned a batch of URLs each) |
| **Tracker** | [`faq-schema-tracker.csv`](./faq-schema-tracker-template.csv) — copy this, fill in the 128 URLs from the tool's export |

## 1. Why we're fixing this

Google (and other AI systems) can only turn your FAQ content into a rich result or
an AI-quoted answer if the Q&A pairs are marked up in **FAQPage structured data
(schema.org JSON-LD)**. Without it, the page's questions and answers are just
plain text — an AI system has to *guess* what's a question and what's the answer,
and usually skips it instead. This is flagged as **priority #1** because it's:

- **Mechanical, not creative** — no new content needs to be written, just markup added.
- **High volume, quick win** — 128 pages, same fix pattern every time.
- **A prerequisite** — some AI-readiness checks (like "does the page answer its own
  headline") are easier to verify once the Q&A structure is explicit in schema.

## 2. What "done" looks like (Definition of Done)

A page is done when **all** of the following are true:
1. The page has one `FAQPage` JSON-LD block containing every visible FAQ Q&A pair.
2. The question/answer text in the schema **matches the visible on-page text exactly**
   (no paraphrasing, no added/removed words).
3. It passes Google's [Rich Results Test](https://search.google.com/test/rich-results)
   with **zero errors** (warnings about eligibility are OK; hard errors are not).
4. The tracker row is marked `Done` with the date and validator link.
5. It no longer appears in the AI Readiness tool's "FAQ without schema" report on
   the next scan.

## 3. Step-by-step (per page)

**Step 1 — Get your assigned URLs**
Pull your batch from the tracker sheet (Lead assigns ~15–20 pages per person for
128 pages ÷ team size). Mark each row `In Progress` as you start it.

**Step 2 — Confirm it's genuinely FAQ content**
Open the page. Confirm there are real, visible question-and-answer pairs (an
"FAQ" section, an accordion, a Q&A block). If the page doesn't actually have
Q&A-formatted content (false positive from the tool), mark the row `Blocked –
not real FAQ content` and notify the Lead. Do not force-fit schema onto content
that isn't a question/answer.

**Step 3 — Copy the exact visible text**
For every Q&A pair on the page, copy the question and answer text **verbatim** —
same wording, same punctuation. This is a hard Google requirement: schema text
must match what a user actually sees.

**Step 4 — Build the JSON-LD**
Use this template. Add one `Question` object per Q&A pair on the page:

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "Exact visible question text goes here",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Exact visible answer text goes here. Plain text only — no HTML tags."
      }
    },
    {
      "@type": "Question",
      "name": "Second question on the page",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Second answer, again matching the visible text."
      }
    }
  ]
}
```

**Step 5 — Insert it on the page**
Add the JSON-LD inside a `<script type="application/ld+json">` tag, placed in the
page's `<head>` (or via the CMS's "custom schema / structured data" field if one
exists). Save as draft first if your CMS supports preview.

```html
<script type="application/ld+json">
{ ...paste the JSON-LD from Step 4 here... }
</script>
```

**Step 6 — Validate**
Run the live URL through **both**:
- [Google Rich Results Test](https://search.google.com/test/rich-results) — confirm "FAQ" is detected with 0 errors.
- [Schema Markup Validator](https://validator.schema.org/) — confirm no syntax errors.

Screenshot or copy the result link.

**Step 7 — Publish and update the tracker**
Publish the page. In the tracker, set `Status = Done`, fill in the date, paste the
validator link, and put your initials in `Notes`.

**Step 8 — Lead's weekly re-scan**
Project Lead re-runs the AI Readiness tool weekly and confirms completed pages
have dropped off the "FAQ without schema" report. Spot-check ~10% of "Done" rows
against the live Rich Results Test.

## 4. Common mistakes (QA checklist before marking Done)

- [ ] Schema question/answer text matches the visible page text **exactly**
- [ ] Only **one** `FAQPage` block per page (don't stack duplicates)
- [ ] Answer `text` is plain text — no `<b>`, `<a>`, or other HTML tags inside it
- [ ] Didn't mark up promotional copy, testimonials, or non-Q&A content as "questions"
- [ ] Ran both validators and got zero hard errors
- [ ] Page still displays normally to users (schema is invisible markup — nothing on the visible page should change)

## 5. Escalation

If you hit any of these, stop and flag the row `Blocked` with a reason, then
notify the Project Lead — don't guess:
- The page isn't real FAQ content (tool false positive).
- The CMS has no way to add custom `<script>` / structured data to this page type.
- The page already has a *different* schema type that conflicts.
- You're unsure whether content counts as a "question" (e.g., a single Q&A buried in body text).
