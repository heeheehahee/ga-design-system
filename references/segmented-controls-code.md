---
category: components-code
name: SegmentedControls 分段按钮代码
description: SegmentedControls CSS + JS 代码。规范见 segmented-controls.md
---

# SegmentedControls 代码

⚠️ 复制代码前必须先读使用规则：[segmented-controls.md](segmented-controls.md)

---

## CSS

```css
/* SegmentedControls 分段按钮 */
.seg-ctrl { display: flex; align-items: center; padding: 0 12px; width: 100%; height: 40px; }
.seg-ctrl__track { position: relative; width: 100%; height: 40px; background: var(--surface_high); border-radius: 20px; }
.seg-ctrl__group { position: absolute; top: 4px; left: 4px; right: 4px; bottom: 4px; display: flex; align-items: center; gap: 2px; }
.seg-ctrl__indicator { position: absolute; top: 0; bottom: 0; border-radius: 16px; background: var(--surface_container); transition: left 0.25s ease, width 0.25s ease; }
.seg-ctrl__item { flex: 1; display: flex; align-items: center; justify-content: center; gap: 2px; padding: 6px 0; border-radius: 16px; font-size: 14px; font-weight: 380; color: var(--on_surface); cursor: pointer; white-space: nowrap; }

/* 白色背景 */
.seg-ctrl--on-white .seg-ctrl__track { border: 1px solid var(--surface_container); }

/* 彩色背景（玻璃材质） */
.seg-ctrl--on-color .seg-ctrl__track { background: linear-gradient(rgba(0,0,0,0.1), rgba(0,0,0,0.1)), linear-gradient(rgba(86,86,86,0.4), rgba(86,86,86,0.4)), linear-gradient(rgba(63,63,63,0.6), rgba(63,63,63,0.6)); backdrop-filter: blur(40px); -webkit-backdrop-filter: blur(40px); box-shadow: 0px 24px 60px rgba(0,0,0,0.08), 0px 0px 40px rgba(0,0,0,0.06); overflow: hidden; position: relative; }
.seg-ctrl--on-color .seg-ctrl__track::before { content: ''; position: absolute; inset: 0; border-radius: 20px; padding: 0.8px; background: linear-gradient(180deg, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 50%, rgba(255,255,255,0.1) 100%); -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0); mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0); -webkit-mask-composite: xor; mask-composite: exclude; pointer-events: none; }
.seg-ctrl--on-color .seg-ctrl__indicator { background: rgba(177,177,177,0.19); }
.seg-ctrl--on-color .seg-ctrl__item { color: rgba(255,255,255,0.95); font-weight: 450; padding: 4px 0; }
```

---

## JS

```js
/**
 * SegmentedControls
 * 用法：new SegmentedControls(container, { items, active, onChange })
 */
class SegmentedControls {
  constructor(el, opts = {}) {
    this.el = typeof el === 'string' ? document.querySelector(el) : el;
    this.items = opts.items || [];
    this.activeIndex = opts.active || 0;
    this.onChange = opts.onChange || null;
    this.render();
  }

  render() {
    this.el.className = 'seg-ctrl';
    this.el.innerHTML = `<div class="seg-ctrl__track"><div class="seg-ctrl__group"><div class="seg-ctrl__indicator"></div>${this.items.map((item, i) => `<div class="seg-ctrl__item" data-index="${i}">${item}</div>`).join('')}</div></div>`;
    this.indicator = this.el.querySelector('.seg-ctrl__indicator');
    new IntersectionObserver((entries, obs) => {
      if (entries[0].isIntersecting) { this.moveIndicator(false); obs.disconnect(); }
    }).observe(this.el);
    new ResizeObserver(() => { this.moveIndicator(false); }).observe(this.el);
    this.el.querySelectorAll('.seg-ctrl__item').forEach(item => {
      item.addEventListener('click', () => {
        const index = parseInt(item.dataset.index);
        if (index === this.activeIndex) {
          if (!this.indicator.offsetWidth) this.moveIndicator(false);
          return;
        }
        if (!this.indicator.offsetWidth) this.moveIndicator(false);
        requestAnimationFrame(() => { this.activeIndex = index; this.moveIndicator(true); });
        if (this.onChange) this.onChange(index, this.items[index]);
      });
    });
  }

  moveIndicator(animate) {
    const items = this.el.querySelectorAll('.seg-ctrl__item');
    const active = items[this.activeIndex];
    if (!active) return;
    if (!animate) this.indicator.style.transition = 'none';
    this.indicator.style.left = active.offsetLeft + 'px';
    this.indicator.style.width = active.offsetWidth + 'px';
    if (!animate) requestAnimationFrame(() => { this.indicator.style.transition = ''; });
  }
}
```

---

## 使用示例

```html
<div id="seg-demo"></div>

<script>
new SegmentedControls('#seg-demo', {
  items: ['推荐', '最新', '热门'],
  active: 0,
  onChange: (index, label) => console.log(index, label)
});
</script>
```
