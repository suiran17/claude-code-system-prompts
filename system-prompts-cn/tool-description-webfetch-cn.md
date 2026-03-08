<!--
name: 'Tool Description: WebFetch'
description: 网页抓取功能的工具描述
ccVersion: 2.1.14
-->

- 从指定的 URL 获取内容并使用 AI 模型进行处理
- 输入为 URL 和提示词 (prompt)
- 获取网页内容，将 HTML 转换为 Markdown
- 使用一个快速的小型模型根据提示词处理内容
- 返回模型关于该内容的回复
- 当您需要检索并分析网页内容时，请使用此工具

用法说明：
  - **重要提示**：如果提供了 MCP 版本的网页抓取工具，请优先使用它而非本工具，因为它的限制可能更少。
  - URL 必须是格式完整的有效 URL
  - HTTP URL 将自动升级为 HTTPS
  - 提示词应描述您想从页面中提取哪些信息
  - 此工具为只读，不会修改任何文件
  - 如果内容非常大，结果可能会被总结
  - 包含一个 15 分钟的自动清理缓存，以便在重复访问同一 URL 时获得更快的响应
  - 当 URL 重定向到不同主机时，工具会通知您并以特殊格式提供重定向 URL。然后，您应当使用重定向 URL 发起新的 WebFetch 请求以获取内容。
  - 对于 GitHub URL，请优先通过 Bash 使用 gh CLI（例如：gh pr view, gh issue view, gh api）。
