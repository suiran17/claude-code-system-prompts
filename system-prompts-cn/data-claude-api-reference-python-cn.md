<!--
name: 'Data: Claude API reference — Python'
description: Python SDK 参考，包括安装、客户端初始化、基础请求、思考和多轮对话
ccVersion: 2.1.63
-->
# Claude API — Python

## 安装

\`\`\`bash
pip install anthropic
\`\`\`

## 客户端初始化

\`\`\`python
import anthropic

# 默认方式（使用 ANTHROPIC_API_KEY 环境变量）
client = anthropic.Anthropic()

# 显式指定 API 密钥
client = anthropic.Anthropic(api_key="your-api-key")

# 异步客户端
async_client = anthropic.AsyncAnthropic()
\`\`\`

---

## 基础消息请求

\`\`\`python
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "法国的首都是哪里？"}
    ]
)
print(response.content[0].text)
\`\`\`

---

## 系统提示词 (System Prompts)

\`\`\`python
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    system="你是一位有用的编码助手。务必用 Python 提供示例。",
    messages=[{"role": "user", "content": "如何读取 JSON 文件？"}]
)
\`\`\`

---

## 视觉（图像识别）

### Base64 方式

\`\`\`python
import base64

with open("image.png", "rb") as f:
    image_data = base64.standard_b64encode(f.read()).decode("utf-8")

response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    messages=[{
        "role": "user",
        "content": [
            {
                "type": "image",
                "source": {
                    "type": "base64",
                    "media_type": "image/png",
                    "data": image_data
                }
            },
            {"type": "text", "text": "这张图片里有什么？"}
        ]
    }]
)
\`\`\`

### URL 方式

\`\`\`python
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    messages=[{
        "role": "user",
        "content": [
            {
                "type": "image",
                "source": {
                    "type": "url",
                    "url": "https://example.com/image.png"
                }
            },
            {"type": "text", "text": "描述这张图片"}
        ]
    }]
)
\`\`\`

---

## Prompt 缓存 (Prompt Caching)

缓存大型上下文以降低成本（最高可节省 90%）。

### 自动缓存（推荐）

使用顶级的 \`cache_control\` 自动缓存请求中的最后一个可缓存块 —— 无需标注单个内容块：

\`\`\`python
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    cache_control={"type": "ephemeral"},  # 自动缓存最后一个可缓存块
    system="你是一份大型文档的专家...",
    messages=[{"role": "user", "content": "总结关键点"}]
)
\`\`\`

### 手动缓存控制

如需精细控制，可在特定内容块中添加 \`cache_control\`：

\`\`\`python
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    system=[{
        "type": "text",
        "text": "你是一份大型文档的专家...",
        "cache_control": {"type": "ephemeral"}  # 默认 TTL 为 5 分钟
    }],
    messages=[{"role": "user", "content": "总结关键点"}]
)

# 使用显式 TTL (生存时间)
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    system=[{
        "type": "text",
        "text": "你是一份大型文档的专家...",
        "cache_control": {"type": "ephemeral", "ttl": "1h"}  # 1 小时 TTL
    }],
    messages=[{"role": "user", "content": "总结关键点"}]
)
\`\`\`

---

## 扩展思考 (Extended Thinking)

> **Opus 4.6 和 Sonnet 4.6：** 使用适应性思考 (Adaptive thinking)。\`budget_tokens\` 在 Opus 4.6 和 Sonnet 4.6 上均已弃用。
> **旧模型：** 使用 \`thinking: {type: "enabled", budget_tokens: N}\`（必须小于 \`max_tokens\`，最小值为 1024）。

\`\`\`python
# Opus 4.6: 适应性思考（推荐）
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=16000,
    thinking={"type": "adaptive"},
    output_config={"effort": "high"},  # low | medium | high | max
    messages=[{"role": "user", "content": "逐步解决此问题..."}]
)

# 访问思考过程和回复内容
for block in response.content:
    if block.type == "thinking":
        print(f"思考中: {block.thinking}")
    elif block.type == "text":
        print(f"回复: {block.text}")
\`\`\`

---

## 错误处理

\`\`\`python
import anthropic

try:
    response = client.messages.create(...)
except anthropic.BadRequestError as e:
    print(f"坏请求: {e.message}")
except anthropic.AuthenticationError:
    print("无效的 API 密钥")
except anthropic.PermissionDeniedError:
    print("API 密钥缺少所需权限")
except anthropic.NotFoundError:
    print("无效的模型或端点")
except anthropic.RateLimitError as e:
    retry_after = int(e.response.headers.get("retry-after", "60"))
    print(f"触发频率限制。请在 {retry_after} 秒后重试。")
except anthropic.APIStatusError as e:
    if e.status_code >= 500:
        print(f"服务器错误 ({e.status_code})。请稍后重试。")
    else:
        print(f"API 错误: {e.message}")
except anthropic.APIConnectionError:
    print("网络错误。请检查互联网连接。")
\`\`\`

---

## 多轮对话 (Multi-Turn Conversations)

API 是无 stateless 的 —— 每次连接时都需发送完整的对话历史。

\`\`\`python
class ConversationManager:
    """管理与 Claude API 的多轮对话。"""

    def __init__(self, client: anthropic.Anthropic, model: str, system: str = None):
        self.client = client
        self.model = model
        self.system = system
        self.messages = []

    def send(self, user_message: str, **kwargs) -> str:
        """发送消息并获取回复。"""
        self.messages.append({"role": "user", "content": user_message})

        response = self.client.messages.create(
            model=self.model,
            max_tokens=kwargs.get("max_tokens", 1024),
            system=self.system,
            messages=self.messages,
            **kwargs
        )

        assistant_message = response.content[0].text
        self.messages.append({"role": "assistant", "content": assistant_message})

        return assistant_message

# 使用方法
conversation = ConversationManager(
    client=anthropic.Anthropic(),
    model="{{OPUS_ID}}",
    system="你是一位有用的助手。"
)

response1 = conversation.send("我的名字是 Alice。")
response2 = conversation.send("我的名字是什么？")  # Claude 记得 "Alice"
\`\`\`

**规则：**

- 消息必须在 \`user\` 和 \`assistant\` 之间交替进行
- 第一条消息必须是 \`user\`

---

### 对话压缩（长对话）

> **Beta 功能，仅限 Opus 4.6。** 当对话接近 200K 上下文窗口时，压缩功能会自动在服务端总结早期上下文。API 会返回一个 \`compaction\` 块；您必须在后续请求中将其传回 —— 请追加 \`response.content\`，而不仅仅是文本。

\`\`\`python
import anthropic

client = anthropic.Anthropic()
messages = []

def chat(user_message: str) -> str:
    messages.append({"role": "user", "content": user_message})

    response = client.beta.messages.create(
        betas=["compact-2026-01-12"],
        model="{{OPUS_ID}}",
        max_tokens=4096,
        messages=messages,
        context_management={
            "edits": [{"type": "compact_20260112"}]
        }
    )

    # 追加完整内容 —— 必须保留压缩块 (Compaction blocks)
    messages.append({"role": "assistant", "content": response.content})

    return next(block.text for block in response.content if block.type == "text")

# 当上下文变长时自动触发压缩
print(chat("帮我构建一个 Python 网页爬虫"))
print(chat("添加对 JavaScript 渲染页面的支持"))
print(chat("现在添加频率限制和错误处理"))
\`\`\`

---

## 停止原因 (Stop Reasons)

回复中的 \`stop_reason\` 字段指明了模型停止生成的原因：

| 值              | 意义                                                         |
| --------------- | ------------------------------------------------------------ |
| \`end_turn\`      | Claude 自然地完成了其回复                                    |
| \`max_tokens\`    | 达到了 \`max_tokens\` 限制 —— 请增加限制或使用流式传输       |
| \`stop_sequence\` | 达到了自定义停止序列                                         |
| \`tool_use\`      | Claude 想要调用工具 —— 请执行该工具并继续                    |
| \`pause_turn\`    | 模型暂停并可以恢复（智能体工作流）                             |
| \`refusal\`       | Claude 出于安全原因拒绝 —— 输出可能不符合您的架构要求        |

---

## 成本优化策略

### 1. 为重复内容使用 Prompt 缓存

\`\`\`python
# 自动缓存（最简便 —— 缓存最后一个可缓存块）
response = client.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    cache_control={"type": "ephemeral"},
    system=large_document_text,  # 例如 50KB 的上下文
    messages=[{"role": "user", "content": "总结关键点"}]
)

# 第一次请求：全额支出
# 后续请求：已缓存部分约可节省 90% 的成本
\`\`\`

### 2. 选择正确的模型

\`\`\`python
# 对于大多数任务，默认为 Opus
response = client.messages.create(
    model="{{OPUS_ID}}",  # 每 100 万 token 为 $5.00/$25.00
    max_tokens=1024,
    messages=[{"role": "user", "content": "解释量子计算"}]
)

# 针对高吞吐量的生产环境负载，使用 Sonnet
standard_response = client.messages.create(
    model="{{SONNET_ID}}",  # 每 100 万 token 为 $3.00/$15.00
    max_tokens=1024,
    messages=[{"role": "user", "content": "总结这份文档"}]
)

# Haiku 仅用于简单且对速度敏感的任务
simple_response = client.messages.create(
    model="{{HAIKU_ID}}",  # 每 100 万 token 为 $1.00/$5.00
    max_tokens=256,
    messages=[{"role": "user", "content": "将此分类为正面或反面"}]
)
\`\`\`

### 3. 请求前进行 Token 计数

\`\`\`python
count_response = client.messages.count_tokens(
    model="{{OPUS_ID}}",
    messages=messages,
    system=system
)

estimated_input_cost = count_response.input_tokens * 0.000005  # $5/100万 token
print(f"预计输入成本: \${estimated_input_cost:.4f}")
\`\`\`

---

## 指数退避重试 (Retry with Exponential Backoff)

> **注意：** Anthropic SDK 会针对频率限制 (429) 和服务器错误 (5xx) 自动进行指数退避重试。您可以通过 \`max_retries\`（默认值：2）来配置此项。只有在您需要 SDK 提供以外的行为时，才需实现自定义重试逻辑。

\`\`\`python
import time
import random
import anthropic

def call_with_retry(
    client: anthropic.Anthropic,
    max_retries: int = 5,
    base_delay: float = 1.0,
    max_delay: float = 60.0,
    **kwargs
):
    """通过指数退避重试调用 API。"""
    last_exception = None

    for attempt in range(max_retries):
        try:
            return client.messages.create(**kwargs)
        except anthropic.RateLimitError as e:
            last_exception = e
        except anthropic.APIStatusError as e:
            if e.status_code >= 500:
                last_exception = e
            else:
                raise  # 客户端错误（除 429 以外的 4xx）不应重试

        delay = min(base_delay * (2 ** attempt) + random.uniform(0, 1), max_delay)
        print(f"尝试重试 {attempt + 1}/{max_retries}，将在 {delay:.1f} 秒后进行")
        time.sleep(delay)

    raise last_exception
\`\`\`
