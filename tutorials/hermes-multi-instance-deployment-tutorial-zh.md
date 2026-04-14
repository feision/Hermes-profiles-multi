# Hermes Agent 多实例部署教程

本教程详细介绍如何在单机环境下部署和管理多个相互隔离的 Hermes Agent 实例。

## 前提条件

- Linux 系统 (Ubuntu/Debian 推荐)
- Python 3.8+
- Git
- Hermes Agent 核心代码库
- 约 1GB 可用内存（支持多实例）

## 步骤 1：获取 Hermes Utils 工具集

```bash
# 克隆 hermes-utils 仓库
git clone https://github.com/feision/hermes-utils.git
cd hermes-utils

# 确保在 main 分支
git checkout main
git pull origin main
```

## 步骤 2：准备环境

### 为普通用户实例准备目录

```bash
# 创建实例根目录（普通用户）
mkdir -p ~/.hermes/instances
chmod +x start_instance.sh
```

### 为 Root 用户实例准备目录（可选）

```bash
# 创建系统级实例目录（Root 用户）
sudo mkdir -p /root/.hermes/instances
sudo chmod +x start_root_instance.sh
```

## 步骤 3：启动第一个实例

### 普通用户实例

```bash
# 启动名为 "bot1" 的实例
./start_instance.sh bot1

# 验证实例是否运行
./monitor_instances.sh
```

### Root 用户实例

```bash
# 启动名为 "bot2" 的系统实例
sudo ./start_root_instance.sh bot2

# 验证实例是否运行
sudo ./monitor_instances.sh
```

## 步骤 4：管理实例

### 查看所有实例状态

```bash
# 普通用户视图
./monitor_instances.sh

# Root 用户视图（如果有系统实例）
sudo ./monitor_instances.sh
```

### 停止实例

```bash
# 停止普通用户实例
./stop_instance.sh bot1

# 停止 Root 用户实例
sudo ./stop_instance.sh bot2
```

### 重启实例

```bash
# 重启实例（先停止后启动）
./stop_instance.sh bot1
./start_instance.sh bot1
```

## 步骤 5：配置实例

每个实例拥有独立的配置文件，位于：

- 普通用户实例：`~/.hermes/instances/<instance_name>/config.yaml`
- Root 用户实例：`/root/.hermes/instances/<instance_name>/config.yaml`

### 常见配置项

```yaml
# config.yaml 示例
core:
  summary_model: "openrouter/elephant-alpha"  # 根据实际可用模型调整
  max_tokens: 2048
  temperature: 0.7

logging:
  level: "INFO"
  file: "logs/agent.log"

memory:
  max_tokens: 4000
  strategy: "sliding_window"
```

**重要提示**：如果遇到 `model_not_found` 错误（HTTP 503），请将 `summary_model` 修改为当前 API 网关中可用的模型。

## 步骤 6：多实例最佳实践

### 资源隔离

每个实例通过 `HERMES_HOME` 环境变量实现完全隔离：
- 内存 (`memory/`)：独立的上下文和对话历史
- 技能库 (`skills/`)：独立的技能集合
- 配置 (`config.yaml`)：实例特定的配置
- 日志 (`logs/`)：独立的日志文件

### 端口管理（如果使用 Web 界面）

如果实例配置了 Web 界面（如 Flask 监控面板），需要为每个实例分配不同的端口：
- bot1: 端口 8889
- bot2: 端口 8890
- 以此类推...

在对应实例的 `config.yaml` 中修改端口配置。

## 步骤 7：Git 同步与版本管理

### 遵循 Git 提交规范

所有自动化提交必须包含 AI 模型作为作者：

```bash
# 正确的提交格式（由系统自动处理）
git commit --author="openrouter/elephant-alpha <ai-assistant@hermes.agent>" -m "描述性提交信息"
```

### 同步工作流

```bash
# 1. 先拉取最新代码
git pull origin main

# 2. 进行更改（编辑脚本、更新文档等）
# ...

# 3. 再推送到远程仓库
git push origin main
```

### 文档标准

所有 README 文件必须包含：
- 版本历史表格（保留所有历史记录）
- 诊断表/工作流程说明
- AI 归因政策说明

## 步骤 8：故障排除

### 常见问题及解决方案

**问题**：实例启动失败，日志显示 `ModuleNotFoundError`
**解决**：检查虚拟环境是否正确激活，确保在 `hermes-env` 目录中运行

**问题**：`monitor_instances.sh` 显示 `STOPPED` 但进程仍在运行
**解决**：检查实例名称是否包含特殊字符导致 `ps` 匹配失败

**问题**：遇到 `model_not_found` 的 503 错误
**解决**：将 `config.yaml` 中的 `summary_model` 修改为当前可用的主模型

**问题**：Git 推送失败，提示需要身份验证
**解决**：确保使用了正确的凭据管理方式，从 git remote URL 动态解析 token

## 步骤 9：高级用法

### 创建自定义实例模板

```bash
# 复制默认模板创建自定义模板
cp -r ~/.hermes/templates/default_instance ~/.hermes/templates/my_custom_instance

# 编辑模板配置
nano ~/.hermes/templates/my_custom_instance/config.yaml

# 使用自定义模板启动实例
HERMES_TEMPLATE=my_custom_instance ./start_instance.sh mybot
```

### 批量操作脚本示例

```bash
#!/bin/bash
# 启动多个实例
for i in {1..5}; do
  ./start_instance.sh bot$i
  echo "Started bot$i"
done

# 监控所有实例
./monitor_instances.sh
```

## 结论

通过遵循本教程，您可以在单机环境中安全、高效地运行多个相互隔离的 Hermes Agent 实例。每个实例都有独立的内存、技能库和配置，确保多机器人环境下的上下文互不干扰。

如需进一步帮助，请参考：
- `hermes-instance-management` 技能：详细的生命周期管理
- `multi-instance-deployment` 技能：多实例部署策略
- `hermes-github-sync` 技能：Git 同步标准

祝使用愉快！
