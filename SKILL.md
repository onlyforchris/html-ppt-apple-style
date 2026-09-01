---
name: html-ppt-apple-style
description: >
  Use this skill to create beautiful, interactive HTML presentations (PPT slides)
  in the style of Apple Keynote product launches: cinematic dark backgrounds,
  massive typography, gradient accents, and smooth transitions. Trigger this skill
  when the user asks to create a PPT, make slides, or produce a presentation,
  especially with phrases like "苹果风格", "发布会风格", "高端大气", "简洁大气",
  "科技感", "keynote style", or "Apple style". The output is a single self-contained
  HTML file that runs in any browser with keyboard navigation, dot indicators,
  fullscreen support, and touch swipe for mobile.
agent_created: true
---

# HTML PPT — Apple Keynote 风格

## 概述

将任何主题制作成苹果发布会风格的 HTML 演示文稿：黑色主基调、电影级排版、渐变文字、大数字统计、毛玻璃卡片，以及丝滑的切换动效。输出单个 HTML 文件，在浏览器中即开即用。

---

## 工作流程

### Step 1：理解需求

收集以下信息（可以从用户对话中推断，不必全问）：

- **主题**：产品发布 / 公司介绍 / 项目汇报 / 课题演讲 / 融资路演 / 其他
- **页数**：默认 6–10 页；如用户指定则遵从
- **语言**：中文 / 英文 / 双语
- **色调偏好**：默认暗色（苹果风）；也可选白色简洁风
- **内容**：用户提供的文字、数据、要点

### Step 2：规划幻灯片结构

标准苹果风格演示文稿的页面节奏：

```
1. Hero 开场        — 产品名/主题 + 一句 Tagline
2. 问题/背景        — "为什么"（可选）
3. 解决方案公告     — "One More Thing" 式惊喜
4. 核心特性 ×3      — 三列卡片
5. 数据/统计        — 大数字展示
6. 产品详情         — 白底页（节奏对比）
7. 名言/愿景        — 引用页（情感共鸣）
8. 价格/行动        — CTA 页
9. 结尾谢幕         — 简洁收尾
```

根据主题灵活增删页面。每页只传达 **1 个核心信息**。

### Step 3：生成 HTML 文件

**使用模板起步**：参考 `assets/apple-ppt-template.html` 作为基础结构，在此基础上修改内容和配色。

**设计系统规范**：参考 `references/apple-design-system.md` 查阅详细的颜色、字体、组件规范。

**关键实现要点**：

#### 基础结构
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <!-- 引入 Inter 字体 (Google Fonts) -->
  <!-- 字体栈: 'Inter', -apple-system, 'SF Pro Display', 'Helvetica Neue', sans-serif -->
  <!-- 全局变量: --apple-black, --apple-blue: #0071e3, --slide-w: 1280px, --slide-h: 720px -->
</head>
<body>
  <div class="presentation">
    <!-- 每个 .slide 默认 display:none，active 的显示 -->
    <div class="slide slide-hero active"> ... </div>
    <div class="slide slide-dark"> ... </div>
    <!-- ... 更多幻灯片 ... -->
  </div>
  <div class="nav-bar"><!-- 导航点 + 上一页/下一页 --></div>
  <script><!-- 切换逻辑、键盘、触摸 --></script>
</body>
```

#### 必须包含的交互功能
- `← →` 方向键 / `Space` 键切换
- `F` 键全屏（`requestFullscreen`）
- 点击导航圆点跳转
- 触摸滑动（`touchstart` + `touchend`，阈值 50px）
- 右下角显示 `当前页 / 总页数`
- 入场动画：`opacity:0 + translateY(20px)` → 自然态，持续 0.45s

#### 幻灯片主题 class 对照
| Class | 用途 |
|-------|------|
| `slide-hero` | 开场 / 结尾，纯黑 |
| `slide-dark` | 功能介绍，深色渐变 |
| `slide-white` | 产品详情，纯白 |
| `slide-gradient` | 公告页，深蓝黑渐变 |
| `slide-carbon` | 产品规格，碳纤纹理 |
| `slide-stats` | 数字统计，深黑 + 光晕 |

#### 排版系统（字号层级）
```css
.headline-xl  { font-size: 108px; font-weight: 800; letter-spacing: -0.04em; }
.headline     { font-size: 80px;  font-weight: 700; letter-spacing: -0.03em; }
.headline-sm  { font-size: 52px;  font-weight: 700; letter-spacing: -0.025em; }
.subheadline  { font-size: 28px;  font-weight: 300; color: rgba(255,255,255,0.7); }
.eyebrow      { font-size: 16px;  font-weight: 600; letter-spacing: 0.15em; text-transform: uppercase; color: #0071e3; }
.stat-number  { font-size: 96px;  font-weight: 800; letter-spacing: -0.04em; }
```

#### 渐变文字（必须与深色背景搭配）
```css
.text-gradient-blue   { background: linear-gradient(90deg, #4facfe, #00f2fe); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
.text-gradient-purple { background: linear-gradient(90deg, #c471ed, #f64f59); ... }
.text-gradient-gold   { background: linear-gradient(90deg, #f7971e, #ffd200); ... }
.text-gradient-green  { background: linear-gradient(90deg, #43e97b, #38f9d7); ... }
```

#### 背景光晕（必须 position:absolute）
```html
<div class="bg-glow bg-glow-blue" style="top:-100px;left:-100px;"></div>
<!-- width:600px height:600px, filter:blur(80px), background: radial-gradient(...rgba(0,113,227,0.25)...) -->
```

#### 特性卡片（深色）
```html
<div class="feature-card">
  <div class="feature-icon">⚡</div>
  <div class="feature-title">标题</div>
  <div class="feature-desc">2–3 句话的说明</div>
</div>
<!-- background: rgba(255,255,255,0.06); border: 1px solid rgba(255,255,255,0.10); border-radius: 20px; -->
```

### Step 4：输出文件

将生成的 HTML 写入文件：

- **文件名**：`{主题关键词}-presentation.html`（如 `product-launch.html`、`company-intro.html`）
- **位置**：用户指定路径，或当前工作目录
- **格式**：单文件，所有 CSS/JS 内联，无外部依赖（字体可用 Google Fonts CDN）

> ⚠️ **离线/内网注意**：字体用 Google Fonts CDN，离线或内网环境会加载失败。请保留字体栈里的系统兜底（`-apple-system`、`'PingFang SC'` 等），离线场景改用本地字体。

> ⚠️ **移动端自适应**：模板固定 `1280×720`，手机上会溢出被裁掉。建议在 `.presentation` 外层加一个按视口缩放的容器：`transform: scale(calc(100vw / 1280))` + `transform-origin: top left`，或用 JS 按 `window.innerWidth / 1280` 计算缩放系数。

输出后，把生成的 HTML 文件**交给用户预览**（用当前环境对应的交付工具，例如 `dsh_im_return_file` / `present_files`）。文件是单文件、可离线打开，也可直接丢进 GitHub Pages 托管展示。

### Step 5：迭代优化

常见调整方向（按用户反馈执行）：

- **更换配色主题**：整体改为白底（`.slide-white` 为主）
- **增加/删除页面**：直接操作对应 `<div class="slide">` 块
- **调整字体大小**：根据文字长度适当缩放 `.headline` 的 `font-size`
- **替换图标**：Emoji 替换为 SVG 图标或图片 `<img>`
- **添加图片**：使用 `<img>` 或 CSS `background-image` 做全出血背景

---

## 设计规则（绝不打破）

1. **每页最多 1 个核心信息** — 宁可多加一页，不要内容堆砌
2. **正文不超过 3 行** — 要点用卡片格式，不用段落
3. **渐变文字只用于深色背景** — 白底上渐变文字会不可见
4. **不使用鲜艳纯色背景** — 用 `#000`、深渐变、或纯白
5. **字重要有层次** — 大标题 700–800，副标题 300–400，正文 400
6. **光晕装饰要克制** — 透明度 0.15–0.25，不超过 3 个光晕/页
7. **所有间距成倍数** — 使用 8px 网格（8, 16, 24, 32, 40, 48, 60, 80px）

---

## 与本博客放映器集成（可选）

如果你要把产出的演示放进 `onlyforchris` 的博客（GitHub Pages），博客已内置一套放映器：

- **直接用**：把这份单文件 HTML 作为静态页放进博客仓库（无 frontmatter 的 `.html` 会被原样托管），再链接访问即可（可参考博客里的 `/apple-demo.html`）。
- **复用博客放映器**：博客提供 `layout: slideshow` + 可选 `style: apple` 主题。把每个 `<div class="slide">` 迁移成 `<section class="slide">`，并加上对应的背景类（`slide-hero`/`gradient`/`stats`/`white`），即可用博客的目录跳页、全屏、自动播放、明暗切换，且自动汇总到 `/slideshows/` 目录。苹果风的大字号、渐变文字、特性卡片等 class 在博客里也已支持。

> 一句话：想"完整还原苹果风"就托管独立 HTML；想"和其它演示统一、进目录"就迁移到博客的 `style: apple`。

---

## 资源文件

| 文件 | 用途 |
|------|------|
| `assets/apple-ppt-template.html` | 完整的 8 页演示模板，包含所有组件和样式，直接复制修改 |
| `references/apple-design-system.md` | 详细的颜色、字体、组件、布局规范参考 |
