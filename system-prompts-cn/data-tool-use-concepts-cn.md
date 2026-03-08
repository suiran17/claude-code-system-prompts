<!--
name: 'Data: Tool use concepts'
description: Claude API 工具使用的概念基础，包括工具定义、工具选择和最佳实践
ccVersion: 2.1.63
-->
# 工具使用概念 (Tool Use Concepts)

本文件涵盖了 Claude API 工具使用的概念基础。有关特定语言的代码示例，请参阅 \`python/\`、\`typescript/\` 或其他语言文件夹。

## 用户定义工具 (User-Defined Tools)

### 工具定义结构

> **注意：** 当使用工具运行器 (Tool Runner，Beta 版) 时，工具架构 (Schema) 将根据您的函数签名 (Python)、Zod 架构 (TypeScript)、注解类 (Java)、\`jsonschema\` 结构体标签 (Go) 或 \`BaseTool\` 子类 (Ruby) 自动生成。下方的原始 JSON 架构格式适用于手动方法或尚不支持工具运行器的 SDK。

每个工具都需要一个名称 (name)、描述 (description) 以及输入项的 JSON 架构 (JSON Schema)：

\`\`\`json
{
  "name": "get_weather",
  "description": "获取给定位置的当前天气",
  "input_schema": {
    "type": "object",
    "properties": {
      "location": {
        "type": "string",
        "description": "城市和州，例如 San Francisco, CA"
      },
      "unit": {
        "type": "string",
        "enum": ["celsius", "fahrenheit"],
        "description": "温度单位"
      }
    },
    "required": ["location"]
  }
}
\`\`\`

**工具定义最佳实践：**

- 使用清晰且具有描述性的名称（例如 \`get_weather\`, \`search_database\`, \`send_email\`）
- 编写详细的描述 —— Claude 根据这些描述来决定何时使用该工具
- 为每个属性编写描述信息
- 对于具有固定取值范围的参数，使用 \`enum\`
- 在 \`required\` 中标记真正必需的参数；将其他参数设为带有默认值的可选参数

---

### 工具选择选项 (Tool Choice Options)

控制 Claude 何时使用工具：

| 值                                | 行为                                          |
| --------------------------------- | --------------------------------------------- |
| \`{"type": "auto"}\`                | Claude 自行决定是否使用工具（默认）           |
| \`{"type": "any"}\`                 | Claude 必须使用至少一个工具                   |
| \`{"type": "tool", "name": "..."}\` | Claude 必须使用指定的工具                     |
| \`{"type": "none"}\`                | Claude 不允许使用工具                         |

任何 \`tool_choice\` 值都可以包含 \`"disable_parallel_tool_use": true\`，以强制 Claude 在单次响应中最多仅使用一个工具。默认情况下，Claude 可能会在单次响应中请求多次工具调用。

---

### 工具运行器 vs 手动循环

**工具运行器 (Tool Runner，推荐)：** SDK 的工具运行器会自动处理代理循环 (Agentic loop) —— 它负责调用 API、检测工具使用请求、执行您的工具函数、将结果反馈给 Claude，并不断重复直到 Claude 停止调用工具。适用于 Python, TypeScript, Java, Go, 和 Ruby SDK (Beta)。

**手动代理循环：** 当您需要对循环进行精细化控制时使用（例如自定义日志记录、有条件的工具执行、需要人工介入审批等）。循环持续进行直到 \`stop_reason == "end_turn"\`。务必追加完整的 \`response.content\` 以保留 \`tool_use\` 块，并确保每个 \`tool_result\` 都包含匹配的 \`tool_use_id\`。

**服务端工具的停止原因：** 当使用服务端工具（代码执行、网页搜索等）时，API 会运行一个服务端采样循环。如果该循环达到默认的 10 次迭代限制，响应将返回 \`stop_reason: "pause_turn"\`。若要继续，请重新发送用户消息和助手响应并再次发起 API 请求 —— 服务器将从中断处继续。请“不要”添加额外的用户消息（如“继续”）。API 会检测末尾的 \`server_tool_use\` 块并自动恢复。

\`\`\`python
# 在您的代理循环中处理 pause_turn
if response.stop_reason == "pause_turn":
    messages = [
        {"role": "user", "content": user_query},
        {"role": "assistant", "content": response.content},
    ]
    # 再次发起 API 请求 —— 服务器会自动恢复
    response = client.messages.create(
        model="{{OPUS_ID}}", messages=messages, tools=tools
    )
\`\`\`

请设置 \`max_continuations\` 限制（例如 5）以防止无限循环。完整指南请参阅：\`https://platform.claude.com/docs/en/build-with-claude/handling-stop-reasons\`

> **安全警告：** 工具运行器会在 Claude 请求时自动执行您的工具函数。对于具有副作用的工具（发送电子邮件、修改数据库、金融交易），请在工具函数内部验证输入，并考虑对破坏性操作要求二次确认。如果您在每次工具执行前需要进行人工审批，请使用“手动代理循环”。

---

### 处理工具结果

当 Claude 使用工具时，响应会包含 \`tool_use\` 块。您必须：

1. 使用提供的输入执行工具
2. 将结果放回 \`tool_result\` 消息中并发送
3. 继续对话

**工具结果中的错误处理：** 如果工具执行失败，请设置 \`"is_error": true\` 并提供具有参考价值的错误消息。Claude 通常会承认错误并尝试不同的方案或要求澄清。

**多次工具调用：** Claude 可能会在单次响应中请求多个工具调用。请先处理完所有请求再继续 —— 将所有结果放进同一条 \`user\` 消息中返回。

---

## 服务端工具：代码执行 (Code Execution)

代码执行工具让 Claude 在一个安全的沙盒容器中运行代码。与用户定义工具不同，服务端工具运行在 Anthropic 的基础设施上 —— 您无需在客户端执行任何操作。只需包含工具定义，Claude 即可处理其余部分。

### 核心事实

- 运行在隔离的容器中（1 核 CPU, 5 GiB RAM, 5 GiB 磁盘）
- 无互联网访问（完全沙盒化）
- 预装了 Python 3.11 及数据科学相关库
- 容器持久保存 30 天，可跨请求复用
- 与网页搜索/网页获取工具搭配使用时免费；否则在每组织每月 1,550 小时免费额度后，按 $0.05/小时计费

### 工具定义

该工具无需架构 (Schema) —— 只需在 \`tools\` 数组中声明即可：

\`\`\`json
{
  "type": "code_execution_20260120",
  "name": "code_execution"
}
\`\`\`

Claude 将自动获得 \`bash_code_execution\`（运行 shell 命令）和 \`text_editor_code_execution\`（创建/查看/编辑文件）的访问权限。

### 预装的 Python 库

- **数据科学**: pandas, numpy, scipy, scikit-learn, statsmodels
- **可视化**: matplotlib, seaborn
- **文件处理**: openpyxl, xlsxwriter, pillow, pypdf, pdfplumber, python-docx, python-pptx
- **数学**: sympy, mpmath
- **实用工具**: tqdm, python-dateutil, pytz, sqlite3

其他包可以在运行时通过 \`pip install\` 安装。

### 支持上传的文件类型

| 类型   | 扩展名                             |
| ------ | ---------------------------------- |
| 数据   | CSV, Excel (.xlsx/.xls), JSON, XML |
| 图像   | JPEG, PNG, GIF, WebP               |
| 文本   | .txt, .md, .py, .js 等             |

### 容器复用

跨请求复用容器以保持状态（文件、已安装的包、变量）。从第一个响应中提取 \`container_id\` 并将其传递给后续请求。

### 响应结构

响应包含交织的文本和工具结果块：

- \`text\` — Claude 的解释说明
- \`server_tool_use\` — Claude 正在执行的操作
- \`bash_code_execution_tool_result\` — 代码执行输出（检查 \`return_code\` 以确认成功/失败）
- \`text_editor_code_execution_tool_result\` — 文件操作结果

> **安全提示：** 在将下载的文件写入磁盘之前，务必使用 \`os.path.basename()\` / \`path.basename()\` 对文件名进行过滤，以防止路径穿越攻击。请将文件写入专用的输出目录。

---

## 服务端工具：网页搜索与网页获取 (Web Search & Web Fetch)

网页搜索和网页获取让 Claude 能够搜索网页并获取页面内容。它们在服务端运行 —— 只需包含工具定义，Claude 即可自动处理查询、抓取和结果处理。

### 工具定义

\`\`\`json
[
  { "type": "web_search_20260209", "name": "web_search" },
  { "type": "web_fetch_20260209", "name": "web_fetch" }
]
\`\`\`

### 动态过滤 (Opus 4.6 / Sonnet 4.6)

\`web_search_20260209\` 和 \`web_fetch_20260209\` 版本支持 **动态过滤 (Dynamic filtering)** —— Claude 会编写并执行代码，在搜索结果进入上下文窗口之前对其进行过滤，从而提高准确性和 Token 效率。动态过滤已内置于这些工具版本中并自动激活；您无需单独声明 \`code_execution\` 工具，也不需要传递任何 Beta 版标头。

\`\`\`json
{
  "tools": [
    { "type": "web_search_20260209", "name": "web_search" },
    { "type": "web_fetch_20260209", "name": "web_fetch" }
  ]
}
\`\`\`

如果不使用动态过滤，之前的 \`web_search_20250305\` 版本仍然可用。

> **注意：** 只有当您的应用程序为了独立于网页搜索的目的（如数据分析、文件处理、可视化）需要代码执行时，才包含独立的 \`code_execution\` 工具。将其与 \`_20260209\` 系列网页工具并列使用会产生第二个执行环境，可能会令模型产生困惑。

---

## 服务端工具：编程式工具调用 (Programmatic Tool Calling)

编程式工具调用让 Claude 能在代码中执行复杂的多工具工作流，从而使中间结果留在上下文窗口之外。Claude 直接编写调用您工具的代码，减少多步骤操作的 Token 消耗。

有关完整文档，请使用 WebFetch 访问：

- URL: \`https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling\`

---

## 服务端工具：工具搜索 (Tool Search)

工具搜索工具让 Claude 能够从大型工具库中动态发现工具，而无需将所有定义都加载到上下文窗口中。当您有很多工具但具体请求仅需其中少数几个时非常有用。

有关完整文档，请使用 WebFetch 访问：

- URL: \`https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool\`

---

## 工具示例 (Tool Use Examples)

您可以直接在工具定义中提供示例调用，以演示使用模式并减少参数错误。这有助于 Claude 理解如何正确格式化工具输入，特别是对于架构复杂的工具。

有关完整文档，请使用 WebFetch 访问：

- URL: \`https://platform.claude.com/docs/en/agents-and-tools/tool-use/implement-tool-use\`

---

## 服务端工具：电脑使用 (Computer Use)

电脑使用让 Claude 能够与桌面环境交互（截图、鼠标、键盘）。它可以是由 Anthropic 托管的（服务端，类似于代码执行）或者是自托管的（您提供环境并在客户端执行操作）。

有关完整文档，请使用 WebFetch 访问：

- URL: \`https://platform.claude.com/docs/en/agents-and-tools/computer-use/overview\`

---

## 客户端工具：记忆 (Memory)

记忆工具使 Claude 能够通过一个记忆文件目录在对话之间存储和获取信息。Claude 可以创建、读取、更新和删除在会话间持久存在的文件。

### 核心事实

- 客户端工具 —— 您通过自己的实现来控制存储。
- 支持的命令：\`view\`, \`create\`, \`str_replace\`, \`insert\`, \`delete\`, \`rename\`
- 在 \`/memories\` 目录中的文件上操作。
- SDK 提供了实现记忆后端的辅助类/函数。

> **安全提示：** 绝不要在记忆文件中存储 API 密钥、密码、令牌或其他秘密信息。谨慎处理个人身份信息 (PII) —— 在持久化用户数据前请检查数据隐私法规（如 GDPR, CCPA）。参考实现没有内置的访问控制；在多用户系统中，请在工具处理器中实现针对每个用户的记忆目录和身份验证。

有关完整的实现示例，请使用 WebFetch 访问：

- 文档: \`https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool.md\`

---

## 结构化输出 (Structured Outputs)

结构化输出强制 Claude 的回复遵循特定的 JSON 架构 (JSON Schema)，保证输出是有效且可解析的。这并不是一个单独的工具 —— 它增强了 Messages API 的响应格式和/或工具参数验证。

提供了两个特性：

- **JSON 输出** (\`output_config.format\`): 控制 Claude 的响应格式
- **严格工具使用** (\`strict: true\`): 保证工具参数符合架构要求

**支持的模型：** {{OPUS_NAME}}, {{SONNET_NAME}}, 和 {{HAIKU_NAME}}。旧模型（Claude Opus 4.5, Claude Opus 4.1）也支持结构化输出。

> **推荐做法：** 使用 \`client.messages.parse()\`，它会自动根据您的架构验证响应。当直接使用 \`messages.create()\` 时，请设置 \`output_config: {format: {...}}\`。\`.parse()\` 等部分 SDK 方法也接受 \`output_format\` 便捷参数，但 \`output_config.format\` 是规范的 API 级参数。

### JSON 架构限制

**支持项：**

- 基础类型：object, array, string, integer, number, boolean, null
- \`enum\`, \`const\`, \`anyOf\`, \`allOf\`, \`$ref\`/\`$def\`
- 字符串格式：\`date-time\`, \`time\`, \`date\`, \`duration\`, \`email\`, \`hostname\`, \`uri\`, \`ipv4\`, \`ipv6\`, \`uuid\`
- \`additionalProperties: false\`（所有对象均必需）

**不支持项：**

- 递归架构
- 数值约束（\`minimum\`, \`maximum\`, \`multipleOf\`）
- 字符串约束（\`minLength\`, \`maxLength\`）
- 复杂的数组约束
- 将 \`additionalProperties\` 设置为 \`false\` 以外的其他值

Python 和 TypeScript SDK 会通过从发送给 API 的架构中移除不支持的约束，并在客户端进行验证来自动处理这些限制。

### 重要注意事项

- **首次请求延迟**：新架构会产生一次性的编译成本。后续具有相同架构的请求将使用 24 小时的缓存。
- **拒绝 (Refusals)**：如果 Claude 出于安全原因拒绝回复 (\`stop_reason: "refusal"\`)，输出可能不符合您的架构。
- **Token 限制**：如果 \`stop_reason: "max_tokens"\`，输出可能不完整。请增加 \`max_tokens\` 值。
- **不兼容项**：引用功能 (Citations) —— 返回 400 错误；消息预填充 (Message prefilling)。
- **兼容项**：批量任务 API (Batches API)、流式传输、Token 计数、扩展思考。

---

## 高效使用工具的建议

1. **提供详细描述**：Claude 极度依赖描述来理解何时以及如何使用工具。
2. **使用具体的工具名称**：\`get_current_weather\` 比 \`weather\` 更好。
3. **验证输入**：务必在执行前验证工具输入。
4. **优雅处理错误**：返回具有参考价值的错误消息，以便 Claude 进行调整。
5. **限制工具数量**：工具过多可能会令模型产生困惑 —— 保持工具集精炼且专注。
6. **测试工具交互**：在各种场景下验证 Claude 是否能正确使用工具。

有关工具使用的详细文档，请使用 WebFetch 访问：

- URL: \`https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview\`
