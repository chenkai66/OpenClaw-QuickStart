# Chapter 1.2: Installation Guide

## System Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| OS | Ubuntu 20.04+ / Debian 10+ / CentOS 8+ / macOS | Ubuntu 22.04 LTS |
| Node.js | v18.0.0+ | v22.x (LTS) |
| RAM | 1 GB | 2 GB+ |
| Disk | 2 GB free | 5 GB+ |

## Step 1: Install Node.js

OpenClaw requires Node.js 18+ (recommended 22.x).

### Ubuntu / Debian

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs
node --version  # v22.x.x
npm --version   # 10.x.x
```

### CentOS / RHEL / Alibaba Cloud Linux

```bash
curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -
sudo yum install -y nodejs
```

### Using nvm (Universal)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 22
```

### macOS

```bash
brew install node@22
```

## Step 2: Install OpenClaw

```bash
npm install -g openclaw
openclaw --version  # 2026.3.1+
```

**Notes:**
- Package is large (600+ dependencies), takes 2-3 minutes
- If version shows empty: `npm uninstall -g openclaw && npm install -g openclaw@latest`

```bash
mkdir -p ~/.openclaw
```

## Step 3: Configure API Key

OpenClaw works with any OpenAI-compatible API. Simplest: use **Alibaba Cloud DashScope**:

### Get DashScope API Key

1. Visit https://dashscope.console.aliyun.com/
2. Sign up / log in
3. Create an API Key and copy it

### Set Environment Variables

Add to `~/.bashrc` or `~/.zshrc`:

```bash
export OPENAI_API_KEY="your-dashscope-api-key"
export OPENAI_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"
```

Reload: `source ~/.zshrc`

### Or Use Anthropic API Directly

```bash
export ANTHROPIC_API_KEY="your-anthropic-api-key"
# No base URL override needed
```

## Step 4: Initialize OpenClaw

```bash
openclaw init
openclaw chat "Hello, this is a test"
ls -la ~/.openclaw/agents/main/sessions/
```

## Step 5: Set Up Gateway (Optional)

Gateway enables multi-channel routing (Telegram, DingTalk, etc.):

```bash
openclaw config set gateway.mode local
openclaw gateway
# Listens on ws://127.0.0.1:18789
```

For production, create systemd service:

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

## Deployment Checklist

- [ ] Node.js >= 22 installed
- [ ] OpenClaw installed
- [ ] API Key configured (DashScope or Anthropic)
- [ ] First conversation works
- [ ] Session files exist

---

*Next: [First Chat →](03-first-chat.md)*
