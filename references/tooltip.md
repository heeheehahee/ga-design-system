---
category: components
name: Tooltip 提示气泡
description: 轻量提示组件，用于引导用户关注新功能或操作提示
source: Figma ga_component / Tooltip-ComponentSet (node 197:7545)
---

# Tooltip 提示气泡

代码：[tooltip-code.md](tooltip-code.md)

## 何时使用

- 新功能引导提示
- 操作说明提示
- 临时信息展示

## 方向（8 种）

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

## 交互

- 点击气泡外部区域或滚动时自动关闭
- 同一时间页面上最多显示一个 Tooltip
- 为临时提示，不承载核心信息

## 调用方式

```js
const tip = new Tooltip({
  text: '提示文字，提示一些新功能。',
  direction: 'top-left',   // 8 种方向
  target: '#some-element'
});

tip.destroy();
```

> 所有精确样式值（圆角、间距、颜色、箭头尺寸）见 [tooltip-code.md](tooltip-code.md)
