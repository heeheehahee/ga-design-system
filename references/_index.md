---
name: GA/GH 设计
version: "0.3"
created: 2026-04-15
updated: 2026-05-14
description: 工作参考手册 — 间距/命名/暗色/切图/Token 详细规范
---

# GA/GH 设计 — 工作参考手册

> 核心铁律和工作流见 SKILL.md（必读）。本文件为工作中按需查阅的详细规范。

---

## 一、间距与布局

### 基础单位

| 属性 | 值 | 说明 |
|------|------|------|
| 设计基准宽度 | 392px | 逻辑像素 |
| 基础网格 | 4px | 最小步进单位，所有间距为 4 的倍数 |

### 间距 Token 表

| Token | 倍数 | 值 | 用途 |
|-------|------|------|------|
| `spacing.1x` | 1× | 4px | 极小间距（图标与文字紧贴） |
| `spacing.2x` | 2× | 8px | 内部元素小间距 |
| `spacing.3x` | 3× | 12px | 栅格列间距（Gutter） |
| `spacing.4x` | 4× | 16px | 常用内边距、组件内间距 |
| `spacing.5x` | 5× | 20px | 页面左右边距 |
| `spacing.6x` | 6× | 24px | 区块间距 |
| `spacing.8x` | 8× | 32px | 大区块间距 |
| `spacing.12x` | 12× | 48px | 页面级大间距 |

### 布局规则

- 页面左右边距：`spacing.5x`（20px）
- 卡片内边距：`spacing.4x`（16px）
- 卡片内区块间距：`spacing.2x`（8px）~ `spacing.3x`（12px）
- 关联元素间距：`spacing.1x`（4px）
- 栅格 Gutter：`spacing.3x`（12px）
- 大区块分隔：`spacing.8x`（32px）~ `spacing.12x`（48px）

### 前端实现

- **Web**：使用 `px`，布局采用 flex/grid，大屏居中留白
- **Android**：使用 `dp`，布局采用 ConstraintLayout / Compose

---

## 二、字体

统一字体族：**MiSans Latin VF**

详细 Text Style 见 [typography.md](typography.md)。

---

## 三、暗色模式

### 工作流

1. 确认所有颜色 Token 在 `:root`（dark）和 `.theme-light` 中都有定义
2. 需要 alpha 渐变的颜色检查 `--token-rgb` 变体
3. 业务专属色需单独写 `body:not(.theme-light)` 覆盖
4. **先验 Token 覆盖率 → 再写代码**

### RGB 辅助变量规范

每个用于 `rgba(var(--token-rgb), alpha)` 渐变的颜色都需要提供 RGB 变体：

```css
--surface: #ffffff;
--surface-rgb: 255,255,255;  /* 同步提供 */
```

命名规则：`--原token名-rgb`，值为逗号分隔的 `R,G,B`。

---

## 四、图标与切图

### 图标渲染规范

| 场景 | 方式 | 说明 |
|------|------|------|
| 标题栏/Tab 图标 | JS 常量 + `innerHTML` | `fill="currentColor"`，颜色由父元素 CSS 控制 |
| 功能小图标（眼睛、下载等） | `<span>` + CSS mask（data URI） | `background: var(--token)` 控制颜色 |
| 多色图标（Logo 等） | 内联 SVG（渐变/滤镜） | 不跟随 tint，Android 转 PNG 2x/3x |
| ❌ 禁止 | `<img src="*.svg">` | 不支持 `currentColor`，高分屏可能模糊 |

### 切图导出规范

交付给研发的 icon 切图规则：

| 规则 | 说明 |
|------|------|
| **必须从 Figma 切** | 禁止自己画 SVG path、禁止自己拼图标 |
| **按画框尺寸切** | 导出 Frame 完整尺寸（含留白），留白是设计意图 |
| **统一 WEBP 3x** | 格式 WEBP（质量 90），尺寸 = 逻辑尺寸 × 3，透明背景 |
| **模块前缀命名** | `模块-功能.webp`，如 `navbar-search.webp` |
| **同图标不同状态用 tint** | 满星/空星只出一张，研发通过颜色控制 |
| **只切实际使用的** | 先 grep 确认图标被引用 |
| **图片资源不切** | 接口下发的内容不需要切图 |

**导出流程**：列清单 → 从 Figma 找节点（优先外层 Frame）→ 3x 导出 → 转 WEBP → 逐张预览检查 → 删除未使用的

---

## 五、命名规范

### 组件命名

格式：`层级前缀_功能_类型_核心属性`

| 层级前缀 | 适用范围 | 示例 |
|---------|---------|------|
| `atoms_` | 不可再拆分的最小单元 | `atoms_button_primary` |
| `molecules_` | 由 atoms 组合而成 | `molecules_card_title` |
| `organisms_` | 完整业务卡片/模块 | `organisms_checkin_card` |

规则：全英文、全小写、下划线分隔（snake_case）

### Token 引用规则

| 技术栈 | 颜色引用 | 示例 |
|--------|---------|------|
| H5 | `var(--Token名)` | `color: var(--on_surface)` |
| Android | `R.color.Token名` | `colorResource(R.color.on_surface)` |

Figma 中的 semantic token 路径映射：取最后两段，去掉 `semantic/` 前缀，用 `_` 连接。

### Android 目录结构

```
ui/
          ← atoms、molecules 通用组件
  features/
    <模块名>/        ← organisms 及页面级组件
      organisms_<name>.kt
```

### 资源文件命名

| 类型 | 格式 | 示例 |
|------|------|------|
| XML Layout | `<层级前缀>_<功能模块>.xml` | `organisms_checkin_card.xml` |
| Compose | PascalCase，与组件命名映射 | `OrganismsCheckinCard()` |
| String 资源 | `<模块>_<描述>` | `R.string.checkin_btn_label` |
| Color 资源 | 语义 token 名，`/` 替换为 `_` | `R.color.text_title` |

---

## 六、基础 Token

| 文件 | 内容 |
|------|------|
| [color.md](color.md) | HyperOS3 Feature Token 66 色（light/dark 双模式） |
| [typography.md](typography.md) | 字体族、字号、字重、行高 |
| [border-radius.md](border-radius.md) | 圆角梯度规范（8 级） |
| [elevation.md](elevation.md) | 阴影梯度（shadow-low/regular/high/float 4 级 + z-index 层级） |

---

## 七、组件清单

### 已实现

| 规范 | 代码 | 内容 |
|------|------|------|
| [button.md](button.md) | [button-code.md](button-code.md) | 按钮（Base / Custom / Install 三类） |
| [navbar.md](navbar.md) | [navbar-code.md](navbar-code.md) | 页面结构（标题栏 4 类型 × 5 维度 / 底 Tab 栏） |
| [tooltip.md](tooltip.md) | [tooltip-code.md](tooltip-code.md) | Tooltip 提示气泡（8 个方向） |
| [dialog.md](dialog.md) | [dialog-code.md](dialog-code.md) | Dialog 弹窗（7 种类型 + 内容滚动） |
| [icons.md](icons.md) | — | 图标库 |
| [toast.md](toast.md) | [toast-code.md](toast-code.md) | 轻提示 Toast |
| [noticebar.md](noticebar.md) | [noticebar-code.md](noticebar-code.md) | 信息提示 NoticeBar（4 类型） |
| [loading.md](loading.md) | [loading-code.md](loading-code.md) | 加载 Loading |
| [input.md](input.md) | [input-code.md](input-code.md) | 输入框 |
| [list.md](list.md) | [list-code.md](list-code.md) | 列表 |
| [drawer.md](drawer.md) | [drawer-code.md](drawer-code.md) | 底部抽屉 |
| [segmented-controls.md](segmented-controls.md) | [segmented-controls-code.md](segmented-controls-code.md) | 分段按钮 |
| [checkbox.md](checkbox.md) | [checkbox-code.md](checkbox-code.md) | Checkbox 多选 |
| [switch.md](switch.md) | [switch-code.md](switch-code.md) | Switch 开关 |
| — | [chips-code.md](chips-code.md) | Chips 横滑标签栏 |
| — | [scrollbar-code.md](scrollbar-code.md) | 自定义滚动条 |
| — | [skeleton-code.md](skeleton-code.md) | 骨架占位块 |
| — | [star-rating-code.md](star-rating-code.md) | 星级评分 |
| — | [underline-tabbar-code.md](underline-tabbar-code.md) | 二级文字标签 |
| — | [sub-chips-code.md](sub-chips-code.md) | 二级胶囊标签 |
| — | [popup-menu-code.md](popup-menu-code.md) | PopupMenu 近手菜单 |
| — | [rating-bars-code.md](rating-bars-code.md) | 评分分布条 |
| — | [content-cards-code.md](content-cards-code.md) | 内容卡片集 |
| — | [bottom-action-bar-code.md](bottom-action-bar-code.md) | 底部操作栏 |

### 开发工具

| 代码 | 内容 |
|------|------|
| [debug-tool-code.md](debug-tool-code.md) | DebugTool 页面调试工具（新页面必装） |

### 预览

[design-system-preview.html](design-system-preview.html) — 所有已实现组件的交互预览，支持明暗/主色切换
