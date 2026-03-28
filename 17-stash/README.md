# 17 暫時收起未完成工作：`git stash`

> 有時你做到一半，突然要切去修別的東西；這時 `stash` 非常好用。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git stash` | 暫時收起目前修改 |
| `git stash list` | 看有哪些 stash 紀錄 |
| `git stash pop` | 把最近一筆 stash 套回來並移除 |
| `git stash -u` | 連未追蹤檔案一起收起來 |

## 最常見用法

```bash
git stash
git switch main
```

## 回來繼續做時

```bash
git switch feature/login-page
git stash pop
```

## 看目前 stash 清單

```bash
git stash list
```

## 如果你有新檔案也想一起藏起來

```bash
git stash -u
```

## 什麼時候特別適合用 stash

| 場景 | 為什麼適合 |
| --- | --- |
| 突然要切去處理緊急 bug | 不想為了半成品硬 commit |
| 只是暫時中斷 | `stash` 比開臨時提交更乾淨 |

## `stash` 與 `commit` 怎麼選

| 情況 | 建議 |
| --- | --- |
| 工作已經完整可描述 | 優先 `commit` |
| 只是暫停，內容還很亂 | 用 `stash` |

## 本章你要記住

1. `stash` 是「暫放」，不是正式版本。
2. 回來工作時，最常用的是 `git stash pop`。
3. 如果有未追蹤檔案，記得考慮 `git stash -u`。
