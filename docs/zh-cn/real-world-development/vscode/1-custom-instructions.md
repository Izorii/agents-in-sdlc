---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是[指令文件][instruction-files]，用于说明代码生成时应遵循的指导原则。

本练习将：

- 了解项目特定上下文、编码指南和文档标准如何通过存储库自定义指令及按路径应用的指令文件传递给 Copilot；
- 在当前指令生效时，生成筛选功能的第一部分数据层代码（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准；
- 重新运行相同提示，观察生成的代码如何遵循新标准；
- 将指令更新和筛选功能的数据层代码提交到 `main`，供下一课使用。

> [!CAUTION]
> 生成的代码可能不会完全遵循你设定的标准。Copilot 具有概率性。本练习的重点是观察更新指令后行为的整体变化趋势，而不是逐字匹配生成结果。

## 指令文件

### 场景

作为一家优秀的开发公司，Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应有一段注释，说明文件的用途。

通过使用指令文件，可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可以为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。这项功能可以帮助你引导 Copilot 提供更相关的建议和代码片段。你可以指定首选编码约定、库，以及希望添加的注释类型。可以为整个存储库创建指令，也可以为特定文件类型或任务提供指令。

指令文件有两种类型：

- `.github/copilot-instructions.md`：一个指令文件，会随存储库中的**每个**请求发送给 Copilot。该文件应包含项目级信息，也就是大多数发送给 Copilot Chat 或 CLI 的请求都相关的上下文，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可以针对特定任务或文件类型创建这些文件，例如 TypeScript 语言的指引，或创建 UI 组件、编写单元测试的指引。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

创建指令文件的完整讨论超出了本研讨会的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的内容聚焦于项目级指导，例如正在构建的内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为文件类型（单元测试、Astro 组件、数据层等）或特定任务提供说明。
- 使用自然语言，保持指引清晰，并提供代码示例说明应该和不应该采用的写法。

创建指令文件并没有唯一的方法，使用 AI 也一样。通过实践，你会逐步找到适合自己项目的做法。

> [!TIP]
> 使用 GitHub Copilot 的每个项目都应该有完善的指令文件。查看此项目中的文件时，你可能会发现针对多种任务的指引，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。每种使用方式的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适用时介绍这些入口。
>
> 想寻找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看此项目中的自定义指令文件

花些时间阅读此存储库自带的指令文件：一个核心文件 `copilot-instructions.md`，以及一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器中打开，也可以通过 GitHub 网页界面查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指引。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹浏览。可以看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件开头的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定指令的适用范围。此处，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 查看此项目针对单元测试编写的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以把较长的指令拆分为较小、可复用的文件，并为 Copilot 提供生成代码时可参考的示例。（其中的路径相对于指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求提供代码文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

现在按照最佳实践，为更改创建一个分支。

1. 返回上一个练习中打开的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。不同练习所用的模式和模型可能不同；需要指定时，每个步骤都会说明。

要观察自定义指令的影响，先在当前指令文件生效时发送提示。稍后更新指令后，再发送同一提示进行比较。

1. 关闭之前练习中打开的编辑器标签页，确保 Copilot 只获取你需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试文件。
6. 留意生成的辅助函数是一个带类型的函数：第一个参数是 `db` 客户端，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有相关要求，它也可能添加文档注释。如果出现这种情况也没关系；本练习的重点仍然是观察更新指令后的一致性改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，它通常位于第 27 行附近。留意其中记录了项目的编码标准，但尚未说明代码文档要求。这就是刚才生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所见，可以在存储库级别的 `.github/copilot-instructions.md` 中创建全局指引，也可以针对特定语言、文件类型或任务创建 `*.instructions.md` 文件。刚才添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话上下文。
2. 再次选中 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意现在生成的文件开头会出现类似这样的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 留意现在生成的函数包含类似这样的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

现在，你已经引导 Copilot 遵循新的项目标准，并将标准应用于真实代码。下一课将基于这些代码继续实现筛选功能。

## 提交指令并推送分支

指令文件和其他项目资源一样，需要通过源代码管理进行管理。现在将它们提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新的终端窗口。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础代码修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示有其他配套更改（例如测试），也将这些文件加入暂存区。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，就可以创建拉取请求，并关联待办事项列表中的文档标准议题。你可以手动操作，不过也可以让 Copilot 通过已连接到项目的 GitHub 工具代为处理。提示 Copilot 查找议题、创建关联该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已在工作区中配置好 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。请按提示完成操作。后续练习会介绍这种连接方式（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选中了 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和发行商辅助函数基础代码：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 当系统提示你 **Allow** Copilot 代表你执行 GitHub 操作时，请审查相关命令，并酌情选择 **Allow**。
6. 创建 PR 后，提示 Copilot 合并 PR 并让本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当系统提示你 **Allow** Copilot 代表你执行 GitHub 操作和 shell 命令时，请先审查命令，再酌情选择 **Allow**。

现在，你已在 GitHub Copilot 的帮助下创建并合并了拉取请求！

## 总结与后续步骤

你了解了 Copilot 如何从此项目的指令文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下操作：

- 发送代码生成提示，观察 Copilot 根据**现有**指令生成的内容；
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准；
- 重新发送相同提示，观察生成的代码如何遵循新标准；
- 将指令和辅助函数基础代码提交到 `main`，供下一课继续使用。

接下来，在[智能体模式][next-lesson]中使用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
