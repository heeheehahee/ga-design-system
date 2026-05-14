---
category: components
name: 加载 Loading
description: 加载状态反馈组件，包含旋转图标（LoadingIcon）和加载弹层（LoadingToast），用于页面加载、下拉刷新、组件内加载等场景
source: Figma ga_component (node 319:9852)
---

# 加载 Loading

代码：[loading-code.md](loading-code.md)

---

## 组件选择

| 组件 | 适用场景 |
|------|---------|
| LoadingIcon | 页面内局部加载（全页、下拉刷新、组件内） |
| LoadingToast | 页面级二次加载（居中弹层 + 遮罩覆盖） |

## LoadingIcon 加载图标

旋转圆环 + 圆点指示器，可选附带文字。

### 变体

| 维度 | 选项 | 说明 |
|------|------|------|
| 尺寸 | Default / Small | Default 用于页面级，Small 用于组件内/下拉刷新 |
| 类型 | 无文案 / 有文案 | 有文案时下方显示"加载中"等文字 |
| 状态 | 默认 / 禁用 | 禁用态颜色变淡 |

### 应用场景

| 场景 | 尺寸 | 文案 |
|------|------|------|
| 全页面加载 | Default | 无 |
| 下拉刷新 | Small | 无 |
| 组件内加载 | Small | 无 |
| 带提示的等待 | Default | 有（如"加载中"） |

## LoadingToast 加载弹层

页面居中弹层，包含 spinner + 可选文字，覆盖在内容之上。用于需要阻断用户操作的加载场景。

---

## 调用方式

```javascript
// 页面加载 - 纯 spinner
new LoadingIcon(container, { size: 'default' });

// 带文字
new LoadingIcon(container, { size: 'default', text: '加载中' });

// 小尺寸（组件内/下拉刷新）
new LoadingIcon(container, { size: 'small' });

// 加载弹层
LoadingToast.show('加载中');
LoadingToast.hide();
```

> 所有精确样式值（尺寸、颜色、圆角、投影、动画）见 [loading-code.md](loading-code.md)
