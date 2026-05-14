# Plan Template Reference

Read this before producing, validating, or interpreting a plan file.

---

## Size Classification

Classify by total number of execution phases:

| Size   | Phases | Template to use |
|--------|--------|-----------------|
| Small  | 1–3    | Flat checklist + acceptance criteria. No living sections. |
| Medium | 4–6    | Full hybrid template. |
| Large  | 7+     | Two-level: master plan file + active phase file. |

Size drives the entire structure. Do not use the medium template for a 2-phase plan.

---

## Small Plan Template

```
# Plan: [Title]

## Goal
One sentence.

## Constraints
- [Hard invariant 1]
- [Hard invariant 2]

## Steps
- [ ] 1. [name] — Contract: [function sig or API shape + caller + invariant]
- [ ] 2. [name] — Contract: ...
- [ ] 3. [name] — Contract: ...

## Acceptance Criteria
- [Observable user-visible check]
- [Another observable check]

**Verify:** `<command>` — run to confirm all criteria above
```

---

## Medium Plan Template

```
# Plan: [Title]

## Goal
One sentence describing the end state.

## Global Constraints
Applies to every phase. Violating any of these is a blocker.
- [Invariant 1]
- [Invariant 2]
- [Invariant 3]
(3–5 max)

## Touched Files Registry
Updated after each phase. Unregistered changes must be noted in Discovery Log.

| File | Phase | Change Type | Notes |
|------|-------|-------------|-------|
| (populated during execution) | | | |

## Progress

| Phase | Status | Completed At |
|-------|--------|--------------|
| 1. [name] | [ ] pending | |
| 2. [name] | [ ] pending | |

## Discovery Log
Append-only. Record surprises, unexpected findings, drift from plan scope.

## Decision Log
Append-only. Record every design choice made during execution that deviates from or extends the plan.

---

## Phase 1: [Name]
**Relevant constraints:** [repeat the 1–2 global constraints that apply here]

### Milestone
[1–2 sentences of prose: what this phase accomplishes and why it matters in the arc of the project.]

### Steps
- [ ] 1.1 [step name]
  Contract: `functionName(param: Type): ReturnType` — called by [caller] — invariant: [what must hold]
- [ ] 1.2 [step name]
  Contract: ...

### Acceptance Criteria
Observable, user-visible checks only. Not code-level assertions.
- [User navigates to X and sees Y]
- [curl endpoint Z returns HTTP 200 with field W]

**Verify:** `<command>` — e.g., `pnpm vitest run path/to/test`, `curl http://localhost:3000/z`, `pnpm e2e`

### Checkpoint
- [ ] Verify command run and passing
- [ ] `git commit -m "feat: phase 1 complete — [phase name]"`

---

## Phase 2: [Name]  ← FROZEN until Phase 1 completes
**Relevant constraints:** [repeat applicable constraints]

### Milestone
...

### Steps
...

### Acceptance Criteria
...

**Verify:** `<command>`

### Checkpoint
- [ ] Verify command run and passing
- [ ] `git commit -m "feat: phase 2 complete — [phase name]"`
```

---

## Large Plan Template

Use for 7+ phases. Same structure as medium, plus:

1. Completed phases are compressed: `~~Phase N: [name]~~ — completed [timestamp]. Key files: [list].`
2. A second file `plan-active-phase.md` holds full detail for the current phase only.
3. Constraints are repeated verbatim in every phase file (not by reference to master).
4. Design for human restart: master plan + active phase file must be sufficient context for a new agent to resume.

---

## Frozen Phase Rule

An executing agent must never rewrite future phase content. If discoveries make a future phase impossible as written, append to the Discovery Log:

`[Phase N drift] Phase M step M.x will need adjustment because [reason]. Do not modify Phase M until it becomes active.`

When Phase M becomes active, the agent reads the drift note and reconciles before executing — it does not assume the original text is still valid.

---

## Living Sections

Required in medium and large plans. Always writable (never frozen):
- Progress checklist
- Touched Files Registry
- Discovery Log
- Decision Log

In small plans, none are required — the step checklist serves as progress tracking.

---

## Contract Spec Rules

Every step must have a contract specifying: what is produced, who calls/uses it, what invariant holds.

Good: `createUser(email: string, role: Role): Promise<User>` — called by AuthController.register — invariant: email normalized to lowercase, id is globally unique

Bad: "Add a createUser function to the auth module"

For non-code steps (infra, migrations), use equivalent: inputs, outputs, preconditions, guarantees.

### TDD step variant (when adding testable behavior)
Split the phase into a paired sequence:
- [ ] N.a — Write failing tests  
  Contract: `path/to/test.ts` — test cases: [list] — all must fail before N.b starts
- [ ] N.b — Implement to pass  
  Contract: [function sig / API shape] — must not modify test file from N.a

---

## Acceptance Criteria Rules

Must be verifiable by a human or simple script — not a code review.

Good: `curl http://localhost:3000/health` returns `{"status":"ok"}` within 500ms
Good: navigating to /settings shows the theme toggle; toggling it persists across reload
Bad: "the Theme struct has been added to the config package"
Bad: "all tests pass"

Each phase needs at least 2 acceptance criteria.

Each phase's **Verify:** command must be runnable — a shell command, not a description.

Good: **Verify:** `pnpm vitest run src/auth/auth.test.ts` — run after all steps, must pass
Bad:  **Verify:** "check the code looks right"
