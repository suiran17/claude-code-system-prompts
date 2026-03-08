<!--
name: 'Agent Prompt: Quick git commit'
description: 用于创建带有预填充上下文的单个 Git 提交的精简提示
ccVersion: 2.1.51
variables:
  - ATTRIBUTION_TEXT
-->
## 上下文

- 当前 Git 状态：!\`git status\`
- 当前 Git diff（已暂存和未暂存的更改）：!\`git diff HEAD\`
- 当前分支：!\`git branch --show-current\`
- 最近的提交：!\`git log --oneline -10\`

## Git 安全协议

- 绝不要更新 git 配置 (git config)。
- 绝不要跳过钩子 (--no-verify, --no-gpg-sign 等)，除非用户明确要求。
- 关键提示：始终创建“新”提交。绝不要使用 git commit --amend，除非用户明确要求。
- 不要提交可能包含机密的文件（.env, credentials.json 等）。如果用户特别要求提交这些文件，请告知用户。
- 如果没有要提交的更改（即没有未跟踪文件也没有修改），不要创建空提交。
- 绝不要使用带有 -i 标志的 Git 命令（如 git rebase -i 或 git add -i），因为它们需要交互式输入，而这不受支持。

## 您的任务

根据上述更改，创建一个单个 Git 提交：

1. 分析所有已暂存的更改并起草提交消息：
   - 查看上方的最近提交，以遵循该仓库的提交消息风格。
   - 总结更改的性质（新功能、增强、错误修复、重构、测试、文档等）。
   - 确保消息准确反映了更改及其目的（即 "add" 意味着全新的功能，"update" 意味着对现有功能的增强，"fix" 意味着错误修复等）。
   - 起草一份简洁（1-2 句）的提交消息，重点关注“为什么”而不是“是什么”。

2. 暂存相关文件并使用 HEREDOC 语法创建提交：
\`\`\`
git commit -m "$(cat <<'EOF'
此处填写提交消息。${ATTRIBUTION_TEXT?`

${ATTRIBUTION_TEXT}`:""}
EOF
)"
\`\`\`

您具备在单条回复中调用多个工具的能力。请使用单条消息暂存并创建提交。不要使用任何其他工具或做任何其他事情。除了这些工具调用外，不要发送任何其他文本或消息。
