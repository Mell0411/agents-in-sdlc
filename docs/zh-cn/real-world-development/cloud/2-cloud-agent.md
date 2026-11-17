---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主完成更新代码和添加功能等任务。任务完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给 Copilot，加快开发流程，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot cloud agent：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 了解明确界定 issue 范围的重要性；
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初为网站开发的承包商没有做好文档工作——实际上，代码完全缺少文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望在每款游戏的详情页显示相关游戏，也就是同一类别中的其他游戏。目前不需要完善的实现，只需要一个可用于验收测试用户体验的版本。虽然这是一个阻碍，但目前还有优先级更高的 issue。

这两项工作都可能被不断推迟，很适合交给 Copilot cloud agent。它可以异步处理任务，让开发人员先处理其他事情，再回来审查 Copilot 的成果并确认一切符合预期。

## 介绍 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发人员一样在后台执行任务。就像与开发人员协作一样，你可以通过多种方式启动任务，包括[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。你可以在 Copilot 会话进行时或完成后查看相关操作。准备好审查建议的解决方案时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非万能工具，使用它并不能省去思考。即使看似简单的操作，深入分析后也会发现其复杂性。因此，[为 Copilot cloud agent 合理界定任务范围][cloud-agent-best-practices]非常重要。应将它视为 AI 结对程序员：分阶段工作、学习、尝试和调整。生成式 AI 不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令练习，就看过 `.github/copilot-instructions.md` 的使用方式；如果还没看过，现在可以快速浏览一下。在将工作分配给 Copilot cloud agent 之前，先以只读方式检查存储库中已有的指令文件，以便稍后观察它们的影响。

在存储库的 GitHub 网站界面中打开以下文件；如果已有 codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别是其中对 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

将下一节中的 `Code lacks documentation` issue 分配给 cloud agent 后，留意生成的拉取请求是否采用了 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自上述指令文件。后续练习会在审查 PR 时再次提到这些要求。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行一些设置脚本，确保环境就绪。将任务分配给 Copilot 时也一样，因为它会像软件工程师一样完成相关工作。

Cloud agent 使用 [GitHub Actions][github-actions] 执行任务。你可以在 `.github/workflows/copilot-setup-steps.yml` 文件中创建并配置[专用设置工作流][setup-workflow]，让它在 Copilot 开始工作前运行，以便安装所需的开发工具和依赖项。项目已预先配置该工作流，方便练习顺利进行并展示设置过程。该工作流会为 Copilot 安装 Node.js、项目依赖项和端到端测试所需的浏览器，并为这个单体 Astro 应用准备好已迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 设置运行步骤所需的最低权限。Copilot 会使用自己的令牌执行相关操作。
    permissions:
      # 如果需要在设置步骤中克隆存储库（例如安装依赖项），需要设置 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在都是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个要点：

- 文件中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 留意 `workflow_dispatch` 触发条件。它允许你从存储库的 **Actions** 选项卡手动运行工作流，这样可以在开始等待 Copilot 运行之前确认工作流是否成功。

## 添加文档

虽然每个人都知道文档的重要性，但大多数项目的信息要么已过时，要么完全缺失。这类技术债务常常无人处理，进而降低工作效率，也增加维护代码库或引入新团队成员的难度。Copilot 很擅长创建文档，因此很适合将这类 issue 分配给 Copilot cloud agent。它会在后台生成所需文档，后续练习会回来审查其成果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 生成待办事项列表可能需要几分钟。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适的计划，以及编写代码。

处理过程可能需要几分钟。Copilot cloud agent 的主要目标之一是异步完成任务，让我们能专注于其他工作。现在就利用这一特性，再分配一项任务给 Copilot cloud agent，然后开始编写应用代码、为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一个主要优势是可以拆分工作：你处理一组任务时，它可以同时处理另一组。为设计团队添加相关游戏部分可能不需要很长时间，但这段时间仍可用来完成其他工作。现在将任务分配给 Copilot cloud agent！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

之后，你会看到第一条 issue 评论中出现一组 👀，表示 Copilot 已开始处理任务！

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！Copilot cloud agent 的工作方式与软件工程师相似，你无需主动监控，而是在它完成后审查结果。现在开始了解如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将 issue 分配给 cloud agent，让它异步处理。可以让 Copilot 解决技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本练习介绍了以下概念：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 了解明确界定 issue 范围的重要性；
- 将 issue 分配给 Copilot。

Copilot cloud agent 正在后台工作，现在可以开始创建和使用自定义代理。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并能读取自定义指令；前面的模块已经介绍过这些内容。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
