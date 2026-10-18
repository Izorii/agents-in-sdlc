---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些操作会在你的计算机上实时进行，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以直接在编辑器中审查每项差异、接受或拒绝单项更改，还可以通过后续提示引导对话，以完善或扩展工作。

本练习将：

- 实时监控无障碍访问智能体的工作；
- 审查并接受建议的文件编辑；
- 在会话进行期间引导智能体添加浅色模式切换开关，与高对比度模式并存。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍访问自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出了一个需求：他们还希望添加浅色模式切换开关，方便觉得默认主题太暗的用户使用。与其从头开始新的对话，不如引导当前会话扩展已有工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每一步，包括探索的文件、运行的命令和建议的编辑。每次文件编辑都会以内嵌差异的形式显示，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 智能体工作期间，在聊天中滚动查看每次工具调用。你会看到文件读取、文件编辑，以及智能体请求运行的终端命令。
3. 智能体提出文件编辑时，差异会以内嵌形式显示：
   - 选择 **Keep** 接受更改；
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在此会话中修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你确认可以运行的命令。

5. 等待智能体完成高对比度模式。完成后，你会看到总结所做更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式就绪后，可以在同一对话中继续添加浅色模式切换开关。由于继续使用当前聊天会话，智能体会保留之前工作的上下文，包括修改的组件、采用的样式方法，以及设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续之前的工作，探索已做出的更改，并提出添加浅色模式的方案。
3. 与高对比度模式一样，审查每项新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合你的预期，例如它试图以不符合预期的方式重构切换 UI，可以选择聊天输入框旁边的 **Stop** 按钮停止它，然后发送澄清提示。

> [!TIP]
> 引导正在运行的会话比从头开始更快。可以使用后续提示要求调整（“简化 CSS，使用 CSS 变量，而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板”）或扩展范围（“也为这两个开关添加键盘快捷键”）。

## 审查工作集

提交工作之前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有已更改文件。你应该会看到 Astro 组件、样式和相关测试等文件的更新。
3. 打开几个已更改文件并检查差异。确认其中体现了自定义智能体要求的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。你将在[下一课][next-lesson]中发布这些更改。

## 总结与后续步骤

你使用 Copilot Chat 智能体模式监控并引导了正在进行的会话：在每项文件编辑完成时审查、接受或拒绝更改，并通过后续提示扩展工作，同时保留会话上下文。

本课介绍了以下内容：

- 实时监控智能体的工作；
- 接受或拒绝单项文件编辑；
- 在会话进行期间发送后续提示，引导智能体继续工作。

在[下一课][next-lesson]中，你将把无障碍访问工作发布为拉取请求，并审查此实验中本地自定义智能体持续生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
