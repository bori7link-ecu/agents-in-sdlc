---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 Copilot 云智能体处理了三个改善用户体验和增加功能的独立任务。云智能体可以异步、自主地工作，但仍然需要监控任务进度。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，可以查看所有带有开放拉取请求 (PR) 的智能体任务、了解已执行的操作，甚至引导正在进行的会话。

本课将：

- 查看智能体页面并监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也应该添加浅色模式。既然正在更新网站样式并添加切换功能，顺便实现浅色模式也很合适。你将引导智能体，让它在实现高对比度模式的同时添加浅色模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务的当前状态：

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 留意主面板和左侧面板中的任务列表。你应该能看到已分配给 Copilot 的任务，例如：
   - 更新代码库文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的各项步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 的工作状态和处理方式而异。

4. 还要留意右侧的拉取请求 (PR) 面板。你可以在这里查看 PR 和更改文件，进一步监控任务。

## 引导云智能体

了解当前进行中的任务后，让 Copilot 在实现高对比度模式的同时也添加浅色模式。

1. 选择与高对比度模式相关的会话。标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到状态显示设置完成并已开始工作。出现类似下方的消息时，说明已进入该阶段。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务，突出显示引导正在进行的会话对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。它会先完成收到消息时正在执行的步骤，然后将新要求纳入后续工作。

这也需要几分钟。等待期间，可以回顾和思考目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪正在进行和已完成的任务、审查工作内容，还可以重定向正在进行的任务以调整范围或提供额外指导。

本课探索了以下内容：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话以请求添加功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
