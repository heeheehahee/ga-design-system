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
| 形态 | 独立（每项单独卡片，gap 8px）/ 组合（一张卡片）/ 通栏（无卡片背景） |
| 左侧区域 | 无 / 缩进 checkmark / 图标（28×28 套底） |
| 辅助文案 | 无 / 有（标题下方，14px/330） |
| 右侧功能区 | 无 / 12 种类型 |

任意维度自由组合均有效。

---

## 基础属性

| 属性 | 值 |
|------|------|
| 容器 padding | 0 12px |
| 卡片背景 | `container_list` |
| 卡片圆角 | 20px |
| 行 padding | 14px 16px |
| 行内 gap | 14px |
| 分割线 | 1px `outline`（left/right 16px 缩进） |
| 无分割线 | `.list-item--no-divider` |
| 分组标题 padding | 20px 16px 8px（上方分隔分组，下方紧贴卡片） |
| 底部说明 padding | 8px 16px 20px |
| 按下态 | `surface_container` |

---

## 形态

| 形态 | 说明 | 关键 class |
|------|------|-----------|
| 独立 | 每项独立卡片，间距 8px | `.list-item--standalone` 每项包在单独 `.list-card` |
| 组合 | 多项在一张卡片内 | `.list-item--top/middle/bottom` 在同一个 `.list-card` |
| 通栏 | 无卡片背景，无容器 padding | `.list-card` 去掉 background + border-radius |

---

## 文本样式

| 元素 | 字号 | 字重 | Token |
|------|------|------|-------|
| 标题 | 17px | 380 | `on_surface` |
| 辅助文案 | 14px | 330 | `on_surface_tertiary` |
| 右侧文字 | 14px | 380 | `on_surface_tertiary` |
| 分组标题 | 14px | 380 | `gray` |
| 底部说明 | 13px | 330 | `on_surface_tertiary` |
| 选中态标题 | 17px | 380 | `primary` |

---

## 左侧区域

| 类型 | class | 说明 |
|------|-------|------|
| 无 | — | 默认无左侧 |
| 缩进 checkmark | `.list-item__checkmark` 放在标题前 | 20×20 蓝勾，用于左侧勾选 |
| 图标 | `.list-item__icon` | 28×28 圆角 8px，bg `surface_container` |

---

## 右侧功能区（13 种）

| 类型 | class / 实现 | 说明 |
|------|-------------|------|
| 单独进入二级 | `.list-item__trailing--arrow` | 28×28 圆底 + chevron |
| 整个列表进入二级 | `.list-item__trailing--arrow-bare` | 8×14 裸箭头 |
| 状态+进入二级 | `.list-item__trailing` 包裹文字+裸箭头 | trailing 内 gap 8px |
| 状态切换 | `.list-item__trailing--status` | 14px/330/quaternary 文字 + 10×16 上下箭头 |
| 开关 | `.switch` | 复用 Switch 组件 |
| 单选 | `.list-item__checkmark` | 20×20 蓝勾 + 标题变 primary |
| 纯文字 | `.list-item__trailing-text` | 14px/380/tertiary |
| 小按钮 | pill 样式 | padding 8px 20px, radius 999px |
| 添加 | SVG 28×28 | 蓝色圆底 + 白色 ＋ |
| 去除 | SVG 28×28 | 红色圆底 + 白色 － |
| 展开收起 | SVG 28×28 | 浅灰圆底 + 向下 chevron（旋转切换） |
| 清除 | SVG 28×28 | 浅灰圆底 + × |
| loading | SVG 26×26 | 圆环旋转 |

---

## 圆角（组合形态按位置）

| 位置 | border-radius |
|------|--------------|
| standalone | 20px |
| top | 20px 20px 0 0 |
| middle | 0 |
| bottom | 0 0 20px 20px |

---

## 行结构

```
[.list-container, padding 0 12px]
  [.list-group-title (可选), padding 20px 16px 8px] — 14px/380/gray
  [.list-card, bg container_list, radius 20px]
    [.list-item, padding 14px 16px, row, gap 14px]
      [左侧区域 (可选)] — checkmark 或 icon
      [.list-item__text, column]
        [.list-item__title] — 17px/380
        [.list-item__desc (可选)] — 14px/330
      [右侧功能区 (可选)] — trailing 容器 gap 8px
  [.list-footer (可选), padding 8px 16px 20px] — 13px/330/tertiary
```

---

## 调用方式

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
