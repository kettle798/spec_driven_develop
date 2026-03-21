**English** | [中文](./README.zh-CN.md)

# Spec-Driven Develop

**One Markdown file. Zero dependencies. Full pre-development automation.**

Spec-Driven Develop is a cross-platform AI agent skill that automates the pre-development workflow for large-scale complex tasks. It is not a framework, not a runtime, not a package manager — it is a single `SKILL.md` file that teaches your AI agent a structured methodology.

When you tell your agent something like "rewrite this project in Rust" or "migrate to a microservice architecture", it automatically kicks off a standardized preparation pipeline before writing a single line of code:

1. Deep project analysis
2. Task decomposition and planning
3. Progress tracking documentation
4. Task-specific sub-SKILL generation
5. Iterative development with document-driven progress awareness

A master progress file (`docs/progress/MASTER.md`) serves as the agent's "memory anchor" across conversations, so it never loses track of where things stand — no matter how many sessions it takes.

## Why Not Superpowers / oh-my-claude / ...?

The Claude Code ecosystem now has full-blown frameworks with dozens of agents, multi-phase pipelines, and opinionated workflows. They're powerful — but they're also heavy.

| | Spec-Driven Develop | Superpowers | oh-my-claudecode |
|---|---|---|---|
| **What it is** | A single SKILL file | Full skills framework + methodology | Multi-agent orchestration system |
| **Core files** | 1 Markdown file (~200 lines) | Plugin with multiple skills, agents, hooks | Plugin with 32+ specialized agents |
| **Dependencies** | None | Requires Claude Code plugin system | Requires Claude Code plugin system |
| **Methodology** | Document-driven planning | Enforced TDD (RED-GREEN-REFACTOR) | Team-based multi-agent delegation |
| **Cross-platform** | Claude Code, Codex, Cursor | Claude Code (primary) | Claude Code only |
| **Philosophy** | Do one thing, do it well | Complete development methodology | Parallel multi-agent orchestration |

Spec-Driven Develop takes a fundamentally different approach: instead of wrapping your agent in a framework, it gives the agent a methodology through a plain Markdown file. No hooks, no runtime overhead, no forced workflows. You keep full control.

**Lightweight does not mean weak.** A well-structured Markdown file can carry a surprisingly sophisticated workflow — project analysis, phased task decomposition, progress tracking, sub-SKILL generation — all without a single line of executable code. The agent reads the instructions and executes them. That's it. The simplicity *is* the feature.

This makes it especially suited for:

- **Teams that already have their own workflow** and just need structured planning for big tasks
- **Multi-platform users** who work across Claude Code, Codex, and Cursor
- **Developers who want control**, not a black-box pipeline deciding how they should code

## Supported Platforms

- **Claude Code** — installed as a plugin (with enhanced agent/command support)
- **Codex (OpenAI)** — installed as a skill
- **Cursor** — installed as a global or project-level skill

## Installation

### Claude Code

```
/plugin marketplace add zhu1090093659/spec_driven_develop
/plugin install spec-driven-develop@spec-driven-develop
```

After installation, run `/reload-plugins` to activate.

### Codex CLI

Use the built-in skill installer (inside a Codex session):

```
$skill-installer install https://github.com/zhu1090093659/spec_driven_develop/tree/main/plugins/spec-driven-develop/skills/spec-driven-develop
```

Or install via shell:

```bash
bash <(curl -sL https://raw.githubusercontent.com/zhu1090093659/spec_driven_develop/main/scripts/install-codex.sh)
```

### Cursor

```bash
bash <(curl -sL https://raw.githubusercontent.com/zhu1090093659/spec_driven_develop/main/scripts/install-cursor.sh)
```

Or clone the repo and run locally:

```bash
git clone https://github.com/zhu1090093659/spec_driven_develop.git
bash spec_driven_develop/scripts/install-cursor.sh
```

## Usage

### Automatic Trigger

Simply describe your large-scale task to the agent. The skill triggers on keywords like:

- English: "rewrite", "migrate", "overhaul", "refactor entire project", "transform", "rebuild in [language]"
- Chinese: "改造", "重写", "迁移", "重构", "大规模"

### Manual Trigger (Claude Code)

```
/spec-dev rewrite this Python project in Rust
```

### Cross-Conversation Continuity

When working on a long-running task across multiple conversations, the agent reads `docs/progress/MASTER.md` at the start of each new session to restore context and continue from where it left off.

### Cleanup

When all tasks are marked complete in the master progress file, the agent enters cleanup mode: it asks which artifacts you want to keep and removes the rest.

## Project Structure

```
spec_driven_develop/
├── .claude-plugin/
│   └── marketplace.json                   # Claude Code marketplace catalog
├── plugins/spec-driven-develop/           # Self-contained Claude Code plugin
│   ├── .claude-plugin/plugin.json         # Plugin manifest
│   ├── skills/spec-driven-develop/
│   │   ├── SKILL.md                       # Core skill (works on all platforms)
│   │   └── references/doc-templates.md    # Document templates
│   ├── agents/                            # Claude Code sub-agents
│   │   ├── project-analyzer.md
│   │   └── task-architect.md
│   └── commands/spec-dev.md               # /spec-dev slash command
├── scripts/                               # Installation scripts
│   ├── install-cursor.sh
│   ├── install-codex.sh
│   └── install-all.sh
└── LICENSE
```

## Star History

<p align="center">
  <a href="https://www.star-history.com/#zhu1090093659/spec_driven_develop&Date" target="_blank">
    <img src="https://api.star-history.com/svg?repos=zhu1090093659/spec_driven_develop&type=Date" alt="Star History" width="600">
  </a>
</p>

## License

MIT
