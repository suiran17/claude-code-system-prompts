<!--
name: 'Data: Agent SDK reference — Python'
description: Python Agent SDK 参考，包括安装、快速入门、通过 MCP 的自定义工具和 Hook
ccVersion: 2.1.63
-->
# Agent SDK — Python

Claude Agent SDK 为构建具有内置工具、安全特性和代理能力的 AI 代理提供了高级接口。

## 安装

\`\`\`bash
pip install claude-agent-sdk
\`\`\`

---

## 快速入门

\`\`\`python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async def main():
    async for message in query(
        prompt="解释这个代码库",
        options=ClaudeAgentOptions(allowed_tools=["Read", "Glob", "Grep"])
    ):
        if isinstance(message, ResultMessage):
            print(message.result)

anyio.run(main)
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

## 主要接口

### \`query()\` — 简单的单次使用

\`query()\` 函数是运行代理最简单的方式。它返回消息的异步迭代器。

\`\`\`python
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async for message in query(
    prompt="解释这个代码库",
    options=ClaudeAgentOptions(allowed_tools=["Read", "Glob", "Grep"])
):
    if isinstance(message, ResultMessage):
        print(message.result)
\`\`\`

### \`ClaudeSDKClient\` — 全面控制

\`ClaudeSDKClient\` 提供了对代理生命周期的全面控制。当您需要自定义工具、Hook、流式传输或中断执行的能力时，请使用它。

\`\`\`python
import anyio
from claude_agent_sdk import ClaudeSDKClient, ClaudeAgentOptions, AssistantMessage, TextBlock

async def main():
    options = ClaudeAgentOptions(allowed_tools=["Read", "Glob", "Grep"])
    async with ClaudeSDKClient(options=options) as client:
        await client.query("解释这个代码库")
        async for message in client.receive_response():
            if isinstance(message, AssistantMessage):
                for block in message.content:
                    if isinstance(block, TextBlock):
                        print(block.text)

anyio.run(main)
\`\`\`

\`ClaudeSDKClient\` 支持：

- **内容管理器** (\`async with\`) 用于自动资源清理
- **\`client.query(prompt)\`** 向代理发送提示词
- **\`receive_response()\`** 用于流式传输消息直至完成
- **\`interrupt()\`** 在任务中途停止代理执行
- **自定义工具必需项**（通过 SDK MCP 服务器）

---

## 权限系统

\`\`\`python
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async for message in query(
    prompt="重构身份验证模块",
    options=ClaudeAgentOptions(
        allowed_tools=["Read", "Edit", "Write"],
        permission_mode="acceptEdits"  # 自动接受文件编辑
    )
):
    if isinstance(message, ResultMessage):
        print(message.result)
\`\`\`

权限模式 (Permission modes)：

- \`"default"\`：对危险操作进行提示
- \`"plan"\`：仅规划，不执行
- \`"acceptEdits"\`：自动接受文件编辑
- \`"dontAsk"\`：不提示（适用于 CI/CD）
- \`"bypassPermissions"\`：跳过所有提示（需要在选项中设置 \`allow_dangerously_skip_permissions=True\`）

---

## MCP（模型上下文协议）支持

\`\`\`python
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async for message in query(
    prompt="打开 example.com 并描述你看到的内容",
    options=ClaudeAgentOptions(
        mcp_servers={
            "playwright": {"command": "npx", "args": ["@playwright/mcp@latest"]}
        }
    )
):
    if isinstance(message, ResultMessage):
        print(message.result)
\`\`\`

---

## Hook

使用回调函数通过 Hook 自定义代理行为：

\`\`\`python
from claude_agent_sdk import query, ClaudeAgentOptions, HookMatcher, ResultMessage

async def log_file_change(input_data, tool_use_id, context):
    file_path = input_data.get('tool_input', {}).get('file_path', '未知')
    print(f"已修改: {file_path}")
    return {}

async for message in query(
    prompt="重构 utils.py",
    options=ClaudeAgentOptions(
        permission_mode="acceptEdits",
        hooks={
            "PostToolUse": [HookMatcher(matcher="Edit|Write", hooks=[log_file_change])]
        }
    )
):
    if isinstance(message, ResultMessage):
        print(message.result)
\`\`\`

可用的 Hook 事件：\`PreToolUse\`, \`PostToolUse\`, \`PostToolUseFailure\`, \`Notification\`, \`UserPromptSubmit\`, \`SessionStart\`, \`SessionEnd\`, \`Stop\`, \`SubagentStart\`, \`SubagentStop\`, \`PreCompact\`, \`PermissionRequest\`, \`Setup\`, \`TeammateIdle\`, \`TaskCompleted\`, \`ConfigChange\`

---

## 常用选项

\`query()\` 接受一个顶级的 \`prompt\`（字符串）和一个 \`options\` 对象 (\`ClaudeAgentOptions\`)：

\`\`\`python
async for message in query(prompt="...", options=ClaudeAgentOptions(...)):
\`\`\`

| 选项                              | 类型   | 描述                                                                |
| --------------------------------- | ------ | ------------------------------------------------------------------- |
| \`cwd\`                               | string | 文件操作的工作目录                                                  |
| \`allowed_tools\`                     | list   | 代理可以使用的工具（例如 \`["Read", "Edit", "Bash"]\`）             |
| \`tools\`                             | list   | 设置可用的内置工具（限制默认集合）                                  |
| \`disallowed_tools\`                  | list   | 明确禁止使用的工具                                                  |
| \`permission_mode\`                   | string | 如何处理权限提示                                                    |
| \`allow_dangerously_skip_permissions\`| bool   | 必须为 \`True\` 才能使用 \`permission_mode="bypassPermissions"\`    |
| \`mcp_servers\`                       | dict   | 要连接的 MCP 服务器                                                 |
| \`hooks\`                             | dict   | 用于自定义行为的 Hook                                               |
| \`system_prompt\`                     | string | 自定义系统提示词                                                    |
| \`max_turns\`                         | int    | 停止前的最大代理轮数                                                |
| \`max_budget_usd\`                    | float  | 查询的最大预算（美元）                                              |
| \`model\`                             | string | 模型 ID（默认：由 CLI 决定）                                        |
| \`agents\`                            | dict   | 子代理定义 (\`dict[str, AgentDefinition]\`)                         |
| \`output_format\`                     | dict   | 结构化输出架构                                                      |
| \`thinking\`                          | dict   | 思考/推理控制                                                       |
| \`betas\`                             | list   | 要启用的 Beta 功能（例如 \`["context-1m-2025-08-07"]\`）            |
| \`setting_sources\`                   | list   | 要加载的设置（例如 \`["project"]\`）。默认：无（不加载 CLAUDE.md 文件）|
| \`env\`                               | dict   | 为会话设置的环境变量                                                |

---

## 消息类型

\`\`\`python
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage, SystemMessage

async for message in query(
    prompt="查找 TODO 注释",
    options=ClaudeAgentOptions(allowed_tools=["Read", "Glob", "Grep"])
):
    if isinstance(message, ResultMessage):
        print(message.result)
    elif isinstance(message, SystemMessage) and message.subtype == "init":
        session_id = message.session_id  # 捕获以便稍后恢复
\`\`\`

---

## 子代理 (Subagents)

\`\`\`python
from claude_agent_sdk import query, ClaudeAgentOptions, AgentDefinition, ResultMessage

async for message in query(
    prompt="使用 code-reviewer 代理审查此代码库",
    options=ClaudeAgentOptions(
        allowed_tools=["Read", "Glob", "Grep", "Agent"],
        agents={
            "code-reviewer": AgentDefinition(
                description="负责代码质量和安全审查的专家代码审查代理。",
                prompt="分析代码质量并建议改进方案。",
                tools=["Read", "Glob", "Grep"]
            )
        }
    )
):
    if isinstance(message, ResultMessage):
        print(message.result)
\`\`\`

---

## 错误处理

\`\`\`python
from claude_agent_sdk import query, ClaudeAgentOptions, CLINotFoundError, CLIConnectionError, ResultMessage

try:
    async for message in query(
        prompt="...",
        options=ClaudeAgentOptions(allowed_tools=["Read"])
    ):
        if isinstance(message, ResultMessage):
            print(message.result)
except CLINotFoundError:
    print("未找到 Claude Code CLI。请通过 pip install claude-agent-sdk 安装")
except CLIConnectionError as e:
    print(f"连接错误: {e}")
\`\`\`

---

## 最佳实践

1. **务必指定 allowed_tools** —— 明确列出代理可以使用的工具
2. **设置工作目录** —— 始终为文件操作指定 \`cwd\`
3. **使用适当的权限模式** —— 从 \`"default"\` 开始，仅在需要时提升
4. **处理所有消息类型** —— 检查 \`ResultMessage\` 以获取代理输出
5. **限制 max_turns** —— 设置合理的限制以防止代理失控
