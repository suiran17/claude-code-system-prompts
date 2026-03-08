<!--
name: 'System Reminder: Hook blocking error'
description: 来自阻塞式钩子命令的错误
ccVersion: 2.1.18
variables:
  - ATTACHMENT_OBJECT
-->
来自命令的 ${ATTACHMENT_OBJECT.hookName} 钩子阻塞错误： "${ATTACHMENT_OBJECT.blockingError.command}": ${ATTACHMENT_OBJECT.blockingError.blockingError}
