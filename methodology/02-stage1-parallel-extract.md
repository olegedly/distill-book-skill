# Stage 1 — 5 sub-agents parallel extraction

## Goal

Instead of reading through with a single perspective, **scan the entire book simultaneously from 5 different angles** to maximize candidate unit coverage.

## Why parallel

- **Coverage**: Single perspective misses things. What the framework extractor can't find as "counter-examples", the counter-example extractor will find.
- **Speed**: Claude Code's Agent tools support parallel execution, might as well use it.
- **Independence**: Each extractor makes independent judgments, avoiding mutual contamination — triple verification only works when truly independent (V1 cross-domain requirement "independent occurrence")

## 5 sub-agents

Each sub-agent receives:
- `BOOK_OVERVIEW.md` (output from Stage 0, provides global context)
- Book text (or text path)
- Corresponding extractor prompt (`extractors/<type>-extractor.md`)

And through Agent tools **spawn 5 simultaneously** in one call, not serially.

| # | extractor | What to find | Output file |
|---|---|---|---|
| 1 | framework-extractor | Mental models / decision frameworks / reasoning methods | `candidates/frameworks.md` |
| 2 | principle-extractor | Principles / checklists / rules / assertions | `candidates/principles.md` |
| 3 | case-extractor | Examples author personally uses in the book | `candidates/cases.md` |
| 4 | counter-example-extractor | Failures / counter-examples / pitfalls author warns about | `candidates/counter-examples.md` |
| 5 | glossary-extractor | Key concept dictionary | `candidates/glossary.md` |

## Minimum fields for each candidate unit

Regardless of which extractor, each candidate unit produced must contain:

```yaml
id: f01                           # Type abbreviation + sequence number
title: reverse thinking           # Short title
type: framework                   # framework / principle / case / counter-example / term
source_chapter: Lecture 3         # Location in book
source_quote: |                   # Original quote ≤150 characters
  "Think backward, always think backward..."
summary: |                        # In your own words, 5-10 lines
  ...
tags: [decision, mental-model]    # For subsequent linking
```

## Self-check before output

Each extractor asks itself before submitting candidates:
1. Does this unit have clear basis **in the book**? (not something I made up)
2. Does it fall within my extractor's responsibility scope? (don't overstep)
3. Has it already been extracted by another extractor elsewhere? (duplication is not a problem, Stage 1.5 will merge)

## Not done in this stage

- **No filtering** — Better to over-filter, leave to Stage 1.5 triple verification
- **No skill writing** — Only produce candidates, not SKILL.md
- **No cross-unit linking** — Leave to Stage 3