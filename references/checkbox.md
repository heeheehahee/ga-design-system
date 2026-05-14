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

## 类型选择

| 类型 | 适用场景 |
|------|---------|
| Default | 普通页面背景上（列表多选、设置勾选） |
| Media | 图片/视频内容上方叠加（相册选择、视频选取） |

---

## 状态

| 状态 | 行为 |
|------|------|
| Unselected | 未选中，显示圆环 |
| Selected | 已选中，实心圆 + 白色勾 |
| Unselected_Disabled | 未选中 + 不可操作 |
| Selected_Disabled | 已选中 + 不可操作 |

---

## 交互

| 触发 | 行为 |
|------|------|
| tap | Unselected ↔ Selected 切换 |
| disabled | 不响应点击 |

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

> 所有精确样式值（尺寸、颜色 Token、描边）见 [checkbox-code.md](checkbox-code.md)
