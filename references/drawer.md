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

## 基础属性

| 属性 | 值 |
|------|------|
| 宽度 | 392px（满屏） |
| 顶部圆角 | 36px |
| 底部圆角 | 0px |
| 背景色 | `surface_pop_window` |

---

## 高度模式（3 种）

| 模式 | class | 行为 |
|------|-------|------|
| 内容自适应（默认） | 无需额外 class | 内容撑开高度，最大 70vh，超出时内容区滚动 |
| 全屏 | `.drawer--fullscreen` | 高度 calc(100vh - 52px)，顶部留 52px 间距 |
| 自定义高度 | `.drawer--custom` + 内联 height | 调用方指定固定高度 |

---

## 结构

从上到下依次为：

### 1. 拖拽手柄区域

| 属性 | 值 |
|------|------|
| 区域高度 | 24px（含 padding 10px） |
| 手柄条尺寸 | 60×3px |
| 手柄圆角 | 1.5px |
| 手柄颜色 | `outline` |
| 手柄居中 | 水平居中 |

### 2. 标题栏

复用已有 NavigationBar（小标题类型）：

| 属性 | 值 |
|------|------|
| 高度 | auto（padding 6px 12px） |
| 左图标 | Close（关闭） |
| 标题 | 18px / weight 380 / 居中 |
| 右图标 | Checkmark（确认）（可选） |
| 标题色 | `on_surface` |
| 图标色 | `on_surface` |
| 图标板 | 44×44px |

### 3. 内容区域

| 属性 | 值 |
|------|------|
| padding | 0 20px 40px |
| overflow-y | auto |
| 底部留白 | 40px（确保内容不紧贴底边） |

由调用方填充内容，Drawer 提供滚动和底部留白。

---

## 颜色 Token

| 元素 | Token | 说明 |
|------|-------|------|
| 拖拽手柄 | `outline` | rgba(0,0,0,0.1) |
| 标题文字 | `on_surface` | 黑色 |
| 图标 | `on_surface` | 黑色 |
| 底部指示条 | `on_surface_quaternary` | rgba(0,0,0,0.4) |

---

---

## 交互

| 触发 | 行为 |
|------|------|
| 上滑出现 | 从底部滑入，伴随遮罩淡入 |
| 点击抽屉上方区域（遮罩） | 关闭抽屉 |
| 拖拽手柄向下 | 关闭抽屉（下滑超过阈值时触发） |
| 点击关闭图标 | 关闭抽屉 |

---

## 调用方式

```html
<!-- 遮罩 -->
<div class="drawer-mask"></div>

<!-- 抽屉 -->
<div class="drawer drawer--max">
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
