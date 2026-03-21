[English](./README.md) | **中文**

# Spec-Driven Develop

**一个 Markdown 文件，零依赖，完整的开发前自动化流程。**

Spec-Driven Develop 是一个跨平台的 AI Agent 技能，专门为大规模复杂任务自动化"开发前准备"流程。它不是框架，不是运行时，不是包管理器——它就是一个 `SKILL.md` 文件，教会你的 AI Agent 一套结构化的工作方法论。

当你对 Agent 说出"把这个项目用 Rust 重写"或者"迁移到微服务架构"这样的话时，它会在动手写代码之前，自动执行一套标准化的准备流水线：

1. 深度项目分析
2. 任务分解与规划
3. 进度追踪文档生成
4. 针对每个子任务生成专属 sub-SKILL
5. 以文档驱动的方式进行迭代开发，全程保持进度感知

一个主进度文件（`docs/progress/MASTER.md`）充当 Agent 跨对话的"记忆锚点"，不管对话切换多少次，都不会丢失当前进展。

## 为什么不用 Superpowers / oh-my-claude / ...？

Claude Code 生态里现在已经有了不少重量级选手：几十个 Agent 的编排系统、多阶段流水线、强制性的开发方法论。它们很强大——但也很重。

| | Spec-Driven Develop | Superpowers | oh-my-claudecode |
|---|---|---|---|
| **本质** | 一个 SKILL 文件 | 完整的技能框架 + 方法论 | 多 Agent 编排系统 |
| **核心体积** | 1 个 Markdown 文件（约 200 行） | 插件 + 多个 Skill、Agent、Hook | 插件 + 32 个以上专用 Agent |
| **外部依赖** | 无 | 依赖 Claude Code 插件系统 | 依赖 Claude Code 插件系统 |
| **方法论** | 文档驱动的规划 | 强制 TDD（RED-GREEN-REFACTOR） | Team 模式多 Agent 委派 |
| **跨平台** | Claude Code、Codex、Cursor | Claude Code（主要） | 仅 Claude Code |
| **设计哲学** | 只做一件事，做到极致 | 完整的软件开发方法论 | 并行多 Agent 编排 |

Spec-Driven Develop 走了一条截然不同的路：它不是用框架把你的 Agent 包裹起来，而是通过一个纯 Markdown 文件交给 Agent 一套方法论。没有 Hook，没有运行时开销，没有强制工作流。你始终保有完全的控制权。

**轻量不等于简陋。** 一个结构良好的 Markdown 文件，能承载出乎意料的复杂工作流——项目分析、分阶段任务分解、进度追踪、子 SKILL 生成——全部不需要一行可执行代码。Agent 读取指令，然后执行。就这么简单。这种简单本身就是最大的特性。

这让它特别适合以下场景：

- **已有成熟工作流的团队**，只是在大型任务前需要一个结构化的规划环节
- **多平台用户**，需要在 Claude Code、Codex、Cursor 之间无缝切换
- **想要掌控感的开发者**，而不是被一个黑盒流水线决定该怎么写代码

## 支持平台

- **Claude Code** -- 以插件形式安装（支持增强的 Agent/命令能力）
- **Codex (OpenAI)** -- 以 Skill 形式安装
- **Cursor** -- 以全局或项目级 Skill 形式安装

## 安装

### Claude Code

```
/plugin marketplace add zhu1090093659/spec_driven_develop
/plugin install spec-driven-develop@spec-driven-develop
```

安装完成后，执行 `/reload-plugins` 激活。

### Codex CLI

在 Codex 会话中使用内置的 Skill 安装器：

```
$skill-installer install https://github.com/zhu1090093659/spec_driven_develop/tree/main/plugins/spec-driven-develop/skills/spec-driven-develop
```

或者通过 Shell 脚本安装：

```bash
bash <(curl -sL https://raw.githubusercontent.com/zhu1090093659/spec_driven_develop/main/scripts/install-codex.sh)
```

### Cursor

```bash
bash <(curl -sL https://raw.githubusercontent.com/zhu1090093659/spec_driven_develop/main/scripts/install-cursor.sh)
```

也可以克隆仓库后本地安装：

```bash
git clone https://github.com/zhu1090093659/spec_driven_develop.git
bash spec_driven_develop/scripts/install-cursor.sh
```

## 使用方法

### 自动触发

直接向 Agent 描述你的大规模任务即可，Skill 会根据以下关键词自动触发：

- 英文关键词：rewrite、migrate、overhaul、refactor entire project、transform、rebuild in [language]
- 中文关键词：改造、重写、迁移、重构、大规模

### 手动触发（Claude Code）

```
/spec-dev rewrite this Python project in Rust
```

### 跨对话连续性

在跨多轮对话处理长周期任务时，Agent 会在每次新对话开始时读取 `docs/progress/MASTER.md`，恢复上下文并从上次中断的地方继续。

### 清理

当主进度文件中的所有任务都标记为完成后，Agent 会进入清理模式：它会询问你希望保留哪些产出物，然后移除其余的。

## 项目结构

```
spec_driven_develop/
├── .claude-plugin/
│   └── marketplace.json                   # Claude Code Marketplace 目录
├── plugins/spec-driven-develop/           # 独立的 Claude Code 插件
│   ├── .claude-plugin/plugin.json         # 插件清单
│   ├── skills/spec-driven-develop/
│   │   ├── SKILL.md                       # 核心 Skill（全平台通用）
│   │   └── references/doc-templates.md    # 文档模板
│   ├── agents/                            # Claude Code 子 Agent
│   │   ├── project-analyzer.md
│   │   └── task-architect.md
│   └── commands/spec-dev.md               # /spec-dev 斜杠命令
├── scripts/                               # 安装脚本
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

## 许可证

MIT
