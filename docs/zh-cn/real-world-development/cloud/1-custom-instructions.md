---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或者 Copilot 应该了解背景信息，就要确保这些上下文随时可用。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

本练习将介绍如何：

- 通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目上下文、编码准则和文档标准；
- 在 `.github/copilot-instructions.md` 中添加一项新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会比较提示的前后效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，你会看到这些指令发挥的作用。

## 指令文件

### 场景

Tailspin Toys 和优秀的开发团队一样，制定了一套开发实践准则和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 每个文件顶部都应添加一段注释，说明该文件的作用。

使用指令文件可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和需求。你可以指定偏好的编码约定、库，以及希望加入代码的注释类型。可以为整个存储库创建指令，也可以针对特定文件类型创建指令，以提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次向 Copilot 发送存储库请求时，都会附带这个单一的指令文件。它应包含项目级信息，也就是大多数聊天或 CLI 请求都会用到的上下文，例如技术栈、项目概况、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可以针对特定任务或文件类型创建这些文件。例如，为 TypeScript 或 Astro 等语言提供指导，或者针对创建 UI 组件或一组单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中工作时，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的做法。概括来说：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目概况、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码应该和不应该如何编写的示例。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过实践，你会找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。查看本项目中的指令文件时，可能会发现其中包含多种任务类型，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同界面的操作方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。当前学习路径会在相关步骤中说明。
>
> 想找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中汇集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 和一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 中嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于指定指令适用的文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 查看其中针对本项目单元测试的专门指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可以将大型指令集拆分成更小、可复用的文件，并提供可供 Copilot 参考的示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但目前尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

接下来只需在 `.github/copilot-instructions.md` 中添加一项项目级规则，要求代码使用 TSDoc 文档注释和文件级注释头。稍后分配议题时，云智能体会采用这项规则。

开始编辑前，先创建一个分支（云智能体会从议题目标分支读取指令，但仍建议单独提交本次更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应在第 27 行附近）。它记录了项目的编码标准，但还没有关于代码文档的规则，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。像文档注释规则这样的项目级标准适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习会将议题分配给 Copilot，而它会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并完成合并。下一练习中，云智能体就能读取这些指令。

> [!TIP]
> 如果希望在本次研讨会中直接使用 `main`，可以跳过分支操作并直接提交到 `main`。此处保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你了解了 Copilot 如何使用本项目的指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的全库标准。后续练习中，你可以在云智能体创建的拉取请求中看到该标准发挥作用。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
