<!--
name: 'Tool Description: SendMessageTool'
description: 向队友发送消息并在集群中处理协议请求/响应的工具
ccVersion: 2.1.32
-->

# SendMessageTool

向智能体队友发送消息，并在团队中处理协议请求/响应。

## 消息类型

### type: "message" - 发送直接消息

向**单个特定队友**发送消息。您必须指定接收者。

**给队友的重要提示**：您的纯文本输出对团队负责人或其他队友是**不可见**的。要与团队中的任何人交流，您**必须**使用此工具。仅在文本中输入回复或确认是不够的。

\`\`\`
{
  "type": "message",
  "recipient": "researcher",
  "content": "您要发送的消息",
  "summary": "关于 auth 模块的简短进度更新"
}
\`\`\`

- **recipient**：要发消息的队友名称（必填）
- **content**：消息文本（必填）
- **summary**：在 UI 预览中显示的 5-10 字摘要（必填）

### type: "broadcast" - 向所有队友广播消息（谨慎使用）

一次性向团队中的**所有人**发送相同的消息。

**警告：广播的开销很大。** 每次广播都会分别向每位队友发送一条消息，这意味着：
- N 个队友 = N 次独立的消息传递
- 每次传递都会消耗 API 资源
- 成本随团队规模呈线性增长

\`\`\`
{
  "type": "broadcast",
  "content": "向所有队友发送的消息",
  "summary": "发现关键阻塞问题"
}
\`\`\`

- **content**：要广播的消息内容（必填）
- **summary**：在 UI 预览中显示的 5-10 字摘要（必填）

**关键提示：仅在绝对必要时使用广播。** 合理的场景包括：
- 需要全团队立即关注的关键问题（例如：“停止所有工作，发现阻塞性 Bug”）
- 真正平等影响到每位队友的重大公告

**默认使用 "message" 而非 "broadcast"。** 在以下情况使用 "message"：
- 回复单个队友
- 正常的往返沟通
- 与某人跟进任务
- 分享仅与某些队友相关的发现
- 任何不需要所有人关注的消息

### type: "shutdown_request" - 请求队友关闭

使用此类型请求队友优雅地关闭：

\`\`\`
{
  "type": "shutdown_request",
  "recipient": "researcher",
  "content": "任务完成，正在结束会话"
}
\`\`\`

队友将收到关闭请求，并可以选择批准（退出）或拒绝（继续工作）。

### type: "shutdown_response" - 回应关闭请求

#### 批准关闭

当您收到类型为 \`type: "shutdown_request"\` 的 JSON 格式的关闭请求时，您**必须**回应批准或拒绝。不要仅在文本中确认请求——您必须实际调用此工具。

\`\`\`
{
  "type": "shutdown_response",
  "request_id": "abc-123",
  "approve": true
}
\`\`\`

**重要提示**：从 JSON 消息中提取 \`requestId\` 并作为 \`request_id\` 传递给工具。仅仅说“我会关闭”是不够的——您必须调用该工具。

这将向负责人发送确认并终止您的进程。

#### 拒绝关闭

\`\`\`
{
  "type": "shutdown_response",
  "request_id": "abc-123",
  "approve": false,
  "content": "仍在此任务 #3 上工作，还需要 5 分钟"
}
\`\`\`

负责人将收到您的拒绝及理由。

### type: "plan_approval_response" - 批准或拒绝队友的计划

#### 批准计划

当设置了 \`plan_mode_required\` 的队友调用 ExitPlanMode 时，他们会向您发送一个类型为 \`type: "plan_approval_request"\` 的 JSON 格式的计划审批请求。使用此类型批准他们的计划：

\`\`\`
{
  "type": "plan_approval_response",
  "request_id": "abc-123",
  "recipient": "researcher",
  "approve": true
}
\`\`\`

批准后，该队友将自动退出计划模式并可以继续进行实现。

#### 拒绝计划

\`\`\`
{
  "type": "plan_approval_response",
  "request_id": "abc-123",
  "recipient": "researcher",
  "approve": false,
  "content": "请为 API 调用添加错误处理"
}
\`\`\`

该队友将收到拒绝及您的反馈，并可以修改其计划。

## 重要提示

- 队友发送的消息会自动送达给您。您无需手动检查收件箱。
- 在汇报队友消息时，您无需引用原消息——因为它已经呈现给用户了。
- **重要提示**：始终通过队友的**名称**（例如 "team-lead"、"researcher"、"tester"）来称呼他们，绝不要使用 UUID。
- 不要发送结构化的 JSON 状态消息。请使用 TaskUpdate 将任务标记为已完成，系统会在您停止时自动发送空闲通知。
