<!--
name: 'Data: Streaming reference — TypeScript'
description: TypeScript 流式传输参考，包括基础流式传输和处理不同内容类型
ccVersion: 2.1.63
-->
# 流式传输 (Streaming) — TypeScript

## 快速入门

\`\`\`typescript
const stream = client.messages.stream({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  messages: [{ role: "user", content: "写一个故事" }],
});

for await (const event of stream) {
  if (
    event.type === "content_block_delta" &&
    event.delta.type === "text_delta"
  ) {
    process.stdout.write(event.delta.text);
  }
}
\`\`\`

---

## 处理不同内容类型

> **Opus 4.6：** 使用 \`thinking: {type: "adaptive"}\`。在旧模型上，请转而使用 \`thinking: {type: "enabled", budget_tokens: N}\`。

\`\`\`typescript
const stream = client.messages.stream({
  model: "{{OPUS_ID}}",
  max_tokens: 16000,
  thinking: { type: "adaptive" },
  messages: [{ role: "user", content: "分析此问题" }],
});

for await (const event of stream) {
  switch (event.type) {
    case "content_block_start":
      switch (event.content_block.type) {
        case "thinking":
          console.log("\\n[思考中...]");
          break;
        case "text":
          console.log("\\n[回复:]");
          break;
      }
      break;
    case "content_block_delta":
      switch (event.delta.type) {
        case "thinking_delta":
          process.stdout.write(event.delta.thinking);
          break;
        case "text_delta":
          process.stdout.write(event.delta.text);
          break;
      }
      break;
  }
}
\`\`\`

---

## 带工具使用的流式传输 (工具运行器)

使用工具运行器并将 \`stream: true\`。外层循环迭代工具运行器的迭代次数（消息），内层循环处理流事件：

\`\`\`typescript
import Anthropic from "@anthropic-ai/sdk";
import { betaZodTool } from "@anthropic-ai/sdk/helpers/beta/zod";
import { z } from "zod";

const client = new Anthropic();

const getWeather = betaZodTool({
  name: "get_weather",
  description: "获取给定位置的当前天气",
  inputSchema: z.object({
    location: z.string().describe("城市和州，例如 San Francisco, CA"),
  }),
  run: async ({ location }) => \`在 \${location} 天气晴朗，72°F\`,
});

const runner = client.beta.messages.toolRunner({
  model: "{{OPUS_ID}}",
  max_tokens: 4096,
  tools: [getWeather],
  messages: [
    { role: "user", content: "巴黎和伦敦的天气怎么样？" },
  ],
  stream: true,
});

// 外层循环：工具运行器的每次迭代
for await (const messageStream of runner) {
  // 内层循环：本次迭代的流事件
  for await (const event of messageStream) {
    switch (event.type) {
      case "content_block_delta":
        switch (event.delta.type) {
          case "text_delta":
            process.stdout.write(event.delta.text);
            break;
          case "input_json_delta":
            // 正在流式传输的工具输入
            break;
        }
        break;
    }
  }
}
\`\`\`

---

## 获取最终消息

\`\`\`typescript
const stream = client.messages.stream({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  messages: [{ role: "user", content: "你好" }],
});

for await (const event of stream) {
  // 处理事件...
}

const finalMessage = await stream.finalMessage();
console.log(\`使用的 Token 数: \${finalMessage.usage.output_tokens}\`);
\`\`\`

---

## 流事件类型 (Stream Event Types)

| 事件类型              | 描述                       | 触发时机                           |
| --------------------- | -------------------------- | ---------------------------------- |
| \`message_start\`       | 包含消息元数据             | 开始时触发一次                     |
| \`content_block_start\` | 新内容块开始               | 当文本/tool_use 块开始时           |
| \`content_block_delta\` | 内容增量更新               | 针对每个 token/chunk 触发          |
| \`content_block_stop\`  | 内容块完成                 | 当一个块结束时                     |
| \`message_delta\`       | 消息级更新                 | 包含 \`stop_reason\`、Token 使用量 |
| \`message_stop\`        | 消息完成                   | 结束时触发一次                     |

## 最佳实践

1. **务必刷新输出** —— 使用 \`process.stdout.write()\` 进行即时显示
2. **处理部分响应** —— 如果流被中断，您可能会得到不完整的内容
3. **追踪 Token 使用情况** —— \`message_delta\` 事件包含使用信息
4. **使用 \`finalMessage()\`** —— 即使在流式传输时也能获取完整的 \`Anthropic.Message\` 对象。不要将 \`.on()\` 事件包装在 \`new Promise()\` 中 —— \`finalMessage()\` 会在内部处理所有的完成/错误/中止状态
5. **为 Web UI 设置缓冲区** —— 考虑在渲染前缓存一些 token，以避免过多的 DOM 更新
6. **使用 \`stream.on("text", ...)\` 获取增量** —— \`text\` 事件直接提供增量字符串，比手动过滤 \`content_block_delta\` 事件更简单
7. **用于带流式传输的代理循环** —— 请参阅 tool-use.md 中的 [流式传输手动循环](./tool-use.md#streaming-manual-loop) 部分，了解如何向工具调用循环结合 \`stream()\` + \`finalMessage()\`

## 原始 SSE 格式

如果不使用 SDK 而直接使用原始 HTTP，流将返回服务器发送事件 (Server-Sent Events)：

\`\`\`
event: message_start
data: {"type":"message_start","message":{"id":"msg_...","type":"message",...}}

event: content_block_start
data: {"type":"content_block_start","index":0,"content_block":{"type":"text","text":""}}

event: content_block_delta
data: {"type":"content_block_delta","index":0,"delta":{"type":"text_delta","text":"Hello"}}

event: content_block_stop
data: {"type":"content_block_stop","index":0}

event: message_delta
data: {"type":"message_delta","delta":{"stop_reason":"end_turn"},"usage":{"output_tokens":12}}

event: message_stop
data: {"type":"message_stop"}
\`\`\`
