#1 環境安裝

#2 設定config 與 初始化
##2.1 使用者設定
```sh
git config --global user.name "你的使用者名稱"
git config --global user.email "你的E-mail"
```
####如果不同專案想設定不同的作者呢？
```sh
git config --local user.name Sherly
git config --local user.email shely53@yagoole.tw
```
輸入完成後，可以檢視目前設定
####檢視目前設定
```sh
git config --list
user.name=你的使用者名稱
user.email=你的E-mail
```
#3 新增、初始 Repository
##3.1 初始化
```sh
cd /tmp                 # 切換到 /tmp目錄
mkdir git-practice      # 建立 git-practice目錄
cd git-practice         # 切換進 git-practice目錄
git init                # 初始化這個目錄，Git開始對目錄進行版控
  Initialized empty Git repository in /Private/tmp/git-practice/.git/
```
此時 /Private/tmp/git-practice 便成為我們工作目錄（Working Directory）
##3.2 把檔案交給 Git 控管
開始前，我們先了解目錄當下的狀況吧！我們可以透過 git status 這個指令查詢現在該目錄的「狀態」。
```sh
git status

  On branch master
  No commits yet
  nothing to commit (create/copy files and use "git add" to track)
```
現在我們的工作目錄底下是空的，因此也沒有東西必須被提交（commit）。
___

#####當每次的 Coding 撰寫、修改進行到一個段落後，我們便會把檔案放入暫存區（add），再提交至本地的儲存庫（commit），也就是建立了一個版本。

接著，讓我們開始吧！

```sh
touch index.html        # 建立一個名為index.html的檔案
git status              # 檢查狀況，Git發現index.html這個檔案存在，但尚未進行追蹤

  On branch master
  No commits yet
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
          index.html
  nothing added to commit but untracked files present 
  (use "git add" to track)
```
我們有了一個檔案，Git還沒開始追蹤，我們把它交給Git吧！

```sh
git add index.html
git status              # 檔案被新增至暫存區了，Git開始追蹤，並且可以準備提交了

  On branch master
  No commits yet
  Changes to be committed:
    (use "git rm --cached <file>..." to unstage)
          new file:   index.html

# git add *.html 新增所有副檔名為html的檔案
# git add --all  加入 所有 「新增的檔案(Untracked)」「修改過的檔案」「被刪除的檔案」 進入 暫存區
# git add .      在2.x版本後，功能與上面相同
```
Git 會開始與本地端的儲存庫（之前提交的commit）比較檔案的差異。

接著我們準備把放在暫存區的東西提交到儲存庫，即一個存檔。也就是建立了一個**版本**。
```sh
git commit -m "create index page"      # 我們將暫存區的檔案提交給本地的檔案儲存庫

  [master (root-commit) e46eb29] create index page
  1 file changed, 0 insertions(+), 0 deletions(-)
  create mode 100644 index.html
```
-m 的參數是關於此次 commit 的 Message，我們在提交檔案時，可以在此處說明這次的Commit（這個版本）做了什麼事情。


___

####為什麼我們一定要這麼麻煩，分成兩階段來提交我們的版本呢？

看似麻煩的兩段式，其實也是有好處的，想像我們的專案裡有很多個檔案，add就像是我們把這次所有需要修改的檔案放入暫存區，集中在一起，commit把所有放在暫存區得檔案提交給本地的儲存庫（建立我們的版本）。



#####上面add 跟 commit的動作也可以結合成一個指令，如下：
```sh
git commit -a -m "create index page"
```

####一定要有東西才能commit嗎？
作為練習的用途，我們也可以使用以下指令，提交空的commit
```sh
git commit --allow-empty -m "一個空的commit"
```

#4 檢視紀錄
```sh
git log
git log --oneline --graph 
git log --oneline --author="ken"
#查看ken所有的提交
git log --oneline --author="ken\|Eddie"
#查ken跟Eddie兩個人的commit紀錄
git log --oneline --grep="wtf"
#查commit訊息有符合wtf字樣的內容
git log --oneline -S "Ruby"
#檔案內容中有提到"Ruby"的 
git log --oneline --since="9am" --until="12am"
#查看9~12點之間所有commit
git log --oneline --since="9am" --until="12am" --after="2017-01"
#查2017年1月之後，9~12點之間所有commit
```
#5 如何在Git底下刪除檔案或變更檔名呢？

##5.1 刪除檔案
我們版本中原本有個叫做welcome的檔案
```sh
echo 'Welcome' > welcome.html
git commit -a -m "Insert welcome page"
```
####一般指令、直接刪除檔案
```sh

rm welcome.html
git add welcome.html  #狀態欄會顯示deleted

git commit  -m "delete the welcome page"
```
git追蹤到這個檔案不見了，因此我們必須更新版本狀態，git add 之後，該份檔案狀態會標示為deleted
####Git幫我們刪除檔案
```sh
git rm welcome.html
```
這一行指令等同於原本的rm 與git add，如此我們便可以少輸入一行指令


#####那我們沒有要刪除，只是不想再讓Git追蹤、控管這份檔案呢？
##### 加上--cached 這個參數！
```sh
git rm welcome.html --cached
```
如此git 便會從目錄中刪除此份檔案的追蹤
##5.2 變更檔案名稱
我們版本中，原本有個hello.html的檔案，想把它改名為world.html
```sh
echo 'Hello Git' > hello.html
git commit -a -m "create hello.html"
```
####直接改名
```sh
mv hello.html world.html      #改名字
git add --all                 #狀態欄會顯示renamed

git commit  -m "change file name"
```
Git 偵測到原本hello.html檔案不見了，但是多了一個 world.html的檔案，Git辨識出雖然檔案名稱不一樣，但是檔案的內容卻是一樣的，因此我們會看到狀態顯示為renamed
####Git幫我們改名
```sh
git mv hello.html world.html
```
這一行指令等同於原本的mv 與git add，如此我們便可以少輸入一行指令

#6 修改commit的紀錄
#### 修改最近一次commit訊息
```sh
git commit --amend -m "我想說的新訊息"
```
#### 新增檔案到最近一次commit提交
```sh
echo '追加檔案到commit' > addup.html
git add addup.html
```
我們想要把一份新增的addup.html進最後一次的commit
```sh
git commit --amend --no-edit
```
#7 新增目錄

#8 有些檔案我不想放在Git裡...