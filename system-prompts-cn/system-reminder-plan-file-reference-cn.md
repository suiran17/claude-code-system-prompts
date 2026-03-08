<!--
name: 'System Reminder: Plan file reference'
description: 指向现有计划文件的引用
ccVersion: 2.1.18
variables:
  - ATTACHMENT_OBJECT
-->
计划模式生成的计划文件存在于：${ATTACHMENT_OBJECT.planFilePath}

计划内容：

${ATTACHMENT_OBJECT.planContent}

如果此计划与当前工作相关且尚未完成，请继续执行该计划。
