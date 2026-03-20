# Chapter 3.1: openclaw.json Configuration

## Configuration File Location

```
~/.openclaw/openclaw.json
```

## Using Alibaba Cloud DashScope (Recommended for China)

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
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "dashscope/qwen-max"
      }
    }
  }
}
```

### Getting a DashScope API Key

1. Visit https://dashscope.console.aliyun.com/
2. Sign up / log in
3. Create an API Key in the console
4. Copy the key into the config

## Using Anthropic API Directly

```json
{
  "models": {
    "providers": {
      "anthropic": {
        "apiKey": "YOUR_ANTHROPIC_API_KEY",
        "api": "anthropic-messages",
        "models": [
          {
            "id": "claude-sonnet-4-5-20260514",
            "name": "Claude Sonnet 4.5",
            "description": "Balanced performance and speed"
          },
          {
            "id": "claude-opus-4-6-20260620",
            "name": "Claude Opus 4.6",
            "description": "Most capable model"
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-sonnet-4-5-20260514"
      }
    }
  }
}
```

## Available Models

### Alibaba Cloud Qwen (via DashScope)

| Model ID | Description |
|----------|-------------|
| `qwen-max` | Most capable, best for complex tasks |
| `qwen3.5-flash` | Fast, good for simple tasks |
| `qwen3.5-122b` | Large parameter model |

### Anthropic Claude

| Model ID | Description |
|----------|-------------|
| `claude-sonnet-4-5-*` | Default balanced model |
| `claude-opus-4-6-*` | Most powerful |
| `claude-haiku-4-5-*` | Fastest, simplest tasks |

## How to Switch Models

### In openclaw.json

Change the `primary` model:
```json
{ "agents": { "defaults": { "model": { "primary": "dashscope/qwen-max" } } } }
```

### At Runtime (TUI)

```bash
/model opus     # Switch to Claude Opus
/model haiku    # Switch to Claude Haiku
```

## Environment Variables

Add to `~/.bashrc` or `~/.zshrc`:

```bash
# For Qwen models via DashScope
export OPENAI_API_KEY="your-dashscope-api-key"
export OPENAI_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"

# OR for Anthropic Claude
export ANTHROPIC_API_KEY="your-anthropic-api-key"

# OpenClaw session path (usually default is fine)
export OPENCLAW_SESSIONS_PATH="$HOME/.openclaw/agents/main/sessions"
```

## Verify Model Configuration

```bash
openclaw models list
```

## Security Best Practices

```bash
chmod 600 ~/.openclaw/openclaw.json
cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.bak
```

---

*Next: [Model Providers →](02-model-providers.md)*
