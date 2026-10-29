# AIDD-2026-10-29

A minimal [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces)
for the AIDD training. Participants contribute skills on their own branches and
open pull requests into `main`. No application server, dependencies, or build step
is needed.

## Install

You need Claude Code installed with plugin support. Inside Claude Code, run:

```text
/plugin marketplace add Zenika-Training/AIDD-2026-10-29
/plugin install training-skills@aidd-2026-10-29
```

Restart Claude Code to load the plugin, then run:

```text
/training-skills:hello
```

The example only replies with "Welcome to the AIDD skills marketplace!" and does
not use tools or modify files.

## Structure

```text
.claude-plugin/marketplace.json                 # Marketplace catalog
plugins/training-skills/
  .claude-plugin/plugin.json                   # Shared plugin metadata
  skills/
    hello/SKILL.md                             # One directory per skill
```

Skills are discovered automatically from the plugin's `skills/` directory.
Adding a skill to this plugin does not require changing either JSON manifest.

## Contribute a skill

1. Clone this repository and create a branch from the latest `main`:

   ```sh
   git switch main
   git pull --ff-only
   git switch -c add-my-skill
   ```

2. Create `plugins/training-skills/skills/my-skill/SKILL.md`, replacing
   `my-skill` with a unique lowercase, hyphen-separated name. Start with:

   ```markdown
   ---
   name: my-skill
   description: Describe what this skill does and when to use it.
   disable-model-invocation: true
   ---

   Write clear instructions for Claude to follow here.
   ```

   Keep `disable-model-invocation: true` for skills that should run only when
   explicitly invoked. Never include secrets or credentials.

3. From the repository root, validate and test your local plugin:

   ```sh
   claude plugin validate .
   claude plugin validate ./plugins/training-skills
   claude --plugin-dir ./plugins/training-skills
   ```

   In that session, run `/training-skills:my-skill` and check its behavior.
   Use a fresh session after editing a skill. Local testing does not require
   installing the GitHub marketplace.

4. Commit your skill, push your branch, and open a pull request targeting `main`.
   Describe what the skill does, how to invoke it, and how you tested it.
   Keep changes focused on your skill to reduce conflicts with other participants.

## Get merged skills

After contributions merge, update the marketplace in Claude Code:

```text
/plugin marketplace update aidd-2026-10-29
/plugin update training-skills@aidd-2026-10-29
```

Restart Claude Code to load the updated skills.
