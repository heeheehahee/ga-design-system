---
category: components
name: 按钮
description: 按钮组件总目录 — 场景描述、规格、调用方式。代码实现见 button-code.md
---

# 按钮

预览：[preview.html](../preview.html)
代码：[button-code.md](button-code.md) — 需要实现按钮时，到此文件中取 CSS + JS 代码

### 按钮选型指引

遇到以下关键词时，需确认使用哪种按钮：

| 关键词 | 需确认 | 说明 |
|--------|--------|------|
| 下载、安装、更新 | BaseButton vs InstallButton | 如果点击后有**连接→下载进度→安装**的完整流程，用 InstallButton；如果只是一次性操作（如弹窗确认下载），用 BaseButton |
| 预约 | BaseButton vs ReservationButton | 如果需要切换预约/已预约状态，用 ReservationButton；否则用 BaseButton |
| 播放 | BaseButton vs SpeedUp / PlayButton | 如果是游戏加速入口用 SpeedUp，视频播放控件用 PlayButton，普通操作用 BaseButton |

> **规则：当需求未明确点击后的交互流程时，必须先询问用户确认按钮类型，再实现。**

### 主色规则

本规范用于 **Getapps** 设计。按钮主色根据页面场景切换：

| 场景 | 主色 | Token | 说明 |
|------|------|-------|------|
| 默认（全局） | `#05C474` | `color.accent.primary` | 应用商店通用主色 |
| 社区模块页面 | `#FFE249` | `color.accent.community` | 社区相关页面的按钮主色 |

实现时通过 CSS 变量切换主色，并在 `<body>` 添加 `theme-community` class，明暗模式用 `theme-light` class。

**社区主色文字色规则（按底色类型 × 明暗模式）：**

`#FFE249` 是浅色，文字色不能一刀切，需按底色和明暗分别处理。

**1. 实色主色底 → 始终黑字（不分明暗）**

| 组件 | 影响状态 |
|------|---------|
| BaseButton Primary | 所有尺寸所有状态 |
| InstallButton Large/Medium | enabled / pressed / installing / open / update |
| InstallButton Small/Icon | installing / open |
| ReservationButton 已预约 | 实色底 |
| 下载进度条重叠部分 | 裁切层文字 |

**2. 浅色底 / 灰底 → 暗色用主题黄色，亮色用黑色**

| 组件 | 影响状态 | 暗色文字 | 亮色文字 |
|------|---------|---------|---------|
| BaseButton Small Secondary | 浅色底 | 黄色 | 黑色 |
| InstallButton Large/Medium | connecting / downloading / resume | 黄色 | 黑色 |
| InstallButton Small/Icon | enabled / pressed / connecting / downloading / resume / update | 黄色 | 黑色 |
| InstallButton 下载进度未重叠文字 | 底层文字 | 黄色 | 黑色 |
| ReservationButton 未预约 | 浅色底 | 黄色 | 黑色 |

**3. 亮色下浅色底透明度调整**

| 默认 | 社区亮色 | 说明 |
|------|---------|------|
| 10% | 30% | 黄色 10% 在白底上太淡 |
| 16%（pressed） | 38% | 按下态相应提升 |

---

## BaseButton — 基础按钮

通用操作按钮，3 种类型 × 3 种状态 × 4 种尺寸。
来源：Figma ga_component / 基础按钮 (node 142:10652)

### 尺寸

| 尺寸 | 名称 | 宽度 | 高度 | 圆角 | 内边距 | 字号/字重 |
|------|------|------|------|------|--------|-----------|
| Large | 大 | 填充容器 (100%) | 50 | 16 `{radius.common}` | 8/20 | 17 / 380 |
| Medium | 中 | 自适应内容 | 36 | 999 `{radius.circle}` | 8/20 | 14 / 380 |
| Small | 小 (GA) | 自适应内容 | 32 | 999 `{radius.circle}` | 0/12 | 14 / 500 |
| Mini | mini | 自适应内容 | 28 | 999 `{radius.circle}` | 4/12 | 13 / 330 |

### 类型 × 状态

**Primary（强调）**

| 状态 | 背景 | 文字色 |
|------|------|--------|
| Enabled | `accent.primary` | `on_primary` |
| Pressed | `accent.primary` + `surface_touch` 蒙层 | `on_primary` |
| Disabled | `accent.primary` | `on_surface_button_disable` |

**Secondary（基础）**

| 状态 | 背景 | 文字色 |
|------|------|--------|
| Enabled | `tertiary` | `on_surface_secondary` |
| Pressed | `tertiary` + `container_list_touch` 蒙层 | `on_surface_secondary` |
| Disabled | `surface_container` | `on_surface_quaternary` |

**Destructive（警示）**

| 状态 | 背景 | 文字色 |
|------|------|--------|
| Enabled | `tertiary` | `error` |
| Pressed | `tertiary` + `container_list_touch` 蒙层 | `error` |
| Disabled | `surface_container` | `error_disable` |

> Small 尺寸仅有 Primary 和 Secondary 两种类型，Secondary 底色为 `#05C474` 10%，Pressed 为 16% + 黑色 10% 蒙层，Disabled 底色为黑色 5%。

### 按钮组合

| 布局 | 说明 |
|------|------|
| 上下两个 | Primary 在上，Secondary 在下，间距 12 |
| 左右两个 | Secondary 在左，Primary 在右，各占 50%，间距 12 |
| 左右（警示） | Secondary 在左，Destructive 在右 |

### 调用方式

```js
const btn = new BaseButton('#container', {
  size: 'large',      // large | medium | small | mini
  type: 'primary',    // primary | secondary | destructive
  state: 'enabled',   // enabled | pressed | disabled
  text: '强调按钮'
});

btn.setState('disabled');
btn.onClick = (instance) => { ... };
```

---

## CustomButton — 个性化场景定制按钮

来源：Figma ga_component / 个性化场景定制按钮 (node 153:12131)

> 以下按钮用于特定业务场景，非通用按钮。使用时根据场景选择对应组件。

### 使用场景速查

| 组件 | 场景 | 何时使用 |
|------|------|----------|
| PostButton | 社区模块 | 需要发布内容或组队邀请时 |
| SpeedUp | 游戏模块 | 需要启动游戏加速功能时 |
| PlayButton | 视频播放 | 放在视频容器的**水平/垂直居中**位置，控制播放暂停 |
| VideoControls | 视频播放 | 放在视频容器的**右下角**位置，提供暂停/音量/全屏等控制 |
| ReactButton | 内容反馈 | 点赞/踩切换按钮，线框↔实心 |
| SoundButton | 视频/媒体 | 玻璃材质圆形声音开关，切换静音/有声 |
| FullscreenButton | 视频/媒体 | 玻璃材质圆形全屏开关，切换全屏/退出 |
| ReservationButton | 应用预约 | 应用未上线时提供预约提醒入口 |

### PostButton（社区组队按钮）

用于社区模块的发布/组队类操作入口，带 IP 形象装饰。

| 属性 | 值 |
|------|------|
| 整体尺寸 | 128 × 65 |
| 按钮区域 | 128 × 46，r=28 |
| 背景 | `rgba(0,0,0,0.8)` + 黑色 1px 描边 |
| Pressed | 底色 + 黑色 10% 蒙层 |
| 文字 | "Team Up" fs=16 fw=450，黑色 |
| 图标 | "+" fs=15 fw=500，黑色 |
| 特殊元素 | 顶部有 IP 形象图标（42×33），溢出按钮区域 |

### SpeedUp（游戏加速启动按钮）

用于游戏模块，启动加速功能的入口按钮。

| 属性 | 值 |
|------|------|
| 尺寸 | 72 × 36，r=999（胶囊） |
| 背景 | `#FFE249` |
| Pressed | 底色 + 黑色 10% 蒙层 |
| Disabled | 同底色，降低对比度 |
| 文字 | "Play" fs=14 fw=450，黑色 |
| 图标 | 闪电图标 12×18，黑色，在文字左侧 |
| 内边距 | 9/12/9/12 |

### PlayButton（视频播放/暂停按钮）

视频控件，放在视频容器的**水平/垂直居中位置**，控制播放和暂停。

| 属性 | 值 |
|------|------|
| 尺寸 | 40 × 40 |
| 状态 | Play（播放三角）/ Pause（暂停双竖线），点击切换 |
| 图标填充 | 白色线性渐变（顶部 #FFF 100% → 底部 #FFF 90%） |
| 背景 | 透明（叠加在视频画面上） |
| 投影 | `0px 3px 12px rgba(0,0,0,0.1)` |
| 布局位置 | 视频容器内，水平垂直居中 |

### VideoControls（视频控制栏按钮）

视频控件，放在视频容器的**右下角位置**，提供播放、音量、屏幕方向等控制。

| 属性 | 值 |
|------|------|
| 单个尺寸 | 38 × 28，r=8 |
| 背景 | `rgba(0,0,0,0.8)` |
| Pressed | 底色 + 黑色 10% 蒙层 |
| 图标尺寸 | 26 × 26，白色 |
| 内边距 | 1/6/1/6 |
| 布局位置 | 视频容器内，右下角 |

**7 种控制图标：** Pause / Play / Mute / Voice / Full screen / Landscape / Vertical

### ReactButton（点赞/踩按钮）

内容反馈切换按钮，点击在 Default（线框）↔ Selected（实心）之间切换。来源：Figma node 241:4755。

| 属性 | 值 |
|------|------|
| 尺寸 | 26 × 26 |
| 图标尺寸 | ~20 × 20，`fill="currentColor"` |
| Default 色 | `on_surface_quaternary` |
| Selected 色 | `on_surface_secondary` |
| 类型 | Like（点赞）/ Dislike（踩） |
| 状态 | Default（线框）/ Selected（实心） |
| 背景 | 透明，无背景材质 |

图标 SVG 见 [icons.md](icons.md)「反馈图标」。

### SoundButton（声音开关按钮）

玻璃材质圆形切换按钮，控制静音/有声。来源：Figma node 238:4746。

| 属性 | 值 |
|------|------|
| 尺寸 | 32 × 32，r=99（全圆） |
| 材质 | `.glass-btn`（通用玻璃材质，代码见 button-code.md） |
| 内边距 | 1/10/1/10 |
| 图标尺寸 | 20 × 20，白色，`fill="currentColor"` |
| 状态 | Off（静音）/ On（有声），点击切换 |

图标 SVG 见 [icons.md](icons.md)「媒体控制图标」。

### FullscreenButton（全屏开关按钮）

玻璃材质圆形切换按钮，控制全屏/退出全屏。来源：Figma node 238:4746。

| 属性 | 值 |
|------|------|
| 尺寸 | 32 × 32，r=99（全圆） |
| 材质 | `.glass-btn`（通用玻璃材质，代码见 button-code.md） |
| 内边距 | 1/10/1/10 |
| 图标尺寸 | 20 × 20，白色，`fill="currentColor"` |
| 状态 | On（已全屏）/ Off（非全屏），点击切换 |

图标 SVG 见 [icons.md](icons.md)「媒体控制图标」。

### ReservationButton（应用预约按钮）

用于应用预约场景，点击切换预约/已预约状态。

| 属性 | 值 |
|------|------|
| 尺寸 | 54 × 28，r=999（胶囊） |
| 内边距 | 0/8/0/8 |
| 图标尺寸 | 20 × 20 |

| 状态 | 背景 | 图标 |
|------|------|------|
| 未预约 Enabled | `#05C474` 10% | 铃铛图标，绿色 |
| 未预约 Pressed | `#05C474` 16% + 黑色 10% 蒙层 | 铃铛图标，绿色 |
| 已预约 Enabled | `#05C474` 实色 | 勾选图标，白色 |
| 已预约 Pressed | `#05C474` + 黑色 10% 蒙层 | 勾选图标，白色 |

---

## InstallButton — 应用下载安装/更新按钮

主题色 `#05C474`，覆盖应用从安装到打开的完整生命周期。
来源：Figma ga_component / InstallButton_ComponentSet (node 144:11718)

### 尺寸

| 尺寸 | 宽度 | 高度 | 圆角 | 内边距 | 字号/字重 | 内容 |
|------|------|------|------|--------|-----------|------|
| Large | 填充容器 (100%) | 50 | 16 `{radius.common}` | 8/20/8/20 | 17 / 380 | 文字 |
| Medium | 填充容器 (100%) | 44 | 16 `{radius.common}` | 8/20/8/20 | 16 / 380 | 文字 |
| Small | 固定 54 | 28 | 999 `{radius.circle}` | 0/8/0/8 | 14 / 500 | 文字 |
| Icon | 固定 54 | 28 | 999 `{radius.circle}` | 0/8/0/8 | 14 / 500 | 图标 20×20 |

> 同一尺寸的按钮在所有状态下宽度保持一致，不随内容变化。Large/Medium 填充父容器宽度，Small/Icon 为固定 54px。

### 状态

| 状态 | 说明 |
|------|------|
| enabled | 默认，可点击安装 |
| pressed | 按下态（主色 + 黑色 10% 蒙层） |
| connecting | 连接中，显示 Loading 动画 |
| downloading | 下载中，带进度条和百分比 |
| resume | 暂停后继续，带进度条 |
| installing | 安装中，显示 Loading 动画 |
| open | 安装完成，可打开 |
| update | 有更新可用 |
| disabled | 禁用 |

状态流转：`enabled → connecting → downloading ⇄ resume → installing → open → update`

### 调用方式

```js
const btn = new InstallButton('#container', {
  size: 'large',     // large | medium | small | icon
  state: 'enabled',  // 9 种状态
  progress: 0        // 下载进度 0-100
});

btn.setState('downloading', 50);
btn.setState('installing');
btn.setState('open');
btn.setProgress(75);
btn.onClick = (instance) => { ... };
```
