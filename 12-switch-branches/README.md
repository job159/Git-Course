# 12 在分支之間切換：`git switch`

> 建了分支還不夠，你還要會穩定地切來切去。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git switch main` | 切回主分支 |
| `git switch feature/login-page` | 切到指定分支 |
| `git status` | 切換前先確認工作區是否乾淨，確認檔案處於哪個狀態 |
| `git switch --detach 版本號` | 切回舊版本 |
| `git switch -c 分支名稱 版本號` | 建新的分支並且切換到該分支,以該版本號出發 |

## 切換前先看狀態

```bash
git status
git switch main
git switch feature/login-page
```

## 切分支前，為什麼要先看 `status`

| 原因 | 說明 |
| --- | --- |
| 避免未提交修改跟著跑 | 某些修改可能不適合直接帶到別的分支 |
| 避免 Git 阻止你切換 | 如果會覆蓋檔案，Git 會擋下來 |
| 心裡比較清楚 | 你會知道自己現在是乾淨狀態還是半完成狀態 |

## 最常見的切換情境

| 情境 | 指令 |
| --- | --- |
| 回主線看最新版本 | `git switch main` |
| 回到剛才做到一半的功能分支 | `git switch feature/login-page` |

## 一個安全的習慣

```bash
git status
git add .
git commit -m "Save work in progress"
git switch main
```

## 如果你還不想提交

| 情況 | 建議 |
| --- | --- |
| 只是要暫時切走 | 之後會學到 `git stash` |
| 修改已完成 | 先提交再切換最穩 |

## 本章你要記住

1. 切分支前先 `git status`，這是很值得養成的習慣。
2. `git switch <branch>` 比舊式 `checkout` 更直覺。
3. 乾淨的工作區，會讓分支切換輕鬆很多。
