# 21 Git 速查表：什麼時候用什麼招

> 這章是急救卡。  
> 你不需要先背所有細節，只要先分清楚你現在卡在「檔案、暫存區、commit、還是遠端」，就比較不容易下錯指令。

## 先記一句話

| 指令 | 先把它想成什麼 |
| --- | --- |
| `git pull` | 抓遠端更新 |
| `git restore` | 救回檔案內容，或把 staged 拿下來 |
| `git reset` | 撤銷 add / commit / 把狀態往回拉 |

| `git revert` | 安全撤銷已經 push 的 commit |

## 先判斷你現在是哪一層出問題

| 你現在的問題 | 第一個該想到的指令 |
| --- | --- |
| 檔案改壞了、刪掉了、想恢復內容 | `git restore` |
| `git add` 加錯，想取消 staged | `git restore --staged` |
| commit 錯了，但內容還想留 | `git reset` |
| commit 錯了，而且內容也不要了 | `git reset --hard` |
| 已經 push，想安全撤銷 | `git revert` |
| 只是想抓遠端最新內容 | `git pull` |

## 25 個最常見情境

### A. 檔案內容救回

#### 情境 01：你把單一檔案改壞了，想回到目前 commit 的版本

```bash
git restore main.c
```

效果：`main.c` 會回到目前 `HEAD` 的版本。

#### 情境 02：你手動刪掉本地檔案，想救回來

```bash
git restore main.c
```

效果：把被刪掉的已追蹤檔案從目前 commit 還原回來。

#### 情境 03：你把整個工作區改亂了，全部想恢復

```bash
git restore .
```

效果：把目前工作區所有已追蹤檔案恢復成目前 commit 的狀態。

#### 情境 04：只有某個資料夾改亂了，想只救那一區

```bash
git restore src/
```

效果：只還原 `src/` 底下已追蹤檔案的內容。

#### 情境 05：你本地刪檔後，想用 `git pull` 救回來

這時先不要用 `git pull`。  
如果遠端沒有新的 commit，`pull` 不會幫你補回本地刪掉的檔案。

```bash
git restore .
```

效果：直接從目前 commit 把檔案救回來。

### B. `git add` / staged 問題

#### 情境 06：你只把單一檔案 add 錯了，想取消 staged

```bash
git restore --staged secret.txt
```

效果：`secret.txt` 離開暫存區，但檔案修改還留在工作區。

#### 情境 07：你用了 `git add .`，結果加太多檔案

```bash
git restore --staged .
```

效果：全部取消 staged，但不會刪掉你本地修改。

#### 情境 08：你想把 staged 拿下來，再重新精準 add

```bash
git restore --staged .
git add README.md
git add app.js
```

效果：先清空暫存區，再把正確檔案重新加回去。

#### 情境 09：你這個檔案 staged 錯了，而且連工作區修改也不要

```bash
git restore --staged config.yml
git restore config.yml
```

效果：先把 `config.yml` 拿出暫存區，再把檔案內容還原。

#### 情境 10：你整批 `add` 錯了，而且連全部修改都不想留

```bash
git restore --staged .
git restore .
```

效果：暫存區清掉，工作區也回到目前 commit 的版本。

### C. commit 錯了，但還沒 push

#### 情境 11：最後一筆 commit 訊息寫錯了

```bash
git commit --amend -m "Correct commit message"
```

效果：改掉最後一筆 commit 訊息，不用多生一筆 commit。

#### 情境 12：最後一筆 commit 少加一個檔案

```bash
git add missing-file.txt
git commit --amend --no-edit
```

效果：把漏掉的檔案補進最後一筆 commit，訊息維持不變。

#### 情境 13：你想撤銷最後一次 commit，但保留修改在工作區

```bash
git reset HEAD~1
```

或：

```bash
git reset --mixed HEAD~1
```

效果：最後一筆 commit 取消，staged 也取消，但檔案修改還在。

#### 情境 14：你想撤銷最後一次 commit，但保留 staged 狀態

```bash
git reset --soft HEAD~1
```

效果：最後一筆 commit 取消，修改還在，而且仍然維持 staged。

#### 情境 15：你想撤銷最後一次 commit，連修改也不要

```bash
git reset --hard HEAD~1
```

效果：最後一筆 commit 取消，staged 清掉，工作區修改也一起消失。

#### 情境 16：你想直接回到某個指定 commit

先找 commit id：

```bash
git log --oneline
```

再退回去：

```bash
git reset --hard <commit_id>
```

效果：本機直接回到指定版本。  
提醒：這會把該 commit 之後的本地狀態一起丟掉。

### D. commit 已經 push 出去了

#### 情境 17：最後一筆 commit 已經 push，但你想安全撤銷

```bash
git revert HEAD
```

效果：不刪原本 commit，而是新增一筆反向 commit 把它抵消掉。

#### 情境 18：不是最後一筆，而是想安全撤銷某個舊 commit

```bash
git revert <commit_id>
```

效果：指定那筆 commit 會被反向抵消，歷史仍然保留。

#### 情境 19：你已經 push 到自己的分支，而且確定可以改遠端歷史

```bash
git reset --hard HEAD~1
git push --force-with-lease
```

效果：本機和遠端都退回上一版。  
提醒：這適合你自己的分支；多人共享分支不要當成日常操作。

#### 情境 20：你已經 push 多筆，但想安全撤銷其中幾筆

先找出要撤銷的 commit：

```bash
git log --oneline
```

再對要取消的每一筆各跑一次：

```bash
git revert <commit_id_1>
git revert <commit_id_2>
```

效果：每一筆要取消的 commit 都會多出一筆對應的反向 commit。

### E. 遠端同步與急救

#### 情境 21：你只是想抓遠端最新版本

```bash
git pull
```

效果：把遠端最新更新拉回目前分支。

#### 情境 22：你想先抓遠端資訊，但先不要合併進來

```bash
git fetch origin
```

效果：只更新遠端資訊，不會直接改你目前工作區。

#### 情境 23：你 `git push` 被拒，因為遠端比你新

```bash
git pull
git push
```

效果：先把遠端更新拉下來，再把你的提交推上去。  
如果團隊習慣 `rebase`，也可以改用 `git pull --rebase`。

#### 情境 24：GitHub 上現在是對的，你想讓本機完全變成遠端那版

```bash
git fetch origin
git reset --hard origin/main
git clean -nd
git clean -fd
```

效果：本機已追蹤檔案會對齊遠端，未追蹤檔案也能清掉。  
提醒：如果你的主要分支叫 `master`，請把 `origin/main` 改成 `origin/master`。

#### 情境 25：你剛剛 `reset` 過頭了，想找回剛才的位置

先看 HEAD 最近去過哪裡：

```bash
git reflog
```

再退回剛才的位置：

```bash
git reset --hard HEAD@{1}
```

效果：很多剛剛手滑退掉的狀態，還有機會先靠 `reflog` 找回來。

## 三組最容易搞混的差別

### `git restore` 跟 `git reset` 差在哪裡

| 指令 | 主要用途 | 最常見情境 |
| --- | --- | --- |
| `git restore` | 還原檔案內容、取消 staged | 檔案改壞、檔案刪掉、`add` 加錯 |
| `git reset` | 把 Git 狀態往回拉 | 撤銷 add、撤銷 commit、回到舊版本 |

### `git reset` 跟 `git revert` 差在哪裡

| 指令 | 會不會改歷史 | 最常見情境 |
| --- | --- | --- |
| `git reset` | 會 | 還沒 push，想重做或丟掉 commit |
| `git revert` | 不會 | 已經 push，想安全撤銷變更 |

### `git pull` 跟 `git restore` 差在哪裡

| 指令 | 主要用途 | 不適合拿來做什麼 |
| --- | --- | --- |
| `git pull` | 抓遠端最新更新 | 修本地被你刪掉或改壞的檔案 |
| `git restore` | 從目前 commit 還原本地檔案 | 抓遠端新版本 |

## 最常見情境對照表

| 你現在遇到的情況 | 直接先想這招 |
| --- | --- |
| 本地檔案改壞，想恢復 | `git restore 檔名` |
| 本地檔案刪掉，想救回 | `git restore 檔名` |
| 全部檔案恢復 | `git restore .` |
| `git add` 加錯，取消 staged | `git restore --staged 檔名` |
| 全部取消 staged | `git restore --staged .` |
| 撤銷最後一次 commit，但保留修改 | `git reset HEAD~1` |
| 撤銷最後一次 commit，但保留 staged | `git reset --soft HEAD~1` |
| 撤銷最後一次 commit，連修改都不要 | `git reset --hard HEAD~1` |
| 已 push，安全撤銷 commit | `git revert HEAD` |
| 想抓遠端最新版本 | `git pull` |

## 十秒版懶人包

```bash
# 檔案救回
git restore .

# 取消 add
git restore --staged .

# 撤銷最後一次 commit，但保留修改
git reset HEAD~1

# 撤銷最後一次 commit，連修改也丟掉
git reset --hard HEAD~1

# 已 push，安全撤銷
git revert HEAD
```

## 本章你要記住

1. 檔案問題，先想 `restore`。
2. `add` 或 commit 問題，先想 `reset`。
3. 已經 push，又想安全撤銷，先想 `revert`。
4. 只是想抓遠端最新內容，才是 `pull`。
5. 真的怕退錯，先看下一章的安全判斷，再動手。
