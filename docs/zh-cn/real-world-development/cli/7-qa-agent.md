---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题要求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在创建一个自定义智能体，将这些能力整合到流程明确的 QA 工作中。

本课将：

- 了解自定义智能体如何与指令、技能和 MCP 工具协作；
- 创建并检查可复用的质量保证配置文件；
- 选择 QA 智能体，并根据筛选议题审查它的发现。

## 场景

在创建拉取请求（PR）之前，Tailspin Toys 希望一致地审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并生成可重复使用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用角色。该文件描述智能体的用途、指令和可用工具。在本研讨会中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义组件各有不同职责：存储库指令描述团队标准，`quality-checks` 技能封装可重复运行的检查，Playwright MCP 提供浏览器工具。QA 配置文件会指导 Copilot 如何使用这些能力评估需求并报告发现。它不会取代这些组件，也不需要开启另一个对话。

## 创建 QA 配置文件

创建功能 PR 之前，让 Copilot 创建可复用的 QA 配置文件。该文件会定义 QA 执行的检查以及必须遵守的边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体之前，先审查它的配置文件，确认 Copilot 已记录预期的 QA 流程和边界。这样可以避免智能体在你只想验证功能时进行不完整或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 是必需字段；`name` 是可选字段，但填写后智能体会有清晰的显示名称。
3. 阅读配置文件中的指令，确认 QA 会从需求开始、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会报告支持证据、在更改实现代码之前征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件遗漏了上述任何职责或边界，请先让默认智能体修订，然后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后，恢复同一筛选功能对话并选择 QA，这样它就能使用已有上下文中的议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建智能体，它可能不会出现在可用智能体列表中，但仍然存在。上述命令会启用它。

2. 使用以下提示，让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作！
4. 完成后，仔细阅读它提供的报告。

## 总结与后续步骤

你在工作流中添加了可复用的专用角色，并审查了它的工作。本课中，你：

- 了解了自定义智能体如何与指令、技能和 MCP 工具协作；
- 创建并检查了可复用的质量保证配置文件；
- 选择了 QA 智能体，并根据筛选议题审查了它的发现。

现在，你已经准备好实现、技能更新、QA 配置文件、测试和验证报告。接下来，[汇总这些内容并创建功能 PR，然后使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
