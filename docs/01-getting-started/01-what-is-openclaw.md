# Chapter 1.1: What is OpenClaw?

> OpenClaw = Your personal AI agent that can send messages, manage emails, write code, run scripts, automate browsers...

---

## One-Liner

**OpenClaw is an open-source, self-hosted AI Agent gateway platform.**

It connects your chat tools (WhatsApp, Telegram, Discord, DingTalk, Slack, Signal, iMessage, Google Chat, Teams, Matrix) to AI agents, letting you command AI to work for you from anywhere.

---

## How is it Different from ChatGPT/Claude?

| Comparison | ChatGPT / Claude | OpenClaw |
|-----------|------------------|----------|
| Hosting | Cloud-hosted | **Self-hosted**, your data stays with you |
| Capability | Chat, answer questions | **Execute tasks**: send emails, run commands, manipulate files |
| Interface | Web / App only | **Any chat platform**: DingTalk, Telegram, Discord, WhatsApp |
| Extensibility | Limited | **Skills system**: 5,700+ community skills via ClawHub |
| Memory | Limited | **Persistent memory**: gets smarter over time |
| Scheduling | Not supported | **Cron system**: auto-send daily briefs at 7:00 |
| Open Source | No | **MIT License**, free to use |

---

## What Can OpenClaw Do? (Real Examples)

### Daily Life
- "Check tomorrow's weather in Beijing and send to my Telegram"
- "Every morning at 7:00, send me today's to-do list"
- "Compare prices for this headphone on Taobao" (browser automation)

### Office Productivity
- "Organize my unread emails by priority"
- "Send these meeting notes to the DingTalk group"
- "Every Friday at 5 PM, generate this week's summary"

### Developer Workflow
- "Review this PR and give code review feedback"
- "Monitor server logs, alert me immediately on ERROR"
- "Write a Python script to process this CSV data"

### Social Media
- "Post a tech tweet to Twitter every morning"
- "Answer technical questions in the Discord channel"
- "Summarize today's Slack channel discussions"

---

## History

| Date | Name | Event |
|------|------|-------|
| Nov 2025 | Clawdbot | Peter Steinberger creates the project |
| Jan 27, 2026 | Moltbot | Renamed due to Anthropic trademark complaint |
| Jan 30, 2026 | **OpenClaw** | Final name via community vote |
| Feb 2026 | — | 195K GitHub stars in 66 days (18x faster than Kubernetes) |
| Mar 2026 | — | 250K+ GitHub stars, 5,700+ community skills |

---

## Core Architecture (6 Layers)

| Layer | Purpose |
|-------|---------|
| **Gateway** | Central control — message routing, sessions, plugins, tool execution |
| **Channels** | Adapters for Telegram/WhatsApp/Discord/DingTalk into standard message format |
| **Routing + Sessions** | Determines which agent handles which conversation |
| **Agent Runtime** | Processes context, calls model providers, streams responses, requests tools |
| **Tools** | Capabilities — web fetch, browser control, command execution, device pairing |
| **Surfaces** | Interaction — chat apps, web dashboard, macOS menu bar, Live Canvas |

---

## Core Design Principles

1. **Self-hosted first** — Your data, your rules
2. **Multi-channel unified** — One gateway serves all chat platforms
3. **Agent-native** — Built for AI agents with tool calling, sessions, memory
4. **Open community** — MIT license, community-driven with 5,700+ skills on ClawHub

---

## System Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| OS | macOS 11+ / Ubuntu 22.04+ / Windows (WSL2) | Ubuntu 22.04+ |
| Node.js | **v22.0+** | v22.x LTS |
| RAM | 2 GB | 4-8 GB |
| CPU | 2 cores | 2-4 cores |
| Storage | 10 GB SSD | 30-50 GB SSD |

---

*Next: [Installation →](02-installation.md)*
