# 06 建立提交：`git commit`

> `commit` 是 Git 最核心的動作。沒有提交，就沒有真正的版本歷史。

## 今天只學這個指令

| 指令 | 用途 |
| --- | --- |
| `git commit -m "訊息"` | 建立一筆提交並附上訊息 |

## 可直接複製

```bash
git add README.md
git commit -m "Add project README"
```

## 一個完整的新手最穩流程

```bash
git status
git diff
git add README.md
git status
git commit -m "Update README introduction"
git status
```

## 提交訊息怎麼寫最好

| 不好的寫法 | 較好的寫法 |
| --- | --- |
| `update` | `Update README introduction` |
| `fix` | `Fix login validation bug` |
| `change file` | `Rename settings page title` |

## 一句好記的公式

| 公式 | 範例 |
| --- | --- |
| 動詞 + 對象 + 目的 | `Add login form validation` |

## 什麼樣的提交最漂亮

| 原則 | 意思 |
| --- | --- |
| 一次只做一件事 | 方便看歷史、方便回退 |
| 訊息可讀 | 讓三天後的你也看得懂 |
| 提交前先確認差異 | 不要把暫存區當黑盒子 |

## 新手常見錯誤

| 問題 | 原因 | 怎麼避免 |
| --- | --- | --- |
| 明明改了東西卻 commit 不進去 | 忘了先 `git add` | 提交前先看 `git status` |
| 提交太大包 | 一次做太多事 | 用小提交拆開 |
| 訊息太模糊 | 沒說明改了什麼 | 用「動詞 + 對象」 |

## 本章你要記住

1. `commit` 才是 Git 真正記錄版本的時間點。
2. 好的提交訊息，能大幅提升你之後看歷史的效率。
3. 提交前先看 `status` 與 `diff`，幾乎永遠不吃虧。
