<!--
name: 'Data: Files API reference — Python'
description: Python 文件 API 参考，包含文件上传、列表显示、删除及在消息中的用法
ccVersion: 2.1.63
-->
# 文件 (Files) API — Python

文件 (Files) API 用于上传文件，以便在消息 API 请求中使用。请在内容块中通过 \`file_id\` 引用文件，这可以避免在多次 API 调用中重复上传。

**Beta：** 请在 API 调用中传递 \`betas=["files-api-2025-04-14"]\`（SDK 会自动设置所需的标头）。

## 关键信息

- 最大文件限制：500 MB
- 总存储额：每个组织 100 GB
- 除非显式删除，否则文件将持久保存
- 文件操作（上传、展示、删除）本身是免费的；但在消息中使用的内容会作为输入 Token 计费
- 不适用于 Amazon Bedrock 或 Google Vertex AI

---

## 上传文件 (Upload a File)

\`\`\`python
import anthropic

client = anthropic.Anthropic()

uploaded = client.beta.files.upload(
    file=("report.pdf", open("report.pdf", "rb"), "application/pdf"),
)
print(f"文件 ID: {uploaded.id}")
print(f"大小: {uploaded.size_bytes} 字节")
\`\`\`

---

## 在消息中使用文件

### PDF / 文本文件

\`\`\`python
response = client.beta.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "总结这份报告的关键发现。"},
            {
                "type": "document",
                "source": {"type": "file", "file_id": uploaded.id},
                "title": "第四季度报告",           # 可选
                "citations": {"enabled": True}   # 可选，开启引用功能
            }
        ]
    }],
    betas=["files-api-2025-04-14"],
)
print(response.content[0].text)
\`\`\`

### 图像

\`\`\`python
image_file = client.beta.files.upload(
    file=("photo.png", open("photo.png", "rb"), "image/png"),
)

response = client.beta.messages.create(
    model="{{OPUS_ID}}",
    max_tokens=1024,
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "这张图片里有什么？"},
            {
                "type": "image",
                "source": {"type": "file", "file_id": image_file.id}
            }
        ]
    }],
    betas=["files-api-2025-04-14"],
)
\`\`\`

---

## 管理文件

### 列出文件

\`\`\`python
files = client.beta.files.list()
for f in files.data:
    print(f"{f.id}: {f.filename} ({f.size_bytes} 字节)")
\`\`\`

### 获取文件元数据

\`\`\`python
file_info = client.beta.files.retrieve_metadata("file_011CNha8iCJcU1wXNR6q4V8w")
print(f"文件名: {file_info.filename}")
print(f"MIME 类型: {file_info.mime_type}")
\`\`\`

### 删除文件

\`\`\`python
client.beta.files.delete("file_011CNha8iCJcU1wXNR6q4V8w")
\`\`\`

### 下载文件

只有由代码执行工具或技能模块生成的文件可以被下载（用户上传的原始文件不可下载）。

\`\`\`python
file_content = client.beta.files.download("file_011CNha8iCJcU1wXNR6q4V8w")
file_content.write_to_file("output.txt")
\`\`\`

---

## 完整端到端示例

一次上传文档，进行多次提问：

\`\`\`python
import anthropic

client = anthropic.Anthropic()

# 1. 仅上传一次
uploaded = client.beta.files.upload(
    file=("contract.pdf", open("contract.pdf", "rb"), "application/pdf"),
)
print(f"已上传: {uploaded.id}")

# 2. 使用同一个 file_id 询问多个问题
questions = [
    "关键的条款和条件有哪些？",
    "终止条款是什么？",
    "总结一下付款时间表。",
]

for question in questions:
    response = client.beta.messages.create(
        model="{{OPUS_ID}}",
        max_tokens=1024,
        messages=[{
            "role": "user",
            "content": [
                {"type": "text", "text": question},
                {
                    "type": "document",
                    "source": {"type": "file", "file_id": uploaded.id}
                }
            ]
        }],
        betas=["files-api-2025-04-14"],
    )
    print(f"\\n问: {question}")
    print(f"答: {response.content[0].text[:200]}")

# 3. 完成后清理文件
client.beta.files.delete(uploaded.id)
\`\`\`
