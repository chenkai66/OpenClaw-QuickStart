# OpenClaw Complete Tutorial — From Zero to Mastery

> **OpenClaw** is the hottest open-source AI Agent platform of 2026, with 250K+ GitHub Stars.
> This tutorial takes you from zero to mastery, step by step.

---

## What Is This?

A **0-to-100** comprehensive OpenClaw tutorial, including:

- **10 chapters** of complete documentation: beginner to advanced
- **8 hands-on examples**: runnable code samples
- **Configuration templates**: ready-to-use config files
- **DingTalk integration**: detailed step-by-step guide
- **Coding Plan guide**: official Alibaba Cloud integration with 8 models

---

## Project Structure

```
openclaw-tutorial/
├── README.md
├── docs/
│   ├── 01-getting-started/            # Ch1: Getting Started
│   │   ├── 01-what-is-openclaw.md     #   What is OpenClaw
│   │   ├── 02-installation.md         #   Installation (all platforms)
│   │   ├── 03-first-chat.md           #   First conversation
│   │   ├── 04-web-dashboard.md        #   Web dashboard
│   │   └── 05-tui-setup.md            #   TUI setup
│   ├── 02-core-concepts/             # Ch2: Core Concepts
│   │   ├── 01-architecture.md         #   6-layer architecture
│   │   ├── 02-gateway.md              #   Gateway system
│   │   ├── 03-agent-loop.md           #   Agent loop
│   │   ├── 04-workspace-files.md      #   SOUL/USER/AGENTS/HEARTBEAT/MEMORY
│   │   └── 05-sessions.md            #   Session management
│   ├── 03-configuration/             # Ch3: Configuration
│   │   ├── 01-openclaw-json.md        #   openclaw.json guide
│   │   ├── 02-model-providers.md      #   Model providers (DashScope/Anthropic)
│   │   ├── 03-tools-config.md         #   Tools configuration
│   │   └── 04-environment-vars.md     #   Environment variables
│   ├── 04-channels/                  # Ch4: Channels
│   │   ├── 01-overview.md             #   Channel overview
│   │   └── 05-china-im.md            #   China IM (DingTalk/WeChat/Feishu)
│   ├── 05-skills/                    # Ch5: Skills System
│   │   ├── 01-understanding-skills.md #   Understanding Skills
│   │   └── 04-custom-skills.md        #   Custom Skill development
│   ├── 06-advanced/                  # Ch6: Advanced
│   │   ├── 01-memory-system.md        #   Memory system
│   │   ├── 02-cron-jobs.md            #   Cron scheduled tasks
│   │   ├── 03-advanced-tips.md        #   Advanced tips & config cheatsheet (NEW!)
│   │   └── 05-mcp-integration.md      #   MCP protocol integration
│   ├── 07-use-cases/                 # Ch7: Use Cases
│   │   └── 01-second-brain.md         #   Second Brain project
│   ├── 08-dingtalk-complete/         # Ch8: DingTalk Integration
│   │   ├── 01-prerequisites.md        #   Prerequisites
│   │   ├── 02-dingtalk-app-setup.md   #   DingTalk app setup
│   │   ├── 03-openclaw-dingtalk.md    #   OpenClaw DingTalk config
│   │   ├── 04-advanced-features.md    #   Advanced features
│   │   └── 05-production-deploy.md    #   Production deployment
│   ├── 09-troubleshooting/           # Ch9: Troubleshooting
│   │   ├── 01-common-issues.md        #   Common issues
│   │   └── 02-faq.md                  #   FAQ
│   └── 10-coding-plan/              # Ch10: Coding Plan (NEW!)
│       └── 01-coding-plan-guide.md    #   DashScope Coding Plan integration
├── examples/                         # Hands-on code samples
├── config/                           # Config templates
│   └── templates/
│       ├── basic-config.json          #   DashScope pay-per-use
│       ├── advanced-config.json       #   DashScope + third-party models
│       ├── coding-plan-config.json    #   Coding Plan (official, NEW!)
│       └── dingtalk-config.json       #   DingTalk integration
└── scripts/
```

---

## Quick Start

### 1. Install OpenClaw (5 min)

```bash
# Linux / macOS
curl -fsSL https://openclaw.ai/install.sh | bash

# Windows PowerShell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

### 2. Onboarding

```bash
openclaw onboard --install-daemon
```

### 3. Start Chatting

```bash
openclaw tui         # Terminal UI
# or
openclaw dashboard   # Web dashboard
```

---

## Learning Roadmap

| Stage | Chapters | Time | Goal |
|-------|----------|------|------|
| Beginner | Ch 1-2 | 2 hrs | Install, understand core concepts |
| Intermediate | Ch 3-5 | 4 hrs | Configuration, channels, Skills |
| Advanced | Ch 6, 10 | 4 hrs | Memory, Cron, Coding Plan, advanced tips |
| Hands-on | Ch 7-8 | 3 hrs | Real projects, DingTalk integration |

---

## API Options

| Option | Cost | Models | Best For |
|--------|------|--------|----------|
| **DashScope Pay-per-use** | Per token | Qwen series | Light usage |
| **Coding Plan** (recommended) | 200 CNY/mo | 8 models | Regular usage |
| **Anthropic** | Per token | Claude series | International users |

See [Ch 10: Coding Plan](docs/10-coding-plan/01-coding-plan-guide.md) for detailed setup.

---

## Resources

| Resource | Link |
|----------|------|
| Official GitHub | https://github.com/openclaw/openclaw |
| Official Docs | https://docs.openclaw.ai |
| ClawHub Skills | https://clawhub.ai |
| Awesome OpenClaw | https://github.com/rohitg00/awesome-openclaw |
| OpenClaw China Plugin | https://github.com/BytePioneer-AI/openclaw-china |

---

## License

MIT License — Free to use, modify, and share.
