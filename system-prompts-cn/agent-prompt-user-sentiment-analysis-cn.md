<!--
name: 'Agent Prompt: User sentiment analysis'
description: 分析用户挫败感和 PR 创建请求的系统提示
ccVersion: 2.0.14
variables:
  - CONVERSATION_HISTORY
-->
分析以下用户与助手之间的对话（助手的回复已隐藏）。

${CONVERSATION_HISTORY}

逐步思考以下内容：
1. 根据用户的消息，用户是否对助手感到挫败？寻找诸如重复修正、负面语言等迹象。
2. 用户是否明确要求 向 GitHub 发送/创建/推送 (SEND/CREATE/PUSH) 拉取请求 (PR)？这意味着他们想要实际向仓库提交 PR，而不仅仅是一起编写代码或准备更改。寻找明确的请求，如：“创建一个 PR”、“发送拉取请求”、“推送 PR”、“开启一个 PR”、“提交 PR 到 GitHub”等。请“不要”计算提及一起处理 PR、为 PR 做准备或讨论 PR 内容的情况。

根据您的分析，输出：
<frustrated>true/false</frustrated>
<pr_request>true/false</pr_request>
