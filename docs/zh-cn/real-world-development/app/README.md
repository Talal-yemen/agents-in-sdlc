---
slug: zh-cn/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** 是一款基于 Copilot CLI 构建的桌面应用，可将智能体驱动的开发集中到统一且专注的工作区。它支持并行智能体会话、可切换的会话模式、共享画布，以及原生 GitHub 议题和拉取请求管理功能。其中包括 **Agent Merge**，可引导拉取请求完成变基、处理审查反馈、修复持续集成 (CI) 问题并完成合并。

本研讨会使用一套连续的 Tailspin Toys 工作流：

1. 准备项目、安装应用、连接存储库，并熟悉工作区和模板预先创建的待办事项。
2. 添加星级评分，在浏览器中审查更改，并手动合并第一个拉取请求 (PR)。
3. 从筛选议题开始，在 **Plan** 模式中确定方案，在 **Autopilot** 模式中构建，然后在 **Interactive** 模式中审查。
4. 更新存储库指令，并将其应用于筛选功能。
5. 自定义现有的 `quality-checks` 技能并使用它运行项目检查。
6. 添加 Playwright 模型上下文协议 (MCP) 服务器，并使用它在浏览器中体验筛选功能。
7. 创建质量保证 (QA) 自定义智能体，审查需求、覆盖范围和验证证据。
8. 审查完整的筛选功能更改，并对第二个 PR 使用 Agent Merge。
9. 检查画布功能是否可用，然后使用 Database Explorer，并创建和测试由存储库支持的分类画布。如果必要功能不可用，请记录受阻原因，使用只读替代方案，或继续学习总结内容。

为了使研讨会重点明确，你将创建两个 PR：先提交星级评分，再提交筛选功能及指令更新、技能更新、QA 配置文件和测试。每个 PR 都从更新后的 `main` 分支开始。筛选和质量工作流共用一个会话、工作树和分支，让你可以在探索各项工具时基于已有成果继续构建。最后的画布练习保留在其会话中，让你专注于创建和测试共享界面，而无需重复 PR 工作流。

## 课程

| 课程 | 主题 | 说明 |
|--------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 安装 Node.js，并创建自己的 Tailspin Toys 项目副本 |
| [1. 安装 Copilot app][ex1] | 设置 | 安装应用、连接项目并熟悉工作区 |
| [2. 添加星级评分：快速上手][ex2] | 首次更改 | 显示现有评分和空值回退，再合并 PR 1 |
| [3. 智能体模式：Plan 和 Autopilot][ex3] | 智能体模式 | 根据议题规划功能，使用 Autopilot 构建，再通过 Interactive 模式审查 |
| [4. 使用自定义指令引导 Copilot][ex4] | 上下文 | 查看并更新指令，然后将其应用于筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 查看现有技能、更改报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 通过 Customize 配置 MCP 并检查筛选行为 |
| [7. 创建并使用 QA 智能体][ex7] | 需求与覆盖范围 | 创建并选择专用配置文件，收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查与合并 | 审查筛选、指令、技能、QA 配置文件和测试，然后对第二个 PR 使用 Agent Merge |
| [9. 探索并创建画布][ex9] | 协作 | 使用 Database Explorer，再创建并测试由存储库支持的分类画布 |
| [10. 总结与后续步骤][ex10] | 总结 | 回顾工作流、产出和更多资源 |

## 先决条件

参加本研讨会前，请确保具备：

- [ ] 拥有有效 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 一台运行 **macOS、Linux 或 Windows** 的计算机
- [ ] 已在计算机上[安装 Git][install-git]

> [!TIP]
> 没有付费计划？通过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会使用的智能体、MCP、代码审查和 Copilot CLI 功能，可用于完成所有学习路径。

> [!NOTE]
> Copilot app 在本地计算机而非 codespace 中运行，因此[先决条件练习][ex0]会指导你安装 Node.js 并创建项目副本，然后再安装应用。

> [!NOTE]
> 如果使用 Copilot Business 或 Copilot Enterprise，管理员必须启用 **GitHub Copilot app** 策略。此策略与 Copilot CLI 策略相互独立。

## 开始

**[从先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
