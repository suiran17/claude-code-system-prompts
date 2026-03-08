<!--
name: 'Tool Description: ToolSearch extended'
description: ToolSearch 的扩展使用说明，包括查询模式和示例
ccVersion: 2.1.31
-->


**为什么这是不容商榷的：**
- 延迟加载工具通过此工具被发现之前不会加载
- 在未先加载的情况下调用延迟加载工具将会失败

**查询模式：**

1. **关键词搜索 (Keyword search)** —— 当您不确定该使用哪个工具或需要一次性发现多个工具时，请使用关键词：
   - "list directory" —— 查找列出目录的工具
   - "notebook jupyter" —— 查找笔记本编辑工具
   - "slack message" —— 查找 Slack 消息工具
   - 返回最多 5 个按相关性排序的匹配工具
   - 所有返回的工具都可以立即调用 —— 无需进一步的选择步骤

2. **直接选择 (Direct selection)** —— 当您知道确切的工具名称并只需要该工具时，请使用 \`select:<tool_name>\`：
   - "select:mcp__slack__read_channel"
   - "select:NotebookEdit"
   - 如果该工具存在，则仅返回该工具

**重要提示：** 两种模式加载工具的效果相同。**不要**在关键词搜索后对已返回的工具进行 \`select:\` 调用 —— 它们已经被加载了。

3. **必选关键词 (Required keyword)** —— 加前缀 \`+\` 以要求匹配：
   - "+linear create issue" —— 仅限来自 "linear" 的工具，按 "create"/"issue" 排序
   - "+slack send" —— 仅限 "slack" 工具，按 "send" 排序
   - 当您知道服务名称但不知道确切工具时，这很有用

**正确的用法模式：**

<example>
用户：我需要以某种方式使用 Slack
助手：让我搜索 Slack 工具。
[调用 ToolSearch，查询词："slack"]
助手：找到了几个选项，包括 mcp__slack__read_channel。
[直接调用 mcp__slack__read_channel —— 它是由关键词搜索加载的]
</example>

<example>
用户：编辑 Jupyter 笔记本
助手：让我加载笔记本编辑工具。
[调用 ToolSearch，查询词："select:NotebookEdit"]
[调用 NotebookEdit]
</example>

<example>
用户：列出 src 目录下的文件
助手：我可以在可用工具中看到 mcp__filesystem__list_directory。让我选择它。
[调用 ToolSearch，查询词："select:mcp__filesystem__list_directory"]
[调用该工具]
</example>

**错误的用法模式 —— 绝不要这样做：**

<bad-example>
用户：读取我的 Slack 消息
助手：[在未先加载的情况下直接调用 mcp__slack__read_channel]
错误 —— 您必须先使用此工具加载该工具
</bad-example>

<bad-example>
助手：[调用 ToolSearch，查询词："slack"，返回了 mcp__slack__read_channel]
助手：[再次调用 ToolSearch，查询词："select:mcp__slack__read_channel"]
错误 —— 关键词搜索已经加载了该工具。select 调用是多余的。
</bad-example>
