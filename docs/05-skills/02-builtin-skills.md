# 第5章·第2节：内置 Skills 详解

> OpenClaw 自带 53 个官方 Skills，覆盖笔记、办公、开发、社交等场景。

---

## 什么是内置 Skills

内置 Skills 是 OpenClaw 安装后就自带的技能，不需要额外安装。它们分为以下几大类：

---

## 核心系统类

| Skill | 功能 | 说明 |
|-------|------|------|
| **AGENTS.md** | Agent 基线规则 | 定义 AI 能做什么、不能做什么 |
| **SOUL.md** | 人格指导 | 控制 AI 说话风格和语气 |
| **summarize** | 总结文本 | 长文章、会议记录一键摘要 |
| **search** | 信息搜索 | 联网搜索最新内容 |
| **skill-creator** | 技能创建器 | 把重复流程封装成新 Skill |

---

## 笔记和知识管理

| Skill | 功能 | 适配工具 |
|-------|------|---------|
| **obsidian** | 笔记管理 | Obsidian vault |
| **notion** | 知识库 | Notion workspace |
| **apple-notes** | 苹果备忘录 | macOS / iOS |
| **todoist** | 任务管理 | Todoist |
| **logseq** | 双链笔记 | Logseq |

---

## 办公和通讯

| Skill | 功能 | 说明 |
|-------|------|------|
| **gmail** | 邮件管理 | 读写邮件、自动回复 |
| **google-calendar** | 日程管理 | 智能排会议、提醒 |
| **gog** | Google 全家桶 | Drive/Docs/Sheets 操作 |
| **slack** | Slack 集成 | 频道消息管理 |
| **linear** | 项目管理 | Issue/Sprint 管理 |

---

## 开发工具

| Skill | 功能 | 说明 |
|-------|------|------|
| **github** | 代码仓库 | PR、Issue、代码审查 |
| **git** | Git 操作 | 本地 Git 命令封装 |
| **docker** | 容器管理 | Docker 命令封装 |
| **vitest** | 单元测试 | 运行 Vitest 测试 |
| **android-emulator** | 安卓模拟器 | 移动端调试 |

---

## 媒体和社交

| Skill | 功能 | 说明 |
|-------|------|------|
| **spotify** | 音乐控制 | 播放/暂停/切歌 |
| **twitter** | 社媒运营 | 发推/监控/摘要 |
| **youtube** | 视频管理 | 字幕提取、内容摘要 |
| **image-generation** | 图片生成 | AI 绘画 |
| **tts** | 文字转语音 | 文本朗读 |

---

## 智能家居和生活

| Skill | 功能 | 说明 |
|-------|------|------|
| **home-assistant** | 智能家居 | 灯/空调/窗帘控制 |
| **weather** | 天气查询 | 实时天气和预报 |
| **travel-planner** | 旅行规划 | 行程安排 |
| **finance-data** | 财经信息 | 股票/基金/汇率 |

---

## 浏览器和自动化

| Skill | 功能 | 说明 |
|-------|------|------|
| **browser** | 网页自动化 | 打开网页/填表/截图 |
| **computer-use** | 电脑操控 | 鼠标/键盘操作 |
| **cron** | 定时任务 | 定时执行/提醒 |
| **nodes** | 手机联动 | 截图/录屏/定位 |

---

## 启用和禁用 Skills

Skills 不是全部默认启用的，OpenClaw 会根据对话上下文**智能加载**需要的 Skill。

手动管理：

```bash
# 查看已启用的 Skills
/skills

# 在对话中临时启用
/skill enable obsidian

# 在对话中临时禁用
/skill disable twitter
```

或在配置文件中：

```json
{
  "skills": {
    "entries": {
      "obsidian": { "enabled": true },
      "twitter": { "enabled": false }
    }
  }
}
```

---

## 下一节

👉 [03-技能管理](03-skill-management.md) — 安装、更新、删除技能
