---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都保存在同一个分支中。现在可以一起审查这些内容并创建拉取请求。你之前亲自合并了星级评分拉取请求（PR）；这次将允许 **Agent Merge** 管理此流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分成几个单独的 PR。为了简化研讨会流程，你将所有筛选和质量工作保留在同一个对话和分支中，并在此 PR 中一起提交。

本课将：

- 了解 Agent Merge 以及它如何自动管理合并生命周期；
- 检查完整的功能更改和验证证据；
- 创建筛选功能 PR；
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动处理剩余的 PR 工作，同时由开发人员控制合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时变基；如果存储库允许，最后会启用 GitHub 自动合并。

之前都是由你选择 **Merge pull request**。现在可以将这项工作交给 Agent Merge。启用 Agent Merge 之前，仍需审查更改并确认已准备就绪。

## 使用 Agent Merge 管理 PR

所有代码都已生成，现在一起审查代码、创建 PR，并让 Agent Merge 管理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查通过后，它会合并 PR。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果合并受阻，请阅读报告的原因，并在存储库允许的情况下手动完成已审查的合并。

## 总结与后续步骤

你自动化了开发流程中的多个环节，包括生成、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 以及它如何自动管理合并生命周期；
- 检查了完整的功能更改和验证证据；
- 创建了筛选功能 PR；
- 在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索其他实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、会话共享和可选的云委派功能。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
