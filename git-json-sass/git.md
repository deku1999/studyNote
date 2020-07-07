# git

## git全局配置

在安装后首次使用需要进行全局配置。这里用户名与邮箱地址建议与github的一样。

```
git config --global user.name '用户名'
git config --global user.email '邮箱地址'
```

## 创建厂库

厂库最好是空目录且不要有中文名

1.创建厂库

直接右键或 $ mkdir 文件夹名

2.厂库初始化

git初始化的目的是为了使git知道要管理这个仓库，初始化之后文件夹下会有个.git的隐藏文件夹，不能删除或随意更改。

```
$ git init
```

## git常用指令操作

### 查看当前状态

```
git status
```

### 添加到缓存区

```
git add 文件名
git add 文件名1 文件名2 文件名3..
git add. 
```

git add.代表添加当前目录下所有文件

### 提交至版本库

```
git commit -m"注释内容"
```

## git版本回退

### 查看版本

```
git log
git log --pretty=oneline
```

### 回退操作

```
git reset --hard 提交编号
```

### 小结

- 回到过去之后，要想再回到之前的最新的版本的时候，需要使用指令区查看历史操作，以得到最新的commit id

  ```
  git reflog
  ```

- 要想回到过去必须先得到commit id，然后通过git reset --hard进行回退
- 要想回到未来，需要使用git reflog进行历史操作查看，得到最新的commit id
- 在写回退指令的时候commit id不用写全，git可以自动识别，不过至少需要写前4位

## github远程仓库的使用

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

### 小记

每天上班第一件事应该就是git pull，下班最后一件事就是git push。

## 分支管理

我们一般默认的分支都是在master分支

### 查看分支

```
git branch
```

### 创建分支

```
git branch 分支名
```

### 切换分支

```
git checkout 分支名
```

### 删除分支

```
git branch -d 分支名
```

### 合并分支

```
git merge 被合并的分支名
```

### 小记

- git checkout -b 分支名，如果该分支不存在将会先创建分支再切换
- 查看当前分支有两种方式，一种是直接在命令窗口查看，不过好像只有git bush窗口才有。第二种就是git branch 查看所有分支，当前分支有梅花标记。
- 分支之间的操作是互不影响的，即在a分支下对文件进行的修改，回到b分支时该文件还是b分支时候的状态
- 合并分支是合并当前分支加git merge后的要合并的分支名
- 删除某一分支时不能位于那一分支下
- 合并玩所有分支后记得提交到远程仓库

## git打tag

### 打tag

```
git tag tag名
git push --tags
```

打完tag后可以进行版本回退来继续进行之前的某个操作。

### 切换tag

```
git checkout tag名
```

## 忽略文件

有时一些文件基本不会改变，或者改变了我们并不想提交到仓库里，我们可以采用忽略文件即

.gitignore 文件，不过该文件没有文件名没法直接创建，可以通过命令行来创建

```
touch .gitignore
```

详细用法参考网上

## 冲突的产生与解决

指我们上班忘了git pull，而同事对该仓库文件进行了修改，这时我们进行git push就会报错，因为仓库状态和我们当前电脑上的并不一样。

这时可以重新git pull拉取，然后与同事商量怎么修改，之后再git add ，commit，push即可。