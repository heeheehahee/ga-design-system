---
category: foundations
tokens: [shadow.low, shadow.regular, shadow.high, shadow.extrahigh, shadow.float]
description: 阴影与层级 — HyperOS3 阴影梯度规范
source: Figma Xiaomi HyperOS3 UI Kit (miuix3.2) 阴影 Shadow (node 43336:168598)
---

# 阴影与层级

> **HyperOS 系统阴影为小米独有定制样式，非安卓原生样式。**
> 工具网站：https://dynamic-shadow.jst.xiaomi.net/
> 技术接入文档：https://xiaomi.f.mioffice.cn/docx/doxk43JxQvuKKKLf7j7gJKekNac

---

## 阴影梯度规范

### Figma 命名样式（4 个，值已确认）

| Token | Y 偏移 | 模糊 | CSS `box-shadow` | 应用场景 |
|-------|--------|------|-------------------|---------|
| `shadow-low` | 10 | 20 | `0px 10px 20px rgba(0,0,0,0.12)` | 卡片、列表基础元素投影 |
| `shadow-regular` | 20 | 30 | `0px 20px 30px rgba(0,0,0,0.1)` | 超大卡片、容器元素投影 |
| `shadow-high` | 24 | 42 | `0px 24px 42px rgba(0,0,0,0.2)` | 通知卡片投影 |
| `shadow-float` | 24 | 50 | `0px 24px 50px rgba(0,0,0,0.18)` | 悬浮型元素（底 bar、toast 等） |

### Figma 表格中列出但无命名样式（1 个，透明度待确认）

| Token | Y 偏移 | 模糊 | CSS `box-shadow` | 应用场景 |
|-------|--------|------|-------------------|---------|
| `shadow-extrahigh` | 28 | 60 | `0px 28px 60px rgba(0,0,0,?)` | 桌面编辑图标定制投影 |

> `shadow-extrahigh` 在 Figma 阴影表中有高度(28)和模糊值(60)，但无命名 Shadow 样式，透明度无法从 Figma 直接获取。如需使用请通过阴影工具网站生成精确参数。

### 代码引用

```css
/* CSS 变量定义 */
--shadow-low: 0px 10px 20px rgba(0, 0, 0, 0.12);
--shadow-regular: 0px 20px 30px rgba(0, 0, 0, 0.1);
--shadow-high: 0px 24px 42px rgba(0, 0, 0, 0.2);
--shadow-float: 0px 24px 50px rgba(0, 0, 0, 0.18);

/* 使用 */
box-shadow: var(--shadow-low);
```

### 梯度规律

- **Y 偏移** 递增：10 → 20 → 24 → 28
- **模糊值** 递增：20 → 30 → 42 → 60
- `shadow-float` 专为悬浮 UI 设计，Y 偏移同 high(24) 但模糊更大(50)、透明度更低(0.18)，视觉上更柔和

---

## 层级顺序（z-index）

| Token | 值 | 用途 |
|-------|------|------|
| `z.base` | 0 | 基础内容 |
| `z.sticky` | 100 | 吸顶元素（标题栏） |
| `z.overlay` | 500 | 遮罩层 |
| `z.modal` | 900 | 模态框、LoadingToast |
| `z.toast` | 1000 | Toast 提示（最高层级） |
