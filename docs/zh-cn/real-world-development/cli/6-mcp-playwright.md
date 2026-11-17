---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是编写代码。我们还需要处理数据和外部服务，并允许 Copilot 使用其他自动化工具。MCP 服务器可以让 Copilot 超越 CLI 内置功能，使用更多工具和服务。

本课将介绍如何：

- 了解 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它；
- 如果尚不可用，将 Playwright MCP 服务器添加到项目；
- 让代理驱动浏览器，体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，就需要实际与 UI 交互。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改验证，并提高对功能正确性的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时连接外部工具和服务，从而访问最新信息并代表你执行操作。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当代理与外部工具和服务之间的桥梁。每个 MCP 服务器都会提供一组 AI 代理可以访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

此外还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，方便大家查找和为生态系统做贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应审查其源代码、验证发布者并评估安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 中尚未提供 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样浏览网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前你已经手动确认功能按预期运行。现在使用 Copilot 中可用的 Playwright MCP 服务器再次验证！

1. 使用以下提示，让 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 实际上不需要明确告诉 Copilot 使用 MCP 服务器，因为它通常会自行判断。不过，既然知道该使用什么工具，明确指出也有好处！这样更容易得到一致的结果，也可以节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的各个步骤，以确认功能是否正常。
3. 阅读报告，确认所有功能都按预期运行。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向你提供报告。

## 总结和后续步骤

恭喜！你使用 Playwright MCP 服务器通过 Copilot CLI 在真实浏览器中体验了功能。总结一下，你：

- 了解了 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它；
- 如果 Playwright MCP 服务器尚未启用，就添加了它；
- 让代理驱动浏览器，体验筛选功能。

接下来创建 [QA 自定义代理][next-lesson]，将技能与浏览器工具结合到一个专用角色中。

## 资源

- [MCP 到底是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
