<!--
name: 'Data: Live documentation sources'
description: 通过 WebFetch 从官方渠道获取最新的 Claude API 和 Agent SDK 文档的 URL 列表
ccVersion: 2.1.63
-->
# 实时文档来源 (Live Documentation Sources)

本文件包含用于从 platform.claude.com 和 Agent SDK 仓库获取实时信息的 WebFetch URL。当用户需要的信息自上次内容缓存更新以后可能已发生变化时，请使用这些链接。

## 何时使用 WebFetch

- 用户明确要求提供“最新 (latest)”或“当前 (current)”信息时
- 缓存数据看起来不正确时
- 用户询问缓存内容未涵盖的特性时
- 用户需要特定的 API 细节或示例时

## Claude API 文档 URL

### 模型与定价

| 主题 | URL | 提取提示 (Extraction Prompt) |
| :--- | :--- | :--- |
| 模型概览 (Models Overview) | \`https://platform.claude.com/docs/en/about-claude/models/overview.md\` | "提取所有 Claude 模型的当前模型 ID、上下文窗口和定价" |
| 定价 (Pricing) | \`https://platform.claude.com/docs/en/pricing.md\` | "提取当前每 100 万 token 的输入和输出定价" |

### 核心特性

| 主题 | URL | 提取提示 (Extraction Prompt) |
| :--- | :--- | :--- |
| 扩展思考 (Extended Thinking) | \`https://platform.claude.com/docs/en/build-with-claude/extended-thinking.md\` | "提取扩展思考参数、budget_tokens 要求和使用示例" |
| 适应性思考 (Adaptive Thinking) | \`https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking.md\` | "提取适应性思考设置、努力程度 (effort) 等级以及 {{OPUS_NAME}} 使用示例" |
| 努力程度参数 (Effort Parameter) | \`https://platform.claude.com/docs/en/build-with-claude/effort.md\` | "提取努力程度等级、成本质量权衡以及与思考功能的交互" |
| 工具使用 (Tool Use) | \`https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview.md\` | "提取工具定义架构 (schema)、tool_choice 选项和工具结果处理" |
| 流式传输 (Streaming) | \`https://platform.claude.com/docs/en/build-with-claude/streaming.md\` | "提取流式传输事件类型、SDK 示例和最佳实践" |
| Prompt 缓存 | \`https://platform.claude.com/docs/en/build-with-claude/prompt-caching.md\` | "提取 cache_control 的用法、定价收益和实现示例" |

### 媒体与文件

| 主题 | URL | 提取提示 (Extraction Prompt) |
| :--- | :--- | :--- |
| 视觉 (Vision) | \`https://platform.claude.com/docs/en/build-with-claude/vision.md\` | "提取支持的图像格式、大小限制和代码示例" |
| PDF 支持 | \`https://platform.claude.com/docs/en/build-with-claude/pdf-support.md\` | "提取 PDF 处理能力、限制和示例" |

### API 操作

| 主题 | URL | 提取提示 (Extraction Prompt) |
| :--- | :--- | :--- |
| 批量处理 (Batch Processing) | \`https://platform.claude.com/docs/en/build-with-claude/batch-processing.md\` | "提取批量 API 的端点、请求格式以及结果轮询" |
| 文件 API | \`https://platform.claude.com/docs/en/build-with-claude/files.md\` | "提取文件上传、下载以及在消息中的引用，包括支持的类型和 Beta 版标头" |
| Token 计数 | \`https://platform.claude.com/docs/en/build-with-claude/token-counting.md\` | "提取 Token 计数 API 的用法和示例" |
| 频率限制 (Rate Limits) | \`https://platform.claude.com/docs/en/api/rate-limits.md\` | "提取按层级 (Tier) 和模型划分的当前频率限制" |
| 错误 (Errors) | \`https://platform.claude.com/docs/en/api/errors.md\` | "提取 HTTP 错误代码、含义和重试指南" |

### 工具

| 主题 | URL | 提取提示 (Extraction Prompt) |
| :--- | :--- | :--- |
| 代码执行 (Code Execution) | \`https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool.md\` | "提取代码执行工具设置、文件上传、容器复用和响应处理" |
| 电脑使用 (Computer Use) | \`https://platform.claude.com/docs/en/agents-and-tools/tool-use/computer-use.md\` | "提取电脑使用工具设置、功能和实现示例" |

### 高级特性

| 主题 | URL | 提取提示 (Extraction Prompt) |
| :--- | :--- | :--- |
| 结构化输出 (Structured Outputs) | \`https://platform.claude.com/docs/en/build-with-claude/structured-outputs.md\` | "提取 output_config.format 的用法和架构强制执行" |
| 对话压缩 (Compaction) | \`https://platform.claude.com/docs/en/build-with-claude/compaction.md\` | "提取压缩设置、触发配置以及带压缩的流式传输" |
| 引用 (Citations) | \`https://platform.claude.com/docs/en/build-with-claude/citations.md\` | "提取引用格式和实现方法" |
| 上下文窗口 | \`https://platform.claude.com/docs/en/build-with-claude/context-windows.md\` | "提取上下文窗口大小和 Token 管理" |

---

## Claude API SDK 仓库

| SDK | URL | 描述 |
| :--- | :--- | :--- |
| Python | \`https://github.com/anthropics/anthropic-sdk-python\` | \`anthropic\` pip 包源码 |
| TypeScript | \`https://github.com/anthropics/anthropic-sdk-typescript\` | \`@anthropic-ai/sdk\` npm 包源码 |
| Java | \`https://github.com/anthropics/anthropic-sdk-java\` | \`anthropic-java\` Maven 源码 |
| Go | \`https://github.com/anthropics/anthropic-sdk-go\` | Go 模块源码 |
| Ruby | \`https://github.com/anthropics/anthropic-sdk-ruby\` | \`anthropic\` gem 源码 |
| C# | \`https://github.com/anthropics/anthropic-sdk-csharp\` | NuGet 包源码 |
| PHP | \`https://github.com/anthropics/anthropic-sdk-php\` | Composer 包源码 |

---

## Agent SDK 文档 URL

### 核心文档

| 主题 | URL | 提取提示 (Extraction Prompt) |
| :--- | :--- | :--- |
| Agent SDK 概览 | \`https://platform.claude.com/docs/en/agent-sdk.md\` | "提取 Agent SDK 概览、核心特性和用例" |
| Agent SDK Python | \`https://github.com/anthropics/claude-agent-sdk-python\` | "提取 Python SDK 的安装、导入和基础用法" |
| Agent SDK TypeScript | \`https://github.com/anthropics/claude-agent-sdk-typescript\` | "提取 TypeScript SDK 的安装、导入和基础用法" |

### SDK 参考 (GitHub README)

| 主题 | URL | 提取提示 (Extraction Prompt) |
| :--- | :--- | :--- |
| Python SDK | \`https://raw.githubusercontent.com/anthropics/claude-agent-sdk-python/main/README.md\` | "提取 Python SDK 的 API 参考、类和方法" |
| TypeScript SDK | \`https://raw.githubusercontent.com/anthropics/claude-agent-sdk-typescript/main/README.md\` | "提取 TypeScript SDK 的 API 参考、类型和函数" |

### npm/PyPI 软件包

| 软件包 | URL | 描述 |
| :--- | :--- | :--- |
| claude-agent-sdk (Python) | \`https://pypi.org/project/claude-agent-sdk/\` | PyPI 上的 Python 软件包 |
| @anthropic-ai/claude-agent-sdk (TS) | \`https://www.npmjs.com/package/@anthropic-ai/claude-agent-sdk\` | npm 上的 TypeScript 软件包 |

### GitHub 仓库

| 资源 | URL | 描述 |
| :--- | :--- | :--- |
| Python SDK | \`https://github.com/anthropics/claude-agent-sdk-python\` | Python 软件包源码 |
| TypeScript SDK | \`https://github.com/anthropics/claude-agent-sdk-typescript\` | TypeScript/Node.js 软件包源码 |
| MCP 服务器 | \`https://github.com/modelcontextprotocol\` | 官方 MCP 服务器实现 |

---

## 备选方案

如果 WebFetch 失败（网络问题、URL 已变更）：

1. 使用来自语言特定文件中的缓存内容（注意缓存日期）
2. 告知用户数据可能已过时
3. 建议直接检查 platform.claude.com 或 GitHub 仓库
