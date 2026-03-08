<!--
name: 'Data: GitHub App installation PR description'
description: 安装 Claude Code GitHub App 集成时的 PR 描述模板
ccVersion: 2.0.14
-->
## \uD83E\uDD16 安装 Claude Code GitHub App

此 PR 添加了一个 GitHub Actions 工作流，以便在我们的仓库中启用 Claude Code 集成。

### 什么是 Claude Code？

[Claude Code](https://claude.com/claude-code) 是一款 AI 编码代理，可以帮助：
- 修复 Bug 并进行改进
- 更新文档
- 实现新功能
- 代码审核与建议
- 编写测试
- 以及更多功能！

### 工作原理

一旦此 PR 被合并，我们就能通过在拉取请求 (PR) 或问题 (Issue) 评论中提及 @claude 来了它进行交互。
触发工作流后，Claude 会分析评论及其周围的上下文，并在 GitHub Action 中执行请求的操作。

### 重要说明

- **此工作流在 PR 合并之前不会生效**
- **在合并完成之前，@claude 提及将不起作用**
- 只要在 PR 或 Issue 评论中提及 Claude，工作流就会自动运行
- Claude 可以访问整个 PR 或 Issue 的上下文，包括文件、Diff 差异和之前的评论

### 安全性

- 我们的 Anthropic API 密钥安全地存储在 GitHub Actions Secret 中
- 只有具有仓库写入权限的用户才能触发工作流
- 所有的 Claude 运行记录都存储在 GitHub Actions 的运行历史中
- Claude 的默认工具仅限于读取/写入文件以及通过创建评论、分支和提交来与我们的仓库交互。
- 我们可以通过在工作流文件中添加更多允许的工具来增加权限，例如：

\`\`\`
allowed_tools: Bash(npm install),Bash(npm run build),Bash(npm run lint),Bash(npm run test)
\`\`\`

更多信息请访问 [Claude Code action 仓库](https://github.com/anthropics/claude-code-action)。

合并此 PR 后，请尝试在任何 PR 的评论中提及 @claude 开启初次体验吧！
