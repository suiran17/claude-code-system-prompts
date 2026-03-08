<!--
name: 'Data: Agent SDK patterns — TypeScript'
description: TypeScript Agent SDK 模式，包括基础智能体、Hook、子智能体和 MCP 集成
ccVersion: 2.1.63
-->
# Agent SDK 模式 — TypeScript

## 基础智能体

\`\`\`typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

async function main() {
  for await (const message of query({
    prompt: "解释此仓库的作用",
    options: {
      cwd: "/path/to/project",
      allowedTools: ["Read", "Glob", "Grep"],
    },
  })) {
    if ("result" in message) {
      console.log(message.result);
    }
  }
}

main();
\`\`\`

---

## Hook

### 工具执行后 Hook (After Tool Use Hook)

\`\`\`typescript
import { query, HookCallback } from "@anthropic-ai/claude-agent-sdk";
import { appendFileSync } from "fs";

const logFileChange: HookCallback = async (input) => {
  const filePath = (input as any).tool_input?.file_path ?? "未知";
  appendFileSync(
    "./audit.log",
    \`\${new Date().toISOString()}: 修改了 \${filePath}\\n\`,
  );
  return {};
};

for await (const message of query({
  prompt: "重构 utils.py 以提高可读性",
  options: {
    allowedTools: ["Read", "Edit", "Write"],
    permissionMode: "acceptEdits",
    hooks: {
      PostToolUse: [{ matcher: "Edit|Write", hooks: [logFileChange] }],
    },
  },
})) {
  if ("result" in message) console.log(message.result);
}
\`\`\`

---

## 子智能体 (Subagents)

\`\`\`typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

for await (const message of query({
  prompt: "使用 code-reviewer 智能体审查此代码库",
  options: {
    allowedTools: ["Read", "Glob", "Grep", "Agent"],
    agents: {
      "code-reviewer": {
        description: "负责代码质量和安全审查的专家代码审查智能体。",
        prompt: "分析代码质量并建议改进方案。",
        tools: ["Read", "Glob", "Grep"],
      },
    },
  },
})) {
  if ("result" in message) console.log(message.result);
}
\`\`\`

---

## MCP 服务器集成

### 浏览器自动化 (Playwright)

\`\`\`typescript
for await (const message of query({
  prompt: "打开 example.com 并描述你看到的内容",
  options: {
    mcpServers: {
      playwright: { command: "npx", args: ["@playwright/mcp@latest"] },
    },
  },
})) {
  if ("result" in message) console.log(message.result);
}
\`\`\`

---

## 会话恢复 (Session Resumption)

\`\`\`typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

let sessionId: string | undefined;

// 第一次查询：捕获会话 ID
for await (const message of query({
  prompt: "读取身份验证模块",
  options: { allowedTools: ["Read", "Glob"] },
})) {
  if (message.type === "system" && message.subtype === "init") {
    sessionId = message.session_id;
  }
}

// 恢复会话，携带第一次查询的完整上下文
for await (const message of query({
  prompt: "现在找出所有调用它的地方",
  options: { resume: sessionId },
})) {
  if ("result" in message) console.log(message.result);
}
\`\`\`

---

## 自定义系统提示词

\`\`\`typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

for await (const message of query({
  prompt: "审查此代码",
  options: {
    allowedTools: ["Read", "Glob", "Grep"],
    systemPrompt: \`您是一位资深代码审查员，专注于：
1. 安全漏洞
2. 性能问题
3. 代码可维护性

务必提供具体的行号和改进建议。\`,
  },
})) {
  if ("result" in message) console.log(message.result);
}
\`\`\`
