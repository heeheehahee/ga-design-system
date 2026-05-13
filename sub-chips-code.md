# 二级胶囊标签（SubChips）

小尺寸筛选药丸标签，支持普通态和高亮态（橙色），用于抽屉/面板内的内容筛选。区别于一级胶囊标签（Chips）。

---

## CSS

```css
/* ====== SubChips 筛选药丸 ====== */

.sub-chips {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 20px;
  overflow-x: auto;
  scrollbar-width: none;
  flex-shrink: 0;
  -webkit-mask-image: linear-gradient(to right, transparent, #000 20px, #000 calc(100% - 20px), transparent);
  mask-image: linear-gradient(to right, transparent, #000 20px, #000 calc(100% - 20px), transparent);
}
.sub-chips::-webkit-scrollbar { display: none; }

.sub-chip {
  height: 32px;
  padding: 0 12px;
  border-radius: 999px;
  font-size: 12px;
  font-weight: 330;
  color: var(--on_surface_tertiary);
  background: var(--surface_container);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  flex-shrink: 0;
  white-space: nowrap;
}
.sub-chip--active {
  background: rgba(var(--bb-primary-rgb), 0.1);
  color: var(--bb-primary);
  font-weight: 450;
}
.sub-chip:active { opacity: 0.7; }

/* 高亮变体（橙色） */
.sub-chip--highlight {
  font-weight: 450;
  color: var(--orange);
}
.sub-chip--highlight.sub-chip--active {
  background: rgba(var(--orange-rgb), 0.1);
  color: var(--orange);
}

/* 带计数 */
.sub-chip__label { font: inherit; color: inherit; }
.sub-chip__count { font: inherit; color: inherit; opacity: 0.5; margin-left: 4px; }
.sub-chip--active .sub-chip__count { opacity: 1; }
```

## JS

```javascript
/* ====== SubChips Class ====== */

class SubChips {
  /**
   * @param {string|Element} container
   * @param {Object} opts
   * @param {Array<{key:string, label:string, count?:number, highlight?:boolean}>} opts.chips
   * @param {string} opts.active  初始激活 key（默认第一个）
   * @param {Function} opts.onChange  (key) => {}
   */
  constructor(container, opts = {}) {
    this.el = typeof container === 'string' ? document.querySelector(container) : container;
    this.chips = opts.chips || [];
    this.activeKey = opts.active || (this.chips[0]?.key);
    this.onChange = opts.onChange || null;
    this.render();
  }

  render() {
    this.el.className = 'sub-chips';
    this.el.innerHTML = this.chips.map(c => {
      const cls = ['sub-chip'];
      if (c.highlight) cls.push('sub-chip--highlight');
      if (c.key === this.activeKey) cls.push('sub-chip--active');
      const countHtml = c.count !== undefined ? `<span class="sub-chip__count">${c.count}</span>` : '';
      return `<div class="${cls.join(' ')}" data-key="${c.key}"><span class="sub-chip__label">${c.label}</span>${countHtml}</div>`;
    }).join('');

    this.el.querySelectorAll('.sub-chip').forEach(chip => {
      chip.addEventListener('click', () => {
        const key = chip.dataset.key;
        this.activeKey = this.activeKey === key ? this.chips[0]?.key : key;
        this.el.querySelectorAll('.sub-chip').forEach(c => {
          c.classList.toggle('sub-chip--active', c.dataset.key === this.activeKey);
        });
        chip.scrollIntoView({ behavior: 'smooth', inline: 'center', block: 'nearest' });
        if (this.onChange) this.onChange(this.activeKey);
      });
    });
  }

  setActive(key) {
    this.activeKey = key;
    this.render();
  }
}
```

## 调用示例

```javascript
// 基础筛选
new SubChips('#filters', {
  chips: [
    { key: 'all', label: 'All' },
    { key: 'guides', label: 'Guides' },
    { key: 'updates', label: 'Updates' },
    { key: 'strategies', label: 'Strategies' }
  ],
  onChange: (key) => filterContent(key)
});

// 带计数 + 高亮变体
new SubChips('#review-filters', {
  chips: [
    { key: 'all', label: 'ALL', count: 8 },
    { key: 'positive', label: 'Positive', count: 5 },
    { key: 'negative', label: 'Negative', count: 3 },
    { key: 'fun', label: 'Fun', count: 84, highlight: true },
    { key: 'smooth', label: 'Smooth', count: 12, highlight: true }
  ],
  onChange: (key) => filterReviews(key)
});
```

## 设计参数

| 属性 | 普通态 | 激活态 |
|------|--------|--------|
| 高度 | 32dp | 32dp |
| padding | 0 12dp | 0 12dp |
| 圆角 | 999dp | 999dp |
| 背景 | `var(--surface_container)` | `rgba(bb-primary-rgb, 0.1)` |
| 文字 | 12sp, 330, `var(--on_surface_tertiary)` | 12sp, 450, `var(--bb-primary)` |
| 高亮激活 | — | `rgba(orange-rgb, 0.1)` + `var(--orange)` |
| 容器 gap | 8dp | — |
| 左右渐隐 | 20dp | — |

## 与 Chips 的区别

| | Chips（导航药丸） | SubChips（筛选药丸） |
|--|-----------------|-------------------|
| 尺寸 | padding 10px 16px | height 32dp, padding 0 12dp |
| 字号 | 14sp | 12sp |
| 激活态 | 绿色实底+白字 | 绿色浅底+绿字 |
| 用途 | 一级 Tab 导航 | 二级内容筛选 |
| 支持高亮 | 否 | 是（橙色变体） |
| 支持计数 | 否 | 是 |
