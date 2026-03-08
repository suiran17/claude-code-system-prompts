<!--
注意：仅对全新的提示词文件使用 **新增：**，而非现有提示词中的新添加/章节。
-->

### Claude Code 系统提示词变更日志 (Changelog)

#### [2.1.63](https://github.com/Piebald-AI/claude-code-system-prompts/commit/7e37a33)

_+4,200 tokens_

- **新增：** 智能体提示词：/batch 斜杠命令 —— 编排跨越代码库的大规模并行更改的指令。
- **新增：** 系统提示词：执行任务者指令 —— 执行更改时工作者应遵循的指令。
- **移除：** 智能体提示词：Bash 命令文件路径提取 —— 从 Bash 命令输出中提取文件路径的系统提示词。
- **移除：** 技能：使用 Claude API 构建 (触发器) —— “使用 Claude API 构建”技能的激活准则。
- **移除：** 系统提醒：待办事项列表已更改 —— 待办事项列表发生更改的通知。
- **移除：** 系统提醒：待办事项列表为空 —— 提醒待办事项列表为空。
- 数据：Claude API 参考 — Go —— 在 `toolrunner` 包中添加了 `BetaToolRunner` 文档；将工具使用重组为“工具运行器 (Beta)”和“手动循环”章节。
- 数据：Claude API 参考 — PHP —— 添加了 Bedrock、Vertex AI 和 Foundry 客户端初始化示例；从安装命令中移除了版本锁定。
- 数据：Claude API 参考 — Java —— 将 SDK 版本从 2.14.0 更新至 2.15.0。
- 数据：Claude API 参考 — Python —— 添加了自动缓存章节，以便在现有的手动缓存控制之外简化提示词缓存。
- 数据：Claude API 参考 — TypeScript —— 添加了自动缓存章节、类型化错误处理指南、SDK 类型指南（`Anthropic.MessageParam` 等）以及多轮对话类型支持改进。
- 数据：HTTP 错误代码参考 —— 添加了将 HTTP 代码映射到 TypeScript 和 Python 异常类的类型化异常表，并附带正确/错误使用示例。
- 数据：工具使用概念 —— 将工具运行器的适用范围扩展到 Java、Go 和 Ruby；通过代码示例和 `max_continuations` 指南改进了 `pause_turn` 处理；简化了动态过滤（不再需要单独的 `code_execution` 工具或 Beta 标头）。
- 数据：工具使用参考 — TypeScript —— 添加了流式传输手动循环章节，结合使用 `stream()` + `finalMessage()` 与工具使用循环；添加了 `pause_turn` 处理；在全文中添加了 SDK 类型注解和错误处理指南。
- 数据：工具使用参考 — Python —— 在手动智能体循环中添加了 `pause_turn` 处理。
- 数据：流式传输参考 — TypeScript —— 增强了最佳实践：扩展了 `finalMessage()` 指南，添加了 `stream.on("text")` 提示，并添加了智能体循环流式传输的交叉引用。
- 数据：Claude 模型目录 —— 将 Claude Haiku 3 从当前模型移至弃用模型。
- 技能：使用 Claude API 构建 —— 更新了 Go SDK 以显示对 Beta 版工具运行器的支持；添加了反对重新实现 SDK 功能、重新定义 SDK 类型的指南，以及通过代码执行沙箱输出报告/文档的指南。
- 智能体 SDK 参考与模式 (Python, TypeScript) —— 在允许的工具、工具表和代码示例中，将 `Task` 工具重命名为 `Agent`。
- 智能体提示词：会话总结 —— 修复了列表缩进并纠正了重复的章节编号（两个章节 6 → 6, 7）。
- 系统提醒：计划模式已激活 (5 阶段) —— 简化了模板变量并移除了多个变量声明。
- 系统提醒：计划模式已激活 (迭代式) —— 重组了计划文件信息的呈现方式并简化了变量引用。
- 工具描述 (EnterPlanMode, TeammateTool) —— 将 `Task` 工具的引用重命名为 `Agent`。
- 全文中的硬编码模型 ID（如 `claude-opus-4-6`）已被替换为模板变量（如 `{{OPUS_ID}}`），涉及所有 SDK 参考、数据和技能文件。

#### [2.1.62](https://github.com/Piebald-AI/claude-code-system-prompts/commit/5e65215)

<sub>_v2.1.62 版本中系统提示词无变更。_</sub>

#### [2.1.61](https://github.com/Piebald-AI/claude-code-system-prompts/commit/c197152)

<sub>_v2.1.61 版本中系统提示词无变更。_</sub>

#### [2.1.59](https://github.com/Piebald-AI/claude-code-system-prompts/commit/6147099)

_-493 tokens_

- **移除：** 数据：Claude Code 版本不匹配警告 —— 包含更新指令的旧版本 Claude Code 警告。
- **移除：** 系统提醒：挂钩 JSON 验证失败 —— 挂钩 JSON 输出未通过模式验证时显示的错误消息。

#### [2.1.58](https://github.com/Piebald-AI/claude-code-system-prompts/commit/e92625f)

<sub>_v2.1.58 版本中系统提示词无变更。_</sub>

#### [2.1.56](https://github.com/Piebald-AI/claude-code-system-prompts/commit/3d084a9)

<sub>_v2.1.56 版本中系统提示词无变更。_</sub>

#### [2.1.55](https://github.com/Piebald-AI/claude-code-system-prompts/commit/97cca68)

<sub>_v2.1.55 版本中系统提示词无变更。_</sub>

#### [2.1.54](https://github.com/Piebald-AI/claude-code-system-prompts/commit/ca8e3dd)

<sub>_v2.1.54 版本中系统提示词无变更。_</sub>

#### [2.1.53](https://github.com/Piebald-AI/claude-code-system-prompts/commit/f7330d2)

_-617 tokens_

- **新增：** 智能体提示词：记忆选择 —— 针对用户查询选择相关记忆的指令 (156 tks)。
- **移除：** 智能体提示词：命令执行专家 —— 移除了用于运行 Bash 命令的命令执行专家智能体 (109 tks)。
- **移除：** 系统提示词：主系统提示词 —— 移除了独立的身份核心提示词；内容已被整合至其他提示词章节 (269 tks)。
- 工具描述：任务 (Task) —— 后台智能体现在在完成后会自动通知，而非提供输出文件路径；明确禁止休眠、轮询或主动检查 (1317 → 1331 tks)。
- 工具描述：写入 (Write) —— 澄清了 Write 与 Edit 的使用指南：修改优先使用 Edit（仅发送差异），新文件或完整重写保留使用 Write (127 → 129 tks)。
- 将 6 个庞大的系统提示词和 2 个工具描述广泛分解为约 70 个较小的原子文件。内容基本保留但重组为可独立调用的单元，并包含一些新的子提示词（如“宏大任务”、“受阻处理”、“代码引用”）和重新分配的内容（如“不提供时间预估”从“语气与风格”移至“执行任务”）：
  - 系统提示词：执行任务 (437 tks) → 拆分为 13 个文件，涵盖软件工程聚焦、修改前必读、安全性、过度设计、非必要添加、错误处理、过早抽象、兼容性补丁、文件创建、时间预估、帮助/反馈、宏大任务以及受阻处理。
  - 系统提示词：语气与风格 (500 tks) → 拆分为 3 个文件，涵盖代码引用、简洁输出（详细版）以及简洁输出（简短版）。
  - 系统提示词：工具使用策略 (352 tks) → 拆分为 11 个文件，涵盖创建/编辑/读取/搜索文件、Bash 保留、内容搜索、委派探索、直接搜索、技能调用、子智能体引导以及任务管理。
  - 系统提示词：任务管理 (565 tks) → 合并至“工具使用（任务管理）”子提示词 (73 tks)。
  - 系统提示词：条件性委派代码库探索 (249 tks) → 合并至“工具使用（委派探索）”子提示词 (114 tks)。
  - 工具描述：Bash (1067 tks) + Bash (沙箱说明) (438 tks) → 拆分为 45 个文件，涵盖概览、工作目录、超时、命令描述、引用、顺序/并行命令、换行符、分号、cwd 维护、专用工具偏好、6 个备选工具说明、Git 安全性 (3 个文件)、休眠指南 (6 个文件)、沙箱政策 (17 个文件) 以及验证父目录。

#### [2.1.52](https://github.com/Piebald-AI/claude-code-system-prompts/commit/94cd8e5)

<sub>_v2.1.52 版本中系统提示词无变更。_</sub>

#### [2.1.51](https://github.com/Piebald-AI/claude-code-system-prompts/commit/1988a63)

_+6,918 tokens_

- **新增：** 智能体提示词：快捷 PR 创建 —— 在预先填充上下文后创建提交和拉取请求的精简提示词 (945 tks)。
- **新增：** 智能体提示词：快捷 Git 提交 —— 在预先填充上下文后创建单个 Git 提交的精简提示词 (507 tks)。
- **新增：** 数据：Agent SDK 参考 — TypeScript —— TypeScript Agent SDK 参考，包含安装、快速入门、自定义工具和挂钩 (2287 tks)。
- **新增：** 数据：Claude Code 版本不匹配警告 —— 当 Claude Code 版本过旧时显示的警告 (173 tks)。
- **新增：** 技能：创建验证器技能 —— 用于为 Verify 智能体创建验证器技能以自动验证代码更改的提示词 (2586 tks)。
- **新增：** 系统提醒：挂钩 JSON 验证失败 —— 挂钩 JSON 输出验证失败时的错误 (320 tks)。
- **移除：** 智能体提示词：单词搜索词提取器 —— 移除了从用户查询中提取单词搜索词的提示词 (361 tks)。
- 数据：Agent SDK 模式 — Python —— 使用 `anyio` 替换了 `asyncio`；将消息类型检查从 `message.type == "result"` 切换为 `isinstance(message, ResultMessage)`；自定义工具现在需要通过 `create_sdk_mcp_server` + `ClaudeSDKClient` 调用 MCP 服务；添加了 `permission_mode="plan"` 和 `allow_dangerously_skip_permissions` 用于绕过模式 (2080 → 2350 tks)。
- 数据：Agent SDK 参考 — Python —— 添加了具有完整生命周期控制的 `ClaudeSDKClient` 接口；扩展了内置工具表 (`AskUserQuestion`, `Task`)；添加了 `plan` 和 `dontAsk` 权限模式；大大扩展了常用选项表，包含 `max_budget_usd`、`output_format`、`thinking`、`betas`、`setting_sources`、`env` 等；更新了具有 15 种以上事件类型的挂钩事件列表 (1718 → 2750 tks)。
- 数据：工具使用概念 —— 代码执行从 Beta 版晋升为正式版 (`code_execution_20260120`)；为网页搜索/获取 (`web_search_20260209`, `web_fetch_20260209`) 添加了新的服务端工具章节，包含动态过滤、编程化工具调用、工具搜索和工具使用示例；移除了记忆工具的 Beta 限制；更新了 `output_config.format` 的结构化输出指南 (2820 → 3640 tks)。
- 数据：工具使用参考 — Python —— 将代码执行和记忆从 `client.beta.messages.create` 迁移到 `client.messages.create`；移除了 `betas` 数组；文件 API Beta 现在通过 `extra_headers` 传递 (4261 → 4180 tks)。
- 数据：工具使用参考 — TypeScript —— 与 Python 版相同的 Beta 到正式版迁移；结构化输出示例从 `output_format` 更新为 `output_config.format` (3294 → 3228 tks)。
- 数据：Claude API 参考 — Python —— 为 `cache_control` 添加了明确的 TTL 支持 (`"ttl": "1h"`)；扩展了自适应思考备注以包含 Sonnet 4.6；添加了停止原因表 (`end_turn`, `max_tokens`, `tool_use`, `pause_turn`, `refusal`)；更新了速率限制错误处理；将 Sonnet 引用更改为 `claude-sonnet-4-6` (2905 → 3248 tks)。
- 数据：Claude API 参考 — TypeScript —— 为 `cache_control` 添加了明确的 TTL；将自适应思考扩展至 Sonnet 4.6；添加了停止原因表 (2024 → 2388 tks)。
- 数据：Claude API 参考 — Java —— 更新 SDK 版本 2.11.1 → 2.14.0；通过链式流 API 改进了流式传输；为结构化输出添加了 `anthropic-beta` 标头；添加了非 Beta 版工具使用章节 (1073 → 1226 tks)。
- 数据：Claude API 参考 — C# —— 移除了“Beta”标签；通过类型化 `RawMessageStreamEvent` 处理扩展了流式传输示例 (458 → 550 tks)。
- 数据：Claude API 参考 — Ruby —— 更新了工具运行器以使用具有 `doc` 方法和 `input` 参数的 `BaseModel` 输入模式 (603 → 622 tks)。
- 数据：Claude API 参考 — Go —— 将模型常量从 `ModelClaudeOpus4_5_20251101` 更新为 `ModelClaudeOpus4_6` (629 → 621 tks)。
- 数据：Claude API 参考 — PHP —— 移除了“Beta”标签；更新 SDK 0.4.0 → 0.5.0；从数组语法切换为命名参数 (410 → 394 tks)。
- 数据：Claude 模型目录 —— 添加了最大输出列（Opus 为 128K，Sonnet/Haiku 为 64K）；Opus 4.6 现在显示 1M Beta 上下文；添加了模型描述章节；将 Sonnet 3.7 和 Haiku 3.5 从“已弃用”移至“已退休”；并相应更新了别名表 (1349 → 1510 tks)。
- 数据：HTTP 错误代码参考 —— 使用 API 错误类型字符串替换了人类可读的错误名称（例如 `invalid_request_error`）；移除了 422 状态码，将验证错误合并至 400；剥离了转义的 Markdown 格式 (1460 → 1387 tks)。
- 技能：使用 Claude API 构建 —— Opus 4.6 现在显示 1M Beta 上下文；更强的默认模型指南（“始终使用 `claude-opus-4-6`”）；将自适应思考和努力值参数扩展到 Sonnet 4.6；扩展了思考/预算 Token 弃用备注；移除了 C#/PHP SDK 中的“Beta”标签（Token 计数不变）。
- 技能：使用 Claude API 构建 (触发器) —— 简化了触发条件，明确为 SDK 导入检查 (`anthropic`, `claude_agent_sdk`)；更清晰的“禁止触发”规则（Token 计数不变）。
- 工具描述：EnterWorktree —— 添加了明确的“何时不使用”章节；将激活限制为仅在用户明确说明“worktree”时；不再由于一般的隔离或分支请求触发 (284 → 334 tks)。
- 数据：Agent SDK 模式 — TypeScript —— 将会话初始化检查从 `"subtype" in message` 修复为 `message.type === "system"` (1067 → 1069 tks)。
- 数据：消息批量处理 API 参考 — Python —— 添加了 `"canceled"` 结果类型处理 (1481 → 1505 tks)。
- 在 12 个文件中进行了广泛的内部变量重命名（例如 `ADDITIONAL_USER_INPUT` → `USER_INPUT`, `PREVIOUS_AGENT_SUMMARY` → `PREVIOUS_SUMMARY`, `SYSTEM_REMINDER` → `PLAN_STATE`, `COMMIT_CO_AUTHORED_BY_CLAUDE_CODE` → `ATTRIBUTION_TEXT` 等）。

#### [2.1.50](https://github.com/Piebald-AI/claude-code-system-prompts/commit/5fa66df)

_+110 tokens_

- 工具描述：EnterWorktree —— 从仅限 Git 泛化为通过 `WorktreeCreate`/`WorktreeRemove` 挂钩支持任何版本控制系统 (VCS) 的隔离；要求现在允许已配置挂钩的非 Git 仓库 (237 → 284 tks)。
- 工具描述：读取文件 (ReadFile) —— 使用 `CONDITIONAL_READ_LINES` 变量替换了硬编码的 "cat -n 格式" 行号备注 (476 → 468 tks)。
- 工具描述：任务 (Task) —— 添加了 `isolation: "worktree"` 选项，以便在具有自动清理功能的临时 Git 工作树中运行智能体 (1228 → 1299 tks)。

#### [2.1.49](https://github.com/Piebald-AI/claude-code-system-prompts/commit/8da43fb)

<sub>_v2.1.49 版本中系统提示词无变更。_</sub>

#### [2.1.48](https://github.com/Piebald-AI/claude-code-system-prompts/commit/0d57836)

_-1,082 tokens_

- **新增：** 工具描述：EnterWorktree —— EnterWorktree 工具的工具描述 (237 tks)。
- **移除：** 系统提示词：MCP CLI —— 移除了有关使用 mcp-cli 与模型上下文协议 (MCP) 服务交互的指令 (1333 tks)。
- 工具描述：任务 (Task) —— 简化了后台智能体输出文件的指引；移除了 `BASH_TOOL` 变量和 `tail` 指令；添加了新的“前台 vs 后台”要点，解释何时使用每种模式 (1214 → 1228 tks)。

#### [2.1.47](https://github.com/Piebald-AI/claude-code-system-prompts/commit/f58cba9)

_+34,752 tokens_

- **新增：** 数据：Agent SDK 模式 — Python (2080 tks), Agent SDK 模式 — TypeScript (1067 tks), Agent SDK 参考 — Python (1718 tks) —— 针对 Python 和 TypeScript Agent SDK 的模式指南与参考。
- **新增：** 数据：Claude API 参考 — C# (458 tks), Go (629 tks), Java (1073 tks), PHP (410 tks), Python (2905 tks), Ruby (603 tks), TypeScript (2024 tks) —— 针对所有受支持的 Claude API 客户端语言的 SDK 参考。
- **新增：** 数据：Claude 模型目录 (1349 tks) —— 包含 ID、别名、上下文窗口及价格的当前和历史模型目录。
- **新增：** 数据：文件 API 参考 — Python (1303 tks), TypeScript (798 tks) —— 针对文件 API 的参考，涵盖上传、列表、删除及消息使用。
- **新增：** 数据：HTTP 错误代码参考 (1460 tks) —— Claude API HTTP 错误代码参考，包含常见原因和处理策略。
- **新增：** 数据：实时文档来源 (2337 tks) —— 用于从官方获取当前 Claude API 和 Agent SDK 文档的 WebFetch URL。
- **新增：** 数据：消息批量处理 API 参考 — Python (1481 tks) —— 批量处理 API 参考，包含批量创建、状态轮询和结果检索。
- **新增：** 数据：流式传输参考 — Python (1534 tks), TypeScript (1553 tks) —— 涵盖同步/异步流式传输和内容类型处理的流式传输参考。
- **新增：** 数据：工具使用概念 (2820 tks) —— 工具使用的概念基础，包含定义、工具选择及最佳实践。
- **新增：** 数据：工具使用参考 — Python (4261 tks), TypeScript (3294 tks) —— 涵盖工具运行器、智能体循环、代码执行及结构化输出的工具使用参考。
- **移除：** 智能体提示词：提示词建议生成器 (协调员) —— 移除了预测团队主管下一步输入内容的协调员模式提示词建议生成器 (283 tks)。
- **移除：** 系统提醒：委派模式提示词 —— 移除了将工具使用限制为团队协调工具的委派模式系统提醒 (185 tks)。
- **移除：** 系统提醒：已退出委派模式 —— 移除了退出委派模式时显示的通知 (50 tks)。
- 智能体提示词：状态栏设置 —— 为通过 `/add-dir` 添加的目录在工作空间模式中添加了 `added_dirs` 字段 (1482 → 1502 tks)。
- 工具描述：正在向用户提问 (AskUserQuestion) —— 添加了 `EXIT_PLAN_MODE_TOOL_NAME` 变量；扩展了计划模式指南，警告不要在提问中引用“计划”，因为用户在调用 `ExitPlanMode` 之前无法看到计划 (194 → 287 tks)。

#### [2.1.45](https://github.com/Piebald-AI/claude-code-system-prompts/commit/36d2856)

_+276 tokens_

- **新增：** 智能体提示词：单词搜索词提取器 —— 用于从用户查询中提取单词搜索词的系统提示词 (361 tks)。
- **新增：** 系统提示词：选项预览器 —— 用于并排预览 UI 选项的系统提示词 (129 tks)。
- **移除：** 智能体提示词：提示词建议生成器 (陈述意图) —— 移除了返回用户明确陈述的下一步操作的陈述意图提示词建议生成器 (166 tks)。
- 智能体提示词：/review-pr 斜杠命令 —— 使用普通反引号包围的 `gh` 命令替换了 `${BASH_TOOL_OBJECT.name}(...)` 模板表达式；移除了 `BASH_TOOL_OBJECT` 变量 (243 → 211 tks)。
- 工具描述：Bash (沙箱说明) —— 移除了 `CONDITIONAL_NEWLINE_IF_SANDBOX_ENABLED` 变量；“设置 dangerouslyDisableSandbox”要点之前的条件换行现在始终包含 (454 → 438 tks)。

#### [2.1.44](https://github.com/Piebald-AI/claude-code-system-prompts/commit/eb6a818)

<sub>_v2.1.44 版本中系统提示词无变更。_</sub>

#### [2.1.42](https://github.com/Piebald-AI/claude-code-system-prompts/commit/8a1123a)

_-1,060 tokens_

- **移除：** 智能体提示词：记忆技能 —— 移除了用于审查会话记忆并根据循环模式和学习心得更新 CLAUDE.local.md 的 `/remember` 技能提示词 (1048 tks)。
- 工具描述：网页搜索 (WebSearch) —— 简化了日期感知变量；使用单个 `CURRENT_MONTH_YEAR` 变量替换了 `GET_CURRENT_DATE_FN` 和 `CURRENT_YEAR`；更新了示例以使用纯文本（“使用今年，而非去年”）而非模板表达式 (331 → 319 tks)。

#### [2.1.41](https://github.com/Piebald-AI/claude-code-system-prompts/commit/91732e4)

_+262 tokens_

- **新增：** 系统提示词：条件性委派代码库探索 —— 添加了关于何时使用 Explore 子智能体与直接调用工具的指令 (249 tks)。
- 系统提示词：工具使用策略 —— 使用条件变量引用替换了有关将代码库探索委派给 Explore 智能体的内置“非常重要”块及示例；移除了 `GLOB_TOOL_NAME` 和 `GREP_TOOL_NAME` 变量 (564 → 352 tks)。
- 系统提示词：技能化当前会话 —— 添加了第二轮提问以询问用户将技能保存在何处（针对特定仓库 vs 个人）；更新了步骤 3 以使用用户选择的位置而非硬编码的 `.claude/skills/`；将步骤 4 更改为将 SKILL.md 输出为 YAML 代码块以供审查，并使用更简单的 AskUserQuestion 进行确认 (1750 → 1882 tks)。
- 系统提醒：计划模式已激活 (5 阶段) —— 使 Explore 子智能体的使用变为可选；禁用时，第 1 阶段现在指示 Claude 直接使用 Glob、Grep 和 Read 工具；更新了计划子智能体和智能体计数的第 2 阶段变量引用 (1429 → 1500 tks)。
- 智能体提示词：状态栏设置 —— 在 JSON 输入规范中添加了 `session_name`（通过 `/rename` 设置的可选人类可读会话名称）字段 (1460 → 1482 tks)。

#### [2.1.40](https://github.com/Piebald-AI/claude-code-system-prompts/commit/06ce2b9)

_-293 tokens_

- **移除：** 智能体提示词：演进当前运行中的技能 —— 移除了根据用户请求或偏好演进当前运行技能的智能体提示词 (293 tks)。

#### [2.1.39](https://github.com/Piebald-AI/claude-code-system-prompts/commit/11e9ec6)

_+293 tokens_

- **新增：** 智能体提示词：演进当前运行中的技能 —— 添加了新智能体提示词，用于根据用户含蓄或明确的要求来演进当前运行中的技能 (293 tks)。

#### [2.1.38](https://github.com/Piebald-AI/claude-code-system-prompts/commit/30adcee)

_+105 tokens_

- **新增：** 智能体提示词：提示词建议生成器 (协调员) —— 针对协调员模式下的提示词建议生成添加了新智能体提示词 (283 tks)。
- **新增：** 系统提示词：上下文压缩总结 —— 添加了针对 SDK 的上下文压缩总结所使用的新提示词 (278 tks)。
- **新增：** 工具描述：待办任务清单 (TaskList) (队友工作流) —— 为队友工作流在 TaskList 工具描述后添加了条件性章节 (133 tks)。
- **移除：** 智能体提示词：提示词建议生成器 (针对智能体团队) —— 移除了针对智能体团队的提示词建议生成器 (209 tks)。
- **移除：** 系统提示词：访问历史会话 —— 移除了搜索历史会话数据（包括记忆总结和对话脚本日志）的指令 (352 tks)。
- 工具描述：休眠 (Sleep) —— 简化了描述；使用“用户可以随时中断休眠”替换了“如果用户发送消息，则提早唤醒”，并移除了其他关于提前唤醒行为的引用。
- 工具描述：任务 (Task) —— 修复了示例智能体描述中的拼写错误并纠正了不匹配的 XML 闭合标签。
- 工具描述：Bash (Git 提交及 PR 创建指令) —— 对 Git 修正警告文本进行了细微的格式清理。

#### [2.1.37](https://github.com/Piebald-AI/claude-code-system-prompts/commit/e687bd6)

<sub>_v2.1.37 版本中系统提示词无变更。_</sub>

#### [2.1.36](https://github.com/Piebald-AI/claude-code-system-prompts/commit/933e339)

<sub>_v2.1.36 版本中系统提示词无变更。_</sub>

#### [2.1.34](https://github.com/Piebald-AI/claude-code-system-prompts/commit/0e01416)

<sub>_v2.1.34 版本中系统提示词无变更。_</sub>

#### [2.1.33](https://github.com/Piebald-AI/claude-code-system-prompts/commit/38ebc6b)

_-1,086 tokens_

- **新增：** 智能体提示词：提示词建议生成器 (针对智能体团队) —— 开启智能体蜂群模式时的提示词建议指令。
- **新增：** 工具描述：删除团队 (TeamDelete) —— 用于删除/清理团队资源的工具描述。
- **移除：** 系统提示词：任务协调员操作建议器 —— 移除了针对任务协调员的操作建议系统提示词。
- **移除：** 工具描述：进入计划模式 (模糊任务) —— 移除了针对模糊任务进入计划模式的独立条件性描述。
- 系统提醒：计划模式已激活 (5 阶段) —— 在第 4 阶段的最终计划开头添加了 **上下文 (Context)** 章节的要求，解释为什么进行该更改。
- 系统提醒：计划模式已激活 (迭代式) —— 重大改写：合并了变量；从 5 步“如何工作”章节重组为精简的“循环”周期（探索 → 更新计划 → 询问用户）；添加了新的“第一轮”、“提出好问题”和“何时汇聚”章节；重新界定为与用户结对计划；从 909 Token 减少至 797 Token。
- 工具描述：进入计划模式 (EnterPlanMode) —— 将“计划模式中会发生什么”章节提取到条件变量 (`CONDITIONAL_WHAT_HAPPENS_NOTE`) 中；从 970 Token 减少至 878 Token。
- 工具描述：任务 (Task) —— 移除了 `AGENT_TEAM_CHECK` 变量及有关在特定套餐中无法使用智能体团队功能的条件性提示；从 1340 Token 减少至 1215 Token。
- 工具描述：团队成员工具 (TeammateTool) —— 将工具标题从 "TeammateTool" 重命名为 "TeamCreate"；移除了 `spawnTeam` 操作标签和 `cleanup` 操作（现为独立的 TeamDelete 工具）；为创建的团队和任务列表资源添加了明确的文件路径；添加了有关自动消息投递的注意事项；更新了工作流以引用 TeamCreate；从 1790 Token 减少至 1642 Token。

#### [2.1.32](https://github.com/Piebald-AI/claude-code-system-prompts/commit/a362f28)

_+2,323 tokens_

- **新增：** 智能体提示词：近期消息总结 —— 用于总结近期消息的智能体提示词。
- **新增：** 系统提示词：任务协调员操作建议器 —— 用于针对任务协调员或团队负责人建议操作的系统提示词。
- **新增：** 系统提示词：智能体摘要生成 —— 用于“智能体摘要”生成的系统提示词。
- **新增：** 系统提示词：技能化当前会话 —— 将当前会话转化为技能的系统提示词。
- 系统提示词：谨慎执行操作 —— 添加了关于锁定文件的指引：调查哪个进程持有锁定文件，而非直接删除。
- 系统提示词：队友沟通 —— 从“队友沟通”更名为“智能体队友沟通”；更新以引用 SendMessage 工具而非 Teammate 工具；简化并澄清了沟通指令；从 138 Token 减少至 127 Token。
- 系统提醒：计划模式已激活 (迭代式) —— 更新了有关使用 Explore 智能体类型的指引，澄清其对并行化复杂搜索很有用，但直接工具对简单查询更简便。
- 工具描述：SendMessageTool —— 将术语从“蜂群中的队友”更新为“团队中的智能体队友”。
- 工具描述：团队成员工具 (TeammateTool) —— 重大重构：移除了操作项 (discoverTeams, requestJoin, approveJoin, rejectJoin) 以及环境变量章节；添加了“何时使用”和“为队友选择智能体类型”章节；在空闲通知中添加了关于对等私聊可见性的提示；精简了团队工作流和协作指令；明确了队友不应发送结构化的 JSON 状态消息；从 2393 Token 减少至 1790 Token。

#### [2.1.31](https://github.com/Piebald-AI/claude-code-system-prompts/commit/e273964400723d0b8b50b871aa056ba3a2267ad0)

_+693 tokens_

- **新增：** 系统提示词：智能体记忆指令 —— 指示在智能体系统提示中包含特定领域（如代码审查员、测试执行者、架构师）的记忆更新引导。
- **新增：** 系统提示词：审查对恶意活动的协助 —— 在拒绝恶意请求的同时，针对授权安全测试、防御性安全、CTF 挑战和教育背景提供协助的准则（此前在 v2.1.20 中移除，现重新添加）。
- **新增：** 系统提示词：工具权限模式 —— 关于工具权限模式及处理被拒工具调用的指导；建议不要重新尝试被拒的工具调用，而应调整方案。
- **新增：** 系统提醒：钩子停止执行前缀 —— 钩子停止执行消息的前缀。
- **新增：** 工具描述：工具搜索 (ToolSearch) 增强版 —— 将 ToolSearch 的扩展使用指令移至独立的条件性提示词中（包含查询模式、示例、正确/错误的使用模式）。
- **移除：** 工具描述：TeammateTool 操作参数 —— 对 TeammateTool 操作参数的描述（已移除）。
- 工具描述：任务 (Task) —— 添加了有关在某些套餐中无法使用“智能体团队”功能（TeammateTool, SendMessage, spawnTeam）的条件性说明；澄清此限制仅在用户明确要求智能体团队或点对点消息传递时适用。
- 工具描述：工具搜索 (ToolSearch) —— 重构：将扩展内容移至独立的“工具搜索增强版”提示词中；简化后的基础描述现在引用 `<available-deferred-tools>` 消息，并通过标识符条件性包含扩展内容。

#### [2.1.30](https://github.com/Piebald-AI/claude-code-system-prompts/commit/87f225d)

_+3,152 tokens_

- **新增：** 系统提示词：谨慎执行操作 —— 有关谨慎执行操作的指令。
- **新增：** 系统提示词：概览见解总结 —— 为见解报告生成包含四个部分（现状、阻碍、快速改进方案、宏大工作流）的简洁总结。
- **新增：** 系统提示词：见解摩擦分析 —— 分析汇总的使用数据以识别摩擦模式并对经常出现的问题进行分类。
- **新增：** 系统提示词：地平线上的见解 —— 识别宏大的未来工作流以及自主 AI 辅助开发的机会。
- **新增：** 系统提示词：见解会话侧面提取 —— 从单次 Claude Code 会话脚本中提取结构化的多维度信息（目标类别、满意度、摩擦点）。
- **新增：** 系统提示词：见解建议 —— 生成可操作的建议，包括 CLAUDE.md 的补充、尝试的功能以及使用模式。
- **新增：** 系统提示词：并行工具调用说明 —— 告诉 Claude 使用并行工具调用的系统提示词。
- **新增：** 工具描述：休眠 (Sleep) —— 用于等待/休眠的工具，接收用户输入时可提早唤醒。
- 系统提示词：访问历史会话 —— 添加了将搜索结果截断为每项匹配 64 个字符的技巧，以保持上下文可管理。
- 系统提示词：钩子配置 —— 显著重构了钩子响应格式，包含 `suppressOutput`、`decision`、`reason` 以及具有事件特定参数的 `hookSpecificOutput` 等新字段。
- 系统提醒：计划模式已激活 (5 阶段) —— 添加了主动搜索并复用现有函数、工具和模式的指南，强调在计划中应包含对已发现工具的引用。
- 系统提醒：计划模式已激活 (迭代式) —— 添加了有关复用现有代码并在计划中包含发现工具引用的类似指南。
- 工具描述：读取文件 (ReadFile) —— 为大型 PDF（超过 10 页）添加了使用 `pages` 参数的要求，每次请求最多 20 页。
- 工具描述：SendMessageTool —— 重组了消息类型（移除了嵌套的“请求”和“响应”类型），为消息和广播类型添加了必需的 `summary` 字段，扁平化了协议以使用诸如 `shutdown_request`、`shutdown_response`、`plan_approval_response` 等具体类型。
- 工具描述：任务 (Task) —— 重组了序言章节。
- 工具描述：团队成员工具 (TeammateTool) —— 澄清了队友在每一轮之后都会进入空闲状态（不只是完成时），解释了空闲的队友仍可接收消息并会被唤醒以处理消息，并澄清空闲通知是自动且正常的。

#### [2.1.29](https://github.com/Piebald-AI/claude-code-system-prompts/commit/e2d243c)

<sub>_v2.1.29 版本中系统提示词无变更。_</sub>

#### [2.1.28](https://github.com/Piebald-AI/claude-code-system-prompts/commit/79616d9)

<sub>_v2.1.28 版本中系统提示词无变更。_</sub>

#### [2.1.27](https://github.com/Piebald-AI/claude-code-system-prompts/commit/de0f1c3)

<sub>_v2.1.27 版本中系统提示词无变更。_</sub>

#### [2.1.26](https://github.com/Piebald-AI/claude-code-system-prompts/commit/f8e3357)

_+0 tokens_

- 智能体提示词：提示词建议生成器 (陈述意图) —— 将最大建议长度从 2-8 个单词增加到 2-12 个单词。
- 智能体提示词：提示词建议生成器 v2 —— 将最大建议长度从 2-8 个单词增加到 2-12 个单词。

#### [2.1.25](https://github.com/Piebald-AI/claude-code-system-prompts/commit/5f194f5)

<sub>_v2.1.25 版本中系统提示词无变更。_</sub>

#### [2.1.23](https://github.com/Piebald-AI/claude-code-system-prompts/commit/44566a0)

_-383 tokens_

- **新增：** 系统提醒：/btw 随口提问 —— 针对不使用工具的 /btw 斜杠命令随口提问的系统提醒。
- **移除：** 智能体提示词：伴随蜂群退出计划模式 —— 当 `isSwarm` 设置为 true 时调用 ExitPlanMode 的系统提醒。
- 系统提示词：主系统提示词 —— 移除了 SECURITY_POLICY 变量后的末尾句号。
- 工具描述：技能 (Skill) —— 简化并精简：移除了示例章节，压缩了重要备注，从内联列出可用技能改为引用系统提醒消息，并更新了变量引用。
- 工具描述：团队成员工具 (TeammateTool) —— 更新了 UI 通知描述：现在在消息等待时显示“带有发送者姓名的简短通知”，而非“排队的队友消息”。

#### [2.1.22](https://github.com/Piebald-AI/claude-code-system-prompts/commit/5c57ba3)

<sub>_v2.1.22 版本中系统提示词无变更。_</sub>

#### [2.1.21](https://github.com/Piebald-AI/claude-code-system-prompts/commit/51239d3)

_+442 tokens_

- **新增：** 系统提示词：访问历史会话 —— 搜索历史会话数据（包括记忆摘要和对话脚本日志）的指令。
- 工具描述：团队成员工具 (TeammateTool) —— 添加了在多个任务可用时优先按 ID 顺序（从小到大）处理的指引，因为早期的任务通常会为后续任务奠定上下文。

#### [2.1.20](https://github.com/Piebald-AI/claude-code-system-prompts/commit/18fd5f9)

_-1,928 tokens_

- **新增：** 系统提示词：执行任务 —— 执行软件工程任务的指令。
- **新增：** 系统提示词：任务管理 —— 使用任务管理工具的指令。
- **新增：** 系统提示词：语气与风格 —— 沟通语气和响应风格的准则。
- **新增：** 系统提示词：工具使用策略 —— 工具使用的政策和准则。
- **新增：** 工具描述：发送消息工具 (SendMessageTool) —— 在蜂群中向队友发送消息并处理协议请求/响应的工具。
- **新增：** 工具描述：进入计划模式 (模糊任务) —— 当任务存在歧义时进入计划模式的工具。
- **移除：** 系统提示词：审查对恶意活动的协助 —— 提供授权安全测试协助的准则。
- **移除：** 系统提醒：排队的命令 (提示词) —— 待处理的排队用户消息（提示词变体）。
- **移除：** 系统提醒：排队的命令 —— 待处理的排队用户消息。
- **移除：** 系统提醒：会话记忆 —— 可能相关的历史会话摘要。
- 系统提示词：主系统提示词 —— 大幅从 2896 缩减至 269 Token；大部分内容被提取到重点明确的独立系统提示词中（执行任务、任务管理、语气与风格、工具使用策略）。
- 智能体提示词：会话标题与分支生成 —— 将输出格式从 XML 标签更改为带有 "title" 和 "branch" 字段的 JSON 对象。
- 智能体提示词：Bash 命令前缀检测 —— 从智能引号切换为标准引号。
- 工具描述：团队成员工具 (TeammateTool) —— 移除了协议操作项 (approvePlan, rejectPlan, requestShutdown, approveShutdown, rejectShutdown, write, broadcast)，并简化为核心团队管理操作。
- 工具描述：团队成员工具操作参数 —— 从“TeammateTool 的操作参数”更名，并从 173 缩减至 72 Token。
- 工具描述：编辑 (Edit) —— 简化，从使用备注中移除了明确的读取工具要求。
- 工具描述：写入 (Write) —— 简化，从使用备注中移除了明确的读取工具要求。
- 工具描述：Bash (Git 提交及 PR 创建指令) —— 添加了将 PR 标题保持在简短长度（70 字符以下）并使用描述/正文补充细节的指引。
- 系统提示词：工具执行被拒 —— 精简了措辞。
- 智能体提示词：带有附加指令的会话总结 —— 合并进基础的“会话总结”提示词；附加指令现在通过代码条件性添加，而非作为独立的字符串。
- 智能体提示词：挂钩执行提示词 —— 从 485 缩减至 263 字符；移除了冗长的 JSON 格式指令。

#### [2.1.19](https://github.com/Piebald-AI/claude-code-system-prompts/commit/fcf3f24)

_+182 tokens_

- **新增：** 系统提示词：工具使用摘要生成 —— 用于生成工具使用摘要的提示词。
- **移除：** 工具描述：待办任务清单 (TaskList) —— 用于列出任务清单中所有任务的 TaskList 工具描述。
- 智能体提示词：状态栏设置 —— 为使用 `--agent` 标志启动的智能体在 `statusLine` 结构中添加了智能体信息（名称和类型）。
- 工具描述：技能 (Skill) —— 将措辞从“仅使用下方‘可用技能’中列出的技能”更新为“下方列出的技能可供调用”。
- 工具描述：创建任务 (TaskCreate) —— 为条件性备注添加了模板变量，并重组了任务分配指令。
- 工具描述：工具搜索 (ToolSearch) —— 重大扩展：重新排列了查询模式（关键词搜索现排第一），澄清了两类模式都会立即加载工具，添加了带 `+` 前缀的必需关键词语法，并扩展了示例以展示应避免的冗余选择模式。

#### [2.1.18](https://github.com/Piebald-AI/claude-code-system-prompts/commit/a3f5e2e)

<sub>_v2.1.18 版本中系统提示词无变更。_</sub>

#### [2.1.17](https://github.com/Piebald-AI/claude-code-system-prompts/commit/4615ff3)

<sub>_v2.1.17 版本中系统提示词无变更。_</sub>

#### [2.1.16](https://github.com/Piebald-AI/claude-code-system-prompts/commit/e8da828)

_+7,114 tokens_

- **新增：** 智能体提示词：伴随蜂群退出计划模式 —— 当 `isSwarm` 设置为 true 时调用 ExitPlanMode 的系统提醒。
- **新增：** 系统提示词：队友沟通 —— 在蜂群中进行队友沟通的系统提示词。
- **新增：** 系统提示词：工具执行被拒 —— 工具执行被拒绝时的系统提示词。
- **新增：** 系统提醒：委派模式提示词 —— 委派模式的系统提醒。
- **新增：** 系统提醒：计划模式已激活 (5 阶段) —— 具备并行探索和多智能体计划功能的增强型计划模式系统提醒。
- **新增：** 系统提醒：计划模式已激活 (迭代式) —— 具备用户访谈工作流的主智能体迭代式计划模式系统提醒。
- **新增：** 系统提醒：团队协作 —— 团队协作的系统提醒。
- **新增：** 系统提醒：团队关停 —— 团队关停的系统提醒。
- **新增：** 工具描述：创建任务 (TaskCreate) —— TaskCreate 工具的工具描述。
- **新增：** 工具描述：待办任务清单 (TaskList) —— 用于列出任务清单中所有任务的 TaskList 工具描述。
- **新增：** 工具描述：队友工具的操作参数 —— TeammateTool 操作参数的工具描述。
- **新增：** 工具描述：队友工具 —— TeammateTool 的工具描述。
- **新增：** 工具参数：电脑工具的电脑操作 —— Chrome 浏览器电脑工具的操作参数选项（包含悬停动作等）。
- 智能体提示词：/security-review 斜杠命令 —— 为了保持一致性，从 "/security-review slash" 更名。
- 系统提示词：学习模式 —— 更新了描述元数据（移除了 "System Prompt:" 前缀）。
- 系统提醒：计划模式已激活 (子智能体版) —— 为了保持一致性，从 "Plan mode is active (for subagents)" 更名。
- 工具描述：Bash (Git 提交及 PR 创建指令) —— 添加了避免在 Git 变基命令中使用 `--no-edit` 标志的指引，因为该标志对 `git rebase` 无效。
- 工具描述：写入 (Write) —— 描述从“创建/覆盖写入单个文件”澄清为“用于创建和覆盖写入单个文件”。

#### [2.1.15](https://github.com/Piebald-AI/claude-code-system-prompts/commit/011066d)

_+183 tokens_

- 工具描述：Bash (Git 提交及 PR 创建指令) —— 扩展了“Git 安全协议”，列出了具体的破坏性命令，并对可能的潜在数据丢失进行了详细解释；澄清了预提交挂钩失败后应避免使用 `--amend`；添加了建议通过名称暂存具体文件，而非使用 "git add -A" 或 "git add ." 的指引，以避免意外包含敏感文件 (.env, 凭据) 或大型二进制文件。
- 工具描述：任务 (Task) —— 将后台智能体输出检索指令从使用 TaskOutput 工具更新为使用 Read 工具读取 `output_file` 路径或使用 Bash 的 `tail` 查看最近输出；添加了有关 `run_in_background`、`name`、`team_name` 和 `mode` 参数在某些上下文中不可用的条件性备注。

#### [2.1.14](https://github.com/Piebald-AI/claude-code-system-prompts/commit/8533e3b)

_-1,153 tokens_

- **新增：** 智能体提示词：提示词建议生成器 (陈述意图) —— 根据用户明确陈述的下一步操作生成提示词建议的指令。
- **新增：** 工具描述：工具搜索 (ToolSearch) —— 从 MCPSearch 更名；用于在使用前加载并搜索延迟加载工具。
- **移除：** 工具描述：ExitPlanMode v2 及 ExitPlanMode v2 (安全备注) —— 将功能整合进基础的 ExitPlanMode 中。
- **移除：** 工具描述：MCPSearch 及 MCPSearch (包含可用工具) —— 由 ToolSearch 取代。
- 工具描述：退出计划模式 (ExitPlanMode) —— 添加了“该工具如何工作”章节，解释了计划文件工作流；澄清了工具是从计划文件中读取，而非将计划作为参数传入；将“处理计划中的歧义”章节简化为“使用该工具前”，并提供了关于何时使用 AskUserQuestion 的清晰指导；移除了变量引用，改为使用直接工具名。
- 工具描述：Bash —— 澄清了会话持久化行为：“工作目录在命令之间保持不变；Shell 状态（其他所有内容）则不然。Shell 环境从用户配置文件（bash 或 zsh）初始化。”
- 工具描述：网页获取 (WebFetch) —— 添加了针对 GitHub URL 优先通过 Bash 使用 `gh` CLI 的指引（例如 `gh pr view`, `gh issue view`, `gh api`）。
- 系统提示词：Chrome 浏览器 MCP 工具 —— 更新以引用 ToolSearch 而非 MCPSearch。

#### [2.1.10](https://github.com/Piebald-AI/claude-code-system-prompts/commit/9cb8c2c)

_-118 tokens_

- 智能体提示词：会话标题与分支生成 —— 添加了明确的指令要求标题使用句式大小写（仅首字母和专有名词大写），而非每个单词首字母大写。
- 工具描述：Bash (Git 提交及 PR 创建指令) —— 通过移除复杂的条件准则（关于何时允许修正的 5 条条件）简化了 `git commit --amend` 指引；替换为简单的“关键”指令：始终创建新提交，除非用户明确要求，否则绝不使用 `--amend`；移除了预提交挂钩失败步骤中对“上述修正规则”的引用。

#### [2.1.9](https://github.com/Piebald-AI/claude-code-system-prompts/commit/0f37d97)

_+963 tokens_

- **新增：** 系统提示词：挂钩配置 —— 挂钩配置的系统提示词，用于 Claude Code 配置技能。
- **移除：** 系统提示词：自主智能体 (独立版) —— 无系统上下文前缀的独立版自主智能体模式提示词。
- **移除：** 系统提示词：自主智能体 (带有上下文) —— 前缀为主系统提示词的自主智能体模式提示词。
- 系统提示词：主系统提示词 —— 将“无时间表的计划”章节重命名为“不提供时间预估”；扩展了指南，明确禁止为 Claude 自身的工作提供时间预估（例如“这只需要我几分钟的时间”，“应该在 5 分钟内完成”，“这是一个快速修复”），此外也禁止建议项目时间线；强调应由用户自己判断时间。

#### [2.1.8](https://github.com/Piebald-AI/claude-code-system-prompts/commit/168ab21)

_-101 tokens_

- 系统提醒：计划模式已激活 —— 将内联的计划文件信息章节提取到独立的、全新的章节中；将硬编码的阶段编号 (2-5) 转换为动态变量，用于条件性的用户访谈阶段；使用专门针对用户访谈的新阶段替换了用户访谈指南。
- 工具描述：网页搜索 (WebSearch) —— 更新了年份示例，使用当前年份值而非硬编码的年份值。

#### [2.1.7](https://github.com/Piebald-AI/claude-code-system-prompts/commit/3772a02)

_+74 tokens_

- **新增：** 工具描述：退出计划模式 v2 (安全备注) —— 使用 ExitPlanMode 工具时范围界定权限的安全指南。
- 系统提示词：Chrome 浏览器上的 Claude 自动化 —— 在警告阻塞浏览器事件的警报和对话框中添加了“重要”强调。
- 系统提醒：计划模式已激活 —— 澄清了计划批准问题（例如“这个计划可以吗？”，“我应该继续吗？”）必须使用 ExitPlanMode 工具，而不能使用文本问题或 AskUserQuestion；扩展了区分何时使用 AskUserQuestion（仅用于需求/方案澄清）与何时使用 ExitPlanMode（用于计划批准）的指引。
- 工具描述：退出计划模式 v2 —— 将详细的安全和范围界定准则提取到全新的 `PERMISSION_SCOPING_GUIDELINES` 变量中；使用变量引用替换了内联的范围界定指令；在“使用该工具前”和“重要事项”章节中更新了工具名引用。

#### [2.1.6](https://github.com/Piebald-AI/claude-code-system-prompts/commit/4843349)

_+742 tokens_

- **新增：** 系统提示词：自主智能体 (独立版) —— 无系统上下文前缀的独立版自主智能体模式提示词。
- **新增：** 系统提示词：自主智能体 (带有上下文) —— 前缀为主系统提示词的自主智能体模式提示词。
- **移除：** 智能体提示词：Bash 命令解释器 —— 为了整合 Bash 命令解释而移除。
- 智能体提示词：状态栏设置 —— 在 `context_window` 对象中添加了预先计算好的 `used_percentage` 和 `remaining_percentage` 字段；更新了示例以使用更简单的语法显示上下文使用情况。
- 智能体提示词：Claude 引导智能体 —— 修复了在整个方案步骤中错误的变量引用（包括文档源码 URL 和工具名）。
- 智能体提示词：会话搜索助手 —— 简化了介绍文本。
- 工具描述：Bash —— 重构了变量使用，使用 `RUN_IN_BACKGROUND_NOTE` 替换了 `BASH_TOOL_NAME`。
- 工具描述：退出计划模式 v2 —— 添加了综合性的“请求权限 (allowedPrompts)”章节，包含针对 Bash 命令请求基于提示词权限的准则，包含具有安全意识的范围界定实践。

#### [2.1.5](https://github.com/Piebald-AI/claude-code-system-prompts/commit/701b0e2)

_-24 tokens_

- 工具描述：Bash —— 在元数据中以 `BASH_TOOL_NAME` 变量替换了 `GIT_COMMIT_AND_PR_CREATION_INSTRUCTION` 变量。
- 工具描述：任务 (Task) —— 重新排列了变量声明，将 `IS_TRUTHY_FN` 和 `PROCESS_OBJECT` 提前。

#### [2.1.4](https://github.com/Piebald-AI/claude-code-system-prompts/commit/42537cb)

_-19 tokens_

- 工具描述：Bash —— 将 `run_in_background` 参数文档移动至新的 `BASH_BACKGROUND_TASK_NOTES_FN` 函数变量中；添加了 `BASH_TOOL_EXTRA_NOTES()` 占位符；修复了专用工具列表中错位的变量引用（文件搜索、内容搜索、读取文件、编辑文件、写入文件此前分别引用了错误的工具名）。
- 工具描述：任务 —— 添加了 `IS_TRUTHY_FN` 和 `PROCESS_OBJECT` 变量用于条件性渲染；后台任务指令现在基于 `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` 环境变量进行条件性渲染。

#### [2.1.3](https://github.com/Piebald-AI/claude-code-system-prompts/commit/3b9438c)

_+1,047 tokens_

- **新增：** 智能体提示词：Bash 命令描述编写器 —— 针对 Bash 命令，生成含义明确、简洁、采用主动语态描述的指引。
- **新增：** 智能体提示词：Bash 命令解释器 —— 包含推理、风险评估和风险等级分类的 Bash 命令解释指令。
- **新增：** 智能体提示词：记忆技能 —— 为 `/remember` 技能准备的系统提示词，用于审查会话记忆并根据循环模式和学习心得更新 CLAUDE.local.md。
- **移除：** 智能体提示词：Bash 命令风险分类器 —— 由新的 Bash 命令解释器智能体取代。
- 工具描述：Bash —— 更新了描述字段指令，为复杂命令（管道命令、晦涩标志等）提供更多上下文，同时保持简单命令简短。
- 工具描述：Bash (Git 提交及 PR 创建指令) —— 添加了绝不要使用 `git status -uall` 标志的警告，因为它会导致大型仓库出现内存问题。
- 工具描述：任务 —— 更新了内部变量引用并改进了后台智能体监控指令。
