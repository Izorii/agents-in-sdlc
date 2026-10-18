---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域量身打造的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业经验编写为可重复使用的智能体，例如用于确保符合 [WCAG][wcag] 标准的无障碍访问智能体、遵循安全编码实践的安全智能体，或维护统一测试模式的测试智能体。

可以使用项目 `.github/agents` 文件夹中的 Markdown 文件定义自定义智能体，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含 YAML frontmatter，至少要有 `name` 和 `description` 字段，后面跟着定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。简单来说，自定义智能体是**工作者**，技能则是工具。

自定义智能体拥有独立的上下文窗口，可以协调技能（甚至其他智能体）来完成工作。在本实验中，无障碍访问自定义智能体会根据无障碍访问指南审查并更新网站；在执行过程中，它还可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方式。与 AI 的其他用法一样，应通过测试和迭代找到适合自己环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，一些用户觉得当前的深色主题难以阅读，因为文本与背景颜色的对比度不足。为了解决这一无障碍访问问题，设计团队提出应实现一个可以开启和关闭的高对比度模式。

由于无障碍访问至关重要，你希望尽快实现这一功能。现在使用自定义智能体生成所需功能。

本练习将：

- 查看现有的无障碍访问自定义智能体；
- 在 Copilot Chat 中使用无障碍访问智能体实现高对比度模式。

## 查看无障碍访问自定义智能体

项目已为无障碍访问创建了一个自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，打开终端，并从 `main` 创建一个新分支以开展无障碍访问工作（将筛选 PR 与练习 3 中的工作分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义智能体的必需字段。

3. 接着浏览并查看以下部分：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍访问最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。你可以选择一个自定义智能体，让该智能体的指令和工具决定当前聊天会话的范围。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。不同练习所用的模式和模型可能不同；需要指定时，每个步骤都会说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（其中显示当前智能体；默认情况下为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍访问智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：它会探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中以内嵌形式显示，并附有文件路径和差异供你审查。
6. 智能体每次进行编辑后，**Files changed** 指示器都会更新，让你了解智能体已修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 正在对存储库进行实际更改，并可能编辑 Astro 组件、CSS 和相关测试等现有文件。

你将在下一课中审查并引导这项正在进行的工作。

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域量身打造的 AI 助手。自定义智能体可以将团队的专业知识和标准编写为可重复使用的指引，帮助 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 自定义智能体的定义方式；
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，你将[监控并引导智能体的工作][next-lesson]，一边审查更改，一边在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
