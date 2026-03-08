<!--
name: 'Data: HTTP error codes reference'
description: Claude API 返回的 HTTP 错误代码参考，包括常见原因和处理策略
ccVersion: 2.1.63
-->
# HTTP 错误代码参考 (HTTP Error Codes Reference)

本文件记录了 Claude API 返回的 HTTP 错误代码、其常见原因以及处理方法。有关特定语言的错误处理示例，请参阅 \`python/\` 或 \`typescript/\` 文件夹。

## 错误代码摘要

| 代码 | 错误类型                | 可重试 | 常见原因                             |
| ---- | ----------------------- | ------ | ------------------------------------ |
| 400  | \`invalid_request_error\` | 否     | 无效的请求格式或参数                 |
| 401  | \`authentication_error\`  | 否     | API 密钥无效或缺失                   |
| 403  | \`permission_error\`      | 否     | API 密钥缺少权限                     |
| 404  | \`not_found_error\`       | 否     | 无效的端点或模型 ID                  |
| 413  | \`request_too_large\`     | 否     | 请求超过了大小限制                   |
| 429  | \`rate_limit_error\`      | 是     | 请求过于频繁                         |
| 500  | \`api_error\`             | 是     | Anthropic 服务端问题                 |
| 529  | \`overloaded_error\`      | 是     | API 暂时过载                         |

## 详细错误信息

### 400 Bad Request (坏请求)

**常见原因：**

- 请求体中的 JSON 格式错误
- 缺少必需参数 (\`model\`, \`max_tokens\`, \`messages\`)
- 参数类型无效（例如，要求整数处提供了字符串）
- messages 数组为空
- 消息未按 user/assistant 交替排列

**错误示例：**

\`\`\`json
{
  "type": "error",
  "error": {
    "type": "invalid_request_error",
    "message": "messages: roles must alternate between \"user\" and \"assistant\""
  }
}
\`\`\`

**修复方案：** 在发送前验证请求结构。检查以下各项：

- \`model\` 是有效的模型 ID
- \`max_tokens\` 是正整数
- \`messages\` 数组非空且正确交替排列

---

### 401 Unauthorized (未授权)

**常见原因：**

- 缺少 \`x-api-key\` 标头或 \`Authorization\` 标头
- API 密钥格式无效
- API 密钥已被撤销或删除

**修复方案：** 确保 \`ANTHROPIC_API_KEY\` 环境变量设置正确。

---

### 403 Forbidden (禁止访问)

**常见原因：**

- API 密钥无权访问所请求的模型
- 组织级别的限制
- 尝试在无 Beta 版权限的情况下访问 Beta 版功能

**修复方案：** 在控制台 (Console) 中检查您的 API 密钥权限。您可能需要更换 API 密钥或申请访问特定功能。

---

### 404 Not Found (未找到)

**常见原因：**

- 模型 ID 拼写错误（例如，应为 \`claude-sonnet-4-6\`，却写成了 \`claude-sonnet-4.6\`）
- 使用了已弃用的模型 ID
- API 端点无效

**修复方案：** 使用模型文档中准确的模型 ID。您可以使用别名（例如 \`{{OPUS_ID}}\`）。

---

### 413 Request Too Large (请求过大)

**常见原因：**

- 请求体超过了最大限制
- 输入中的 Token 数过多
- 图像数据过大

**修复方案：** 减小输入大小 —— 截断对话历史、压缩/调整图像大小，或将大型文档拆分为块。

---

### 400 验证错误 (Validation Errors)

部分 400 错误与参数验证专门相关：

- \`max_tokens\` 超过了模型的限制
- \`temperature\` 值无效（必须在 0.0-1.0 之间）
- 扩展思考中 \`budget_tokens\` >= \`max_tokens\`
- 工具定义架构 (schema) 无效

**扩展思考中的常见错误：**

\`\`\`
# 错误：budget_tokens 必须小于 max_tokens
thinking: budget_tokens=10000, max_tokens=1000  → 错误！

# 正确
thinking: budget_tokens=10000, max_tokens=16000
\`\`\`

---

### 429 Rate Limited (频率限制)

**常见原因：**

- 超过了每分钟请求数 (RPM)
- 超过了每分钟 Token 数 (TPM)
- 超过了每天 Token 数 (TPD)

**需要检查的标头：**

- \`retry-after\`：重试前需等待的秒数
- \`x-ratelimit-limit-*\`：您的限额
- \`x-ratelimit-remaining-*\`：剩余配额

**修复方案：** Anthropic SDK 会对 429 和 5xx 错误自动应用指数退避重试（默认：\`max_retries=2\`）。有关自定义重试行为，请参阅语言特定的错误处理示例。

---

### 500 Internal Server Error (服务器内部错误)

**常见原因：**

- Anthropic 服务端的临时问题
- API 处理中的 Bug

**修复方案：** 采用指数退避重试。如果问题持续存在，请检查 [status.anthropic.com](https://status.anthropic.com)。

---

### 529 Overloaded (过载)

**常见原因：**

- API 请求量巨大
- 服务容量达到上限

**修复方案：** 采用指数退避重试。考虑使用不同的模型（Haiku 通常负载较低）、将请求在时间上分散，或实现请求队列。

---

## 常见错误与修复方案

| 错误行为 | 错误代码 | 修复方案 |
| --- | --- | --- |
| \`budget_tokens\` >= \`max_tokens\` | 400 | 确保 \`budget_tokens\` < \`max_tokens\` |
| 模型 ID 拼写错误 | 404 | 使用有效的模型 ID，如 \`{{OPUS_ID}}\` |
| 第一条消息是 \`assistant\` | 400 | 第一条消息必须是 \`user\` |
| 连续两条消息角色相同 | 400 | 交替使用 \`user\` 和 \`assistant\` |
| 代码中硬编码 API 密钥 | 401 (密钥泄露) | 使用环境变量 |
| 自定义重试需求 | 429/5xx | SDK 会自动重试；可通过 \`max_retries\` 自定义 |

## SDK 中的类型化异常

**务必使用 SDK 的类型化异常类**，而不是通过字符串匹配检查错误消息。每个 HTTP 错误代码都映射到一个特定的异常类：

| HTTP 代码 | TypeScript 类 | Python 类 |
| --- | --- | --- |
| 400 | \`Anthropic.BadRequestError\` | \`anthropic.BadRequestError\` |
| 401 | \`Anthropic.AuthenticationError\` | \`anthropic.AuthenticationError\` |
| 403 | \`Anthropic.PermissionDeniedError\` | \`anthropic.PermissionDeniedError\` |
| 404 | \`Anthropic.NotFoundError\` | \`anthropic.NotFoundError\` |
| 429 | \`Anthropic.RateLimitError\` | \`anthropic.RateLimitError\` |
| 500+ | \`Anthropic.InternalServerError\` | \`anthropic.InternalServerError\` |
| 任意 | \`Anthropic.APIError\` | \`anthropic.APIError\` |

\`\`\`typescript
// ✅ 正确：使用类型化异常
try {
  const response = await client.messages.create({...});
} catch (error) {
  if (error instanceof Anthropic.RateLimitError) {
    // 处理频率限制
  } else if (error instanceof Anthropic.APIError) {
    console.error(\`API 错误 \${error.status}:\`, error.message);
  }
}

// ❌ 错误：不要通过字符串匹配检查错误消息
try {
  const response = await client.messages.create({...});
} catch (error) {
  const msg = error instanceof Error ? error.message : String(error);
  if (msg.includes("429") || msg.includes("rate_limit")) { ... }
}
\`\`\`

所有异常类都继承自 \`Anthropic.APIError\`，它具有 \`status\` 属性。使用 \`instanceof\` 检查时，请按照从最具体到最泛化的顺序（例如，在检查 \`APIError\` 之前先检查 \`RateLimitError\`）。
