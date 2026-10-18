# 用 GitHub 在兩台電腦間同步工作

情境：辦公室電腦與家裡電腦輪流開發同一個專案。是的，最佳做法就是把專案放進 GitHub repository，兩邊各有一份完整副本，以 `push` / `pull` 同步。

## 1. 基本觀念

- **Repository (repo)**：專案資料夾加上完整版本歷史。
- **Local / Remote**：本機副本與 GitHub 上的副本（慣稱 `origin`）。
- **commit**：本機存一個版本快照。**push**：上傳到 GitHub。**pull**：從 GitHub 下載更新。
- 同步不是自動的，必須自己 commit + push / pull。

## 2. 一次性設定（每台電腦各做一次）

```bash
git config --global user.name  "你的名字"
git config --global user.email "你的 GitHub 帳號 email（或 noreply email）"
git config --global init.defaultBranch main
git config --global pull.rebase false
```

### 驗證身分（擇一）

- **HTTPS + Personal Access Token / Git Credential Manager**：最簡單。macOS 可用 `brew install gh` 後執行 `gh auth login`，依指示以瀏覽器登入即可。
- **SSH key**：`ssh-keygen -t ed25519 -C "your_email"`，把 `~/.ssh/id_ed25519.pub` 內容貼到 GitHub > Settings > SSH and GPG keys。clone 時用 `git@github.com:...` 網址。

## 3. 在 GitHub 網站建立 repository

1. 右上角 `+` > **New repository**。
2. **Repository name**：英文、小寫、連字號，例如 `my-design-project`。
3. **Visibility**：公司或未公開的設計程式選 **Private**。之後可改。
4. **不要**勾選 "Add a README"、".gitignore"、"license"（若本機已有專案要推上去，建立空的 repo 可避免首次 push 衝突）。若是全新專案，可勾選。
5. 按 **Create repository**，記下網址。

## 4. 把辦公室現有專案推上去

```bash
cd /path/to/project
git init
# 先建立 .gitignore（見第 6 節），再：
git add .
git status          # 檢查有沒有不該進去的檔案
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<帳號>/<repo>.git
git push -u origin main
```

## 5. 在家裡電腦取得專案

```bash
git clone https://github.com/<帳號>/<repo>.git
```

## 6. 日常工作流程

這是兩台電腦同步的核心習慣：

| 時機 | 動作 |
| --- | --- |
| 開始工作前 | `git pull` |
| 工作中 | 適時 `git add -A && git commit -m "說明"` |
| 離開電腦前 | `git push`（**一定要做，否則另一台看不到**） |

規則：

- 離開前一定要 push，開始前一定要 pull。
- 不要在兩台電腦同時改同一個檔案而不同步，否則會產生衝突。
- 發生衝突時，Git 會在檔案內標示 `<<<<<<<` / `=======` / `>>>>>>>`，手動修正後 `git add` 再 `git commit`。
- 若忘了 push，可用遠端連線回辦公室電腦補 push；不要用 USB 複製來取代。

## 7. 哪些檔案該放、哪些該排除

**該放**：

- 原始碼、腳本、設定範本
- 文件（README、說明）
- 建置設定（`package.json`、`requirements.txt`、`Makefile`、`pyproject.toml` 等）
- 小型的必要資源檔

**該排除**（寫入 `.gitignore`）：

- 可重新產生的內容：`node_modules/`、`__pycache__/`、`.venv/`、`build/`、`dist/`、編譯產物
- **機密**：`.env`、API key、密碼、憑證、`*.pem`
- 作業系統／編輯器雜物：`.DS_Store`、`.idea/`、`.vscode/`（可選擇保留共用設定）
- 大型檔案與資料：資料集、影片、大型二進位檔（單檔超過 100 MB GitHub 會拒收；需要時用 Git LFS）
- 個人暫存、日誌：`*.log`、`tmp/`

注意：

- 機密一旦 commit 並 push，即使之後刪除，歷史中仍然存在，要視為外洩並更換金鑰。
- 公司專案請先確認公司政策是否允許放到個人 GitHub。
- `.gitignore` 只對尚未追蹤的檔案有效；已加入的檔案需 `git rm --cached <檔案>`。

## 8. 建議的 repo 結構

```
my-project/
  README.md        專案說明、如何建置／執行
  .gitignore
  src/             原始碼
  docs/            文件
  tests/           測試
```

## 9. 常用指令速查

```bash
git status                 # 目前狀態
git diff                   # 尚未 commit 的修改
git log --oneline -10      # 最近歷史
git pull                   # 下載並合併
git push                   # 上傳
git restore <檔案>          # 放棄某檔未 commit 的修改
git stash / git stash pop  # 暫存未完成修改，先切換狀態
```

## 10. 在 VS Code 中操作

左側 Source Control 面板可完成 stage、commit、Sync Changes（等同 pull + push），狀態列左下角也會顯示待 push / pull 的數量。
