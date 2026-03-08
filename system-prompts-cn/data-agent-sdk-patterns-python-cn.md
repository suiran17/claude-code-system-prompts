<!--
name: 'Data: Agent SDK patterns — Python'
description: Python Agent SDK 模式，包括自定义工具、Hook、子智能体、MCP 集成和会话恢复
ccVersion: 2.1.63
-->
# Agent SDK 模式 — Python

## 基础智能体

\`\`\`python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async def main():
    async for message in query(
        prompt="解释此仓库的作用",
        options=ClaudeAgentOptions(
            cwd="/path/to/project",
            allowed_tools=["Read", "Glob", "Grep"]
        )
    ):
        if isinstance(message, ResultMessage):
            print(message.result)

anyio.run(main)
\`\`\`

---

## 自定义工具

自定义工具需要 MCP 服务器。使用 \`ClaudeSDKClient\` 以获得全面控制，或者通过 \`mcp_servers\` 将服务器传递给 \`query()\`。

\`\`\`python
import anyio
from claude_agent_sdk import (
    tool,
    create_sdk_mcp_server,
    ClaudeSDKClient,
    ClaudeAgentOptions,
    AssistantMessage,
    TextBlock,
)

@tool("get_weather", "获取指定位置的当前天气", {"location": str})
async def get_weather(args):
    location = args["location"]
    return {"content": [{"type": "text", "text": f"{location} 的天气是晴天，72°F。"}]}

server = create_sdk_mcp_server("weather-tools", tools=[get_weather])

async def main():
    options = ClaudeAgentOptions(mcp_servers={"weather": server})
    async with ClaudeSDKClient(options=options) as client:
        await client.query("巴黎的天气怎么样？")
        async for message in client.receive_response():
            if isinstance(message, AssistantMessage):
                for block in message.content:
                    if isinstance(block, TextBlock):
                        print(block.text)

anyio.run(main)
\`\`\`

---

## Hook

### 工具执行后 Hook (After Tool Use Hook)

在每次编辑后记录文件更改：

\`\`\`python
import anyio
from datetime import datetime
from claude_agent_sdk import query, ClaudeAgentOptions, HookMatcher, ResultMessage

async def log_file_change(input_data, tool_use_id, context):
    file_path = input_data.get('tool_input', {}).get('file_path', '未知')
    with open('./audit.log', 'a') as f:
        f.write(f"{datetime.now()}: 修改了 {file_path}\\n")
    return {}

async def main():
    async for message in query(
        prompt="重构 utils.py 以提高可读性",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Edit", "Write"],
            permission_mode="acceptEdits",
            hooks={
                "PostToolUse": [HookMatcher(matcher="Edit|Write", hooks=[log_file_change])]
            }
        )
    ):
        if isinstance(message, ResultMessage):
            print(message.result)

anyio.run(main)
\`\`\`

---

## 子智能体 (Subagents)

\`\`\`python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions, AgentDefinition, ResultMessage

async def main():
    async for message in query(
        prompt="使用 code-reviewer 智能体审查此代码库",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Glob", "Grep", "Agent"],
            agents={
                "code-reviewer": AgentDefinition(
                    description="负责代码质量和安全审查的专家代码审查智能体。",
                    prompt="分析代码质量并建议改进方案。",
                    tools=["Read", "Glob", "Grep"]
                )
            }
        )
    ):
        if isinstance(message, ResultMessage):
            print(message.result)

anyio.run(main)
\`\`\`

---

## MCP 服务器集成

### 浏览器自动化 (Playwright)

\`\`\`python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async def main():
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

anyio.run(main)
\`\`\`

### 数据库访问 (PostgreSQL)

\`\`\`python
import os
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async def main():
    async for message in query(
        prompt="显示按订单数排名的前 10 名用户",
        options=ClaudeAgentOptions(
            mcp_servers={
                "postgres": {
                    "command": "npx",
                    "args": ["-y", "@modelcontextprotocol/server-postgres"],
                    "env": {"DATABASE_URL": os.environ["DATABASE_URL"]}
                }
            }
        )
    ):
        if isinstance(message, ResultMessage):
            print(message.result)

anyio.run(main)
\`\`\`

---

## 权限模式 (Permission Modes)

\`\`\`python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions

async def main():
    # 默认：对危险操作提示
    async for message in query(
        prompt="删除所有测试文件",
        options=ClaudeAgentOptions(
            allowed_tools=["Bash"],
            permission_mode="default"  # 删除前会提示
        )
    ):
        pass

    # 计划：智能体在进行更改前先创建计划
    async for message in query(
        prompt="重构身份验证系统",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Edit"],
            permission_mode="plan"
        )
    ):
        pass

    # 接受编辑：自动接受文件编辑
    async for message in query(
        prompt="重构此模块",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Edit"],
            permission_mode="acceptEdits"
        )
    ):
        pass

    # 绕过：跳过所有提示（小心使用）
    async for message in query(
        prompt="设置开发环境",
        options=ClaudeAgentOptions(
            allowed_tools=["Bash", "Write"],
            permission_mode="bypassPermissions",
            allow_dangerously_skip_permissions=True
        )
    ):
        pass

anyio.run(main)
\`\`\`

---

## 错误恢复

\`\`\`python
import anyio
from claude_agent_sdk import (
    query,
    ClaudeAgentOptions,
    CLINotFoundError,
    CLIConnectionError,
    ProcessError,
    ResultMessage,
)

async def run_with_recovery():
    try:
        async for message in query(
            prompt="修复失败的测试",
            options=ClaudeAgentOptions(
                allowed_tools=["Read", "Edit", "Bash"],
                max_turns=10
            )
        ):
            if isinstance(message, ResultMessage):
                print(message.result)
    except CLINotFoundError:
        print("未找到 Claude Code CLI。请通过 pip install claude-agent-sdk 安装")
    except CLIConnectionError as e:
        print(f"连接错误: {e}")
    except ProcessError as e:
        print(f"进程错误: {e}")

anyio.run(run_with_recovery)
\`\`\`

---

## 会话恢复 (Session Resumption)

\`\`\`python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage, SystemMessage

async def main():
    session_id = None

    # 第一次查询：捕获会话 ID
    async for message in query(
        prompt="读取身份验证模块",
        options=ClaudeAgentOptions(allowed_tools=["Read", "Glob"])
    ):
        if isinstance(message, SystemMessage) and message.subtype == "init":
            session_id = message.session_id

    # 恢复会话，携带第一次查询的完整上下文
    async for message in query(
        prompt="现在找出所有调用它的地方",  # "它" 指代身份验证模块
        options=ClaudeAgentOptions(resume=session_id)
    ):
        if isinstance(message, ResultMessage):
            print(message.result)

anyio.run(main)
\`\`\`

---

## 自定义系统提示词

\`\`\`python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async def main():
    async for message in query(
        prompt="审查此代码",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Glob", "Grep"],
            system_prompt="""您是一位资深代码审查员，专注于：
1. 安全漏洞
2. 性能问题
3. 代码可维护性

务必提供具体的行号和改进建议。"""
        )
    ):
        if isinstance(message, ResultMessage):
            print(message.result)

anyio.run(main)
\`\`\`
