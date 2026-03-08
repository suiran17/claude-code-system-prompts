<!--
name: 'Data: Claude API reference — TypeScript'
description: TypeScript SDK 参考，包括安装、客户端初始化、基础请求、思考和多轮对话
ccVersion: 2.1.63
-->
# Claude API — TypeScript

## 安装

\`\`\`bash
npm install @anthropic-ai/sdk
\`\`\`

## 客户端初始化

\`\`\`typescript
import Anthropic from "@anthropic-ai/sdk";

// 默认方式（使用 ANTHROPIC_API_KEY 环境变量）
const client = new Anthropic();

// 显式指定 API 密钥
const client = new Anthropic({ apiKey: "your-api-key" });
\`\`\`

---

## 基础消息请求

\`\`\`typescript
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  messages: [{ role: "user", content: "法国的首都是哪里？" }],
});
console.log(response.content[0].text);
\`\`\`

---

## 系统提示词 (System Prompts)

\`\`\`typescript
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  system:
    "你是一位有用的编码助手。务必用 Python 提供示例。",
  messages: [{ role: "user", content: "如何读取 JSON 文件？" }],
});
\`\`\`

---

## 视觉（图像识别）

### URL 方式

\`\`\`typescript
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  messages: [
    {
      role: "user",
      content: [
        {
          type: "image",
          source: { type: "url", url: "https://example.com/image.png" },
        },
        { type: "text", text: "描述这张图片" },
      ],
    },
  ],
});
\`\`\`

### Base64 方式

\`\`\`typescript
import fs from "fs";

const imageData = fs.readFileSync("image.png").toString("base64");

const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  messages: [
    {
      role: "user",
      content: [
        {
          type: "image",
          source: { type: "base64", media_type: "image/png", data: imageData },
        },
        { type: "text", text: "这张图片里有什么？" },
      ],
    },
  ],
});
\`\`\`

---

## Prompt 缓存 (Prompt Caching)

### 自动缓存（推荐）

使用顶级的 \`cache_control\` 自动缓存请求中的最后一个可缓存块：

\`\`\`typescript
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  cache_control: { type: "ephemeral" }, // 自动缓存最后一个可缓存块
  system: "你是一份大型文档的专家...",
  messages: [{ role: "user", content: "总结关键点" }],
});
\`\`\`

### 手动缓存控制

如需精细控制，可在特定内容块中添加 \`cache_control\`：

\`\`\`typescript
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  system: [
    {
      type: "text",
      text: "你是一份大型文档的专家...",
      cache_control: { type: "ephemeral" }, // 默认 TTL 为 5 分钟
    },
  ],
  messages: [{ role: "user", content: "总结关键点" }],
});

// 使用显式 TTL (生存时间)
const response2 = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  system: [
    {
      type: "text",
      text: "你是一份大型文档的专家...",
      cache_control: { type: "ephemeral", ttl: "1h" }, // 1 小时 TTL
    },
  ],
  messages: [{ role: "user", content: "总结关键点" }],
});
\`\`\`

---

## 扩展思考 (Extended Thinking)

> **Opus 4.6 和 Sonnet 4.6：** 使用适应性思考 (Adaptive thinking)。\`budget_tokens\` 在 Opus 4.6 和 Sonnet 4.6 上均已弃用。
> **旧模型：** 使用 \`thinking: {type: "enabled", budget_tokens: N}\`（必须小于 \`max_tokens\`，最小值为 1024）。

\`\`\`typescript
// Opus 4.6: 适应性思考（推荐）
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 16000,
  thinking: { type: "adaptive" },
  output_config: { effort: "high" }, // low | medium | high | max
  messages: [
    { role: "user", content: "逐步解决这个数学问题..." },
  ],
});

for (const block of response.content) {
  if (block.type === "thinking") {
    console.log("思考中:", block.thinking);
  } else if (block.type === "text") {
    console.log("回复:", block.text);
  }
}
\`\`\`

---

## 错误处理

使用 SDK 的类型化异常类 —— 绝不要通过字符串匹配来检查错误消息：

\`\`\`typescript
import Anthropic from "@anthropic-ai/sdk";

try {
  const response = await client.messages.create({...});
} catch (error) {
  if (error instanceof Anthropic.BadRequestError) {
    console.error("坏请求:", error.message);
  } else if (error instanceof Anthropic.AuthenticationError) {
    console.error("无效的 API 密钥");
  } else if (error instanceof Anthropic.RateLimitError) {
    console.error("触发频率限制 - 请稍后再试");
  } else if (error instanceof Anthropic.APIError) {
    console.error(\`API 错误 \${error.status}:\`, error.message);
  }
}
\`\`\`

所有异常类都继承自 \`Anthropic.APIError\`，并带有一个类型化的 \`status\` 字段。请按照从最具体到最泛化的顺序进行检查。完整的错误代码参考请参阅 [shared/error-codes.md](../../shared/error-codes.md)。

---

## 多轮对话 (Multi-Turn Conversations)

API 是无状态的 —— 每次连接时都需发送完整的对话历史。使用 \`Anthropic.MessageParam[]\` 为消息数组定义类型：

\`\`\`typescript
const messages: Anthropic.MessageParam[] = [
  { role: "user", content: "我的名字是 Alice。" },
  { role: "assistant", content: "你好 Alice！很高兴见到你。" },
  { role: "user", content: "我的名字是什么？" },
];

const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  messages: messages,
});
\`\`\`

**规则：**

- 消息必须在 \`user\` 和 \`assistant\` 之间交替进行
- 第一条消息必须是 \`user\`
- 对所有 API 数据结构使用 SDK 类型 (\`Anthropic.MessageParam\`, \`Anthropic.Message\`, \`Anthropic.Tool\` 等) —— 不要重新定义等效的接口

---

### 对话压缩（长对话）

> **Beta 功能，仅限 Opus 4.6。** 当对话接近 200K 上下文窗口时，压缩功能会自动在服务端总结早期上下文。API 会返回一个 \`compaction\` 块；您必须在后续请求中将其传回 —— 请追加 \`response.content\`，而不仅仅是文本。

\`\`\`typescript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();
const messages: Anthropic.Beta.BetaMessageParam[] = [];

async function chat(userMessage: string): Promise<string> {
  messages.push({ role: "user", content: userMessage });

  const response = await client.beta.messages.create({
    betas: ["compact-2026-01-12"],
    model: "{{OPUS_ID}}",
    max_tokens: 4096,
    messages,
    context_management: {
      edits: [{ type: "compact_20260112" }],
    },
  });

  // 追加完整内容 —— 必须保留压缩块 (Compaction blocks)
  messages.push({ role: "assistant", content: response.content });

  const textBlock = response.content.find((block) => block.type === "text");
  return textBlock?.text ?? "";
}

// 当上下文变长时自动触发压缩
console.log(await chat("帮我构建一个 Python 网页爬虫"));
console.log(await chat("添加对 JavaScript 渲染页面的支持"));
console.log(await chat("现在添加频率限制和错误处理"));
\`\`\`

---

## 停止原因 (Stop Reasons)

回复中的 \`stop_reason\` 字段指明了模型停止生成的原因：

| 值              | 意义                                                            |
| --------------- | --------------------------------------------------------------- |
| \`end_turn\`      | Claude 自然地完成了其回复                                       |
| \`max_tokens\`    | 达到了 \`max_tokens\` 限制 —— 请增加限制或使用流式传输          |
| \`stop_sequence\` | 达到了自定义停止序列                                            |
| \`tool_use\`      | Claude 想要调用工具 —— 请执行该工具并继续                       |
| \`pause_turn\`    | 模型暂停并可以恢复（智能体工作流）                                |
| \`refusal\`       | Claude 出于安全原因拒绝 —— 输出可能不符合架构 (Schema)要求     |

---

## 成本优化策略

### 1. 为重复内容使用 Prompt 缓存

\`\`\`typescript
// 自动缓存（最简便 —— 缓存最后一个可缓存块）
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  cache_control: { type: "ephemeral" },
  system: largeDocumentText, // 例如 50KB 的上下文
  messages: [{ role: "user", content: "总结关键点" }],
});

// 第一次请求：全额支出
// 后续请求：已缓存部分约可节省 90% 的成本
\`\`\`

### 2. 请求前进行 Token 计数

\`\`\`typescript
const countResponse = await client.messages.countTokens({
  model: "{{OPUS_ID}}",
  messages: messages,
  system: system,
});

const estimatedInputCost = countResponse.input_tokens * 0.000005; // $5/100万 token
console.log(\`预计输入成本: $\${estimatedInputCost.toFixed(4)}\`);
\`\`\`
