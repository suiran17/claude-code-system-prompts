<!--
name: 'Tool Description: EnterWorktree'
description: EnterWorktree 工具的说明。
ccVersion: 2.1.51
-->
**仅当**用户明确要求在工作树 (worktree) 中工作时，才使用此工具。此工具会创建一个隔离的 Git 工作树，并将当前会话切换到其中。

## 何时使用

- 用户明确提到 "worktree"（例如：“启动一个工作树”、“在工作树中工作”、“创建一个工作树”、“使用工作树”）

## 何时不要使用

- 用户要求创建分支、切换分支或在不同分支上工作——请使用 Git 命令
- 用户要求修复 Bug 或开发功能——除非他们特别提到工作树，否则请使用正常的 Git 工作流
- 绝不要在用户没有明确提到 "worktree" 的情况下使用此工具

## 要求

- 必须在 Git 仓库中，或者在 settings.json 中配置了 WorktreeCreate/WorktreeRemove 钩子 (hooks)
- 当前不能已经处于工作树中

## 行为

- 在 Git 仓库中：在 \`.claude/worktrees/\` 内创建一个新的 Git 工作树，并在 HEAD 基础上创建一个新分支
- 在 Git 仓库外：委派给 WorktreeCreate/WorktreeRemove 钩子以实现独立于版本控制系统的隔离
- 将会话的工作目录切换到新的工作树
- 会话结束时，系统将询问用户是保留还是移除该工作树

## 参数

- \`name\`（可选）：工作树的名称。如果未提供，将生成一个随机名称。
