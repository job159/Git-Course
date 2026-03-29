# 10 讓垃圾檔案不要進 Git：`.gitignore`

> Git 很勤勞，但不是所有檔案都該被追蹤。編譯產物、快取、密碼檔，通常都不應該提交。

## 今天會用到的重點

| 項目 | 用途 |
| --- | --- |
| `.gitignore` | 宣告哪些檔案或資料夾不需要追蹤 |
| `git rm --cached <file>` | 讓已被追蹤的檔案停止被 Git 追蹤 |
| `git rm -r --cached <folder>` | 讓整個資料夾停止被 Git 追蹤 |

## 一個常見的 `.gitignore`

```gitignore
node_modules/
dist/
.env
.vscode/
*.log
a.txt
```

## 如果檔案之前已經被追蹤

```bash
git rm --cached .env
git commit -m "Stop tracking env file"
```

## 如果是一整個資料夾

```bash
git rm -r --cached dist
git commit -m "Stop tracking build output"
```

## `.gitignore` 只是在說什麼

| 規則 | 意思 |
| --- | --- |
| `node_modules/` | 忽略整個資料夾 |
| `*.log` | 忽略所有 `.log` 檔案 |
| `.env` | 忽略單一檔案 |

## 新手最容易踩的坑

| 問題 | 原因 | 解法 |
| --- | --- | --- |
| `.gitignore` 寫了卻沒效果 | 該檔案早就被追蹤了 | 用 `git rm --cached` 取消追蹤 |
| 害怕 `git rm` 把檔案刪掉 | 不知道 `--cached` 的作用 | `--cached` 只移除追蹤，不刪本機檔案 |
| 把密碼檔傳上去 | 太晚建立 `.gitignore` | 一開始就建立忽略規則 |

## 推薦習慣

1. 專案一開始就先建立 `.gitignore`。
2. 只提交原始碼與必要設定。
3. 編譯產物、相依套件、暫存檔不要交進 Git。

## 本章你要記住

1. `.gitignore` 很重要，因為它直接影響你的提交品質。
2. 已被追蹤的檔案，不會因為補寫 `.gitignore` 就自動消失。
3. `git rm --cached` 是處理這種情況的關鍵指令。
