---
category: components
name: 轻提示 Toast
description: 轻量级消息反馈，用于操作结果提示、状态变更通知等场景，自动消失不打断用户操作
source: Figma ga_component (node 288:4763)
---

# 轻提示 Toast

代码：[toast-code.md](toast-code.md)
图标：[icons.md](icons.md)（文案+图标类型时使用）

## 变体

| 维度 | 选项 | 说明 |
|------|------|------|
| 类型 | 仅文案 / 文案+图标 | 图标用于强化语义（成功✓、失败✗等） |
| 行数 | 单行 / 双行 | 文案过长时自动折行，最多两行 |

## 何时使用

| 场景 | 用 Toast 吗 |
|------|------------|
| 操作成功/失败的即时反馈 | ✓ |
| 状态变更通知（已复制、已收藏） | ✓ |
| 需要用户操作的提示 | ✗ 用 Dialog |
| 需要持久展示的信息 | ✗ 用 NoticeBar |

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

> 所有精确样式值（背景、圆角、投影、字号、颜色、间距）见 [toast-code.md](toast-code.md)
