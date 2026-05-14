---
category: components
name: 信息提示 NoticeBar
description: 页面内信息提示条，用于系统通知、功能引导、警告提醒等场景，支持 4 种语义类型
source: Figma ga_component (node 304:9560)
---

# 信息提示 NoticeBar

代码：[noticebar-code.md](noticebar-code.md)
图标：[icons.md](icons.md)

## 类型选择

| 类型 | 语义 | 适用场景 |
|------|------|---------|
| Gray | 默认/中性 | 一般性通知、功能引导 |
| Yellow | 提醒/注意 | 需要用户关注但不紧急 |
| Red | 警告/错误 | 危险操作提醒、错误状态 |
| Blue | 强调/信息 | 重要功能介绍、新功能引导 |

## 变体维度

| 维度 | 选项 | 说明 |
|------|------|------|
| 类型 | Gray / Yellow / Red / Blue | 语义颜色 |
| 前置图标 | 有 / 无 | 可选，用于强化语义 |
| 尾部操作 | 详情（chevron →）/ 关闭（×） | Gray 默认详情，其余默认关闭 |

## 结构

```
┌──────────────────────────────────────┐
│ [前置图标]  提示文字内容        [尾部图标] │
└──────────────────────────────────────┘
```

## 交互行为

| 尾部类型 | 行为 |
|---------|------|
| 详情（chevron） | 整条可点击，跳转目标页 |
| 关闭（close） | 点击 × 关闭，NoticeBar 消失 |

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

> 所有精确样式值（颜色、圆角、间距、字号）见 [noticebar-code.md](noticebar-code.md)
