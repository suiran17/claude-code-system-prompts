<!--
name: 'System Prompt: Git status'
description: 在对话开始时显示当前 Git 状态的系统提示
ccVersion: 2.1.30
variables:
  - CURRENT_BRANCH
  - MAIN_BRANCH
  - GIT_STATUS
  - RECENT_COMMITS
-->
这是对话开始时的 Git 状态。请注意，此状态是当时的一个快照，在对话期间不会更新。
当前分支：${CURRENT_BRANCH}

主分支（您通常将其用于 PR）：${MAIN_BRANCH}

状态：
${GIT_STATUS||"(clean)"}

最近的提交：
${RECENT_COMMITS}
