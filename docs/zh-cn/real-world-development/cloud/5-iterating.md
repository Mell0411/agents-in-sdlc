---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在本实验中，你使用 GitHub Copilot 处理了多个任务，以改进用户体验和添加功能。你让 Copilot 为代码添加文档、实现供设计团队迭代改进的相关游戏功能，以及实现包含高对比度和浅色模式切换开关的无障碍功能。现在来查看代码更改；必要时可以向 Copilot 提供反馈，进一步改进实现。

### 场景

如前面多次提到的那样，生成式 AI 不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来会审查 GitHub Copilot 为代码添加文档、实现相关游戏功能和无障碍功能时提出的更改，然后再为团队其他成员启用审查。

## 安全和 GitHub Copilot cloud agent

由于 Copilot cloud agent 会在没有监督的情况下异步执行任务，因此它受到一些安全限制，以确保操作安全：

- Copilot 只有存储库读取权限，且只能写入它用于处理代码的分支；
- Cloud agent 在 GitHub Actions 中运行，并创建独立的临时环境来工作；
- 运行任何 GitHub Actions 工作流前，都需要经过人工批准；
- [默认情况下，cloud agent 对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对 MCP 服务器的访问。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)，其中为代码添加了文档。你将使用 GitHub.com 中标准的 PR 界面审查更改。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或内容更完善的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。这可能需要几分钟；你可以休息片刻，或回顾迄今为止学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新建的 TSDoc 文档注释和其他文档。具体更改会因实际结果而异。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自练习 2 开始时审查过的自定义指令文件。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到部分工作流正在等待批准的提示。
8. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会在拉取请求的检查部分看到工作流正在排队。如果一切正常，单体 Astro 应用的项目检查将通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向的。你可以像 @ 提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动另一个会话来处理请求。由于结果具有不确定性，无法提供固定的提示文字。可以要求 Copilot 更新以下内容：

- 在每个代码文件开头添加简短的文件注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用结构。

1. 添加评论来请求 Copilot 修改生成的文档，并像提及用户一样标记 **@copilot**。你可以使用上面的任一建议，也可以提出其他想在代码库中看到的文档改进。
2. 选择 **View Session**，观察 Copilot 执行修改。留意 Copilot 如何启动新会话来进行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

你可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或内容更完善的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到部分工作流正在等待批准的提示。
7. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 你应该会在拉取请求的检查部分看到工作流正在排队。如果一切正常，单体 Astro 应用的项目检查将通过。这可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经实现了相关游戏功能！与之前一样，你可以通过迭代让 Copilot cloud agent 继续更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或者添加文件注释头和 TSDoc 文档注释（请记住，分配该 issue 时，你还没有更新自定义指令！）。与之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义代理实现的功能。此 PR 应同时包含在练习 3 中要求的高对比度模式，以及在练习 4 的 mission control 中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或内容更完善的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现时，特别留意以下内容：
   - 用于切换模式的 UI 组件；
   - 使用本地存储保存用户偏好；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到部分工作流正在等待批准的提示。
8. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会在拉取请求的检查部分看到工作流正在排队。如果一切正常，单体 Astro 应用的项目检查将通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能正常，并且重新加载页面后设置仍然有效。

留意无障碍自定义代理如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 - 继续委派工作

Cloud agent 最适合处理待办列表中的实际任务，让你能将注意力转移到其他工作。继续练习这种工作方式，为存储库创建一些 issue 并分配给 Copilot。可以尝试：

- 在游戏详情页创建支持者兴趣表单；
- 为游戏列表页实现分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 环境的练习。在这些课程中，你：

- **检查了此存储库自带的自定义指令文件**，并在稍后的练习中观察这些指令对 cloud agent 输出的影响；
- **将 issue 分配给 Copilot cloud agent**，观察它设置环境、制定计划并异步执行任务；
- **创建并使用自定义代理**处理无障碍工作，添加高对比度和浅色模式切换开关；
- **将 Copilot agents 页面用作控制中心**，监控并指导进行中的无障碍会话；
- **审查并迭代改进 cloud agent 的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 环境的练习。若想继续探索，其他环境可以补充本次实践内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 在 IDE 中体验 Copilot Chat agent mode 和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、agent skills、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给 cloud agent；
- 为其他领域创建自定义代理；
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot agents][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
