---
category: components
name: SegmentedControls 分段按钮
description: 分段选择器，支持 2-5 个选项，平面/浮起两种状态，灰/白/彩色三种背景环境
source: Figma ga_component (node 414:19208)
---

# SegmentedControls 分段按钮

代码：[segmented-controls-code.md](segmented-controls-code.md)

---

## 何时使用

在同一区域内切换 2-5 个互斥视图/分类时使用（如"推荐/最新/热门"切换）。

---

## 变体

| 维度 | 选项 | 说明 |
|------|------|------|
| 选项个数 | 2 / 3 / 4 / 5 | 等分宽度 |
| 背景环境 | 灰色 / 白色 / 彩色 | 根据所在容器背景选择 |
| 状态 | 平面（默认）/ 浮起（玻璃材质） | 浮起态用于彩色/图片背景上 |

### 背景环境选择

| 容器背景 | 选择 | 效果 |
|---------|------|------|
| 灰色页面 | 灰色环境 | 轨道白色底 + 选中项灰色 |
| 白色页面 | 白色环境 | 轨道灰色底 + 选中项白色 |
| 彩色/图片 | 彩色环境 | 玻璃材质 |

---

## 交互

| 触发 | 行为 |
|------|------|
| tap 选项 | 切换选中态，选中项背景滑动动画 |

---

## 调用方式

```js
new SegCtrl(container, {
  items: ['推荐', '最新', '热门'],
  active: 0,
  onColor: false  // true = 彩色背景环境
});
```

```html
<!-- 也可纯 HTML -->
<div class="seg-ctrl">
  <div class="seg-ctrl__track">
    <div class="seg-ctrl__group">
      <div class="seg-ctrl__item seg-ctrl__item--active">选中项</div>
      <div class="seg-ctrl__item">未选中</div>
    </div>
  </div>
</div>
```

> 所有精确样式值（尺寸、圆角、颜色 Token）见 [segmented-controls-code.md](segmented-controls-code.md)
