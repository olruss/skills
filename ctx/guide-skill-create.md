# Skill Creation Guide

Reference when creating or improving skills for Claude.

---

## What a skill is

A skill is a folder that teaches Claude how to handle a specific task or workflow. It packages instructions, optional scripts, and optional reference files so Claude can activate the right behavior automatically — without the user re-explaining each time.

```
your-skill-name/
├── SKILL.md          # Required
├── scripts/          # Optional — executable Python/Bash
├── references/       # Optional — docs loaded on demand
└── assets/           # Optional — templates, fonts, icons
```

---

## File/folder naming rules

| Thing | Rule |
|---|---|
| Skill folder | kebab-case only (`my-skill`, not `My Skill` or `my_skill`) |
| Main file | Exactly `SKILL.md` — case-sensitive, no variations |
| No README.md | Inside the skill folder; all docs go in SKILL.md or references/ |

---

## YAML frontmatter

The frontmatter is the most important part — it's how Claude decides whether to load the skill.

### Minimal required format
```yaml
---
name: your-skill-name
description: What it does. Use when user asks to [specific phrases].
---
```

### All optional fields
```yaml
---
name: skill-name
description: [required]
license: MIT
allowed-tools: "Bash(python:*) Bash(npm:*) WebFetch"
metadata:
  author: Your Name
  version: 1.0.0
  mcp-server: server-name
  category: productivity
  tags: [project-management, automation]
---
```

### Field rules
- `name`: kebab-case, no spaces, no capitals, no "claude" or "anthropic" prefix (reserved)
- `description`: under 1024 chars, no XML angle brackets (`<` `>`), must include BOTH what it does AND when to use it
- `compatibility`: 1–500 chars, indicates environment requirements

### Security: forbidden in frontmatter
- XML angle brackets `< >`
- Code execution in YAML
- Names starting with "claude" or "anthropic"

---

## Writing the description field

This is what Claude reads to decide if a skill is relevant. Structure it as:

```
[What it does] + [When to use it] + [Key capabilities]
```

### Good examples
```
# Specific and actionable
description: Analyzes Figma design files and generates developer handoff docs.
Use when user uploads .fig files, asks for "design specs", "component documentation",
or "design-to-code handoff".

# Includes trigger phrases
description: Manages Linear project workflows including sprint planning, task creation,
and status tracking. Use when user mentions "sprint", "Linear tasks", "project planning",
or asks to "create tickets".
```

### Bad examples
```
description: Helps with projects.               # Too vague
description: Creates documentation systems.     # Missing triggers
description: Implements the Project entity.     # Too technical, no user triggers
```

### Triggering tips
- Claude tends to undertrigger — make descriptions a little "pushy"
- Include specific phrases users would actually say (casual language, abbreviations)
- If skill doesn't trigger: add more detail and keywords to description
- If skill triggers too often: add negative triggers ("Do NOT use for X") or be more specific
- Debug: ask Claude "When would you use the [skill name] skill?" — it'll quote the description back

---

## Three-level progressive disclosure

| Level | What | When loaded |
|---|---|---|
| 1 — YAML frontmatter | name + description | Always, in system prompt |
| 2 — SKILL.md body | Full instructions | When skill is triggered |
| 3 — Linked files | references/, scripts/, assets/ | Only when needed |

Keep SKILL.md under ~500 lines (5,000 words). Move detailed docs to `references/` and link clearly.

---

## SKILL.md body structure

```markdown
---
name: your-skill
description: [what + when]
---

# Your Skill Name

## Instructions

### Step 1: [First Major Step]
Clear explanation of what happens.

### Step 2: [Next Step]
...

## Examples

### Example 1: [common scenario]
User says: "..."
Actions:
1. ...
Result: ...

## Troubleshooting

### Error: [Common error message]
**Cause:** Why it happens
**Solution:** How to fix
```

### Writing tips
- Be specific and actionable — show exact commands, not vague instructions
- Explain the *why* behind instructions; Claude is smart and follows reasoning better than rigid rules
- Reference bundled files clearly: `Before writing queries, consult references/api-patterns.md`
- Put critical instructions at the top; use `## Important` or `## Critical` headers
- For critical validations, bundle a script in `scripts/` — code is deterministic, language isn't

---

## Three skill categories

**Category 1 — Document & Asset Creation**
Creating consistent output: documents, presentations, apps, designs, code.
Techniques: embedded style guides, template structures, quality checklists, no external tools needed.

**Category 2 — Workflow Automation**
Multi-step processes with consistent methodology, possibly across multiple MCP servers.
Techniques: step-by-step workflow with validation gates, templates, iterative refinement loops.

**Category 3 — MCP Enhancement**
Workflow guidance layered on top of MCP tool access.
Techniques: coordinates multiple MCP calls in sequence, embeds domain expertise, error handling for MCP issues.

---

## Workflow patterns

### Pattern 1: Sequential workflow orchestration
Use when users need multi-step processes in a specific order.
Key: explicit step ordering, dependencies between steps, validation at each stage, rollback on failure.

### Pattern 2: Multi-MCP coordination
Use when workflows span multiple services (e.g., Figma → Drive → Linear → Slack).
Key: clear phase separation, data passing between MCPs, validate before moving to next phase.

### Pattern 3: Iterative refinement
Use when output quality improves with iteration (e.g., report generation).
Key: initial draft → quality check → refinement loop → finalize. Define explicit quality criteria and stopping condition.

### Pattern 4: Context-aware tool selection
Use when the same outcome requires different tools depending on context.
Key: clear decision criteria, fallback options, transparency about choices made.

### Pattern 5: Domain-specific intelligence
Use when the skill adds specialized knowledge beyond raw tool access (e.g., compliance checks).
Key: domain expertise embedded in logic, compliance before action, audit trail.

---

## Planning checklist (before writing)

- [ ] Identify 2–3 concrete use cases
- [ ] Define what triggers this skill (exact phrases users say)
- [ ] Identify which tools are needed (built-in or MCP)
- [ ] Define what "done" looks like (success criteria)

---

## Success criteria

**Quantitative targets:**
- Skill triggers on ~90% of relevant queries
- Completes workflow in expected number of tool calls
- 0 failed API calls per workflow run

**Qualitative targets:**
- Users don't need to prompt about next steps
- Workflows complete without user correction
- Consistent results across sessions

---

## Testing approach

**1. Triggering tests** — does it load at the right times?
```
Should trigger:
- "Help me set up a new ProjectHub workspace"
- "I need to create a project in ProjectHub"

Should NOT trigger:
- "What's the weather in San Francisco?"
- "Help me write Python code"
```

**2. Functional tests** — does it produce correct outputs?
```
Given: [inputs]
When: Skill executes
Then: [expected outputs, no errors]
```

**3. Performance comparison** — does it improve over baseline?
Compare token count, tool calls, and back-and-forth messages with vs. without the skill.

**Pro tip:** Iterate on a single challenging task until Claude succeeds, then extract the winning approach into the skill. Faster signal than broad testing.

---

## Troubleshooting reference

| Symptom | Cause | Fix |
|---|---|---|
| "Could not find SKILL.md" | Wrong filename casing | Rename to exactly `SKILL.md` |
| "Invalid frontmatter" | Missing `---` delimiters or unclosed quotes | Fix YAML syntax |
| "Invalid skill name" | Spaces or capitals in name | Use kebab-case |
| Skill never triggers | Description too vague or missing triggers | Add specific trigger phrases, user-facing language |
| Skill triggers too often | Description too broad | Add negative triggers, clarify scope |
| MCP calls fail | Auth, wrong tool names, server not connected | Test MCP independently first |
| Instructions not followed | Too verbose, buried, or ambiguous | Move detail to references/, put critical items first, use explicit language |
| Slow / degraded responses | SKILL.md too large, too many skills active | Move docs to references/, keep SKILL.md under 5,000 words |

---

## Quick pre-publish checklist

**During development:**
- [ ] Folder: kebab-case
- [ ] File: exactly `SKILL.md`
- [ ] YAML: has `---` delimiters
- [ ] `name`: kebab-case, no caps
- [ ] `description`: includes WHAT + WHEN
- [ ] No XML tags anywhere
- [ ] Instructions: clear and actionable
- [ ] Error handling included
- [ ] Examples provided
- [ ] References clearly linked

**Before publishing:**
- [ ] Triggers on obvious tasks
- [ ] Triggers on paraphrased requests
- [ ] Does NOT trigger on unrelated topics
- [ ] Functional tests pass
- [ ] MCP integration works (if applicable)
