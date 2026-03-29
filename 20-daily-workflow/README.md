# 20 每日 Git 工作流總複習

> 到這一章，你已經學完大多數人每天最常用的 Git 40% 核心操作了。接下來把它串成一套流程。

## 每天最常見的工作流

| 階段 | 核心指令 |
| --- | --- |
| 同步主線 | `git switch main`、`git pull --rebase origin main` |
| 開新工作 | `git switch -c feature/...` |
| 檢查與提交 | `git status`、`git diff`、`git add`、`git commit -m` |
| 推上遠端 | `git push -u origin <branch>` 或 `git push` |
| 合併收尾 | `git switch main`、`git merge <branch>` |

## 情境一：開始一個新功能

```bash
git switch main
git pull --rebase origin main
git switch -c feature/navbar-redesign
```

## 情境二：做功能時的日常節奏

```bash
git status
git diff
git add .
git diff --staged
git commit -m "Redesign navbar layout"
git push -u origin feature/navbar-redesign
```

## 情境三：做到一半突然要去修別的事

```bash
git stash
git switch main
git pull --rebase origin main
```

## 回來繼續原本工作

```bash
git switch feature/navbar-redesign
git stash pop
```

## 情境四：功能完成後合併回主線

```bash
git switch main
git merge feature/navbar-redesign
git push
git branch -d feature/navbar-redesign
```

## 情境五：發現剛剛那筆提交有問題

| 狀況 | 優先做法 |
| --- | --- |
| 還沒 push，少加檔案或訊息打錯 | `git commit --amend` |
| 已經 push，而且要安全撤銷 | `git revert <commit>` |
| 找不到剛剛的位置 | `git reflog` |

## 你現在最該背起來的高頻清單

| 類別 | 你最該熟的指令 |
| --- | --- |
| 觀察 | `git status`、`git log --oneline`、`git diff` |
| 提交 | `git add`、`git commit -m` |
| 分支 | `git switch -c`、`git switch`、`git merge` |
| 遠端 | `git clone`、`git remote -v`、`git push`、`git pull --rebase` |
| 修正 | `git restore`、`git stash`、`git commit --amend`、`git revert`、`git reflog` |

## 最後給你的實戰建議

1. 每次卡住先 `git status`。
2. 每次提交前先 `git diff`。
3. 每個功能開新分支，不要硬在 `main` 上直接改。
4. 已推送的歷史先優先用 `revert`，不要衝動做危險重寫。
5. 真的不知道剛剛發生什麼，就去看 `git log` 或 `git reflog`。

## 結語

你不需要一次把 Git 全部學完。  
只要把這 20 份教材裡的高頻流程練熟，就已經足以應付大多數個人專案與團隊開發場景。
123123123123123
1223