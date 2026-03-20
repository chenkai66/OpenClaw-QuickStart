# 第1章·第4节：Web 控制面板

> 在浏览器中管理 OpenClaw，更直观的操作界面。

---

## 打开 Dashboard

```bash
openclaw dashboard
```

这会在浏览器中打开控制面板，默认地址：`http://127.0.0.1:18789/`

![Web Dashboard 控制面板](https://datawhalechina.github.io/hello-claw/assets/openclaw-dashboard-browser.CS3vL0pY.png)

---

## 远程服务器访问

如果 OpenClaw 部署在远程服务器上，使用 SSH 隧道访问：

```bash
# 在本地终端执行
ssh -N -L 18789:127.0.0.1:18789 用户名@服务器IP

# 然后在本地浏览器打开 http://127.0.0.1:18789/
```

---

## Dashboard 功能一览

| 功能区 | 说明 |
|--------|------|
| **Config** | 可视化编辑配置，保存即生效 |
| **Conversations** | 查看对话历史、工具调用记录 |
| **Channels** | 查看已连接渠道状态 |
| **Sessions** | 管理活跃会话和上下文 |
| **Skills** | 浏览和管理已安装技能 |
| **Cron** | 查看和管理定时任务 |
| **Logs** | 实时查看 Gateway 日志 |

> 发送 `/status` 可快速检查 Gateway 状态

### 聊天界面
- 直接在浏览器中和 OpenClaw 对话
- 支持发送文件和图片
- 实时查看 AI 回复

### 会话管理
- 查看所有活跃会话
- 切换不同会话
- 查看会话历史

### 配置管理
- 可视化修改配置
- 管理 API Key
- 开关 Tools 和 Skills

### 节点管理
- 查看连接的移动设备
- 管理 iOS/Android 节点


---

## Dashboard 认证

如果 Gateway 配了认证，打开 Dashboard 时需要凭证：

```json
{
  "gateway": {
    "auth": {
      "mode": "token",
      "token": "${OPENCLAW_GATEWAY_TOKEN}",
      "allowTailscale": true
    }
  }
}
```

---

## 修改端口

```bash
openclaw gateway --port 19000
```

改完后地址变为 `http://localhost:19000`。


---

## 安全注意事项

> ⚠️ Dashboard 默认不设密码，仅监听 localhost
> 
> **不要直接将 18789 端口暴露到公网！**

安全的远程访问方式：
1. **SSH 隧道**（推荐）：如上所述
2. **Tailscale**：通过 VPN 网络访问
3. **Nginx 反向代理 + Basic Auth**：加密码保护

---

## 下一章

👉 [第2章：核心概念](../02-core-concepts/01-architecture.md) — 深入理解 OpenClaw 的工作原理
