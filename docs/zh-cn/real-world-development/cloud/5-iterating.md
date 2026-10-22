---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本练习中，你让 GitHub Copilot 处理了多个改善用户体验和添加功能的任务：为代码添加文档、构建相关游戏功能供设计团队迭代，以及实现高对比度和浅色模式切换等无障碍功能。现在检查代码更改；如有必要，可向 Copilot 提供反馈以改进结果。

### 场景

前面已多次强调，生成式 AI 不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在启用团队其他成员的审查前，先检查 GitHub Copilot 创建的文档、相关游戏功能和无障碍功能。

## GitHub Copilot 云智能体的安全性

Copilot 云智能体异步且自主地执行任务，因此采取了一些安全限制：

- Copilot 只能读取存储库，并且**只能**写入用于代码更改的分支。
- 云智能体在 GitHub Actions 中运行，并会创建独立、临时的工作环境。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先检查 GitHub Copilot 云智能体生成的第一个拉取请求 (PR)，即为代码添加文档的 PR。使用 GitHub.com 的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 在 github.com 上返回存储库。
2. 选择 **Pull Requests** 查看 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。请等待 Copilot 完成，这可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 检查更新后的代码，其中包含新创建的 TSDoc 文档注释和其他文档。具体更改会有所不同。

   查看更改时，留意 TSDoc 注释、TypeScript 约定和文件注释头。这些内容来自练习 2 开始时查看的自定义说明文件。

6. 审查完成且确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会出现在 PR 检查列表中。正常情况下，单体 Astro 应用的项目检查会通过。完成可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像联系团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论并启动新会话来处理请求。由于结果具有不确定性，无法给出固定的提示文本。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 在评论中使用 **@copilot** 请求修改生成的文档。可以采用上面的建议，也可以提出其他文档改进要求。
2. 选择 **View Session** 查看 Copilot 的工作。注意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回 PR。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应会出现新提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。准备好后，可将 PR 从草稿转换为准备审查状态，并合并到主分支。

![将 PR 转为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查完成且确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流。
7. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流应会出现在 PR 检查列表中。正常情况下，单体 Astro 应用的项目检查会通过，完成可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。和之前一样，可以继续与云智能体协作并请求更新。例如，调整显示的相关游戏数量，或要求添加文件注释头和 TSDoc 文档注释（请记住：创建此任务时，自定义说明尚未更新）。在 **Conversation** 选项卡发表评论即可提出请求，Copilot 会启动新会话。

## 审查无障碍功能

最后，审查由无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。请等待任务完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的 UI 组件
   - 用于保存用户偏好的本地存储
   - 高对比度和浅色模式的 CSS 或样式更改
   - 无障碍属性（ARIA 标签、键盘导航等）
   - 控制模式切换的 JavaScript/TypeScript 代码

6. 审查完成且确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会出现在 PR 检查列表中。正常情况下，单体 Astro 应用的项目检查会通过，完成可能需要几分钟。
10. **可选：**在 Codespace 中切换到此分支，手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式开关，并确认刷新页面后设置仍然保留。

可以看到，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或改进机会，可以像之前一样在评论中提及 **@copilot**，请求更新。

## 可选练习 — 继续委派任务

云智能体最适合处理真实待办事项，让你可以继续处理其他工作。可以在存储库中创建更多议题并分配给 Copilot，练习这种工作方式。例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页添加分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体练习路径。通过这些课程，你：

- **检查了存储库附带的自定义说明文件**，以便之后观察云智能体输出中的影响。
- **向 Copilot 云智能体分配了议题**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用了自定义智能体**，实现高对比度和浅色模式切换等无障碍功能。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导正在进行的无障碍会话。
- **审查并迭代改进了云智能体的拉取请求**，通过 @copilot 请求修改并批准工作流。

## 回顾和后续步骤

云智能体练习路径已完成。若要继续探索，可学习其他练习路径：

- 🖥️ **[VS Code 练习路径](../../vscode/)** — 在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 练习路径](../../cli/)** — 在终端中交付两项经过审查的更改，学习 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下后续操作：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
