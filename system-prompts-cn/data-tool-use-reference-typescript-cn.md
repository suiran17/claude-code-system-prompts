<!--
name: 'Data: Tool use reference — TypeScript'
description: TypeScript 工具使用参考，包括工具运行器、手动代理循环、代码执行和结构化输出
ccVersion: 2.1.63
-->
# 工具使用 (Tool Use) — TypeScript

有关概念性概览（工具定义、工具选择、建议提示），请参阅 [shared/tool-use-concepts.md](../../shared/tool-use-concepts.md)。

## 工具运行器 (Tool Runner，推荐使用)

**Beta：** 工具运行器在 TypeScript SDK 中处于 Beta 测试阶段。

要使用 Zod 架构 (Zod schemas) 定义带有 \`run\` 函数的工具，请使用 \`betaZodTool\`，然后将其传递给 \`client.beta.messages.toolRunner()\`：

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
    unit: z.enum(["celsius", "fahrenheit"]).optional(),
  }),
  run: async (input) => {
    // 此处为您的实现细节
    return \`在 \${input.location} 天气晴朗，72°F\`;
  },
});

// 工具运行器负责处理代理循环并返回最终消息
const finalMessage = await client.beta.messages.toolRunner({
  model: "{{OPUS_ID}}",
  max_tokens: 4096,
  tools: [getWeather],
  messages: [{ role: "user", content: "巴黎的天气怎么样？" }],
});

console.log(finalMessage.content);
\`\`\`

**工具运行器的核心优势：**

- 无需手动编写循环 —— SDK 负责调用工具并反馈结果
- 通过 Zod 架构实现类型安全的工具输入
- 工具架构会根据 Zod 定义自动生成
- 当 Claude 不再发起工具调用时，迭代会自动停止

---

## 手动代理循环 (Manual Agentic Loop)

当您需要更精细的控制（如自定义日志记录、有条件的工具执行、流式输出单次迭代、人工审批等）时，请使用此方法：

\`\`\`typescript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();
const tools: Anthropic.Tool[] = [...]; // 您的工具定义
let messages: Anthropic.MessageParam[] = [{ role: "user", content: userInput }];

while (true) {
  const response = await client.messages.create({
    model: "{{OPUS_ID}}",
    max_tokens: 4096,
    tools: tools,
    messages: messages,
  });

  if (response.stop_reason === "end_turn") break;

  // 服务端工具达到迭代限制；重新发送以继续执行
  if (response.stop_reason === "pause_turn") {
    messages = [
      { role: "user", content: userInput },
      { role: "assistant", content: response.content },
    ];
    continue;
  }

  const toolUseBlocks = response.content.filter(
    (b): b is Anthropic.ToolUseBlock => b.type === "tool_use",
  );

  messages.push({ role: "assistant", content: response.content });

  const toolResults: Anthropic.ToolResultBlockParam[] = [];
  for (const tool of toolUseBlocks) {
    const result = await executeTool(tool.name, tool.input);
    toolResults.push({
      type: "tool_result",
      tool_use_id: tool.id,
      content: result,
    });
  }

  messages.push({ role: "user", content: toolResults });
}
\`\`\`

### 流式传输下的手动循环

当需要在手动循环中实现流式传输时，请使用 \`client.messages.stream()\` + \`finalMessage()\` 而不是 \`.create()\`。Text deltas 会在每次迭代中流式输出；\`finalMessage()\` 则收集完整的 \`Message\` 供您检查 \`stop_reason\` 并提取工具调用块：

\`\`\`typescript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();
const tools: Anthropic.Tool[] = [...];
let messages: Anthropic.MessageParam[] = [{ role: "user", content: userInput }];

while (true) {
  const stream = client.messages.stream({
    model: "{{OPUS_ID}}",
    max_tokens: 4096,
    tools,
    messages,
  });

  // 在每次迭代中流式显示文本内容
  stream.on("text", (delta) => {
    process.stdout.write(delta);
  });

  // finalMessage() 会解析为完整的 Message —— 无需手动绑定 
  // .on("message") / .on("error") / .on("abort") 等事件
  const message = await stream.finalMessage();

  if (message.stop_reason === "end_turn") break;

  // 服务端工具达到迭代限制；重新发送以继续执行
  if (message.stop_reason === "pause_turn") {
    messages = [
      { role: "user", content: userInput },
      { role: "assistant", content: message.content },
    ];
    continue;
  }

  const toolUseBlocks = message.content.filter(
    (b): b is Anthropic.ToolUseBlock => b.type === "tool_use",
  );

  messages.push({ role: "assistant", content: message.content });

  const toolResults: Anthropic.ToolResultBlockParam[] = [];
  for (const tool of toolUseBlocks) {
    const result = await executeTool(tool.name, tool.input);
    toolResults.push({
      type: "tool_result",
      tool_use_id: tool.id,
      content: result,
    });
  }

  messages.push({ role: "user", content: toolResults });
}
\`\`\`

> **重要提示：** 不要通过在 \`.on()\` 事件中包装 \`new Promise()\` 的方式来收集最终消息 —— 请直接使用 \`stream.finalMessage()\`。SDK 内部会处理所有的错误/中止/完成状态。

> **循环中的错误处理：** 请使用 SDK 的类型化异常类（如 \`Anthropic.RateLimitError\`, \`Anthropic.APIError\`）—— 示例请参阅 [错误处理](./README.md#error-handling)。不要使用字符串匹配来检查错误消息。

> **SDK 类型：** 对于所有 API 相关的数据结构，请使用 \`Anthropic.MessageParam\`, \`Anthropic.Tool\`, \`Anthropic.ToolUseBlock\`, \`Anthropic.ToolResultBlockParam\`, \`Anthropic.Message\` 等。不要重新定义等效的接口。

---

## 处理工具结果

\`\`\`typescript
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  tools: tools,
  messages: [{ role: "user", content: "巴黎的天气怎么样？" }],
});

for (const block of response.content) {
  if (block.type === "tool_use") {
    const result = await executeTool(block.name, block.input);

    const followup = await client.messages.create({
      model: "{{OPUS_ID}}",
      max_tokens: 1024,
      tools: tools,
      messages: [
        { role: "user", content: "巴黎的天气怎么样？" },
        { role: "assistant", content: response.content },
        {
          role: "user",
          content: [
            { type: "tool_result", tool_use_id: block.id, content: result },
          ],
        },
      ],
    });
  }
}
\`\`\`

---

## 工具选择 (Tool Choice)

\`\`\`typescript
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  tools: tools,
  tool_choice: { type: "tool", name: "get_weather" },
  messages: [{ role: "user", content: "巴黎的天气怎么样？" }],
});
\`\`\`

---

## 代码执行 (Code Execution)

### 基础用法

\`\`\`typescript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();

const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 4096,
  messages: [
    {
      role: "user",
      content:
        "计算 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] 的均值和标准差",
    },
  ],
  tools: [{ type: "code_execution_20260120", name: "code_execution" }],
});
\`\`\`

### 上传文件进行分析

\`\`\`typescript
import Anthropic, { toFile } from "@anthropic-ai/sdk";
import { createReadStream } from "fs";

const client = new Anthropic();

// 1. 上传文件
const uploaded = await client.beta.files.upload({
  file: await toFile(createReadStream("sales_data.csv"), undefined, {
    type: "text/csv",
  }),
  betas: ["files-api-2025-04-14"],
});

// 2. 传递给代码执行工具
// 代码执行是 GA 特性；文件 API 仍处于 Beta 阶段（需要通过 RequestOptions 传递）
const response = await client.messages.create(
  {
    model: "{{OPUS_ID}}",
    max_tokens: 4096,
    messages: [
      {
        role: "user",
        content: [
          {
            type: "text",
            text: "分析这些销售数据。展示趋势并生成可视化图表。",
          },
          { type: "container_upload", file_id: uploaded.id },
        ],
      },
    ],
    tools: [{ type: "code_execution_20260120", name: "code_execution" }],
  },
  { headers: { "anthropic-beta": "files-api-2025-04-14" } },
);
\`\`\`

### 获取生成的文件

\`\`\`typescript
import path from "path";
import fs from "fs";

const OUTPUT_DIR = "./claude_outputs";
await fs.promises.mkdir(OUTPUT_DIR, { recursive: true });

for (const block of response.content) {
  if (block.type === "bash_code_execution_tool_result") {
    const result = block.content;
    if (result.type === "bash_code_execution_result" && result.content) {
      for (const fileRef of result.content) {
        if (fileRef.type === "bash_code_execution_output") {
          const metadata = await client.beta.files.retrieveMetadata(
            fileRef.file_id,
          );
          const response = await client.beta.files.download(fileRef.file_id);
          const fileBytes = Buffer.from(await response.arrayBuffer());
          const safeName = path.basename(metadata.filename);
          if (!safeName || safeName === "." || safeName === "..") {
            console.warn(\`跳过无效的文件名: \${metadata.filename}\`);
            continue;
          }
          const outputPath = path.join(OUTPUT_DIR, safeName);
          await fs.promises.writeFile(outputPath, fileBytes);
          console.log(\`已保存: \${outputPath}\`);
        }
      }
    }
  }
}
\`\`\`

### 容器复用 (Container Reuse)

\`\`\`typescript
// 第一个请求：设置环境
const response1 = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 4096,
  messages: [
    {
      role: "user",
      content: "安装 tabulate 并创建一个带有示例用户数据的 data.json 文件",
    },
  ],
  tools: [{ type: "code_execution_20260120", name: "code_execution" }],
});

// 复用容器
const containerId = response1.container.id;

const response2 = await client.messages.create({
  container: containerId,
  model: "{{OPUS_ID}}",
  max_tokens: 4096,
  messages: [
    {
      role: "user",
      content: "读取 data.json 并以格式化表格的形式展示",
    },
  ],
  tools: [{ type: "code_execution_20260120", name: "code_execution" }],
});
\`\`\`

---

## 记忆工具 (Memory Tool)

### 基础用法

\`\`\`typescript
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 2048,
  messages: [
    {
      role: "user",
      content: "请记住我偏好的语言是 TypeScript。",
    },
  ],
  tools: [{ type: "memory_20250818", name: "memory" }],
});
\`\`\`

### SDK 记忆助手 (Memory Helper)

通过实现 \`MemoryToolHandlers\` 来使用 \`betaMemoryTool\`：

\`\`\`typescript
import {
  betaMemoryTool,
  type MemoryToolHandlers,
} from "@anthropic-ai/sdk/helpers/beta/memory";

const handlers: MemoryToolHandlers = {
  async view(command) { ... },
  async create(command) { ... },
  async str_replace(command) { ... },
  async insert(command) { ... },
  async delete(command) { ... },
  async rename(command) { ... },
};

const memory = betaMemoryTool(handlers);

const runner = client.beta.messages.toolRunner({
  model: "{{OPUS_ID}}",
  max_tokens: 2048,
  tools: [memory],
  messages: [{ role: "user", content: "记住我的偏好" }],
});

for await (const message of runner) {
  console.log(message);
}
\`\`\`

有关完整的实现示例，请使用 WebFetch 访问：

- \`https://github.com/anthropics/anthropic-sdk-typescript/blob/main/examples/tools-helpers-memory.ts\`

---

## 结构化输出 (Structured Outputs)

### JSON 输出 (Zod — 推荐)

\`\`\`typescript
import Anthropic from "@anthropic-ai/sdk";
import { z } from "zod";
import { zodOutputFormat } from "@anthropic-ai/sdk/helpers/zod";

const ContactInfoSchema = z.object({
  name: z.string(),
  email: z.string(),
  plan: z.string(),
  interests: z.array(z.string()),
  demo_requested: z.boolean(),
});

const client = new Anthropic();

const response = await client.messages.parse({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  messages: [
    {
      role: "user",
      content:
        "提取: Jane Doe (jane@co.com) 想要企业版 (Enterprise), 对 API 和 SDK 感兴趣, 需要演示 (demo)。",
    },
  ],
  output_config: {
    format: zodOutputFormat(ContactInfoSchema),
  },
});

console.log(response.parsed_output.name); // "Jane Doe"
\`\`\`

### 严格工具使用 (Strict Tool Use)

\`\`\`typescript
const response = await client.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  messages: [
    {
      role: "user",
      content: "预订一张 3 月 15 日去东京的机票，2 名乘客",
    },
  ],
  tools: [
    {
      name: "book_flight",
      description: "预订前往特定目的地的机票",
      strict: true,
      input_schema: {
        type: "object",
        properties: {
          destination: { type: "string" },
          date: { type: "string", format: "date" },
          passengers: {
            type: "integer",
            enum: [1, 2, 3, 4, 5, 6, 7, 8],
          },
        },
        required: ["destination", "date", "passengers"],
        additionalProperties: false,
      },
    },
  ],
});
\`\`\`
