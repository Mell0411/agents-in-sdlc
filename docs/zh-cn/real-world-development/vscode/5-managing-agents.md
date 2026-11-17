---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

当你让 GitHub Copilot 进入 agent mode 后，它会自主探索代码库、提出更改、编辑文件并运行命令。由于 Copilot 会在本地实时工作，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以在编辑器中查看每个差异、接受或拒绝单项更改，并通过后续提示指导对话以完善或扩展工作。

本练习将介绍如何：

- 实时监控无障碍代理的工作；
- 审查并接受代理建议的文件编辑；
- 在会话进行时指导代理添加浅色模式切换开关，并保留高对比度模式。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队提出了额外需求：他们也希望为觉得默认主题太暗的用户添加浅色模式切换开关。你无需从头开始新对话，可以指导现有会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联方式显示在 Chat 中，并提供差异以及**保留**/**撤消**控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 代理工作时，滚动查看 Chat 中的工具调用。你会看到文件读取、文件编辑以及代理请求运行的终端命令。
3. 代理提出文件编辑时，差异会以内联方式显示：
    - 选择**保留**以接受更改；
    - 选择**撤消**以还原更改，并让代理再次尝试。
4. 也可以打开 Chat 顶部的 **Files changed** 指示器，查看代理在本次会话中修改的全部文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），代理会暂停并显示**允许**提示。只批准你信任的命令。

5. 等待代理完成高对比度模式。最后会看到一条总结所做更改的消息。

## 指导代理添加浅色模式

高对比度模式完成后，继续在同一对话中添加浅色模式切换开关。继续使用同一 Chat 会话时，代理会保留先前工作的上下文，包括修改的组件、使用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会接着之前的工作继续，探索现有更改，并提出添加浅色模式的方案。
3. 像审查高对比度模式一样，逐项检查新编辑。使用**保留**或**撤消**处理每个差异。
4. 如果代理的方向不符合预期（例如尝试以不喜欢的方式重构切换 UI），可以选择 Chat 输入框旁边的**停止**按钮，然后发送说明要求的后续消息。

> [!TIP]
> 在进行中的会话中指导代理，比重新开始更快。可以通过后续提示要求调整（“简化 CSS，改用 CSS 变量，不要重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式集成到现有设置面板中”）或扩展范围（“同时为两个开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有更改文件的列表。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开部分已更改文件并查看差异。确认实现符合自定义代理提出的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat agent mode 监控并指导了进行中的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本练习介绍了以下概念：

- 实时监控代理的工作；
- 接受或拒绝单项文件编辑；
- 使用后续提示在会话进行中指导代理。

在[下一课][next-lesson]中，你将把无障碍功能更改作为拉取请求发布，并审查本实验中本地自定义代理所做的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 agent mode][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
