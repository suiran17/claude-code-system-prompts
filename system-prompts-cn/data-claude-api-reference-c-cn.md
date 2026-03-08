<!--
name: 'Data: Claude API reference — C#'
description: C# SDK 参考，包括安装、客户端初始化、基础请求、流式传输和工具使用
ccVersion: 2.1.51
-->
# Claude API — C#

> **注意：** 此 C# SDK 是 Anthropic 官方为 C# 提供的 SDK。通过 Messages API 支持工具使用。目前没有提供基于类注解的工具运行器；请直接通过 JSON 架构 (Schema) 使用原始工具定义。该 SDK 还支持与带有函数调用能力的 Microsoft.Extensions.AI IChatClient 进行集成。

## 安装

\`\`\`bash
dotnet add package Anthropic
\`\`\`

## 客户端初始化

\`\`\`csharp
using Anthropic;

// 默认方式（使用 ANTHROPIC_API_KEY 环境变量）
AnthropicClient client = new();

// 显式指定 API 密钥（使用环境变量 —— 绝不硬编码密钥）
AnthropicClient client = new() {
    ApiKey = Environment.GetEnvironmentVariable("ANTHROPIC_API_KEY")
};
\`\`\`

---

## 基础消息请求

\`\`\`csharp
using Anthropic.Models.Messages;

var parameters = new MessageCreateParams
{
    Model = Model.ClaudeOpus4_6,
    MaxTokens = 1024,
    Messages = [new() { Role = Role.User, Content = "法国的首都是哪里？" }]
};
var message = await client.Messages.Create(parameters);
Console.WriteLine(message);
\`\`\`

---

## 流式传输 (Streaming)

\`\`\`csharp
using Anthropic.Models.Messages;

var parameters = new MessageCreateParams
{
    Model = Model.ClaudeOpus4_6,
    MaxTokens = 1024,
    Messages = [new() { Role = Role.User, Content = "写一首俳句" }]
};

await foreach (RawMessageStreamEvent streamEvent in client.Messages.CreateStreaming(parameters))
{
    if (streamEvent.TryPickContentBlockDelta(out var delta) &&
        delta.Delta.TryPickText(out var text))
    {
        Console.Write(text.Text);
    }
}
\`\`\`

---

## 工具使用 (手动循环)

C# SDK 通过 JSON 架构 (Schema) 支持原始工具定义。有关工具定义格式和代理循环模式，请参阅 [共享工具使用概念](../shared/tool-use-concepts.md)。
