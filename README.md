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
│   ├── testing-strategy/
│   └── ui-design/
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
| `ui-design` | Designing, building, restyling, or reviewing any screen, component, or landing page |
| `laravel-conventions` | Creating or refactoring PHP classes, Livewire components, or routes in Laravel |
| `python-conventions` | Creating or changing a Python project, its dependencies, modules, or tests |

## Design skills

`ui-design` only holds my decisions and says which skill is in charge of each
job. The design work itself comes from these third-party skills, installed per
project, never copied into this repo:

| Skill | Install in | Command |
|---|---|---|
| `impeccable` | Every project with UI | `npx skills add pbakaus/impeccable` |
| `fixing-accessibility` | Every project with UI | `npx skills add ibelick/ui-skills --skill fixing-accessibility` |
| `break-ui` | Every project with UI | `npx skills add emilkowalski/skills --skill break-ui` |
| Playwright MCP | Every project with UI | `claude mcp add playwright npx @playwright/mcp@latest` |
| `frontend-design` | New apps | `npx skills add anthropics/skills --skill frontend-design` |
| `taste-skill` | Marketing landing pages only | `npx skills add Leonxlnx/taste-skill` |
| `web-design-guidelines` | Web projects | `npx skills add vercel-labs/agent-skills --skill web-design-guidelines` |
| `pick-ui-library` | React projects only | `npx skills add emilkowalski/skills --skill pick-ui-library` |
| `mobbin` | Only with a Mobbin Pro plan | `claude plugin marketplace add mobbin/skills` |

- Never install `frontend-design` and `taste-skill` in the same project.
- `web-design-guidelines` downloads its rules from the `main` branch of
  `vercel-labs/web-interface-guidelines` on every review. After installing,
  point that URL to a fixed commit so the rules do not change silently.
- Keep each project under about ten design skills; too many compete to trigger.
- Not used: `transitions.dev` (covered by `impeccable` and the motion budget in
  `ui-design`).

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
