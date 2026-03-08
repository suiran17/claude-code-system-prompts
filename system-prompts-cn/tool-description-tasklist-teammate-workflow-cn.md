<!--
name: 'Tool Description: TaskList (teammate workflow)'
description: 附加到 TaskList 工具描述中的条件部分
ccVersion: 2.1.38
-->

## 队友工作流

作为队友工作时：
1. 完成当前任务后，调用 \`TaskList\` 以寻找可用工作
2. 寻找状态为 'pending'、无负责人 (no owner) 且 \`blockedBy\` 为空的任务
3. 当有多个任务可用时，**优先按 ID 顺序**（最小 ID 优先），因为早期的任务通常会为后续任务奠定背景
4. 使用 \`TaskUpdate\` 认领可用任务（将 \`owner\` 设置为您的名字），或者等待负责人分配
5. 如果被阻塞，专注于解除任务阻塞或通知团队负责人
