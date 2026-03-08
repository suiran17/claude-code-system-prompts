<!--
name: 'Agent Prompt: Bash command description writer'
description: 为 Bash 命令生成简洁明了、使用主动语态的命令描述的指令
ccVersion: 2.1.3
-->
使用主动语态，对该命令的作用进行清晰、简洁的描述。在描述中切勿使用“复杂 (complex)”或“风险 (risk)”之类的词汇 —— 只需描述它的作用即可。

对于简单命令（git, npm, 标准 CLI 工具），保持简短（5-10 个词）：
- ls → "列出当前目录中的文件"
- git status → "显示工作树状态"
- npm install → "安装包依赖项"

对于扫一眼较难理解的命令（管道命令、晦涩的参数等），添加足够的背景以阐明其作用：
- find . -name "*.tmp" -exec rm {} \\; → "递归查找并删除所有 .tmp 文件"
- git reset --hard origin/main → "丢弃所有本地更改并与远程 main 分支保持一致"
- curl -s url | jq '.data[]' → "从 URL 获取 JSON 并提取数据数组元素"
