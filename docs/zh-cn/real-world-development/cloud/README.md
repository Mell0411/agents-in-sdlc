---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可让 GitHub Copilot 在云端异步工作。你在 GitHub 上分配任务后，cloud agent 会在后台处理：探索存储库、进行更改并创建拉取请求，而你可以继续处理其他事情。

在这些练习中，你将添加 cloud agent 会遵循的自定义指令，然后分配一个 GitHub issue 并让它实现需求。你将审查其工作，使用自定义代理来引导工作方式，在代理控制面板中监控和指导会话，最后审查拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加 cloud agent 会遵循的自定义指令 |
| [2. Cloud Agent][ex2] | 异步代理 | 将 issue 分配给 Copilot cloud agent |
| [3. 自定义代理][ex3] | 专用代理 | 审查并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并指导代理会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为帐户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education]，已验证的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的 agent、MCP、代码审查和 Copilot CLI 功能，因此可用它完成所有环境的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot cloud agent 适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，除 Copilot Free 外的所有计划均可使用。对于 Copilot Business 或 Enterprise，管理员必须为组织启用此功能。

> [!NOTE]
> 本环境的练习不涉及 MCP。若要探索如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 环境][cli-harness]或 [VS Code 环境][vscode-harness]。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli-harness]: ../cli/
[vscode-harness]: ../vscode/
