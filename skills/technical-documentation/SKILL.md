---
name: technical-documentation
description: Build and review high-quality technical docs and agent instruction files for any repository.
license: MIT
---

# Technical Documentation

## Purpose

Produce and review technical documentation that is clear, actionable, and maintainable for both humans and agents — including contributor governance files and agent instruction files.

## When to use

- Creating or overhauling docs in an existing codebase (brownfield).
- Building evergreen docs meant to stay accurate over time.
- Reviewing doc diffs for structure, clarity, and correctness.
- Running full-repo documentation audits covering governance files and product docs.
- Updating AGENTS.md and/or CONTRIBUTING.md to align with current repo practices.
- Diagnosing agent-file drift (missing files, broken commands, policy conflicts across CLAUDE.md, .cursorrules, etc.).

## Workflow

1. Classify task: `build` or `review`; context: `brownfield` or `evergreen`.
2. Inventory full documentation scope: AGENTS.md / CONTRIBUTING.md / aliases plus docs directories, framework sources, and root READMEs.
3. Detect multilingual scope and define required parity level.
4. Read `references/governance.md` for agent instruction and CONTRIBUTING.md rules.
5. Read `references/principles.md` for the governing ruleset.
6. For build tasks, follow `references/build.md`.
7. For review tasks, follow `references/review.md`; proactively detect issues without waiting for repeated prompts.
8. Return deliverables plus validation notes, parity status, and remaining gaps.

## Inputs / Outputs

**Inputs:** task type (build/review), context (brownfield/evergreen), target audience, scope (specific files or full repo).

**Outputs:** new or revised doc files, governance files (AGENTS.md, CONTRIBUTING.md, aliases), review findings (blocking issues + improvements + validation notes).
