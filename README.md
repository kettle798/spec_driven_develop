**English** | [中文](./README.zh-CN.md)

# Spec-Driven Develop

**One Markdown file. Any coding agent. Full pre-development automation.**

Spec-Driven Develop is a platform-agnostic AI agent skill that automates the pre-development workflow for large-scale complex tasks. It is not a framework, not a runtime, not a package manager — it is a single `SKILL.md` file that teaches *any* AI coding agent a structured methodology.

The core mechanism is dead simple: your agent reads a Markdown file containing structured instructions, then follows them. No SDK, no API integration, no platform-specific hooks. If your coding agent can read Markdown — and they all can — it works.

When you tell your agent something like "rewrite this project in Rust" or "migrate to a microservice architecture", it automatically kicks off a standardized preparation pipeline before writing a single line of code:

1. Deep project analysis
2. Task decomposition and planning
3. Progress tracking documentation
4. Task-specific sub-SKILL generation
5. Iterative development with document-driven progress awareness

A master progress file (`docs/progress/MASTER.md`) serves as the agent's "memory anchor" across conversations, so it never loses track of where things stand — no matter how many sessions it takes.

## Why Not Superpowers / oh-my-claude / ...?

The Claude Code ecosystem now has full-blown frameworks with dozens of agents, multi-phase pipelines, and opinionated workflows. They're powerful — but they're also heavy, and they lock you into a single platform.

| | Spec-Driven Develop | Superpowers | oh-my-claudecode |
|---|---|---|---|
| **What it is** | A single SKILL file | Full skills framework + methodology | Multi-agent orchestration system |
| **Core files** | 1 Markdown file (~200 lines) | Plugin with multiple skills, agents, hooks | Plugin with 32+ specialized agents |
| **Dependencies** | None | Requires Claude Code plugin system | Requires Claude Code plugin system |
| **Methodology** | Document-driven planning | Enforced TDD (RED-GREEN-REFACTOR) | Team-based multi-agent delegation |
| **Cross-platform** | Any agent that reads Markdown | Claude Code (primary) | Claude Code only |
| **Philosophy** | Do one thing, do it well | Complete development methodology | Parallel multi-agent orchestration |

Spec-Driven Develop takes a fundamentally different approach: instead of wrapping your agent in a framework, it gives the agent a methodology through a plain Markdown file. No hooks, no runtime overhead, no forced workflows. You keep full control.

**Lightweight does not mean weak.** A well-structured Markdown file can carry a surprisingly sophisticated workflow — project analysis, phased task decomposition, progress tracking, sub-SKILL generation — all without a single line of executable code. The agent reads the instructions and executes them. That's it. The simplicity *is* the feature.

This makes it especially suited for:

- **Teams that already have their own workflow** and just need structured planning for big tasks
- **Multi-platform users** who don't want to be locked into a single agent ecosystem
- **Developers who want control**, not a black-box pipeline deciding how they should code

## Platform Compatibility

The SKILL prompt is written in a generic, platform-neutral way. It gracefully degrades on platforms without certain capabilities — for example, if sub-agents aren't available, it falls back to sequential execution automatically.

**Tested platforms with install scripts:**

- **Claude Code** — installed as a plugin (with enhanced agent/command support)
- **Codex (OpenAI)** — installed as a skill
- **Cursor** — installed as a global or project-level skill

**Any other agent** — copy `SKILL.md` to wherever your agent reads instructions. That's it. The file has no external dependencies and no platform-specific logic. It works with Windsurf, Cline, Aider, Continue, Roo Code, Augment, or any other coding agent that reads Markdown-based skills or system prompts.

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

### Other Agents (Generic)

For any other coding agent, just grab the SKILL file and put it where your agent reads instructions:

```bash
# Download the SKILL.md
curl -sL https://raw.githubusercontent.com/zhu1090093659/spec_driven_develop/main/plugins/spec-driven-develop/skills/spec-driven-develop/SKILL.md -o SKILL.md
```

Where to place it depends on your agent:

| Agent | Location |
|---|---|
| Windsurf | `.windsurf/skills/` or project rules |
| Cline | `.cline/skills/` or custom instructions |
| Aider | Reference via `.aider.conf.yml` or paste into chat |
| Continue | `.continue/` config or system prompt |
| Others | Wherever your agent reads custom instructions or system prompts |

If your agent doesn't have a formal "skills" directory, you can paste the content of `SKILL.md` into its system prompt or custom instructions field — the effect is the same.

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

### Native Task Tracking

When the agent starts a work session, it automatically loads the current phase's pending tasks into the platform's native task tracking tool (e.g. TodoWrite in Claude Code). You get real-time visual progress in your IDE sidebar — no need to open Markdown files manually. MASTER.md remains the persistent source of truth across conversations; the native tool provides in-session visibility.

### Progress Export

An optional script exports your progress data to structured JSON, making it easy to import into external project management tools (Linear, Jira, Notion, etc.):

```bash
python scripts/export-progress.py docs/progress/
```

The output includes project metadata, per-phase task details, and an overall completion summary.

### Archive

When all tasks are marked complete in the master progress file, the agent enters archive mode: it moves all workflow artifacts (analysis, plan, progress, and a copy of the sub-SKILL) into `docs/archives/<project-name>/` and updates an index at `docs/archives/README.md`. Nothing is deleted — everything is preserved for future traceability.

## Architecture: S.U.P.E.R Philosophy

The skill itself — and all code it guides agents to produce — follows the **S.U.P.E.R** design philosophy:

| Principle | Meaning | In Practice |
|:----------|:--------|:------------|
| **S**ingle Purpose | One module, one job | Each reference file, template, and agent handles exactly one concern |
| **U**nidirectional Flow | Data flows one way | Phase 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6, no reverse dependencies |
| **P**orts over Implementation | Define contracts first | Templates serve as port definitions between phases and agents |
| **E**nvironment-Agnostic | Run anywhere | Pure Markdown, no platform lock-in, graceful degradation |
| **R**eplaceable Parts | Swap without ripple | Change one template, one protocol, or one agent without touching the rest |

The full philosophy is bundled as `references/super-philosophy.md` and automatically embedded into every generated sub-SKILL, so development agents follow these principles when writing code.

## Project Structure

```
spec_driven_develop/
├── plugins/spec-driven-develop/              # Self-contained Claude Code plugin
│   ├── skills/spec-driven-develop/
│   │   ├── SKILL.md                          # The core — works on ANY platform
│   │   └── references/
│   │       ├── super-philosophy.md           # S.U.P.E.R architecture principles
│   │       ├── parallel-protocol.md          # Parallel execution protocol
│   │       ├── behavioral-rules.md           # Non-negotiable workflow rules
│   │       └── templates/                    # Document templates (one per concern)
│   │           ├── analysis.md
│   │           ├── plan.md
│   │           ├── progress.md
│   │           ├── archive.md
│   │           └── sub-skill.md
│   ├── agents/                               # Claude Code sub-agents (optional)
│   │   ├── project-analyzer.md
│   │   ├── task-architect.md
│   │   └── task-executor.md
│   └── commands/spec-dev.md                  # /spec-dev slash command (Claude Code)
├── scripts/                                  # Installation & utility scripts
│   ├── install-cursor.sh
│   ├── install-codex.sh
│   ├── install-all.sh
│   └── export-progress.py                    # Export progress to JSON
└── LICENSE
```

The only file that matters for cross-platform use is `SKILL.md`. Everything else — agents, commands, plugin manifests — is platform-specific sugar for Claude Code.

## Star History

<p align="center">
  <a href="https://www.star-history.com/#zhu1090093659/spec_driven_develop&Date" target="_blank">
    <img src="https://api.star-history.com/svg?repos=zhu1090093659/spec_driven_develop&type=Date" alt="Star History" width="600">
  </a>
</p>

## Friendly Links

- [linux.do](https://linux.do)

## License

MIT
