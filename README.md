# Git 101：從零開始的版本控制大師

> 🎯 **目標受眾**：具備基本電腦操作能力，但對版本控制一竅不通的初學者
>
> ✅ **課程結束後，你將能夠**：獨立完成本地端存檔、回溯版本，並理解 Git 的基本工作流

---

## 📚 學習地圖

| 章節 | 主題 | 核心技能 |
|------|------|----------|
| 第 1 章 | 為什麼需要 Git？ | 觀念建立、環境安裝 |
| 第 2 章 | 存檔的藝術 | `add`、`commit`、`status`、`log` |
| 第 3 章 | 回到過去 | `checkout`、`reset`、`diff` |
| 第 4 章 | 平行宇宙 | `branch`、`merge` |
| 第 5 章 | 連接世界 | `remote`、`push`、`pull`、`clone` |

---

## 第一章：為什麼要學 Git？

### 1. 生活化案例：你一定遇過這些慘劇

想像你在寫一份重要的企劃書。你的桌面大概長這樣：

```
企劃書_最終版.docx
企劃書_最終版v2.docx
企劃書_最終版v2_主管修改.docx
企劃書_最終版v2_主管修改_真的最終.docx
企劃書_最終版v2_主管修改_真的最終_這次是真的.docx
```

**這就是沒有版本控制的世界。** 每個人都在用「改檔名」來假裝自己有版本管理。結果：

- 不知道哪個才是真正的最新版
- 想回到上週的版本？找不到了
- 和同事同時修改同一份？直接覆蓋彼此的工作

Git 解決的，正是這個問題。它就像一個**超級聰明的時光機**，幫你記錄每一次的改動，讓你隨時可以回到任何一個過去的狀態，而且不需要複製一堆檔案。

---

### 2. 核心觀念：Git 的三個區域

Git 的工作流程，可以用一個超好懂的生活比喻來理解：

> **Git 三區 = 廚師的出菜流程**

```
┌──────────────────┐     git add      ┌──────────────────┐    git commit    ┌──────────────────┐
│                  │ ────────────────▶ │                  │ ────────────────▶ │                  │
│     工作區        │                  │     暫存區        │                  │     儲存庫        │
│ Working Directory│                  │  Staging Area    │                  │   Repository     │
│                  │                  │                  │                  │                  │
│  📄 你正在編輯    │                  │  ✅ 準備好的改動  │                  │  🕐 永久版本紀錄  │
│  ✏️ 未追蹤的更動  │                  │  📦 等待打包      │                  │  🕐 每次 commit  │
│                  │                  │                  │                  │                  │
│  🍳 廚房料理台   │                  │  🍽️ 待出菜區     │                  │  📒 出菜記錄本   │
└──────────────────┘                  └──────────────────┘                  └──────────────────┘
```

**三區的關係一句話總結**：

你在工作區修改檔案 → 用 `git add` 把改動搬進暫存區「打包」→ 用 `git commit` 把這包改動永久存入儲存庫，並附上說明文字。

---

### 3. 環境準備與基礎指令

#### 安裝 Git

| 系統 | 安裝方式 |
|------|----------|
| **macOS** | 終端機輸入 `git --version`，有版本號即已安裝；否則前往 [git-scm.com](https://git-scm.com) 下載 |
| **Windows** | 前往 [git-scm.com](https://git-scm.com) 下載 Git for Windows，安裝時一路 Next |
| **Linux (Ubuntu)** | `sudo apt install git` |

#### 第一次使用必做：設定你的身分

Git 每次 commit 都會記錄「是誰做的」，所以先告訴 Git 你是誰：

```bash
git config --global user.name "你的名字"
git config --global user.email "your@email.com"
```

> 💡 這只需要做**一次**，之後所有專案都會沿用這個設定。

#### 101 階段必學的 5 個指令

| 指令 | 功能 | 比喻 |
|------|------|------|
| `git init` | 初始化一個新的 Git 儲存庫 | 在資料夾裡安裝「監視器」 |
| `git status` | 查看目前三區的狀態 | 問「現在有什麼變動？」 |
| `git add <檔案>` | 將改動放入暫存區 | 把菜放到待出菜區 |
| `git commit -m "說明"` | 將暫存區的內容永久記錄 | 出菜，並在記錄本寫下說明 |
| `git log` | 查看所有 commit 歷史 | 翻閱記錄本 |

---

### 4. 實戰演練：你的第一個 Commit

跟著下面的步驟，完整走一遍「工作區 → 暫存區 → 儲存庫」的流程。

#### 步驟一：建立你的第一個 Git 專案

```bash
# 建立一個新資料夾並進入
mkdir my-first-repo
cd my-first-repo

# 讓 Git 開始追蹤這個資料夾
git init
```

你應該看到：

```
Initialized empty Git repository in .../my-first-repo/.git/
```

這代表 Git 已經在這個資料夾裡安裝好「監視器」了。有一個隱藏的 `.git` 資料夾被建立，所有的版本歷史都會存在裡面。

---

#### 步驟二：建立你的第一個檔案

```bash
# 建立一個 README.md 檔案
echo "# 我的第一個 Git 專案" > README.md
```

現在查看狀態：

```bash
git status
```

你會看到：

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present
```

> 📌 `Untracked files` 就是 Git 在說：「我看到這個新檔案，但你還沒叫我追蹤它。」

---

#### 步驟三：把檔案放進暫存區

```bash
git add README.md
```

再次查看狀態：

```bash
git status
```

輸出變成：

```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
```

> 📌 `Changes to be committed` 代表這個檔案已經在「待出菜區」了，隨時可以 commit。

---

#### 步驟四：你的第一個 Commit！

```bash
git commit -m "feat: 新增 README.md，開始我的第一個 Git 專案"
```

輸出：

```
[main (root-commit) a1b2c3d] feat: 新增 README.md，開始我的第一個 Git 專案
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

> 💡 **Commit 訊息怎麼寫才好？**
>
> 一個好的 commit 訊息應該完成這個句子：「如果套用這個 commit，它將會...（你的訊息）」。
>
> - ✅ `feat: 新增用戶登入功能`
> - ✅ `fix: 修正首頁排版錯誤`
> - ❌ `改了一些東西`（沒有意義，未來的你會後悔）

---

#### 步驟五：查看你的版本歷史

```bash
git log
```

你會看到：

```
commit a1b2c3d4e5f6... (HEAD -> main)
Author: 你的名字 <your@email.com>
Date:   Sun Mar 15 10:00:00 2026

    feat: 新增 README.md，開始我的第一個 Git 專案
```

那串英數字（`a1b2c3d...`）叫做 **commit hash**，是這個版本的「身分證字號」，全宇宙唯一。

想看精簡版歷史？用：

```bash
git log --oneline
```

#### 完整流程回顧

```
建立/修改檔案（工作區）
         │
         │  git add README.md
         ▼
   暫存區等待確認
         │
         │  git commit -m "說明"
         ▼
  永久存入儲存庫 ✓
```

---

### 5. 常見新手坑洞與解法

#### 🕳️ 坑洞一：commit 之後發現訊息寫錯了

```bash
# 修改最後一次 commit 的訊息（還沒 push 之前才能用）
git commit --amend -m "正確的訊息"
```

---

#### 🕳️ 坑洞二：`git add` 了不該加的檔案

```bash
# 把檔案從暫存區移回工作區（不會刪掉檔案本身）
git restore --staged 檔案名稱
```

---

#### 🕳️ 坑洞三：想忽略某些檔案（例如密碼、node_modules）

在專案根目錄建立一個 `.gitignore` 檔案，把不想追蹤的檔名或資料夾寫進去：

```
# .gitignore 範例
node_modules/
.env
*.log
.DS_Store
```

> ⚠️ 這是**非常重要**的習慣，尤其在處理含有密碼或 API 金鑰的 `.env` 檔案時。Git 會自動忽略列在這裡的所有檔案。

---

#### 🕳️ 坑洞四：搞不清楚現在在哪個狀態

`git status` 是你的最佳朋友。任何時候迷失了，就打一次 `git status`，它會告訴你現在的狀態，以及下一步可以做什麼。

---

#### 🕳️ 坑洞五：`git log` 卡住出不來

`git log` 預設會用 `less` 分頁顯示，按 **`q`** 即可退出。

---

### 本章小結

恭喜你完成了 Git 101 的第一章！你現在已經會：

- [x] 理解為什麼需要版本控制
- [x] 知道工作區、暫存區、儲存庫的關係
- [x] 完成你的第一個 `git init`、`git add`、`git commit`
- [x] 用 `git log` 查看歷史

> 🧠 **第一章的核心心法**：Git 的本質是「有意識地記錄你的決定」。每個 commit 都應該代表一個有意義的進展，而不只是「存檔」。

#### 課後練習

打開你現在正在做的任何一個專案資料夾，對它執行 `git init`，然後試著把所有現有的檔案做第一個 commit。感受一下「這個專案的起點」被永久記錄下來的感覺。

---

> **下一章預告 →** 學會了存檔，現在來學怎麼**回到過去**——用 `git diff` 看改了什麼、用 `git restore` 反悔最近的修改，以及理解 `HEAD` 這個神秘指標到底是什麼。

---

*課程設計：Git 101 教學團隊 ｜ 以類比教學法讓版本控制不再神秘*
