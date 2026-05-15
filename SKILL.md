---
name: ga-gh-design
description: Getapps 高保真原型设计系统 — 完整的移动端界面开发 skill。所有界面代码必须遵循此设计系统：有组件则调用组件库代码，无组件则使用基础 Token 自建。覆盖组件调用、Token 规范、暗色适配、切图交付全流程。当用户要做页面、还原设计稿、写移动端 UI、调用组件、用 Figma 生成代码时触发。
---

# Getapps 设计系统

**职责：做高保真原型页面。** 从设计稿或需求描述出发，产出可运行的移动端页面。不负责制作/更新组件规范（那是 `/spec-maker` 的事）。

所有文件位于 `~/.claude/commands/ga-gh-design/references/`。

---

## 一、设计哲学

Getapps 是小米应用商店海外版，基于 HyperOS 3 设计语言。

| 特征 | 实际指导 |
|------|---------|
| 三色体系 | 绿色 #05C474（默认）、黄色 #FFE249（社区）、蓝色 #3482FF（OS 系统控件），通过 CSS 变量切换 |
| 移动端优先 | 无 :hover 只有 :active，触控区最小 44px，392px 基准宽度 |
| Token 驱动 | 所有颜色/字号/圆角/间距/阴影由 Token 定义，禁止 hardcode，明暗自动切换 |

---

## 二、核心铁律

以下规则在任何情况下都不可违反。

### 组件调用规则

| 情况 | 做法 |
|------|------|
| 有现成组件 | 先读决策指南（组件名.md），再从 code MD 复制代码。**两个文件必须都读**，禁止跳过决策指南直接取代码 |
| 没有现成组件 | 用基础 Token 自建（color.md / typography.md / border-radius.md / elevation.md） |
| 不确定有没有 | 先查下方「快速查找」表 |
| 组件库无此代码 | 标注 `/* 组件库无此代码 */` 并告知用户 |

> **前置条件：** 写任何代码前，如果当前上下文中主题色未明确（用户没说、Figma 没给、现有项目没有配置），**必须先询问用户确认主题色**，然后再调用组件或自建。

### 写代码前必须检索

每写一个属性都要检索对应规范，不能凭印象：

| 要写什么 | 检索哪里 | 找到 → | 没找到 → |
|---------|---------|--------|---------|
| 组件 | `preview.html` → `*-code.md` | 直接复制 | 标注"组件库无此代码"并告知用户 |
| 颜色 | `color.md` → `:root` 变量 | 用 `var(--token)` | 不可 hardcode，先确认是否需要新增 Token |
| 图标 | `icons.md` → JS 常量 | 复制 SVG | 从 Figma 导出，`fill="currentColor"` |
| 字号/字重 | `typography.md` | 用规范值 | 不可自创，先和设计确认 |
| 圆角 | `border-radius.md` | 用 8 级体系值 | 不可自创 |
| 间距 | `_index.md`「间距规范」 | 用 4px 网格值 | 不允许 2/3/5/6/7/9 等非网格值 |

### 禁止行为（Anti-patterns）

| 禁止 | 为什么 | 正确做法 |
|------|--------|---------|
| hardcode 颜色（`#fff`、`rgba`） | 暗色模式失效，主题切换失败 | `var(--token名)` |
| 自写组件代码 | 与组件库不一致，无法同步更新 | 从 code MD 复制 |
| 非 4px 间距（2/3/5/7/9px） | 破坏视觉网格，与其他元素对不齐 | 用 4 的倍数 |
| 写 `:hover` | 移动端无鼠标，永远不会触发 | 用 `:active` |
| 自制图标 | 与设计稿不一致，风格不统一 | 从 icons.md 取或 Figma 导出 |
| inline style | 无法被主题覆盖，暗色模式失效 | 写 CSS class |
| hardcode 字体名 | 字体通过全局样式统一定义 | 不写 font-family |
| 绝对定位布局 | 布局混乱，无法适配 | 用 flex + padding |

---

## 三、工作流

### 操作模式

| 模式 | 触发场景 | 入口 |
|------|---------|------|
| 从 Figma 还原 | 用户提供 Figma URL 或截图 | Phase 1 从 Figma 分析开始 |
| 从描述做页面 | 用户描述想要的页面/功能 | Phase 1 从需求确认开始 |
| 迭代现有页面 | 用户说"继续"/"改一下"/"加个功能" | 直接打开项目，定位修改点 |

### Phase 1: 分析

目标：搞清楚要做什么、用什么。

**从 Figma 还原时：**
1. 从 URL 提取 fileKey + nodeId
2. 调用 Figma API 获取设计数据
3. 识别页面结构：
   - 标题栏类型？（大/中/小/可点击 → 查 navbar.md）
   - 有底 Tab 栏？（→ 查 navbar-code.md TabBar）
   - 有弹窗/抽屉/Toast？（→ 查对应组件）
   - 有列表/输入框/按钮？（→ 查对应组件）
4. 提取颜色值 → 逐个匹配 color.md Token
5. 输出分析结果：「组件清单 + Token 清单 + 需自建的部分」

**从描述做页面时：**
1. 确认技术栈：H5（默认）/ Android
2. 确认主题色：绿色（GA 默认）/ 黄色（社区）/ 蓝色（OS 系统控件）
3. 确认明暗模式：亮色（默认）/ 暗色
4. 分析需求描述，列出需要的组件
5. 不确定的地方主动询问用户

### Phase 2: 搭建

目标：把页面写出来。

**项目初始化：**
1. 从 `page-template-code.md` 复制页面骨架（HTML + CSS + JS）
2. 安装 `debug-tool-code.md`（新页面必装）
3. 配置 `:root` 颜色变量（从 color.md 复制需要的 Token）

**组件调用（有组件时）：**
1. 读决策指南（如 `navbar.md`）→ 确定用哪个变体
2. 从 code MD（如 `navbar-code.md`）复制 HTML + CSS + JS
3. 填入具体内容：
   - 文字：用户提供的文案，原文照搬
   - 图标：从 `icons.md` 取 SVG，`fill="currentColor"`
   - 参数：按决策指南传入

**Token 自建（无组件时）：**
1. 布局：flex/grid，间距用 4px 网格（查 `_index.md` 间距表）
2. 颜色：查 `color.md`，用 `var(--token)`
3. 字号/字重：查 `typography.md`，用规范值
4. 圆角：查 `border-radius.md`，用 8 级体系
5. 阴影：查 `elevation.md`，用 4 级梯度
6. 交互：只写 `:active`，不写 `:hover`

**暗色模式：**
1. 所有颜色 Token 已在 `:root` 定义 light/dark 双值
2. 检查是否需要 `--token-rgb` 变体（用于 rgba 渐变）
3. 用 debug-tool 切换明暗验证

### Phase 3: 检查

目标：确保代码合规。

| 检查项 | 方法 | 不通过怎么办 |
|--------|------|-------------|
| 无 hardcode 颜色 | grep `#[0-9a-f]` / `rgba(` | 替换为 `var(--token)` |
| 间距合规 | grep 所有 px 值，确认为 4 的倍数 | 调整为最近的 4px 倍数 |
| 无 :hover | grep `:hover` | 改为 `:active` 或删除 |
| 图标规范 | grep `fill=`，确认为 currentColor | 修改 fill 值 |
| 无 inline style | grep `style="` | 改为 CSS class |
| 明暗适配 | 浏览器切换 dark mode | 检查哪些 Token 缺 dark 值 |
| 组件一致 | 对比 code MD 原文 | 回到 code MD 重新复制 |

### Phase 4: 交付

目标：产出可用的页面文件。

1. **预览**：
   - 启动本地服务器（端口 9000）
   - 浏览器打开页面
   - 生成二维码，提供手机预览链接
2. **命名检查**：
   - 文件名/class 名符合命名规则（见 `_index.md` 第六章）
3. **切图导出**（如需）：
   - 按 `_index.md` 切图规范执行
   - 从 Figma 按画框切、WEBP 3x、模块前缀命名
4. **代码整理**：
   - 删除调试代码
   - CSS 按组件分块
   - JS 逻辑清晰

---

## 四、快速查找

### 基础 Token

| 需要什么 | 查哪个文件 |
|---------|-----------|
| 详细规范（间距/命名/暗色/切图） | `_index.md` |
| 颜色 Token | `color.md` — 66 个 Token，`var(--Token名)` |
| 字体 | `typography.md` — 15 个 Text Style |
| 圆角 | `border-radius.md` — 8 级梯度 |
| 阴影 | `elevation.md` — 4 级投影 |

### 组件（决策 → 代码）

| 组件 | 决策指南 | 代码（复制用） |
|------|---------|--------------|
| 按钮 | `button.md` | `button-code.md` |
| 标题栏 / Tab 栏 / 状态栏 | `navbar.md` | `navbar-code.md` |
| 弹窗 | `dialog.md` | `dialog-code.md` |
| 输入框 | `input.md` | `input-code.md` |
| 列表 | `list.md` | `list-code.md` |
| 抽屉 | `drawer.md` | `drawer-code.md` |
| 分段按钮 | `segmented-controls.md` | `segmented-controls-code.md` |
| Checkbox | `checkbox.md` | `checkbox-code.md` |
| Switch 开关 | `switch.md` | `switch-code.md` |
| Toast 轻提示 | `toast.md` | `toast-code.md` |
| NoticeBar 信息提示 | `noticebar.md` | `noticebar-code.md` |
| Loading 加载 | `loading.md` | `loading-code.md` |
| Tooltip 提示气泡 | `tooltip.md` | `tooltip-code.md` |
| Chips 标签栏 | — | `chips-code.md` |
| 自定义滚动条 | — | `scrollbar-code.md` |
| 骨架占位块 | — | `skeleton-code.md` |
| 星级评分 | — | `star-rating-code.md` |
| 二级文字标签 | — | `underline-tabbar-code.md` |
| 二级胶囊标签 | — | `sub-chips-code.md` |
| PopupMenu 近手菜单 | — | `popup-menu-code.md` |
| 评分分布条 | — | `rating-bars-code.md` |
| 内容卡片集 | — | `content-cards-code.md` |
| 底部操作栏 | — | `bottom-action-bar-code.md` |

### 资源 & 工具

| 需要什么 | 查哪个文件 |
|---------|-----------|
| 图标 SVG | `icons.md` |
| 页面调试工具 | `debug-tool-code.md` — 新页面必装 |
| 页面骨架模板 | `page-template-code.md` |
| 组件预览（人看） | `design-system-preview.html` |

---

## 五、启动选项

运行 `/ga-gh-design` 时，先询问用户：

### 1. 创建新项目
- `cp -r references/page-template/ ~/Documents/claude/output/项目名/`
- 浏览器打开页面
- 在 `~/Documents/claude/output/` 启动本地服务器（端口 9000）
- 生成二维码（`qrcode.png`）放进项目目录
- 提供手机预览链接：`http://本机IP:9000/项目名/index.html`
- 询问下一步：**提供 Figma URL** / **直接描述需求** / **自行开始**
- 如果用户的回复中未明确主题色，再确认：**绿色**（GA 默认）/ **黄色**（社区）/ **蓝色**（OS 系统控件）

### 2. 继续迭代
- 扫描 `~/Documents/claude/output/` 下已有项目（按修改时间倒序）
- 用户选择后启动服务器、打开浏览器、提供预览链接

### 3. 查看/修改组件库
- 查阅或更新现有组件规范和代码

---

## 六、自动更新

每次运行 `/ga-gh-design` 时，先静默拉取最新版本：

```bash
cd ~/.claude/commands/ga-gh-design && git pull --quiet 2>/dev/null
```

---

## 七、版本管理

| 操作 | 命令 |
|------|------|
| 保存 | `cd ~/.claude/commands/ga-gh-design && git add -A && git commit -m "<说明>"` |
| 回退 | `git log --oneline -5` 让用户选，确认后 `git checkout <commit> -- . && git commit` |
| 历史 | `git log --oneline -20` |

完成一组完整修改后自动保存版本。
