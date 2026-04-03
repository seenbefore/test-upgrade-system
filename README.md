# Testing Upgrade System

一个面向 `Vue2 + Java8` 全栈开发者的轻量测试升级仓库。它的目标不是一上来建设完整测试体系，而是帮助“主要靠手工点页面测试”的开发者，先学会最小可用测试能力，再逐步减少线上返工和加班。

## Start Here

1. 先看设计文档：`docs/superpowers/specs/2026-04-03-testing-upgrade-design.md`
2. 再看实施计划：`docs/plans/2026-04-03-testing-upgrade-system.md`
3. 如果你在公司电脑上使用 `Kimi K2.5`，先看：`docs/guides/kimi-k2.5-company-computer-guide.md`
4. 回到真实项目练手时，优先使用：`prompts/kimi/teach-me-one-test-kimi.md`

## Who This Is For

- 主要依赖手工测试的全栈开发者
- 想从 `0` 开始学习后端最小测试能力的人
- 需要在高压排期下，用低负担方式逐步补测试的人
- 想让 AI 协助自己维护风险清单、回归测试和发版检查的人

## What This Repo Contains

- `docs/superpowers/specs/`: 方案设计文档
- `docs/plans/`: 可执行实施计划
- `docs/guides/`: 公司电脑和低模型能力场景下的使用指南
- `prompts/kimi/`: 面向 `Kimi K2.5` 的固定提示词模板

## Suggested Usage Order

1. 先理解整体设计和演进路径
2. 再看实施计划，明确要落哪些资产
3. 在公司电脑上按 `Kimi` 指南执行
4. 从一个真实 `Java8 service` 方法开始，补第一个最小单元测试

## Core Idea

这套方法的核心不是“补很多测试”，而是：

1. 先学会一个最小测试
2. 再把真实 bug 变成回归测试
3. 再维护风险清单和测试债务
4. 最后才逐步演进到更完整的测试体系

## Kimi Quick Entry

如果你在公司电脑上只能使用 `Kimi K2.5`，建议固定使用下面三个提示词：

1. `prompts/kimi/teach-me-one-test-kimi.md`
2. `prompts/kimi/high-risk-change-kimi.md`
3. `prompts/kimi/bug-to-test-kimi.md`

原则只有一条：一次只让模型做一件事，并把输出格式限制死。
