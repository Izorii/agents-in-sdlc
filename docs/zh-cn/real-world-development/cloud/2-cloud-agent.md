---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因一直没有时间实现而积压的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成后，它会生成一个草稿 PR，供开发人员审查。这样可以委派繁琐的任务、加快开发流程，让开发人员专注于更重要的工作。

本课将使用 Copilot 云智能体了解：

- 如何自定义代码生成环境；
- 如何确保操作安全；
- 明确界定议题范围的重要性；
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受聘开发网站的承包商没有留下完善的文档——实际上，网站完全缺少文档。第一步是为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们想在每款游戏的详情页显示同类别的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要用于验收测试用户体验的版本。目前这项工作受到其他优先级更高议题的阻碍。

这两项任务都可能很快被降低优先级，因此非常适合分配给 Copilot 云智能体。它可以异步处理任务，让开发人员专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。与开发人员协作的方式类似，你可以通过多种方式分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。收到分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话进行时或完成后查看 Copilot 的工作。准备好让你审查建议的解决方案时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并没有什么魔法，你仍然需要思考。即使看似简单的操作也可能包含复杂细节，因此[应谨慎界定 Copilot 云智能体任务的范围][cloud-agent-best-practices]。要把它视为 AI 结对编程伙伴：分阶段工作、学习、试验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习已经介绍了自定义指令及其引导 Copilot 的方式。如果你已经完成了自定义指令练习，就已了解 `.github/copilot-instructions.md` 的作用；否则，现在可以快速查看一下。将任务分配给 Copilot 云智能体之前，先以只读方式查看此存储库中已有的指令文件，稍后便于观察它们的影响。

在存储库的 GitHub 网页界面中打开以下文件；如果已有运行中的 codespace，也可以在其中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些指令应用于 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给云智能体时，留意其 PR 中的 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自上述指令文件。稍后的课程审查 PR 时还会再次说明。

## 为 Copilot 云智能体设置开发环境

编写代码通常都需要特定环境和一些设置脚本，以确保环境状态良好，无论执行者是谁。将任务分配给 Copilot 也一样，因为它的工作方式与软件工程师类似。

云智能体使用 [GitHub Actions][github-actions] 环境执行工作。你可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]来自定义环境，让它在开始工作前安装所需的开发工具和依赖项。实验开始前已预先配置此文件，以便顺利完成练习。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库。此项目是单一 Astro 应用：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的 Actions 选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅授予步骤所需的最低权限。Copilot 会获得自己的操作令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果不在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: 检出代码
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在都是 Astro + Drizzle/libSQL）
      - name: 设置 Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: 安装 JavaScript 依赖项
        run: npm ci

      - name: 安装 Playwright 浏览器
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: 设置数据库
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个关键点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。此作业会在 Copilot 开始处理拉取请求前通过 GitHub Actions 执行。
- 留意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，从而在等待 Copilot 运行工作流之前检查其是否正常运行。

## 添加文档

大家都明白文档的重要性，但多数项目的信息要么已经过时，要么完全缺失。这类技术债务经常得不到处理，导致效率下降、代码库更难维护，也让新开发人员更难加入团队。Copilot 擅长创建文档，因此这是一个很适合交给云智能体处理的议题。它会在后台生成所需文档，你将在后续练习中回来审查其成果。

1. 在新的浏览器标签页中打开 GitHub.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详细信息](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应该会创建待办任务列表。

> [!NOTE]
> Copilot 创建的待办任务列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳解决方案。

13. 查看任务列表和 Copilot 即将完成的工作。
14. 向下滚动查看拉取请求时间线，留意 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 滚动查看实时会话，观察 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定合适方案，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们可以专注于其他工作。现在同时利用这项功能，让 Copilot 云智能体处理另一项任务，然后我们可以着手编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务，它处理另一组任务。为设计团队添加相关游戏部分可能不需要太长时间，但也可以把这些时间用于其他工作。现在将任务分配给 Copilot 云智能体。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论会显示一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！Copilot 云智能体的工作方式类似软件工程师，因此无需主动监控；待任务完成后再审查即可。现在来创建并使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体异步处理。Copilot 可以帮助解决技术债务、创建新功能，或将代码从一个框架迁移到另一个框架。

本课介绍了以下概念：

- 自定义代码生成环境；
- 确保操作安全；
- 明确界定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并且可以获取你在前面模块中介绍过的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
