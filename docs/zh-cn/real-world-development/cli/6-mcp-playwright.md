---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并使用它在浏览器中试用筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不仅仅是编写代码。我们还需要处理数据、外部服务，甚至需要让 Copilot 使用其他自动化功能。MCP 服务器可以帮助 Copilot 扩展 CLI 内置的功能，提供更多工具和服务。

本课将：

- 了解模型上下文协议（MCP）及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加 Playwright MCP 服务器；
- 让智能体操作浏览器并试用筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，还需要实际与 UI 交互。你希望 Copilot 能像用户一样使用网站，从而进一步自动化更改验证，并更有把握地确认功能符合预期。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时与外部工具和服务通信，从而获取最新信息并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具及服务之间的桥梁。每个 MCP 服务器都代表一组 AI 智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP 服务器][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP 服务器][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器可用。GitHub 提供 [MCP 注册表][mcp-registry]，方便用户发现并为生态系统做出贡献。

> [!CAUTION]
> 对待 MCP 服务器时，应像对待项目中的其他依赖项一样谨慎。使用前请审查源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 在名称字段中输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 使用 Playwright 让 Copilot 试用功能

之前你已手动确认功能符合预期。现在使用 Copilot 中配置的 Playwright MCP 服务器重复验证。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它一般会自行判断。不过，既然你知道应该使用哪个工具，明确指出也无妨！这样更有助于获得一致的结果，也能减少一些 token 使用量。

2. 观察 Copilot 如何列出通过浏览器确认功能是否正常的步骤。
3. 阅读报告，确认所有行为都符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向你提供报告。

## 总结与后续步骤

恭喜！你使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中试用了功能。回顾一下，你：

- 了解了模型上下文协议（MCP）及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加了 Playwright MCP 服务器；
- 让智能体操作浏览器并试用了筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专门的角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
