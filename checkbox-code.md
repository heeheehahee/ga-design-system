---
category: components-code
name: 多选 Checkbox 代码
description: Checkbox CSS + JS 代码。规范见 checkbox.md
---

# Checkbox 代码

规范：[checkbox.md](checkbox.md)

---

## CSS

```css
/* Checkbox 多选复选框 */
.checkbox {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 22px;
  height: 22px;
  border-radius: 50%;
  border: 1.6px solid var(--on_surface_octonary);
  background: transparent;
  position: relative;
  flex-shrink: 0;
  cursor: pointer;
  transition: background 0.2s, border-color 0.2s;
}

/* Selected */
.checkbox--selected {
  border-color: transparent;
  background: var(--primary);
}

/* Checkmark (injected via JS or pseudo-element) */
.checkbox--selected::after {
  content: '';
  position: absolute;
  width: 10px;
  height: 9px;
  background: var(--on_primary);
  -webkit-mask: url("data:image/svg+xml,%3Csvg width='10' height='9' viewBox='0 0 10 9' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1.01426 4.55086L3.36815 7.3058C3.57737 7.55067 3.95888 7.54101 4.15544 7.28587L8.98711 1.01422' stroke='black' stroke-width='2' stroke-linecap='round'/%3E%3C/svg%3E") center / contain no-repeat;
  mask: url("data:image/svg+xml,%3Csvg width='10' height='9' viewBox='0 0 10 9' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1.01426 4.55086L3.36815 7.3058C3.57737 7.55067 3.95888 7.54101 4.15544 7.28587L8.98711 1.01422' stroke='black' stroke-width='2' stroke-linecap='round'/%3E%3C/svg%3E") center / contain no-repeat;
}

/* Disabled */
.checkbox--disabled {
  cursor: not-allowed;
  border-color: var(--outline);
}

.checkbox--selected.checkbox--disabled {
  border-color: transparent;
  background: var(--disable_primary);
}

/* --- Media Type --- */
.checkbox--media {
  border-color: var(--white);
  background: var(--mask_menu);
}

.checkbox--media.checkbox--selected {
  border-color: var(--white);
  background: var(--primary);
}

.checkbox--media.checkbox--disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.checkbox--media.checkbox--selected.checkbox--disabled {
  border-color: var(--white);
  background: var(--primary);
  opacity: 0.5;
}
```

---

## JS

```js
/**
 * Checkbox
 * 用法：new Checkbox(container, { type, disabled, selected, onChange })
 */
class Checkbox {
  constructor(el, opts = {}) {
    this.el = el;
    this.type = opts.type || 'default'; // 'default' | 'media'
    this.disabled = opts.disabled || false;
    this.selected = opts.selected || false;
    this.onChange = opts.onChange || null;

    this.render();
    if (!this.disabled) {
      this.el.addEventListener('click', () => this.toggle());
    }
  }

  toggle() {
    if (this.disabled) return;
    this.selected = !this.selected;
    this.render();
    if (this.onChange) this.onChange(this.selected);
  }

  render() {
    this.el.className = 'checkbox';
    if (this.type === 'media') this.el.classList.add('checkbox--media');
    if (this.selected) this.el.classList.add('checkbox--selected');
    if (this.disabled) this.el.classList.add('checkbox--disabled');
  }

  setSelected(val) {
    this.selected = val;
    this.render();
  }

  setDisabled(val) {
    this.disabled = val;
    this.render();
  }
}
```

---

## 使用示例

```html
<span id="radio1"></span>
<span id="radio2"></span>

<script>
const r1 = new Checkbox(document.getElementById('radio1'), {
  type: 'default',
  onChange: (checked) => console.log('r1:', checked)
});

const r2 = new Checkbox(document.getElementById('radio2'), {
  type: 'media',
  selected: true,
  onChange: (checked) => console.log('r2:', checked)
});
</script>
```
