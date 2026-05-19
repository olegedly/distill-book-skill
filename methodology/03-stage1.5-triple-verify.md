# Stage 1.5 — Triple Verification Filtering

## Goal

Filter from the candidate pool to find **methodology units that are truly worthy of becoming independent skills**. Those that don't pass are downgraded to examples / references / terminology, but not made into independent skills.

This is the core quality gate that distinguishes book2skill from "book summarization tools."

## Triple Verification (Must pass all three to qualify)

### V1 — Cross-domain Verification

**Question**: Does this unit have supporting evidence in **at least 2 independent contexts** within the book?

- "Independent" means: not just the same case rephrased, but different stories / different chapters / different subjects all discussing the same principle
- **Pass**: "Reverse thinking" in Poor Charlie's Almanac appears in investment decisions, disaster avoidance, and teaching methods as three separate scenarios → Pass
- **Fail**: A beautiful sentence that only appears once in one chapter, with no independent evidence within the book → Downgraded to golden sentence example

**Why**: What repeatedly appears across multiple contexts is the author's truly intended stable methodology, not just a fleeting expression.

### V2 — Predictive Power Test

**Question**: Can this unit be used to deduce answers to problems not explicitly discussed in the book?

- Design a scenario that's not directly discussed in the book
- Try to analyze it using this methodology
- **Pass**: Can derive a meaningful, non-trivial conclusion → Pass
- **Fail**: Can only derive platitudes like "hard work leads to success" → This unit has no real explanatory power, downgraded

**Why**: True methodology must have **extrapolation capability**. If it can only repeat the book's examples, it's a description, not a method.

### V3 — Uniqueness Check

**Question**: Is this unit "common sense that any smart person would say"?

- If you remove the author's name, a smart person with no knowledge of the book's domain could say it → Fail
- Must be the author's **unique perspective / counter-intuitive insights / unique terminology system** → Pass
- **Pass**: Duan Yongping's "stop doing list" — proactively listing what not to do, counter-intuitive → Pass
- **Fail**: "Respect time" — too common sense, no one needs a skill to tell them this

**Why**: Common sense doesn't need a skill to carry it — Claude already knows it. Only the author's **differentiated insights** are worth solidifying into a skill.

## Verification Execution Process

1. Merge the 5 candidates/*.md from Stage 1 into a single candidate pool
2. Deduplicate: merge methodology units extracted by multiple extractors into single entries
3. Run V1/V2/V3 on each candidate, record judgment and reasoning
4. Passers → write to `books/<slug>/verified.md`, advance to Stage 2
5. Failures → write to `books/<slug>/rejected/<id>.md`, **must specify which criterion failed and why** (auditing value)

## Output Template (verified.md entry)

```yaml
id: f01
title: Reverse Thinking
type: framework
V1_cross_domain:
  passed: true
  evidence:
    - Lecture 3: Investment decision scenario
    - Lecture 7: Engineering design scenario
    - Lecture 11: Teaching method scenario
V2_predictive_power:
  passed: true
  novel_question: "What should I do if an interviewer asks me a question I don't know the answer to?"
  derived_answer: "Ask 'What do I least want him to think of me?', work backwards from this reverse to determine what should be demonstrated"
V3_exclusivity:
  passed: true
  why_not_common: "Common sense is 'think more', reverse thinking is 'prefer to think in reverse' — this is counter-intuitive prioritization"
→ Advance to Stage 2
```

## Common Failure Patterns

1. **V1 Cheating** — Counting the same case rephrased as two separate instances. Requirement: must be different chapters + different subjects + different conclusions.
2. **V2 Cheating** — Using a problem actually discussed in the book disguised as a "new problem". Requirement: the new problem should make someone unfamiliar with the book unable to guess how it would be addressed.
3. **V3 Too Lenient** — Considering something "elegantly phrased" as not common sense. Requirement: look at the **content itself** for counter-intuitiveness, not just the wording.

## Quantity Expectations

Experience shows, for methodology-dense books (like Poor Charlie's Almanac), pass rate is about 30–50%. For prose books, maybe only 5–10%. Pass rates too low (<5%) or too high (>80%) are both red flags:
- Too low: extractor quality may be poor, need to rerun
- Too high: verification criteria may be too loose