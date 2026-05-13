---
category: components-code
name: 按钮代码实现
description: 按钮组件的 CSS + JS 代码，按需读取。设计规范见 button.md
---

# 按钮代码实现

设计规范：[button.md](button.md)
预览：[preview.html](../preview.html)

---

## 通用样式 — 玻璃材质圆形按钮

适用于标题栏浮起态图标、悬浮操作按钮等需要玻璃拟态效果的圆形按钮。
直接给元素加 `.glass-btn` 即可，尺寸由调用方自定义。

```css
.glass-btn {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 1000px;
  /* 填充：#1f1f1f 50% 半透明深色 */
  background: rgba(31,31,31,0.5);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: none;
  /* 双向 inner shadow：左上+右下各一层白色微光 */
  box-shadow:
    inset 2px 2px 3px rgba(255,255,255,0.1),
    inset -2px -2px 4px rgba(255,255,255,0.1);
  color: var(--on_surface);
  cursor: pointer;
  transition: background 0.3s, box-shadow 0.3s, backdrop-filter 0.3s;
}
/* 亮色模式：填充 #e5e5e5 50%，图标用 token */
.theme-light .glass-btn {
  background: rgba(229,229,229,0.5);
}
/* 渐显/渐隐：滚动浮起时从透明过渡到玻璃材质，图标不受影响 */
.glass-btn--entering {
  background: transparent !important;
  box-shadow: none !important;
  backdrop-filter: blur(0) !important;
  -webkit-backdrop-filter: blur(0) !important;
}
.glass-btn--entering::before { opacity: 0 !important; }
/* 渐变描边：0.8px 135deg，用 ::before + mask-composite 挖空 padding-box 只露边缘 */
.glass-btn::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  border: 0.8px solid transparent;
  background: linear-gradient(135deg,
    rgba(255,255,255,0.5) 0%,
    rgba(255,255,255,0.1) 40%,
    rgba(255,255,255,0.1) 60%,
    rgba(255,255,255,0.5) 100%) border-box;
  -webkit-mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0) border-box;
  -webkit-mask-composite: xor;
  mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0) border-box;
  mask-composite: exclude;
  pointer-events: none;
  z-index: 1;
  transition: opacity 0.3s;
}
/* 按下态遮罩 */
.glass-btn::after {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  background: transparent;
  pointer-events: none;
  transition: background 0.15s;
}
.glass-btn:active::after { background: rgba(0,0,0,0.15); }
```

### 调用示例

```html
<!-- 标题栏浮起态图标按钮 44×44 -->
<div class="glass-btn" style="width:44px;height:44px;padding:10px 12px">
  <svg ...><!-- 图标 SVG --></svg>
</div>

<!-- 大号悬浮按钮 56×56 -->
<div class="glass-btn" style="width:56px;height:56px">
  <svg ...><!-- 图标 SVG --></svg>
</div>
```

---

## 社区主色覆盖 CSS

社区模块页面需在 `<body>` 添加 `theme-community`，亮色模式加 `theme-light`。
文字色按「底色类型 × 明暗模式」处理，详见 button.md 主色规则。

```css
/* 覆盖主色变量 */
body.theme-community {
  --bb-primary: #FFE249;
  --bb-primary-rgb: 255, 226, 73;
  --ib-primary: #FFE249;
  --ib-primary-rgb: 255, 226, 73;
}

/* ---- 1. 实色主色底 → 始终黑字 ---- */

body.theme-community .base-btn--primary { color: #000; }
body.theme-community .base-btn--primary::after { background: rgba(0,0,0,0.1); }

body.theme-community .install-btn--large[data-state="enabled"],
body.theme-community .install-btn--medium[data-state="enabled"],
body.theme-community .install-btn--large[data-state="pressed"],
body.theme-community .install-btn--medium[data-state="pressed"],
body.theme-community .install-btn--large[data-state="installing"],
body.theme-community .install-btn--medium[data-state="installing"],
body.theme-community .install-btn--large[data-state="open"],
body.theme-community .install-btn--medium[data-state="open"],
body.theme-community .install-btn--large[data-state="update"],
body.theme-community .install-btn--medium[data-state="update"],
body.theme-community .install-btn--small[data-state="installing"],
body.theme-community .install-btn--small[data-state="open"],
body.theme-community .install-btn--icon[data-state="installing"],
body.theme-community .install-btn--icon[data-state="open"] { color: #000; }

body.theme-community .reservation-btn--reserved { color: #000; }
body.theme-community .install-btn__dl-clip .install-btn__dl-top { color: #000 !important; }

/* ---- 2. 浅色底/灰底 → 暗色主题黄色，亮色黑字 ---- */

body.theme-community.theme-light .base-btn--small.base-btn--secondary { color: #000; }

body.theme-community.theme-light .install-btn--large[data-state="connecting"],
body.theme-community.theme-light .install-btn--medium[data-state="connecting"],
body.theme-community.theme-light .install-btn--large[data-state="downloading"],
body.theme-community.theme-light .install-btn--medium[data-state="downloading"],
body.theme-community.theme-light .install-btn--large[data-state="resume"],
body.theme-community.theme-light .install-btn--medium[data-state="resume"],
body.theme-community.theme-light .install-btn--small[data-state="enabled"],
body.theme-community.theme-light .install-btn--small[data-state="pressed"],
body.theme-community.theme-light .install-btn--small[data-state="connecting"],
body.theme-community.theme-light .install-btn--small[data-state="downloading"],
body.theme-community.theme-light .install-btn--small[data-state="resume"],
body.theme-community.theme-light .install-btn--small[data-state="update"],
body.theme-community.theme-light .install-btn--icon[data-state="enabled"],
body.theme-community.theme-light .install-btn--icon[data-state="pressed"],
body.theme-community.theme-light .install-btn--icon[data-state="connecting"],
body.theme-community.theme-light .install-btn--icon[data-state="downloading"],
body.theme-community.theme-light .install-btn--icon[data-state="resume"],
body.theme-community.theme-light .install-btn--icon[data-state="update"] { color: #000; }

body.theme-community.theme-light .install-btn__dl-top { color: #000 !important; }
body.theme-community.theme-light .reservation-btn--not-reserved { color: #000; }

/* ---- 3. 亮色下浅色底透明度 10% → 30% ---- */

body.theme-community.theme-light .base-btn--small.base-btn--secondary { background: rgba(var(--bb-primary-rgb), 0.3); }

body.theme-community.theme-light .install-btn--small[data-state="enabled"],
body.theme-community.theme-light .install-btn--icon[data-state="enabled"],
body.theme-community.theme-light .install-btn--small[data-state="connecting"],
body.theme-community.theme-light .install-btn--icon[data-state="connecting"],
body.theme-community.theme-light .install-btn--small[data-state="downloading"],
body.theme-community.theme-light .install-btn--icon[data-state="downloading"],
body.theme-community.theme-light .install-btn--small[data-state="resume"],
body.theme-community.theme-light .install-btn--icon[data-state="resume"],
body.theme-community.theme-light .install-btn--small[data-state="update"],
body.theme-community.theme-light .install-btn--icon[data-state="update"] { background: rgba(var(--ib-primary-rgb), 0.3); }

body.theme-community.theme-light .install-btn--small[data-state="pressed"],
body.theme-community.theme-light .install-btn--icon[data-state="pressed"] { background: rgba(var(--ib-primary-rgb), 0.38); }

body.theme-community.theme-light .reservation-btn--not-reserved { background: rgba(var(--ib-primary-rgb), 0.3); }
body.theme-community.theme-light .reservation-btn--not-reserved[data-state="pressed"] { background: rgba(var(--ib-primary-rgb), 0.38); }
```

---

## BaseButton

### CSS

```css
/* 主色变量 + HyperOS3 Feature Token 映射（暗色默认） */
:root {
  --bb-primary: #05C474;
  --bb-primary-rgb: 5, 196, 116;
  --bb-danger: #FA382E;
  --bb-danger-rgb: 250, 56, 46;
  --ib-primary: #05C474;
  --ib-primary-rgb: 5, 196, 116;
  --btn-on-primary: #ffffffe6;           /* on_primary dark */
  --btn-secondary-bg: #ffffff24;          /* tertiary dark */
  --btn-secondary-text: #ffffffcc;        /* on_surface_secondary dark */
  --btn-secondary-pressed: #ffffff1f;     /* container_list_touch dark */
  --btn-disabled-bg: #ffffff14;           /* surface_container dark */
  --btn-disabled-text: #ffffff4d;         /* on_surface_button_disable dark */
  --btn-destructive: #fa4238;             /* error dark */
  --btn-destructive-disabled: #fa42384c;  /* error_disable dark */
  --btn-surface-touch: #ffffff24;         /* surface_touch dark */
}
/* 亮色模式 */
body.theme-light {
  --btn-on-primary: #ffffff;              /* on_primary light */
  --btn-secondary-bg: #0000000f;          /* tertiary light */
  --btn-secondary-text: #000000cc;        /* on_surface_secondary light */
  --btn-secondary-pressed: #0000001f;     /* container_list_touch light */
  --btn-disabled-bg: #0000000a;           /* surface_container light */
  --btn-disabled-text: #00000066;         /* on_surface_quaternary light */
  --btn-destructive: #fa382e;             /* error light */
  --btn-destructive-disabled: #fa382e4c;  /* error_disable light */
  --btn-surface-touch: #0000001a;         /* surface_touch light */
}

.base-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: none;
  cursor: pointer;
  overflow: hidden;
  user-select: none;
  -webkit-tap-highlight-color: transparent;
  font-family: -apple-system, "MiSans", "Helvetica Neue", sans-serif;
  flex-shrink: 0;
  outline: none;
  white-space: nowrap;
}

.base-btn::after {
  content: '';
  position: absolute;
  inset: 0;
  z-index: 10;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.1s;
  border-radius: inherit;
}
.base-btn:active::after,
.base-btn[data-state="pressed"]::after {
  opacity: 1;
}
.base-btn--primary::after { background: var(--btn-surface-touch); }
.base-btn--secondary::after,
.base-btn--destructive::after { background: var(--btn-secondary-pressed); }
.base-btn[data-state="disabled"]::after { display: none; }
.base-btn[data-state="disabled"] { cursor: not-allowed; }

.base-btn--large { width: 100%; height: 50px; border-radius: 16px; padding: 8px 20px; font-size: 17px; font-weight: 380; }
.base-btn--medium { height: 36px; border-radius: 999px; padding: 8px 20px; font-size: 14px; font-weight: 380; }
.base-btn--small { height: 32px; border-radius: 999px; padding: 0 12px; font-size: 14px; font-weight: 500; }
.base-btn--mini { height: 28px; border-radius: 999px; padding: 4px 12px; font-size: 13px; font-weight: 330; }

.base-btn--primary { background: var(--bb-primary); color: var(--btn-on-primary); }
.base-btn--secondary { background: var(--btn-secondary-bg); color: var(--btn-secondary-text); }
.base-btn--secondary[data-state="disabled"] { background: var(--btn-disabled-bg); color: var(--btn-disabled-text); }
.base-btn--small.base-btn--secondary { background: rgba(var(--bb-primary-rgb), 0.1); color: var(--bb-primary); }
.base-btn--small.base-btn--secondary[data-state="pressed"] { background: rgba(var(--bb-primary-rgb), 0.16); }
.base-btn--small.base-btn--secondary[data-state="disabled"] { background: var(--btn-disabled-bg); color: var(--btn-disabled-text); }
.base-btn--destructive { background: var(--btn-secondary-bg); color: var(--btn-destructive); }
.base-btn--destructive[data-state="disabled"] { background: var(--btn-disabled-bg); color: var(--btn-destructive-disabled); }

.base-btn-group { display: flex; gap: 12px; }
.base-btn-group--vertical { flex-direction: column; }
.base-btn-group--horizontal { flex-direction: row; }
.base-btn-group--horizontal .base-btn { flex: 1; }
```

### JS

```js
class BaseButton {
  constructor(container, { size = 'large', type = 'primary', state = 'enabled', text = '' } = {}) {
    this.container = typeof container === 'string' ? document.querySelector(container) : container;
    this.size = size; this.type = type; this.state = state; this.text = text; this.onClick = null;
    this._build();
  }
  _build() {
    const btn = document.createElement('button');
    btn.className = `base-btn base-btn--${this.size} base-btn--${this.type}`;
    btn.dataset.state = this.state;
    btn.textContent = this.text;
    this.el = btn;
    btn.addEventListener('click', () => { if (this.state !== 'disabled' && this.onClick) this.onClick(this); });
    this.container.innerHTML = '';
    this.container.appendChild(btn);
  }
  setState(state) { this.state = state; this.el.dataset.state = state; }
  setText(text) { this.text = text; this.el.textContent = text; }
}
```

---

## CustomButton

### PostButton CSS

```css
.post-btn-container {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
  transition: transform 0.15s;
}
.post-btn-container:active {
  transform: scale(0.95);
}
.post-btn-container__avatar {
  margin-bottom: -20px;
  z-index: 1;
  pointer-events: none;
}
.post-btn-container__btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 128px;
  height: 46px;
  border-radius: 23px;
  border: 1px solid rgba(0,0,0,0.08);
  background: linear-gradient(180deg, #FFDC21 0%, #FFEF98 100%);
  gap: 4px;
  cursor: pointer;
  outline: none;
  font-family: inherit;
  overflow: hidden;
  box-shadow: 0 24px 60px rgba(0,0,0,0.08), 0 0 40px rgba(0,0,0,0.06);
}
.post-btn-container__btn::after {
  content: '';
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.1);
  opacity: 0;
  pointer-events: none;
  border-radius: inherit;
  transition: opacity 0.1s;
}
.post-btn-container:active .post-btn-container__btn::after {
  opacity: 1;
}
.post-btn-container__plus {
  font-size: 15px;
  font-weight: 500;
  color: #000;
}
.post-btn-container__text {
  font-size: 16px;
  font-weight: 450;
  color: #000;
}
```

### PostButton HTML 结构

```html
<div class="post-btn-container">
  <div class="post-btn-container__avatar">
    <!-- IP 形象 SVG，从 Figma 导出 -->
    <svg width="43" height="34" viewBox="0 0 43 34">...</svg>
  </div>
  <div class="post-btn-container__btn">
    <span class="post-btn-container__plus">+</span>
    <span class="post-btn-container__text">Team Up</span>
  </div>
</div>
```

> IP 形象 SVG 完整代码见 preview.html 中的内嵌版本。

### SpeedUp CSS

```css
.speedup-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 4px;
  height: 36px;
  border-radius: 999px;
  padding: 9px 12px;
  border: none;
  cursor: pointer;
  background: #FFE249;
  color: #000;
  font-family: inherit;
  font-size: 14px;
  font-weight: 450;
  overflow: hidden;
  outline: none;
}
.speedup-btn::after {
  content: '';
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.1);
  opacity: 0;
  pointer-events: none;
  border-radius: inherit;
  transition: opacity 0.1s;
}
.speedup-btn:active::after,
.speedup-btn[data-state="pressed"]::after { opacity: 1; }
.speedup-btn[data-state="disabled"] { opacity: 0.4; cursor: not-allowed; }
.speedup-btn[data-state="disabled"]::after { display: none; }
.speedup-btn svg { width: 12px; height: 18px; flex-shrink: 0; }
```

### VideoControls CSS

```css
.video-ctrl {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 38px;
  height: 28px;
  border-radius: 8px;
  border: none;
  cursor: pointer;
  background: rgba(0,0,0,0.8);
  color: white;
  padding: 1px 6px;
  outline: none;
  overflow: hidden;
}
.video-ctrl::after {
  content: '';
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.1);
  opacity: 0;
  pointer-events: none;
  border-radius: inherit;
  transition: opacity 0.1s;
}
.video-ctrl:active::after { opacity: 1; }
.video-ctrl svg { width: 26px; height: 26px; }
```

### PlayButton CSS

```css
.play-btn-lg {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border: none;
  cursor: pointer;
  background: transparent;
  padding: 0;
  outline: none;
  filter: drop-shadow(0px 3px 12px rgba(0,0,0,0.1));
}
.play-btn-lg svg { width: 40px; height: 40px; }
.play-btn-lg-wrap {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 64px;
  height: 64px;
  background: rgba(0,0,0,0.6);
  border-radius: 16px;
}
```

> PlayButton 图标使用白色线性渐变填充，用于深色背景叠加。`.play-btn-lg-wrap` 是演示用的深色容器。SVG 图标见 preview.html。

### ReactButton

26×26 点赞/踩切换按钮，点击切换线框↔实心。

```html
<button class="react-btn" data-type="like" data-state="default">
  <!-- SVG 从 icons.md「反馈图标」取 likeDefault / likeSelected -->
</button>
<button class="react-btn" data-type="dislike" data-state="default">
  <!-- SVG 从 icons.md「反馈图标」取 dislikeDefault / dislikeSelected -->
</button>
```

```css
.react-btn {
  width: 26px;
  height: 26px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  color: var(--on_surface_quaternary);
  transition: transform 0.15s, color 0.15s;
}
.react-btn[data-state="selected"] { color: var(--on_surface_secondary); }
.react-btn:active { transform: scale(0.9); }
.react-btn svg { width: 26px; height: 26px; }
```

```js
class ReactButton {
  constructor(el, { defaultIcon, selectedIcon, onChange } = {}) {
    this.el = el;
    this.defaultIcon = defaultIcon;
    this.selectedIcon = selectedIcon;
    this.onChange = onChange;
    this.selected = el.dataset.state === 'selected';
    this._render();
    this.el.addEventListener('click', () => this.toggle());
  }
  toggle() {
    this.selected = !this.selected;
    this.el.dataset.state = this.selected ? 'selected' : 'default';
    this._render();
    if (this.onChange) this.onChange(this.selected);
  }
  _render() {
    this.el.innerHTML = this.selected ? this.selectedIcon : this.defaultIcon;
  }
}
```

---

### SoundButton / FullscreenButton

32×32 玻璃材质圆形切换按钮，复用通用 `.glass-btn` 样式。

```html
<!-- 声音开关 -->
<button class="glass-btn media-toggle" style="width:32px;height:32px;padding:1px 10px" data-state="off">
  <!-- SVG 从 icons.md「媒体控制图标」取 soundOff / soundOn -->
</button>

<!-- 全屏开关 -->
<button class="glass-btn media-toggle" style="width:32px;height:32px;padding:1px 10px" data-state="off">
  <!-- SVG 从 icons.md「媒体控制图标」取 fullscreenOn / fullscreenOff -->
</button>
```

```css
.media-toggle {
  color: #fff;
  border: none;
  outline: none;
  font-family: inherit;
}
.media-toggle svg { width: 20px; height: 20px; }
```

```js
class MediaToggle {
  constructor(el, { onIcon, offIcon, onChange } = {}) {
    this.el = el;
    this.onIcon = onIcon;
    this.offIcon = offIcon;
    this.onChange = onChange;
    this.state = el.dataset.state === 'on';
    this._render();
    this.el.addEventListener('click', () => this.toggle());
  }
  toggle() {
    this.state = !this.state;
    this.el.dataset.state = this.state ? 'on' : 'off';
    this._render();
    if (this.onChange) this.onChange(this.state);
  }
  _render() {
    this.el.innerHTML = this.state ? this.onIcon : this.offIcon;
  }
}

// 调用示例（SVG 从 icons.md 取）
const soundBtn = new MediaToggle(document.querySelector('.sound-toggle'), {
  onIcon: MEDIA_ICONS.soundOn,
  offIcon: MEDIA_ICONS.soundOff,
  onChange: (on) => console.log('sound', on)
});
```

---

### ReservationButton CSS

```css
.reservation-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 54px;
  height: 28px;
  border-radius: 999px;
  border: none;
  cursor: pointer;
  padding: 0 8px;
  overflow: hidden;
  outline: none;
}
.reservation-btn::after {
  content: '';
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.1);
  opacity: 0;
  pointer-events: none;
  border-radius: inherit;
  transition: opacity 0.1s;
}
.reservation-btn:active::after,
.reservation-btn[data-state="pressed"]::after { opacity: 1; }
.reservation-btn--not-reserved { background: rgba(var(--ib-primary-rgb), 0.1); color: var(--ib-primary); }
.reservation-btn--not-reserved[data-state="pressed"] { background: rgba(var(--ib-primary-rgb), 0.16); }
.reservation-btn--reserved { background: var(--ib-primary); color: #000; }
.reservation-btn svg { width: 20px; height: 20px; }
```

---

## InstallButton

### CSS

```css
/* 变量已在 BaseButton 的 :root 中统一定义，此处直接复用 --btn-* 和 --ib-* */

.install-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: none;
  cursor: pointer;
  overflow: hidden;
  user-select: none;
  -webkit-tap-highlight-color: transparent;
  font-family: -apple-system, "MiSans", "Helvetica Neue", sans-serif;
  flex-shrink: 0;
  outline: none;
  box-sizing: border-box;
  vertical-align: top;
}

.install-btn::after {
  content: '';
  position: absolute;
  inset: 0;
  background: var(--btn-surface-touch);
  z-index: 10;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.1s;
  border-radius: inherit;
}
.install-btn:active::after,
.install-btn[data-state="pressed"]::after { opacity: 1; }

.install-btn--large { width: 100%; height: 50px; border-radius: 16px; padding: 8px 20px; gap: 4px; font-size: 17px; font-weight: 380; }
.install-btn--medium { width: 100%; height: 44px; border-radius: 16px; padding: 8px 20px; gap: 4px; font-size: 16px; font-weight: 380; }
.install-btn--small { width: 54px; height: 28px; border-radius: 999px; padding: 0 8px; font-size: 14px; font-weight: 500; }
.install-btn--icon { height: 28px; width: 54px; border-radius: 999px; padding: 0 8px; font-size: 14px; font-weight: 500; }

.install-btn__label { position: relative; z-index: 2; white-space: nowrap; line-height: 1; }
.install-btn__icon { position: relative; z-index: 2; width: 20px; height: 20px; display: flex; align-items: center; justify-content: center; }
.install-btn__icon svg { width: 20px; height: 20px; }
.install-btn__progress-bar { position: absolute; left: 0; top: 0; height: 100%; width: 0%; z-index: 1; background: var(--ib-primary); transition: width 0.15s linear; }
.install-btn__dl-clip { position: absolute; left: 0; top: 0; height: 100%; width: 0%; overflow: hidden; z-index: 3; transition: width 0.15s linear; }
.install-btn__dl-top { position: absolute; top: 0; left: 0; height: 100%; display: flex; align-items: center; justify-content: center; white-space: nowrap; line-height: 1; color: var(--btn-on-primary); }
.install-btn__stripes { position: absolute; inset: 0; display: flex; z-index: 1; overflow: hidden; border-radius: inherit; }
.install-btn__stripe { flex: 1; height: 100%; transition: background-color 0.15s; }
@keyframes ib-spin { to { transform: rotate(360deg); } }
.install-btn__icon--spinning svg { animation: ib-spin 0.8s linear infinite; }

/* 实色主色底状态 → on_primary 文字 */
.install-btn--large[data-state="enabled"],
.install-btn--medium[data-state="enabled"],
.install-btn--large[data-state="pressed"],
.install-btn--medium[data-state="pressed"] { background: var(--ib-primary); color: var(--btn-on-primary); }
.install-btn[data-state="installing"] { background: var(--ib-primary); color: var(--btn-on-primary); }
.install-btn[data-state="open"] { background: var(--ib-primary); color: var(--btn-on-primary); }
.install-btn--large[data-state="update"],
.install-btn--medium[data-state="update"] { background: var(--ib-primary); color: var(--btn-on-primary); }

/* 浅主色底状态 → 主色文字 */
.install-btn--small[data-state="enabled"] { background: rgba(var(--ib-primary-rgb), 0.1); color: var(--ib-primary); }
.install-btn--small[data-state="pressed"] { background: rgba(var(--ib-primary-rgb), 0.16); color: var(--ib-primary); }
.install-btn--icon[data-state="enabled"] { background: rgba(var(--ib-primary-rgb), 0.1); color: var(--ib-primary); }
.install-btn--icon[data-state="pressed"] { background: rgba(var(--ib-primary-rgb), 0.16); color: var(--ib-primary); }
.install-btn--small[data-state="connecting"],
.install-btn--icon[data-state="connecting"] { background: rgba(var(--ib-primary-rgb), 0.1); color: var(--ib-primary); }
.install-btn--small[data-state="downloading"],
.install-btn--icon[data-state="downloading"] { background: rgba(var(--ib-primary-rgb), 0.1); color: var(--ib-primary); }
.install-btn--small[data-state="resume"],
.install-btn--icon[data-state="resume"] { background: rgba(var(--ib-primary-rgb), 0.1); color: var(--ib-primary); }
.install-btn--small[data-state="update"],
.install-btn--icon[data-state="update"] { background: rgba(var(--ib-primary-rgb), 0.1); color: var(--ib-primary); }

/* 灰底状态 → secondary bg + 主色文字 */
.install-btn--large[data-state="connecting"],
.install-btn--medium[data-state="connecting"] { background: var(--btn-secondary-bg); color: var(--ib-primary); }
.install-btn--large[data-state="downloading"],
.install-btn--medium[data-state="downloading"] { background: var(--btn-secondary-bg); color: var(--ib-primary); }
.install-btn--large[data-state="resume"],
.install-btn--medium[data-state="resume"] { background: var(--btn-secondary-bg); color: var(--ib-primary); }

/* Disabled → Feature Token 禁用色 */
.install-btn--large[data-state="disabled"],
.install-btn--medium[data-state="disabled"] { background: var(--btn-secondary-bg); color: var(--btn-disabled-text); cursor: not-allowed; }
.install-btn--small[data-state="disabled"] { background: var(--btn-disabled-bg); color: var(--btn-disabled-text); cursor: not-allowed; }
.install-btn--icon[data-state="disabled"] { background: var(--btn-disabled-bg); color: var(--btn-disabled-text); cursor: not-allowed; }
.install-btn[data-state="disabled"]::after { display: none; }
```

### JS（含 SVG 图标）

```js
const IB_ICONS = {
  download: `<svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M16.334 15.834C16.5148 15.834 16.6057 15.8338 16.6777 15.8613C16.7879 15.9036 16.8756 15.9905 16.918 16.1006C16.9456 16.1726 16.9453 16.2635 16.9453 16.4443C16.9453 16.6251 16.9455 16.7161 16.918 16.7881C16.8757 16.8983 16.788 16.986 16.6777 17.0283C16.6057 17.0559 16.5148 17.0557 16.334 17.0557H3.66797C3.4871 17.0557 3.39627 17.0559 3.32422 17.0283C3.21397 16.986 3.1263 16.8983 3.08398 16.7881C3.0565 16.7161 3.05664 16.625 3.05664 16.4443C3.05664 16.2635 3.05633 16.1726 3.08398 16.1006C3.12634 15.9905 3.21407 15.9036 3.32422 15.8613C3.39627 15.8338 3.48707 15.834 3.66797 15.834H16.334ZM10.001 2.22266C10.1711 2.22266 10.2567 2.22245 10.3252 2.24609C10.4506 2.28948 10.5494 2.38825 10.5928 2.51367C10.6164 2.5822 10.6162 2.66768 10.6162 2.83789V12.3652L13.8916 9.08984C14.0129 8.96852 14.0738 8.90778 14.1396 8.87598C14.2578 8.8191 14.3956 8.8191 14.5137 8.87598C14.5796 8.90775 14.6403 8.9684 14.7617 9.08984C14.8831 9.21127 14.9438 9.27202 14.9756 9.33789C15.0325 9.456 15.0325 9.5938 14.9756 9.71191C14.9438 9.77773 14.8831 9.83863 14.7617 9.95996L10.4961 14.2256C10.4894 14.2323 10.4627 14.2593 10.4346 14.2832C10.3986 14.3137 10.3221 14.374 10.208 14.4111C10.1408 14.433 10.0707 14.4433 10.001 14.4434C9.93119 14.4434 9.86114 14.433 9.79395 14.4111C9.67989 14.3741 9.60344 14.3138 9.56738 14.2832C9.53921 14.2593 9.51252 14.2323 9.50586 14.2256L9.50488 14.2246L5.23926 9.95996C5.11791 9.83861 5.05714 9.77779 5.02539 9.71191C4.96852 9.59383 4.96855 9.45598 5.02539 9.33789C5.05712 9.272 5.11793 9.21117 5.23926 9.08984C5.36079 8.96831 5.42234 8.90773 5.48828 8.87598C5.6063 8.8192 5.74331 8.81921 5.86133 8.87598C5.92727 8.90773 5.98882 8.96831 6.11035 9.08984L9.38574 12.3652V2.83789C9.38574 2.66768 9.38553 2.5822 9.40918 2.51367C9.4526 2.38821 9.55124 2.28941 9.67676 2.24609C9.74529 2.22247 9.83075 2.22266 10.001 2.22266Z" fill="currentColor"/></svg>`,
  loading: `<svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" clip-rule="evenodd" d="M9.99822 2.22266C8.31833 2.22276 6.68298 2.76683 5.33806 3.77344C3.9934 4.78 3.0109 6.19513 2.53728 7.80664C2.06373 9.41837 2.1246 11.1407 2.71111 12.7148C3.29772 14.2889 4.37847 15.6312 5.79119 16.54C7.20416 17.4488 8.87422 17.8759 10.55 17.7568C12.2257 17.6377 13.8178 16.9784 15.0881 15.8789C16.3582 14.7794 17.2387 13.2985 17.5969 11.6572C17.9549 10.0159 17.7712 8.30209 17.0744 6.77344C16.9343 6.46697 16.5726 6.33121 16.2658 6.4707C15.9589 6.61065 15.8234 6.97329 15.9631 7.28027C16.5503 8.56865 16.7043 10.0131 16.4025 11.3965C16.1007 12.7799 15.3588 14.0284 14.2883 14.9551C13.2176 15.8818 11.8755 16.4367 10.4631 16.5371C9.05071 16.6374 7.64319 16.2776 6.45232 15.5117C5.26176 14.7458 4.351 13.6146 3.85662 12.2881C3.36227 10.9613 3.311 9.50981 3.71013 8.15137C4.10931 6.79313 4.93716 5.60033 6.07049 4.75195C7.20401 3.90356 8.58237 3.44444 9.99822 3.44434C10.3356 3.44434 10.6093 3.17127 10.6095 2.83398C10.6094 2.4966 10.3356 2.22266 9.99822 2.22266Z" fill="currentColor"/></svg>`,
  open: `<svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M8.83203 4.16504C9.01244 4.16504 9.10289 4.16486 9.1748 4.19238C9.28521 4.2347 9.37272 4.32221 9.41504 4.43262C9.44262 4.50462 9.44238 4.59554 9.44238 4.77637C9.44238 4.9568 9.44254 5.04722 9.41504 5.11914C9.37272 5.22954 9.28521 5.31706 9.1748 5.35938C9.10281 5.38694 9.01184 5.38672 8.83105 5.38672H6.05566C5.62806 5.38672 5.41434 5.3875 5.25098 5.4707C5.1074 5.54386 4.99024 5.6602 4.91699 5.80371C4.83374 5.9671 4.83398 6.18162 4.83398 6.60938V13.9424C4.83398 14.3701 4.83377 14.5847 4.91699 14.748C4.99023 14.8917 5.10734 15.0088 5.25098 15.082C5.41431 15.1651 5.62829 15.165 6.05566 15.165H13.3896C13.817 15.165 14.031 15.1652 14.1943 15.082C14.338 15.0088 14.4551 14.8917 14.5283 14.748C14.6115 14.5847 14.6113 14.3701 14.6113 13.9424V11.1641C14.6113 10.9831 14.6111 10.8924 14.6387 10.8203C14.681 10.7102 14.7688 10.6224 14.8789 10.5801C14.951 10.5525 15.0418 10.5527 15.2227 10.5527C15.4036 10.5527 15.4943 10.5525 15.5664 10.5801C15.6765 10.6224 15.7643 10.7102 15.8066 10.8203C15.8342 10.8924 15.834 10.9831 15.834 11.1641V13.2764C15.834 14.3647 15.8337 14.9094 15.6221 15.3252C15.4357 15.6911 15.1373 15.9894 14.7715 16.1758C14.3556 16.3875 13.8111 16.3877 12.7227 16.3877H6.72266C5.63371 16.3877 5.08878 16.3877 4.67285 16.1758C4.307 15.9894 4.00966 15.6911 3.82324 15.3252C3.61152 14.9093 3.61133 14.3649 3.61133 13.2764V7.27637C3.61133 6.18744 3.61132 5.64249 3.82324 5.22656C4.00965 4.86082 4.3071 4.56334 4.67285 4.37695C5.08879 4.16504 5.6337 4.16504 6.72266 4.16504H8.83203ZM16.8877 2.49902C17.2252 2.49902 17.499 2.77284 17.499 3.11035V7.44336C17.499 7.62442 17.4993 7.71502 17.4717 7.78711C17.4294 7.89725 17.3426 7.98503 17.2324 8.02734C17.1604 8.05486 17.0693 8.05469 16.8887 8.05469C16.7082 8.05469 16.6169 8.05484 16.5449 8.02734C16.4349 7.98497 16.347 7.89723 16.3047 7.78711C16.2771 7.71507 16.2773 7.62419 16.2773 7.44336V4.58203L9.33594 11.5234C9.17317 11.6858 8.90965 11.686 8.74707 11.5234L8.47266 11.249C8.31022 11.0864 8.31022 10.8218 8.47266 10.6592L15.4111 3.72168H12.5547C12.3739 3.72168 12.283 3.72101 12.2109 3.69336C12.1009 3.65099 12.013 3.56421 11.9707 3.4541C11.9431 3.38206 11.9434 3.2912 11.9434 3.11035C11.9434 2.92966 11.9432 2.83862 11.9707 2.7666C12.013 2.65648 12.1009 2.56874 12.2109 2.52637C12.2829 2.49879 12.374 2.49902 12.5547 2.49902H16.8877Z" fill="currentColor"/></svg>`,
  update: `<svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" clip-rule="evenodd" d="M10.6113 2.24633C14.6213 2.55797 17.7783 5.91131 17.7783 10.0012C17.7781 14.2966 14.2954 17.7786 10 17.7786C5.70478 17.7783 2.22287 14.2964 2.22266 10.0012C2.22266 7.25736 3.64398 4.84572 5.79004 3.46118L3.11133 3.46118C2.93024 3.46118 2.83967 3.4615 2.76758 3.43383C2.65748 3.39157 2.5707 3.30461 2.52832 3.19457C2.50066 3.12252 2.5 3.03172 2.5 2.85082C2.5 2.66982 2.5007 2.57915 2.52832 2.50707C2.57064 2.39683 2.65733 2.30916 2.76758 2.26684C2.83965 2.23922 2.93033 2.2395 3.11133 2.2395L7.44434 2.2395C7.46534 2.2395 7.48638 2.24036 7.50684 2.24243C7.81499 2.27372 8.05566 2.53441 8.05566 2.85082V7.18383C8.05566 7.36486 8.05595 7.4555 8.02832 7.52758C7.986 7.63783 7.89833 7.72452 7.78809 7.76684C7.71601 7.79447 7.62536 7.79516 7.44434 7.79516C7.2634 7.79516 7.17265 7.7945 7.10059 7.76684C6.99053 7.72447 6.90361 7.63768 6.86133 7.52758C6.83365 7.45549 6.83301 7.36494 6.83301 7.18383L6.83301 4.24633C6.78157 4.28325 6.71031 4.32813 6.6084 4.38989C4.71236 5.53879 3.44531 7.62239 3.44531 10.0012C3.44553 13.6214 6.3798 16.5557 10 16.5559C13.6204 16.5559 16.5554 13.6216 16.5557 10.0012C16.5557 6.58666 13.9455 3.78098 10.6113 3.47289C10.4083 3.45414 10.306 3.44504 10.2402 3.4143C10.1336 3.36434 10.0653 3.28884 10.0254 3.17797C10.0008 3.10966 10 3.01779 10 2.83422C10 2.65709 10.0008 2.56859 10.0312 2.4934C10.0755 2.38397 10.1785 2.28791 10.291 2.25219C10.3683 2.22768 10.4495 2.23375 10.6113 2.24633Z" fill="currentColor"/></svg>`
};

const IB_STATE_CONFIG = {
  enabled:     { largeText: 'Install',       smallText: 'Install', icon: 'download' },
  pressed:     { largeText: 'Install',       smallText: 'Install', icon: 'download' },
  connecting:  { largeText: 'Connecting...', smallText: null,      icon: 'loading' },
  downloading: { largeText: null,            smallText: null,      icon: 'download' },
  resume:      { largeText: 'Resume',        smallText: null,      icon: 'download' },
  installing:  { largeText: 'Installing...', smallText: null,      icon: 'loading' },
  open:        { largeText: 'Open',          smallText: 'Open',    icon: 'open' },
  update:      { largeText: 'Update',        smallText: 'Update',  icon: 'update' },
  disabled:    { largeText: 'Install',       smallText: 'Install', icon: 'download' }
};

class InstallButton {
  constructor(container, { size='large', state='enabled', progress=50 }={}) {
    this.container = typeof container === 'string' ? document.querySelector(container) : container;
    this.size=size; this.state=state; this.progress=progress; this.onClick=null;
    this._build();
  }
  _build() {
    const btn = document.createElement('button');
    btn.className = `install-btn install-btn--${this.size}`;
    btn.dataset.state = this.state;
    this.el = btn;
    this._updateContent();
    btn.addEventListener('mousedown', () => { if(this.state==='disabled')return; btn.classList.add('install-btn--active'); });
    btn.addEventListener('mouseup', () => btn.classList.remove('install-btn--active'));
    btn.addEventListener('mouseleave', () => btn.classList.remove('install-btn--active'));
    btn.addEventListener('click', () => { if(this.state!=='disabled'&&this.onClick) this.onClick(this); });
    this.container.innerHTML = '';
    this.container.appendChild(btn);
  }
  _updateContent() {
    const btn=this.el, conf=IB_STATE_CONFIG[this.state]||IB_STATE_CONFIG.enabled;
    const isLM=this.size==='large'||this.size==='medium', isSmall=this.size==='small', isIcon=this.size==='icon';
    btn.innerHTML=''; btn.dataset.state=this.state;
    if(this.state==='downloading'||this.state==='resume'){this._renderDL(btn,conf,isLM);return;}
    if(isIcon||(isSmall&&conf.smallText===null)){
      const w=document.createElement('span');w.className='install-btn__icon';
      if(this.state==='connecting'||this.state==='installing')w.classList.add('install-btn__icon--spinning');
      w.innerHTML=IB_ICONS[conf.icon];btn.appendChild(w);return;
    }
    const l=document.createElement('span');l.className='install-btn__label';
    l.textContent=isLM?conf.largeText:(conf.smallText||conf.largeText);btn.appendChild(l);
  }
  _renderDL(btn,conf,isLM) {
    const pct=this.progress, text=this.state==='resume'?(isLM?'Resume':null):`${pct}%`;
    if(isLM){
      const bar=document.createElement('div');bar.className='install-btn__progress-bar';bar.style.width=pct+'%';btn.appendChild(bar);
      const bl=document.createElement('div');bl.className='install-btn__dl-top';bl.style.cssText='width:100%;color:var(--ib-primary);z-index:2;';bl.textContent=text;btn.appendChild(bl);
      const clip=document.createElement('div');clip.className='install-btn__dl-clip';clip.style.width=pct+'%';
      const tl=document.createElement('div');tl.className='install-btn__dl-top';tl.style.width=pct>0?(10000/pct)+'%':'100%';tl.textContent=text;clip.appendChild(tl);btn.appendChild(clip);
    } else {
      const stripes=document.createElement('div');stripes.className='install-btn__stripes';
      const filled=Math.round(pct/10);
      for(let i=0;i<10;i++){const s=document.createElement('div');s.className='install-btn__stripe';s.style.background=i<filled?'var(--ib-primary)':'rgba(var(--ib-black-rgb),0.06)';stripes.appendChild(s);}
      btn.appendChild(stripes);
      const fp=filled*10, ip=fp>0?(10000/fp)+'%':'100%';
      if(this.state==='downloading'){
        const bl=document.createElement('div');bl.className='install-btn__dl-top';bl.style.cssText='width:100%;color:var(--ib-primary);z-index:2;';bl.textContent=`${pct}%`;btn.appendChild(bl);
        const clip=document.createElement('div');clip.className='install-btn__dl-clip';clip.style.width=fp+'%';
        const tl=document.createElement('div');tl.className='install-btn__dl-top';tl.style.width=ip;tl.textContent=`${pct}%`;clip.appendChild(tl);btn.appendChild(clip);
      } else {
        const bi=document.createElement('div');bi.className='install-btn__dl-top';bi.style.cssText='width:100%;color:var(--ib-primary);z-index:2;';bi.innerHTML=IB_ICONS.download;btn.appendChild(bi);
        const clip=document.createElement('div');clip.className='install-btn__dl-clip';clip.style.width=fp+'%';
        const ti=document.createElement('div');ti.className='install-btn__dl-top';ti.style.width=ip;ti.style.color='var(--ib-white)';ti.innerHTML=IB_ICONS.download;clip.appendChild(ti);btn.appendChild(clip);
      }
    }
  }
  setState(s,p){this.state=s;if(p!==undefined)this.progress=p;this._updateContent();}
  setProgress(p){this.progress=p;if(this.state==='downloading'||this.state==='resume')this._updateContent();}
  setSize(sz){this.size=sz;this.el.className=`install-btn install-btn--${sz}`;this._updateContent();}
}
```
