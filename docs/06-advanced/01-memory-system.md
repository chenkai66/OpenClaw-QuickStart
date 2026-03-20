# Chapter 6.1: Memory System

> OpenClaw's persistent memory lets your AI assistant get smarter over time.

---

## Overview

OpenClaw uses a **file-based memory system** that persists across sessions. Every new session, the AI starts fresh — but by reading memory files, it recovers context.

```
Session starts -> AI reads workspace files:
  1. SOUL.md (identity)
  2. USER.md (user info)
  3. AGENTS.md (behavior rules)
  4. memory/YYYY-MM-DD.md (recent daily logs)
  5. MEMORY.md (core facts index)
```

---

## Memory Architecture: 4 Layers

| Layer | File | Persistence | Content |
|-------|------|------------|---------|
| Index | `MEMORY.md` | Long-term | Core facts, preferences, memory index (< 40 lines) |
| Projects | `memory/projects.md` | Medium-term | Active project status and TODOs |
| Lessons | `memory/lessons.md` | Long-term | Mistakes and solutions by severity |
| Daily | `memory/YYYY-MM-DD.md` | Short-term | Daily raw logs with conclusions |

---

## Setting Up Memory

### 1. Create Memory Directory

```bash
cd ~/.openclaw/workspace
mkdir -p memory
```

### 2. Initialize MEMORY.md

```markdown
# Memory

## Key Facts
- User prefers Python
- Timezone: Asia/Shanghai
- Primary editor: VSCode

## Active Projects
- See memory/projects.md

## Memory Index
- Daily logs: memory/YYYY-MM-DD.md
- Project status: memory/projects.md
- Lessons learned: memory/lessons.md
```

### 3. Configure AGENTS.md Memory Rules

Add to your `AGENTS.md` (see [Chapter 2.4](../02-core-concepts/04-workspace-files.md)):

```markdown
## Memory Rules
- Write daily logs to `memory/YYYY-MM-DD.md`
- Record conclusions, not process
- Use #tags for memorySearch retrieval
- Update MEMORY.md only when index changes
- Keep MEMORY.md < 40 lines
```

---

## Preventing Memory Loss: memoryFlush

When conversations get long, OpenClaw compresses old messages. Enable memoryFlush to auto-save before compression:

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

See [Advanced Tips](03-advanced-tips.md) for detailed explanation.

---

## memorySearch: Semantic Search over Memory

OpenClaw supports semantic search over your memory files. To enable, you need an embedding API.

### Free Option: SiliconFlow bge-m3

```json
{
  "tools": {
    "memorySearch": {
      "enabled": true,
      "embedding": {
        "provider": "openai-compatible",
        "baseUrl": "https://api.siliconflow.cn/v1",
        "apiKey": "YOUR_SILICONFLOW_KEY",
        "model": "BAAI/bge-m3"
      }
    }
  }
}
```

Get a free API key at [SiliconFlow](https://siliconflow.cn/).

### How It Works

1. Memory files are chunked and embedded into vectors
2. When you ask "what did we discuss about deployment?", OpenClaw searches semantically
3. Relevant memories are injected into context

---

## Automated Knowledge Sync (Advanced)

For power users who want to combine memories from multiple AI tools (e.g., Claude Code + OpenClaw):

### Cron-based Sync

```bash
# Hourly knowledge sync
openclaw cron add \
  --name "Knowledge Sync" \
  --cron "0 * * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "Read all memory files, deduplicate, update MEMORY.md index"

# Daily summary (23:00)
openclaw cron add \
  --name "Daily Summary" \
  --cron "0 23 * * *" \
  --tz "Asia/Shanghai" \
  --delivery announce \
  --message "Summarize today's work from memory/$(date +%Y-%m-%d).md"
```

### Multi-Source Memory Pipeline

```
Claude Code sessions    OpenClaw TUI sessions
  |                        |
  +-- ~/.claude/           +-- ~/.openclaw/sessions/
  |                        |
  +---------+--------------+
            |
    Knowledge Agent (hourly cron)
            |
    Deduplicate + Extract insights
            |
    Update memory/ directory
```

---

## Memory Maintenance

Add to HEARTBEAT.md for automatic cleanup:

```markdown
## Memory Maintenance
- Every Sunday at 03:00:
  1. Deduplicate MEMORY.md entries
  2. Archive daily logs older than 30 days
  3. Verify projects.md matches current status
```

---

## Daily Log Format

```markdown
### [PROJECT:MyApp] Deployment Complete
- **Conclusion**: Deployed with nginx on port 80
- **Files**: /etc/nginx/sites-available/myapp
- **Lesson**: Must use reverse proxy, direct port exposure fails
- **Tags**: #myapp #deploy #nginx
```

---

## Monitoring

```bash
# Check memory health
wc -l ~/.openclaw/workspace/MEMORY.md      # Should be < 40 lines
ls -lt ~/.openclaw/workspace/memory/*.md    # Recent logs

# Count session files
ls ~/.openclaw/agents/main/sessions/*.jsonl | wc -l
```

---

## Best Practices

1. **Keep MEMORY.md lean**: < 40 lines, index only
2. **Enable memoryFlush**: Prevents losing context during long chats
3. **Record conclusions, not process**: "Deployed with nginx on port 80" not "Tried running directly, got errors..."
4. **Use #tags**: Makes memorySearch much more effective
5. **Weekly maintenance**: Clean up stale entries
6. **Backup regularly**: `cp -r ~/.openclaw/ ~/.openclaw-backup-$(date +%Y%m%d)`

---

*Next: [Cron Jobs →](02-cron-jobs.md)*
