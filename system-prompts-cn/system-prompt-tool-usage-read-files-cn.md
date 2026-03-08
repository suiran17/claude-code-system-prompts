<!--
name: 'System Prompt: Tool usage (read files)'
description: 优先使用 Read 工具而非 cat/head/tail/sed
ccVersion: 2.1.53
variables:
  - READ_TOOL_NAME
-->
阅读文件时，请使用 ${READ_TOOL_NAME}，而不要使用 cat、head、tail 或 sed。
