---
title: "第 8 课 - 创建并合并功能 PR"
description: "审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都已保存在同一个分支中。现在需要一起审查这些内容并创建拉取请求。你已经手动合并了星级评分拉取请求 (PR)，这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化本研讨会的流程，你将所有筛选和质量工作都保留在同一个对话和分支中，并通过这个 PR 一起提交。

本课将介绍如何：

- 了解 Agent Merge 及其如何自动化 PR 合并生命周期；
- 检查完整的功能更改和验证证据；
- 创建筛选功能 PR；
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余 PR 工作，同时让开发人员掌握合并授权权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用该功能后，Copilot 会处理阻塞 PR 的问题，包括修复失败的持续集成 (CI) 检查、回复审查评论，以及在需要时变基；然后在存储库允许的情况下启用 GitHub 自动合并。

之前由你手动选择 **Merge pull request**。Agent Merge 可以代为处理这项工作。启用前，先审查并确认更改已准备好合并。

## 使用 Agent Merge 管理 PR

所有代码都已完成，现在一起审查并创建 PR，然后让 Agent Merge 处理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。一切成功后，它会执行合并。
7. 确认看到 Copilot 显示类似于“PR #14 was squash-merged successfully”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限要求。如果操作受阻，请阅读报告的原因，并在存储库允许时手动完成审查后的合并。

## 总结与后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动化 PR 合并生命周期；
- 检查了完整的功能更改和验证证据；
- 创建了筛选功能 PR；
- 只在审查后启用 Agent Merge，并确认 PR 已合并。

接下来，[探索更多有用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型和共享，并了解可选的云端委派功能。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
