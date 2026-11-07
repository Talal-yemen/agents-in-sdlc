---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。任务完成后，它会创建一个草稿 PR，供开发人员审查。这样可以把繁琐任务交给智能体，加快开发进度，让开发人员专注于更重要的工作。

本练习将介绍如何使用云智能体：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初为网站开发的承包商没有留下完善的文档，事实上，代码几乎完全没有说明。团队首先希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每个游戏详情页都显示相关游戏，也就是同一类别中的其他作品，让用户可以继续浏览。他们暂时不需要完善的实现，只需要能用于用户体验验收测试的版本。这项工作目前不是优先事项，其他议题优先级更高。

这类任务很容易被不断推迟，适合交给 Copilot 云智能体处理。云智能体可以在后台异步完成任务，让开发人员继续处理其他工作，之后再回来审查其成果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发人员一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度，准备环境并开始处理。你可以在会话进行中或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 编写范围明确的指令

GitHub Copilot 并非魔法——使用它并不意味着可以跳过思考。即使看似简单的操作，深入分析后也会发现复杂性，因此请留意[如何界定云智能体任务范围][cloud-agent-best-practices]。将它视为 AI 结对编程伙伴：分阶段工作、学习、尝试并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已经完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的效果；如果还没完成，现在可以快速查看。将任务分配给 Copilot 云智能体前，先只读查看存储库中已有的指令文件，稍后便能观察它们的影响。

在 GitHub Web UI 中打开以下文件；如果已启动 codespace，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意它创建的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自指令文件，稍后的练习还会再次审查。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行设置脚本，以确保环境正常。将任务分配给 Copilot 时也是如此：它会像软件工程师一样完成任务。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。你可以通过创建专用设置工作流来自定义环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 中，并会在智能体开始工作前运行。这样可以为它准备所需的开发工具和依赖项。实验已预先配置此工作流，以确保流程顺畅。它会安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 用于在存储库的 Actions 选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 只授予步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 例如，如果设置步骤需要克隆存储库来安装依赖项，则需要 contents: read 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流相同，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，这样无需等待 Copilot 启动工作流，就能先验证其是否运行成功。

## 添加文档

虽然人人都知道文档的重要性，但大多数项目中的文档要么过时，要么根本不存在。这类技术债务经常无人处理，会拖慢开发进度，让代码库更难维护，也更难让新开发人员融入团队。Copilot 擅长编写文档，因此很适合将这类议题分配给云智能体。它会在后台生成所需文档，之后你可以在后续练习中审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新建的拉取请求 (PR)，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表，因为它需要准备环境（运行前面介绍的工作流）、分析项目并确定解决问题的方法。

13. 查看待办事项列表及其计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并制定计划，以及编写代码。

这可能需要几分钟。云智能体的主要目标之一是异步执行任务，让我们能处理其他工作。接下来会利用这一点：先为云智能体分配另一个任务，然后转而为应用编写新功能。

## 在游戏详情页中添加相关游戏版块

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，它则处理另一组。为设计团队添加相关游戏版块也许不需要太久，但这段时间仍可用于其他工作。现在就将它分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论旁会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！云智能体的工作方式类似软件工程师，无需持续监控，完成后再审查即可。现在可以转而创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、创建新功能，或协助将代码迁移到其他框架。

你了解了以下概念：

- 自定义代码生成环境；
- 确保操作安全；
- 明确界定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台工作，接下来可以创建并使用自定义智能体。[云智能体也支持 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
