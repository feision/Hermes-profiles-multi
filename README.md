# Hermes Agent Profiles (多实例配置文件)

在同一台机器上运行多个独立的 Hermes agent —— 每个实例拥有独立的配置、API 密钥、记忆、会话、技能和网关。

## 什么是 Profiles？

Profile 是一个完全隔离的 Hermes 环境。每个 Profile 都有自己的独立目录，包含其专属的 `config.yaml`、`.env`、`SOUL.md`、记忆 (memories)、会话 (sessions)、技能 (skills)、定时任务 (cron jobs) 和状态数据库 (state database)。

通过 Profiles，您可以为不同的用途运行不同的 Agent（例如：一个编程助手、一个个人机器人、一个研究 Agent），而无需担心任何数据交叉污染。

当您创建 Profile 时，它会自动成为一个独立的命令。例如，创建名为 `coder` 的 Profile 后，您可以直接使用 `coder chat`、`coder setup`、`coder gateway start` 等命令。

## 快速上手

```bash
hermes profile create coder # 创建 profile 并生成 "coder" 命令别名
coder setup                  # 配置 API 密钥和模型
coder chat                   # 开始对话
```

就这么简单。`coder` 现在是一个完全独立的 Agent，拥有自己的配置、记忆和所有资源。

## 创建 Profile

### 空白 Profile (Blank Profile)
```bash
hermes profile create mybot
```
创建一个带有预设基础技能的全新 Profile。运行 `mybot setup` 来配置 API 密钥、模型和网关 Token。

### 仅克隆配置 (`--clone`)
```bash
hermes profile create work --clone
```
将当前 Profile 的 `config.yaml`、`.env` 和 `SOUL.md` 复制到新 Profile 中。这意味着使用相同的 API 密钥和模型，但拥有全新的会话记录和记忆。您可以编辑 `~/.hermes/profiles/work/.env` 来更改 API 密钥，或编辑 `~/.hermes/profiles/work/SOUL.md` 来更改人格定义。

### 全量克隆 (`--clone-all`)
```bash
hermes profile create backup --clone-all
```
复制 **所有内容** —— 包括配置、API 密钥、人格定义、所有记忆、完整会话历史、技能、定时任务和插件。这是一个完整的快照，适用于备份或基于已有上下文的 Agent 进行分支开发。

### 从特定 Profile 克隆
```bash
hermes profile create work --clone --clone-from coder
```

---

## 版本历史

| 版本 | 日期 | 摘要 | 作者 |
|------|------|------|------|
| v1.0.3 | 2026-04-14 | 将文档更新为中文版，严格对齐官方指南 | openrouter/free |
| v1.0.2 | 2026-04-14 | 严格对齐官方文档 (Profiles guide) | openrouter/free |
| v1.0.1 | 2026-04-14 | 删除过时教程目录，优化 README 为纯净指南版 | openrouter/free |
| v1.0.0 | 2026-04-14 | 初始官方 Profiles 部署指南 | openrouter/free |

**官方文档**: [hermes-agent.nousresearch.com/docs/user-guide/profiles](https://hermes-agent.nousresearch.com/docs/user-guide/profiles)
