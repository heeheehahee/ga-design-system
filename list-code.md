---
category: components-code
name: List 列表代码
description: List CSS + JS 代码。规范见 list.md
---

# List 代码

规范：[list.md](list.md)

---

## CSS

```css
/* List 列表 */
.list-container { padding: 0 12px; }
.list-group-title { padding: 20px 16px 8px; font-size: 14px; font-weight: 380; color: var(--gray); }
.list-footer { padding: 8px 16px 20px; font-size: 13px; font-weight: 330; color: var(--on_surface_tertiary); }
.list-card { background: var(--container_list); border-radius: 20px; overflow: hidden; }

/* 列表行 */
.list-item { display: flex; align-items: center; gap: 14px; padding: 14px 16px; position: relative; }
.list-item--standalone { border-radius: 20px; }
.list-item--top { border-radius: 20px 20px 0 0; }
.list-item--middle { border-radius: 0; }
.list-item--bottom { border-radius: 0 0 20px 20px; }
.list-item--top::after, .list-item--middle::after { content: ''; position: absolute; left: 16px; right: 16px; bottom: 0; height: 1px; background: var(--outline); }
.list-item--no-divider::after { display: none; }
.list-item:active { background: var(--surface_container); }

/* 文本区 */
.list-item__text { flex: 1; min-width: 0; display: flex; flex-direction: column; }
.list-item__title { font-size: 17px; font-weight: 380; color: var(--on_surface); flex: 1; }
.list-item__desc { font-size: 14px; font-weight: 330; color: var(--on_surface_tertiary); margin-top: 4px; }

/* 右侧功能区 */
.list-item__trailing { display: flex; align-items: center; flex-shrink: 0; gap: 8px; }

/* 单独进入二级 — 28×28 圆底 + chevron */
.list-item__trailing--arrow { width: 28px; height: 28px; border-radius: 50%; background: var(--on_surface_right_button_bg); display: flex; align-items: center; justify-content: center; }
.list-item__trailing--arrow::after { content: ''; width: 28px; height: 28px; background: var(--on_surface_octonary); -webkit-mask: url("data:image/svg+xml,%3Csvg width='28' height='28' viewBox='0 0 28 28' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M11.8323 9.55351C12.263 9.12279 12.9613 9.12279 13.392 9.55351L17.0401 13.2016C17.329 13.4905 17.4241 13.8996 17.3256 14.2678C17.2807 14.4622 17.1824 14.6468 17.0309 14.7984L13.3828 18.4465C12.9521 18.8772 12.2538 18.8772 11.823 18.4465C11.3923 18.0157 11.3923 17.3174 11.823 16.8867L14.7143 13.9954L11.8323 11.1133C11.4015 10.6826 11.4015 9.98424 11.8323 9.55351Z' fill='black'/%3E%3C/svg%3E") center / contain no-repeat; mask: url("data:image/svg+xml,%3Csvg width='28' height='28' viewBox='0 0 28 28' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M11.8323 9.55351C12.263 9.12279 12.9613 9.12279 13.392 9.55351L17.0401 13.2016C17.329 13.4905 17.4241 13.8996 17.3256 14.2678C17.2807 14.4622 17.1824 14.6468 17.0309 14.7984L13.3828 18.4465C12.9521 18.8772 12.2538 18.8772 11.823 18.4465C11.3923 18.0157 11.3923 17.3174 11.823 16.8867L14.7143 13.9954L11.8323 11.1133C11.4015 10.6826 11.4015 9.98424 11.8323 9.55351Z' fill='black'/%3E%3C/svg%3E") center / contain no-repeat; }

/* 整个列表进入二级 — 裸箭头 8×14 */
.list-item__trailing--arrow-bare { width: 8px; height: 14px; flex-shrink: 0; background: var(--on_surface_octonary); -webkit-mask: url("data:image/svg+xml,%3Csvg width='8' height='14' viewBox='0 0 8 14' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1 1L7 7L1 13' stroke='black' stroke-width='1.5' stroke-linecap='round' stroke-linejoin='round'/%3E%3C/svg%3E") center / contain no-repeat; mask: url("data:image/svg+xml,%3Csvg width='8' height='14' viewBox='0 0 8 14' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1 1L7 7L1 13' stroke='black' stroke-width='1.5' stroke-linecap='round' stroke-linejoin='round'/%3E%3C/svg%3E") center / contain no-repeat; }

/* 右侧纯文字 */
.list-item__trailing-text { font-size: 14px; font-weight: 380; color: var(--on_surface_tertiary); flex-shrink: 0; }

/* 状态切换（状态文字 + 上下箭头） */
.list-item__trailing--status { display: flex; align-items: center; gap: 8px; cursor: pointer; }
.list-item__trailing--status-text { font-size: 14px; font-weight: 330; color: var(--on_surface_quaternary); }
.list-item__trailing--status-arrow { width: 10px; height: 16px; color: var(--on_surface_octonary); opacity: 0.7; display: flex; }
.list-item__trailing--status-arrow svg { width: 10px; height: 16px; }

/* 左侧图标区 */
.list-item__icon { width: 28px; height: 28px; border-radius: 8px; background: var(--surface_container); display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.list-item__icon--lg { width: 40px; height: 40px; border-radius: 10px; border: 0.36px solid var(--outline); }
.list-item__icon svg { width: 24px; height: 24px; }

/* 单选勾选标记 — 20×20, Figma 导出 path */
.list-item__checkmark { width: 20px; height: 20px; flex-shrink: 0; background: var(--primary); -webkit-mask: url("data:image/svg+xml,%3Csvg width='20' height='20' viewBox='0 0 20 20' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M17.4687 6.28732C17.9089 5.75551 17.8347 4.96755 17.3028 4.52735C16.771 4.08715 15.9831 4.16141 15.5429 4.69322L8.94602 12.6629L5.53427 9.25116C5.04612 8.763 4.25466 8.763 3.76651 9.25116C3.27835 9.73931 3.27835 10.5308 3.76651 11.0189L8.14393 15.3964C8.50702 15.7594 9.03789 15.8525 9.48696 15.6754C9.68259 15.6012 9.86224 15.4767 10.0052 15.304L17.4687 6.28732Z' fill='black'/%3E%3C/svg%3E") center / contain no-repeat; mask: url("data:image/svg+xml,%3Csvg width='20' height='20' viewBox='0 0 20 20' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M17.4687 6.28732C17.9089 5.75551 17.8347 4.96755 17.3028 4.52735C16.771 4.08715 15.9831 4.16141 15.5429 4.69322L8.94602 12.6629L5.53427 9.25116C5.04612 8.763 4.25466 8.763 3.76651 9.25116C3.27835 9.73931 3.27835 10.5308 3.76651 11.0189L8.14393 15.3964C8.50702 15.7594 9.03789 15.8525 9.48696 15.6754C9.68259 15.6012 9.86224 15.4767 10.0052 15.304L17.4687 6.28732Z' fill='black'/%3E%3C/svg%3E") center / contain no-repeat; }

/* 选中态 */
.list-item--selected .list-item__title { color: var(--primary); }

```

---

## 使用示例

```html
<div class="list-container">
  <div class="list-group-title">网络设置</div>

  <div class="list-item list-item--top">
    <div class="list-item__title">WLAN</div>
    <div class="list-item__trailing-text">已连接</div>
    <div class="list-item__trailing list-item__trailing--arrow"></div>
  </div>

  <div class="list-item list-item--middle">
    <div class="list-item__title">蓝牙</div>
    <div class="list-item__trailing-text">已开启</div>
    <div class="list-item__trailing list-item__trailing--arrow"></div>
  </div>

  <div class="list-item list-item--bottom">
    <div class="list-item__title">飞行模式</div>
    <div class="list-item__trailing">
      <div class="switch"></div>
    </div>
  </div>

  <div class="list-footer">开启飞行模式后将关闭所有无线连接</div>

  <!-- 状态切换 trailing -->
  <div class="list-item list-item--standalone">
    <div class="list-item__title">排序方式</div>
    <div class="list-item__trailing--status">
      <span class="list-item__trailing--status-text">状态</span>
      <div class="list-item__trailing--status-arrow"><svg width="10" height="16" viewBox="0 0 10 16" fill="none"><path fill-rule="evenodd" clip-rule="evenodd" d="M2.396 4.738L4.568 2.566L5.007 2.127L5.426 2.546L7.598 4.718L8.53 5.651C8.827 5.948 9.309 5.948 9.606 5.651C9.904 5.353 9.904 4.872 9.606 4.574L8.674 3.642L6.502 1.47L5.57 0.537C5.358 0.326 5.054 0.265 4.789 0.354C4.655 0.385 4.528 0.453 4.424 0.557L3.491 1.489L1.319 3.661L0.387 4.594C0.09 4.891 0.09 5.373 0.387 5.67C0.684 5.968 1.166 5.968 1.464 5.67L2.396 4.738ZM2.396 11.257L4.568 13.428L5.007 13.867L5.426 13.448L7.598 11.276L8.53 10.344C8.827 10.046 9.309 10.046 9.606 10.344C9.904 10.641 9.904 11.123 9.606 11.42L8.674 12.353L6.502 14.524L5.57 15.457C5.358 15.668 5.054 15.729 4.789 15.64C4.655 15.609 4.528 15.542 4.424 15.437L3.491 14.505L1.319 12.333L0.387 11.4C0.09 11.103 0.09 10.621 0.387 10.324C0.684 10.027 1.166 10.027 1.464 10.324L2.396 11.257Z" fill="currentColor"/></svg></div>
    </div>
  </div>
</div>
```
