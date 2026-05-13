---
category: components
name: Switch 开关
description: 开关切换组件，支持开/关/禁用/加载 4 种状态，用于设置项的布尔值控制
source: Figma ga_component (node 197:11355)
---

# Switch 开关

代码：[switch-code.md](switch-code.md)

---

## 基础属性

| 属性 | 值 |
|------|------|
| 轨道尺寸 | 49×28px |
| 轨道圆角 | 14px（pill 全圆角） |
| 滑块尺寸 | 20×20px |
| 滑块圆角 | 50%（圆形） |
| 滑块内边距 | 4px（四周等距） |
| 滑块颜色 | `white` |

---

## 状态

| 状态 | 轨道 Token | 滑块位置 | 说明 |
|------|-----------|---------|------|
| Off | `outline` | 左（left: 4px） | 默认关闭 |
| Off Disabled | `surface_container` | 左（left: 4px） | 关闭且不可操作 |
| On | `primary` | 右（left: 25px） | 已开启 |
| On Disabled | `disable_primary` | 右（left: 25px） | 开启且不可操作 |
| Loading | `disable_primary` | 右（left: 25px） | 切换中加载 |

---

## 颜色 Token

| 元素 | 状态 | Token | 说明 |
|------|------|-------|------|
| 轨道 | Off | `outline` | rgba(0,0,0,0.1) |
| 轨道 | Off Disabled | `surface_container` | rgba(0,0,0,0.04) |
| 轨道 | On | `primary` | 系统主色蓝 |
| 轨道 | On Disabled | `disable_primary` | 30% 主色 |
| 轨道 | Loading | `disable_primary` | 同禁用态 |
| 滑块 | 所有状态 | `white` | 始终白色 |

---

## 交互

| 触发 | 行为 |
|------|------|
| tap | Off ↔ On 切换，滑块滑动动画 |
| disabled | 不响应点击 |
| loading | 显示旋转图标，不响应点击 |

### 动画

- 滑块位移：`transform: translateX()` 过渡 0.2s ease
- 轨道颜色：`background` 过渡 0.2s ease

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
