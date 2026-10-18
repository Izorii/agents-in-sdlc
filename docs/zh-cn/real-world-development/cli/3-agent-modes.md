---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 协商方案，使用 Autopilot 根据议题实现筛选功能，并通过 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

之前我们在项目中添加了一个小功能。更大的更改需要更完善的流程。幸运的是，GitHub Copilot CLI 可以融入组织现有的工作流程，帮助我们用正确的方式构建正确的功能。本课是多节课程中的第一课，你将遵循典型的智能体驱动开发流程：从议题出发生成新功能，确保代码有效且功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能开发过程中会继续使用同一对话和分支。通常，不同文件类型的工作会使用不同的分支或 PR，但本研讨会会采用简化流程，以便聚焦核心概念。

本课将：

- 从 GitHub 议题启动新的 Copilot 对话；
- 在 Plan 模式中定义需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续完成此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断增加，访客需要按类别和出版社筛选游戏。待办议题描述了该功能，但诸如是否支持组合类别等细节仍需在编码前达成一致。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 在限定范围内实现功能。

## 背景

在开发工作流中引入 AI 编码智能体，并不会改变软件开发的基本原则。事实上，这些原则变得更加重要！多数开发人员会采用类似以下的流程：

1. 从已创建的议题开始，明确需要完成的工作。
2. 制定实现方案。
3. 构建并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程与上述过程类似。

遵循标准方法可以确保 AI 生成的代码符合要求，并且经过与手写代码相同的审查流程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你输入后再继续。
- **Plan**：智能体先制定计划，但无法编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并不断迭代，直到任务完成。

先从 Plan 模式开始，审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选工作前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建用于筛选工作的分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

留意提示现在以 `#7`（或类似编号）开头。`#` 用于将 GitHub 上的议题或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划可以让你在将任务交给 Copilot 之前，先定义实现功能或完成任务的方式。处理复杂任务时，花些时间规划总是有益的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用之后，输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划！它会先探索项目，再确定最合适的方法。

3. Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答即可，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看计划。
6. 如果计划中的任何部分与你的决定不符，请让 Copilot 修改。

## 批准 Autopilot

计划编写并审查完毕后，就可以实现它了！现在使用 Autopilot，让 Copilot 完成工作。

Autopilot 会让 Copilot 持续迭代问题，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照既定计划不断迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，与其他代码一样，合并前需要审查。现在审查代码并运行网站，确保一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照议题和规划阶段作出的决定检查结果。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并让 Copilot 修复失败项。

## 试用新功能

代码看起来不错，但能否正常运行？现在像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 提示 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按出版社筛选，以及计划中商定的组合筛选方式。
4. 确认重置和无结果时的行为符合议题要求和你的决定。
5. 返回 Codespace，要求 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你使用不同的对话模式构建并审查了功能。本课中，你：

- 从 GitHub 议题启动了新的 Copilot 对话；
- 在 Plan 模式中定义了需求；
- 使用 Autopilot 模式实现了新功能；
- 审查了代码；
- 在转发的浏览器中手动验证了功能。

接下来，通过[使用自定义指令][next-lesson]了解代码如何生成，并确保它遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
