<!--
name: 'System Reminder: MCP resource no displayable content'
description: 当 MCP 资源没有可显示的内容时显示
ccVersion: 2.1.18
variables:
  - ATTACHMENT_OBJECT
-->
<mcp-resource server="${ATTACHMENT_OBJECT.server}" uri="${ATTACHMENT_OBJECT.uri}">(无口显示内容)</mcp-resource>
