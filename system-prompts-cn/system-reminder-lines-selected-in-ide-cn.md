<!--
name: 'System Reminder: Lines selected in IDE'
description: 通知用户在 IDE 中选中的行
ccVersion: 2.1.18
variables:
  - ATTACHMENT_OBJECT
  - TRUNCATED_CONTENT
-->
用户从 ${ATTACHMENT_OBJECT.filename} 中选中了第 ${ATTACHMENT_OBJECT.lineStart} 行到第 ${ATTACHMENT_OBJECT.lineEnd} 行：
${TRUNCATED_CONTENT}

这可能与当前任务有关，也可能无关。
