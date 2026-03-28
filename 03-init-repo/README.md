# 03 建立第一個版本庫：`git init`

> 當一個普通資料夾被 `git init` 過之後，它才會變成 Git 專案。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git init` | 在目前資料夾建立 Git 倉庫 |
| `git status` | 看初始化後的狀態 |

## 可直接複製

```bash
mkdir demo-repo
cd demo-repo
git init
git status
```

## 幫自己做一個最小練習專案

```bash
echo "# demo-repo" > README.md
git status
```

## `git init` 做了什麼

| 你看到的變化 | 意思 |
| --- | --- |
| 產生 `.git` 資料夾 | Git 把版本資料存在這裡 |
| 目前還沒有提交 | 只有倉庫骨架，還沒有版本歷史 |
| 檔案可能顯示為 untracked | Git 看見檔案了，但尚未追蹤 |

## 初始化後，最常見的下一步

| 順序 | 指令 |
| --- | --- |
| 1 | `git status` |
| 2 | `git add README.md` |
| 3 | `git commit -m "Initial commit"` |

## 什麼時候用 `git init`

| 場景 | 是否適合 |
| --- | --- |
| 你自己剛建立一個新專案 | 非常適合 |
| 團隊已經有遠端專案 | 通常改用 `git clone` |
| 你只是想在既有資料夾開始追蹤 | 很適合 |

## 新手常見錯誤

| 問題 | 原因 |
| --- | --- |
| 在錯誤資料夾執行 `git init` | 當前路徑沒確認好 |
| 初始化後以為自動有版本 | `init` 只建倉庫，不會自動 `commit` |
| 看不到 `.git` | 它通常是隱藏資料夾 |

## 本章你要記住

1. `git init` 是把普通資料夾變成 Git 專案。
2. `init` 完不代表有版本，還要 `add` 與 `commit`。
3. 初始化後第一件事幾乎永遠是 `git status`。
