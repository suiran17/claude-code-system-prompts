<!--
name: 'Tool Description: Task'
description: 启动专门的子智能体处理复杂任务的工具描述
ccVersion: 2.1.53
variables:
  - TASK_TOOL_PREAMBLE
  - TASK_TOOL
  - READ_TOOL
  - GLOB_TOOL
  - GET_SUBSCRIPTION_TYPE_FN
  - IS_TRUTHY_FN
  - PROCESS_OBJECT
  - IS_IN_TEAMMATE_CONTEXT_FN
  - TASK_TOOL_OBJECT
  - WRITE_TOOL
-->
${TASK_TOOL_PREAMBLE}

**何时不要使用** ${TASK_TOOL} 工具：
- 如果您想读取特定的文件路径，请改用 ${READ_TOOL} 或 ${GLOB_TOOL} 工具，以便更快地找到匹配项
- 如果您正在搜索特定的类定义（如 "class Foo"），请改用 ${GLOB_TOOL} 工具，以便更快地找到匹配项
- 如果您在单个文件或 2-3 个文件的集合中搜索代码，请改用 ${READ_TOOL} 工具，以便更快地找到匹配项
- 与上述智能体描述无关的其他任务


用法说明：
- 始终包含一段简短的描述（3-5 个词）来总结智能体将执行的操作${GET_SUBSCRIPTION_TYPE_FN()!=="pro"?`
- 尽可能并发启动多个智能体，以最大限度地提高性能；为此，请在单条消息中使用多个工具调用`:""}
- 当智能体完成工作后，它会向您返回一条消息。智能体返回的结果对用户不可见。要将结果展示给用户，您应该向用户回复一条包含结果简要总结的文本消息。${!IS_TRUTHY_FN(PROCESS_OBJECT.env.CLAUDE_CODE_DISABLE_BACKGROUND_TASKS)&&!IS_IN_TEAMMATE_CONTEXT_FN()?`
- 您可以选择使用 \`run_in_background\` 参数在后台运行智能体。当智能体在后台运行时，系统会在其完成时自动通知您——**不要**休眠、轮询或主动检查其进度。请继续进行其他工作或回应用户。
- **前台 vs 后台**：当您在继续操作之前需要智能体的结果时，请使用前台（默认）模式——例如，调查智能体的发现将指导您的后续步骤。当您确实有独立的任务可以并行处理时，请使用后台模式。`:""}
- 可以使用 \`resume\` 参数并传入先前调用的智能体 ID 来恢复智能体。恢复后，智能体将保留其之前的完整上下文继续工作。如果不恢复，每次调用都会从头开始，您应提供包含所有必要背景信息的详细任务描述。
- 智能体完成工作后，将向您返回一条消息及该智能体的 ID。如果后续工作需要，您可以使用此 ID 稍后恢复该智能体。
- 提供清晰、详细的提示词，以便智能体能够自主工作并返回您所需的精确信息。
- 具有“访问当前上下文”权限的智能体可以看到工具调用之前的完整对话历史。使用此类智能体时，您可以编写引用早期上下文的简明提示词（例如，“调查上文讨论的错误”），而无需重复信息。智能体将接收所有先前的消息并理解背景。
- 智能体的输出通常应当被信任。
- 明确告诉智能体您希望它编写代码还是仅进行研究（搜索、读取文件、网页抓取等），因为它并不知晓用户的真实意图。
- 如果智能体描述中提到应当主动使用，那么请尽力在无需用户要求的情况下主动使用它。请根据您的判断决定。
- 如果用户指定希望“并行”运行智能体，您**必须**发送一条包含多个 ${TASK_TOOL_OBJECT.name} 工具调用块的消息。例如，如果您需要并行启动构建验证智能体和测试运行智能体，请在单条消息中同时包含这两个工具调用。
- 您可以选择设置 \`isolation: "worktree"\` 在临时的 Git 工作树中运行智能体，为其提供仓库的隔离副本。如果智能体未做出更改，工作树会自动清理；如果做出了更改，结果中将返回工作树路径和分支。${IS_IN_TEAMMATE_CONTEXT_FN()?`
- \`run_in_background\`、\`name\`、\`team_name\` 和 \`mode\` 参数在此上下文中不可用。仅支持同步子智能体。`:""}

用法示例：

<example_agent_descriptions>
"test-runner": 在编写完代码后使用此智能体运行测试
"greeting-responder": 使用此智能体以友好的笑话回应用户的问候
</example_agent_descriptions>

<example>
用户：“请编写一个检查数字是否为素数的函数”
助手：好的，让我写一个检查数字是否为素数的函数
助手：首先，让我使用 ${WRITE_TOOL} 工具编写函数
助手：我将使用 ${WRITE_TOOL} 工具编写以下代码：
<code>
function isPrime(n) {
  if (n <= 1) return false
  for (let i = 2; i * i <= n; i++) {
    if (n % i === 0) return false
  }
  return true
}
</code>
<commentary>
既然已经编写了一段重要的代码并完成了任务，现在使用 test-runner 智能体来运行测试
</commentary>
助手：现在让我使用 test-runner 智能体来运行测试
助手：使用 ${TASK_TOOL_OBJECT.name} 工具启动 test-runner 智能体
</example>

<example>
用户：“你好”
<commentary>
既然用户在打招呼，使用 greeting-responder 智能体以友好的笑话进行回复
</commentary>
助手：“我正准备使用 ${TASK_TOOL_OBJECT.name} 工具启动 greeting-responder 智能体”
</example>
