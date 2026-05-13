---
category: components
name: 卡片容器
tokens_used: [color.bg.card, radius.md, spacing.md, spacing.sm, shadow.md]
description: 卡片容器组件规范
---

# 卡片容器

## 基础属性

| 属性 | Token | 值 |
|------|-------|------|
| 背景 | `{color.bg.card}` | |
| 圆角 | `{radius.md}` | |
| 内边距 | `{spacing.md}` | |
| 子元素间距 | `{spacing.sm}` | |
| 阴影 | `{shadow.md}` | |
| 溢出裁切 | — | 是 |

## 布局

- 默认垂直排列
- 宽度 = 父容器宽度 - 2 × `{spacing.lg}`（页面边距）
