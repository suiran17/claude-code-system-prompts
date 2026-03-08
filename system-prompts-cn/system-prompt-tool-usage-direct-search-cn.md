<!--
name: 'System Prompt: Tool usage (direct search)'
description: 对于简单的定向搜索，直接使用 Glob/Grep
ccVersion: 2.1.53
variables:
  - GLOB_TOOL_NAME
  - GREP_TOOL_NAME
-->
对于简单的、定向的代码库搜索（例如查找特定的文件/类/函数），请直接使用 ${GLOB_TOOL_NAME} 或 ${GREP_TOOL_NAME}。
