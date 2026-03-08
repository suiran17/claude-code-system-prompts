<!--
name: 'Data: Claude API reference — PHP'
description: PHP SDK 参考
ccVersion: 2.1.63
-->
# Claude API — PHP

> **注意：** 此 PHP SDK 是 Anthropic 官方为 PHP 提供的 SDK。不支持工具运行器 (Tool runner) 和 Agent SDK。支持 Bedrock、Vertex AI 和 Foundry 客户端。

## 安装

\`\`\`bash
composer require "anthropic-ai/sdk"
\`\`\`

## 客户端初始化

\`\`\`php
use Anthropic\\Client;

// 使用环境变量中的 API 密钥
$client = new Client(apiKey: getenv("ANTHROPIC_API_KEY"));
\`\`\`

### Amazon Bedrock

\`\`\`php
use Anthropic\\BedrockClient;

$client = new BedrockClient(
    region: 'us-east-1',
);
\`\`\`

### Google Vertex AI

\`\`\`php
use Anthropic\\VertexClient;

$client = new VertexClient(
    region: 'us-east5',
    projectId: 'my-project-id',
);
\`\`\`

### Anthropic Foundry

\`\`\`php
use Anthropic\\FoundryClient;

$client = new FoundryClient(
    authToken: getenv("ANTHROPIC_AUTH_TOKEN"),
);
\`\`\`

---

## 基础消息请求

\`\`\`php
$message = $client->messages->create(
    model: '{{OPUS_ID}}',
    maxTokens: 1024,
    messages: [
        ['role' => 'user', 'content' => '法国的首都是哪里？'],
    ],
);
echo $message->content[0]->text;
\`\`\`

---

## 流式传输 (Streaming)

\`\`\`php
$stream = $client->messages->createStream(
    model: '{{OPUS_ID}}',
    maxTokens: 1024,
    messages: [
        ['role' => 'user', 'content' => '写一首俳句'],
    ],
);

foreach ($stream as $event) {
    echo $event;
}
\`\`\`

---

## 工具使用 (手动循环)

PHP SDK 通过 JSON 架构 (Schema) 支持原始工具定义。有关工具定义格式和代理循环模式，请参阅 [共享工具使用概念](../shared/tool-use-concepts.md)。
