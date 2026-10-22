---
name: content-draft
description: Write a full draft for a content piece that already has a brief. Reads the brief and relevant Binders, shows a summary for review, then writes a complete draft named "Draft - <Title>.md" to the piece folder. Use after content-brief when the brief has been reviewed and you're ready to write.
disable-model-invocation: true
---

# Content Draft

Write a full draft from an existing brief. Requires a piece folder created by `content-brief`.

## Context

Each content piece lives in `Notes/Content/YYYY-MM-DD <safe title>/` with:
- `index.md`: goal, expected results, current status
- `Brief - <Title>.md`: audience, angle, outline, Binder refs
- `Draft - <Title>.md`: written by this skill

The Binders are the authoritative source for company voice, positioning, and product details. Always re-read them before writing. Do not rely on memory of earlier sessions.

## Use `humanize-writing`

Use `humanize-writing` so the draft keeps the brand voice and avoids generic AI phrasing.

1. In Step 2, read the core `humanize-writing` skill fully and follow its embedded workflow. It owns the instructions for finding and applying Voice & Tone.
2. Give it the brief and relevant Binders as the grounded source material for the draft.
3. Before Step 5, run its entire Pattern review and then its Final check on the whole draft. Fix every problem they find.
4. Write only the revised draft to the file. Do not add a style report or checklist to the draft.

---

## Step 1: Find the piece

Accept either:
- A path to the piece folder (e.g. `Notes/Content/2026-06-02 Context Fragmentation/`)
- A slug or partial title: search `Notes/Content/` for a matching folder
- A path to the brief file or `index.md` directly

Read `index.md` fully. The `brief` field in its frontmatter gives the exact filename of the brief. Read that file too.

---

## Step 2: Re-read Binders

Read every Binder listed in the brief file's frontmatter under `binders_used`. Follow `humanize-writing` for the brand voice instructions.

If a listed Binder file doesn't exist or has moved, check `Binders/index.md` to find the current equivalent, or use the `binders` skill (lookup task) and read that instead.

---

## Step 3: Confirm before writing

Present a short summary to the user before writing anything:

- **Goal** (from `index.md`)
- **Angle** (from the brief)
- **Planned outline** (H2 headings from the brief)
- **Binders being used** (list)
- **Approximate target length** (based on content type, see Step 4)

Ask: "Any adjustments or additional direction before I start writing?"

Wait for the user's response. If they request changes:
- Update the brief file to reflect the adjustments before continuing
- Do not start writing until the user confirms

---

## Step 4: Write the draft

Apply the writing approach for the content type listed in `index.md`:

| Type | Approach | Target length |
|---|---|---|
| `blog` | Narrative, brand voice, educational arc. First or second person where natural. | 800–1500 words |
| `comparison` | Structured and balanced. Intro, comparison section (table or parallel prose), verdict. Fair to alternatives. | 1000–1800 words |
| `listicle` | Numbered sections. Each item self-contained with a clear takeaway. Scannable. | 600–1200 words |
| `how-to` | Numbered steps. Clear prerequisites up front. Each step is concrete and actionable. | 800–1500 words |
| `glossary` | Short definitional lead sentence. Then context, examples, and any related terms. | 400–700 words |
| `case-study` | Problem → solution → results arc. Specific and concrete. If using a representative scenario, keep it grounded in real ICP needs from Binders. | 700–1200 words |
| `use-case` | Audience's problem first, then the product solution. Reads like a focused landing page. Clear CTA at the end. | 600–1000 words |

**Voice rules** (derived from reading the voice and tone Binder; these are examples of the kind of guidance to apply, not a substitute for reading it):
- Follow the company's voice as described in the Binders
- Do not invent product claims, features, or customer quotes not grounded in Binder content
- Do not use filler phrases like "In today's world..." or "As we all know..."
- End with a clear CTA or takeaway that connects back to the goal

---

## Step 5: Write the draft file

Determine the safe title from `index.md` (the title with filename-unsafe characters removed: `:`, `?`, `/`, `"`, `*`, `|`, `<`, `>`, `\`).

Write `Notes/Content/<folder>/Draft - <safe title>.md`:

```markdown
---
piece: ../index.md
brief: ./Brief - <safe title>.md
status: draft
---

<full draft content here>
```

---

## Step 6: Update piece `index.md`

Update the piece's `index.md` frontmatter:
- Set `status: draft`
- Set `draft: Draft - <safe title>.md`

---

## Step 7: Update `Notes/Content/index.md`

Update the row for this piece: change the Status column from `brief` to `draft`.

---

## Step 8: Report back

Tell the user:
- Draft written to `Draft - <safe title>.md`
- Word count (approximate)
- Which Binders were used
- Anything from the brief that couldn't be addressed (e.g. a claim requiring customer data that doesn't exist in the Binders)
- Suggested next step: review the draft, then run `/content-publish` when ready
