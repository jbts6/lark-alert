# Lark Alert — 让 AI 主动给你发飞书通知

> 一个 AI Agent Skill，让 Claude Code / Trae 等 AI 编程助手在关键时刻主动推送飞书消息——任务开始、进度更新、完成通知、异常告警、Git Commit，一个都不漏。

---

## ✨ 它能做什么？

安装后，AI 会在以下场景**自动**给你发飞书消息：

| 场景 | 飞书消息效果 | 何时触发 |
|------|------------|---------|
| 🚀 任务开始 | `## 🚀 【实现背包管理系统】` | AI 判断为复杂任务时 |
| 📊 进度更新 | `## 📊 【实现背包管理系统 - 进度2/5】` | 每个步骤完成时 |
| ✅ 任务完成 | `## ✅ 【实现背包管理系统】` | 任务全部完成时 |
| 📝 Plan 执行 | `## 📝 【背包管理系统 - 进度1/5】` | 写/执行 Plan 每一步 |
| 📦 Git Commit | `## 📦 【背包管理系统 Commit】` | 每次 git commit 后 |
| ⚠️ 异常告警 | `## ⚠️ 【背包管理系统-异常】` | 任务中断/异常时 |
| 🎯 需要操作 | `## 🎯 【需要用户操作】` | 需要 bash 确认等 |
| ⚠️ 执行命令 | `## ⚠️ 【删除旧文件】` | 执行终端命令前 |
| 🔥 grill-me | `## 🔥 【问题5→问题6】` | grill-me 每轮问答 |

**核心价值：** 你不用一直盯着终端——切到飞书，AI 会主动告诉你发生了什么。

---

## 📦 安装

### 前置条件

1. **安装 Lark CLI**

   ```bash
   npm install -g @larksuite/cli
   ```

2. **初始化飞书应用**

   ```bash
   lark-cli config init --new
   ```

   按提示在飞书开放平台创建应用，获取 App ID 和 App Secret。

3. **安装 Lark Skills**

   ```bash
   npx skills add larksuite/cli -g -y
   ```

4. **验证安装**

   ```bash
   lark-cli im +messages-send --as bot --chat-id oc_xxx --text "Hello from AI!"
   ```

### 安装 Skill

#### 方式一：手动创建（推荐）

创建文件 `~/.agents/skills/lark-alert/SKILL.md`（Trae 用户创建在 `~/.trae-cn/skills/飞书排队提醒/SKILL.md`），粘贴下方 [SKILL.md 完整内容](#skillmd-完整内容)。

#### 方式二：一键安装（即将支持）

```bash
npx skills add lark-alert -g -y
```

---

## 🚀 快速开始

### 1. 获取你的飞书群 chat_id

```bash
# 搜索群聊
lark-cli im +chat-search --keyword "群名"

# 或列出机器人所在的群
lark-cli im chats list --as bot
```

### 2. 修改 SKILL.md 中的目标群组

将 `--chat-id oc_xxx` 替换为你的群 ID。

### 3. 开始使用

直接在 AI 对话中工作即可——AI 会自动判断何时发送通知。

**你说：** "帮我实现一个背包管理系统"

**AI 自动执行：**

```
1. 判断为复杂任务 → 发送 🚀 开始通知
2. 完成步骤 1   → 发送 📊 进度 1/4
3. 完成步骤 2   → 发送 📊 进度 2/4
4. git commit   → 发送 📦 Commit 通知
5. 全部完成     → 发送 ✅ 完成通知
```

---

## 📖 消息格式详解

### 统一格式

所有消息使用 Markdown 格式，标题格式为 `## emoji 【具体名称】`：

```bash
lark-cli im +messages-send --as bot --chat-id oc_xxx --markdown "<消息内容>"
```

### 各场景示例

#### 🚀 复杂任务开始

```markdown
## 🚀 【实现背包管理系统】

这是一个复杂任务，包含以下步骤：
1. 分析需求
2. 设计架构
3. 实现核心功能
4. 测试验证
```

#### 📊 复杂任务进度

```markdown
## 📊 【实现背包管理系统 - 进度2/4】

已完成：设计架构
内容：...
```

#### ✅ 复杂任务完成

```markdown
## ✅ 【实现背包管理系统】

任务完成！
内容：...
```

#### 📝 Plan 执行

```markdown
## 📝 【背包管理系统 - 进度1/5】

步骤 1：分析需求
内容：...
```

#### 📦 Git Commit

```markdown
## 📦 【背包管理系统 Commit】

Commit: feat: add inventory system
内容：新增背包物品增删改查功能
```

#### ⚠️ 任务异常

```markdown
## ⚠️ 【实现背包管理系统-异常】

原因：数据库连接超时
建议：检查数据库服务是否正常运行
```

#### 🎯 需要用户操作

```markdown
## 🎯 【需要用户操作】

bash waiting — 需要你确认是否删除旧文件
```

#### ⚠️ 执行终端命令

```markdown
## ⚠️ 【删除旧文件】

命令：rm -rf /tmp/old-build/
原因：清理过期的构建缓存，释放磁盘空间
```

#### 🔥 grill-me 问答

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

---

## ⚙️ 自定义配置

### 修改目标群组

在 SKILL.md 中找到 `--chat-id oc_xxx`，替换为你的群 ID。

### 多群路由

在 SKILL.md 中配置多个群，AI 根据场景选择：

```markdown
## 基础配置
- **开发群**: `--chat-id oc_dev_xxx`
- **告警群**: `--chat-id oc_alert_xxx`
- **产品群**: `--chat-id oc_product_xxx`
```

### 修改 emoji 和标题格式

| 原格式 | 替代格式 |
|--------|---------|
| `## 🚀 【任务名称】` | `## 🔥 [任务名称]` |
| `## ✅ 【任务名称】` | `## 🎉 [任务名称] Done` |
| `## ⚠️ 【任务名称】` | `## 🚨 [任务名称] Alert` |

### 添加新场景

在 SKILL.md 的触发场景表中添加新行：

```markdown
| 代码审查 | `## 🔍 【PR审查】` | 审查结果 | `## 🔍 【PR #123 审查】` |
| 部署通知 | `## 🚢 【部署】` | 部署状态 | `## 🚢 【生产环境部署】` |
```

### 发送给个人私聊

将 `--chat-id oc_xxx` 替换为 `--user-id ou_xxx`：

```bash
lark-cli im +messages-send --as bot --user-id ou_xxx --markdown "..."
```

### 使用用户身份发送

将 `--as bot` 替换为 `--as user`（需先 `lark-cli auth login`）：

```bash
lark-cli im +messages-send --as user --chat-id oc_xxx --markdown "..."
```

---

## 🏗️ 工作原理

```
┌──────────────────┐     ┌──────────────┐     ┌──────────────┐
│  AI 编程助手      │     │  Lark CLI     │     │  飞书服务器    │
│  (Claude Code /  │────→│  (npm 全局包) │────→│              │
│   Trae / Cursor) │     │              │     │              │
└──────────────────┘     └──────────────┘     └──────────────┘
        │
        │  lark-alert Skill
        │  定义触发时机 + 消息格式 + 发送命令
        │
        │  lark-im Skill（依赖）
        │  定义 lark-cli 命令参数和用法
        └───────────────────┘
```

**工作流程：**

1. AI 判断当前场景（复杂任务 / Commit / 异常 / grill-me / ...）
2. 根据 Skill 规则确定消息格式和标题
3. 调用 `lark-cli im +messages-send` 发送消息
4. 飞书服务器推送消息到群聊/私聊
5. AI 确认消息发送成功后继续执行

---

## ❓ 常见问题

### Q: 消息发送失败怎么办？

AI 会自动重试最多 3 次（HTTP 429 限流除外）。如果仍然失败，跳过此次消息继续执行任务。

常见原因及修复：

| 错误 | 原因 | 修复 |
|------|------|------|
| Permission denied | 机器人未加入目标群 | 把机器人拉进群 |
| invalid chat_id | 群 ID 不正确 | `lark-cli im +chat-search --keyword "群名"` |
| token expired | Token 过期 | 重新 `lark-cli config init` |
| scope not granted | 缺少权限 | 在飞书开发者后台开通 `im:message:send_as_bot` |

### Q: 支持 Windows 吗？

支持。lark-cli 跨平台。Windows PowerShell 中注意多行文本引号：

```powershell
lark-cli im +messages-send --as bot --chat-id oc_xxx --markdown '## 🚀 【任务开始】

这是一个复杂任务'
```

### Q: Skill 和全局 User Rule 有什么区别？

| | Skill | 全局 User Rule |
|---|---|---|
| **作用范围** | 需要主动调用或被 AI 识别 | 始终生效 |
| **适合场景** | 复杂的、需要详细指导的操作 | 简单的、需要始终记住的规范 |
| **优先级** | 调用时生效 | 始终作为背景知识 |
| **可移植性** | ✅ 可分享给他人 | ❌ 仅本地生效 |

**建议：** 将 SKILL.md 的核心内容同时配置为全局 User Rule，确保 AI 始终记住发送规范。

### Q: 如何同时配置为全局规则？

#### Trae IDE

设置 → Rules → User Rules → 添加 SKILL.md 中的「Lark-Cli 消息发送规则」部分

#### Claude Code

在 `~/.claude/settings.json` 的 `userRules` 中添加规则内容

---

## 🤝 贡献

欢迎提交 Issue 和 PR！

- GitHub: [https://github.com/你的用户名/lark-alert](https://github.com/你的用户名/lark-alert)
- 问题反馈: [GitHub Issues](https://github.com/你的用户名/lark-alert/issues)

---

## 📄 许可证

MIT
