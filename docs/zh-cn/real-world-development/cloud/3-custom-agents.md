---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域创建的专用 AI 助手。你可以在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义智能体，并为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如执行 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含至少带有 `name` 和 `description` 的 YAML frontmatter，后接定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。可以这样区分：**自定义智能体负责工作，技能则是工具。**

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）来完成任务。在本实验中，无障碍自定义智能体会根据无障碍指南审查和更新网站；执行过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。和 AI 的其他用法一样，需要测试并不断迭代，找到适合当前环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将介绍如何：

- 定义自定义智能体；
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的限制。近期用户反馈指出，由于文字和背景颜色对比度不足，有些用户难以阅读当前的深色主题。为解决这个无障碍问题，设计团队要求实现可供用户开启或关闭的高对比度模式。

无障碍至关重要，因此希望尽快完成这项功能。你将使用自定义智能体生成所需功能。

## 查看无障碍自定义智能体

实验已为你创建了一个无障碍自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后查看无障碍智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接着浏览后续部分，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误。

## 创建并分配议题

智能体页面是集中管理环境中所有智能体的位置。你可以在这里将任务分配给 Copilot 云智能体、监控任务，甚至引导会话。现在先将实现高对比度模式的任务交给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![Copilot 云智能体分配界面，其中突出显示了自定义智能体和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 就会在后台开始处理任务。

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域设计的专用 AI 助手。你可以借助自定义智能体，将团队的专业知识和标准整理成可复用的指引，让 Copilot 更有效地完成特定类型的工作。

你了解了以下概念：

- 定义自定义智能体；
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式，接下来可以从智能体控制中心[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
