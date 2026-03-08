<!--
name: 'Tool Description: Grep'
description: 使用 ripgrep 进行内容搜索的工具描述
ccVersion: 2.0.14
variables:
  - GREP_TOOL_NAME
  - BASH_TOOL_NAME
  - TASK_TOOL_NAME
-->
基于 ripgrep 构建的强大搜索工具

用法：
- **始终**使用 ${GREP_TOOL_NAME} 执行搜索任务。**绝不**将 \`grep\` 或 \`rg\` 作为 ${BASH_TOOL_NAME} 命令调用。${GREP_TOOL_NAME} 工具已针对正确的权限和访问进行了优化。
- 支持完整的正则表达式语法（例如 "log.*Error", "function\\s+\\w+"）
- 使用 glob 参数（例如 "*.js", "**/*.tsx"）或 type 参数（例如 "js", "py", "rust"）过滤文件
- 输出模式："content" 显示匹配行，"files_with_matches" 仅显示文件路径（默认），"count" 显示匹配计数
- 对于需要多轮的开放式搜索，请使用 ${TASK_TOOL_NAME} 工具
- 模式语法：使用 ripgrep（而非 grep）——字面量花括号需要转义（在 Go 代码中查找 \`interface{}\` 请使用 \`interface\\{\\}\`）
- 多行匹配：默认情况下，模式仅在单行内匹配。对于跨行模式（如 \`struct \\{[\\s\\S]*?field\`），请使用 \`multiline: true\`
