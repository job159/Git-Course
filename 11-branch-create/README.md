# 11 分支入門：不要再直接在 `main` 上亂改

> 分支的核心價值很簡單：把不同工作切開，讓主線保持穩定。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git branch` | 查看目前有哪些本地分支 |
| `git switch -c <branch>` | 建立新分支並立刻切過去 |

## 先看自己目前在哪裡

```bash
git branch
git branch --show-current
```

## 建立一條功能分支

```bash
git switch -c feature/login-page
git branch
```

## 為什麼新功能應該開分支

| 原因 | 說明 |
| --- | --- |
| 主線更穩 | `main` 保持乾淨，不被半成品污染 |
| 修改更清楚 | 每個功能一條分支，歷史更好讀 |
| 方便協作 | 團隊可以並行做不同任務 |

## 分支命名建議

| 類型 | 範例 |
| --- | --- |
| 功能 | `feature/login-page` |
| 修 bug | `fix/navbar-overlap` |
| 文件 | `docs/update-readme` |

## 很推薦的新手流程

```bash
git switch main
git switch -c feature/profile-page
git branch --show-current
```

## 本章你要記住

1. 有新工作就先開分支，別直接在 `main` 上改。
2. `git switch -c <branch>` 是非常高頻、非常重要的日常指令。
3. 分支名稱越清楚，未來越好維護。
