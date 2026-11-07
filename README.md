# agent-skills

Personal agent skills in the open [`SKILL.md` format](https://agentskills.io/specification).
They encode my own decisions about code, architecture, and stacks, so every
project and every coding agent follows the same conventions.

Framework knowledge is **not** here. It comes from official tools:

| Stack | Official source |
|---|---|
| Laravel, Livewire, Flux, Pest, Tailwind | [Laravel Boost](https://laravel.com/docs/boost) (MCP, guidelines, skills, docs search) |
| Flutter, Dart | Dart and Flutter MCP server (`dart mcp-server`) and `flutter/agent-plugins` rules |
| Python | No official agent tooling; covered by `python-conventions` |
| Any library | Context7 MCP |

## Layout

```
skills/
├── practices/       Language-agnostic rules
│   ├── clean-code/
│   ├── error-handling/
│   ├── project-docs/
│   └── testing-strategy/
└── stacks/          How the rules map to a stack
    ├── laravel-conventions/
    │   └── references/   livewire.md, pest.md, tooling.md, errors.md
    └── python-conventions/
        └── references/   tooling.md
```

Skills live under `skills/` because the `skills` CLI only discovers
`SKILL.md` files inside container folders, up to three levels deep.

## Skills

| Skill | Use when |
|---|---|
| `clean-code` | Writing, refactoring, or reviewing code in any language |
| `testing-strategy` | Writing tests, or before refactoring existing code |
| `error-handling` | Throwing, catching, logging, or displaying errors |
| `project-docs` | Starting a project, before and after a task, architecture decisions, editing `AGENTS.md` |
| `laravel-conventions` | Creating or refactoring PHP classes, Livewire components, or routes in Laravel |
| `python-conventions` | Creating or changing a Python project, its dependencies, modules, or tests |

## Install

```bash
# List the skills in this repo
npx skills add Jeyker-Dev/agent-skills --list

# Install all skills into the current project for Claude Code and Gemini CLI
npx skills add Jeyker-Dev/agent-skills -a claude-code -a gemini-cli

# Install only some skills (for example, in a non-Laravel project)
npx skills add Jeyker-Dev/agent-skills -a claude-code -a gemini-cli \
  --skill clean-code --skill testing-strategy --skill error-handling --skill project-docs

# Install globally for every project
npx skills add Jeyker-Dev/agent-skills -a claude-code -a gemini-cli -g
```

Project installs go to `.claude/skills/` (Claude Code) and `.agents/skills/`
(Gemini CLI). Global installs go to `~/.claude/skills/` and `~/.gemini/skills/`.

```bash
npx skills update      # pull new versions
npx skills list        # show installed skills
npx skills remove clean-code
```

## Writing a skill

- Folder name equals the `name` field; lowercase with hyphens.
- `description` in third person: what the skill does and when to use it.
- Only decisions the model would not make by itself. No textbook theory.
- Body under 500 lines; move long detail to `references/`, linked one level
  deep from `SKILL.md`.
- Use one term per concept throughout a skill.
- If a rule can be measured, enforce it with a tool or an architecture test
  and let the skill point to it.

## Planned

| Skill | Notes |
|---|---|
| `architecture/clean-architecture` | Generic layers, once a second stack needs them |
