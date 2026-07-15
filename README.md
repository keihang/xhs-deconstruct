# xhs-deconstruct

小红书博主拆解 skill — 分析指定时间段内流量最好的10篇图文帖子（点赞>100），提炼标题公式、封面设计、内容结构等可复用模式。

## 适用场景

- 拆解竞品博主的内容策略
- 提炼爆款标题公式和封面设计规律
- 分析互动数据背后的内容逻辑
- 为自己的内容创作积累可复用模板

## 前置条件

> 本 skill 依赖 [OpenCLI](https://github.com/jackwener/OpenCLI)（Apache-2.0）实现浏览器自动化操作。

**方式 A：OpenCLIApp（推荐 macOS / Windows）**

下载安装包：https://opencli.info/download

安装后打开一次 app，在 System 页面安装或修复 `opencli` 命令。

**方式 B：npm 全局安装（CLI-only / CI / 服务器）**

需要 **Node.js >= 20**：

```bash
node --version                          # 确认版本 >= 20
npm install -g @jackwener/opencli      # 全局安装
```

### 2. 安装 Browser Bridge 扩展

OpenCLI 通过浏览器扩展连接 Chrome/Chromium。

**方式 A：Chrome Web Store（推荐）**

安装 [OpenCLI 扩展](https://chromewebstore.google.com/detail/opencli/ildkmabpimmkaediidaifkhjpohdnifk)。

**方式 B：手动安装**

1. 从 [Releases 页面](https://github.com/jackwener/opencli/releases) 下载 `opencli-extension-v{version}.zip`
2. 解压，打开 `chrome://extensions`，启用**开发者模式**
3. 点击**加载已解压的扩展程序**，选择解压后的文件夹

### 3. 验证安装

```bash
opencli doctor
```

### 4. 安装小红书适配器

```bash
opencli install xiaohongshu
```

### 5. 网络环境

需能访问小红书，建议使用国内 IP。

## 30 秒开始

```bash
npx skills add https://github.com/keihang/xhs-deconstruct --skill xhs-deconstruct
```

也可以直接把这段话发给有 shell 权限的 AI Agent：

```
帮我安装 xhs-deconstruct。请把 https://github.com/keihang/xhs-deconstruct 克隆到 ~/.claude/skills/xhs-deconstruct
```

已经安装过的话，用这段话更新：

```
帮我更新 xhs-deconstruct skill。请进入 ~/.claude/skills/xhs-deconstruct 执行 git pull
```

## 使用方式

用 `/xhs-deconstruct` 斜杠命令触发：

### 方式A：搜索博主名

```
/xhs-deconstruct 拆解 数字生命卡兹克
```

### 方式B：直接发博主主页链接

```
/xhs-deconstruct 拆解 https://www.xiaohongshu.com/user/profile/xxx?xsec_token=xxx
```


skill 会自动执行完整流程：锁定博主 → 确认时间段 → 筛选图文帖子 → 抓取内容 → 写概览 → 写原文 → 拆解分析 → 总结模式 → 沉淀发现 → 生成报告。

### 使用示例

| 场景 | Prompt | 说明 |
|------|--------|------|
| 标准拆解 | `/xhs-deconstruct 拆解 数字生命卡兹克` | 搜索博主，自动筛选爆款帖 |
| 直接链接 | `/xhs-deconstruct 拆解 https://www.xiaohongshu.com/user/profile/xxx` | 跳过搜索，直接用链接获取数据 |
| 大 V 博主 | `/xhs-deconstruct 拆解 张琦` | 数据量大，严格筛选 Top 10 |
| 小博主 | `/xhs-deconstruct 拆解 一个粉丝500的小博主` | 数据不足时会降级处理或终止 |
| 视频博主 | `/xhs-deconstruct 拆解 何同学` | 视频帖子自动跳过，只分析图文 |

## 输出结构

```
xhs-work/
├── bloggers/
│   └── 01-博主名/
│       ├── 概览.md           # 博主信息 + Top 10 数据表
│       ├── 总结.md           # 跨帖子模式提炼（标题公式、封面规律等）
│       ├── 报告.html         # 莫兰迪风格可视化报告（浏览器直接打开）
│       ├── 原文/             # 帖子原文 + 图片
│       │   ├── 01-标题.md    # 含 frontmatter 元数据（blogger/platform/type/date）
│       │   └── image/        # 封面 + 内容图
│       │       ├── 01-标题/              # 每篇帖子一个子目录
│       │       │   └── 帖子ID_1.jpg      # opencli download 会在目录内再建一层帖子ID子目录
│       │       │   └── 帖子ID_2.jpg
│       │       │   └── ...
│       │       └── ...
│       └── 拆解/             # 7 维度拆解分析
├── raw-data/                 # API 原始数据（可删除）
├── 总结.md                   # 跨博主总结（可选）
└── CLAUDE.md
```

**关键说明**：
- 原文文件含 frontmatter 元数据（blogger, platform, type, date）
- 图片目录有两层：`image/序号-标题/帖子ID/图片文件`，原文引用路径需包含帖子ID子目录
- `raw-data/` 是中间产物，确认无误后可删除
- 总结文件放在博主目录内（`博主目录/总结.md`），不是根目录

## 拆解维度

1. **标题公式** — 关键词、结构、情绪钩子
2. **封面设计** — 色彩、排版、视觉焦点
3. **图片结构** — 数量、类型、编排逻辑
4. **内容结构** — 段落组织、字数、CTA
5. **选题角度** — 时效性、痛点、差异化
6. **互动数据** — 点赞/收藏/评论关系
7. **可复用模式** — 标题模板、内容套路、封面套路

## 参考资料

| 文件 | 用途 |
|------|------|
| [拆解模板](references/deconstruct-template.md) | 7 维度拆解的完整模板 |
| [总结模板](references/summary-template.md) | 跨帖子总结模板 |
| [概览模板](references/overview-template.md) | 博主概览文件模板 |
| [原文模板](references/original-template.md) | 原文文件模板 |
| [HTML 报告模板](references/html-report-template.md) | 莫兰迪风格可视化报告模板 |
| [反例清单](references/do-nots.md) | 不要做什么 |
| [质量自检](references/quality-checklist.md) | 完成后检查清单 |
| [常见问题](references/faq.md) | 排错指南 |
| [风控指南](references/rate-limit-guide.md) | 小红书反爬策略与规避方法 |

## opencli 小红书常用命令

| 命令 | 用途 | 本 skill 使用步骤 | 注意事项 |
|------|------|------------------|---------|
| `opencli xiaohongshu search` | 搜索帖子 | 第1步：搜索博主 | `--limit 20` |
| `opencli xiaohongshu user` | 获取用户帖子列表 | 第1步：锁定博主 | ⚠️ 必须加 `--limit 50`，默认15篇会遗漏大量帖子 |
| `opencli xiaohongshu note` | 读取帖子详情 | 第4步：读取内容 | 每次间隔15秒 |
| `opencli xiaohongshu download` | 下载图片/视频 | 第4步：下载图片 | 每次间隔15秒 |
| `opencli xiaohongshu comments` | 获取评论 | 未使用（可扩展） | — |

完整命令列表见 [OpenCLI xiaohongshu 文档](https://github.com/jackwener/OpenCLI)。
