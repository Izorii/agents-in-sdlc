---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到已创建的筛选议题；
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）；
- 从筛选议题开始，在 Plan 模式中确定方案、在 Autopilot 模式中实现功能，并在 Interactive 模式中审查；
- 使用自定义指令引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中试用筛选功能；
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖率、技能结果和浏览器证据；
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR；
- 探索用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 交付的成果

本研讨会包含两个 PR 里程碑，每个都从更新后的 `main` 创建各自的分支：

1. **星级评分：** 在游戏卡片上显示已有的 `starRating`，并明确显示无评分状态。
2. **筛选与质量工作流：** 实现筛选、更新并应用指令、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建 PR，你一直使用同一对话和分支。为了简化研讨会流程，这些工作会合并到同一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、自己在浏览器中检查，以及通过 MCP 让 Copilot 在浏览器中测试。`quality-checks` 技能运行单元测试、lint 和类型检查，并使用你设置的新格式报告结果。QA 还会结合需求和测试覆盖率审查这些结果，然后再创建 PR。

新增测试应解决真实的覆盖缺口；如果 QA 认为无需添加测试，也可能是正确的。授权合并之前，请审查代码和证据；更改后，应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作方式。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中重复使用这些自定义内容，并根据团队需求调整。指令用于设定标准，技能用于说明可重复任务，自定义智能体用于定义专门角色，MCP 服务器则用于连接外部工具。审查实际更改和工具结果，而不只是智能体的总结。

应根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前梳理方案，使用 **Interactive** 跟进范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。日常编辑可以选择更快的模型，复杂工作则可以选择能力更强的模型。

上下文仍与基础设施同样重要。清楚说明要构建**什么**、**为什么**要构建，以及**如何**构建，会显著影响生成结果。

## 探索更多内容

已学习核心工作流。还可以了解以下 CLI 功能：

- 使用 `/review` 让代码审查智能体分析更改；
- 使用 `/rubber-duck` 讨论问题并获取新的观点；
- 使用 `/fleet` 并行协调互相独立的子任务；
- 使用 `/worktree` 隔离单独任务；
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用水平的最佳方式就是不断使用。可以用它编写生产代码、个人项目，或者实现那个多年来一直想做却还没动手的小应用。与团队分享经验，也向团队成员学习。别忘了查阅文档。

如果想继续在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。如要比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
