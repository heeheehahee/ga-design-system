---
category: components-code
name: Input 输入框代码
description: Input CSS + JS 代码。规范见 input.md
---

# Input 输入框代码

规范：[input.md](input.md)

---

## CSS

```css
/* ====== Input 输入框 ====== */

/* 外容器 */
.input-wrap { display: flex; align-items: center; padding: 0 12px; width: 100%; box-sizing: border-box; }
.input-wrap--full { padding: 0; }

/* 输入框背景 */
.input-bg {
  display: flex;
  align-items: center;
  flex: 1;
  height: 56px;
  background: var(--surface);
  position: relative;
}

/* 背景环境 */
.input-wrap--gray-bg .input-bg { background: var(--surface_container_low); }

/* 位置圆角 */
.input-bg--standalone { border-radius: 16px; }
.input-bg--top { border-radius: 16px 16px 0 0; }
.input-bg--middle { border-radius: 0; }
.input-bg--bottom { border-radius: 0 0 16px 16px; }

/* 分隔线（上/中行底部） */
.input-bg--top::after, .input-bg--middle::after {
  content: '';
  position: absolute;
  bottom: 0; left: 16px; right: 16px;
  height: 0.5px;
  background: var(--outline);
}

/* 激活态 — 整圈描边 */
.input-bg--active {
  box-shadow: inset 0 0 0 2px var(--primary);
  border-radius: 16px;
}
.input-bg--active::after { display: none; }

/* 报错态 */
.input-bg--error {
  box-shadow: inset 0 0 0 2px var(--error);
  border-radius: 16px;
}
.input-bg--error::after { display: none; }

/* ---- 后缀输入框 ---- */
.input-text-area {
  flex: 1;
  padding: 16.5px 16px;
  min-width: 0;
}
.input-field {
  display: block;
  width: 100%;
  border: none;
  background: transparent;
  outline: none;
  font-size: 17px;
  font-weight: 380;
  color: var(--on_surface);
  font-family: inherit;
  padding: 0;
  caret-color: var(--primary);
  box-sizing: border-box;
}
.input-field::placeholder { color: var(--on_surface_octonary); }

/* 后缀图标区 */
.input-suffix {
  display: flex;
  align-items: center;
  padding: 0 16px 0 0;
  align-self: stretch;
}
.input-suffix__icon {
  width: 28px; height: 28px;
  color: var(--on_surface_quaternary);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}
.input-suffix__icon svg { width: 28px; height: 28px; }
.input-suffix__icon:active { opacity: 0.6; }

/* ---- 前缀输入框 ---- */
.input-prefix-area {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 16.5px 10px 16.5px 16px;
  flex-shrink: 0;
}
.input-prefix-label {
  font-size: 17px;
  font-weight: 380;
  color: var(--on_surface);
  flex-shrink: 0;
}
.input-value-area {
  display: flex;
  align-items: center;
  gap: 12px;
  flex: 1;
  padding: 0 16px 0 0;
  min-width: 0;
}
.input-field--prefix {
  display: block;
  min-width: 0;
  flex: 1;
  border: none;
  background: transparent;
  outline: none;
  font-size: 16px;
  font-weight: 330;
  color: var(--on_surface);
  font-family: inherit;
  padding: 0;
  text-align: right;
  width: 100%;
  caret-color: var(--primary);
}
.input-field--prefix::placeholder { color: var(--on_surface_octonary); }

/* 报错提示文字 */
.input-error-msg {
  padding: 8px 12px 16px;
  font-size: 13px;
  font-weight: 330;
  color: var(--error);
}
```

---

## JS

```js
class InputField {
  constructor(container, options = {}) {
    this.container = typeof container === 'string' ? document.querySelector(container) : container;
    this.type = options.type || 'suffix';
    this.label = options.label || '前缀';
    this.placeholder = options.placeholder || '输入提示';
    this.position = options.position || 'standalone';
    this.card = options.card !== false;
    this.grayBg = options.grayBg || false;
    this.error = options.error || '';
    this.showSuffix = options.showSuffix || false;
    this.onChange = options.onChange || null;
    this._build();
  }

  _build() {
    this.wrap = document.createElement('div');
    this.wrap.className = 'input-wrap' + (this.card ? '' : ' input-wrap--full') + (this.grayBg ? ' input-wrap--gray-bg' : '');

    this.bg = document.createElement('div');
    this.bg.className = 'input-bg input-bg--' + this.position;

    if (this.type === 'suffix') {
      this._buildSuffix();
    } else {
      this._buildPrefix();
    }

    this.wrap.appendChild(this.bg);
    this.container.appendChild(this.wrap);

    if (this.error) {
      this.bg.classList.add('input-bg--error');
      const err = document.createElement('div');
      err.className = 'input-error-msg';
      err.textContent = this.error;
      this.wrap.style.flexWrap = 'wrap';
      this.wrap.appendChild(err);
    }
  }

  _buildSuffix() {
    const textArea = document.createElement('div');
    textArea.className = 'input-text-area';
    this.field = document.createElement('input');
    this.field.className = 'input-field';
    this.field.type = 'text';
    this.field.placeholder = this.placeholder;
    textArea.appendChild(this.field);
    this.bg.appendChild(textArea);

    if (this.showSuffix) {
      const suffix = document.createElement('div');
      suffix.className = 'input-suffix';
      const icon = document.createElement('div');
      icon.className = 'input-suffix__icon';
      icon.innerHTML = '<svg viewBox="0 0 24 24" fill="none"><path d="M12 4.5C7 4.5 2.73 7.61 1 12c1.73 4.39 6 7.5 11 7.5s9.27-3.11 11-7.5c-1.73-4.39-6-7.5-11-7.5zM12 17c-2.76 0-5-2.24-5-5s2.24-5 5-5 5 2.24 5 5-2.24 5-5 5zm0-8c-1.66 0-3 1.34-3 3s1.34 3 3 3 3-1.34 3-3-1.34-3-3-3z" fill="currentColor"/></svg>';
      suffix.appendChild(icon);
      this.bg.appendChild(suffix);
    }

    this._bindEvents();
  }

  _buildPrefix() {
    const prefixArea = document.createElement('div');
    prefixArea.className = 'input-prefix-area';
    const label = document.createElement('span');
    label.className = 'input-prefix-label';
    label.textContent = this.label;
    prefixArea.appendChild(label);
    this.bg.appendChild(prefixArea);

    const valueArea = document.createElement('div');
    valueArea.className = 'input-value-area';
    this.field = document.createElement('input');
    this.field.className = 'input-field--prefix';
    this.field.type = 'text';
    this.field.placeholder = this.placeholder;
    valueArea.appendChild(this.field);

    if (this.showSuffix) {
      const icon = document.createElement('div');
      icon.className = 'input-suffix__icon';
      icon.innerHTML = '<svg viewBox="0 0 24 24" fill="none"><path d="M12 4.5C7 4.5 2.73 7.61 1 12c1.73 4.39 6 7.5 11 7.5s9.27-3.11 11-7.5c-1.73-4.39-6-7.5-11-7.5zM12 17c-2.76 0-5-2.24-5-5s2.24-5 5-5 5 2.24 5 5-2.24 5-5 5zm0-8c-1.66 0-3 1.34-3 3s1.34 3 3 3 3-1.34 3-3-1.34-3-3-3z" fill="currentColor"/></svg>';
      valueArea.appendChild(icon);
    }

    this.bg.appendChild(valueArea);
    this._bindEvents();
  }

  _bindEvents() {
    this.field.addEventListener('focus', () => {
      if (!this.bg.classList.contains('input-bg--error')) {
        this.bg.classList.add('input-bg--active');
      }
    });
    this.field.addEventListener('blur', () => {
      this.bg.classList.remove('input-bg--active');
    });
    this.field.addEventListener('input', () => {
      if (this.onChange) this.onChange(this.field.value);
    });
  }

  setError(msg) {
    this.bg.classList.toggle('input-bg--error', !!msg);
    const existing = this.wrap.querySelector('.input-error-msg');
    if (existing) existing.remove();
    if (msg) {
      const err = document.createElement('div');
      err.className = 'input-error-msg';
      err.textContent = msg;
      this.wrap.appendChild(err);
    }
  }
}
```

---

## 调用示例

```js
// 后缀输入框 — 套卡/独立/默认
new InputField('#demo', {
  type: 'suffix',
  placeholder: '请输入密码',
  position: 'standalone',
  showSuffix: true
});

// 前缀输入框 — 套卡/独立
new InputField('#demo', {
  type: 'prefix',
  label: '手机号',
  placeholder: '请输入',
  position: 'standalone'
});

// 通栏/灰反白
new InputField('#demo', {
  type: 'suffix',
  placeholder: '搜索',
  card: false,
  grayBg: true
});

// 带报错
new InputField('#demo', {
  type: 'suffix',
  placeholder: '请输入密码',
  error: '密码不正确，请重新输入'
});
```

---

## 依赖

- `var(--surface)`, `var(--surface_container)` — 背景环境
- `var(--on_surface)`, `var(--on_surface_octonary)`, `var(--on_surface_quaternary)` — 文字/图标色
- `var(--primary)` — 激活态描边
- `var(--error)` — 报错态描边 + 文字
- `var(--outline)` — 分隔线
