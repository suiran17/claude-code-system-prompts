<!--
name: 'Data: Message Batches API reference — Python'
description: Python Batches API 参考，包含批量创建、状态轮询和以 50% 成本获取结果的方法
ccVersion: 2.1.63
-->
# 消息批量处理 (Message Batches) API — Python

批量处理 (Batches) API (\`POST /v1/messages/batches\`) 以标准价格的 50% 异步处理消息。

## 关键信息

- 每批次最高支持 100,000 个请求或 256 MB 数据
- 大多数批次在 1 小时内完成；最长不超过 24 小时
- 结果在创建后保留 29 天
- 所有 Token 消耗享受 50% 的成本减免
- 支持所有的消息 API 特性（视觉、工具、缓存等）

---

## 创建批次 (Create a Batch)

\`\`\`python
import anthropic
from anthropic.types.message_create_params import MessageCreateParamsNonStreaming
from anthropic.types.messages.batch_create_params import Request

client = anthropic.Anthropic()

message_batch = client.messages.batches.create(
    requests=[
        Request(
            custom_id="request-1",
            params=MessageCreateParamsNonStreaming(
                model="{{OPUS_ID}}",
                max_tokens=1024,
                messages=[{"role": "user", "content": "总结气候变化的影响"}]
            )
        ),
        Request(
            custom_id="request-2",
            params=MessageCreateParamsNonStreaming(
                model="{{OPUS_ID}}",
                max_tokens=1024,
                messages=[{"role": "user", "content": "解释量子计算基础"}]
            )
        ),
    ]
)

print(f"批次 ID: {message_batch.id}")
print(f"状态: {message_batch.processing_status}")
\`\`\`

---

## 轮询完成情况 (Poll for Completion)

\`\`\`python
import time

while True:
    batch = client.messages.batches.retrieve(message_batch.id)
    if batch.processing_status == "ended":
        break
    print(f"状态: {batch.processing_status}, 正在处理: {batch.request_counts.processing}")
    time.sleep(60)

print("批次已完成！")
print(f"成功: {batch.request_counts.succeeded}")
print(f"失败: {batch.request_counts.errored}")
\`\`\`

---

## 获取结果 (Retrieve Results)

> **注意：** 下面的示例使用了 \`match/case\` 语法，需要 Python 3.10+ 环境。对于更早的版本，请使用 \`if/elif\` 链。

\`\`\`python
for result in client.messages.batches.results(message_batch.id):
    match result.result.type:
        case "succeeded":
            print(f"[{result.custom_id}] {result.result.message.content[0].text[:100]}")
        case "errored":
            if result.result.error.type == "invalid_request":
                print(f"[{result.custom_id}] 验证错误 - 请修正请求后重试")
            else:
                print(f"[{result.custom_id}] 服务器错误 - 可安全重试")
        case "canceled":
            print(f"[{result.custom_id}] 已取消")
        case "expired":
            print(f"[{result.custom_id}] 已过期 - 请重新提交")
\`\`\`

---

## 取消批次 (Cancel a Batch)

\`\`\`python
cancelled = client.messages.batches.cancel(message_batch.id)
print(f"状态: {cancelled.processing_status}")  # "canceling"
\`\`\`

---

## 结合 Prompt 缓存使用批量处理

\`\`\`python
shared_system = [
    {"type": "text", "text": "你是一位文学分析师。"},
    {
        "type": "text",
        "text": large_document_text,  # 在所有请求中共享的大型文档内容
        "cache_control": {"type": "ephemeral"}
    }
]

message_batch = client.messages.batches.create(
    requests=[
        Request(
            custom_id=f"analysis-{i}",
            params=MessageCreateParamsNonStreaming(
                model="{{OPUS_ID}}",
                max_tokens=1024,
                system=shared_system,
                messages=[{"role": "user", "content": question}]
            )
        )
        for i, question in enumerate(questions)
    ]
)
\`\`\`

---

## 完整端到端示例

\`\`\`python
import anthropic
import time
from anthropic.types.message_create_params import MessageCreateParamsNonStreaming
from anthropic.types.messages.batch_create_params import Request

client = anthropic.Anthropic()

# 1. 准备请求
items_to_classify = [
    "产品质量非常好！",
    "客服太糟糕了，再也不会购买。",
    "还行，没什么特别的。",
]

requests = [
    Request(
        custom_id=f"classify-{i}",
        params=MessageCreateParamsNonStreaming(
            model="{{HAIKU_ID}}",
            max_tokens=50,
            messages=[{
                "role": "user",
                "content": f"请将其分类为 positive/negative/neutral (仅输出一个单词): {text}"
            }]
        )
    )
    for i, text in enumerate(items_to_classify)
]

# 2. 创建批次
batch = client.messages.batches.create(requests=requests)
print(f"已创建批次: {batch.id}")

# 3. 等待完成
while True:
    batch = client.messages.batches.retrieve(batch.id)
    if batch.processing_status == "ended":
        break
    time.sleep(10)

# 4. 收集结果
results = {}
for result in client.messages.batches.results(batch.id):
    if result.result.type == "succeeded":
        results[result.custom_id] = result.result.message.content[0].text

for custom_id, classification in sorted(results.items()):
    print(f"{custom_id}: {classification}")
\`\`\`
