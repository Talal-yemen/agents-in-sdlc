---
title: "练习 6 - 迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 上一课：监控和管理智能体][previous-lesson] |
| :-- |

## 审查工作成果

在本实验中，你与 GitHub Copilot 合作完成了多项改进用户体验的任务。你使用代理模式添加了跨客户端和服务器的筛选功能，通过 Playwright MCP 服务器在浏览器中手动测试，然后使用自定义智能体实现高对比度和浅色模式切换，并在会话进行期间引导智能体扩展工作。现在需要发布本地更改，并像团队一样审查这些更改。

### 场景

加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。Copilot 生成的内容仍然需要经过真实审查。现在从 codespace 推送无障碍功能，创建拉取请求，并在邀请团队其他成员参与前检查差异。

## 发布无障碍功能

你在[练习 4][exercise-4]和[练习 5][exercise-5]中使用无障碍自定义智能体实现的高对比度和浅色模式切换，现已作为提交保存在 codespace 中。将它们推送到分支并创建拉取请求，让团队审查。

1. 返回 codespace。
2. 在 VS Code 中打开 **Source Control** 视图。
3. 确认无障碍功能更改已提交。如果练习 5 后仍有未提交的更改，请使用描述性消息（例如 `Add high-contrast and light-mode toggles`）暂存并提交。
4. 选择 **Publish Branch** 发布分支（也可以使用 **...** 菜单 → **Push**）。
5. VS Code 会提示你在 github.com 上打开新分支。接受提示，或者手动打开存储库并在分支横幅中选择 **Compare & pull request**。
6. 设置清晰的标题（例如 `Add high-contrast and light-mode toggles`），并用简短说明概括所做更改及其原因。
7. 选择 **Create pull request**。
8. PR 创建后，选择 **Files changed** 选项卡，全面审查工作。重点查看：
   - 用于切换模式的 UI 组件；
   - 使用本地存储保留用户偏好的方式；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

9. 返回 **Conversation** 选项卡。
10. 如果有待批准的工作流，选择 **Approve and run workflows**。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

11. 等待工作流完成。正常情况下，工作流应会通过。

> [!TIP]
> 想听听其他人对无障碍功能的看法？在 PR 评论中提及 `@copilot`，并提出“检查此 PR 是否还有其他 WCAG 问题”或“建议改进键盘导航”等请求。Copilot 会启动新会话处理评论。

## 可选练习——继续在本地探索

在 IDE 中与智能体迭代是一项需要反复练习的技能。可以在 VS Code 中启动以下后续会话：

- 在游戏详情页添加支持者兴趣表单；
- 为游戏列表页实现分页；
- 为 `src/lib/` 中的数据访问 helper 添加输入验证和错误处理；
- 扩展无障碍智能体的工作范围，例如审查整个网站的键盘焦点顺序。

## 总结

恭喜！你已完成 VS Code 学习路径。本实验中，你：

- **使用 Playwright MCP 手动测试功能。**你添加了 Playwright MCP 服务器，并让 Copilot 控制浏览器，在创建拉取请求前验证筛选功能。
- **使用代理模式协调跨技术栈的更改。**在同一会话中添加了涉及客户端、服务器和测试的筛选功能。
- **使用了自定义智能体。**从智能体选择器中选择无障碍自定义智能体，并观察它如何根据存储库要求实现高对比度模式。
- **管理并引导了智能体会话。**在聊天中审查建议的更改，接受需要的内容，并通过浅色模式后续请求扩展会话。
- **通过拉取请求完成工作流程。**发布本地更改，并像团队一样全面审查。

## 回顾与后续步骤

必需的 VS Code 学习路径到此结束，可以在此完成研讨会。

如果想进一步了解 Copilot 的智能体功能，其他学习路径也会通过不同界面探索相关场景：

- 💻 **[CLI 学习路径](../../cli/)**——从终端交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、技能、自定义 QA 智能体、Playwright MCP、Agent Merge 和上下文交接。
- ☁️ **[云智能体学习路径](../../cloud/)**——将议题分配给云智能体、通过智能体页面监控会话，并异步迭代改进拉取请求。

也可以继续扩展刚才的工作。[awesome-copilot][awesome-copilot] 汇集了可按需调整的指令文件、自定义智能体和技能。

若要进行可选扩展，[可选：集成 Foundry][exercise-7] 会使用 VS Code 和 Microsoft Foundry Toolkit 准备模型、部署 Backer Concierge 并将其连接到网站。

## 资源

- [GitHub Copilot][github-copilot]
- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：监控和管理智能体][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
