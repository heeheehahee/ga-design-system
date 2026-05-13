---
category: components
name: Input 输入框
description: 单行输入框，支持前缀/后缀类型 × 套卡/通栏 × 位置圆角 × 背景环境 × 状态
source: Figma ga_component (node 447:8137)
---

# Input 输入框

代码：[input-code.md](input-code.md)

---

## 变体维度

| 维度 | 选项 | 说明 |
|------|------|------|
| 套卡/通栏 | 套卡 / 通栏 | 套卡外容器 padding 0 12px；通栏无 padding |
| 类型 | 前缀输入框 / 后缀输入框 | 前缀：标签左+输入右对齐固定宽；后缀：输入左fill+图标右 |
| 位置 | 上 / 中 / 下 / 独立 | 控制圆角位置 |
| 背景环境 | 白反灰 / 灰反白 | 白底→输入行 surface / 灰底→输入行 surface_container |
| 状态 | 默认 / 激活 / 输入 | 激活/输入态为整圈 2px 描边 |

---

## 基础属性

| 属性 | 值 |
|------|------|
| 外容器（套卡） | row, padding: 0 12px, width: 100% |
| 外容器（通栏） | row, padding: 0, width: 100% |
| 输入框背景 | row, align-items: center, fill × 56px |
| 独立圆角 | 16px |
| 上圆角 | 16px 16px 0 0 |
| 中圆角 | 0 |
| 下圆角 | 0 0 16px 16px |
| 分隔线 | 0.5px `outline`，上/中行底部，左右各留 16px |

---

## 后缀输入框

| 属性 | 值 |
|------|------|
| 文本区 | row, align-items: center, gap: 10px, padding: 16.5px 16px, flex: 1 |
| 输入文字 | 17px/380 `on_surface` |
| 占位文字 | 17px/380 `on_surface_octonary` |
| 后缀图标区 | row, align-items: center, padding: 0 16px 0 0, align-self: stretch |
| 图标 | 24×24, `on_surface_quaternary` |

---

## 前缀输入框

| 属性 | 值 |
|------|------|
| 前缀区 | row, align-items: center, gap: 10px, padding: 16.5px 10px 16.5px 16px, flex: 1 |
| 前缀标签 | 17px/380 `on_surface`, text-align: LEFT |
| 输入区 | row, justify-content: flex-end, gap: 12px, width: 116px 固定, padding: 0 16px 0 0 |
| 输入文字 | 16px/330 `on_surface`, text-align: RIGHT |
| 占位文字 | 16px/330 `on_surface_octonary`, text-align: RIGHT |

---

## 背景环境

| 环境 | 容器背景 | 输入行背景 |
|------|---------|-----------|
| 白反灰 | `surface`（白） | `surface_container_low`（rgba(0,0,0,0.06)） |
| 灰反白 | `surface_pop_window`（#F7F7F7） | `surface`（白） |

### 调用规则

根据输入框所在容器的背景色选择：
- **容器是白色/浅色**（如 `surface`）→ 使用**白反灰**，输入框用灰色背景以形成对比
- **容器是灰色/彩色/深色**（如 `surface_pop_window`、`surface_high` 或彩色背景）→ 使用**灰反白**，输入框用白色背景以形成对比

原则：**输入框背景必须与容器背景产生对比**，不能和容器同色融为一体。

---

## 状态

| 状态 | 表现 |
|------|------|
| 默认 | 无描边 |
| 激活 | 整圈 2px 描边 `primary`，圆角 16px |
| 输入 | 同激活 + 显示输入文字 |

---

## 报错

| 属性 | 值 |
|------|------|
| 描边 | 2px `error`，圆角 16px |
| 错误提示文字 | 13px/330 `error`，padding: 8px 12px 16px |
