# 第5章·第3节：技能管理

> 从 ClawHub 安装社区技能，管理和更新你的技能库。

---

## ClawHub 技能市场

[ClawHub](https://clawhub.com) 是 OpenClaw 官方技能市场，截至 2026 年 3 月已收录 **17000+** 社区技能。

### 安装 ClawHub CLI

```bash
npm i -g clawhub --registry https://registry.npmmirror.com
```

---

## 基本操作

### 搜索技能

```bash
clawhub search "日历"
clawhub search "邮件处理"
clawhub search "home assistant"
```

### 安装技能

```bash
# 安装单个技能
clawhub install browser

# 批量安装
clawhub install browser brave-search shell cron

# 从 GitHub 安装
openclaw plugins install https://github.com/xxx/xxx.git
```

### 更新技能

```bash
# 更新单个技能
clawhub update browser

# 批量更新所有
clawhub update --all
```

### 删除技能

```bash
clawhub uninstall twitter
```

---

## 懒人操作：自然语言安装

直接在对话中告诉 AI：

```
帮我安装 Gmail 和 Notion 技能
```

```
搜索并安装智能家居相关的 Skill
```

```
更新所有已安装的技能到最新版本
```

AI 会自动执行对应的命令。

---

## 安全注意事项

1. **来源验证**：优先选 ClawHub 官方认证的技能，下载量 1000+ 的更可靠
2. **权限管控**：Browser、Shell 等高危技能建议开启"确认模式"
3. **授权谨慎**：涉及 Gmail、GitHub 等账号的技能，只勾选必要权限
4. **定期更新**：每周跑一次 `clawhub update --all`，修复已知漏洞
5. **查看源码**：陌生技能安装前查看 `SKILL.md` 文件，确认无高风险操作

---

## 技能文件位置

```
~/.openclaw/skills/         # 用户安装的 Skills
~/.openclaw/plugins/        # 插件
```

---

## 下一节

👉 [04-自定义 Skills](04-custom-skills.md) — 创建你自己的技能
