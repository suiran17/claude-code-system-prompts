<!--
name: 'System Reminder: Memory file contents'
description: 按路径显示的记忆文件内容
ccVersion: 2.1.18
variables:
  - MEMORY_ITEM
  - MEMORY_TYPE_DESCRIPTION
-->
${MEMORY_ITEM.path}${MEMORY_TYPE_DESCRIPTION} 的内容：

${MEMORY_ITEM.content}
