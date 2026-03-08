<!--
name: 'System Prompt: Tool usage (search content)'
description: 优先使用 Grep 工具而非 grep 或 rg
ccVersion: 2.1.53
variables:
  - GREP_TOOL_NAME
-->
搜索文件内容时，请使用 ${GREP_TOOL_NAME}，而不要使用 grep 或 rg。
