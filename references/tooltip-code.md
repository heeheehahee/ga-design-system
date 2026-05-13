---
category: components-code
name: Tooltip 代码
description: Tooltip 提示气泡的 CSS + JS 代码。设计规范见 tooltip.md
---

# Tooltip 代码

设计规范：[tooltip.md](tooltip.md)

---

### CSS

```css
.tooltip {
  position: fixed;
  z-index: 1000;
  animation: tooltip-in 0.2s ease;
}
.tooltip--out {
  animation: tooltip-out 0.15s ease forwards;
}
@keyframes tooltip-in {
  from { opacity: 0; transform: scale(0.4); }
  to { opacity: 1; transform: scale(1); }
}
@keyframes tooltip-out {
  from { opacity: 1; transform: scale(1); }
  to { opacity: 0; transform: scale(0.4); }
}

/* 缩放起点：箭头方向 */
.tooltip--top { transform-origin: bottom center; }
.tooltip--top-left { transform-origin: bottom left; }
.tooltip--top-center { transform-origin: bottom center; }
.tooltip--top-right { transform-origin: bottom right; }
.tooltip--bottom { transform-origin: top center; }
.tooltip--bottom-left { transform-origin: top left; }
.tooltip--bottom-center { transform-origin: top center; }
.tooltip--bottom-right { transform-origin: top right; }
.tooltip--left-center { transform-origin: center right; }
.tooltip--right-center { transform-origin: center left; }
.tooltip__body {
  position: relative;
  background: var(--surface);
  border-radius: 12px;
  padding: 15px 14px;
  font-size: 14px;
  font-weight: 330;
  color: var(--on_surface_tertiary);
  box-shadow: 0 4px 24px rgba(0,0,0,0.12), 0 0 1px rgba(0,0,0,0.08);
  white-space: nowrap;
}
.tooltip__arrow {
  position: absolute;
  color: var(--surface);
  line-height: 0;
}
.tooltip__arrow svg { display: block; width: 28px; height: 10px; }

/* 上方气泡：箭头在底部朝下 */
.tooltip--top .tooltip__arrow { bottom: -9px; }
.tooltip--top .tooltip__arrow svg { transform: rotate(180deg); }
.tooltip--top-left .tooltip__arrow { left: 16px; }
.tooltip--top-center .tooltip__arrow { left: 50%; margin-left: -14px; }
.tooltip--top-right .tooltip__arrow { right: 16px; }

/* 下方气泡：箭头在顶部朝上 */
.tooltip--bottom .tooltip__arrow { top: -9px; }
.tooltip--bottom-left .tooltip__arrow { left: 16px; }
.tooltip--bottom-center .tooltip__arrow { left: 50%; margin-left: -14px; }
.tooltip--bottom-right .tooltip__arrow { right: 16px; }

/* 左侧气泡：箭头在右侧朝右 */
.tooltip--left-center .tooltip__arrow { right: -18px; top: 50%; margin-top: -14px; }
.tooltip--left-center .tooltip__arrow svg { transform: rotate(90deg); width: 28px; height: 28px; }

/* 右侧气泡：箭头在左侧朝左 */
.tooltip--right-center .tooltip__arrow { left: -18px; top: 50%; margin-top: -14px; }
.tooltip--right-center .tooltip__arrow svg { transform: rotate(-90deg); width: 28px; height: 28px; }
```

### 箭头 SVG

从 Figma 导出的真实箭头路径（朝上，28×10），通过 CSS rotate 控制方向：

```svg
<svg width="28" height="10" viewBox="0 0 28 10">
  <path d="M3.8643 9.5033C2.6482 9.9665 0 10 0 10L28 9.9992C28 9.9992 25.3522 9.9646 24.1357 9.5026C22.8225 9.002 21.9543 8.1269 21.0778 7.097C20.4475 6.3562 19.2101 4.7757 18.6084 4.0126C18.1148 3.3874 17.1495 2.1499 16.6225 1.552C15.9594 0.8001 15.1397 0 14.0006 0C12.8615 0 12.0414 0.8001 11.3779 1.5528C10.8508 2.1511 9.8855 3.3877 9.3924 4.0134C8.7899 4.7765 7.5525 6.357 6.9225 7.0978C6.0476 8.1277 5.1779 9.0028 3.8643 9.5033Z" fill="currentColor"/>
</svg>
```

### JS

```js
class Tooltip {
  constructor({ text = '', direction = 'top-center', target } = {}) {
    this.text = text;
    this.direction = direction;
    this.target = typeof target === 'string' ? document.querySelector(target) : target;
    this._build();
  }
  _build() {
    const [main, align] = this.direction.split('-');
    this.el = document.createElement('div');
    this.el.className = `tooltip tooltip--${main} tooltip--${this.direction}`;
    const body = document.createElement('div');
    body.className = 'tooltip__body';
    body.textContent = this.text;
    const arrow = document.createElement('div');
    arrow.className = 'tooltip__arrow';
    arrow.innerHTML = '<svg width="28" height="10" viewBox="0 0 28 10"><path d="M3.8643 9.5033C2.6482 9.9665 0 10 0 10L28 9.9992C28 9.9992 25.3522 9.9646 24.1357 9.5026C22.8225 9.002 21.9543 8.1269 21.0778 7.097C20.4475 6.3562 19.2101 4.7757 18.6084 4.0126C18.1148 3.3874 17.1495 2.1499 16.6225 1.552C15.9594 0.8001 15.1397 0 14.0006 0C12.8615 0 12.0414 0.8001 11.3779 1.5528C10.8508 2.1511 9.8855 3.3877 9.3924 4.0134C8.7899 4.7765 7.5525 6.357 6.9225 7.0978C6.0476 8.1277 5.1779 9.0028 3.8643 9.5033Z" fill="currentColor"/></svg>';
    body.appendChild(arrow);
    this.el.appendChild(body);
    this.el.style.animation = 'none';
    this.el.style.visibility = 'hidden';
    document.body.appendChild(this.el);

    if (this.target) {
      const rect = this.target.getBoundingClientRect();
      const tipRect = this.el.getBoundingClientRect();
      let top, left;
      const topBottom = main === 'top' || main === 'bottom';
      if (main === 'top') top = rect.top - tipRect.height - 8;
      else if (main === 'bottom') top = rect.bottom + 8;
      else if (main === 'left') { top = rect.top + (rect.height - tipRect.height)/2; left = rect.left - tipRect.width - 8; }
      else if (main === 'right') { top = rect.top + (rect.height - tipRect.height)/2; left = rect.right + 8; }
      if (topBottom) {
        if (align === 'left') left = rect.left;
        else if (align === 'center') left = rect.left + (rect.width - tipRect.width)/2;
        else if (align === 'right') left = rect.right - tipRect.width;
      }
      this.el.style.top = top + 'px';
      this.el.style.left = left + 'px';
      this.el.style.animation = '';
      this.el.style.visibility = '';
    }

    this._outsideHandler = (e) => { if (!this.el.contains(e.target)) this.destroy(); };
    setTimeout(() => document.addEventListener('click', this._outsideHandler), 0);
  }
  destroy() {
    document.removeEventListener('click', this._outsideHandler);
    if (this.el.parentNode) {
      this.el.classList.add('tooltip--out');
      this.el.addEventListener('animationend', () => this.el.remove());
    }
  }
}
```

### 使用示例

```js
// 显示
const tip = new Tooltip({
  text: '提示文字，提示一些新功能。',
  direction: 'top-center',  // top-left|top-center|top-right|bottom-left|bottom-center|bottom-right|left-center|right-center
  target: '#some-button'
});

// 关闭
tip.destroy();
```
