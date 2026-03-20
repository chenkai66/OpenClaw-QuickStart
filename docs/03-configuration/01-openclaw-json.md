# Chapter 3.1: openclaw.json Configuration

## Config Location

```
~/.openclaw/openclaw.json
```

## Using Alibaba Cloud DashScope (Recommended)

DashScope provides Qwen models and third-party models (DeepSeek, Kimi, GLM, MiniMax) via an OpenAI-compatible API.

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
            "id": "qwen3-max",
            "name": "Qwen3 Max",
            "description": "Strongest, best for complex tasks (262K context)"
          },
          {
            "id": "qwen3.5-plus",
            "name": "Qwen3.5 Plus",
            "description": "Balanced quality, speed, cost (1M context)"
          },
          {
            "id": "qwen3.5-flash",
            "name": "Qwen3.5 Flash",
            "description": "Fastest, cheapest, simple tasks (1M context)"
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "dashscope/qwen3.5-plus"
      }
    }
  }
}
```

### Get API Key

1. Visit https://dashscope.console.aliyun.com/
2. Sign up / log in
3. Create an API Key and copy it

## DashScope Model Catalog

### Flagship Models

| Model ID | Best For | Context | Input Price | Output Price |
|----------|----------|---------|------------|-------------|
| `qwen3-max` | Complex reasoning | 262K | 2.5 CNY/M | 10 CNY/M |
| `qwen3.5-plus` | Balanced (recommended) | 1M | 0.8 CNY/M | 4.8 CNY/M |
| `qwen3.5-flash` | Speed & cost | 1M | 0.2 CNY/M | 2 CNY/M |

### Third-Party Models on DashScope

| Provider | Models |
|----------|--------|
| DeepSeek | deepseek-chat, deepseek-reasoner |
| Kimi | kimi-k2.5 |
| GLM | glm-5 |
| MiniMax | MiniMax-M2.5 |

### Specialized Models

| Model | Use Case |
|-------|----------|
| `qwen3.5-coder` | Code generation and understanding |
| `qwen3.5-plus` (multimodal) | Image + video understanding |
| `qwen-audio` | Speech understanding |

## Using Anthropic API

If you have an Anthropic API key:

```json
{
  "models": {
    "providers": {
      "anthropic": {
        "apiKey": "YOUR_ANTHROPIC_API_KEY",
        "api": "anthropic-messages",
        "models": [
          { "id": "claude-sonnet-4-5-20260514", "name": "Claude Sonnet 4.5" },
          { "id": "claude-opus-4-6-20260620", "name": "Claude Opus 4.6" }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": { "primary": "anthropic/claude-sonnet-4-5-20260514" }
    }
  }
}
```

## Switch Models

### In openclaw.json

```json
{ "agents": { "defaults": { "model": { "primary": "dashscope/qwen3-max" } } } }
```

### At Runtime (TUI)

```bash
/model qwen3-max
/model qwen3.5-flash
```

## Environment Variables

```bash
# DashScope (recommended)
export OPENAI_API_KEY="your-dashscope-api-key"
export OPENAI_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"

# OR Anthropic
export ANTHROPIC_API_KEY="your-anthropic-api-key"
```

## Verify

```bash
openclaw models list
```

## Security

```bash
chmod 600 ~/.openclaw/openclaw.json
```

---

*Next: [Model Providers →](02-model-providers.md)*
