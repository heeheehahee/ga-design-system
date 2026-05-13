# BottomActionBar 底部操作栏

固定在页面底部的操作栏，带渐变遮罩让内容自然过渡。适用于详情页、商品页、编辑页等需要底部按钮的场景。

---

## CSS

```css
/* ====== BottomActionBar 底部操作栏 ====== */

.bottom-action-bar {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 20;
  pointer-events: none;
}

.bottom-action-bar__overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100px;
  background: linear-gradient(180deg,
    rgba(var(--surface_high-rgb), 0) 0%,
    rgba(var(--surface_high-rgb), 0.85) 60%,
    rgba(var(--surface_high-rgb), 1) 100%);
}

.bottom-action-bar__content {
  position: absolute;
  bottom: 24px;
  left: 20px;
  right: 20px;
  display: flex;
  gap: 10px;
  height: 50px;
  pointer-events: auto;
}

/* 左侧辅助按钮（可选） */
.bottom-action-bar__side-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px 20px;
  border-radius: 16px;
  background: var(--surface_high);
  border: 0.74px solid var(--divider);
  cursor: pointer;
  flex-shrink: 0;
}
.bottom-action-bar__side-btn:active { opacity: 0.7; }
.bottom-action-bar__side-btn svg {
  width: 28px;
  height: 28px;
}

/* 主按钮容器（InstallButton 等） */
.bottom-action-bar__main {
  flex: 1;
  position: relative;
  background: var(--surface_high);
  border-radius: 16px;
}
```

## HTML

```html
<!-- 基础用法：仅主按钮 -->
<div class="bottom-action-bar">
  <div class="bottom-action-bar__overlay"></div>
  <div class="bottom-action-bar__content">
    <div class="bottom-action-bar__main" id="main-btn"></div>
  </div>
</div>

<!-- 带左侧辅助按钮 -->
<div class="bottom-action-bar">
  <div class="bottom-action-bar__overlay"></div>
  <div class="bottom-action-bar__content">
    <button class="bottom-action-bar__side-btn" id="side-btn">
      <!-- SVG 图标 -->
    </button>
    <div class="bottom-action-bar__main" id="main-btn"></div>
  </div>
</div>
```

## 调用示例

```javascript
// 主按钮用 InstallButton
const mainBtn = new InstallButton('#main-btn', { size: 'large', state: 'enabled' });

// 左侧辅助按钮可选（如活动入口）
document.getElementById('side-btn').innerHTML = ACTIVITY_ICON_SVG;
```

## 设计参数

| 元素 | 属性 |
|------|------|
| 渐变遮罩 | 100dp 高, `surface_high-rgb` 从 0→0.85→1 |
| 按钮区 | bottom 24dp, left/right 20dp, gap 10dp, height 50dp |
| 辅助按钮 | padding 8dp 20dp, radius 16dp, bg `var(--surface_high)`, border 0.74dp `var(--divider)` |
| 主按钮容器 | flex:1, bg `var(--surface_high)`, radius 16dp（防下载透明） |
| pointer-events | 外层 none，按钮区 auto（让滚动穿透） |

## 依赖

- 需要 `--surface_high-rgb` CSS 变量（RGB 辅助变量，用于渐变）
- 主按钮通常搭配 `InstallButton`（button-code.md）

## 注意事项

- 渐变遮罩颜色需和页面背景/抽屉背景一致
- 主按钮容器加了 `background: var(--surface_high)` 防止下载进度条状态时按钮变透明
- `pointer-events: none` + 内部 `auto` 确保不阻挡页面滚动
