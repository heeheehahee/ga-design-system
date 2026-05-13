---
category: components
name: Tooltip 提示气泡
description: 轻量提示组件，用于引导用户关注新功能或操作提示
source: Figma ga_component / Tooltip-ComponentSet (node 197:7545)
---

# Tooltip 提示气泡

代码：[tooltip-code.md](tooltip-code.md)

## 使用场景

- 新功能引导提示
- 操作说明提示
- 临时信息展示

## 规格

| 属性 | 值 |
|------|------|
| 宽度 | 自适应内容 |
| 高度 | 自适应内容 |
| 圆角 | 12 `{radius.small}` |
| 内边距 | 15/14/15/14 |
| 背景 | `surface`（亮色 #ffffff / 暗色 #000000），箭头同色 |
| 文字 | fs=14 fw=330（`font.body2`） |
| 文字色 | `on_surface_tertiary`（亮色 rgba(0,0,0,0.6) / 暗色 rgba(255,255,255,0.5)） |
| 箭头尺寸 | 36×12 |

## 方向

8 个方向，由箭头位置决定：

| 方向 | 箭头位置 | 说明 |
|------|---------|------|
| 上-左 | 底部左侧 | 气泡在目标上方，箭头指向下偏左 |
| 上-中 | 底部居中 | 气泡在目标上方，箭头指向正下 |
| 上-右 | 底部右侧 | 气泡在目标上方，箭头指向下偏右 |
| 下-左 | 顶部左侧 | 气泡在目标下方，箭头指向上偏左 |
| 下-中 | 顶部居中 | 气泡在目标下方，箭头指向正上 |
| 下-右 | 顶部右侧 | 气泡在目标下方，箭头指向上偏右 |
| 左-中 | 右侧居中 | 气泡在目标左侧，箭头指向右 |
| 右-中 | 左侧居中 | 气泡在目标右侧，箭头指向左 |

## 调用方式

```js
// 显示 Tooltip
const tip = new Tooltip({
  text: '提示文字，提示一些新功能。',
  direction: 'top-left',   // top-left|top-center|top-right|bottom-left|bottom-center|bottom-right|left-center|right-center
  target: '#some-element'  // 目标元素选择器或 DOM 元素
});

// 关闭
tip.destroy();
```

## 注意事项

- Tooltip 为临时提示，不承载核心信息
- 同一时间页面上最多显示一个 Tooltip
- 点击气泡外部区域或滚动时自动关闭
- 背景使用 `surface_pop_window`，跟随明暗模式自动切换
