# Chapter 1.2: Installation Guide

> Based on real-world deployment experience on Alibaba Cloud ECS

## System Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| OS | Ubuntu 20.04+ / Debian 10+ / CentOS 8+ | Ubuntu 22.04 LTS |
| Node.js | v18.0.0+ | v22.x (LTS) |
| RAM | 1 GB | 2 GB+ |
| Disk | 2 GB free | 5 GB+ |
| Network | Public internet access | Access to idealab.alibaba-inc.com |

## Step 1: Install Node.js

OpenClaw requires Node.js 18+ (recommended 22.x).

### Ubuntu / Debian

```bash
# Download and run NodeSource installation script
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -

# Install Node.js and npm
sudo apt-get install -y nodejs

# Verify installation
node --version  # Should show v22.x.x
npm --version   # Should show 10.x.x
```

### CentOS / RHEL / Alibaba Cloud Linux

```bash
curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -
sudo yum install -y nodejs
```

### Using nvm (Universal)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 22
```

### macOS

```bash
# Using Homebrew
brew install node@22

# Or using the official installer
# Download from https://nodejs.org/
```

## Step 2: Install OpenClaw

```bash
# Install OpenClaw globally via npm
npm install -g openclaw

# Verify installation (may take 2-3 minutes on first run)
openclaw --version  # Should show 2026.3.1 or higher
```

**Important notes:**
- The OpenClaw package is large (600+ dependencies), installation takes 2-3 minutes
- If installation fails (version shows empty), uninstall and reinstall:
  ```bash
  npm uninstall -g openclaw
  npm install -g openclaw@latest
  ```
- Gateway installation is optional for basic functionality

### Create Configuration Directory

```bash
mkdir -p ~/.openclaw
chmod 700 ~/.openclaw
```

## Step 3: Install Claude Code (Optional, for Claude integration)

If you plan to use Claude models with OpenClaw:

```bash
# Install Claude Code CLI (must use official package name!)
npm install -g @anthropic-ai/claude-code

# Verify installation
claude --version  # Should show 2.1.63 or higher

# Create configuration directory
mkdir -p ~/.claude
```

> **Warning**: You MUST use `@anthropic-ai/claude-code` as the package name, NOT `claude-code`. The wrong name installs a fake package.
> Official package: https://www.npmjs.com/package/@anthropic-ai/claude-code

## Step 4: Deploy Dashscope Proxy (For China Region)

If you are accessing AI models through Alibaba Cloud Idealab API, you need a proxy server.

### Why the Proxy?

```
OpenClaw/Claude Code → Dashscope Proxy (localhost:8080) → Idealab API
```

The Idealab API responses lack the required `id` field. The proxy automatically adds IDs and handles gzip compression and SSE streaming.

### Create Proxy Script

Create file `/root/dashscope-proxy.js`:

```bash
cat > /root/dashscope-proxy.js << 'EOF'
#!/usr/bin/env node

/**
 * Dashscope Proxy for Linux - Universal JSON/SSE/Gzip Support
 * Handles both streaming SSE and non-streaming JSON responses
 */

const http = require("http");
const https = require("https");
const zlib = require("zlib");
const { randomUUID } = require("crypto");

const PORT = 8080;
const TARGET_HOST = "idealab.alibaba-inc.com";
const TARGET_PATH_PREFIX = "/api/code";

const server = http.createServer((req, res) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);

  if (!req.url.startsWith("/idealab")) {
    res.writeHead(404, { "Content-Type": "text/plain" });
    res.end("Not Found");
    return;
  }

  const targetPath = req.url.replace("/idealab", TARGET_PATH_PREFIX);

  const options = {
    hostname: TARGET_HOST,
    path: targetPath,
    method: req.method,
    headers: { ...req.headers, host: TARGET_HOST },
  };

  const proxyReq = https.request(options, (proxyRes) => {
    const isGzipped = proxyRes.headers["content-encoding"] === "gzip";
    const contentType = proxyRes.headers["content-type"] || "";
    const isSSE = contentType.includes("text/event-stream");

    let stream = proxyRes;
    if (isGzipped) {
      stream = proxyRes.pipe(zlib.createGunzip());
    }

    const messageId = `msg_${randomUUID().replace(/-/g, "")}`;
    let chunks = [];
    let isFirstChunk = true;

    stream.on("data", (chunk) => {
      const chunkStr = chunk.toString();

      if (isSSE || chunkStr.includes("data: {")) {
        if (!res.headersSent) {
          const responseHeaders = { ...proxyRes.headers };
          delete responseHeaders["content-length"];
          delete responseHeaders["transfer-encoding"];
          if (isGzipped) delete responseHeaders["content-encoding"];
          res.writeHead(proxyRes.statusCode, responseHeaders);
        }

        if (chunkStr.includes("data: {") && isFirstChunk) {
          try {
            const lines = chunkStr.split("\n");
            const modifiedLines = lines.map((line) => {
              if (line.startsWith("data: {")) {
                const jsonData = JSON.parse(line.substring(6));
                if (!jsonData.id && jsonData.message) {
                  jsonData.message.id = messageId;
                }
                return "data: " + JSON.stringify(jsonData);
              }
              return line;
            });
            res.write(modifiedLines.join("\n"));
            isFirstChunk = false;
          } catch (e) {
            res.write(chunk);
          }
        } else {
          res.write(chunk);
        }
      } else {
        chunks.push(chunk);
      }
    });

    stream.on("end", () => {
      if (chunks.length > 0) {
        const buffer = Buffer.concat(chunks);
        const data = buffer.toString();
        try {
          const jsonData = JSON.parse(data);
          if (jsonData && !jsonData.id) jsonData.id = messageId;
          const modifiedData = JSON.stringify(jsonData);
          const responseHeaders = { ...proxyRes.headers };
          delete responseHeaders["content-length"];
          delete responseHeaders["transfer-encoding"];
          if (isGzipped) delete responseHeaders["content-encoding"];
          responseHeaders["content-length"] = Buffer.byteLength(modifiedData);
          res.writeHead(proxyRes.statusCode, responseHeaders);
          res.end(modifiedData);
        } catch (e) {
          const responseHeaders = { ...proxyRes.headers };
          delete responseHeaders["content-length"];
          if (isGzipped) delete responseHeaders["content-encoding"];
          responseHeaders["content-length"] = Buffer.byteLength(data);
          res.writeHead(proxyRes.statusCode, responseHeaders);
          res.end(data);
        }
      } else {
        res.end();
      }
    });

    stream.on("error", (err) => {
      console.error("Stream error:", err);
      if (!res.headersSent) res.writeHead(500);
      res.end("Stream Error");
    });
  });

  proxyReq.on("error", (err) => {
    console.error("Proxy error:", err);
    res.writeHead(500, { "Content-Type": "text/plain" });
    res.end("Proxy Error");
  });

  req.on("data", (chunk) => proxyReq.write(chunk));
  req.on("end", () => proxyReq.end());
});

server.listen(PORT, "127.0.0.1", () => {
  console.log(`Proxy running on http://127.0.0.1:${PORT}/idealab`);
});
EOF
chmod +x /root/dashscope-proxy.js
```

### Set Up as systemd Service

```bash
cat > /etc/systemd/system/dashscope-proxy.service << 'EOF'
[Unit]
Description=Dashscope Proxy Server
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/root
ExecStart=/usr/bin/node /root/dashscope-proxy.js
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable dashscope-proxy
systemctl start dashscope-proxy

# Verify
systemctl status dashscope-proxy
```

### Service Management Commands

```bash
systemctl start dashscope-proxy      # Start
systemctl stop dashscope-proxy       # Stop
systemctl restart dashscope-proxy    # Restart
journalctl -u dashscope-proxy -f     # View live logs
journalctl -u dashscope-proxy -n 50  # View last 50 log entries
```

## Step 5: Set Up OpenClaw Gateway Service

```bash
# Set gateway mode
openclaw config set gateway.mode local

# Create necessary directories
mkdir -p ~/.openclaw/agents/main/sessions
chmod 700 ~/.openclaw

# Create systemd service
cat > /etc/systemd/system/openclaw-gateway.service << 'EOF'
[Unit]
Description=OpenClaw Gateway Service
After=network.target dashscope-proxy.service

[Service]
Type=simple
User=root
WorkingDirectory=/root
ExecStart=/usr/bin/openclaw gateway
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable openclaw-gateway
systemctl start openclaw-gateway
systemctl status openclaw-gateway
```

**Important:**
- Confirm OpenClaw path: `which openclaw`
- If path is not `/usr/bin/openclaw`, update the `ExecStart` in the service file
- Gateway listens on `ws://127.0.0.1:18789`

## Step 6: Verify Everything

### Quick Verification Script

```bash
#!/bin/bash
echo "=== OpenClaw Installation Verification ==="

# 1. Check Node.js
echo -n "Node.js: "
node --version 2>/dev/null || echo "NOT INSTALLED"

# 2. Check OpenClaw
echo -n "OpenClaw: "
openclaw --version 2>/dev/null || echo "NOT INSTALLED"

# 3. Check Claude Code
echo -n "Claude Code: "
claude --version 2>/dev/null || echo "NOT INSTALLED (optional)"

# 4. Check Proxy
echo -n "Dashscope Proxy: "
systemctl is-active dashscope-proxy 2>/dev/null || echo "NOT RUNNING"

# 5. Check Gateway
echo -n "OpenClaw Gateway: "
systemctl is-active openclaw-gateway 2>/dev/null || echo "NOT RUNNING"

# 6. Check port 8080
echo -n "Proxy Port 8080: "
ss -tlnp | grep -q 8080 && echo "LISTENING" || echo "NOT LISTENING"

# 7. Check port 18789
echo -n "Gateway Port 18789: "
ss -tlnp | grep -q 18789 && echo "LISTENING" || echo "NOT LISTENING"

echo "=== Verification Complete ==="
```

### Test API Connection

```bash
node -e "
const http = require(http);
const data = JSON.stringify({
  model: claude_sonnet4_5,
  max_tokens: 100,
  messages: [{role: user, content: Reply with just: test ok}]
});
const req = http.request({
  hostname: 127.0.0.1, port: 8080,
  path: /idealab/v1/messages, method: POST,
  headers: {
    Content-Type: application/json,
    x-api-key: YOUR_API_KEY,
    anthropic-version: 2023-06-01,
    Content-Length: data.length
  }
}, (res) => {
  let body = ;
  res.on(data, c => body += c);
  res.on(end, () => {
    const json = JSON.parse(body);
    console.log(Status:, res.statusCode);
    console.log(Has ID:, !!json.id);
    console.log(Response OK!);
  });
});
req.write(data);
req.end();
"
```

## Deployment Time Reference

| Step | Estimated Time |
|------|---------------|
| Node.js installation | ~2 minutes |
| Claude Code installation | ~5 seconds |
| OpenClaw installation | ~2-3 minutes |
| Proxy deployment | ~30 seconds |
| systemd configuration | ~1 minute |
| Config file creation | ~30 seconds |
| Testing & verification | ~1 minute |
| **Total** | **~7-10 minutes** |

## Deployment Checklist

**Infrastructure:**
- [ ] Node.js >= 22.0.0 (`node --version`)
- [ ] npm works (`npm --version`)

**Software:**
- [ ] OpenClaw installed (`openclaw --version` shows 2026.3.1+)
- [ ] Claude Code installed (`claude --version` shows 2.1.63+)

**Services:**
- [ ] Proxy running (`systemctl status dashscope-proxy` shows active)
- [ ] Proxy listening on 8080 (`ss -tlnp | grep 8080`)
- [ ] Gateway running (`systemctl status openclaw-gateway` shows active)
- [ ] Auto-start enabled (`systemctl is-enabled dashscope-proxy`)

**Configuration:**
- [ ] Claude config exists (`ls ~/.claude/settings.json`)
- [ ] OpenClaw config exists (`ls ~/.openclaw/openclaw.json`)
- [ ] API Key correctly configured
- [ ] BASE_URL correctly set

**Security:**
- [ ] Config file permissions correct (`chmod 600`)
- [ ] Proxy only listens on 127.0.0.1 (not exposed externally)

---

*Next: [First Chat with OpenClaw →](03-first-chat.md)*
