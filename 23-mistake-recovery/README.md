# 23 Git 常見手滑補救教學：`add` 錯、`commit` 錯、`push` 錯怎麼辦

> 真正常見的不是你不會 Git，而是你手太快。  
> 這一章把最常見的幾種「不小心」拆成可操作步驟：先判斷錯在哪一層，再選最安全的補救方式。

## 先看你是錯在哪一層

| 你手滑在哪裡 | 常見症狀 | 優先處理方式 |
| --- | --- | --- |
| `add` 錯 | 檔案進了暫存區，但還沒 commit | `git restore --staged` |
| commit 訊息寫錯 | 內容對，但訊息不對 | `git commit --amend -m` |
| commit 內容錯一部分 | 多交了、少交了某些檔案 | `git commit --amend` 或 `git reset` |
| 整筆 commit 都錯 | 這筆提交本身就不該存在 | 未 push 用 `git reset`，已 push 看情況用 `revert` 或 force push |
| push 錯 | 遠端已經出現不該有的提交 | 私人分支可考慮重寫，共享分支優先 `revert` |

## 手滑之後，先不要急著亂打指令

先看這四個：

```bash
git status
git diff
git diff --staged
git log --oneline -3
```

| 指令 | 你要看什麼 |
| --- | --- |
| `git status` | 現在有沒有未提交修改、哪些檔案 staged 了 |
| `git diff` | 工作區還有哪些修改 |
| `git diff --staged` | 這次 commit 真正會交出去的是什麼 |
| `git log --oneline -3` | 最近幾筆提交長什麼樣 |

## 情境一：不小心 `git add` 到錯的檔案

### 狀況

你本來只想 `add` 一個檔案，結果多加了一個不該進這次 commit 的檔案。

### 做法

```bash
git status
git diff --staged
git restore --staged secret.txt
git status
```

### 效果

| 會發生什麼 | 說明 |
| --- | --- |
| `secret.txt` 會離開暫存區 | 這次 commit 不會包含它 |
| 檔案內容還留在工作區 | 你沒有把修改刪掉，只是先不交 |

### 什麼時候很適合用

| 情況 | 適不適合 |
| --- | --- |
| 只是 staged 錯了 | 很適合 |
| 你還想保留檔案修改 | 很適合 |

## 情境二：完全 `git add .` 到錯誤東西

### 狀況

你一個 `git add .` 下去，結果很多不該進這次 commit 的檔案都被 staged 了。

### 做法

```bash
git status
git diff --staged
git restore --staged .
git status
```

然後再重新精準加入：

```bash
git add README.md
git add app.js
git diff --staged
```

### 效果

| 會發生什麼 | 說明 |
| --- | --- |
| 暫存區會被清空 | staged 的東西都先拿回工作區 |
| 檔案修改還在 | 你可以重新挑正確檔案 `add` |

### 如果你連工作區修改也不要了

先把它從暫存區拿下來，再丟掉工作區內容：

```bash
git restore --staged .
git restore .
```

這一步才會真的把內容丟掉，所以執行前要很確定。

## 情境三：不小心 commit 訊息寫錯

### 狀況

內容是對的，但你 commit 訊息：

- 打錯字
- 寫太爛
- 寫成完全不同意思

### 還沒 push 的做法

```bash
git commit --amend -m "Correct commit message"
```

### 已經 push，但這是你自己的分支

```bash
git commit --amend -m "Correct commit message"
git push --force-with-lease
```

### 已經 push，而且是共享分支怎麼辦

這裡要特別注意：

如果只是訊息不好看、打錯字、語氣不漂亮，通常**不要為了改訊息去重寫共享歷史**。

比較穩的做法是：

- 先接受這筆 commit 訊息已經存在
- 在 PR、說明文字、下一筆 commit 補充真正意思
- 如果真的嚴重到必須改，先和團隊確認流程

### 為什麼

因為「只改訊息」這件事，本質上還是在改寫那筆 commit。  
如果它已經是共享歷史，風險通常大於收益。

## 情境四：不小心 commit 到錯誤檔案或少了一些檔案

### 狀況

這一筆 commit 不是完全錯，但有問題，例如：

- 多交了一個不該進來的檔案
- 少交了一個應該一起進來的檔案
- 這筆 commit 的內容想重新整理

### 少一個檔案，但整體大致正確

```bash
git add missing-file.txt
git commit --amend --no-edit
```

### 多交了或少交了好幾個檔案，想重整這筆 commit

```bash
git reset HEAD~1
git status
git add 正確檔案
git diff --staged
git commit -m "正確的提交訊息"
```

### 為什麼用 `git reset HEAD~1`

| 原因 | 說明 |
| --- | --- |
| 這筆 commit 會先拆掉 | 讓你重新整理內容 |
| 修改還保留在工作區 | 不會直接丟掉內容 |
| 比 `--hard` 溫和很多 | 適合你還想救內容的情況 |

## 情境五：完全 commit 到錯誤東西

### 狀況

你剛剛那筆 commit 根本整筆都不該存在。

### 如果還沒 push，而且內容還可能有用

```bash
git reset HEAD~1
```

效果：

| 會發生什麼 | 說明 |
| --- | --- |
| commit 會被拆掉 | 這筆提交不見了 |
| 檔案修改還留在工作區 | 你還能重新整理、挑內容、重新 commit |

### 如果還沒 push，而且內容完全不要了

```bash
git status
git log --oneline -3
git reset --hard HEAD~1
```

效果：

| 會發生什麼 | 說明 |
| --- | --- |
| commit 會退掉 | 最新一筆提交會消失 |
| 工作區也一起回退 | 那些修改也會消失 |

### 這裡的重點

如果你只是 commit 錯，不代表一定要 `--hard`。  
只要內容還可能有用，先想 `git reset HEAD~1`，通常更安全。

## 情境六：完全 push 到錯誤東西

這是最需要先判斷「是不是共享分支」的情況。

### 狀況 A：已經 push，但這是你自己的功能分支

```bash
git log --oneline -3
git reset --hard HEAD~1
git push --force-with-lease
```

### 什麼時候可以這樣做

| 情況 | 適不適合 |
| --- | --- |
| 這個分支主要只有你自己在用 | 比較適合 |
| 沒有人基於這個分支繼續開發 | 比較適合 |
| 你很確定要把遠端那筆改掉 | 才適合 |

### 狀況 B：已經 push，而且是共享分支

```bash
git log --oneline -3
git revert HEAD
git push
```

### 為什麼這是比較安全的答案

| 原因 | 說明 |
| --- | --- |
| 不改寫遠端歷史 | 原本錯的 commit 還在，但會被反向取消 |
| 對隊友比較友善 | 別人 pull 時不會遇到歷史突然改掉 |
| 可追蹤 | 大家看 log 能知道這筆錯誤是怎麼被修掉的 |

## 情境七：完全 add 錯、commit 錯、push 錯，我現在超亂

### 先做這一套

```bash
git status
git log --oneline --decorate -5
git diff
git diff --staged
```

然後用這張表判斷：

| 你目前位置 | 最先做什麼 |
| --- | --- |
| 還沒 commit，只是 staged 錯 | `git restore --staged` |
| commit 了但還沒 push，內容還想留 | `git reset HEAD~1` |
| commit 了但還沒 push，內容完全不要 | `git reset --hard HEAD~1` |
| 已 push 到私人分支 | `git reset --hard` + `git push --force-with-lease` |
| 已 push 到共享分支 | `git revert` |

## 一個超實用判斷口訣

先問自己三件事：

1. 你是 `add` 錯，還是 `commit` 錯，還是 `push` 錯？
2. 那筆東西有沒有已經 push？
3. 內容你還要不要留？

對應原則：

| 如果你想要的結果 | 優先想法 |
| --- | --- |
| 只是不要 staged | `git restore --staged` |
| 只是重做 commit | `git commit --amend` 或 `git reset` |
| 不想丟內容，只想拆掉 commit | `git reset HEAD~1` |
| 內容和提交都不要了 | `git reset --hard` |
| 已經是共享歷史 | `git revert` 比較穩 |

## 本章你要記住

1. `add` 錯，通常先用 `git restore --staged`，不是直接 `reset --hard`。
2. commit 訊息寫錯，還沒 push 用 `git commit --amend -m` 最直覺。
3. commit 內容錯，但內容還想保留，優先用 `git reset HEAD~1`。
4. 完全 push 錯東西時，先判斷是不是共享分支；共享分支通常優先 `revert`。
5. 手滑補救最重要的不是背最多指令，而是先判斷：現在錯在哪一層、內容要不要留、遠端有沒有人一起用。
