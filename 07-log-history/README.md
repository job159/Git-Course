# 07 查看歷史版本：`git log`

> 你不只要會提交，還要會回頭看歷史。這就是 `git log` 的價值。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git log --oneline` | 用簡短格式看提交歷史 |
| `git log --oneline --graph --decorate --all` | 用圖形化方式看所有分支歷史 |

## 最常用版本

```bash
git log --oneline
```

## 想看得更清楚時

```bash
git log --oneline --graph --decorate --all
```

## 你會看到什麼

| 欄位 | 意思 |
| --- | --- |
| 短 SHA | 每筆提交的短識別碼 |
| 提交訊息 | 這筆版本做了什麼 |
| `HEAD` / 分支名稱 | 目前指標在哪裡 |

## 什麼時候用哪一種

| 情境 | 指令 |
| --- | --- |
| 只想快速看最近做了什麼 | `git log --oneline` |
| 想看分支怎麼分叉、怎麼合併 | `git log --oneline --graph --decorate --all` |

## 實用流程

```bash
git status
git log --oneline
git log --oneline --graph --decorate --all
```

## 為什麼歷史很重要

| 場景 | 你需要 `log` 幫什麼忙 |
| --- | --- |
| 找出哪次改壞了 | 先定位提交 |
| 準備回退 | 你得先知道要回哪一筆 |
| 檢查某個功能是否已合併 | 看歷史圖最直觀 |

## 本章你要記住

1. `git log --oneline` 是最適合日常使用的歷史檢查方式。
2. 看到分支歷史亂時，用 `--graph --decorate --all` 幾乎最有幫助。
3. 會看歷史，才算真的開始會用 Git。
