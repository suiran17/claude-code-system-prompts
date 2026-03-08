<!--
name: 'System Reminder: USD budget'
description: 当前 USD 预算统计
ccVersion: 2.1.18
variables:
  - ATTACHMENT_OBJECT
-->
USD 预算：$${ATTACHMENT_OBJECT.used}/$${ATTACHMENT_OBJECT.total}；剩余 $${ATTACHMENT_OBJECT.remaining}
