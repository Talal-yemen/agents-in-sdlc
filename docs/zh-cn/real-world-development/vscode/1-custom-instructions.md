---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解背景信息，就要确保这些上下文随时可用。有多种方法可以向 Copilot 提供特定上下文，其中一种关键方式是使用[指令文件][instruction-files]，指导 Copilot 如何生成代码。

本练习将介绍如何：

- 通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目上下文、编码准则和文档标准；
- 在当前指令生效时生成筛选功能的第一块数据切片（publishers helper）；
- 在 `.github/copilot-instructions.md` 中添加一项新的全库标准；
- 再次运行相同提示，观察生成的代码如何采用新标准；
- 将指令更新和筛选代码切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不符合所有既定标准。Copilot 具有非确定性。本练习的目标是观察更新指令后行为变化的*趋势*，而不是要求输出逐字符完全一致。

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

- `.github/copilot-instructions.md`：每次向 Copilot 发送请求时都会附带的单一指令文件。它应包含适用于大多数请求的项目级信息，例如使用的技术栈、项目概况、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可以针对特定任务或文件类型创建这些文件。例如，为 TypeScript 或 Astro 等语言提供指导，或者针对创建 UI 组件或一组单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中工作时，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的做法。概括来说：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目概况、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
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
2. 浏览该文件，注意其中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 中嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览。其中包含针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于指定指令适用的文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 查看其中针对本项目单元测试的专门指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可以将大型指令集拆分成更小、可复用的文件，并提供可供 Copilot 参考的示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但目前尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一练习使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用模式和模型可能不同，相关步骤会具体说明。

为了观察自定义指令的影响，先在当前指令生效的情况下发送提示。稍后更新指令文件后，再次发送相同提示并比较结果。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪个位置。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常还会修改 helper 文件及其测试。
6. 注意生成的 helper 是一个带类型的函数，第一个参数接收 `db` 客户端，并返回带类型的 publishers 数组。这些约定来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性——即使没有明确要求，也可能添加文档注释。这种情况没有问题；本练习的重点是观察更新指令后行为*一致性*的提升。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应在第 27 行附近）。它记录了项目的编码标准，但尚未包含代码文档规则，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。像文档注释规则这样的项目级标准适合放在存储库级文件中。

## 重新运行提示并观察更改

既然指令中已加入文档注释标准，现在让 Copilot CLI 更新刚生成的 publishers 文件。同一条标准指令会引导这次更新。

1. 在 Copilot CLI 会话中发送 `/clear`，开始全新对话。
2. 发送以下提示：

   ```plaintext
   Update src/lib/publishers.ts to follow the latest documentation conventions in .github/copilot-instructions.md.
   ```

3. 等待编辑完成，然后重新打开 `src/lib/publishers.ts`。
4. 注意文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Tailspin Toys Crowd Funding platform 的 publisher 数据访问辅助函数。
    * 提供从数据库检索 publisher 信息的函数。
    */
   ```

5. 注意生成的函数现在会包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有 publisher 的列表，包含其 id 和 name。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为 publisher 对象数组。
    */
   ```

6. 保留此更新后的文件。下一节练习会继续扩展这一块数据切片。

## 提交指令和推送分支

指令文件与其他项目文件一样，通过源代码管理进行维护。接下来提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和 helper 更改已经存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新以及 Copilot 为 helper 初始实现修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他辅助更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在应创建 PR 并关联待办事项中的文档标准议题。也可以手动操作，不过 Copilot 可以使用已连接项目的 GitHub 工具帮你完成。让它查找议题、创建并关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已经配置好 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成授权。后续 MCP 练习会介绍这种连接方式。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中已选中 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和新增的 publishers helper：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 当 Copilot 提示允许它代表你执行 GitHub 操作时，先审查操作，再按需选择 **Allow**。
6. PR 创建后，要求 Copilot 将其合并并把当前分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示允许它执行 GitHub 操作和 shell 命令，请先审查操作，再按需选择 **Allow**。

现在已经在 GitHub Copilot 的帮助下创建并合并了拉取请求！

## 总结与后续步骤

你了解了 Copilot 如何使用本项目的指令文件获取上下文，并在 VS Code 的 Copilot Chat 中完成以下工作：

- 发送代码生成提示，并观察现有指令下 Copilot 的输出；
- 在 `.github/copilot-instructions.md` 中添加新的全库标准；
- 再次发送相同提示，观察生成代码如何采用新标准；
- 将指令更新和 helper 提交到 `main`，供下一练习使用。

下一步，在[代理模式][next-lesson]中应用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot——指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
