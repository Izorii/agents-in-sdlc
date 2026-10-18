---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解你**要做什么**，也了解你希望**如何完成**。

本练习将：

- 了解项目特定上下文、编码指南和文档标准如何通过存储库自定义指令及按路径应用的指令文件传递给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，这里不会运行更新前后的提示进行对比。Copilot 云智能体会异步处理 GitHub 议题，因此很难实时并排展示指令的影响。稍后审查云智能体生成的拉取请求时，就能看到指令文件如何影响结果。

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
- `.github/instructions/*.instructions.md`：可以针对特定任务或文件类型创建这些文件，例如 TypeScript 或 Astro 语言指引，或创建 UI 组件、编写单元测试的指引。

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

花些时间阅读此存储库自带的指令文件：一个核心文件 `copilot-instructions.md`，以及一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指引。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹浏览。可以看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件开头的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定指令的适用范围。此处，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 查看此项目针对单元测试编写的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以把较长的指令拆分为较小、可复用的文件，并为 Copilot 提供生成代码时可参考的示例。（其中的路径相对于指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求提供代码文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来是本练习中唯一需要编辑的内容：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。后续练习中，云智能体会在处理你分配的议题时读取这项规则。

编辑之前，先创建一个分支（云智能体会从议题目标分支读取指令，但仍要通过分支提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，它通常位于第 27 行附近。留意其中记录了项目的编码标准，但尚未说明代码文档要求。这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所见，可以在存储库级别的 `.github/copilot-instructions.md` 中创建全局指引，也可以针对特定语言、文件类型或任务创建 `*.instructions.md` 文件。刚才添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能获取这些指令。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后的练习中，云智能体处理分配给它的议题时就会读取这些指令。

> [!TIP]
> 如果想在本研讨会中直接使用 `main`，可以跳过创建分支，直接提交到 `main`。此处创建分支是为了让研讨会模拟真实项目中的操作方式。

## 总结与后续步骤

你了解了 Copilot 如何从此项目的指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的全局存储库标准。接下来的学习路径中，云智能体创建的拉取请求会展示这项标准的应用效果。

接下来，将你的第一个议题[分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
