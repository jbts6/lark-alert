# Lark Alert — 让 AI 主动给你发飞书通知

[![skills.sh](https://img.shields.io/badge/install-via%20skills.sh-blue)](https://skills.sh)
[![license](https://img.shields.io/badge/license-MIT-green)](LICENSE)

一个 AI Agent Skill，让 Claude Code / Trae 等 AI 编程助手在关键时刻主动推送飞书消息。

要想在Trae里实现排队通知的效果，需要将下面写到全局Rules里。

```
每次收到用户新消息后，你的第一条响应必须是飞书通知（用lark-cli发送），告知用户"AI已开始处理"。发完通知后，再继续回复用户的消息或执行任务。
调用lark-cli时，将"oc_xxx" 替换为 “你的chat_id”
```

## 一键安装

```bash
npx skills add jbts6/lark-alert -g -y
```

安装后修改 SKILL.md 中的 `--chat-id oc_xxx` 为你的飞书群 ID。

## 它能做什么？

| 场景 | 飞书消息效果 | 何时触发 |
|------|------------|----------|
| 🚀 任务开始 | `## 🚀 【背包系统】` | AI 判断为复杂任务时 |
| 📊 进度更新 | `## 📊 【背包系统 - 进度2/5】` | 每个步骤完成时 |
| ✅ 任务完成 | `## ✅ 【背包系统】` | 任务全部完成时 |
| 📝 Plan 执行 | `## 📝 【背包系统 - 进度1/5】` | 写/执行 Plan 每一步 |
| 📦 Git Commit | `## 📦 【背包系统 Commit】` | 每次 git commit 后 |
| ⚠️ 异常告警 | `## ⚠️ 【背包系统-异常】` | 任务中断/异常时 |
| 🎯 需要操作 | `## 🎯 【需要用户操作】` | 需要 bash 确认等 |
| ℹ️ 执行命令 | `## ℹ️ 【删除旧文件】` | 执行终端命令前 |
| 🔥 grill-me | `## 🔥 【问题5→问题6】` | grill-me 每轮问答 |

## 前置条件

1. 安装 Lark CLI：`npm install -g @larksuite/cli`
2. 初始化飞书应用：`lark-cli config init --new`
3. 安装 Lark Skills：`npx skills add larksuite/cli -g -y`

## 自定义

修改 SKILL.md 中的配置：

- `--chat-id oc_xxx` → 替换为你的飞书群 ID
- `--as bot` → 替换为 `--as user` 使用用户身份发送
- `--chat-id` → 替换为 `--user-id ou_xxx` 发送给个人私聊

## 许可证

MIT
