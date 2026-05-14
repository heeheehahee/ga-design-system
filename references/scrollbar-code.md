---
category: components-code
name: Scrollbar 自定义滚动条
description: 自定义滚动条样式，Default 4px / Activated 6px，支持明暗模式。加 class 即用。
source: Figma ga_component (node 416:7500)
---

# Scrollbar 自定义滚动条

来源：Figma ga_component node 416:7500

---

## 规格

| 属性 | Default | Activated |
|------|---------|-----------|
| 触摸区域宽度 | 12px | 12px |
| 可见轨道宽度 | 4px | 6px |
| 圆角 | 999px | 999px |
| 颜色（亮色） | rgba(0,0,0,0.1) | rgba(0,0,0,0.3) |
| 颜色（暗色） | rgba(255,255,255,0.2) | rgba(255,255,255,0.3) |
| 轨道背景 | 透明 | 透明 |

状态切换：滚动中触摸/拖拽时 → Activated，松手后回到 Default。

---

## CSS

```css
/* Scrollbar 自定义滚动条 — 加到可滚动容器上 */
.custom-scrollbar {
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: thin;
  scrollbar-color: rgba(0,0,0,0.1) transparent;
}
.custom-scrollbar:active,
.custom-scrollbar:hover {
  scrollbar-color: rgba(0,0,0,0.3) transparent;
}
.custom-scrollbar::-webkit-scrollbar { width: 12px; }
.custom-scrollbar::-webkit-scrollbar-thumb {
  background: rgba(0,0,0,0.1);
  border-radius: 999px;
  border: 4px solid transparent;
  background-clip: content-box;
}
.custom-scrollbar:active::-webkit-scrollbar-thumb,
.custom-scrollbar::-webkit-scrollbar-thumb:active {
  background: rgba(0,0,0,0.3);
  border: 3px solid transparent;
  background-clip: content-box;
}
.custom-scrollbar::-webkit-scrollbar-track { background: transparent; }

/* 暗色模式 */
@media (prefers-color-scheme: dark) {
  .custom-scrollbar { scrollbar-color: rgba(255,255,255,0.2) transparent; }
  .custom-scrollbar:active,
  .custom-scrollbar:hover { scrollbar-color: rgba(255,255,255,0.3) transparent; }
  .custom-scrollbar::-webkit-scrollbar-thumb {
    background: rgba(255,255,255,0.2);
    border: 4px solid transparent;
    background-clip: content-box;
  }
  .custom-scrollbar:active::-webkit-scrollbar-thumb,
  .custom-scrollbar::-webkit-scrollbar-thumb:active {
    background: rgba(255,255,255,0.3);
    border: 3px solid transparent;
    background-clip: content-box;
  }
}
```

---

## 使用方式

在任何需要自定义滚动条的容器上加 `.custom-scrollbar` class：

```html
<div class="custom-scrollbar" style="max-height: 400px;">
  <!-- 长内容 -->
</div>
```

已使用此组件的位置：
- Dialog 弹窗内容超高时（`.dialog__body--scrollable`）
- Drawer 内容区（`.drawer__content`）
