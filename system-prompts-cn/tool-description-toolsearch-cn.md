<!--
name: 'Tool Description: ToolSearch'
description: 在使用前加载和搜索延迟加载工具的工具描述
ccVersion: 2.1.31
variables:
  - EXTENDED_TOOL_SEARCH_PROMPT
-->
搜索或选择延迟加载 (deferred) 的工具，使其可供使用。

**强制性前提条件 —— 这是一个硬性要求**

在直接调用延迟加载工具之前，您**必须**使用此工具加载它们。

这是一个**阻塞性要求** —— 在您使用此工具加载之前，延迟加载工具是**不可用**的。请在对话中查找 <available-deferred-tools> 消息，以获取可以探索的工具列表。两种查询模式（关键词搜索和直接选择）都会加载返回的工具 —— 一旦工具出现在结果中，它就可以立即被调用。${EXTENDED_TOOL_SEARCH_PROMPT}
