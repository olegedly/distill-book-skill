# Stage 0 — Book Comprehension (Adler Analysis Reading)

## Goal

Before breaking down the book, first **truly understand the book**. Without this step, the extracted skills will just be a collection of golden quotes, carrying the author's blind spots without awareness.

Output: `books/<slug>/BOOK_OVERVIEW.md` (filled according to `templates/BOOK_OVERVIEW.md.template`).

## Four Steps (First three from Adler, fourth is newly added for this skill)

### Step 1 — Structural

Identify the book's "skeleton" and answer:

- **What type of book is this?** (Methodology / Biography / Philosophy / Practical manual / ...)
- **What is the main thesis in one sentence?** — Must be truly compressible to one sentence
- **How do the main parts combine into a whole?** — List 3–7 primary arguments and mark their relationships (parallel / progressive / contrastive / rebuttal)
- **What core problem is the author trying to solve?**

### Step 2 — Interpretive

- **Key terms**: List concepts the author repeatedly uses with specific meanings, write a "author's own usage" definition for each term (not dictionary definition)
- **Core propositions**: Restate the author's 5–15 main claims in your own words
- **Argument chain**: How do these claims derive from each other? What evidence does the author use to support them?

### Step 3 — Critical ★ Most easily skipped but most important

Adler's original words: "You cannot disagree with the author until you find errors in their arguments." Conversely: **You cannot fully agree with the author until you find their limitations.**

Must answer:
- **Author's era limitations**: When was this book written? Which premises may no longer hold?
- **Author's blind spots**: What does the author overlook due to their identity / industry / cultural background?
- **Unproven assumptions**: What does the author take as self-evident but actually needs论证?
- **Counterarguments**: What would be the strongest arguments if someone were to rebut this book?

The output from this step will directly become the source for the **Boundary (B)** field of each skill.

### Step 4 — Applicability (newly added for this skill)

- **What content can be turned into skills?** — Frameworks / Checklists / Principles / Decision procedures
- **What content is unsuitable for skills?** — Pure historical data / Pure stories / Pure emotions (but can serve as examples for other skills)
- **Estimated skill count**: Give a rough range (don't force it)
- **Estimated priority**: Rank candidate skills from "most empowering to ordinary people" perspective

## Quality Gate (must be satisfied before entering Stage 1)

- [ ] Main thesis can be stated in one sentence
- [ ] Skeleton lists 3–7 primary arguments
- [ ] Key terms dictionary has ≥5 entries
- [ ] Critical phase lists at least 3 author limitations (cannot proceed if this step is incomplete)
- [ ] BOOK_OVERVIEW.md has been presented to user and confirmed

## Common Failure Modes

1. **Skipping the critical phase** — Leads to skills treating the author's biases as truth
2. **Skeleton is "your own skeleton" not the author's** — Note whether you're writing a summary or a reflection
3. **Term definitions use dictionary/common sense rather than the author's specific usage** — The author's use of "circle of competence" is not the same as the dictionary's