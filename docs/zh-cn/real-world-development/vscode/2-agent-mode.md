---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏文件并再次修改，工作还会继续延伸。

这正是 Copilot 智能体模式的用武之地。

Copilot 智能体模式可以在 IDE 中自主工作。它会先探索现有项目结构，再进行所需更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加功能。

> [!NOTE]
> 名称相似，但智能体模式和云智能体提供的是两种不同体验。智能体模式在 IDE 中执行任务，可快速反馈并与用户交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现功能。
- 合并前如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表增长，你希望用户能够按类别和发行商筛选游戏。上一练习中已添加 publishers helper，现在使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站前，先了解 Tailspin Toys 的当前功能。

此网站是一个以开发者为主题的桌游众筹平台，用户可以浏览游戏列表和游戏详情。它是一个单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API，也没有客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，当前分支应为 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果系统提示打开 `http://localhost:4321` 浏览器窗口，请选择 **x** 关闭对话框。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，系统会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道，网站服务器现已在其中运行。

### 浏览网站

网站运行后，可以探索其功能：

- **主页**：显示桌游列表及其标题、图像和说明。
- **游戏详情页**：选择游戏后会进入详情页，其中包含标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；各步骤会在适用时说明。

网站的初始实现已可运行，但还需要添加功能。先查看待办事项列表。使用项目模板时，系统已自动创建议题；让 GitHub Copilot 列出这些项目：

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划和实现跨文件更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用了 Claude Sonnet 4.5，因此教程包含该模型，以便尽量保持体验一致。不过，LLM 具有概率性，你收到的建议可能与本教程不同。这完全正常。

> [!NOTE]
> 由于 LLM 的概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 审查生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度排列优先级。

## 查看说明文件

让智能体生成代码之前，先查看用于向 Copilot 提供工作上下文的说明文件。本练习将使用[用户界面 (UI) 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中记录了向网站添加功能时应遵循的方法。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概览。
   - 组件设计、可测试性和无障碍原则。
   - 指向特定文件类型说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用说明、拆分复杂内容，并提供代码示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项独立更新：

- 在数据层 (`src/lib/`) 添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

合并前还必须运行并通过测试。Copilot 智能体模式可以完成这些任务：

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍使用 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位相关文件。它应查找数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，可能需要几分钟。过程中编辑器可能显示 **Keep** 和 **Undo**；Copilot 完成后，所有更改都会显示这些选项，因此无需在工作进行时逐项选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 请求确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，也可能看到测试失败，这没有关系。Copilot 会反复生成代码并运行测试，直到完成任务且未发现错误。

   ![Copilot 智能体模式完整 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了新功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并前如何审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 奖励探索练习 — 实现分页

游戏列表不断增长后，需要添加分页。使用本练习所学知识，提示 Copilot 更新网站以实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件。
- 决定如何实现分页：允许用户选择每页大小，或将其固定为常量。
- 编写提示时提供所需指导，让 Copilot 按预期实现功能。
- 可能需要多次迭代，向 GitHub Copilot 请求修改并提供上下文。这是使用 Copilot 的常见工作方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
