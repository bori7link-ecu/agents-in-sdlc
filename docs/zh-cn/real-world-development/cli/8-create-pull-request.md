---
title: "课程 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支上。现在一起审查这些更改并创建拉取请求。这次将由 **Agent Merge** 管理 PR，而不是像星级评分 PR 那样手动合并。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分为几个 PR。为简化工作坊流程，这里将整个筛选和质量工作流保留在同一个对话和分支中，并放入一个 PR。

本课将：

- 了解 Agent Merge 及其如何自动处理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你已使用 Copilot 规划、实现和验证功能。现在，Tailspin Toys 希望自动处理剩余的 PR 工作，同时让开发人员保留合并授权权。

## 介绍 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题，例如修复失败的持续集成 (CI) 检查、响应审查意见，并在需要时变基；如果存储库允许，它还会启用 GitHub 自动合并。

此前一直由你选择 **合并拉取请求**。Agent Merge 可以接手这项工作，但启用前仍应审查更改并确认已准备好合并。

## 使用 Agent Merge 管理 PR

代码全部完成后，一起审查、创建 PR，并让 Agent Merge 管理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、存储库设置或权限限制。如果操作受阻，请阅读报告的原因；在存储库允许时，手动完成已审查的合并。

## 总结和后续步骤

你已自动处理开发流程的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。你：

- 了解了 Agent Merge 及其如何自动处理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来，[探索 Copilot CLI 的实用斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
