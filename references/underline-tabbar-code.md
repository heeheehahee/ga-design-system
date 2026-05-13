# 二级文字标签（UnderlineTabBar）

文字标签 + 滑动下划线指示器，区别于药丸 Chips。适用于抽屉/面板内的 Tab 切换（如应用详情页的 Details / Reviews / Guides 标签）。

---

## CSS

```css
/* ====== UnderlineTabBar 下划线标签栏 ====== */

.underline-tabs-wrap {
  position: relative;
  flex-shrink: 0;
  z-index: 2;
  background: inherit;
}
.underline-tabs {
  position: relative;
  display: flex;
  align-items: center;
  gap: 28px;
  padding: 8px 20px 12px;
  overflow-x: auto;
  scrollbar-width: none;
  -webkit-mask-image: linear-gradient(to right, transparent, #000 20px, #000 calc(100% - 20px), transparent);
  mask-image: linear-gradient(to right, transparent, #000 20px, #000 calc(100% - 20px), transparent);
}
.underline-tabs::-webkit-scrollbar { display: none; }

.underline-tab {
  font-size: 16px;
  font-weight: 380;
  color: var(--on_surface_tertiary);
  cursor: pointer;
  flex-shrink: 0;
  white-space: nowrap;
}
.underline-tab--active {
  font-weight: 450;
  color: var(--on_surface);
}
.underline-tab:active { opacity: 0.5; }

.underline-tabs__indicator {
  position: absolute;
  bottom: 5px;
  width: 20px;
  height: 3px;
  border-radius: 10.5px;
  background: var(--bb-primary);
  will-change: left;
}
```

## JS

```javascript
/* ====== UnderlineTabBar Class ====== */

class UnderlineTabBar {
  /**
   * @param {string|Element} wrapContainer  .underline-tabs-wrap 容器
   * @param {Object} opts
   * @param {Array<{key:string, label:string}>} opts.tabs  标签列表
   * @param {number} opts.active  初始激活索引（默认 0）
   * @param {Function} opts.onChange  切换回调 (key, index) => {}
   */
  constructor(wrapContainer, opts = {}) {
    this.wrap = typeof wrapContainer === 'string' ? document.querySelector(wrapContainer) : wrapContainer;
    this.tabs = opts.tabs || [];
    this.activeIndex = opts.active || 0;
    this.onChange = opts.onChange || null;
    this._build();
  }

  _build() {
    this.wrap.classList.add('underline-tabs-wrap');
    const tabsEl = document.createElement('div');
    tabsEl.className = 'underline-tabs';

    this.tabEls = this.tabs.map((t, i) => {
      const el = document.createElement('div');
      el.className = 'underline-tab' + (i === this.activeIndex ? ' underline-tab--active' : '');
      el.textContent = t.label;
      el.setAttribute('role', 'tab');
      el.setAttribute('aria-selected', i === this.activeIndex ? 'true' : 'false');
      el.addEventListener('click', () => this.switchTo(i));
      tabsEl.appendChild(el);
      return el;
    });

    this.indicator = document.createElement('div');
    this.indicator.className = 'underline-tabs__indicator';
    tabsEl.appendChild(this.indicator);

    this.wrap.innerHTML = '';
    this.wrap.setAttribute('role', 'tablist');
    this.wrap.appendChild(tabsEl);
    this.tabsEl = tabsEl;

    requestAnimationFrame(() => this._moveIndicator(false));
  }

  _moveIndicator(animate) {
    const tab = this.tabEls[this.activeIndex];
    if (!tab) return;
    const left = tab.offsetLeft + tab.offsetWidth / 2 - 10;
    if (!animate) {
      this.indicator.style.transition = 'none';
      this.indicator.style.left = left + 'px';
      this.indicator.offsetHeight;
      this.indicator.style.transition = '';
    } else {
      this.indicator.style.transition = 'left 0.25s ease';
      this.indicator.style.left = left + 'px';
    }
  }

  switchTo(index) {
    if (index === this.activeIndex) return;
    this.tabEls[this.activeIndex].classList.remove('underline-tab--active');
    this.tabEls[this.activeIndex].setAttribute('aria-selected', 'false');
    this.tabEls[index].classList.add('underline-tab--active');
    this.tabEls[index].setAttribute('aria-selected', 'true');
    this.activeIndex = index;
    this._moveIndicator(true);

    const left = this.tabEls[index].offsetLeft - this.tabsEl.offsetWidth / 2 + this.tabEls[index].offsetWidth / 2;
    this.tabsEl.scrollTo({ left: Math.max(0, left), behavior: 'smooth' });

    if (this.onChange) this.onChange(this.tabs[index].key, index);
  }

  switchToKey(key) {
    const idx = this.tabs.findIndex(t => t.key === key);
    if (idx >= 0) this.switchTo(idx);
  }

  updateLabel(index, label) {
    if (this.tabEls[index]) this.tabEls[index].textContent = label;
    if (index === this.activeIndex) this._moveIndicator(false);
  }
}
```

## 调用示例

```javascript
const tabBar = new UnderlineTabBar('#tab-container', {
  tabs: [
    { key: 'details', label: 'Details' },
    { key: 'reviews', label: 'Reviews · 6' },
    { key: 'guides', label: 'Guides' },
    { key: 'leaderboard', label: 'Hero Leaderboard' }
  ],
  active: 0,
  onChange: (key, index) => console.log('switched to', key)
});

// 编程式切换
tabBar.switchToKey('reviews');

// 动态更新标签文字
tabBar.updateLabel(1, 'Reviews · 12');
```

## 设计参数

| 属性 | 值 |
|------|-----|
| 标签间距 | 28dp |
| 容器 padding | 8dp 20dp 12dp |
| 非激活文字 | 16sp, weight 380, `var(--on_surface_tertiary)` |
| 激活文字 | 16sp, weight 450, `var(--on_surface)` |
| 指示器 | 20×3dp, radius 10.5dp, `var(--bb-primary)` |
| 指示器距底 | 5dp |
| 滑动动画 | 250ms ease |
| 左右渐隐 | 20dp mask-image gradient |
| 容器背景 | `var(--surface_high)` |

## 与 Chips 的区别

| | Chips（药丸） | UnderlineTabBar（下划线） |
|--|--------------|------------------------|
| 形状 | 圆角药丸填充 | 纯文字 + 底部指示器 |
| 激活态 | 绿色背景 | 加粗 + 下划线 |
| 适用 | 标签筛选、横滑分类 | 页面 Tab 切换 |
