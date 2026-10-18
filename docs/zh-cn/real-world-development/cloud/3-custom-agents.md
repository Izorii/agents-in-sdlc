---
title: "练习 3 - 自定义智能体"
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

使用自定义智能体时，你将了解：

- 如何定义自定义智能体；
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于确保所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，一些用户觉得当前的深色主题难以阅读，因为文本与背景颜色的对比度不足。为了解决这一无障碍访问问题，设计团队提出应实现一个可以开启和关闭的高对比度模式。

由于无障碍访问至关重要，你希望尽快实现这一功能。现在使用自定义智能体生成所需功能。

## 查看无障碍访问自定义智能体

项目已为无障碍访问创建了一个自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后查看无障碍访问自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义智能体的必需字段。

3. 接着浏览并查看以下部分：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍访问最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 创建议题并分配任务

任务控制中心是管理环境中所有智能体的中央位置。你可以将任务分配给 Copilot 云智能体、监控任务，甚至重定向任务并提供额外指导。现在将实现高对比度模式的任务分配给 Copilot。

1. 打开你的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配截图，其中自定义智能体和 Accessibility agent 已突出显示](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域量身打造的 AI 助手。自定义智能体可以将团队的专业知识和标准编写为可重复使用的指引，帮助 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 自定义智能体的定义方式；
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，在任务控制中心[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为在组织中使用自定义智能体做好准备][org-custom-agents]
- [为在企业中使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
