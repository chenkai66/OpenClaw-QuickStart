# Chapter 1.5: OpenClaw TUI Setup Guide

## TUI Overview

OpenClaw TUI provides interactive chat in your terminal with full agent capabilities.

```
🦞 OpenClaw 2026.x.x
session agent:main:main

> your message here
```

## Launch

```bash
# Set API key (DashScope)
export OPENAI_API_KEY="your-dashscope-api-key"
export OPENAI_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"

# Launch TUI
openclaw tui
```

Or add env vars to `~/.zshrc` permanently, then just `openclaw tui`.

## TUI Commands

| Command | Description |
|---------|-------------|
| `/model <name>` | Switch model (e.g. `/model qwen3-max`) |
| `/help` | Show help |
| `Ctrl+C` or `/exit` | Exit |

## DashScope Available Models

| Model ID | Best For | Context | Price (per M tokens) |
|----------|----------|---------|---------------------|
| `qwen3-max` | Complex tasks, strongest | 262K | Input 2.5 / Output 10 |
| `qwen3.5-plus` | Balanced speed & quality | 1M | Input 0.8 / Output 4.8 |
| `qwen3.5-flash` | Simple tasks, fastest | 1M | Input 0.2 / Output 2 |

Third-party models also available on DashScope: DeepSeek, Kimi, GLM, MiniMax.

## Verify Models

```bash
openclaw models list
```

## Session Storage

All conversations auto-save to `~/.openclaw/agents/main/sessions/`

---

*Next: [Architecture →](../02-core-concepts/01-architecture.md)*
