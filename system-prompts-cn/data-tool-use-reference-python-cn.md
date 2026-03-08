<!--
name: 'Data: Tool use reference — Python'
description: Python 工具使用参考，包括工具运行器、手动智能体循环、代码执行和结构化输出
ccVersion: 2.1.63
-->
# 工具使用 (Tool Use) — Python

有关概念性概览（工具定义、工具选择、建议提示），请参阅 [shared/tool-use-concepts.md](../../shared/tool-use-concepts.md)。

## 工具运行器 (Tool Runner，推荐使用)

**Beta：** 工具运行器在 Python SDK 中处于 Beta 测试阶段。

使用 \`@beta_tool\` 装饰器将工具定义为类型化函数，然后将其传递给 \`client.beta.messages.tool_runner()\`：

\`\`\`python
import anthropic
from anthropic import beta_tool

client = anthropic.Anthropic()

@beta_tool
def get_weather(location: str, unit: str = "celsius") -> str:
    """获取给定位置的当前天气。

    Args:
        location: 城市和州，例如 San Francisco, CA。
        unit: 温度单位，"celsius" 或 "fahrenheit"。
    """
    # 此处为您的实现细节
    return f"在 {location} 天气晴朗，72°F"

# 工具运行器会自动处理智能体循环
runner = client.beta.messages.tool_runner(
    model="{{OPUS_ID}}",
    max_tokens=4096,
    tools=[get_weather],
    messages=[{"role": "user", "content": "巴黎的天气怎么样？"}],
)

# 每次迭代都会产生一条 BetaMessage；当 Claude 任务完成后迭代停止
for message in runner:
    print(message)
\`\`\`

对于异步用法，请针对 \`async def\` 函数使用 \`@beta_async_tool\`。

**工具运行器的核心优势：**

- 无需手动编写循环 —— SDK 负责调用工具并反馈结果
- 通过装饰器实现类型安全的工具输入
- 工具架构 (Schema) 会根据函数签名自动生成
- 当 Claude 不再发起工具调用时，迭代会自动停止

---

## 手动智能体循环 (Manual Agentic Loop)

当您需要更精细的控制（如自定义日志记录、有条件的工具执行、人工审批等）时，请使用此方法：

\`\`\`python
import anthropic

client = anthropic.Anthropic()
tools = [...]  # 您的工具定义
messages = [{"role": "user", "content": user_input}]

# 智能体循环：持续执行直到 Claude 停止调用工具
while True:
    response = client.messages.create(
        model="{{OPUS_ID}}",
        max_tokens=4096,
        tools=tools,
        messages=messages
    )

    # 如果 Claude 任务已完成（不再有工具调用），则跳出循环
    if response.stop_reason == "end_turn":
        break

    # 服务端工具达到迭代限制；重新发送以继续执行
    if response.stop_reason == "pause_turn":
        messages = [
            {"role": "user", "content": user_input},
            {"role": "assistant", "content": response.content},
        ]
        continue

    # 从响应中提取工具调用块
    tool_use_blocks = [b for b in response.content if b.type == "tool_use"]

    # 追加助手的响应（包含 tool_use 块）
    messages.append({"role": "assistant", "content": response.content})

    # 执行每个工具并收集结果
    tool_results = []
    for tool in tool_use_blocks:
        result = execute_tool(tool.name, tool.input)  # 您的实现细节
        tool_results.append({
            "type": "tool_result",
            "tool_use_id": tool.id,  # 必须与 tool_use 块对应的 id 一致
            "content": result
        })

    # 将工具结果作为一条 user 消息追加
    messages.append({"role": "user", "content": tool_results})

# 最终的回复文本
final_text = next(b.text for b in response.content if b.type == "text")
\`\`\`

---

## 处理工具结果

\`\`\`python
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    tools=tools,
    messages=[{"role": "user", "content": "巴黎的天气怎么样？"}]
)

for block in response.content:
    if block.type == "tool_use":
        tool_name = block.name
        tool_input = block.input
        tool_use_id = block.id

        result = execute_tool(tool_name, tool_input)

        followup = client.messages.create(
            model="{{OPUS_ID}}",
            max_tokens=1024,
            tools=tools,
            messages=[
                {"role": "user", "content": "巴黎的天气怎么样？"},
                {"role": "assistant", "content": response.content},
                {
                    "role": "user",
                    "content": [{
                        "type": "tool_result",
                        "tool_use_id": tool_use_id,
                        "content": result
                    }]
                }
            ]
        )
\`\`\`

---

## 多次工具调用

\`\`\`python
tool_results = []

for block in response.content:
    if block.type == "tool_use":
        result = execute_tool(block.name, block.input)
        tool_results.append({
            "type": "tool_result",
            "tool_use_id": block.id,
            "content": result
        })

# 一次性返回所有结果
if tool_results:
    followup = client.messages.create(
        model="{{OPUS_ID}}",
        max_tokens=1024,
        tools=tools,
        messages=[
            *previous_messages,
            {"role": "assistant", "content": response.content},
            {"role": "user", "content": tool_results}
        ]
    )
\`\`\`

---

## 工具结果中的错误处理

\`\`\`python
tool_result = {
    "type": "tool_result",
    "tool_use_id": tool_use_id,
    "content": "错误：找不到位置 'xyz'。请提供有效的城市名称。",
    "is_error": True
}
\`\`\`

---

## 工具选择 (Tool Choice)

\`\`\`python
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    tools=tools,
    tool_choice={"type": "tool", "name": "get_weather"},  # 强制使用特定工具
    messages=[{"role": "user", "content": "巴黎的天气怎么样？"}]
)
\`\`\`

---

## 代码执行 (Code Execution)

### 基础用法

\`\`\`python
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=4096,
    messages=[{
        "role": "user",
        "content": "计算 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] 的均值和标准差"
    }],
    tools=[{
        "type": "code_execution_20260120",
        "name": "code_execution"
    }]
)

for block in response.content:
    if block.type == "text":
        print(block.text)
    elif block.type == "bash_code_execution_tool_result":
        print(f"stdout: {block.content.stdout}")
\`\`\`

### 上传文件进行分析

\`\`\`python
# 1. 上传文件
uploaded = client.beta.files.upload(file=open("sales_data.csv", "rb"))

# 2. 通过 container_upload 块传递给代码执行
# 代码执行是 GA 特性；文件 API 仍处于 Beta 阶段（通过 extra_headers 传递）
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=4096,
    extra_headers={"anthropic-beta": "files-api-2025-04-14"},
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "分析这些销售数据。展示趋势并生成可视化图表。"},
            {"type": "container_upload", "file_id": uploaded.id}
        ]
    }],
    tools=[{"type": "code_execution_20260120", "name": "code_execution"}]
)
\`\`\`

### 获取生成的文件

\`\`\`python
import os

OUTPUT_DIR = "./claude_outputs"
os.makedirs(OUTPUT_DIR, exist_ok=True)

for block in response.content:
    if block.type == "bash_code_execution_tool_result":
        result = block.content
        if result.type == "bash_code_execution_result" and result.content:
            for file_ref in result.content:
                if file_ref.type == "bash_code_execution_output":
                    metadata = client.beta.files.retrieve_metadata(file_ref.file_id)
                    file_content = client.beta.files.download(file_ref.file_id)
                    # 使用 basename 过滤文件名防止路径穿越；验证结果
                    safe_name = os.path.basename(metadata.filename)
                    if not safe_name or safe_name in (".", ".."):
                        print(f"跳过无效文件名: {metadata.filename}")
                        continue
                    output_path = os.path.join(OUTPUT_DIR, safe_name)
                    file_content.write_to_file(output_path)
                    print(f"已保存: {output_path}")
\`\`\`

### 容器复用 (Container Reuse)

\`\`\`python
# 第一个请求：设置环境
response1 = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=4096,
    messages=[{"role": "user", "content": "安装 tabulate 并创建一个带有示例数据的 data.json 文件"}],
    tools=[{"type": "code_execution_20260120", "name": "code_execution"}]
)

# 从响应中获取容器 ID
container_id = response1.container.id

# 第二个请求：复用同一个容器
response2 = client.messages.create(
    container=container_id,
    model="{{OPUS_ID}}",
    max_tokens=4096,
    messages=[{"role": "user", "content": "读取 data.json 并以格式化表格形式展示"}],
    tools=[{"type": "code_execution_20260120", "name": "code_execution"}]
)
\`\`\`

### 响应结构

\`\`\`python
for block in response.content:
    if block.type == "text":
        print(block.text)  # Claude 的解释说明
    elif block.type == "server_tool_use":
        print(f"正在运行: {block.name} - {block.input}")  # Claude 正在执行的操作
    elif block.type == "bash_code_execution_tool_result":
        result = block.content
        if result.type == "bash_code_execution_result":
            if result.return_code == 0:
                print(f"输出: {result.stdout}")
            else:
                print(f"错误: {result.stderr}")
        else:
            print(f"工具错误: {result.error_code}")
    elif block.type == "text_editor_code_execution_tool_result":
        print(f"文件操作: {block.content}")
\`\`\`

---

## 记忆工具 (Memory Tool)

### 基础用法

\`\`\`python
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=2048,
    messages=[{"role": "user", "content": "请记住我偏好的语言是 Python。"}],
    tools=[{"type": "memory_20250818", "name": "memory"}],
)
\`\`\`

### SDK 记忆助手 (Memory Helper)

继承 \`BetaAbstractMemoryTool\`：

\`\`\`python
from anthropic.lib.tools import BetaAbstractMemoryTool

class MyMemoryTool(BetaAbstractMemoryTool):
    def view(self, command): ...
    def create(self, command): ...
    def str_replace(self, command): ...
    def insert(self, command): ...
    def delete(self, command): ...
    def rename(self, command): ...

memory = MyMemoryTool()

# 与工具运行器结合使用
runner = client.beta.messages.tool_runner(
    model="{{OPUS_ID}}",
    max_tokens=2048,
    tools=[memory],
    messages=[{"role": "user", "content": "记住我的偏好"}],
)

for message in runner:
    print(message)
\`\`\`

有关完整的实现示例，请使用 WebFetch 访问：

- \`https://github.com/anthropics/anthropic-sdk-python/blob/main/examples/memory/basic.py\`

---

## 结构化输出 (Structured Outputs)

### JSON 输出 (Pydantic — 推荐)

\`\`\`python
from pydantic import BaseModel
from typing import List
import anthropic

class ContactInfo(BaseModel):
    name: str
    email: str
    plan: str
    interests: List[str]
    demo_requested: bool

client = anthropic.Anthropic()

response = client.messages.parse(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    messages=[{
        "role": "user",
        "content": "提取信息: Jane Doe (jane@co.com) 想要企业版 (Enterprise), 对 API 和 SDK 感兴趣, 需要演示 (demo)。"
    }],
    output_format=ContactInfo,
)

# response.parsed_output 是一个通过验证的 ContactInfo 实例
contact = response.parsed_output
print(contact.name)           # "Jane Doe"
print(contact.interests)      # ["API", "SDKs"]
\`\`\`

### 严格工具使用 (Strict Tool Use)

\`\`\`python
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    messages=[{"role": "user", "content": "预订一张 3 月 15 日去东京的机票，2 名乘客"}],
    tools=[{
        "name": "book_flight",
        "description": "预订前往特定目的地的机票",
        "strict": True,
        "input_schema": {
            "type": "object",
            "properties": {
                "destination": {"type": "string"},
                "date": {"type": "string", "format": "date"},
                "passengers": {"type": "integer", "enum": [1, 2, 3, 4, 5, 6, 7, 8]}
            },
            "required": ["destination", "date", "passengers"],
            "additionalProperties": False
        }
    }]
)
\`\`\`
