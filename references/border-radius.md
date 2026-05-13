---
category: foundations
tokens: [radius.tiny, radius.small, radius.common, radius.medium, radius.demi-big, radius.big, radius.circle, radius.dynamic]
description: 圆角梯度规范 — 以 4 为模度递增的 8 级圆角体系
source: Figma ga_component / 圆角 Radius (node 120:3613)
---

# 圆角梯度规范

> 圆角以 4 为模度递增，不可使用规范外参数，8dp 以下圆角可使用灵活偶数值。

| Token | 名称 | 值 | 倍率 | 应用场景 |
|-------|------|------|------|----------|
| `radius.tiny` | Tiny | 8 | 2X | 小图标、标签 |
| `radius.small` | Small | 12 | 3X | 分段控制按钮、子页签、气泡 |
| `radius.common` | Common | 16 | 4X | 大按钮、输入框、行动操作按钮、信息提示 |
| `radius.medium` | Medium | 20 | 5X | 列表、宫格、侧边栏、小窗、mini窗、游戏工具箱 |
| `radius.demi-big` | Demi_big | 24 | 6X | 菜单、通知中心、控制中心、超大卡片、小部件、文件夹 |
| `radius.big` | Big | 36 | 9X | 弹窗、抽屉、浮窗 |
| `radius.circle` | Circle | 999 | XX | 搜索框、小按钮（胶囊按钮）、进度条、滑动选择器、页面指示器 |
| `radius.dynamic` | Dynamic | 动态计算 | -- | 配对连接弹窗（机身圆角大于32的机型） |
