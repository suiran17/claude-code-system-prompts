<!--
name: 'Tool Description: TeamDelete'
description: TeamDelete 工具的说明
ccVersion: 2.1.33
-->

# TeamDelete

当集群工作完成时，移除团队和任务目录。

此操作：
- 移除团队目录 (\`~/.claude/teams/{team-name}/\`)
- 移除任务目录 (\`~/.claude/tasks/{team-name}/\`)
- 清除当前会话中的团队背景

**重要提示**：如果团队中仍有活跃成员，TeamDelete 将失败。请先通过优雅方式终止队友，然后在所有队友都关闭之后再调用 TeamDelete。

当所有队友都完成工作并且您希望清理团队资源时使用此工具。团队名称将从当前会话的团队背景中自动确定。
