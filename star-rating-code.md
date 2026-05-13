# StarRating 星级评分

星级评分的原子组件。支持满星、空星、半星三种状态，颜色通过 CSS 变量控制，自动适配亮暗模式。

---

## CSS

```css
/* ====== StarRating 星级评分 ====== */

.star-full { color: var(--bb-primary); }
.star-empty { color: var(--surface_container_high); }

.star-half {
  position: relative;
  display: inline-block;
  width: 16px;
  height: 16px;
}
.star-half > .star-empty,
.star-half > .star-full {
  position: absolute;
  left: 0;
  top: 0;
}
.star-half__clip {
  clip-path: inset(0 50% 0 0);
}
```

## JS

```javascript
/* ====== StarRating SVG + Class ====== */

const STAR_SVG = {
  full: '<svg width="16" height="16" viewBox="0 0 16 16" fill="none"><path d="M7.63 1.96c.117-.36.52-.36.637 0L9.68 6h4.24c.376 0 .533.48.23.7L10.72 9.2l1.31 4.04c.117.36-.32.66-.59.44L8 11.17 4.56 13.68c-.27.22-.707-.08-.59-.44L5.28 9.2 1.85 6.7c-.303-.22-.146-.7.23-.7H6.32L7.63 1.96z" fill="currentColor"/></svg>',
  empty: '<svg width="16" height="16" viewBox="0 0 16 16" fill="none"><path d="M7.63 1.96c.117-.36.52-.36.637 0L9.68 6h4.24c.376 0 .533.48.23.7L10.72 9.2l1.31 4.04c.117.36-.32.66-.59.44L8 11.17 4.56 13.68c-.27.22-.707-.08-.59-.44L5.28 9.2 1.85 6.7c-.303-.22-.146-.7.23-.7H6.32L7.63 1.96z" fill="currentColor"/></svg>'
};

class StarRating {
  /**
   * @param {string|Element} container  容器
   * @param {Object} opts
   * @param {number} opts.score       评分值（0-5，支持 0.5 步进）
   * @param {number} opts.total       总星数（默认 5）
   * @param {number} opts.size        星星尺寸 px（默认 16）
   * @param {number} opts.gap         间距 px（默认 2）
   * @param {Function} opts.onSelect  点击回调 (starIndex) => {}
   */
  constructor(container, opts = {}) {
    this.el = typeof container === 'string' ? document.querySelector(container) : container;
    this.score = opts.score || 0;
    this.total = opts.total || 5;
    this.size = opts.size || 16;
    this.gap = opts.gap || 2;
    this.onSelect = opts.onSelect || null;
    this.render();
  }

  render() {
    const full = Math.floor(this.score);
    const hasHalf = this.score - full >= 0.25;
    let html = '';

    for (let i = 0; i < this.total; i++) {
      if (i < full) {
        html += `<span class="star-full">${STAR_SVG.full}</span>`;
      } else if (i === full && hasHalf) {
        html += `<span class="star-half"><span class="star-empty">${STAR_SVG.empty}</span><span class="star-full star-half__clip">${STAR_SVG.full}</span></span>`;
      } else {
        html += `<span class="star-empty">${STAR_SVG.empty}</span>`;
      }
    }

    this.el.style.display = 'flex';
    this.el.style.gap = this.gap + 'px';
    if (this.size !== 16) {
      this.el.style.cssText += `;--star-size:${this.size}px`;
      this.el.querySelectorAll('svg').forEach(svg => {
        svg.setAttribute('width', this.size);
        svg.setAttribute('height', this.size);
      });
    }
    this.el.innerHTML = html;

    if (this.onSelect) {
      this.el.querySelectorAll('.star-full, .star-empty, .star-half').forEach((star, i) => {
        star.style.cursor = 'pointer';
        star.addEventListener('click', () => {
          this.score = i + 1;
          this.render();
          this.onSelect(this.score);
        });
      });
    }
  }

  setScore(score) {
    this.score = score;
    this.render();
  }
}
```

## 调用示例

```javascript
// 纯展示（3.5 星）
new StarRating('#rating', { score: 3.5 });

// 纯展示（4 星，28px 大星星）
new StarRating('#rating-lg', { score: 4, size: 28, gap: 4 });

// 可交互打分
new StarRating('#my-rating', {
  score: 0,
  size: 28,
  gap: 8,
  onSelect: (star) => console.log('selected', star)
});

// 动态更新
const sr = new StarRating('#rating', { score: 0 });
sr.setScore(4.5);
```

## 设计参数

| 属性 | 值 |
|------|-----|
| 满星颜色 | `var(--bb-primary)` (#05C474) |
| 空星颜色 | `var(--surface_container_high)` |
| 半星实现 | 两层叠加 + clip-path inset(0 50% 0 0) |
| SVG fill | `currentColor`（跟随 .star-full / .star-empty 的 color） |
| 默认尺寸 | 16×16, gap 2px |
| 大尺寸 | 28×28, gap 4px（轻评分场景） |

## 变体

| 场景 | score | size | gap | onSelect |
|------|-------|------|-----|---------|
| 评论卡内星级 | 1-5 整数 | 16 | 2 | null |
| 评分概览 | 0-5 含半星 | 16 | 2 | null |
| 轻评分交互 | 0-5 整数 | 28 | 8 | 跳转写评论页 |
