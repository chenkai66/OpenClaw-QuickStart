# 第7章：实战项目与应用案例

> 从社区最火的项目中学习，把 OpenClaw 变成你的效率引擎。

---

## 为什么需要实战项目？

看完前 6 章，你已经掌握了 OpenClaw 的核心能力。但理论和实践之间隔着一条河——这一章就是桥。

我们从社区 5000+ 个 Skill、GitHub 上数百个开源项目、Reddit/Discord 上的真实分享中，精选了 **5 个最受欢迎、最实用的应用方向**，每个都配有完整的配置、提示词和踩坑经验。

---

## 本章内容

| 节 | 项目 | 难度 | 预计搭建时间 |
|----|------|------|-------------|
| 7.2 | [第二大脑知识系统](02-second-brain.md) | ⭐⭐⭐ | 2 小时 |
| 7.3 | [每日简报与健康监控](03-daily-briefing.md) | ⭐ | 30 分钟 |
| 7.4 | [开发运维自动化](04-devops-automation.md) | ⭐⭐ | 1 小时 |
| 7.5 | [内容研究与写作流水线](05-content-pipeline.md) | ⭐⭐ | 1 小时 |
| 7.6 | [个人智能助理](06-smart-assistant.md) | ⭐⭐ | 1 小时 |

---

## 通用前置条件

所有项目都需要：

1. **OpenClaw 已安装并运行**（第1章）
2. **至少一个 AI 模型 API 配好**（推荐 DashScope qwen3.5-plus）
3. **至少一个消息渠道**（TUI / 钉钉 / Telegram / Discord 任选）

推荐配置：

```json
{
  "agents": {
    "defaults": {
      "model": { "primary": "dashscope/qwen3.5-plus" },
      "compaction": {
        "memoryFlush": { "enabled": true }
      }
    }
  }
}
```

---

## 项目灵感来源

这些项目来自社区的真实实践：

- [awesome-openclaw-skills](https://github.com/VoltAgent/awesome-openclaw-skills) — 5400+ 社区 Skills 精选
- [awesome-openclaw](https://github.com/rylena/awesome-openclaw) — 社区资源汇总
- [ClawHub 技能市场](https://clawhub.com) — 官方技能市场
- OpenClaw Discord / Reddit 社区的真实分享

> 如果你有自己的项目想分享，欢迎提交 PR！

---

*下一节：[第二大脑知识系统 →](02-second-brain.md)*
