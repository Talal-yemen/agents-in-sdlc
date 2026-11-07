---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两节练习中，你让 Copilot 云智能体处理了三个改进用户体验和添加功能的任务。云智能体虽然能够异步自主工作，但监控这些任务仍然很重要。

GitHub 提供了多种工具来管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。你可以在这个控制中心查看所有包含未关闭拉取请求的智能体任务，了解已执行的操作，还可以引导正在进行的会话。

本课将介绍如何：

- 使用智能体页面监控云智能体任务；
- 引导正在进行的会话，让智能体添加额外功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，加入浅色模式也很合理。现在需要引导智能体，让它在实现高对比度模式的同时也添加浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。这里应包含你分配给 Copilot 的任务，例如：
    - 更新代码文档；
    - 生成用于修改游戏的 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码；
    - 为 Copilot 创建运行环境；
    - 设置 MCP 服务器；
    - 执行完成任务所需的各种步骤。

> [!NOTE]
> 此处显示的具体步骤会根据 Copilot 的工作状态和处理方式而有所不同。

4. 同时留意右侧出现的拉取请求窗格，可以在此查看 PR 及其更改的文件，以便进一步监控。

## 引导云智能体

现在已经了解当前活动任务，可以要求 Copilot 在实现高对比度模式的同时添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![智能体控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成准备工作并开始处理任务。开始看到类似下方的消息时，就表示已进入处理阶段。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的 Copilot 云智能体任务，突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 观察 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会在完成你发送消息时正在执行的具体步骤后，将新请求纳入工作流程。

这同样需要几分钟，可以趁此机会回顾之前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，也就是监控和引导 GitHub Copilot 云智能体任务的中心。你可以用这个控制中心跟踪所有正在进行和已完成的任务、查看工作内容，并重定向正在运行的任务来调整范围或补充指导。

你了解了以下概念：

- 使用控制中心和智能体页面监控云智能体任务；
- 引导正在进行的会话，使其添加额外功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
