# RatingBars 评分分布条

评分概览模块：左侧大数字+星星，右侧 5 条进度条。数据驱动渲染。

---

## CSS

```css
/* ====== RatingBars 评分分布条 ====== */

.rating-bars {
  display: flex;
  justify-content: space-between;
  padding: 0 20px;
}

.rating-bars__left {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.rating-bars__score {
  font-size: 56px;
  font-weight: 330;
  line-height: 52px;
  color: var(--on_surface);
}
.rating-bars__stars {
  display: flex;
  gap: 2px;
}

.rating-bars__right {
  display: flex;
  flex-direction: column;
  gap: 4px;
  justify-content: center;
}
.rating-bars__count {
  font-size: 13px;
  font-weight: 330;
  color: var(--on_surface_quaternary);
  text-align: right;
}

.rating-bars__row {
  display: flex;
  align-items: center;
  gap: 8px;
}
.rating-bars__label {
  display: flex;
  gap: 1px;
  width: 50px;
  justify-content: flex-end;
  flex-shrink: 0;
}
.rating-bars__mini-star {
  display: inline-block;
  width: 8px;
  height: 8px;
  background: var(--on_surface_quaternary);
  -webkit-mask: url('data:image/svg+xml,%3Csvg%20viewBox%3D%220%200%2016%2016%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cpath%20d%3D%22M7.64%202.4C7.75%202.05%208.25%202.05%208.36%202.4L9.65%206.37H13.82C14.19%206.37%2014.35%206.85%2014.05%207.06L10.67%209.52L11.96%2013.48C12.08%2013.84%2011.67%2014.13%2011.37%2013.91L8%2011.46L4.63%2013.91C4.33%2014.13%203.92%2013.84%204.04%2013.48L5.33%209.52L1.95%207.06C1.65%206.85%201.81%206.37%202.18%206.37H6.35L7.64%202.4Z%22%20fill%3D%22black%22%2F%3E%3C%2Fsvg%3E') no-repeat center / contain;
  mask: url('data:image/svg+xml,%3Csvg%20viewBox%3D%220%200%2016%2016%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cpath%20d%3D%22M7.64%202.4C7.75%202.05%208.25%202.05%208.36%202.4L9.65%206.37H13.82C14.19%206.37%2014.35%206.85%2014.05%207.06L10.67%209.52L11.96%2013.48C12.08%2013.84%2011.67%2014.13%2011.37%2013.91L8%2011.46L4.63%2013.91C4.33%2014.13%203.92%2013.84%204.04%2013.48L5.33%209.52L1.95%207.06C1.65%206.85%201.81%206.37%202.18%206.37H6.35L7.64%202.4Z%22%20fill%3D%22black%22%2F%3E%3C%2Fsvg%3E') no-repeat center / contain;
}
.rating-bars__track {
  width: 120px;
  height: 4px;
  border-radius: 2px;
  background: var(--surface_container_medium);
}
.rating-bars__fill {
  height: 100%;
  border-radius: 2px;
  background: var(--bb-primary);
}
```

## JS

```javascript
/* ====== RatingBars Class ====== */

class RatingBars {
  /**
   * @param {string|Element} container
   * @param {Object} opts
   * @param {number} opts.score          平均分（如 4.5）
   * @param {Array<{stars:number, count:number}>} opts.distribution  5→1 星分布
   * @param {string} opts.countText      底部文字（如 "10,489,578 Ratings"）
   */
  constructor(container, opts = {}) {
    this.el = typeof container === 'string' ? document.querySelector(container) : container;
    this.score = opts.score || 0;
    this.distribution = opts.distribution || [];
    this.countText = opts.countText || '';
    this.render();
  }

  render() {
    const max = Math.max(...this.distribution.map(d => d.count), 1);

    // 左侧：分数 + 星星
    const full = Math.floor(this.score);
    const hasHalf = this.score - full >= 0.25;
    let starsHtml = '';
    for (let i = 0; i < 5; i++) {
      if (i < full) starsHtml += `<span class="star-full">${STAR_SVG.full}</span>`;
      else if (i === full && hasHalf) starsHtml += `<span class="star-half"><span class="star-empty">${STAR_SVG.empty}</span><span class="star-full star-half__clip">${STAR_SVG.full}</span></span>`;
      else starsHtml += `<span class="star-empty">${STAR_SVG.empty}</span>`;
    }

    // 右侧：进度条
    let barsHtml = '';
    this.distribution.forEach(d => {
      const pct = max > 0 ? (d.count / max * 100) : 0;
      const miniStars = Array(d.stars).fill('<span class="rating-bars__mini-star"></span>').join('');
      barsHtml += `<div class="rating-bars__row"><span class="rating-bars__label">${miniStars}</span><div class="rating-bars__track"><div class="rating-bars__fill" style="width:${pct}%"></div></div></div>`;
    });

    this.el.className = 'rating-bars';
    this.el.innerHTML = `
      <div class="rating-bars__left">
        <div class="rating-bars__score">${this.score.toFixed(1)}</div>
        <div class="rating-bars__stars">${starsHtml}</div>
      </div>
      <div class="rating-bars__right">
        ${barsHtml}
        <div class="rating-bars__count">${this.countText}</div>
      </div>`;
  }

  update(score, distribution, countText) {
    this.score = score;
    this.distribution = distribution;
    if (countText) this.countText = countText;
    this.render();
  }
}
```

## 调用示例

```javascript
new RatingBars('#rating-module', {
  score: 4.5,
  distribution: [
    { stars: 5, count: 7834 },
    { stars: 4, count: 1572 },
    { stars: 3, count: 628 },
    { stars: 2, count: 312 },
    { stars: 1, count: 141 }
  ],
  countText: '10,489 Ratings'
});
```

## 设计参数

| 元素 | 属性 |
|------|------|
| 分数 | 56sp, weight 330, `var(--on_surface)` |
| 星星 | 16×16, StarRating 组件 |
| mini-star | 8×8, CSS mask, `var(--on_surface_quaternary)` |
| 进度条底 | 120×4dp, radius 2dp, `var(--surface_container_medium)` |
| 进度条填充 | `var(--bb-primary)` |
| 行间距 | 4dp |
| 标签-进度条间距 | 8dp |
| 底部文字 | 13sp, 330, `var(--on_surface_quaternary)`, 右对齐 |

## 依赖

需要先加载 `StarRating` 组件的 CSS（`.star-full` / `.star-empty` / `.star-half`）和 `STAR_SVG` 常量。
