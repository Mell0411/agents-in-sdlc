---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]是提供此类指导的方式，可告诉 Copilot 不仅要做什么，还要如何完成。

本练习将介绍如何：

- 通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目上下文、编码准则和文档标准；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会比较更新指令前后 Copilot 的响应。Copilot cloud agent 会异步处理 GitHub issue，因此难以实时并排展示差异。后续练习中审查 cloud agent 创建的拉取请求时，你会看到指令文件如何影响生成结果。

## 指令文件

### 场景

像所有优秀的软件开发团队一样，Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，你可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过这种方式引导 Copilot 提供更相关的建议和代码片段，例如指定偏好的编码约定、库以及注释风格。你可以为整个存储库创建指令，也可以为特定文件类型或任务提供指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个指令文件，会随针对该存储库的**每个**请求发送给 Copilot。此文件应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型创建。例如，为 TypeScript 或 Astro 编写指引，或者为创建 UI 组件或一组单元测试提供指导。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 生成代码，不会影响代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳做法

关于如何创建指令文件的完整介绍超出了本练习的范围。不过，示例项目展示了常见的组织方式。总体而言：

- 在 `copilot-instructions.md` 中集中说明项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明推荐和不推荐的写法。

指令文件没有唯一的正确写法。与使用 AI 的其他方面一样，你可以通过实践找到适合自己项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护一套完善的指令文件。浏览本项目中的文件时，你会发现其中包含多种任务的指引，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式有所不同（例如 VS Code 中的**配置 Chat → 生成代理指令**，或 Copilot CLI 中的 `/init`）；相关练习会在适用时介绍这些方式。
>
> 想寻找模板或入门示例？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花点时间阅读存储库自带的指令文件：一个核心文件 `copilot-instructions.md`，以及一组用于不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网站界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览该文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。查看 **Code standards** 下的嵌套 **GitHub Actions Workflows** 指引。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它指定一个相对于存储库根目录的 glob，用来确定指令适用的文件。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 留意此文件中有关如何为项目创建单元测试的具体指引。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，也可以为 Copilot 提供代码示例。（这里的路径相对于指令文件所在目录，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

本练习中只有这一处需要编辑：添加一项项目级规则，要求使用 TSDoc 文档注释和文件注释头。稍后在本环境的练习中，你会看到 cloud agent 如何遵循这项规则。

编辑前，先创建一个分支（cloud agent 会从 issue 所指向的分支读取指令，但你仍应在独立分支上提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库编码标准添加规则，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常接近第 27 行）。留意该部分介绍了项目编码标准，但尚未包含代码文档规则，这也是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中定义全局指引，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指引。存储库级文件适合放置刚刚添加的文档注释规则等项目范围标准。

## 提交、推送并合并指令更新

Cloud agent 会从 issue 目标分支读取指令。下一练习中将 issue 分配给 Copilot 时，它会从 `main` 分支创建新分支。因此，必须先将指令更新合并到 `main`，cloud agent 才能使用它们。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。这样，cloud agent 就能在后续练习处理 issue 时读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤，直接提交到 `main`。此处创建分支是为了让工作坊更贴近实际项目的工作方式。

## 总结和后续步骤

本练习介绍了 Copilot 如何获取指令文件中的上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。接下来会将第一个 issue [分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
