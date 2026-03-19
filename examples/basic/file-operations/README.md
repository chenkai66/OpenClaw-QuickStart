# 文件操作进阶

学习使用 OpenClaw 进行复杂的文件操作。

## 示例 1: 批量文件处理

```
You: 请找出当前目录下所有 .txt 文件，并统计每个文件的行数

OpenClaw: 好的，让我执行这个任务。

[执行命令]
find . -name "*.txt" -type f -exec wc -l {} \; -print

结果：
- test.txt: 1 行
- notes.txt: 25 行
- readme.txt: 150 行

总共找到 3 个文本文件。
```

## 示例 2: 文件内容搜索

```
You: 请在所有 .md 文件中搜索包含 "OpenClaw" 的内容

OpenClaw: 让我搜索所有 Markdown 文件。

[执行命令]
grep -r "OpenClaw" --include="*.md"

找到 5 处匹配：
1. README.md:12 - "OpenClaw 是一个..."
2. docs/intro.md:5 - "安装 OpenClaw"
...
```

## 示例 3: 自动化文件整理

```
You: 请帮我整理 Downloads 文件夹，把所有图片移动到 Images 子文件夹

OpenClaw: 好的，让我整理文件。

[规划]
1. 创建 Downloads/Images 目录
2. 查找所有图片文件（.jpg, .png, .gif等）
3. 移动到 Images 目录

[执行]
mkdir -p ~/Downloads/Images
find ~/Downloads -maxdepth 1 -type f \( -name "*.jpg" -o -name "*.png" -o -name "*.gif" \) -exec mv {} ~/Downloads/Images/ \;

✅ 完成！已将 23 个图片文件移动到 Images 文件夹。
```

## 脚本示例

创建 `file-organizer.sh`：

```bash
#!/bin/bash
# 自动文件整理脚本

# 使用 OpenClaw 执行
openclaw tui << 'EOFCLAW'
请帮我执行以下任务：
1. 在 ~/Downloads 创建以下文件夹：Images, Documents, Videos, Archives
2. 根据文件扩展名自动分类：
   - .jpg, .png, .gif → Images
   - .pdf, .doc, .txt → Documents  
   - .mp4, .avi, .mkv → Videos
   - .zip, .tar, .gz → Archives
3. 移动文件到对应文件夹
4. 生成整理报告
EOFCLAW
```

运行：
```bash
chmod +x file-organizer.sh
./file-organizer.sh
```

## 下一步

- [API 调用](../api-calls/README.md)
- [Skills 基础](../../intermediate/skills-basic/README.md)
