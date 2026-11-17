---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Agent mode →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]是提供此类指导的方式，可告诉 Copilot 不仅要做什么，还要如何完成。

本练习将介绍如何：

- 通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目上下文、编码准则和文档标准；
- 在现有指令下生成筛选功能的第一个数据切片（publisher helper）；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 使用相同提示再次生成代码，观察新标准如何影响生成结果；
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一课中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设置的部分标准。Copilot 的输出具有不确定性。本练习的目的是观察更新指令前后行为的*趋势*，而不是要求输出逐字一致。

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

## 为更改创建分支

接下来按照最佳实践为更改创建分支。

1. 返回上一练习中打开的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令*之前*先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板，然后选择**新建 Chat** 开始干净的对话。模式和模型的选择因练习而异；每个步骤会在适用时说明。

为了观察自定义指令的效果，先在当前指令文件下发送提示。稍后更新指令后，再发送相同提示并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，这样 Copilot 才能应用文件更改。

   ![Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件及其测试。
6. 留意生成的 helper 是一个类型化函数：它将 `db` 客户端作为第一个参数，并返回 publisher 的类型化数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，因此它有可能在未明确要求的情况下添加文档注释。这也没关系；本练习要观察的是更新指令后行为的一致性变化。

## 添加新的存储库标准

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

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择**新建 Chat** 清除对话内容。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将该标准应用于实际代码。下一练习会以此为基础继续开发。

## 提交指令并推送分支

指令文件和存储库中的其他资源一样，需要通过源代码管理进行维护。现在将指令更新和 helper 更改提交到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和 helper 更改都已保存：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为 helper 基础实现更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他辅助更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

现在分支已推送，可以创建拉取请求并将它关联到待办列表中的文档标准 issue。也可以手动操作，不过此处会让 Copilot 使用已连接到项目的 GitHub 工具代为处理。提示 Copilot 查找 issue、创建用于关闭此 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已配置 GitHub 工具，因此无需额外设置。Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录即可。后续练习会介绍这种连接的工作方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选择了 **Agent**。
3. 提示 Copilot 查找与更新指令文件相关的 issue，并从当前分支创建 PR，说明指令更新和新的 publisher helper 基础实现：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 将查找 issue 并创建 PR。
5. 如果系统提示你**允许** Copilot 代表你执行 GitHub 操作，请先检查操作内容，再酌情选择**允许**。
6. PR 创建后，提示 Copilot 合并 PR 并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统提示你**允许** Copilot 代表你执行 GitHub 操作和 shell 命令，请先检查命令，再酌情选择**允许**。

你已经借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

本练习介绍了 Copilot 如何获取指令文件中的上下文。你还在 VS Code 中使用 Copilot Chat 完成了以下任务：

- 发送代码生成提示，并观察 Copilot 根据*现有*指令生成的结果；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 再次发送相同提示，观察生成的代码如何遵循新标准；
- 将指令更新和 helper 基础实现提交到 `main`，供下一练习使用。

接下来在 [agent mode][next-lesson] 中使用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
