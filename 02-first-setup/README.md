# 02 第一次設定 Git 身分

> 你每次提交的作者名稱與 email，都是從這裡來的。

## 今天只學這些指令

| 指令 | 用途 |
| --- | --- |
| `git config --global user.name "你的名字"` | 設定全域作者名稱 |
| `git config --global user.email "you@example.com"` | 設定全域作者 email |
| `git config --global init.defaultBranch main` | 新倉庫預設分支改成 `main` |
| `git config --global --list` | 檢查目前全域設定 |

## 可直接複製

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global --list
```

## 這些設定到底在做什麼

| 設定項目 | 作用 | 建議 |
| --- | --- | --- |
| `user.name` | 顯示在提交紀錄裡的作者名稱 | 用真實名稱或團隊慣用名稱 |
| `user.email` | 顯示在提交紀錄裡的作者 email | 盡量和 GitHub/GitLab 綁定信箱一致 |
| `init.defaultBranch` | `git init` 後預設分支名稱 | 建議直接用 `main` |

## `--global` 是什麼意思

| 層級 | 影響範圍 | 什麼時候用 |
| --- | --- | --- |
| `--global` | 你的電腦上所有 Git 專案 | 個人預設值 |
| 不加 `--global` | 只有目前這一個專案 | 公司專案需要不同 email 時 |

## 如果某個專案想用不同 email

```bash
git config user.name "Work Name"
git config user.email "work@example.com"
git config --list
```

## 新手常見錯誤

| 問題 | 原因 | 怎麼做 |
| --- | --- | --- |
| 提交作者名字不對 | 沒先設定 `user.name` | 重新設定 config |
| GitHub 看不到頭像 | email 跟平台綁定的不一致 | 改成平台註冊信箱 |
| 每個專案都重設一次 | 不知道有 `--global` | 先設全域，特殊專案再覆蓋 |

## 本章你要記住

1. `git config --global` 是第一次安裝後最該先做的事。
2. 作者資訊會一直跟著你的提交紀錄。
3. `git config --global --list` 是最方便的檢查方式。
