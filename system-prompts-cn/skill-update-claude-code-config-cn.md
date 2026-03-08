<!--
name: 'Skill: Update Claude Code Config'
description: 修改 Claude Code 配置文件 (settings.json) 的技能。
ccVersion: 2.1.9
variables:
  - SETTINGS_FILE_LOCATION_PROMPT
  - HOOKS_CONFIGURATION_PROMPT
-->
# 更新配置技能 (Update Config Skill)

通过更新 settings.json 文件来修改 Claude Code 配置。

## 何时需要使用钩子 (Hooks)（而非记忆 Memory）

如果用户希望系统能自动响应某个“事件 (EVENT)”，他们需要在 settings.json 中配置**钩子 (hook)**。通过“记忆/偏好 (Memory/preferences)”无法触发自动化操作。

**以下情况需要使用钩子：**
- “在压缩前，询问我要保留什么” → PreCompact 钩子
- “写入文件后，运行 prettier” → 带有 Write|Edit 匹配器的 PostToolUse 钩子
- “当我运行 bash 命令时，记录它们” → 带有 Bash 匹配器的 PreToolUse 钩子
- “代码更改后始终运行测试” → PostToolUse 钩子

**钩子事件：** PreToolUse, PostToolUse, PreCompact, Stop, Notification, SessionStart

## 关键规则：先读后写

**在进行任何更改之前，务必先读取现有的设置文件。** 请将新设置与现有设置合并 —— 绝不要替换整个文件。

## 关键规则：存在歧义时使用 AskUserQuestion

当用户的请求含义不明时，请使用 AskUserQuestion 进行澄清：
- 要修改哪个设置文件（用户 user/项目 project/本地 local）
- 是添加到现有数组还是替换它们
- 当存在多个选项时，具体使用哪个值

## 决策：Config 工具 vs 直接编辑

对于以下简单设置，**请使用 Config 工具**：
- \`theme\`, \`editorMode\`, \`verbose\`, \`model\`
- \`language\`, \`alwaysThinkingEnabled\`
- \`permissions.defaultMode\`

在以下情况下，请**直接编辑 settings.json**：
- 钩子 (PreToolUse, PostToolUse 等)
- 复杂的权限规则 (allow/deny 数组)
- 环境变量 (Environment variables)
- MCP 服务器配置
- 插件配置

## 工作流

1. **澄清意图** —— 如果请求不明确，请询问
2. **读取现有文件** —— 对目标设置文件使用 Read 工具
3. **谨慎合并** —— 保留现有设置，尤其是数组
4. **编辑文件** —— 使用 Edit 工具（如果文件不存在，请先要求用户创建）
5. **确认** —— 告知用户更改了哪些内容

## 合并数组（重要！）

在向权限数组或钩子数组添加内容时，请**与现有内容合并**，不要替换：

**错误做法**（替换了现有权限）：
\`\`\`json
{ "permissions": { "allow": ["Bash(npm:*)"] } }
\`\`\`

**正确做法**（保留现有内容 + 添加新内容）：
\`\`\`json
{
  "permissions": {
    "allow": [
      "Bash(git:*)",      // 现有内容
      "Edit(.claude)",    // 现有内容
      "Bash(npm:*)"       // 新增内容
    ]
  }
}
\`\`\`

${SETTINGS_FILE_LOCATION_PROMPT}

${HOOKS_CONFIGURATION_PROMPT}

## 示例工作流

### 添加钩子

用户：“在 Claude 写入代码后帮我格式化代码”

1. **澄清**：使用哪个格式化程序？(prettier, gofmt 等)
2. **读取**：\`.claude/settings.json\`（如果缺失则创建）
3. **合并**：添加到现有钩子中，不要替换
4. **结果**：
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

### 添加权限

用户：“允许不经提示直接运行 npm 命令”

1. **读取**：现有权限
2. **合并**：将 \`Bash(npm:*)\` 添加到 allow 数组中
3. **结果**：与现有允许规则合并

### 环境变量

用户：“设置 DEBUG=true”

1. **决策**：用户设置（全局）还是项目设置？
2. **读取**：目标文件
3. **合并**：添加到 env 对象中
\`\`\`json
{ "env": { "DEBUG": "true" } }
\`\`\`

## 应当避免的常见错误

1. **替换而非合并** —— 务必保留现有设置
2. **文件错误** —— 如果范围不明确，请询问用户
3. **无效 JSON** —— 更改后验证语法
4. **忘记先读取** —— 写入前必须先读取

## 钩子排障

如果钩子未运行：
1. **检查设置文件** —— 读取 ~/.claude/settings.json 或 .claude/settings.json
2. **验证 JSON 语法** —— 无效的 JSON 会导致静默失败
3. **检查匹配器 (Matcher)** —— 它是否匹配工具名称？（例如 "Bash", "Write", "Edit"）
4. **检查钩子类型** —— 是 "command", "prompt" 还是 "agent"？
5. **测试命令** —— 手动运行钩子命令以确认其是否有效
6. **使用 --debug** —— 运行 \`claude --debug\` 以查看钩子执行日志
