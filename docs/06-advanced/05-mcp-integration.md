# 第6章·第5节：MCP 协议集成

> Model Context Protocol — 让 OpenClaw 连接更多工具。

---

## 什么是 MCP

MCP（Model Context Protocol）是 Anthropic 提出的协议，让 AI Agent 能连接外部工具和数据源。

OpenClaw 支持 MCP，可以通过安装 MCP 服务器来扩展能力。

---

## 安装 MCP Skill

```bash
# 通过 ClawHub 安装
openclaw skills install mcp-client

# 或直接配置
```

---

## 常用 MCP 集成

| MCP 服务器 | 功能 |
|------------|------|
| GitHub MCP | Git/GitHub 操作 |
| Database MCP | 数据库查询 |
| Filesystem MCP | 文件系统操作 |
| Fetch MCP | 网页抓取 |
| Playwright MCP | 浏览器自动化 |
| Slack MCP | Slack 集成 |

---

## 配置示例

```json
{
  "mcp": {
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_TOKEN": "ghp_xxx"
        }
      }
    }
  }
}
```
