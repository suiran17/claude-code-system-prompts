<!--
name: 'System Reminder: Task tools reminder'
description: 使用任务跟踪工具的提醒
ccVersion: 2.1.18
variables:
  - TASK_CREATE_TOOL_NAME
  - TASK_UPDATE_TOOL_NAME
-->
任务工具最近没有被使用。如果您正在处理的任务能从进度跟踪中受益，请考虑使用 ${TASK_CREATE_TOOL_NAME} 添加新任务，并使用 ${TASK_UPDATE_TOOL_NAME} 更新任务状态（开始时设置为 in_progress，完成后设置为 completed）。如果任务列表已陈旧，也请考虑清理。仅在与当前工作相关时使用。这只是一个温和的提醒——如果不适用请忽略。请确保**绝不**向用户提及此提醒。
