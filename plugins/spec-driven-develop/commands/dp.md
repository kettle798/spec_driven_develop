---
description: 启动结构化深度讨论，用于问题分析、方案设计和头脑风暴
argument-hint: <问题描述，例如 "我们的 API 响应时间最近突然变慢了">
allowed-tools: [Read, Glob, Grep, Bash, LS, WebFetch, WebSearch, BashOutput]
---

# Deep Discuss — 结构化深度讨论

用户的问题描述: **$ARGUMENTS**

执行此任务时，请完整遵循 `deep-discuss` skill 定义的工作流。该 skill 定义了完整的讨论流程（Phase 1 到 Phase 7）和所有行为准则。

**从 Phase 1 开始**：接收并复述用户提供的信息，确认理解无误后再推进。
