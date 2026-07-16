---
name: xhs-deconstruct
description: 拆解小红书博主，默认分析最近半年的图文帖子，按点赞排序筛出前20篇图文候选，抓取其中最多10篇做拆解，提炼标题公式、封面设计、内容结构等可复用模式。视频帖子不进入候选池。触发方式：/xhs-deconstruct 拆解 博主名 或 /xhs-deconstruct 拆解 链接。
version: 3.0
author: 阿恒
---

# 小红书博主拆解

分析小红书博主的内容模式，输出可复用的标题、封面、内容结构和选题规律。

## 前置条件

- 已安装并可使用 `opencli`
- 已安装小红书适配器
- 浏览器桥接可正常连接
- 详细安装说明看 `README.md`

## 触发方式

- `/xhs-deconstruct 拆解 博主名字`
- `/xhs-deconstruct 拆解 小红书主页链接`

## 筛选硬规则

- 默认只分析**最近半年**
- 候选数据来源只用 `opencli xiaohongshu user ... --limit 50 --format json`
- 先按时间过滤，再按 `type` 过滤，只保留图文
- 图文帖子按点赞降序排序，取前20篇进入候选池
- 按候选顺序抓取，最多保留10篇
- 不足10篇就有多少抓多少
- 视频帖子不进入候选池
- 下载阶段只做复核，不承担主筛选

## 执行流程

### 1. 锁定博主

- 用户给名字：先搜索，再定位正确主页
- 用户给链接：直接使用主页链接
- 获取主页后，统一用：

```bash
opencli xiaohongshu user "用户主页链接" --limit 50 --format json
```

### 2. 建立候选池

1. 只保留最近半年内的帖子
2. 只保留 `type` 为图文的帖子
3. 按点赞数降序排序
4. 取前20篇作为候选池；不足20篇就全取

🔴 **CHECKPOINT：先向用户展示最近半年图文总数、候选池数量，再继续。**

### 3. 顺序抓取候选

对候选池按点赞从高到低逐条执行：

```bash
opencli xiaohongshu note "搜索结果URL" --format json > raw-data/notes/博主名/01-标题.json
opencli xiaohongshu download "搜索结果URL" --output bloggers/01-博主名/原文/image/01-标题/
```

抓取规则：
- 必须使用带 `xsec_token` 的 URL
- 若下载结果是图片文件（如 `.jpg/.png/.webp`），继续
- 若下载结果只有 `.mp4` 或明显是视频，视为类型误判，跳过并记录异常
- 成功保留满10篇就停止
- 若候选抓完仍不足10篇，直接如实返回

### 4. 生成产物

按顺序生成：
1. `概览.md`
2. 每篇 `原文/*.md`
3. 每篇 `拆解/*.md`
4. `总结.md`
5. `报告.html`

模板来源：
- `references/overview-template.md`
- `references/original-template.md`
- `references/deconstruct-template.md`
- `references/summary-template.md`
- `references/html-report-template.md`

🔴 **CHECKPOINT：下载完成后，先确认图片数量和格式，再写原文。**

🔴 **CHECKPOINT：所有拆解完成后，先向用户展示拆解概况，再写总结。**

## 异常处理

- 风控或空结果：降到 `--limit 30`，或等待 5-10 分钟后重试
- 连续3篇读取失败：停止抓取，等待 10 分钟
- `type` 与下载结果不一致：以下载结果为准，跳过并记录
- 最近半年图文不足10篇：如实说明，不补视频，不降低门槛

## 输出结构

```text
xhs-work/
├── bloggers/01-博主名/
│   ├── 概览.md
│   ├── 总结.md
│   ├── 报告.html
│   ├── 原文/
│   │   ├── 01-标题.md
│   │   └── image/01-标题/帖子ID/1.jpg
│   └── 拆解/
│       └── 01-标题.md
└── raw-data/
    ├── posts/
    └── notes/
```

## 命名与约束

- 文件名用连字符，不用空格
- 序号统一两位：`01`、`02`
- 图片引用路径必须包含帖子ID子目录
- 不要编造数据，所有统计基于实际抓取结果
- HTML 报告必须自包含，可直接浏览器打开

## 完成前必读

- `references/do-nots.md`
- `references/quality-checklist.md`
- `references/faq.md`
