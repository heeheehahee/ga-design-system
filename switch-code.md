---
category: components-code
name: Switch 开关代码
description: Switch CSS + JS 代码。规范见 switch.md
---

# Switch 代码

规范：[switch.md](switch.md)

---

## CSS

```css
/* Switch 开关 */
.switch {
  position: relative;
  width: 49px;
  height: 28px;
  border-radius: 14px;
  background: var(--outline);
  cursor: pointer;
  transition: background 0.2s ease;
  flex-shrink: 0;
}

/* 滑块 */
.switch::after {
  content: '';
  position: absolute;
  top: 4px;
  left: 4px;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: var(--white);
  box-shadow: 0 2px 4px rgba(0,0,0,0.15);
  transition: transform 0.2s ease;
}

/* On */
.switch--on {
  background: var(--primary);
}

.switch--on::after {
  transform: translateX(21px);
}

/* Disabled */
.switch--disabled {
  cursor: not-allowed;
  background: var(--surface_container);
}

.switch--on.switch--disabled {
  background: var(--disable_primary);
}

/* Loading */
.switch--loading {
  cursor: not-allowed;
  background: var(--disable_primary);
}

.switch--loading::after {
  transform: translateX(21px);
}
```

---

## JS

```js
/**
 * Switch
 * 用法：new Switch(container, { on, disabled, loading, onChange })
 */
class Switch {
  constructor(el, opts = {}) {
    this.el = el;
    this.on = opts.on || false;
    this.disabled = opts.disabled || false;
    this.loading = opts.loading || false;
    this.onChange = opts.onChange || null;

    this.render();
    this.el.addEventListener('click', () => this.toggle());
  }

  toggle() {
    if (this.disabled || this.loading) return;
    this.on = !this.on;
    this.render();
    if (this.onChange) this.onChange(this.on);
  }

  render() {
    this.el.className = 'switch';
    if (this.on) this.el.classList.add('switch--on');
    if (this.disabled) this.el.classList.add('switch--disabled');
    if (this.loading) this.el.classList.add('switch--loading');
  }

  setOn(val) {
    this.on = val;
    this.render();
  }

  setDisabled(val) {
    this.disabled = val;
    this.render();
  }

  setLoading(val) {
    this.loading = val;
    this.render();
  }
}
```

---

## 使用示例

```html
<div id="switch1"></div>
<div id="switch2"></div>

<script>
const s1 = new Switch(document.getElementById('switch1'), {
  onChange: (on) => console.log('switch1:', on)
});

const s2 = new Switch(document.getElementById('switch2'), {
  on: true,
  onChange: (on) => {
    s2.setLoading(true);
    setTimeout(() => s2.setLoading(false), 2000);
  }
});
</script>
```
