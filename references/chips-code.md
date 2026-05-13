# Chips 标签栏

横滑标签栏组件，用于抽屉内多面板切换。支持滑动切入/切出动画和自动居中定位。

---

## HTML

```html
<div class="chips-bar">
  <div class="chips-bar__chip chips-bar__chip--active" data-tab="tab1">Details</div>
  <div class="chips-bar__chip" data-tab="tab2">Comments</div>
  <div class="chips-bar__chip" data-tab="tab3">Guides</div>
  <div class="chips-bar__chip" data-tab="tab4">Leaderboard</div>
</div>

<!-- 对应面板 -->
<div class="chips-bar__panel" id="tab-tab1">...</div>
<div class="chips-bar__panel" id="tab-tab2" style="display:none">...</div>
<div class="chips-bar__panel" id="tab-tab3" style="display:none">...</div>
<div class="chips-bar__panel" id="tab-tab4" style="display:none">...</div>
```

## CSS

```css
/* Chips 标签栏 */
.chips-bar {
  position: relative;
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 4px 20px 24px;
  flex-shrink: 0;
  overflow-x: auto;
  overflow-y: visible;
  scrollbar-width: none;
  clip-path: inset(-40px 0);
  z-index: 2;
}
.chips-bar::-webkit-scrollbar { display: none; }

.chips-bar__chip {
  padding: 10px 16px;
  border-radius: 999px;
  font-size: 14px;
  font-weight: 330;
  color: var(--on_surface_tertiary);
  background: var(--surface_high);
  box-shadow: 0px 12px 40px rgba(0,0,0,0.04), 0px 0px 20px rgba(0,0,0,0.02);
  cursor: pointer;
  flex-shrink: 0;
  white-space: nowrap;
}

.chips-bar__chip--active {
  background: var(--bb-primary);
  color: var(--btn-on-primary);
  font-weight: 450;
  box-shadow: none;
}
.theme-community .chips-bar__chip--active { color: #000; }

.chips-bar__chip:active {
  opacity: 0.7;
}

/* 切换动画 */
@keyframes chipsSlideLeft {
  from { transform: translateX(30px); opacity: 0; }
  to   { transform: translateX(0);    opacity: 1; }
}
@keyframes chipsSlideRight {
  from { transform: translateX(-30px); opacity: 0; }
  to   { transform: translateX(0);     opacity: 1; }
}
```

## JS — ChipsBar

```js
class ChipsBar {
  /**
   * @param {string|Element} container  .chips-bar 容器
   * @param {Object} opts
   * @param {string} opts.panelSelector  面板选择器，默认 '.chips-bar__panel'
   * @param {Function} opts.onChange      切换回调 (tabKey, index) => {}
   */
  constructor(container, opts = {}) {
    this.el = typeof container === 'string' ? document.querySelector(container) : container;
    this.chips = Array.from(this.el.querySelectorAll('.chips-bar__chip'));
    this.panelSelector = opts.panelSelector || '.chips-bar__panel';
    this.panels = document.querySelectorAll(this.panelSelector);
    this.onChange = opts.onChange || null;
    this.activeIndex = 0;

    this.chips.forEach((chip, i) => {
      chip.addEventListener('click', () => this.switchTo(i));
    });
  }

  switchTo(index) {
    const chip = this.chips[index];
    const left = chip.offsetLeft - this.el.offsetWidth / 2 + chip.offsetWidth / 2;
    this.el.scrollTo({ left: Math.max(0, left), behavior: 'smooth' });
    if (index === this.activeIndex) return;
    const direction = index > this.activeIndex ? 'chipsSlideRight' : 'chipsSlideLeft';

    const prev = this.chips[this.activeIndex];
    prev.style.transition = 'none';
    prev.classList.remove('chips-bar__chip--active');
    chip.style.transition = 'none';
    chip.classList.add('chips-bar__chip--active');
    this.activeIndex = index;

    const tab = chip.dataset.tab;
    this.panels.forEach(p => {
      if (p.id === 'tab-' + tab) {
        p.style.display = '';
        p.style.animation = 'none';
        p.offsetHeight;
        p.style.animation = direction + ' 0.25s ease';
      } else {
        p.style.display = 'none';
      }
    });

    if (this.onChange) this.onChange(tab, index);
  }

  /** 通过 data-tab 值切换 */
  switchToTab(tabKey) {
    const idx = this.chips.findIndex(c => c.dataset.tab === tabKey);
    if (idx >= 0) this.switchTo(idx);
  }
}
```

### 使用示例

```js
const chipsBar = new ChipsBar('.chips-bar', {
  panelSelector: '.chips-bar__panel',
  onChange: (tab, index) => console.log('switched to', tab)
});

// 编程式切换
chipsBar.switchToTab('comments');
```

---

## 设计参数

| 属性 | 值 |
|------|-----|
| 高度 | 自适应（padding 10px 16px） |
| 圆角 | 999px（全圆角药丸） |
| 间距 | chip 间 12px |
| 容器 padding | 4px 20px 24px |
| 默认态字体 | 14px / weight 330 |
| 激活态字体 | 14px / weight 450 |
| 默认背景 | var(--surface_high) |
| 激活背景 | var(--bb-primary) |
| 默认文字色 | var(--on_surface_tertiary) |
| 激活文字色 | var(--btn-on-primary) |
| 阴影（默认态） | 0 12px 40px rgba(0,0,0,0.04), 0 0 20px rgba(0,0,0,0.02) |
| 切换动画 | translateX ±30px + opacity, 0.25s ease |
