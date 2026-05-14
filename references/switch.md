---
category: components
name: Switch 开关
description: 开关切换组件，支持开/关/禁用/加载 4 种状态，用于设置项的布尔值控制
source: Figma ga_component (node 197:11355)
---

# Switch 开关

代码：[switch-code.md](switch-code.md)

---

## 何时使用

用于设置项的布尔值控制（开/关）。通常配合 List 组件使用，放在列表行的右侧功能区。

---

## 状态

| 状态 | 说明 |
|------|------|
| Off | 默认关闭 |
| On | 已开启 |
| Off Disabled | 关闭且不可操作 |
| On Disabled | 开启且不可操作 |
| Loading | 切换中加载（显示旋转图标） |

---

## 交互

| 触发 | 行为 |
|------|------|
| tap | Off ↔ On 切换，滑块滑动动画 |
| disabled | 不响应点击 |
| loading | 显示旋转图标，不响应点击 |

---

## 调用方式

```html
<!-- Off -->
<div class="switch"></div>

<!-- On -->
<div class="switch switch--on"></div>

<!-- Off Disabled -->
<div class="switch switch--disabled"></div>

<!-- On Disabled -->
<div class="switch switch--on switch--disabled"></div>

<!-- Loading -->
<div class="switch switch--on switch--loading"></div>
```

> 所有精确样式值（尺寸、颜色 Token、动画）见 [switch-code.md](switch-code.md)
