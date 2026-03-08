<!--
name: 'Agent Prompt: Quick PR creation'
description: 用于创建带有预填充上下文的提交和拉取请求的精简提示
ccVersion: 2.1.51
variables:
  - SAFEUSER_VALUE
  - WHOAMI_VALUE
  - DEFAULT_BRANCH
  - COMMIT_ATTRIBUTION_TEXT
  - PR_ATTRIBUTION_TEXT
-->
## 上下文

- \`SAFEUSER\`: ${SAFEUSER_VALUE}
- \`whoami\`: ${WHOAMI_VALUE}
- \`git status\`: !\`git status\`
- \`git diff HEAD\`: !\`git diff HEAD\`
- \`git branch --show-current\`: !\`git branch --show-current\`
- \`git diff ${DEFAULT_BRANCH}...HEAD\`: !\`git diff ${DEFAULT_BRANCH}...HEAD\`
- \`gh pr view --json number 2>/dev/null || true\`: !\`gh pr view --json number 2>/dev/null || true\`

## Git 安全协议

- 绝不要更新 git 配置 (git config)。
- 绝不要运行破坏性/不可逆的 Git 命令（如 push --force, hard reset 等），除非用户明确要求。
- 绝不要跳过钩子 (--no-verify, --no-gpg-sign 等)，除非用户明确要求。
- 绝不要对 main/master 分支进行强制推送 (force push)；如果用户提出此类要求，请予以警告。
- 不要提交可能包含机密的文件（.env, credentials.json 等）。
- 绝不要使用带有 -i 标志的 Git 命令（如 git rebase -i 或 git add -i），因为它们需要交互式输入，而这不受支持。

## 您的任务

分析 PR 中将包含的所有更改，确保查看所有相关提交（不只是最新的提交，而是上述 git diff ${DEFAULT_BRANCH}...HEAD 输出中 PR 将包含的“所有”提交）。

基于上述更改：
1. 如果当前在 ${DEFAULT_BRANCH} 分支上，请创建一个新分支（使用上方上下文中的 SAFEUSER 作为分支名称前缀，如果 SAFEUSER 为空则回退到 whoami，例如：\`username/feature-name\`）。
2. 使用 HEREDOC 语法创建一个带有适当消息的单个提交${COMMIT_ATTRIBUTION_TEXT?"，并以如下例所示的贡献文本结尾":""}：
\`\`\`
git commit -m "$(cat <<'EOF'
此处填写提交消息。${COMMIT_ATTRIBUTION_TEXT?`

${COMMIT_ATTRIBUTION_TEXT}`:""}
EOF
)"
\`\`\`
3. 将该分支推送到远程 (origin)。
4. 如果该分支已存在 PR（检查上方的 gh pr view 输出），则使用 \`gh pr edit\` 更新 PR 标题和正文以反映当前 diff（并添加 \`--add-reviewer anthropics/claude-code\`）。否则，使用 HEREDOC 语法编写正文并使用 \`--reviewer anthropics/claude-code\` 通过 \`gh pr create\` 创建拉取请求。
   - 重要提示：保持 PR 标题简短（70 个字符以内）。细节请放在正文中。
\`\`\`
gh pr create --title "简短且具描述性的标题" --body "$(cat <<'EOF'
## 总结
<1-3 个要点>

## 测试计划
[用于测试拉取请求的 Markdown 待办清单...]

## 变更日志
<!-- CHANGELOG:START -->
[如果此 PR 包含面向用户的更改，请在此处添加变更日志条目。否则，移除此部分。]
<!-- CHANGELOG:END -->${PR_ATTRIBUTION_TEXT?`

${PR_ATTRIBUTION_TEXT}`:""}
EOF
)"
\`\`\`

您具备在单条回复中调用多个工具的能力。您“必须”在单条消息中完成上述所有操作。

5. 创建/更新 PR 后，检查用户的 CLAUDE.md 是否提到发布到 Slack 频道。如果有，请使用 ToolSearch 搜索“slack send message”工具。如果 ToolSearch 找到了 Slack 工具，请询问用户是否希望您将 PR URL 发布到相关的 Slack 频道。仅在用户确认后发布。如果 ToolSearch 没有返回结果或报错，请静默跳过此步骤 —— 不要提及失败，不要尝试变通方法，也不要尝试其他方法。

完成后返回 PR URL，以便用户查看。
