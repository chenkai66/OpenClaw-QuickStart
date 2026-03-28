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

## 重要说明：MCPorter

OpenClaw **原生不直接支持 MCP 协议**，官方推荐通过 **MCPorter** 作为中间层来调用 MCP 服务器。

### 安装 MCPorter

```bash
# 安装 MCPorter
npm i mcporter

# 需要 uvx（Python 工具链）
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc
```

### 添加 MCP 服务器

```bash
# 添加 GitHub MCP
npx mcporter config add mcp-server-github \
  --stdio "uvx mcp-server-github" \
  --env GITHUB_TOKEN=ghp_xxx

# 添加自定义 MCP（如 TAPD）
npx mcporter config add mcp-server-tapd \
  --stdio "uvx mcp-server-tapd" \
  --env TAPD_ACCESS_TOKEN=xxxx
```

### 查看已配置的 MCP

```bash
mcporter list
```

> 💡 **使用技巧**：第一次可以明确告诉 AI "用 mcporter 下的 xxx 工具"，AI 记住后，以后可以直接自然语言调用。

---

## 常用 MCP 服务器推荐

| MCP 服务器 | 功能 | 安装命令 |
|------------|------|---------|
| **GitHub** | Git/PR/Issue 操作 | `npx mcporter config add mcp-server-github` |
| **Filesystem** | 文件系统读写 | `npx mcporter config add mcp-server-filesystem` |
| **Fetch** | 网页内容抓取 | `npx mcporter config add mcp-server-fetch` |
| **Playwright** | 浏览器自动化 | `npx mcporter config add mcp-server-playwright` |
| **PostgreSQL** | 数据库查询 | `npx mcporter config add mcp-server-postgres` |
| **Slack** | Slack 消息读写 | `npx mcporter config add mcp-server-slack` |
| **Google Drive** | 文档管理 | `npx mcporter config add mcp-server-gdrive` |
| **小红书** | 小红书内容操作 | `npx mcporter config add xiaohongshu-mcp` |

> 💡 **MCP 目录**：[mcp.so](https://mcp.so) 收录了超过 **1.8 万个**第三方 MCP 服务器，覆盖数据源、开发工具、生产力工具等。

---

## 配置示例

```json
// MCPorter 配置文件位于 ~/config/mcporter.json
// 也可以在 openclaw.json 中通过 skills 方式集成
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


---

## 实战示例

### 用 GitHub MCP 自动审查 PR

```
帮我审查 GitHub 上 openclaw/openclaw 仓库的最新 PR，
重点看代码质量和安全问题。
```

### 用 Fetch MCP 抓取网页

```
用 fetch 工具抓取 https://news.ycombinator.com 首页，
整理出今天排名前 10 的新闻。
```

### 用数据库 MCP 查询数据

```
连接 PostgreSQL 数据库，查询 users 表中
最近 7 天注册的用户数量。
```

---

## 下一节

👉 [Agent 设计哲学与最佳实践](06-best-practices.md) — 知识体系、记忆分层、协作协议的设计原则
