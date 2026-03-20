# Chapter 6.3: Advanced Tips & Configuration Cheatsheet

> Real-world techniques from power users to get the most out of OpenClaw.

---

## 1. Fix "AI Amnesia" with memoryFlush

### The Problem

OpenClaw compresses old conversation when approaching the model's context window limit. During compression, details may be lost — the AI "forgets" what you discussed.

### The Solution

Enable `memoryFlush`: before compression triggers, the AI automatically saves critical information to files.

Add to `~/.openclaw/openclaw.json`:

```json
{
  "agents": {
    "defaults": {
      "compaction": {
        "reserveTokensFloor": 20000,
        "memoryFlush": {
          "enabled": true,
          "softThresholdTokens": 4000
        }
      }
    }
  }
}
```

| Parameter | Meaning | Recommended |
|-----------|---------|-------------|
| `reserveTokensFloor` | Minimum tokens to keep after compaction | 20000 |
| `memoryFlush.enabled` | Auto-save before compression | `true` |
| `softThresholdTokens` | Start flushing this many tokens before limit | 4000 |

### Manual Compaction

When in a long conversation, manually trigger with preservation hints:

```
/compact Keep all technical decisions and code snippets
```

### Other Memory Commands

```
/new          # Start a fresh session
/status       # Check current token usage
```

---

## 2. Block Streaming — Fix Slow Long Responses

By default, OpenClaw waits until the full response is generated before sending. Enable block streaming to send in chunks:

```json
{
  "agents": {
    "defaults": {
      "blockStreamingDefault": "on",
      "blockStreamingBreak": "text_end",
      "blockStreamingChunk": {
        "minChars": 200,
        "maxChars": 1500
      }
    }
  }
}
```

Now long responses appear progressively in your chat instead of one giant message.

---

## 3. Heartbeat Optimization — Prevent Late-Night Disturbances

Set active hours so the AI doesn't message you at 3 AM:

```json
{
  "agents": {
    "defaults": {
      "heartbeat": {
        "every": "30m",
        "target": "last",
        "activeHours": {
          "start": "08:00",
          "end": "23:00"
        }
      }
    }
  }
}
```

---

## 4. Acknowledge Reactions — Instant "Message Received" Feedback

When you send a message, the AI can immediately react with an emoji so you know it's processing:

```json
{
  "channels": {
    "discord": { "ackReaction": "🫐" },
    "telegram": { "ackReaction": "👀" }
  }
}
```

---

## 5. Model Tiering Strategy — Save 60-70% on Tokens

Don't use the most expensive model for everything. In your AGENTS.md, define a tiering strategy:

```markdown
## Model Usage Strategy

- **Simple tasks** (search, summarize, format): Use `qwen3.5-flash` or `MiniMax-M2.5`
- **Standard tasks** (writing, analysis, chat): Use `qwen3.5-plus`
- **Complex tasks** (architecture, debugging, reasoning): Use `qwen3-max` or `kimi-k2.5`
- **Code tasks**: Use `qwen3-coder-next`
```

In TUI, switch on-the-fly:
```
/model qwen3.5-flash     # Quick cheap query
/model qwen3-max          # Need heavy reasoning now
```

---

## 6. Sub-Agent Parallel Tasks

OpenClaw can spawn sub-agents to handle tasks in parallel. Instead of the main agent doing everything sequentially, it delegates:

### How It Works

1. You tell the main agent: "Research these 3 topics and summarize"
2. Main agent spawns 3 sub-agents, each researching one topic
3. Sub-agents report back, main agent synthesizes

### Enable in AGENTS.md

```markdown
## Sub-Agents

When a task has 3+ independent subtasks, spawn sub-agents:
- Each sub-agent gets a focused task description
- Sub-agents work in parallel
- Main agent collects and synthesizes results
- Sub-agents use lighter models (qwen3.5-flash) unless task requires heavy reasoning
```

### How Sub-Agents Work in Practice

```
You: "Compare React, Vue, and Svelte for our new project"

Main Agent thinking:
  -> Spawn sub-agent 1: "Research React pros/cons for enterprise"
  -> Spawn sub-agent 2: "Research Vue pros/cons for enterprise"
  -> Spawn sub-agent 3: "Research Svelte pros/cons for enterprise"
  -> Wait for all 3 to complete
  -> Synthesize into comparison table
```

---

## 7. Memory Maintenance — Prevent Memory Rot

Over time, memories accumulate and become stale. Add automatic maintenance to HEARTBEAT.md:

```markdown
## Memory Maintenance (Weekly)
- Every Sunday at 03:00:
  1. Deduplicate entries in MEMORY.md
  2. Archive daily logs older than 30 days to memory/archive/
  3. Verify memory/projects.md matches actual project status
  4. Remove completed/abandoned projects from active list
```

---

## 8. Multi-Channel Format Adaptation

Different platforms support different Markdown features:

| Format | Discord | Telegram | WhatsApp | DingTalk |
|--------|---------|----------|----------|----------|
| Tables | Yes | No | No | No |
| Code blocks | Yes | Yes | No | Yes |
| Bold/Italic | Yes | Yes | Yes | Yes |
| Emoji reactions | Yes | Yes | Yes | Yes |

Add to AGENTS.md for auto-adaptation:

```markdown
## Platform Formatting
- **Discord/Telegram**: Use markdown, wrap code in triple backticks
- **WhatsApp/DingTalk**: No tables — use bullet lists instead
- **Discord**: Wrap multiple links in <> to prevent preview spam
```

---

## 9. Full openclaw.json Configuration Cheatsheet

| Config Path | Purpose | Recommended |
|-------------|---------|-------------|
| `models.providers.bailian.baseUrl` | API endpoint | See Coding Plan chapter |
| `agents.defaults.model.primary` | Default model | `bailian/qwen3.5-plus` |
| `agents.defaults.blockStreamingDefault` | Progressive responses | `"on"` |
| `agents.defaults.compaction.memoryFlush.enabled` | Save before compress | `true` |
| `agents.defaults.heartbeat.activeHours` | Quiet hours | `08:00 - 23:00` |
| `tools.exec.enabled` | Allow shell commands | `true` |
| `tools.web.search.enabled` | Allow web search | `true` |
| `tools.web.search.apiKey` | Brave Search API key | Free at brave.com |
| `tools.media.image.enabled` | Image recognition | `true` (needs vision model) |
| `agents.defaults.workspace` | Workspace path | `~/.openclaw/workspace` |
| `channels.discord.maxLinesPerMessage` | Max lines per message | 17 |

---

## 10. Priority Checklist (1 Hour to Transform Your Setup)

| Priority | Task | Time |
|----------|------|------|
| 1 | **AGENTS.md**: Session startup + memory rules + safety boundaries | 30 min |
| 2 | **memoryFlush**: Enable auto-save before compression | 5 min |
| 3 | **ackReaction**: Message received confirmation | 1 min |
| 4 | **blockStreaming**: Progressive response delivery | 5 min |
| 5 | **Heartbeat activeHours**: No late-night messages | 5 min |
| 6 | **Model tiering**: Use cheap models for simple tasks | 15 min |
| 7 | **Cron tasks**: Daily brief / weekly report | 15 min |
| 8 | **Memory maintenance**: Weekly auto-cleanup | 10 min |
| 9 | **Custom Skill**: Write your first SKILL.md | 30 min |
| 10 | **Multi-channel**: Add Discord or Telegram | 30 min |

---

*Previous: [Cron Jobs →](02-cron-jobs.md) | Next: [MCP Integration →](05-mcp-integration.md)*
