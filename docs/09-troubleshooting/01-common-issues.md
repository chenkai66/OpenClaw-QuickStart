# Chapter 9.1: Common Issues & Solutions

---

## 1. "openclaw: command not found"

**Cause:** OpenClaw not in PATH or not installed.

```bash
# Check if installed
which openclaw
npm list -g openclaw

# Fix
npm install -g openclaw@latest
# Or reinstall
curl -fsSL https://openclaw.ai/install.sh | bash
```

---

## 2. Gateway Won't Start

**Symptom:** `openclaw gateway` exits immediately.

```bash
# Check port conflict
lsof -i :18789
# Kill conflicting process
kill $(lsof -t -i :18789)

# Check status
openclaw status
openclaw gateway --verbose
```

---

## 3. API Key Invalid / Authentication Error

**Symptom:** "Unauthorized" or "Invalid API Key"

```bash
# Check environment variables
echo $OPENAI_API_KEY
echo $OPENAI_BASE_URL

# Common mistakes:
# 1. Mixing Coding Plan key (sk-sp-xxx) with regular DashScope URL
# 2. Mixing regular key (sk-xxx) with Coding Plan URL
# 3. Key has expired or been regenerated
```

**Fix:** Match the correct pair:

| Key Type | API Key Format | Base URL |
|----------|---------------|----------|
| Regular DashScope | `sk-xxxxx` | `https://dashscope.aliyuncs.com/compatible-mode/v1` |
| Coding Plan | `sk-sp-xxxxx` | `https://coding.dashscope.aliyuncs.com/v1` |
| Anthropic | `sk-ant-xxxxx` | (no override needed) |

---

## 4. Model Not Found

**Symptom:** "Model not available" error

```bash
# List available models
openclaw models list

# Common cause: model ID typo in openclaw.json
# Correct IDs (DashScope):
#   qwen3-max, qwen3.5-plus, qwen3.5-flash
# Correct IDs (Coding Plan):
#   qwen3.5-plus, qwen3-max-2026-01-23, qwen3-coder-next,
#   qwen3-coder-plus, kimi-k2.5, glm-5, glm-4.7, MiniMax-M2.5
```

---

## 5. Cron Task Not Triggering

**Cause 99%:** Timezone issue — no `tz` field means UTC.

```json
{
  "tools": {
    "cron": {
      "enabled": true,
      "tz": "Asia/Shanghai"
    }
  }
}
```

Also check `delivery.mode`:
- `"announce"` = sends message to your channel
- Not set = runs silently, you won't see output

---

## 6. AI "Forgets" Mid-Conversation

**Cause:** Context compression triggered.

**Fix:** Enable memoryFlush (see [Advanced Tips](../06-advanced/03-advanced-tips.md)):

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

**Quick workaround:** `/compact Keep all technical decisions`

---

## 7. DingTalk Bot Not Responding

```bash
# Check gateway is running
openclaw status

# Check DingTalk config
cat ~/.openclaw/openclaw.json | grep -A 10 dingtalk

# Common issues:
# 1. AppKey/AppSecret incorrect
# 2. Robot not enabled in DingTalk admin
# 3. Stream mode not configured
# 4. Gateway not restarted after config change
openclaw gateway restart
```

---

## 8. Discord Bot Online But Not Replying

**99% cause:** MESSAGE CONTENT INTENT not enabled.

Fix:
1. Go to [Discord Developer Portal](https://discord.com/developers/applications)
2. Select your bot -> Bot tab
3. Scroll to "Privileged Gateway Intents"
4. Enable: **MESSAGE CONTENT INTENT**
5. Click Save Changes
6. `openclaw gateway restart`

---

## 9. Permission Denied on Gateway Service

```bash
# Check file permissions
ls -la /etc/systemd/system/openclaw-gateway.service

# Ensure openclaw binary is accessible
which openclaw

# If using systemd, reload and restart
systemctl daemon-reload
systemctl restart openclaw-gateway
journalctl -u openclaw-gateway -n 50 --no-pager
```

---

## 10. Session Files Not Saving

```bash
# Check sessions directory exists
ls -la ~/.openclaw/agents/main/sessions/

# Check disk space
df -h

# Check file permissions
ls -la ~/.openclaw/
```

---

## 11. Rate Limit Exceeded (Coding Plan)

**Symptom:** 429 errors or "rate limit" messages.

Pro Plan limits:
- 6,000 requests per 5 hours
- 45,000 requests per week
- 90,000 requests per month

**Strategies:**
- Use lighter models for simple tasks (`/model MiniMax-M2.5`)
- Use `/new` to start fresh sessions (shorter context = fewer API calls per message)
- Space out heavy work sessions

---

## 12. Skills Not Triggering

```bash
# Check skill is installed
openclaw skills list

# Common causes:
# 1. Skill description doesn't match your trigger words
# 2. Skill SKILL.md not in the right directory
# 3. Gateway hasn't loaded the skill (restart needed)
openclaw gateway restart
```

---

## Quick Diagnostic Script

```bash
#!/bin/bash
echo "=== OpenClaw Diagnostics ==="
echo "Node.js: $(node -v 2>/dev/null || echo 'NOT INSTALLED')"
echo "OpenClaw: $(openclaw --version 2>/dev/null || echo 'NOT INSTALLED')"
echo "Gateway: $(openclaw status 2>/dev/null | head -3)"
echo ""
echo "API Config:"
echo "  OPENAI_API_KEY: $([ -n "$OPENAI_API_KEY" ] && echo "SET (${OPENAI_API_KEY:0:8}...)" || echo "NOT SET")"
echo "  OPENAI_BASE_URL: ${OPENAI_BASE_URL:-NOT SET}"
echo "  ANTHROPIC_API_KEY: $([ -n "$ANTHROPIC_API_KEY" ] && echo "SET" || echo "NOT SET")"
echo ""
echo "Ports:"
lsof -i :18789 2>/dev/null || echo "  Port 18789: FREE"
echo ""
echo "Disk: $(df -h ~ | tail -1 | awk '{print $4}') available"
echo "Sessions: $(ls ~/.openclaw/agents/main/sessions/ 2>/dev/null | wc -l) files"
```

---

*Next: [FAQ →](02-faq.md)*
