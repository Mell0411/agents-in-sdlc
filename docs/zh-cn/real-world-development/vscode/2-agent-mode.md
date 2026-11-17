---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发人员通常需要查找所有相关文件、进行修改、运行测试、调试、发现遗漏的文件并再次修改……还会有很多类似的工作。

Copilot Agent Mode 正是为此而设计的。

Copilot Agent Mode 能在 IDE 中更自主地工作。它的工作方式类似开发人员：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。下面来看看如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> Agent Mode 和 cloud agent 名称相似，但面向不同的使用场景。Agent Mode 在 IDE 中执行任务，支持快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot Agent Mode 探索项目、找到相关文件并协调多处更改；
- 使用 GitHub Copilot Agent Mode 跨 UI 和数据层实现新功能；
- 在将代码合并到项目之前，审查 Copilot Agent Mode 生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行商筛选。上一练习已添加 publisher helper，现在将使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

在修改代码之前，先浏览 Tailspin Toys 网站，了解它目前提供的功能。

该网站是一个桌游众筹平台，并采用开发者主题。用户可以浏览游戏列表和游戏详情。网站是单个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面在 frontmatter 中直接通过 Drizzle ORM 查询本地 SQLite 数据库，不需要单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能则通过小型、作用域受限且使用标准 DOM API 的 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，可启动单个 Astro 开发服务器，方便你运行网站。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该已经切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost URL 时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 codespace 的私有隧道，此时 Web 服务器正在 codespace 中运行！

### 浏览网站

网站运行后，可以浏览其中的功能。主要功能包括：

- **主页**：显示桌游列表，包括游戏名称、图片和说明。
- **游戏详情页**：选择游戏后会进入详情页，其中包含名称、说明、发行商和类别等信息。

## 使用 Copilot 浏览待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板，然后选择**新建 Chat** 开始干净的对话。模式和模型的选择因练习而异；每个步骤会在适用时说明。

网站的初始实现已经可用，但我们还想添加新功能。先查看待办列表。从模板创建项目时，系统已自动生成一组 issue；现在让 GitHub Copilot 展示这些 issue。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此在说明中列出该模型，希望由此获得相对一致的代码建议和学习体验。不过，LLM 的输出具有概率性，你看到的建议可能与本工作坊中的说明不同。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 的输出具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择**继续**以运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

请注意，Copilot 还会根据它认为对用户最有用的程度，为这些事项确定优先级。

## 审查指令文件

让代理生成代码前，先查看用于向 Copilot 提供上下文的指令文件。项目中的[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)文件介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍访问原则；
   - 指向多种文件类型对应指令文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在目录。这样可以重复使用指引、将较大的指令集拆分成更易于管理的小块，并为 Copilot 提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要在应用中进行以下三项更新：

- 在数据层（`src/lib/`）中添加或完善剩余的筛选逻辑；
- 添加或更新筛选行为的测试；
- 更新游戏列表页面并添加筛选 UI。

此外，合并前还需要运行测试并确认测试通过。Copilot Agent Mode 可以完成这些任务，现在就来实现该功能。

1. 可以继续当前的 Copilot 对话，也可以选择**新建 Chat** 开始新对话。
2. 依次选择**添加上下文**、**指令**和 **ui** 指令文件。

   ![选择 UI 指令文件的屏幕截图示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单仍选中 **Agent**。

   ![Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用待办列表中的相关 issue 提示 Copilot 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何开始探索项目并查找与功能相关的文件。你应该会看到它查找数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂程度和代码库规模，这可能需要几分钟。在此过程中，代码编辑器中可能会显示**保留**和**撤消**按钮。Copilot 完成后，所有更改旁都会显示**保留**或**撤消**选项，因此不必在它工作时立即选择。

7. 按 Copilot 的提示选择**继续**以运行测试。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 处理过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会反复生成代码和运行测试，直到完成任务且不再发现错误。

   ![完整 Copilot Agent Mode 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，确认没有潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他任何 AI 工具生成的代码。

10. 返回运行网站的浏览器，探索新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择**保留**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot Agent Mode 跨 UI 和数据层实现新功能；
- 使用 Copilot Agent Mode 探索项目、找到相关文件并协调多处更改；
- 在将代码合并到项目之前，审查 Copilot Agent Mode 生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习 - 实现分页

随着游戏列表不断增长，网站需要支持分页。使用本练习学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用已有的指令文件；
- 考虑分页方式：允许用户选择每页大小，还是使用固定值；
- 编写提示时，提供足够的指导，让 Copilot 按照你的设想实现功能；
- 可能需要通过多轮交互要求 GitHub Copilot 修改实现并补充上下文。这正是使用 Copilot 时的正常工作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能与适用场景][choose-mode]
- [VS Code 中的 agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
