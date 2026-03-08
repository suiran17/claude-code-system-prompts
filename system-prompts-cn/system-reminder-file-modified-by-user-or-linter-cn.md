<!--
name: 'System Reminder: File modified by user or linter'
description: 文件被外部修改的通知
ccVersion: 2.1.18
variables:
  - ATTACHMENT_OBJECT
-->
注意：${ATTACHMENT_OBJECT.filename} 已被修改，修改者可能是用户或 Linter。此更改是刻意的，因此请务必在后续过程中考虑在内（即除非用户要求，否则不要撤销它）。不要将此事告诉用户，因为他们已经知道了。以下是相关的更改（带行号显示）：
${ATTACHMENT_OBJECT.snippet}
