# RIA-TV++ Methodology Overview

This document explains the design specification for the SOP used by book2skill, explaining "why it's done this way". For specific execution steps, please read `SKILL.md` and `methodology/01-*` to `06-*`.

## Naming

**RIA-TV++** =
- **RIA** — Zhao Zhou's note-taking拆书法 (Reading / Interpretation / Appropriation) from 《这样读书就够了》
- **TV** — Triple Verification, borrowed from nuwa-skill
- **++** — Extensions for agent execution: E (Executable steps) + B (Boundaries)

## Ideological Sources

| Source | Borrowed Content |
|---|---|
| Mortimer Adler 《如何阅读一本书》 | Stage 0: Three stages of analytical reading (structure/interpretation/criticism) |
| Zhao Zhou RIA拆书法 | Stage 2: R-I-A1-A2 basic framework, especially A2 → trigger |
| Niklas Luhmann Zettelkasten | Atomic + links + rewrite in own words |
| Tiago Forte Progressive Summarization | Stage 4 "verifiable compression chain" concept |
| nuwa-skill | Stage 1 parallel extractor + Stage 1.5 triple verification |
| darwin-skill | Stage 4 test-prompts.json format + evolvability |

## Fundamental Insight

**Existing reading methodologies are distilled for human readers, not for agent execution.**

| Dimension | For human viewing | For agent use (book2skill goal) |
|---|---|---|
| Key fields | Story / golden quotes / emotional hooks | trigger / executable steps / stop criteria |
| Failure modes | Forgetting after reading | inaccurate trigger → never called or wrongly called |
| Success criteria | Reader "has收获" (gains) | real problems are solved |

So all the "extensions" in RIA-TV++ (TV / E / B / test-prompts) exist to solve the new problems brought by this goal migration.

## Pipeline

```
          ┌───────────────────┐
          │ Stage 0: Whole Book Understanding │  Adler four steps
          └─────────┬─────────┘
                    │ BOOK_OVERVIEW.md
                    ▼
          ┌───────────────────┐
          │ Stage 1: Parallel Extraction │  5 sub-agents run simultaneously
          └─────────┬─────────┘
                    │ candidates/
                    ▼
          ┌───────────────────┐
          │ Stage 1.5: Triple Verification │  V1 cross-domain / V2 predictive power / V3 uniqueness
          └─────────┬─────────┘
                    │ passed unit tests + rejected/
                    ▼
          ┌───────────────────┐
          │ Stage 2: RIA++ Construction │  R / I / A1 / A2 / E / B
          └─────────┬─────────┘
                    │ Each skill's SKILL.md
                    ▼
          ┌───────────────────┐
          │ Stage 3: Linking │  Zettelkasten + INDEX.md
          └─────────┬─────────┘
                    │
                    ▼
          ┌───────────────────┐
          │ Stage 4: Stress Testing │  test-prompts.json + local execution + refinement
          └───────────────────┘
                    │
                    ▼
          Can be fed to darwin-skill for automatic evolution
```

## Invariants (cannot be violated in any iteration)

1. **Atomicity**: A skill only does one methodological unit, cannot be "large and comprehensive"
2. **Traceability**: Each skill must have source text citations pointing to source book chapters
3. **Verifiability**: Each skill must pass triple verification + stress testing
4. **Evolvability**: Each skill must include darwin-compatible test-prompts.json
5. **User Participation**: After stage 0, users must confirm the skeleton before continuing