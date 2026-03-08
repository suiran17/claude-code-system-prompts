<!--
name: 'Data: Streaming reference — Python'
description: Python 流式传输参考，包括同步/异步流式传输和处理不同内容类型
ccVersion: 2.1.63
-->
# 流式传输 (Streaming) — Python

## 快速入门

\`\`\`python
with client.messages.stream(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    messages=[{"role": "user", "content": "写一个故事"}]
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)
\`\`\`

### 异步 (Async)

\`\`\`python
async with async_client.messages.stream(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    messages=[{"role": "user", "content": "写一个故事"}]
) as stream:
    async for text in stream.text_stream:
        print(text, end="", flush=True)
\`\`\`

---

## 处理不同内容类型

Claude 可能会返回文本、思考块或工具使用请求。请进行相应处理：

> **Opus 4.6：** 使用 \`thinking: {type: "adaptive"}\`。在旧模型上，请转而使用 \`thinking: {type: "enabled", budget_tokens: N}\`。

\`\`\`python
with client.messages.stream(
    model="{{OPUS_ID}}",
    max_tokens=16000,
    thinking={"type": "adaptive"},
    messages=[{"role": "user", "content": "分析此问题"}]
) as stream:
    for event in stream:
        if event.type == "content_block_start":
            if event.content_block.type == "thinking":
                print("\\n[思考中...]")
            elif event.content_block.type == "text":
                print("\\n[回复:]")

        elif event.type == "content_block_delta":
            if event.delta.type == "thinking_delta":
                print(event.delta.thinking, end="", flush=True)
            elif event.delta.type == "text_delta":
                print(event.delta.text, end="", flush=True)
\`\`\`

---

## 带工具使用的流式传输

Python 的工具运行器 (Tool runner) 目前返回的是完整的消息。如果您在手动循环中需要每个 token 的实时流式处理并包含工具调用，请在单次 API 调用中使用流：

\`\`\`python
with client.messages.stream(
    model="{{OPUS_ID}}",
    max_tokens=4096,
    tools=tools,
    messages=messages
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)

    response = stream.get_final_message()
    # 如果 response.stop_reason == "tool_use"，则继续执行工具
\`\`\`

---

## 获取最终消息

\`\`\`python
with client.messages.stream(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    messages=[{"role": "user", "content": "你好"}]
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)

    # 流式传输结束后获取完整消息
    final_message = stream.get_final_message()
    print(f"\\n\\n使用的 Token 数: {final_message.usage.output_tokens}")
\`\`\`

---

## 带进度更新的流式传输

\`\`\`python
def stream_with_progress(client, **kwargs):
    """通过进度更新流式传输响应。"""
    total_tokens = 0
    content_parts = []

    with client.messages.stream(**kwargs) as stream:
        for event in stream:
            if event.type == "content_block_delta":
                if event.delta.type == "text_delta":
                    text = event.delta.text
                    content_parts.append(text)
                    print(text, end="", flush=True)

            elif event.type == "message_delta":
                if event.usage and event.usage.output_tokens is not None:
                    total_tokens = event.usage.output_tokens

        final_message = stream.get_final_message()

    print(f"\\n\\n[使用的 Token 数: {total_tokens}]")
    return "".join(content_parts)
\`\`\`

---

## 流中的错误处理

\`\`\`python
try:
    with client.messages.stream(
        model="{{OPUS_ID}}",
        max_tokens=1024,
        messages=[{"role": "user", "content": "写一个故事"}]
    ) as stream:
        for text in stream.text_stream:
            print(text, end="", flush=True)
except anthropic.APIConnectionError:
    print("\\n连接已断开。请重试。")
except anthropic.RateLimitError:
    print("\\n触发速率限制。请等待并重试。")
except anthropic.APIStatusError as e:
    print(f"\\nAPI 错误: {e.status_code}")
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

1. **务必刷新输出** —— 使用 \`flush=True\` 以立即显示 token
2. **处理部分响应** —— 如果流被中断，您可能会得到不完整的内容
3. **追踪 Token 使用情况** —— \`message_delta\` 事件包含使用信息
4. **使用超时 (Timeouts)** —— 为您的应用设置合适的超时时间
5. **默认为流式传输** —— 即使在流式传输时也使用 \`.get_final_message()\` 来获取完整响应，这能为您提供超时保护，而无需手动处理各个事件
