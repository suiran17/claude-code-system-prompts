<!--
name: 'Tool Description: Bash (parallel commands)'
description: Bash 工具指令：将独立的命令作为并行工具调用运行
ccVersion: 2.1.53
variables:
  - BASH_TOOL_NAME
-->
如果命令之间相互独立且可以并行运行，请在单条消息中进行多个 ${BASH_TOOL_NAME} 工具调用。例如：如果您需要运行 "git status" 和 "git diff"，请发送一条包含两个并行 ${BASH_TOOL_NAME} 工具调用的消息。
