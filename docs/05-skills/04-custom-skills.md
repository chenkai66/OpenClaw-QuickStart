# Chapter 5.4: Writing Custom Skills (With Real Production Examples)

> Actual SKILL.md files from a production OpenClaw deployment

## Skill File Structure Recap

Every skill is defined by a `SKILL.md` file in Markdown format. OpenClaw reads these files and loads them into the agent context when relevant.

```
~/.openclaw/skills/
+-- my-custom-skill/
    +-- SKILL.md          # Required: The skill definition
    +-- config.json       # Optional: Configuration
    +-- README.md         # Optional: Documentation
```

## Real Example 1: Knowledge Sync Agent

This is a real, production SKILL.md that runs hourly via cron job to process conversations:

```markdown
# KNOWLEDGE-AGENT Skill

> You are a sub-Agent called by cron jobs, responsible for executing
> the complete conversation summary data pipeline.

## Important: Cron Job Configuration

**You do NOT need to create or modify cron jobs!** Scheduled tasks
are managed by the main Agent.

If the user asks to configure cron jobs, tell them:

### Step 1: Find Project Path

\`\`\`bash
ls -la ~/openclaw/workspace/openclaw-second-brain 2>/dev/null || \
ls -la /root/openclaw-second-brain 2>/dev/null || \
find ~ -type d -name "openclaw-second-brain" 2>/dev/null | head -1
\`\`\`

### Step 2: Create Cron Job

\`\`\`bash
openclaw cron add \
  --name "Knowledge Sync" \
  --cron "0 * * * *" \
  --session isolated \
  --message "cd <PROJECT_PATH> && npm run agent:knowledge" \
  --delivery none
\`\`\`

## Execution Command

\`\`\`bash
npm run agent:knowledge
\`\`\`

This script automatically:
1. Initialize system - verify config, ensure directories exist
2. Discover Claude Code conversations - auto-find session files
3. Convert formats - Claude Code to OpenClaw unified format
4. Process conversations - generate summaries, intelligent categorization
5. Convert to Markdown - create/update Notes and Logs
6. Create backup - auto-backup data
7. Return structured JSON results

## Conversation Sources

1. **OpenClaw TUI** - ~/.openclaw/agents/main/sessions/*.jsonl
2. **Claude Code** - ~/.claude/projects/*/xxx.jsonl (auto-converted)

## Output

- Notes: content/notes/
- Logs: content/logs/
- Summaries: data/summaries/
```

### Key Patterns in This Skill

1. **Clear role definition** — "You are a sub-Agent called by cron jobs"
2. **Boundary setting** — "You do NOT need to create or modify cron jobs"
3. **Actionable instructions** — Step-by-step commands for users
4. **Multi-source processing** — Handles both OpenClaw and Claude Code sessions
5. **Structured output** — JSON result format specified

---

## Real Example 2: Daily Research Agent

This agent runs every night to analyze trending topics and generate research reports:

```markdown
# RESEARCH-AGENT Skill

> You are a sub-Agent called by cron jobs, responsible for analyzing
> user interests and generating research reports.

## Execution Command

\`\`\`bash
npm run agent:research
\`\`\`

## Workflow

### What the Script Does (Automatic)

1. Get hot topics - summaryRetriever.getTopTopics(10), filter last 7 days
2. Get hot keywords - summaryRetriever.getTopKeywords(20)
3. Get statistics - total conversations, topics, domains
4. Return structured data as JSON

### What the Agent Does (Manual)

1. Analyze data - review top_topics and top_keywords, choose research direction
2. Internet search - Google, GitHub, HN, Dev.to
3. Generate report - save to content/reports/YYYY-MM-DD-topic.md

## Report Structure

\`\`\`markdown
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
\`\`\`

## Notes

- Script only provides data, does NOT generate the report
- Agent must do its own internet searches
- Agent generates and saves the report itself
- Prioritize topics with most discussion
- Generate 1 research report per day
```

### Key Patterns

1. **Clear separation** — script vs agent responsibilities
2. **Structured output format** — frontmatter + sections template
3. **One report per day** — prevents overload
4. **Agent autonomy** — agent decides research direction based on data

---

## Real Example 3: Social Research Agent

An on-demand research agent that uses LLM to analyze user intent:

```markdown
# SOCIAL-RESEARCH Skill

> You are a research assistant that uses LLMs to analyze needs
> and execute community research.

## Intelligent Workflow

### 1. Analyze User Request (LLM)

Input:
- User research request
- Recent conversation history (from summary system)
- User tech stack and interests

Output JSON:
{
  "research_type": "trend_analysis" | "tool_comparison" | "best_practices",
  "search_keywords": ["keyword1", "keyword2"],
  "platforms": ["reddit", "twitter", "hackernews"],
  "focus_areas": ["implementation", "UX", "performance"],
  "report_structure": {
    "sections": ["Overview", "Discussion", "Findings", "Recommendations"],
    "depth": "detailed" | "summary"
  }
}

### 2. Execute Research (Based on LLM Decision)

Research Types:
1. Trend Analysis - search 3-month discussions, analyze trends
2. Tool Comparison - collect real user reviews, compare pros/cons
3. Best Practices - search practical experience, code examples
4. Community Opinion - analyze consensus and controversy

### 3. Intelligent Content Integration (LLM)

Input: search results (Reddit, Twitter, HN)
Output: structured report, key findings, personalized recommendations
```

### Key Patterns

1. **LLM-driven decision making** — uses AI to analyze user intent
2. **Dynamic search strategy** — keywords and platforms adapt to query
3. **Multi-platform search** — Reddit, Twitter, HackerNews
4. **Personalized output** — adapts to user background

---

## Real Example 4: Project Developer Agent

A comprehensive multi-phase project development agent:

```markdown
# Project Developer Agent

You are a senior project development expert, specializing in
building monetizable projects from 0 to 1.

## Your Capabilities

### 1. Requirements Analysis
- Market research and analysis
- User needs discovery
- Competitive analysis
- Business model design
- MVP planning

### 2. Technical Planning
- Tech stack selection
- Architecture design
- Development plan
- Risk assessment
- Cost estimation

### 3. Virtual Team Coordination
- Product Manager Agent
- Tech Architect Agent
- Frontend Developer Agent
- Backend Developer Agent
- QA Agent
- Operations Agent

## Work Phases

### Phase 1: Research (2-4 hours)
Market analysis, feasibility study, project proposal

### Phase 2: Design (2-3 hours)
Tech selection, architecture, development plan

### Phase 3: Development (12-16 hours)
Environment setup, core features, testing, deployment

### Phase 4: Operations (ongoing)
User acquisition, analytics, iteration
```

### Key Patterns

1. **Multi-phase workflow** — clear stages with time estimates
2. **Virtual team roles** — defines specialized sub-agents
3. **Goal-oriented** — "building monetizable projects"
4. **Complete lifecycle** — from research to operations

---

## Writing Your Own Skill: Template

```markdown
# MY-SKILL-NAME Skill

> One-line description of what this skill does

## When to Use

Describe the trigger conditions - when should this skill activate?

## Input

What information does the agent need?

| Parameter | Description | Required | Example |
|-----------|-------------|----------|---------|
| topic | Research topic | Yes | "React hooks" |
| depth | Analysis depth | No | "detailed" |

## Workflow

### Step 1: [Action Name]
Detailed instructions...

### Step 2: [Action Name]
Detailed instructions...

## Output Format

\`\`\`markdown
# Output Title
## Section 1
## Section 2
\`\`\`

## Configuration

\`\`\`json
{
  "key": "value",
  "setting": true
}
\`\`\`

## Notes

- Important caveat 1
- Important caveat 2
```

## Skill Configuration (config.json)

Real example from the Knowledge Agent:

```json
{
  "contentDirectories": {
    "notes": "content/notes",
    "logs": "content/logs",
    "reports": "content/reports"
  },
  "processing": {
    "batchSize": 10,
    "minConversationLength": 50,
    "similarityThreshold": 0.7
  },
  "tagCategories": [
    "technology",
    "architecture",
    "debugging",
    "learning",
    "project-management"
  ],
  "schedule": {
    "syncInterval": "0 * * * *",
    "reportTime": "0 23 * * *"
  }
}
```

---

*Next: [Memory System →](../06-advanced/01-memory-system.md)*
