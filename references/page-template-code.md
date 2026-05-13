---
category: template
name: 页面模板
description: 空白页面骨架 + DebugTool。新建页面时从此文件提取 HTML/CSS/JS 三段代码。
---

# 页面模板

新建页面的起点。包含 392px 页面容器、完整 Token 变量、DebugTool 调试工具。

**代码来源**：

| 模块 | 来源 | 适配说明 |
|------|------|---------|
| Token 变量 | navbar-code.md `:root` + color.md 补全 | — |
| 页面容器 | navbar-code.md `.page` `.content` | — |
| Navbar 图标 | navbar-code.md `.navbar-icon` | — |
| Drawer | drawer-code.md | `position: fixed` → `absolute`（在 .page 容器内定位） |
| PopupMenu | popup-menu-code.md | — |
| List | list-code.md | — |
| DebugTool | debug-tool-code.md | — |

---

## HTML (index.html)

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>New Page</title>
<link rel="stylesheet" href="style.css">
</head>
<body class="theme-light">
<div class="page">

  <!-- 系统状态栏占位 46px -->
  <div style="height:46px;flex-shrink:0"></div>

  <!-- 内容区 -->
  <div class="content">
    <!-- 页面内容从这里开始 -->
  </div>

</div>
<script src="script.js"></script>
</body>
</html>
```

---

## CSS (style.css)

```css
/* ====== Design Tokens (from navbar-code.md + color.md) ====== */
:root {
  --surface: #000000;
  --surface_low: #000000;
  --surface_high: #242424;
  --surface_pop_window: #242424;
  --surface_container: #ffffff14;
  --surface_container_low: #ffffff24;
  --surface_container_medium: #ffffff24;
  --on_surface: #fffffff2;
  --on_surface_secondary: #ffffffcc;
  --on_surface_tertiary: #ffffff80;
  --on_surface_quaternary: #ffffff66;
  --on_surface_octonary: #ffffff4d;
  --on_surface_right_button_bg: #ffffff33;
  --primary: #4788ff;
  --mask: #00000099;
  --outline: #ffffff33;
  --container_list: #ffffff24;
  --gray: #92a1b4;
  --white: #ffffff;
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
  --error: #fa4238;
}
body.theme-light {
  --surface: #ffffff;
  --surface_low: #f7f7f7;
  --surface_high: #ffffff;
  --surface_pop_window: #f3f3f3;
  --surface_container: #0000000a;
  --surface_container_low: #0000000f;
  --surface_container_medium: #00000014;
  --on_surface: #000000;
  --on_surface_secondary: #000000cc;
  --on_surface_tertiary: #00000099;
  --on_surface_quaternary: #00000066;
  --on_surface_octonary: #0000004c;
  --on_surface_right_button_bg: #0000000f;
  --primary: #3482ff;
  --mask: #00000033;
  --outline: #0000001a;
  --container_list: #ffffff;
  --gray: #8c9db0;
  --white: #ffffff;
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
  --error: #fa382e;
}

/* ====== Page (from navbar-code.md) ====== */
* { margin: 0; padding: 0; box-sizing: border-box; }
body {
  background: var(--surface_low);
  display: flex;
  justify-content: center;
  min-height: 100vh;
}
.page {
  position: relative;
  width: 100%;
  height: 100vh;
  background: var(--surface);
  color: var(--on_surface);
  font-family: "MiSans Latin VF", -apple-system, sans-serif;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
@media (min-width: 768px) {
  .page { max-width: 392px; }
}
.content { flex: 1; padding: 0 20px; overflow-y: auto; }

/* ====== Navbar Icon (from navbar-code.md) ====== */
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

/* ====== Drawer (from drawer-code.md — position: fixed→absolute) ====== */
/* ====== Drawer (from drawer-code.md — position: fixed→absolute) ====== */
/* Drawer 底部抽屉 */
.drawer-mask {
  position: absolute;
  inset: 0;
  background: var(--mask);
  z-index: 900;
  opacity: 0;
  transition: opacity 0.3s ease;
  pointer-events: none;
}

.drawer-mask--visible {
  opacity: 1;
  pointer-events: auto;
}

.drawer {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  border-radius: 36px 36px 0 0;
  background: var(--surface_pop_window);
  z-index: 901;
  transform: translateY(100%);
  transition: transform 0.3s ease;
  display: flex;
  flex-direction: column;
}

.drawer--visible {
  transform: translateY(0);
}

/* 高度模式 */
/* 默认：内容自适应，最大 70vh，超出时内容区滚动 */
.drawer { max-height: 70vh; }

/* 全屏模式 — 顶部留 52px */
.drawer--fullscreen { height: calc(100vh - 52px); max-height: calc(100vh - 52px); }

/* 自定义高度 */
.drawer--custom { max-height: none; /* height 由调用方指定 */ }

/* 拖拽手柄 */
.drawer__handle {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 24px;
  flex-shrink: 0;
}

.drawer__handle::after {
  content: '';
  width: 60px;
  height: 3px;
  border-radius: 1.5px;
  background: var(--outline);
}

/* 标题栏 */
.drawer__header {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 6px 12px;
  flex-shrink: 0;
}

.drawer__title {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  font-weight: 380;
  color: var(--on_surface);
  pointer-events: none;
}

/* 内容区 */
.drawer__content {
  flex: 1;
  overflow-y: auto;
  padding: 0 0 40px;
}

/* ====== PopupMenu (from popup-menu-code.md) ====== */
/* ====== PopupMenu 近手菜单 ====== */

.popup-menu-overlay {
  position: absolute;
  inset: 0;
  z-index: 50;
  background: var(--mask);
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.2s;
}
.popup-menu-overlay.visible {
  opacity: 1;
  pointer-events: auto;
}

.popup-menu {
  position: absolute;
  z-index: 51;
  width: 200px;
  background: var(--surface_pop_window);
  border-radius: 16px;
  box-shadow: 0 12px 40px rgba(0,0,0,0.15);
  opacity: 0;
  transform: scale(0.9) translateY(-8px);
  pointer-events: none;
  transition: opacity 0.2s, transform 0.2s;
  overflow-y: auto;
}
.popup-menu.visible {
  opacity: 1;
  transform: scale(1) translateY(0);
  pointer-events: auto;
}
.popup-menu--wide { width: 288px; }

/* 定位变体 */
.popup-menu--top-right { transform-origin: top right; }
.popup-menu--top-left { transform-origin: top left; }
.popup-menu--bottom-right { transform-origin: bottom right; }

/* 菜单项 */
.popup-menu__item {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 11px 20px;
  cursor: pointer;
}
.popup-menu__item:first-child { padding-top: 17px; }
.popup-menu__item:last-child { padding-bottom: 17px; }
.popup-menu__item:active { background: var(--btn-surface-touch); }

/* 文本区 */
.popup-menu__text { flex: 1; min-width: 0; }
.popup-menu__title {
  font-size: 16px;
  font-weight: 380;
  color: var(--on_surface);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.popup-menu__subtitle {
  font-size: 14px;
  font-weight: 330;
  color: var(--on_surface_tertiary);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* 选中态 */
.popup-menu__item--selected .popup-menu__title,
.popup-menu__item--selected .popup-menu__subtitle { color: var(--primary); }

/* 选中勾选图标 */
.popup-menu__check {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  background: var(--primary);
  -webkit-mask: url("data:image/svg+xml,%3Csvg width='20' height='20' viewBox='0 0 20 20' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M17.4687 6.28732C17.9089 5.75551 17.8347 4.96755 17.3028 4.52735C16.771 4.08715 15.9831 4.16141 15.5429 4.69322L8.94602 12.6629L5.53427 9.25116C5.04612 8.763 4.25466 8.763 3.76651 9.25116C3.27835 9.73931 3.27835 10.5308 3.76651 11.0189L8.14393 15.3964C8.50702 15.7594 9.03789 15.8525 9.48696 15.6754C9.68259 15.6012 9.86224 15.4767 10.0052 15.304L17.4687 6.28732Z' fill='black'/%3E%3C/svg%3E") center / contain no-repeat;
  mask: url("data:image/svg+xml,%3Csvg width='20' height='20' viewBox='0 0 20 20' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M17.4687 6.28732C17.9089 5.75551 17.8347 4.96755 17.3028 4.52735C16.771 4.08715 15.9831 4.16141 15.5429 4.69322L8.94602 12.6629L5.53427 9.25116C5.04612 8.763 4.25466 8.763 3.76651 9.25116C3.27835 9.73931 3.27835 10.5308 3.76651 11.0189L8.14393 15.3964C8.50702 15.7594 9.03789 15.8525 9.48696 15.6754C9.68259 15.6012 9.86224 15.4767 10.0052 15.304L17.4687 6.28732Z' fill='black'/%3E%3C/svg%3E") center / contain no-repeat;
}

/* 危险项 */
.popup-menu__item--destructive .popup-menu__title { color: var(--error); }

/* 分隔线 */
.popup-menu__divider {
  height: 1px;
  margin: 4px 0;
  background: var(--outline);
}

/* ====== List (from list-code.md) ====== */
/* List 列表 */
.list-container { padding: 0 12px; }
.list-group-title { padding: 20px 16px 8px; font-size: 14px; font-weight: 380; color: var(--gray); }
.list-footer { padding: 8px 16px 20px; font-size: 13px; font-weight: 330; color: var(--on_surface_tertiary); }
.list-card { background: var(--container_list); border-radius: 20px; overflow: hidden; }

/* 列表行 */
.list-item { display: flex; align-items: center; gap: 14px; padding: 14px 16px; position: relative; }
.list-item--standalone { border-radius: 20px; }
.list-item--top { border-radius: 20px 20px 0 0; }
.list-item--middle { border-radius: 0; }
.list-item--bottom { border-radius: 0 0 20px 20px; }
.list-item--top::after, .list-item--middle::after { content: ''; position: absolute; left: 16px; right: 16px; bottom: 0; height: 1px; background: var(--outline); }
.list-item--no-divider::after { display: none; }
.list-item:active { background: var(--surface_container); }

/* 文本区 */
.list-item__text { flex: 1; min-width: 0; display: flex; flex-direction: column; }
.list-item__title { font-size: 17px; font-weight: 380; color: var(--on_surface); flex: 1; }
.list-item__desc { font-size: 14px; font-weight: 330; color: var(--on_surface_tertiary); margin-top: 4px; }

/* 右侧功能区 */
.list-item__trailing { display: flex; align-items: center; flex-shrink: 0; gap: 8px; }

/* 单独进入二级 — 28×28 圆底 + chevron */
.list-item__trailing--arrow { width: 28px; height: 28px; border-radius: 50%; background: var(--on_surface_right_button_bg); display: flex; align-items: center; justify-content: center; }
.list-item__trailing--arrow::after { content: ''; width: 28px; height: 28px; background: var(--on_surface_octonary); -webkit-mask: url("data:image/svg+xml,%3Csvg width='28' height='28' viewBox='0 0 28 28' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M11.8323 9.55351C12.263 9.12279 12.9613 9.12279 13.392 9.55351L17.0401 13.2016C17.329 13.4905 17.4241 13.8996 17.3256 14.2678C17.2807 14.4622 17.1824 14.6468 17.0309 14.7984L13.3828 18.4465C12.9521 18.8772 12.2538 18.8772 11.823 18.4465C11.3923 18.0157 11.3923 17.3174 11.823 16.8867L14.7143 13.9954L11.8323 11.1133C11.4015 10.6826 11.4015 9.98424 11.8323 9.55351Z' fill='black'/%3E%3C/svg%3E") center / contain no-repeat; mask: url("data:image/svg+xml,%3Csvg width='28' height='28' viewBox='0 0 28 28' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M11.8323 9.55351C12.263 9.12279 12.9613 9.12279 13.392 9.55351L17.0401 13.2016C17.329 13.4905 17.4241 13.8996 17.3256 14.2678C17.2807 14.4622 17.1824 14.6468 17.0309 14.7984L13.3828 18.4465C12.9521 18.8772 12.2538 18.8772 11.823 18.4465C11.3923 18.0157 11.3923 17.3174 11.823 16.8867L14.7143 13.9954L11.8323 11.1133C11.4015 10.6826 11.4015 9.98424 11.8323 9.55351Z' fill='black'/%3E%3C/svg%3E") center / contain no-repeat; }

/* 整个列表进入二级 — 裸箭头 8×14 */
.list-item__trailing--arrow-bare { width: 8px; height: 14px; flex-shrink: 0; background: var(--on_surface_octonary); -webkit-mask: url("data:image/svg+xml,%3Csvg width='8' height='14' viewBox='0 0 8 14' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1 1L7 7L1 13' stroke='black' stroke-width='1.5' stroke-linecap='round' stroke-linejoin='round'/%3E%3C/svg%3E") center / contain no-repeat; mask: url("data:image/svg+xml,%3Csvg width='8' height='14' viewBox='0 0 8 14' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1 1L7 7L1 13' stroke='black' stroke-width='1.5' stroke-linecap='round' stroke-linejoin='round'/%3E%3C/svg%3E") center / contain no-repeat; }

/* 右侧纯文字 */
.list-item__trailing-text { font-size: 14px; font-weight: 380; color: var(--on_surface_tertiary); flex-shrink: 0; }

/* 状态切换（状态文字 + 上下箭头） */
.list-item__trailing--status { display: flex; align-items: center; gap: 8px; cursor: pointer; }
.list-item__trailing--status-text { font-size: 14px; font-weight: 330; color: var(--on_surface_quaternary); }
.list-item__trailing--status-arrow { width: 10px; height: 16px; color: var(--on_surface_octonary); opacity: 0.7; display: flex; }
.list-item__trailing--status-arrow svg { width: 10px; height: 16px; }

/* 左侧图标区 */
.list-item__icon { width: 28px; height: 28px; border-radius: 8px; background: var(--surface_container); display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.list-item__icon--lg { width: 40px; height: 40px; border-radius: 10px; border: 0.36px solid var(--outline); }
.list-item__icon svg { width: 24px; height: 24px; }

/* 单选勾选标记 — 20×20, Figma 导出 path */
.list-item__checkmark { width: 20px; height: 20px; flex-shrink: 0; background: var(--primary); -webkit-mask: url("data:image/svg+xml,%3Csvg width='20' height='20' viewBox='0 0 20 20' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M17.4687 6.28732C17.9089 5.75551 17.8347 4.96755 17.3028 4.52735C16.771 4.08715 15.9831 4.16141 15.5429 4.69322L8.94602 12.6629L5.53427 9.25116C5.04612 8.763 4.25466 8.763 3.76651 9.25116C3.27835 9.73931 3.27835 10.5308 3.76651 11.0189L8.14393 15.3964C8.50702 15.7594 9.03789 15.8525 9.48696 15.6754C9.68259 15.6012 9.86224 15.4767 10.0052 15.304L17.4687 6.28732Z' fill='black'/%3E%3C/svg%3E") center / contain no-repeat; mask: url("data:image/svg+xml,%3Csvg width='20' height='20' viewBox='0 0 20 20' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M17.4687 6.28732C17.9089 5.75551 17.8347 4.96755 17.3028 4.52735C16.771 4.08715 15.9831 4.16141 15.5429 4.69322L8.94602 12.6629L5.53427 9.25116C5.04612 8.763 4.25466 8.763 3.76651 9.25116C3.27835 9.73931 3.27835 10.5308 3.76651 11.0189L8.14393 15.3964C8.50702 15.7594 9.03789 15.8525 9.48696 15.6754C9.68259 15.6012 9.86224 15.4767 10.0052 15.304L17.4687 6.28732Z' fill='black'/%3E%3C/svg%3E") center / contain no-repeat; }

/* 选中态 */
.list-item--selected .list-item__title { color: var(--primary); }


/* ====== DebugTool (from debug-tool-code.md) ====== */
/* ====== DebugTool 页面调试工具 ====== */

/* FAB 按钮 */
.ctrl-fab {
  position: absolute;
  right: 16px;
  bottom: 126px;
  width: 52px;
  height: 52px;
  border-radius: 50%;
  background: var(--primary);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 20;
  color: var(--white);
  box-shadow: 0 2px 8px rgba(0,0,0,0.3);
  touch-action: none;
  user-select: none;
}
.ctrl-fab:active { opacity: 0.85; }
.ctrl-fab.dragging { transition: none; }
.ctrl-fab.snapping { transition: left 0.25s ease, right 0.25s ease; }

/* FAB 收起竖条态 */
.ctrl-fab--bar {
  width: 5px;
  height: 64px;
  border-radius: 2.5px;
  left: 8px !important;
  right: auto !important;
  opacity: 0.7;
  transition: width 0.25s ease, border-radius 0.25s ease, opacity 0.25s ease, left 0.25s ease, right 0.25s ease;
  box-shadow: none;
}
.ctrl-fab--bar::after {
  content: '';
  position: absolute;
  inset: -12px -20px -12px -12px;
}
.ctrl-fab--bar img, .ctrl-fab--bar svg { display: none; }
.ctrl-fab--bar-right {
  border-radius: 2.5px;
  left: auto !important;
  right: 8px !important;
}
.ctrl-fab--bar-right::after {
  inset: -12px -12px -12px -20px;
}

/* 调试工具开关（系统蓝） */
.ctrl-switch {
  position: relative;
  width: 49px;
  height: 28px;
  border-radius: 14px;
  background: var(--outline);
  cursor: pointer;
  transition: background 0.2s ease;
  flex-shrink: 0;
}
.ctrl-switch::after {
  content: '';
  position: absolute;
  top: 4px;
  left: 4px;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: var(--white);
  transition: transform 0.2s ease;
  box-shadow: 0 1px 3px rgba(0,0,0,0.3);
}
.ctrl-switch--on { background: var(--primary); }
.ctrl-switch--on::after { transform: translateX(21px); }

/* 底部说明文字 */
.ctrl-note {
  padding: 20px;
  font-size: 12px;
  color: var(--on_surface_octonary);
  text-align: center;
  line-height: 1.6;
}
```

---

## JS (script.js)

```js
/* ====== Drawer (from drawer-code.md) ====== */
/**
 * Drawer
 * 用法：new Drawer({ size, title, onClose })
 */
class Drawer {
  constructor(opts = {}) {
    this.size = opts.size || 'medium';
    this.title = opts.title || '';
    this.onClose = opts.onClose || null;
    this.container = opts.container || document.querySelector('.page') || document.body;
    this.visible = false;

    this.mask = document.createElement('div');
    this.mask.className = 'drawer-mask';
    this.mask.addEventListener('click', () => this.close());

    this.el = document.createElement('div');
    this.el.className = `drawer drawer--${this.size}`;
    this.el.innerHTML = `
      <div class="drawer__handle"></div>
      <div class="drawer__header">
        <span class="navbar-icon" style="cursor:pointer">${this.getCloseIcon()}</span>
        <span class="drawer__title">${this.title}</span>
        <span class="navbar-icon" style="visibility:hidden">${this.getCloseIcon()}</span>
      </div>
      <div class="drawer__content"></div>
    `;

    this.el.querySelector('.navbar-icon').addEventListener('click', () => this.close());
    this._initDrag();
    this.container.appendChild(this.mask);
    this.container.appendChild(this.el);
  }

  getCloseIcon() {
    return '<svg width="40" height="40" viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M19.965 18.739L27.42 11.242C27.56 11.088 27.707 11.011 27.861 11.011C28.015 11.011 28.162 11.088 28.302 11.242L28.764 11.704C28.904 11.844 28.974 11.991 28.974 12.145C28.974 12.285 28.904 12.432 28.764 12.586L21.309 20.083L28.785 27.559C28.939 27.699 29.016 27.846 29.016 28C29.016 28.154 28.939 28.301 28.785 28.441L28.323 28.903C28.029 29.197 27.735 29.197 27.441 28.903L19.965 21.427L12.468 28.882C12.328 29.022 12.181 29.092 12.027 29.092C11.873 29.092 11.726 29.022 11.586 28.882L11.124 28.42C10.984 28.28 10.914 28.133 10.914 27.979C10.914 27.825 10.984 27.678 11.124 27.538L18.621 20.083L11.145 12.586C11.005 12.432 10.935 12.285 10.935 12.145C10.935 11.991 11.005 11.844 11.145 11.704L11.607 11.242C11.761 11.088 11.908 11.011 12.048 11.011C12.202 11.011 12.349 11.088 12.489 11.242L19.965 18.739Z" fill="currentColor"/></svg>';
  }

  _initDrag() {
    const handle = this.el.querySelector('.drawer__handle');
    let startY = 0;
    let dragging = false;

    const onStart = (e) => {
      startY = e.touches ? e.touches[0].clientY : e.clientY;
      dragging = true;
      this.el.style.transition = 'none';
    };
    const onMove = (e) => {
      if (!dragging) return;
      const y = e.touches ? e.touches[0].clientY : e.clientY;
      const dy = y - startY;
      if (dy > 0) this.el.style.transform = `translateY(${dy}px)`;
    };
    const onEnd = (e) => {
      if (!dragging) return;
      dragging = false;
      this.el.style.transition = '';
      const y = e.changedTouches ? e.changedTouches[0].clientY : e.clientY;
      if (y - startY > 80) {
        this.close();
      } else {
        this.el.style.transform = 'translateY(0)';
      }
    };

    handle.addEventListener('touchstart', onStart, { passive: true });
    handle.addEventListener('touchmove', onMove, { passive: true });
    handle.addEventListener('touchend', onEnd);
    handle.addEventListener('mousedown', onStart);
    document.addEventListener('mousemove', onMove);
    document.addEventListener('mouseup', onEnd);
  }

  open() {
    this.visible = true;
    this.mask.classList.add('drawer-mask--visible');
    this.el.classList.add('drawer--visible');
  }

  close() {
    this.visible = false;
    this.mask.classList.remove('drawer-mask--visible');
    this.el.classList.remove('drawer--visible');
    if (this.onClose) this.onClose();
  }

  setContent(html) {
    this.el.querySelector('.drawer__content').innerHTML = html;
  }
}

/* ====== PopupMenu (from popup-menu-code.md) ====== */
class PopupMenu {
  constructor(opts = {}) {
    this.anchor = opts.anchor;
    this.container = opts.container || document.querySelector('.page') || document.body;
    this.items = opts.items || [];
    this.position = opts.position || 'top-right';
    this.wide = opts.wide || false;
    this.onSelect = opts.onSelect || null;
    this._build();
  }

  _build() {
    this.overlay = document.createElement('div');
    this.overlay.className = 'popup-menu-overlay';
    this.overlay.addEventListener('click', () => this.close());

    this.menu = document.createElement('div');
    this.menu.className = 'popup-menu popup-menu--' + this.position + (this.wide ? ' popup-menu--wide' : '');

    this.items.forEach(item => {
      if (item === 'divider') {
        const d = document.createElement('div');
        d.className = 'popup-menu__divider';
        this.menu.appendChild(d);
        return;
      }
      const row = document.createElement('div');
      row.className = 'popup-menu__item' + (item.selected ? ' popup-menu__item--selected' : '') + (item.destructive ? ' popup-menu__item--destructive' : '');
      row.dataset.key = item.key;

      const text = document.createElement('div');
      text.className = 'popup-menu__text';
      text.innerHTML = '<div class="popup-menu__title">' + item.label + '</div>' + (item.subtitle ? '<div class="popup-menu__subtitle">' + item.subtitle + '</div>' : '');
      row.appendChild(text);

      if (item.selected) {
        const check = document.createElement('div');
        check.className = 'popup-menu__check';
        row.appendChild(check);
      }

      row.addEventListener('click', () => {
        this.close();
        if (this.onSelect) this.onSelect(item.key);
      });
      this.menu.appendChild(row);
    });

    this.container.appendChild(this.overlay);
    this.container.appendChild(this.menu);

    if (this.anchor) {
      this.anchor.addEventListener('click', () => this.open());
    }
  }

  // 定位规则：
  // 1. 位置固定紧贴锚点下方（top = anchor.bottom + 4px），不因超屏而移动位置
  // 2. 高度自适应收缩：max-height = 锚点到容器底部的剩余空间 - 8px，超长内容靠 overflow-y: auto 滚动
  // 3. 水平对齐：默认右对齐锚点右边缘（近手原则），position 为 top-left 时左对齐锚点左边缘
  open(anchorEl) {
    const a = anchorEl || this.anchor;
    if (a) {
      const rect = a.getBoundingClientRect();
      const cr = this.container.getBoundingClientRect();
      const top = rect.bottom - cr.top + 4;
      const availableH = cr.height - top - 8;
      this.menu.style.top = top + 'px';
      this.menu.style.maxHeight = availableH + 'px';
      if (this.position === 'top-left') {
        this.menu.style.left = (rect.left - cr.left) + 'px';
        this.menu.style.right = 'auto';
      } else {
        this.menu.style.right = (cr.right - rect.right) + 'px';
        this.menu.style.left = 'auto';
      }
    }
    this.overlay.classList.add('visible');
    this.menu.classList.add('visible');
  }

  close() {
    this.overlay.classList.remove('visible');
    this.menu.classList.remove('visible');
  }

  destroy() {
    this.overlay.remove();
    this.menu.remove();
  }
}

/* ====== DebugTool (from debug-tool-code.md) ====== */
/* ====== DebugTool 页面调试工具 ====== */

class DebugTool {
  /**
   * @param {Object} opts
   * @param {Array}  opts.controls - 控件分组配置
   *   [{ group: '分组名', items: [
   *     { type: 'switch', key: 'xxx', label: '标签', value: false },
   *     { type: 'select', key: 'xxx', label: '标签', options: [{key,label}], value: 'key' },
   *     { type: 'action', key: 'xxx', label: '标签' }
   *   ]}]
   * @param {Function} opts.onChange - function(key, value, tool) 控件变化回调
   * @param {string}   opts.icon - FAB 图标 HTML（默认内置调试图标）
   */
  constructor(opts) {
    opts = opts || {};
    this.controls = opts.controls || [];
    this.onChange = opts.onChange || null;
    this.icon = opts.icon || '<svg width="28" height="28" viewBox="3.5 3 29 30" fill="none"><path fill-rule="evenodd" clip-rule="evenodd" d="M26.2651 4.10612C26.9041 4.10612 27.424 4.05351 27.424 4.69258V17.1863C27.424 17.3284 27.5391 17.4435 27.6811 17.4435H29.4818C30.5185 17.4435 31.359 18.2839 31.359 19.3206V28.8323C31.359 30.8916 29.6896 32.5609 27.6304 32.5609H24.9033C22.844 32.5609 21.1747 30.8916 21.1747 28.8323V19.3206C21.1747 18.2839 22.0151 17.4435 23.0518 17.4435H24.8526C24.9946 17.4435 25.1097 17.3284 25.1097 17.1863V4.69258C25.1097 4.05351 25.626 4.10612 26.2651 4.10612ZM23.489 20.0149V28.8323C23.489 29.6134 24.1222 30.2466 24.9033 30.2466H27.6304C28.4115 30.2466 29.0447 29.6134 29.0447 28.8323V20.0149C29.0447 19.8729 28.9295 19.7578 28.7875 19.7578H23.7461C23.6041 19.7578 23.489 19.8729 23.489 20.0149ZM10.9721 5.15849C10.9721 4.31581 10.1002 3.75573 9.33384 4.10612C6.56696 5.37115 4.64062 8.16478 4.64062 11.4108C4.64062 14.0559 5.91982 16.4022 7.89332 17.8649C8.02908 17.9655 8.1128 18.1228 8.1128 18.2918V28.1291C8.1128 30.646 10.1532 32.6864 12.67 32.6864C15.1869 32.6864 17.2273 30.646 17.2273 28.1291V18.2901C17.2273 18.1212 17.311 17.9639 17.4466 17.8633C19.4189 16.4005 20.6972 14.0549 20.6972 11.4108C20.6972 8.16478 18.7709 5.37115 16.004 4.10612C15.2376 3.75573 14.3657 4.31581 14.3657 5.15849V9.21517C14.3657 9.35719 14.2506 9.47231 14.1086 9.47231H11.2293C11.0873 9.47231 10.9721 9.35719 10.9721 9.21517V5.15849ZM10.4271 28.1291V19.1219L10.4288 19.1224V17.0087C10.4288 16.8027 10.3048 16.6184 10.1204 16.5264C8.24382 15.5897 6.95491 13.6507 6.95491 11.4108C6.95491 9.92924 7.51876 8.57841 8.44488 7.56238C8.52175 7.47806 8.65785 7.5341 8.65785 7.6482V9.90946C8.65785 10.9462 9.49827 11.7866 10.535 11.7866H14.8029C15.8396 11.7866 16.68 10.9462 16.68 9.90946V7.6482C16.68 7.5341 16.8161 7.47806 16.893 7.56238C17.8191 8.57841 18.3829 9.92924 18.3829 11.4108C18.3829 13.649 17.096 15.5867 15.2217 16.5242C15.0376 16.6163 14.9138 16.8005 14.9137 17.0063L14.913 28.1291C14.913 29.3679 13.9088 30.3721 12.67 30.3721C11.4313 30.3721 10.4271 29.3679 10.4271 28.1291Z" fill="currentColor"/></svg>';
    this.state = {};
    this.activePopup = null;

    this._ensureDarkMode();
    this._initState();
    this._createFAB();
    this._createDrawer();
    this._initFABBehavior();
  }

  /* 确保暗色模式开关存在 */
  _ensureDarkMode() {
    var hasDisplay = this.controls.some(function(g) { return g.group === '显示'; });
    var hasDark = false;
    this.controls.forEach(function(g) {
      g.items.forEach(function(item) { if (item.key === 'dark') hasDark = true; });
    });
    if (!hasDark) {
      var darkItem = { type: 'switch', key: 'dark', label: '暗色模式', value: !document.body.classList.contains('theme-light') };
      if (hasDisplay) {
        var displayGroup = this.controls.find(function(g) { return g.group === '显示'; });
        displayGroup.items.unshift(darkItem);
      } else {
        this.controls.push({ group: '显示', items: [darkItem] });
      }
    }
  }

  /* 从 controls 配置初始化 state */
  _initState() {
    var self = this;
    this.controls.forEach(function(g) {
      g.items.forEach(function(item) {
        if (item.type === 'switch') self.state[item.key] = !!item.value;
        else if (item.type === 'select') self.state[item.key] = item.value || (item.options[0] && item.options[0].key);
      });
    });
  }

  /* 创建 FAB 按钮 */
  _createFAB() {
    var page = document.querySelector('.page') || document.body;
    this.fab = document.createElement('div');
    this.fab.className = 'ctrl-fab';
    this.fab.id = 'ctrl-fab';
    this.fab.innerHTML = this.icon;
    page.appendChild(this.fab);
  }

  /* 创建抽屉 */
  _createDrawer() {
    this.drawer = new Drawer({ title: '页面调试工具' });
  }

  /* STATUS_ARROW SVG */
  _statusArrow() {
    return '<svg width="10" height="16" viewBox="0 0 10 16" fill="none"><path fill-rule="evenodd" clip-rule="evenodd" d="M2.396 4.738L4.568 2.566L5.007 2.127L5.426 2.546L7.598 4.718L8.53 5.651C8.827 5.948 9.309 5.948 9.606 5.651C9.904 5.353 9.904 4.872 9.606 4.574L8.674 3.642L6.502 1.47L5.57 0.537C5.358 0.326 5.054 0.265 4.789 0.354C4.655 0.385 4.528 0.453 4.424 0.557L3.491 1.489L1.319 3.661L0.387 4.594C0.09 4.891 0.09 5.373 0.387 5.67C0.684 5.968 1.166 5.968 1.464 5.67L2.396 4.738ZM2.396 11.257L4.568 13.428L5.007 13.867L5.426 13.448L7.598 11.276L8.53 10.344C8.827 10.046 9.309 10.046 9.606 10.344C9.904 10.641 9.904 11.123 9.606 11.42L8.674 12.353L6.502 14.524L5.57 15.457C5.358 15.668 5.054 15.729 4.789 15.64C4.655 15.609 4.528 15.542 4.424 15.437L3.491 14.505L1.319 12.333L0.387 11.4C0.09 11.103 0.09 10.621 0.387 10.324C0.684 10.027 1.166 10.027 1.464 10.324L2.396 11.257Z" fill="currentColor"/></svg>';
  }

  /* 构建面板 HTML */
  _buildHTML() {
    var self = this;
    var html = '<div class="list-container">';

    this.controls.forEach(function(group) {
      html += '<div class="list-group-title">' + group.group + '</div>';
      html += '<div class="list-card">';
      var len = group.items.length;

      group.items.forEach(function(item, i) {
        var pos = len === 1 ? 'standalone' : (i === 0 ? 'top' : (i === len - 1 ? 'bottom' : 'middle'));
        var divider = i < len - 1 ? '' : '';
        var noDivider = ' list-item--no-divider';

        if (item.type === 'switch') {
          html += '<div class="list-item list-item--' + pos + noDivider + '">' +
            '<div class="list-item__text"><div class="list-item__title">' + item.label + '</div></div>' +
            '<div class="ctrl-switch' + (self.state[item.key] ? ' ctrl-switch--on' : '') + '" data-ctrl="' + item.key + '"></div>' +
          '</div>';
        } else if (item.type === 'select') {
          var curLabel = '';
          item.options.forEach(function(o) { if (o.key === self.state[item.key]) curLabel = o.label; });
          html += '<div class="list-item list-item--' + pos + noDivider + '" data-select="' + item.key + '">' +
            '<div class="list-item__text"><div class="list-item__title">' + item.label + '</div></div>' +
            '<div class="list-item__trailing list-item__trailing--status">' +
              '<span class="list-item__trailing--status-text" data-val="' + item.key + '">' + curLabel + '</span>' +
              '<span class="list-item__trailing--status-arrow">' + self._statusArrow() + '</span>' +
            '</div>' +
          '</div>';
        } else if (item.type === 'action') {
          html += '<div class="list-item list-item--' + pos + noDivider + '" data-action="' + item.key + '">' +
            '<div class="list-item__text"><div class="list-item__title">' + item.label + '</div></div>' +
            '<div class="list-item__trailing--arrow-bare"></div>' +
          '</div>';
        }
      });

      html += '</div>';
    });

    html += '<div class="ctrl-note">页面调试工具仅用于原型演示，不包含在研发交付物中</div>';
    html += '</div>';
    return html;
  }

  /* 绑定面板事件 */
  _bindEvents() {
    var self = this;
    var content = this.drawer.el.querySelector('.drawer__content');

    content.querySelectorAll('.ctrl-switch').forEach(function(sw) {
      sw.addEventListener('click', function(e) {
        e.stopPropagation();
        var key = sw.dataset.ctrl;
        self.state[key] = !self.state[key];
        sw.classList.toggle('ctrl-switch--on', self.state[key]);
        if (key === 'dark') {
          document.body.classList.toggle('theme-light', !self.state.dark);
        } else if (self.onChange) {
          self.onChange(key, self.state[key], self);
        }
      });
    });

    this.controls.forEach(function(group) {
      group.items.forEach(function(item) {
        if (item.type === 'select') {
          var row = content.querySelector('[data-select="' + item.key + '"]');
          if (!row) return;
          row.addEventListener('click', function() {
            if (self.activePopup) self.activePopup.destroy();
            self.activePopup = new PopupMenu({
              wide: true,
              items: item.options.map(function(o) {
                return { key: o.key, label: o.label, selected: o.key === self.state[item.key] };
              }),
              onSelect: function(key) {
                self.state[item.key] = key;
                var label = '';
                item.options.forEach(function(o) { if (o.key === key) label = o.label; });
                var valEl = content.querySelector('[data-val="' + item.key + '"]');
                if (valEl) valEl.textContent = label;
                if (self.onChange) self.onChange(item.key, key, self);
              }
            });
            self.activePopup.open(row);
          });
        } else if (item.type === 'action') {
          var actionRow = content.querySelector('[data-action="' + item.key + '"]');
          if (!actionRow) return;
          actionRow.addEventListener('click', function() {
            self.drawer.close();
            if (self.onChange) self.onChange(item.key, true, self);
          });
        }
      });
    });
  }

  /* 打开面板 */
  openPanel() {
    this.drawer.setContent(this._buildHTML());
    this.drawer.open();
    this._bindEvents();
  }

  /* FAB 拖拽 + 吸边 + 3s 自动收为竖条 */
  _initFABBehavior() {
    var fab = this.fab;
    var page = document.querySelector('.page') || document.body;
    var self = this;
    var startX, startY, fabX, fabY, dragged;
    var hideTimer = null;
    var isBar = false;
    var onRight = true;

    function resetHideTimer() {
      clearTimeout(hideTimer);
      hideTimer = setTimeout(toBar, 3000);
    }

    function toBar() {
      if (isBar) return;
      isBar = true;
      fab.classList.remove('snapping');
      fab.classList.add('ctrl-fab--bar');
      if (onRight) {
        fab.classList.add('ctrl-fab--bar-right');
        fab.style.left = 'auto';
        fab.style.right = '0';
      } else {
        fab.classList.remove('ctrl-fab--bar-right');
        fab.style.right = 'auto';
        fab.style.left = '0';
      }
      fab.style.bottom = '';
    }

    function toCircle() {
      if (!isBar) return;
      isBar = false;
      fab.classList.remove('ctrl-fab--bar', 'ctrl-fab--bar-right');
      fab.classList.add('snapping');
      if (onRight) {
        fab.style.left = ''; fab.style.right = '16px';
      } else {
        fab.style.left = '16px'; fab.style.right = 'auto';
      }
      resetHideTimer();
    }

    function onStart(e) {
      if (isBar) {
        e.preventDefault();
        toCircle();
        return;
      }
      clearTimeout(hideTimer);
      var t = e.touches ? e.touches[0] : e;
      var rect = fab.getBoundingClientRect();
      var pageRect = page.getBoundingClientRect();
      startX = t.clientX; startY = t.clientY;
      fabX = rect.left - pageRect.left;
      fabY = rect.top - pageRect.top;
      dragged = false;
      fab.classList.add('dragging');
      fab.classList.remove('snapping');
    }

    function onMove(e) {
      if (fabX === undefined) return;
      var t = e.touches ? e.touches[0] : e;
      var dx = t.clientX - startX, dy = t.clientY - startY;
      if (Math.abs(dx) > 4 || Math.abs(dy) > 4) dragged = true;
      if (!dragged) return;
      e.preventDefault();
      var pageRect = page.getBoundingClientRect();
      var nx = fabX + dx, ny = fabY + dy;
      nx = Math.max(0, Math.min(nx, pageRect.width - 52));
      ny = Math.max(0, Math.min(ny, pageRect.height - 52));
      fab.style.left = nx + 'px';
      fab.style.top = ny + 'px';
      fab.style.right = 'auto';
      fab.style.bottom = 'auto';
    }

    function onEnd() {
      if (fabX === undefined) return;
      fab.classList.remove('dragging');
      if (dragged) {
        var pageRect = page.getBoundingClientRect();
        var rect = fab.getBoundingClientRect();
        var cx = rect.left - pageRect.left + 26;
        fab.classList.add('snapping');
        if (cx < pageRect.width / 2) {
          fab.style.left = '16px'; fab.style.right = 'auto';
          onRight = false;
        } else {
          fab.style.left = ''; fab.style.right = '16px';
          onRight = true;
        }
      } else {
        self.openPanel();
      }
      fabX = undefined;
      resetHideTimer();
    }

    fab.addEventListener('touchstart', onStart, { passive: false });
    document.addEventListener('touchmove', onMove, { passive: false });
    document.addEventListener('touchend', onEnd);
    fab.addEventListener('mousedown', onStart);
    document.addEventListener('mousemove', onMove);
    document.addEventListener('mouseup', onEnd);

    resetHideTimer();
  }
}

/* ====== Init ====== */
new DebugTool();
```
