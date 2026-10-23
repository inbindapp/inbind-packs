---
name: content-publish
description: Prepare a reviewed draft for publishing. Reads "Draft - <Title>.md" and "Brief - <Title>.md", re-checks voice against Binders, polishes the piece, and writes the final clean version to the correct Content/ subfolder. Updates all index files and closes the loop back to the original brief. Use when a draft has been reviewed and is ready to ship.
disable-model-invocation: true
---

# Content Publish

Move a reviewed draft from `Notes/Content/` to `Content/` as a publish-ready piece. Requires a piece folder with both `Brief - <Title>.md` and `Draft - <Title>.md`.

## Context

The project uses three layers:
- `Binders/`: authoritative company knowledge (voice, positioning, product, market)
- `Notes/Content/<folder>/`: work in progress (brief and draft)
- `Content/<Type>/`: published, ship-ready content

This skill is the final stage. It polishes the draft, writes the clean output, and updates all tracking files to reflect the published state.

## Use `humanize-writing`

The final polish can reintroduce generic AI phrasing even when the draft was clean. Use `humanize-writing` so the published version keeps the brand voice.

1. In Step 2, read the core `humanize-writing` skill fully and follow its embedded workflow. It owns the instructions for finding and applying Voice & Tone.
2. Use its revision mode while polishing the draft in Step 3.
3. Before writing the published file, run its entire Pattern review and then its Final check on the whole publishable article, including passages you did not change. Confirm that the meaning and grounded claims did not change.
4. Publish only the revised content. Do not add a style report or checklist to the article.

---

## Step 1: Find the piece

Accept either:
- A path to the piece folder (e.g. `Notes/Content/2026-06-02 AI Writing Problems/`)
- A slug or partial title: search `Notes/Content/` for a matching folder
- A path to `draft.md` or `index.md` directly

Read `index.md` fully. The `brief` and `draft` fields in its frontmatter give the exact filenames. Read both files.

If the draft file is missing or `status` in `index.md` is still `brief`, stop and tell the user: this piece hasn't been drafted yet. Suggest running `/content-draft` first.

---

## Step 2: Re-read Binders for voice check

Follow `humanize-writing` for the brand voice instructions. Also re-read any positioning or key message Binders listed in the brief file to ensure the final piece still reflects current company positioning and hasn't drifted during drafting.

---

## Step 3: Polish the draft

Make the following improvements before writing the final file:

**Opening**: Does the first sentence earn the reader's attention? Tighten if it starts with setup or context instead of the hook.

**Heading hierarchy**: Verify H2 and H3 headings are logical and scannable. Remove any headings that are just noise.

**Voice consistency**: Read for tone against the voice Binder. Adjust any sections that drift into generic or corporate language.

**CTA / closing**: Does the piece end with a clear action or takeaway? It should connect back to the goal stated in `index.md`. If it doesn't, rewrite the closing.

**Internal scaffolding**: Remove any notes-to-self, bracketed placeholders (e.g. `[TODO]`, `[add example here]`), or draft-stage comments left in the text.

Do not rewrite the substance of the piece. Polish only. If you notice a significant structural issue (missing section, weak argument), flag it to the user rather than silently fixing it.

---

## Step 4: Determine output path

Map the `type` from `index.md` to the output folder:

| Type | Output folder |
|---|---|
| `blog` | `Content/Blog Posts/` |
| `comparison` | `Content/Comparison Articles/` |
| `listicle` | `Content/Listicles/` |
| `how-to` | `Content/How-to Guides/` |
| `glossary` | `Content/Glossary/` |
| `case-study` | `Content/Case Studies/` |
| `use-case` | `Content/Use Case Pages/` |

If the type is anything else, ask the user which `Content/` subfolder to use. Create the subfolder if it doesn't exist.

Output filename: `YYYY-MM-DD <slug>.md` using today's date and the slug from the folder name.

---

## Step 5: Write the published file

Write to `Content/<Type>/YYYY-MM-DD <slug>.md`:

```markdown
---
type: <type>
status: published
published: <today's ISO date>
piece: Notes/Content/<folder>/index.md
---

<polished content, no internal scaffolding, no draft markers>
```

The content should read as a standalone, finished piece. No references to the brief process, no internal notes.

---

## Step 6: Update piece `index.md`

Update the piece's `index.md` frontmatter:
- Set `status: published`
- Set `published_at: <today's ISO date>`
- Set `published: Content/<Type>/YYYY-MM-DD <slug>.md`

---

## Step 7: Update `Content/index.md`

Add a row to the tracking table in `Content/index.md`:

```
| <Title> | <type> | <today's date> | published | [brief](Notes/Content/<folder>/index.md) |
```

If `Content/index.md` doesn't have a column for the brief link, add it to the header row as well.

---

## Step 8: Update `Notes/Content/index.md`

Update the row for this piece: change the Status column to `published`.

---

## Step 9: Report back

Tell the user:
- Final file written to: `Content/<Type>/YYYY-MM-DD <slug>.md`
- What was polished (brief description of changes made)
- Any issues flagged but not fixed (e.g. structural gaps)
- The original goal from the brief (so they have it in view for measuring results later)
- Reminder: check back against the expected results in `Notes/Content/<folder>/index.md` after the piece has been live for a while
