---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可以使用高级功能、查看后台信息并调整其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来探索一些其他实用命令。

本课将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文；
- 使用 `/model` 查看可用模型；
- 了解如何使用 `/share` 导出或共享会话；
- 探索用于并行工作、worktree 和委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已经完成。现在来看看其他功能：管理上下文、切换模型、共享会话，以及选择是否将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的上下文窗口上限。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请在存储库根目录中启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统指令、工具、消息和可用空间之间的分配情况。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口即将用满时自动压缩上下文。想自行选择时间时，可以使用 `/compact`。如果要切换到无关任务并开始新的对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发人员也可能有不同偏好。Copilot CLI 可以列出模型并允许你选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

通过团队协作和分享经验，大家可以一起改进 AI 工具的使用方式。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中的 `/share` 选项。
2. 如果要共享会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容之前先进行审查。会话导出内容可能包含提示、回复和项目详细信息。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库内容或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已经结束。Copilot CLI 还提供了适用于更大任务的命令：

- `/fleet` 可以将互不相关的子任务分配给子智能体并行处理；
- `/worktree` 可以创建隔离的 Git worktree，用于处理单独的任务；
- `/delegate` 可以将任务交给 Copilot 云智能体，异步执行并可能创建拉取请求。

这些命令可能会创建额外的 worktree 或远程任务，因此属于可选功能。试用前，请先创建一个范围明确的新任务，并通过正常工作流审查结果。想深入了解异步智能体工作方式，可以继续学习[云智能体研讨会][cloud-workshop]。

## 总结与后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、共享会话并了解其后台工作。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文；
- 使用 `/model` 查看可用模型；
- 了解如何使用 `/share` 导出或共享会话；
- 探索了用于并行工作、worktree 和委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令等待探索！接下来，[回顾学到的内容][next-lesson]，并了解后续学习建议。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
