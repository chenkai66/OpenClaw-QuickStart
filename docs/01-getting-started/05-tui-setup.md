# Chapter 1.5: OpenClaw TUI Setup Guide

## TUI Overview

OpenClaw TUI provides interactive chat in your terminal with full agent capabilities.

```
🦞 OpenClaw 2026.2.26
session agent:main:main

> your message here
```

## Launch

```bash
# Set API key first (DashScope example)
export OPENAI_API_KEY="your-dashscope-api-key"
export OPENAI_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"

# Launch TUI
openclaw tui
```

Or add env vars to `~/.zshrc` permanently, then just `openclaw tui`.

## TUI Commands

| Command | Description |
|---------|-------------|
| `/model opus` | Switch to Claude Opus 4.6 |
| `/model haiku` | Switch to Claude Haiku 4.5 |
| `/help` | Show help |
| `Ctrl+C` or `/exit` | Exit |

## Verify Models

```bash
openclaw models list

# Should show configured models with "yes" for auth
```

## Session Storage

All conversations auto-save to `~/.openclaw/agents/main/sessions/`

---

*Next: [Architecture →](../02-core-concepts/01-architecture.md)*
