---
name: plan-validate
description: Validates a plan file against the hybrid template rules before agent execution. Use when the user says "validate this plan", "check my plan", "is this plan ready for an agent", "review this plan file", or before running plan-execute. Produces a structured pass/fail report with specific issues and suggestions.
---

# Plan Validate

Reads a plan file and checks it against the hybrid template rules. Produces a pass/fail report per check with actionable suggestions.

Read `references/plan-template.md` before running any checks.

## How to run

Ask for the plan file path if not provided. Read the file, run all checks below. Do not stop at the first failure — run every check.

## Structure Checks

**S1 — Size classification is correct**
Count phases. Verify the template variant matches the size rule (1–3 → small, 4–6 → medium, 7+ → large). Fail if a 2-phase plan uses living sections, or a 6-phase plan uses the small flat structure.

**S2 — Living sections present and initialized (medium/large only)**
Progress, Touched Files Registry, Discovery Log, Decision Log must all exist. Initialized but empty is fine. Missing is a failure.

**S3 — Future phases marked FROZEN**
In medium/large plans at creation time, every phase after Phase 1 should be marked FROZEN. In a partially-executed plan, every phase after the current active one should be marked FROZEN.

**S4 — Global Constraints block present (medium/large only)**
Must exist with 3–5 hard invariants. Fewer than 3 or more than 5: flag as warning, not hard fail.

**S5 — Constraints repeated inline per phase (medium/large only)**
Each phase block must inline the 1–2 constraints relevant to it. A phase that only says "see Global Constraints" fails this check.

## Contract Checks

**C1 — Every step has a contract**
Each step bullet must specify: what is produced (function sig, API shape, schema, resource), who calls/uses it, what invariant holds. A step that only describes an action fails C1.

**C2 — No vague contracts**
Contracts like "returns a result" or "updates the database" fail. Must name types, endpoints, or resource names specifically.

## Acceptance Criteria Checks

**A1 — Each phase has at least 2 acceptance criteria**
Hard fail if any phase has 0 or 1.

**A2 — Criteria are observable, not code-level**
Each criterion must be verifiable without reading code. Flag any criterion describing internal state ("the struct has been added", "all tests pass", "the field exists in the schema").

## Scope Checks

**P1 — Touched Files Registry present (medium/large)**
Must exist, even if empty at plan-creation time.

**P2 — Drift mechanism exists**
Plan must have a Discovery Log (medium/large) or equivalent. Plans with no mechanism for recording surprises fail.

## Output Format

```
## Plan Validation Report: [filename]

### Summary
- Size: [small/medium/large] — [correct/incorrect]
- Phases: N
- Overall: PASS / FAIL / PASS WITH WARNINGS

### Check Results

| Check | Result | Detail |
|-------|--------|--------|
| S1 Size classification | PASS | 5 phases → medium, correct |
| S2 Living sections | FAIL | Discovery Log section missing |
| ...   |        |        |

### Issues to Fix
[For each FAIL: specific actionable fix]

### Warnings
[For each WARNING: explanation and suggestion]
```

If all checks pass, tell the user the plan is ready for `plan-execute`.
