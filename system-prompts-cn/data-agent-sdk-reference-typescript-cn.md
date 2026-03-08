<!--
name: 'Data: Agent SDK reference — TypeScript'
description: TypeScript Agent SDK 参考，包括安装、快速入门、自定义工具和 Hook
ccVersion: 2.1.63
-->
# Agent SDK — TypeScript

Claude Agent SDK 为构建具有内置工具、安全特性和代理能力的 AI 代理提供了高级接口。

## 安装

\`\`\`bash
npm install @anthropic-ai/claude-agent-sdk
\`\`\`

---

## 快速入门

\`\`\`typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

for await (const message of query({
  prompt: "解释这个代码库",
  options: { allowedTools: ["Read", "Glob", "Grep"] },
})) {
  if ("result" in message) {
    console.log(message.result);
  }
}
\`\`\`

---

## 内置工具

| 工具      | 描述                          |
| --------- | ----------------------------- |
| Read      | 读取工作区中的文件            |
| Write     | 创建新文件                    |
| Edit      | 对现有文件进行精确编辑        |
| Bash      | 执行 shell 命令               |
| Glob      | 按模式查找文件                |
| Grep      | 按内容搜索文件                |
| WebSearch | 在网页上搜索信息              |
| WebFetch        | 获取并分析网页内容            |
| AskUserQuestion | 向用户询问澄清性问题          |
| Agent           | 启动子代理 (Subagent)         |

---

## 权限系统

\`\`\`typescript
for await (const message of query({
  prompt: "重构身份验证模块",
  options: {
    allowedTools: ["Read", "Edit", "Write"],
    permissionMode: "acceptEdits",
  },
})) {
  if ("result" in message) console.log(message.result);
}
\`\`\`

权限模式 (Permission modes)：

- \`"default"\`：对危险操作进行提示
- \`"plan"\`：仅规划，不执行
- \`"acceptEdits"\`：自动接受文件编辑
- \`"dontAsk"\`：不提示（适用于 CI/CD）
- \`"bypassPermissions"\`：跳过所有提示（需要在选项中设置 \`allowDangerouslySkipPermissions: true\`）

---

## MCP（模型上下文协议）支持

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

### 进程内 MCP 工具

您可以使用 \`tool()\` 和 \`createSdkMcpServer\` 定义在进程内运行的自定义工具：

\`\`\`typescript
import { query, tool, createSdkMcpServer } from "@anthropic-ai/claude-agent-sdk";
import { z } from "zod";

const myTool = tool("my-tool", "说明", { input: z.string() }, async (args) => {
  return { content: [{ type: "text", text: "结果" }] };
});

const server = createSdkMcpServer({ name: "my-server", tools: [myTool] });

// 传递给 query
for await (const message of query({
  prompt: "使用 my-tool 执行某些操作",
  options: { mcpServers: { myServer: server } },
})) {
  if ("result" in message) console.log(message.result);
}
\`\`\`

---

## Hook

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

可用的 Hook 事件：\`PreToolUse\`, \`PostToolUse\`, \`PostToolUseFailure\`, \`Notification\`, \`UserPromptSubmit\`, \`SessionStart\`, \`SessionEnd\`, \`Stop\`, \`SubagentStart\`, \`SubagentStop\`, \`PreCompact\`, \`PermissionRequest\`, \`Setup\`, \`TeammateIdle\`, \`TaskCompleted\`, \`ConfigChange\`

---

## 常用选项

\`query()\` 接受一个顶级的 \`prompt\`（字符串）和一个 \`options\`（对象）：

\`\`\`typescript
query({ prompt: "...", options: { ... } })
\`\`\`

| 选项                              | 类型   | 描述                                                                |
| --------------------------------- | ------ | ------------------------------------------------------------------- |
| \`cwd\`                               | string | 文件操作的工作目录                                                  |
| \`allowedTools\`                      | array  | 代理可以使用的工具（例如 \`["Read", "Edit", "Bash"]\`）             |
| \`tools\`                             | array  | 设置可用的内置工具（限制默认集合）                                  |
| \`disallowedTools\`                   | array  | 明确禁止使用的工具                                                  |
| \`permissionMode\`                    | string | 如何处理权限提示                                                    |
| \`allowDangerouslySkipPermissions\`   | bool   | 必须为 \`true\` 才能使用 \`permissionMode: "bypassPermissions"\`    |
| \`mcpServers\`                        | object | 要连接的 MCP 服务器                                                 |
| \`hooks\`                             | object | 用于自定义行为的 Hook                                               |
| \`systemPrompt\`                      | string | 自定义系统提示词                                                    |
| \`maxTurns\`                          | number | 停止前的最大代理轮数                                                |
| \`maxBudgetUsd\`                      | number | 查询的最大预算（美元）                                              |
| \`model\`                             | string | 模型 ID（默认：由 CLI 决定）                                        |
| \`agents\`                            | object | 子代理定义 (\`Record<string, AgentDefinition>\`)                    |
| \`outputFormat\`                      | object | 结构化输出架构                                                      |
| \`thinking\`                          | object | 思考/推理控制                                                       |
| \`betas\`                             | array  | 要启用的 Beta 功能（例如 \`["context-1m-2025-08-07"]\`）            |
| \`settingSources\`                    | array  | 要加载的设置（例如 \`["project"]\`）。默认：无（不加载 CLAUDE.md 文件）|
| \`env\`                               | object | 为会话设置的环境变量                                                |

---

## 子代理 (Subagents)

\`\`\`typescript
for await (const message of query({
  prompt: "使用 code-reviewer 代理审查此代码库",
  options: {
    allowedTools: ["Read", "Glob", "Grep", "Agent"],
    agents: {
      "code-reviewer": {
        description: "负责代码质量和安全审查的专家代码审查代理。",
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

## 消息类型

\`\`\`typescript
for await (const message of query({
  prompt: "查找 TODO 注释",
  options: { allowedTools: ["Read", "Glob", "Grep"] },
})) {
  if ("result" in message) {
    console.log(message.result);
  } else if (message.type === "system" && message.subtype === "init") {
    const sessionId = message.session_id; // 捕获以便稍后恢复
  }
}
\`\`\`

---

## 最佳实践

1. **务必指定 allowedTools** —— 明确列出代理可以使用的工具
2. **设置工作目录** —— 始终为文件操作指定 \`cwd\`
3. **使用适当的权限模式** —— 从 \`"default"\` 开始，仅在需要时提升
4. **处理所有消息类型** —— 检查 \`result\` 属性以获取代理输出
5. **限制 maxTurns** —— 设置合理的限制以防止代理失控
