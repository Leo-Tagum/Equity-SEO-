# SOP #1: Add FAQPage Schema to FAQ Content

| | |
|---|---|
| **Status** | 🟢 Ready to delegate — fix this first |
| **Scope** | 128 pages flagged by the AI Readiness tool as "FAQ content without FAQPage schema" |
| **Tech stack** | WordPress + Elementor, hosted on WP Engine |
| **Owner (assigns work, unblocks issues)** | Project Lead |
| **Executors** | Team members (assigned a batch of URLs each) |
| **Tracker** | [Live Google Sheet, `task` tab](https://docs.google.com/spreadsheets/d/1zy-ye4GGKH1xWCMS2Ie0l1yn9sOHRdtSgtzHuOs2sGM/edit?gid=0#gid=0) — SSO-restricted to the Twinhomebuyer org, your team already has access. This *is* the ticket; there's no separate ticketing tool. (See [`faq-schema-tracker-template.csv`](./faq-schema-tracker-template.csv) for a plain-text mirror of the same columns.) |

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
1. The page has one `FAQPage` JSON-LD block, inserted via an Elementor **HTML
   widget**, containing every visible FAQ Q&A pair on the page.
2. The question/answer text in the schema **matches the visible on-page text exactly**
   (no paraphrasing, no added/removed words).
3. It passes [validator.schema.org](https://validator.schema.org/) with **zero errors**.
4. [Google's Rich Results Test](https://search.google.com/test/rich-results) shows
   the page as FAQ-eligible.
5. Cache is purged (WP Engine + Elementor) and, viewed in an incognito window, the
   page's HTML source actually contains the script block.
6. **Only after all of the above pass**, the tracker row (`task` tab) is set to
   `Progress = Done` with today's date — `Done` is the signal that every check
   above already happened, not just that the schema was pasted in.
7. It no longer appears in the AI Readiness tool's "FAQ without schema" report on
   the next scan.

## 3. Step-by-step (per page)

**Step 1 — Get your assigned URLs**
Open the [`task` tab](https://docs.google.com/spreadsheets/d/1zy-ye4GGKH1xWCMS2Ie0l1yn9sOHRdtSgtzHuOs2sGM/edit?gid=0#gid=0)
and find your section (rows are grouped by name). Pick the next row with no
`Progress` set, and set `Progress = ongoing` as you start it.

**Step 2 — Confirm it's genuinely FAQ content**
Open the page. Confirm there are real, visible question-and-answer pairs (an
"FAQ" section, an accordion, a Q&A block). If the page doesn't actually have
Q&A-formatted content (false positive from the tool), leave `Progress` as-is
(don't mark `Done`), note the reason in `Issue` (e.g. "not real FAQ content"),
and notify the Lead. Do not force-fit schema onto content that isn't a
question/answer.

**Step 3 — Copy the exact visible text**
No copy needs rewriting. For every Q&A pair on the page, copy the question
(heading) and answer text **verbatim** — same wording, same punctuation. Note the
question doesn't have to be phrased as a literal question ("How does X work?") —
a heading like "Why Some Sellers Choose an As-Is Cash Sale" is a valid FAQ
question for schema purposes as long as the text directly below it answers it.

Type the text into a **plain text editor (Notepad, VS Code)** before building the
JSON — never Google Docs. Google Docs silently converts straight quotes (`"`) into
curly quotes (`" "`), which breaks the JSON without any obvious error.

**Step 4 — Build the JSON-LD**
Use this template. Set `@id` to the **live URL of that exact page** + `#faq`, then
add one `Question` object per Q&A pair already on the page — just wrap the
existing heading and paragraph, don't rewrite either. Add or remove `Question`
blocks so the count matches the page exactly; don't invent questions.

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "@id": "https://www.example.com/this-exact-page-url/#faq",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "Exact visible question/heading text goes here",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Paste the answer already on the page here. Plain text only — no HTML tags."
      }
    },
    {
      "@type": "Question",
      "name": "Second question/heading on the page",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Paste that answer already on the page here too."
      }
    }
  ]
}
```

Strip any HTML tags out of every `text` value. Escape any double quotes that
appear *inside* an answer as `\"`. If a question on the page has no answer text
underneath it, **flag it** (see Escalation below) — don't write one yourself.

**Step 5 — Insert it in Elementor**
1. Edit the page in Elementor.
2. Scroll to the bottom of the page and add a new **container**. Keep it empty
   of styling — this is a wrapper, nothing visual goes in it.
3. Drag in the **HTML widget**. Not Text Editor, not Shortcode — Text Editor will
   escape the code and break it.
4. Paste the whole block from Step 4, wrapped in script tags, into the HTML widget:

```html
<script type="application/ld+json">
{ ...your JSON here... }
</script>
```

**Step 6 — Update / publish**
Save/publish the page in Elementor.

**Step 7 — Purge cache**
- WP Engine dashboard → your site → **Caching** → **Clear all caches**.
- If the page looks off after publishing, also go to **Elementor → Tools →
  Regenerate CSS & Data**.

Skipping this step is the most common way to "finish" a page and then fail your
own checks in Step 9 — you'd be looking at a cached, pre-fix version of the page.

**Step 8 — Confirm it actually landed**
Open the live page in an **incognito window**, view source (not the Elementor
editor), and confirm the `<script type="application/ld+json">` block is really
there, exactly as pasted. This is different from checking the FAQ is visible
(that's content; this is markup) — both need to be true.

**Step 9 — Validate**
Don't use the AI Readiness tool for this — it crawls the whole ~800-page site, so
it's too slow to re-check a single page as you finish it. Instead, per page:

1. **[validator.schema.org](https://validator.schema.org/)** — paste in the live
   page URL (or the JSON-LD directly), confirm **zero errors**. This is your fast
   self-check while you're still working.
2. **[Google Rich Results Test](https://search.google.com/test/rich-results)** —
   confirm the page shows as FAQ-eligible. Take a screenshot for your own
   record — the tracker doesn't have a column for it, but keep it in case a
   page gets questioned later.
3. **Visually confirm the FAQ is actually visible on the page** — every
   question/answer you put in the JSON-LD needs to really be there, displayed to
   a normal visitor (not hidden, not removed, not behind a login). Schema for
   content that isn't visibly on the page is invalid and can get the page
   penalized — this check matters as much as the validators passing.

**Step 10 — Update the tracker**
In the `task` tab, fill in today's `Date` and set `Progress = Done` — only once
every check in Step 9 has actually passed. `Done` on its own is read as "this page
is fully verified," so don't set it as a placeholder while something is still
pending.

**Step 11 — Lead's weekly re-scan**
Project Lead re-runs the AI Readiness tool weekly (this full-site crawl is a
Lead-only step, not per-page) and confirms completed pages have dropped off the
"FAQ without schema" report. Spot-check ~10% of "Done" rows against
validator.schema.org and the live page.

## 4. Rules (non-negotiable)

- **One `FAQPage` block per page.** Never stack duplicates.
- **Never copy another page's `@id`.** Always swap in the live URL of the exact
  page you're working on.
- **HTML widget only.** Text Editor and Shortcode widgets will escape or mangle
  the `<script>` tag.
- **Type JSON in a plain text editor**, never Google Docs (see Step 3 — silent
  curly-quote corruption).
- **Strip HTML tags** out of every answer `text` value; **escape internal quotes**
  as `\"`.
- **Don't touch the visible FAQ content** to make this fix. If the on-page copy
  changes later, the schema has to be updated to match — mismatched schema and
  content is worse than no schema at all.
- **If a question has no answer text under it, flag it** — don't write one.

## 5. Common mistakes (QA checklist before marking Done)

- [ ] Schema question/answer text matches the visible page text **exactly**
- [ ] Only **one** `FAQPage` block per page (don't stack duplicates)
- [ ] `@id` is this page's own live URL + `#faq` — not copied from another page
- [ ] Added via the **HTML widget**, not Text Editor or Shortcode
- [ ] Answer `text` is plain text — no `<b>`, `<a>`, or other HTML tags inside it
- [ ] Internal double quotes in answer text are escaped as `\"`
- [ ] Didn't mark up promotional copy, testimonials, or non-Q&A content as "questions"
- [ ] Cache purged (WP Engine +, if needed, Elementor regenerate)
- [ ] Viewed live page in incognito, view source confirms the script block is present
- [ ] Ran validator.schema.org and got zero errors
- [ ] Ran Rich Results Test, confirmed FAQ-eligible
- [ ] Opened the live page and visually confirmed every Q&A pair in the schema is actually displayed there
- [ ] Page still displays normally to users (schema is invisible markup — nothing on the visible page should change)

## 6. Escalation

If you hit any of these, stop — leave `Progress` as-is (don't mark `Done`), note
the reason in the `Issue` column, and notify the Project Lead directly. Don't guess:
- The page isn't real FAQ content (tool false positive).
- A question on the page has no answer text underneath it.
- The page already has a *different* `FAQPage` (or conflicting) schema block on it.
- You're unsure whether content counts as a "question" (e.g., a single Q&A buried in body text).
- The Elementor page doesn't allow adding a container/HTML widget (locked template, page builder restriction, etc.).
