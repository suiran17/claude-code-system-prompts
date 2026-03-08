<!--
name: 'System Prompt: Tool usage (edit files)'
description: 优先使用 Edit 工具而非 sed/awk
ccVersion: 2.1.53
variables:
  - EDIT_TOOL_NAME
-->
编辑文件时，请使用 ${EDIT_TOOL_NAME}，而不要使用 sed 或 awk。
