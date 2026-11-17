---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前面的几项练习中，你让 Copilot cloud agent 处理了三个不同任务，以改进用户体验和添加功能。Cloud agent 虽然可以自主异步工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的 [agents 页面][agents-page]。在此控制中心中，你可以查看所有有未关闭拉取请求 (PR) 的代理任务、探索执行的操作，甚至可以指导正在进行的会话。

本练习将介绍如何：

- 浏览 agents 页面并监控 cloud agent 任务；
- 指导进行中的会话，要求添加其他功能。

## 场景

将任务分配给代理以创建高对比度模式后，团队意识到也可以顺便添加浅色模式。由于代理已经在更新网站样式并添加切换功能，因此增加此功能很合理。现在需要指导代理，让它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 留意主窗格和左侧窗格中的任务列表。你应该会看到已分配给 Copilot 的任务，包括：
    - 为代码库更新文档；
    - 生成用于修改游戏的 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作。这些操作可能包括：
    - 从存储库检出代码；
    - 为 Copilot 创建工作环境；
    - 设置 MCP 服务器；
    - 执行完成任务所需的各项操作。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作进度和采用的方法而变化。

4. 同时留意右侧显示的拉取请求 (PR) 窗格。在此处可以查看 PR 和已更改文件，进一步监控任务。

## 指导 cloud agent

现在已经了解当前活动任务，可以让 Copilot 在实现高对比度模式时添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。出现类似以下消息时，就表示已经开始工作。
3. 在**Copilot 正在工作时指导活动会话**对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中的 cloud agent 任务屏幕截图，突出显示“Copilot 正在工作时指导活动会话”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！完成发送消息时正在执行的特定步骤后，它会将新请求加入工作流程。

与之前一样，这需要几分钟。你可以借此机会回顾和思考目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，这是监控和指导 GitHub Copilot cloud agent 任务的中心位置。通过此控制中心，你可以跟踪所有正在进行和已完成的任务、审查相关工作，还可以重定向进行中的任务以调整范围或提供额外指导。

本练习介绍了以下概念：

- 使用 mission control 和 agents 页面监控 cloud agent 任务；
- 重定向进行中的会话，要求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进它创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
