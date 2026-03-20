# Chapter 6.1: Unified Memory System (Real Implementation)

> Based on the production Claude Code + OpenClaw unified memory system

## Overview

The memory system unifies conversations from both **Claude Code** and **OpenClaw TUI**, automatically capturing, processing, and accumulating knowledge.

```
Claude Code Conversations      OpenClaw TUI Conversations
    |                              |
    +-- ~/.claude/projects/       +-- ~/.openclaw/agents/main/sessions/
    |                              |
    +-------------+----------------+
                  |
          Enhanced Knowledge Agent (hourly cron)
                  |
          +-- Auto-discover sessions
          +-- Format conversion
          +-- Deduplication
          +-- LLM knowledge extraction
                  |
          Second Brain Knowledge Base
          (Notes + Logs + Summary)
                  |
          Memory System Update (daily midnight)
                  |
          Next conversation carries memory
```

## Memory Architecture: 4 Layers

| Layer | Persistence | Content |
|-------|------------|---------|
| User Preferences | Long-term | Name, habits, communication style |
| Decision History | Medium-term | Past choices, project decisions |
| Technical Knowledge | Short-medium | Learned concepts, patterns |
| Conversation History | Short-term | Recent interactions |

## Quick Start

```bash
cd ~/openclaw-second-brain

# Method 1: Use startup script (recommended)
./scripts/openclaw-tui.sh

# Method 2: Manual environment setup
export ANTHROPIC_API_KEY="your-idealab-api-key"
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080/idealab"
openclaw tui
```

## Prerequisites

**dashscope-proxy must be running:**
```bash
ps aux | grep dashscope-proxy
systemctl start dashscope-proxy  # if not running
```

**Gateway must be running:**
```bash
openclaw health
openclaw gateway restart  # if not running
```

## Automated Knowledge Pipeline

1. **Storage** - OpenClaw: `~/.openclaw/agents/main/sessions/*.jsonl`, Claude Code: `~/.claude/projects/*/*.jsonl`
2. **Discovery** - Knowledge Agent scans both directories hourly, skips `.jsonl.lock` files
3. **Format Conversion** - Claude Code adapter converts to OpenClaw format, filters < 50 chars
4. **LLM Processing** - Summarization, keyword extraction, topic clustering, sentiment analysis
5. **Intelligent Merging** - Similarity threshold 0.7, combines related conversations
6. **Content Generation** - Notes, Logs, Reports in Markdown
7. **Indexing** - Full-text search, tag library with frequency counts

## Configuring Cron Jobs

```bash
# Create enhanced knowledge sync (every hour)
openclaw cron add \
  --name "Enhanced Knowledge Sync" \
  --cron "0 * * * *" \
  --session isolated \
  --message "cd /path/to/second-brain && npm run agent:knowledge:enhanced" \
  --no-deliver

# Daily research report (23:00)
openclaw cron add \
  --name "Daily Research" \
  --cron "0 23 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "cd /path/to/second-brain && npm run agent:research" \
  --delivery none

# Manage tasks
openclaw cron list
openclaw cron runs --name "Knowledge Sync" --limit 10
openclaw cron run --name "Knowledge Sync"  # manual trigger
```

## Feature Comparison

| Feature | Basic Sync | Enhanced Sync |
|---------|-----------|--------------|
| OpenClaw TUI conversations | Yes | Yes |
| Claude Code conversations | No | **Yes** |
| Deduplication | Basic | **Complete tracking** |
| Format conversion | Single | **Multi-format** |
| Error handling | Basic | **Robust** |

## Monitoring

```bash
# Count Claude Code sessions
find ~/.claude/projects -name "*.jsonl" -type f | wc -l

# Check processed sessions
cat ~/.openclaw/workspace/memory/processed-claude-code-sessions.json | jq '. | length'

# View latest content
ls -lt content/notes/*.md | head -5
ls -lt content/logs/*.md | head -5
```

## Expected Output

```
Claude Code: Processed: 5, Exported: 5, Errors: 0
OpenClaw + Claude Code: Total: 8, Logs: 5, Notes: 3
Duration: 3.45s
Knowledge sync completed successfully!
```

## Deduplication

Tracking file: `~/.openclaw/workspace/memory/processed-claude-code-sessions.json`
- Each run only processes new sessions
- Delete tracking file to reprocess all: `rm ~/.openclaw/workspace/memory/processed-claude-code-sessions.json`

## LLM Config (summary-config.json)

```json
{
  "llm": { "model": "qwen-plus", "max_retries": 3, "temperature": 0.3 },
  "processing": { "batch_size": 10, "min_conversation_length": 50, "similarity_threshold": 0.7 },
  "clustering": { "algorithm": "semantic", "min_cluster_size": 2, "merge_threshold": 0.8 }
}
```

## Important Notes

1. First run processes ALL history - may take longer
2. dashscope-proxy must be running for Claude models
3. Deduplication is automatic
4. Cron tasks consume API quota - adjust frequency as needed
5. Regular backups recommended: `cp -r ~/.openclaw/ ~/.openclaw-backup-$(date +%Y%m%d)`

---

*Next: [Cron Jobs →](02-cron-jobs.md)*
