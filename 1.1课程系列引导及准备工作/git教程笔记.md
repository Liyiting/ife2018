# Git教程笔记
教程来自:[廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
## Git简介
### Git的诞生
Linux的创始人Linus在2005年花两周时间用C语言写成，迅速成为最流行的分布式版本控制系统。

2008年Github网站上线，它为开源项目免费提供Git存储。

### 集中式VS分布式
CVS和SVN都是集中式版本控制系统，而Git是分布式版本控制系统。

**集中式版本控制系统**的版本库集中放在中央服务器，每次需从中央服务器取回最新版本，再开始干活，干完活再把自己的活推送给中央服务器。最大的**缺点**就是必须联网工作。

**分布式版本控制系统**没有中央服务器，每个人的电脑上都有一个完整的版本库。通常分布式版本控制系统也有一台充当“中央服务器”的电脑，但仅作用于方便“交换”大家的修改。**优势**是不必联网，还有强大的分支管理。

### 安装git
略
### 创建版本库
* 第一步，找到合适的地方，创建一个空目录

```
$ mkdir learngit
$ cd learngit
```
* 第二步，通过`git init`把这个目录变成git可以管理的仓库:

```
$git init
```
可以通过`ls -ah`命令可以看到隐藏的`.git`目录。

### 添加文件到Git仓库
* 第一步，使用命令`git add <file>`，可反复多次使用，添加多个文件。
* 第二步，使用命令`git commit`，加上需要备注的内容，完成。

## 时光机穿梭
* 使用`git status`掌握仓库当前状态，如待提交等
* 使用`git diff`查看对文件进行了什么修改，difference。

### 版本回退
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907584977fc9d4b96c99f4b5f8e448fbd8589d0b2000/0)
* `HEAD`指向的版本就是当前的版本，版本穿梭使用命令`git reset --hard commit_id`，上个版本是`HEAD^`,上上个版本是`HEAD^^`,往上100个版本是`HEAD~100`

* 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。如果嫌输出信息太多，可以加上`--pretty=online`参数。

* 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。
### 工作区和暂存区
* 工作区: 就是在电脑里能看到的目录
* 版本库: 工作区隐藏目录`.git`，是Git的版本库。

Git的版本库中存了很多东西，最重要的有暂存区(stage)，还有git为我们自动创建的第一个分支`master`，以及指向`master`的指针叫`HEAD`
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

所以`git add`就是把要提交的多有修改放到暂存区(stage)，然后执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

### 管理修改
第一次修改 -> `git add` -> 第二次修改 -> `git commit`

(第二次修改不会被提交)

第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

(这样就能提交第二次修改了)

### 撤销修改
* 场景一：想直接丢掉工作区修改时，用`git checkout -- file`，撤销回和版本库一样的状态。
* 场景二：不但修改了文件，还添加到了暂存区时，想丢弃修改，分两步，第一步用`git reset HEAD <file>`，回到场景1，第二步再按场景一操作。
* 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考之前版本回退一节，使用`git reset --hard commit_id`进行版本回退。前提是没有提交到远程库。

### 删除文件
* 命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。
* 不小心删错时，可以用`git checkout -- <file>`，轻松把误删文件恢复到最新版本。这个操作其实是用版本库里的版本替换工作区的版本，无论修改或删除都能一键还原。

## 远程仓库
除了可以自己搭建一台运行Git的服务器，还可以将代码先托管在GitHub(Git的远程仓库)上。

由于本地Git和GitHub仓库之间是通过SSH加密的，所以需要一点设置：

* 第1步：创建SSH key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

* 第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面，然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0)
点“Add Key”，你就应该看到已经添加的Key。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

### 添加远程库
