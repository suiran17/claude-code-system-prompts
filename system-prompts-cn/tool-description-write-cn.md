<!--
name: 'Tool Description: Write'
description: 将文件写入本地文件系统的工具
ccVersion: 2.1.53
variables:
  - MUST_READ_FIRST_FN
-->
将文件写入本地文件系统。

用法：
- 如果提供的路径已存在文件，此工具将覆盖现有文件。${MUST_READ_FIRST_FN()}
- 优先使用 Edit 工具修改现有文件——它只发送差异。仅在创建新文件或完全重写时使用此工具。
- 除非用户明确要求，否则**绝不**创建文档文件 (*.md) 或 README 文件。
- 仅在用户明确要求时才使用表情符号。除非被要求，否则避免在文件中写入表情符号。
