# 第1章·第2节：安装指南

> 从零开始，5 分钟装好 OpenClaw。

---

## 系统要求

| 项目 | 最低要求 | 推荐配置 |
|------|---------|---------|
| 操作系统 | macOS 11+ / Ubuntu 22.04+ / Windows (WSL2) | Ubuntu 22.04+ |
| Node.js | **v22.16+** | v24.x（官方推荐） |
| 内存 | 2 GB | 4-8 GB |
| CPU | 2 核 | 2-4 核 |
| 存储 | 10 GB SSD | 30 GB+ SSD |

## 第一步：安装 Node.js

> **注意**：OpenClaw 要求 Node.js **v22.16 或更高版本**，低版本跑不起来。官方推荐用 v24。

先看看你现在装的版本：
```bash
node -v   # 需要 v22.16+
```

### Ubuntu / Debian
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs
node --version  # 确认 v22.x.x
```

### CentOS / RHEL / Alibaba Cloud Linux
```bash
curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -
sudo yum install -y nodejs
```

### macOS
```bash
brew install node@22
```

### 用 nvm 管理（通用方案）
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
source ~/.bashrc
nvm install 22
nvm use 22
```

## 第二步：安装 OpenClaw

### 方式 A：官方安装脚本（推荐）
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### 方式 B：通过 npm 安装
```bash
npm install -g openclaw@latest
openclaw --version  # 2026.3.x
```

## 第三步：初始化引导

运行交互式引导向导：
```bash
openclaw onboard --install-daemon
```

向导会问你几个问题，建议这样选：

| 提示 | 建议选择 |
|------|---------|
| I understand this is powerful and risky | **Yes** |
| Onboarding mode | **QuickStart** |
| Model/auth provider | **Skip for now**（后面再配） |
| Select channel | **Skip for now** |
| Configure skills? | **No**（后面再配） |
| Enable hooks? | **Skip for now** |
| How to hatch your bot? | **Hatch in TUI** |

## 第四步：配置 API Key

### 方案 A：通义千问 DashScope API（国内推荐）

1. 打开 https://dashscope.console.aliyun.com/
2. 注册登录，创建一个 API Key

把下面两行加到 `~/.bashrc` 或 `~/.zshrc`：
```bash
export OPENAI_API_KEY="你的DashScope-API-Key"
export OPENAI_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"
```

生效：`source ~/.zshrc`

### 方案 B：百炼 Coding Plan（性价比最高）

如果你开通了[百炼 Coding Plan](https://help.aliyun.com/zh/model-studio/coding-plan)（200 元/月，包含 8 个模型）：

```bash
export OPENAI_API_KEY="sk-sp-xxxxx"   # Coding Plan 的 Key 以 sk-sp- 开头
export OPENAI_BASE_URL="https://coding.dashscope.aliyuncs.com/v1"
```

> **别搞混了**：Coding Plan 的 Key（`sk-sp-` 开头）和普通 DashScope Key（`sk-` 开头）不能混用，Base URL 也不一样。

### 方案 C：Anthropic API
```bash
export ANTHROPIC_API_KEY="你的Anthropic-API-Key"
```

## 第五步：试跑一下

```bash
openclaw tui
```

随便输入点什么测试一下。然后看看会话文件有没有生成：
```bash
ls -la ~/.openclaw/agents/main/sessions/
```

## 第六步：启动 Gateway（可选）

Gateway 是多渠道接入（钉钉、Telegram、Discord 等）的前提：
```bash
openclaw gateway install
openclaw gateway
# 默认监听 ws://127.0.0.1:18789
```

生产环境建议设成开机自启：
```bash
cat > /etc/systemd/system/openclaw-gateway.service << 'SVCEOF'
[Unit]
Description=OpenClaw Gateway
After=network.target
[Service]
Type=simple
ExecStart=/usr/bin/openclaw gateway
Restart=always
RestartSec=10
[Install]
WantedBy=multi-user.target
SVCEOF
systemctl daemon-reload && systemctl enable --now openclaw-gateway
```

## 检查清单

- [ ] Node.js >= 22.16
- [ ] OpenClaw 已安装（`openclaw --version`）
- [ ] 引导向导跑完了
- [ ] API Key 配好了（DashScope / Coding Plan / Anthropic）
- [ ] `openclaw tui` 能正常对话
- [ ] 会话文件已生成

---

*下一节：[第一次对话 →](03-first-chat.md)*


---

## Docker 安装（适合服务器/NAS）

如果你用 Docker 或群晖/绿联 NAS，有社区维护的一键镜像：

```bash
# 拉取社区 Docker 镜像（预装钉钉/飞书/QQ/企微等插件）
docker pull openclaw/openclaw-full:latest

# 启动容器
docker run -d \
  --name openclaw \
  -p 18789:18789 \
  -v ~/.openclaw:/root/.openclaw \
  -e DASHSCOPE_API_KEY=sk-xxx \
  --restart unless-stopped \
  openclaw/openclaw-full:latest
```

| 平台 | 说明 |
|------|------|
| **群晖 NAS** | Docker 套件中搜索 `openclaw`，直接安装 |
| **绿联 NAS** | 固件 1.11+ 支持，Docker 中搜索安装 |
| **阿里云** | [一键部署专题页](https://www.aliyun.com/activity/ecs/clawdbot) |

> 💡 Docker 方案的好处：7x24 小时运行、数据持久化、随时迁移。

