# HTML 报告模板

根据博主的所有拆解数据，生成一个自包含的 HTML 报告。

## 风格

采用莫兰迪灰紫色系，参考 gzh-design 的 theme-morandi-gradient 组件库。

## 设计变量

| 变量 | 值 | 用途 |
|------|-----|------|
| `--purple` | `#72749A` | 主色，标题、强调 |
| `--purple-light` | `#8B7FAA` | 辅助色 |
| `--purple-pale` | `#B8B0C8` | 淡色装饰 |
| `--purple-faint` | `rgba(114,116,154,0.08)` | 浅底色 |
| `--warm-bg` | `#FFF5DF` | 暖色渐变终点 |
| `--text-dark` | `#3D3D3D` | 标题文字 |
| `--text-body` | `#5A5A5A` | 正文文字 |
| `--text-muted` | `#B8B0C0` | 辅助文字 |
| `--white` | `#FFFFFF` | 白色背景 |
| `--border` | `rgba(114,116,154,0.12)` | 边框 |

## 要求

1. **自包含**：所有 CSS 写在 `<style>` 标签内，不引用外部文件
2. **纯文字**：不嵌入封面图片，仅展示文字数据
3. **可直接打开**：浏览器打开即可阅读，无需服务器
4. **页面宽度**：`max-width: 900px`
5. **行高**：`line-height: 1.9`，`letter-spacing: 0.3px`

## 报告结构

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{博主名} — 小红书拆解报告</title>
  <style>
    :root {
      --purple: #72749A;
      --purple-light: #8B7FAA;
      --purple-pale: #B8B0C8;
      --purple-faint: rgba(114,116,154,0.08);
      --warm-bg: #FFF5DF;
      --text-dark: #3D3D3D;
      --text-body: #5A5A5A;
      --text-muted: #B8B0C0;
      --white: #FFFFFF;
      --border: rgba(114,116,154,0.12);
      --radius: 10px;
    }
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'SF Pro Display', -apple-system, BlinkMacSystemFont, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;
      background: var(--white);
      color: var(--text-body);
      line-height: 1.9;
      letter-spacing: 0.3px;
      -webkit-font-smoothing: antialiased;
    }
    .page {
      max-width: 900px;
      margin: 0 auto;
      padding: 40px 40px 60px;
      background: var(--white);
      min-height: 100vh;
    }
    @media (min-width: 960px) {
      body { background: #F0EEF5; }
    }

    /* ── Hero 渐变封面 ── */
    .hero {
      background: linear-gradient(135deg, #72749A 0%, #A8A0C0 50%, #FFF5DF 100%);
      border-radius: var(--radius);
      padding: 40px 32px 32px;
      margin-bottom: 32px;
    }
    .hero .eyebrow {
      font-size: 13px;
      font-weight: 700;
      letter-spacing: 2px;
      color: rgba(255,255,255,0.85);
      margin-bottom: 10px;
    }
    .hero h1 {
      font-size: 32px;
      font-weight: 700;
      color: #FFFFFF;
      line-height: 1.3;
      margin-bottom: 12px;
    }
    .hero .meta {
      font-size: 15px;
      color: rgba(255,255,255,0.85);
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
    }

    /* ── 期号标签条 ── */
    .tag-bar {
      display: flex;
      align-items: center;
      margin-bottom: 32px;
      padding: 0 4px;
    }
    .tag-bar .left {
      font-size: 12px;
      color: var(--purple);
      letter-spacing: 1.5px;
      font-weight: 700;
      white-space: nowrap;
    }
    .tag-bar .line {
      flex: 1;
      height: 1px;
      background: linear-gradient(90deg, var(--purple), transparent);
      margin: 0 12px;
    }
    .tag-bar .right {
      font-size: 12px;
      color: var(--text-muted);
      letter-spacing: 1px;
      white-space: nowrap;
    }

    /* ── 章节标题 ── */
    .section-heading {
      margin-bottom: 24px;
      padding: 0 4px;
    }
    .section-heading .num {
      font-size: 13px;
      color: var(--purple-pale);
      letter-spacing: 2px;
      margin-bottom: 4px;
    }
    .section-heading h3 {
      font-size: 19px;
      font-weight: 700;
      color: var(--purple);
      line-height: 1.4;
      margin-bottom: 6px;
    }
    .section-heading .en {
      font-size: 13px;
      color: var(--text-muted);
      letter-spacing: 1px;
      margin-bottom: 12px;
    }
    .section-heading .divider {
      height: 1px;
      background: var(--border);
    }

    /* ── 指标卡 ── */
    .stats-row {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
      margin-bottom: 40px;
    }
    .stat-card {
      background: linear-gradient(135deg, var(--purple-faint), rgba(255,245,223,0.3));
      border-radius: var(--radius);
      padding: 20px 12px;
      text-align: center;
    }
    .stat-card .value {
      font-size: 30px;
      font-weight: 700;
      color: var(--purple);
      line-height: 1;
    }
    .stat-card .label {
      font-size: 13px;
      color: var(--text-muted);
      margin-top: 6px;
    }

    /* ── 博主信息卡 ── */
    .blogger-card {
      background: var(--purple-faint);
      border-radius: var(--radius);
      padding: 20px 24px;
      margin-bottom: 40px;
    }
    .blogger-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 14px 24px;
    }
    .blogger-item .k {
      font-size: 12px;
      color: var(--purple);
      font-weight: 700;
      letter-spacing: 1px;
      margin-bottom: 2px;
    }
    .blogger-item .v {
      font-size: 15px;
      color: var(--text-dark);
    }

    /* ── 数据表格 ── */
    .data-table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 40px;
      font-size: 15px;
    }
    .data-table th {
      background: var(--purple-faint);
      padding: 10px 14px;
      text-align: left;
      font-size: 13px;
      font-weight: 700;
      color: var(--purple);
      border-bottom: 1px solid var(--border);
    }
    .data-table th:not(:first-child):not(:nth-child(2)) { text-align: right; }
    .data-table td {
      padding: 12px 14px;
      border-bottom: 1px solid rgba(114,116,154,0.1);
      color: var(--text-dark);
    }
    .data-table td:not(:first-child):not(:nth-child(2)) { text-align: right; font-variant-numeric: tabular-nums; }
    .data-table .rank {
      display: inline-flex;
      width: 22px; height: 22px;
      align-items: center;
      justify-content: center;
      border-radius: 50%;
      font-size: 12px;
      font-weight: 700;
      color: var(--white);
    }
    .rank.s { background: var(--purple); }
    .rank.a { background: var(--purple-light); }
    .rank.b { background: var(--purple-pale); }
    .data-table .highlight { color: var(--purple); font-weight: 700; }

    /* ── 帖子拆解卡片 ── */
    .post-card {
      border: 1px solid var(--border);
      border-radius: var(--radius);
      margin-bottom: 14px;
      overflow: hidden;
      transition: border-color 0.2s;
    }
    .post-card:hover { border-color: var(--purple-light); }
    .post-header {
      padding: 18px 20px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 14px;
      user-select: none;
    }
    .post-header:hover { background: rgba(114,116,154,0.03); }
    .post-rank {
      width: 32px; height: 32px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 13px;
      font-weight: 700;
      color: var(--white);
      flex-shrink: 0;
    }
    .post-rank.s { background: var(--purple); }
    .post-rank.a { background: var(--purple-light); }
    .post-info { flex: 1; }
    .post-info .post-title { font-size: 16px; font-weight: 700; color: var(--text-dark); line-height: 1.4; }
    .post-info .post-sub { font-size: 13px; color: var(--text-muted); margin-top: 4px; }
    .post-metrics {
      display: flex;
      gap: 14px;
      font-size: 14px;
      color: var(--text-muted);
      flex-shrink: 0;
    }
    .post-metrics .m-val { font-weight: 700; color: var(--purple); }
    .chevron {
      color: var(--purple-pale);
      font-size: 11px;
      transition: transform 0.2s;
      flex-shrink: 0;
    }
    .post-card.open .chevron { transform: rotate(90deg); }
    .post-body {
      display: none;
      padding: 0 20px 20px;
    }
    .post-card.open .post-body { display: block; }

    /* ── 拆解维度网格 ── */
    .dim-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 14px;
    }
    .dim-block {
      background: rgba(114,116,154,0.04);
      border-radius: 6px;
      padding: 16px;
    }
    .dim-block.full { grid-column: 1 / -1; }
    .dim-block h4 {
      font-size: 12px;
      font-weight: 700;
      color: var(--purple);
      letter-spacing: 1.5px;
      margin-bottom: 8px;
    }
    .dim-block p {
      font-size: 14px;
      color: var(--text-body);
      margin-bottom: 5px;
      line-height: 1.7;
    }
    .dim-block p:last-child { margin-bottom: 0; }
    .dim-block strong { color: var(--text-dark); }
    .dim-block code {
      font-family: 'SF Mono', 'Fira Code', monospace;
      background: var(--purple-faint);
      padding: 2px 7px;
      border-radius: 4px;
      font-size: 13px;
      color: var(--purple);
    }

    /* ── 模式列表 ── */
    .pattern-section { margin-bottom: 36px; }
    .pattern-section h3 {
      font-size: 16px;
      font-weight: 700;
      color: var(--text-dark);
      margin-bottom: 14px;
    }
    .pattern-list { list-style: none; }
    .pattern-list li {
      background: var(--white);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 14px 16px;
      margin-bottom: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 15px;
      color: var(--text-dark);
    }
    .pattern-list li .p-text { flex: 1; }
    .pattern-list li .p-tag {
      font-size: 12px;
      font-weight: 700;
      padding: 3px 10px;
      border-radius: 16px;
      white-space: nowrap;
      margin-left: 12px;
    }
    .p-tag.main { background: var(--purple); color: var(--white); }
    .p-tag.light { background: var(--purple-faint); color: var(--purple); }
    .p-tag.accent { background: rgba(255,245,223,0.6); color: var(--purple); }

    .code-block {
      background: rgba(114,116,154,0.04);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 14px 18px;
      font-family: 'SF Mono', 'Fira Code', monospace;
      font-size: 14px;
      line-height: 1.8;
      color: var(--text-body);
      overflow-x: auto;
      white-space: pre-wrap;
    }
    .code-block .kw { color: var(--purple); font-weight: 600; }
    .code-block .cm { color: var(--text-muted); }

    /* ── 发现卡片 ── */
    .finding-card {
      background: var(--purple-faint);
      border-left: 3px solid var(--purple-light);
      border-radius: 0 var(--radius) var(--radius) 0;
      padding: 18px 20px;
      margin-bottom: 10px;
    }
    .finding-card h4 {
      font-size: 16px;
      font-weight: 700;
      color: var(--text-dark);
      margin-bottom: 6px;
    }
    .finding-card p {
      font-size: 14px;
      color: var(--text-body);
      line-height: 1.7;
    }
    .finding-card .finding-meta {
      margin-top: 8px;
      font-size: 13px;
      color: var(--text-muted);
    }

    /* ── 建议列表 ── */
    .suggestion-group { margin-bottom: 28px; }
    .suggestion-group h4 {
      font-size: 13px;
      font-weight: 700;
      color: var(--purple);
      letter-spacing: 1.5px;
      margin-bottom: 10px;
    }
    .suggestion-list {
      list-style: none;
      counter-reset: sug;
    }
    .suggestion-list li {
      counter-increment: sug;
      padding: 10px 0 10px 32px;
      border-bottom: 1px solid rgba(114,116,154,0.1);
      font-size: 15px;
      color: var(--text-dark);
      position: relative;
    }
    .suggestion-list li::before {
      content: counter(sug, decimal-leading-zero);
      position: absolute;
      left: 0;
      font-size: 13px;
      font-weight: 700;
      color: var(--purple);
    }

    /* ── 末尾互动区 ── */
    .ending {
      background: linear-gradient(180deg, rgba(114,116,154,0.06), rgba(255,245,223,0.15));
      border-radius: var(--radius);
      padding: 28px 24px;
      text-align: center;
      margin-top: 48px;
    }
    .ending .tag {
      font-size: 13px;
      font-weight: 700;
      color: var(--purple);
      letter-spacing: 2px;
      margin-bottom: 8px;
    }
    .ending p {
      font-size: 15px;
      color: var(--text-body);
      line-height: 1.8;
    }
    .ending .end-mark {
      font-size: 13px;
      color: var(--text-muted);
      margin-top: 16px;
    }

    /* ── 页脚 ── */
    .footer {
      background: linear-gradient(135deg, var(--purple), var(--purple-light));
      border-radius: var(--radius);
      padding: 14px;
      text-align: center;
      margin-top: 24px;
    }
    .footer p {
      font-size: 12px;
      color: rgba(255,255,255,0.85);
      letter-spacing: 1px;
    }

    /* ── 分隔符 ── */
    .separator {
      text-align: center;
      padding: 16px;
      font-size: 18px;
      color: var(--purple-pale);
      letter-spacing: 6px;
    }

    /* ── 响应式 ── */
    @media (max-width: 768px) {
      .stats-row { grid-template-columns: repeat(2, 1fr); }
      .hero h1 { font-size: 24px; }
      .hero .meta { flex-direction: column; gap: 6px; }
      .dim-grid { grid-template-columns: 1fr; }
      .post-metrics { display: none; }
      .blogger-grid { grid-template-columns: 1fr; }
    }

    /* ── 动画 ── */
    @media (prefers-reduced-motion: no-preference) {
      .post-body { animation: fadeSlide 0.25s ease-out; }
      @keyframes fadeSlide {
        from { opacity: 0; transform: translateY(-6px); }
        to { opacity: 1; transform: translateY(0); }
      }
    }
  </style>
</head>
<body>
<div class="page">

  <!-- 渐变封面卡 -->
  <div class="hero">
    <div class="eyebrow">小红书博主拆解报告</div>
    <h1>{博主名}</h1>
    <div class="meta">
      <span>📅 {分析时间段}</span>
      <span>📊 {数量} 篇图文</span>
      <span>🎯 最近半年图文前20候选，最多拆解10篇</span>
    </div>
  </div>

  <!-- 期号标签条 -->
  <div class="tag-bar">
    <span class="left">REPORT</span>
    <span class="line"></span>
    <span class="right">NO.{期号}</span>
  </div>

  <!-- 博主信息 -->
  <div class="section-heading">
    <div class="num">01</div>
    <h3>博主信息</h3>
    <div class="en">BLOGGER INFO</div>
    <div class="divider"></div>
  </div>
  <div class="blogger-card">
    <div class="blogger-grid">
      <div class="blogger-item"><div class="k">昵称</div><div class="v">{博主名}</div></div>
      <div class="blogger-item"><div class="k">小红书号</div><div class="v">{小红书号}</div></div>
      <div class="blogger-item"><div class="k">IP属地</div><div class="v">{IP属地}</div></div>
      <div class="blogger-item"><div class="k">粉丝数</div><div class="v">{粉丝数}</div></div>
      <div class="blogger-item"><div class="k">获赞与收藏</div><div class="v">{总获赞}</div></div>
      <div class="blogger-item"><div class="k">赛道</div><div class="v">{内容领域}</div></div>
      <div class="blogger-item full"><div class="k">简介</div><div class="v">{个人简介}</div></div>
      <div class="blogger-item full"><div class="k">目标人群</div><div class="v">{目标受众}</div></div>
    </div>
  </div>

  <!-- 数据总览 -->
  <div class="section-heading">
    <div class="num">02</div>
    <h3>数据总览</h3>
    <div class="en">DATA OVERVIEW</div>
    <div class="divider"></div>
  </div>
  <div class="stats-row">
    <div class="stat-card"><div class="value">{平均点赞}</div><div class="label">平均点赞</div></div>
    <div class="stat-card"><div class="value">{平均收藏}</div><div class="label">平均收藏</div></div>
    <div class="stat-card"><div class="value">{平均评论}</div><div class="label">平均评论</div></div>
    <div class="stat-card"><div class="value">{平均收藏率}</div><div class="label">平均收藏率</div></div>
  </div>

  <table class="data-table">
    <thead>
      <tr><th>#</th><th>标题</th><th>点赞</th><th>收藏</th><th>评论</th><th>收藏率</th><th>互动率</th></tr>
    </thead>
    <tbody>
      <tr>
        <td><span class="rank s">1</span></td>
        <td>{标题}</td>
        <td class="highlight">{点赞}</td>
        <td>{收藏}</td>
        <td>{评论}</td>
        <td>{收藏率}</td>
        <td>{互动率}</td>
      </tr>
      <!-- ... 其余帖子 ... -->
    </tbody>
  </table>

  <!-- 帖子拆解详情 -->
  <div class="section-heading">
    <div class="num">03</div>
    <h3>帖子拆解详情</h3>
    <div class="en">POST ANALYSIS</div>
    <div class="divider"></div>
  </div>

  <!-- 每篇帖子一个卡片，重复以下结构 -->
  <div class="post-card">
    <div class="post-header" onclick="this.parentElement.classList.toggle('open')">
      <span class="post-rank s">TOP1</span>
      <div class="post-info">
        <div class="post-title">{标题}</div>
        <div class="post-sub">{类型} · {日期}</div>
      </div>
      <div class="post-metrics">
        <span>👍 <span class="m-val">{点赞}</span></span>
        <span>⭐ <span class="m-val">{收藏}</span></span>
        <span>💬 <span class="m-val">{评论}</span></span>
      </div>
      <span class="chevron">▶</span>
    </div>
    <div class="post-body">
      <div class="dim-grid">
        <div class="dim-block">
          <h4>1 · 标题公式</h4>
          <p><strong>结构</strong>：{结构分析}</p>
          <p><strong>可复用公式</strong>：<code>{公式}</code></p>
          <p><strong>情绪钩子</strong>：{情绪类型} — {具体表达}</p>
        </div>
        <div class="dim-block">
          <h4>2 · 封面设计</h4>
          <p><strong>风格</strong>：{风格类型}</p>
          <p><strong>排版</strong>：{排版方式}</p>
          <p><strong>主色</strong>：{主色调}</p>
        </div>
        <div class="dim-block">
          <h4>3 · 图片结构</h4>
          <p><strong>数量</strong>：{数量}张</p>
          <p><strong>编排逻辑</strong>：{整体结构}</p>
        </div>
        <div class="dim-block">
          <h4>4 · 内容结构</h4>
          <p><strong>开头钩子</strong>：{钩子类型} — {具体内容前3行}</p>
          <p><strong>信息组织</strong>：{组织方式} ｜ <strong>字数</strong>：{字数}字</p>
          <p><strong>结尾CTA</strong>：{CTA类型} — {具体内容}</p>
        </div>
        <div class="dim-block">
          <h4>5 · 选题角度</h4>
          <p><strong>切入痛点</strong>：{痛点}</p>
          <p><strong>目标人群</strong>：{人群特征}</p>
          <p><strong>情绪价值</strong>：{情绪类型}</p>
        </div>
        <div class="dim-block">
          <h4>6 · 互动数据</h4>
          <p><strong>收藏率</strong>：{收藏率} ｜ <strong>评论率</strong>：{评论率} ｜ <strong>互动率</strong>：{互动率}</p>
          <p><strong>数据解读</strong>：{解读}</p>
        </div>
        <div class="dim-block full">
          <h4>7 · 可复用模式</h4>
          <p><strong>标题公式</strong>：<code>{公式}</code> ｜ <strong>内容模板</strong>：<code>{模板}</code> ｜ <strong>封面模板</strong>：<code>{模板}</code> ｜ <strong>选题方向</strong>：<code>{方向}</code></p>
        </div>
      </div>
    </div>
  </div>
  <!-- 帖子卡片结束 -->

  <div class="separator">· · ·</div>

  <!-- 共性模式总结 -->
  <div class="section-heading">
    <div class="num">04</div>
    <h3>共性模式总结</h3>
    <div class="en">PATTERNS</div>
    <div class="divider"></div>
  </div>

  <div class="pattern-section">
    <h3>🏷️ 标题公式</h3>
    <ul class="pattern-list">
      <li><span class="p-text">{结构1}</span><span class="p-tag main">👍{点赞} ⭐{收藏率}</span></li>
      <li><span class="p-text">{结构2}</span><span class="p-tag light">👍{点赞} ⭐{收藏率}</span></li>
    </ul>
  </div>

  <div class="pattern-section">
    <h3>📝 可复用标题模板库</h3>
    <div class="code-block"><span class="kw">模板1</span>（{适用场景}）
<span class="kw">模板2</span>（{适用场景}）
<span class="kw">模板3</span>（{适用场景}）</div>
  </div>

  <div class="pattern-section">
    <h3>📐 内容结构</h3>
    <ul class="pattern-list">
      <li><span class="p-text">{钩子类型} — {示例摘要}</span><span class="p-tag main">👍{点赞}</span></li>
    </ul>
  </div>

  <div class="pattern-section">
    <h3>🎨 封面设计</h3>
    <ul class="pattern-list">
      <li><span class="p-text">{风格}</span><span class="p-tag accent">👍{点赞}</span></li>
    </ul>
  </div>

  <div class="pattern-section">
    <h3>🧭 可复用选题方向</h3>
    <ul class="pattern-list">
      <li><span class="p-text">{方向1}（{目标人群}，{痛点}）</span><span class="p-tag light">可复用</span></li>
    </ul>
  </div>

  <div class="separator">· · ·</div>

  <!-- 关键发现 -->
  <div class="section-heading">
    <div class="num">05</div>
    <h3>关键发现</h3>
    <div class="en">KEY FINDINGS</div>
    <div class="divider"></div>
  </div>

  <div class="finding-card">
    <h4>发现1：{标题}</h4>
    <p>{具体发现}</p>
    <div class="finding-meta">数据支撑：{数据} ｜ 应用建议：{怎么用}</div>
  </div>
  <!-- ... 更多发现 ... -->

  <div class="separator">· · ·</div>

  <!-- 下一步建议 -->
  <div class="section-heading">
    <div class="num">06</div>
    <h3>下一步建议</h3>
    <div class="en">NEXT STEPS</div>
    <div class="divider"></div>
  </div>

  <div class="suggestion-group">
    <h4>⚡ 短期行动</h4>
    <ol class="suggestion-list">
      <li>{行动1}</li>
      <li>{行动2}</li>
      <li>{行动3}</li>
    </ol>
  </div>

  <div class="suggestion-group">
    <h4>🔄 长期优化</h4>
    <ol class="suggestion-list">
      <li>{优化1}</li>
      <li>{优化2}</li>
      <li>{优化3}</li>
    </ol>
  </div>

  <div class="suggestion-group">
    <h4>🧪 待验证假设</h4>
    <ol class="suggestion-list">
      <li>{假设1}</li>
      <li>{假设2}</li>
      <li>{假设3}</li>
    </ol>
  </div>

  <!-- 末尾互动区 -->
  <div class="ending">
    <div class="tag">END OF REPORT</div>
    <p>数据周期：{时间段} ｜ 样本量：{数量}篇</p>
    <p>报告基于公开数据分析，仅供内容创作参考</p>
    <div class="end-mark">— · —</div>
  </div>

  <!-- 页脚 -->
  <div class="footer">
    <p>由 xhs-deconstruct skill 自动生成 · {日期}</p>
  </div>

</div>
</body>
</html>
```

## 数据来源

| 区块 | 数据来源文件 |
|------|-------------|
| 博主信息 | `概览.md` |
| 数据总览 | `总结.md` → 数据总览 + 数据统计 |
| 帖子拆解详情 | 各 `拆解/XX-标题.md` |
| 共性模式总结 | `总结.md` → 各维度总结 |
| 关键发现 | `总结.md` → 关键发现 |
| 下一步建议 | `总结.md` → 下一步建议 |

## 生成规则

1. 用 `{占位符}` 标记的地方，填入实际数据
2. 数据从对应的 Markdown 文件中提取，不要编造
3. 统计数字（平均值、百分比）必须基于实际数据计算
4. 如果某篇帖子缺少某个维度的数据，标注"数据不足"而非留空
5. HTML 文件保存到 `bloggers/XX-博主名/报告.html`
