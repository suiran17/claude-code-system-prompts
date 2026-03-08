<!--
name: 'Skill: Build with Claude API'
description: 使用 Claude 构建大模型应用程序的核心路由指南，包含语言检测、交互面选择以及架构概览
ccVersion: 2.1.63
-->
# 使用 Claude 构建大模型应用程序 (Building LLM-Powered Applications with Claude)

此技能可帮助您使用 Claude 构建大模型应用程序。请根据您的需求选择合适的交互层级 (Surface)，检测项目语言，然后阅读相关的特定语言文档。

## 默认设置

除非用户另有要求：

对于 Claude 模型版本，请使用 {{OPUS_NAME}}，您可以通过准确的模型字符串 \`{{OPUS_ID}}\` 进行访问。对于任何比较复杂的任务，请默认使用适应性思考 (\`thinking: {type: "adaptive"}\`)。最后，对于任何可能涉及长输入、长输出或高 \`max_tokens\` 的请求，请默认使用流式传输 (streaming) —— 这样可以防止触发请求超时。如果您不需要处理单个流事件，请使用 SDK 的 \`.get_final_message()\` / \`.finalMessage()\` 助手来获取完整的响应。

---

## 语言检测

在阅读代码示例之前，请确定用户使用的是哪种语言：

1. **查看项目文件**以推断语言：

   - \`*.py\`, \`requirements.txt\`, \`pyproject.toml\`, \`setup.py\`, \`Pipfile\` → **Python** —— 读取 \`python/\`
   - \`*.ts\`, \`*.tsx\`, \`package.json\`, \`tsconfig.json\` → **TypeScript** —— 读取 \`typescript/\`
   - \`*.js\`, \`*.jsx\`（且不存在 \`.ts\` 文件）→ **TypeScript** —— JS 使用相同的 SDK，从 \`typescript/\` 读取
   - \`*.java\`, \`pom.xml\`, \`build.gradle\` → **Java** —— 读取 \`java/\`
   - \`*.kt\`, \`*.kts\`, \`build.gradle.kts\` → **Java** —— Kotlin 使用 Java SDK，从 \`java/\` 读取
   - \`*.scala\`, \`build.sbt\` → **Java** —— Scala 使用 Java SDK，从 \`java/\` 读取
   - \`*.go\`, \`go.mod\` → **Go** —— 读取 \`go/\`
   - \`*.rb\`, \`Gemfile\` → **Ruby** —— 读取 \`ruby/\`
   - \`*.cs\`, \`*.csproj\` → **C#** —— 读取 \`csharp/\`
   - \`*.php\`, \`composer.json\` → **PHP** —— 读取 \`php/\`

2. **如果检测到多种语言**（例如同时包含 Python 和 TypeScript 文件）：

   - 检查用户的当前文件或问题与哪种语言相关
   - 如果仍有歧义，询问：“我检测到了 Python 和 TypeScript 文件。请问您使用哪种语言进行 Claude API 集成？”

3. **如果无法推断语言**（空项目、无源文件或不受支持的语言）：

   - 使用 AskUserQuestion 提供以下选项：Python, TypeScript, Java, Go, Ruby, cURL/raw HTTP, C#, PHP
   - 如果 AskUserQuestion 不可用，默认提供 Python 示例并注明：“正在展示 Python 示例。如果您需要其他语言，请告知我。”

4. **如果检测到不受支持的语言**（Rust, Swift, C++, Elixir 等）：

   - 建议参考 \`curl/\` 中的 cURL/原生 HTTP 示例，并注明可能存在社区维护的 SDK
   - 也可以提议展示 Python 或 TypeScript 示例作为参考实现

5. **如果用户需要 cURL/原生 HTTP 示例**，请从 \`curl/\` 中读取。

### 特定语言特性支持

| 语言 | 工具运行器 (Tool Runner) | Agent SDK | 备注 |
| :--- | :--- | :--- | :--- |
| Python | 支持 (Beta) | 支持 | 全面支持 —— \`@beta_tool\` 装饰器 |
| TypeScript | 支持 (Beta) | 支持 | 全面支持 —— \`betaZodTool\` + Zod |
| Java | 支持 (Beta) | 不支持 | 带注解类的 Beta 版工具使用 |
| Go | 支持 (Beta) | 不支持 | \`toolrunner\` 包中的 \`BetaToolRunner\` |
| Ruby | 支持 (Beta) | 不支持 | Beta 版中的 \`BaseTool\` + \`tool_runner\` |
| cURL | 不适用 | 不适用 | 原生 HTTP，无集成 SDK 特性 |
| C# | 不支持 | 不支持 | 官方 SDK |
| PHP | 不支持 | 不支持 | 官方 SDK |

---

## 我该使用哪个交互层级 (Surface)？

> **从简开始。** 默认选择能满足您需求的最低层级。单次 API 调用和工作流足以处理大多数用例 —— 只有当任务真正需要开放式、由模型驱动的探索时，才考虑使用智能体 (Agents)。

| 用例 | 层级 | 推荐层级 | 原因 |
| :--- | :--- | :--- | :--- |
| 分类、总结、提取、问答 | 单次 LLM 调用 | **Claude API** | 一次请求，一次响应 |
| 批量处理或嵌入 (Embeddings) | 单次 LLM 调用 | **Claude API** | 专用端点 |
| 带代码控制逻辑的多步流水线 | 工作流 (Workflow) | **Claude API + 工具使用** | 由您编排循环 |
| 带有自定义工具的专用智能体 | 智能体 (Agent) | **Claude API + 工具使用** | 最大的灵活性 |
| 具有文件/网页/终端访问能力的 AI 智能体 | 智能体 (Agent) | **Agent SDK** | 内置工具、安全性和 MCP 支持 |
| 智能体化编码助手 | 智能体 (Agent) | **Agent SDK** | 专为此用例设计 |
| 需要内置权限和护栏 | 智能体 (Agent) | **Agent SDK** | 包含安全特性 |

> **注意：** Agent SDK 适用于您希望开箱即用地获得内置文件/网页/终端工具、权限和 MCP 的场景。如果您想构建一个使用自己开发的工具的智能体，Claude API 是正确的选择 —— 请使用工具运行器自动处理循环，或者使用手动循环进行精细化控制（审批环节、自定义日志记录、条件执行）。

### 决策树

\`\`\`
您的应用程序需要什么？

1. 单次 LLM 调用（分类、总结、提取、问答）
   └── Claude API —— 一次请求，一次响应

2. Claude 是否需要在工作中读取/写入文件、浏览网页或运行 Shell 命令？
   （注：不是您的应用读取文件并交给 Claude —— 而是 Claude 本身是否需要
   自主发现并访问文件/网页/Shell？）
   └── 是 → Agent SDK —— 内置工具，无需重复造轮子
       示例：“扫描代码库查找 Bug”、“总结目录中的每个文件”、
             “使用子智能体查找 Bug”、“通过网络搜索研究主题”

3. 工作流（多步骤、代码编排、使用您自己的工具）
   └── Claude API 配合工具使用 —— 您控制循环

4. 开放式智能体（模型决定自己的轨迹，使用您自己的工具）
   └── Claude API 智能体循环（最大灵活性）
\`\`\`

### 我应该构建智能体 (Agent) 吗？

在选择智能体层级之前，请检查以下四个标准：

- **复杂度** —— 任务是否是多步骤的，且难以提前完全规定？（例如：“将此设计文档转化为 PR” vs “从此 PDF 中提取标题”）
- **价值** —— 结果是否值得付出更高的成本和延迟？
- **可行性** —— Claude 是否胜任此类任务？
- **错误成本** —— 错误是否可以被捕获并修复？（测试、审查、回滚）

如果上述任何一项的回答为“否”，请留在更简单的层级（单次调用或工作流）。

---

## 架构

一切都通过 \`POST /v1/messages\` 进行。工具和输出约束是这一单一端点的特性，而非独立的 API。

**用户定义工具** —— 您定义工具（通过装饰器、Zod 架构或原始 JSON），SDK 的工具运行器负责调用 API、执行您的函数并循环执行直至 Claude 完成。如需完全控制，您可以手动编写循环。

**服务端工具** —— 运行在 Anthropic 基础架构上的 Anthropic 托管工具。代码执行完全在服务端完成（在 \`tools\` 中声明，Claude 自动运行代码）。电脑使用 (Computer use) 可以由服务端托管或自托管。

**结构化输出** —— 约束消息 API 的响应格式 (\`output_config.format\`) 且/或进行工具参数验证 (\`strict: true\`)。推荐的方法是使用 \`client.messages.parse()\`，它会根据您的架构自动验证响应。注意：旧的 \`output_format\` 参数已被弃用；请在 \`messages.create()\` 上使用 \`output_config: {format: {...}}\`。

**支持端点** —— 批量处理 (\`POST /v1/messages/batches\`)、文件 (\`POST /v1/files\`) 和 Token 计数端点为消息 API 请求提供数据或支持。

---

## 当前模型 (缓存日期: 2026-02-17)

| 模型 | 模型 ID | 上下文 | 输入 $/1M | 输出 $/1M |
| :--- | :--- | :--- | :--- | :--- |
| Claude Opus 4.6 | \`claude-opus-4-6\` | 200K (1M beta) | $5.00 | $25.00 |
| Claude Sonnet 4.6 | \`claude-sonnet-4-6\` | 200K (1M beta) | $3.00 | $15.00 |
| Claude Haiku 4.5 | \`claude-haiku-4-5\` | 200K | $1.00 | $5.00 |

**除非用户明确指明使用其他模型，否则请务必使用 \`{{OPUS_ID}}\`。** 这是不可商榷的。除非用户字面上说“使用 sonnet”或“使用 haiku”，否则不要使用 \`{{SONNET_ID}}\`, \`{{PREV_SONNET_ID}}\` 或任何其他模型。绝不要为了节省成本而降级模型 —— 那是用户的决定，不是您的决定。

**关键提示：请仅使用上表中的确切模型 ID 字符串 —— 它们本身就是完整的。不要附加日期后缀。** 例如，使用 \`claude-sonnet-4-5\`，绝不要使用 \`claude-sonnet-4-5-20250514\` 或任何您可能从训练数据中记住的其他带日期后缀的变体。如果用户请求表中未列出的旧模型（例如 "opus 4.5", "sonnet 3.7"），请阅读 \`shared/models.md\` 获取确切 ID —— 不要自行构造。

一则说明：如果上述任何模型字符串对您来说看起来很陌生，这是正常的 —— 这意味着它们是在您的训练数据截止日期之后发布的。请放心，它们是真实的模型，我们不会那样捉弄您。

---

## 思考与努力程度 (快速参考)

**Opus 4.6 —— 适应性思考 (推荐使用)：** 使用 \`thinking: {type: "adaptive"}\`。Claude 会动态决定何时思考以及思考多少。无需 \`budget_tokens\` —— \`budget_tokens\` 在 Opus 4.6 和 Sonnet 4.6 上已被弃用，不得使用。适应性思考还会自动开启交替思考 (interleaved thinking，无需 beta 标头)。**当用户要求“扩展思考 (extended thinking)”、“思考预算 (thinking budget)”或 \`budget_tokens\` 时：请始终使用带有 \`thinking: {type: "adaptive"}\` 的 Opus 4.6。固定 Token 预算思考的概念已被弃用 —— 适应性思考取而代之。不要使用 \`budget_tokens\`，也不要切换到旧模型。**

**努力程度 (Effort) 参数 (GA 特性，无需 beta 标头)：** 通过 \`output_config: {effort: "low"|"medium"|"high"|"max"}\` 来控制思考深度和整体 Token 花费（位于 \`output_config\` 内部，而非顶级层级）。默认值为 \`high\`（等同于忽略该参数）。\`max\` 仅适用于 Opus 4.6。该参数适用于 Opus 4.5, Opus 4.6, 以及 Sonnet 4.6。在 Sonnet 4.5 / Haiku 4.5 上会报错。配合适应性思考使用可获得最佳的成本质量权衡。对于子智能体或简单任务请使用 \`low\`；对于最深层的推理请使用 \`max\`。

**Sonnet 4.6：** 支持适应性思考 (\`thinking: {type: "adaptive"}\`)。\`budget_tokens\` 在 Sonnet 4.6 上已被弃用 —— 请改用适应性思考。

**旧版模型（仅在明确要求时使用）：** 如果用户专门要求 Sonnet 4.5 或其他旧模型，请使用 \`thinking: {type: "enabled", budget_tokens: N}\`。\`budget_tokens\` 必须小于 \`max_tokens\`（最小值为 1024）。绝不要仅仅因为用户提到了 \`budget_tokens\` 就选择旧模型 —— 请使用带有适应性思考的 Opus 4.6 代替。

---

## 对话压缩 (Compaction) (快速参考)

**Beta 特性，仅限 Opus 4.6。** 对于可能超过 200K 上下文窗口的长时间对话，请开启服务端对话压缩。API 会在上下文接近触发阈值（默认 150K token）时自动总结早期背景。需要 beta 标头 \`compact-2026-01-12\`。

**关键操作：** 在每一轮对话中，将完整的 \`response.content\`（而不仅仅是文本）追加回您的消息中。响应中的压缩块 (Compaction blocks) 必须被完整保留 —— API 会在下次请求中使用它们来替换被压缩的历史记录。如果仅提取文本字符串并追加，将静默丢失压缩状态。

代码示例请参阅 \`{lang}/claude-api/README.md\` 的“对话压缩 (Compaction)”部分。完整文档可通过 \`shared/live-sources.md\` 中的 WebFetch 获取。

---

## 阅读指南

在检测到语言后，根据用户的需求阅读相关文件：

### 任务快速参考

**单次文本分类/总结/提取/问答：**
→ 仅阅读 \`{lang}/claude-api/README.md\`

**聊天 UI 或实时响应展示：**
→ 阅读 \`{lang}/claude-api/README.md\` + \`{lang}/claude-api/streaming.md\`

**长时间对话（可能超出上下文窗口）：**
→ 阅读 \`{lang}/claude-api/README.md\` —— 见“对话压缩 (Compaction)”部分

**函数调用 / 工具使用 / 智能体 (Agents)：**
→ 阅读 \`{lang}/claude-api/README.md\` + \`shared/tool-use-concepts.md\` + \`{lang}/claude-api/tool-use.md\`

**批量处理（对延迟不敏感）：**
→ 阅读 \`{lang}/claude-api/README.md\` + \`{lang}/claude-api/batches.md\`

**跨多个请求的文件上传：**
→ 阅读 \`{lang}/claude-api/README.md\` + \`{lang}/claude-api/files-api.md\`

**具有内置工具（文件/网页/终端）的智能体：**
→ 阅读 \`{lang}/agent-sdk/README.md\` + \`{lang}/agent-sdk/patterns.md\`

### Claude API (完整文件参考)

阅读**特定语言的 Claude API 文件夹** (\`{language}/claude-api/\`)：

1. **\`{language}/claude-api/README.md\`** —— **请先阅读此文件。** 安装、快速入门、常用模式、错误处理。
2. **\`shared/tool-use-concepts.md\`** —— 当用户需要函数调用、代码执行、记忆或结构化输出时阅读。涵盖概念基础。
3. **\`{language}/claude-api/tool-use.md\`** —— 用于查看特定语言的工具使用代码示例（工具运行器、手动循环、代码执行、记忆、结构化输出）。
4. **\`{language}/claude-api/streaming.md\`** —— 在构建聊天 UI 或渐进式显示响应的界面时阅读。
5. **\`{language}/claude-api/batches.md\`** —— 在脱机处理大量请求（对延迟不敏感）时阅读。异步运行，成本降低 50%。
6. **\`{language}/claude-api/files-api.md\`** —— 在跨多个请求发送同一文件且不重复上传时阅读。
7. **\`shared/error-codes.md\`** —— 在调试 HTTP 错误或实现错误处理时阅读。
8. **\`shared/live-sources.md\`** —— 用于获取最新官方文档的 WebFetch URL。

> **注意：** 对于 Java, Go, Ruby, C#, PHP 和 cURL —— 它们各自只有一个涵盖所有基础知识的文件。请阅读该文件，并根据需要阅读 \`shared/tool-use-concepts.md\` 和 \`shared/error-codes.md\`。

### Agent SDK

阅读**特定语言的 Agent SDK 文件夹** (\`{language}/agent-sdk/\`)。Agent SDK 仅适用于 **Python 和 TypeScript**。

1. **\`{language}/agent-sdk/README.md\`** —— 安装、快速入门、内置工具、权限、MCP、钩子 (Hooks)。
2. **\`{language}/agent-sdk/patterns.md\`** —— 自定义工具、钩子、子智能体、MCP 集成、会话恢复。
3. **\`shared/live-sources.md\`** —— 当前 Agent SDK 文档的 WebFetch URL。

---

## 何时使用 WebFetch

在以下情况下，请使用 WebFetch 获取最新文档：

- 用户要求“最新”或“当前”信息
- 缓存数据看起来不正确
- 用户询问了此处未涵盖的特性

实时文档 URL 位于 \`shared/live-sources.md\` 中。

## 常见陷阱

- 在向 API 传递文件或内容时，不要截断输入。如果内容过长无法放入上下文窗口，请通知用户并讨论方案（分块、总结等），而不是静默截断。
- **Opus 4.6 / Sonnet 4.6 思考：** 使用 \`thinking: {type: "adaptive"}\` —— 不要使用 \`budget_tokens\`（在 Opus 4.6 和 Sonnet 4.6 上均已弃用）。对于旧版模型，\`budget_tokens\` 必须小于 \`max_tokens\`（最小值 1024）。如果弄错了，API 会报错。
- **Opus 4.6 移除了预填 (prefill)：** Assistant 消息预填（上一轮助手的预填）在 Opus 4.6 上会返回 400 错误。请改用结构化输出 (\`output_config.format\`) 或系统提示词指令来控制响应格式。
- **128K 输出 Token：** Opus 4.6 支持最高 128K \`max_tokens\`，但 SDK 要求对大型 \`max_tokens\` 进行流式传输，以避免 HTTP 超时。请配合使用 \`.stream()\` 和 \`.get_final_message()\` / \`.finalMessage()\`。
- **工具调用的 JSON 解析 (Opus 4.6)：** Opus 4.6 可能会在工具调用的 \`input\` 字段中产生不同的 JSON 字符串转义（例如 Unicode 或正斜杠转义）。请始终使用 \`json.loads()\` / \`JSON.parse()\` 解析工具输入 —— 绝不要对序列化后的输入进行原始字符串匹配。
- **结构化输出（适用所有模型）：** 在 \`messages.create()\` 上使用 \`output_config: {format: {...}}\` 代替已弃用的 \`output_format\` 参数。这是通用的 API 变更，并非仅针对 4.6。
- **不要重复实现 SDK 功能：** SDK 提供了高级助手函数 —— 请使用它们而非从头构建。具体而言：使用 \`stream.finalMessage()\` 而非将 \`.on()\` 事件包装在 \`new Promise()\` 中；使用类型化的异常类 (\`Anthropic.RateLimitError\` 等) 而非字符串匹配错误消息；使用 SDK 类型 (\`Anthropic.MessageParam\`, \`Anthropic.Tool\`, \`Anthropic.Message\` 等) 而非重新定义等效接口。
- **不要为 SDK 数据结构定义自定义类型：** SDK 为所有 API 对象导出了类型。使用 \`Anthropic.MessageParam\` 表示消息，\`Anthropic.Tool\` 表示工具定义，\`Anthropic.ToolUseBlock\` / \`Anthropic.ToolResultBlockParam\` 表示工具结果，\`Anthropic.Message\` 表示响应。定义您自己的 \`interface ChatMessage { role: string; content: unknown }\` 会导致与 SDK 提供的功能重复，并丢失类型安全性。
- **报告与文档输出：** 对于生成报告、文档或可视化图表的任务，代码执行沙箱预装了 \`python-docx\`, \`python-pptx\`, \`matplotlib\`, \`pillow\` 和 \`pypdf\`。Claude 可以生成格式化文件 (DOCX, PDF, 图表) 并通过 Files API 返回 —— 对于“报告”或“文档”类型的请求，请考虑使用此方式而非纯 stdout 文本。
