---
category: components
name: 信息提示 NoticeBar
description: 页面内信息提示条，用于系统通知、功能引导、警告提醒等场景，支持 4 种语义类型
source: Figma ga_component (node 304:9560)
---

# 信息提示 NoticeBar

代码：[noticebar-code.md](noticebar-code.md)
图标：[icons.md](icons.md)

## 变体矩阵

| 维度 | 选项 | 说明 |
|------|------|------|
| 模式 | Light / Dark | 跟随系统主题 |
| 类型 | Gray / Yellow / Red / Blue | 语义：默认 / 提醒 / 警告 / 强调 |
| 前置图标 | on / off | 可选，用于强化语义 |
| 尾部操作 | 详情（chevron →） / 关闭（×） | Gray 默认详情，Yellow/Red/Blue 默认关闭 |

共 4 类型 × 2 模式 = **8 个变体**，前置图标可切换。

## 基础属性

| 属性 | 值 | Figma 标注 |
|------|------|-----------|
| 圆角 | 20px（`radius.medium`） | Figma 标注「圆角更新为 20」 |
| 内边距 | 12px 16px | Figma 标注「padding：上下12，左右16」 |
| 内容间距 | 8px | ContentRow gap |
| 文字-关闭按钮间距 | 12px | TextRow gap（Yellow/Red/Blue 关闭型） |
| 宽度 | 填充父容器 | layout: fill |

## 颜色方案

### Gray（默认）

| 属性 | Light | Dark | Token |
|------|-------|------|-------|
| 容器背景 | `rgba(0,0,0,0.04)` | `rgba(255,255,255,0.14)` | `container_notice_bar` — L:`#0000000a` D:`#ffffff24` |
| 文字 + 图标 | `rgba(0,0,0,0.6)` | `rgba(255,255,255,0.5)` | `on_surface_tertiary` — L:`#00000099` D:`#ffffff80` |

> Figma Light 容器 `rgba(0,0,0,0.04)` ≈ `#0000000a`（差异 <1%）。Dark 容器 `rgba(255,255,255,0.14)` = `#ffffff24` 精确匹配 `container_notice_bar` Dark。

### Yellow（提醒 / caution）

| 属性 | Light | Dark | Token |
|------|-------|------|-------|
| 容器背景 | `rgba(255,159,5,0.1)` | `rgba(255,163,15,0.22)` | `caution_container` — L:`#ff9f051a` D:`#ffa30f38` |
| 文字 + 图标 | `#FF9F05` | `#FFA30F` | `caution` — L:`#ff9f05` D:`#ffa30f` |

> Dark 文字 Figma 节点有 `opacity: 0.9`，实际渲染 = `#FFA30F` 90% 不透明。

### Red（警告 / error）

| 属性 | Light | Dark | Token |
|------|-------|------|-------|
| 容器背景 | `rgba(250,56,46,0.08)` | `rgba(250,66,56,0.22)` | `error_container` — L:`#fa382e14` D:`#fa423838` |
| 文字 + 图标 | `#FA382E` | `#FA4238` | `error` — L:`#fa382e` D:`#fa4238` |

### Blue（强调 / primary）

| 属性 | Light | Dark | Token |
|------|-------|------|-------|
| 容器背景 | `rgba(52,130,255,0.08)` | `rgba(71,136,255,0.25)` | `secondary` — L:`#3482ff14` D:`#4788ff40` |
| 文字 + 图标 | `#3482FF` | `#4788FF` | `primary` — L:`#3482ff` D:`#4788ff` |

## 文字

| 属性 | 值 |
|------|------|
| 字体 | `font.body2`（Body 2） |
| 字号 | 14px |
| 字重 | Regular（330） |
| 对齐 | 左对齐 |
| 溢出 | 单行截断 / 多行 wrap |

## 图标

| 位置 | 尺寸 | 说明 |
|------|------|------|
| 前置（leading） | 20×20 | 可选（`Icon` 属性），颜色同文字色 |
| 尾部详情（trailing） | 20×20 | chevron_forward，颜色同文字色 |
| 尾部关闭（trailing） | 20×20 | close ×，颜色同文字色 |

> 前置图标为 HyperOS Symbols VF 字体图标（15px，20×20 frame）。HTML 实现使用 SVG 替代。

## 结构

```
NoticeBar-Container  (row, center, padding 12px 16px, border-radius 20px)
├── LeadingIcon      (20×20, optional)
├── Text             (flex: 1, Body 2 14px/330)
└── TrailingIcon     (20×20, detail chevron 或 close ×)
```

Gray 和 Blue 类型的 trailing icon 在 ContentRow 内（gap 8px 统一控制）。
Yellow 和 Red 类型的文字和 trailing icon 包裹在 TextRow 内（gap 12px），与 leading icon 之间 gap 8px。

## 交互行为

| 行为 | 说明 |
|------|------|
| 详情型（chevron） | 整条可点击，跳转目标页 |
| 关闭型（close） | 点击 × 关闭，NoticeBar 消失 |
| 按下态 | 详情型整条按下变暗；关闭型仅 × 区域按下 |

## 调用方式

```javascript
// 默认 Gray + 详情
new NoticeBar(container, {
  text: '开启小米云同步，重要笔记、待办不丢失',
  type: 'gray'
});

// Yellow + 关闭 + 前置图标
new NoticeBar(container, {
  text: '开启小米云同步，重要笔记、待办不丢失',
  type: 'yellow',
  trailing: 'close',
  icon: true,
  onClose: () => {}
});
```
