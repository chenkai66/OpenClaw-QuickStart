# 详细安装与配置指南

本文档提供 OpenClaw 的完整安装和配置说明。

---

## 📋 系统要求

### 最低配置

| 项目 | 要求 |
|------|------|
| **操作系统** | macOS 12+, Ubuntu 20.04+, Windows 10+ (WSL2) |
| **Node.js** | v22.0.0+ |
| **内存** | 1GB+ RAM |
| **存储** | 500MB+ 可用空间 |
| **网络** | 能访问 LLM API |

### 推荐配置

| 项目 | 推荐 |
|------|------|
| **CPU** | 2 核+ |
| **内存** | 4GB+ RAM |
| **存储** | 2GB+ SSD |

---

## 🖥️ 各平台安装

### macOS 安装

```bash
# 1. 安装 Homebrew（如果没有）
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 2. 安装 Node.js
brew install node@22

# 3. 安装 OpenClaw
npm install -g openclaw

# 4. 验证安装
openclaw --version
```

### Ubuntu/Debian 安装

```bash
# 1. 更新包管理器
sudo apt-get update

# 2. 安装 Node.js 22
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# 3. 安装 OpenClaw
sudo npm install -g openclaw

# 4. 验证安装
openclaw --version
```

### CentOS/RHEL 安装

```bash
# 1. 安装 Node.js 22
curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -
sudo dnf install -y nodejs

# 2. 安装 OpenClaw
sudo npm install -g openclaw

# 3. 验证安装
openclaw --version
```

### Windows (WSL2) 安装

```bash
# 1. 启用 WSL2 并安装 Ubuntu
wsl --install

# 2. 在 WSL 中按 Ubuntu 步骤安装
```

---

## 🔧 配置 API Key

OpenClaw 支持多个 LLM 提供商。

### Anthropic Claude（推荐）

```bash
# 1. 获取 API Key
# 访问 https://console.anthropic.com/

# 2. 配置
openclaw configure

# 或手动创建配置
mkdir -p ~/.openclaw
cat > ~/.openclaw/openclaw.json << 'EOF'
{
  "models": {
    "providers": {
      "anthropic": {
        "baseUrl": "https://api.anthropic.com",
        "apiKey": "sk-ant-your-key-here",
        "api": "anthropic-messages",
        "models": [
          {
            "id": "claude-sonnet-4-5",
            "name": "Claude Sonnet 4.5"
          },
          {
            "id": "claude-opus-4-6",
            "name": "Claude Opus 4.6"
          },
          {
            "id": "claude-haiku-4-5",
            "name": "Claude Haiku 4.5"
          }
        ]
      }
    }
  }
}
EOF
```

### 阿里云百炼（国内用户）

```bash
cat > ~/.openclaw/openclaw.json << 'EOF'
{
  "models": {
    "providers": {
      "bailian": {
        "baseUrl": "https://dashscope.aliyuncs.com/compatible-mode/v1",
        "apiKey": "sk-your-bailian-key",
        "api": "openai",
        "models": [
          {
            "id": "qwen-max",
            "name": "Qwen Max"
          },
          {
            "id": "qwen3.5-flash",
            "name": "Qwen 3.5 Flash"
          }
        ]
      }
    }
  }
}
EOF
```

### 多模型配置（高级）

```json
{
  "models": {
    "providers": {
      "anthropic": {
        "baseUrl": "http://127.0.0.1:8080/idealab",
        "apiKey": "your-idealab-key",
        "api": "anthropic-messages",
        "models": [
          {"id": "claude-sonnet-4-5", "name": "Claude Sonnet 4.5"}
        ]
      },
      "bailian": {
        "baseUrl": "https://dashscope.aliyuncs.com/compatible-mode/v1",
        "apiKey": "your-bailian-key",
        "api": "openai",
        "models": [
          {"id": "qwen-max", "name": "Qwen Max"}
        ]
      }
    }
  }
}
```

---

## 🚀 启动 OpenClaw

### Gateway 模式（推荐）

```bash
# 启动 Gateway（后台运行）
openclaw gateway

# Gateway 信息
# - WebSocket 服务器: ws://localhost:18789
# - Dashboard: http://127.0.0.1:18789

# 检查状态
openclaw health
```

### TUI 模式

```bash
# 直接启动对话
openclaw tui
```

### Web UI 模式

```bash
# 启动 Gateway 后，访问
open http://127.0.0.1:18789
```

---

## ⚙️ 高级配置

### 环境变量

```bash
# 添加到 ~/.zshrc 或 ~/.bashrc

# API Keys
export ANTHROPIC_API_KEY="sk-ant-your-key"
export BAILIAN_API_KEY="sk-your-bailian-key"

# Base URLs
export ANTHROPIC_BASE_URL="https://api.anthropic.com"
export BAILIAN_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"

# OpenClaw 配置
export OPENCLAW_SESSIONS_PATH="$HOME/.openclaw/agents/main/sessions"
export OPENCLAW_WORKSPACE="$HOME/.openclaw/workspace"

# 加载配置
source ~/.zshrc
```

### Gateway 配置

编辑 `~/.openclaw/gateway.json`：

```json
{
  "port": 18789,
  "host": "0.0.0.0",
  "ssl": false,
  "auth": {
    "enabled": true,
    "tokenExpiry": 86400
  },
  "cors": {
    "enabled": true,
    "origins": ["http://localhost:3000"]
  }
}
```

---

## 🔐 安全配置

### API Key 管理

```bash
# ✅ 推荐：使用环境变量
export ANTHROPIC_API_KEY="sk-ant-..."

# ✅ 推荐：使用 .env 文件
cat > ~/.openclaw/.env << 'EOF'
ANTHROPIC_API_KEY=sk-ant-...
BAILIAN_API_KEY=sk-...
EOF
chmod 600 ~/.openclaw/.env

# ❌ 避免：硬编码在配置文件中
```

### 文件权限

```bash
# 配置文件权限
chmod 600 ~/.openclaw/openclaw.json
chmod 600 ~/.openclaw/gateway.json

# 目录权限
chmod 700 ~/.openclaw
```

---

## ✅ 验证安装

```bash
# 1. 检查版本
openclaw --version

# 2. 测试 API 连接
openclaw test

# 3. 列出可用模型
openclaw models list

# 4. 启动对话测试
openclaw tui

# 5. 检查 Gateway
openclaw health
```

---

## 🐛 故障排查

### 常见问题

**问题1: 安装失败**
```bash
# 清理 npm 缓存
npm cache clean --force

# 重新安装
npm install -g openclaw
```

**问题2: API 连接失败**
```bash
# 测试网络
curl https://api.anthropic.com

# 检查 API Key
openclaw models list
```

**问题3: Gateway 无法启动**
```bash
# 检查端口占用
lsof -i :18789

# 杀死占用进程
kill -9 <PID>

# 重新启动
openclaw gateway
```

---

## 🎯 下一步

安装完成！接下来：

1. [第一个对话](03-first-conversation.md) - 学习基本交互
2. [记忆系统](../advanced/memory-system.md) - 配置持久记忆
3. [定时任务](../advanced/cron-jobs.md) - 自动化工作流

---

**上一节**: [快速开始](01-quick-start.md)  
**下一节**: [第一个对话](03-first-conversation.md)
