---
title: Git 和 GitHub 学习笔记
tags: [Git,GitHub]
date: 2019-02-22 22:48:00
updated:
categories: '笔记'
description: "Git/GitHub 使用过程中的一些笔记"
copyright: true
top:
image:
---

<p class="description"></p>

<img src="http://qiniu.mrshulan.com/git%E5%9F%BA%E6%9C%AC%E5%91%BD%E4%BB%A4%E5%A4%A7%E5%85%A8.jpg" alt="Git 常用命令速查表" style="width:100%" />

<!-- more -->

## 广播

历史：Linus的作者创建了开源的Linux，02年以前代码管理都依赖手动合并，后来管理不了了，拒绝SVN和CVS这些中央式版本控制的工具(原因如下表格)，采用免费授权给Linux社区的BitKeeper工具（这个公司就只授权了Linux社区，其他人使用都是要钱的），再后来05年社区的大牛要破解BitKeeper被人家公司发现要收回BitKeeper对Linux的免费的使用权，Linus一口气两周内用C写了一个分布式的版本控制系统——Git。接着08年GitHub问世，利用Git为无数开源项目提供代码的托管存储

分布式版本控制系统：Git,BitKeeper

集中式版本控制系统：CVS,SVN



## Git和SVN对比

|                | 集中式（SVN）                                                | 分布式（Git）                                                |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 代码保存       | 项目要开发完推送给中央服务器。                               | 开发人员在本地仓库存储提交代码修改的历史                     |
| 网络           | b必需是在**联网**的环境下工作,受制于网络和文件               | 没有网络的情况下也可以在**本地仓库**执行commit、查看版本提交记录、以及分支操作，在有网络的情况下执行 push到Remote Repository**远端仓库**。 |
| 文件存储格式   | 按照原始文件存储，体积较大                                   | 按照元数据方式存储，体积很小                                 |
| 分支操作的影响 | 创建新的分支则所有的人都会拥有和你一样的分支，本质上是因为都在中央仓库上操作 | 分支操作不会影响其他开发人员，备份灵活                       |
| 提交           | 提交的文件会直接记录到中央版本库                             | 提交是本地操作，需要执行push操作才会到远端仓库               |

分布式版本控制系统的远端仓库，有时候也被叫“中央服务器”，不同于集中式的中央服务器，分布式中它可以理解成一个中转站，用来协作同步各个本地仓库的代码，实际上任何一个服务器都可以取代它的作用，只是为了方便大家“交换”代码

## 克隆仓库和创建仓库

git clone/ git init/ git remote add origin

如果你已经在github自己创建了一个远程仓库（如果是空的话）会有一波提示，接下来我们

`git clone https
:
//github.com/****.git`  会自动在你选中的目录下生成一个一模一样的文件夹

当然我们可以直接在本地直接创建然后在和远程仓库进行关联

在创建好的文件夹下

`git init` 生成一个.git隐藏的文件夹也就是我们的本地仓库（Local Repository）.git文件夹所在的根目录就是我们的工作目录（Working Directory）

然后进行关联

`git remote add origin https
:
//github.com/****.git` 如果没有响应就是最好的回应

## 工作目录 暂存区 版本库 远程仓库

**工作目录（working directory）**就是我们创建的项目文件夹，我们开发项目的地方，.git所在根目录

`新创建的文件添加到暂存区--- git add` file

`暂存区的文件拉回来修改文件 --- git checkout -- file` 注意是要修改

**暂存区（index/staging area）**是指储存了所有待提交的改动的地方，只有在暂存区存在的文件，本地仓库才会追踪(track)到它的变化。

暂存区对应在.git文件夹中的index中，是一个二进制文件，可以理解为一个索引，内容包括根据文件名，文件模式和元数据进行排序的文件路径列表，每个路径都有权限以及Blob类型的SHA-1标识符（就是我们平时提交记录对应的那一串编码，下面会讲到）

`把文件从暂存区提交到本地的版本库中——git commit -m"备注"` 会head对应的提交信息生成其sha-1值

`从本地版本库中拉回来到暂存区 --- git reset HEAD file`

**版本库/本地仓库**（Repository）可以理解为就是我们分布式版本控制中提到的我们本地的代码版本库，这里面有我们所有提交版本版本的数据。

对应.git中的HEAD,实质上是一个指针，指向最新放入仓库的版本，默认情况下git为我们自动生了一个分支master，head就指向这个分支

**远程仓库**（Remote）托管代码的服务器，上面介绍分布式版本控制中的远程中央仓库，可以理解为一台专门用于协作开发时数据交换的电脑。

![1546406825010](http://qiniu.mrshulan.com/1546406825010.png)

![1546406852485](http://qiniu.mrshulan.com/1546406852485.png)

#### git diff

`git diff 比较的是工作区和暂存区的差别`

`git diff HEAD 可以查看工作区和版本库的差别`

`git diff --cached(===staged) 比较的是暂存区和版本库的差别`

## 一个简单的流程(素质三连🍋)

#### git status/ git add/ git commit/ git log/ git pull /git push

前提是在本地仓库已经和远程仓库相互关联的情况下操作 push

​	

`touch test.js` 

`git status` 查看工作目录和暂存区的状态 

![1546413685960](http://qiniu.mrshulan.com/1546413685960.png)

untracked files表示未追踪的文件，就是新创建的test.js，未追踪的意思就是当前本地git仓库对它没有任何的记录，对本地仓库来说是不存在的，在我们提交代码的时候也不会提交上去，这时就用到了add命令

`git add 文件` 将文件添加到暂存区 如果是 `git add .`就是所有改动的文件

![1546413926159](http://qiniu.mrshulan.com/1546413926159.png)

新添加的文件进入暂存区，从untracked未跟踪状态变为stage已暂存状态。接着文件进入暂存区之后，我们的修改就都可以被暂存区追踪到，并且会显示状态。

![1546414060174](http://qiniu.mrshulan.com/1546414060174.png)

new file modified这类标志的状态提醒，下面一个提示不再是untracked（不在追踪范围）而是not staged for commit（还不在待提交的暂存区中）。意思就是，本地仓库现在已经认识了这个文件，它被修改了，还没到储存待提交信息的暂存区中，还是使用add添加到暂存区，所以还得需要git add命令才会把颜色从红色变成绿色。

同时要注意，通过 add 添加进暂存区的不是文件名，而是具体的文件改动内容，我们把在执行add时的改动都被添加进了暂存区，在add 之后的新改动并不会自动被添加进暂存区。所以对test.js执行了add之后如果再修改test.js，那么工作目录和暂存区都会有这个文件

`git commit -m"提交到版本库的地址信息备注 方便识别"`

如果没有-m 会自动进入vim编辑模式 i(插入编辑模式) -> esc->:wq 退出并强制保存(注意在英文状态下，不然就会有奇怪的闪烁)

![1546414815222](http://qiniu.mrshulan.com/1546414815222.png)

`git log` 查看提交历史

![1546415158924](http://qiniu.mrshulan.com/1546415158924.png)

`git log -p` 查看每个commit的每一行改动

`git log --stat` 查看文件修改，不展示具体修改细节

`git relog` 简短的查看改动 （用的比较多）

`git show commit编码(例如7c62943) 只要能唯一识别即可` 查看该commit的具体改动

![1546415497198](http://qiniu.mrshulan.com/1546415497198.png)

`git push` 将版本提交到远程仓库 （这个步骤一般出错居多）

如果是这样的话

![1546415767181](http://qiniu.mrshulan.com/1546415767181.png)

这就说明你没有git add remote origin http：.... 一开始就跟你们说过了

如果是这样的话

![1546415862596](http://qiniu.mrshulan.com/1546415862596.png)

当前分支没有和上游远程分支做关联，git不知道你要推送到远程仓库的哪个分支上，我们想要和远程的master分支关联，按照提示输入：git push --set--upstream origin master 即可

origin是远程仓库的代指，master是远程仓库上的分支名，这里的origin/master，即远程仓库的master分支，就是我们test项目的远程仓库。我们把关联的远程分支设置成了origin/master，之后直接执行git push默认就会推到远程的master下，当然我们不省略传入远程的分支名就会推送到对应的分支上。

如果是这样的话

![1546416177911](http://qiniu.mrshulan.com/1546416177911.png)

如果你不是clone的初始化而是直接本地init而在远程仓库创建的时候添加了readme 或者 选上了 readme/.gitignore，证书等这些都会算作一次线上提交，而然本地没有，这不就冲突了吗，或者说是在远程仓库做出了修改，与远程仓库版本不一致，

当然光执行pull也是不够的，远程仓库有一个提交，我们本地仓库也有一个提交，直接拉取远端的代码，这两个提交谁先谁后呢？没有操作相同文件时可能无所谓，但是一旦都修改了同一个文件，就涉及到哪次提交在后，覆盖的问题，所以要执行：

`git pull --rebase origin master` 把远程库中的更新合并到本地库中，–rebase的作用是取消掉本地库中刚刚的commit，并把他们接到更新后的版本库之中,这样push就ok拉

看一样提交记录

![1546416846353](http://qiniu.mrshulan.com/1546416846353.png)

远程仓库的initial commit是第一条记录，我们刚提交的在后面

## commit信息历史

#### commit的id

每一个commit对应一个唯一id，是40为的数字和字符组成的字符串，是属于每一个commit的一个id，一个SHA-1校验和

来详细的看一看

`git cat-file commit HEAD`

![1546417067435](http://qiniu.mrshulan.com/1546417067435.png)

第一行，tree和对应的hash值，根据这个hash值我们可以得到本次提交的整个目录树和对应的hash值

`git cat-file -p hash值` -p 一种更加优雅的方式展示对象的内容

![1546417235612](http://qiniu.mrshulan.com/1546417235612.png)

里面的每一个文件都可以根据hash继续展开 直到叶子结点。

回到head信息组成这里，第二行parent，是当前查看的commit的上一条commit的id；第三行作者信息以及提交时的时间戳；第四行提交者的信息以及提交时的时间戳。

#### HEAD

commit记录后面括号对应着指向这个commit的引用，注意到commit提交信息第一条后面的括号里的HEAD，它永远指向当前的commit，就是当前工作目录对应的提交的commit。

HEAD同时也指向一个分支，图中的HEAD->master，表示当前工作目录对应的是本地master分支

通常每次有一条新的commit记录时，工作目录会与这条最新的commit对应，HEAD指针也会指向它（在使用checkout reset等操作切换当前工作目录对应的commit时，HEAD也会跟过去）

![1546417810514](http://qiniu.mrshulan.com/1546417810514.png)

我们commit了最新的提交信息还没push到远端时，本地的HEAD指向我们最新的提交，而远端仓库的还停留在之前的那条commit记录，origin/master指向它。

在我们push操作的时候，HEAD并不会推送到远端，远端的HEAD永远指向默认分支master

#### master分支

一个没有提交记录的新项目，在创建第一条commit时，会默认提交到master分支，同时HEAD也指向它。

在我们clone远端项目时，默认也会在本地checkout出一个master分支，并将本地工作目录的文件内容保持与clone下来的项目的master分支的最新commit一致，HEAD也会指向它。

绝大多数团队会选择master作为核心的分支，其余分支都是围绕master来开发，但本质上各个分支都是一样的，都是一串commit信息的记录。

## breach分支创建和切换

git branch/git checkout/git checkout -b/git branch -d

`git branch 新的分支名` 创建分支

这样创建完新的分支，并不会自动切换到新的分支上

`git checkout 分支名` 切换分支

一步到位

`git checkout -b 新的分支名` 创建并切换到新的分支

`git checkout -b origin/feature1` 创建并切换到新的分支并与远程进行关联

![1546418561648](http://qiniu.mrshulan.com/1546418561648.png)

切换到新的分支之后，HEAD也跟着指了过去，当前新分支有新的commit时，HEAD会指向这个分支最新的commit，master会停留在它之前对应的commit记录那里，因为那是属于master分支最新的commit记录。

`git branch -d 分支名` 删除分支

注意，HEAD指向的分支无法删除，也就是我们所在的分支，需要先checkout切换到别的分支，再去删除之前的分支。

我们删除了一个分支后，并不会删除这个分支上的提交记录，其实branch这个分支的概念，更确切的说是一个引用，是一个指向，指向一串的提交记录，我们删除了分之后只是删除了对这个分支包含的提交记录的一个引用，虽然说我们没有删除它们，但是Git的回收机制会定期清理那些不在任何分支上的commit记录。

同时在分支push的时候记得在远程仓库也要创建一个相同的分支名，并且相关联起来，才可以准确的push

## merge 合并冲突

git merge/git merge --abort

merge意思为合并，把目标分支合并到当前分支，一般在我们分支协作开发，某一分支的开发完成可以合并如主流程的时候，这样去操作。

实际的行为是，从当前分支和要合并的目标分支的分叉点开始，将目标分支路径上的所有commit内容应用到当的commit，并生成一个新的commit

我们在本地master分支上执行

`git merge 分支名`

被合并的分支有许多新的动作，如果要是把主分支的东西给改了就会还提交了 就会出现合并冲突

![1546421607709](http://qiniu.mrshulan.com/1546421607709.png)

直接打开vi test.js

![1546421637898](http://qiniu.mrshulan.com/1546421637898.png)

<<  ===  >>> 作为冲突分隔内容 你看你要要留住啥就改成啥样 然后在**重新 add commit push**就好了

`git merge --abort` 取消merge操作 在没有push之前

当**我们所在分支落后于目标分支**时（目标分支包含当前分支所有的提交记录），在当前分支对目标分支执行merge，就是直接把HEAD和所在分支都指向目标分支最新的commit，也成为fast-forward快速前移

例如我们现在从master新建一个分支branch2，并修改了一些内容，有两次提交记录，期间master没有新的提交，一直停留在分叉点，然后我们回到master去merge分支branch2,这时branch2是包括我们所在分支的所有记录，领先于我们所在分支的，执行以下操作

`git merge origin/branch2` 就会产生 fast-forward

![1546493234042](http://qiniu.mrshulan.com/1546493234042.png)

到branch2领先的两个提交都被master拿到，接着push到远端master，远端master上的提交记录就多了两条branch2的提交记录

当**我们所在分支领先于目标分支**时（当前分支包含目标分支所有的提交记录），这种时候，merge相当于空操作

当然，我们也可以通过命令修改默认生成的提交的信息，也可以不默认生成新的commit，这里只是简单介绍常用命令的基本用法，merge详细的使用可以看下：

https://blog.csdn.net/andyzhaojianhui/article/details/78072143

![1546426461949](http://qiniu.mrshulan.com/1546426461949.png)



## rebase避免出现的分支合并

git rebase

通过merge来协作开发，历史记录会出现很多分支，如果想避免这样导致过乱，可是采用rebase命令。

`git rebase 目标分支`

假设我们需要将branch2的记录合并到master，并且丢弃现有的分叉，执行

`git checkout branch2` 先切换到需要被合并的分支branch2

`git rebase master` 向要合进去的分支master发出rebase命令

实际上是我们需要被合并的分支feature1，将其分叉点2重新设置为要被合进的目标分支master的最新commit3上，4和5的基础点从2变成了3，同时我们所在的分支的最新一条记录和HEAD都对应到合并后的最新的commit记录7上

![1546426251806](http://qiniu.mrshulan.com/1546426251806.png)

![1546426261456](http://qiniu.mrshulan.com/1546426261456.png)

![1546495518692](http://qiniu.mrshulan.com/1546495518692.png)

branch2  rebase 之后打印git log

![1546495554111](http://qiniu.mrshulan.com/1546495554111.png)

4和5因为没有分支引用指向它，之后会被Git回收机制清除

然后，我们**回到master上对feature1执行一次merge**，回忆下上面讲的fast-forward，如果所在分支包含要merge分支的commit信息，我们就只是把HEAD和对应分支向后移动，指向最新的commit，也就是master和HEAD都指向7

![1546495710467](http://qiniu.mrshulan.com/1546495710467.png)

也就是绕个圈子在 branch2上rebase 把head移动然后 再到master是进行merge产生一个fast-forward

#### 为什么不在master上执行rebase呢？

在我们分支开发的时候，通常都是以master以核心的分支，如果我们在master上对feature1执行rebase，那么3和6就夫指出新的接在5后面，3和6这两个commit在我们核心分支所包含的路径中不存在了，现在的master是124567，这样协作开发的其余同事在push代码时，因为他们本地有3和6而远端master没有3和6，就是提交失败（具体原因开篇readme和.gitignore那里同理）

关于rebase，只要记住，它是修改需要**被合并**的分支的基础点，同时与merge相反，需要在**被合并**的分支上操作的指令



## 修改被rebase分支的历史记录

rebase -i/git rebase --amend/git rebase --continue/git rebase --abort

如果我们想在rebase的过程中对一部分提commit交进行修改，可以在'git rebase'命令中加入'-i'或'--interactive'参数去调用交互模式。

假设我们要将feature1上的commit记录的**基础点重设为master分支**的最后一条，同时希望修改我们接到master后面的feature1上的提交信息。

看下feature1上的commit记录，倒数第三条是master上的提交，那次提交便是feature1在master上的基础点：

![1546588612739](http://qiniu.mrshulan.com/1546588612739.png)

我们在feature1上执行：

`git rebase -i master`

接着我们进入了编辑页面，顶部列出了将要「被 rebase」的所有 commit记录，也就是我们从master分支checkout出feature1分支后的两条提交记录。这个排列是正序的，和log显示的顺序相反

pick的意思是直接应用，我们如果要修改某次的提交信息，需要把提交信息修改成edit，这样在应用到这条commit记录时，Git会停下来让我们修正，假设我们要对这两条commit提交信息分别修改，在vim下讲两个pick改成eidt，然后输入"：wq!保存并退出"

这里Git在执行到"feature1 first commit"便停了下来，提示我们可以通过amend来修改这条commit记录，amend就是用来修改HEAD所指向的这条最新记录，这个具体下面会讲。我们输入git commit --amend，然后进入编辑页面修改上条commit信息，保存。

`git rebase --amend`  修改提交的信息，不能为空，空了就没有用

`git rebase --continue`  继续执行rebase  如果你改了多个pick -> edit就要多次 --amend --continue来回

![1546588780856](http://qiniu.mrshulan.com/1546588780856.png)

`git rebase --abort` 放弃rebase过程

成功之后 git log就会成功的看到修改之后commit

![1546588809485](http://qiniu.mrshulan.com/1546588809485.png)



#### 修改当前的分支的历史记录

对历史commit记录修改的功能，不仅适用在需要合分支的时候，我们也可以在当前分支进行原地操作，直接对当前分支历史错误的提交记录进行修改。

`git rebase -i HEAD~2` <=> `git rebase -i HEAD^^` ^和~都是偏移符号也就是最近的第几个

**^ 的用法**：表示对当前指针指向的commit记录向前偏移，偏移数量就是^的数量，例如： HEAD^^^，表示的是HEAD所指向的那个commit往前三个的那条commit记录，也就是图中圈出来我们要修改的那个commit前面的那个commit

**~ 的用法**：同样是当前指针的基础上往回偏移，偏移数量就是~后跟着的数字，例如：HEAD ~1表示的同样是图中的commit前面的那条commit

![1546589101346](http://qiniu.mrshulan.com/1546589101346.png)

rebase它其实是对分支重设基础点的一个操作，在对别的分支操作时，会找被rebase的分支和要rebase到的分支两个分支的交点，也就是被rebase的分支的一个基础点，分叉点，然后对从基础点分叉出来的提交重新设置为要rebase到的分支最新一条记录

所以这里git rebase -i HEAD^^^，rebase后面跟着的是一个自己分支的某个提交记录，实际上就是对rebase -i 后面跟着的那条记录开始（不包括开始点）往后的所有commit重新设置基础点，把这些commit重新生成一遍再接在这个新的基础点后面，对于文件历史变化来说，这个其实就是空操作。

![1546508313713](http://qiniu.mrshulan.com/1546508313713.png)



#### 替换最近一条commit信息

git commit --amend

git commit --amend是对上一条commit命令进行修正。当我们执行这条命令时时，Git会把当前暂存区的内容和这次commit的新内容合并创建一个新的commit，把我们当前HEAD指向的最新的commit替换掉。例如我们当前最新一条commit记录中，我们输入错了提交信息，想要修改，又或者我们提交错了一点东西，又不想生成一个新的commit记录，我们都可以使用这个命令。

这里假设我们需要修复一个上次提交错的文件，同时想修改上一个commit的信息

修改之后添加的 暂存区

​	

`git commit --amend` 进入vim编辑模式进行修改

amend用于修改上一条commit信息时，实际上并不是对上一个commit修改，而是生成新的对它进行替换。我们看第一张图我们操作的那条commit修改之前，和我们修改后生成的新的commit，id是完全不一样的（文章上介绍过生成方式），是两个不同的commit

所以这个时候如果我们对已经push到远端的commit记录在本地仓库进行--amend操作之后，直接push到远端仓库是不会成功的，因为本地丢失了远端仓库那个我们替换的commit

当然如果你啥也不改直接保存，那就相当于空操作嘛，老的commit就不会被替换了，还是它本身



## 丢弃最新的提交

git reset

最新一次的commit的内容有问题，想要丢弃这次提交，先log看下提交记录：

`git reset HEAD~1`

被撤销的那条提交并没有消失，只是log不再展现出来，因为已经被丢弃。如果你在撤销它之前记下了它的 SHA-1 码，那么还可以通过这个编码找到它，执行如下：

`git reset commit编码`

![1546589548440](http://qiniu.mrshulan.com/1546589548440.png)

然后我们再次git log 就可以看到那调丢弃的已经恢复了 而且head指向了它

#### 参数--hard --sort --mixed

这里reset后面跟的参数影响的正是这工作目录（working area),暂存区（index)和本地版本库（HEAD）的区别内部的数据状态。

`git reset --soft HEAD~1

执行这句命令时，实际上我们只是把本地版本库，指向了我们要指向的那个commit，而暂存区和本地工作目录是一致的，保留着我们的文件修改，操作看下：

![1546589609264](http://qiniu.mrshulan.com/1546589609264.png)

执行完，status看下工作区状态，我们可以看到现在我们的暂存区有一个待commit的文件，证明现在本地版本库和暂存区是不一致的，而这个不一致刚好是我们丢弃的那次commit修改的内容，同时我们并没有看到有文件是"changes not staged for commit",说明当前我们的工作目录和暂存区文件状态是一致的。（绿色是撤销成功的状态，红色是未撤销的状态）

总结如下 HEAD(本地版本库） != INDEX （暂存区文件内容）== WORKING （本地工作目录）(就是撤销这一次的commit)

![1546508879972](http://qiniu.mrshulan.com/1546508879972.png)



`git reset --hard HEAD~1`  这个就是版本回退

执行这句命令时，不仅本地版本库会指向我们制定的commit记录，同时暂存区和本地工作目录也会同步变化成我们制定的commit记录的状态，期间所有的更改全部丢失，操作看下：

![1546589909398](http://qiniu.mrshulan.com/1546589909398.png)

执行完我们看到，暂存区和工作目录都没有文件记录（啥都给你没了）

但是我们可以通过git reflog进行前进  或者你自己再次提交（这样就不划算了）

![1546590086538](http://qiniu.mrshulan.com/1546590086538.png)

`git reset 前进的commit编码` 只不过需要重新提交，因为监视到文件改动了 当然可以checkout -- file 从暂存区拉回去

总结如下 HEAD(本地版本库） == INDEX （暂存区文件内容）== WORKING （本地工作目录）

![1546589755454](http://qiniu.mrshulan.com/1546589755454.png)

`git reset --mixed HEAD~1` 默认就是这个参数

--mixed是reset的默认参数，也就是当我们不指定任何参数时的参数。它将我们本地版本库指向我们制定的commit记录，同时暂存区也同步变化，而本地工作目录并不变化，所有我们丢弃的commit记录涉及的文件更改都会保存在本地工作目录working area中，所以数据不会丢失，但是所有改动都未被添加进暂存区，方便我们检查修改，操作看下：

![1546589483409](http://qiniu.mrshulan.com/1546589483409.png)

执行完我们看到，在工作目录中有文件修改，而暂存区和本地版本库与我们指定的commit记录保持一致

总结如下 HEAD(本地版本库） == INDEX （暂存区文件内容）！= WORKING （本地工作目录）

![1546508986655](http://qiniu.mrshulan.com/1546508986655.png)

## 丢弃历史中的某一条提交

git rebase -i/ git rebase --onto

上面我们说到reset可以让我们回归到历史的某条commit记录，但是我们从那条记录之后的记录就都被丢弃，如果我们只想丢弃历史记录中的某一条而不影响其之后的记录要怎么做呢？

还是通过git rebase。这里不放在上面rebase的部分一起说是因为rebase的这个用法，在reset之后来讲会更方便理解。

`git rebase -i HEAD~2`  -i后边跟着我们要删除的记录前面（老的）的任意记录，设置为基础点，想象一下那个链

进入编辑页后，i进入插入模式，我们之前修改commit是将pick(应用）修改为edit，这次要撤销某条记录，我们直接把改条记录删除

然后直接就成功了

![1546590957204](http://qiniu.mrshulan.com/1546590957204.png)

正序 所以我们删除 第二行（最新的）

![1546591027615](http://qiniu.mrshulan.com/1546591027615.png)

`git rebase --onto`

我们之前在对分支执行rebase时，是选择所在分支与目标分支的交叉点作为起点，把所在分支从这个起点到最新的commit记录接到目标分支的结尾。

而rebase --onto可以帮我们指定这个起点，从新起点到所在分支最新记录之前的commit记录才接到目标分支上：

![1546509229559](http://qiniu.mrshulan.com/1546509229559.png)

假设我们再1245这条分支上，对123（master)执行rebase不带任何参数,默认我们所在分支的起点是2，2后面的4和5会复制出来一个6和7接在3后面

如果我只想把5提交到3上，不想附带上4，那么我可以执行：

`git rebase --onto 3 4 5 //345分别是commit记录的代指`

--onto 参数后面有三个附加参数：目标 commit（要接在哪次记录后面）、起点 commit（起点排除在外，从起点之后的记录）、终点 commit。所以上面这行指令就会从4（不包括4）往下数，一直数到5，把中间涵盖的所有commit记录，重新提交到 3 上去。

假设我们要丢失当前分支倒数第二个提交，HEAD^对应的那个，那么我们只要执行：

`git rebase --onto HEAD^^ HEAD^ HEAD`

这句的意思是，以倒数第三个新的目标点，从倒数第二个不包括倒数第二个的commit记录开始，到HEAD之间的（本例中只有HEAD一个）接到新的目标点之后，所以倒数第二个就被跳过，直接最新的接在倒数第三个的后面。

## 生成一条新提交的撤销操作

git revert

在我们已经push到远端仓库后发现有一条commit记录对应的修改应该被删除时，我们可以在用上面的操作方式在本地仓库操作删除那条记录，再推送到远端，但是注意，因为我们是删除了一条记录，所以在我们推送远端仓库的时候，会因为我们本地没有远端对应的那条记录而提示push失败

这时，如果你本来就希望用本地的内容覆盖掉中央仓库的内容，并且有足够的把握不会影响别的同事的代码，那么就不要按照提示去先pull再push了，而是要选择「强行」push：

git push origin 分支名 -f //-f force 强制`

但是在我们分支协作开发时，在向master分支提交代码时，是不应该用-f的，因为这样很容易让我们本地的内容覆盖或者影响同事们提交上去的代码。那这个时候如果我们想要撤销某次提交对应的改动要怎么办呢？

![1546591330733](http://qiniu.mrshulan.com/1546591330733.png)

生成一条与我们要撤销的那条记录相反的新的commit记录：

`git revert 要撤销的改动对应的commit记录` 之后会进入vi 你说明这次提交的目的就好了

![1546591451610](http://qiniu.mrshulan.com/1546591451610.png)

git revert会生成一个与后面对应的commit记录相反的一次文件提交，从而将那次修改抵消，达到撤销的效果。

执行revert后再push到远端，我们文件内容就恢复了。revert的方式并不会造成某条记录在历史记录中消失，而只是生成一个新的与我们要撤销的记录相反的文件提交而已。

## 分支和HEAD分离

git checkout

在我们执行git checkout branch分支名的时候，HEAD会指向这个分支，同时两者都指向这个分支最新的那条commit记录。其实我们操作的这些分支，就是我们的一种理解，本质上它也是一个指针，对应着commit记录，所以checkout后面也可以直接指定某一条commit记录。

但是不一样的是，在我们checkout到某一条特定的commit记录时，HEAD和分支就脱离了，我们只是让HEAD指向了我们指定的记录，而所在的分支的指针并没有同步过来。HEAD 处于游离状态时，开发者可以很方便地在历史版本之间互相切换，比如要回到某次提交，只需要  对应的  或者  名即可。

![1546594111369](http://qiniu.mrshulan.com/1546594111369.png)

checkout本质上的功能其实是出入到指定的commit记录的一种操作。

`git checkout --detach` Git就会把HEAD和feature_1原地脱离，直接指向当前commit

![1546592200410](http://qiniu.mrshulan.com/1546592200410.png)

变成

![1546592218902](http://qiniu.mrshulan.com/1546592218902.png)

`git checkout commit编码`指定游离

![1546592410298](http://qiniu.mrshulan.com/1546592410298.png)

并且会产生新的分支...(弊端)

![1546592473426](http://qiniu.mrshulan.com/1546592473426.png)

如果你在游离的分支产生了提交

![1546593178406](http://qiniu.mrshulan.com/1546593178406.png)

![1546593281212](http://qiniu.mrshulan.com/1546593281212.png)

切换会 master 分支时，在游离状态所做的修改和提交无法追溯，

![1546593539415](http://qiniu.mrshulan.com/1546593539415.png)

然后再游离回去 

``git checkout 记录的commit编码

![1546593669237](http://qiniu.mrshulan.com/1546593669237.png)

**创建一个新的分支**temp 然后 **合并他** 继而**删除他**

## 临时存放工作目录的改动

git stash

工作中可能经常遇到我们现在在某个分支开发，突然需要切换到master发个包或者到别的分支做些修复，但是我们本地改了一半儿的东西又不想先提交（例如可能会有改了一半儿的bug，推上去的话搞得一起在这个分支的小伙伴拉下来项目都跑不起来），为了防止带到别的分支同时不用提交到远端分支，又不丢弃自己现在的改动，我们可以借用stash

`git stash` 临时保存工作区的改动

stash指令可以帮你把工作目录的内容全部放在你本地的一个独立的地方，不是暂存区，它不会被提交，也不会被删除，同时你的工作目录已经清理干净，可以随时切换分支，等之后需要的时候，再回到这个分支把这部分改动取出来

`git stash pop` 取出工作区的改动

![1546594318251](http://qiniu.mrshulan.com/1546594318251.png)

这里注意，我们untracked的文件，是不在本地仓库追踪记录里的（上开头部分说过），自然stash的时候也会忽略他们，这时如果想要stash一起保存这些untracked的文件，我们可以

`git stash -u` --include-untracked 的简写，将untracked的文件一并临时存储

## 操作记录恢复

git reflog

它是Git仓库中引用的移动记录，如果不指定引用，git log默认展示HEAD所指向的一个顺序的提交列表。它不是本地仓库的一部分，它单独存储，而且在push，fetch或者clone时都不包括它，它纯属是本地的一个操作的记录。

每行记录都由版本号，HEAD值和操作描述三列分组成。

- 版本号：这次操作的一个id
- HEAD值：同样用来标识版本，但是不同于版本号的是，Head值是相对的。括号里的值越小，表示版本越新
- 描述：操作行为的描述，包括我们commit的信息或者分支的切换等。

reflog为每一条**操作**显示其对应的id版本号，这个id可以很好地帮助我们你恢复误操作的数据，例如我们错误地reset了一个旧的提交，或者rebase等操作，这个时候我们可以使用reflog去查看在误操作之前的信息，并且使用git reset 版本号，去恢复之前的某一次操作状态，操作过后依然可以在reflog中看到状态之后的所有操作记录

区别于git log，**log显示的是所有本地版本库的提交信息**，commit记录，且不能察看已经删除了的commit记录。而git reflog可以查看所有分支的所有操作记录（包括commit和reset的操作），包括已经被删除的commit记录，几乎所有的操作都记录在其中，随时可以回滚。

![1546594341532](http://qiniu.mrshulan.com/1546594341532.png)

## 打上标签（tag）

git tag / git tag show/ git tag -a  -m/

轻量级的（lightweight）和含附注的（annotated）。轻量级标签就像是个不会变化的分支，实际上它就是个指向特定提交对象的引用。而含附注标签，实际上是存储在仓库中的一个独立对象，它有自身的校验和信息，包含着标签的名字，电子邮件地址和日期，以及标签说明，标签本身也允许使用 GNU Privacy Guard (GPG) 来签署或验证。一般我们都建议使用含附注型的标签，以便保留相关信息；当然，如果只是临时性加注标签，或者不需要旁注额外信息，用轻量级标签也没问题。

`git tag` 

`git show tagname`查看版本信息

​	

`git tag -a tagname -m"information"` 含附注型的标签

![1546598944875](http://qiniu.mrshulan.com/1546598944875.png)

如果你有自己的私钥，还可以用 GPG 来签署标签，只需要把之前的 `-a` 改为 `-s` （译注： 取 `signed` 的首字母）即可：

git tag -s tagname -m"information"` 签署标签 git show 的时候可以看见

补打标签只需要在-a 后边加上 commit编码

`git tag -d tagname`删除本地标签

`git tag -l 1.*.`* 查看指定类型版本

`git push origin --tags` 推送所有标签 也可以指定版本 只有这样推送标签远程仓库才会有release

![1546599787497](http://qiniu.mrshulan.com/1546599787497.png)

`git push origin -d tagname` 删除远程仓库标签 同时记得 删除本地标签

老版本是这么删的 (< 1.7.0)` git push origin :refs/tags/tagname`

## vim操作

1.输入i进入插入模式，INSECT

2.按下ESC键，退出编辑模式，切换到命令模式。 

3.保存修改并且退出 vim：":wq" (英文输入法状态，中文会直接闪烁)

4.保存文件，不退出vim：":w"

5.放弃修改并退出vim：":q!"

6.放弃所有文件修改，但不退出 vim：":e!"

## 参考文章

[花点时间顺顺Git](https://mp.weixin.qq.com/s/iF6M55WdwinAxyovMm38eg)