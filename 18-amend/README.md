# 18 修正最近一次提交：`git commit --amend`

> 很多時候你不是要回退整個歷史，你只是剛剛那一筆 commit 少放了一個檔案，或訊息打錯。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git commit --amend --no-edit` | 補內容到上一筆提交，但不改訊息 |
| `git commit --amend -m "新訊息"` | 重寫上一筆提交訊息 |

## 我忘記把一個檔案加進上一筆提交

```bash
git add README.md
git commit --amend --no-edit
```

## 我上一筆提交訊息打錯了

```bash
git commit --amend -m "Fix login validation message"
```

## `amend` 最適合的情境

| 情境 | 適不適合 |
| --- | --- |
| 剛做完 commit，立刻發現少一個檔案 | 很適合 |
| 剛做完 commit，訊息太爛 | 很適合 |
| 這筆 commit 已經推上去且團隊可能已經拉了 | 要小心，先避免 |

## 非常重要的提醒

| 提醒 | 說明 |
| --- | --- |
| `amend` 會改寫最近一筆提交 | 本質上是重做上一筆 |
| 尚未推送時最安全 | 已共享歷史先不要隨便改 |

## 一個標準修正流程

```bash
git status
git add README.md
git diff --staged
git commit --amend --no-edit
git log --oneline -1
```

## 本章你要記住

1. `amend` 是修正最近一筆提交的高頻工具。
2. 還沒 push 的提交，用 `amend` 很方便。
3. 已經分享出去的歷史，不要隨便用 `amend` 改。
