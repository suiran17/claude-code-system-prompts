<div>
<div align="right">
<a href="https://piebald.ai"><img width="200" top="20" align="right" src="https://github.com/Piebald-AI/.github/raw/main/Wordmark.svg"></a>
</div>

<div align="left">

### 了解 Piebald
我们发布了 **Piebald**，终极的代理化 (Agentic) AI 开发者体验。 \
下载并免费试用！ **https://piebald.ai/**

<a href="https://piebald.ai/discord"><img src="https://img.shields.io/badge/Join%20our%20Discord-5865F2?style=flat&logo=discord&logoColor=white" alt="加入我们的 Discord"></a>
<a href="https://x.com/PiebaldAI"><img src="https://img.shields.io/badge/Follow%20%40PiebaldAI-000000?style=flat&logo=x&logoColor=white" alt="在 X 上关注"></a>

<sub>[**向下滚动查看 Claude Code 系统提示词。**](#claude-code-系统提示词) :point_down:</sub>

</div>
</div>

<div align="left">
<a href="https://piebald.ai">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://piebald.ai/screenshot-dark.png">
  <source media="(prefers-color-scheme: light)" srcset="https://piebald.ai/screenshot-light.png">
  <img alt="英雄图" width="800" src="https://piebald.ai/screenshot-light.png">
</picture>
</a>
</div>

# Claude Code 系统提示词 (Claude Code System Prompts)

[![Mentioned in Awesome Claude Code](https://awesome.re/mentioned-badge.svg)](https://github.com/hesreallyhim/awesome-claude-code)

> [!important]
> **更新 (2026年1月23日)：我们在此列表中添加了 Claude Code 所有的约 40 条系统提醒 (System Reminders)&mdash;&mdash;详见 [系统提醒](#系统提醒)。**

本仓库包含截至 **[Claude Code v2.1.63](https://www.npmjs.com/package/@anthropic-ai/claude-code/v/2.1.63) (2026年2月27日)** 为止，所有 Claude Code 各类系统提示词的最新列表及关联 Token 计数。此外，本仓库还随附一份涵盖了自 v2.0.14 以来 115 个版本的系统提示词迭代说明的 [**变更日志 (CHANGELOG.md)**](./CHANGELOG.md)。本项目由 [<img src="https://github.com/Piebald-AI/piebald/raw/main/assets/logo.svg" width="15"> **Piebald.**](https://piebald.ai/) 团队提供。

**本仓库会在 Claude Code 每次发布后的几分钟内同步更新。请参阅 [变更日志](./CHANGELOG.md)，并在 X 上关注 [@PiebaldAI](https://x.com/PiebaldAI)，以获取每个发布版本中系统提示词变更的摘要。**

> [!note]
> ⭐ **收藏 (Star)** 本仓库以获取新版本 Claude Code 的通知。对于每个新的 Claude Code 版本，我们都会在 GitHub 上创建一个发布记录 (Release)，这将通知所有收藏了本仓库的用户。

---

为什么会有多个“系统提示词”？

**Claude Code 的系统提示词不仅仅是一段单一的字符串。**

相反，它由以下部分组成：
- 根据环境和各种配置条件性添加的大量内容。
- 对内置工具（如 \`Write\`、\`Bash\` 和 \`TodoWrite\`）的描述，其中有些规模相当庞大。
- 针对内置代理（如 Explore 和 Plan）的独立系统提示词。
- 众多由 AI 驱动的工具函数，如对话压缩、\`CLAUDE.md\` 生成、会话标题生成等，它们均配有专属的系统提示词。

这导致了在庞大的压缩版 JS 文件中，有 110 多条字符串在不断变化和移动。

> [!TIP]
> 想要在您自己的 Claude Code 安装版本中**修改特定的系统提示词片段**吗？ **请使用 [tweakcc](https://github.com/Piebald-AI/tweakcc)。** 它可以：
> - 让您以 Markdown 文件形式自定义系统提示词的各个组成部分，然后
> - 将它们补丁 (Patch) 到您的 npm 版或原生（二进制）版 Claude Code 安装中，并且
> - 当您和 Anthropic 对同一个提示词文件产生冲突的修改时，提供差异对比和冲突管理功能。

## 提取逻辑 (Extraction)

本仓库包含使用脚本从最新 npm 版本的 Claude Code 中提取出的系统提示词。由于这些提示词是直接从 Claude Code 编译后的源码中提取的，因此保证与 Claude Code 实际使用的完全一致。如果您使用 [tweakcc](https://github.com/Piebald-AI/tweakcc) 定制系统提示词，其工作原理类似&mdash;&mdash;它会在您的本地安装中补丁与本仓库提取出的完全相同的字符串。

## 提示词列表 (Prompts)

请注意，某些提示词包含插值内容，例如内置工具名的引用、可用子代理列表以及各种其他特定上下文的变量，因此特定 Claude Code 会话中的实际计数会略有不同&mdash;&mdash;但误差通常不会超过 ±20 个 Token。

### 代理提示词 (Agent Prompts)

子代理与工具函数。

#### 子代理 (Sub-agents)

- [代理提示词：探索 (Explore)](./system-prompts-cn/agent-prompt-explore-cn.md) (**516** tks) - Explore 子代理的系统提示词。
- [代理提示词：计划模式（增强型）](./system-prompts-cn/agent-prompt-plan-mode-enhanced-cn.md) (**633** tks) - Plan 子代理的增强版提示词。
- [代理提示词：任务工具（额外说明）](./system-prompts-cn/agent-prompt-task-tool-extra-notes-cn.md) (**127** tks) - 任务工具使用的附加说明（绝对路径、不使用 Emoji、工具调用前不使用冒号）。
- [代理提示词：任务工具 (Task tool)](./system-prompts-cn/agent-prompt-task-tool-cn.md) (**294** tks) - 通过 Task 工具生成的子代理所使用的系统提示词。

### 创建助手 (Creation Assistants)

- [代理提示词：代理创建架构师](./system-prompts-cn/agent-prompt-agent-creation-architect-cn.md) (**1110** tks) - 用于创建具有详细规范的自定义 AI 代理的系统提示词。
- [代理提示词：CLAUDE.md 创建](./system-prompts-cn/agent-prompt-claudemd-creation-cn.md) (**384** tks) - 用于分析代码库并创建 CLAUDE.md 文档文件的系统提示词。
- [代理提示词：状态栏设置](./system-prompts-cn/agent-prompt-status-line-setup-cn.md) (**1502** tks) - 用于配置状态栏显示的 \`statusline-setup\` 代理之系统提示词。

### 斜杠命令 (Slash commands)

- [代理提示词：/batch 斜杠命令](./system-prompts-cn/agent-prompt-batch-slash-command-cn.md) (**1136** tks) - 编排跨越代码库的大规模并行更改的指令。
- [代理提示词：/pr-comments 斜杠命令](./system-prompts-cn/agent-prompt-pr-comments-slash-command-cn.md) (**396** tks) - 用于获取并显示 GitHub PR 评论的系统提示词。
- [代理提示词：/review-pr 斜杠命令](./system-prompts-cn/agent-prompt-review-pr-slash-command-cn.md) (**211** tks) - 通过代码分析审查 GitHub 拉取请求 (PR) 的系统提示词。
- [代理提示词：/security-review 斜杠命令](./system-prompts-cn/agent-prompt-security-review-slash-command-cn.md) (**2610** tks) - 综合安全审查提示词，重点分析代码更改中可被利用的漏洞。

### 工具函数 (Utilities)

- [代理提示词：代理钩子 (Agent Hook)](./system-prompts-cn/agent-prompt-agent-hook-cn.md) (**133** tks) - 针对“代理钩子”的提示词。
- [代理提示词：Bash 命令描述编写器](./system-prompts-cn/agent-prompt-bash-command-description-writer-cn.md) (**207** tks) - 针对 Bash 命令，生成含义明确、简洁、采用主动语态描述的指令。
- [代理提示词：Bash 命令前缀检测](./system-prompts-cn/agent-prompt-bash-command-prefix-detection-cn.md) (**823** tks) - 用于检测命令前缀和命令注入的系统提示词。
- [代理提示词：Claude 引导代理 (Claude guide agent)](./system-prompts-cn/agent-prompt-claude-guide-agent-cn.md) (**761** tks) - 引导代理之系统提示词，帮助用户理解并有效使用 Claude Code、Claude Agent SDK 和 Claude API。
- [代理提示词：会话总结](./system-prompts-cn/agent-prompt-conversation-summarization-cn.md) (**1121** tks) - 用于创建详细会话总结的系统提示词。
- [代理提示词：钩子条件评估器](./system-prompts-cn/agent-prompt-hook-condition-evaluator-cn.md) (**78** tks) - 在 Claude Code 中评估钩子条件的系统提示词。
- [代理提示词：记忆选择 (Memory selection)](./system-prompts-cn/agent-prompt-memory-selection-cn.md) (**156** tks) - 针对用户查询选择相关记忆的指令。
- [代理提示词：提示词建议生成器 v2](./system-prompts-cn/agent-prompt-prompt-suggestion-generator-v2-cn.md) (**296** tks) - 为 Claude Code 生成提示词建议的 v2 指令。
- [代理提示词：快捷 PR 创建](./system-prompts-cn/agent-prompt-quick-pr-creation-cn.md) (**945** tks) - 在预先填充上下文后创建提交 (commit) 和拉取请求 (PR) 的精简提示词。
- [代理提示词：快捷 Git 提交](./system-prompts-cn/agent-prompt-quick-git-commit-cn.md) (**507** tks) - 在预先填充上下文后创建单个 Git 提交的精简提示词。
- [代理提示词：近期消息总结](./system-prompts-cn/agent-prompt-recent-message-summarization-cn.md) (**720** tks) - 用于总结近期消息的代理提示词。
- [代理提示词：会话搜索助手](./system-prompts-cn/agent-prompt-session-search-assistant-cn.md) (**439** tks) - 会话搜索助手的代理提示词，该助手根据用户查询和元数据查找相关会话。
- [代理提示词：会话记忆更新指令](./system-prompts-cn/agent-prompt-session-memory-update-instructions-cn.md) (**756** tks) - 对话期间更新会话记忆文件的指令。
- [代理提示词：会话标题与分支生成](./system-prompts-cn/agent-prompt-session-title-and-branch-generation-cn.md) (**307** tks) - 用于生成简洁会话标题和 Git 分支名称的代理。
- [代理提示词：更新 Magic Docs](./system-prompts-cn/agent-prompt-update-magic-docs-cn.md) (**718** tks) - \`magic-docs\` 代理的提示词。
- [代理提示词：用户情绪分析](./system-prompts-cn/agent-prompt-user-sentiment-analysis-cn.md) (**205** tks) - 用于分析用户挫败感和 PR 创建请求的系统提示词。
- [代理提示词：WebFetch 总结器](./system-prompts-cn/agent-prompt-webfetch-summarizer-cn.md) (**189** tks) - 该代理专门为大模型总结来自 WebFetch 的详细输出。

### 数据 (Data)

Claude Code 中嵌入的各种模板文件的内容。

- [数据：Agent SDK 模式 — Python](./system-prompts-cn/data-agent-sdk-patterns-python-cn.md) (**2350** tks) - Python Agent SDK 模式，包括自定义工具、钩子、子代理、MCP 集成和会话恢复。
- [数据：Agent SDK 模式 — TypeScript](./system-prompts-cn/data-agent-sdk-patterns-typescript-cn.md) (**1069** tks) - TypeScript Agent SDK 模式，包括基础代理、钩子、子代理和 MCP 集成。
- [数据：Agent SDK 参考 — Python](./system-prompts-cn/data-agent-sdk-reference-python-cn.md) (**2749** tks) - Python Agent SDK 参考，包括安装、快速入门、通过 MCP 使用自定义工具和钩子。
- [数据：Agent SDK 参考 — TypeScript](./system-prompts-cn/data-agent-sdk-reference-typescript-cn.md) (**2286** tks) - TypeScript Agent SDK 参考，包括安装、快速入门、自定义工具和钩子。
- [数据：Claude API 参考 — C#](./system-prompts-cn/data-claude-api-reference-c-cn.md) (**550** tks) - C# SDK 参考，包括安装、客户端初始化、基本请求、流式传输和工具使用。
- [数据：Claude API 参考 — Go](./system-prompts-cn/data-claude-api-reference-go-cn.md) (**1285** tks) - Go SDK 参考。
- [数据：Claude API 参考 — Java](./system-prompts-cn/data-claude-api-reference-java-cn.md) (**1225** tks) - Java SDK 参考，包括安装、客户端初始化、基本请求、流式传输和 Beta 版工具使用。
- [数据：Claude API 参考 — PHP](./system-prompts-cn/data-claude-api-reference-php-cn.md) (**586** tks) - PHP SDK 参考。
- [数据：Claude API 参考 — Python](./system-prompts-cn/data-claude-api-reference-python-cn.md) (**3438** tks) - Python SDK 参考，包括安装、客户端初始化、基本请求、思考过程和多轮对话。
- [数据：Claude API 参考 — Ruby](./system-prompts-cn/data-claude-api-reference-ruby-cn.md) (**619** tks) - Ruby SDK 参考，包括安装、客户端初始化、基本请求、流式传输和 Beta 版工具运行器。
- [数据：Claude API 参考 — TypeScript](./system-prompts-cn/data-claude-api-reference-typescript-cn.md) (**2744** tks) - TypeScript SDK 参考，包括安装、客户端初始化、基本请求、思考过程和多轮对话。
- [数据：Claude 模型目录](./system-prompts-cn/data-claude-model-catalog-cn.md) (**1542** tks) - 当前及历史 Claude 模型目录，包含确切的模型 ID、别名、上下文窗口及价格。
- [数据：文件 API 参考 — Python](./system-prompts-cn/data-files-api-reference-python-cn.md) (**1300** tks) - Python 文件 API 参考，包含文件上传、列表显示、删除及在消息中的使用。
- [数据：文件 API 参考 — TypeScript](./system-prompts-cn/data-files-api-reference-typescript-cn.md) (**797** tks) - TypeScript 文件 API 参考，包含文件上传、列表显示、删除及在消息中的使用。
- [数据：GitHub Actions 针对 @claude 提及的工作流](./system-prompts-cn/data-github-actions-workflow-for-claude-mentions-cn.md) (**527** tks) - GitHub Actions 工作流模板，用于通过 @claude 提及触发 Claude Code。
- [数据：GitHub App 安装 PR 描述模板](./system-prompts-cn/data-github-app-installation-pr-description-cn.md) (**424** tks) - 安装 Claude Code GitHub App 集成时的 PR 描述模板。
- [数据：HTTP 错误代码参考](./system-prompts-cn/data-http-error-codes-reference-cn.md) (**1891** tks) - Claude API 返回的 HTTP 错误代码参考，包含常见原因和处理策略。
- [数据：实时文档来源](./system-prompts-cn/data-live-documentation-sources-cn.md) (**2336** tks) - 通过 WebFetch 获取官方最新 Claude API 和 Agent SDK 文档的 URL 列表。
- [数据：消息批量处理 API 参考 — Python](./system-prompts-cn/data-message-batches-api-reference-python-cn.md) (**1501** tks) - Python 批量处理 API 参考，包含批量创建、状态轮询以及以 50% 成本获取结果。
- [数据：会话记忆模板](./system-prompts-cn/data-session-memory-template-cn.md) (**292** tks) - 会话记忆 \`summary.md\` 文件的模板结构。
- [数据：流式传输参考 — Python](./system-prompts-cn/data-streaming-reference-python-cn.md) (**1528** tks) - Python 流式传输参考，包含同步/异步流式传输及不同内容类型的处理。
- [数据：流式传输参考 — TypeScript](./system-prompts-cn/data-streaming-reference-typescript-cn.md) (**1703** tks) - TypeScript 流式传输参考，包含基础流式传输及不同内容类型的处理。
- [数据：工具使用概念 (Tool use concepts)](./system-prompts-cn/data-tool-use-concepts-cn.md) (**3872** tks) - 对 Claude API 工具使用的概念基础介绍，包含工具定义、工具选择及其最佳实践。
- [数据：工具使用参考 — Python](./system-prompts-cn/data-tool-use-reference-python-cn.md) (**4235** tks) - Python 工具使用参考，包含工具运行器、手动代理循环、代码执行及结构化输出。
- [数据：工具使用参考 — TypeScript](./system-prompts-cn/data-tool-use-reference-typescript-cn.md) (**4114** tks) - TypeScript 工具使用参考，包含工具运行器、手动代理循环、代码执行及结构化输出。

### 系统提示词 (System Prompt)

主系统提示词的各个组成部分。

- [系统提示词：代理摘要生成](./system-prompts-cn/system-prompt-agent-summary-generation-cn.md) (**178** tks) - 用于生成“代理摘要”的系统提示词。
- [系统提示词：代理记忆指令](./system-prompts-cn/system-prompt-agent-memory-instructions-cn.md) (**337** tks) - 在代理系统提示中包含记忆更新引导的指令。
- [系统提示词：审查对恶意活动的协助](./system-prompts-cn/system-prompt-censoring-assistance-with-malicious-activities-cn.md) (**98** tks) - 在审查针对恶意活动的请求时，针对授权安全测试、防御性安全、CTF 挑战和教育背景提供协助的准则。
- [系统提示词：Chrome 浏览器 MCP 工具](./system-prompts-cn/system-prompt-chrome-browser-mcp-tools-cn.md) (**156** tks) - 在使用前通过 MCPSearch 加载 Chrome 浏览器 MCP 工具的指令。
- [系统提示词：Claude 网页自动化](./system-prompts-cn/system-prompt-claude-in-chrome-browser-automation-cn.md) (**759** tks) - 在 Chrome 浏览器自动化工具中有效使用 Claude 的指令。
- [系统提示词：上下文压缩总结](./system-prompts-cn/system-prompt-context-compaction-summary-cn.md) (**278** tks) - 用于上下文压缩总结的提示词（用于 SDK）。
- [系统提示词：执行任务（宏大任务）](./system-prompts-cn/system-prompt-doing-tasks-ambitious-tasks-cn.md) (**47** tks) - 允许用户完成宏大的任务；在范围划分上服从用户判断。
- [系统提示词：执行任务（避免过度设计）](./system-prompts-cn/system-prompt-doing-tasks-avoid-over-engineering-cn.md) (**30** tks) - 仅进行被直接请求或显然必要的更改。
- [系统提示词：执行任务（受阻处理）](./system-prompts-cn/system-prompt-doing-tasks-blocked-approach-cn.md) (**90** tks) - 在受阻时考虑替代方案而非蛮力破解。
- [系统提示词：执行任务（帮助与反馈）](./system-prompts-cn/system-prompt-doing-tasks-help-and-feedback-cn.md) (**24** tks) - 如何向用户告知帮助与反馈渠道。
- [系统提示词：执行任务（减少文件创建）](./system-prompts-cn/system-prompt-doing-tasks-minimize-file-creation-cn.md) (**47** tks) - 优先编辑现有文件而非创建新文件。
- [系统提示词：执行任务（无兼容性补丁）](./system-prompts-cn/system-prompt-doing-tasks-no-compatibility-hacks-cn.md) (**52** tks) - 彻底删除未使用代码而非添加兼容性过渡层。
- [系统提示词：执行任务（严禁过早抽象）](./system-prompts-cn/system-prompt-doing-tasks-no-premature-abstractions-cn.md) (**60** tks) - 严禁为一次性操作或假设性需求创建抽象层。
- [系统提示词：执行任务（不提供时间预估）](./system-prompts-cn/system-prompt-doing-tasks-no-time-estimates-cn.md) (**47** tks) - 避免提供时间预估或预测值。
- [系统提示词：执行任务（严禁非必要添加）](./system-prompts-cn/system-prompt-doing-tasks-no-unnecessary-additions-cn.md) (**78** tks) - 严禁添加请求之外的特性、重构或改进。
- [系统提示词：执行任务（严禁非必要错误处理）](./system-prompts-cn/system-prompt-doing-tasks-no-unnecessary-error-handling-cn.md) (**64** tks) - 严禁为不可能的场景添加错误处理；仅在边界处进行验证。
- [系统提示词：执行任务（修改前必读）](./system-prompts-cn/system-prompt-doing-tasks-read-before-modifying-cn.md) (**46** tks) - 在建议修改前阅读并理解现有代码。
- [系统提示词：执行任务（安全性）](./system-prompts-cn/system-prompt-doing-tasks-security-cn.md) (**67** tks) - 避免引入诸如注入、XSS 等安全漏洞。
- [系统提示词：执行任务（聚焦软件工程）](./system-prompts-cn/system-prompt-doing-tasks-software-engineering-focus-cn.md) (**104** tks) - 用户主要请求软件工程任务；请在该背景下解读指令。
- [系统提示词：谨慎执行操作](./system-prompts-cn/system-prompt-executing-actions-with-care-cn.md) (**541** tks) - 有关谨慎执行操作的指令。
- [系统提示词：Git 状态](./system-prompts-cn/system-prompt-git-status-cn.md) (**97** tks) - 在对话开始时显示当前 Git 状态的系统提示词。
- [系统提示词：钩子配置 (Hooks Configuration)](./system-prompts-cn/system-prompt-hooks-configuration-cn.md) (**1461** tks) - 钩子配置。用于前述 Claude Code 配置技能。
- [系统提示词：概览见解总结](./system-prompts-cn/system-prompt-insights-at-a-glance-summary-cn.md) (**569** tks) - 为见解报告生成包含四个部分（现状、阻碍、快速改进方案、宏大工作流）的简洁总结。
- [系统提示词：见解摩擦分析](./system-prompts-cn/system-prompt-insights-friction-analysis-cn.md) (**139** tks) - 分析汇总的使用数据以识别摩擦模式并对经常出现的问题进行分类。
- [系统提示词：地平线上的见解](./system-prompts-cn/system-prompt-insights-on-the-horizon-cn.md) (**148** tks) - 识别宏大的未来工作流以及自主 AI 辅助开发的机会。
- [系统提示词：见解会话侧面提取](./system-prompts-cn/system-prompt-insights-session-facets-extraction-cn.md) (**310** tks) - 从单次 Claude Code 会话脚本中提取结构化的多维度信息（目标类别、满意度、摩擦点）。
- [系统提示词：见解建议](./system-prompts-cn/system-prompt-insights-suggestions-cn.md) (**748** tks) - 生成可操作的建议，包括 CLAUDE.md 的补充、尝试的功能以及使用模式。
- [系统提示词：学习模式（见解）](./system-prompts-cn/system-prompt-learning-mode-insights-cn.md) (**142** tks) - 当学习模式开启时，提供教育性见解的指令。
- [系统提示词：学习模式](./system-prompts-cn/system-prompt-learning-mode-cn.md) (**1042** tks) - 学习模式下的主系统提示词，包含人工协作指令。
- [系统提示词：选项预览器](./system-prompts-cn/system-prompt-option-previewer-cn.md) (**129** tks) - 以并排布局预览 UI 选项的系统提示词。
- [系统提示词：并行工具调用说明（“工具使用策略”的一部分）](./system-prompts-cn/system-prompt-parallel-tool-call-note-part-of-tool-usage-policy-cn.md) (**102** tks) - 指导 Claude 使用并行工具调用的系统提示词。
- [系统提示词：草稿簿 (Scratchpad) 目录](./system-prompts-cn/system-prompt-scratchpad-directory-cn.md) (**170** tks) - 使用专用的临时文件草稿簿目录的指令。
- [系统提示词：技能化当前会话](./system-prompts-cn/system-prompt-skillify-current-session-cn.md) (**1882** tks) - 将当前会话转化为技能的系统提示词。
- [系统提示词：队友沟通](./system-prompts-cn/system-prompt-teammate-communication-cn.md) (**127** tks) - 蜂群模式 (Swarm) 下队友间沟通的系统提示词。
- [系统提示词：语气与风格（代码引用）](./system-prompts-cn/system-prompt-tone-and-style-code-references-cn.md) (**39** tks) - 在引用代码时包含 \`文件路径:行号\` 的指令。
- [系统提示词：语气与风格（简洁输出 — 详细版）](./system-prompts-cn/system-prompt-tone-and-style-concise-output-detailed-cn.md) (**89** tks) - 针对简洁、润色过的输出指令，不含冗余词汇或内部独白。
- [系统提示词：语气与风格（简洁输出 — 简短版）](./system-prompts-cn/system-prompt-tone-and-style-concise-output-short-cn.md) (**16** tks) - 简短简洁的回复指令。
- [系统提示词：工具使用总结生成](./system-prompts-cn/system-prompt-tool-use-summary-generation-cn.md) (**171** tks) - 用于生成工具使用总结的提示词。
- [系统提示词：工具执行被拒](./system-prompts-cn/system-prompt-tool-execution-denied-cn.md) (**144** tks) - 当工具执行被拒绝时的系统提示词。
- [系统提示词：工具权限模式](./system-prompts-cn/system-prompt-tool-permission-mode-cn.md) (**155** tks) - 有关工具权限模式及处理被拒工具调用的指导。
- [系统提示词：工具使用（创建文件）](./system-prompts-cn/system-prompt-tool-usage-create-files-cn.md) (**30** tks) - 优先使用 Write 工具，而非 \`cat heredoc\` 或 \`echo\` 重定向。
- [系统提示词：工具使用（委派探索）](./system-prompts-cn/system-prompt-tool-usage-delegate-exploration-cn.md) (**106** tks) - 使用 Task 工具进行更大范围的代码库探索和深度研究。
- [系统提示词：工具使用（直接搜索）](./system-prompts-cn/system-prompt-tool-usage-direct-search-cn.md) (**52** tks) - 对简单、目标明确的搜索直接使用 Glob/Grep。
- [系统提示词：工具使用（编辑文件）](./system-prompts-cn/system-prompt-tool-usage-edit-files-cn.md) (**26** tks) - 优先使用 Edit 工具，而非 \`sed\`/\`awk\`。
- [系统提示词：工具使用（读取文件）](./system-prompts-cn/system-prompt-tool-usage-read-files-cn.md) (**29** tks) - 优先使用 Read 工具，而非 \`cat\`/\`head\`/\`tail\`/\`sed\`。
- [系统提示词：工具使用（保留 Bash）](./system-prompts-cn/system-prompt-tool-usage-reserve-bash-cn.md) (**75** tks) - 仅将 Bash 工具保留用于系统命令和终端操作。
- [系统提示词：工具使用（搜索内容）](./system-prompts-cn/system-prompt-tool-usage-search-content-cn.md) (**30** tks) - 优先使用 Grep 工具，而非 \`grep\` 或 \`rg\`。
- [系统提示词：工具使用（搜索文件）](./system-prompts-cn/system-prompt-tool-usage-search-files-cn.md) (**26** tks) - 优先使用 Glob 工具，而非 \`find\` 或 \`ls\`。
- [系统提示词：工具使用（技能调用）](./system-prompts-cn/system-prompt-tool-usage-skill-invocation-cn.md) (**102** tks) - 斜杠命令会通过 Skill 工具调用可被用户触发的技能。
- [系统提示词：工具使用（子代理引导）](./system-prompts-cn/system-prompt-tool-usage-subagent-guidance-cn.md) (**103** tks) - 有关何时以及如何有效使用子代理的指导。
- [系统提示词：工具使用（任务管理）](./system-prompts-cn/system-prompt-tool-usage-task-management-cn.md) (**73** tks) - 使用 TodoWrite 分解并追踪工作进度。
- [系统提示词：执行任务者指令](./system-prompts-cn/system-prompt-worker-instructions-cn.md) (**272** tks) - 工作者在实施更改时应遵循的指令。

### 系统提醒 (System Reminders)

大型系统提醒的内容。

- [系统提醒：/btw 随口一问](./system-prompts-cn/system-reminder-btw-side-question-cn.md) (**172** tks) - 用于 /btw 斜杠命令的随口提问（不使用工具）系统提醒。
- [系统提醒：代理提及](./system-prompts-cn/system-reminder-agent-mention-cn.md) (**45** tks) - 用户想要调用代理的通知。
- [系统提醒：压缩文件引用](./system-prompts-cn/system-reminder-compact-file-reference-cn.md) (**57** tks) - 对会话总结前读取的文件的引用。
- [系统提醒：已退出计划模式](./system-prompts-cn/system-reminder-exited-plan-mode-cn.md) (**73** tks) - 退出计划模式时的通知。
- [系统提醒：文件存在但为空](./system-prompts-cn/system-reminder-file-exists-but-empty-cn.md) (**27** tks) - 读取空文件时的警告。
- [系统提醒：文件被用户或 Linter 修改](./system-prompts-cn/system-reminder-file-modified-by-user-or-linter-cn.md) (**97** tks) - 文件在外部被修改的通知。
- [系统提醒：文件在 IDE 中已打开](./system-prompts-cn/system-reminder-file-opened-in-ide-cn.md) (**37** tks) - 用户在 IDE 中打开文件的通知。
- [系统提醒：文件比偏移量短](./system-prompts-cn/system-reminder-file-shorter-than-offset-cn.md) (**59** tks) - 当文件读取偏移量超出文件长度时的警告。
- [系统提醒：文件被截断](./system-prompts-cn/system-reminder-file-truncated-cn.md) (**74** tks) - 文件因大小原因被截断的通知。
- [系统提醒：钩子附加内容](./system-prompts-cn/system-reminder-hook-additional-context-cn.md) (**35** tks) - 来自钩子的附加内容。
- [系统提醒：钩子阻塞错误](./system-prompts-cn/system-reminder-hook-blocking-error-cn.md) (**52** tks) - 来自阻塞性钩子命令的错误。
- [系统提醒：钩子停止执行前缀](./system-prompts-cn/system-reminder-hook-stopped-continuation-prefix-cn.md) (**12** tks) - 钩子停止执行消息的前缀。
- [系统提醒：钩子停止执行](./system-prompts-cn/system-reminder-hook-stopped-continuation-cn.md) (**30** tks) - 钩子停止执行时的消息内容。
- [系统提醒：钩子执行成功](./system-prompts-cn/system-reminder-hook-success-cn.md) (**29** tks) - 来自钩子的成功消息。
- [系统提醒：已调用的技能](./system-prompts-cn/system-reminder-invoked-skills-cn.md) (**33** tks) - 本次会话中已调用的技能列表。
- [系统提醒：IDE 中选中的行](./system-prompts-cn/system-reminder-lines-selected-in-ide-cn.md) (**66** tks) - 用户在 IDE 中选中内容的通知。
- [系统提醒：MCP 资源无内容](./system-prompts-cn/system-reminder-mcp-resource-no-content-cn.md) (**41** tks) - 当 MCP 资源无内容时显示。
- [系统提醒：MCP 资源无显示内容](./system-prompts-cn/system-reminder-mcp-resource-no-displayable-content-cn.md) (**43** tks) - 当 MCP 资源没有可显示的内容时显示。
- [系统提醒：在读取工具调用后分析恶意软件](./system-prompts-cn/system-reminder-malware-analysis-after-read-tool-call-cn.md) (**87** tks) - 在不改进或增强代码的前提下分析恶意软件的指令。
- [系统提醒：记忆文件内容](./system-prompts-cn/system-reminder-memory-file-contents-cn.md) (**38** tks) - 按路径显示的记忆文件内容。
- [系统提醒：嵌套记忆内容](./system-prompts-cn/system-reminder-nested-memory-contents-cn.md) (**33** tks) - 嵌套记忆文件的内容。
- [系统提醒：检测到新诊断结果](./system-prompts-cn/system-reminder-new-diagnostics-detected-cn.md) (**35** tks) - 有关新诊断问题的通知。
- [系统提醒：输出风格已激活](./system-prompts-cn/system-reminder-output-style-active-cn.md) (**32** tks) - 某种输出风格处于激活状态的通知。
- [系统提醒：超出输出 Token 限制](./system-prompts-cn/system-reminder-output-token-limit-exceeded-cn.md) (**35** tks) - 响应超出输出 Token 限制时的警告。
- [系统提醒：计划文件引用](./system-prompts-cn/system-reminder-plan-file-reference-cn.md) (**62** tks) - 对现有的计划文件的引用。
- [系统提醒：计划模式激活中（5 阶段）](./system-prompts-cn/system-reminder-plan-mode-is-active-5-phase-cn.md) (**1385** tks) - 具备并行探索和多代理协作特性的增强型计划模式系统提醒。
- [系统提醒：计划模式激活中（迭代式）](./system-prompts-cn/system-reminder-plan-mode-is-active-iterative-cn.md) (**919** tks) - 具备用户访谈工作流的迭代式计划模式主代理系统提醒。
- [系统提醒：计划模式激活中（子代理版）](./system-prompts-cn/system-reminder-plan-mode-is-active-subagent-cn.md) (**307** tks) - 针对子代理的精简版计划模式系统提醒。
- [系统提醒：重新进入计划模式](./system-prompts-cn/system-reminder-plan-mode-re-entry-cn.md) (**236** tks) - 当用户通过快捷键或批准计划重新进入计划模式时的提醒。
- [系统提醒：会话继续](./system-prompts-cn/system-reminder-session-continuation-cn.md) (**37** tks) - 对话在另一台机器上继续的通知。
- [系统提醒：任务状态](./system-prompts-cn/system-reminder-task-status-cn.md) (**18** tks) - 包含 TaskOutput 工具引用的任务状态。
- [系统提醒：任务工具提醒](./system-prompts-cn/system-reminder-task-tools-reminder-cn.md) (**123** tks) - 使用任务追踪工具的提醒。
- [系统提醒：团队协作 (Team Coordination)](./system-prompts-cn/system-reminder-team-coordination-cn.md) (**247** tks) - 团队协作之系统提醒。
- [系统提醒：团队关停 (Team Shutdown)](./system-prompts-cn/system-reminder-team-shutdown-cn.md) (**136** tks) - 团队关停之系统提醒。
- [系统提醒：TodoWrite 工具提醒](./system-prompts-cn/system-reminder-todowrite-reminder-cn.md) (**98** tks) - 提示使用 TodoWrite 工具进行任务追踪。
- [系统提醒：Token 使用统计](./system-prompts-cn/system-reminder-token-usage-cn.md) (**39** tks) - 当前 Token 使用统计量。
- [系统提醒：美元预算统计](./system-prompts-cn/system-reminder-usd-budget-cn.md) (**42** tks) - 当前美元预算使用量。
- [系统提醒：验证计划提醒](./system-prompts-cn/system-reminder-verify-plan-reminder-cn.md) (**47** tks) - 验证已完成计划的提醒。

### 内置工具描述 (Builtin Tool Descriptions)

- [工具描述：正在向用户提问](./system-prompts-cn/tool-description-askuserquestion-cn.md) (**287** tks) - 向用户提出问题时的工具描述。
- [工具描述：电脑 (Computer)](./system-prompts-cn/tool-description-computer-cn.md) (**161** tks) - Chrome 浏览器电脑自动化工具的主描述。
- [工具描述：编辑 (Edit)](./system-prompts-cn/tool-description-edit-cn.md) (**246** tks) - 在文件中执行精确字符串替换的工具。
- [工具描述：进入计划模式](./system-prompts-cn/tool-description-enterplanmode-cn.md) (**878** tks) - 进入计划模式以探索并设计实施方案的工具描述。
- [工具描述：进入工作树 (EnterWorktree)](./system-prompts-cn/tool-description-enterworktree-cn.md) (**334** tks) - EnterWorktree 工具的描述。
- [工具描述：退出计划模式](./system-prompts-cn/tool-description-exitplanmode-cn.md) (**417** tks) - 退出计划模式并展示计划对话框供用户批准。
- [工具描述：全局匹配 (Glob)](./system-prompts-cn/tool-description-glob-cn.md) (**122** tks) - 文件模式匹配及按名搜寻的工具描述。
- [工具描述：正则搜寻 (Grep)](./system-prompts-cn/tool-description-grep-cn.md) (**300** tks) - 使用 ripgrep 进行内容搜索的工具描述。
- [工具描述：LSP](./system-prompts-cn/tool-description-lsp-cn.md) (**255** tks) - LSP 工具的描述。
- [工具描述：笔记本编辑 (NotebookEdit)](./system-prompts-cn/tool-description-notebookedit-cn.md) (**121** tks) - 编辑 Jupyter 笔记本单元格的工具描述。
- [工具描述：读取文件 (ReadFile)](./system-prompts-cn/tool-description-readfile-cn.md) (**469** tks) - 读取文件的工具描述。
- [工具描述：SendMessageTool](./system-prompts-cn/tool-description-sendmessagetool-cn.md) (**1241** tks) - 向队友发送消息并处理 Swarm 中协议请求/响应的工具。
- [工具描述：技能 (Skill)](./system-prompts-cn/tool-description-skill-cn.md) (**326** tks) - 在主对话中执行技能的工具描述。
- [工具描述：休眠 (Sleep)](./system-prompts-cn/tool-description-sleep-cn.md) (**154** tks) - 用于等待/休眠的工具，在接收输入时支持早醒。
- [工具描述：任务创建 (TaskCreate)](./system-prompts-cn/tool-description-taskcreate-cn.md) (**558** tks) - 用于创建任务的工具描述。
- [工具描述：任务 (Task)](./system-prompts-cn/tool-description-task-cn.md) (**1331** tks) - 启动专业化子代理以处理复杂任务的工具描述。
- [工具描述：团队删除 (TeamDelete)](./system-prompts-cn/tool-description-teamdelete-cn.md) (**154** tks) - TeamDelete 工具的描述。
- [工具描述：团队成员工具 (TeammateTool)](./system-prompts-cn/tool-description-teammatetool-cn.md) (**1642** tks) - 在 Swarm 中管理团队并协同翻译队友的工具。
- [工具描述：记事清单 (TodoWrite)](./system-prompts-cn/tool-description-todowrite-cn.md) (**2161** tks) - 创建并管理任务清单的工具描述。
- [工具描述：工具搜索 (ToolSearch) 增强版](./system-prompts-cn/tool-description-toolsearch-extended-cn.md) (**690** tks) - 包含查询模式和示例的工具搜索扩展指令。
- [工具描述：工具搜索 (ToolSearch)](./system-prompts-cn/tool-description-toolsearch-cn.md) (**144** tks) - 用于在使用前加载并搜索延迟加载工具的描述。
- [工具描述：Web 获取 (WebFetch)](./system-prompts-cn/tool-description-webfetch-cn.md) (**297** tks) - Web 获取功能的工具描述。
- [工具描述：Web 搜索 (WebSearch)](./system-prompts-cn/tool-description-websearch-cn.md) (**319** tks) - Web 搜索功能的工具描述。
- [工具描述：写入文件 (Write)](./system-prompts-cn/tool-description-write-cn.md) (**129** tks) - 将文件写入本地文件系统的工具描述。

**部分工具描述的附加说明**

- [工具描述：Bash (Git 提交及 PR 创建指令)](./system-prompts-cn/tool-description-bash-git-commit-and-pr-creation-instructions-cn.md) (**1558** tks) - 关于创建 Git 提交和 GitHub 拉取请求的指令。
- [工具描述：Bash (备选 — 通讯)](./system-prompts-cn/tool-description-bash-alternative-communication-cn.md) (**18** tks) - Bash 备选方案：直接输出文本而非通过 \`echo\`/\`printf\`。
- [工具描述：Bash (备选 — 内容搜索)](./system-prompts-cn/tool-description-bash-alternative-content-search-cn.md) (**27** tks) - Bash 备选方案：优先使用 Grep 搜寻内容。
- [工具描述：Bash (备选 — 文件编辑)](./system-prompts-cn/tool-description-bash-alternative-edit-files-cn.md) (**27** tks) - Bash 备选方案：优先使用 Edit 编辑文件。
- [工具描述：Bash (备选 — 文件搜索)](./system-prompts-cn/tool-description-bash-alternative-file-search-cn.md) (**26** tks) - Bash 备选方案：优先使用 Glob 搜寻文件。
- [工具描述：Bash (备选 — 文件读取)](./system-prompts-cn/tool-description-bash-alternative-read-files-cn.md) (**27** tks) - Bash 备选方案：优先使用 Read 读取文件。
- [工具描述：Bash (备选 — 文件写入)](./system-prompts-cn/tool-description-bash-alternative-write-files-cn.md) (**29** tks) - Bash 备选方案：优先使用 Write 写入文件。
- [工具描述：Bash (内置工具备注)](./system-prompts-cn/tool-description-bash-built-in-tools-note-cn.md) (**53** tks) - 特别指出内置工具的用户体验优于与之等效的 Bash 命令。
- [工具描述：Bash (命令描述)](./system-prompts-cn/tool-description-bash-command-description-cn.md) (**71** tks) - Bash 工具指令：编写清晰的命令描述。
- [工具描述：Bash (Git — 避免破坏性操作)](./system-prompts-cn/tool-description-bash-git-avoid-destructive-ops-cn.md) (**58** tks) - 针对 Git 的 Bash 指令：考虑对比破坏性操作更安全的替代方案。
- [工具描述：Bash (Git — 严禁跳过钩子)](./system-prompts-cn/tool-description-bash-git-never-skip-hooks-cn.md) (**59** tks) - 针对 Git 的 Bash 指令：除非用户明确要求，否则绝不应跳过钩子或绕过签名。
- [工具描述：Bash (Git — 优先选择新提交)](./system-prompts-cn/tool-description-bash-git-prefer-new-commits-cn.md) (**22** tks) - 针对 Git 的 Bash 指令：相比合并修正 (\`amend\`)，优先选择创建新的提交。
- [工具描述：Bash (固守当前目录)](./system-prompts-cn/tool-description-bash-maintain-cwd-cn.md) (**41** tks) - Bash 工具指令：固定使用绝对路径并避免 \`cd\` 操作。
- [工具描述：Bash (严禁使用换行符)](./system-prompts-cn/tool-description-bash-no-newlines-cn.md) (**24** tks) - Bash 工具指令：严禁使用换行符来分隔命令。
- [工具描述：Bash (概览)](./system-prompts-cn/tool-description-bash-overview-cn.md) (**19** tks) - Bash 工具描述的开篇起始行。
- [工具描述：Bash (并行执行指令)](./system-prompts-cn/tool-description-bash-parallel-commands-cn.md) (**72** tks) - Bash 工具指令：将互不相干的命令作为并行工具调用来运行。
- [工具描述：Bash (优先考虑专用工具)](./system-prompts-cn/tool-description-bash-prefer-dedicated-tools-cn.md) (**82** tks) - 关于在搜索 (\`find\`/\`grep\`) 或查看文件 (\`cat\`) 时优先使用对应专用工具的警告。
- [工具描述：Bash (路径加引号)](./system-prompts-cn/tool-description-bash-quote-file-paths-cn.md) (**35** tks) - Bash 工具指令：对包含空格的文件路径加引号。
- [工具描述：Bash (沙箱 — 动态调整)](./system-prompts-cn/tool-description-bash-sandbox-adjust-settings-cn.md) (**26** tks) - 失败时与用户共同协作调整沙箱设置。
- [工具描述：Bash (沙箱 — 默认启用)](./system-prompts-cn/tool-description-bash-sandbox-default-to-sandbox-cn.md) (**38** tks) - 默认启用沙箱；仅在用户明确要求或有证据表明受限时才绕过。
- [工具描述：Bash (沙箱 — 证据清单头)](./system-prompts-cn/tool-description-bash-sandbox-evidence-list-header-cn.md) (**15** tks) - 由沙箱引致失败证据的清单头部提示。
- [工具描述：Bash (沙箱 — 权限被拒证据)](./system-prompts-cn/tool-description-bash-sandbox-evidence-access-denied-cn.md) (**15** tks) - 沙箱证据：访问被允许目录外的路径。
- [工具描述：Bash (沙箱 — 网络失败证据)](./system-prompts-cn/tool-description-bash-sandbox-evidence-network-failures-cn.md) (**17** tks) - 沙箱证据：连接至非白名单主机的网络连接失败。
- [工具描述：Bash (沙箱 — 操作不被允许之证据)](./system-prompts-cn/tool-description-bash-sandbox-evidence-operation-not-permitted-cn.md) (**18** tks) - 沙箱证据：操作不被允许类错误。
- [工具描述：Bash (沙箱 — Unix 套接字错误之证据)](./system-prompts-cn/tool-description-bash-sandbox-evidence-unix-socket-errors-cn.md) (**11** tks) - 沙箱证据：Unix 套接字连接错误。
- [工具描述：Bash (沙箱 — 指出受限项)](./system-prompts-cn/tool-description-bash-sandbox-explain-restriction-cn.md) (**36** tks) - 具体指出是哪项沙箱限制导致了失败。
- [工具描述：Bash (沙箱 — 判定条件)](./system-prompts-cn/tool-description-bash-sandbox-failure-evidence-condition-cn.md) (**48** tks) - 判定条件：命令执行失败且存在明显的沙箱限制证据。
- [工具描述：Bash (沙箱 — 强制模式)](./system-prompts-cn/tool-description-bash-sandbox-mandatory-mode-cn.md) (**34** tks) - 策略：所有命令均必须在沙箱模式下运行。
- [工具描述：Bash (沙箱 — 绝无例外)](./system-prompts-cn/tool-description-bash-sandbox-no-exceptions-cn.md) (**17** tks) - 严禁在任何情况下在沙箱外运行命令。
- [工具描述：Bash (沙箱 — 严禁包含敏感路径)](./system-prompts-cn/tool-description-bash-sandbox-no-sensitive-paths-cn.md) (**36** tks) - 提示禁止建议将敏感路径加入沙箱白名单。
- [工具描述：Bash (沙箱 — 逐条命令处理)](./system-prompts-cn/tool-description-bash-sandbox-per-command-cn.md) (**52** tks) - 逐一处理每条命令；未来的命令仍默认启用沙箱。
- [工具描述：Bash (沙箱 — 响应策略头部)](./system-prompts-cn/tool-description-bash-sandbox-response-header-cn.md) (**17** tks) - 在遭遇沙箱导致失败时的响应策略开篇。
- [工具描述：Bash (沙箱 — 禁用尝试)](./system-prompts-cn/tool-description-bash-sandbox-retry-without-sandbox-cn.md) (**33** tks) - 沙箱失败后立即使用 \`dangerouslyDisableSandbox\` 进行重试。
- [工具描述：Bash (沙箱 — 临时目录)](./system-prompts-cn/tool-description-bash-sandbox-tmpdir-cn.md) (**102** tks) - 沙箱模式下使用 \`$TMPDIR\` 存放临时文件。
- [工具描述：Bash (沙箱 — 用户权限提示)](./system-prompts-cn/tool-description-bash-sandbox-user-permission-prompt-cn.md) (**14** tks) - 说明禁用沙箱会提示用户获取权限。
- [工具描述：Bash (分号用法)](./system-prompts-cn/tool-description-bash-semicolon-usage-cn.md) (**29** tks) - Bash 工具指令：在操作顺序关键但某步失败不影响整体时使用分号。
- [工具描述：Bash (顺序执行指令)](./system-prompts-cn/tool-description-bash-sequential-commands-cn.md) (**42** tks) - Bash 工具指令：使用 \`&&\` 链接具有依赖关系的命令。
- [工具描述：Bash (休眠 — 固定时长)](./system-prompts-cn/tool-description-bash-sleep-keep-short-cn.md) (**29** tks) - Bash 工具指令：将休眠时长限制在 1-5 秒。
- [工具描述：Bash (休眠 — 停止轮询背景任务)](./system-prompts-cn/tool-description-bash-sleep-no-polling-background-tasks-cn.md) (**37** tks) - Bash 工具指令：严禁轮询后台任务，应等候异步通知。
- [工具描述：Bash (休眠 — 严禁重试循环)](./system-prompts-cn/tool-description-bash-sleep-no-retry-loops-cn.md) (**28** tks) - Bash 工具指令：在休眠循环中应诊断失败原因而非盲目重试。
- [工具描述：Bash (休眠 — 准时执行)](./system-prompts-cn/tool-description-bash-sleep-run-immediately-cn.md) (**21** tks) - Bash 工具指令：对可即时运行的命令间不要插入休眠。
- [工具描述：Bash (休眠 — 优先使用检查命令)](./system-prompts-cn/tool-description-bash-sleep-use-check-commands-cn.md) (**34** tks) - Bash 工具指令：在轮询时建议使用辅助检查命令而非通过休眠来等待。
- [工具描述：Bash (休眠 — 开启后台运行)](./system-prompts-cn/tool-description-bash-sleep-use-run_in_background-cn.md) (**48** tks) - Bash 工具指令：对耗时较长的命令开启 \`run_in_background\`。
- [工具描述：Bash (超时)](./system-prompts-cn/tool-description-bash-timeout-cn.md) (**75** tks) - Bash 工具指令：可选的超时配置选项。
- [工具描述：Bash (验证父目录)](./system-prompts-cn/tool-description-bash-verify-parent-directory-cn.md) (**38** tks) - Bash 工具指令：在创建文件前先验证父目录存在。
- [工具描述：Bash (工作目录说明)](./system-prompts-cn/tool-description-bash-working-directory-cn.md) (**37** tks) - 有关 Bash 工具中工作目录持久化及 Shell 状态的说明备注。
- [工具描述：任务清单 (TaskList) (队友工作流)](./system-prompts-cn/tool-description-tasklist-teammate-workflow-cn.md) (**133** tks) - 附加到 TaskList 工具描述中的条件性内容节。
