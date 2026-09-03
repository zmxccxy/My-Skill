---
name: "html-ppt"
description: "生成深色科技风格的 HTML 幻灯片演示文稿。当用户需要创建 HTML 演示文稿、PPT、培训课件、会议演讲幻灯片时使用。生成单文件 HTML，包含完整的样式、动画、导航和交互。"
---

# HTML PPT 演示文稿生成器

生成深色科技风格的单文件 HTML 幻灯片演示文稿。所有 CSS 和 JS 内联在 HTML 中，无需外部依赖（仅引用 Google Fonts）。

## 整体架构

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>标题</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;500;700;900&family=JetBrains+Mono:wght@400;500;700&family=Playfair+Display:wght@700;900&display=swap" rel="stylesheet">
  <style>
    /* 所有 CSS 内联 */
  </style>
</head>
<body>
  <!-- 背景装饰 -->
  <div class="bg-grid"></div>
  <div class="bg-glow bg-glow-1"></div>
  <div class="bg-glow bg-glow-2"></div>
  <div class="bg-glow bg-glow-3"></div>

  <div class="slides-wrapper" id="slidesWrapper">
    <!-- 幻灯片内容 -->
  </div>

  <!-- 激光笔 -->
  <div id="laserPointer">...</div>

  <!-- 导航栏 -->
  <div class="nav-bar">...</div>

  <script>
    // 所有 JS 内联
  </script>
</body>
</html>
```

## 色彩系统（CSS 变量）

```css
:root {
  --bg-deep: #0a0e1a;          /* 深空蓝黑背景 */
  --bg-card: #111827;          /* 卡片背景 */
  --bg-card-hover: #1a2332;    /* 卡片悬停背景 */
  --accent-cyan: #06d6a0;      /* 主强调色 - 青绿 */
  --accent-blue: #118ab2;      /* 辅助色 - 蓝 */
  --accent-yellow: #ffd166;    /* 警告/重点色 - 黄 */
  --accent-red: #ef476f;       /* 错误/卡点色 - 红 */
  --accent-purple: #8338ec;    /* 技能/模型色 - 紫 */
  --text-primary: #e8edf5;     /* 主文字 */
  --text-secondary: #8899aa;   /* 次要文字 */
  --text-dim: #4a5568;         /* 暗淡文字 */
  --border: rgba(255,255,255,0.06);  /* 边框 */
  --glow-cyan: rgba(6, 214, 160, 0.15); /* 青绿光晕 */
}
```

## 字体配置

- **正文**: `Noto Sans SC` (300/400/500/700/900)
- **代码/数字**: `JetBrains Mono` (400/500/700)
- **装饰标题**: `Playfair Display` (700/900)
- **根字体大小**: `20px`（确保开会时投影清晰）

## 背景装饰

### 网格背景
```css
.bg-grid {
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(rgba(255,255,255,0.015) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,0.015) 1px, transparent 1px);
  background-size: 60px 60px;
  pointer-events: none;
  z-index: 0;
}
```

### 光晕效果（3 个）
```css
.bg-glow {
  position: fixed;
  width: 600px;
  height: 600px;
  border-radius: 50%;
  filter: blur(120px);
  opacity: 0.12;
  pointer-events: none;
  z-index: 0;
}
.bg-glow-1 { background: var(--accent-cyan); top: -200px; right: -100px; }
.bg-glow-2 { background: var(--accent-blue); bottom: -300px; left: -200px; }
.bg-glow-3 { background: var(--accent-purple); top: 50%; left: 50%; transform: translate(-50%, -50%); opacity: 0.05; }
```

### 噪点纹理
```css
body::before {
  content: '';
  position: fixed;
  inset: 0;
  background-image: url("data:image/svg+xml,..."); /* SVG fractalNoise */
  pointer-events: none;
  z-index: 9999;
}
```

## 幻灯片容器

```css
.slides-wrapper {
  position: relative;
  width: 100vw;
  height: 100vh;
}

.slide {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 4vh 8vw;
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1), transform 0.6s cubic-bezier(0.22, 1, 0.36, 1);
  pointer-events: none;
  overflow-y: auto;
}

.slide.active {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}

.slide.exit-up {
  opacity: 0;
  transform: translateY(-30px);
}
```

## 幻灯片类型

### 1. 封面页（Hero Slide）
```html
<div class="slide hero active" data-index="0">
  <div class="anim-item hero-badge">标签文字</div>
  <h1 class="anim-item"><span class="gradient-text">AI 辅助</span><br>代码生成实践</h1>
  <p class="anim-item">副标题描述</p>
  <div class="hero-meta anim-item">
    <span>📅 日期</span>
    <span>👤 演讲者</span>
    <span>💻 技术栈</span>
  </div>
</div>
```

### 2. 内容页（Content Slide）
```html
<div class="slide" data-index="N">
  <div class="slide-number anim-item">PART ONE / 子标题</div>
  <div class="slide-title anim-item">主标题 <span class="gradient-text">高亮</span></div>
  <!-- 内容区域 -->
</div>
```

### 3. 章节分隔页（Section Divider）
```html
<div class="slide section-divider" data-index="N">
  <div class="section-big">01</div>
  <div class="slide-number anim-item">PART ONE</div>
  <div class="slide-title anim-item" style="font-size: clamp(2.6rem, 5.5vw, 4.2rem);">
    章节标题
  </div>
  <p class="slide-subtitle anim-item" style="margin: 16px auto 0; text-align: center;">
    章节描述
  </p>
</div>
```

### 4. 结束页（End Slide）
```html
<div class="slide end-slide" data-index="N">
  <div class="anim-item hero-badge" style="margin-bottom: 24px;">THANK YOU</div>
  <h1 class="anim-item"><span class="gradient-text">谢谢</span></h1>
  <p class="anim-item">结束语</p>
</div>
```

## 组件库

### 卡片（Card）
```html
<div class="card anim-item">
  <div class="card-icon cyan">01</div>  <!-- 或 SVG 图标 -->
  <h3>标题</h3>
  <p>描述文字</p>
</div>
```
- 图标颜色类: `cyan`, `blue`, `yellow`, `red`, `purple`
- 卡片自动有顶部渐变线条悬停效果

### 内容网格（Content Grid）
```html
<div class="content-grid cols-3" style="margin-top: 36px;">
  <!-- 3 列卡片 -->
</div>
<div class="content-grid cols-2" style="margin-top: 32px;">
  <!-- 2 列卡片 -->
</div>
```

### 场景块（Scenario Block）
```html
<div class="scenario-block anim-item">
  <div class="scenario-header">
    <div class="card-icon cyan" style="margin-bottom:0;">
      <svg>...</svg>
    </div>
    <h3>标题</h3>
  </div>
  <div class="scenario-body">正文内容</div>
</div>
```

### 高亮提示框（Highlight Box）
```html
<div class="highlight-box anim-item">
  <strong>标签：</strong>内容文字
</div>
<div class="highlight-box warn anim-item">  <!-- 黄色警告 -->
<div class="highlight-box info anim-item">  <!-- 紫色信息 -->
```

### 代码块（Code Block）
```html
<div class="code-block anim-item">
  <span class="code-label">文件名</span>
  <span class="cm">注释内容</span>
  <span class="ann">@注解</span>
  <span class="type">类型名</span>
  <span class="str">字符串</span>
  <span class="kw">关键字</span>
  <span class="fn">函数名</span>
</div>
```
- 代码高亮类: `cm`(注释灰), `ann`(注解黄), `type`(类型青), `str`(字符串蓝), `kw`(关键字红), `fn`(函数紫)

### 解决方案列表（Solution List）
```html
<div class="solution-list">
  <div class="solution-item anim-item">
    <div class="solution-num">1</div>
    <div class="solution-content">
      <h4>标题 <span class="tag cyan">标签</span></h4>
      <p>描述</p>
    </div>
  </div>
</div>
```

### 标签（Tag）
```html
<span class="tag cyan">文本</span>
<span class="tag blue">文本</span>
<span class="tag yellow">文本</span>
<span class="tag red">文本</span>
<span class="tag purple">文本</span>
```

### 流程图（Flow Diagram）
```html
<div class="flow-row anim-item">
  <div class="flow-node">节点1</div>
  <div class="flow-arrow">&rarr;</div>
  <div class="flow-node accent">节点2（高亮）</div>
</div>
```
- 纵向流程图: 添加 `style="flex-direction: column; gap: 6px;"`，箭头加 `style="transform: rotate(90deg);"`

### 技能卡片（Skill Card）
```html
<a class="skill-card anim-item" href="URL" target="_blank">
  <div class="card-icon purple" style="margin-bottom: 0;">
    <svg>...</svg>
  </div>
  <h3>标题</h3>
  <p>描述</p>
  <div class="skill-link">链接文字</div>
</a>
```

## 排版样式

### 渐变文字
```css
.gradient-text       /* 青绿→蓝 */
.gradient-text-warm  /* 黄→红 */
.gradient-text-purple /* 紫→蓝 */
```

### 标题层级
- `.slide-number`: 0.9rem, JetBrains Mono, 青绿色, 全大写
- `.slide-title`: clamp(2.4rem, 5vw, 4rem), 900 粗体
- `.slide-subtitle`: clamp(1.2rem, 2.2vw, 1.6rem), 300 细体, 次要色
- `.hero h1`: clamp(3.5rem, 7vw, 6rem), 900 粗体
- `.section-big`: clamp(3rem, 8vw, 7rem), 900 粗体, opacity 0.08

## 动画系统

### 入场动画
```css
.anim-item {
  opacity: 0;
  transform: translateY(16px);
}

.slide.active .anim-item {
  animation: fadeSlideUp 0.5s cubic-bezier(0.22, 1, 0.36, 1) forwards;
}

.anim-item:nth-child(1) { animation-delay: 0.1s !important; }
.anim-item:nth-child(2) { animation-delay: 0.2s !important; }
/* ... 最多到第 8 个 */

@keyframes fadeSlideUp {
  to { opacity: 1; transform: translateY(0); }
}
```

### 切换动画
- 前进: 当前页 `exit-up`（向上移出），新页从下方淡入
- 后退: 当前页直接消失，新页从下方淡入
- 过渡时间: 0.6s, cubic-bezier(0.22, 1, 0.36, 1)

### 脉冲动画（Hero Badge 圆点）
```css
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.3; }
}
```

## 导航系统

### 底部导航栏
```html
<div class="nav-bar">
  <div class="nav-progress" id="navProgress"></div>
  <div class="nav-info">
    <span id="slideCounter">1 / N</span>
    <div class="nav-controls">
      <button class="nav-btn nav-btn-lg" onclick="navigate(-1)">&larr; 上一页</button>
      <button class="nav-btn nav-btn-lg" onclick="navigate(1)">下一页 &rarr;</button>
    </div>
  </div>
</div>
```

### 导航点（自动生成）
- 圆点: 10px, 次要色
- 激活: 24px 宽, 圆角 4px, 青绿色 + 光晕
- 点击可跳转到对应页

### 键盘导航
- `ArrowRight` / `ArrowDown` / `Space`: 下一页
- `ArrowLeft` / `ArrowUp`: 上一页
- `Home`: 第一页
- `End`: 最后一页
- 使用 `capture: true` 确保在 IDE 预览中也能工作

### 触摸导航
- 水平滑动 > 50px: 翻页
- 垂直滑动 > 50px: 翻页

### 激光笔
```html
<div id="laserPointer" style="position: fixed; width: 36px; height: 36px; border-radius: 50%; background: radial-gradient(circle, rgba(255,30,30,1) 0%, rgba(255,0,0,0.8) 30%, rgba(255,0,0,0.3) 60%, transparent 80%); pointer-events: none; z-index: 9999; transform: translate(-50%, -50%); box-shadow: 0 0 20px rgba(255,0,0,0.8), 0 0 50px rgba(255,0,0,0.5), 0 0 80px rgba(255,0,0,0.3); display: none;"></div>
```
- 鼠标移动时显示，停止 2 秒后隐藏
- 红色发光圆点，36px 直径

## JavaScript 核心逻辑

```javascript
const slides = document.querySelectorAll('.slide');
const totalSlides = slides.length;
let currentSlide = 0;

// 自动生成导航点
const navProgress = document.getElementById('navProgress');
for (let i = 0; i < totalSlides; i++) {
  const dot = document.createElement('div');
  dot.className = 'nav-dot' + (i === 0 ? ' active' : '');
  dot.onclick = () => goToSlide(i);
  navProgress.appendChild(dot);
}

function goToSlide(index) {
  if (index < 0 || index >= totalSlides || index === currentSlide) return;
  const direction = index > currentSlide ? 1 : -1;
  const current = slides[currentSlide];
  const next = slides[index];

  // 重置动画
  next.querySelectorAll('.anim-item').forEach(el => {
    el.style.animation = 'none';
    el.offsetHeight;
    el.style.animation = '';
  });

  current.classList.remove('active');
  if (direction > 0) {
    current.classList.add('exit-up');
    setTimeout(() => current.classList.remove('exit-up'), 600);
  }
  next.classList.add('active');

  // 更新导航点和计数器
  document.querySelectorAll('.nav-dot').forEach((dot, i) => {
    dot.classList.toggle('active', i === index);
  });
  document.getElementById('slideCounter').textContent = `${index + 1} / ${totalSlides}`;
  currentSlide = index;
}

function navigate(direction) {
  goToSlide(currentSlide + direction);
}
```

## 响应式

```css
@media (max-width: 768px) {
  .slide { padding: 3vh 5vw; }
  .content-grid.cols-2, .content-grid.cols-3 { grid-template-columns: 1fr; }
  .flow-row { flex-direction: column; }
  .flow-arrow { transform: rotate(90deg); }
  .hero-meta { flex-direction: column; gap: 8px; }
}
```

## 使用规范

1. **单文件**: 所有 CSS、JS 内联在 HTML 中，不依赖外部文件
2. **字体**: 仅引用 Google Fonts（Noto Sans SC + JetBrains Mono + Playfair Display）
3. **幻灯片编号**: 从 `data-index="0"` 开始，封面页为 0
4. **动画类**: 需要动画的元素添加 `anim-item` 类
5. **颜色语义**:
   - 青绿(cyan): 主要信息、正常状态
   - 蓝(blue): 辅助信息、技术方案
   - 黄(yellow): 警告、重点、卡点
   - 红(red): 错误、问题、风险
   - 紫(purple): 技能、模型、高级功能
6. **字体大小**: 根字体 20px，确保投影清晰
7. **导航栏**: 固定在底部 64px 高度，毛玻璃效果
8. **激光笔**: 默认隐藏，鼠标移动时显示

## 生成步骤

1. 确认幻灯片数量和每页内容
2. 按顺序编写 HTML 结构（封面 → 目录 → 章节分隔 → 内容页 → 结束页）
3. 为需要动画的元素添加 `anim-item` 类
4. 更新导航栏中的总页数
5. 验证所有 `data-index` 连续且从 0 开始

## 模板文件

完整的 2 页 HTML 模板已保存在 `assets/template.html` 中，包含：
- 封面页（Hero Slide）
- 内容页（3 列卡片布局）
- 完整的 CSS 样式和 JavaScript 交互

### 使用方式

1. **读取模板**: 从 `assets/template.html` 读取完整代码
2. **复制模板**: 将模板代码复制到新文件中
3. **修改内容**: 根据需求修改标题、幻灯片数量、卡片内容等
4. **更新计数**: 修改导航栏中的页数显示（如 `1 / 2` 改为 `1 / N`）
5. **保持样式**: 不要修改 CSS 变量和核心样式，确保视觉一致性

### 模板结构

```
封面页 (data-index="0")
  ├── hero-badge（标签）
  ├── h1（主标题）
  ├── p（副标题）
  └── hero-meta（日期/演讲者/技术栈）

内容页 (data-index="1")
  ├── slide-number（章节编号）
  ├── slide-title（标题）
  └── content-grid（3 列卡片）
```

### 添加新页面

在 `slides-wrapper` 中添加新的 `<div class="slide">` 元素：

```html
<!-- ═══════════ SLIDE N: 页面标题 ══════════ -->
<div class="slide" data-index="N">
  <div class="slide-number anim-item">PART X / 子标题</div>
  <div class="slide-title anim-item">页面标题 <span class="gradient-text">高亮</span></div>
  <!-- 内容区域 -->
</div>
```

### 注意事项

- 第一页（封面）必须包含 `active` 类
- 所有 `data-index` 必须连续且从 0 开始
- 需要动画的元素添加 `anim-item` 类
- 导航栏中的页数必须与实际幻灯片数量一致
