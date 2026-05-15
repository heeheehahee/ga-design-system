---
category: components-code
name: Drawer 底部抽屉代码
description: Drawer CSS + JS 代码。规范见 drawer.md
---

# Drawer 代码

⚠️ 复制代码前必须先读使用规则：[drawer.md](drawer.md)

---

## CSS

```css
/* Drawer 底部抽屉 */
.drawer-mask {
  position: fixed;
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
  position: fixed;
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
```

---

## JS

```js
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
```

---

## 使用示例

```html
<script>
const drawer = new Drawer({
  size: 'medium',
  title: '选择选项',
  onClose: () => console.log('closed')
});

drawer.setContent('<p style="padding:20px;color:var(--on_surface)">抽屉内容</p>');
drawer.open();
</script>
```
