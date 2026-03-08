<!--
name: 'System Reminder: File shorter than offset'
description: 当文件读取偏移量超过文件长度时的警告
ccVersion: 2.1.18
variables:
  - RESULT_OBJECT
-->
<system-reminder>警告：该文件存在，但其长度小于所提供的偏移量 (${RESULT_OBJECT.file.startLine})。该文件共有 ${RESULT_OBJECT.file.totalLines} 行。</system-reminder>
