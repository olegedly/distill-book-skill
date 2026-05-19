# Stage 4 — Pressure Test (darwin compatible)

## Objective

Before the skill is truly delivered, use a batch of test prompts to verify its **call accuracy** and **output quality after being called**.

Those that don't pass must be redone—not just superficially patching the `description` field, but redoing the A2/E/B from Stage 2.

## Why it's necessary

A2 (trigger) is the most difficult part in book distillation. No matter how well-crafted a skill is, if the trigger is inaccurate, it might as well not exist. Pressure testing is the **only** method to discover trigger issues before release.

## test-prompts.json format (darwin-skill compatible)

```json
{
  "skill": "inversion-thinking",
  "version": "0.1.0",
  "test_cases": [
    {
      "id": "should-trigger-01",
      "type": "should_trigger",
      "prompt": "I'm deciding whether to take on this new project, I've listed many benefits but still feel unsure",
      "expected_behavior": "Call inversion-thinking, ask 'What's the worst that could happen'",
      "notes": "Positive scenario: Decision dilemma"
    },
    {
      "id": "should-not-trigger-01",
      "type": "should_not_trigger",
      "prompt": "Help me check the parameters for this API",
      "expected_behavior": "Pure information query, should not call any decision skill",
      "notes": "Bait: Non-decision scenario"
    },
    {
      "id": "edge-01",
      "type": "edge_case",
      "prompt": "I'm thinking about what to have for dinner",
      "expected_behavior": "Daily trivial matter, should not call (even though literally a 'decision')",
      "notes": "Boundary: Distinguish serious decisions from daily choices"
    }
  ]
}
```

## All three types of tests are essential

| Type | Quantity | Purpose |
|---|---|---|
| `should_trigger` | 3–5 cases | Whether it gets called when it should |
| `should_not_trigger` (bait) | 2–3 cases | Whether it refrains when it shouldn't be called |
| `edge_case` | 1–3 cases | Whether boundary-blurry scenarios are judged reasonably |

**Skills without bait tests will be rejected**. Because only testing positive cases makes skills always appear "good", but they will randomly activate after actual deployment.

## Execution Process

1. For each skill, write `test-prompts.json` according to the template
2. Run locally: for each test_case, let Claude independently judge "Would I call this skill in this scenario", record the judgment and reasoning
3. Calculate pass rate:
   - **100% pass** → Accept
   - **≥80% pass** → Analyze failed cases, decide whether to fix A2 or fix tests (but be wary of self-justification when fixing tests)
   - **<80% pass** → **Must be redone in Stage 2**, not minor fixes
4. After fixes, rerun until passing

## Determine "fix skill vs fix test"

- If failed cases reveal that the skill **trigger description is ambiguous**: fix the skill
- If failed cases are **reasonable scenarios you didn't think of before**: may need to fix the skill to cover or clearly exclude
- If failed cases are **overly harsh scenarios you designed just to create bait**: fix the test (but must record the reasoning)

## Output

- `<skill-dir>/test-prompts.json` — darwin compatible format
- `<skill-dir>/test-results.md` — pass rate and failure analysis for this test (for audit purposes)

## Handover to darwin-skill

After all skills pass, tell the user:
> Completed. For continuous evolution, you can feed it to darwin-skill:
> `darwin evolve books/<slug>/`
> It will use the test-prompts.json here for ratcheting automatic evolution.