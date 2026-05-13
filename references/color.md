---
category: foundations
tokens: [color.text, color.bg, color.accent, color.state, color.border, color.palette, color.gradient]
description: 颜色体系 — 基于 HyperOS3 Feature Token，含 light/dark 双模式
source: Figma ga_component (node 331:11036) 色彩 Color
---

# 颜色体系

基于 HyperOS3 Feature Token，所有颜色均包含 light / dark 两套值。
Getapps 主色：`#05C474`（默认）/ `#FFE249`（社区模块），详见 [button.md 主色规则](button.md)。

### 代码引用规则

Token 名（如 `on_surface`）在代码中的引用方式：

| 技术栈 | 引用方式 | 示例 |
|--------|---------|------|
| H5 (CSS) | `var(--Token名)` | `color: var(--on_surface)` |
| Android | `R.color.Token名` | `colorResource(R.color.on_surface)` |

> 禁止 hardcode hex 值。所有颜色必须通过变量/资源引用。

---

## 文字图标色（共 9 个）

| Token | Light | Dark | 用途 |
|-------|-------|------|------|
| `on_surface` | `#000000` | `#fffffff2` | 主要文字 |
| `on_surface_secondary` | `#000000cc` | `#ffffffcc` | 次要文字 |
| `on_surface_tertiary` | `#00000099` | `#ffffff80` | 三级文字 |
| `on_surface_quaternary` | `#00000066` | `#ffffff80` | 四级文字 |
| `on_surface_octonary` | `#0000004c` | `#ffffff4c` | 最弱文字 |
| `on_surface_right_icon` | `#0000004c` | `#ffffff66` | 右侧图标 |
| `on_surface_right_button_bg` | `#0000000f` | `#ffffff33` | 右侧按钮背景 |
| `on_surface_button_disable` | `#ffffff` | `#ffffff4c` | 按钮禁用文字 |
| `on_surface_empty_state` | `#00000033` | `#ffffff33` | 空状态文字 |

## 系统主色

> **这是 HyperOS 手机操作系统的系统级主色（蓝色），由 OS 全局控制，非 Getapps 应用主色。** 用于系统控件（开关、进度条、选中态等）和系统级交互元素。Getapps 应用自身的品牌主色见下方「Getapps 应用主色」。

| Token | Light | Dark | 用途 |
|-------|-------|------|------|
| `primary` | `#3482ff` | `#4788ff` | OS 系统主色（HyperOS 蓝） |
| `on_primary` | `#ffffff` | `#ffffffe6` | 主色背景上的文字 |
| `secondary` | `#3482ff14` | `#4788ff40` | OS 次要色（主色低透明度） |
| `on_secondary` | `#3482ff` | `#4788ff` | 次要色背景上的文字 |
| `tertiary` | `#0000000f` | `#ffffff24` | OS 三级色 |
| `on_tertiary` | `#000000cc` | `#ffffffcc` | 三级色背景上的文字 |
| `disable_primary` | `#3482ff4c` | `#4788ff4c` | 主色禁用态（非标准 Token，Figma 标注） |

### Getapps 应用主色

| Token | 值 | 用途 |
|-------|------|------|
| `color.accent.primary` | `#05C474` | Getapps 默认主色 |
| `color.accent.community` | `#FFE249` | 社区模块主色 |

## 系统功能色（共 6 个）

| Token | Light | Dark | 用途 |
|-------|-------|------|------|
| `caution` | `#ff9f05` | `#ffa30f` | 警告 |
| `caution_container` | `#ff9f051a` | `#ffa30f38` | 警告容器背景 |
| `error` | `#fa382e` | `#fa4238` | 错误/危险 |
| `error_container` | `#fa382e14` | `#fa423838` | 错误容器背景 |
| `success` | `#1dcd3a` | `#28d244` | 成功 |
| `success_container` | `#1dcd3a1a` | `#28d24438` | 成功容器背景 |

## 线框插图色（共 4 个）

| Token | Light | Dark | 用途 |
|-------|-------|------|------|
| `container_illustration_quaternary` | `#e9eaf0` | `#333333` | 插图四级色 |
| `container_illustration_tertiary` | `#dfe1eb` | `#424242` | 插图三级色 |
| `container_illustration_secondary` | `#ced2e0` | `#565656` | 插图次要色 |
| `container_illustration_primary` | `#bec3d8` | `#666666` | 插图主色 |

## 背景色（共 7 个）

| Token | Light | Dark | 用途 |
|-------|-------|------|------|
| `surface` | `#ffffff` | `#000000` | 页面主背景 |
| `surface_low` | `#f7f7f7` | `#000000` | 低层级背景 |
| `surface_medium` | `#ffffff` | `#242424` | 中层级背景 |
| `surface_high` | `#ffffff` | `#242424` | 高层级背景 |
| `surface_highest` | `#ffffff` | `#2c2c2c` | 最高层级背景 |
| `surface_pop_window` | `#f7f7f7` | `#242424` | 弹窗背景 |
| `surface_menubar` | `#ffffff` | `#666666` | 菜单栏背景 |

## 元素色（共 8 个）

| Token | Light | Dark | 用途 |
|-------|-------|------|------|
| `surface_container` | `#0000000a` | `#ffffff14` | 通用容器 |
| `surface_container_low` | `#0000000f` | `#ffffff24` | 低层级容器 |
| `surface_container_medium` | `#00000014` | `#ffffff24` | 中层级容器 |
| `surface_container_high` | `#0000001a` | `#ffffff33` | 高层级容器 |
| `container_list` | `#ffffff` | `#ffffff24` | 列表项 |
| `container_input` | `#ffffff` | `#ffffff24` | 输入框 |
| `container_list_solid` | `#ffffff` | `#4c4c4c` | 列表项（实色） |
| `container_notice_bar` | `#0000000a` | `#ffffff24` | 通知栏 |

## 系统彩色色板（共 18 个）

| Token | Light | Dark |
|-------|-------|------|
| `red` | `#fa382e` | `#fa4238` |
| `deep_orange` | `#ff612e` | `#ff6a05` |
| `orange` | `#ff6900` | `#f97a1f` |
| `apricot_orange` | `#ff9f05` | `#ffa30f` |
| `yellow` | `#ffbb0f` | `#ffbf0f` |
| `lemon_yellow` | `#ffd000` | `#ffdd33` |
| `chartreuse` | `#94e203` | `#9fe421` |
| `green` | `#1dcd3a` | `#28d244` |
| `turquoise` | `#0cce94` | `#11d097` |
| `cyan` | `#0fcaca` | `#13cdcd` |
| `sky_blue` | `#29a9ff` | `#36acfc` |
| `blue` | `#3482ff` | `#4788ff` |
| `purple` | `#7767f9` | `#8370ff` |
| `violet` | `#b74af7` | `#bd57fa` |
| `magenta` | `#e74095` | `#ee4f9f` |
| `gray` | `#8c9db0` | `#92a1b4` |
| `white` | `#ffffff` | `#ffffff` |
| `black` | `#000000` | `#000000` |

## 应用浅色色板

18 色中 9 色有浅色变体，其余标注「无」。浅色 = 基色 80% 不透明度（Light 叠在白底上，Dark 直接降低透明度）。

**有浅色变体的 9 色：** Red / Apricot Orange / Lemon Yellow / Chartreuse / Green / Cyan / Blue / Purple / Gray

**无浅色变体的 9 色：** Deep Orange / Orange / Yellow / Turquoise / Sky Blue / Violet / Magenta / White / Black

**CSS 实现方式：**

```css
/* 需要 RGB 辅助变量 */
--red-rgb: 250, 56, 46;           /* Light */
--apricot_orange-rgb: 255, 159, 5;
--lemon_yellow-rgb: 255, 208, 0;
--chartreuse-rgb: 148, 226, 3;
--green-rgb: 29, 205, 58;
--cyan-rgb: 15, 202, 202;
--blue-rgb: 52, 130, 255;
--purple-rgb: 119, 103, 249;
--gray-rgb: 140, 157, 176;

/* 使用时 */
background: rgba(var(--red-rgb), 0.8);  /* 浅色 80% */
background: rgba(var(--red-rgb), 0.4);  /* 浅色 40% */
```

> Dark 模式使用 Dark 色板值的对应 RGB 变量。

## 遮罩色（共 2 个）

| Token | Light | Dark | 用途 |
|-------|-------|------|------|
| `mask` | `#00000033` | `#00000099` | 页面遮罩 |
| `mask_menu` | `#00000033` | `#00000066` | 菜单遮罩 |

## 分割线色（共 1 个）

| Token | Light | Dark | 用途 |
|-------|-------|------|------|
| `outline` | `#0000001a` | `#ffffff33` | 分割线/描边 |

## 应用渐变色色板（共 8 个）

标注「无」表示该色无渐变变体。

| Token | 方向 | 色值 |
|-------|------|------|
| `gradient_red` | 180deg | `#FF6F5F` → `#FA382E` |
| `gradient_orange` | 180deg | `#FF933B` → `#FF6900` 70% → `#FF5E1E` |
| `gradient_yellow` | 180deg | `#FFD230` → `#FFC30F` 40% → `#FFA41B` |
| `gradient_green` | 180deg | `#46F35A` 10% → `#1DCD3A` 65% → `#00BA00` 90% |
| `gradient_cyan` | 180deg | `#17E9DB` → `#0FCACA` 65% → `#0CB9C3` |
| `gradient_blue` | 180deg | `#00AEFF` → `#3381FF` 65% → `#006AFF` |
| `gradient_purple` | 0deg | `#6553FC` → `#6D5CFE` 30% → `#A27DFF` |
| `gradient_gray` | 180deg | `#ABBACC` → `#8C9DB0` |
