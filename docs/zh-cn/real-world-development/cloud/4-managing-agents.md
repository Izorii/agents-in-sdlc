---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，你让 Copilot 云智能体处理了三个独立任务，重点是改善用户体验并添加功能。云智能体虽然以异步和自主的方式工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。通过这个任务控制中心，可以查看所有带有未关闭拉取请求（PR）的智能体任务，了解智能体执行的操作，还可以引导正在进行的会话。

本课将：

- 浏览智能体页面，监控云智能体任务；
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以趁此机会添加浅色模式。既然已经在更新网站样式并实现切换功能，加入此功能也很合理。你希望引导智能体同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
   - 更新代码文档；
   - 为修改游戏生成 API；
   - 为网站添加高对比度模式。
3. 选择其中一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码；
   - 为 Copilot 创建工作环境；
   - 设置 MCP 服务器；
   - 执行完成指定任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 工作状态和所采用的方法而异。

4. 还要留意右侧显示的拉取请求（PR）面板。你可以在这里查看 PR 和更改的文件，以便进一步监控任务。

## 引导云智能体

现在已经了解活动任务的情况，接下来要求 Copilot 在实现高对比度模式的同时添加浅色模式切换开关。

1. 选择负责添加高对比度模式的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成环境设置并开始工作。看到类似下方的消息时，就表示它已开始执行任务。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，其中突出显示了 Steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会开始处理更新后的任务！收到消息时，它可能正在执行某个步骤，因此会在完成该步骤后将新请求纳入工作流程。

与之前一样，此过程需要几分钟。现在可以暂停一下，回顾到目前为止学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中央控制中心。通过该页面，你可以跟踪所有活动任务和已完成任务、审查正在执行的工作，还可以重定向进行中的任务以调整范围或提供额外指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务；
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
