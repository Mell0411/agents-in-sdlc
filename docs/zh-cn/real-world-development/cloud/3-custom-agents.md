---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义代理，并为其提供专门的指令、最佳实践、编码模式和领域知识，帮助它更高效地完成特定类型的工作。团队可以将专业知识整理成可重复使用的代理，例如确保符合 [WCAG][wcag] 要求的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，其中至少要有 `name` 和 `description`，后面是用于定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与 agent skills 的区别

自定义代理和 [agent skills][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，用来指导 AI 执行操作。最简单的区分方式是：**自定义代理负责执行工作，而技能提供工具**。

自定义代理有自己的上下文窗口，并负责协调技能（甚至其他代理）来完成任务。在本实验中，无障碍自定义代理会按照无障碍准则审查和更新网站；在执行过程中，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一的正确写法。与使用 AI 的其他方面一样，请通过测试和迭代，找到适合自己环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将介绍如何：

- 定义自定义代理；
- 将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，部分用户觉得当前暗色主题难以阅读，因为文本与背景颜色的对比度不足。为了解决这个无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍访问至关重要，因此你希望尽快实现此功能。接下来会使用自定义代理生成相关功能。

## 审查无障碍自定义代理

项目已经为你创建了无障碍自定义代理。先查看其内容，了解代理会如何指导 Copilot。

返回 codespace，查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续章节，了解其中介绍的内容：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍访问最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见错误和陷阱。

## 创建并分配 issue

Mission control 是管理环境中所有代理的中心位置。在这里可以将任务分配给 Copilot cloud agent、监控任务，甚至重定向会话并提供额外指导。现在将实现高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![代理分配界面截图，突出显示自定义代理和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，以及如何通过专用 AI 助手处理特定任务和领域。自定义代理可以将团队的专业知识和标准整理成可重复使用的指引，帮助 Copilot 更有效地完成特定类型的工作。

本练习介绍了以下概念：

- 自定义代理的定义方式；
- 将任务分配给自定义代理。

Copilot 正在实现高对比度模式，接下来可以在 mission control 中[监控并指导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
