---
title: Git 學習筆記
date: 2022-01-20
tags:
 - [Git]
categories: "學習筆記"
---



以前在初接觸到網頁領域的時候，偶爾會聽到版本控制一詞，當時的理解是把每一次完成編輯的專案，當作一次的版本發佈到網路空間呈現出來；雖然表面上確實是如此，但是實際在接觸到 Git 之後，發現所謂的版本控制並不是這麼單純；為了拓展自己的技能樹，這幾天幾乎是把時間都投資在 Git 的學習上，也藉由這次機會把學到的知識做個紀錄。

*備註：本篇僅記錄這段時間學習 Git 所做的筆記，主要是用來回顧與找資料，沒有過多專有名詞的解釋，內容會視情況更新。*

<!--more-->



## 環境安裝：

在進入正題之前，必須先根據電腦的作業系統安裝 [Git](https://git-scm.com/) 的環境，安裝成功後在 Git 資料夾中會有 "Git Bash" 的應用程式，可以透過它來操作 Git 指令；或是使用電腦本身就有的命令提示字元 / 終端機也可以，依使用者習慣為主。

## Command Line 基本指令：

除了 Git 指令，通常還會搭配一些命令提示字元的原生指令去輔助操作，常用的 command line 指令如下：

### **移動到指定目錄**

指令：`cd 資料夾路徑`

如果覺得直接輸入路徑很麻煩，可以直接將資料夾拖曳至 Git Bash 視窗中。

### **展開當前目錄**

指令：`ls`

列出當前目錄下的所有檔案。

### **回前一層目錄**

指令：`cd ..`

從當前位置回到上一層目錄。

### 回到根目錄

指令：`cd\`

無論在哪個位置，都會移動到根目錄。

### **新增資料夾**

指令：`mkdir 資料夾名稱`

在當前目錄下建立一個資料夾。

### **新增檔案**

指令：`touch 檔案名稱.檔案類型`

在當前目錄建立一個檔案，以 index 網頁為例，即輸入 `touch index.html`

### 刪除檔案

指令：`del 檔案名稱.檔案類型`


## Git 版本檢視：

環境安裝完成後開啟終端機，嘗試輸入指令 `$ git version` ，如果若出現 Git 版本訊息即表示安裝成功。

版本訊息：`git version 2.32.0.windows.2`

## Git 環境設定：

使用 Git 進行版本控制時，每一個更新紀錄的更新人、更新時間以及一些詳細的內容都會被記錄；因此在開始使用 Git 之前，會需要先設定一些基本的資訊。

### **設定使用者暱稱**

指令：`$ git config --global user.name "使用者暱稱"`

### **設定使用者信箱**

指令：`$ git config --global user.email "使用者信箱"`

### **查看設定值**

指令：`$ git config --list`

## Git 常用指令：

Git 基本操作流程：

![](https://i.imgur.com/zLOR8aN.png)

Git 操作方式會根據不同情境而有所差異，但是原理大同小異；這裡開始會先以本地端會使用到的指令說明，並且皆使用 Git Bash 來操作。

### **建立本地儲存**

指令：`$ git init`

儲存庫（Repositiory）基本上就是用來存放一個專案所有檔案內容的資料夾。建立方式只需要開啟終端機，並以任意資料夾作為根目錄輸入上述指令即可。

以檔名為 Project 的資料夾為例，建立成功後會出現以下訊息：

```
$ git init
Initialized empty Git repository in C:/Users/user/Desktop/Project/.git/
```

並且可以發現資料夾的路徑資訊後方出現 `(master)`  字樣，這個是資料夾在 Git 初始化成功時所產生的預設分支（branch）。

此外也可以開啟初始化的資料夾，查看根目錄是否多出一個 .git 的資料夾，它主要是用於監控與紀錄初始化資料夾內檔案的所有變動；通常 .git 資料夾預設會隱藏，可以從資料夾上方列的 "檢視" 開啟隱藏的項目。

*備註：為了方便閱讀，後續皆以 "儲存庫" 來表示初始化的資料夾。*

### **檢查狀態**

指令：`$ git status`

查看工作目錄與暫存區的內容是否有變動；能檢視那些檔案被修改，那些檔案沒有被 Git 追蹤到。

以 Project（本地端儲存庫）為例，初始化後透過指令查看狀態時出現以下訊息，表示尚未做任何新增或編輯：

```
$ git status
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```



### **加入暫存區**

指令：`$ git add .`

工作目錄有新增或編輯的檔案時，可以使用指令將檔案加入暫存區 / 索引，並等待提交（commit）成一次新的版本。

嘗試在 Project 新增檔案 index.html，並使用指令 `$ git status` 查看狀態時出現以下訊息：

```
$ git status
On branch master

No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        index.html

nothing added to commit but untracked files present (use "git add" to track)
```

從第 5 行可以得知，已偵測到尚未追蹤的檔案，而第 7 行為檔案的名稱；此時可以使用指令 `$ git add .` 將其加入暫存區。

透過指令將 index.html 加入暫存區後，再使用 `$ git status` 查看狀態時出現以下訊息：

```
$ git status
On branch master

No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   index.html
```

從第 5 行可以得知，新增檔案的動作被儲存到暫存區，並等待 commit 成一次新版本。

補充說明，使用指令 `$ git status` 查詢狀態時，檔案如果是新增在工作目錄的，狀態會是 `Untracked files`，表示尚未加入暫存區追蹤；但如果是已被加入到暫存區，後續再進行修改的檔案，狀態會是 `Changes not staged for commit`，並且檔案前面會有 `modified` 的字樣，表示該檔案已被編輯，需要再次加入暫存區等待提交。

以下列出不同狀態所代表的意思：

```
Changes to be committed        # 已加入暫存區，準備 commit 成一次新的版本紀錄。
Changes not staged for commit  # 已被追蹤，但尚未加入至暫存區。
Untracked file                 # 新建立，尚未被追蹤的檔案。
```



### **提交更新**

指令：`$ git commit -m "編輯內容說明"`

將加入暫存區的檔案提交一次新的版本。

先前做了新增檔案 index.html 並加入暫存區；現在嘗試將暫存區的內容提交成新的版本，輸入指令 `$ git commit -m "新增 index"` 後，如果出現以下訊息表示成功提交。

```
$ git commit -m "新增 index"
[master (root-commit) ce9d927] 新增 index
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 index.html
```

可以再透過指令 `$ git status` 查看狀態，出現以下訊息表示沒有更動需要 commit，並且目前工作目錄非常乾淨，完全沒有新增、修改或刪除。

```
$ git status
On branch master
nothing to commit, working tree clean
```



### **查詢提交紀錄**

指令：`$ git log`

查看詳細的歷史 commit 資訊。

前一個指令進行了 commit 的動作，現在嘗試輸入指令 `$ git log` 來查詢提交紀錄，從訊息內容可以得知詳細的提交資訊，如下所示：

```
$ git log
commit ce9d92723df3302156a9523453c0fff21f7fdcc0 (HEAD -> master)
Author: Cliff Chu <wave99487chu@gmail.com>
Date:   Sun Jan 23 17:53:08 2022 +0800

    新增 index
```

如果要退出畫面，按下鍵盤 "q" 即可返回原內容。



### **忽略檔案**

不希望某些檔案被 Git 追蹤時，可以新增一個 ".gitignore" 檔案，透過程式碼編輯器將該檔案開啟後，直接輸入要被忽略的檔案名稱和檔案類型並儲存即可；這裡要注意的是 .gitignore 本身不會被 Git 忽略。

被忽略檔案的輸入格式分別有以下幾種，這裡以 index.html 為例：

**忽略單一檔案**

輸入格式：`index.html`

**忽略檔案類型**

輸入格式：`*.html`

**忽略單一資料夾**

輸入格式：`html/`

### **取消加入暫存區**

將暫存區的檔案回到尚未執行 `$ git add .` 的狀態，相關指令分為下列幾種：

**取消單一檔案加入暫存**

指令：`$ git reset HEAD 檔案名稱`

**取消所有檔案加入暫存**

指令：`$ git reset HEAD`



### 還原檔案資料內容

將檔案內容回復到最後一次 commit 的狀態，可視情況使用以下指令：

**還原單一檔案**

指令：`$ git checkout 檔案名稱.檔案類型`

**還原工作目錄與暫存區**

指令：`$ git reset --hard`

需要注意的是，空的資料夾本身會被 git 忽略，因此使用指令後不會被還原；但是資料夾內有存在檔案時，就會被 git 追蹤並還原。

### 刪除提交紀錄

刪除 commit 的相關指令分別為以下幾種：

**刪除最後一次的提交紀錄**

指令：`$ git reset --hard HEAD^`

需要注意的是，該指令除了刪除最新的提交紀錄之外，當前加入暫存區的檔案也會被刪除，但是當前工作目錄的檔案不受影響。

**還原刪除的最後一次提交紀錄**

指令：`$ git reset --hard ORIG_HEAD`

還原 `$ git reset --hard HEAD^` 刪除的 commit，並刪除當前加入暫存區的檔案，但是當前工作目錄不受影響。

<!--hard 模式不管如何都會刪除暫存區的檔案，待查證-->

**刪除最後一次的提交紀錄時保留異動內容**

指令：`$ git reset --soft HEAD^ `

可以理解成沒有做最後一次 commit 的動作；而當前的狀態會變成 commit 前的所有狀態，加上執行該指令前的所有狀態。

<!-- 敘述可以更明確 -->

### 修改提交紀錄

依修改的內容可視情況使用以下指令：

**修改最後一次提交紀錄**

指令：`$ git commit --amend -m "修改內容"`

該指令僅能夠修改最後一次的 commit 訊息，如果要修改更早之前的，則需要使用 `$ git rebase` 相關指令。

<!-- rebase 待加入描述 -->

**追加檔案到最後一次提交紀錄**

指令：`$ git commit --amend --no-edit`

執行該指令時如果未加上 `--no-edit` 參數，會跳出 Vim 編輯視窗，因此建議加上。

## 分支介紹

分支（branch）是開發者在團隊協作一個專案時，會需要使用到的 Git 架構內容；可以理解成將現階段儲存庫的所有檔案複製一份，而這兩個版本可以獨立修改各自的內容。這樣的好處是如果檔案的內容需要做更動，就可以先建立一個分支並在上面進行修改，同時也不用擔心影響到原始的檔案內容。

以下列出常見分支名稱與主要用途：

- master 預設分支 - 合併時才會產生 commit
- develop 開發分支 - 合併時才會產生 commit
- feature 開發新功能分支

開始進入分支之前，必須先對 HEAD 指標有些概念，可以把它理解成目前所在的分支；也可以在資料夾路徑看到目前 HEAD 的指向，如下圖，可以看到目前 HEAD 正指向 master 分支。

```
user@DESKTOP-LV1TU83 MINGW64 ~/Desktop/Project (master)
```



### 檢視當前所有分支

指令：`$ git branch`

用來查詢目前本地端儲存庫的所有分支。

這裡以 Project 為例，透過上述指令查詢後出現以下訊息；可以得知目前本地端有兩個分支，分別為 develop 與 master，而標示星號的分支表示目前 HEAD 指向的分支。

```
$ git branch
  develop
* master
```



### 移動 HEAD 到指定 commit 位置

指令：`$ git checkout 序號前4碼`

用來查看某一次的 commit 內容；而 commit 序號可透過 `$ git log` 查詢，如下所示：

```
commit ce9d92723df3302156a9523453c0fff21f7fdcc0
Author: Cliff Chu <wave99487chu@gmail.com>
Date:   Sun Jan 23 17:53:08 2022 +0800

    新增 index
```

接著輸入指令 `$ git checkout ce9d` 就能夠移動 HEAD 到這次 commit，如果要回到原本位置，只需要輸入 `$ git checkout 當前分支名稱` 即可。

### 建立分支

指令：`$ git branch 分支名稱`

建立一個新的分支，但是 HEAD 的指向不變。

### 切換當前分支

指令：`$ git checkout 分支名稱`

HEAD 的指向會移動到指定分支的最後一次 commit 上。

### 合併分支 

可視情況使用以下兩種合併模式：

**快轉模式（Fast-Forward）**

指令：`$ git merge 合併的分支名稱`

快轉模式下，兩分支合併後回重疊，並且不會產生額外的 commit。

如果合併前兩個分支的最後 commit 位置相同，合併時就會產生快轉模式（Fast - Forward），以下面這張圖為例：

![](https://i.imgur.com/uRzQ0zw.png)

圖中左邊的部分為 master 分支與 feature 分支合併前的狀態；可以得知 feature 分支的 commit 有兩個，依序為 add css 與 add js，而 master 分支並沒有做變動；接著再將 HEAD 切換回 master 分支後，執行指令 `$ git merge feature` ，狀態變為右邊的部分，可以發現合併後 master 分支的位置移動到 feature 分支 add js 的 commit 上，也就是說兩個分支的差別只是 feature 分支多了兩次 commit 而已，因為兩者都是同一個 commit 分出去的，所以 master 分支有的內容 feature 分支也都有；因此 master 分支只是在合併自己未來的版本而已，而這種合併模式就稱為 "快轉模式（Fast - Forward）"。

**非快轉模式（--no-ff）**

指令：`$ git merge 合併的分支名稱 --no-ff`

非快轉模式下，兩分支合併後會產生額外的 commit。

前面提到分支在合併時，如果兩者最後一次的 commit 位置相同，合併時就會進行快轉模式；雖然兩者最後都會合併到同一個 commit 上，但是以開發者的角度來思考，都會盡可能的區隔每個分支所做的工作，這也是分支的意義；因此如果要更明確的區隔兩個分支，就可以使用指令 `$ git merge 合併的分支名稱 --no-ff`，來關閉快轉模式進行合併；以下面這張圖為例：

![](https://i.imgur.com/KFo2foP.png)

圖中左邊的部分所做的事情與前一張圖是相同的，但是兩個分支在關閉快轉模式進行合併時，可以發現 master 分支與 feature 分支合併後，額外產生出一個新的 commit，但是 feature 分支並沒有與 master 分支重疊，並且進行路線停留在合併前最後一次的 commit 位置 add js 上，而這種合併模式就稱為 ＂非快轉模式＂。

<!-- 也許內容可以描述更明確 -->

補充說明一點，使用指令 `$ git merge 合併的分支名稱 --no-ff` 來合併兩個分支時，可能會出現以下的訊息：

![](https://i.imgur.com/6MWIo44.png)

如果跳出上圖的訊息，代表需要輸入合併的原因；此時可以按下鍵盤 " i " 進入輸入模式，接著把黃色文字的部分改成合併原因的簡單說明（同 commit 訊息），輸入完畢後再按下鍵盤 " Esc " 退出輸入模式，最後在最下方輸入 `：wq` 即可回到原來的畫面。

**手動合併（衝突）**

內容待編輯。

## 標籤

開發專案的過程中，會因為檔案不斷新增、編輯或刪除而增加許多 commit，雖然每個 commit 都有自己的版本內容，但是這樣很難去區分彼此之間的重要程度，比方說不同時期的上線版本；因此為了有效區別，可以使用標籤的相關指令。

標籤和分支一樣都是指向某一個 commit 的指標，但兩者的差別在於標籤只會指向固定的 commit，而分支會隨著 commit 的推進而移動。

### 加入輕量標籤

指令：`$ git tag 標籤名稱`

可以在當前的 commit 新增一個標籤；後續要查詢標籤時，可使用指令 `$ git tag` 來做查詢。

### 加入標示標籤

指令：`$ git tag -am "標示內容" 標籤名稱`

除了可以在當前的 commit 新增一個標籤外，還能加入一些詳細的描述；可以使用指令 `$ git tag -n` 來查詢標籤的詳細資訊。

### 刪除標籤

指令：`$ git tag -d 標籤名稱`

可以將標籤想像成一張貼紙，撕掉並不會影響 commit 的內容。

### 移動至指定標籤位置

指令：`$ git checkout 標籤名稱`

可以移動 HEAD 的位置到指定標籤上查看檔案內容，輸入 `$ git checkout 分支名稱` 即可回到原本內容。

<!-- 標籤類似快照，移動過去時無法編輯檔案，待補充說明 -->

## 暫存

團隊開發專案的過程中，偶爾會開發到一半時，出現臨時的狀況要處理；其中一種作法是，先將目前的工作內容加入暫存區並 commit，處理完緊急狀況後，再透過指令 `$ git reset --soft HEAD^` 將最後一次 commit 刪除，同時保留當時提交前的所有狀態；另一種方式則是使用 `$ git stash` 相關指令來暫存檔案內容。

這裡的暫存（stash）和暫存區（Staging Area）兩者屬於不同的概念。

<!-- 什麼不同概念，待補充 -->

### 暫存當前目錄

指令：`$ git stash`

將當前工作目錄已被追蹤和暫存區（Staging Area）的檔案內容，無論有沒有編輯都暫存起來。

### 檢視暫存列表

指令：`$ git stash list`

透過指令 `$ git stash` 暫存的檔案可以使用上述指令來查詢，查詢結果會以列表的方式依序顯示，如下所示：

```
$ git stash list
stash@{0}: WIP on master: ce9d927 新增 index
```

### 還原暫存的目錄

指令：`$ git stash pop`

將暫存的檔案內容還原到當前分支上，有搬運資料的功能。

### 清除最新暫存

指令：`$ git stash drop`

刪除最後一次的暫存檔案並無法還原。

### 清除全部暫存

指令：`$ git stash clear`

刪除暫存列表所有的暫存紀錄，並無法還原。

## 遠端儲存庫操作指令

這裡開始會介紹推送本地端資料到遠端儲存庫時，較常會用到的指令；本篇筆記主要是在著重講述 Git 的基本操作，因此先不討論遠端儲存庫的建立方式與相關內容。

### 連結遠端儲存庫

指令：`$ git remote add origin 遠端儲存庫網址`

指令中的 `origin` 為遠端儲存庫的預設名稱；在實務上通常會有多個儲存庫，像是正式主機與測試主機；因此名稱也會有所不同。

### 查詢遠端儲存庫

指令：`$ git remote`

遠端資料與本地端建立連結時，可以使用該指令來查詢遠端儲存庫的名稱；也可以使用指令 `$ git remote -v` 來查詢目前連結的所有遠端儲存庫名稱與路徑。

### 複製遠端儲存庫

指令：`$ git clone 遠端儲存庫網址`

複製遠端資料到本地端資料夾，並建立工作目錄與本地端儲存庫（.git 資料夾），可以在指令後方加上 `-b 遠端分支名稱` 來指定要複製的遠端分支，該指令通常使用在本地端尚未有遠端的資料時；而後續本地端的資料更新都是透過接下來會介紹的指令 `$ git pull` 來操作。

### 推送本地端分支至遠端分支

指令：`$ git push 遠端儲存庫名稱 遠端分支名稱`

需要注意的是，第一次使用該指令推送時，可以輸入成 `$ git push -u 遠端儲存庫名稱 遠端分支名稱` ，這樣可以使 Git 將這次的推送行為紀錄起來，後續在推送本地端資料到遠端時，只需要輸入指令 `$ git push` 即可；但是也可能會遇到無法推送的情形，這部分會在後面 `$ git pull` 的地方說明。

補充說明，如果本地端連結的遠端儲存庫有多個，在推送的時候要注意指令中 `遠端儲存庫名稱` 的地方，需要改成要推送的遠端儲存庫名稱，避免推送到錯誤的地方。

### 變更遠端儲存庫名稱

指令：`$ git remote rename 原始儲存庫名稱 修改後名稱`

先前說明到，當遠端資料被複製到本地端時，預設的遠端儲存庫名稱為 " origin "，但是也可以使用上述指令來變更遠端儲存庫的名稱；需要注意的是後續在推送時，遠端儲存庫的路徑要改成新的名稱，假設修改後的名稱為 "newRepo"，推送指令就要輸入 `$ git push -u newRepo 遠端分支名稱` 。

### 複製遠端儲存庫分支與本地端分支合併

指令：`$ git pull`

團隊協作開發專案時，因為每個人的進度不同，常會需要將遠端最新的資料更新到本地端，這種情況就會使用到上述指令；與 `$ git clone` 不同的是， `$ git pull` 是使用在本地端已經下載過遠端的資料，但是需要做更新時。

前面 `$ git push` 的地方有提到，在推送本地端資料回遠端時，偶爾會遇到無法推送的情況；通常造成此情形的原因可能是遠端資料已更新，導致本地端在推送時產生衝突，下列是在推送時可能會出現錯誤訊息：

![](https://i.imgur.com/WYOmVjP.png)



圖中左邊的部分是推送本地端資料到遠端時，因為遠端資料有更動所出現推送被拒絕的訊息，這種狀況需要使用 `$ git pull` 先將本地端做資料更新，執行指令後可能會出現右邊部分的訊息，主要是要求遠端儲存庫與本地端進行合併（merge）分支的動作。

由此可知，指令 `$ git pull` 所做的事情是將遠端分支複製一份到本地端，再做合併的動作。



## 參考資料

- *[如何使用 --amend 追加檔案](https://gitbook.tw/chapters/using-git/amend-commit2)*
- *[Fast-Forward 快轉模式詳細介紹](https://ithelp.ithome.com.tw/articles/10211795)*
- *[Git Stash 詳細使用方式](https://gitbook.tw/chapters/faq/stash)*
- *[Git Reset 使用模式](https://gitbook.tw/chapters/using-git/reset-commit)*
- *[Clone 與 Pull 差異](https://gitbook.tw/chapters/github/clone-vs-pull)*
- *[Git Clone 指定分支](https://shengyu7697.github.io/git-clone-specific-branch/)*
