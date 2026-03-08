<!--
name: 'Data: Files API reference — TypeScript'
description: TypeScript 文件 API 参考，包含文件上传、列表显示、删除及在消息中的用法
ccVersion: 2.1.63
-->
# 文件 (Files) API — TypeScript

文件 (Files) API 用于上传文件，以便在消息 API 请求中使用。请在内容块中通过 \`file_id\` 引用文件，这可以避免在多次 API 调用中重复上传。

**Beta：** 请在 API 调用中传递 \`betas: ["files-api-2025-04-14"]\`（SDK 会自动设置所需的标头）。

## 关键信息

- 最大文件限制：500 MB
- 总存储额：每个组织 100 GB
- 除非显式删除，否则文件将持久保存
- 文件操作（上传、展示、删除）本身是免费的；但在消息中使用的内容会作为输入 Token 计费
- 不适用于 Amazon Bedrock 或 Google Vertex AI

---

## 上传文件 (Upload a File)

\`\`\`typescript
import Anthropic, { toFile } from "@anthropic-ai/sdk";
import fs from "fs";

const client = new Anthropic();

const uploaded = await client.beta.files.upload({
  file: await toFile(fs.createReadStream("report.pdf"), undefined, {
    type: "application/pdf",
  }),
  betas: ["files-api-2025-04-14"],
});

console.log(\`文件 ID: \${uploaded.id}\`);
console.log(\`大小: \${uploaded.size_bytes} 字节\`);
\`\`\`

---

## 在消息中使用文件

### PDF / 文本文件

\`\`\`typescript
const response = await client.beta.messages.create({
  model: "{{OPUS_ID}}",
  max_tokens: 1024,
  messages: [
    {
      role: "user",
      content: [
        { type: "text", text: "总结这份报告的关键发现。" },
        {
          type: "document",
          source: { type: "file", file_id: uploaded.id },
          title: "第四季度报告",
          citations: { enabled: true },
        },
      ],
    },
  ],
  betas: ["files-api-2025-04-14"],
});

console.log(response.content[0].text);
\`\`\`

---

## 管理文件

### 列出文件

\`\`\`typescript
const files = await client.beta.files.list({
  betas: ["files-api-2025-04-14"],
});
for (const f of files.data) {
  console.log(\`\${f.id}: \${f.filename} (\${f.size_bytes} 字节)\`);
}
\`\`\`

### 删除文件

\`\`\`typescript
await client.beta.files.delete("file_011CNha8iCJcU1wXNR6q4V8w", {
  betas: ["files-api-2025-04-14"],
});
\`\`\`

### 下载文件

\`\`\`typescript
const response = await client.beta.files.download(
  "file_011CNha8iCJcU1wXNR6q4V8w",
  { betas: ["files-api-2025-04-14"] },
);
const content = Buffer.from(await response.arrayBuffer());
await fs.promises.writeFile("output.txt", content);
\`\`\`
