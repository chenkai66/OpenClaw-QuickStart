# Chapter 9.1: Common Issues (From Real Deployments)

> Real troubleshooting scenarios from production OpenClaw deployments

## Issue 1: `openclaw: command not found`

**Cause:** OpenClaw not installed or not in PATH

```bash
# Reinstall
curl -fsSL https://openclaw.ai/install.sh | sh

# Or manually add to PATH
export PATH="$PATH:$HOME/.openclaw/bin"

# Or reinstall via npm
npm install -g openclaw@latest
```

## Issue 2: Proxy Service Won't Start

**Symptom:** `systemctl status dashscope-proxy` shows "failed"

```bash
# Check detailed error logs
journalctl -u dashscope-proxy -n 50 --no-pager

# Common causes:
# 1. Port 8080 occupied
lsof -i :8080
# Kill conflicting process or change proxy port

# 2. Node.js path wrong
which node  # Check actual path
# Update ExecStart in service file

# 3. File permissions
ls -l /root/dashscope-proxy.js
chmod +x /root/dashscope-proxy.js
```

## Issue 3: Claude Code Returns "terminated" Error

**Symptom:** `API Error: terminated`

```bash
# 1. Check proxy receives requests
journalctl -u dashscope-proxy --since '1 minute ago' -f
# Run claude command, watch for request logs

# 2. If no logs, check config
cat ~/.claude/settings.json
# Ensure ANTHROPIC_BASE_URL is correct

# 3. Manual test
curl -v http://127.0.0.1:8080/idealab/v1/messages \
  -H "x-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"claude_sonnet4_5","max_tokens":10,"messages":[{"role":"user","content":"hi"}]}'
```

## Issue 4: Response Missing ID Field

**Symptom:** Test shows `Has ID: false`

```bash
# 1. Check proxy version
head -5 /root/dashscope-proxy.js
# Should contain "Universal JSON/SSE/Gzip Support"

# 2. Check proxy logs for "Added ID"
journalctl -u dashscope-proxy -n 100

# 3. Redeploy proxy script if needed
# See Chapter 1.2 for the complete script

# 4. Restart service
systemctl restart dashscope-proxy
```

## Issue 5: OpenClaw Gateway Connection Failed

**Symptom:** Gateway startup fails or times out

```bash
# 1. Check if already running
ps aux | grep openclaw

# 2. Kill old processes
pkill -f "openclaw gateway"

# 3. Reinstall gateway
openclaw gateway install

# 4. Start with verbose logging
openclaw gateway --verbose

# 5. Check port conflict
lsof -i :18789
```

## Issue 6: Session Directory Not Found

**Cause:** Never had a conversation yet

```bash
# Start first conversation to create directory
openclaw chat "test conversation"

# Verify
ls -la ~/.openclaw/agents/main/sessions/
```

## Issue 7: API Key Invalid / Connection Refused

```bash
# Check environment variables
echo $OPENAI_API_KEY
echo $OPENAI_BASE_URL
echo $ANTHROPIC_API_KEY
echo $ANTHROPIC_BASE_URL

# Reset
export ANTHROPIC_API_KEY="your-valid-key"
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080/idealab"

# Test connection
openclaw chat "test"
```

## Issue 8: Cron Jobs Not Running

```bash
# Check task status
openclaw cron list

# Enable disabled task
openclaw cron edit <job-id> --enabled true

# Verify cron expression at crontab.guru
# "0 * * * *" = every hour on the hour

# Manual test
openclaw cron run --name "Knowledge Sync"
```

## Issue 9: Gzip Decompression Error

**Symptom:** Proxy logs show "Gunzip error"

```bash
# Check Node.js version (must be >= 18)
node --version

# Test zlib module
node -e "const zlib = require('zlib'); console.log('zlib OK');"

# If broken, reinstall Node.js
npm cache clean --force
# Then reinstall Node.js (see Chapter 1.2)
```

## Issue 10: Network / DNS Issues

**Symptom:** ECONNREFUSED or ETIMEDOUT

```bash
# 1. Test connectivity
ping idealab.alibaba-inc.com
curl -I https://idealab.alibaba-inc.com

# 2. Check firewall
sudo iptables -L -n

# 3. Check DNS
nslookup idealab.alibaba-inc.com

# 4. For internal networks, add proxy to systemd service:
# Environment="HTTP_PROXY=http://your-proxy:port"
# Environment="HTTPS_PROXY=http://your-proxy:port"
```

## Issue 11: Second Brain Web UI Not Loading

```bash
# Check if dev server is running
lsof -i :3000

# Start dev server
cd ~/openclaw-second-brain
npm run dev

# Check for build errors
npm run build 2>&1 | tail -20
```

## Issue 12: Knowledge Sync Produces Empty Results

```bash
# Check for conversation files
find ~/.openclaw/agents/main/sessions -name "*.jsonl" | wc -l
find ~/.claude/projects -name "*.jsonl" -type f | wc -l

# Reset processing tracker
rm ~/.openclaw/workspace/memory/processed-claude-code-sessions.json

# Run sync manually with verbose output
cd ~/openclaw-second-brain
npm run agent:knowledge:enhanced
```

## Quick Diagnostic Script

```bash
#!/bin/bash
echo "=== OpenClaw Diagnostic ==="
echo "Node.js: $(node --version 2>/dev/null || echo 'NOT FOUND')"
echo "OpenClaw: $(openclaw --version 2>/dev/null || echo 'NOT FOUND')"
echo "Claude: $(claude --version 2>/dev/null || echo 'NOT FOUND')"
echo "Proxy: $(systemctl is-active dashscope-proxy 2>/dev/null || echo 'NOT RUNNING')"
echo "Gateway: $(systemctl is-active openclaw-gateway 2>/dev/null || echo 'NOT RUNNING')"
echo "Port 8080: $(ss -tlnp | grep -q 8080 && echo 'OK' || echo 'NOT LISTENING')"
echo "Port 18789: $(ss -tlnp | grep -q 18789 && echo 'OK' || echo 'NOT LISTENING')"
echo "Sessions: $(find ~/.openclaw/agents/main/sessions -name '*.jsonl' 2>/dev/null | wc -l) files"
echo "Config: $(test -f ~/.openclaw/openclaw.json && echo 'EXISTS' || echo 'MISSING')"
echo "==========================="
```

---

*Next: [FAQ →](02-faq.md)*
