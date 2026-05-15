---
category: components-code
name: 加载 Loading 代码
description: Loading CSS + JS 代码。规范见 loading.md
---

# 加载 Loading 代码

⚠️ 复制代码前必须先读使用规则：[loading.md](loading.md)

## CSS

```css
/* ====== Loading 加载 ====== */

@keyframes loading-spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

/* —— LoadingIcon 加载图标 —— */
.loading-icon {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  color: var(--on_surface_secondary);
}

.loading-icon--disabled {
  color: var(--on_surface_octonary);
}

.loading-icon__spinner {
  display: flex;
  align-items: center;
  justify-content: center;
  animation: loading-spin 1s linear infinite;
}

.loading-icon__spinner svg {
  width: 24px;
  height: 24px;
}

.loading-icon--small .loading-icon__spinner svg {
  width: 20px;
  height: 20px;
}

.loading-icon__text {
  font-size: 16px;
  font-weight: 380;
  color: inherit;
}

.loading-icon--small .loading-icon__text {
  font-size: 14px;
}

/* —— LoadingToast 加载弹层 —— */
.loading-toast-overlay {
  position: fixed;
  inset: 0;
  z-index: 900;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--mask);
}

.loading-toast {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background: var(--surface);
  border-radius: 24px;
  box-shadow: 0px 36px 80px rgba(0, 0, 0, 0.08), 0px 0px 60px rgba(0, 0, 0, 0.06);
}

.loading-toast .loading-icon {
  color: var(--on_surface_secondary);
}
```

## JS

```javascript
/* ====== Loading SVG — Figma 导出 ====== */

const LOADING_ICONS = {
  spinnerDefault: `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M12 0C18.6274 0 24 5.37258 24 12C24 18.6274 18.6274 24 12 24C5.37258 24 0 18.6274 0 12C0 5.37258 5.37258 0 12 0ZM12 2.30762C6.64709 2.30762 2.30762 6.64709 2.30762 12C2.30762 17.3529 6.64709 21.6924 12 21.6924C17.3529 21.6924 21.6924 17.3529 21.6924 12C21.6924 6.64709 17.3529 2.30762 12 2.30762ZM6.92285 9.69238C8.19736 9.69238 9.23047 10.7255 9.23047 12C9.23047 13.2745 8.19735 14.3076 6.92285 14.3076C5.64845 14.3075 4.61523 13.2744 4.61523 12C4.61523 10.7256 5.64845 9.6925 6.92285 9.69238Z" fill="currentColor"/></svg>`,
  spinnerSmall: `<svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M10 0C15.5228 0 20 4.47715 20 10C20 15.5228 15.5228 20 10 20C4.47715 20 0 15.5228 0 10C0 4.47715 4.47715 0 10 0ZM10 2C5.58172 2 2 5.58172 2 10C2 14.4183 5.58172 18 10 18C14.4183 18 18 14.4183 18 10C18 5.58172 14.4183 2 10 2ZM6 8C7.10457 8 8 8.89543 8 10C8 11.1046 7.10457 12 6 12C4.89543 12 4 11.1046 4 10C4 8.89543 4.89543 8 6 8Z" fill="currentColor"/></svg>`
};

/* ====== LoadingIcon Class ====== */

class LoadingIcon {
  constructor(container, options = {}) {
    const {
      size = 'default',
      text = null,
      disabled = false
    } = options;

    this.el = document.createElement('div');
    this.el.className = 'loading-icon';
    if (size === 'small') this.el.classList.add('loading-icon--small');
    if (disabled) this.el.classList.add('loading-icon--disabled');

    const svg = size === 'small' ? LOADING_ICONS.spinnerSmall : LOADING_ICONS.spinnerDefault;
    let html = `<div class="loading-icon__spinner">${svg}</div>`;
    if (text) {
      html += `<div class="loading-icon__text">${text}</div>`;
    }
    this.el.innerHTML = html;
    container.appendChild(this.el);
  }

  remove() {
    this.el.remove();
  }
}

/* ====== LoadingToast Class ====== */

class LoadingToast {
  static instance = null;

  static show(text = '加载中') {
    if (LoadingToast.instance) return;

    const overlay = document.createElement('div');
    overlay.className = 'loading-toast-overlay';
    const toast = document.createElement('div');
    toast.className = 'loading-toast';
    new LoadingIcon(toast, { size: 'default', text });
    overlay.appendChild(toast);
    document.body.appendChild(overlay);
    LoadingToast.instance = overlay;
  }

  static hide() {
    if (!LoadingToast.instance) return;
    LoadingToast.instance.remove();
    LoadingToast.instance = null;
  }
}
```

## 调用示例

```javascript
// 页面加载 - 纯 spinner（Default 尺寸）
new LoadingIcon(container, { size: 'default' });

// 小尺寸 + 带文字
new LoadingIcon(container, { size: 'small', text: '加载中' });

// 禁用态
new LoadingIcon(container, { size: 'default', disabled: true });

// 加载弹层（居中遮罩）
LoadingToast.show('加载中');
// 关闭
LoadingToast.hide();
```
