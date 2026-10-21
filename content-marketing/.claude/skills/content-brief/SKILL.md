---
name: content-brief
description: Start a new content piece. Walks through topic, goal, and expected results one question at a time, recommends the best-fitting content type, then reads relevant Binders for company context and writes a structured brief to Notes/Content/<slug>/. Creates the piece folder with index.md (goal + expected results) and "Brief - <Title>.md" (outline, angle, Binder refs). Use when you want to plan a new blog post, comparison article, listicle, how-to guide, glossary entry, case study, or use case page.
disable-model-invocation: true
---

# Content Brief

Create a structured brief for a new content piece. Every piece starts here before any writing begins.

## Context

The project uses three layers:
- `Binders/`: structured company knowledge (brand, product, market, company context). This is the authoritative source for all content.
- `Notes/Content/`: content in progress (briefs and drafts)
- `Content/`: published, ship-ready pieces

Content briefs live at `Notes/Content/YYYY-MM-DD <safe title>/`. Each folder contains:
- `index.md`: the permanent project record: goal, expected results, status, links
- `Brief - <Title>.md`: the working document: audience, angle, outline, key messages

The master index at `Notes/Content/index.md` tracks all pieces.

## Use `humanize-writing`

Use `humanize-writing` so the brief follows the brand voice without falling back to generic AI phrasing.

1. As soon as the topic is known, read the core `humanize-writing` skill fully and follow its embedded workflow. It owns the instructions for finding and applying Voice & Tone.
2. Use it for the options proposed in Step 1 and for all prose written into the brief.
3. Before saving, run its entire Pattern review and then its Final check on the whole brief. Fix every problem they find.
4. Save only the revised brief. Include the Voice & Tone file that was read in `binders_used`.

The short setup questions in Step 1 are workflow prompts, not article copy. Keep them plain and concise; do not delay the questions to run a brand-style review.

---

## Step 1: Gather inputs (guided, one question at a time)

Walk the user through the inputs one step at a time. Do the thinking for them: at each step, propose concrete options and let them pick or supply their own. Do not advance to the next sub-step until the current one is answered.

**How to ask (important `AskUserQuestion` rules):**
- **One sub-step per call.** Each `AskUserQuestion` call carries a single question. Never batch 1b through 1e into one call.
- **2 to 4 options, always concrete.** Every question needs between 2 and 4 real options. Never send a single option, and never exceed 4.
- **Do not add a "type my own" option.** The tool always appends an "Other" free-text choice automatically, so an explicit placeholder option is both redundant and, on its own, an invalid single-option list.
- **Fully open inputs use plain text, not the tool.** When there is nothing meaningful to pre-fill (the opening topic), just ask in a normal message.

**1a. Topic (plain text, no tool).** If the user already gave a topic when invoking the skill, use it. Otherwise ask plainly in a normal message: "What is this piece about?" Do not use `AskUserQuestion` here; topics are open-ended and there is nothing to pre-fill yet.

**1b. Sharpen the angle (propose options).** A specific angle or question is far better than a broad subject. From the raw topic, generate 2 to 4 sharper, more specific angles the piece could take and offer them with `AskUserQuestion` (header "Angle"). Each option is a one-line framing that turns the broad subject into a specific take. The user picks one or uses "Other" to write their own. Record the chosen angle; it carries into the brief's Angle section later.

**1c. Goal (propose options).** Offer 2 to 4 goal framings tailored to the topic with `AskUserQuestion` (header "Goal"). Draw from: rank for a target keyword or search intent, drive product sign-ups or trials, build awareness or thought leadership, support sales enablement, educate or nurture existing users. Pick the handful most relevant to this topic rather than listing all of them. The user picks one or uses "Other". If the chosen goal needs a specific (which keyword, which audience), ask one short follow-up to capture it.

**1d. Recommend the content type (recommend, allow override).** Do not ask the user to choose a format cold. Infer the best-fit type from the topic, angle, and goal using the heuristic below, then present it with `AskUserQuestion` (header "Format"). Make the recommended type the first option, label it "(Recommended)", and give a one-line "why this fits". Then add only the 2 or 3 next-most-plausible types as the remaining options (never all seven, since the tool caps at 4 options), so the user can override. Resolve to exactly one `type` from: `blog`, `comparison`, `listicle`, `how-to`, `glossary`, `case-study`, `use-case`.

  Inference heuristic (topic and angle plus goal, resolving to a type):
  - Evaluation or "X vs Y" intent, or a sales-support goal framed against alternatives: `comparison`
  - "How to", setup, tutorial, or step-by-step intent: `how-to`
  - "What is", definitional, single-term intent: `glossary`
  - "Best X", "N ways / tools / tips", or otherwise enumerable intent: `listicle`
  - Customer story, results, or proof intent: `case-study`
  - Audience plus a job-to-be-done, "for <role>", or landing/conversion intent: `use-case`
  - Educational, narrative, awareness, or thought leadership (default fallback): `blog`

  When two types are plausible, let the goal break the tie: sales support favours `comparison`, `use-case`, or `case-study`; SEO ranking favours the informational type that matches the search intent (`how-to`, `glossary`, `listicle`, or `comparison`); awareness or thought leadership favours `blog`.

**1e. Expected results (propose options, multi-select).** Propose 2 to 4 concrete, measurable results tied to the goal from 1c with `AskUserQuestion` (header "Success", `multiSelect: true`). Make them specific: for a ranking goal, "reach page 1 for <keyword> within N months"; for sign-ups, "N trial sign-ups attributed to this piece"; for awareness, a concrete traffic or engagement target. The user can select several or use "Other" to add their own. Record the selections as the `expected_results` list.

Do not proceed to Step 2 until topic, a sharpened angle, goal, expected results, and a confirmed content type are all captured.

---

## Step 2: Read Binders

Read `Binders/index.md` first to understand what sections and pages exist. Then read the Binders most relevant to grounding the brief.

**For every brief, find and read:**
- Key messages or positioning statements
- Ideal customer profile (ICP) or customer segments
- Product positioning and how the product is described

**Additionally, based on content type:**
- `comparison`, `use-case`: also read competitive landscape and product overview Binders
- `how-to`, `glossary`: also read product features and use case Binders
- `case-study`: also read use case and company mission/vision Binders
- `blog`: also read any Binders relevant to the topic (e.g. market trends, company story)

Use `Binders/index.md` and the section index files to navigate. Track every Binder file you actually read and list them in the brief frontmatter.

If a page you need is missing or empty, tell the user and offer to create it with the `binders` skill before continuing.

---

## Step 3: Determine the title, slug, safe title, and paths

From the topic and content type, decide on:
- **Title**: the full human-readable title of the piece (e.g. "Context Fragmentation: The Unsolved Problem in AI Content Workflows")
- **Safe title**: the title with characters that are invalid in filenames removed (strip `:`, `?`, `/`, `"`, `*`, `|`, `<`, `>`, `\`). Used for folder and file names. (e.g. `Context Fragmentation The Unsolved Problem in AI Content Workflows`)
- **Slug**: URL-friendly version of the title: lowercase, hyphens for spaces, no punctuation, under 50 characters. Used only for the final published file in `Content/`. (e.g. `context-fragmentation-ai-content-workflows`)

Paths:
- Folder: `Notes/Content/YYYY-MM-DD <safe title>/` where the date is today
- Brief file: `Brief - <safe title>.md`
- Draft file (when created later): `Draft - <safe title>.md`

---

## Step 4: Write `index.md`

Write `Notes/Content/<folder>/index.md`:

```markdown
---
title: <full title of the piece>
type: <blog|comparison|listicle|how-to|glossary|case-study|use-case>
status: brief
created: <today's ISO date>
published_at:
goal: <one-sentence goal from the user>
expected_results:
  - <result 1>
  - <result 2>
brief: Brief - <safe title>.md
draft:
published:
---

# <Title>

> **Goal:** <one-sentence goal>

> **Expected results:** <bullet list of results>

This piece is at the **brief** stage. See `Brief - <safe title>.md` for the outline and approach.
```

---

## Step 5: Write the brief file

Write `Notes/Content/<folder>/Brief - <safe title>.md`:

```markdown
---
piece: ../index.md
status: brief
binders_used:
  - <list every Binder file you read, as relative paths>
---

# Brief: <Title>

## Audience

<Who is this written for? Reference the ICP from Binders: what do they care about, what problem are they trying to solve, what language do they use?>

## Angle

<The specific take or hook for this piece. Should be differentiated: not just "what" but "why it matters from the company's perspective". Ground this in positioning and key messages from the Binders.>

## Outline

<H2-level section structure with one-line description per section. Structure should match the content type:>
- Blog: 4–7 sections, narrative arc
- Comparison: intro + feature-by-feature or approach comparison + verdict
- Listicle: intro + 5–10 numbered items + conclusion
- How-to: prerequisites + numbered steps + summary
- Glossary: definition + context + examples + related terms
- Case study: situation + challenge + solution + results
- Use case: who this is for + the job-to-be-done + how the product solves it + CTA

## Key messages to land

<3–5 specific messages this piece must convey. Pull directly from Binder content, quoting or paraphrasing faithfully. Do not invent positioning.>
```

Content-type notes for the Angle and Outline:
- **Comparison**: Be honest and fair. Lead with the company's distinctive approach rather than feature lists. Use the competitive positioning from Binders to frame alternatives.
- **Glossary**: Write for someone searching "what is X". Lead with a crisp definition, then add context that connects to why the company exists in that space.
- **Case study**: Use concrete specifics wherever possible. If real customer data isn't available, frame as a representative scenario grounded in real ICP needs from the Binders.
- **Use case**: This reads like a mini landing page. Lead with the audience's problem, not the product.

---

## Step 6: Update `Notes/Content/index.md`

If `Notes/Content/index.md` does not exist, create it:

```markdown
# Content

Master index of all content pieces across stages.

| Folder | Title | Type | Goal | Status |
|---|---|---|---|---|
```

Add a row for the new piece:

```
| [<folder>](<folder>/) | <Title> | <type> | <one-line goal> | brief |
```

---

## Step 7: Report back

Confirm to the user:
- Folder created and files written
- Binders read (list them)
- What the brief says the piece is trying to achieve
- Suggested next step: run `/content-draft` pointing at this folder when ready to write
