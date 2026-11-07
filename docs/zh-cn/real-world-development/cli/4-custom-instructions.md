---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就要让 Copilot 能够获取相关指导。[指令文件][instruction-files]不仅说明想要什么代码，也说明代码应该如何组织。筛选功能已经构建完成，现在来了解 Copilot 使用的指令、添加文档标准，并将其应用于代码。

本课将介绍如何：

- 了解存储库指令和按路径限定范围的指令文件如何传递给智能体；
- 更新指令文件，确保遵循编码标准；
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 和优秀的开发团队一样，制定了一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码；
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（如有 `db` 参数，也要说明该参数可注入）；
- 可复用的 Astro 组件应说明其 `Props` 接口契约，相关代码更改时也应更新注释；
- 保留现有格式和 lint 指南。

使用指令文件可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和需求。你可以指定偏好的编码约定、库，以及希望加入代码的注释类型。可以为整个存储库创建指令，也可以针对特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次向 Copilot 发送请求时都会附带的单一指令文件。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的指令文件。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。使用特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

起始项目中已包含一组指令文件。先查看已有内容，再进行更改，以便了解更改的效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。此文件中的指令适用于在此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包含针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段；它使用 glob 指定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它链接的其他指令文件和项目现有文件。这样可以将大型指令集拆分成更小、可复用的文件，并提供可供 Copilot 参考的示例。

## 更新指令文件以符合团队指导

现有文件是不错的起点，但仍缺少一些内容。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，应该在第 35 行附近。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选会话。这样 Copilot 会加载新指导，同时保留功能上下文。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并检查更改的 TypeScript 文件。查看新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你了解了 Copilot 如何从指令文件中获取上下文，并将新的标准应用于功能代码。具体来说，你：

- 了解了存储库指令和按路径限定范围的指令文件如何传递给智能体；
- 更新了指令文件，确保遵循编码标准；
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保始终运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot——指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
