# Governance

Rules for agent instruction files (AGENTS.md, CLAUDE.md, .cursorrules, etc.) and contributor governance files (CONTRIBUTING.md).

## Discovery

Before editing any governance file, run a full discovery sweep:

```sh
rg -l "AGENTS|CONTRIBUTING|cursorrules|\.claude|\.cursor|\.pi" --glob "*.md" --glob "*.yaml" --glob "*.yml" --glob "*.mdc" .
```

Always read the root-level AGENTS.md and the nearest-scope AGENTS.md before making changes.

## Canonical source and aliases

- `AGENTS.md` is canonical when present. All other instruction files are compatibility surfaces.
- Compatibility aliases: `AGENT.md`, `.cursorrules`, `.cursor/rules/*`, `.agent/`, `.agents/`, `.pi/`
- DRY policy: one core payload, exposed through alias/symlink — never duplicate prose across surfaces.
- Symlink pattern: `.cursor/rules/` → `.agents/rules/`; validate that `.mdc` payloads are consistent.

## AGENTS.md quality bar

Every AGENTS.md must have:
- YAML frontmatter with explicit persona, scope, and model hint (if relevant).
- `Always` / `Ask first` / `Never` behavior boundaries — concrete, not vague.
- Exact shell commands for setup, lint, test, and build (no placeholder steps).
- Explicit file-reference style for Codex; narrow glob-friendly patterns for Cursor/Claude.
- No conflicting instructions across nested scope files.

## CONTRIBUTING.md quality bar

- Cover: repo setup, issue filing, PR workflow, test/lint commands, review gates.
- Use template links rather than duplicating workflow prose inline.
- Split if oversized (>600 lines) — separate CONTRIBUTING-AGENTS.md or CONTRIBUTING-DEV.md.
- Optimize for machine readability: consistent heading levels, no ambiguous pronouns.

## Platform-aware authoring

| Platform | Instruction style |
|---|---|
| Cursor / Claude Code | Short, narrow glob-scoped files; avoid large monolithic instruction blobs |
| OpenAI Codex | Explicit file references; no implicit glob resolution |
| Generic agent | AGENTS.md as single entry point; aliases for compat |

## Proactive issue sweep

On every governance review pass, check for:
1. Conflicting `Always`/`Never` rules across nested AGENTS.md files.
2. Commands that no longer exist or have changed flags.
3. Missing canonical entry file when aliases are present.
4. Context bloat — instructions that repeat what the code already makes obvious.

Apply high-confidence fixes in the same pass unless report-only mode is requested.

## Reference repos

Well-maintained governance to emulate: [Vercel](https://github.com/vercel/next.js), [Kubernetes](https://github.com/kubernetes/kubernetes), [React](https://github.com/facebook/react), [Rails](https://github.com/rails/rails).

## Practical merge policy

**Contributor/reader task success > instruction clarity > maintainability > agent optimization.**
