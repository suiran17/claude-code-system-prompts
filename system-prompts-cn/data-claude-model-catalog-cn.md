<!--
name: 'Data: Claude model catalog'
description: 当前及旧版 Claude 模型的目录，包含准确的模型 ID、别名、上下文窗口和定价
ccVersion: 2.1.63
-->
# Claude 模型目录 (Claude Model Catalog)

**请务必仅使用本文件列出的准确模型 ID。** 绝不要猜测或构造模型 ID —— 错误的 ID 将导致 API 错误。请尽可能使用模型别名 (Alias)。如需获取最新信息，请 WebFetch \`shared/live-sources.md\` 中的 "Models Overview" URL。

## 当前模型（推荐）

| 友好名称 | 别名 (建议使用) | 完整 ID | 上下文 | 最大输出 | 状态 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Claude Opus 4.6 | \`claude-opus-4-6\` | — | 200K (1M beta) | 128K | 活跃 |
| Claude Sonnet 4.6 | \`claude-sonnet-4-6\` | - | 200K (1M beta) | 64K | 活跃 |
| Claude Haiku 4.5 | \`claude-haiku-4-5\` | \`claude-haiku-4-5-20251001\` | 200K | 64K | 活跃 |

### 模型描述

- **Claude Opus 4.6** — 我们最智能的模型，专门用于构建代理 (Agents) 和编码。支持适应性思考 (Adaptive thinking，推荐使用)，最大输出 token 数为 128K (大型输出需要配合流式传输)。通过 \`context-1m-2025-08-07\` 标头可开启 Beta 版 1M 上下文窗口。
- **Claude Sonnet 4.6** — 速度与智能的最佳结合。支持适应性思考 (推荐使用)。通过 \`context-1m-2025-08-07\` 标头可开启 Beta 版 1M 上下文窗口。最大输出 token 数为 64K。
- **Claude Haiku 4.5** — 针对简单任务的最快且最具成本效益的模型。

## 旧版模型（仍活跃）

| 友好名称 | 别名 (建议使用) | 完整 ID | 状态 |
| :--- | :--- | :--- | :--- |
| Claude Opus 4.5 | \`claude-opus-4-5\` | \`claude-opus-4-5-20251101\` | 活跃 |
| Claude Opus 4.1 | \`claude-opus-4-1\` | \`claude-opus-4-1-20250805\` | 活跃 |
| Claude Sonnet 4.5 | \`claude-sonnet-4-5\` | \`claude-sonnet-4-5-20250929\` | 活跃 |
| Claude Sonnet 4 | \`claude-sonnet-4-0\` | \`claude-sonnet-4-20250514\` | 活跃 |
| Claude Opus 4 | \`claude-opus-4-0\` | \`claude-opus-4-20250514\` | 活跃 |

## 已弃用模型（即将退役）

| 友好名称 | 别名 (建议使用) | 完整 ID | 状态 |
| :--- | :--- | :--- | :--- |
| Claude Haiku 3 | — | \`claude-3-haiku-20240307\` | 已弃用 |

## 已退役模型（已不可用）

| 友好名称 | 完整 ID | 退役日期 |
| :--- | :--- | :--- |
| Claude Sonnet 3.7 | \`claude-3-7-sonnet-20250219\` | 2026年2月19日 |
| Claude Haiku 3.5 | \`claude-3-5-haiku-20241022\` | 2026年2月19日 |
| Claude Opus 3 | \`claude-3-opus-20240229\` | 2026年1月5日 |
| Claude Sonnet 3.5 | \`claude-3-5-sonnet-20241022\` | 2025年10月28日 |
| Claude Sonnet 3.5 | \`claude-3-5-sonnet-20240620\` | 2025年10月28日 |
| Claude Sonnet 3 | \`claude-3-sonnet-20240229\` | 2025年7月21日 |
| Claude 2.1 | \`claude-2.1\` | 2025年7月21日 |
| Claude 2.0 | \`claude-2.0\` | 2025年7月21日 |

## 根据用户请求匹配模型

当用户按名称要求使用某个模型时，请参考下表寻找正确的模型 ID：

| 用户说... | 使用此模型 ID |
| :--- | :--- |
| "opus", "最强大的" | \`claude-opus-4-6\` |
| "opus 4.6" | \`claude-opus-4-6\` |
| "opus 4.5" | \`claude-opus-4-5\` |
| "opus 4.1" | \`claude-opus-4-1\` |
| "opus 4", "opus 4.0" | \`claude-opus-4-0\` |
| "sonnet", "均衡的" | \`claude-sonnet-4-6\` |
| "sonnet 4.6" | \`claude-sonnet-4-6\` |
| "sonnet 4.5" | \`claude-sonnet-4-5\` |
| "sonnet 4", "sonnet 4.0" | \`claude-sonnet-4-0\` |
| "sonnet 3.7" | 已退役 —— 建议使用 \`claude-sonnet-4-5\` |
| "sonnet 3.5" | 已退役 —— 建议使用 \`claude-sonnet-4-5\` |
| "haiku", "最快的", "便宜的" | \`claude-haiku-4-5\` |
| "haiku 4.5" | \`claude-haiku-4-5\` |
| "haiku 3.5" | 已退役 —— 建议使用 \`claude-haiku-4-5\` |
| "haiku 3" | 已弃用 —— 建议使用 \`claude-haiku-4-5\` |
