---
category: components
name: 按钮
description: 按钮组件总目录 — 场景描述、规格、调用方式。代码实现见 button-code.md
---

# 按钮

代码：[button-code.md](button-code.md)
图标：[icons.md](icons.md)

---

## 按钮选型指引

| 关键词 | 需确认 | 说明 |
|--------|--------|------|
| 下载、安装、更新 | BaseButton vs InstallButton | 有**连接→下载进度→安装**完整流程用 InstallButton；一次性操作用 BaseButton |
| 预约 | BaseButton vs ReservationButton | 需要切换预约/已预约状态用 ReservationButton；否则用 BaseButton |
| 播放 | BaseButton vs SpeedUp / PlayButton | 游戏加速用 SpeedUp，视频播放控件用 PlayButton，普通操作用 BaseButton |

> **规则：当需求未明确点击后的交互流程时，必须先询问用户确认按钮类型。**

---

## 主色规则

| 场景 | 主色 | 说明 |
|------|------|------|
| 默认（全局） | 绿色 | 应用商店通用 |
| 社区模块页面 | 黄色 | 在 `<body>` 加 `theme-community` class 切换 |

社区主色是浅色，文字色由 CSS 自动处理（`.theme-community` + `.theme-light` / `.theme-dark`），无需手动指定。

---

## BaseButton — 基础按钮

通用操作按钮。

### 尺寸选择

| 尺寸 | 适用场景 |
|------|---------|
| Large | 全宽 CTA（弹窗底部、页面底部操作栏） |
| Medium | 行内操作（胶囊形） |
| Small | 紧凑区域（列表行内、卡片角落） |
| Mini | 极小场景 |

### 类型选择

| 类型 | 适用场景 |
|------|---------|
| Primary（强调） | 页面主操作、最重要的按钮 |
| Secondary（基础） | 次要操作、取消 |
| Destructive（警示） | 危险操作（删除、卸载） |

### 状态

| 状态 | 触发条件 |
|------|---------|
| Enabled | 默认可点击 |
| Pressed | 手指按下 |
| Disabled | 条件不满足，不可点击 |

### 按钮组合规则

| 布局 | 说明 |
|------|------|
| 上下两个 | Primary 在上，Secondary 在下 |
| 左右两个 | Secondary 在左，Primary 在右 |
| 左右（警示） | Secondary 在左，Destructive 在右 |

### 调用方式

```js
new BaseButton('#container', {
  size: 'large',      // large | medium | small | mini
  type: 'primary',    // primary | secondary | destructive
  state: 'enabled',   // enabled | pressed | disabled
  text: '按钮文字'
});
```

---

## CustomButton — 定制按钮

| 组件 | 场景 | 何时使用 |
|------|------|----------|
| PostButton | 社区模块 | 发布内容或组队邀请 |
| SpeedUp | 游戏模块 | 启动游戏加速 |
| PlayButton | 视频播放 | 视频容器**居中**位置，控制播放/暂停 |
| VideoControls | 视频播放 | 视频容器**右下角**，播放/音量/全屏控制 |
| ReactButton | 内容反馈 | 点赞/踩切换（线框↔实心） |
| SoundButton | 视频/媒体 | 玻璃材质圆形声音开关 |
| FullscreenButton | 视频/媒体 | 玻璃材质圆形全屏开关 |
| ReservationButton | 应用预约 | 切换预约/已预约状态 |

### ReactButton

点赞/踩切换按钮。Default（线框）↔ Selected（实心），颜色自动切换。

### SoundButton / FullscreenButton

使用 `.glass-btn` 通用玻璃材质，点击切换 On/Off 状态。图标 SVG 见 [icons.md](icons.md)「媒体控制图标」。

### ReservationButton

| 状态 | 行为 |
|------|------|
| 未预约 | 浅色底 + 铃铛图标 |
| 已预约 | 实色底 + 勾选图标 |

---

## InstallButton — 下载安装按钮

覆盖应用从安装到打开的完整生命周期。

### 尺寸选择

| 尺寸 | 适用场景 |
|------|---------|
| Large | 应用详情页底部（全宽） |
| Medium | 应用详情页内容区（全宽） |
| Small | 列表/卡片中（固定宽度，文字） |
| Icon | 列表/卡片中（固定宽度，图标） |

### 状态流转

```
enabled → connecting → downloading ⇄ resume → installing → open → update
```

| 状态 | 说明 |
|------|------|
| enabled | 默认，可点击安装 |
| connecting | 连接中，Loading 动画 |
| downloading | 下载中，带进度条和百分比 |
| resume | 暂停后继续 |
| installing | 安装中，Loading 动画 |
| open | 安装完成，可打开 |
| update | 有更新可用 |
| disabled | 禁用 |

### 调用方式

```js
const btn = new InstallButton('#container', {
  size: 'large',     // large | medium | small | icon
  state: 'enabled',  // 9 种状态
  progress: 0        // 下载进度 0-100
});

btn.setState('downloading', 50);
btn.setState('open');
btn.setProgress(75);
```

---

## 通用玻璃材质按钮 (.glass-btn)

用于浮起态标题栏图标、SoundButton、FullscreenButton 等需要半透明背景的圆形按钮。

何时使用：标题栏浮起态、视频叠加层等需要「透过背景」效果的场景。

> 所有精确样式值（尺寸、间距、颜色、材质参数）见 [button-code.md](button-code.md)
