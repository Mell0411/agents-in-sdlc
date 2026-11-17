---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的存储库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于代理驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此存储库、分支和 CI 管道都可以直接使用。它适用于同时指挥多个代理的工作流程，每个代理都在独立工作区中运行；无需自行完成所有工作，也可以自动化重复任务。现在已经安装 Node.js 并准备好项目副本，接下来安装应用并连接该存储库。

本课将介绍如何：

- 安装 GitHub Copilot app 并登录；
- 从 GitHub 存储库将项目添加到应用；
- 熟悉工作区，包括模板创建的待办列表；
- 通过快速聊天了解应用本身。

## 场景

团队正采用 AI 代理处理不断增长的待办列表。Copilot app 提供统一的工作位置来分配 issue、运行代理、审查更改并合并拉取请求。本课将完成安装和连接，并让你熟悉如何开始有关项目的对话。

> [!NOTE]
> 必须拥有符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任一付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略启用。

## 安装并配置 GitHub Copilot app

要使用 GitHub Copilot app，第一步自然是安装它。应用支持 Windows、macOS 和 Linux。现在安装应用、完成身份验证并将 Tailspin Toys 存储库添加到应用。

1. 在浏览器中打开 [GitHub Copilot app 产品页面][download-app]。
2. 下载适用于当前平台的应用，并按照产品页面上的说明进行安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，并按照提示使用 GitHub.com 或 GHE.com 帐户登录。如果使用支持数据驻留的 GitHub Enterprise Cloud，选择 **GitHub Enterprise Cloud**，并在提示时输入 `*.ghe.com` 主机名。

   如果使用 GitHub Enterprise Server，先使用云帐户完成初始设置。然后打开 **Settings**，依次选择 **Accounts**、**Add account** 和 **GitHub Enterprise Server**。按照 [Enterprise Server 登录指南][ghes-sign-in]中的说明，在浏览器中授权应用。

5. 如果初始设置引导流程允许选择存储库，请选择从模板创建的 Tailspin Toys 存储库。也可以跳过存储库选择，在完成初始设置后添加项目。
6. 按提示选择主题，然后完成初始设置。
7. 在侧边栏的 **Sessions** 下查找存储库。如果已经列出，无需再次添加。
8. 如果尚未连接存储库，选择 **Sessions** 旁边的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件练习中记录的所有者和存储库名称。
9. 确认存储库显示在侧边栏中。此处采用的布局会将 `tailspin-toys` 等存储库文件夹分组显示在 **Sessions** 下，每个存储库下方列出单独的会话。

## 熟悉工作区

连接项目后，花点时间熟悉工作区。应用将功能组织在侧边栏的几个区域中：

- **New**：从这里开始新的 Copilot 聊天会话。
- **Pull requests**：通过应用与 GitHub 的原生集成显示你的拉取请求。在这里无需离开应用即可浏览和筛选 PR、检查 CI 状态并审查 PR。
- **Issues**：以同样的方式显示你的 issue。在这里可以浏览和筛选 issue，并从 issue 启动会话。
- **Automations**：按计划或按需运行的已保存代理任务。适合管理待办列表、定期维护项目或处理其他重复工作。总结课程会将其作为后续学习方向，而不是另一项工作坊练习。
- **Customize**：通过 MCP 服务器、插件、技能和其他组件为 Copilot app 添加功能。你将使用此区域配置 Playwright MCP。
- **Chats**：用于提问和集思广益的轻量对话，无需单独创建分支或工作区。首次开始聊天后，此部分才会显示。如果尚未显示，选择 **Sessions** 旁的 **+** 开始第一次聊天。
- **Sessions**：按存储库分组显示文件夹及其代理会话。选择存储库名称（例如 `tailspin-toys`）可打开存储库视图；选择存储库下方的会话可返回相应对话。每个会话都在独立工作区中运行，因此可以同时运行多个会话，而不会发生更改冲突。添加星级评分时会开始第一个会话。

完成工作坊的过程中，你会逐步了解工作区！

> [!TIP]
> 有疑问就问 Copilot！如果不确定如何操作，或某件事是否可行，可以向 Copilot 提问，让它提供指导。

### 查找模板创建的待办列表

几乎每个项目都有待办列表，Tailspin Toys 也不例外。现在来查看创建模板时自动生成的待办列表。

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 存储库文件夹，而不是其下方的单个会话。然后在存储库顶部导航中选择 **Issues**。
2. 按标题查找以下 issue，不要假设它们的编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择一个 issue 并阅读其详细信息。每个 issue 都可以作为代理会话的启动点。完成第一次快速更改后，你会从筛选 issue 启动会话。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示已添加到 Copilot app 的存储库中的项目。若要查看其他存储库中的工作项，请将对应存储库添加到应用。

## 尝试快速聊天

了解应用本身是熟悉应用的好方法，而**快速聊天**正适合这种情况。快速聊天无需创建分支或工作树即可提问或集思广益，非常适合不需要会话的一次性问题。

1. 在侧边栏中选择 **Chats** 旁的 **+**，开始新聊天。如果尚未显示 **Chats**，选择 **Sessions** 旁的 **+** 开始第一次聊天；之后 **Chats** 就会显示。
2. 询问应用自身的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。每个会话都在独立的 git 工作树中运行，因此可以并行运行多个代理，而不会发生更改冲突。你可以随时继续对话或开始新聊天。

## 总结和后续步骤

恭喜！你已安装 GitHub Copilot app、连接项目并探索工作区。本课中，你了解了如何：

- 安装应用并登录 GitHub；
- 从 GitHub 存储库添加项目；
- 熟悉工作区，并在 **Issues** 中查找模板创建的待办列表；
- 使用快速聊天提出一次性问题。

接下来[启动第一个代理会话][next-lesson]，并用它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用代理会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
