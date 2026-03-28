# 24 Git 常見卡住情境排解：切不走、拉不下、推不上怎麼辦

> 你會 Git，不代表 Git 不會卡你。  
> 很多時候不是你不懂指令，而是你看到錯誤訊息會慌。這一章把最常見的卡住情境整理成「看到什麼、代表什麼、下一步做什麼」。

## 先講結論：Git 卡住時，先看這幾個

```bash
git status
git branch --show-current
git log --oneline --decorate -5
git stash list
```

| 指令 | 你要確認什麼 |
| --- | --- |
| `git status` | 工作區乾不乾淨、是否在衝突中 |
| `git branch --show-current` | 你現在到底站在哪個分支 |
| `git log --oneline --decorate -5` | 最近幾筆提交與分支指向 |
| `git stash list` | 你有沒有把東西收進 stash 過 |

## 一張先用的快速判斷表

| 你看到的情況 | 最常見原因 | 優先做法 |
| --- | --- | --- |
| 切 branch 被擋 | 本機有未提交修改 | `commit`、`stash` 或 `restore` |
| `git pull` 被擋 | 本機工作區不乾淨，或遠端有更新衝突 | 先清工作區，再 `pull --rebase` |
| `git push` 被拒 | 遠端比你新 | 先 `fetch` / `pull --rebase`，再 push |
| merge 衝突 | 同一段內容被不同地方改了 | 手動解衝突、`add`、完成 merge |
| stash pop 衝突 | stash 內容和目前檔案撞到了 | 手動解衝突、`add`、再繼續工作 |
| branch 刪不掉 | Git 判定那個分支還沒合併 | 確認是否真的要刪，再用 `-d` 或 `-D` |
| `git push` 說沒有 upstream | 這個分支還沒綁定遠端 | `git push -u origin <branch>` |
| Detached HEAD | 你切到某個 commit，不是在正式 branch 上 | 切回 branch，或立刻開新 branch 保留工作 |

## 情境一：想切分支，但 Git 不讓你切

### 常見訊息

你可能會看到類似：

- Your local changes would be overwritten by checkout
- Please commit your changes or stash them before you switch branches

### 這代表什麼

Git 發現你目前工作區有修改。  
如果直接切走，可能會把檔案蓋掉或讓你狀態更亂，所以它先擋下來。

### 解法一：這些修改已經做完，先 commit

```bash
git status
git add .
git commit -m "Save work before switching"
git switch main
```

### 解法二：只是暫時切走，先 stash

```bash
git stash -u
git switch main
```

回來時：

```bash
git switch 原本分支
git stash pop
```

### 解法三：這些修改不要了，直接丟掉

```bash
git restore .
```

如果 staged 了，先拿下來：

```bash
git restore --staged .
git restore .
```

## 情境二：`git pull` 被拒，因為你本機有修改

### 常見訊息

你可能會看到類似：

- Please commit your changes or stash them before you merge
- cannot pull with rebase: You have unstaged changes

### 這代表什麼

你現在本機工作區不乾淨。  
Git 不知道要先更新遠端，還是先保留你本機的半成品，所以先停下來。

### 比較穩的做法

先看狀態：

```bash
git status
git diff
```

如果修改還要：

```bash
git stash -u
git pull --rebase origin main
git stash pop
```

如果修改已完成：

```bash
git add .
git commit -m "Save local work"
git pull --rebase origin main
```

如果修改不要了：

```bash
git restore .
git pull --rebase origin main
```

## 情境三：`git push` 被拒，因為遠端比你新

### 常見訊息

你可能會看到：

- rejected
- non-fast-forward
- Updates were rejected because the remote contains work that you do not have locally

### 這代表什麼

遠端分支已經有比你更新的內容。  
也就是說，你不能直接把自己的版本硬推上去蓋掉它。

### 一般最穩的處理流程

```bash
git fetch origin
git pull --rebase origin main
git push
```

如果你不是在 `main`，就改成你目前分支對應的名稱。

### 為什麼要先 `pull --rebase`

因為它會先把遠端最新接上，再把你的提交放回去。  
這通常比直接亂 force push 穩很多。

### 什麼時候才考慮 force push

| 情況 | 要不要考慮 |
| --- | --- |
| 你自己的私人功能分支 | 可以非常小心地考慮 |
| 團隊共享分支 | 通常不要 |

如果真的要強推，優先想：

```bash
git push --force-with-lease
```

## 情境四：合併時發生衝突

### 常見訊息

你可能看到：

- CONFLICT
- Automatic merge failed

### 這代表什麼

同一個檔案、甚至同一段內容，在不同分支被改成不同版本，Git 無法替你自動決定要留哪個。

### 標準解法

先看有哪些衝突：

```bash
git status
```

打開衝突檔，你會看到像這樣：

```text
<<<<<<< HEAD
你的版本
=======
對方版本
>>>>>>> feature/x
```

### 你要做什麼

1. 手動把檔案改成你要的最終內容。
2. 刪掉 `<<<<<<<`、`=======`、`>>>>>>>` 這些標記。
3. 存檔。
4. 告訴 Git 這個檔案已處理完。

```bash
git add app.js
git commit
```

### 這裡最重要的觀念

Git 衝突不是叫你二選一而已。  
很多時候正確答案是把兩邊內容重新整理成第三種版本。

## 情境五：`pull --rebase` 時發生衝突

### 常見訊息

你可能會看到：

- could not apply ...
- Resolve all conflicts manually, mark them as resolved with git add

### 這代表什麼

遠端最新內容和你的本地提交撞到了。  
因為你用的是 `pull --rebase`，所以解完衝突後不是直接 `commit`，而是要繼續 rebase 流程。

### 解法

```bash
git status
```

手動修好衝突檔後：

```bash
git add 衝突檔案
git rebase --continue
```

如果又出現下一個衝突，就再重複一次。

### 如果你想放棄這次 rebase

```bash
git rebase --abort
```

這會把你帶回 rebase 開始前的狀態。

## 情境六：`git stash pop` 之後發生衝突

### 這代表什麼

你 stash 起來的內容，和現在分支上的檔案狀態撞到了。

### 解法

先看狀態：

```bash
git status
```

打開衝突檔，手動修好之後：

```bash
git add 衝突檔案
```

如果你要把修完的結果正式存成版本：

```bash
git commit -m "Resolve stash pop conflict"
```

### 一個很重要的小提醒

`stash pop` 發生衝突時，stash 有時不會被刪掉。  
所以你可以再看一下：

```bash
git stash list
```

不要以為它一定已經消失。

## 情境七：branch 刪不掉

### 常見訊息

你可能會看到：

- The branch 'feature/x' is not fully merged

### 這代表什麼

Git 認為這個分支還有提交沒有被合進目前分支。  
它怕你誤刪，所以先擋下來。

### 如果它已經合併完成

先切到正確分支，再刪：

```bash
git switch main
git branch -d feature/x
```

### 如果你很確定不要了，就強制刪

```bash
git branch -D feature/x
```

### 判斷重點

| 做法 | 什麼時候用 |
| --- | --- |
| `git branch -d` | 分支已合併，正常刪除 |
| `git branch -D` | 你知道它還沒合併，但就是不要了 |

## 情境八：`git push` 說沒有 upstream branch

### 常見訊息

你可能會看到：

- The current branch has no upstream branch

### 這代表什麼

你目前這個本地分支，還沒綁定對應的遠端分支。  
所以 Git 不知道你 `git push` 是要推去哪裡。

### 解法

```bash
git push -u origin feature/login-page
```

之後這個分支通常就可以直接：

```bash
git push
```

## 情境九：不小心進到 Detached HEAD

### 常見情況

你可能不是故意的，只是：

- 切到某個舊 commit 看看
- 用了某些舊教學裡的 `checkout <commit>`
- 看 log 後跳到某個歷史點

### 這代表什麼

你現在不是站在正式 branch 上，而是暫時停在某個提交點。

### 如果你只是看一下，想回正常分支

```bash
git switch main
```

或回到上一個分支：

```bash
git switch -
```

### 如果你已經在 Detached HEAD 上做了修改，想保留

```bash
git switch -c rescue/from-detached-head
```

這樣就會立刻開一個新分支，把你現在的位置和修改接住。

## 最後一張整體判斷表

| 你現在卡在哪 | 第一反應不要做什麼 | 優先做什麼 |
| --- | --- | --- |
| 切分支被擋 | 不要亂 `reset --hard` | 先判斷要 commit、stash 還是 restore |
| pull 被擋 | 不要直接亂 merge | 先清工作區，再 pull |
| push 被拒 | 不要先衝動 force push | 先 fetch / pull --rebase |
| merge 衝突 | 不要只刪整個檔案了事 | 先看衝突標記，整理成最終版本 |
| stash pop 衝突 | 不要以為 stash 一定消失 | 解衝突後再看 `stash list` |
| branch 刪不掉 | 不要直接 `-D` 當習慣 | 先確認是不是已合併 |
| Detached HEAD | 不要繼續亂做一堆提交 | 要嘛切回 branch，要嘛立刻開新 branch |

## 本章你要記住

1. Git 卡住時，先看 `status`，不要先慌。
2. 很多問題不是 Git 壞掉，而是它在保護你不要覆蓋本機或遠端內容。
3. `pull` 被拒、`push` 被拒、切分支被擋，背後通常都是「你的狀態和別人的狀態還沒對齊」。
4. 衝突不是災難，重點是修內容、`add`，再完成當下流程。
5. 看到不熟的訊息時，先判斷自己卡在工作區、提交、分支、還是遠端這一層。
