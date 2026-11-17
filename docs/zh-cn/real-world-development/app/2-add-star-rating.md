---
title: "第 2 课 - 添加星级评分：快速改进"
description: "在 GitHub Copilot app 中启动第一个代理会话，对游戏卡片进行一项小改动，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

浏览工作区并尝试快速聊天后，现在可以启动**代理会话**，对项目进行第一次更改。先做一个小改动：游戏数据中已有星级评分，但主页的游戏卡片尚未显示评分。你将让代理显示评分、审查更改，并将其作为第一个拉取请求合并。

本课将介绍如何：

- 启动代理会话并了解会话结构；
- 让代理对项目进行小幅且范围明确的更改；
- 在工作区差异视图中审查更改；
- 在本地运行应用并在浏览器中确认更改；
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，并且该评分已经显示在游戏详情页上。不过，主页上的游戏卡片目前只显示标题、类别、发行商和说明。作为热身练习，让代理在每张卡片上显示现有评分。这项小型独立更改非常适合第一次会话。

## 会话的组成

**会话**是与代理进行的对话，在独立工作区中运行。每个会话都有专用的 **git 工作树和分支**，因此可以同时运行多个会话，例如一个添加功能，另一个修复 bug，而不会发生更改冲突。会话按存储库分组显示在侧边栏中，选择任一会话即可切换。

会话中包含三部分：与代理的**对话**、代理探索和编辑文件时的**工具活动**，以及带有差异的**已更改文件**列表。

## 启动会话并请求更改

现在启动新会话，探索项目并实现功能。在[应用设置][prior-lesson]期间，你已从 GitHub 存储库添加项目。接下来为该存储库创建新会话并请求更改。

1. 返回（或打开）GitHub Copilot app。
2. 选择侧边栏顶部的 **+**，开始会话。
3. 选择 `tailspin-toys` 存储库。
4. 在提示框下方选择**新工作树**和 **Interactive** 模式。使用以下提示请求更改：

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. 按 <kbd>Enter</kbd> 将提示发送给 Copilot。

Copilot app 会先创建新的工作树，也就是项目的隔离副本。然后它会探索项目，找到添加新功能所需更新的文件，并创建必要的代码。现在你已经使用 Copilot app 添加了新功能！

## 审查差异

合并前应审查所有 AI 生成的更改，即使是小改动也不例外。现在就在 Copilot app 中检查更改。

1. 在应用右上角选择 **Toggle review panel**。在打开的面板中选择 **Changes**，查看已更改文件及其差异。

    ![GitHub Copilot app 顶部工具栏的屏幕截图，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 留意 `GameCard.astro` 中新增的代码，该文件用于显示游戏详情。代码应与以下示例类似：有评分时显示评分，`starRating` 为 `null` 时显示 `No rating yet`：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性而不是确定性，因此生成的代码可能与上述示例有所不同，但总体应较为相似。

## 检查更改

创建 PR 前，先审查代理自动检查的结果。确认测试覆盖数值类型的 `starRating` 和 `null` 回退状态。缺少先决条件或跳过检查不算通过；批准任何安装请求前先审查。

当然，不能只阅读代码就假定它能正常工作。让 Copilot 打开网站，以便检查更新后的用户界面 (UI)。可以让它启动网站，并在浏览器画布中打开。

> [!TIP]
> 画布是 Copilot app 内的交互式小组件。稍后你将探索自定义画布，甚至创建自己的画布；现在先使用内置的浏览器画布。

1. 使用以下提示，让 Copilot 启动应用并在浏览器画布中打开页面：

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. 稍等片刻，应用将启动，Copilot app 内会打开浏览器窗口。
3. 确认已评分的游戏卡片显示满分五分的评分值。
4. 完成后，使用以下提示让 Copilot 停止为此会话启动的开发服务器并关闭浏览器画布：

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 创建并合并第一个拉取请求

你已完成该功能！现在创建拉取请求 (PR)，将新代码合并到项目中。

1. 选择右上角的 **Create PR**。
2. 如果系统提示，请选择 **Sign in with your browser**，并按照提示完成身份验证。
3. 如果 Copilot 说明存储库策略要求创建 PR 前必须关联 issue，请确认它应遵循该流程。
4. Copilot 会开始创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查窗格中打开拉取请求并查看内容。你可以根据需要在此审查 PR。
6. 准备好后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并 PR！

## 总结和后续步骤

恭喜！你使用 GitHub Copilot app 发布了第一个更改！具体来说，你：

- 启动了代理会话并了解了会话的结构；
- 指示代理对游戏卡片进行小幅且范围明确的更改；
- 在工作区差异视图中审查了更改；
- 在本地运行应用，并在浏览器中确认了星级评分；
- 打开 PR 1、审查其检查结果并明确执行合并。

接下来从筛选 issue 出发，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [在 GitHub Copilot app 中使用代理会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [使用 GitHub Copilot app 管理 issue 和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#安装并配置-github-copilot-app
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
