# Chapter 10: Alibaba Cloud Coding Plan for OpenClaw

> Use DashScope Coding Plan to power OpenClaw with 8 top models at a fixed monthly price.

---

## What is Coding Plan?

**Coding Plan** is a subscription from Alibaba Cloud's Model Studio (百炼) that bundles 8 top AI models into a fixed monthly fee. It's specifically designed for AI coding tools like OpenClaw, Claude Code, Qwen Code, Cursor, etc.

### Why Use Coding Plan?

| Feature | Regular DashScope API | Coding Plan |
|---------|----------------------|-------------|
| Pricing | Pay-per-token | **Fixed 200 CNY/month** |
| Models | One at a time | **8 models included** |
| Cost risk | Can accumulate | **Fixed fee, no surprises** |
| API Key | `sk-xxxxx` | `sk-sp-xxxxx` |
| Base URL | `dashscope.aliyuncs.com` | `coding.dashscope.aliyuncs.com` |

---

## Included Models (Pro Plan)

| Model | Strengths | Context | Multimodal |
|-------|-----------|---------|------------|
| **qwen3.5-plus** | Balanced, recommended default | 1M tokens | Text + Image |
| **kimi-k2.5** | Strong reasoning | 262K tokens | Text + Image |
| **glm-5** | General tasks | 202K tokens | Text |
| **MiniMax-M2.5** | Fast responses | 196K tokens | Text |
| **qwen3-max-2026-01-23** | Complex reasoning | 262K tokens | Text |
| **qwen3-coder-next** | Code generation | 262K tokens | Text |
| **qwen3-coder-plus** | Code + long context | 1M tokens | Text |
| **glm-4.7** | General tasks | 202K tokens | Text |

---

## Step 1: Subscribe

1. Visit [Coding Plan page](https://dashscope.console.aliyun.com/) on Alibaba Cloud
2. Choose **Pro Plan** (200 CNY/month)
3. Complete payment

### Usage Limits (Pro Plan)

| Period | Request Limit |
|--------|--------------|
| Per 5 hours | 6,000 requests |
| Per week | 45,000 requests |
| Per month | 90,000 requests |

> A simple task uses ~5-10 requests, complex tasks use ~10-30+ requests.

---

## Step 2: Get Coding Plan API Key

On the Coding Plan page, copy your **Coding Plan-specific API Key**.

- Format: `sk-sp-xxxxx` (note the `sp` prefix!)
- Base URL: `https://coding.dashscope.aliyuncs.com/v1`

> **CRITICAL**: Do NOT mix Coding Plan credentials with regular DashScope credentials!
>
> | Type | API Key Format | Base URL |
> |------|---------------|----------|
> | Regular DashScope | `sk-xxxxx` | `https://dashscope.aliyuncs.com/compatible-mode/v1` |
> | Coding Plan | `sk-sp-xxxxx` | `https://coding.dashscope.aliyuncs.com/v1` |

---

## Step 3: Configure OpenClaw

Edit `~/.openclaw/openclaw.json`:

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "bailian": {
        "baseUrl": "https://coding.dashscope.aliyuncs.com/v1",
        "apiKey": "sk-sp-YOUR_CODING_PLAN_KEY",
        "api": "openai-completions",
        "models": [
          {
            "id": "qwen3.5-plus",
            "name": "qwen3.5-plus",
            "reasoning": false,
            "input": ["text", "image"],
            "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 },
            "contextWindow": 1000000,
            "maxTokens": 65536,
            "compat": { "thinkingFormat": "qwen" }
          },
          {
            "id": "qwen3-max-2026-01-23",
            "name": "qwen3-max-2026-01-23",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 },
            "contextWindow": 262144,
            "maxTokens": 65536,
            "compat": { "thinkingFormat": "qwen" }
          },
          {
            "id": "qwen3-coder-next",
            "name": "qwen3-coder-next",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 },
            "contextWindow": 262144,
            "maxTokens": 65536
          },
          {
            "id": "qwen3-coder-plus",
            "name": "qwen3-coder-plus",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 },
            "contextWindow": 1000000,
            "maxTokens": 65536
          },
          {
            "id": "MiniMax-M2.5",
            "name": "MiniMax-M2.5",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 },
            "contextWindow": 196608,
            "maxTokens": 32768
          },
          {
            "id": "glm-5",
            "name": "glm-5",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 },
            "contextWindow": 202752,
            "maxTokens": 16384,
            "compat": { "thinkingFormat": "qwen" }
          },
          {
            "id": "glm-4.7",
            "name": "glm-4.7",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 },
            "contextWindow": 202752,
            "maxTokens": 16384,
            "compat": { "thinkingFormat": "qwen" }
          },
          {
            "id": "kimi-k2.5",
            "name": "kimi-k2.5",
            "reasoning": false,
            "input": ["text", "image"],
            "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 },
            "contextWindow": 262144,
            "maxTokens": 32768,
            "compat": { "thinkingFormat": "qwen" }
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": { "primary": "bailian/qwen3.5-plus" },
      "models": {
        "bailian/qwen3.5-plus": {},
        "bailian/qwen3-max-2026-01-23": {},
        "bailian/qwen3-coder-next": {},
        "bailian/qwen3-coder-plus": {},
        "bailian/MiniMax-M2.5": {},
        "bailian/glm-5": {},
        "bailian/glm-4.7": {},
        "bailian/kimi-k2.5": {}
      }
    }
  },
  "gateway": { "mode": "local" }
}
```

> This config is from the [official Alibaba Cloud documentation](https://help.aliyun.com/zh/model-studio/openclaw-coding-plan).

---

## Step 4: Apply Config

```bash
openclaw gateway restart
```

Verify:
```bash
openclaw status
```

## Step 5: Switch Models in TUI

```bash
openclaw tui
```

Then use `/model` to switch:
```
/model qwen3.5-plus       # Default, balanced
/model kimi-k2.5           # Strong reasoning
/model qwen3-coder-next    # Best for code
/model glm-5               # Alternative
```

---

## Model Selection Strategy

| Task Type | Recommended Model | Why |
|-----------|------------------|-----|
| General chat | qwen3.5-plus | Balanced quality/speed/cost |
| Complex reasoning | qwen3-max-2026-01-23 | Strongest Qwen model |
| Code generation | qwen3-coder-next | Optimized for coding |
| Long document processing | qwen3-coder-plus | 1M context window |
| Image understanding | qwen3.5-plus or kimi-k2.5 | Multimodal support |
| Fast simple tasks | MiniMax-M2.5 | Quick responses |

---

## Important Rules

1. **Interactive use only**: Coding Plan is for coding tools (OpenClaw, Claude Code, etc.), NOT for API automation scripts or batch processing
2. **No account sharing**: The subscription is personal
3. **Data usage**: Input/output data may be used for service improvement (per Alibaba Cloud ToS 5.2)

---

## Troubleshooting

### "Still being charged pay-per-use after buying Coding Plan"

You're using the wrong API Key. Ensure:
- Key starts with `sk-sp-` (NOT `sk-`)
- Base URL is `coding.dashscope.aliyuncs.com` (NOT `dashscope.aliyuncs.com`)

### "Model not found"

Check that you've registered all 8 models in the `models` array. Use the exact model IDs from the config above.

### "Rate limit exceeded"

Pro Plan limit: 6,000 requests per 5 hours. For heavy usage, pace your work or upgrade if available.

---

*Next: [Advanced Tips →](../06-advanced/03-advanced-tips.md)*
