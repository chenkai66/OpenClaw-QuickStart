# Chapter 3.1: openclaw.json Configuration (Real Examples)

> Actual production configuration from a working OpenClaw + Claude Code deployment

## Configuration File Location

```
~/.openclaw/openclaw.json
```

## Complete Production Configuration

The following is a real, working `openclaw.json` from a production deployment using Alibaba Cloud Idealab API through dashscope-proxy:

```json
{
  "models": {
    "providers": {
      "anthropic": {
        "baseUrl": "http://127.0.0.1:8080/idealab",
        "apiKey": "YOUR_IDEALAB_API_KEY",
        "api": "anthropic-messages",
        "models": [
          {
            "id": "claude_sonnet4_5",
            "name": "Claude Sonnet 4.5",
            "description": "Balanced performance and speed"
          },
          {
            "id": "claude-opus-4-6",
            "name": "Claude Opus 4.6",
            "description": "Most capable model"
          },
          {
            "id": "claude-haiku-4-5",
            "name": "Claude Haiku 4.5",
            "description": "Fastest model for simple tasks"
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude_sonnet4_5"
      }
    }
  }
}
```

## Claude Code Configuration (~/.claude/settings.json)

When using OpenClaw alongside Claude Code, you also need:

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "YOUR_IDEALAB_API_KEY",
    "ANTHROPIC_BASE_URL": "http://127.0.0.1:8080/idealab",
    "ANTHROPIC_MODEL": "claude_sonnet4_5",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "claude-opus-4-6",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "claude_sonnet4_5",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "claude-haiku-4-5"
  },
  "model": "sonnet"
}
```

## Available Models

| Model ID | Name | Use Case |
|----------|------|----------|
| `claude_sonnet4_5` | Claude Sonnet 4.5 | Default. Balanced performance and speed |
| `claude-opus-4-6` | Claude Opus 4.6 | Most powerful. Complex reasoning tasks |
| `claude-haiku-4-5` | Claude Haiku 4.5 | Fastest. Simple tasks and quick responses |

## How to Switch Models

### In openclaw.json

Change the `primary` model:

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-opus-4-6"
      }
    }
  }
}
```

### In Claude Code settings.json

```json
{
  "model": "opus"
}
```

### At Runtime (OpenClaw TUI)

```bash
# Switch to Opus during a conversation
/model opus

# Switch to Haiku
/model haiku
```

## Verify Model Configuration

```bash
# Set environment variables first
export ANTHROPIC_API_KEY="your-api-key"
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080/idealab"

# List all configured models
openclaw models list | grep anthropic

# Expected output:
# anthropic/claude-sonnet-4-5  ... yes  default
# anthropic/claude-opus-4-6    ... yes  configured
# anthropic/claude-haiku-4-5   ... yes  configured
```

## Using Alternative Providers (Alibaba Qwen)

If you prefer using Alibaba Qwen models directly via DashScope API:

```json
{
  "models": {
    "providers": {
      "dashscope": {
        "baseUrl": "https://dashscope.aliyuncs.com/compatible-mode/v1",
        "apiKey": "YOUR_DASHSCOPE_API_KEY",
        "api": "openai-chat",
        "models": [
          {
            "id": "qwen-max",
            "name": "Qwen Max",
            "description": "Most capable Qwen model"
          },
          {
            "id": "qwen3.5-flash",
            "name": "Qwen 3.5 Flash",
            "description": "Fast and efficient"
          },
          {
            "id": "qwen3.5-122b",
            "name": "Qwen 3.5 122B",
            "description": "Large model for complex tasks"
          }
        ]
      }
    }
  }
}
```

### Getting a DashScope API Key

1. Visit https://dashscope.console.aliyun.com/
2. Sign up / log in
3. Create an API Key in the console
4. Copy the key and use it in the configuration

## Environment Variables

These environment variables can also be set in `~/.bashrc` or `~/.zshrc`:

```bash
# For OpenClaw + Claude Code via Dashscope Proxy
export ANTHROPIC_API_KEY="your-idealab-api-key"
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080/idealab"

# For Qwen models via DashScope directly
export OPENAI_API_KEY="your-dashscope-api-key"
export OPENAI_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"

# OpenClaw session storage path
export OPENCLAW_SESSIONS_PATH="$HOME/.openclaw/agents/main/sessions"
```

## How to Change API Key

```bash
# 1. Edit Claude Code configuration
vim ~/.claude/settings.json
# Update ANTHROPIC_AUTH_TOKEN

# 2. Edit OpenClaw configuration
vim ~/.openclaw/openclaw.json
# Update apiKey

# 3. No service restart needed - config changes take effect automatically
```

## Security Best Practices

```bash
# Protect configuration files containing API keys
chmod 600 ~/.claude/settings.json
chmod 600 ~/.openclaw/openclaw.json

# Back up before making changes
cp ~/.claude/settings.json ~/.claude/settings.json.bak
cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.bak
```

---

*Next: [Model Providers →](02-model-providers.md)*
