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
