# Install Skill

Follow these steps in order.

1. **Check your docs** — look up how your tool handles custom skills or agents: where they are stored, and whether project-scope and user-scope are supported. Do this before asking the user anything.

2. **List available skills** — scan the `skills/` directory and present skills grouped by topic. Example format:

   **planning**: plan-write, plan-validate, plan-execute, plan-postmortem, skill-creator

3. **Choose** — ask the user which skill(s) to install. Accepting "all" is valid.

4. **Choose scope** — ask: project (this repo only) or user (all projects)?

5. **Confirm directory** — based on your tool's conventions and the chosen scope, state the exact target directory and ask the user to confirm before proceeding.

6. **Install** — copy each chosen skill's full folder (including any subdirectories) into the confirmed directory.
