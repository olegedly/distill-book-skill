---
name: distill-book-skill
description: |
  Distill a book into a coherent set of executable skills. Use when the user asks to turn a book or video(s) into skills — i.e. wants a book's frameworks, principles, and methodologies extracted into atomic, reusable agentic skills that an agent can invoke in real-world situations. NOT for simple summarization, book reviews, or role-playing as the author (that is distill-person-skill's job).
---

# distill-book-skill — A meta-skill for distilling a book into a set of executable skills

## Mission

To distill the methodologies in a book into a set of **atomic skills that can be invoked by agents in real-world scenarios**, allowing readers to actually use them.

**Boundaries**:
- ✅ Do: Distillation of methodologies / decision frameworks / checklists / principles / conceptual systems
- ❌ Don't do: Book excerpts / book reviews / author role-playing (the latter should use nuwa-skill)

## Core Methodology: RIA-TV++

A pipeline with four stages + parallel extraction + triple verification + darwin-compatible testing. See `methodology/00-overview.md` for details.

```
Stage 0: Adler whole-book understanding     → BOOK_OVERVIEW.md
Stage 1: 5 parallel agent extractions     → Candidate methodology unit pool
Stage 1.5: Triple verification filtering   → Passed units
Stage 2: RIA++ skill construction         → SKILL.md for each skill
Stage 3: Zettelkasten linking             → INDEX.md
Stage 4: Pressure testing (darwin compatible) → test-prompts.json + elimination
```

## When to Call This Skill

When the user says things like:
- "Help me distill 'Poor Charlie's Almanac'"
- "Distill Mao's selected works into skills"
- "distill this book into skills: <path>"
- "I want to turn this book's methodologies into usable skills"

## Input Requirements

**Must** confirm from the user before starting:
1. **Book text source**: PDF / EPUB / TXT file path, or accessible plain text. **Do not** "reconstruct from memory" when extracting from books — stop and ask the user for the text.
2. **Book title + author + publication year**: Used for directory naming and auditing.
3. **Whether this is a pilot run**: If the user is using book2skill for the first time, recommend distilling 1 book to verify the process before batch processing.

## Output Structure

```
books/<book-slug>/
├── BOOK_OVERVIEW.md           # Stage 0 output: main ideas/skeleton/terms/critique
├── INDEX.md                   # Stage 3 output: skill overview + reference graph
├── candidates/                # Stage 1 output: original candidate pool (for auditing)
├── rejected/                  # Stage 1.5 eliminated units + reasons (for auditing)
├── <skill-slug-1>/
│   ├── SKILL.md
│   └── test-prompts.json      # darwin-skill compatible format
├── <skill-slug-2>/
│   └── ...
```

## Execution Flow (Follow Strict Sequence)

### Stage 0 — Whole Book Understanding

1. Read the book text provided by the user. For large files, read in chunks.
2. Execute the Adler four-step process from `methodology/01-stage0-adler.md`: Structure / Interpretation / Critique / Application.
3. Fill according to `templates/BOOK_OVERVIEW.md.template` and write to `books/<slug>/BOOK_OVERVIEW.md`.
4. Show the output to the user for confirmation: "Do I understand the skeleton correctly? Are there directions you want me to emphasize?" Get confirmation before proceeding to Stage 1.

### Stage 1 — 5 sub-agent parallel extraction

**Parallel** spawn 5 Task sub-agents (using Agent tool, initiate 5 in one call):

| sub-agent | prompt read | output |
|---|---|---|
| Framework Extractor | `extractors/framework-extractor.md` | Decision frameworks / mental models |
| Principle Extractor | `extractors/principle-extractor.md` | Principles / checklists / rules |
| Case Extractor | `extractors/case-extractor.md` | Examples personally used by the author in the book |
| Counter-example Extractor | `extractors/counter-example-extractor.md` | Failure patterns warned about in the book |
| Terminology Extractor | `extractors/glossary-extractor.md` | Key concept dictionary |

Each sub-agent independently reads the book, extracts content, and outputs to `books/<slug>/candidates/<type>.md`.

### Stage 1.5 — Triple verification filtering

Read `methodology/03-stage1.5-triple-verify.md` and execute for each candidate unit:

- **V1 Cross-domain**: Is there supporting evidence in at least 2 independent paragraphs in the book?
- **V2 Predictive power**: Can it answer a new question not explicitly stated in the book?
- **V3 Uniqueness**: Is it not just common sense that any smart person would say?

Passed units proceed to Stage 2. Failed units are written to `books/<slug>/rejected/` with reasons — preserve audit trail, allowing users to retrieve them later.

### Stage 2 — RIA++ skill construction

For each passed unit, fill according to `templates/SKILL.md.template`:

- **R (Reading)**: Original text citation ≤150 words/section
- **I (Interpretation)**: Rewrite the methodology skeleton in your own words (avoid copying translations)
- **A1 (Past Application)**: Cases used by the author in the book
- **A2 (Future Trigger)** ★: What situations would the user need this → skill's `description` field
- **E (Execution)**: 1-2-3 executable steps
- **B (Boundary)**: When not applicable / author's blind spots from Stage 0 critique

See `methodology/04-stage2-ria-plus.md` for detailed rules.

### Stage 3 — Zettelkasten linking

Follow `methodology/05-stage3-zettelkasten.md`:
1. Find reference relationships between skills (A depends on B / A contrasts B / A combines B)
2. Add "related skills" section at the end of each SKILL.md
3. Generate `INDEX.md` according to `templates/INDEX.md.template` (including reference graph in mermaid)

### Stage 4 — Pressure testing (darwin compatible)

For each skill, follow `methodology/06-stage4-pressure-test.md`:
1. Design 5–10 test prompts, write to `test-prompts.json` according to `templates/test-prompts.json.template`
2. Include at least 3 types: **Should invoke** / **Should not invoke (decoy)** / **Boundary fuzzy**
3. Run locally, **failed ones go back to Stage 2** — no "superficial fixes"
4. After all pass, notify user: "Complete, can be fed to darwin-skill for automatic evolution"

## Quality Red Lines (Violations block output)

1. Each skill must pass **all** triple verifications
2. Each skill must have complete R / I / A1 / A2 / E / B sections
3. Original text citation ≤150 words/section
4. Each skill must have `test-prompts.json` with decoy tests (scenarios where it should not be invoked)
5. `description` field must clearly state trigger conditions, not just "a skill about X"

## Ecosystem Positioning with nuwa-skill / darwin-skill

- **nuwa-skill**: Distiller (thinking style / expression DNA)
- **book2skill** (this skill): Book distiller (methodologies / frameworks / principles)
- **darwin-skill**: Evolves any skill

The three work together: This skill's output `test-prompts.json` strictly follows darwin-skill format, so output skills can directly connect to darwin for automatic evolution.

## Calling Conventions

- **Always pilot 1 book first** — unless user explicitly says "batch"
- **Report progress between stages** — don't run silently and then dump results
- **Don't extract from memory** — stop and ask if no text
- **Preserve audit trail** — keep both candidates/ and rejected/
