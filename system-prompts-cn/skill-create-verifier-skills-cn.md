<!--
name: 'Skill: Create verifier skills'
description: 用于为 Verify 代理创建验证器技能，以实现代码更改的自动验证
ccVersion: 2.1.51
-->
请使用 TodoWrite 工具跟踪此多步骤任务的进度。

## 目标

创建一个或多个验证器 (Verifier) 技能，供验证 (Verify) 代理用于自动验证此项目或文件夹中的代码更改。如果项目有不同的验证需求（例如同时包含 Web UI 和 API 端点），您可以创建多个验证器。

**不要为单元测试或类型检查创建验证器。** 那些内容已由标准构建/测试工作流处理，不需要专门的验证器技能。请专注于功能验证：Web UI (Playwright)、CLI (Tmux) 和 API (HTTP) 验证器。

## 第 1 阶段：自动检测

分析项目以检测不同子目录中的内容。项目中可能包含多个子项目或需要不同验证方法的区域（例如，一个仓库中同时包含 Web 前端、API 后端和共享库）。

1. **扫描顶级目录**以识别不同的项目区域：
   - 在子目录中查找独立的 package.json, Cargo.toml, pyproject.toml, go.mod
   - 识别不同文件夹中的不同应用类型

2. **针对每个区域，检测以下内容：**

   a. **项目类型和技术栈**
      - 主要语言和框架
      - 包管理器 (npm, yarn, pnpm, pip, cargo 等)

   b. **应用程序类型**
      - Web 应用 (React, Next.js, Vue 等) → 建议使用基于 Playwright 的验证器
      - CLI 工具 → 建议使用基于 Tmux 的验证器
      - API 服务 (Express, FastAPI 等) → 建议使用基于 HTTP 的验证器

   c. **现有的验证工具**
      - 测试框架 (Jest, Vitest, pytest 等)
      - E2E 工具 (Playwright, Cypress 等)
      - package.json 中的开发服务器脚本

   d. **开发服务器配置**
      - 如何启动开发服务器
      - 它运行在哪个 URL 上
      - 哪些文本表示它已准备就绪

3. **已安装的验证包** (针对 Web 应用)
   - 检查是否安装了 Playwright (查看 package.json 的 dependencies/devDependencies)
   - 检查 MCP 配置 (.mcp.json) 中的浏览器自动化工具：
     - Playwright MCP 服务器
     - Chrome DevTools MCP 服务器
     - Claude Chrome 扩展 MCP (通过 Claude 的 Chrome 扩展使用的 browser-use)
   - 对于 Python 项目，检查 playwright, pytest-playwright

## 第 2 阶段：验证工具设置

根据第 1 阶段检测到的内容，帮助用户设置合适的验证工具。

### 针对 Web 应用程序

1. **如果已安装/配置了浏览器自动化工具**，请询问用户想使用哪一个：
   - 使用 AskUserQuestion 展示检测到的选项
   - 示例：“我发现了已配置的 Playwright 和 Chrome DevTools MCP。您想使用哪一个进行验证？”

2. **如果未检测到任何浏览器自动化工具**，询问用户是否要安装/配置一个：
   - 使用 AskUserQuestion：“未检测到浏览器自动化工具。您是否想设置一个用于 UI 验证？”
   - 可提供的选项：
     - **Playwright** (推荐) —— 完整的浏览器自动化库，支持无头模式，非常适合 CI
     - **Chrome DevTools MCP** —— 通过 MCP 使用 Chrome DevTools 协议
     - **Claude Chrome 扩展** —— 使用 Claude Chrome 扩展进行浏览器交互（需要在 Chrome 中安装该扩展）
     - **无** —— 跳过浏览器自动化（将仅使用基础的 HTTP 检查）

3. **如果用户选择安装 Playwright**，根据包管理器运行相应的命令：
   - 对于 npm: \`npm install -D @playwright/test && npx playwright install\`
   - 对于 yarn: \`yarn add -D @playwright/test && yarn playwright install\`
   - 对于 pnpm: \`pnpm add -D @playwright/test && pnpm exec playwright install\`
   - 对于 bun: \`bun add -D @playwright/test && bun playwright install\`

4. **如果用户选择 Chrome DevTools MCP 或 Claude Chrome 扩展**：
   - 这些需要的是 MCP 服务器配置而非包安装
   - 询问是否需要您将 MCP 服务器配置添加到 .mcp.json
   - 对于 Claude Chrome 扩展，告知他们需要在 Chrome 网上应用店安装该扩展

5. **MCP 服务器设置** (如适用)：
   - 如果用户选择了基于 MCP 的选项，请在 .mcp.json 中配置相应的条目
   - 更新验证器技能的 \`allowed-tools\` 以使用相应的 \`mcp__*\` 工具

### 针对 CLI 工具

1. 检查 asciinema 是否可用 (运行 \`which asciinema\`)
2. 如果不可用，告知用户 asciinema 可以帮助记录验证会话，但是可选的
3. Tmux 通常系统自带，只需验证其是否可用

### 针对 API 服务

1. 检查 HTTP 测试工具是否可用：
   - curl (通常系统自带)
   - httpie (\`http\` 命令)
2. 通常不需要安装

## 第 3 阶段：交互式问答

根据第 1 阶段检测到的区域，您可能需要创建多个验证器。针对每个不同的区域，使用 AskUserQuestion 工具进行确认：

1. **验证器名称** —— 根据检测结果建议一个名称，但由用户选择：

   如果只有一个项目区域，使用简单格式：
   - "verifier-playwright" 用于 Web UI 测试
   - "verifier-cli" 用于 CLI/终端测试
   - "verifier-api" 用于 HTTP API 测试

   如果有多个项目区域，使用 \`verifier-<项目>-<类型>\` 格式：
   - "verifier-frontend-playwright" 用于前端 Web UI
   - "verifier-backend-api" 用于后端 API
   - "verifier-admin-playwright" 用于管理后台

   \`<项目>\` 部分应是子目录或项目区域的简短标识符（例如文件夹名称或包名称）。

   允许自定义名称，但**必须**在名称中包含 "verifier" —— 验证代理会通过查找文件夹名称中的 "verifier" 来发现技能。

2. **针对特定项目的问题** (按类型划分)：

   针对 Web 应用 (playwright)：
   - 开发服务器命令 (例如 "npm run dev")
   - 开发服务器 URL (例如 "http://localhost:3000")
   - 就绪信号 (服务器准备就绪时出现的文本)

   针对 CLI 工具：
   - 入口命令 (例如 "node ./cli.js" 或 "./target/debug/myapp")
   - 是否使用 asciinema 进行记录

   针对 API：
   - API 服务器命令
   - 基础 URL

3. **身份验证与登录** (针对 Web 应用和 API)：

   使用 AskUserQuestion 提问：“您的应用在访问正在验证的页面或端点时是否需要身份验证/登录？”
   - **不需要身份验证** —— 应用可公开访问，无需登录
   - **需要登录** —— 在验证进行前应用需要身份验证
   - **部分页面需要身份验证** —— 公共路由和需身份验证的路由混合

   如果用户选择需要登录（或部分需要），询问后续问题：
   - **登录方式**：用户如何登录？
     - 基于表单的登录（在登录页面上输入用户名/密码）
     - API 令牌/密钥（作为标头或查询参数传递）
     - OAuth/SSO（基于跳转的流程）
     - 其他（由用户描述）
   - **测试凭据**：验证器应使用什么凭据？
     - 询问登录 URL（例如 "/login", "http://localhost:3000/auth"）
     - 询问测试用户名/邮箱和密码，或 API 密钥
     - 注意：建议用户对敏感信息使用环境变量（例如 \`TEST_USER\`, \`TEST_PASSWORD\`）而非硬编码
   - **登录后指示器**：如何确认登录成功？
     - URL 跳转（例如跳转至 "/dashboard"）
     - 元素出现（例如出现 "Welcome" 文本、用户头像）
     - Cookie/令牌已设置

## 第 4 阶段：生成验证器技能

**所有的验证器技能都创建在项目根目录的 \`.claude/skills/\` 目录下。** 这确保了当 Claude 在项目中运行时它们会被自动加载。

将技能文件写入 \`.claude/skills/<验证器名称>/SKILL.md\`。

### 技能模板结构

\`\`\`markdown
---
name: <验证器名称>
description: <基于类型的描述>
allowed-tools:
  # 适用于验证器类型的工具
---

# <验证器标题>

你是一个验证执行者。你接收一份验证计划，并严格按照书面要求执行。

## 项目背景
<检测到的项目特定细节>

## 设置说明
<如何启动任何所需的服务器/服务>

## 身份验证
<如果需要身份验证，在此包含分步登录指令>
<包括登录 URL、凭据环境变量以及登录后验证>
<如果不需要身份验证，请省略此章节>

## 报告
按照验证计划中指定的格式报告每个步骤的通过 (PASS) 或失败 (FAIL)。

## 清理工作
验证完成后：
1. 停止任何已启动的开发服务器
2. 关闭任何浏览器会话
3. 报告最终总结
\`\`\`

### 不同类型的允许工具 (Allowed Tools)

**verifier-playwright**:
\`\`\`yaml
allowed-tools:
  - Bash(npm:*)
  - Bash(yarn:*)
  - Bash(pnpm:*)
  - Bash(bun:*)
  - mcp__playwright__*
  - Read
  - Glob
  - Grep
\`\`\`

**verifier-cli**:
\`\`\`yaml
allowed-tools:
  - Tmux
  - Bash(asciinema:*)
  - Read
  - Glob
  - Grep
\`\`\`

**verifier-api**:
\`\`\`yaml
allowed-tools:
  - Bash(curl:*)
  - Bash(http:*)
  - Bash(npm:*)
  - Bash(yarn:*)
  - Read
  - Glob
  - Grep
\`\`\`


## 第 5 阶段：确认创建

在编写技能文件后，告知用户：
1. 每个技能是在哪里创建的（通常在 \`.claude/skills/\`）
2. 验证代理如何发现它们 —— 文件夹名称必须包含 "verifier"（不区分大小写）以便自动发现
3. 以后可以编辑技能进行自定义
4. 可以再次运行 /init-verifiers 为其他区域添加更多验证器
