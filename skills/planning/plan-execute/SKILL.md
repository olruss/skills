---
name: plan-execute
description: Executes a plan file phase by phase, updating Progress, Touched Files Registry, and Discovery Log after each phase. Use when the user says "execute this plan", "run this plan", "work through the plan", "start on phase N", "continue the plan", or "implement according to the plan". Enforces the frozen-phase rule and surfaces drift as a blocker note rather than silently accommodating it.
---

# Plan Execute

Executes a plan one phase at a time. Maintains living sections throughout. Never rewrites future phases.

Read `references/plan-template.md` before starting — you need the frozen-phase rule, living section responsibilities, and contract/acceptance criteria definitions.

## Before starting

1. Read the plan file in full.
2. Identify the current phase from the Progress checklist (first pending phase, or the phase the user specified).
3. Confirm: "I'm starting Phase N: [name]. Acceptance criteria for this phase: [list]. Shall I proceed?"
4. If the plan hasn't been validated, recommend `plan-validate` first. If the user says proceed anyway, note it in the Decision Log.

## Executing a phase

Work through steps in order. For each step:

- Treat the step's contract as a specification. Produce exactly what it describes — not a looser approximation.
- If something unexpected happens during a step, append it to the Discovery Log immediately (don't wait for phase end): `[Phase N, Step N.x] [what was discovered and why it matters]`.

After completing all steps, verify the phase's acceptance criteria. For each criterion, confirm it is actually met — observable criteria can often be checked with a Bash command or file read. If a criterion is not met, do not mark the phase done. Document the gap and ask the user how to proceed.

## After each phase

Update the living sections:

1. **Progress**: Mark done with timestamp.
2. **Touched Files Registry**: Add every file created or modified. Include phase number, change type, brief note.
3. **Discovery Log**: Any surprises should already be logged. Add a phase-end note if warranted.
4. **Decision Log**: Log any architectural or implementation choices not specified in the plan (e.g., chose library X over Y, restructured a module).

## Frozen-phase rule

Never rewrite content of any phase that has not yet started. If Phase 2 reveals that Phase 3 needs different steps, append to Discovery Log:

`[Phase 3 drift] Step 3.x will need adjustment because [reason]. Do not modify Phase 3 until it becomes active.`

When Phase 3 becomes active, read drift notes and reconcile before executing.

## Blockers

If a step conflicts with a global constraint, treat it as a blocker. Surface it explicitly:

`[BLOCKER] Step N.x conflicts with global constraint "[quote verbatim]". Cannot proceed without human decision.`

Do not proceed past a blocker unilaterally.

## Phase check-in

After marking a phase complete, ask: "Phase N is complete and all acceptance criteria are met. Proceed to Phase N+1, or review first?"

Do not automatically continue. Human checkpoints between phases are part of the design.

## When the plan is complete

Tell the user: all phases done, all acceptance criteria met, how many Discovery Log entries were added (signals postmortem complexity). Suggest running `plan-postmortem`.
