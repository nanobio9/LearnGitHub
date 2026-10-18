# GitHub Pages：發布 HTML 網頁給別人看

GitHub Pages 會把 repo 中的靜態檔案（HTML、CSS、JS、圖片）發布成網站，網址為：

- 專案網站：`https://nanobio9.github.io/<repo 名稱>/`
- 個人網站（特殊 repo 名稱 `nanobio9.github.io`）：`https://nanobio9.github.io/`

## 重要注意事項

- **發布出去的網站是公開的**，即使 repo 是 Private（個人帳號）。不要放機密或不想公開的內容。
- 免費帳號只能對 **Public** repo 使用 Pages；Private repo 使用 Pages 需要付費方案。
- 只支援靜態內容，不能執行伺服器端程式。
- 有使用量上限（網站大小約 1 GB、每月流量約 100 GB 的軟上限），不適合大型下載站。
- 檔名與路徑區分大小寫（`Index.html` 與 `index.html` 不同）。

## 步驟一：準備 HTML

repo 根目錄（或 `docs/` 資料夾）必須有 `index.html`，這是首頁。

```
my-site/
  index.html
  about.html
  style.css
  images/
```

最小範例：

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="utf-8">
  <title>我的網頁</title>
</head>
<body>
  <h1>Hello, GitHub Pages</h1>
</body>
</html>
```

內部連結請用相對路徑（`style.css`、`images/a.png`），不要用 `/style.css`，否則專案網站會找不到檔案。

## 步驟二：上傳（push）

```bash
git add -A
git commit -m "Add site"
git push
```

不習慣指令的話，也可以在 GitHub 網頁 Add file > Upload files 上傳。

## 步驟三：啟用 Pages

1. 打開 repo 頁面 > **Settings** > 左側 **Pages**。
2. **Source** 選 **Deploy from a branch**。
3. **Branch** 選 `main`，資料夾選 `/ (root)` 或 `/docs`，按 **Save**。
4. 約一到數分鐘後，頁面上方會出現網站網址。可在 repo 的 **Actions** 分頁看部署進度。

## 步驟四：更新網站

修改 HTML 後 commit 並 push 到 `main`，Pages 會自動重新發布，通常一到數分鐘生效。若沒更新，強制重新整理瀏覽器（Cmd+Shift+R）。

## 管理

- **自訂網域**：Settings > Pages > Custom domain，並在 DNS 設定 CNAME；建議勾選 **Enforce HTTPS**。
- **下線網站**：Settings > Pages，把 Source 改為 None，或將 repo 設為 Private（免費帳號會因此停止發布）。
- **Jekyll**：Pages 預設會用 Jekyll 處理。純 HTML 不需要，可在發布根目錄放一個空檔 `.nojekyll`，避免以底線開頭的資料夾被忽略。
- **部署失敗**：到 Actions 分頁查看紅色失敗項目的日誌。
- **404**：確認有 `index.html`、分支與資料夾設定正確、檔名大小寫相符。

## 與 Markdown 搭配

只想寫文章的話，可以直接把 `.md` 放進 repo；Pages 預設會用 Jekyll 把 `.md` 轉成網頁。需要完整控制排版時，才自己寫 HTML。
