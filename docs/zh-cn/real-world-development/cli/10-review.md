---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用的自定义设置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

在 Tailspin Toys 的连续工作流程中，你使用了 GitHub Copilot CLI。你完成了以下任务：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到预先创建的筛选 issue；
- 添加星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求 (PR)；
- 从筛选 issue 出发，在 Plan 模式中确定实现方案，在 Autopilot 模式中构建功能，并在 Interactive 模式中审查代码；
- 使用自定义指令引导代理、自定义现有 `quality-checks` 技能，并使用该技能运行单元测试、lint 和类型检查；
- 添加 Playwright Model Context Protocol (MCP) 服务器，并使用它在真实浏览器中体验筛选功能；
- 创建并选择质量保证 (QA) 自定义代理，以检查需求、测试覆盖率、技能结果和浏览器证据；
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选功能 PR；
- 探索用于管理上下文、模型、共享和可选 cloud agent 委派的斜杠命令。

## 已交付的功能

本工作坊包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的“尚未评分”状态。
2. **筛选和质量工作流程：**实现筛选功能、更新指令并将其应用到功能代码、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为了简化本工作坊，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：运行自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 代理在 PR 创建前综合检查这些结果、需求和测试覆盖率。

只有发现实际缺口时才需要新增测试；QA 运行后无需添加测试也可能是正确结果。授权合并前，请审查代码和证据；更改后应重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在多个对话中重复使用这些自定义设置，并随着团队需求变化进行调整。指令定义标准，技能描述可重复执行的任务，自定义代理定义专门角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是代理的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前确定方案，使用 **Interactive** 参与范围明确的更改过程，使用 **Autopilot** 处理范围明确的任务。常规编辑可以选择速度更快的模型，复杂任务则可以选择能力更强的模型。

上下文本身仍与基础设施同样重要。清晰说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出。

## 进一步探索

你已经完成核心工作流程。还可以了解一些 Copilot CLI 功能：

- 使用 `/review` 让代码审查代理分析更改；
- 使用 `/rubber-duck` 讨论问题并获得另一种观点；
- 使用 `/fleet` 并行协调互不依赖的子任务；
- 使用 `/worktree` 隔离单独的任务；
- 使用 `/delegate` 将任务发送给 Copilot cloud agent。

## 后续步骤

熟练使用任何工具的最佳方式都是继续练习！将它用于生产代码、个人项目，或者多年来一直想构建却还没开始的小应用。与团队分享经验，并向他们学习。还要记得查看文档。

如果想继续从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
