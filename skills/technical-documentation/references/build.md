# Build Playbook

Follow these sections in order for any documentation build task.

## 1. Detect governance constraints first

Before writing, read `references/governance.md`. Identify canonical instruction files and alias surfaces. Capture any constraints that must be respected (naming conventions, section requirements, prohibited content).

## 2. Inventory documentation surfaces

Map all surfaces before drafting a single line:
- Governance: AGENTS.md, CONTRIBUTING.md, and all alias files.
- Product docs: `docs/`, `README*`, `.md/.mdx/.mdc` files, framework sources (Fern, Sphinx, Mintlify, etc.).
- Output: coverage map listing present files, missing files, and known broken paths.

## 3. Framework config and path mapping

If a docs framework is present (Fern, Sphinx, Mintlify, or custom):
- Read the framework config file before editing any paths.
- Resolve all file paths relative to the declaring config file — not the repo root.
- Validate filesystem paths and URL routes separately; they diverge in most frameworks.
- Flag any config-to-file mismatches before writing.

## 4. Define intent and success

Nail down before drafting: audience, prerequisites, job-to-be-done, doc type (tutorial / how-to / reference / explanation), and success criteria. A doc without a clear reader outcome is not ready to write.

## 5. Build structure before prose

- Open with what/why, then quickstart, then depth (funnel structure).
- Use informative headings — describe the outcome, not the section category ("Configure rate limits" not "Configuration").
- Mark decision points explicitly. Keep task-critical config inline, not in appendices.

## 6. Build AGENTS.md and CONTRIBUTING.md intentionally

Follow `references/governance.md` quality bars exactly. Create missing canonical entry files. Ensure all commands are tested and exact. Keep alias surfaces in sync via symlinks.

## 7. Writing constraints

- Precise language — no ambiguous pronouns, no filler ("simply", "just", "easy").
- Code examples: copy-ready, self-contained, realistic variable names, angle-bracket placeholders for user-supplied values, expected success output shown.
- Include common failure modes and how to recover.
- No placeholder guidance ("configure as needed", "set appropriate values").

## 8. Validation before delivery

- Run all commands and code examples; fix anything that fails.
- Verify all links and anchors exist.
- Check all referenced file paths against the filesystem.
- For framework-driven docs: confirm config-to-file consistency.
- Confirm no secrets or unsafe patterns appear in examples.
- For multilingual repos: verify parity for task-critical content; note any intentional divergence.

## Tooling note

Match recommendations to the existing docs stack — switching platforms is a last resort. In brownfield mode, prefer the smallest safe change set using existing components and conventions. In evergreen mode, favor stable concepts, isolate volatile details, and add ownership/refresh signals on sections with a short validity window.
