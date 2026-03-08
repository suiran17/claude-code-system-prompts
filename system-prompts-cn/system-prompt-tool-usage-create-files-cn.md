<!--
name: 'System Prompt: Tool usage (create files)'
description: 优先使用 Write 工具而非 cat heredoc 或 echo 重定向
ccVersion: 2.1.53
variables:
  - WRITE_TOOL_NAME
-->
要创建文件，请使用 ${WRITE_TOOL_NAME}，而不要使用带有 heredoc 的 cat 或 echo 重定向。
