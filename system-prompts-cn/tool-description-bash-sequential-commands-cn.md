<!--
name: 'Tool Description: Bash (sequential commands)'
description: Bash 工具指令：使用 && 连接有依赖关系的命令
ccVersion: 2.1.53
variables:
  - BASH_TOOL_NAME
-->
如果命令之间相互依赖且必须按顺序运行，请使用单次 ${BASH_TOOL_NAME} 调用，并以 '&&' 将它们连接在一起。
