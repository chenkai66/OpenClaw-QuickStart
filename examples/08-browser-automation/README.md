# 案例8：浏览器自动化

## 目标
使用 OpenClaw 的 browser 工具自动化网页操作。

## 前提
启用 browser 工具：
```json
{
  "tools": {
    "browser": { "enabled": true }
  }
}
```

## 场景1：价格比较

```
帮我在京东和淘宝上搜索 "AirPods Pro 2"，
比较价格，列出 top 3 最便宜的选项。
```

## 场景2：填写表单

```
帮我打开 https://forms.example.com/survey
按照以下信息填写：
- 姓名：张三
- 邮箱：zhangsan@example.com
- 部门：技术部
```

## 场景3：网页截图

```
帮我打开 https://github.com/openclaw/openclaw
截图保存到 ~/screenshots/openclaw-github.png
```

## 场景4：数据提取

```
帮我打开 https://news.ycombinator.com/
提取前 10 条新闻的标题和链接，
保存为 CSV 文件。
```

## 安全提醒
- 不要让 AI 操作涉及支付的页面
- 不要在 browser 中输入敏感密码
- 建议开启 exec 审批模式
