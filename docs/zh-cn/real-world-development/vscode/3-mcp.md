---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：Agent mode][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用 agent mode 构建了筛选功能。在创建拉取请求之前，应确认它在浏览器中能够正常工作。你无需亲自在应用中操作，而是可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器为你测试功能，然后发布分支并创建 PR。

本练习将介绍如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支，并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

Agent mode 接入编辑器以外的工具后，功能会更加强大。Copilot 使用 Model Context Protocol (MCP) 连接这些工具。这是一种代理与外部工具和服务通信的标准方式。

![agent mode 的工作原理及其与上下文、LLM 和工具的交互示意图，其中包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时连接外部工具和服务，从而访问最新信息（通过资源）并代表你执行操作（通过工具）。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当代理与外部工具和服务之间的桥梁，并负责管理通信；外部工具可以是现有 API，也可以是 NPM 包等本地工具。每个 MCP 服务器都会提供一组 AI 代理可以访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 代理可以使用这些 API 创建存储库、更新现有存储库，以及管理 issue 和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力。AI 代理可以使用它访问网页、填写表单和选择按钮。

此外还有许多提供不同工具和资源的 MCP 服务器。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，方便大家查找和为生态系统做贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应仔细审查其源代码、验证发布者并评估安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 审查 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可以使用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` 配置由项目模板提供，因此上一练习中 Copilot 能够读取待办列表中的 issue。该配置使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过 HTTP 连接到服务器；Copilot 首次使用其中一个工具时，你需要通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 配置，并与 `github` 并列，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在后台运行浏览器，不会显示窗口。这在没有桌面环境的 codespace 中是必需的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 执行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后提示缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器。Copilot 首次需要使用服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会显示**启动**操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要使用其工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 驱动真实浏览器。你无需亲自浏览应用来检查更改，代理可以打开页面、浏览页面、应用筛选条件并读取结果，然后向你总结观察到的情况。无需离开当前对话，即可快速确认功能是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着代理会分析结构化且带标签的元素（按钮、链接和列表项），与辅助技术使用的方式相同。因此，快速功能测试也能顺便检查基本的无障碍访问情况。

连接服务器并启动应用后，提示 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并执行各个步骤，然后报告结果。根据 issue 中的验收条件检查其总结；如果发现异常，可以继续提问或让 Copilot 修复代码，然后再创建拉取请求。

> [!NOTE]
> 要执行此测试，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器。如果提示缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择**发布**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，包括使用 github.com 和 GitHub 命令行界面 (CLI)。既然你正在使用 GitHub Copilot，不妨让它代为创建 PR！Copilot 可以查找相关 issue，并将其与 PR 关联。

1. 打开 Copilot Chat 面板，然后选择**新建 Chat** 开始新会话。
2. 确认在代理下拉菜单中选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 提示 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择**继续**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你进行 GitHub 身份验证；按照提示完成操作即可。
5. 留意 Copilot 如何搜索 issue、找到相关 issue 并创建 PR。
6. 选择 Copilot 生成的链接，审查拉取请求，但暂时**不要合并**。

## 总结和后续步骤

恭喜！本练习中你完成了以下任务：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支并为筛选功能创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来创建[自定义代理][next-lesson]，简化无障碍审查等专项任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
