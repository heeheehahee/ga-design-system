---
category: components-code
name: 轻提示 Toast 代码
description: Toast CSS + JS 代码。规范见 toast.md
---

# 轻提示 Toast 代码

⚠️ 复制代码前必须先读使用规则：[toast.md](toast.md)

## CSS

```css
/* ====== Toast 轻提示 ====== */

/* 组件级变量 — Figma 原值精确还原 */
:root {
  --toast-bg: #242424;                 /* Dark: Figma #242424 = surface_pop_window */
  --toast-text: rgba(255,255,255,0.6); /* Dark: Figma 次要文字60% = #ffffff99 */
}
body.theme-light {
  --toast-bg: #FAFAFA;                 /* Light: Figma #FAFAFA（无精确 Token 对应） */
  --toast-text: rgba(0,0,0,0.6);      /* Light: Figma 次要文字60% = on_surface_tertiary */
}

.toast {
  position: fixed;
  bottom: 120px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1000;
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.toast--visible {
  opacity: 1;
}

.toast--entering {
  transform: translateX(-50%) translateY(20px);
  opacity: 0;
}

.toast--exiting {
  transform: translateX(-50%) translateY(20px);
  opacity: 0;
}

.toast__bg {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  padding: 15.5px 20px;
  background: var(--toast-bg);
  border: 0.34px solid var(--outline);
  border-radius: 20px;
  box-shadow: 0px 18px 36px rgba(0, 0, 0, 0.1);
}

.toast--with-icon .toast__bg {
  padding: 13px 20px;
  gap: 10px;
}

.toast__icon {
  width: 24px;
  height: 24px;
  flex-shrink: 0;
  color: var(--toast-text);
  display: flex;
  align-items: center;
  justify-content: center;
}

.toast__icon svg {
  width: 24px;
  height: 24px;
}

.toast__text {
  font-size: 14px;
  font-weight: 380;
  color: var(--toast-text);
  text-align: center;
  line-height: 1.4;
}

.toast--multiline .toast__text {
  text-align: left;
  max-width: 244px;
}
```

## JS

```javascript
/* ====== Toast Class ====== */

class Toast {
  static instance = null;
  static queue = null;

  static show(text, options = {}) {
    if (Toast.instance) {
      Toast.instance.remove();
    }

    const { icon = null, duration = 3000 } = options;
    const isMultiline = text.length > 16;

    const el = document.createElement('div');
    el.className = 'toast toast--entering';
    if (icon) el.classList.add('toast--with-icon');
    if (isMultiline) el.classList.add('toast--multiline');

    let inner = '';
    if (icon) {
      inner += `<div class="toast__icon">${icon}</div>`;
    }
    inner += `<div class="toast__text">${text}</div>`;

    el.innerHTML = `<div class="toast__bg">${inner}</div>`;
    document.body.appendChild(el);

    requestAnimationFrame(() => {
      requestAnimationFrame(() => {
        el.classList.remove('toast--entering');
        el.classList.add('toast--visible');
      });
    });

    Toast.instance = el;
    Toast.queue = setTimeout(() => Toast.hide(), duration);
  }

  static hide() {
    const el = Toast.instance;
    if (!el) return;

    clearTimeout(Toast.queue);
    el.classList.remove('toast--visible');
    el.classList.add('toast--exiting');

    el.addEventListener('transitionend', () => el.remove(), { once: true });
    setTimeout(() => el.remove(), 350);
    Toast.instance = null;
    Toast.queue = null;
  }
}
```

## 调用示例

```javascript
// 纯文案 - 单行
Toast.show('已复制到剪贴板');

// 纯文案 - 长文案自动折行
Toast.show('提示文案较长无法避免时，折行需使用左对齐方式');

// 文案 + 图标
Toast.show('操作成功', {
  icon: '<svg width="24" height="24" viewBox="0 0 24 24" fill="none"><path d="M9 16.2L4.8 12l-1.4 1.4L9 19 21 7l-1.4-1.4L9 16.2z" fill="currentColor"/></svg>'
});

// 自定义时长（5秒）
Toast.show('操作成功', { duration: 5000 });
```
