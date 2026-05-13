---
category: components-code
name: DebugTool 页面调试工具代码
description: 页面调试工具 CSS + JS。FAB 按钮 + 底部抽屉 + List/Switch/PopupMenu 控件面板，内置暗色模式
---

# DebugTool 页面调试工具

每个原型页面自带的调试面板。FAB 按钮可拖拽吸边、3 秒自动收为竖条；点击打开底部抽屉，内含 List 形式的控件面板。

**内置控件**：暗色模式开关（自动处理 `theme-light` class，无需 onChange）

**依赖**：Drawer（drawer-code.md）、PopupMenu（popup-menu-code.md）、List CSS（list-code.md）、color.md 变量

**铁律**：页面调试工具永远处于所有内容的最上层。即使需求要求某个元素「置于最上层」，也必须在调试工具之下。任何页面元素的 z-index 不得超过调试工具。

---

## CSS

```css
/* ====== DebugTool 页面调试工具 ====== */

/* FAB 按钮 */
.ctrl-fab {
  position: absolute;
  right: 16px;
  bottom: 126px;
  width: 52px;
  height: 52px;
  border-radius: 50%;
  background: var(--primary);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 20;
  color: var(--white);
  box-shadow: 0 2px 8px rgba(0,0,0,0.3);
  touch-action: none;
  user-select: none;
}
.ctrl-fab:active { opacity: 0.85; }
.ctrl-fab.dragging { transition: none; }
.ctrl-fab.snapping { transition: left 0.25s ease, right 0.25s ease; }

/* FAB 收起竖条态 */
.ctrl-fab--bar {
  width: 5px;
  height: 64px;
  border-radius: 2.5px;
  left: 8px !important;
  right: auto !important;
  opacity: 0.7;
  transition: width 0.25s ease, border-radius 0.25s ease, opacity 0.25s ease, left 0.25s ease, right 0.25s ease;
  box-shadow: none;
}
.ctrl-fab--bar::after {
  content: '';
  position: absolute;
  inset: -12px -20px -12px -12px;
}
.ctrl-fab--bar img, .ctrl-fab--bar svg { display: none; }
.ctrl-fab--bar-right {
  border-radius: 2.5px;
  left: auto !important;
  right: 8px !important;
}
.ctrl-fab--bar-right::after {
  inset: -12px -12px -12px -20px;
}

/* 调试工具开关（系统蓝） */
.ctrl-switch {
  position: relative;
  width: 49px;
  height: 28px;
  border-radius: 14px;
  background: var(--outline);
  cursor: pointer;
  transition: background 0.2s ease;
  flex-shrink: 0;
}
.ctrl-switch::after {
  content: '';
  position: absolute;
  top: 4px;
  left: 4px;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: var(--white);
  transition: transform 0.2s ease;
  box-shadow: 0 1px 3px rgba(0,0,0,0.3);
}
.ctrl-switch--on { background: var(--primary); }
.ctrl-switch--on::after { transform: translateX(21px); }

/* 底部说明文字 */
.ctrl-note {
  padding: 20px;
  font-size: 12px;
  color: var(--on_surface_octonary);
  text-align: center;
  line-height: 1.6;
}
```

---

## JS

```javascript
/* ====== DebugTool 页面调试工具 ====== */

class DebugTool {
  /**
   * @param {Object} opts
   * @param {Array}  opts.controls - 控件分组配置
   *   [{ group: '分组名', items: [
   *     { type: 'switch', key: 'xxx', label: '标签', value: false },
   *     { type: 'select', key: 'xxx', label: '标签', options: [{key,label}], value: 'key' },
   *     { type: 'action', key: 'xxx', label: '标签' }
   *   ]}]
   * @param {Function} opts.onChange - function(key, value, tool) 控件变化回调
   * @param {string}   opts.icon - FAB 图标 HTML（默认内置调试图标）
   */
  constructor(opts) {
    opts = opts || {};
    this.controls = opts.controls || [];
    this.onChange = opts.onChange || null;
    this.icon = opts.icon || '<svg width="28" height="28" viewBox="3.5 3 29 30" fill="none"><path fill-rule="evenodd" clip-rule="evenodd" d="M26.2651 4.10612C26.9041 4.10612 27.424 4.05351 27.424 4.69258V17.1863C27.424 17.3284 27.5391 17.4435 27.6811 17.4435H29.4818C30.5185 17.4435 31.359 18.2839 31.359 19.3206V28.8323C31.359 30.8916 29.6896 32.5609 27.6304 32.5609H24.9033C22.844 32.5609 21.1747 30.8916 21.1747 28.8323V19.3206C21.1747 18.2839 22.0151 17.4435 23.0518 17.4435H24.8526C24.9946 17.4435 25.1097 17.3284 25.1097 17.1863V4.69258C25.1097 4.05351 25.626 4.10612 26.2651 4.10612ZM23.489 20.0149V28.8323C23.489 29.6134 24.1222 30.2466 24.9033 30.2466H27.6304C28.4115 30.2466 29.0447 29.6134 29.0447 28.8323V20.0149C29.0447 19.8729 28.9295 19.7578 28.7875 19.7578H23.7461C23.6041 19.7578 23.489 19.8729 23.489 20.0149ZM10.9721 5.15849C10.9721 4.31581 10.1002 3.75573 9.33384 4.10612C6.56696 5.37115 4.64062 8.16478 4.64062 11.4108C4.64062 14.0559 5.91982 16.4022 7.89332 17.8649C8.02908 17.9655 8.1128 18.1228 8.1128 18.2918V28.1291C8.1128 30.646 10.1532 32.6864 12.67 32.6864C15.1869 32.6864 17.2273 30.646 17.2273 28.1291V18.2901C17.2273 18.1212 17.311 17.9639 17.4466 17.8633C19.4189 16.4005 20.6972 14.0549 20.6972 11.4108C20.6972 8.16478 18.7709 5.37115 16.004 4.10612C15.2376 3.75573 14.3657 4.31581 14.3657 5.15849V9.21517C14.3657 9.35719 14.2506 9.47231 14.1086 9.47231H11.2293C11.0873 9.47231 10.9721 9.35719 10.9721 9.21517V5.15849ZM10.4271 28.1291V19.1219L10.4288 19.1224V17.0087C10.4288 16.8027 10.3048 16.6184 10.1204 16.5264C8.24382 15.5897 6.95491 13.6507 6.95491 11.4108C6.95491 9.92924 7.51876 8.57841 8.44488 7.56238C8.52175 7.47806 8.65785 7.5341 8.65785 7.6482V9.90946C8.65785 10.9462 9.49827 11.7866 10.535 11.7866H14.8029C15.8396 11.7866 16.68 10.9462 16.68 9.90946V7.6482C16.68 7.5341 16.8161 7.47806 16.893 7.56238C17.8191 8.57841 18.3829 9.92924 18.3829 11.4108C18.3829 13.649 17.096 15.5867 15.2217 16.5242C15.0376 16.6163 14.9138 16.8005 14.9137 17.0063L14.913 28.1291C14.913 29.3679 13.9088 30.3721 12.67 30.3721C11.4313 30.3721 10.4271 29.3679 10.4271 28.1291Z" fill="currentColor"/></svg>';
    this.state = {};
    this.activePopup = null;

    this._ensureDarkMode();
    this._initState();
    this._createFAB();
    this._createDrawer();
    this._initFABBehavior();
  }

  /* 确保暗色模式开关存在 */
  _ensureDarkMode() {
    var hasDisplay = this.controls.some(function(g) { return g.group === '显示'; });
    var hasDark = false;
    this.controls.forEach(function(g) {
      g.items.forEach(function(item) { if (item.key === 'dark') hasDark = true; });
    });
    if (!hasDark) {
      var darkItem = { type: 'switch', key: 'dark', label: '暗色模式', value: !document.body.classList.contains('theme-light') };
      if (hasDisplay) {
        var displayGroup = this.controls.find(function(g) { return g.group === '显示'; });
        displayGroup.items.unshift(darkItem);
      } else {
        this.controls.push({ group: '显示', items: [darkItem] });
      }
    }
  }

  /* 从 controls 配置初始化 state */
  _initState() {
    var self = this;
    this.controls.forEach(function(g) {
      g.items.forEach(function(item) {
        if (item.type === 'switch') self.state[item.key] = !!item.value;
        else if (item.type === 'select') self.state[item.key] = item.value || (item.options[0] && item.options[0].key);
      });
    });
  }

  /* 创建 FAB 按钮 */
  _createFAB() {
    var page = document.querySelector('.page') || document.body;
    this.fab = document.createElement('div');
    this.fab.className = 'ctrl-fab';
    this.fab.id = 'ctrl-fab';
    this.fab.innerHTML = this.icon;
    page.appendChild(this.fab);
  }

  /* 创建抽屉 */
  _createDrawer() {
    this.drawer = new Drawer({ title: '页面调试工具' });
  }

  /* STATUS_ARROW SVG */
  _statusArrow() {
    return '<svg width="10" height="16" viewBox="0 0 10 16" fill="none"><path fill-rule="evenodd" clip-rule="evenodd" d="M2.396 4.738L4.568 2.566L5.007 2.127L5.426 2.546L7.598 4.718L8.53 5.651C8.827 5.948 9.309 5.948 9.606 5.651C9.904 5.353 9.904 4.872 9.606 4.574L8.674 3.642L6.502 1.47L5.57 0.537C5.358 0.326 5.054 0.265 4.789 0.354C4.655 0.385 4.528 0.453 4.424 0.557L3.491 1.489L1.319 3.661L0.387 4.594C0.09 4.891 0.09 5.373 0.387 5.67C0.684 5.968 1.166 5.968 1.464 5.67L2.396 4.738ZM2.396 11.257L4.568 13.428L5.007 13.867L5.426 13.448L7.598 11.276L8.53 10.344C8.827 10.046 9.309 10.046 9.606 10.344C9.904 10.641 9.904 11.123 9.606 11.42L8.674 12.353L6.502 14.524L5.57 15.457C5.358 15.668 5.054 15.729 4.789 15.64C4.655 15.609 4.528 15.542 4.424 15.437L3.491 14.505L1.319 12.333L0.387 11.4C0.09 11.103 0.09 10.621 0.387 10.324C0.684 10.027 1.166 10.027 1.464 10.324L2.396 11.257Z" fill="currentColor"/></svg>';
  }

  /* 构建面板 HTML */
  _buildHTML() {
    var self = this;
    var html = '<div class="list-container">';

    this.controls.forEach(function(group) {
      html += '<div class="list-group-title">' + group.group + '</div>';
      html += '<div class="list-card">';
      var len = group.items.length;

      group.items.forEach(function(item, i) {
        var pos = len === 1 ? 'standalone' : (i === 0 ? 'top' : (i === len - 1 ? 'bottom' : 'middle'));
        var divider = i < len - 1 ? '' : '';
        var noDivider = ' list-item--no-divider';

        if (item.type === 'switch') {
          html += '<div class="list-item list-item--' + pos + noDivider + '">' +
            '<div class="list-item__text"><div class="list-item__title">' + item.label + '</div></div>' +
            '<div class="ctrl-switch' + (self.state[item.key] ? ' ctrl-switch--on' : '') + '" data-ctrl="' + item.key + '"></div>' +
          '</div>';
        } else if (item.type === 'select') {
          var curLabel = '';
          item.options.forEach(function(o) { if (o.key === self.state[item.key]) curLabel = o.label; });
          html += '<div class="list-item list-item--' + pos + noDivider + '" data-select="' + item.key + '">' +
            '<div class="list-item__text"><div class="list-item__title">' + item.label + '</div></div>' +
            '<div class="list-item__trailing list-item__trailing--status">' +
              '<span class="list-item__trailing--status-text" data-val="' + item.key + '">' + curLabel + '</span>' +
              '<span class="list-item__trailing--status-arrow">' + self._statusArrow() + '</span>' +
            '</div>' +
          '</div>';
        } else if (item.type === 'action') {
          html += '<div class="list-item list-item--' + pos + noDivider + '" data-action="' + item.key + '">' +
            '<div class="list-item__text"><div class="list-item__title">' + item.label + '</div></div>' +
            '<div class="list-item__trailing--arrow-bare"></div>' +
          '</div>';
        }
      });

      html += '</div>';
    });

    html += '<div class="ctrl-note">页面调试工具仅用于原型演示，不包含在研发交付物中</div>';
    html += '</div>';
    return html;
  }

  /* 绑定面板事件 */
  _bindEvents() {
    var self = this;
    var content = this.drawer.el.querySelector('.drawer__content');

    content.querySelectorAll('.ctrl-switch').forEach(function(sw) {
      sw.addEventListener('click', function(e) {
        e.stopPropagation();
        var key = sw.dataset.ctrl;
        self.state[key] = !self.state[key];
        sw.classList.toggle('ctrl-switch--on', self.state[key]);
        if (key === 'dark') {
          document.body.classList.toggle('theme-light', !self.state.dark);
        } else if (self.onChange) {
          self.onChange(key, self.state[key], self);
        }
      });
    });

    this.controls.forEach(function(group) {
      group.items.forEach(function(item) {
        if (item.type === 'select') {
          var row = content.querySelector('[data-select="' + item.key + '"]');
          if (!row) return;
          row.addEventListener('click', function() {
            if (self.activePopup) self.activePopup.destroy();
            self.activePopup = new PopupMenu({
              wide: true,
              items: item.options.map(function(o) {
                return { key: o.key, label: o.label, selected: o.key === self.state[item.key] };
              }),
              onSelect: function(key) {
                self.state[item.key] = key;
                var label = '';
                item.options.forEach(function(o) { if (o.key === key) label = o.label; });
                var valEl = content.querySelector('[data-val="' + item.key + '"]');
                if (valEl) valEl.textContent = label;
                if (self.onChange) self.onChange(item.key, key, self);
              }
            });
            self.activePopup.open(row);
          });
        } else if (item.type === 'action') {
          var actionRow = content.querySelector('[data-action="' + item.key + '"]');
          if (!actionRow) return;
          actionRow.addEventListener('click', function() {
            self.drawer.close();
            if (self.onChange) self.onChange(item.key, true, self);
          });
        }
      });
    });
  }

  /* 打开面板 */
  openPanel() {
    this.drawer.setContent(this._buildHTML());
    this.drawer.open();
    this._bindEvents();
  }

  /* FAB 拖拽 + 吸边 + 3s 自动收为竖条 */
  _initFABBehavior() {
    var fab = this.fab;
    var page = document.querySelector('.page') || document.body;
    var self = this;
    var startX, startY, fabX, fabY, dragged;
    var hideTimer = null;
    var isBar = false;
    var onRight = true;

    function resetHideTimer() {
      clearTimeout(hideTimer);
      hideTimer = setTimeout(toBar, 3000);
    }

    function toBar() {
      if (isBar) return;
      isBar = true;
      fab.classList.remove('snapping');
      fab.classList.add('ctrl-fab--bar');
      if (onRight) {
        fab.classList.add('ctrl-fab--bar-right');
        fab.style.left = 'auto';
        fab.style.right = '0';
      } else {
        fab.classList.remove('ctrl-fab--bar-right');
        fab.style.right = 'auto';
        fab.style.left = '0';
      }
      fab.style.bottom = '';
    }

    function toCircle() {
      if (!isBar) return;
      isBar = false;
      fab.classList.remove('ctrl-fab--bar', 'ctrl-fab--bar-right');
      fab.classList.add('snapping');
      if (onRight) {
        fab.style.left = ''; fab.style.right = '16px';
      } else {
        fab.style.left = '16px'; fab.style.right = 'auto';
      }
      resetHideTimer();
    }

    function onStart(e) {
      if (isBar) {
        e.preventDefault();
        toCircle();
        return;
      }
      clearTimeout(hideTimer);
      var t = e.touches ? e.touches[0] : e;
      var rect = fab.getBoundingClientRect();
      var pageRect = page.getBoundingClientRect();
      startX = t.clientX; startY = t.clientY;
      fabX = rect.left - pageRect.left;
      fabY = rect.top - pageRect.top;
      dragged = false;
      fab.classList.add('dragging');
      fab.classList.remove('snapping');
    }

    function onMove(e) {
      if (fabX === undefined) return;
      var t = e.touches ? e.touches[0] : e;
      var dx = t.clientX - startX, dy = t.clientY - startY;
      if (Math.abs(dx) > 4 || Math.abs(dy) > 4) dragged = true;
      if (!dragged) return;
      e.preventDefault();
      var pageRect = page.getBoundingClientRect();
      var nx = fabX + dx, ny = fabY + dy;
      nx = Math.max(0, Math.min(nx, pageRect.width - 52));
      ny = Math.max(0, Math.min(ny, pageRect.height - 52));
      fab.style.left = nx + 'px';
      fab.style.top = ny + 'px';
      fab.style.right = 'auto';
      fab.style.bottom = 'auto';
    }

    function onEnd() {
      if (fabX === undefined) return;
      fab.classList.remove('dragging');
      if (dragged) {
        var pageRect = page.getBoundingClientRect();
        var rect = fab.getBoundingClientRect();
        var cx = rect.left - pageRect.left + 26;
        fab.classList.add('snapping');
        if (cx < pageRect.width / 2) {
          fab.style.left = '16px'; fab.style.right = 'auto';
          onRight = false;
        } else {
          fab.style.left = ''; fab.style.right = '16px';
          onRight = true;
        }
      } else {
        self.openPanel();
      }
      fabX = undefined;
      resetHideTimer();
    }

    fab.addEventListener('touchstart', onStart, { passive: false });
    document.addEventListener('touchmove', onMove, { passive: false });
    document.addEventListener('touchend', onEnd);
    fab.addEventListener('mousedown', onStart);
    document.addEventListener('mousemove', onMove);
    document.addEventListener('mouseup', onEnd);

    resetHideTimer();
  }
}
```

---

## 调用示例

```javascript
// 最简用法：只有暗色模式开关（自动内置）
new DebugTool();

// 完整用法：多组控件
new DebugTool({
  controls: [
    { group: '用户状态', items: [
      { type: 'select', key: 'appState', label: '安装状态',
        options: [
          { key: 'not-installed', label: '未安装' },
          { key: 'installed', label: '已安装' }
        ],
        value: 'not-installed' },
      { type: 'switch', key: 'login', label: '已登录', value: true }
    ]},
    { group: '显示', items: [
      // 暗色模式会自动插入到此分组第一项
      { type: 'switch', key: 'activity', label: '活动入口', value: false }
    ]},
    { group: '调试', items: [
      { type: 'action', key: 'skeleton', label: '显示骨架屏' }
    ]}
  ],
  onChange: function(key, value, tool) {
    switch (key) {
      case 'appState': setAppState(value); break;
      case 'login': updateLoginState(value); break;
      case 'activity': toggleActivity(value); break;
      case 'skeleton': showSkeleton(); break;
    }
  }
});
```

---

## 控件类型

| 类型 | 说明 | state 值类型 |
|------|------|-------------|
| `switch` | 开关，系统蓝色 `var(--primary)` | `boolean` |
| `select` | 状态选择，点击弹出 PopupMenu（宽版 288px） | `string`（option key） |
| `action` | 动作项，点击关闭抽屉并触发回调 | `true` |

## FAB 行为

| 状态 | 说明 |
|------|------|
| 圆形（默认） | 52×52，系统蓝，可拖拽，松手吸边 |
| 竖条（3s 后） | 5×64，圆角 2.5px，边距 8px，透明度 0.7 |
| 点击竖条 | 恢复圆形，重置 3s 倒计时 |
| 点击圆形 | 打开调试面板 |

## 依赖

- **Drawer**：drawer-code.md（抽屉容器）
- **PopupMenu**：popup-menu-code.md（select 类型控件）
- **List CSS**：list-code.md（列表布局）
- **颜色变量**：color.md — `--primary`、`--outline`、`--on_surface`、`--on_surface_octonary`、`--surface_pop_window`、`--container_list`、`--mask`
