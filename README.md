# xhs-deconstruct

小红书博主拆解 skill。默认分析最近半年的图文帖子，先按点赞筛出前 20 篇图文候选，再从候选里顺序抓取最多 10 篇做拆解，输出标题、封面、内容结构和选题规律。视频帖子不进入候选池。

## 核心规则

- 默认只看最近半年
- 候选列表来自 `opencli xiaohongshu user ... --limit 50 --format json`
- 列表阶段先按时间过滤，再按 `type` 过滤，只保留图文
- 图文按点赞降序排序，取前 20 篇进入候选池
- 从候选池顺序抓取，最多保留 10 篇
- 不足 10 篇就抓多少算多少
- 视频不进入候选池
- 下载阶段只做复核，不承担主筛选

## 适用场景

- 拆解竞品博主的内容策略
- 提炼爆款标题公式和封面设计规律
- 分析互动数据背后的内容逻辑
- 为自己的内容创作积累可复用模板

## 安装

### 1. 安装 `opencli`

推荐直接安装 OpenCLIApp：

- 下载地址：https://opencli.info/download
- 安装后打开一次 app，在 System 页面安装或修复 `opencli` 命令

也可以用 npm 全局安装：

```bash
node --version
npm install -g @jackwener/opencli
```

要求 `Node.js >= 20`。

### 2. 安装浏览器扩展

OpenCLI 通过浏览器扩展连接 Chrome/Chromium。

- Chrome Web Store：[OpenCLI 扩展](https://chromewebstore.google.com/detail/opencli/ildkmabpimmkaediidaifkhjpohdnifk)
- 或从 [Releases](https://github.com/jackwener/opencli/releases) 下载扩展包后手动加载

### 3. 验证安装

```bash
opencli doctor
opencli install xiaohongshu
```

### 4. 网络环境

需能访问小红书，建议使用国内 IP。

## 快速开始

```bash
npx skills add https://github.com/keihang/xhs-deconstruct --skill xhs-deconstruct
```

如果你让有 shell 权限的 Agent 安装，也可以直接发：

```text
帮我安装 xhs-deconstruct。请把 https://github.com/keihang/xhs-deconstruct 克隆到 ~/.claude/skills/xhs-deconstruct
```

更新时可发：

```text
帮我更新 xhs-deconstruct skill。请进入 ~/.claude/skills/xhs-deconstruct 执行 git pull
```

## 使用方式

### 用博主名触发

```text
/xhs-deconstruct 拆解 数字生命卡兹克
```

### 用主页链接触发

```text
/xhs-deconstruct 拆解 https://www.xiaohongshu.com/user/profile/xxx
```

skill 会自动完成：锁定博主、拉最近半年图文列表、建立候选池、抓取样本、生成概览、拆解、总结和 HTML 报告。

## 执行流程

1. 锁定博主主页
2. 拉取最近半年帖子列表，并按 `type` 过滤图文
3. 图文按点赞排序，取前 20 篇做候选
4. 从候选里顺序抓取，最多保留 10 篇
5. 生成 `概览.md`、`原文/*.md`、`拆解/*.md`、`总结.md`、`报告.html`

## 输出结构

```text
xhs-work/
├── bloggers/
│   └── 01-博主名/
│       ├── 概览.md
│       ├── 总结.md
│       ├── 报告.html
│       ├── 原文/
│       │   ├── 01-标题.md
│       │   └── image/
│       │       └── 01-标题/帖子ID/1.jpg
│       └── 拆解/
│           └── 01-标题.md
└── raw-data/
    ├── posts/
    └── notes/
```

说明：

- `概览.md`：博主信息和本次实际抓取样本汇总
- `总结.md`：跨帖子共性模式提炼
- `报告.html`：可直接浏览器打开的可视化报告
- `原文/`：帖子原文和图片
- `拆解/`：逐篇拆解结果
- `raw-data/`：中间数据，确认无误后可删除

## 拆解维度

1. 标题公式
2. 封面设计
3. 图片结构
4. 内容结构
5. 选题角度
6. 互动数据
7. 可复用模式

## 参考文件

| 文件 | 用途 |
|------|------|
| [拆解模板](references/deconstruct-template.md) | 7 维度拆解模板 |
| [总结模板](references/summary-template.md) | 跨帖子总结模板 |
| [概览模板](references/overview-template.md) | 博主概览模板 |
| [原文模板](references/original-template.md) | 原文模板 |
| [HTML 报告模板](references/html-report-template.md) | 可视化报告模板 |
| [反例清单](references/do-nots.md) | 不要做什么 |
| [质量自检](references/quality-checklist.md) | 完成后检查清单 |
| [常见问题](references/faq.md) | 排错指南 |
| [风控指南](references/rate-limit-guide.md) | 风控与重试建议 |

## 常用命令

| 命令 | 用途 | 说明 |
|------|------|------|
| `opencli xiaohongshu search` | 搜索 | 用于按名字找博主 |
| `opencli xiaohongshu user` | 拉列表 | 候选池以这个命令返回的 `type` 和点赞数据为准 |
| `opencli xiaohongshu note` | 读详情 | 用于生成原文和拆解 |
| `opencli xiaohongshu download` | 下载图片 | 用于保存原图，并复核是否误判为视频 |

推荐使用：

```bash
opencli xiaohongshu user "用户主页链接" --limit 50 --format json
```

## 文档分工

- `README.md`：安装、使用、输出说明
- `SKILL.md`：执行规则、异常处理、命名约束

具体执行以 `SKILL.md` 为准。
