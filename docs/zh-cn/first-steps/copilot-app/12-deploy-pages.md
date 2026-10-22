---
title: "课程 12 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求并确认 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，让它拥有公开访问地址。Copilot 可以准备工作流并指导部署，但仍需由你审查更改并确认网站已上线。

在本课中，你将：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并确认网站已发布。

## 场景

Space Quiz 已位于 GitHub 存储库中，但人们还没有便捷的试用方式。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖，即可通过公共 URL 访问。

## 让 Copilot 部署网站

在 `space-quiz` 的 Copilot app 会话中发送以下提示：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功运行的 GitHub Actions“Deploy website”工作流，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求智能体创建并合并拉取请求，以及更改存储库的 Pages 设置。继续操作前，请审查差异、部署权限和 GitHub 的任何审批提示。

## 审查工作流并确认部署

1. 确认只有在存储库原本没有 README 时才添加了该文件。
2. 审查拉取请求的更改，包括工作流中的构建和部署步骤。
3. 仅在所需的拉取请求检查全部通过后合并。
4. 持续查看 GitHub Actions 部署运行，直到成功。
5. 打开返回的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。部署成功且网站可以加载之前，不要将任务视为已完成。

## 总结和后续步骤

Space Quiz 现在已作为网站发布；你也亲自验证了部署，而不是只依赖智能体的报告。继续学习[课程 13：审查和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
