---
name: plan-write
description: Writes a structured, correctly-sized implementation plan file when decisions are already made. Use when the user says "write a plan for", "create a plan file", "document this plan", "I have decisions made, now write the plan", "turn these decisions into a plan", or wants to record an architectural decision as an executable plan. Use before handing work to an agent in plan-driven development.
---

# Plan Write

This skill produces a plan file following the hybrid template. Read `references/plan-template.md` before writing anything — it contains the size rules, templates, contract requirements, and acceptance criteria rules.

## When to use

The user has already made decisions. They have a feature goal, rough phases, and want the plan documented before agent execution. Your job is to fill in what they haven't fully articulated: contracts per step, acceptance criteria per phase, global constraints, and the correct size classification.

Do not invent decisions. Ask for anything missing.

## Interview

Extract from the conversation what you can, then ask only for what's missing:

1. **Feature goal** — one sentence describing the end state
2. **Phase list** — names and sequence (informal is fine)
3. **Hard constraints** — ask: "are there invariants that must always hold, or things that must never happen?"
4. **File/module scope** — which parts of the codebase are in scope
5. **Acceptance criteria per phase** — "how will you know this phase is done from a user's perspective?"

For each step, you need a contract. If the user hasn't specified function signatures or API shapes, derive them from context and confirm before writing. A step that says "add the createUser function" without a real contract fails validation.

Confirm the phase count before writing — it determines the size classification and therefore the template.

## Writing

Once inputs are gathered:

1. Classify size from phase count (see `references/plan-template.md`).
2. Select the correct template variant.
3. Fill every required field. Living sections start initialized but empty. Future phases marked FROZEN.
4. Repeat the 1–2 relevant global constraints inline in each phase block — do not just reference the global section by number.
5. Write acceptance criteria that describe observable, user-visible behavior. If you find yourself writing "the X struct has been added", rewrite it as something a human can verify externally.
6. Write the plan to `./plans/<feature-name>.md` unless the user specifies otherwise.

## Output

Tell the user:
- Where the plan file was written
- What size classification was used and why
- Any steps where you assumed a contract (flag these so the user can correct them)
- Recommend running `plan-validate` before handing the plan to an agent
