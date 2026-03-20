# Chapter 1.2: Installation Guide

## System Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| OS | macOS 11+ / Ubuntu 22.04+ / Windows (WSL2) | Ubuntu 22.04+ |
| Node.js | **v22.0+** | v22.x (LTS) |
| RAM | 2 GB | 4-8 GB |
| CPU | 2 cores | 2-4 cores |
| Disk | 10 GB SSD | 30 GB+ SSD |

## Step 1: Install Node.js (v22+)

> **Important**: OpenClaw requires Node.js **v22.0 or higher**. Earlier versions will not work.

Check existing version:
```bash
node -v   # Need v22.0+
```

### Ubuntu / Debian
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs
node --version  # v22.x.x
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

### Using nvm (Universal)
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
source ~/.bashrc
nvm install 22
nvm use 22
```

## Step 2: Install OpenClaw

### Method A: Official Install Script (Recommended)
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### Method B: npm
```bash
npm install -g openclaw@latest
openclaw --version  # 2026.3.x
```

## Step 3: Onboarding

Run the interactive onboarding wizard:
```bash
openclaw onboard --install-daemon
```

The wizard will ask:
| Prompt | Recommended Choice |
|--------|-------------------|
| I understand this is powerful and risky | **Yes** |
| Onboarding mode | **QuickStart** |
| Model/auth provider | **Skip for now** (configure later) |
| Select channel | **Skip for now** |
| Configure skills? | **No** (configure later) |
| Enable hooks? | **Skip for now** |
| How to hatch your bot? | **Hatch in TUI** |

## Step 4: Configure API Key

### Option A: DashScope API (Recommended for China)

1. Visit https://dashscope.console.aliyun.com/
2. Sign up / log in, create an API Key

Add to `~/.bashrc` or `~/.zshrc`:
```bash
export OPENAI_API_KEY="your-dashscope-api-key"
export OPENAI_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"
```

Reload: `source ~/.zshrc`

### Option B: DashScope Coding Plan (Best Value)

If you subscribe to [Coding Plan](https://help.aliyun.com/zh/model-studio/coding-plan) (200 CNY/month, 8 models included):

```bash
export OPENAI_API_KEY="sk-sp-xxxxx"   # Coding Plan key starts with sk-sp-
export OPENAI_BASE_URL="https://coding.dashscope.aliyuncs.com/v1"
```

> **Warning**: Coding Plan API Key (`sk-sp-`) and regular DashScope key (`sk-`) are NOT interchangeable. Don't mix them!

### Option C: Anthropic API
```bash
export ANTHROPIC_API_KEY="your-anthropic-api-key"
```

## Step 5: First Chat

```bash
openclaw tui
```

Type a message to test. Then verify session files:
```bash
ls -la ~/.openclaw/agents/main/sessions/
```

## Step 6: Gateway (Optional)

Gateway enables multi-channel routing (DingTalk, Telegram, Discord):
```bash
openclaw gateway install
openclaw gateway
# Listens on ws://127.0.0.1:18789
```

For production (auto-start on boot):
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

## Quick Checklist

- [ ] Node.js >= 22
- [ ] OpenClaw installed (`openclaw --version`)
- [ ] Onboarding complete
- [ ] API Key configured (DashScope / Coding Plan / Anthropic)
- [ ] `openclaw tui` works
- [ ] Session files saved

---

*Next: [First Chat →](03-first-chat.md)*
