---
category: components
name: Drawer 底部抽屉
description: 底部上滑抽屉面板，支持高/中/低三档位，玻璃材质 + 拖拽手柄 + 标题栏
source: Figma ga_component (node 366:6119)
---

# Drawer 底部抽屉

代码：[drawer-code.md](drawer-code.md)
图标：[icons.md](icons.md)（close / checkmark 图标）

---

## 高度模式选择

| 模式 | 适用场景 | class |
|------|---------|-------|
| 内容自适应（默认） | 内容不多，自动撑高（最大 70vh，超出滚动） | 无需额外 class |
| 全屏 | 内容很多，需要大面积展示 | `.drawer--fullscreen` |
| 自定义高度 | 特殊需求，固定高度 | `.drawer--custom` + 内联 height |

---

## 结构

```
┌────────────────────────────────────┐
│         ─── 拖拽手柄 ───            │
│  [关闭]      标题      [确认(可选)]  │  ← 复用 navbar-icon
│  ──────────────────────────────────│
│                                    │
│            内容区域                  │  ← 调用方填充
│          （可滚动）                  │
│                                    │
└────────────────────────────────────┘
```

---

## 内容区边距规则

内容区必须有左右边距，不允许内容贴边：

| 情况 | 边距来源 |
|------|---------|
| 内容组件自带边距（如 List 有 padding） | 使用组件自身的边距 |
| 内容组件无边距 | 使用默认 20px 左右边距 |

---

## 交互

| 触发 | 行为 |
|------|------|
| 上滑出现 | 从底部滑入 + 遮罩淡入 |
| 点击遮罩 | 关闭抽屉 |
| 拖拽手柄向下 | 关闭抽屉 |
| 点击关闭图标 | 关闭抽屉 |

---

## 调用方式

```html
<!-- 遮罩 -->
<div class="drawer-mask"></div>

<!-- 抽屉 -->
<div class="drawer">
  <div class="drawer__handle"></div>
  <div class="drawer__header">
    <span class="navbar-icon"><!-- close icon --></span>
    <span class="drawer__title">标题</span>
    <span class="navbar-icon"><!-- checkmark icon --></span>
  </div>
  <div class="drawer__content">
    <!-- 内容 -->
  </div>
</div>
```

> 所有精确样式值（圆角、间距、颜色、动画）见 [drawer-code.md](drawer-code.md)
