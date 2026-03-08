# claude-code-system-prompts 项目说明

这个项目名为 **`claude-code-system-prompts`**，由 Piebald AI 团队维护。它是一个专门用于**收集、公开和持续追踪 Anthropic 官方 AI 编程助手 Claude Code 的所有底层系统提示词（System Prompts）**的开源代码库。

## 1. 项目背景与目的
Claude Code 在运行时并不是仅仅依赖一段简单的“你是一个 AI 编程助手”的提示词。实际上，它由 **110 多段不同的提示词、指令和数据模板**动态组合而成。这些提示词分散在被压缩混淆的 JavaScript 源码中，普通用户很难看到。

这个项目通过专门的提取脚本，在 Claude Code 每次发布新版本时，直接从源码中将这些提示词一字不差地提取出来，并整理成易于阅读的 Markdown 文件。

## 2. 核心内容分类
这里的提示词主要分为以下几大类：

- **智能体提示词 (Agent Prompts)**：
  - **子智能体**：比如专门用于探索代码库的 Explore 模式提示词，以及负责制定架构设计的 Plan 模式提示词。
  - **斜杠命令机制**：比如当你输入 `/review-pr`、`/security-review` 或 `/batch` 时，Claude Code 后台实际加载的具体指令。
  - **辅助工具**：用于生成 `CLAUDE.md`、生成 Git 提交信息（Commit）、总结上下文的内置 AI 辅助流程提示词。
- **系统行为准则 (System Prompts & Reminders)**：
  这部分规定了 Claude 作为程序员的“性格和做事方式”。例如：
  - *避免过度工程化 (avoid over-engineering)*：要求 Claude 只做用户要求的事。
  - *禁止盲目试错 (blocked approach)*：遇阻时停下来思考替代方案。
  - *安全指令 (security)*：防止写出包含注入或 XSS 漏洞的代码。
  - 包含了大量事件触发时的“系统隐式提醒”（如文件太大被截断、Token 即将用尽等）。
- **内置工具描述 (Builtin Tool Descriptions)**：
  披露了 Claude Code 是如何认知自己的工具的（比如 `Bash`、`Grep`、`Edit`、`ReadFile`、`Computer` 等），以及何时该用哪种工具。
- **上下文参考数据 (Data)**：
  内置了许多开发语言（Python, TypeScript, Go 等）的最新的 SDK 用法和 API 参考文档模板，用于写代码时参考。

## 3. 项目价值与用途
- **版本追踪与 Token 消耗统计**：项目提供了提示词库，列出了每一段提示词的 Token 长度，并维护了 `CHANGELOG.md` 追踪了上百个版本的迭代细节。
- **支持深度定制**：官方推荐搭配 `tweakcc` 工具使用。开发者可以直接修改这里的 Markdown 文件，然后打补丁到本地的 Claude Code 中，实现对官方 AI 的深度定制。

**总结**：
这就像是 Claude Code 的“大脑解剖图”或“底层源代码说明书”。对于想要学习顶级公司如何编写复杂系统提示词、设计多智能体协作架构，或者想要重度定制 AI 编程助手的开发者来说，这是一个极具价值的研究宝库。
