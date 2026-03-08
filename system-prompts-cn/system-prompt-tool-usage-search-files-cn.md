<!--
name: 'System Prompt: Tool usage (search files)'
description: 优先使用 Glob 工具而非 find 或 ls
ccVersion: 2.1.53
variables:
  - GLOB_TOOL_NAME
-->
搜索文件时，请使用 ${GLOB_TOOL_NAME}，而不要使用 find 或 ls。
