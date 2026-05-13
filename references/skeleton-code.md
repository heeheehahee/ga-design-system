# SkeletonBone 骨架占位块

骨架屏的最小原子单元。带 shimmer 扫光动画的灰色占位块，用于拼装各种页面的骨架屏。

---

## CSS

```css
/* ====== SkeletonBone 骨架占位块 ====== */

.skeleton-bone {
  background: var(--surface_container_medium);
  position: relative;
  overflow: hidden;
  border-radius: 8px;
}

.skeleton-bone::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, transparent 0%, rgba(255,255,255,0.15) 50%, transparent 100%);
  animation: shimmer 1.5s linear infinite;
}

body.theme-light .skeleton-bone::after {
  background: linear-gradient(90deg, transparent 0%, rgba(255,255,255,0.6) 50%, transparent 100%);
}

@keyframes shimmer {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}

/* 预设形状 */
.skeleton-bone--circle { border-radius: 999px; }
.skeleton-bone--pill { border-radius: 999px; }
.skeleton-bone--card { border-radius: 20px; }
.skeleton-bone--bar { border-radius: 4px; }
.skeleton-bone--line { border-radius: 4px; height: 14px; }
```

## HTML 用法

```html
<!-- 圆形（头像/图标） -->
<div class="skeleton-bone skeleton-bone--circle" style="width:44px;height:44px"></div>

<!-- 矩形（图片/卡片） -->
<div class="skeleton-bone skeleton-bone--card" style="width:100%;aspect-ratio:352/197"></div>

<!-- 文字行 -->
<div class="skeleton-bone skeleton-bone--line" style="width:70%"></div>

<!-- 细条（进度条/分隔线） -->
<div class="skeleton-bone skeleton-bone--bar" style="width:100%;height:4px"></div>

<!-- 药丸（标签） -->
<div class="skeleton-bone skeleton-bone--pill" style="width:60px;height:32px"></div>
```

## 拼装示例

```html
<!-- 用户信息骨架 -->
<div style="display:flex;gap:12px;align-items:center">
  <div class="skeleton-bone skeleton-bone--circle" style="width:40px;height:40px"></div>
  <div style="flex:1;display:flex;flex-direction:column;gap:8px">
    <div class="skeleton-bone skeleton-bone--line" style="width:60%"></div>
    <div class="skeleton-bone skeleton-bone--line" style="width:40%"></div>
  </div>
</div>
```

## 设计参数

| 属性 | 值 |
|------|-----|
| 背景色 | `var(--surface_container_medium)` |
| Shimmer 渐变(dark) | `rgba(255,255,255,0.15)` |
| Shimmer 渐变(light) | `rgba(255,255,255,0.6)` |
| 动画 | 1.5s linear infinite, translateX(-100%→100%) |
| 默认圆角 | 8dp |

## 预设形状速查

| Class | 圆角 | 典型用途 |
|-------|------|---------|
| `--circle` | 999dp | 头像、图标 |
| `--pill` | 999dp | 标签、按钮 |
| `--card` | 20dp | 卡片、图片 |
| `--bar` | 4dp | 进度条、分隔线 |
| `--line` | 4dp, h=14dp | 文字行 |
