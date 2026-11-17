---
title: "第 7 课 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，结合 issue 需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中检查筛选体验。现在将这些能力整合到一个自定义代理中，明确规定 QA 流程。

本课将介绍如何：

- 了解自定义代理如何配合指令、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置；
- 选择 QA 代理，并根据筛选 issue 审查其发现。

## 场景

在创建拉取请求 (PR) 之前，Tailspin Toys 希望统一检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。在本工作坊中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，然后在 Copilot CLI 中选择它。

你使用的各种自定义功能各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置告诉 Copilot 如何使用这些能力评估需求并报告发现。QA 配置不会取代这些功能，也不需要启动另一个对话。

## 创建 QA 配置

创建功能 PR 前，让 Copilot 创建一个可复用的 QA 配置。该配置将定义 QA 执行的检查以及必须遵守的限制。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 让默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理之前，先审查其配置文件，确认 Copilot 捕获了预期的 QA 流程和限制。这样可以避免在你只想验证功能时，代理却创建了不完整或范围过大的配置并更改功能代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需字段；`name` 是可选字段，但添加后代理会有明确的显示名称。
3. 阅读配置中的指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，而且不会提交更改或创建拉取请求。
5. 如果生成的配置缺少任何职责或限制，请先让默认代理修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置后，恢复同一筛选功能对话并选择 QA，这样它就可以使用现有上下文中的 issue 和计划决策。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 你刚创建代理，因此它可能暂时不会显示在可用代理列表中。代理已经存在，以上命令可以启用它。

2. 使用以下提示要求 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理会开始工作！
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你在工作流程中添加了可复用的专用角色，并审查了它的工作。本课中，你：

- 了解了自定义代理如何配合指令、技能和 MCP 工具；
- 创建并检查了可复用的质量保证配置；
- 选择了 QA 代理，并根据筛选 issue 审查其发现。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备好供审查。接下来[将这些内容汇总到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
