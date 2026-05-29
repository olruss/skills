# Principles

Two governing sources for all documentation work.

## Matt Palmer — 8 rules for better docs

1. **Write for humans; optimize for agents.** Human clarity is primary. Agent-friendly structure (stable anchors, structured lists, key facts in prose) is a free benefit when done well.
2. **Funnel structure.** Lead with what/why, then quickstart, then depth. Every doc earns its reader's attention before asking for it.
3. **Diataxis scaffold.** Separate tutorials (learning), how-to guides (task), reference (information), and explanation (understanding). Mixed types confuse both humans and agents.
4. **Write with AI but structure for agents.** AI can draft; structure must be intentional. Agents navigate headings and lists, not paragraphs.
5. **CI quality automation.** Broken links, stale commands, and missing files are CI failures, not review feedback.
6. **Scaffold automation.** Templates reduce blank-page friction and enforce consistency.
7. **Make contribution easy and visible.** Clear CONTRIBUTING.md, reproducible setup, fast feedback loop.
8. **Background agents for routine ops.** Doc linting, link checking, and parity checks run automatically — not on request.

## OpenAI cookbook quality constraints

- Use specific, accurate terminology — no vague generalities.
- Examples must be self-contained and copy-pasteable.
- Cover high-value topics; don't over-index on edge cases.
- No unsafe patterns (exposed secrets, destructive defaults).
- Orient the reader fast — first paragraph answers "what is this and why would I use it."
- Empathy over rigid rules — meet readers where they are.

## Practical merge policy

**Reader task success > structural clarity > long-term maintainability > agent optimization.**

Agent optimization that reduces human clarity is always the wrong trade-off.

## Execution policy

- Long investigations are acceptable when scope is uncertain.
- One merged, coherent outcome — not a raw list of sub-findings.
- Cross-locale parity for task-critical content; if not achievable, publish explicit parity status.
