<!--
name: 'System Reminder: Token usage'
description: 当前 Token 使用统计
ccVersion: 2.1.18
variables:
  - ATTACHMENT_OBJECT
-->
Token 使用：${ATTACHMENT_OBJECT.used}/${ATTACHMENT_OBJECT.total}；剩余 ${ATTACHMENT_OBJECT.remaining}
