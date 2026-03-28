# 09 修改錯了怎麼還原：`git restore`

> 新手很常問：「我改壞了，能不能回到上一個乾淨狀態？」答案通常是可以。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git restore <file>` | 捨棄工作區中尚未提交的修改 |
| `git restore --staged <file>` | 把檔案從暫存區拿回來，但保留工作區修改 |

## 捨棄工作區修改

```bash
git restore README.md
```

## 把檔案從暫存區拿回來

```bash
git restore --staged README.md
```

## 兩者差在哪裡

| 指令 | 影響範圍 | 結果 |
| --- | --- | --- |
| `git restore README.md` | 工作區 | 直接丟掉尚未提交的修改 |
| `git restore --staged README.md` | 暫存區 | 不提交它，但工作區修改還在 |

## 很常見的兩個場景

| 場景 | 你該用哪個 |
| --- | --- |
| 我改壞了，不想留 | `git restore <file>` |
| 我只是暫時不想把它放進這次提交 | `git restore --staged <file>` |

## 安全操作流程

```bash
git status
git diff
git restore --staged README.md
git status
```

## 重要提醒

| 提醒 | 說明 |
| --- | --- |
| `git restore <file>` 會丟掉未提交修改 | 執行前先看 `git diff` |
| `--staged` 比較溫和 | 它只把檔案從暫存區拿出來 |
| 先不要碰危險回退指令 | 新手先把 `restore` 與後面的 `revert` 學熟就好 |

## 本章你要記住

1. 工作區還原與暫存區還原是兩回事。
2. 捨棄修改前，先看 `git diff` 很重要。
3. `git restore --staged` 是拆分提交時很好用的工具。
