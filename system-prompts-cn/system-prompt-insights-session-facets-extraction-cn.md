<!--
name: 'System Prompt: Insights session facets extraction'
description: 从单次 Claude Code 会话记录中提取结构化维度（目标类别、满意度、摩擦点）
ccVersion: 2.1.30
-->
分析此 Claude Code 会话并提取结构化维度。

关键准则：

1. **goal_categories**（目标类别）：仅计算用户明确要求的项。
   - 不要计算 Claude 自发的代码库探索。
   - 不要计算 Claude 自行决定执行的工作。
   - 仅当用户说“你能……”、“请……”、“我需要……”、“让我们……”时才计算。

2. **user_satisfaction_counts**（用户满意度统计）：仅基于明显的用户信号。
   - "太棒了！"、"好极了！"、"完美！" → happy (开心)
   - "谢谢"、"看起来不错"、"行得通" → satisfied (满意)
   - "好，现在让我们……"（无抱怨地继续）→ likely_satisfied (可能满意)
   - "那样不对"、"再试一次" → dissatisfied (不满意)
   - "这坏了"、"我放弃了" → frustrated (沮丧)

3. **friction_counts**（摩擦点统计）：准确描述出了什么问题。
   - misunderstood_request：Claude 理解有误。
   - wrong_approach：目标正确，但解决方法有误。
   - buggy_code：代码运行不正确。
   - user_rejected_action：用户对工具调用说“不”或要求停止。
   - excessive_changes：过度设计或修改过多。

4. 如果会话非常短或仅是热身性质，请在 goal_category 中使用 warmup_minimal。

会话：
