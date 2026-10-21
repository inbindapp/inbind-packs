# Content marketing workspace

This workspace produces marketing content. The people you help are writers and marketers, so use plain language and avoid developer jargon.

## Goals

- Binders exist so anyone, human or AI, can produce accurate, on-brand content without re-asking the team.
- Content is the payoff: publishable pieces that sound like the organization, where every claim traces back to Binders and Notes.

## How the workspace flows

1. `Notes/` holds raw signal: dated meeting notes, transcripts, research, decisions. Work-in-progress briefs and drafts live in `Notes/Content/`.
2. `Binders/` holds the distilled, reviewed truth, organized into four sections. Start from `Binders/index.md`.
   - Brand: voice and tone, key messages, brand story, boilerplates, visual identity.
   - Product: overview, features, use cases, pricing, roadmap, vision.
   - Company: mission and vision, founding story, team, why now, tools.
   - Market: ICP, customer segments, competitive landscape, positioning, trends, channels.
3. `Content/` holds finished output, one subfolder per type (for example `Content/Blog Posts/`). Put drafts and finished pieces in the matching `Content/<type>/` folder.

A note's `binder` frontmatter field lists the sections it is relevant to, using these values: Brand, Product, Company, Market.

## Grounding

- Read `Binders/Brand/Voice & Tone.md` before writing anything user-facing.
- Read the Product pages before making product claims.
- Read `Binders/Market/ICP.md` and `Binders/Market/Positioning.md` before writing audience-facing copy.
- If a page you need is missing or still empty, say so and offer to create it with the `binders` skill, instead of falling back on generic knowledge.
- Check recent Notes for fast-changing topics (pricing, positioning, roadmap, competitors) and flag Binder pages that look stale.

## Onboarding

The `binders` skill's setup creates the onboarding questionnaire in `Notes/Onboarding/` when the user clicks "Set up your Binders" or asks to get started. It uses the content-marketing questions, one page per Binder page. Then point the user to the questionnaire, offer to pre-fill answers from their sources (documents, website, repo) and to interview them in chat, and update the Binders from the answers.

## On-brand content

- Use the `humanize-writing` skill whenever you draft or edit user-facing prose. This includes new content and any later change to generated content: rewriting a paragraph, shortening or expanding copy, applying review comments, correcting wording, polishing a draft, or preparing it for publication. The user does not need to ask for the skill by name.
- Read and follow the skill before making the prose change. Apply its Voice & Tone rules and pattern review as part of the edit, not as a separate report or optional final cleanup.
- Keep the requested scope. For a targeted edit, apply the skill to the changed passage and enough surrounding text to preserve voice and flow; do not rewrite untouched sections. The content skills (`content-brief`, `content-draft`, `content-publish`) may require a whole-piece review.
- This applies to prose for an external or internal audience, such as articles, website copy, briefs, case studies, newsletters, and announcements. Do not use it to rewrite raw Notes, factual Binder records, quotations, code, configuration, or metadata unless the user asks to turn that material into audience-facing content.

## Content skills

- `content-brief`: plan a new piece.
- `content-draft`: write a draft from a brief.
- `content-publish`: polish a reviewed draft and move it to `Content/`.
