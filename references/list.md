---
category: components
name: List 列表
description: 可自由组合的列表组件，形态(独立/组合/通栏) × 左侧(无/缩进/图标) × 右侧(12种功能区) × 辅助文案
source: Figma ga_component (node 372:9273)
---

# List 列表

代码：[list-code.md](list-code.md)

---

## 组合维度

| 维度 | 选项 |
|------|------|
| 形态 | 独立（每项单独卡片）/ 组合（一张卡片内多项）/ 通栏（无卡片背景） |
| 左侧区域 | 无 / 缩进 checkmark / 图标（圆角套底） |
| 辅助文案 | 无 / 有（标题下方副文字） |
| 右侧功能区 | 无 / 13 种类型（见下方） |

任意维度自由组合均有效。

---

## 形态选择

| 形态 | 适用场景 | 关键 class |
|------|---------|-----------|
| 独立 | 每项可独立操作、间距明显 | `.list-item--standalone` 各自独立 `.list-card` |
| 组合 | 设置页、属性列表等紧凑排列 | `.list-item--top/middle/bottom` 在同一 `.list-card` |
| 通栏 | 无卡片背景，全宽展示 | `.list-card` 去掉背景和圆角 |

---

## 左侧区域

| 类型 | 何时用 | class |
|------|--------|-------|
| 无 | 大多数列表 | — |
| 缩进 checkmark | 左侧勾选 | `.list-item__checkmark` |
| 图标 | 需要视觉标识（设置项、应用列表） | `.list-item__icon` |

---

## 右侧功能区（13 种）

| 类型 | 何时用 | class |
|------|--------|-------|
| 单独进入二级 | 只有箭头可点 | `.list-item__trailing--arrow` |
| 整行进入二级 | 整行可点击跳转 | `.list-item__trailing--arrow-bare` |
| 状态+进入二级 | 显示当前值+跳转 | trailing 内文字 + arrow-bare |
| 状态切换 | 上下切换值 | `.list-item__trailing--status` |
| 开关 | 布尔值控制 | `.switch` 组件 |
| 单选 | 列表中选一项 | checkmark + 标题变色 |
| 纯文字 | 只显示状态文字 | `.list-item__trailing-text` |
| 小按钮 | 行内操作 | pill 样式 |
| 添加 | 新增项 | 蓝色圆底 + 白色 ＋ |
| 去除 | 删除项 | 红色圆底 + 白色 － |
| 展开收起 | 折叠内容 | chevron 旋转 |
| 清除 | 清除内容 | 浅灰圆底 + × |
| loading | 异步操作中 | 圆环旋转 |

---

## 位置圆角

组合形态时，根据行在卡片中的位置决定圆角：

| 位置 | 说明 |
|------|------|
| standalone | 单独一项，四角圆 |
| top | 卡片第一项，顶部圆角 |
| middle | 中间项，无圆角 |
| bottom | 卡片最后一项，底部圆角 |

---

## 调用方式（HTML 结构）

```html
<!-- 组合形态 + 右侧箭头 -->
<div class="list-container">
  <div class="list-card">
    <div class="list-item list-item--top list-item--no-divider">
      <div class="list-item__text">
        <div class="list-item__title">标题</div>
      </div>
      <div class="list-item__trailing list-item__trailing--arrow"></div>
    </div>
    <div class="list-item list-item--bottom">
      <div class="list-item__text">
        <div class="list-item__title">标题</div>
      </div>
      <div class="list-item__trailing list-item__trailing--arrow"></div>
    </div>
  </div>
</div>

<!-- 独立形态 + 左侧图标 + 辅助文案 + 状态箭头 -->
<div class="list-container" style="display:flex;flex-direction:column;gap:8px">
  <div class="list-card">
    <div class="list-item list-item--standalone">
      <div class="list-item__icon"><!-- SVG --></div>
      <div class="list-item__text">
        <div class="list-item__title">标题</div>
        <div class="list-item__desc">辅助文案</div>
      </div>
      <div class="list-item__trailing">
        <div class="list-item__trailing-text">状态</div>
        <div class="list-item__trailing--arrow-bare"></div>
      </div>
    </div>
  </div>
</div>

<!-- 组合形态 + 开关 -->
<div class="list-container">
  <div class="list-card">
    <div class="list-item list-item--top list-item--no-divider">
      <div class="list-item__text">
        <div class="list-item__title">飞行模式</div>
      </div>
      <div class="switch"></div>
    </div>
    <div class="list-item list-item--bottom">
      <div class="list-item__text">
        <div class="list-item__title">深色模式</div>
      </div>
      <div class="switch switch--on"></div>
    </div>
  </div>
</div>
```

> 所有精确样式值（间距、字号、圆角、颜色）见 [list-code.md](list-code.md)
