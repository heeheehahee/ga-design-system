---
name: GA/GH 设计
version: "0.2"
created: 2026-04-15
updated: 2026-04-21
description: Getapps 设计系统总索引，做页面前先读此文件
---

# GA/GH 设计

---

## 一、工作流程

做页面前必须确认以下信息，未明确则**主动询问**：

| 确认项 | 选项 | 说明 |
|--------|------|------|
| **技术栈** | H5（HTML/CSS/JS）/ Android（Kotlin/Compose） | 决定产出格式、单位、资源引用方式 |
| **主题色** | GA 默认绿色 `#05C474` / 社区黄色 `#FFE249` | 需求未说明是否社区相关时，必须先询问 |
| **明暗模式** | 亮色（GA 默认）/ 暗色（社区默认） | GA 全局页面默认亮色，社区相关页面默认暗色 |

**代码调用规则**：从对应的代码 MD 文件（如 button-code.md、navbar-code.md）直接复制代码，不可凭记忆重写。H5 取 CSS + JS，Android 取 Compose/XML 代码。图标从 [icons.md](icons.md) 取 SVG。

### 写代码前必须检索（逐项）

每写一个属性都要检索对应规范，不能凭印象：

| 要写什么 | 检索哪里 | 找到 → | 没找到 → |
|---------|---------|--------|---------|
| 组件 | `preview.html` → `*-code.md` | 直接复制 | 标注 `/* 组件库无此代码 */` 并告知用户 |
| 颜色 | `color.md` → `:root` 变量 | 用 `var(--token)` | 不可 hardcode，先确认是否需要新增 Token |
| 图标 | `icons.md` → JS 常量 | 复制 SVG | 从 Figma 导出，`fill="currentColor"` |
| 字号/字重 | `typography.md` | 用规范值 | 不可自创，先和设计确认 |
| 圆角 | `border-radius.md` | 用 8 级体系值 | 不可自创 |
| 间距 | 本文件「间距规范」 | 用 4px 网格值 | 不允许 2/3/5/6/7/9 等非网格值 |

### AI Coding 禁止行为

- 禁止 hardcode 颜色值（如 `#FFE249`、`Color.WHITE`），H5 必须使用 CSS 变量，Android 必须使用 `R.color.xxx`
- 禁止 hardcode 字体名称字符串，字体通过全局样式统一定义
- 禁止绝对定位，布局使用 flex/Column/Row + padding 表达间距
- 禁止自定义样式、颜色、圆角、间距、图标，生成前必须先查本规范和组件库，优先复用已有资源
- 组件库中无对应图标时，使用占位图标，禁止自制图标
- 禁止内联样式 `style=""`（`display:none` 状态切换和 `background-image` 数据绑定除外）
- 禁止非 4px 网格的间距值（2/3/5/6/7/9px 等），组件内部微调（如星星 gap 2px）需标注原因
- 禁止在 `body.theme-*` 选择器内 hardcode 颜色，必须通过 `:root` 变量覆盖

### 图标渲染规范

| 场景 | 方式 | 说明 |
|------|------|------|
| 标题栏/Tab 图标 | JS 常量 + `innerHTML` | `fill="currentColor"`，颜色由父元素 CSS 控制 |
| 功能小图标（眼睛、下载等） | `<span>` + CSS mask（data URI） | `background: var(--token)` 控制颜色 |
| 多色图标（Logo 等） | 内联 SVG（渐变/滤镜） | 不跟随 tint，Android 转 PNG 2x/3x |
| ❌ 禁止 | `<img src="*.svg">` | 不支持 `currentColor`，高分屏可能模糊 |

### 切图导出规范

交付给研发的 icon 切图，必须遵守以下规则：

| 规则 | 说明 |
|------|------|
| **必须从 Figma 切** | 禁止自己画 SVG path、禁止自己拼图标。从设计稿或组件库导出 |
| **按画框尺寸切** | 导出图标所在 Frame 的完整尺寸（含留白），不是紧贴图形内容。留白是设计意图，保证对齐 |
| **统一 WEBP 3x** | 格式 WEBP（质量 90），尺寸 = 逻辑尺寸 × 3，透明背景 |
| **模块前缀命名** | `模块-功能.webp`，如 `navbar-search.webp`、`baseinfo-star-rating.webp`、`comment-like-default.webp` |
| **同图标不同状态用 tint** | 满星/空星、选中/未选中只出一张切图，研发通过颜色控制。不出多张 |
| **只切实际使用的** | 先在 HTML/JS 中 grep 确认图标被引用，组件库有但页面没用的不切 |
| **图片资源不切** | app icon、截图、feed 缩略图等接口下发的内容不需要切图，只切 icon 类 |

**导出流程**：列清单 → 从 Figma 找节点（优先外层 Frame）→ 3x 导出 → 转 WEBP → 逐张预览检查 → 删除未使用的

### 暗色模式工作流

做页面前必须确认：
1. 所有用到的颜色 Token 是否在 `:root`（dark）和 `.theme-light` 中都有定义
2. 需要 alpha 渐变的颜色是否有 `--token-rgb` 变体
3. 签到等业务专属色需单独写 `body:not(.theme-light)` 覆盖

**先验 Token 覆盖率 → 再写代码**，不能先亮色做完再补暗色。

### RGB 辅助变量规范

每个用于 `rgba(var(--token-rgb), alpha)` 渐变的颜色都需要提供 RGB 变体：

```css
--surface: #ffffff;
--surface-rgb: 255,255,255;  /* 同步提供 */
```

命名规则：`--原token名-rgb`，值为逗号分隔的 `R,G,B`（无 # 无空格后的空格）。

---

## 二、全局规则

### 字体

统一字体族：**MiSans Latin VF**

### 基础单位

| 属性 | 值 | 说明 |
|------|------|------|
| 设计基准宽度 | 392px | 逻辑像素 |
| 基础网格 | 4px | 最小步进单位，所有间距为 4 的倍数 |

前端实现：
- **Web**：使用 `px`，布局采用 flex/grid，大屏居中留白
- **Android**：使用 `dp`，布局采用 ConstraintLayout / Compose

### 间距规范

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

### 组件命名规则

格式：`层级前缀_功能_类型_核心属性`

| 层级前缀 | 适用范围 | 示例 |
|---------|---------|------|
| `atoms_` | 不可再拆分的最小单元 | `atoms_button_primary` |
| `molecules_` | 由 atoms 组合而成 | `molecules_card_title` |
| `organisms_` | 完整业务卡片/模块 | `organisms_checkin_card` |

规则：
- 全英文、全小写、下划线分隔（snake_case）
- Figma 图层名、文件名、Kotlin 类名保持一致映射：
  - 文件名：`atoms_button_primary.kt`
  - Kotlin 类名：`AtomsButtonPrimary`（转 PascalCase）

### Token 引用规则

颜色 Token 名在 [color.md](color.md) 中定义（如 `on_surface`、`surface_pop_window`），代码引用方式：

| 技术栈 | 颜色引用 | 示例 |
|--------|---------|------|
| H5 | `var(--Token名)` | `color: var(--on_surface)` |
| Android | `R.color.Token名` | `colorResource(R.color.on_surface)` |

Figma 中的 semantic token 路径映射：取最后两段，去掉 `semantic/` 前缀，用 `_` 连接。

| Figma token | Android | H5 |
|-------------|---------|------|
| `semantic/text/title/title` | `R.color.text_title` | `var(--text_title)` |
| `semantic/fill/popup` | `R.color.fill_popup` | `var(--fill_popup)` |
| `semantic/spacing/xl` | 直接写 `20dp` | 直接写 `20px` |
| `semantic/button/radius/md` | 直接写 `12dp` | 直接写 `12px` |

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

## 三、基础 Token

| 文件 | 内容 |
|------|------|
| [color.md](color.md) | HyperOS3 Feature Token 66 色（light/dark 双模式） |
| [typography.md](typography.md) | 字体族、字号、字重、行高 |
| [border-radius.md](border-radius.md) | 圆角梯度规范（8 级） |
| [elevation.md](elevation.md) | 阴影梯度（shadow-low/regular/high/float 4 级 + z-index 层级） |

---

## 四、组件

### 已实现

| 规范 | 代码 | 内容 |
|------|------|------|
| [button.md](button.md) | [button-code.md](button-code.md) | 按钮（Base / Custom / Install 三类） |
| [navbar.md](navbar.md) | [navbar-code.md](navbar-code.md) | 页面结构（标题栏 4 类型 × 5 维度 / 底 Tab 栏） |
| [tooltip.md](tooltip.md) | [tooltip-code.md](tooltip-code.md) | Tooltip 提示气泡（8 个方向） |
| [dialog.md](dialog.md) | [dialog-code.md](dialog-code.md) | Dialog 弹窗（确认/警示/引导/输入/进度/选择/权限 7 种类型） |
| [icons.md](icons.md) | — | 图标库（标题栏 19 个 + 底 Tab 5 个 + 按钮图标索引） |
| [toast.md](toast.md) | [toast-code.md](toast-code.md) | 轻提示 Toast（仅文案 / 文案+图标 × 单行 / 双行） |
| [noticebar.md](noticebar.md) | [noticebar-code.md](noticebar-code.md) | 信息提示 NoticeBar（Gray/Yellow/Red/Blue 4 类型 × 详情/关闭） |
| [loading.md](loading.md) | [loading-code.md](loading-code.md) | 加载 Loading（LoadingIcon 旋转图标 + LoadingToast 加载弹层） |
| — | [chips-code.md](chips-code.md) | Chips 横滑标签栏（药丸切换 + 滑动动画 + 自动居中） |
| — | [skeleton-code.md](skeleton-code.md) | SkeletonBone 骨架占位块（shimmer 动画 + 5 种预设形状） |
| — | [star-rating-code.md](star-rating-code.md) | StarRating 星级评分（满星/空星/半星 + 展示/交互模式） |
| — | [underline-tabbar-code.md](underline-tabbar-code.md) | 二级文字标签（文字 Tab + 滑动下划线指示器，适用抽屉/面板内切换） |
| — | [sub-chips-code.md](sub-chips-code.md) | 二级胶囊标签（小尺寸筛选药丸，普通+高亮+计数，用于抽屉内内容筛选） |
| — | [popup-menu-code.md](popup-menu-code.md) | PopupMenu 近手菜单（遮罩 + 缩放动画 + 分隔线 + 危险项） |
| — | [rating-bars-code.md](rating-bars-code.md) | RatingBars 评分分布条（大数字+星星+5 条进度条，依赖 StarRating） |
| — | [content-cards-code.md](content-cards-code.md) | 内容卡片集：ReviewCard 评论卡 / FeedCard 攻略卡 / AppCard 应用卡 |
| — | [bottom-action-bar-code.md](bottom-action-bar-code.md) | BottomActionBar 底部操作栏（渐变遮罩 + 主按钮 + 可选辅助按钮） |
| [checkbox.md](checkbox.md) | [checkbox-code.md](checkbox-code.md) | Checkbox 多选（Default/Media 2 类型 × 4 状态） |
| [switch.md](switch.md) | [switch-code.md](switch-code.md) | Switch 开关（On/Off × Disabled/Loading 5 状态） |
| [drawer.md](drawer.md) | [drawer-code.md](drawer-code.md) | Drawer 底部抽屉（Max/Medium/Min 3 档 + 玻璃材质） |
| [list.md](list.md) | [list-code.md](list-code.md) | List 列表（基础行 + 分组标题 + 右侧功能区 + 按位置圆角） |
| [segmented-controls.md](segmented-controls.md) | [segmented-controls-code.md](segmented-controls-code.md) | SegmentedControls 分段按钮（2-5 选项 × 灰/白/彩背景） |

### 开发工具

| 代码 | 内容 |
|------|------|
| [debug-tool-code.md](debug-tool-code.md) | DebugTool 页面调试工具（FAB + 抽屉 + List/Switch/PopupMenu 控件面板，内置暗色模式，新页面必装） |

### 预览

[设计组件库.html](设计组件库.html) — 所有已实现组件的交互预览，支持明暗/主色切换
