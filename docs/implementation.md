# Implementation Notes

## Behavior

The skill is prompt-only. It instructs agents to refactor toward readable, pragmatic structure:

- keep top-level control flow visible
- extract helpers only for meaningful domain actions or noisy stable details
- avoid trivial one-line helper indirection
- prefer derived values over surprising mutation
- preserve behavior unless a behavior change is explicitly requested
- run focused validation after editing

The style source is the Obsidian wiki note:

```text
Wiki/prompting-service/readable-refactoring-die-goldene-mitte.md
```

## Agent Package

The repository builds one agent-agnostic skill package. The same `SKILL.md` is copied to each supported agent location:

- Codex: `~/.codex/skills/gm-refactor`
- Claude Code: `~/.claude/skills/gm-refactor`
- ForgeCode: `~/.forge/skills/gm-refactor`

The package intentionally does not include repository docs or task files. It does include `commands/gm-refactor.md` so the runtime package carries the command/custom-prompt wrapper next to the skill.

## Slash Command Compatibility

Codex CLI 0.128 does not register top-level custom slash commands such as `/gm-refactor`. The supported way to explicitly invoke the skill is to mention `$gm-refactor` or ask Codex to refactor in the Goldene Mitte style.

For deprecated custom-prompt compatibility, `task install:codex` also copies:

```text
~/.codex/prompts/gm-refactor.md
```

If the active Codex frontend supports custom prompts, that wrapper is invoked as `/prompts:gm-refactor`, not `/gm-refactor`. Current Codex CLI slash dispatch resolves slash commands against built-ins only, so this file existing on disk does not prove slash registration.

The local Claude Code install also has a `/gm-refactor` command at:

```text
~/.claude/commands/gm-refactor.md
```

This repository stores the command source in `commands/gm-refactor.md`. It is copied into the built skill package, installed to Codex prompts by `task install:codex` for deprecated compatibility, and installed to Claude Code with `task install:claude-command`.

## Installation

`Taskfile.yml` first builds `dist/gm-refactor`, then copies only runtime files into the selected agent skill directory:

- `SKILL.md`
- `commands/gm-refactor.md`

The compatibility installers copy:

- Codex deprecated custom prompt: `~/.codex/prompts/gm-refactor.md`
- Claude Code: `~/.claude/commands/gm-refactor.md`
