# entree-threads-media

席 Entrée Threads 圖文貼文的素材與排程佇列。**這個 repo 是公開的** —— Threads API 只能從公開網址抓圖。

n8n 工作流「[排程] Threads 圖文貼文」每小時讀一次 `queue.json`，把「時間已到、還沒發過」的貼文發出去（每次最多一篇）。

## 新增一篇貼文

1. 圖片放進 `images/`（JPEG/PNG、寬 320–1440px、≤ 8MB；1:1 建議 1080×1080）
2. 在 `queue.json` 的 `posts` 加一筆：

```json
{
  "id": "2026-10-03-chef-home",
  "publish_at": "2026-10-03T19:00:00+08:00",
  "images": ["images/2026-10-03-chef-home.jpg"],
  "text": "貼文內容，最多 500 字",
  "topic": "到府私廚"
}
```

| 欄位 | 規則 |
|---|---|
| `id` | 唯一、不可改（n8n 用它記錄「發過了」） |
| `publish_at` | ISO 時間，帶 `+08:00` |
| `images` | 1 張 = 單圖貼文；2–20 張 = 輪播 |
| `text` | ≤ 500 字，可省略 |
| `topic` | 選填，一篇一個，不要打 `#`，不能含 `.` 和 `&` |

3. 在 `n8n-threads/` 執行 `node scripts/push-media.mjs` —— 先驗證格式、圖片規格，通過才 commit + push。

改文案 = 改 `queue.json` 再 push 一次。已發出的貼文不會重發。
