# 16 拉回遠端最新內容：`fetch` 與 `pull`

> 協作時，不只要會推，也要會穩定地把別人的更新拉回來。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git fetch origin` | 抓回遠端最新資訊，但先不自動合併 |
| `git pull --rebase origin main` | 下載並把本地修改接到最新遠端之後 |

## 先抓資料但不急著合進來

```bash
git fetch origin
```

## 想直接同步主分支最新狀態

```bash
git pull --rebase origin main
```

## `fetch` 和 `pull` 的差別

| 指令 | 做了什麼 | 適合情境 |
| --- | --- | --- |
| `git fetch origin` | 只更新遠端資訊，不改你目前工作內容 | 你想先觀察 |
| `git pull --rebase origin main` | 抓下來並同步到目前分支 | 你準備正式更新 |

## 為什麼很多人喜歡 `pull --rebase`

| 好處 | 說明 |
| --- | --- |
| 歷史較乾淨 | 不容易出現一堆不必要的 merge commit |
| 同步流程直觀 | 先把遠端最新接上，再放回本地提交 |

## 很穩的同步習慣

```bash
git switch main
git fetch origin
git pull --rebase origin main
```

## 重要提醒

| 提醒 | 說明 |
| --- | --- |
| 如果團隊有固定流程，先跟團隊一致 | 有些團隊偏好 merge-based pull |
| 拉更新前先看工作區乾不乾淨 | 未提交修改可能影響同步流程 |

## 本章你要記住

1. `fetch` 比 `pull` 更保守，適合先觀察。
2. `pull --rebase` 是很常見的日常同步習慣。
3. 同步前先確認工作區乾淨，會少很多麻煩。
