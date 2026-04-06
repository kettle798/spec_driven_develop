[English](./README.md) | **中文**

# Spec-Driven Develop

**一个 Markdown 文件，任意 Coding Agent，完整的开发前自动化流程。**

Spec-Driven Develop 是一个平台无关的 AI Agent 技能，专门为大规模复杂任务自动化"开发前准备"流程。它不是框架，不是运行时，不是包管理器——它就是一个 `SKILL.md` 文件，教会**任何** AI Coding Agent 一套结构化的工作方法论。

核心机制极其简单：你的 Agent 读取一个包含结构化指令的 Markdown 文件，然后照着执行。没有 SDK，没有 API 集成，没有平台特定的 Hook。只要你的 Coding Agent 能读 Markdown——它们都能——就能用。

当你对 Agent 说出"把这个项目用 Rust 重写"或者"迁移到微服务架构"这样的话时，它会在动手写代码之前，自动执行一套标准化的准备流水线：

1. 深度项目分析
2. 任务分解与规划
3. 进度追踪文档生成
4. 针对每个子任务生成专属 sub-SKILL
5. 以文档驱动的方式进行迭代开发，全程保持进度感知

一个主进度文件（`docs/progress/MASTER.md`）充当 Agent 跨对话的"记忆锚点"，不管对话切换多少次，都不会丢失当前进展。

## 为什么不用 Superpowers / oh-my-claude / ...？

Claude Code 生态里现在已经有了不少重量级选手：几十个 Agent 的编排系统、多阶段流水线、强制性的开发方法论。它们很强大——但也很重，而且把你锁死在单一平台上。

| | Spec-Driven Develop | Superpowers | oh-my-claudecode |
|---|---|---|---|
| **本质** | 一个 SKILL 文件 | 完整的技能框架 + 方法论 | 多 Agent 编排系统 |
| **核心体积** | 1 个 Markdown 文件（约 200 行） | 插件 + 多个 Skill、Agent、Hook | 插件 + 32 个以上专用 Agent |
| **外部依赖** | 无 | 依赖 Claude Code 插件系统 | 依赖 Claude Code 插件系统 |
| **方法论** | 文档驱动的规划 | 强制 TDD（RED-GREEN-REFACTOR） | Team 模式多 Agent 委派 |
| **跨平台** | 任何能读 Markdown 的 Agent | Claude Code（主要） | 仅 Claude Code |
| **设计哲学** | 只做一件事，做到极致 | 完整的软件开发方法论 | 并行多 Agent 编排 |

Spec-Driven Develop 走了一条截然不同的路：它不是用框架把你的 Agent 包裹起来，而是通过一个纯 Markdown 文件交给 Agent 一套方法论。没有 Hook，没有运行时开销，没有强制工作流。你始终保有完全的控制权。

**轻量不等于简陋。** 一个结构良好的 Markdown 文件，能承载出乎意料的复杂工作流——项目分析、分阶段任务分解、进度追踪、子 SKILL 生成——全部不需要一行可执行代码。Agent 读取指令，然后执行。就这么简单。这种简单本身就是最大的特性。

这让它特别适合以下场景：

- **已有成熟工作流的团队**，只是在大型任务前需要一个结构化的规划环节
- **多平台用户**，不想被锁死在某个特定的 Agent 生态里
- **想要掌控感的开发者**，而不是被一个黑盒流水线决定该怎么写代码

## 平台兼容性

SKILL 的 prompt 以通用、平台中立的方式编写。在缺少某些能力的平台上会自动优雅降级——比如，如果平台不支持子 Agent，就自动回退到顺序执行。

**已测试并提供安装脚本的平台：**

- **Claude Code** -- 以插件形式安装（支持增强的 Agent/命令能力）
- **Codex (OpenAI)** -- 以 Skill 形式安装
- **Cursor** -- 以全局或项目级 Skill 形式安装

**其他任意 Agent** -- 把 `SKILL.md` 复制到你的 Agent 读取指令的位置就行了。这个文件没有任何外部依赖，没有任何平台特定逻辑。Windsurf、Cline、Aider、Continue、Roo Code、Augment，或者其他任何能读 Markdown 技能/系统提示词的 Coding Agent，都能直接使用。

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

### 其他 Agent（通用方式）

对于其他任何 Coding Agent，只需要拿到 SKILL 文件，放到你的 Agent 读取指令的地方：

```bash
# 下载 SKILL.md
curl -sL https://raw.githubusercontent.com/zhu1090093659/spec_driven_develop/main/plugins/spec-driven-develop/skills/spec-driven-develop/SKILL.md -o SKILL.md
```

放置位置取决于你用的 Agent：

| Agent | 位置 |
|---|---|
| Windsurf | `.windsurf/skills/` 或项目规则 |
| Cline | `.cline/skills/` 或自定义指令 |
| Aider | 通过 `.aider.conf.yml` 引用，或直接粘贴到对话中 |
| Continue | `.continue/` 配置或系统提示词 |
| 其他 | 你的 Agent 读取自定义指令或系统提示词的任何位置 |

如果你的 Agent 没有正式的"技能"目录，可以直接把 `SKILL.md` 的内容粘贴到它的系统提示词或自定义指令字段里——效果是一样的。

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

### 原生任务追踪

Agent 在每次工作会话开始时，会自动把当前阶段的待办任务加载到平台原生的任务追踪工具中（例如 Claude Code 的 TodoWrite）。你可以直接在 IDE 侧边栏里看到实时进度，不需要手动翻 Markdown 文件。MASTER.md 依然是跨对话的持久化真相源，原生工具负责的是会话内的即时可视化。

### 进度导出

一个可选的脚本可以把进度数据导出为结构化 JSON，方便导入到外部项目管理工具（Linear、Jira、Notion 等）：

```bash
python scripts/export-progress.py docs/progress/
```

输出包含项目元数据、每个阶段的任务明细，以及整体完成度汇总。

### 归档

当主进度文件中的所有任务都标记为完成后，Agent 会进入归档模式：它会把所有工作产出物（分析文档、计划文档、进度记录，以及子 SKILL 的副本）移动到 `docs/archives/<项目名>/` 目录下，并更新 `docs/archives/README.md` 索引文件。不会删除任何东西，全部保留以便后续溯源。

## 架构哲学：S.U.P.E.R

这个 Skill 本身，以及它指导 Agent 产出的所有代码，都遵循 **S.U.P.E.R** 设计哲学：

| 原则 | 含义 | 实践体现 |
|:-----|:-----|:---------|
| **S**ingle Purpose | 一个模块，一个职责 | 每个 reference 文件、模板、Agent 只处理一个关注点 |
| **U**nidirectional Flow | 数据单向流动 | Phase 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6，无反向依赖 |
| **P**orts over Implementation | 先定契约再写实现 | 模板作为阶段和 Agent 之间的端口定义 |
| **E**nvironment-Agnostic | 到哪都能跑 | 纯 Markdown，不锁平台，自动优雅降级 |
| **R**eplaceable Parts | 换件不波及全局 | 改一个模板、一个协议或一个 Agent，不用动其余部分 |

完整的哲学文档以 `references/super-philosophy.md` 的形式打包在 Skill 中，并会自动嵌入到每个生成的子 SKILL 里，让开发阶段的 Agent 在写代码时也自觉遵循这套原则。

## 项目结构

```
spec_driven_develop/
├── plugins/spec-driven-develop/              # 独立的 Claude Code 插件
│   ├── skills/spec-driven-develop/
│   │   ├── SKILL.md                          # 核心——全平台通用
│   │   └── references/
│   │       ├── super-philosophy.md           # S.U.P.E.R 架构原则
│   │       ├── parallel-protocol.md          # 并行执行协议
│   │       ├── behavioral-rules.md           # 不可违反的工作流规则
│   │       └── templates/                    # 文档模板（按关注点拆分）
│   │           ├── analysis.md
│   │           ├── plan.md
│   │           ├── progress.md
│   │           ├── archive.md
│   │           └── sub-skill.md
│   ├── agents/                               # Claude Code 子 Agent（可选增强）
│   │   ├── project-analyzer.md
│   │   ├── task-architect.md
│   │   └── task-executor.md
│   └── commands/spec-dev.md                  # /spec-dev 斜杠命令（Claude Code）
├── scripts/                                  # 安装与工具脚本
│   ├── install-cursor.sh
│   ├── install-codex.sh
│   ├── install-all.sh
│   └── export-progress.py                    # 进度导出为 JSON
└── LICENSE
```

跨平台使用唯一需要关心的文件就是 `SKILL.md`。其他的东西——agents、commands、插件清单——都是 Claude Code 平台上的锦上添花。

## Star History

<p align="center">
  <a href="https://www.star-history.com/#zhu1090093659/spec_driven_develop&Date" target="_blank">
    <img src="https://api.star-history.com/svg?repos=zhu1090093659/spec_driven_develop&type=Date" alt="Star History" width="600">
  </a>
</p>

## 友情链接

- [linux.do](https://linux.do)

## 许可证

MIT
