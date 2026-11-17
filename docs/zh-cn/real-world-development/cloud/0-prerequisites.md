---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot cloud agent 练习前，需要做好准备。你将创建一份 Tailspin Toys 存储库副本，并启动一个 [codespace][codespaces]，用于编辑指令文件和审查 cloud agent 生成的工作。

## 设置练习存储库

你将从[模板][template-repository]创建存储库副本。新存储库包含练习所需的所有文件，后续练习都会使用它。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在练习存储库页面上选择**使用此模板**按钮，然后选择**创建新存储库**。

    ![使用此模板按钮](../../../_images/ex0-use-template.png)

3. 如果你是在 GitHub 或 Microsoft 举办的活动中参加本工作坊，请遵循导师的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下所创建存储库的路径（**organization-or-user-name/repository-name**），稍后练习会用到。

## 创建 codespace

接下来，你将使用 codespace 完成练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但比手动安装所有服务快得多！你可以利用这段时间了解 GitHub Copilot 的其他功能，接下来会介绍这些功能。

> [!CAUTION]
> 后续练习还会使用此 codespace。现在先让它保持打开状态，并留在浏览器选项卡中。

> [!NOTE]
> 本工作坊可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都包含顺利完成练习所需的先决条件。如果你更愿意在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择**在容器中重新打开**。VS Code 将构建与 codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜你创建了练习存储库副本！你也启动了 codespace，后续会在与 Copilot cloud agent 协作时使用它。

## 下一步

接下来添加 cloud agent 会遵循的自定义指令。继续学习[练习 1：自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
