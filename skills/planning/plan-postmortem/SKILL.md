---
name: plan-postmortem
description: Reviews a completed plan execution and produces a structured report scoring plan adherence, acceptance criteria, code quality, and process mechanics. Use when the user says "run a postmortem", "review how the plan went", "how did execution compare to the plan", "what should we improve", or after plan-execute finishes. Produces suggestions for improving the planning skills — human decides whether to apply them.
---

# Plan Postmortem

Runs after a plan is fully executed. Compares what happened against what was planned, assesses quality, and produces improvement suggestions for the planning skills.

Read `references/plan-template.md` to understand what the plan was supposed to look like.

## Inputs

Ask the user for:
1. Plan file path
2. Feature branch or commit range (for `git diff`)
3. Any specific concerns to investigate

## Review Areas

### 1. Plan adherence

Run `git diff [base]..[head] --name-only` to list files actually changed. Compare against Touched Files Registry.

Check:
- Files changed but not in registry (unplanned scope)
- Files in registry but not changed (overplanned)
- Drift noted in Discovery Log — was it surfaced correctly, or silently accommodated?
- Global constraints honored? Read constraints, look at diff for evidence of violations.

Score 0–10. Deduct for unregistered changes, unacknowledged drift, constraint violations.

### 2. Acceptance criteria

Go through each phase's criteria. Attempt to verify observable ones now. For criteria requiring a running system, note which cannot be verified and why.

Status per criterion: met / not met / cannot verify (with reason). Flag any criterion marked met by plan-execute that you cannot reproduce.

Score 0–10. Deduct for unmet criteria, code-level criteria that slipped through, and criteria marked met without evidence.

### 3. Code quality of changed files

Read files that were created or substantially modified. Assess:
- Do implementations match their step contracts (function signature, return type, invariants)?
- Are contracted invariants actually enforced in the code?
- Any hardcoded values where the plan specified configuration?

Focus on contract fidelity, not a full code review.

Score 0–10. Deduct for contract drift, missing invariant enforcement, hardcoded values.

### 4. Process quality

Assess execution mechanics:
- Discovery Log and Decision Log kept current? (Empty logs after non-trivial multi-phase execution are suspicious.)
- Future phases kept frozen? (Check git history of the plan file.)
- Were phase check-ins recorded in Decision Log?
- Did any code-level acceptance criteria slip through validation undetected?

Score 0–10. Deduct for empty logs that should have entries, silent frozen-phase violations, unobservable criteria that were never caught.

## Output Format

```
## Plan Postmortem: [plan title]

### Scores
| Area                 | Score | Summary      |
|----------------------|-------|--------------|
| Plan adherence       | N/10  | [one line]   |
| Acceptance criteria  | N/10  | [one line]   |
| Code quality         | N/10  | [one line]   |
| Process quality      | N/10  | [one line]   |
| **Overall**          | **N/10** |           |

### Findings

#### Plan Adherence
[Specific findings with file names and evidence]

#### Acceptance Criteria
[Per-criterion status]

#### Code Quality
[Specific findings with file:line references]

#### Process Quality
[Specific findings]

### Improvement Suggestions
Each suggestion names the skill to improve and what specifically to change.

1. [plan-write / plan-validate / plan-execute] — [specific change] — [why this would have caught or prevented the issue]
2. ...
```

After the report, summarize the top 3 improvements worth carrying forward. The user decides whether to act on them.
