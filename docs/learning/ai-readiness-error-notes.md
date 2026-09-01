# Learning Notes: AI Readiness Errors (not yet delegated)

**Purpose of this file:** this is *your* scratchpad, not a team doc. Each section
below is one error the tool flagged that you haven't turned into an SOP yet. Work
through the questions in each section using the actual flagged pages + Google
Search Console + GA4 + the AI Readiness tool. Once a section is filled in and you've
piloted the fix on a handful of pages, copy it into `docs/sop/` as a real numbered
SOP (use `01-faq-schema-markup.md` as the template for structure).

## The method: turning "an error the tool flagged" into an SOP

Don't skip straight to writing steps — that's how you end up with an SOP the team
can't actually follow. Work in this order:

1. **See it yourself.** Open 5–10 pages the tool flagged for this error. Look at
   what they have in common.
2. **Understand *why* the tool flags it.** Connect it back to the one idea in the
   README: can an AI system find, trust, and cleanly lift an answer from this page?
   Which part of that breaks?
3. **Find the root cause pattern.** Is it a content problem (someone wrote it
   wrong), a template problem (every page built from the same template has this
   flaw), or a process problem (no one checks for this before publishing)? The fix
   — and who can do it — is different for each.
4. **Draft a fix on one page.** Make the change, re-run the tool (or the relevant
   validator) on just that page, confirm the flag clears.
5. **Pilot on 5 pages.** Different authors/templates if possible. See where your
   draft steps are ambiguous — that ambiguity is exactly what will trip up the team.
6. **Write the SOP.** Definition of done, numbered steps, a validation step, a
   tracker, an escalation list for "when this doesn't apply." Mirror the structure
   of SOP #1.
7. **Hand off, spot-check, re-scan.** Same as SOP #1's weekly re-scan step.

---

## 1. "Page does not answer its own headline"

**What it likely means:** the H1 (or title tag) promises something — often phrased
as a question or a specific claim — and the body content doesn't actually deliver
a clear, direct answer to it. An AI system reads the headline as "what is this page
about," scans for the answer, and doesn't find one it can confidently quote.

**Why an AI system cares (more than a human skimmer might):** a human will keep
reading and infer the answer from context. An AI answer engine is looking for a
short, extractable passage that directly matches the implied question — if the
first few hundred words don't contain it, it moves on to a competitor's page.

**Investigate — pull up several flagged pages and check:**
- [ ] Is the headline phrased as a question or a claim (e.g., "How does X work?" / "X is the best Y")?
- [ ] Does the first paragraph *directly* answer it, or does the page open with
      throat-clearing (company background, a story, a definition of an unrelated term) first?
- [ ] Is the actual answer present *somewhere* on the page, just buried below the fold?
- [ ] Is this a template-wide issue (e.g., every blog post opens with the same
      "In today's world..." intro) or a one-off per page?

**Questions to answer before writing the SOP:**
- What's the fix pattern — rewrite the opening paragraph? Add a short direct-answer
  summary box right under the H1? Both?
- Is this something writers can self-check with a rule of thumb ("your headline's
  answer should appear in the first 2 sentences"), or does it need a rewrite pass?
- Does GA4 show these pages have high bounce/low scroll depth — i.e., does the data
  back up that people (and AI) aren't finding the answer either?

**Draft SOP shape (fill in once researched):**
1. Read the headline, write down the implicit question it's asking.
2. Check if the first ~150 words answer it directly. If not, rewrite the opening
   to lead with the answer, then follow with supporting detail/context.
3. Validate: could you paste the first paragraph into a search result as an answer
   snippet and have it make sense on its own?
4. Track pages fixed, re-scan.

---

## 2. "Pages open with the same words"

**What it likely means:** multiple pages (often from the same template — product
pages, location pages, blog posts) start with near-identical boilerplate ("Welcome
to [Brand]..." / "Looking for the best..."). This makes it hard for an AI system
(and Google) to tell pages apart or know which one is the authoritative answer for
a given query — everything looks like duplicate content at the start.

**Why an AI system cares:** if the opening lines are generic, the AI can't use
them to judge relevance or uniqueness, and duplicate-sounding openings across many
pages can suppress how many of *your* pages get considered as sources at all.

**Investigate:**
- [ ] Pull 10 flagged pages — literally paste their first sentence into a doc side
      by side. How similar are they, word for word?
- [ ] Is this coming from a CMS template default (e.g., an auto-generated intro
      sentence) or from writers reusing a formula?
- [ ] Which pages are highest-traffic/highest-value (check GA4) — those are your
      priority rewrites, not necessarily the full 128-style batch-fix.

**Questions to answer before writing the SOP:**
- Is the fix "rewrite the opening sentence to be page-specific" (content work,
  needs a writer) or "fix the template that auto-generates the intro" (one dev fix
  that solves many pages at once)? Check the template angle first — it may turn a
  128-page problem into a 1-template problem.
- What should a *good*, page-specific opening line contain (the specific
  product/place/topic name, a specific detail — not just swapping in a variable)?

**Draft SOP shape (fill in once researched):**
1. Identify whether the duplication is template-driven or writer-driven.
2. If template-driven: fix the template's intro-generation logic once; re-scan all
   affected pages.
3. If writer-driven: rewrite the opening sentence per page to be specific to that
   page's actual topic (name the entity, don't use the generic formula).
4. Validate: opening sentence should be unique enough that you could identify which
   page it's from without seeing the URL.

---

## 3. "No concrete figures to quote"

**What it likely means:** the page makes claims without specific, citable
numbers — stats, prices, dates, percentages, counts. ("We offer great rates" vs.
"Rates start at 4.5% APR.") AI systems (and journalists, and users) strongly
prefer to quote specific figures because they're checkable and useful as a direct
answer; vague claims don't get quoted.

**Why an AI system cares:** a concrete figure is exactly the kind of short,
self-contained fact an AI answer engine wants to lift verbatim ("X starts at $Y,"
"X takes N days"). A page with no numbers has nothing extractable to cite, even if
the surrounding claims are true.

**Investigate:**
- [ ] On flagged pages, are there real numbers the business *has* (pricing, dates,
      percentages, counts, timeframes) that just weren't written into the page copy?
- [ ] Or is the vagueness intentional (e.g., pricing that legitimately varies)? If
      so, what's the closest honest concrete substitute (a range, a "starting at," a
      typical timeframe)?
- [ ] Who owns the source data for these figures — is it something you (SEO/content)
      can add, or do you need it from another team (finance, product, ops)?

**Questions to answer before writing the SOP:**
- Where do accurate, up-to-date figures come from, and how do we make sure they
  don't go stale (a hardcoded "as of 2024" number left uncorrected two years later
  is worse than no number)?
- What's the minimum bar — one concrete figure per page? Per major claim?

**Draft SOP shape (fill in once researched):**
1. Read the page, list every vague claim that could instead cite a number.
2. Source the real figure from the accurate internal source (not a guess).
3. Rewrite the sentence to lead with the number ("Setup takes 10 minutes" instead
   of "Setup is quick and easy").
4. Add a "figures last verified: [date]" note if the source data changes often, so
   someone owns keeping it current.
5. Validate: could this sentence be quoted as a standalone fact and still be true?

---

## Mini glossary (for you, not the team doc)

- **AEO (Answer Engine Optimization):** optimizing content to be directly quoted/
  cited by AI systems (AI Overviews, ChatGPT, Perplexity), not just ranked in a
  list of links.
- **Structured data / schema.org / JSON-LD:** machine-readable markup added to a
  page (invisible to users) that tells search engines exactly what a piece of
  content *is* (a question, an answer, a price, a review) instead of making them
  guess from plain text.
- **Rich result:** an enhanced Google search result (stars, FAQ dropdown, price)
  made possible by structured data.
- **GSC (Google Search Console):** shows how Google sees and ranks your pages —
  impressions, clicks, indexing issues.
- **GA4 (Google Analytics 4):** shows how *users* behave on your pages — traffic,
  bounce, scroll/engagement — useful for confirming whether a "content quality"
  error (like the three above) is actually costing you engagement.
