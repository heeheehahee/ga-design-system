---
category: components
name: 轻提示 Toast
description: 轻量级消息反馈，用于操作结果提示、状态变更通知等场景，自动消失不打断用户操作
source: Figma ga_component (node 288:4763)
---

# 轻提示 Toast

代码：[toast-code.md](toast-code.md)
图标：[icons.md](icons.md)（文案+图标类型时使用）

## 变体矩阵

| 维度 | 选项 | 说明 |
|------|------|------|
| 模式 | Light / Dark | 跟随系统主题 |
| 类型 | 仅文案 / 文案+图标 | 图标用于强化语义（成功✓、失败✗等） |
| 行数 | 单行 / 双行 | 文案过长时自动折行，最多两行 |

共 2×2×2 = **8 个变体**。

## 基础属性

| 属性 | 值 | 说明 |
|------|------|------|
| 背景 | Light `#FAFAFA` / Dark `#242424` | 自定义变量 `--toast-bg`（Dark 匹配 `surface_pop_window`，Light 为 Figma 原值，无完全匹配 Token） |
| 描边 | `outline` 0.34px | Light `#0000001a` / Dark `#ffffff33` |
| 圆角 | 20px（`radius.medium`） | Dark 仅文案变体 Figma 为 16px（`radius.common`），其余均 20px |
| 投影 | `0px 18px 36px rgba(0,0,0,0.1)` | Toast 专属投影 |
| 层级 | `z.toast` | 最高层级，覆盖所有内容 |

## 文字

| 属性 | 值 |
|------|------|
| 字体 | `font.subtitle`（Subtitle） |
| 字号 | 14px |
| 字重 | Medium（380） |
| 颜色 | Light `rgba(0,0,0,0.6)` = `on_surface_tertiary` / Dark `rgba(255,255,255,0.6)` |
| 单行对齐 | 居中 |
| 双行对齐 | 左对齐 |
| 最大行数 | 2 行 |

> Dark 文字色 `rgba(255,255,255,0.6)` = `#ffffff99`，与 `on_surface_tertiary` Dark（`#ffffff80` = 0.5）有差异。Figma 语义标签为「次要文字 60%」，CSS 使用自定义变量 `--toast-text` 精确还原。

## 图标（文案+图标类型）

| 属性 | 值 |
|------|------|
| 尺寸 | 24×24 |
| 颜色 | 同文字色（`--toast-text`） |
| 位置 | 文案左侧，间距 10px |

图标为插槽设计，由调用方传入具体 SVG。常见语义图标：成功（✓）、失败（✗）、提醒（!）等。

## 布局

| 类型 | 内边距 | 排列 | 对齐 |
|------|--------|------|------|
| 仅文案 | 15.5px 20px | — | 水平居中 |
| 文案+图标（Light） | 13px 20px | 水平排列，gap 10px | 垂直居中 |
| 文案+图标（Dark） | 15.5px 20px | 水平排列，gap 10px | 垂直居中 |

> Light 图标变体 padding-top/bottom 为 13px，Dark 为 15.5px。差异来自 Figma，实际实现可统一取 14px 折中或按模式区分。

- 宽度：自适应内容（hug），最大宽度 = 页面宽度 - 2×20px
- 双行固定宽度时自动折行
- 位置：屏幕底部居中，距底部 Tab 栏上方适当间距

## 交互行为

| 行为 | 说明 |
|------|------|
| 入场 | 淡入 + 上移 |
| 停留 | 默认 3 秒 |
| 退场 | 淡出 + 下移 |
| 用户操作 | 不可点击、不可滑动关闭 |
| 队列 | 新 Toast 替换当前 Toast |

## 调用方式

```javascript
// 纯文案
Toast.show('操作成功');

// 文案 + 图标
Toast.show('操作成功', { icon: '<svg>...</svg>' });

// 自定义时长
Toast.show('操作成功', { duration: 5000 });
```

## 自定义 CSS 变量

Toast 有 3 个值不完全匹配标准 Token，使用组件级变量精确还原 Figma：

| 变量 | Light | Dark | 说明 |
|------|-------|------|------|
| `--toast-bg` | `#FAFAFA` | `#242424` | 背景色（Dark = `surface_pop_window`，Light 为 Figma 原值） |
| `--toast-text` | `rgba(0,0,0,0.6)` | `rgba(255,255,255,0.6)` | 文字 + 图标色（Figma 语义「次要文字 60%」） |

## 设计备注

1. Toast 始终水平居中于页面，不受页面滚动影响（`position: fixed`）
2. 同一时刻只显示一个 Toast，新 Toast 立即替换旧的
3. Dark 仅文案变体圆角为 16px（其余均 20px），疑似 Figma 未同步，实现统一用 20px
