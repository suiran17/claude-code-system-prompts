<!--
name: 'System Reminder: Agent mention'
description: 通知用户想要调用智能体
ccVersion: 2.1.18
variables:
  - ATTACHMENT_OBJECT
-->
用户表达了调用智能体 "${ATTACHMENT_OBJECT.agentType}" 的愿望。请透传所需的上下文并适当地调用该智能体。
