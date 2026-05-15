---
category: components-code
name: 信息提示 NoticeBar 代码
description: NoticeBar CSS + JS 代码。规范见 noticebar.md
---

# 信息提示 NoticeBar 代码

⚠️ 复制代码前必须先读使用规则：[noticebar.md](noticebar.md)

## CSS

```css
/* ====== NoticeBar 信息提示 ====== */

.notice-bar {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 12px 16px;
  border-radius: 20px;
  gap: 8px;
}

/* —— Gray 默认 —— */
.notice-bar--gray {
  background: var(--container_notice_bar);
  color: var(--on_surface_tertiary);
}

/* —— Yellow 提醒 —— */
.notice-bar--yellow {
  background: var(--caution_container);
  color: var(--caution);
}

/* —— Red 警告 —— */
.notice-bar--red {
  background: var(--error_container);
  color: var(--error);
}

/* —— Blue 强调 —— */
.notice-bar--blue {
  background: var(--secondary);
  color: var(--primary);
}

/* —— 内容区 —— */
.notice-bar__icon {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  color: inherit;
  overflow: visible;
  margin: 0 -4px;
}

.notice-bar__icon svg {
  width: 32px;
  height: 32px;
}

.notice-bar__text {
  flex: 1;
  font-size: 14px;
  font-weight: 330;
  color: inherit;
  line-height: 1.4;
}

.notice-bar__trailing {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  color: inherit;
  cursor: pointer;
  border: none;
  background: none;
  padding: 0;
  overflow: visible;
  margin: 0 -4px;
}

.notice-bar__trailing svg {
  width: 32px;
  height: 32px;
}

/* —— 详情型：整条可点 —— */
.notice-bar--detail {
  cursor: pointer;
}
.notice-bar--detail:active {
  opacity: 0.7;
}

/* —— 关闭型：仅 × 可点 —— */
.notice-bar--close .notice-bar__trailing:active {
  opacity: 0.5;
}

/* —— 关闭动画 —— */
.notice-bar--hiding {
  opacity: 0;
  max-height: 0;
  padding-top: 0;
  padding-bottom: 0;
  margin: 0;
  overflow: hidden;
  transition: opacity 0.2s ease, max-height 0.3s ease 0.1s, padding 0.3s ease 0.1s;
}
```

## JS

```javascript
/* ====== NoticeBar Class ====== */

const NOTICE_ICONS = {
  info: `<svg width="40" height="40" viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M25.341 29.386C23.703 30.352 21.911 30.835 19.965 30.835C18.019 30.835 16.227 30.352 14.589 29.386C12.951 28.434 11.649 27.139 10.683 25.501C9.717 23.863 9.234 22.071 9.234 20.125C9.234 18.179 9.717 16.387 10.683 14.749C11.649 13.111 12.951 11.809 14.589 10.843C16.227 9.877 18.019 9.394 19.965 9.394C21.911 9.394 23.703 9.877 25.341 10.843C26.979 11.809 28.274 13.111 29.226 14.749C30.192 16.387 30.675 18.179 30.675 20.125C30.675 22.071 30.192 23.863 29.226 25.501C28.274 27.139 26.979 28.434 25.341 29.386ZM15.471 27.853C16.843 28.651 18.341 29.05 19.965 29.05C21.575 29.05 23.059 28.651 24.417 27.853C25.789 27.055 26.874 25.977 27.672 24.619C28.484 23.247 28.89 21.749 28.89 20.125C28.89 18.501 28.484 17.01 27.672 15.652C26.874 14.28 25.789 13.195 24.417 12.397C23.059 11.599 21.575 11.2 19.965 11.2C18.341 11.2 16.843 11.599 15.471 12.397C14.113 13.195 13.035 14.28 12.237 15.652C11.439 17.01 11.04 18.501 11.04 20.125C11.04 21.749 11.439 23.247 12.237 24.619C13.035 25.977 14.113 27.055 15.471 27.853ZM20.763 15.988C20.539 16.212 20.273 16.324 19.965 16.324C19.657 16.324 19.384 16.212 19.146 15.988C18.922 15.75 18.81 15.477 18.81 15.169C18.81 14.861 18.922 14.595 19.146 14.371C19.37 14.147 19.643 14.035 19.965 14.035C20.287 14.035 20.553 14.147 20.763 14.371C20.987 14.581 21.099 14.847 21.099 15.169C21.099 15.491 20.987 15.764 20.763 15.988ZM20.679 26.047C20.581 26.145 20.441 26.194 20.259 26.194H19.671C19.489 26.194 19.342 26.145 19.23 26.047C19.118 25.949 19.062 25.809 19.062 25.627V18.088C19.062 17.934 19.111 17.801 19.209 17.689C19.321 17.577 19.468 17.521 19.65 17.521H20.238C20.448 17.521 20.602 17.577 20.7 17.689C20.798 17.787 20.847 17.92 20.847 18.088V25.627C20.847 25.809 20.791 25.949 20.679 26.047Z" fill="currentColor"/></svg>`,
  chevron: `<svg width="20" height="20" viewBox="18.3 15.3 11.4 18.2" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M20.6147 16.8446C20.7147 16.8446 20.8047 16.8846 20.8847 16.9646L28.0397 24.1196C28.1097 24.1896 28.1447 24.2796 28.1447 24.3896C28.1447 24.4996 28.1047 24.5946 28.0247 24.6746L20.9147 31.7696C20.8147 31.8696 20.7097 31.9146 20.5997 31.9046C20.4997 31.9046 20.3797 31.8346 20.2397 31.6946L19.9697 31.4246C19.8797 31.3346 19.8297 31.2396 19.8197 31.1396C19.8097 31.0396 19.8447 30.9496 19.9247 30.8696L26.3897 24.3896L19.9247 17.9246C19.8447 17.8446 19.8047 17.7496 19.8047 17.6396C19.8147 17.5296 19.8547 17.4346 19.9247 17.3546L20.3597 16.9346C20.4297 16.8646 20.5147 16.8346 20.6147 16.8446Z" fill="currentColor"/></svg>`,
  close: `<svg width="20" height="20" viewBox="40 14 20 20" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M49.975 23.385L55.3 18.03C55.4 17.92 55.505 17.865 55.615 17.865C55.725 17.865 55.83 17.92 55.93 18.03L56.26 18.36C56.36 18.46 56.41 18.565 56.41 18.675C56.41 18.775 56.36 18.88 56.26 18.99L50.935 24.345L56.275 29.685C56.385 29.785 56.44 29.89 56.44 30C56.44 30.11 56.385 30.215 56.275 30.315L55.945 30.645C55.735 30.855 55.525 30.855 55.315 30.645L49.975 25.305L44.62 30.63C44.52 30.73 44.415 30.78 44.305 30.78C44.195 30.78 44.09 30.73 43.99 30.63L43.66 30.3C43.56 30.2 43.51 30.095 43.51 29.985C43.51 29.875 43.56 29.77 43.66 29.67L49.015 24.345L43.675 18.99C43.575 18.88 43.525 18.775 43.525 18.675C43.525 18.565 43.575 18.46 43.675 18.36L44.005 18.03C44.115 17.92 44.22 17.865 44.32 17.865C44.43 17.865 44.535 17.92 44.635 18.03L49.975 23.385Z" fill="currentColor"/></svg>`
};

class NoticeBar {
  constructor(container, options = {}) {
    const {
      text = '',
      type = 'gray',
      trailing = type === 'gray' ? 'detail' : 'close',
      icon = false,
      onClick = null,
      onClose = null
    } = options;

    this.el = document.createElement('div');
    this.el.className = `notice-bar notice-bar--${type} notice-bar--${trailing === 'detail' ? 'detail' : 'close'}`;

    let html = '';

    if (icon) {
      html += `<div class="notice-bar__icon">${NOTICE_ICONS.info}</div>`;
    }

    html += `<span class="notice-bar__text">${text}</span>`;

    if (trailing === 'detail') {
      html += `<div class="notice-bar__trailing">${NOTICE_ICONS.chevron}</div>`;
    } else {
      html += `<button class="notice-bar__trailing" aria-label="关闭">${NOTICE_ICONS.close}</button>`;
    }

    this.el.innerHTML = html;

    if (trailing === 'detail' && onClick) {
      this.el.addEventListener('click', onClick);
    }
    if (trailing === 'close') {
      this.el.querySelector('.notice-bar__trailing').addEventListener('click', () => {
        this.close();
        if (onClose) onClose();
      });
    }

    container.appendChild(this.el);
  }

  close() {
    this.el.style.maxHeight = this.el.offsetHeight + 'px';
    requestAnimationFrame(() => {
      this.el.classList.add('notice-bar--hiding');
    });
    this.el.addEventListener('transitionend', () => this.el.remove(), { once: true });
    setTimeout(() => this.el.remove(), 500);
  }
}
```

## 调用示例

```javascript
// Gray + 详情箭头（默认）
new NoticeBar(container, {
  text: '开启小米云同步，重要笔记、待办不丢失',
  type: 'gray',
  onClick: () => location.href = '/settings/cloud'
});

// Yellow + 关闭 + 前置图标
new NoticeBar(container, {
  text: '版本更新可用，建议尽快升级',
  type: 'yellow',
  icon: true,
  onClose: () => console.log('dismissed')
});

// Red + 关闭
new NoticeBar(container, {
  text: '存储空间不足，请清理缓存',
  type: 'red'
});

// Blue + 详情
new NoticeBar(container, {
  text: '全新社区功能已上线',
  type: 'blue',
  trailing: 'detail',
  onClick: () => location.href = '/community'
});
```
