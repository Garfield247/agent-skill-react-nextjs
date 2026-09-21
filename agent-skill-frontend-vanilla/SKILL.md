---
name: frontend-vanilla
description: >-
  HTML5/CSS3/Vanilla JavaScript 现代前端静态页面工程规范技能。
  当任务涉及纯静态页面、Landing Page、管理后台原型、H5 营销页或无框架前端工程时自动激活。
  涵盖语义化 HTML5 结构规范、现代 CSS 设计系统（变量/Flexbox/Grid/动效）、ES6+ 模块化编程范式、
  SEO 最佳实践、WCAG 无障碍基线、性能优化铁律与调试排障武器库。
---

# HTML5/CSS3/Vanilla JS 现代前端工程规范技能 (Frontend Vanilla Skill)

## 概述 (Overview)

本技能定义了在**纯静态前端页面开发、Landing Page 设计、H5 营销页与原型快速迭代**时的工业级工程规范。
践行 **"语义优先、设计系统驱动、最小依赖、渐进增强"** 的前端第一性原理，吸收现代社区主流 Cursor Rules 与 awesome-cursorrules 前端板块的精华打法。

---

# 1. 核心铁律与绝对红线 (Core Redlines)

### 🚨 铁律一：严禁内联样式污染结构层
- 禁止在 HTML 中写 style="..." 属性（仅限 JS 动态注入过渡值时例外）；
- 所有样式必须归属 CSS 设计系统（变量 + 类名），保持关注点分离。

### 🚨 铁律二：语义化 HTML 强制门禁
- 严禁无意义的 div 套娃；必须优先使用语义化标签：
  - 页面骨架：header / main / footer / nav / aside / section / article；
  - 表单系：每个 input 必须配对 label[for] 或 aria-label。

### 🚨 铁律三：ES6+ 现代语法强制
- 严禁使用 var，一律 const / let；
- 严禁 == 双等判断，一律 === 严格等；
- 异步操作必须使用 async/await，禁止裸 .then() 链式地狱；
- 严禁修改 Array.prototype / Object.prototype 等原型链。

### 🚨 铁律四：事件处理器命名约定
- 所有事件处理函数必须以 handle 前缀命名（handleClick、handleFormSubmit、handleScroll）；
- 函数单一职责：超过 30 行的函数强制拆分。

### 🚨 铁律五：防御性 DOM 操作
- 访问任何 DOM 元素前，必须先判断存在性，使用早返回模式；
- 禁止在 DOMContentLoaded 外部直接操作 DOM。

### 🚨 铁律六：每个页面必须配置完整 SEO 三件套
- title、meta[name=description]（≤155字符）、og:title

---

# 2. HTML 结构规范 (HTML Architecture)

### 2.1 标准骨架模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="页面精准描述，不超过 155 字符">
  <title>页面标题 | 网站名称</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="stylesheet" href="/styles/main.css">
</head>
<body>
  <header role="banner">
    <nav aria-label="主导航">...</nav>
  </header>
  <main id="main-content">
    <h1>页面唯一 H1 标题</h1>
  </main>
  <footer role="contentinfo">...</footer>
  <script type="module" src="/scripts/main.js"></script>
</body>
</html>
```

### 2.2 标题层级铁律
- 每个页面有且仅有一个 h1；
- 标题层级必须连续递减（h1→h2→h3），严禁跳级；
- 标题不得用于视觉加粗目的，强调用 strong。

### 2.3 图片性能规范

```html
<!-- ✅ 推荐：懒加载 + 尺寸声明 + 描述性 alt -->
<img src="hero.webp" alt="产品首页主视觉图"
     width="1200" height="630"
     loading="lazy" decoding="async">

<!-- ❌ 禁止：缺少 alt、缺少尺寸（引发 CLS 布局抖动） -->
<img src="hero.jpg">
```

---

# 3. CSS 设计系统规范 (CSS Design System)

### 3.1 CSS 变量系统（Design Tokens 必须优先定义）

```css
:root {
  /* 色彩（使用 HSL 便于动态调整亮度） */
  --color-primary: hsl(220, 85%, 56%);
  --color-primary-dark: hsl(220, 85%, 40%);
  --color-surface: #0f172a;
  --color-surface-elevated: #1e293b;
  --color-text-primary: #f1f5f9;
  --color-text-muted: #94a3b8;

  /* 间距（8px 基准网格） */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-16: 4rem;

  /* 字体 */
  --font-sans: 'Inter', 'Noto Sans SC', system-ui, sans-serif;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-xl: 1.25rem;
  --text-4xl: 2.25rem;

  /* 过渡 */
  --transition-fast: 150ms ease;
  --transition-base: 250ms ease;
  --transition-slow: 400ms ease;

  /* 阴影 */
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.3);
  --shadow-glow: 0 0 30px rgba(99,102,241,0.3);
}
```

### 3.2 微动效规范

```css
/* ✅ 悬停微动效（GPU 加速） */
.card {
  transition: transform var(--transition-base), box-shadow var(--transition-base);
  will-change: transform;
}
.card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-glow);
}

/* ✅ 尊重减少动效偏好 */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

### 3.3 CSS 文件组织

```
styles/
├── tokens.css        # Design Tokens（:root 变量）
├── reset.css         # 现代 CSS Reset
├── typography.css    # 字体排版系统
├── layout.css        # 容器、Grid、Flexbox 通用类
├── components/       # 按组件拆分
│   ├── button.css
│   └── card.css
└── main.css          # 入口（@import 汇总）
```

---

# 4. JavaScript 编程规范 (JavaScript Patterns)

### 4.1 模块化结构

```
scripts/
├── main.js           # 入口，仅做初始化编排
├── utils/
│   └── dom.js        # DOM 工具函数
└── components/
    └── nav.js        # 各组件交互
```

### 4.2 DOM 工具函数标准

```javascript
// utils/dom.js
export const $ = (selector, ctx = document) => ctx.querySelector(selector);
export const $$ = (selector, ctx = document) => [...ctx.querySelectorAll(selector)];

export function on(el, event, handler, options) {
  if (!el) return;
  el.addEventListener(event, handler, options);
  return () => el.removeEventListener(event, handler, options);
}
```

### 4.3 Intersection Observer 懒加载动效

```javascript
// ✅ 推荐：IntersectionObserver，严禁 scroll 事件轮询
const observer = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      if (!entry.isIntersecting) return;
      entry.target.classList.add('animate-fade-in');
      observer.unobserve(entry.target);
    });
  },
  { threshold: 0.15 }
);
document.querySelectorAll('[data-animate]').forEach((el) => observer.observe(el));
```

### 4.4 表单处理范式

```javascript
async function handleFormSubmit(event) {
  event.preventDefault();
  const form = event.currentTarget;
  const submitBtn = form.querySelector('[type="submit"]');
  if (submitBtn.disabled) return; // 防重复提交

  submitBtn.disabled = true;
  try {
    const data = Object.fromEntries(new FormData(form));
    const res = await fetch('/api/submit', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data),
    });
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
  } catch (err) {
    console.error('[Form] 提交失败:', err);
  } finally {
    submitBtn.disabled = false;
  }
}
```

---

# 5. 性能优化铁律 (Performance)

| 优化项 | 规范 |
| :--- | :--- |
| **图片格式** | 优先 WebP/AVIF；图标用 SVG inline |
| **字体加载** | rel="preconnect" + font-display: swap，避免 FOIT |
| **脚本加载** | type="module" 置于 body 底部；禁止 document.write |
| **CLS 防抖** | 图片/视频必须声明 width + height；Lighthouse CLS < 0.1 |
| **LCP 优化** | 首屏大图 loading="eager" + fetchpriority="high"；LCP < 2.5s |

---

# 6. 无障碍基线 (Accessibility Baseline - WCAG AA)

```html
<!-- ✅ 键盘可聚焦的自定义控件 -->
<div role="button" tabindex="0" aria-label="关闭弹窗">✕</div>

<!-- ✅ 图标按钮必须有文本替代 -->
<button aria-label="搜索">
  <svg aria-hidden="true" focusable="false">...</svg>
</button>
```

- 文本与背景颜色对比度 ≥ 4.5:1（WCAG AA）；
- 所有交互元素必须可通过键盘访问（Tab + Enter/Space）。

---

# 7. 排障武器库 (Troubleshooting)

| 症状 | 根因 | 解决方案 |
| :--- | :--- | :--- |
| 字体 FOIT（页面空白闪烁） | 字体加载阻塞渲染 | CSS font-display: swap |
| 布局 CLS（内容跳动） | 图片无尺寸声明 | 强制设置 width/height |
| 滚动性能卡顿 | scroll 事件未节流 | 改用 IntersectionObserver |
| 暗黑模式闪白 | 未适配 prefers-color-scheme | :root 下添加 media 查询 |
| 移动端点击延迟 300ms | 未设置 touch-action | touch-action: manipulation |
| 表单重复提交 | 未禁用提交按钮 | 提交期间 disabled = true |

---

# 8. 现代版本适配基线

| 特性 | 最低支持目标 | 覆盖率 |
| :--- | :--- | :--- |
| CSS Grid | Chrome 57+ / Safari 10.1+ | > 98% |
| CSS 变量 | Chrome 49+（放弃 IE） | > 97% |
| ES Module | Chrome 61+ / Safari 10.1+ | > 96% |
| IntersectionObserver | Chrome 51+ | > 97% |
