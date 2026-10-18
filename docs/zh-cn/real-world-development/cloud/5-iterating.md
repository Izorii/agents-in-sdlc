---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

整个实验过程中，你让 GitHub Copilot 处理了多个任务，用于改善用户体验并添加功能。你要求 Copilot 为代码添加文档、构建设计团队可以迭代改进的相关游戏功能，以及实现包括高对比度和浅色模式开关在内的无障碍功能。现在查看代码更改，并在必要时向 Copilot 提供反馈，改进其工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。现在先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改，然后再为团队其他成员启用审查。

## 安全与 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此设置了一些安全约束，确保整个过程安全：

- Copilot 只能读取存储库，并且**只能**写入用于存放其代码的分支。
- 云智能体在 GitHub Actions 中运行，会创建独立且临时的环境来处理任务。
- 运行任何 GitHub Actions 工作流之前，都必须由人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也在限制范围内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），其中包含为代码添加的文档。你将通过 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 对外部资源（包括外部 MCP 服务器调用）的访问受到限制。如有需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 在 GitHub.com 上打开你的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求，也可能是更完善的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作，这可能需要几分钟。你可以休息一下，或回顾到目前为止学到的内容。

4. 拉取请求就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新添加的 TSDoc 文档注释和其他文档。具体更改可能不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 审查完毕且确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应该会在拉取请求的检查部分排队运行。正常情况下，单个 Astro 应用的项目检查应该全部通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 进行更改

与 Copilot 协作处理拉取请求并不是单向的。你可以像提及团队其他成员一样，在 PR 评论或代码内联评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果具有非确定性，我们无法提供必须照搬的提示文本。可以要求 Copilot：

- 在每个代码文件顶部添加注释头，简要说明文件用途；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，说明 Astro 应用的结构。

1. 在评论中提出修改生成文档的要求，并像提及其他用户一样提及 **@copilot**。可以使用上面的建议，也可以提出其他希望 Copilot 为代码库中的文档进行的更改。
2. 选择 **View Session**，观察 Copilot 执行任务。留意 Copilot 如何启动新会话来进行更新。
3. 选择 **Back to pull request**，返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。确认无误后，可以将 PR 从草稿状态改为就绪，然后合并到主分支。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题创建的 PR。

1. 在 GitHub.com 上打开你的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR，也可能是更完善的标题。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完毕且确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应该会在拉取请求的检查部分排队运行。正常情况下，单个 Astro 应用的项目检查应该全部通过。此过程可能需要几分钟。
9. **可选：** 也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经实现了相关游戏功能！与之前一样，你可以通过迭代让 Copilot 云智能体继续改进。例如，可以要求 Copilot 调整显示的相关游戏数量，或确保添加注释头和 TSDoc 文档注释（请记住：分配此任务时，你还没有更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中添加新评论来提出要求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍访问自定义智能体实现的功能。此 PR 应包含你在练习 3 中分配的高对比度模式，以及你在练习 4 的任务控制中心中提出的浅色模式。

1. 在 GitHub.com 上打开你的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR，也可能是更完善的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换显示模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完毕且确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应该会在拉取请求的检查部分排队运行。正常情况下，单个 Astro 应用的项目检查应该全部通过。此过程可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认它们能够正常运行，并且在重新加载页面后仍保留设置。

请留意无障碍访问自定义智能体如何引导 Copilot 遵循无障碍访问最佳实践来实现这些功能。如果发现无障碍访问方面的问题或改进机会，可以像之前审查 PR 一样，在评论中提及 **@copilot** 并要求更新。

## 可选练习 — 继续委派任务

云智能体最适合处理真实的待办事项，让你可以专注于其他工作。可以通过练习养成这种习惯：在自己的存储库中创建更多议题并分配给 Copilot。可以尝试：

- 在游戏详情页创建支持者兴趣表单；
- 为游戏列表页面实现分页；
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。在这些课程中，你：

- **查看了此存储库自带的自定义指令文件**，之后可以观察它们对云智能体输出的影响；
- **将议题分配给 Copilot 云智能体**，观察它如何设置环境、制定计划并异步执行任务；
- **创建并使用自定义智能体**，实现无障碍访问功能，添加高对比度和浅色模式切换开关；
- **将 Copilot 智能体页面用作任务控制中心**，监控并引导正在进行的无障碍访问会话；
- **审查并迭代改进云智能体创建的拉取请求**，通过提及 `@copilot` 请求更改，并批准工作流。

## 回顾与后续步骤

你已完成云智能体学习路径。想继续探索的话，可以学习其他学习路径，进一步实践本课介绍的内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中交付两个经过审查的更改，并使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge 和可选的 `/delegate` 云智能体桥接功能。

也可以在自己的存储库中尝试以下后续练习：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
