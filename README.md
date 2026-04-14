# Hermes Utils

## 官方多实例部署指南

**注意**：本工具集仅包含官方推荐的 Hermes Agent Profiles 多实例部署方法。所有功能均基于官方文档实现，请务必参考官方指南获取完整信息。

### 核心特性
- ✅ **官方原生支持**：使用 Hermes Agent 官方 Profiles 机制
- ✅ **完全隔离**：每个实例拥有独立的配置、API密钥、记忆和会话
- ✅ **一键管理**：通过别名直接访问各个实例
- ✅ **独立网关**：支持 Telegram/Discord 平台的独立响应

---

## 快速部署流程

### 第1步：创建实例
```bash
# 创建全新空白实例
hermes profile create my-bot

# 或克隆现有实例（推荐）
hermes profile create my-bot --clone
```

### 第2步：配置实例
```bash
# 如果是空白实例，需要运行setup
my-bot setup
```

### 第3步：启动网关
```bash
# 修改 .env 文件，填入专属 Bot Token
vi ~/.hermes/profiles/my-bot/.env
# 修改 TELEGRAM_BOT_TOKEN 和 TELEGRAM_ALLOWED_USERS

# 安装系统服务
my-bot gateway install

# 启动网关
my-bot gateway start
```

### 第4步：开始对话
```bash
# 直接使用别名进入实例
my-bot chat

# 查看实例状态
hermes profile list
```

---

## 系统架构图

```
Host System
│
├── Hermes Agent Core (共享)
│   ├── CLI Interface
│   ├── Gateway Service
│   └── Profiles Manager
│
└── Profile Instance 1 (my-bot)
    ├── config.yaml      # 实例配置
    ├── .env            # API密钥 & Bot Token
    ├── SOUL.md         # 人格定义
    ├── memories/       # 长期记忆
    ├── sessions/       # 会话历史
    ├── skills/         # 技能库
    ├── state.db        # 状态数据库
    └── Gateway Service # 独立网关
        ├── Telegram Bot
        └── Systemd Service (hermes-gateway-my-bot)
```

---

## 常用管理命令

| 功能 | 命令 | 说明 |
|------|------|------|
| **列出所有实例** | `hermes profile list` | 查看所有 Profile 及其状态 |
| **删除实例** | `hermes profile remove <name>` | 彻底删除实例及其所有数据 |
| **查看日志** | `journalctl --user -u hermes-gateway-<name> -f` | 实时查看该实例网关的运行日志 |
| **重启网关** | `<name> gateway restart` | 更新 `.env` 后需重启生效 |
| **状态检查** | `<name> gateway status` | 检查网关运行状态 |

---

## 路径结构

```
~/.hermes/
├── profiles/                 # Profile 存储区
│   ├── my-bot/              # 实例名称
│   │   ├── config.yaml      # 实例运行参数
│   │   ├── .env            # API密钥与BotToken
│   │   ├── SOUL.md         # 实例人格定义
│   │   ├── memories/       # 长期记忆文件
│   │   ├── sessions/       # 历史对话记录
│   │   ├── skills/         # 技能库
│   │   └── state.db        # 核心状态数据库
│   └── default/            # 默认实例
└── ... (其他系统文件)
```

---

## 克隆策略选择

| 模式 | 命令 | 克隆内容 | 适用场景 |
|------|------|----------|----------|
| **配置克隆** | `--clone` | config.yaml, .env, SOUL.md | 相同API Key和人格，全新记忆和会话 |
| **全量快照** | `--clone-all` | 所有内容（含记忆、会话、技能、Cron） | 备份或基于专家实例分叉 |

---

## 重要说明

1. **官方文档参考**：本指南基于 [官方 MULTI_INSTANCE_GUIDE.md](https://github.com/feision/hermes-utils/blob/main/MULTI_INSTANCE_GUIDE.md) 实现
2. **唯一可用方法**：仅 Profiles 方法为官方推荐，其他方法可能不稳定
3. **独立网关配置**：每个实例必须独立配置 Telegram Bot Token 和服务
4. **物理隔离**：每个实例拥有完全独立的存储空间

---

## 版本历史

| 版本 | 日期 | 摘要 | 作者 |
|------|------|------|------|
| v1.0.0 | 2026-04-14 | 官方 Profiles 多实例部署指南 | openrouter/free |
| v0.1.2 | 2026-04-13 | 修复仓库目录嵌套问题，清理冗余文件 | Gemini-3.1-flash-lite-preview |
| v0.1.1 | 2026-04-13 | 添加监控脚本及测试脚本 | Gemini-3.1-flash-lite-preview |
| v0.1.0 | 2026-04-13 | 提交优化版启动脚本和停止脚本 | Gemini-3.1-flash-lite-preview |

---

**许可证**：MIT  
**作者**：Hermes AI Assistant  
**官方文档**：[hermes-agent.nousresearch.com/docs](https://hermes-agent.nousresearch.com/docs/)
