---
category: components
name: 多选 Checkbox
description: 圆形多选 Checkbox，支持 Default/Media 两种类型 × 4 种状态，用于列表多选和媒体内容选择
source: Figma ga_component (node 363:6010)
---

# Checkbox 多选

代码：[checkbox-code.md](checkbox-code.md)
图标：[icons.md](icons.md)（checkmark 勾选图标）

---

## 基础属性

| 属性 | 值 |
|------|------|
| 尺寸 | 22×22px |
| 形状 | 圆形（border-radius: 50%） |
| 描边粗细 | 1.6px（圆环）/ 2px（勾选线条） |
| 勾选图标尺寸 | 10×9px（viewBox），定位居中 |

---

## 变体

| 维度 | 选项 | 说明 |
|------|------|------|
| Type | Default | 用于普通页面背景（跟随主题色） |
| Type | Media | 用于图片/视频内容上方（半透明叠加） |
| State | Unselected | 未选中 |
| State | Selected | 已选中 |
| State | Unselected_Disabled | 未选中禁用 |
| State | Selected_Disabled | 已选中禁用 |

---

## 颜色 Token

### Default 类型

| 状态 | 元素 | Token | 说明 |
|------|------|-------|------|
| Unselected | 圆环描边 | `on_surface_octonary` | 30% 黑 |
| Unselected | 填充 | 无 | 透明 |
| Selected | 圆填充 | `primary` | 系统主色蓝 |
| Selected | 勾选线条 | `on_primary` | 主色上的白 |
| Unselected_Disabled | 圆环描边 | `outline` | 10% 黑 |
| Selected_Disabled | 圆填充 | `disable_primary` | 30% 主色 |
| Selected_Disabled | 勾选线条 | `on_primary` | 主色上的白 |

### Media 类型

| 状态 | 元素 | Token | 说明 |
|------|------|-------|------|
| Unselected | 圆填充 | `mask_menu` | 半透明黑底 |
| Unselected | 圆环描边 | `white` | 白色描边 |
| Selected | 圆填充 | `primary` | 系统主色蓝 |
| Selected | 圆环描边 | `white` | 白色描边 |
| Selected | 勾选线条 | `on_primary` | 主色上的白 |
| Disabled | — | opacity: 0.5 | 整体半透明 |

---

## 状态

| 状态 | Default 表现 | Media 表现 |
|------|-------------|-----------|
| Unselected | 灰色圆环 | 半透明黑底 + 白色圆环 |
| Selected | 蓝色实心圆 + 白色勾 | 蓝色实心圆 + 白色圆环 + 白色勾 |
| Unselected_Disabled | 极淡灰色圆环 | 同 Unselected，opacity 0.5 |
| Selected_Disabled | 淡蓝实心圆 + 白色勾 | 同 Selected，opacity 0.5 |

---

## 调用方式

```html
<!-- Default Unselected -->
<span class="checkbox"></span>

<!-- Default Selected -->
<span class="checkbox checkbox--selected"></span>

<!-- Default Disabled -->
<span class="checkbox checkbox--disabled"></span>
<span class="checkbox checkbox--selected checkbox--disabled"></span>

<!-- Media Unselected -->
<span class="checkbox checkbox--media"></span>

<!-- Media Selected -->
<span class="checkbox checkbox--media checkbox--selected"></span>

<!-- Media Disabled -->
<span class="checkbox checkbox--media checkbox--disabled"></span>
<span class="checkbox checkbox--media checkbox--selected checkbox--disabled"></span>
```

---

## 交互

| 触发 | 行为 |
|------|------|
| tap | Unselected ↔ Selected 切换 |
| disabled | 不响应点击 |
| :active | 无额外按下态（checkbox 本身无 press 状态） |
