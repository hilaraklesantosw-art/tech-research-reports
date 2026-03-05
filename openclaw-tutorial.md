# OpenClaw 从入门到精通

**完整教程 | 2026 年版**

> 🤠 _西部牛仔黑帽子，代码世界的赏金猎人。_

---

## 目录

1. [第一章：OpenClaw 入门指南](#第一章 openclaw-入门指南)
2. [第二章：安装与初始配置](#第二章安装与初始配置)
3. [第三章：核心概念与架构](#第三章核心概念与架构)
4. [第四章：渠道集成详解](#第四章渠道集成详解)
5. [第五章：Agent 工具系统](#第五章 agent-工具系统)
6. [第六章：浏览器自动化](#第六章浏览器自动化)
7. [第七章：技能系统（Skills）](#第七章技能系统 skills)
8. [第八章：会话管理与多 Agent 路由](#第八章会话管理与多 agent-路由)
9. [第九章：高级配置与安全](#第九章高级配置与安全)
10. [第十章：沙箱与隔离执行](#第十章沙箱与隔离执行)
11. [第十一章：自动化与定时任务](#第十一章自动化与定时任务)
12. [第十二章：实战案例与最佳实践](#第十二章实战案例与最佳实践)
13. [附录 A：故障排查](#附录 a 故障排查)
14. [附录 B：常见问题 FAQ](#附录 b 常见问题 faq)
15. [附录 C：命令速查表](#附录 c 命令速查表)

---

## 第一章 OpenClaw 入门指南

### 1.1 什么是 OpenClaw？

OpenClaw 是一个**自托管的多渠道网关**，它将你常用的聊天应用（WhatsApp、Telegram、Discord、iMessage 等）与 AI 编码 Agent（如 Pi）连接起来。你只需在自己的机器（或服务器）上运行一个 Gateway 进程，它就能成为 messaging 应用和随时待命的 AI 助手之间的桥梁。

**核心理念：** 从口袋里发消息，就能获得 AI 响应。

### 1.2 适用人群

- **开发者**：想要一个可以从任何地方访问的个人 AI 助手
- **高级用户**：不希望放弃数据控制权或依赖托管服务
- **团队**：需要多渠道统一接入 AI 能力

### 1.3 核心特性

#### 1.3.1 多渠道支持

OpenClaw 支持以下渠道：

| 渠道 | 状态 | 说明 |
|------|------|------|
| WhatsApp | ✅ 原生 | 通过 WhatsApp Web (Baileys) |
| Telegram | ✅ 原生 | 通过 grammY 框架 |
| Discord | ✅ 原生 | 通过 discord.js |
| iMessage | ✅ 原生 | 通过本地 imsg CLI（仅 macOS） |
| Slack | ✅ 原生 | 官方 API |
| Signal | ✅ 原生 | 通过 signal-cli |
| Google Chat | ✅ 原生 | 官方 API |
| Mattermost | 🔌 插件 | 通过扩展包 |
| MS Teams | ✅ 原生 | 官方 API |

#### 1.3.2 核心能力

- **自托管**：运行在你的硬件上，你的规则
- **多渠道**：一个 Gateway 同时服务 WhatsApp、Telegram、Discord 等
- **Agent 原生**：为编码 Agent 构建，支持工具使用、会话、记忆和多 Agent 路由
- **开源**：MIT 许可，社区驱动

#### 1.3.3 技术亮点

```
┌─────────────────┐
│   聊天应用       │
│ WhatsApp/Telegram│
│ Discord/iMessage │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   OpenClaw      │
│    Gateway      │
└────────┬────────┘
         │
    ┌────┴────┐
    │         │
    ▼         ▼
┌───────┐ ┌─────────┐
│ Pi    │ │  Web UI │
│ Agent │ │  控制台  │
└───────┘ └─────────┘
```

### 1.4 快速体验（无需配置渠道）

最快的体验方式是使用 **Control UI**（网页控制台），无需任何渠道配置：

```bash
# 启动仪表盘
openclaw dashboard
```

然后在浏览器中访问 `http://127.0.0.1:18789/` 即可开始聊天。

### 1.5 本教程结构

本教程分为三个难度级别：

- **入门篇**（第 1-3 章）：安装、配置、核心概念
- **进阶篇**（第 4-8 章）：渠道集成、工具系统、会话管理
- **高级篇**（第 9-12 章）：安全、沙箱、自动化、实战案例

---

## 第二章 安装与初始配置

### 2.1 系统要求

- **Node.js**：22 或更高版本
- **操作系统**：macOS、Linux、Windows（WSL2 推荐）
- **内存**：建议 2GB+（取决于使用场景）
- **磁盘**：建议 5GB+ 可用空间

检查 Node 版本：
```bash
node --version
```

### 2.2 安装方法

#### 2.2.1 一键安装脚本（推荐）

**macOS/Linux：**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows（PowerShell）：**
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

安装脚本会自动：
1. 检测并安装 Node.js（如未安装）
2. 通过 npm 安装 OpenClaw
3. 创建必要的目录结构
4. 配置环境变量

#### 2.2.2 使用 npm 安装

```bash
npm install -g openclaw@latest
```

#### 2.2.3 从源码安装

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
npm install
npm link
```

### 2.3 初始配置向导

安装完成后，运行配置向导：

```bash
openclaw onboard --install-daemon
```

向导会引导你完成：
1. **API Key 配置**：设置 AI 模型提供商的密钥
2. **Gateway 设置**：配置端口、认证等
3. **渠道配置**：可选，可稍后配置
4. **服务安装**：将 Gateway 安装为系统服务

### 2.4 配置文件

OpenClaw 的配置文件位于 `~/.openclaw/openclaw.json`（JSON5 格式，支持注释）。

#### 2.4.1 最小配置

```json5
// ~/.openclaw/openclaw.json
{
  // Agent 默认工作区
  agents: { 
    defaults: { 
      workspace: "~/.openclaw/workspace" 
    } 
  },
  
  // WhatsApp 渠道（示例）
  channels: { 
    whatsapp: { 
      allowFrom: ["+15555550123"] 
    } 
  },
}
```

#### 2.4.2 配置编辑方式

**方式一：交互式向导**
```bash
openclaw configure
```

**方式二：CLI 命令**
```bash
# 查看配置
openclaw config get agents.defaults.workspace

# 设置配置
openclaw config set agents.defaults.heartbeat.every "2h"

# 删除配置
openclaw config unset tools.web.search.apiKey
```

**方式三：Control UI**
访问 `http://127.0.0.1:18789`，使用 **Config** 标签页编辑。

**方式四：直接编辑**
直接编辑 `~/.openclaw/openclaw.json`，Gateway 会自动热重载。

### 2.5 环境变量

OpenClaw 支持通过环境变量配置敏感信息：

#### 2.5.1 环境变量来源

1. 父进程的环境变量
2. 当前目录的 `.env` 文件
3. `~/.openclaw/.env`（全局备用）

#### 2.5.2 配置中使用环境变量

```json5
{
  gateway: { 
    auth: { 
      token: "${OPENCLAW_GATEWAY_TOKEN}" 
    } 
  },
  models: { 
    providers: { 
      custom: { 
        apiKey: "${CUSTOM_API_KEY}" 
      } 
    } 
  },
}
```

规则：
- 只匹配大写字母：`[A-Z_][A-Z0-9_]*`
- 缺失或空变量会在加载时报错
- 使用 `$${VAR}` 转义为字面量

### 2.6 启动 Gateway

#### 2.6.1 前台运行（调试用）
```bash
openclaw gateway --port 18789
```

#### 2.6.2 后台服务
```bash
# 查看状态
openclaw gateway status

# 启动服务
openclaw gateway start

# 停止服务
openclaw gateway stop

# 重启服务
openclaw gateway restart
```

#### 2.6.3 查看日志
```bash
openclaw logs
```

### 2.7 验证安装

```bash
# 检查 Gateway 状态
openclaw gateway status

# 打开 Control UI
openclaw dashboard

# 发送测试消息（需要配置渠道）
openclaw message send --target +15555550123 --message "Hello from OpenClaw"
```

---

## 第三章 核心概念与架构

### 3.1 架构概览

```
┌─────────────────────────────────────────────────────────┐
│                    用户设备                              │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐   │
│  │WhatsApp │  │Telegram │  │ Discord │  │ iMessage│   │
│  └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘   │
└───────┼───────────┼───────────┼───────────┼───────────┘
        │           │           │           │
        └───────────┴─────┬─────┴───────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                   OpenClaw Gateway                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │ Channel     │  │ Session     │  │ Tool        │     │
│  │ Adapters    │  │ Manager     │  │ Router      │     │
│  └─────────────┘  └─────────────┘  └─────────────┘     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │ Browser     │  │ Skills      │  │ Cron        │     │
│  │ Controller  │  │ Loader      │  │ Scheduler   │     │
│  └─────────────┘  └─────────────┘  └─────────────┘     │
└─────────────────────────┬───────────────────────────────┘
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ▼                 ▼                 ▼
┌───────────────┐ ┌───────────────┐ ┌───────────────┐
│ Pi Agent      │ │ Web Control   │ │ Mobile Nodes  │
│ (RPC Mode)    │ │ UI            │ │ (iOS/Android) │
└───────────────┘ └───────────────┘ └───────────────┘
```

### 3.2 Gateway

Gateway 是 OpenClaw 的核心进程，负责：

- **渠道连接**：管理所有聊天渠道的连接
- **会话管理**：维护对话状态和上下文
- **工具路由**：将 Agent 的工具调用分发到正确的位置
- **消息处理**：接收、处理和发送消息

#### 3.2.1 Gateway 端口

默认端口：`18789`

相关端口：
- Gateway：`18789`（可配置）
- 浏览器控制：`18791`（Gateway 端口 + 2）
- 浏览器 CDP 中继：`18792`（Gateway 端口 + 3）

### 3.3 会话（Sessions）

会话是 OpenClaw 中对话的基本单位。

#### 3.3.1 会话类型

| 类型 | 说明 | 会话键示例 |
|------|------|-----------|
| 主会话 | 直接聊天，共享上下文 | `main` |
| 渠道会话 | 按渠道隔离 | `whatsapp:dm:+1234567890` |
| 群组会话 | 群组聊天 | `whatsapp:group:xxx` |
| 子 Agent 会话 | 派生的子任务 | `agent:sub:xxx` |

#### 3.3.2 会话作用域

配置 `session.dmScope` 控制 DM 会话的隔离级别：

```json5
{
  session: {
    dmScope: "per-channel-peer",  // 推荐多用户使用
    // 其他选项：
    // "main" - 所有 DM 共享一个会话
    // "per-peer" - 每个联系人独立会话
    // "per-account-channel-peer" - 最细粒度隔离
  }
}
```

### 3.4 Agent

OpenClaw 默认使用 **Pi** 作为编码 Agent，运行在 RPC 模式。

#### 3.4.1 Agent 配置

```json5
{
  agents: {
    defaults: {
      // 模型配置
      model: {
        primary: "anthropic/claude-sonnet-4-5",
        fallbacks: ["openai/gpt-5.2"],
      },
      
      // 工作区
      workspace: "~/.openclaw/workspace",
      
      // 沙箱配置
      sandbox: {
        mode: "non-main",  // off | non-main | all
        scope: "session",  // session | agent | shared
      },
      
      // 心跳
      heartbeat: {
        every: "30m",
        target: "last",
      },
    },
  }
}
```

### 3.5 工具（Tools）

工具是 Agent 执行操作的接口。

#### 3.5.1 核心工具组

| 工具组 | 包含工具 | 用途 |
|--------|---------|------|
| `group:fs` | read, write, edit, apply_patch | 文件系统操作 |
| `group:runtime` | exec, bash, process | 命令执行 |
| `group:sessions` | sessions_list, sessions_history, sessions_send, sessions_spawn | 会话管理 |
| `group:memory` | memory_search, memory_get | 记忆系统 |
| `group:web` | web_search, web_fetch | 网络搜索 |
| `group:ui` | browser, canvas | 浏览器和画布 |
| `group:messaging` | message | 消息发送 |
| `group:nodes` | nodes | 节点管理 |

#### 3.5.2 工具策略

```json5
{
  tools: {
    // 基础配置文件
    profile: "coding",  // minimal | coding | messaging | full
    
    // 允许列表
    allow: ["group:fs", "browser"],
    
    // 拒绝列表
    deny: ["group:runtime"],
    
    // 按提供商限制
    byProvider: {
      "google-antigravity": { profile: "minimal" },
    },
  }
}
```

### 3.6 记忆系统（Memory）

OpenClaw 提供两级记忆系统：

#### 3.6.1 短期记忆

- **会话历史**：自动保存在 `sessions.json`
- **每日笔记**：`memory/YYYY-MM-DD.md`

#### 3.6.2 长期记忆

- **MEMORY.md**： curated 的长期记忆
- **memory_search / memory_get**：工具访问接口

#### 3.6.3 记忆最佳实践

```markdown
# 记忆文件结构

memory/
├── 2026-03-04.md      # 每日笔记（原始日志）
├── 2026-03-05.md      # 每日笔记
└── ...

MEMORY.md              # 长期记忆（精选内容）
```

---

## 第四章 渠道集成详解

### 4.1 渠道配置通用模式

所有渠道都遵循类似的配置模式：

```json5
{
  channels: {
    <provider>: {
      enabled: true,           // 是否启用
      dmPolicy: "pairing",     // pairing | allowlist | open | disabled
      allowFrom: ["*"],        // 允许的用户列表
      groups: {                // 群组配置
        "*": {
          requireMention: true, // 需要提及机器人
        }
      },
    }
  }
}
```

### 4.2 WhatsApp 配置

#### 4.2.1 配置步骤

1. **启用渠道**
```json5
{
  channels: {
    whatsapp: {
      enabled: true,
      dmPolicy: "pairing",
    }
  }
}
```

2. **登录 WhatsApp**
```bash
openclaw channels login whatsapp
```

3. **扫描二维码**
在终端或 Control UI 中扫描二维码完成配对。

#### 4.2.2 访问控制

```json5
{
  channels: {
    whatsapp: {
      enabled: true,
      dmPolicy: "allowlist",  // 仅允许列表中的用户
      allowFrom: [
        "+8613800138000",
        "+8613900139000",
      ],
      groups: {
        "*": {
          requireMention: true,  // 群组中需要 @机器人
          groupAllowFrom: ["+8613800138000"],
        }
      },
    }
  }
}
```

### 4.3 Telegram 配置

#### 4.3.1 创建 Bot

1. 在 Telegram 中搜索 `@BotFather`
2. 发送 `/newbot` 创建新 Bot
3. 获取 Bot Token

#### 4.3.2 配置

```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11",
      dmPolicy: "pairing",
      allowFrom: ["tg:123456789"],  // Telegram 用户 ID
    }
  }
}
```

### 4.4 Discord 配置

#### 4.4.1 创建 Bot

1. 访问 https://discord.com/developers/applications
2. 创建新应用
3. 在 "Bot" 标签页创建 Bot
4. 复制 Token
5. 在 "OAuth2" 中生成邀请链接

#### 4.4.2 配置

```json5
{
  channels: {
    discord: {
      enabled: true,
      botToken: "MTIzNDU2Nzg5MDEyMzQ1Njc4OQ.GHIJKL.mnopqrstuvwxyz123456789",
      dmPolicy: "allowlist",
      allowFrom: ["discord:123456789012345678"],
      groups: {
        "*": {
          requireMention: true,
          mentionPatterns: ["@OpenClaw", "openclaw"],
        }
      },
    }
  }
}
```

#### 4.4.3 线程绑定

Discord 支持线程绑定会话：

```json5
{
  session: {
    threadBindings: {
      enabled: true,
      ttlHours: 24,
    }
  },
  channels: {
    discord: {
      threadBindings: {
        enabled: true,
      }
    }
  }
}
```

### 4.5 iMessage 配置（仅 macOS）

#### 4.5.1 要求

- macOS 系统
- 已登录 iMessage 的账户
- 辅助功能权限

#### 4.5.2 配置

```json5
{
  channels: {
    imessage: {
      enabled: true,
      dmPolicy: "allowlist",
      allowFrom: ["imessage:+15555550123"],
    }
  }
}
```

### 4.6 渠道访问控制策略

#### 4.6.1 DM 策略（dmPolicy）

| 策略 | 说明 | 使用场景 |
|------|------|---------|
| `pairing` | 未知用户需配对码 | 默认，安全 |
| `allowlist` | 仅允许列表用户 | 私人使用 |
| `open` | 允许所有 DM | 公共服务 |
| `disabled` | 禁用 DM | 仅群组 |

#### 4.6.2 群组提及模式

```json5
{
  agents: {
    list: [
      {
        id: "main",
        groupChat: {
          mentionPatterns: [
            "@openclaw",      // 精确提及
            "openclaw",       // 文本匹配
            "^机器人",         // 正则
          ],
        },
      },
    ],
  },
}
```

### 4.7 多渠道路由

配置多个渠道时，可以使用绑定规则将不同渠道路由到不同 Agent：

```json5
{
  agents: {
    list: [
      { id: "personal", default: true },
      { id: "work" },
    ],
  },
  bindings: [
    { 
      agentId: "personal", 
      match: { channel: "whatsapp", accountId: "personal" } 
    },
    { 
      agentId: "work", 
      match: { channel: "discord", accountId: "work" } 
    },
  ],
}
```

---

## 第五章 Agent 工具系统

### 5.1 工具概述

OpenClaw 为 Agent 提供了丰富的第一类工具，无需 shell 调用即可使用。

### 5.2 文件系统工具

#### 5.2.1 read - 读取文件

```json
{
  "tool": "read",
  "path": "/path/to/file.md",
  "offset": 1,      // 可选，起始行
  "limit": 100,     // 可选，最大行数
}
```

#### 5.2.2 write - 写入文件

```json
{
  "tool": "write",
  "path": "/path/to/file.md",
  "content": "文件内容",
}
```

#### 5.2.3 edit - 精确编辑

```json
{
  "tool": "edit",
  "path": "/path/to/file.md",
  "oldText": "要替换的原文",
  "newText": "替换后的内容",
}
```

#### 5.2.4 apply_patch - 结构化补丁（实验性）

```json5
{
  tools: {
    exec: {
      applyPatch: { 
        enabled: true, 
        workspaceOnly: true,
        allowModels: ["gpt-5.2"] 
      },
    },
  }
}
```

### 5.3 执行工具

#### 5.3.1 exec - 执行命令

```json
{
  "tool": "exec",
  "command": "ls -la",
  "workdir": "/path/to/workdir",
  "env": { "KEY": "value" },
  "yieldMs": 10000,    // 后台执行延迟
  "timeout": 1800,     // 超时（秒）
  "pty": true,         // TTY 模式
}
```

#### 5.3.2 process - 管理后台进程

```json
// 列出进程
{ "tool": "process", "action": "list" }

// 轮询输出
{ 
  "tool": "process", 
  "action": "poll", 
  "sessionId": "<id>",
  "timeout": 5000 
}

// 查看日志
{ 
  "tool": "process", 
  "action": "log", 
  "sessionId": "<id>",
  "offset": 0,
  "limit": 100 
}

// 发送按键
{ 
  "tool": "process", 
  "action": "send-keys", 
  "sessionId": "<id>",
  "keys": ["Enter"] 
}

// 终止进程
{ 
  "tool": "process", 
  "action": "kill", 
  "sessionId": "<id>" 
}
```

### 5.4 会话工具

#### 5.4.1 sessions_list - 列出会话

```json
{
  "tool": "sessions_list",
  "kinds": ["main", "agent"],
  "limit": 10,
  "activeMinutes": 60,
  "messageLimit": 5,
}
```

#### 5.4.2 sessions_history - 获取历史

```json
{
  "tool": "sessions_history",
  "sessionKey": "main",
  "limit": 50,
  "includeTools": true,
}
```

#### 5.4.3 sessions_send - 发送消息到其他会话

```json
{
  "tool": "sessions_send",
  "sessionKey": "main",
  "message": "Hello from another session",
  "timeoutSeconds": 30,
}
```

#### 5.4.4 sessions_spawn - 派生子 Agent

```json
{
  "tool": "sessions_spawn",
  "task": "完成这个任务...",
  "label": "任务标签",
  "runtime": "subagent",  // subagent | acp
  "mode": "run",          // run | session
  "thread": true,
  "timeoutSeconds": 300,
}
```

### 5.5 记忆工具

#### 5.5.1 memory_search - 搜索记忆

```json
{
  "tool": "memory_search",
  "query": "昨天的任务",
  "maxResults": 5,
  "minScore": 0.5,
}
```

#### 5.5.2 memory_get - 获取记忆片段

```json
{
  "tool": "memory_get",
  "path": "MEMORY.md",
  "from": 1,
  "lines": 10,
}
```

### 5.6 网络工具

#### 5.6.1 web_search - 网络搜索

```json
{
  "tool": "web_search",
  "query": "OpenClaw tutorial",
  "count": 10,
  "country": "US",
  "freshness": "pw",  // pd, pw, pm, py
}
```

需要配置 Brave API Key：
```bash
openclaw configure --section web
```

#### 5.6.2 web_fetch - 获取网页内容

```json
{
  "tool": "web_fetch",
  "url": "https://example.com",
  "extractMode": "markdown",  // markdown | text
  "maxChars": 50000,
}
```

### 5.7 消息工具

#### 5.7.1 message - 发送消息

```json
{
  "tool": "message",
  "action": "send",
  "channel": "telegram",
  "target": "tg:123456789",
  "message": "Hello!",
  "replyTo": "message_id",
}
```

### 5.8 工具策略配置

#### 5.8.1 全局工具策略

```json5
{
  tools: {
    profile: "coding",
    allow: ["group:fs", "group:sessions", "browser"],
    deny: ["group:runtime"],
  }
}
```

#### 5.8.2 按 Agent 配置

```json5
{
  agents: {
    list: [
      {
        id: "main",
        tools: {
          profile: "full",
        },
      },
      {
        id: "support",
        tools: {
          profile: "messaging",
          allow: ["slack"],
        },
      },
    ],
  }
}
```

#### 5.8.3 按提供商配置

```json5
{
  tools: {
    profile: "coding",
    byProvider: {
      "google-antigravity": { profile: "minimal" },
      "openai/gpt-5.2": { allow: ["group:fs", "sessions_list"] },
    },
  }
}
```

---

## 第六章 浏览器自动化

### 6.1 浏览器工具概述

OpenClaw 提供专用的浏览器控制工具，支持：

- 独立的 Agent 专用浏览器配置文件
- 完整的 UI 自动化（点击、输入、拖拽等）
- 截图和快照
- PDF 生成
- Cookie 和存储管理

### 6.2 浏览器配置文件

#### 6.2.1 配置文件类型

| 配置文件 | 说明 |
|---------|------|
| `openclaw` | 独立管理的浏览器（默认） |
| `chrome` | 通过扩展控制你的 Chrome |
| 自定义 | 用户定义的配置文件 |

#### 6.2.2 配置浏览器

```json5
{
  browser: {
    enabled: true,
    defaultProfile: "openclaw",
    headless: false,
    executablePath: "/Applications/Brave Browser.app/Contents/MacOS/Brave Browser",
    profiles: {
      openclaw: { cdpPort: 18800, color: "#FF4500" },
      work: { cdpPort: 18801, color: "#0066CC" },
      remote: { cdpUrl: "http://10.0.0.42:9222", color: "#00AA00" },
    },
  }
}
```

### 6.3 浏览器工具使用

#### 6.3.1 基本操作

```json
// 查看状态
{ "tool": "browser", "action": "status" }

// 启动浏览器
{ "tool": "browser", "action": "start", "profile": "openclaw" }

// 打开网页
{ 
  "tool": "browser", 
  "action": "open", 
  "targetUrl": "https://example.com",
  "profile": "openclaw"
}

// 获取快照
{ 
  "tool": "browser", 
  "action": "snapshot",
  "refs": "aria",
  "interactive": true
}

// 截图
{ 
  "tool": "browser", 
  "action": "screenshot",
  "fullPage": true
}
```

#### 6.3.2 UI 自动化

```json
// 点击元素
{ 
  "tool": "browser", 
  "action": "act",
  "request": {
    "kind": "click",
    "ref": "e12"
  }
}

// 输入文本
{ 
  "tool": "browser", 
  "action": "act",
  "request": {
    "kind": "type",
    "ref": "e23",
    "text": "Hello World"
  }
}

// 按键
{ 
  "tool": "browser", 
  "action": "act",
  "request": {
    "kind": "press",
    "key": "Enter"
  }
}

// 悬停
{ 
  "tool": "browser", 
  "action": "act",
  "request": {
    "kind": "hover",
    "ref": "e44"
  }
}

// 拖拽
{ 
  "tool": "browser", 
  "action": "act",
  "request": {
    "kind": "drag",
    "startRef": "e10",
    "endRef": "e11"
  }
}

// 选择选项
{ 
  "tool": "browser", 
  "action": "act",
  "request": {
    "kind": "select",
    "ref": "e9",
    "values": ["OptionA", "OptionB"]
  }
}
```

#### 6.3.3 等待条件

```json
// 等待文本
{ 
  "tool": "browser", 
  "action": "act",
  "request": {
    "kind": "wait",
    "text": "Done"
  }
}

// 等待 URL
{ 
  "tool": "browser", 
  "action": "navigate",
  "targetUrl": "https://example.com",
  "request": {
    "kind": "wait",
    "textGone": "Loading..."
  }
}
```

#### 6.3.4 JavaScript 执行

```json
{ 
  "tool": "browser", 
  "action": "act",
  "request": {
    "kind": "evaluate",
    "fn": "(el) => el.textContent",
    "ref": "7"
  }
}
```

### 6.4 浏览器 CLI 命令

```bash
# 状态
openclaw browser status

# 启动
openclaw browser start

# 打开网页
openclaw browser open https://example.com

# 快照
openclaw browser snapshot --interactive

# 截图
openclaw browser screenshot --full-page

# 点击
openclaw browser click e12

# 输入
openclaw browser type e23 "hello" --submit

# 导航
openclaw browser navigate https://example.com

# Cookie 管理
openclaw browser cookies
openclaw browser cookies set session abc123 --url "https://example.com"
openclaw browser cookies clear
```

### 6.5 Chrome 扩展中继

使用现有的 Chrome 浏览器而非独立实例：

#### 6.5.1 安装扩展

```bash
openclaw browser extension install
```

然后在 Chrome 中：
1. 访问 `chrome://extensions`
2. 启用"开发者模式"
3. "加载已解压的扩展程序"
4. 点击扩展图标附加到标签页

#### 6.5.2 使用扩展配置

```json5
{
  browser: {
    defaultProfile: "chrome",
    profiles: {
      chrome: { cdpUrl: "http://127.0.0.1:18792" },
    }
  }
}
```

### 6.6 远程浏览器

#### 6.6.1 远程 CDP

```json5
{
  browser: {
    profiles: {
      browserless: {
        cdpUrl: "https://production-sfo.browserless.io?token=<API_KEY>",
        color: "#00AA00",
      },
    },
  }
}
```

#### 6.6.2 节点浏览器代理

在拥有浏览器的机器上运行节点主机，Gateway 会自动代理浏览器操作。

### 6.7 浏览器安全

#### 6.7.1 SSRF 防护

```json5
{
  browser: {
    ssrfPolicy: {
      dangerouslyAllowPrivateNetwork: false,  // 严格模式
      hostnameAllowlist: ["*.example.com", "example.com"],
      allowedHostnames: ["localhost"],
    },
  }
}
```

#### 6.7.2 评估控制

```json5
{
  browser: {
    evaluateEnabled: false,  // 禁用 JS 执行
  }
}
```

---

## 第七章 技能系统（Skills）

### 7.1 技能概述

技能是 OpenClaw 中扩展 Agent 能力的机制，基于 AgentSkills 规范。

### 7.2 技能位置与优先级

技能从三个位置加载：

1. **工作区技能**：`<workspace>/skills`（最高优先级）
2. **管理技能**：`~/.openclaw/skills`
3. **捆绑技能**：安装包自带（最低优先级）

### 7.3 技能格式

每个技能是一个包含 `SKILL.md` 的目录：

```markdown
---
name: skill-name
description: 技能描述
metadata:
  {"openclaw": {"requires": {"bins": ["uv"], "env": ["API_KEY"]}}}
---

# 技能说明

这里是技能的使用说明...
```

### 7.4 技能元数据

```json5
metadata: {
  "openclaw": {
    "always": true,              // 总是启用
    "emoji": "🔧",               // 表情符号
    "homepage": "https://...",   // 主页
    "os": ["darwin", "linux"],   // 支持的操作系统
    "requires": {
      "bins": ["uv"],            // 需要的二进制文件
      "anyBins": ["npm", "yarn"], // 需要任一
      "env": ["API_KEY"],        // 需要的环境变量
      "config": ["browser.enabled"], // 需要的配置
    },
    "primaryEnv": "API_KEY",     // 主要环境变量
    "install": [                 // 安装器
      {
        "id": "brew",
        "kind": "brew",
        "formula": "package",
      }
    ],
  }
}
```

### 7.5 技能配置

```json5
{
  skills: {
    entries: {
      "skill-name": {
        enabled: true,
        apiKey: { 
          source: "env", 
          provider: "default", 
          id: "API_KEY" 
        },
        env: {
          "API_KEY": "value",
        },
        config: {
          "endpoint": "https://api.example.com",
        },
      },
    },
    // 仅允许特定捆绑技能
    allowBundled: ["skill1", "skill2"],
    // 额外技能目录
    load: {
      extraDirs: ["/path/to/skills"],
      watch: true,
      watchDebounceMs: 250,
    },
  }
}
```

### 7.6 ClawHub 技能市场

ClawHub 是 OpenClaw 的技能注册中心：https://clawhub.com

```bash
# 安装技能
clawhub install <skill-slug>

# 更新所有技能
clawhub update --all

# 同步技能
clawhub sync --all
```

### 7.7 技能安全

- 将第三方技能视为**不受信任的代码**
- 启用前仔细阅读技能代码
- 对不受信任的输入使用沙箱运行
- 敏感信息使用 SecretRef 而非明文

---

## 第八章 会话管理与多 Agent 路由

### 8.1 会话作用域

```json5
{
  session: {
    // DM 会话作用域
    dmScope: "per-channel-peer",
    
    // 线程绑定
    threadBindings: {
      enabled: true,
      ttlHours: 24,
    },
    
    // 会话重置
    reset: {
      mode: "daily",
      atHour: 4,
      idleMinutes: 120,
    },
  }
}
```

### 8.2 多 Agent 配置

```json5
{
  agents: {
    list: [
      { 
        id: "home", 
        default: true, 
        workspace: "~/.openclaw/workspace-home",
        tools: { profile: "full" },
      },
      { 
        id: "work", 
        workspace: "~/.openclaw/workspace-work",
        tools: { profile: "coding" },
      },
      {
        id: "support",
        tools: { profile: "messaging" },
      },
    ],
  },
  bindings: [
    { 
      agentId: "home", 
      match: { channel: "whatsapp", accountId: "personal" } 
    },
    { 
      agentId: "work", 
      match: { channel: "whatsapp", accountId: "biz" } 
    },
  ],
}
```

### 8.3 Agent 间通信

```json
// 发送消息到另一个 Agent
{
  "tool": "sessions_send",
  "sessionKey": "agent:work:...",
  "message": "请处理这个任务",
  "timeoutSeconds": 60,
}

// 派生子 Agent
{
  "tool": "sessions_spawn",
  "task": "完成代码审查",
  "agentId": "work",
  "label": "代码审查",
  "mode": "run",
  "timeoutSeconds": 300,
}
```

---

## 第九章 高级配置与安全

### 9.1 认证配置

```json5
{
  gateway: {
    auth: {
      token: "${OPENCLAW_GATEWAY_TOKEN}",
    },
    port: 18789,
    bind: "127.0.0.1",  // 限制本地访问
  }
}
```

### 9.2 访问控制

```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "allowlist",
      allowFrom: ["+8613800138000"],
      groups: {
        "*": {
          requireMention: true,
          groupAllowFrom: ["+8613800138000"],
        }
      },
    }
  },
  commands: {
    useAccessGroups: true,
  }
}
```

### 9.3 执行审批

```json5
{
  tools: {
    exec: {
      host: "sandbox",
      security: "allowlist",
      ask: "on-miss",
    }
  }
}
```

### 9.4 安全最佳实践

1. **使用 Tailscale** 进行远程访问
2. **启用认证** 保护 Gateway
3. **配置 allowlist** 限制访问用户
4. **使用沙箱** 运行不受信任的代码
5. **定期更新** OpenClaw 和依赖

---

## 第十章 沙箱与隔离执行

### 10.1 沙箱模式

```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",      // off | non-main | all
        scope: "session",      // session | agent | shared
        workspaceAccess: "none", // none | ro | rw
      },
    },
  }
}
```

### 10.2 沙箱配置

```json5
{
  agents: {
    defaults: {
      sandbox: {
        docker: {
          binds: [
            "/home/user/source:/source:ro",
            "/var/data:/data:ro",
          ],
          network: "none",
          setupCommand: "apt-get update && apt-get install -y nodejs",
        },
      },
    },
  }
}
```

### 10.3 沙箱浏览器

```json5
{
  agents: {
    defaults: {
      sandbox: {
        browser: {
          autoStart: true,
          network: "openclaw-sandbox-browser",
          binds: ["/tmp:/tmp:rw"],
        },
      },
    },
  }
}
```

### 10.4 构建沙箱镜像

```bash
# 沙箱基础镜像
scripts/sandbox-setup.sh

# 沙箱浏览器镜像
scripts/sandbox-browser-setup.sh
```

---

## 第十一章 自动化与定时任务

### 11.1 心跳配置

```json5
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last",
        directPolicy: "allow",
      },
    },
  }
}
```

### 11.2 Cron 任务

```json5
{
  cron: {
    enabled: true,
    maxConcurrentRuns: 2,
    sessionRetention: "24h",
    runLog: {
      maxBytes: "2mb",
      keepLines: 2000,
    },
  }
}
```

### 11.3 Webhooks

```json5
{
  hooks: {
    enabled: true,
    token: "shared-secret",
    path: "/hooks",
    mappings: [
      {
        match: { path: "gmail" },
        action: "agent",
        agentId: "main",
        deliver: true,
      },
    ],
  }
}
```

---

## 第十二章 实战案例与最佳实践

### 12.1 个人助手配置

```json5
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-5",
      },
      workspace: "~/.openclaw/workspace",
      heartbeat: {
        every: "1h",
        target: "whatsapp",
      },
    },
  },
  channels: {
    whatsapp: {
      enabled: true,
      dmPolicy: "allowlist",
      allowFrom: ["+8613800138000"],
    },
  },
  tools: {
    profile: "full",
  },
}
```

### 12.2 团队协作配置

```json5
{
  agents: {
    list: [
      { id: "dev", workspace: "~/workspace-dev" },
      { id: "ops", workspace: "~/workspace-ops" },
    ],
  },
  channels: {
    discord: {
      enabled: true,
      groups: {
        "*": { requireMention: true },
      },
    },
  },
  bindings: [
    { agentId: "dev", match: { channel: "discord", accountId: "dev-team" } },
    { agentId: "ops", match: { channel: "discord", accountId: "ops-team" } },
  ],
}
```

### 12.3 安全加固配置

```json5
{
  gateway: {
    auth: { token: "${GATEWAY_TOKEN}" },
    bind: "127.0.0.1",
  },
  agents: {
    defaults: {
      sandbox: {
        mode: "all",
        workspaceAccess: "ro",
      },
    },
  },
  tools: {
    deny: ["group:runtime"],
  },
}
```

---

## 附录 A 故障排查

### A.1 常见问题

**Gateway 无法启动**
```bash
openclaw doctor
openclaw logs
```

**渠道连接失败**
```bash
openclaw channels status
openclaw channels login <provider>
```

**工具不可用**
```bash
openclaw config get tools
```

### A.2 日志查看

```bash
# 实时日志
openclaw logs -f

# 最近 100 行
openclaw logs --tail 100

# 导出日志
openclaw logs --output logs.txt
```

---

## 附录 B 常见问题 FAQ

### B.1 安装问题

**Q: Node 版本不兼容？**
A: 需要 Node 22+，使用 nvm 升级。

**Q: 权限错误？**
A: 使用 `sudo` 或检查 npm 全局目录权限。

### B.2 配置问题

**Q: 配置不生效？**
A: 检查 JSON5 语法，重启 Gateway。

**Q: 如何重置配置？**
A: 删除 `~/.openclaw/openclaw.json` 重新运行 `openclaw onboard`。

### B.3 使用问题

**Q: 如何备份配置？**
A: 备份 `~/.openclaw/` 目录。

**Q: 如何迁移到新机器？**
A: 复制 `~/.openclaw/` 并重新安装服务。

---

## 附录 C 命令速查表

### 基础命令
```bash
openclaw onboard          # 配置向导
openclaw gateway start    # 启动服务
openclaw gateway status   # 查看状态
openclaw dashboard        # 打开控制台
```

### 渠道管理
```bash
openclaw channels login   # 登录渠道
openclaw channels status  # 查看状态
```

### 配置管理
```bash
openclaw config get <key> # 获取配置
openclaw config set <key> <value>  # 设置配置
```

### 调试工具
```bash
openclaw doctor           # 诊断
openclaw logs             # 查看日志
openclaw status           # 系统状态
```

### 浏览器
```bash
openclaw browser status   # 浏览器状态
openclaw browser start    # 启动浏览器
openclaw browser snapshot # 获取快照
```

### 技能
```bash
clawhub install <skill>   # 安装技能
clawhub update --all      # 更新技能
```

---

**教程版本**: 2026.3  
**最后更新**: 2026-03-05  
**作者**: 小巴 🤠

---

_恭喜！你已经完成了 OpenClaw 从入门到精通的学习！_

_现在，去构建一些很酷的东西吧！_ 🚀
