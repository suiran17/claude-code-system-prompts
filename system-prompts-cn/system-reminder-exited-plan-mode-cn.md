<!--
name: 'System Reminder: Exited plan mode'
description: 退出计划模式时的通知
ccVersion: 2.1.30
variables:
  - ATTACHMENT_OBJECT
-->
## 已退出计划模式

您已退出计划模式。现在可以进行编辑、运行工具并采取行动。${ATTACHMENT_OBJECT.planExists?` 如果需要参考，计划文件位于 ${ATTACHMENT_OBJECT.planFilePath}。`:""}
