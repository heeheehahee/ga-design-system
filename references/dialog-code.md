---
category: components-code
name: Dialog 弹窗代码
description: 弹窗 CSS + JS 工具函数。规范见 dialog.md
---

# Dialog 弹窗代码

规范：[dialog.md](dialog.md)

---

## CSS

```css
/* 弹窗遮罩 */
.dialog-mask {
  position: fixed;
  inset: 0;
  background: var(--mask);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 0 12px;
}

/* 底部弹窗 */
.dialog-mask--bottom {
  align-items: flex-end;
  padding: 0 12px 16px;
}

/* 弹窗容器 */
.dialog {
  width: 100%;
  max-width: 368px;
  border-radius: 36px;
  background: var(--surface_pop_window);
  padding: 24px 0;
  display: flex;
  flex-direction: column;
  gap: 16px;
  box-shadow: 0 8px 40px rgba(0,0,0,0.15);
  box-sizing: border-box;
}

/* 标题 */
.dialog__title {
  text-align: center;
  font-size: 18px;
  font-weight: 380;
  color: var(--on_surface);
  padding: 0 28px;
  min-height: 24px;
}

/* 正文 */
.dialog__body {
  padding: 0 28px;
  font-size: 16px;
  font-weight: 330;
  color: var(--on_surface);
}
.dialog__body--single { text-align: center; }
.dialog__body--multi { text-align: left; }

/* 备注 */
.dialog__note {
  padding: 0 28px;
  font-size: 13px;
  font-weight: 330;
  color: var(--on_surface_quaternary);
}
.dialog__note--center { text-align: center; }
.dialog__note--left { text-align: left; }

/* 勾选行 */
.dialog__checkbox {
  padding: 0 28px;
  display: flex;
  align-items: center;
  gap: 8px;
  height: 22px;
}
.dialog__checkbox-label {
  font-size: 16px;
  font-weight: 330;
  color: var(--on_surface_secondary);
}

/* 按钮区 — 竖排 */
.dialog__btns-vertical {
  padding: 8px 24px 0;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

/* 按钮区 — 横排 */
.dialog__btns-horizontal {
  padding: 8px 24px 0;
  display: flex;
  gap: 12px;
}
.dialog__btns-horizontal .base-btn {
  flex: 1;
  min-width: 0;
}

/* 选项列表行 */
.dialog__option {
  height: 56px;
  padding: 14px 28px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  cursor: pointer;
}
.dialog__option-text {
  font-size: 17px;
  font-weight: 380;
  color: var(--on_surface);
}
.dialog__option-check {
  color: var(--bb-primary);
  font-size: 18px;
  font-weight: 600;
}

/* 插图区域 */
.dialog__illustration {
  width: calc(100% - 48px);
  aspect-ratio: 320 / 88;
  margin: 0 auto;
  background: var(--surface_container);
  border-radius: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* 输入框 */
.dialog__input {
  padding: 0 24px;
}
.dialog__input-field {
  width: 100%;
  height: 56px;
  border-radius: 12px;
  background: var(--surface_container);
  border: 1px solid var(--outline);
  display: flex;
  align-items: center;
  padding: 0 16px;
  font-size: 16px;
  color: var(--on_surface);
  box-sizing: border-box;
}

/* ProgressDialog（特殊：pill 形状） */
.dialog--progress {
  width: 100%;
  max-width: 368px;
  height: 72px;
  border-radius: 36px;
  background: var(--surface_pop_window);
  display: flex;
  align-items: center;
  padding: 0 24px;
  gap: 8px;
  box-shadow: 0 8px 40px rgba(0,0,0,0.15);
  box-sizing: border-box;
}

/* 权限弹窗选项卡 */
.dialog__perm-options {
  padding: 0 20px;
  display: flex;
  gap: 12px;
}
.dialog__perm-option {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  cursor: pointer;
}
.dialog__perm-preview {
  width: 100%;
  height: 99px;
  border-radius: 12px;
  background: var(--surface_container);
  border: 2px solid transparent;
}
.dialog__perm-preview--active {
  border-color: var(--bb-primary);
}
.dialog__perm-label {
  font-size: 14px;
  font-weight: 380;
  color: var(--on_surface);
}
```

---

## JS

```js
class Dialog {
  constructor(options = {}) {
    this.type = options.type || 'confirm';
    this.title = options.title || '';
    this.body = options.body || '';
    this.note = options.note || '';
    this.buttons = options.buttons || [];
    this.buttonsLayout = options.buttonsLayout || 'vertical';
    this.checkbox = options.checkbox || null;
    this.options = options.options || null;
    this.illustration = options.illustration || false;
    this.position = options.position || 'center';
    this.onButton = options.onButton || null;
    this.onClose = options.onClose || null;
    this._build();
  }

  _build() {
    this.mask = document.createElement('div');
    this.mask.className = 'dialog-mask' + (this.position === 'bottom' ? ' dialog-mask--bottom' : '');
    this.mask.addEventListener('click', (e) => {
      if (e.target === this.mask) this.close();
    });

    if (this.type === 'progress') {
      this._buildProgress();
    } else {
      this._buildStandard();
    }

    document.body.appendChild(this.mask);
  }

  _buildProgress() {
    const d = document.createElement('div');
    d.className = 'dialog--progress';
    d.innerHTML = `<div class="loading-icon" style="flex-direction:row;gap:8px;color:var(--on_surface)"><div class="loading-icon__spinner"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><path d="M12 0C18.6274 0 24 5.37258 24 12C24 18.6274 18.6274 24 12 24C5.37258 24 0 18.6274 0 12C0 5.37258 5.37258 0 12 0ZM12 2.30762C6.64709 2.30762 2.30762 6.64709 2.30762 12C2.30762 17.3529 6.64709 21.6924 12 21.6924C17.3529 21.6924 21.6924 17.3529 21.6924 12C21.6924 6.64709 17.3529 2.30762 12 2.30762ZM6.92285 9.69238C8.19736 9.69238 9.23047 10.7255 9.23047 12C9.23047 13.2745 8.19735 14.3076 6.92285 14.3076C5.64845 14.3075 4.61523 13.2744 4.61523 12C4.61523 10.7256 5.64845 9.6925 6.92285 9.69238Z" fill="currentColor"/></svg></div><span style="font-size:17px;font-weight:380">${this.body}</span></div>`;
    this.mask.appendChild(d);
    this.el = d;
  }

  _buildStandard() {
    const d = document.createElement('div');
    d.className = 'dialog';

    if (this.illustration) {
      const img = document.createElement('div');
      img.className = 'dialog__illustration';
      d.appendChild(img);
    }

    if (this.title) {
      const t = document.createElement('div');
      t.className = 'dialog__title';
      t.textContent = this.title;
      d.appendChild(t);
    }

    if (this.body) {
      const b = document.createElement('div');
      b.className = 'dialog__body ' + (this.body.length > 20 ? 'dialog__body--multi' : 'dialog__body--single');
      b.textContent = this.body;
      d.appendChild(b);
    }

    if (this.note) {
      const n = document.createElement('div');
      n.className = 'dialog__note ' + (this.body && this.body.length > 20 ? 'dialog__note--left' : 'dialog__note--center');
      n.textContent = this.note;
      d.appendChild(n);
    }

    if (this.checkbox) {
      const ck = document.createElement('div');
      ck.className = 'dialog__checkbox';
      ck.innerHTML = `<div class="checkbox"></div><span class="dialog__checkbox-label">${this.checkbox}</span>`;
      ck.querySelector('.checkbox').addEventListener('click', function() { this.classList.toggle('checkbox--selected'); });
      d.appendChild(ck);
    }

    if (this.options) {
      const list = document.createElement('div');
      let selected = this.options.findIndex(o => o.selected) || 0;
      const rows = [];
      this.options.forEach((opt, i) => {
        const row = document.createElement('div');
        row.className = 'dialog__option';
        row.innerHTML = `<span class="dialog__option-text">${opt.text}</span><span class="dialog__option-check" style="display:${i === selected ? 'block' : 'none'}">✓</span>`;
        row.addEventListener('click', () => {
          selected = i;
          rows.forEach((r, j) => { r.querySelector('.dialog__option-check').style.display = j === i ? 'block' : 'none'; });
        });
        rows.push(row);
        list.appendChild(row);
      });
      d.appendChild(list);
    }

    if (this.buttons.length) {
      const a = document.createElement('div');
      a.className = this.buttonsLayout === 'horizontal' ? 'dialog__btns-horizontal' : 'dialog__btns-vertical';
      this.buttons.forEach((btn, i) => {
        const b = document.createElement('button');
        b.className = 'base-btn base-btn--large base-btn--' + (btn.type || 'secondary');
        b.dataset.state = 'enabled';
        b.textContent = btn.text;
        b.addEventListener('click', () => {
          if (this.onButton) this.onButton(i, btn);
          this.close();
        });
        a.appendChild(b);
      });
      d.appendChild(a);
    }

    this.mask.appendChild(d);
    this.el = d;
  }

  close() {
    this.mask.remove();
    if (this.onClose) this.onClose();
  }
}
```

---

## 调用示例

```js
// 确认弹窗 — 单行
new Dialog({
  title: '标题',
  body: '正文，单行居中',
  buttons: [
    { text: '主操作', type: 'primary' },
    { text: '辅助操作', type: 'secondary' }
  ]
});

// 确认弹窗 — 带勾选
new Dialog({
  title: '标题',
  body: '确认删除吗？',
  checkbox: '不再提醒',
  buttons: [
    { text: '确定', type: 'primary' },
    { text: '取消', type: 'secondary' }
  ]
});

// 警告弹窗 — 横排按钮
new Dialog({
  title: '卸载"快手"',
  body: '卸载后其所有数据也将被删除',
  buttonsLayout: 'horizontal',
  buttons: [
    { text: '取消', type: 'secondary' },
    { text: '卸载', type: 'destructive' }
  ]
});

// 选择弹窗
new Dialog({
  title: '单选弹窗',
  options: [
    { text: '选项一' },
    { text: '选项二' },
    { text: '选项三', selected: true }
  ],
  buttonsLayout: 'horizontal',
  buttons: [
    { text: '取消', type: 'secondary' },
    { text: '确定', type: 'primary' }
  ]
});

// 引导弹窗 — 带插图
new Dialog({
  illustration: true,
  title: '电量已低于20%',
  body: '建议开启省电模式',
  buttonsLayout: 'horizontal',
  buttons: [
    { text: '取消', type: 'secondary' },
    { text: '开启', type: 'primary' }
  ]
});

// 进度弹窗
new Dialog({
  type: 'progress',
  body: '正在更新请稍候...'
});
```

---

## 依赖

- `.base-btn`（button-code.md）— 弹窗按钮
- `.checkbox`（checkbox-code.md）— 勾选框
- `.loading-icon`（loading-code.md）— 进度弹窗旋转图标
- `var(--surface_pop_window)`、`var(--on_surface)` 等 Token（color.md）
