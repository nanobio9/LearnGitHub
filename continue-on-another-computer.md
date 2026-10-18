# 在另一台電腦接手專案（參考或繼續修改）

## 情境 A：第一次在這台電腦取得專案

```bash
cd ~/local/Projects
git clone https://github.com/nanobio9/LearnGitHub.git
cd LearnGitHub
```

前置條件：已安裝 git、已設定身分、能通過 GitHub 驗證（見 [README.md](README.md) 第 2 節）。

```bash
git config --global user.name  "nanobio9"
git config --global user.email "kuokan.liang@gmail.com"
```

私有 repo 第一次 clone / push 時需登入：用瀏覽器登入的 Git Credential Manager，或 `gh auth login`。

## 情境 B：只想參考（唯讀）

- 直接 `git clone` 後閱讀，不要 commit，也不需 push。
- 之後要看最新內容：`git pull`。
- 不想安裝 git：在 GitHub 網頁上瀏覽，或 Code > Download ZIP。

## 情境 C：繼續修改並同步回去

```bash
git pull                      # 1. 開工前先取得最新
# ... 編輯檔案 ...
git status                    # 2. 看改了什麼
git add -A
git commit -m "說明這次改了什麼"
git push                      # 3. 收工前一定要推回去
```

回到原本那台電腦後，第一件事是 `git pull`。

## 情境 D：這台電腦已有舊副本

```bash
cd ~/local/Projects/LearnGitHub
git status                    # 先確認沒有未 commit 的修改
git pull
```

如果有未 commit 的修改：先 commit，或 `git stash` 暫存，再 `git pull`，最後 `git stash pop`。

## 常見問題

| 狀況 | 處理 |
| --- | --- |
| `git push` 被拒絕（rejected, non-fast-forward） | 另一台電腦有新 commit。先 `git pull`，解決衝突後再 `git push`。 |
| 合併出現 `<<<<<<<` 衝突標記 | 手動編輯成想要的內容，`git add <檔案>`，`git commit`。 |
| 驗證失敗 | 重新登入（`gh auth login`），或檢查 token / SSH key 是否有效。 |
| 不確定目前狀態 | `git status`、`git log --oneline -5`、`git remote -v`。 |
| 想放棄這台電腦的所有未 commit 修改 | `git restore .`（**會永久丟失修改，先確認**）。 |

## 專案環境還原

repo 只保存原始碼，不含 `.gitignore` 排除的內容。接手後依專案說明重建，例如：

- Python：`python3 -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt`
- Node.js：`npm install`
- 機密設定（`.env`）：不在 repo 裡，需自行從安全管道複製或重建。
