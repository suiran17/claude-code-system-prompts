<!--
name: 'Skill: Build with Claude API (reference guide)'
description: 用于展示特定语言参考文档并提供任务快速导航的模板
ccVersion: 2.1.47
-->
## 参考文档

所检测语言的相关文档已包含在下方的 \`<doc>\` 标签中。每个标签都有一个 \`path\` 属性显示其原始文件路径。请根据此属性找到正确的章节：

### 任务快速参考

**单次文本分类/总结/提取/问答：**
→ 参考 \`{lang}/claude-api/README.md\`

**聊天 UI 或实时响应展示：**
→ 参考 \`{lang}/claude-api/README.md\` + \`{lang}/claude-api/streaming.md\`

**长时间对话（可能超出上下文窗口）：**
→ 参考 \`{lang}/claude-api/README.md\` —— 见“对话压缩 (Compaction)”部分

**函数调用 / 工具使用 / 代理 (Agents)：**
→ 参考 \`{lang}/claude-api/README.md\` + \`shared/tool-use-concepts.md\` + \`{lang}/claude-api/tool-use.md\`

**批量处理（对延迟不敏感）：**
→ 参考 \`{lang}/claude-api/README.md\` + \`{lang}/claude-api/batches.md\`

**跨多个请求的文件上传：**
→ 参考 \`{lang}/claude-api/README.md\` + \`{lang}/claude-api/files-api.md\`

**带有内置工具（文件/网页/终端）的代理 (仅限 Python & TypeScript)：**
→ 参考 \`{lang}/agent-sdk/README.md\` + \`{lang}/agent-sdk/patterns.md\`

**错误处理：**
→ 参考 \`shared/error-codes.md\`

**通过 WebFetch 获取最新文档：**
→ 参考 \`shared/live-sources.md\` 获取 URL
