---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能、自定义其报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是编写代码。我们已经手动验证代码是否正常运行，也使用指令文件确保代码遵循标准。但测试、lint 以及持续集成（CI）的其他环节又该如何处理？

对于这类任务，**智能体技能**最为合适！技能可以帮助 Copilot 正确执行这类操作。

本课将：

- 查看现有的 `quality-checks` 技能；
- 自定义结果的报告格式；
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更容易阅读。

## 指令、脚本和资源

智能体技能将可复用的任务指令、可执行脚本和配套资源打包在一起，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该文件通过 frontmatter 中的名称和说明定义技能，并介绍技能功能及其调用时机。文件夹还可以包含脚本和其他资源子文件夹，供调用技能时使用。

> [!NOTE]
> 技能不一定需要包含其他文件夹或文件。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以位于项目的 `.github/skills` 文件夹中，作为可供团队共享和重复使用的存储库资源；也可以位于用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

现在来查看 Tailspin Toys 团队为运行单元测试、lint 和类型检查创建的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明有助于 Copilot 判断何时调用技能。
3. 阅读指令，留意它们如何引导 Copilot 完成测试和 lint 流程。
4. 留意技能目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言。现在让 Copilot 运行该技能中的三项检查。

1. 返回 Interactive 模式下的筛选功能对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告。

## 自定义报告

我们希望报告能更清楚地说明运行了什么、是否成功，以及工具报告了什么。现在更新技能，让它生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改已完成，现在来试试看！Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用之前**完全相同**的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告，并与第一次的报告进行比较。

## 总结与后续步骤

你自定义并使用了现有的智能体技能。本课中，你：

- 查看了用于单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了结果的报告格式；
- 重新加载并运行了技能。

此更改会与筛选功能一起放入功能 PR。接下来，使用 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，并非本练习中的其他任务：

- [智能体技能规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和内置导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
