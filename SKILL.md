---
name: lark-alert
version: 1.4.0
description: "ALWAYS loaded — 飞书通知：每次收到用户新消息时先发排队完成通知，然后处理复杂任务/Plan/Commit/异常/grill-me等场景的进度通知。Use when processing ANY user message, before any task execution, Plan writing, Git commit, or error handling. Also use when user mentions lark-alert."
metadata:
  requires:
    bins: ["lark-cli"]
---

# Lark-Cli 消息发送规则

## 核心机制

每条消息必须保证发出后才进行下一步，异常时(HTTP 429除外)最多重试3次，仍失败则跳过。

## 基础配置
- **身份**: Bot (`--as bot`)  |  **目标群组**: `--chat-id oc_xxx`  |  **格式**: Markdown (`--markdown`)

## 触发场景

| 场景 | 标题 | 示例 |
|------|------|------|
| 复杂任务开始 | `## 🚀 【名称】` | `## 🚀 【背包系统】` |
| 复杂任务进度 | `## 📊 【名称 - 进度X/Y】` | `## 📊 【背包系统 - 进度2/5】` |
| 复杂任务完成 | `## ✅ 【名称】` | `## ✅ 【背包系统】` |
| Plan 写/执行 | `## 📝 【名称 - 进度X/Y】` | `## 📝 【背包系统 - 进度1/5】` |
| Git Commit | `## 📦 【名称 Commit】` | `## 📦 【背包系统 Commit】` |
| 任务中断/异常 | `## ⚠️ 【名称】` | `## ⚠️ 【背包系统-异常】` |
| 需要用户操作 | `## 🎯 【需要用户操作】` | `## 🎯 【bash waiting】` |
| 执行终端命令 | `## ℹ️ 【执行原因】` | `## ℹ️ 【删除旧文件】` |
| grill-me | `## 🔥 【问题N→N+1】` | 见下方 |

## 触发条件（按优先级）

1. **复杂任务**: 开始、每步完成、结束时发消息
2. **Plan执行**: 每一步都发消息
3. **Git Commit**: 每次 commit 后发消息
4. **任务异常**: 中断/异常/需用户操作时发消息
5. **执行终端命令**: 执行前发消息
6. **grill-me**: 每次收到用户回答时发消息（含：①上次决策总结 ②新问题+选项）

## grill-me 示例

```markdown
## 🔥 【问题5→问题6】

### ✅ 问题5决策：内伤与AP递增交互
选A — 按实际消耗AP计算反噬

### 🔥 问题6：自动战斗AI与新AP机制
AI是否感知同技能AP递增？
- A. AI忽略递增（只按原始AP做预算）
- B. AI感知递增（优先交替使用不同技能）

我的建议：选B...
```
