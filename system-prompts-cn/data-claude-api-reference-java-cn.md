<!--
name: 'Data: Claude API reference — Java'
description: Java SDK 参考，包括安装、客户端初始化、基础请求、流式传输和 Beta 版工具使用
ccVersion: 2.1.63
-->
# Claude API — Java

> **注意：** Java SDK 支持 Claude API 并通过注解类支持 Beta 版工具使用。Agent SDK 尚未在 Java 语言中提供。

## 安装

Maven：

\`\`\`xml
<dependency>
    <groupId>com.anthropic</groupId>
    <artifactId>anthropic-java</artifactId>
    <version>2.15.0</version>
</dependency>
\`\`\`

Gradle：

\`\`\`groovy
implementation("com.anthropic:anthropic-java:2.15.0")
\`\`\`

## 客户端初始化

\`\`\`java
import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;

// 默认方式（从环境中读取 ANTHROPIC_API_KEY）
AnthropicClient client = AnthropicOkHttpClient.fromEnv();

// 显式指定 API 密钥
AnthropicClient client = AnthropicOkHttpClient.builder()
    .apiKey("your-api-key")
    .build();
\`\`\`

---

## 基础消息请求

\`\`\`java
import com.anthropic.models.messages.MessageCreateParams;
import com.anthropic.models.messages.Message;
import com.anthropic.models.messages.Model;

MessageCreateParams params = MessageCreateParams.builder()
    .model(Model.CLAUDE_OPUS_4_6)
    .maxTokens(1024L)
    .addUserMessage("法国的首都是哪里？")
    .build();

Message response = client.messages().create(params);
response.content().stream()
    .flatMap(block -> block.text().stream())
    .forEach(textBlock -> System.out.println(textBlock.text()));
\`\`\`

---

## 流式传输 (Streaming)

\`\`\`java
import com.anthropic.core.http.StreamResponse;
import com.anthropic.models.messages.RawMessageStreamEvent;

MessageCreateParams params = MessageCreateParams.builder()
    .model(Model.CLAUDE_OPUS_4_6)
    .maxTokens(1024L)
    .addUserMessage("写一首俳句")
    .build();

try (StreamResponse<RawMessageStreamEvent> streamResponse = client.messages().createStreaming(params)) {
    streamResponse.stream()
        .flatMap(event -> event.contentBlockDelta().stream())
        .flatMap(deltaEvent -> deltaEvent.delta().text().stream())
        .forEach(textDelta -> System.out.print(textDelta.text()));
}
\`\`\`

---

## 工具使用 (Tool Use - Beta 版)

Java SDK 支持使用注解类进行 Beta 版工具使用。工具类实现 \`Supplier<String>\` 以便通过 \`BetaToolRunner\` 自动执行。

### 工具运行器 (Tool Runner - 自动循环)

\`\`\`java
import com.anthropic.models.beta.messages.MessageCreateParams;
import com.anthropic.models.beta.messages.BetaMessage;
import com.anthropic.helpers.BetaToolRunner;
import com.fasterxml.jackson.annotation.JsonClassDescription;
import com.fasterxml.jackson.annotation.JsonPropertyDescription;
import java.util.function.Supplier;

@JsonClassDescription("获取给定位置的天气")
static class GetWeather implements Supplier<String> {
    @JsonPropertyDescription("城市和州，例如 San Francisco, CA")
    public String location;

    @Override
    public String get() {
        return "旧金山的天气是晴天，72°F";
    }
}

BetaToolRunner toolRunner = client.beta().messages().toolRunner(
    MessageCreateParams.builder()
        .model("{{OPUS_ID}}")
        .maxTokens(1024L)
        .putAdditionalHeader("anthropic-beta", "structured-outputs-2025-11-13")
        .addTool(GetWeather.class)
        .addUserMessage("旧金山的天气怎么样？")
        .build());

for (BetaMessage message : toolRunner) {
    System.out.println(message);
}
\`\`\`

### 非 Beta 版工具使用

对于手动定义的 JSON 架构 (Schema)，也可以通过非 Beta 版的 \`com.anthropic.models.messages.MessageCreateParams\` 及其 \`addTool(Tool)\` 使用工具，无需进入 Beta 命名空间。Beta 命名空间仅在需要类注解便捷层（如 \`@JsonClassDescription\`, \`BetaToolRunner\`）时才需要。

### 手动循环

对于手动工具循环，请在请求中将工具定义为 JSON 架构格式，处理响应中的 \`tool_use\` 块，并发回 \`tool_result\`，不断循环直至 \`stop_reason\` 为 \`"end_turn"\`。有关代理循环模式，请参阅 [共享工具使用概念](../shared/tool-use-concepts.md)。
