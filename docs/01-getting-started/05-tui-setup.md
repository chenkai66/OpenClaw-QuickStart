# Chapter 1.5: OpenClaw TUI Setup Guide

> How to configure and launch OpenClaw's Terminal User Interface

## TUI Overview

OpenClaw TUI (Terminal User Interface) provides an interactive chat experience directly in your terminal, similar to using ChatGPT but with full agent capabilities.

```
🦞 OpenClaw 2026.2.26
session agent:main:main

> your message here
```

## Launch Methods

### Method 1: Startup Script (Recommended)

```bash
cd ~/openclaw-second-brain
./scripts/openclaw-tui-with-keys.sh
```

This automatically:
- Sets all API Keys
- Checks dashscope-proxy status
- Launches OpenClaw TUI

### Method 2: Manual Launch

```bash
export ANTHROPIC_API_KEY="your-idealab-api-key"
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080/idealab"
openclaw tui
```

### Method 3: Permanent Config (Daily Use)

Add to `~/.zshrc` or `~/.bashrc`:

```bash
export ANTHROPIC_API_KEY="your-idealab-api-key"
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080/idealab"
```

Then just run:
```bash
openclaw tui
```

## TUI Commands

| Command | Description |
|---------|-------------|
| `/model opus` | Switch to Claude Opus 4.6 |
| `/model haiku` | Switch to Claude Haiku 4.5 |
| `/help` | Show help |
| `Ctrl+C` or `/exit` | Exit TUI |

## Current Configuration

| Setting | Value |
|---------|-------|
| Default Model | Claude Sonnet 4.5 |
| API Source | idealab (via dashscope-proxy) |
| Backup Models | Claude Opus 4.6, Haiku 4.5 |
| Session Storage | `~/.openclaw/agents/main/sessions/` |

## Verify Configuration

```bash
export ANTHROPIC_API_KEY="your-api-key"
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080/idealab"
openclaw models list | grep anthropic

# Expected output:
# anthropic/claude-sonnet-4-5  ... yes  default
# anthropic/claude-opus-4-6    ... yes  configured
# anthropic/claude-haiku-4-5   ... yes  configured
```

## Quick Verification

```bash
# 1. Start TUI
./scripts/openclaw-tui-with-keys.sh

# 2. Type test message
> Hello, which model are you using?

# 3. Should see Claude Sonnet 4.5 response
```

## Conversation Records

All conversations automatically save to:
```
~/.openclaw/agents/main/sessions/
```

If Knowledge Agent is configured, conversations sync to Second Brain every hour.

---

*Next: [Architecture →](../02-core-concepts/01-architecture.md)*
