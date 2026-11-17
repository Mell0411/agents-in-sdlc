---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义代理，并为其提供专门的指令、最佳实践、编码模式和领域知识，帮助它更高效地完成特定类型的工作。团队可以将专业知识整理成可重复使用的代理，例如确保符合 [WCAG][wcag] 要求的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，其中至少要有 `name` 和 `description`，后面是用于定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与 agent skills 的区别

自定义代理和 [agent skills][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，用来指导 AI 执行操作。最简单的区分方式是：**自定义代理负责执行工作，而技能提供工具**。

自定义代理有自己的上下文窗口，并负责协调技能（甚至其他代理）来完成任务。在本练习中，无障碍自定义代理会按照无障碍准则审查和更新网站；在执行过程中，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一的正确写法。与使用 AI 的其他方面一样，请通过测试和迭代，找到适合自己环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，部分用户觉得当前暗色主题难以阅读，因为文本与背景颜色的对比度不足。为了解决这个无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍访问至关重要，因此你希望尽快实现此功能。接下来会使用自定义代理生成相关功能。

本练习将介绍如何：

- 审查现有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 审查无障碍自定义代理

项目已经为你创建了无障碍自定义代理。先查看其内容，了解代理会如何指导 Copilot。

返回 codespace，打开终端并从 `main` 创建新分支，用于单独处理无障碍功能（这样 Exercise 3 中的筛选功能 PR 会保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续章节，了解其中介绍的内容：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍访问最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见错误和陷阱。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理后，该聊天会话将遵循相应代理的指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板，然后选择**新建 Chat** 开始干净的对话。模式和模型的选择因练习而异；每个步骤会在适用时说明。

1. 如果代理下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单，其中显示当前代理；默认情况下为 **default**。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并将编辑应用到项目文件。每项编辑都会以内联方式显示在 Chat 中，其中包含文件路径和可审查的差异。
6. 随着编辑内容加入，Chat 顶部的 **Files changed** 指示器会更新，显示代理在本次会话中修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习将审查和指导这项进行中的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，以及如何通过专用 AI 助手处理特定任务和领域。自定义代理可以将团队的专业知识和标准整理成可重复使用的指引，帮助 Copilot 更有效地完成特定类型的工作。

本练习介绍了以下概念：

- 自定义代理的定义方式；
- 在 Copilot Chat agent mode 中使用自定义代理。

接下来[监控并指导代理的工作][next-lesson]，审查实时产生的更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
