---
name: ga-gh-design
description: GA/GH 设计 — Getapps 设计系统组件代码库。写代码时遇到已有的组件、Token、图标，直接从这里取现成代码复用，不重新实现。也用于从 Figma 设计稿生成页面。
---

# Getapps 设计系统 — 组件代码库

**职责：做原型页面。** 创建项目、调用组件、设计还原、迭代优化。不负责制作/更新组件规范（那是 `/spec-maker` 的事）。

经过调试验证的移动端组件代码。写代码前先读 `_index.md` 了解全局规则。

所有文件位于 `~/.claude/commands/ga-gh-design/references/`。

## 自动更新

每次运行 `/ga-gh-design` 时，先静默执行以下命令拉取最新版本，无需告知用户：

```bash
cd ~/.claude/commands/ga-gh-design && git pull --quiet 2>/dev/null
```

## 启动选项

运行 `/ga-gh-design` 时，先询问用户：

1. **创建新项目** — `cp -r references/page-template/ ~/Documents/claude/output/项目名/`，然后：
   - 浏览器打开页面
   - 在 `~/Documents/claude/output/` 目录启动本地服务器（端口 9000）
   - 生成二维码图片（`qrcode.png`）放进项目目录
   - 提供手机预览链接：`http://本机IP:9000/项目名/index.html`
   - 询问用户下一步：**提供 Figma URL** / **直接描述需求** / **自行开始**
2. **继续迭代** — 扫描 `~/Documents/claude/output/` 下已有项目（按修改时间倒序），用户选择后启动服务器、打开浏览器、提供预览链接
3. **查看/修改组件库** — 查阅或更新现有组件规范和代码

---

## 快速查找

### 基础 Token

| 需要什么 | 查哪个文件 |
|---------|-----------|
| 全局规则（间距/命名/禁止行为） | `_index.md` |
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

## 版本管理

references 目录有 git 管理：

| 操作 | 命令 |
|------|------|
| 保存 | `cd ~/.claude/commands/ga-gh-design/references && git add -A && git commit -m "<说明>"` |
| 回退 | `git log --oneline -5` 让用户选，确认后 `git checkout <commit> -- . && git commit` |
| 历史 | `git log --oneline -20` |

完成一组完整修改后自动保存版本。
