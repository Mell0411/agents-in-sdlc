---
slug: zh-cn/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** 是一款基于 Copilot CLI 构建的桌面应用，可将代理驱动的开发集中到统一且专注的工作区。它支持并行代理会话、可切换的会话模式、共享画布，以及原生 GitHub issue 和拉取请求管理功能。其中包括 **Agent Merge**，可引导拉取请求完成变基、处理审查反馈、修复持续集成 (CI) 问题并执行合并。

本工作坊围绕 Tailspin Toys 的连续工作流程展开：

1. 准备项目、安装应用、连接存储库，并熟悉工作区和模板创建的待办列表。
2. 进行有明确目标的星级评分更改，在浏览器中审查结果，并手动合并第一个拉取请求 (PR)。
3. 从筛选 issue 出发，在 **Plan** 模式中确定实现方案，在 **Autopilot** 模式中构建功能，然后在 **Interactive** 模式中审查代码。
4. 更新存储库指令并将其应用于筛选功能。
5. 自定义现有 `quality-checks` 技能，并使用它运行项目检查。
6. 添加 Playwright Model Context Protocol (MCP) 服务器，并使用它在浏览器中体验筛选功能。
7. 创建质量保证 (QA) 自定义代理，并用它审查需求、覆盖率和验证证据。
8. 审查完整的筛选功能更改，并对第二个 PR 使用 Agent Merge。
9. 检查画布是否可用，然后使用 Database Explorer 创建并测试由存储库支持的分流画布。如果缺少所需功能，请记录阻碍因素，并使用只读替代方案或继续学习总结课程。

为了让工作坊专注于核心内容，你将创建两个 PR：一个用于星级评分，另一个用于筛选功能（包含指令更新、技能更新、QA 配置和测试）。每个 PR 都从更新后的 `main` 分支开始。筛选和质量工作流程共用一个会话、工作树和分支，这样可以在学习不同工具时接续前序工作。最后的画布练习保留在其会话中，让你专注于创建和测试共享界面，而无需重复 PR 工作流程。

## 课程

| 课程 | 主题 | 说明 |
|--------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 安装 Node.js，并创建 Tailspin Toys 项目副本 |
| [1. 安装 Copilot app][ex1] | 设置 | 安装应用、连接项目并熟悉工作区 |
| [2. 添加星级评分：快速改进][ex2] | 首次更改 | 显示现有评分和未评分时的备用状态，然后合并 PR 1 |
| [3. 代理模式：Plan 和 Autopilot][ex3] | 代理模式 | 根据 issue 规划功能，使用 Autopilot 构建，再使用 Interactive 模式审查 |
| [4. 使用自定义指令引导 Copilot][ex4] | 上下文 | 浏览并更新指令，然后将其应用于筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 浏览现有技能、更改报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 通过 Customize 配置 MCP，并检查筛选行为 |
| [7. 创建并使用 QA 代理][ex7] | 需求和覆盖率 | 创建并选择专用配置，然后收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查和合并 | 审查筛选功能、指令、技能、QA 配置和测试，再对第二个 PR 使用 Agent Merge |
| [9. 探索并创建画布][ex9] | 协作 | 使用 Database Explorer，再创建并测试由存储库支持的分流画布 |
| [10. 总结和后续步骤][ex10] | 总结 | 回顾工作流程、成果和更多资源 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 一台运行 **macOS、Linux 或 Windows** 的计算机
- [ ] 已在计算机上[安装 Git][install-git]

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education]，已验证的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的 agent、MCP、代码审查和 Copilot CLI 功能，因此可用它完成所有环境的练习。

> [!NOTE]
> Copilot app 在本地计算机而不是 codespace 中运行，因此[先决条件练习][ex0]会指导你先安装 Node.js 并创建项目副本，然后再安装应用。

> [!NOTE]
> 如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略启用。此策略与 Copilot CLI 策略不同。

## 开始

**[从先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
