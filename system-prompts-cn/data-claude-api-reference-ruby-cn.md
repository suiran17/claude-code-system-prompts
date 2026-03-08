<!--
name: 'Data: Claude API reference — Ruby'
description: Ruby SDK 参考，包括安装、客户端初始化、基础请求、流式传输和 Beta 版工具运行器
ccVersion: 2.1.63
-->
# Claude API — Ruby

> **注意：** Ruby SDK 支持 Claude API。可以通过 \`client.beta.messages.tool_runner()\` 使用 Beta 版工具运行器。Agent SDK 尚未在 Ruby 语言中提供。

## 安装

\`\`\`bash
gem install anthropic
\`\`\`

## 客户端初始化

\`\`\`ruby
require "anthropic"

# 默认方式（使用 ANTHROPIC_API_KEY 环境变量）
client = Anthropic::Client.new

# 显式指定 API 密钥
client = Anthropic::Client.new(api_key: "your-api-key")
\`\`\`

---

## 基础消息请求

\`\`\`ruby
message = client.messages.create(
  model: :"{{OPUS_ID}}",
  max_tokens: 1024,
  messages: [
    { role: "user", content: "法国的首都是哪里？" }
  ]
)
puts message.content.first.text
\`\`\`

---

## 流式传输 (Streaming)

\`\`\`ruby
stream = client.messages.stream(
  model: :"{{OPUS_ID}}",
  max_tokens: 1024,
  messages: [{ role: "user", content: "写一首俳句" }]
)

stream.text.each { |text| print(text) }
\`\`\`

---

## 工具使用 (Tool Use)

Ruby SDK 支持通过原始 JSON 架构定义使用工具，还提供了一个 Beta 版工具运行器用于自动执行工具。

### 工具运行器 (Tool Runner - Beta)

\`\`\`ruby
class GetWeatherInput < Anthropic::BaseModel
  required :location, String, doc: "城市和州，例如 San Francisco, CA"
end

class GetWeather < Anthropic::BaseTool
  doc "获取给定位置的当前天气"

  input_schema GetWeatherInput

  def call(input)
    "#{input.location} 的天气是晴天，72°F。"
  end
end

client.beta.messages.tool_runner(
  model: :"{{OPUS_ID}}",
  max_tokens: 1024,
  tools: [GetWeather.new],
  messages: [{ role: "user", content: "旧金山的天气怎么样？" }]
).each_message do |message|
  puts message.content
end
\`\`\`

### 手动循环

有关工具定义格式和代理循环模式，请参阅 [共享工具使用概念](../shared/tool-use-concepts.md)。
