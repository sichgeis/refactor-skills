# GM Refactor Skill

This repository is the source of truth for the local `gm-refactor` Agent Skill.

The skill helps agents refactor code in the "Goldene Mitte" style: visible control flow, meaningful extraction boundaries, less surprising mutation, and focused validation. The same skill package can be installed for Codex, Claude Code, and ForgeCode.

The repository also includes a Claude Code `/gm-refactor` command compatibility file. Codex CLI 0.128 does not register top-level custom slash commands such as `/gm-refactor`; use the skill by mentioning `$gm-refactor` or asking for a Goldene Mitte refactor.

## Layout

- `SKILL.md`: agent-agnostic skill manifest and operating instructions.
- `commands/gm-refactor.md`: command/custom-prompt compatibility wrapper.
- `docs/`: project documentation for maintainers.
- `Taskfile.yml`: local development tasks.

## Build

Run:

```bash
task build
```

The build task creates:

```text
dist/gm-refactor
```

Only runtime files are copied into the package:

```text
dist/gm-refactor/SKILL.md
dist/gm-refactor/commands/gm-refactor.md
```

## Install

Install for all supported agents and compatibility wrappers:

```bash
task install
```

Or install one target:

```bash
task install:codex
task install:claude
task install:forge
task install:claude-command
```

The install targets copy the built package into:

```text
~/.codex/skills/gm-refactor
~/.claude/skills/gm-refactor
~/.forge/skills/gm-refactor
```

Codex reads the skill from `~/.codex/skills/gm-refactor`. Current Codex CLI versions do not register `/gm-refactor` as a top-level custom slash command.

For deprecated custom-prompt compatibility, the Codex install also copies:

```text
~/.codex/prompts/gm-refactor.md
```

If custom prompts are supported in your Codex frontend, invoke that file as `/prompts:gm-refactor`, not `/gm-refactor`. The reliable invocation is `$gm-refactor` or a natural-language request like "refactor this in the Goldene Mitte style".

The Codex skill package also keeps a copy at `~/.codex/skills/gm-refactor/commands/gm-refactor.md`, but Codex CLI 0.128 does not load skill-local `commands/` files as slash commands.

The Claude command target also copies:

```text
~/.claude/commands/gm-refactor.md
```

Restart the target agent after installing or changing the skill so the skill manifest is reloaded.

## Verify

Run:

```bash
task verify
```

You can also verify installed targets:

```bash
task verify:codex
task verify:claude
task verify:forge
task verify:claude-command
```

## Update Workflow

1. Edit the source files in this repository.
2. Run `task verify`.
3. Run `task install`.
4. Start a new agent session.

The installed skill is a copied directory, so repository changes are not reflected globally until the install task runs again.
