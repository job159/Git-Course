# Git 40% 高頻指令 · 互動教學

> 學會這 40% 的 Git 指令，就能應付日常 80% 的情境。一頁式互動教學，附**動畫圖解**與白話解說。

## 🔗 線上直接看

### 👉 https://job159.github.io/Git-Course/

點開即用，免安裝。手機、電腦皆可，支援深色模式。

---

## 這是什麼

一份用單一 `index.html` 寫成的互動 Git 教學網頁，鎖定新手最常卡關的核心概念，**邊點邊看動畫**理解，而不是只讀文字：

- 🎯 **HEAD 怎麼移動**、`reset` 三種模式（soft / mixed / hard）
- 🌿 **分支怎麼「岔出去」**、`merge` vs `rebase` 差在哪
- 🗺️ **四個核心區域**（工作區 → 暫存區 → 本機 → 遠端）跟著一個檔案跑一遍
- 🔀 `git diff` 在比哪兩區、`git stash` 把改動收進口袋
- 🔺 Fork / origin / upstream 三角關係（貢獻別人專案的完整流程）
- 🚫 `.gitignore` 規則語法（`*`、`?`、`[]`、`**`、`!` 反向）動手玩
- 💬 「說人話」白話框：把難概念講清楚，不是空話

全部圖解都是 SVG、會動（指標滑動、線條畫出來），並隨深色模式自動切換。

## 本機預覽

這是純靜態網頁，用任何方式開 `index.html` 都行。內建設定用 Python 起一個本機伺服器：

```bash
python -m http.server 8011
# 然後打開 http://localhost:8011
```

## 內容章節

倉庫同時附了分章說明（`01-…` ～ `24-…` 各資料夾的 `README.md`），對應網頁裡的單元：

| 主題 | 涵蓋 |
|------|------|
| 基礎 / 設定 | 四區觀念、`git config`、起手式第一次上傳 |
| 常用指令 | `init` / `status` / `add` / `commit` / `log` / `diff` / `restore` / `.gitignore` |
| 分支 | `branch` / `switch` / `merge`、merge vs rebase |
| 版本移動 | `HEAD`、`reset` / `revert` / `reflog` |
| 遠端協作 | `clone` / `remote` / `fetch` / `pull` / `push`、Fork → PR |
| 急救 | `stash` / `commit --amend`、常見手滑情境補救 |

---

歡迎 Fork、開 PR 或回報問題 🙌
