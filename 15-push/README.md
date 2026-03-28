# 15 把版本推上遠端：`git push`

> 你本機 commit 完，不代表團隊看得到；要 `push` 才會同步到遠端。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git push -u origin main` | 第一次把 `main` 推上遠端並設定追蹤 |
| `git push` | 之後的常規推送 |
| `git push origin feature/login-page` | 把指定分支推上遠端 |

## 第一次推主分支

```bash
git push -u origin main
```

## 第一次推功能分支

```bash
git push -u origin feature/login-page
```

## 之後就可以簡化成

```bash
git push
```

## `-u` 是在做什麼

| 選項 | 意思 |
| --- | --- |
| `-u` | 設定 upstream，之後這個本地分支知道自己要對應哪個遠端分支 |

## 最常見推送場景

| 場景 | 指令 |
| --- | --- |
| 專案剛建立完 | `git push -u origin main` |
| 新功能分支第一次上傳 | `git push -u origin feature/login-page` |
| 同一分支後續更新 | `git push` |

## 推送前的穩定節奏

```bash
git status
git log --oneline
git push
```

## 本章你要記住

1. `commit` 只在本機，`push` 才會送到遠端。
2. 第一次推某個分支，用 `git push -u origin <branch>` 最方便。
3. 設好 upstream 之後，日常多半只要 `git push`。
