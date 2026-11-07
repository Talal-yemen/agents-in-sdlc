---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也要修改多个文件并执行测试等操作。开发人员的工作流程往往包括查找所有相关文件、进行更改、运行测试、调试、发现遗漏的文件并再次修改……还有许多类似步骤。

Copilot 代理模式正是为此设计的。

Copilot 代理模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。现在来看看如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但适用于不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 让 Copilot 代理模式探索项目、查找相关文件并协同修改；
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 在将代码合并到代码库之前，审查代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行方筛选。上一练习已添加 publishers helper，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站前，先了解 Tailspin Toys 当前的功能。

该网站是一个开发者主题棋盘游戏的众筹平台。它可以列出游戏并显示游戏详情。整个网站是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能通过少量使用标准 DOM API 的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。可以按以下步骤在 GitHub Codespace 中运行：

1. 返回上一练习使用的 Codespace。此时应已回到 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示框询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从终端选择 localhost URL 会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前运行 Web 服务器的 codespace 的私有隧道。

### 浏览网站

网站运行后，可以浏览现有功能：

- **主页**：显示棋盘游戏列表，包括标题、图像和说明。
- **游戏详情页**：选择游戏后，会打开详情页，显示标题、说明、发行方和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用模式和模型可能不同，相关步骤会具体说明。

网站的初始实现已可正常使用，但还可以添加新功能。先查看待办事项。通过模板设置项目时，系统已自动创建待办议题。让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未推荐任何特定模型。编写研讨会时使用了 Claude Sonnet 4.5，因此将它列在说明中。这样做是希望代码建议相对一致，带来更顺畅的体验。不过，由于 LLM 具有概率性，你看到的建议可能与研讨会示例不同，这是正常且符合预期的。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 显示议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 甚至根据它认为对用户最有用的程度，为议题排定了优先顺序。

## 查看指令文件

让智能体生成代码前，先查看将用于向 Copilot 提供上下文的指令文件。本练习将使用 [用户界面 (UI)][ui-instructions] 文件，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，了解添加功能的方式，包括：
   - 应用架构概览；
   - 组件设计、可测试性和无障碍原则；
   - Astro、Tailwind CSS 和 Drizzle 等特定文件类型的指令链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用内容、拆分复杂指令，并为 Copilot 提供参考示例。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项更改：

- 在数据层 (`src/lib/`) 中添加或完善筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，加入筛选 UI。

此外，测试必须在合并到代码库前运行并通过。Copilot 代理模式可以完成这些任务，现在来添加该功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍然选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍然选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并查找与功能有关的文件。它应该会找到数据层 helper、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库大小，这可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo**，因此无需在执行过程中选择它们。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 窗格中的对话框，询问是否允许运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 可能会暂停一段时间，或在过程中遇到测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未发现错误。

   ![已完成的 Copilot 代理模式会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，体验新功能。
11. 确认功能正常且已审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习中，你使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加了新功能。你了解到如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 让代理模式探索项目、查找相关文件并协同修改；
- 在将代码合并到代码库之前，审查代理模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习——实现分页

随着游戏列表增长，网站需要分页。使用本练习学到的技能，提示 Copilot 为网站添加分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有指令文件；
- 考虑分页方式：允许用户选择页面大小，还是使用固定页面大小；
- 编写提示时提供足够指导，让 Copilot 按照预期实现；
- 可能需要与 GitHub Copilot 多次交互，请它修改并提供上下文。这是使用 Copilot 的正常工作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
