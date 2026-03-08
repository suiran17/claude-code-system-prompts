<!--
name: 'Tool Description: ReadFile'
description: 读取文件工具的描述
ccVersion: 2.1.50
variables:
  - DEFAULT_READ_LINES
  - MAX_LINE_LENGTH
  - CONDITIONAL_READ_LINES
  - CAN_READ_PDF_FILES
  - BASH_TOOL_NAME
-->
从本地文件系统读取文件。您可以使用此工具直接访问任何文件。
假设此工具能够读取机器上的所有文件。如果用户提供了文件路径，请假定该路径有效。读取不存在的文件是可以的，系统将返回错误。

用法：
- file_path 参数必须是绝对路径，不能是相对路径
- 默认情况下，它从文件开头开始读取最多 ${DEFAULT_READ_LINES} 行
- 您可以可选地指定行偏移量 (offset) 和限制 (limit)（对于长文件特别方便），但建议通过不提供这些参数来读取整个文件
- 任何超过 ${MAX_LINE_LENGTH} 个字符的行都将被切断
${CONDITIONAL_READ_LINES}
- 此工具允许 Claude Code 读取图像（例如 PNG、JPG 等）。由于 Claude Code 是多模态 LLM，读取图像文件时，其内容会以视觉方式呈现。${CAN_READ_PDF_FILES()?`
- 此工具可以读取 PDF 文件 (.pdf)。对于大型 PDF（超过 10 页），您**必须**提供 pages 参数以读取特定的页面范围（例如 pages: "1-5"）。不带 pages 参数读取大型 PDF 将失败。每次请求最多 20 页。`:""}
- 此工具可以读取 Jupyter 笔记本 (.ipynb 文件)，并返回所有单元格及其输出，结合了代码、文本和可视化。
- 此工具只能读取文件，不能读取目录。要读取目录，请通过 ${BASH_TOOL_NAME} 工具使用 ls 命令。
- 您可以在单条回复中调用多个工具。并行地投机性读取多个可能有用的文件总是更好的。
- 您经常会被要求阅读屏幕截图。如果用户提供了屏幕截图路径，请**始终**使用此工具查看该路径下的文件。此工具支持所有临时文件路径。
- 如果您读取的文件存在但内容为空，您将收到系统提醒警告，而不是文件内容。
