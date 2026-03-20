# Chapter 7.1: Real Project — Second Brain Knowledge System

> A complete, production-ready project that combines OpenClaw + Claude Code into a unified personal knowledge management system.

## Project Overview

**openclaw-second-brain** is a real-world project that demonstrates the full power of OpenClaw as an AI Agent infrastructure. It automatically:

1. Captures conversations from OpenClaw TUI and Claude Code
2. Processes them through LLM-powered summarization pipelines
3. Generates structured knowledge notes and logs
4. Maintains a unified memory system with knowledge graph
5. Runs automated agents for daily research and knowledge sync

```
Claude Code Conversations      OpenClaw TUI Conversations
    |                              |
    +-- ~/.claude/projects/       +-- ~/.openclaw/agents/main/sessions/
    |                              |
    +-------------+----------------+
                  |
          Enhanced Knowledge Agent
          (runs every hour via cron)
                  |
          +-- Auto-discover Claude Code sessions
          +-- Format conversion to unified format
          +-- Deduplication (avoid reprocessing)
          +-- LLM knowledge extraction
                  |
          Second Brain Knowledge Base
          (Notes + Logs + Summary)
                  |
          Memory System Update
          (daily at midnight)
                  |
          Next conversation carries memory
```

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Runtime | Node.js 22+ |
| Frontend | Next.js (Web UI) |
| Knowledge Graph | D3.js |
| AI Models | Claude Sonnet 4.5 (primary), Qwen models (backup) |
| LLM Pipeline | TypeScript custom summarization |
| API | DashScope API (OpenAI-compatible) or Anthropic API |
| Process Manager | systemd |
| Scheduling | OpenClaw Cron |
| Storage | JSONL files + JSON metadata |

## Project Structure

```
openclaw-second-brain/
+-- package.json              # npm scripts and dependencies
+-- .env.example              # Environment template
+-- summary-config.json       # LLM pipeline configuration
|
+-- lib/                      # Core library
|   +-- claude-code-adapter.ts    # Claude Code session converter
|   +-- ai-organizer.ts          # AI organization
|   +-- content-manager.ts       # Content management
|   +-- search.ts                # Full-text search
|   +-- graph-builder.ts         # Knowledge graph
|   +-- summary/                 # Summarization pipeline
|       +-- conversation-processor.ts
|       +-- llm-integration.ts
|       +-- clustering.ts
|       +-- content-merger.ts
|
+-- skills/                   # OpenClaw Agent Skills
|   +-- knowledge-agent-skill/
|   |   +-- SKILL.md             # Knowledge sync agent
|   |   +-- config.json          # Agent configuration
|   +-- research-agent-skill/
|   |   +-- SKILL.md             # Daily research agent
|   |   +-- config.json
|   +-- project-developer-skill/
|   |   +-- SKILL.md             # Project development agent
|   +-- social-research-skill/
|       +-- SKILL.md             # Social research agent
|       +-- README.md
|
+-- scripts/                  # Shell scripts
|   +-- openclaw-tui-with-keys.sh   # Launch TUI with API keys
|   +-- test-enhanced-sync.sh       # Test knowledge sync
|   +-- health-check.sh            # Health check
|
+-- content/                  # Generated content
|   +-- notes/                    # Knowledge notes (Markdown)
|   +-- logs/                     # Conversation logs
|   +-- reports/                  # Research reports
|
+-- data/                     # Data storage
    +-- conversations.json        # Conversation metadata
    +-- summaries/                # Summary JSON files
    +-- tag-library.json          # Tag taxonomy
```

## Setting Up the Project

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/openclaw-second-brain.git
cd openclaw-second-brain
```

### 2. Create Environment File

```bash
cp .env.example .env
```

Edit `.env`:

```bash
# OpenAI Compatible API Configuration
OPENAI_API_KEY=your-dashscope-api-key
OPENAI_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1

# OpenClaw Sessions Path (usually no need to change)
OPENCLAW_SESSIONS_PATH=/home/youruser/.openclaw/agents/main/sessions

# Optional
PORT=3000
LOG_LEVEL=info
```

### 3. Install Dependencies

```bash
npm install
```

### 4. Initialize Summary System

```bash
npm run summary:init
```

### 5. Start Development Server

```bash
npm run dev
# Visit http://localhost:3000
```

## Session Format (JSONL)

OpenClaw stores conversations in JSONL (JSON Lines) format:

```jsonl
{"type":"session","version":3,"id":"session-id","timestamp":"2026-02-27T03:00:00.000Z","cwd":"/path"}
{"type":"message","id":"msg-1","timestamp":"2026-02-27T03:00:01.000Z","message":{"role":"user","content":[{"type":"text","text":"Hello"}]}}
{"type":"message","id":"msg-2","timestamp":"2026-02-27T03:00:02.000Z","message":{"role":"assistant","content":[{"type":"text","text":"Hi!"}]}}
```

The Knowledge Agent reads these files and:
1. Reads all `.jsonl` files
2. Skips `.jsonl.lock` files (being written)
3. Extracts `type: "message"` records
4. Keeps only `role: "user"` and `role: "assistant"` messages
5. Filters conversations shorter than 50 characters

## Configuring Cron Jobs

### Knowledge Sync (Every Hour)

```bash
openclaw cron add \
  --name "Knowledge Sync" \
  --cron "0 * * * *" \
  --session isolated \
  --message "cd /path/to/openclaw-second-brain && npm run agent:knowledge" \
  --delivery none
```

### Daily Research Report (23:00 daily)

```bash
openclaw cron add \
  --name "Daily Research" \
  --cron "0 23 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "cd /path/to/openclaw-second-brain && npm run agent:research" \
  --delivery none
```

### Managing Cron Jobs

```bash
# List all jobs
openclaw cron list

# View execution history
openclaw cron runs --name "Knowledge Sync" --limit 10

# Manually trigger a job
openclaw cron run --name "Knowledge Sync"

# Disable a job
openclaw cron edit <job-id> --enabled false

# Enable a job
openclaw cron edit <job-id> --enabled true

# Delete a job
openclaw cron remove <job-id>
```

## The Knowledge Agent in Detail

The Knowledge Agent is the heart of the system. Here is the actual SKILL.md:

```markdown
# KNOWLEDGE-AGENT Skill

> You are a sub-Agent called by cron jobs, responsible for executing
> the complete conversation summary data pipeline.

## Execution Command

npm run agent:knowledge

This script automatically completes:
1. Initialize system - verify config, ensure directories exist
2. Discover Claude Code conversations - auto-find and parse session files
3. Unified format conversion - convert Claude Code to OpenClaw format
4. Process conversations - read unprocessed conversations, generate summaries
5. Convert to Markdown - create/update Notes and Logs files
6. Create backup - auto-backup data
7. Statistical analysis - return complete system statistics
8. Return results - structured JSON for Agent use
```

### Conversation Sources

The agent processes two sources:

| Source | Path | Format | Auto-discover |
|--------|------|--------|--------------|
| OpenClaw TUI | `~/.openclaw/agents/main/sessions/*.jsonl` | Native OpenClaw | Yes |
| Claude Code | `~/.claude/projects/*/xxx.jsonl` | Claude format (auto-converted) | Yes |

### Output Locations

| Type | Path | Description |
|------|------|-------------|
| Notes | `content/notes/` | Knowledge documents |
| Logs | `content/logs/` | Conversation records |
| Summaries | `data/summaries/` | JSON format metadata |
| Tracking | `~/.openclaw/workspace/memory/processed-claude-code-sessions.json` | Processed sessions |

## The Research Agent

Runs daily, analyzes conversation topics, and generates research reports:

```bash
npm run agent:research
```

### Workflow

1. **Get hot topics** — calls `summaryRetriever.getTopTopics(10)`, filters last 7 days
2. **Get hot keywords** — calls `summaryRetriever.getTopKeywords(20)`
3. **Get statistics** — total conversations, topics, domains
4. **Agent searches internet** — Google, GitHub, HN, Dev.to
5. **Generate research report** — saves to `content/reports/YYYY-MM-DD-topic.md`

### Report Structure

```markdown
---
date: YYYY-MM-DD
type: daily-research
title: Research Topic Title
tags: [tag1, tag2]
ai_generated: true
sources: [url1, url2]
---

# Research Topic Title

## Research Background
## Key Findings
## Technical Deep Analysis
## Best Practices
## Recommended Actions
## References
```

## The Social Research Agent

An on-demand research agent that analyzes community discussions:

```
User Request → LLM Analyzes Intent → Generate Research Plan → Parallel Search → LLM Integration → Generate Report
```

### Research Types

| Type | Description |
|------|-------------|
| Trend Analysis | Search 3-month discussions, analyze trends |
| Tool Comparison | Collect real user reviews, compare pros/cons |
| Best Practices | Search practical experience, code examples |
| Community Opinion | Analyze consensus and controversy |

### Usage

```bash
# Via API
curl -X POST http://localhost:3000/api/social-research/analyze \
  -H "Content-Type: application/json" \
  -d {query: Cursor vs Copilot comparison}

# Or just ask in conversation
"Help me research [topic]"
```

## The Project Developer Agent

A multi-stage workflow agent for building monetizable projects:

### Phases

1. **Research** (2-4h): Market analysis, feasibility study, proposal
2. **Design** (2-3h): Tech stack, architecture, development plan
3. **Development** (12-16h): Environment setup, core features, testing, deployment
4. **Operations** (ongoing): User acquisition, analytics, iteration

### Virtual Team Roles

| Role | Responsibilities |
|------|-----------------|
| Product Manager | Requirements, UX design, priority management |
| Tech Architect | Technology selection, architecture, code review |
| Frontend Developer | UI, interactions, frontend optimization |
| Backend Developer | API, database, business logic, performance |
| QA Engineer | Test cases, automation, quality assurance |
| Operations | Marketing, user ops, data analysis |

## Memory System Architecture

The system uses a 4-layer memory architecture:

| Layer | Persistence | Content |
|-------|------------|---------|
| User Preferences | Long-term | Names, habits, communication style |
| Decision History | Medium-term | Past choices, project decisions |
| Technical Knowledge | Short to medium-term | Learned concepts, code patterns |
| Conversation History | Short-term | Recent interactions |

## Best Practices from Production

### 1. Have Meaningful Conversations

```bash
# Set titles for important conversations
openclaw chat --title "React Performance Optimization" "Discuss React performance"
```

### 2. Archive Old Sessions

```bash
# Archive sessions older than 30 days
openclaw session archive --older-than 30d

# Or manual backup
cp -r ~/.openclaw/agents/main/sessions \
  ~/.openclaw/backups/sessions-$(date +%Y%m%d)
```

### 3. Monitor Cron Jobs

```bash
# Weekly check on job execution
openclaw cron runs --name "Knowledge Sync" --limit 50

# Check failed jobs
openclaw cron runs --name "Knowledge Sync" --failed
```

### 4. Protect Your API Keys

Add to `.gitignore`:

```gitignore
# OpenClaw config and auth files (contain API keys)
.openclaw/
**/auth-profiles.json
scripts/openclaw-tui*.sh
scripts/*-with-keys.sh

# Environment variables and keys
.env
.env.local
*.key
*.pem
```

---

*Next: [Code Reviewer Bot →](02-code-reviewer.md)*
