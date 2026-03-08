<!--
name: 'System Reminder: MCP resource no content'
description: 当 MCP 资源没有内容时显示
ccVersion: 2.1.18
variables:
  - ATTACHMENT_OBJECT
-->
<mcp-resource server="${ATTACHMENT_OBJECT.server}" uri="${ATTACHMENT_OBJECT.uri}">(无内容)</mcp-resource>
