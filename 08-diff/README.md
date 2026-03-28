# 08 比較檔案差異：`git diff`

> 提交前先看差異，是 Git 使用者非常值得養成的習慣。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git diff` | 看工作區尚未暫存的差異 |
| `git diff --staged` | 看已暫存、準備提交的差異 |
| `git diff HEAD` | 看目前工作區和上一個提交相比差了哪些 |

## 可直接複製

```bash
git diff
git diff --staged
git diff HEAD
```

## 三種 diff 的差別

| 指令 | 比較的對象 | 什麼時候最有用 |
| --- | --- | --- |
| `git diff` | 工作區 vs 暫存區 | 你剛改完檔案，想看還沒 `add` 的內容 |
| `git diff --staged` | 暫存區 vs 最新提交 | 你想確認這次提交到底會送出什麼 |
| `git diff HEAD` | 工作區 vs 最新提交 | 想一次看全部本地變更 |

## 提交前最穩的流程

```bash
git status
git diff
git add README.md
git diff --staged
git commit -m "Refine README wording"
```

## 你在 diff 裡通常會看到什麼

| 標記 | 意思 |
| --- | --- |
| `-` | 被刪掉的內容 |
| `+` | 新增的內容 |
| 檔名 | 哪個檔案有改動 |

## 為什麼要養成先 diff 再 commit

| 好處 | 說明 |
| --- | --- |
| 防止誤提交 | 很多不小心加進去的除錯字串都會被先看到 |
| 提交更乾淨 | 你知道這筆提交到底在做什麼 |
| 降低回退成本 | 不會把無關修改混進去 |

## 本章你要記住

1. `git diff` 看還沒進暫存區的內容。
2. `git diff --staged` 看這次真的準備提交的內容。
3. 新手只要養成「先 diff 再 commit」，就會少踩很多坑。
