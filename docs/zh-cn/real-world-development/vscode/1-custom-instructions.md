---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要采用特定方式完成，或 Copilot 应了解某些背景信息，就要确保它能获取这些上下文。你可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种是[说明文件][instruction-files]，它们用于指导代码生成。

本练习将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 在当前说明生效时生成筛选功能的第一块数据切片（publishers helper）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能不完全符合你设置的标准。Copilot 具有概率性而非确定性。本练习的重点是观察更新说明后行为变化的*趋势*，而不是让输出逐字符匹配。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 代码应使用 TSDoc 文档注释。
- 每个文件开头都应有注释块，说明文件用途。

使用说明文件可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。你可以指定编码约定、库，以及希望添加的注释类型；也可以为整个存储库创建说明，或针对特定文件类型提供任务级上下文。

说明文件有两类：

- `.github/copilot-instructions.md`：针对存储库的**每个**请求发送的单一说明文件，应包含项目级信息，例如技术栈、项目概览、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如指导 TypeScript 或 Astro，也可用于创建 UI 组件或新增单元测试等任务。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持采用其他标准的说明文件，例如 [AGENTS.md 和 CLAUDE.md][custom-instructions-support]。

### 管理说明文件的最佳实践

本练习无法全面讨论如何创建说明文件，但示例项目展示了有代表性的做法：

- 将 `copilot-instructions.md` 中的内容聚焦于项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供说明。
- 使用自然语言，保持指导清晰，并提供代码应该和不应该如何编写的示例。

创建说明文件没有唯一正确的方法，就像使用 AI 也没有唯一正确的方法。通过实践和迭代，可以找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的说明文件。浏览本项目文件时，会发现其中包含许多任务类型的指导，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面提供此功能的方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前练习路径会在适用时介绍。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中汇集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义说明文件

花些时间阅读存储库附带的说明文件，其中包括一个核心 `copilot-instructions.md` 和针对不同任务的一组 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹，查看其中为 Astro 文件、Drizzle 数据层、测试等编写的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用来确定说明适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会符合条件。
5. 查看其中针对本项目单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看底部。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目文件的链接。这可以将较大的说明拆分为更小、可复用的文件，并为 Copilot 提供代码示例。路径相对于说明文件本身，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支：

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明*之前*使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；各步骤会在适用时说明。

为了观察自定义说明的影响，先在当前说明生效时发送提示。稍后更新说明后，再次发送相同提示以比较结果。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会修改 helper 文件和相关测试。
6. 留意建议的 helper 是类型化函数，接收 `db` 客户端作为第一个参数，并返回类型化的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，它也可能添加文档注释。若出现这种情况也没关系；本练习仍要观察更新说明后*一致性*的提升。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，以改善代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。注意其中记录了项目编码标准，但尚未包含代码文档规则，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以用 `*.instructions.md` 文件指导特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话上下文。
2. 重新打开 `src/lib/publishers.ts`，让 Copilot 关注正确文件。
3. 再次发送相同提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于下一练习要继续扩展的实际代码。

## 提交说明并推送分支

说明文件和其他资源一样，由版本控制管理。将说明和 helper 更改提交并推送到存储库：

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新终端。
2. 在终端中运行以下命令，确认说明和 helper 更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新以及 Copilot 为 helper 基础所更改的文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他支持性更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在可以创建拉取请求，并关联待办列表中的文档标准议题。也可以让 Copilot 使用已连接的 GitHub 工具查找议题、创建关闭该议题的 PR，然后合并。

> [!NOTE]
> 这是本练习中首次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub；按照提示授权即可。后续课程会介绍此连接使用的 Model Context Protocol。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 要求 Copilot 查找更新说明文件的议题，并从当前分支创建 PR，说明其中包含说明更新和新的 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果 Copilot 请求 **Allow** 以代表你执行 GitHub 操作，请审查命令并酌情授权。
6. 创建 PR 后，要求 Copilot 合并 PR 并在本地切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 请求 **Allow** 以执行 GitHub 操作或 shell 命令，请审查命令并酌情授权。

现在，你已在 GitHub Copilot 的帮助下创建并合并了拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何从说明文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成以下操作：

- 发送代码生成提示，观察现有说明下 Copilot 的输出。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 再次发送相同提示，观察建议的代码如何遵循新标准。
- 将说明更新和 helper 基础提交到 `main`，供下一练习继续使用。

接下来，Copilot 将在[智能体模式][next-lesson]中跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
