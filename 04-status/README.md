# 04 看懂目前狀態：`git status`

> Git 卡住時，第一個先打的指令，幾乎都應該是 `git status`。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git status` | 顯示完整狀態 |
| `git status -s` | 用短格式快速看重點 |

## 可直接複製

```bash
git status
git status -s
```

## 你會在 `status` 看到哪些狀態

| 狀態 | 意思 | 接下來通常做什麼 |
| --- | --- | --- |
| `untracked` | 新檔案還沒被 Git 追蹤 | `git add <file>` |
| `modified` | 已追蹤檔案被修改了 | `git diff` 或 `git add <file>` |
| `staged` | 已放進暫存區，準備提交 | `git commit -m "..."` |
| `nothing to commit` | 目前沒有可提交變更 | 可以切分支、push 或繼續工作 |

## 短格式怎麼看

| 代碼 | 意思 |
| --- | --- |
| `??` | 未追蹤檔案 |
| `M` | 已修改 |
| `A` | 已加入暫存區 |

## 一個很實用的檢查節奏

```bash
git status
git diff
git add README.md
git status
git commit -m "Update README"
git status
```

## 為什麼 `status` 這麼重要

| 你遇到的問題 | `git status` 幫你回答什麼 |
| --- | --- |
| 為什麼不能切分支 | 有沒有未提交修改 |
| 為什麼不能推送 | 你是不是還落後遠端 |
| 這個檔案有沒有進暫存區 | 狀態會直接寫出來 |
| 我現在到底在哪個分支 | 最上方通常就能看到 |

## 本章你要記住

1. 看不懂時先 `git status`，不要先亂試指令。
2. `git status -s` 適合你已經熟一點後快速掃描。
3. `status` 是 Git 日常操作的起點。
