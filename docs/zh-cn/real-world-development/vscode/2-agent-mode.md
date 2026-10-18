---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行多项操作，例如运行测试。开发人员的典型工作流程包括：找出所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……诸如此类。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式可以在 IDE 中更自主地执行任务。它的工作方式类似开发人员：先探索现有项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。下面来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们提供的是两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 智能体模式探索项目、查找相关文件并协调更改；
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 在合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望允许用户按类别和出版社筛选。上一练习已经添加了出版社辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改之前，先了解 Tailspin Toys 网站当前的功能。

该网站是一个面向开发者主题棋盘游戏的众筹平台，用户可以浏览游戏列表和详细信息。它是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库；没有单独的后端 API，也没有客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型、作用域受限的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动 Astro 开发服务器。可以按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束时，你应该已经切回 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新的终端窗口。
3. 创建并切换到用于筛选工作的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示你打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost URL 时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 codespace 的专用隧道，此时网站服务器已在其中运行。

### 浏览网站

网站运行后，可以了解其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括游戏名称、图片和说明。
- **游戏详情页**：选择某个游戏后，会打开详情页，显示名称、说明、出版社和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。不同练习所用的模式和模型可能不同；需要指定时，每个步骤都会说明。

网站的初始版本已经能够正常运行，但我们还想增加新功能。先查看待办事项列表。使用模板设置项目时，系统会自动创建一组议题作为待办事项。让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。Agent 能自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会的作者并未表示偏好任何特定模型。制作本研讨会时使用的是 Claude Sonnet 4.5，因此练习说明中列出了该模型。这样做是希望代码建议相对一致，从而带来良好的体验。不过，由于大型语言模型具有概率性，你可能会收到与本研讨会示例不同的建议。这完全正常，也是预期行为。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问待办事项列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还会根据它判断的用户价值为议题排序。

## 查看指令文件

让智能体生成代码之前，先查看将用于向 Copilot 提供上下文的指令文件。这里将使用[用户界面（UI）指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含为网站添加功能时应采用的方法。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向特定文件类型指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件的位置，因此可以重复利用内容、将较大的指令拆分为便于管理的小文件，并提供可供 Copilot 参考的示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项单独的更新：

- 在数据层（`src/lib/`）中添加或完善筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，加入筛选 UI。

此外，合并前必须运行测试并确认全部通过。Copilot 智能体模式可以帮你完成这些工作。现在开始添加该功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选中 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目，查找与目标功能相关的文件。它会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。执行期间，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会出现 **Keep** 或 **Undo** 按钮，因此无需在工作进行时逐项选择。

7. 根据 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板提示用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至有些测试会失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且没有发现错误。

   ![Copilot 智能体模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意是否存在潜在问题。

> [!CAUTION]
> 请务必审查 Copilot 或任何 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了：

- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改；
- 如何在合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 拓展练习 - 实现分页

随着游戏列表不断增加，网站需要支持分页。运用本练习学到的技巧，提示 Copilot 更新网站以实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件；
- 考虑分页的实现方式，例如是否允许用户选择每页大小，还是采用固定值；
- 编写提示时，提供 Copilot 实现目标功能所需的指导；
- 你可能需要与 GitHub Copilot 迭代交流，要求修改并提供上下文。这是使用 Copilot 时的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
