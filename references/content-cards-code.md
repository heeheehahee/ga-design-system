# Content Cards 内容卡片集

三种数据驱动卡片：ReviewCard（评论）、FeedCard（攻略/资讯）、AppCard（应用推荐）。

---

## 一、ReviewCard 评论卡

### CSS

```css
/* ====== ReviewCard 评论卡 ====== */

.review-card {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 16px;
  border-radius: 20px;
  background: var(--surface_container);
}

/* 横滑容器 */
.review-carousel {
  display: flex;
  gap: 10px;
  padding: 0 20px;
  overflow-x: auto;
  scrollbar-width: none;
  -webkit-overflow-scrolling: touch;
}
.review-carousel::-webkit-scrollbar { display: none; }
.review-carousel .review-card {
  flex-shrink: 0;
  width: calc(100vw - 40px);
  max-width: 352px;
}

.review-card__header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}
.review-card__user {
  display: flex;
  align-items: center;
  gap: 12px;
}
.review-card__avatar {
  width: 32px;
  height: 32px;
  border-radius: 999px;
  flex-shrink: 0;
  background-size: cover;
  background-position: center;
  border: 0.35px solid rgba(0,0,0,0.1);
}
.review-card__name {
  font-size: 14px;
  font-weight: 380;
  color: var(--on_surface);
}
.review-card__time {
  font-size: 12px;
  font-weight: 330;
  color: var(--on_surface_quaternary);
}
.review-card__rating {
  display: flex;
  align-items: center;
  gap: 8px;
}
.review-card__stars {
  display: flex;
  gap: 2px;
}
.review-card__hot {
  font-size: 12px;
  font-weight: 380;
  color: var(--orange);
  background: rgba(var(--orange-rgb), 0.12);
  padding: 2px 8px;
  border-radius: 99px;
}
.review-card__text {
  font-size: 14px;
  font-weight: 330;
  line-height: 20px;
  color: var(--on_surface_secondary);
}
.review-card__source {
  font-size: 12px;
  font-weight: 330;
  color: var(--on_surface_quaternary);
  display: flex;
  align-items: center;
  gap: 4px;
}
.review-card__likes {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 13px;
  color: var(--on_surface_quaternary);
}
```

### 数据模型

```javascript
{
  name: 'Marco Braddy',
  avatar: 'images/avatar.png',    // null → 用纯色背景
  time: 'December 6 · Played 72 hours',
  stars: 4,                        // 1-5
  text: 'Great game...',
  device: 'Redmi K50',
  hotTag: '🔥 Latest hot reviews', // null → 不显示
  likes: 124
}
```

---

## 二、FeedCard 内容卡（攻略/资讯）

### CSS

```css
/* ====== FeedCard 内容卡 ====== */

.feeds-gallery {
  display: flex;
  gap: 12px;
}
.feeds-col {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.feed-card {
  display: flex;
  flex-direction: column;
  gap: 8px;
  border-radius: 12px;
  overflow: hidden;
}
.feed-card__img {
  width: 100%;
  aspect-ratio: 170 / 227;
  border-radius: 12px;
  background-size: cover;
  background-position: center;
  position: relative;
}
.feed-card__time {
  position: absolute;
  top: 8px;
  right: 8px;
  font-size: 11px;
  font-weight: 380;
  color: rgba(255,255,255,0.95);
  background: rgba(0,0,0,0.5);
  backdrop-filter: blur(6px);
  border-radius: 8px;
  padding: 2px 4px;
}
.feed-card__content {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 0 4px;
}
.feed-card__text {
  font-size: 13px;
  font-weight: 330;
  line-height: 150%;
  color: var(--on_surface);
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.feed-card__views {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 11px;
  font-weight: 330;
  color: var(--on_surface_octonary);
}
.feed-card__eye {
  display: inline-block;
  width: 15px;
  height: 15px;
  flex-shrink: 0;
  background: var(--on_surface_octonary);
  -webkit-mask: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='15' height='15'%3E%3Cpath d='M7.5 3C11.54 3 14.04 6.33 15 8c-.77 1.67-3.46 5-7.5 5S.77 9.67 0 8C.96 6.33 3.46 3 7.5 3zm0 1C4.35 4 2.21 6.39 1.15 8.04c.4.67 1.08 1.55 2.01 2.31C4.3 11.29 5.77 12 7.5 12s3.2-.71 4.34-1.65c.93-.76 1.61-1.64 2.01-2.31C12.78 6.39 10.65 4 7.5 4zm0 1.5a2.5 2.5 0 110 5 2.5 2.5 0 010-5zm0 1a1.5 1.5 0 100 3 1.5 1.5 0 000-3z' fill='black'/%3E%3C/svg%3E") no-repeat center / contain;
  mask: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='15' height='15'%3E%3Cpath d='M7.5 3C11.54 3 14.04 6.33 15 8c-.77 1.67-3.46 5-7.5 5S.77 9.67 0 8C.96 6.33 3.46 3 7.5 3zm0 1C4.35 4 2.21 6.39 1.15 8.04c.4.67 1.08 1.55 2.01 2.31C4.3 11.29 5.77 12 7.5 12s3.2-.71 4.34-1.65c.93-.76 1.61-1.64 2.01-2.31C12.78 6.39 10.65 4 7.5 4zm0 1.5a2.5 2.5 0 110 5 2.5 2.5 0 010-5zm0 1a1.5 1.5 0 100 3 1.5 1.5 0 000-3z' fill='black'/%3E%3C/svg%3E") no-repeat center / contain;
}
```

### 双列瀑布流用法

```html
<div class="feeds-gallery">
  <div class="feeds-col">
    <!-- 奇数 feed cards -->
  </div>
  <div class="feeds-col">
    <!-- 偶数 feed cards -->
  </div>
</div>
```

### 数据模型

```javascript
{
  img: 'images/feed-thumb.png',
  time: '12:48',
  text: 'The MLBB x Saint Seiya collaboration...',
  views: '48.8 K',
  category: 'guides'
}
```

---

## 三、AppCard 应用推荐卡

### CSS

```css
/* ====== AppCard 应用推荐卡 ====== */

.app-card-list {
  display: flex;
  flex-direction: column;
  padding: 0 20px;
}

.app-card {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 0;
}
.app-card + .app-card {
  border-top: 1px solid var(--outline);
}
.app-card__icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  border: 0.74px solid rgba(0,0,0,0.1);
  flex-shrink: 0;
  object-fit: cover;
}
.app-card__info {
  flex: 1;
  min-width: 0;
}
.app-card__name {
  font-size: 14px;
  font-weight: 380;
  color: var(--on_surface);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.app-card__meta {
  display: flex;
  flex-direction: column;
  gap: 2px;
  margin-top: 4px;
}
.app-card__cats {
  font-size: 12px;
  font-weight: 330;
  color: var(--on_surface_quaternary);
}
.app-card__vals {
  font-size: 12px;
  font-weight: 330;
  color: var(--on_surface_quaternary);
}
.app-card__rating {
  color: var(--orange);
}
.app-card__install {
  flex-shrink: 0;
}
```

### 用法

```html
<div class="app-card-list">
  <div class="app-card">
    <img class="app-card__icon" src="icon.png" alt="">
    <div class="app-card__info">
      <div class="app-card__name">Clash of Clans</div>
      <div class="app-card__meta">
        <span class="app-card__cats">Strategy · Multiplayer</span>
        <span class="app-card__vals"><span class="app-card__rating">4.5 ★</span> · 256 MB</span>
      </div>
    </div>
    <div class="app-card__install" id="install-1"></div>
  </div>
</div>
```

```javascript
new InstallButton('#install-1', { size: 'small', state: 'enabled' });
```

### 数据模型

```javascript
{
  icon: 'images/app-icon.png',
  name: 'Clash of Clans',
  categories: 'Strategy · Multiplayer',
  rating: '4.5 ★',
  size: '256 MB'
}
```

---

## 设计参数汇总

| 卡片 | 圆角 | 背景 | 内边距 | 间距 |
|------|------|------|--------|------|
| ReviewCard | 20dp | `var(--surface_container)` | 16dp | 12dp |
| FeedCard | 12dp | 无（封面图） | 0 4dp（内容区） | 8dp |
| AppCard | 无 | 透明 | 12dp 0 | 12dp |
| 横滑容器 | — | — | 0 20dp | 10dp |
| 双列瀑布流 | — | — | 0 20dp | 12dp |
| 应用列表 | — | — | 0 20dp | 分隔线 |
