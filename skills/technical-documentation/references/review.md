# Review Playbook

Follow these sections in order for any documentation review task.

## 1. Scope and classification

Identify: doc type (tutorial / how-to / reference / explanation), context (brownfield vs evergreen), expected reader outcome. Include both governance files and product-doc surfaces in scope unless explicitly narrowed.

## 2. Investigation behavior

Proactively find issues — don't wait for repeated prompts. Continue past the first pass when signals suggest more problems. Apply high-confidence fixes inline unless report-only mode is requested.

## 3. Governance surface review

Check AGENTS.md / CONTRIBUTING.md and all alias files for:
- Missing or wrong YAML frontmatter.
- Vague `Always`/`Ask first`/`Never` boundaries — must be concrete and executable.
- Stale or incorrect commands (setup, lint, test, build).
- Conflicts across nested scope files or platform-specific alias consumers.
- Context bloat and duplicated instructions.
- Missing canonical entry file when aliases exist.

See `references/governance.md` for full quality bars and platform-awareness rules.

## 4. Product documentation surface review

- Docs IA: logical hierarchy, no orphaned pages, consistent nav depth.
- Stale commands, broken links, and missing referenced files — these are blocking issues.
- Split/merge candidates: docs >1500 lines covering unrelated tasks; stub pages with <100 words of content.
- Verify all code examples are self-contained and runnable.

## 5. Structural review

- Funnel check: does each doc open with what/why before diving into steps?
- Heading flow: are headings outcome-oriented and scannable?
- Diataxis alignment: is the doc type consistent throughout (not mixing tutorial and reference prose)?
- Critical content not buried in images or collapsed sections.

## 6. Writing quality review

- Concise, scannable paragraphs — no walls of prose.
- No ambiguous pronouns or vague filler words.
- Directive technical tone — active voice, imperative mood for instructions.
- Platform-native components used correctly (callouts, tabs, endpoint blocks) — flag technically correct but hard-to-scan structure.

## 7. Output format

Return findings in three sections:

**(1) Blocking issues** — file path + exact required fix. These must be resolved before the doc ships.

**(2) Non-blocking improvements** — file path + recommendation. Worth doing; won't block.

**(3) Validation notes** — what was verified (commands run, links checked, paths confirmed), parity status for multilingual repos, and any remaining gaps that couldn't be fully resolved in this pass.

## Mode notes

**Brownfield:** respect existing IA, conventions, and anchors/redirects. Flag regressions and terminology drift. Propose the smallest safe change set.

**Evergreen:** flag date-stamped or brittle wording, missing ownership/refresh signals, and content that will silently go stale after product evolution.
