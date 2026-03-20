# Chapter 2.4: Workspace Files (SOUL / USER / AGENTS / HEARTBEAT / MEMORY)

> OpenClaw's "personality" is defined entirely in plain-text Markdown files.

---

## Overview

OpenClaw Agent behavior is defined by Markdown files in the workspace directory (`~/.openclaw/workspace/`). Each new session, the agent loads these files in order:

```
1. SOUL.md      -> "Who am I?"
2. USER.md      -> "Who am I serving?"
3. AGENTS.md    -> "How should I work?" (the behavior manual)
4. HEARTBEAT.md -> "What automatic behaviors do I have?"
5. MEMORY.md    -> Load memory index
```

---

## SOUL.md — Core Personality

Defines the agent's identity, values, and communication style.

**Location**: `~/.openclaw/workspace/SOUL.md`

```markdown
# Soul

## Core Truths
- I am Lobster, a helpful AI assistant
- I prioritize accuracy over speed
- I always explain my reasoning

## Communication Style
- Friendly but professional
- Use simple language, avoid jargon
- Provide code examples when helpful

## Values
- Privacy first: never share user data
- Transparency: always explain what I'm doing
- Reliability: double-check before confirming
```

**Customization tips:**
- Chinese responses: add `Always respond in Chinese (简体中文)`
- More humor: add `Use witty humor when appropriate`
- More rigorous: add `Always verify facts before stating them`

---

## USER.md — User Profile

Stores information about you. The agent auto-updates this as it learns your preferences.

```markdown
# User

## Basic Info
- Name: Boss
- Timezone: Asia/Shanghai
- Language: Chinese (Simplified)

## Preferences
- Prefers concise responses
- Likes code examples in Python
- Uses VSCode as primary editor

## Work Context
- Software engineer at a tech company
- Works on cloud infrastructure
- Interested in AI and automation
```

---

## AGENTS.md — The Behavior Manual (Critical!)

This is the most important file for advanced users. It's the AI's **work manual**: what to do at startup, how to manage memory, what's safe to do autonomously.

> Without AGENTS.md, the AI doesn't know: which files to read first, where to write memories, or what requires your permission.

### Session Startup Flow

```markdown
## Every Session

Before doing anything else:

1. Read `SOUL.md` — this is who you are
2. Read `USER.md` — this is who you're helping
3. Read `memory/YYYY-MM-DD.md` (today + yesterday) for recent context
4. If in MAIN SESSION: Also read `MEMORY.md`

Don't ask permission. Just do it.
```

Why read yesterday's log too? At 1 AM, today's log may be empty.

### Memory Writing Rules

```markdown
## Memory

You wake up fresh each session. These files are your continuity.

| Layer | File | Purpose |
|-------|------|---------|
| Index | `MEMORY.md` | Core info, memory index. Keep < 40 lines |
| Projects | `memory/projects.md` | Current project status and TODOs |
| Lessons | `memory/lessons.md` | Mistakes and lessons, by severity |
| Daily Log | `memory/YYYY-MM-DD.md` | Daily raw records |

### Writing Rules
- Write to `memory/YYYY-MM-DD.md` for daily logs
- Update `memory/projects.md` when projects progress
- Write `memory/lessons.md` after encountering issues
- Update MEMORY.md only when the index changes
- **Record conclusions, not process** (critical principle!)
- Use #tags for memorySearch retrieval
```

**Good vs bad log examples:**

Bad (wastes tokens, poor search accuracy):
```
### Deployment
Deployed today. First tried running directly, got errors.
Then spent hours debugging, turned out port was occupied...
(3 pages of stream-of-consciousness)
```

Good (concise, high memorySearch hit rate):
```
### [PROJECT:MyApp] Deployment Complete
- **Conclusion**: Deployed with nginx reverse proxy on port 80
- **Files Changed**: `/etc/nginx/sites-available/myapp`
- **Lesson**: Direct port exposure doesn't work, must use nginx
- **Tags**: #myapp #deploy #nginx
```

### Safety Boundaries

```markdown
## Safety
- Don't exfiltrate private data. Ever.
- Don't run destructive commands without asking.
- `trash` > `rm` (recoverable beats gone forever)
- When in doubt, ask.

**Safe to do freely:** Read files, search, organize, work within workspace
**Ask first:** Sending emails/tweets, anything that leaves the machine
```

### Complete AGENTS.md Template (Copy-Paste Ready)

```markdown
# AGENTS.md - Your Workspace

This folder is home. Treat it that way.

## Every Session
1. Read `SOUL.md`
2. Read `USER.md`
3. Read `memory/YYYY-MM-DD.md` (today + yesterday)
4. If in MAIN SESSION: Also read `MEMORY.md`

## Memory
| Layer | File | Purpose |
|-------|------|---------|
| Index | `MEMORY.md` | Core info, keep concise |
| Projects | `memory/projects.md` | Project status & TODOs |
| Lessons | `memory/lessons.md` | Mistakes by severity |
| Daily | `memory/YYYY-MM-DD.md` | Daily records |

### Rules
- Record conclusions, not process
- Use #tags for search
- If you want to remember it, write it to a file

## Safety
- No data exfiltration
- No destructive commands without asking
- `trash` > `rm`

**Free:** Read, search, organize within workspace
**Ask first:** Emails, tweets, anything external

## Group Chats
You have access to your human's stuff. Don't share it.
```

---

## HEARTBEAT.md — Heartbeat & Automation

Defines automatic behaviors and periodic checks.

```markdown
# Heartbeat

## Daily Brief
- Every day at 07:00: Check weather, calendar, news -> send summary

## Weekly Report
- Every Friday at 17:00: Summarize week's activities -> send report

## Memory Maintenance
- Every Sunday at 03:00: Deduplicate MEMORY.md, archive old daily logs
```

---

## MEMORY.md — Memory Index

Auto-managed by the system. Stores memory index and references.

```markdown
# Memory

## Key Facts
- User prefers Python over JavaScript
- Project uses PostgreSQL database
- Git branch naming: feature/xxx

## Active Projects
- See memory/projects.md for details

## Recent Context
- See memory/YYYY-MM-DD.md for daily logs
```

---

## Best Practices

1. **SOUL.md should be concise**: 5-10 core traits max
2. **USER.md evolves**: Let agent auto-learn, periodically verify accuracy
3. **AGENTS.md is your biggest lever**: Start simple, add rules as you encounter issues. Experienced users have 200+ lines
4. **Backup workspace**: These files are the agent's "soul"
5. **Multi-agent isolation**: Different agents have independent workspace files

---

## Session Types

| Type | Description | MEMORY.md Access |
|------|-------------|-----------------|
| Main | Direct chat (TUI, WebChat, DM) | Yes |
| Group | Group chat (Discord server, DingTalk group) | No (privacy) |
| Sub-agent | Child process dispatched by main agent | No |
| Cron | Triggered by scheduled task | No |

---

*Next: [Sessions →](05-sessions.md)*
