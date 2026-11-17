---
title: "第 0 课 - 先决条件"
description: "为 GitHub Copilot app 课程做好准备：为 Tailspin Toys 项目安装 Node.js，并通过模板创建自己的存储库副本。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app 是一款桌面应用，也是你使用 Copilot 和 GitHub 的中央枢纽。它支持快速访问 issue 和拉取请求，并可让你使用 GitHub Copilot 构建项目。本工作坊中，你将在本地使用 GitHub Copilot app 更新基于 Astro 的 Tailspin Toys 应用。开始前，先确认本地已安装 Node.js，然后再安装 Copilot app。

本课将介绍如何：

- 安装 Node.js，以便在本机运行项目测试；
- 通过模板创建自己的 Tailspin Toys 项目副本。

## 安装 Node.js

多节课程会要求代理构建功能，并在本地运行 Tailspin Toys 测试套件。项目只需要一个运行时：[Node.js][nodejs]。请安装当前的 **LTS** 版本。

所有平台上最简单的方式都是使用官方安装程序：

1. 在操作系统中使用 Windows Terminal、macOS 终端或常用工具打开终端窗口。
2. 运行以下命令，检查已安装的 Node.js 和 npm 版本：

    ```shell
    node --version
    npm --version
    ```

3. 如果两条命令都成功，且 Node.js 版本符合项目 README 和 `package.json` 中的要求，可以跳到下一节。

> [!TIP]
> 仅当尚未安装 Node.js 或需要更新时，才需要完成以下步骤。

4. 打开 [Node.js 下载页面][node-download]。
5. 下载适用于当前操作系统的 **LTS** 版本。
6. 运行安装程序并接受默认设置。在 Windows 上，保持选中的 **Add to PATH** 选项。
7. 安装完成后，打开新的终端窗口。
8. 在新终端窗口中运行以下命令，确认安装成功：

    ```bash
    node --version
    npm --version
    ```

9. 应显示已安装的 Node.js 和 npm 版本。

> [!IMPORTANT]
> Copilot app 中的代理也必须能够运行 `node --version` 和 `npm --version`。常用终端中运行成功，不代表应用也能访问这些命令。安装 Node.js 或更改 PATH 后，请完全退出并重新打开应用。如果会话中仍有任一命令无法运行，请在继续前按照[会话环境故障排除步骤][session-environment]操作。

> [!IMPORTANT]
> 每个工作树也需要项目依赖项和用于 E2E 检查的 Playwright Chromium。准备工作树时，请遵循 Tailspin Toys 存储库 README，并在批准前审查任何安装请求。

## 设置练习存储库

你将在 Tailspin Toys 项目的个人副本中进行操作。现在从[模板存储库][template-repository]创建副本。新存储库包含练习所需的所有文件，安装应用后会将其连接到应用。

1. 在新的浏览器窗口中打开本练习的 GitHub 存储库：`https://github.com/github-samples/tailspin-toys`。
2. 在练习存储库页面上选择**使用此模板**按钮，然后选择**创建新存储库**。

    ![使用此模板按钮的下拉菜单中已选择“创建新存储库”](../../../_images/app-0-use-template.png)

3. 如果你是在 GitHub 或 Microsoft 举办的活动中参加本工作坊，请遵循导师的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![创建新存储库表单，其中将 github-samples/tailspin-toys 设为模板并已填写存储库名称](../../../_images/app-0-create-repository.png)

4. 记下所创建存储库的路径（**organization-or-user-name/repository-name**），稍后练习会用到。

> [!NOTE]
> 从模板创建存储库时，系统会自动为你创建 GitHub issue 待办列表。整个工作坊都会使用这些 issue，无需自行创建。

请使用工作坊模板的全新副本。该模板包含存储库指令、应用代码、测试、`quality-checks` 技能和现有画布扩展。你将在本工作坊中自定义技能并创建 QA 代理。如果使用较旧的副本，请向主持人确认其中是否包含所需文件。

## 总结和后续步骤

准备工作已完成！本课中你：

- 安装了 Node.js，以便在本机构建和测试项目；
- 通过模板创建了自己的 Tailspin Toys 存储库副本。

接下来[安装 GitHub Copilot app][next-lesson]，连接刚创建的存储库并熟悉工作区。

## 资源

- [下载 Node.js][node-download]
- [从模板创建存储库][template-repository]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#从-issue-启动会话
