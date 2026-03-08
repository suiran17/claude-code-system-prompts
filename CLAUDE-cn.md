# Claude Code 系统提示词 (Claude Code System Prompts)

## 仓库性质

本仓库包含使用脚本从 Claude Code npm 包编译后的 JavaScript 源码中提取的系统提示词。本项目由 [Piebald AI](https://piebald.ai/) 维护，而非 Anthropic 官方。

有关提取方法的详细信息，请参阅 [README.md 中的提取章节](./README-cn.md#提取逻辑-extraction)。

## 什么是 Claude Code

Claude Code 是 Anthropic 推出的用于代理化编程 (Agentic Coding) 的命令行工具 (CLI)。它以编译后的 npm 包 (`@anthropic-ai/claude-code`) 形式分发。其源代码不公开。 [anthropics/claude-code](https://github.com/anthropics/claude-code) GitHub 仓库仅用于发布问题 (Issues) 和版本 (Releases)。

## 如何使用这些文件

- **参考资料：** 了解 Claude Code 使用哪些提示词，以及这些提示词在不同版本之间是如何变化的。
- **本地打补丁：** 使用 [tweakcc](https://github.com/Piebald-AI/tweakcc) 在您的本地 Claude Code 安装中自定义单个提示词片段。
- **功能请求：** 若要对 Claude Code 的提示词提出修改建议，请在 [anthropics/claude-code/issues](https://github.com/anthropics/claude-code/issues) 提交 Issue。

## 针对处理该仓库的 AI 代理

- 这些文件是**提取出的参考资料**，而非可修改的源代码。
- 修改此处的文件不会改变 Claude Code 的行为。
- `system-prompts/` 目录包含带有 YAML Frontmatter 的 Markdown 文件，其中注明了 Claude Code 版本和模板变量。
- 类似于 `${BASH_TOOL_NAME}` 的模板变量在运行时由 Claude Code 进行插值 —— 在这些文件中，它们以字面字符串形式出现。
- [CHANGELOG.md](./CHANGELOG.md) 追踪了 Claude Code 各个版本之间的提示词变更。
