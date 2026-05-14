# Skills

A personal collection of Claude Code skills for plan-driven development and skill authoring.

Skills are folders that teach Claude how to handle specific workflows — bundled instructions, scripts, and reference files that activate automatically. See [guide-skill-create.md](guide-skill-create.md) for the authoring guide.

## Available skills

| Skill | Description |
|---|---|
| [plan-write](skills/planning/plan-write/) | Writes a structured implementation plan file |
| [plan-validate](skills/planning/plan-validate/) | Validates a plan file before agent execution |
| [plan-execute](skills/planning/plan-execute/) | Executes a plan file phase by phase |
| [plan-postmortem](skills/planning/plan-postmortem/) | Reviews a completed plan execution |
| [skill-creator](skills/planning/skill-creator/) | Creates and improves skills with evals *(Anthropic, Apache 2.0)* |

## Installing

Ask Claude: `"Install skills from this repo"` — it will follow [skills/install-prompt.md](skills/install-prompt.md).

## License

MIT — see [LICENSE](LICENSE). `skill-creator` is Apache 2.0 by Anthropic.
