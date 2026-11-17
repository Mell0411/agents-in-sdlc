---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方案，使用 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以与组织现有的工作流程协作，确保以正确的方式构建正确的功能。接下来的几课将介绍典型的代理驱动开发流程：从 issue 出发生成新功能、确认代码有效且功能符合预期，最后成功将其合并到项目中。

> [!NOTE]
> 后续功能工作会继续使用同一对话和分支。通常会为不同类型的文件创建不同分支或 PR，但本工作坊为了专注于核心概念，将简化流程。

本课将介绍如何：

- 根据 GitHub issue 开始新的 Copilot 对话；
- 在 Plan 模式中定义需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还会更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩展，访问者需要按类别和发行商筛选游戏。待办 issue 已描述该功能，但筛选类别组合等细节需要在编写代码前确定。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 执行范围明确的实现。

## 背景

在开发流程中引入 AI 编码代理并不会改变基本原则，事实上这些原则更加重要！大多数开发人员都会遵循类似的流程：

1. 从描述需求的已提交 issue 开始；
2. 规划要构建的内容；
3. 编写并审查代码；
4. 运行测试以验证代码；
5. 手动验证新功能；
6. 创建拉取请求 (PR)；
7. 代码经过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准方法，可以确保 AI 生成的代码满足需求，并经历与手写代码相同的审核流程。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换模式：

- **Interactive**：你和代理协同工作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，并且无法编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并不断迭代，直至任务完成。

先进入 Plan 模式并审查计划，然后使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能开发前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果 Codespace 已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中并返回 **Session** 选项卡。

留意提示现在以 `#7`（或类似编号）开头。`#` 让你可以将 GitHub issue 或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划能让你在交给 Copilot 实施前，先确定实现功能或执行任务的方法。对于复杂工作，花一些时间制定计划总是有帮助的。现在切换到 Plan 模式，让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用之后，输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 将开始制定计划！它会先探索项目，再决定最佳实现方式。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何内容与你的决定不符，请让 Copilot 修改。

## 批准 Autopilot

计划已经完成并经过审查，现在开始实现！使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已经完成。

1. 选择**接受计划并使用 Autopilot 构建（推荐）**，或当前版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照计划生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照 issue 和规划期间做出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 审查项目检查结果，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但运行正常吗？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择**在浏览器中打开**。
3. 尝试按类别筛选、按发行商筛选，以及计划中约定的筛选组合。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace 并让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中，你：

- 根据 GitHub issue 开始新的 Copilot 对话；
- 在 Plan 模式中定义需求；
- 使用 Autopilot 模式实现新功能；
- 审查了代码；
- 在转发的浏览器中手动验证了功能。

接下来[使用自定义指令][next-lesson]，深入了解如何让代码遵循书面规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
