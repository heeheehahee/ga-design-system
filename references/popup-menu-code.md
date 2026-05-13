---
category: components-code
name: PopupMenu 近手菜单代码
description: PopupMenu CSS + JS 代码。Figma ga_component (node 443:7822)
---

# PopupMenu 近手菜单

点击触发的弹出菜单，带遮罩和缩放动画。适用于更多操作、长按菜单等场景。

---

## CSS

```css
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
```

## JS

```javascript
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
```

## 调用示例

```javascript
// 单行菜单（标准宽度 200px）
new PopupMenu({
  anchor: document.getElementById('more-btn'),
  items: [
    { label: '163邮箱', key: 'mail163', selected: true },
    { label: 'Gmail', key: 'gmail' },
    { label: 'Hotmail', key: 'hotmail' },
    { label: 'Foxmail', key: 'foxmail' },
    { label: 'Yahoo', key: 'yahoo' }
  ],
  onSelect: (key) => console.log(key)
});

// 双行菜单（带副标题）
new PopupMenu({
  anchor: document.getElementById('more-btn'),
  items: [
    { label: '163邮箱', subtitle: 'gaojin06@163.com', key: 'mail163', selected: true },
    { label: 'Gmail', subtitle: 'gaojin06@gmail.com', key: 'gmail' }
  ]
});

// 最大宽度菜单
new PopupMenu({
  anchor: document.getElementById('more-btn'),
  wide: true,
  items: [
    { label: '163邮箱', subtitle: 'stevejobs@163.com', key: 'mail163' },
    { label: 'Gmail', subtitle: 'stevejobs@gmail.com', key: 'gmail', selected: true }
  ]
});
```

## 依赖

- `var(--surface)`, `var(--on_surface)`, `var(--on_surface_tertiary)`, `var(--primary)` — color.md
- `var(--mask)` — 遮罩
- `var(--btn-surface-touch)` — 按压态
