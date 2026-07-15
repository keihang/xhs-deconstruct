---
name: xhs-deconstruct
description: 拆解小红书博主，分析指定时间段内流量最好的10篇图文帖子（点赞>100），提炼标题公式、封面设计、内容结构等可复用模式。视频帖子不计入，自动跳过。触发方式：/xhs-deconstruct 拆解 博主名 或 /xhs-deconstruct 拆解 链接。
version: 3.0
author: 阿恒
---

# 小红书博主拆解

分析小红书博主的爆款帖，提炼可复用的内容模式。

## 前置条件

安装 [OpenCLI](https://github.com/jackwener/OpenCLI)（把网站变成 CLI，让 AI agent 操作你的浏览器）：

```bash
# 方式A：下载安装包 → https://opencli.info/download
# 方式B：npm 全局安装（Node.js >= 20）
npm install -g @jackwener/opencli

# 安装 Browser Bridge 扩展（Chrome Web Store 搜索 OpenCLI）
# 验证连接
opencli doctor
# 安装小红书适配器
opencli install xiaohongshu
```

## 目标

拆解小红书博主，分析每位博主指定时间段内**流量最好的10篇图文帖子**（点赞>100），提炼标题公式、内容结构、封面设计等可复用模式。视频帖子不计入，自动跳过。

## 工作流程

**博主锁定方式（二选一，都用 `/xhs-deconstruct` 触发）**：
- **方式A**：`/xhs-deconstruct 拆解 博主名字` → AI 搜索博主名字
- **方式B**：`/xhs-deconstruct 拆解 链接` → AI 直接用链接获取数据

### 第1步：锁定博主

```bash
# 方式A：搜索博主名字
opencli xiaohongshu search "博主名字" --limit 20

# 方式B：直接用链接获取数据（必须加 --limit 50，否则默认只返回15篇）
opencli xiaohongshu user "用户发的链接" --limit 50
```

**⚠️ 风控注意**：每次操作间隔**15秒**。如果触发风控（返回空结果或"安全验证"），降到 `--limit 30` 重试，或等5-10分钟后再试。

### 第2步：确认时间段

🔴 **CHECKPOINT：必须先问用户再继续。**

询问用户要抓取的时间段：一个月 / 半年 / 一年。

### 第3步：筛选帖子（图文优先）

**目标**：找到该时间段内，点赞>100的**图文帖子**，取前10篇。

**筛选流程**：
1. 按时间筛选：只保留用户选择时间段内的帖子
2. 按类型筛选：只保留图文帖子，跳过视频
3. 按点赞筛选：只保留点赞>100的帖子
4. 排序取前10：按点赞数从高到低

**不足10篇时**：有多少抓多少，如实告知用户。低于3篇可降低门槛到点赞>50。

🔴 **CHECKPOINT：筛选完成后，向用户确认筛选结果再继续。**

### 第4步：读取帖子内容并下载原图

```bash
# 读取帖子内容
opencli xiaohongshu note "搜索结果URL" --format json > raw-data/posts/博主名/01-标题.json

# 下载所有图片（不只是封面）
opencli xiaohongshu download "搜索结果URL" --output bloggers/01-博主名/原文/image/01-标题/
```

**关键**：
- 必须用搜索结果URL（带xsec_token），explore URL 会触发安全验证
- 下载时观察文件格式：`.mp4` = 视频跳过，`.jpg/.png` = 图文继续
- 读取间隔**15秒**，连续3篇失败则停止，等10分钟

### 第5步：写概览文件

读取 `references/overview-template.md`，为当前博主创建 `概览.md`。

### 第6步：写原文文件

读取 `references/original-template.md`，每篇帖子创建原文文件。

🔴 **CHECKPOINT：下载完成后，确认图片数量和格式再写文件。**

### 第7步：写拆解分析

读取 `references/deconstruct-template.md`，每篇帖子按7个维度拆解。

### 第8步：写总结

🔴 **CHECKPOINT：所有拆解完成后，先向用户展示拆解概况，确认无遗漏再写总结。**

读取 `references/summary-template.md`，分析所有拆解文件，提炼共性模式。

### 第9步：沉淀发现

把数据验证的模式更新到 `gzh-to-xhs` skill 中：
- 标题公式 → 更新到标题部分
- 内容结构 → 更新到正文部分
- 封面设计 → 更新到封面部分
- 新模式 → 创建新的参考文档

**原则**：所有更新都要有数据支撑，不是"我觉得"而是"数据证明"。

## 输出结构

```
xhs-work/
├── bloggers/
│   ├── 01-博主名/
│   │   ├── 概览.md              # 博主基本信息、内容定位、Top 10 数据表
│   │   ├── 总结.md              # 该博主的拆解总结
│   │   ├── 原文/                 # 笔记原文 + 所有图片
│   │   │   ├── 01-标题.md       # 原文文件（含 frontmatter 元数据）
│   │   │   ├── image/           # 该博主所有帖子的图片
│   │   │   │   ├── 01-标题.jpg  # 封面
│   │   │   │   ├── 01-标题_2.jpg # 多图帖子的第2张
│   │   │   │   └── ...
│   │   │   └── ...
│   │   └── 拆解/                 # 每篇笔记的7维度拆解分析
│   │       ├── 01-标题.md
│   │       └── ...
│   └── 02-博主名/
│       └── ...（同结构）
├── raw-data/                    # 原始 JSON 数据（中间产物，可删除）
│   ├── posts/                   # 搜索结果 JSON
│   └── notes/                   # 帖子详情 JSON
└── CLAUDE.md
```

## 注意事项

- 文件命名：用连字符（-）不用空格，序号用两位数（01、02、...）
- 避免命名冲突：先检查已有博主目录，确保序号不重复
- 风控：搜索/读取间隔15秒，返回空数据等几分钟再继续

## 参考文件

完成拆解后，读取以下文件进行自检和排错：

- `references/do-nots.md` — 不要做什么（反例清单）
- `references/quality-checklist.md` — 质量自检清单
- `references/faq.md` — 常见问题
