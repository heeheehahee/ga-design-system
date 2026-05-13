---
category: components
name: 加载 Loading
description: 加载状态反馈组件，包含旋转图标（LoadingIcon）和加载弹层（LoadingToast），用于页面加载、下拉刷新、组件内加载等场景
source: Figma ga_component (node 319:9852)
---

# 加载 Loading

代码：[loading-code.md](loading-code.md)

---

## LoadingIcon 加载图标

旋转圆环 + 圆点指示器，可选附带文字。

### 变体矩阵

| 维度 | 选项 | 说明 |
|------|------|------|
| 尺寸 | Default / Small | Default 24×24，Small 20×20 |
| 类型 | 无文案 / 有文案 | 有文案时下方显示"加载中"等文字 |
| 状态 | 默认 / 禁用 | 禁用态颜色变淡 |

共 2×2×2 = **8 个变体**。

### Spinner 图形

| 属性 | Default | Small |
|------|---------|-------|
| 尺寸 | 24×24 | 20×20 |
| 图形 | 圆环轨道 + 圆形指示器 | 同左（等比缩小） |
| 轨道 | 描边环，宽 ~2.3px | 描边环，宽 2px |
| 指示器 | 实心圆，半径 ~2.3px | 实心圆，半径 2px |
| 动画 | CSS `rotate` 360° 循环 | 同左 |

> Spinner SVG 从 Figma 导出（node 319:10731, 319:10733），path 包含环形轨道和圆形指示器。

### 颜色

| 状态 | Figma 值 | Token |
|------|---------|-------|
| 默认 | `fill-opacity="0.8"` = `rgba(0,0,0,0.8)` | `on_surface_secondary`（L:`#000000cc` D:`#ffffffcc`） |
| 禁用 | `fill-opacity="0.3"` = `rgba(0,0,0,0.3)` | `on_surface_octonary`（L:`#0000004c` D:`#ffffff4d`） |

> Figma 默认态 `rgba(0,0,0,0.8)` 与 `on_surface_secondary` Light（`#000000cc` = 0.8）精确匹配。

### 文字（有文案类型）

| 属性 | Default | Small |
|------|---------|-------|
| 字体 | `font.headline2`（Headline 2） | `font.subtitle`（Subtitle） |
| 字号 | 16px | 14px |
| 字重 | Medium（380） | Medium（380） |
| 默认色 | `rgba(0,0,0,0.8)` = `on_surface_secondary` | 同左 |
| 禁用色 | `rgba(0,0,0,0.3)` = `on_surface_octonary` | 同左 |

### 布局（有文案类型）

| 属性 | 值 |
|------|------|
| 方向 | column（垂直） |
| 对齐 | center |
| 间距 | 8px（spinner 与文字之间） |

---

## LoadingToast 加载弹层

页面居中的加载蒙层，包含 spinner + 可选文字，覆盖在内容之上。

### 变体

| 维度 | 选项 |
|------|------|
| 模式 | Light / Dark |
| 材质 | Default |

### 属性

| 属性 | 值 | Token / 说明 |
|------|------|-------------|
| 背景 | Light `#FFFFFF` / Dark `#242424` | `surface` / `surface_pop_window` |
| 圆角 | 24px | `radius.demi-big` |
| 内边距 | 20px | `spacing.5x` |
| 布局 | column, center |
| 阴影 | `0px 36px 80px rgba(0,0,0,0.08), 0px 0px 60px rgba(0,0,0,0.06)` | `Shadow_High` |
| 层级 | `z.modal`（覆盖页面内容） | — |

### 结构

```
LoadingToast-Container  (column, center, padding 20px, radius 24px)
└── LoadingIcon          (Default + 有文案)
```

遮罩层可选（`mask` Token：L:`#00000033` D:`#00000099`）。

---

## 应用场景

| 场景 | 组件 | 说明 |
|------|------|------|
| 全页面加载 | LoadingIcon（Default + 无文案） | 页面内容区居中 |
| 下拉刷新 | LoadingIcon（Small + 无文案） | 列表顶部 |
| 组件内加载 | LoadingIcon（Small + 无文案） | 按钮/卡片内部 |
| 页面二次加载 | LoadingToast | 居中弹层 + 遮罩 |

## 调用方式

```javascript
// 页面加载 - 纯 spinner
new LoadingIcon(container, { size: 'default' });

// 带文字
new LoadingIcon(container, { size: 'default', text: '加载中' });

// 加载弹层
LoadingToast.show('加载中');
LoadingToast.hide();
```
