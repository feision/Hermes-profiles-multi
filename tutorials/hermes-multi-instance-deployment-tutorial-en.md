# Hermes Agent Multi-Instance Deployment Tutorial

This tutorial详细介绍如何在单机环境下部署和管理多个相互隔离的 Hermes Agent 实例。

## Prerequisites

- Linux system (Ubuntu/Debian recommended)
- Python 3.8+
- Git
- Hermes Agent core codebase
- Approximately 1GB available RAM (supports multiple instances)

## Step 1: Obtain Hermes Utils Toolkit

```bash
# Clone the hermes-utils repository
git clone https://github.com/feision/hermes-utils.git
cd hermes-utils

# Ensure you're on the main branch
git checkout main
git pull origin main
```

## Step 2: Prepare Environment

### Prepare directories for user instances

```bash
# Create instances root directory (user)
mkdir -p ~/.hermes/instances
chmod +x start_instance.sh
```

### Prepare directories for root instances (optional)

```bash
# Create system-level instances directory (root)
sudo mkdir -p /root/.hermes/instances
sudo chmod +x start_root_instance.sh
```

## Step 3: Launch First Instance

### User Instance

```bash
# Start an instance named "bot1"
./start_instance.sh bot1

# Verify instance is running
./monitor_instances.sh
```

### Root Instance

```bash
# Start a system instance named "bot2"
sudo ./start_root_instance.sh bot2

# Verify instance is running
sudo ./monitor_instances.sh
```

## Step 4: Managing Instances

### View all instance status

```bash
# User view
./monitor_instances.sh

# Root view (if system instances exist)
sudo ./monitor_instances.sh
```

### Stop instances

```bash
# Stop user instance
./stop_instance.sh bot1

# Stop root instance
sudo ./stop_instance.sh bot2
```

### Restart instances

```bash
# Restart instance (stop then start)
./stop_instance.sh bot1
./start_instance.sh bot1
```

## Step 5: Configure Instances

Each instance has independent configuration files located at:

- User instances: `~/.hermes/instances/<instance_name>/config.yaml`
- Root instances: `/root/.hermes/instances/<instance_name>/config.yaml`

### Common Configuration Items

```yaml
# config.yaml example
core:
  summary_model: "openrouter/elephant-alpha"  # Adjust based on available model
  max_tokens: 2048
  temperature: 0.7

logging:
  level: "INFO"
  file: "logs/agent.log"

memory:
  max_tokens: 4000
  strategy: "sliding_window"
```

**Important**: If you encounter `model_not_found` errors (HTTP 503), modify `summary_model` to use a currently available model in your API gateway.

## Step 6: Multi-Instance Best Practices

### Resource Isolation

Each instance achieves complete isolation through the `HERMES_HOME` environment variable:
- Memory (`memory/`): Independent context and conversation history
- Skills (`skills/`): Independent skill collection
- Configuration (`config.yaml`): Instance-specific configuration
- Logs (`logs/`): Independent log files

### Port Management (if using Web Interface)

If instances are configured with web interfaces (like Flask monitoring panels), assign different ports to each instance:
- bot1: Port 8889
- bot2: Port 8890
- And so on...

Modify the port configuration in the corresponding instance's `config.yaml`.

## Step 7: Git Synchronization & Version Management

### Follow Git Commit Standards

All automated commits must include the AI model as author:

```bash
# Correct commit format (handled automatically by system)
git commit --author="openrouter/elephant-alpha <ai-assistant@hermes.agent>" -m "Descriptive commit message"
```

### Synchronization Workflow

```bash
# 1. Pull latest code first
git pull origin main

# 2. Make changes (edit scripts, update documentation, etc.)
# ...

# 3. Push to remote repository
git push origin main
```

### Documentation Standards

All README files must include:
- Version history table (preserving all historical records)
- Diagnostic tables/workflow explanations
- AI attribution policy explanation

## Step 8: Troubleshooting

### Common Issues and Solutions

**Issue**: Instance fails to start, logs show `ModuleNotFoundError`
**Solution**: Check if virtual environment is correctly activated, ensure you're running from the `hermes-env` directory

**Issue**: `monitor_instances.sh` shows `STOPPED` but process is still running
**Solution**: Check if instance name contains special characters causing `ps` matching to fail

**Issue**: Encountering `model_not_found` 503 error
**Solution**: Modify `summary_model` in `config.yaml` to use currently available primary model

**Issue**: Git push fails, requesting authentication
**Solution**: Ensure you're using proper credential management, dynamically resolving token from git remote URL

## Step 9: Advanced Usage

### Creating Custom Instance Templates

```bash
# Copy default template to create custom template
cp -r ~/.hermes/templates/default_instance ~/.hermes/templates/my_custom_instance

# Edit template configuration
nano ~/.hermes/templates/my_custom_instance/config.yaml

# Launch instance using custom template
HERMES_TEMPLATE=my_custom_instance ./start_instance.sh mybot
```

### Batch Operation Script Example

```bash
#!/bin/bash
# Start multiple instances
for i in {1..5}; do
  ./start_instance.sh bot$i
  echo "Started bot$i"
done

# Monitor all instances
./monitor_instances.sh
```

## Conclusion

By following this tutorial, you can safely and efficiently run multiple isolated Hermes Agent instances on a single machine. Each instance has independent memory, skills, and configuration, ensuring context isolation in multi-bot environments.

For further assistance, refer to:
- `hermes-instance-management` skill: Detailed lifecycle management
- `multi-instance-deployment` skill: Multi-instance deployment strategy
- `hermes-github-sync` skill: Git synchronization standards

Happy hacking!
