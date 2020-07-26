# git

## git简介

### git工作区和暂存区

- 工作区：在电脑里直接看到的目录就是一个工作区

- 版本库：工作区有一个隐藏目录`.git`，这个不算工作区，而是git的版本库。git版本库里存了很多东西，其中最重要的就是称为`stage(或者叫index)`的缓存区，还有git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。
- 当我们把文件往Git版本库里添加的时候，是分两步执行的。
  - 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区。
  - 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

### git全局配置

在安装后首次使用需要进行全局配置。这里用户名与邮箱地址建议与github的一样。

```
git config --global user.name '用户名'
git config --global user.email '邮箱地址'
```

### 初始化git仓库

厂库最好是空目录且不要有中文名

1.创建厂库

直接右键或 $ mkdir 文件夹名

2.厂库初始化

git初始化的目的是为了使git知道要管理这个仓库，初始化之后文件夹下会有个.git的隐藏文件夹，不能删除或随意更改。

```
$ git init
```

### 基础操作

```
git add filename	//将特定文件添加到暂存区
git add .			//将当前目录下所有文件都添加到暂存区
git commit -m '描述信息'	//把暂存区文件提交到仓库
git status 		//查看仓库当前的状态，例如文件被修改，被新添加等等。
```

## 时光机穿梭

### 查看差异

```
git diff			查看工作区与暂存区的差别
git diff --cached	查看暂存区与本地仓库的差别的
git diff HEAD 		查看工作区和本地仓库的差别的。其中：HEAD代表的是最近的一次commit的信息
```

### git版本回退

- 查看版本：git用`head`表示当前版本。上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，以此类推，但是当版本比较多时一般这样写，`HEAD~100`。

```
git log
git log --pretty=oneline	//这个命令显示的修改是单行显示的
```

- 回退操作

  - 回退的本质是回退的版本库的内容，所以有一些大改变可以都通过git记录方便回退。

  - 回退完可以用`git log`查看当前的版本时间轴。

```
git reset --hard 提交编号	可以回到确定的版本，一般用来返回未来
git reset --hard HEAD^	   回到上一个版本。
```

- 补充：
  - 有时我们关机了失去了未来某一个版本的commit id号，这时想再回到未来只能通过`git reflog`来查看我们的git命令操作记录，这样可以重新查看我们的commit id号，进而重新进行回退。

### 撤销修改

- 当文件没有被添加到缓存区或者是添加到缓存区之后进行的修改

```
git checkout -- filename
可以让这个文件回到最近一次git commit或git add时的状态
```

- 当文件被修改后add到了缓存区，但是还没有被提交

```
git reset HEAD filename
可以把暂存区的修改撤销掉，重新放回工作区
```

- 当文件已经被提交到了仓库，这时只能版本回退了。

### 删除文件

- 当把工作区的文件删除了之后，工作区和版本库就不一致了，这时有两种情况
  - 一，你确实要删掉该文件，那么`git rm filename`删除掉，然后git commit
  - 二，删错了，直接`git checkout -- filename`回退即可。
- `git checkout`其实是用版本库里的版本替换工作区的版本，所以无论工作区是修改还是删除，都可以“一键还原”。

## git远程仓库的使用

### 基于http协议

找到仓库的http地址，通过

```
git clone http地址
```

即可将远程仓库克隆到本地并建立起联系。不过这种情况直接在本地修改往仓库push的话需要加上github的用户名，密码的验证。

可以通过修改.git文件夹下的config文件来配置

![image-20200706152919181](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200706152919181.png)

### 基于ssh协议

1. 生成客户端公私玥文件

需要先自行安装openssh，之后

```
ssh-keygen -t rsa -C "注册邮箱"
```

之后得到公私玥文件(public key)位置，找到它并复制其中内容到github上添加一个公钥，标题任意

2. 将公钥上传到github

![image-20200706153703876](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200706153703876.png)

![image-20200706153715186](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200706153715186.png)

这样可以直接通过git进行操作，不用像http那种方式进行验证

### 关联远程仓库

- 将git仓库同步到本地仓库

```
git clone 远程仓库地址
```

- 将本地仓库同步到git仓库

```
git remote add origin 远程仓库地址

//由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
git push -u origin master	

//之后，只要本地做了提交，那么就可以通过下列命令，把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！
git push origin master 
```

### 远程仓库克隆的那些事

- 默认`git clone`克隆的是master分支下的内容，通过`git checkout`指定的分支名可以查看其他分支下的内容。

- 克隆指定的tag，`git clone -b tag标签 git地址`

- 克隆指定分支的代码,`git clone -b 分支名 git地址`


- `git remote -v`，查看远程仓库的信息。如果没有推送权限，就看不到push的地址。

- 每天上班第一件事应该就是git pull，下班最后一件事就是git push。

## 分支管理

我们一般默认的分支都是在master分支。

### 基本操作

```
git branch				  //查看所有分支，当前的分支下会有小梅花标记
git branch 分支名			//创建分支
git checkout 	分支名		//切换分支
git checkout -b 分支名		//-b表示创建分支并切换
git merge 分支名			//git merge命令用于合并指定分支到当前分支
git brancn -d 分支名		//删除分支，不能位于删除的分支下去执行该操作
git push origin --delete 远程分支名		//删除远程分支

新版的git为了避免git checkout -- 有撤销的意义，切换创建分支可以通过git swtich来实现
git switch -c 分支名		//创建并切换到该分支
git switch 	分支名			//切换到该分支
```

### 解决冲突

- 冲突产生的原因就是，我们在dev分支下对文件进行了修改并进行提交，然后切换回master分支也对文件进行了修改并提交。然后我们进行合并时，如果修改的是相同位置的文件，这时我们需要打开git合并失败的文件去进行手动编辑为我们希望的内容，之后再进行提交。
- `git log --graph`，查看分支合并图。

### 分支管理策略

- 通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

- 如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```
实战 --no-ff 方式的git merge
//因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
git merge --no-ff -m "merge with no-ff" dev
```

- 在实际开发中，我们应该按照几个基本原则进行分支管理：

  - 首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

  - 那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

  - 你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

### bug分支

- 在修复bug时，我们会通过创建新的bug分支并进行修复，然后合并，最后删除。
- 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；
- 在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

### feature分支

- 开发一个新feature，最好新建一个分支；
- 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除

### 多人协作开发

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上

```
推送master分支
$ git push origin master
如果要推送其他分支，比如dev，就改成：
$ git push origin dev
```

```
远程协作大致流程
git clone ...	//默认克隆的是master分支下的内容
git checkout -b dev origin/dev	//创建远程origin的dev分支到本地，用这个命令创建本地dev分支
git push origin dev		//把dev分支push到远程，这一步可能发生冲突，也是需要先git pull拉取最新的然后手动进行修改，然后再次进行提交。
```

- 多人协作的工作模式
  - 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
  - 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
  - 如果合并有冲突，则解决冲突，并在本地提交；
  - 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

## 标签管理

- 发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。
- Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

### 创建标签

- 在Git中打标签非常简单，首先，切换到需要打标签的分支上。然后，敲命令`git tag <name>`就可以打一个新标签。

```
git checkout master
git tag v1.0
```

- 默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？方法是找到历史提交的commit id，然后打上就可以了：例如`git tag v0.9 f52c633`。

```
git tag		//查看所有标签，注意，标签不是按时间顺序列出，而是按字母排序的
git show <tagname>	//查看标签信息
git tag -a v0.1 -m "version 0.1 released" 1094adb	//创建带有说明的标签，用-a指定标签名，-m指定说明文字
```

### 操作标签

创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

```
git tag -d v0.1		//删除本地标签
git push origin <tagname>	//推送标签到远程
git push origin --tags		//一次性推送全部尚未推送到远程的本地标签

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
git tag -d v0.9
然后，从远程删除。删除命令也是push，但是格式如下：
git push origin :refs/tags/v0.9
```

## 忽略文件

有时一些文件基本不会改变，或者改变了我们并不想提交到仓库里，我们可以采用忽略文件即

.gitignore 文件，不过该文件没有文件名没法直接创建，可以通过命令行来创建

```
touch .gitignore
```

详细用法参考网上
