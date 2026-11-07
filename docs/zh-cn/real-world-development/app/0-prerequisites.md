---
title: "第 0 课 - 先决条件"
description: "为 GitHub Copilot app 课程做好准备：为 Tailspin Toys 项目安装 Node.js，并通过模板创建自己的存储库副本。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app 是一款桌面应用，也是使用 Copilot 和 GitHub 的中心入口。它可以快速访问议题和拉取请求，并使用 GitHub Copilot 构建项目。本研讨会中，你将通过 GitHub Copilot app 在本地处理基于 Astro 构建的 Tailspin Toys 应用。开始前，先确认本地已安装 Node.js，再安装 Copilot app。

本课将介绍如何：

- 安装 Node.js，以便在计算机上运行项目测试；
- 通过模板创建自己的 Tailspin Toys 项目副本。

## 安装 Node.js

多节课程会要求智能体构建功能，并在本地运行 Tailspin Toys 测试套件。这需要 **[Node.js][nodejs]**，它是项目唯一需要的运行时。安装当前的 **LTS** 版本。

所有平台上最简单的方式都是使用官方安装程序：

1. 在操作系统中打开终端窗口，例如 Windows Terminal、macOS 终端或常用的其他终端工具。
2. 运行以下命令，检查已安装的 Node.js 版本：

    ```shell
    node --version
    npm --version
    ```

3. 如果两个命令都能成功运行，并且 Node.js 满足项目 README 和 `package.json` 中的要求，可以跳到下一节。

> [!TIP]
> 仅当尚未安装 Node，或需要更新版本时，才需要完成以下步骤。

4. 打开 [Node.js 下载页面][node-download]。
5. 下载适用于当前操作系统的 **LTS** 版本。
6. 运行安装程序并接受默认设置。在 Windows 上，保留选中的 **Add to PATH** 选项。
7. 安装完成后，打开新的终端窗口。
8. 在新终端窗口中运行以下命令，确认安装成功：

    ```bash
    node --version
    npm --version
    ```

9. 应显示已安装的 Node.js 和 npm 版本。

> [!IMPORTANT]
> Copilot app 的智能体也必须能够运行 `node --version` 和 `npm --version`。常用终端中运行成功，并不表示 app 也能访问这些命令。安装 Node.js 或更改 PATH 后，请完全退出并重新打开应用。如果会话中仍有命令无法运行，请先按照[会话环境故障排除步骤][session-environment]操作，再继续。

> [!IMPORTANT]
> 每个工作树还需要项目依赖项和用于 E2E 检查的 Playwright Chromium。准备工作树时，请遵循 Tailspin Toys 存储库的 README，并在批准前审查所有安装请求。

## 设置实验存储库

你将使用自己的 Tailspin Toys 项目副本。现在通过[模板存储库][template-repository]创建副本。新存储库包含实验所需的全部文件，安装应用后会将其连接到项目。

1. 在新的浏览器窗口中，转到本实验的 GitHub 存储库：`https://github.com/github-samples/tailspin-toys`。
2. 在实验存储库页面选择 **Use this template** 按钮，然后选择 **Create a new repository**，创建自己的存储库副本。

    ![Use this template 下拉菜单中选中了 Create a new repository](../../../_images/app-0-use-template.png)

3. 如果参加的是由 GitHub 或 Microsoft 主办的活动，请按照导师提供的说明操作。否则，可以在有权使用 GitHub Copilot 的组织中创建新存储库。

    ![Create a new repository 表单，已将 github-samples/tailspin-toys 设为模板并填写存储库名称](../../../_images/app-0-create-repository.png)

4. 记下所创建的存储库路径（**organization-or-user-name/repository-name**），后续实验会用到它。

> [!NOTE]
> 通过模板创建存储库时，系统会自动创建一组 GitHub 议题作为待办事项。整个研讨会都会围绕这些议题展开，无需自行创建。

使用工作坊模板创建全新的副本。它包含存储库指令、应用代码、测试、quality-checks 技能和现有画布扩展。你将在研讨会中自定义该技能并创建 QA 智能体。如果使用旧副本，请向讲师确认其中包含所需文件。

## 总结与后续步骤

准备工作已完成。本课中，你：

- 安装了 Node.js，以便在本机运行项目构建和测试；
- 通过模板创建了自己的 Tailspin Toys 存储库副本。

接下来，[安装 GitHub Copilot app][next-lesson]、连接刚创建的存储库并熟悉工作区。

## 资源

- [下载 Node.js][node-download]
- [通过模板创建存储库][template-repository]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#从议题启动会话
