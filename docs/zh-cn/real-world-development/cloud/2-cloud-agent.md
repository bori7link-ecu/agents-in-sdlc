---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

很少有组织完全不受技术债务困扰。技术债务可能包括尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期积压的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成任务后，它会创建草稿 PR，供开发人员审查。这样可以委派繁琐的工作、加快开发进度，让开发人员专注于更重要的整体工作。

本课将探索以下内容：

- 自定义代码生成环境。
- 确保操作安全执行。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些待处理的技术债务。最初承包网站开发工作的人员没有妥善编写文档，代码几乎完全缺少文档。团队希望先为应用中所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们希望在每款游戏的详情页显示同一类别中的其他游戏，方便访客继续浏览。当前只需要可用于 UX 验收测试的初步实现，无需精细打磨。此需求目前受阻，但还有优先级更高的议题。

这类工作很容易被延后，因此适合交给 Copilot 云智能体异步处理。开发人员可以继续处理其他任务，之后再回来审查 Copilot 的工作。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。你可以通过多种方式分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在会话进行时或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中通知你。

## 明确界定任务的重要性

GitHub Copilot 没有魔法，不能代替思考。即使看似简单的操作，深入分析后也会发现复杂性，因此请[谨慎界定交给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段处理、学习、试验并根据结果调整。生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其作用。如果已经完成自定义说明练习，就见过 `.github/copilot-instructions.md` 的实际效果；否则现在可以快速查看。将任务分配给云智能体前，先只读检查存储库中已有的说明文件，之后就能观察其影响。

在 GitHub Web UI 或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards**，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解这些说明如何限定到 `**/*.test.ts` 文件。

下一节将把标题为 `Code lacks documentation` 的议题分配给云智能体。留意生成的拉取请求中的 TSDoc 注释、TypeScript 约定和注释头；这些要求来自说明文件。后续练习审查 PR 时会再次提及。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，确保状态正常。将任务分配给 Copilot 时也一样；它会以类似软件工程师 (SWE) 的方式处理工作。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。你可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，让它在开始工作前运行，以便访问所需的开发工具和依赖项。练习项目已预先配置此工作流，确保 Copilot 能使用 Node.js、项目依赖、端到端测试所需的浏览器二进制文件，以及为单体 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为步骤所需的最低级别。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流类似，但有几点值得注意：

- 工作流只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这样可以先验证工作流是否成功，而不必等待 Copilot 启动。

## 添加文档

大多数项目都存在信息过时或完全缺少文档的问题。这类技术债务容易被忽视，降低工作效率，也让维护代码库或引入新开发人员变得困难。Copilot 擅长创建文档，因此很适合将此任务交给云智能体；之后可在后续练习中审查结果。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

    ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，其标题可能类似 `[WIP]: Code lacks documentation`。若列表中尚未显示新 PR，请等待片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 创建待办事项列表可能需要几分钟，因为它正在设置环境（运行前面介绍的工作流）、分析项目并确定处理问题的方案。

13. 审查待办事项及其将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定方案，以及创建代码。

这可能需要几分钟。Copilot 云智能体的一项主要优势是可以异步处理任务，让你专注于其他工作。现在利用这一点，再分配另一项任务，然后继续编写应用功能。

## 在游戏详情页添加相关游戏部分

将工作拆分后，你可以专注于一组任务，同时让云智能体处理另一组任务。即使设计团队要求的相关游戏部分不一定需要很长时间，也可以把这段时间用于其他工作。现在将任务交给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论旁会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在处理第二项请求。它的工作方式类似软件工程师，因此无需持续监控；完成后再审查即可。接下来，了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体异步处理，用它解决技术债务、创建功能，或协助将代码从一个框架迁移到另一个框架。

本课探索了以下内容：

- 自定义代码生成环境。
- 确保操作安全执行。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，接下来可以创建并使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并且可以读取前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
