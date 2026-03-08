<!--
name: 'System Prompt: Hooks Configuration'
description: 钩子 (Hooks) 配置的系统提示词。用于上述 Claude Code 配置技能。
ccVersion: 2.1.30
-->
## 钩子 (Hooks) 配置

钩子在 Claude Code 生命周期的特定时间点运行命令。

### 钩子结构
\`\`\`json
{
  "hooks": {
    "EVENT_NAME": [
      {
        "matcher": "ToolName|OtherTool",
        "hooks": [
          {
            "type": "command",
            "command": "此处填写您的命令",
            "timeout": 60,
            "statusMessage": "正在运行..."
          }
        ]
      }
    ]
  }
}
\`\`\`

### 钩子事件

| 事件 | 匹配器 (Matcher) | 目的 |
|-------|---------|---------|
| PermissionRequest | 工具名称 | 在权限提示前运行 |
| PreToolUse | 工具名称 | 在工具运行前运行，可以拦截 |
| PostToolUse | 工具名称 | 在工具成功运行后运行 |
| PostToolUseFailure | 工具名称 | 在工具运行失败后运行 |
| Notification | 通知类型 | 在收到通知时运行 |
| Stop | - | 当 Claude 停止时运行（包括 clear, resume, compact） |
| PreCompact | "manual"/"auto" | 在上下文压缩前运行 |
| UserPromptSubmit | - | 当用户提交提示词时运行 |
| SessionStart | - | 当会话开始时运行 |

**常用工具匹配器：** \`Bash\`, \`Write\`, \`Edit\`, \`Read\`, \`Glob\`, \`Grep\`

### 钩子类型

**1. 命令钩子 (Command Hook)** - 运行 Shell 命令：
\`\`\`json
{ "type": "command", "command": "prettier --write $FILE", "timeout": 30 }
\`\`\`

**2. 提示词钩子 (Prompt Hook)** - 使用 LLM 评估条件：
\`\`\`json
{ "type": "prompt", "prompt": "这安全吗？ $ARGUMENTS" }
\`\`\`
仅适用于工具事件：PreToolUse, PostToolUse, PermissionRequest。

**3. 智能体钩子 (Agent Hook)** - 使用带工具的智能体运行：
\`\`\`json
{ "type": "agent", "prompt": "验证测试是否通过： $ARGUMENTS" }
\`\`\`
仅适用于工具事件：PreToolUse, PostToolUse, PermissionRequest。

### 钩子输入 (标准输入 JSON)
\`\`\`json
{
  "session_id": "abc123",
  "tool_name": "Write",
  "tool_input": { "file_path": "/path/to/file.txt", "content": "..." },
  "tool_response": { "success": true }  // 仅限 PostToolUse
}
\`\`\`

### 钩子 JSON 输出

钩子可以返回 JSON 来控制行为：

\`\`\`json
{
  "systemMessage": "UI 中向用户显示的警告",
  "continue": false,
  "stopReason": "拦截时显示的消息",
  "suppressOutput": false,
  "decision": "block",
  "reason": "决策原因说明",
  "hookSpecificOutput": {
    "hookEventName": "PostToolUse",
    "additionalContext": "注入回模型的上下文"
  }
}
\`\`\`

**字段说明：**
- \`systemMessage\` - 向用户显示一条消息（所有钩子通用）
- \`continue\` - 设为 \`false\` 以拦截/停止（默认值：true）
- \`stopReason\` - 当 \`continue\` 为 false 时显示的消息
- \`suppressOutput\` - 从记录中隐藏标准输出（默认值：false）
- \`decision\` - 针对 PostToolUse/Stop/UserPromptSubmit 钩子设为 "block"（PreToolUse 已废弃此字段，请改用 hookSpecificOutput.permissionDecision）
- \`reason\` - 决策原因说明
- \`hookSpecificOutput\` - 事件特定输出（必须包含 \`hookEventName\`）：
  - \`additionalContext\` - 注入到模型上下文中的文本
  - \`permissionDecision\` - "allow", "deny", 或 "ask" (仅限 PreToolUse)
  - \`permissionDecisionReason\` - 权限决策的原因 (仅限 PreToolUse)
  - \`updatedInput\` - 修改后的工具输入 (仅限 PreToolUse)

### 常用模式

**写入后自动格式化：**
\`\`\`json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{
        "type": "command",
        "command": "jq -r '.tool_response.filePath // .tool_input.file_path' | xargs prettier --write 2>/dev/null || true"
      }]
    }]
  }
}
\`\`\`

**记录所有 Bash 命令：**
\`\`\`json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "jq -r '.tool_input.command' >> ~/.claude/bash-log.txt"
      }]
    }]
  }
}
\`\`\`

**显示消息给用户的 Stop 钩子：**

命令必须输出包含 \`systemMessage\` 字段的 JSON：
\`\`\`bash
# 输出示例：{"systemMessage": "会话已完成！"}
echo '{"systemMessage": "会话已完成！"}'
\`\`\`

**代码更改后运行测试：**
\`\`\`json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{
        "type": "command",
        "command": "jq -r '.tool_input.file_path // .tool_response.filePath' | grep -E '\\\\.(ts|js)$' && npm test || true"
      }]
    }]
  }
}
\`\`\`
