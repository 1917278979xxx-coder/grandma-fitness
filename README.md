# 💃 外婆健身操

为外婆设计的极简健身操视频播放器，大字大按钮，点一下就能看。家人远程管理视频列表，外婆只管点开看。

## ✨ 功能

### 👵 老年模式
- 大字、大按钮，暖色系主题，不怕点错
- 视频以卡片列表展示，点击即可全屏播放
- 播放页只有「返回列表」和「暂停/播放」两个按钮
- 无多余 UI，防误触

### 👨‍💻 管理模式（给年轻人用）
- 连续点击标题「💃 外婆健身操」5 次进入
- 添加在线视频链接（B站 / YouTube / 直链）
- 上传本地视频文件（存浏览器本地）
- 拖拽排序、删除视频
- 所有视频元数据实时同步到云端

### ☁️ 实时云同步
- 基于 [Supabase](https://supabase.com) 的实时数据库
- 家人在管理端添加视频，外婆设备几秒内自动更新
- 邮箱 + 密码登录，同一账号多设备共享

### 📺 支持的视频来源
| 类型 | 播放方式 |
|------|----------|
| B站 | iframe 嵌入播放 |
| YouTube | iframe 嵌入播放 |
| 直链视频 (.mp4/.m3u8/.webm) | 原生播放器 |
| 本地上传 | 原生播放器 |
| 微信视频号/抖音/快手等 | 提示跳转到对应 App |

## 🚀 在线访问

**https://1917278979xxx-coder.github.io/grandma-fitness/**

## 🛠 技术架构

```
┌──────────────────────────────────┐
│          GitHub Pages            │
│     index.html (单文件零依赖)      │
│                                  │
│  ┌─────────┐  ┌───────────────┐  │
│  │ 登录页   │  │  老年模式      │  │
│  │ Supabase │  │  Video Player │  │
│  │ Auth     │  │  IndexedDB    │  │
│  └─────────┘  └───────────────┘  │
│                                  │
│  ┌─────────────────────────────┐ │
│  │       管理模式               │ │
│  │  CRUD + 拖拽排序            │ │
│  │  Supabase Realtime          │ │
│  └─────────────────────────────┘ │
└──────────┬───────────────────────┘
           │
    ┌──────▼──────┐
    │  Supabase   │
    │  Auth + DB  │
    │  + Realtime │
    └─────────────┘
```

- **前端：** 纯 HTML/CSS/JS，单文件零依赖（除了 Supabase SDK CDN）
- **认证：** Supabase Auth（邮箱 + 密码）
- **数据库：** Supabase PostgreSQL
- **实时同步：** Supabase Realtime (Postgres Changes)
- **本地存储：** IndexedDB（本地视频文件）

## 📦 本地开发

直接打开 `index.html` 即可，或在本地起一个 HTTP 服务：

```bash
# Python 3
python -m http.server 8080
```

## 🔄 更新部署

```bash
cd grandma-fitness
# 修改 index.html 后
git add index.html && git commit -m "描述改动" && git push
# GitHub Pages 自动部署，1-2 分钟生效
```

## 📝 数据库表结构

```sql
CREATE TABLE videos (
  id         TEXT PRIMARY KEY,
  title      TEXT NOT NULL,
  type       TEXT NOT NULL,   -- 'link' | 'local'
  source     TEXT,            -- 在线视频 URL
  local_id   TEXT,            -- IndexedDB key
  file_name  TEXT,
  file_size  BIGINT,
  sort_order INTEGER DEFAULT 0,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

## 📄 License

MIT
