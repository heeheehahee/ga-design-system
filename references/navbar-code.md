---
category: components-code
name: 页面结构代码
description: 标题栏 + 底 Tab 栏的 CSS + JS 代码。设计规范见 navbar.md
---

# 页面结构代码

设计规范：[navbar.md](navbar.md)
图标资源：[icons.md](icons.md)

---

## 标题栏

### 通用样式

图标板子有「平面态」和「浮起态（玻璃材质）」两种视觉。所有标题栏共享以下基础样式：

```css
/* 图标板子 — 平面态（默认） */
.navbar-icon {
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 999px;
  cursor: pointer;
  color: var(--on_surface);
  background: transparent;
  transition: background 0.2s, box-shadow 0.3s;
}
.navbar-icon:active:not(.glass-btn) { opacity: 0.6; }
.navbar-icon[disabled] { opacity: 0.3; pointer-events: none; }
.navbar-icon svg { width: 40px; height: 40px; }
.navbar-icons-right,
.navbar-icons-left { display: flex; gap: 8px; }

/* 图标板子 — 浮起态：复用通用玻璃材质（.glass-btn 定义在 button-code.md） */
.navbar-icon.glass-btn { padding: 10px 12px; }

/* 深色模式覆盖 */
.theme-dark .navbar-icon { color: rgba(255,255,255,0.95); }
.theme-dark .navbar-icon:active:not(.glass-btn) { opacity: 0.5; }
.theme-dark .navbar-icon[disabled] { opacity: 0.4; }
```

### 大标题 H5

```html
<div class="navbar-lg">
  <div class="navbar-topbar">
    <div class="navbar-icon"><!-- 返回图标 SVG 从 icons.md 取 --></div>
    <div class="navbar-icons-right">
      <div class="navbar-icon"><!-- 编辑图标 --></div>
      <div class="navbar-icon"><!-- 搜索图标 --></div>
      <div class="navbar-icon"><!-- 更多图标 --></div>
    </div>
  </div>
  <div class="navbar-title-area">
    <div class="navbar-title-lg">页面标题</div>
    <!-- <div class="navbar-subtitle-lg">辅助标题</div> -->
  </div>
</div>
```

```css
.navbar-lg {
  width: 100%;
  backdrop-filter: blur(66px);
}
.navbar-topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 56px;
  padding: 6px 12px;
  gap: 48px;
}
.navbar-title-area {
  padding: 8px 28px 10px;
}
.navbar-title-lg {
  font-size: 32px;
  font-weight: 330;
  color: var(--on-surface);
}
.navbar-subtitle-lg {
  font-size: 14px;
  font-weight: 380;
  color: rgba(0,0,0,0.6);
}
.theme-dark .navbar-subtitle-lg { color: rgba(255,255,255,0.6); }
```

### 中标题Q18 H5

一级页面无返回按钮，标题左对齐，左边距加大。

```html
<div class="navbar-md-q18">
  <div class="navbar-md-q18__container">
    <div class="navbar-md-q18__title-group">
      <div class="navbar-md-q18__title">页面标题</div>
      <!-- <div class="navbar-md-q18__subtitle">辅助标题</div> -->
    </div>
    <div class="navbar-icons-right">
      <div class="navbar-icon"><!-- 搜索图标 --></div>
      <div class="navbar-icon"><!-- 更多图标 --></div>
    </div>
  </div>
</div>
```

```css
.navbar-md-q18 {
  width: 100%;
  height: 56px;
  padding: 6px 12px 6px 28px;
  backdrop-filter: blur(66px);
}
.navbar-md-q18__container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 44px;
}
.navbar-md-q18__title {
  font-size: 26px;
  font-weight: 380;
  color: var(--on-surface);
}
.navbar-md-q18__subtitle {
  font-size: 13px;
  font-weight: 380;
  color: rgba(0,0,0,0.6);
}
.theme-dark .navbar-md-q18__subtitle { color: rgba(255,255,255,0.6); }
```

### 中标题二级页 H5

有返回按钮，标题左对齐。

```html
<div class="navbar-md-sub">
  <div class="navbar-md-sub__container">
    <div class="navbar-icon"><!-- 返回图标 --></div>
    <div class="navbar-md-sub__title-group">
      <div class="navbar-md-sub__title">页面标题</div>
      <!-- <div class="navbar-md-sub__subtitle">辅助标题</div> -->
    </div>
    <div class="navbar-icons-right">
      <div class="navbar-icon"><!-- 搜索图标 --></div>
      <div class="navbar-icon"><!-- 更多图标 --></div>
    </div>
  </div>
</div>
```

```css
.navbar-md-sub {
  width: 100%;
  height: 56px;
  padding: 6px 12px;
  backdrop-filter: blur(66px);
}
.navbar-md-sub__container {
  display: flex;
  align-items: center;
  height: 44px;
  gap: 12px;
}
.navbar-md-sub__title {
  font-size: 26px;
  font-weight: 380;
  color: var(--on-surface);
  flex: 1;
}
.navbar-md-sub__subtitle {
  font-size: 13px;
  font-weight: 380;
  color: rgba(0,0,0,0.6);
}
.theme-dark .navbar-md-sub__subtitle { color: rgba(255,255,255,0.6); }
```

### 小标题 H5

```html
<div class="navbar-sm">
  <div class="navbar-sm__left">
    <div class="navbar-icon"><!-- 返回图标 --></div>
  </div>
  <div class="navbar-sm__title-group">
    <div class="navbar-sm__title">标题名称</div>
    <!-- <div class="navbar-sm__subtitle">辅助标题</div> -->
  </div>
  <div class="navbar-sm__right">
    <div class="navbar-icon"><!-- 搜索图标 --></div>
    <div class="navbar-icon"><!-- 更多图标 --></div>
  </div>
</div>
```

```css
.navbar-sm {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 56px;
  padding: 6px 12px;
  backdrop-filter: blur(66px);
}
.navbar-sm__left, .navbar-sm__right { display: flex; gap: 8px; }
.navbar-sm__title-group {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  white-space: nowrap;
}
.navbar-sm__title {
  font-size: 18px;
  font-weight: 380;
  color: var(--on-surface);
}
.navbar-sm__subtitle {
  font-size: 13px;
  font-weight: 380;
  color: rgba(0,0,0,0.6);
}
.theme-dark .navbar-sm__subtitle { color: rgba(255,255,255,0.6); }
```

### 小标题可点击 H5

```html
<div class="navbar-sm-click">
  <div class="navbar-icons-left">
    <div class="navbar-icon"><!-- 图标1 --></div>
    <div class="navbar-icon"><!-- 图标2 --></div>
    <div class="navbar-icon"><!-- 图标3 --></div>
  </div>
  <div class="navbar-sm-click__pill">可点击文案</div>
  <div class="navbar-icons-right">
    <div class="navbar-icon"><!-- 图标1 --></div>
    <div class="navbar-icon"><!-- 图标2 --></div>
    <div class="navbar-icon"><!-- 图标3 --></div>
  </div>
</div>
```

```css
.navbar-sm-click {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 56px;
  padding: 6px 12px;
  gap: 8px;
  backdrop-filter: blur(66px);
}
.navbar-sm-click__pill {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 24px;
  border-radius: 1000px;
  height: 44px;
  background:
    linear-gradient(rgba(116,116,116,0.9), rgba(116,116,116,0.9)),
    linear-gradient(rgba(209,209,209,0.2), rgba(209,209,209,0.2));
  font-size: 14px;
  font-weight: 380;
  color: rgba(0,0,0,0.6);
  cursor: pointer;
}
.theme-dark .navbar-sm-click__pill { color: rgba(255,255,255,0.6); }
```

### 大标题 → 小标题滚动切换 JS

```js
const scrollArea = document.querySelector('.scroll-content');
const navLg = document.querySelector('.navbar-lg');
const navSm = document.querySelector('.navbar-sm');
navSm.style.display = 'none';

let collapsed = false;
scrollArea.addEventListener('scroll', () => {
  if (scrollArea.scrollTop > 20 && !collapsed) {
    collapsed = true;
    navLg.style.display = 'none';
    navSm.style.display = 'flex';
    // 浮起态：图标切换为玻璃材质
    navSm.querySelectorAll('.navbar-icon').forEach(el => el.classList.add('glass-btn'));
  } else if (scrollArea.scrollTop <= 20 && collapsed) {
    collapsed = false;
    navLg.style.display = 'block';
    navSm.style.display = 'none';
    navSm.querySelectorAll('.navbar-icon').forEach(el => el.classList.remove('glass-btn'));
  }
});
```

---

## 底 Tab 栏

### CSS

```css
.tab-bar {
  display: flex;
  align-items: center;
  width: 100%;
  height: 81px;
  padding: 0 8px 16px;
  background: linear-gradient(rgba(0,0,0,0.32), rgba(0,0,0,0.32)), linear-gradient(rgba(0,0,0,0.46), rgba(0,0,0,0.46));
  backdrop-filter: blur(70px);
  -webkit-backdrop-filter: blur(70px);
  position: relative;
}
.tab-bar::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 0.5px;
  background: rgba(255,255,255,0.15);
}
body.theme-light .tab-bar {
  background: linear-gradient(rgba(247,247,247,0.7), rgba(247,247,247,0.7)), linear-gradient(rgba(249,249,249,0.2), rgba(249,249,249,0.2));
}
body.theme-light .tab-bar::before {
  background: rgba(0,0,0,0.1);
}
.tab-bar__item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  flex: 1;
  height: 44px;
  cursor: pointer;
  color: var(--text-weak);
  transition: color 0.2s;
  border: none;
  background: none;
  font-family: inherit;
  padding: 0;
  outline: none;
}
.tab-bar__item--active { color: #FF6900; }
.tab-bar__item:active .tab-bar__icon { transform: scale(0.85); }
.tab-bar__icon {
  width: 28px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  transition: transform 0.15s ease;
}
.tab-bar__icon svg { width: 28px; height: 28px; }
.tab-bar__label { font-size: 11px; font-weight: 380; }
.tab-bar__badge {
  position: absolute;
  top: -4px;
  right: -8px;
  min-width: 16px;
  height: 16px;
  border-radius: 999px;
  background: #FA382E;
  color: #fff;
  font-size: 10px;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 4px;
}
```

### JS

```js
class TabBar {
  constructor(container, { tabs = [], active = 0 } = {}) {
    this.container = typeof container === 'string' ? document.querySelector(container) : container;
    this.tabs = tabs;
    this.active = active;
    this.onSelect = null;
    this._build();
  }
  _build() {
    this.el = document.createElement('div');
    this.el.className = 'tab-bar';
    this._render();
    this.container.innerHTML = '';
    this.container.appendChild(this.el);
  }
  _render() {
    this.el.innerHTML = '';
    this.tabs.forEach((tab, i) => {
      const item = document.createElement('button');
      item.className = 'tab-bar__item';
      if (i === this.active) item.classList.add('tab-bar__item--active');

      const iconWrap = document.createElement('div');
      iconWrap.className = 'tab-bar__icon';
      iconWrap.innerHTML = tab.icon || '';
      if (tab.badge !== undefined && tab.badge !== null) {
        iconWrap.innerHTML += `<span class="tab-bar__badge">${tab.badge}</span>`;
      }
      item.appendChild(iconWrap);

      const label = document.createElement('span');
      label.className = 'tab-bar__label';
      label.textContent = tab.label || '';
      item.appendChild(label);

      item.addEventListener('click', () => {
        this.active = i;
        this._render();
        if (this.onSelect) this.onSelect(i);
      });
      this.el.appendChild(item);
    });
  }
  setActive(index) { this.active = index; this._render(); }
  setBadge(index, value) {
    if (this.tabs[index]) { this.tabs[index].badge = value; this._render(); }
  }
}
```

### 图标

Tab 图标 SVG 从 [icons.md](icons.md) 的「底 Tab 图标」章节取，共 5 个：

| 变量名 | 文件 | 说明 |
|--------|------|------|
| `tab_home` | tab-home.svg | 首页 |
| `tab_games` | tab-games.svg | 游戏 |
| `tab_gamehub` | tab-gamehub.svg | 社区 |
| `tab_topcharts` | tab-topcharts.svg | 排行榜 |
| `tab_profile` | tab-profile.svg | 个人中心 |

> 所有图标 `fill="currentColor"`，无 `fill-opacity`。颜色完全由 CSS `color` 控制：
> - 未选中：`var(--text-weak)` 灰色
> - 选中：`#FF6900` 橙色（固定，不区分明暗）

使用时先从 icons.md 复制 SVG 代码定义为变量，再传入 TabBar：

```js
// 1. 从 icons.md 复制 SVG 代码
const TAB_ICONS = {
  tab_home: `<svg ...>...</svg>`,
  tab_games: `<svg ...>...</svg>`,
  tab_gamehub: `<svg ...>...</svg>`,
  tab_topcharts: `<svg ...>...</svg>`,
  tab_profile: `<svg ...>...</svg>`
};

// 2. 创建 Tab 栏
const tabBar = new TabBar('#container', {
  tabs: [
    { icon: TAB_ICONS.tab_home, label: 'Home' },
    { icon: TAB_ICONS.tab_games, label: 'Games' },
    { icon: TAB_ICONS.tab_gamehub, label: 'GameHub' },
    { icon: TAB_ICONS.tab_topcharts, label: 'Top charts' },
    { icon: TAB_ICONS.tab_profile, label: 'Profile', badge: 8 }
  ],
  active: 0
});

// 3. API
tabBar.setActive(2);           // 切换选中
tabBar.setBadge(4, 3);         // 设置角标
tabBar.onSelect = (i) => {};   // 监听选中
```

---

## 完整页面骨架模板

**做页面时直接复制此模板作为起点，不要自己重写标题栏/Tab 栏。**

### :root 变量（必须原样复制，变量名用下划线）

```css
/* 暗色模式（默认） */
:root {
  --surface: #000000;
  --surface_high: #242424;
  --surface_pop_window: #242424;
  --surface_container: #ffffff14;
  --surface_container_medium: #ffffff24;
  --on_surface: #fffffff2;
  --on_surface_secondary: #ffffffcc;
  --on_surface_tertiary: #ffffff80;
  --on_surface_quaternary: #ffffff66;
  --on_surface_octonary: #ffffff4d;
  --primary: #4788ff;
  --mask: #00000099;
  --bb-primary: #05C474;
  --bb-primary-rgb: 5, 196, 116;
  --btn-on-primary: #ffffffe6;
  --btn-secondary-bg: #ffffff24;
  --btn-secondary-text: #ffffffcc;
  --btn-secondary-pressed: #ffffff1f;
  --btn-disabled-bg: #ffffff14;
  --btn-disabled-text: #ffffff4d;
  --btn-destructive: #fa4238;
  --btn-surface-touch: #ffffff24;
}
/* 亮色模式 */
body.theme-light {
  --surface: #ffffff;
  --surface_high: #ffffff;
  --surface_pop_window: #f3f3f3;
  --surface_container: #0000000a;
  --surface_container_medium: #00000014;
  --on_surface: #000000;
  --on_surface_secondary: #000000cc;
  --on_surface_tertiary: #00000099;
  --on_surface_quaternary: #00000066;
  --on_surface_octonary: #0000004c;
  --primary: #3482ff;
  --mask: #00000033;
  --bb-primary: #05C474;
  --bb-primary-rgb: 5, 196, 116;
  --btn-on-primary: #ffffff;
  --btn-secondary-bg: #0000000f;
  --btn-secondary-text: #000000cc;
  --btn-secondary-pressed: #0000001f;
  --btn-disabled-bg: #0000000a;
  --btn-disabled-text: #00000066;
  --btn-destructive: #fa382e;
  --btn-surface-touch: #0000001a;
}
```

### HTML 骨架（直接复制）

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>页面标题</title>
<link rel="stylesheet" href="style.css">
</head>
<body class="theme-light">
<div class="page">

  <!-- 标题栏 — 选一种类型，代码从上方对应章节复制 -->
  <div class="navbar-sm">
    <div class="navbar-sm__left">
      <div class="navbar-icon"><!-- 返回图标 SVG 从 icons.md 取 --></div>
    </div>
    <div class="navbar-sm__title-group"><div class="navbar-sm__title">页面标题</div></div>
    <div class="navbar-sm__right">
      <div class="navbar-icon"><!-- 更多图标 SVG 从 icons.md 取 --></div>
    </div>
  </div>

  <!-- 内容区 -->
  <div class="content" style="padding:0 20px">
    <!-- 页面内容 -->
  </div>

  <!-- 底 Tab 栏 65px（一级页面显示，二级页面删除此块） -->
  <div id="tab-bar-container"></div>

</div>
<script src="script.js"></script>
</body>
</html>
```

### 页面容器 CSS

```css
.page {
  width: 100%;
  min-height: 100vh;
  background: var(--surface);
  color: var(--on_surface);
  font-family: "MiSans Latin VF", -apple-system, sans-serif;
}
.content { padding: 0 20px; }
```

### 说明

- **标题栏**：从上方「大标题/中标题Q18/中标题二级页/小标题/小标题可点击」5 种中选一种，整块复制 HTML + CSS
- **浮起态**：如需滚动切换玻璃材质，给 `.navbar-icon` 加 `.glass-btn` class，`.glass-btn` CSS 从 button-code.md 复制
- **底 Tab 栏**：用上方的 TabBar JS class + TAB_ICONS，图标从 icons.md 取
- **变量名**：全部用下划线（`--on_surface`），不能改成连字符（`--on-surface`）
