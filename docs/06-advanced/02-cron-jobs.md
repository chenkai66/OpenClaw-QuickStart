# 第6章·第2节：定时任务（Cron）

> 让 AI 自动干活 — 每天给你发简报、每周生成报告。

---

## 启用 Cron

```json
{
  "tools": {
    "cron": { "enabled": true },
    "message": { "enabled": true }
  }
}
```

---

## 设置定时任务

### 方式一：对话中设置

直接告诉 OpenClaw：

```
请每天早上 7:00 给我发一条每日简报，包括：
1. 今天的天气
2. 今天的待办事项
3. 重要新闻摘要
```

### 方式二：HEARTBEAT.md 配置

```markdown
# Heartbeat

## Daily Brief
- Time: 07:00 every day
- Action: Check weather, calendar, and news
- Deliver: Send summary via Telegram/DingTalk

## Weekly Report
- Time: 17:00 every Friday
- Action: Summarize this week's activities
- Deliver: Send report via DingTalk
```

---

## 经典场景

### 每日简报
```
每天 6:47 通过 Telegram 发送：
- 今天日程
- 待回复消息
- 天气预报
```

### 代码仓库监控
```
每小时检查 GitHub 仓库：
- 新的 PR 和 Issue
- CI/CD 状态
- 依赖安全警告
```

### 竞品监控
```
每天检查竞品网站变化：
- 新功能发布
- 价格变动
- 博客更新
```

---

## 管理定时任务

```
/cron list     — 查看所有定时任务
/cron delete   — 删除定时任务
```

---

## Heartbeat vs Cron

很多人分不清这两个，一句话概括：

> **Heartbeat = 巡逻员**（按节奏巡检，有事才提醒）
> **Cron = 闹钟**（到点就执行，不管有没有事）

| 对比 | Heartbeat | Cron |
|------|-----------|------|
| 触发方式 | 固定间隔（如每 30 分钟） | 精确时间（如每天 8:30） |
| 执行条件 | 有事才行动，没事返回 `HEARTBEAT_OK` | 到点必执行 |
| 适合场景 | 监控异常、检查遗漏 | 定时推送、周期性任务 |
| 会话隔离 | 使用最近活跃的渠道 | 独立会话 |

**最佳实践**：两者结合使用：
- Heartbeat 30-60 分钟巡检一次，抓异常
- Cron 每天 2-3 个关键节点，保底提醒

---

## Heartbeat 配置

```json5
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",           // 每 30 分钟巡检一次
        target: "last",         // 提醒发到最近聊天的渠道
        prompt: "Read HEARTBEAT.md if it exists. Follow it strictly. If nothing needs attention, reply HEARTBEAT_OK.",
        activeHours: {
          start: "09:00",       // 只在白天工作
          end: "22:00",
          timezone: "Asia/Shanghai",
        },
      },
    },
  },
}
```

---

## Cron CLI 命令

```bash
# 添加定时任务（每天 8:30 推送日报）
openclaw cron add \
  --name "每日简报" \
  --cron "30 8 * * *" \
  --tz "Asia/Shanghai" \
  --session main \
  --system-event "请生成今日简报" \
  --wake now

# 添加一次性任务（20 分钟后提醒）
openclaw cron add \
  --name "开会提醒" \
  --at "20m" \
  --session main \
  --system-event "20分钟到了，准备开会"

# 查看所有任务
openclaw cron list

# 删除任务
openclaw cron delete --name "每日简报"
```

> 💡 Cron 任务持久化到磁盘，Gateway 重启不会丢失。
