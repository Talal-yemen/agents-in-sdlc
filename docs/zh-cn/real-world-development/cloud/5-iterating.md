---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 合作完成了多项改进用户体验和增加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在来检查代码更改，并在需要时向 Copilot 提供反馈，进一步完善其工作。

### 场景

如前面多次强调的，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循日常 DevOps 流程。因此，在为团队启用代码审查前，先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能所做的更改。

## 安全与 GitHub Copilot 云智能体

Copilot 云智能体会异步且自主地执行任务，因此系统设置了一些安全限制，以确保操作安全：

- Copilot 只能读取存储库，并且只能写入它用于修改代码的分支。
- 云智能体在 GitHub Actions 中运行，会创建独立的临时环境来工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先通过 GitHub.com 上的标准 PR 界面，查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你也可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 查看拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会显示 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟，可以休息一下或回顾之前学到的内容。

4. PR 准备好后，选择 **Files changed** 选项卡并查看更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中应包含 TSDoc 文档注释和其他文档。具体更改可能有所不同。

   浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 检查完成且确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应出现在拉取请求检查列表中。正常情况下，单体 Astro 应用的项目检查会通过。这可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 合作并非单向过程。你可以像 @提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会看到评论并启动新会话来处理请求。由于结果具有非确定性，无法给出固定提示文本。你可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论并提及 **@copilot**，要求它更新生成的文档。可以使用上面的任一示例，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，观察 Copilot 执行任务。注意 Copilot 会启动新会话来完成更新。
3. 选择 **Back to pull request** 可返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

如果还未满意，可以继续迭代。确认满意后，可以将 PR 从草稿转换为准备审查状态，然后合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查完成且确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应出现在拉取请求检查列表中。正常情况下，单体 Astro 应用的项目检查会通过。这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建好相关游戏功能！和之前一样，可以通过请求更新来迭代改进云智能体的工作。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配该议题时尚未更新自定义指令）。如前所述，可以在 **Conversation** 选项卡中添加评论，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中要求的高对比度模式，以及练习 4 中在智能体控制中心追加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会显示 **[WIP]** 标记。请等待它完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的 UI 组件；
   - 使用本地存储保留用户偏好的方式；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 检查完成且确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应出现在拉取请求检查列表中。正常情况下，单体 Astro 应用的项目检查会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能正常且刷新页面后偏好仍然保留。

留意无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像处理前面几个 PR 一样，在评论中提及 **@copilot** 请求更新。

## 可选练习——继续委派任务

云智能体适合处理真实的积压任务，让你可以转而处理其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，练习这一工作方式。例如：

- 在游戏详情页创建支持者兴趣表单；
- 为游戏列表页添加分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。在这些课程中，你：

- **检查了存储库附带的自定义指令文件**，以便稍后观察它们对云智能体输出的影响；
- **将议题分配给 Copilot 云智能体**，观察它准备环境、规划任务并异步执行；
- **创建并使用自定义智能体**，为无障碍功能添加高对比度和浅色模式切换；
- **使用 Copilot 智能体页面作为控制中心**，监控并引导无障碍功能会话；
- **审查并迭代改进云智能体创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

云智能体学习路径已完成。其他学习路径可以帮助你继续探索：

- 🖥️ **[VS Code 学习路径](../../vscode/)**——在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)**——在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体连接。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
