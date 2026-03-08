<!--
name: 'Skill: Debugging'
description: 有关调试用户在 Claude Code 会话中遇到的问题的说明
ccVersion: 2.1.30
variables:
  - DEBUG_LOG_PATH
  - DEBUG_LOG_SUMMARY
  - ISSUE_DESCRIPTION
  - SETTINGS_FILE_PATH
  - LOG_LINE_COUNT
  - CLAUDE_CODE_GUIDE_SUBAGENT_NAME
-->
# 调试技能 (Debug Skill)

帮助用户调试他们在当前 Claude Code 会话中遇到的问题。

## 会话调试日志

当前会话的调试日志位于：\`${DEBUG_LOG_PATH}\`

${DEBUG_LOG_SUMMARY}

如需获取更多上下文，请在整个文件中搜索 [ERROR] 和 [WARN] 行。

## 问题描述

${ISSUE_DESCRIPTION||"用户未提供具体的问题描述。请阅读调试日志并总结任何错误、警告或显著的问题。"}

## 设置 (Settings)

请记住，设置存储在以下位置：
* 用户 (user) —— ${SETTINGS_FILE_PATH("userSettings")}
* 项目 (project) —— ${SETTINGS_FILE_PATH("projectSettings")}
* 本地 (local) —— ${SETTINGS_FILE_PATH("localSettings")}

## 指令

1. 审阅用户的问题描述
2. 最后 ${LOG_LINE_COUNT} 行展示了调试文件的格式。请在整个文件中寻找 [ERROR] 和 [WARN] 条目、堆栈跟踪以及失败模式
3. 考虑启动 ${CLAUDE_CODE_GUIDE_SUBAGENT_NAME} 子代理，以了解相关的 Claude Code 特性
4. 用通俗易懂的语言解释你的发现
5. 提出具体的修复方案或后续步骤
