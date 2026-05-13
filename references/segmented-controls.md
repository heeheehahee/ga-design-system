---
category: components
name: SegmentedControls 分段按钮
description: 分段选择器，支持 2-5 个选项，平面/浮起两种状态，灰/白/彩色三种背景环境
source: Figma ga_component (node 414:19208)
---

# SegmentedControls 分段按钮

代码：[segmented-controls-code.md](segmented-controls-code.md)

---

## 基础属性

| 属性 | 值 |
|------|------|
| 外容器宽度 | 392px（满屏） |
| 外容器 padding | 0 12px |
| 外容器高度 | 40px |
| 轨道尺寸 | 368×40px |
| 轨道圆角 | 20px（pill） |
| 选项组内缩 | 4px（距轨道边缘） |
| 单选项 padding | 6px 24px |
| 单选项圆角 | 16px |
| 选项间 gap | 2px |

---

## 文本样式

| 状态 | 字号 | 字重 | 颜色 |
|------|------|------|------|
| 选中 | 14px | 380 | `on_surface` |
| 未选中 | 14px | 380 | `on_surface` |

---

## 颜色 Token

| 元素 | 灰色背景环境 | 白色背景环境 |
|------|------------|------------|
| 轨道底板 | `white` (#FFFFFF) | `surface_container` |
| 选中项背景 | `surface_container` (rgba(0,0,0,0.05)) | `white` |
| 未选中项 | 透明 | 透明 |
| 文字 | `on_surface` | `on_surface` |

---

## 变体

| 维度 | 选项 |
|------|------|
| 个数 | 2 / 3 / 4 / 5 |
| 背景环境 | 灰色 / 白色 / 彩色（玻璃材质） |
| 状态 | 平面（默认）/ 浮起（玻璃材质） |

---

## 交互

| 触发 | 行为 |
|------|------|
| tap 选项 | 切换选中态，选中项背景滑动到对应位置 |
| :active | 无额外按下态 |

---

## 调用方式

```html
<div class="seg-ctrl">
  <div class="seg-ctrl__track">
    <div class="seg-ctrl__group">
      <div class="seg-ctrl__item seg-ctrl__item--active">选中项</div>
      <div class="seg-ctrl__item">未选中</div>
    </div>
  </div>
</div>
```
