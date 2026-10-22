---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要采用特定方式完成，或 Copilot 应了解某些背景信息，就要确保它能获取这些上下文。[说明文件][instruction-files]用于提供相关指导，让 Copilot 不仅了解你想让它做*什么*，也了解*如何*完成。

本练习将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习路径不同，这里不会运行更新说明前后的提示进行对比。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。后续审查它创建的拉取请求时，可以看到说明文件的影响。

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

- `.github/copilot-instructions.md`：针对存储库的**每个**请求发送的单一说明文件。此文件应包含项目级信息，即大多数聊天或 CLI 请求都会用到的上下文，例如技术栈、项目概览、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，用于指导特定语言（例如 TypeScript 或 Astro），也可用于创建 UI 组件或添加单元测试等任务。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持采用其他标准的说明文件，例如 [AGENTS.md 和 CLAUDE.md][custom-instructions-support]。

### 管理说明文件的最佳实践

本研讨会无法全面讨论如何创建说明文件，但示例项目展示了有代表性的做法：

- 将 `copilot-instructions.md` 中的内容聚焦于项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体指导。
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
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用来确定适用文件；例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会符合条件。
5. 查看其中针对本项目单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看底部。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可将较大的说明拆分为较小、可复用的文件，并为 Copilot 提供代码示例。路径相对于说明文件本身，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

本练习中唯一需要编辑的内容是：添加一条项目级规则，要求通过 TSDoc 文档注释和文件级注释头在代码中记录文档。后续练习中，云智能体处理分配的议题时会读取这条规则。

编辑前创建工作分支（云智能体会读取议题目标分支中的说明，但仍建议提交自己的更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

存储库编码标准应有助于提升代码建议质量：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。它记录了项目编码标准，但尚未涉及代码文档，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以用 `*.instructions.md` 文件指导特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，因此应放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，云智能体处理议题时就会读取这些说明。

> [!TIP]
> 如果想直接在 `main` 上工作，也可以跳过分支步骤并直接提交。本练习使用分支是为了模拟真实项目中的做法。

## 总结和后续步骤

你已了解 Copilot 如何从说明文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了存储库级标准。后续练习中，可以在云智能体创建的拉取请求里看到这条标准的影响。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
