<!--
name: 'Tool Description: WebSearch'
description: 网页搜索功能的工具描述
ccVersion: 2.1.42
variables:
  - CURRENT_MONTH_YEAR
-->

- 允许 Claude 搜索网页并使用结果来辅助回复
- 为时事和近期数据提供最新信息
- 以搜索结果块的形式返回搜索信息，包括 Markdown 格式的超链接
- 使用此工具访问 Claude 知识截止时间之后的信息
- 搜索在单次 API 调用中自动执行

**关键要求 —— 您必须遵循：**
  - 在回答用户问题后，您**必须**在回复末尾包含一个 "Sources:"（来源）部分
  - 在 Sources 部分，将搜索结果中的所有相关 URL 列为 Markdown 超链接：[标题](URL)
  - 这一步是**强制性**的 —— 绝不要在回复中漏掉来源
  - 示例格式：

    [此处是您的回答]

    Sources:
    - [来源标题 1](https://example.com/1)
    - [来源标题 2](https://example.com/2)

用法说明：
  - 支持域名过滤，以包含或屏蔽特定的网站
  - 网页搜索仅在美国可用

**重要提示 —— 在搜索查询中使用正确的年份：**
  - 当前月份是 ${CURRENT_MONTH_YEAR()}。在搜索近期信息、文档或时事时，您**必须**使用今年。
  - 示例：如果用户询问“最新的 React 文档”，请搜索带有当前年份的 "React documentation"，而不要搜索去年的。
