<!--
name: 'Data: Claude API reference — Go'
description: Go SDK 参考
ccVersion: 2.1.63
-->
# Claude API — Go

> **注意：** Go SDK 支持 Claude API，并通过 \`BetaToolRunner\` 支持 Beta 版工具使用。Agent SDK 尚未在 Go 语言中提供。

## 安装

\`\`\`bash
go get github.com/anthropics/anthropic-sdk-go
\`\`\`

## 客户端初始化

\`\`\`go
import (
    "github.com/anthropics/anthropic-sdk-go"
    "github.com/anthropics/anthropic-sdk-go/option"
)

// 默认方式（使用 ANTHROPIC_API_KEY 环境变量）
client := anthropic.NewClient()

// 显式指定 API 密钥
client := anthropic.NewClient(
    option.WithAPIKey("your-api-key"),
)
\`\`\`

---

## 基础消息请求

\`\`\`go
response, err := client.Messages.New(context.TODO(), anthropic.MessageNewParams{
    Model:     anthropic.ModelClaudeOpus4_6,
    MaxTokens: 1024,
    Messages: []anthropic.MessageParam{
        anthropic.NewUserMessage(anthropic.NewTextBlock("法国的首都是哪里？")),
    },
})
if err != nil {
    log.Fatal(err)
}
fmt.Println(response.Content[0].Text)
\`\`\`

---

## 流式传输 (Streaming)

\`\`\`go
stream := client.Messages.NewStreaming(context.TODO(), anthropic.MessageNewParams{
    Model:     anthropic.ModelClaudeOpus4_6,
    MaxTokens: 1024,
    Messages: []anthropic.MessageParam{
        anthropic.NewUserMessage(anthropic.NewTextBlock("写一首俳句")),
    },
})

for stream.Next() {
    event := stream.Current()
    switch eventVariant := event.AsAny().(type) {
    case anthropic.ContentBlockDeltaEvent:
        switch deltaVariant := eventVariant.Delta.AsAny().(type) {
        case anthropic.TextDelta:
            fmt.Print(deltaVariant.Text)
        }
    }
}
if err := stream.Err(); err != nil {
    log.Fatal(err)
}
\`\`\`

---

## 工具使用 (Tool Use)

### 工具运行器（Tool Runner，Beta 版 —— 推荐）

**Beta：** Go SDK 通过 \`toolrunner\` 包提供了 \`BetaToolRunner\`，用于实现自动化的工具使用循环。

\`\`\`go
import (
    "context"
    "fmt"
    "log"

    "github.com/anthropics/anthropic-sdk-go"
    "github.com/anthropics/anthropic-sdk-go/toolrunner"
)

// 定义带有 jsonschema 标签的工具输入，用于自动生成架构 (Schema)
type GetWeatherInput struct {
    City string \`json:"city" jsonschema:"required,description=城市名称"\`
}

// 通过结构体标签自动生成架构创建工具
weatherTool, err := toolrunner.NewBetaToolFromJSONSchema(
    "get_weather",
    "获取指定城市的当前天气",
    func(ctx context.Context, input GetWeatherInput) (anthropic.BetaToolResultBlockParamContentUnion, error) {
        return anthropic.BetaToolResultBlockParamContentUnion{
            OfText: &anthropic.BetaTextBlockParam{
                Text: fmt.Sprintf("%s 的天气是晴天，72°F", input.City),
            },
        }, nil
    },
)
if err != nil {
    log.Fatal(err)
}

// 创建一个自动处理对话循环的工具运行器
runner := client.Beta.Messages.NewToolRunner(
    []anthropic.BetaTool{weatherTool},
    anthropic.BetaToolRunnerParams{
        BetaMessageNewParams: anthropic.BetaMessageNewParams{
            Model:     anthropic.ModelClaudeOpus4_6,
            MaxTokens: 1024,
            Messages: []anthropic.BetaMessageParam{
                anthropic.NewBetaUserMessage(anthropic.NewBetaTextBlock("巴黎的天气怎么样？")),
            },
        },
        MaxIterations: 5,
    },
)

// 运行直到 Claude 产生最终回复
message, err := runner.RunToCompletion(context.Background())
if err != nil {
    log.Fatal(err)
}
fmt.Println(message.Content[0].Text)
\`\`\`

**Go 工具运行器的核心特性：**

- 通过 \`jsonschema\` 标签从 Go 结构体自动生成架构 (Schema)
- \`RunToCompletion()\` 用于简单的单次使用
- \`All()\` 迭代器用于处理对话中的每一条消息
- \`NextMessage()\` 用于逐步迭代
- 通过 \`NewToolRunnerStreaming()\` 及其 \`AllStreaming()\` 提供流式变体

### 手动循环

对于精细控制，请通过 JSON 架构 (Schema) 使用原始工具定义。有关工具定义格式和智能体循环模式，请参阅 [共享工具使用概念](../shared/tool-use-concepts.md)。
