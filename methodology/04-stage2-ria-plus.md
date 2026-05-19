# Stage 2 — RIA++ Skill Construction

## Goal

Convert each methodology unit that passed Stage 1.5 into a SKILL.md that complies with the Claude Code skill specification.

Use template: `templates/SKILL.md.template`

## RIA++ Six Parts

### R — Reading (Original Text)

- Direct quotation ≤150 characters
- Must cite source (chapter / page number / paragraph identifier)
- If original book is in English, quote the English original + your own Chinese translation, **do not use existing translations** (avoid translator copyrights + potential translation distortions)

### I — Interpretation (In My Own Words)

- Rewrite the core framework of the methodology in **your own words**
- 5–15 lines
- Check: After reading this, can someone who hasn't read the original book understand what this methodology does? If not, rewrite.
- Forbidden: Copying original sentences / piling up rhetoric

### A1 — Past Application (Book Examples)

- Specific cases where the author **personally** applied this methodology in the book
- At least 1, ≤3 cases
- Each case must specify: what problem was encountered → how this methodology was used → what conclusion was drawn → what the actual result was

The purpose of this section is to provide the agent with concrete analogy materials when the skill is called.

### A2 — Future Trigger ★ (Most Critical)

**This determines whether the skill will actually be used.**

Must clearly specify:
1. **In what situations will users encounter这类问题?** (Scenario descriptions, 3–5 items)
2. **What are the linguistic signals in these situations?** (What users will say)
3. **How is it different from adjacent skills?** (Avoid conflicts with other skills)

The output of A2 is directly written to the skill frontmatter's `description` field — Claude uses this to decide whether to activate the skill.

**Good A2 Example** (from "Reverse Thinking" skill):
> When users are struggling with a decision, listing positive reasons but can't organize their thoughts; or when asking "how to do X to succeed"; not applicable to pure information query problems.

**Bad A2 Example**:
> When users need to think. ← Too broad, will be mistakenly activated

### E — Execution (Executable Steps)

- Convert the methodology into 1-2-3 steps
- Each step has **verifiable completion criteria**
- If there are stopping points (if X after step 2, skip to step 5), explicitly state them

The purpose of E is to give the agent a clear execution path when calling this skill, rather than "freestyling".

### B — Boundary

- When **not** to use this skill (inverse scenarios)
- Failure modes warned about by the author in the book
- Author's blind spots from Stage 0 critical phase
- Adjacent but easily confused other methodologies

The purpose of B is **to prevent misuse**. Skills without B will be used when they shouldn't, doing more harm than good.

## Frontmatter Design

```yaml
---
name: <skill-slug>                    # kebab-case, unique
description: |                        # Condensed version of A2, ≤300 characters
  <When to use + When not to use + Key triggers>
source_book: 《Poor Charlie's Almanack》 Charlie Munger
source_chapter: Chapter 3
tags: [decision, mental-model, cognitive-bias]
related_skills: []                    # Filled in Stage 3
---
```

## Common Failure Modes

1. **I section written as book excerpt** — If it reads like "The author said X in this chapter", you're copying the book, not explaining. Rewrite.
2. **A2 too broad** — Triggers like "when needing to make a decision" will never be precisely called. Must provide **recognizable linguistic signals**.
3. **E section only has philosophy, no actions** — "Stay objective" is not a step, "list the 3 worst outcomes that could happen" is.
4. **Missing B section** — Skills without boundaries will be overused, ultimately disappointing users.
5. **Jumping directly from I to E, skipping A1** — Lose the evidence that the author personally used it, the skill loses authority.