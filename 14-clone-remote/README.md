# 14 複製專案與連接遠端：`clone`、`remote`

> 如果專案已經存在遠端，通常不是 `init`，而是直接 `clone`。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git clone <url>` | 把遠端專案完整複製到本機 |
| `git remote -v` | 查看目前遠端連線 |
| `git remote add origin <url>` | 幫本地專案加上一個遠端 |

## 如果遠端專案早就存在

```bash
git clone https://github.com/example/project.git
cd project
git remote -v
```

## 如果你是本地先做，現在才要接遠端

```bash
git remote add origin https://github.com/example/project.git
git remote -v
```

## `origin` 是什麼

| 名稱 | 意思 |
| --- | --- |
| `origin` | 遠端倉庫的預設名字，幾乎所有教學都會看到 |

## 什麼時候用哪一個

| 場景 | 指令 |
| --- | --- |
| 團隊已有專案 | `git clone <url>` |
| 你已在本機做完初版，現在才上傳 | `git remote add origin <url>` |

## 很多人第一次會搞混的地方

| 容易混淆 | 正確理解 |
| --- | --- |
| `git init` vs `git clone` | `init` 是自己建倉庫，`clone` 是拿現成倉庫 |
| Git vs GitHub URL | Git 只管版本，GitHub URL 是遠端位置 |

## 本章你要記住

1. 已經存在的團隊專案，通常直接 `clone`。
2. `git remote -v` 是檢查遠端設定最直接的方法。
3. `origin` 只是慣例名稱，不是特殊魔法。
