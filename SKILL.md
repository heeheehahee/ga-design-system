---
name: ga-gh-design
description: GA/GH 设计 — Getapps 设计系统组件代码库。写代码时遇到已有的组件、Token、图标，直接从这里取现成代码复用，不重新实现。也用于从 Figma 设计稿生成页面。
---

# Getapps 设计系统 — 组件代码库

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

| 需要什么 | 查哪个文件 |
|---------|-----------|
| 全局规则（间距/命名/禁止行为） | `_index.md` |
| 颜色 Token | `color.md` — 62 个 Token，`var(--Token名)` |
| 字体/圆角/投影 | `typography.md` / `border-radius.md` / `elevation.md` |
| 按钮 | `button-code.md` — Base/Install/Glass/React 等 |
| 标题栏 | `navbar-code.md` + `navbar.md`（使用规则和参数） |
| 底 Tab 栏 / 状态栏 / 小白条 | `navbar-code.md` |
| 提示气泡 | `tooltip-code.md` — 8 方向 |
| 弹窗 | `dialog.md` — 7 种类型（无代码文件） |
| 轻提示 Toast | `toast-code.md` |
| 信息提示 NoticeBar | `noticebar-code.md` |
| 加载 Loading | `loading-code.md` |
| Chips 标签栏 | `chips-code.md` |
| 图标 SVG | `icons.md` — 标题栏 19 + Tab 5 + 媒体 4 + 反馈 4 |
| 预编译 CSS/JS | `ga-design-system.css` + `ga-design-system.js` |
| 页面调试工具 | `debug-tool-code.md` — FAB + 抽屉控件面板，内置暗色模式，新页面必装 |
| 页面骨架 | `skeleton-code.md` |
| 组件预览 | `设计组件库.html` — 交互预览，明暗/主色切换 |

---

## 版本管理

references 目录有 git 管理：

| 操作 | 命令 |
|------|------|
| 保存 | `cd ~/.claude/commands/ga-gh-design/references && git add -A && git commit -m "<说明>"` |
| 回退 | `git log --oneline -5` 让用户选，确认后 `git checkout <commit> -- . && git commit` |
| 历史 | `git log --oneline -20` |

完成一组完整修改后自动保存版本。
