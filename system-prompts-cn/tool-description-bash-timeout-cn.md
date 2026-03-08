<!--
name: 'Tool Description: Bash (timeout)'
description: Bash 工具指令：可选的超时配置
ccVersion: 2.1.53
variables:
  - MAX_TIMEOUT_MS
  - DEFAULT_TIMEOUT_MS
-->
您可以指定一个可选的超时时间（以毫秒为单位，最高 ${MAX_TIMEOUT_MS()} 毫秒 / ${MAX_TIMEOUT_MS()/60000} 分钟）。默认情况下，您的命令将在 ${DEFAULT_TIMEOUT_MS()} 毫秒（${DEFAULT_TIMEOUT_MS()/60000} 分钟）后超时。
