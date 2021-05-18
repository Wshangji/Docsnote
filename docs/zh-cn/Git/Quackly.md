# 快速开始

## 初始化Git 

### 初次运行 Git 前的配置

首先在电脑上安装 Git（官网下载安装），接下来会想要做几件事来定制你的 Git 环境。 每台计算机上只需要配置一 次，程序升级时会保留配置信息。 你可以在任何时候再次通过运行命令来修改它们。 Git 自带一个 `git config `的工具来帮助设置控制 Git 外观和行为的配置变量。 在 Windows 系统中，Git 会查找` $HOME `目录下（一般情况下是 `C:\Users\$USER `）的 `.gitconfig `文件。

你可以通过以下命令查看所有的配置以及它们所在的文件：

```bash
$ git config --list --show-origin
```

### 用户信息

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改

```bash
$ git config --global user.name "用户名"
$ git config --global user.email "邮箱"
```

再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任 何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个 项目目录下运行没有` --global `选项的命令来配置。 

很多 GUI 工具都会在第一次运行时帮助你配置这些信息

### 检查配置信息

如果想要检查你的配置，可以使用`git config --list `命令来列出所有 Git 当时能找到的配置

```bash
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...

```

你可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置（例如： `/etc/gitconfig `与 `~/.gitconfig `）。 这种情况下，Git 会使用它找到的每一个变量的最后一个配置。 

你可以通过输入 `git config ` ： 来检查 Git 的某一项配置

```bash
$ git config user.name
```

### 尝试

在自己方便的盘中新建一个文件夹，这里以MyProject为例，注意路径中不要含有中文字符。打开cmd 命令窗口，操作如下：

#### 初始化仓库

```bash
//初始化仓库
$ git init
```

#### 添加文件

```bash
// 添加一个文件
$ touch README.md
```

#### 提交至缓存区

```bash
//提交单个文件
$ git add README.md

//提交整个目录
$ git add .
```

#### 添加注释

```bash
//添加注释，同时也会提交到本地仓库
$ git commit -m 'first'
```

### 获取帮助

若使用 Git 时需要获取帮助，有三种等价的方法可以找到 Git 命令的综合手册（manpage）：

```bash
$ git help <verb>
$ git <verb> --help
```
例如，要想获得` git config `命令的手册，执行

```bash
$ git help config
```

此外，如果你不需要全面的手册，只需要可用选项的快速参考，那么可以用` -h `选项获得更简明的
“help” 输出：

```bash
$ git add -h
usage: git add [<options>] [--] <pathspec>...
-n, --dry-run dry run
-v, --verbose be verbose
-i, --interactive interactive picking
-p, --patch select hunks interactively
-e, --edit edit current diff and apply
-f, --force allow adding otherwise ignored files
-u, --update update tracked files
--renormalize renormalize EOL of tracked files (implies -u)
-N, --intent-to-add record only the fact that the path will be added
later
-A, --all add changes from all tracked and untracked files
--ignore-removal ignore paths removed in the working tree (same as
--no-all)
--refresh don't add, only refresh the index
--ignore-errors just skip files which cannot be added because of
errors
--ignore-missing check if - even missing - files are ignored in dry
run
--chmod (+|-)x override the executable bit of the listed files
```

## 基础命令

我们怎么知道哪些文件是新添加的，哪些文件已经加入了暂存区域呢？

当然不，作为世界上最伟大的版本控制系统，你能遇到的囧境，Git 早已有了相应的解决方案。随时随地都可以使用git status查看当前状态

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

如果代码报错：git上传代码报错-The file will have its original line endings in your working directory
原因是因为文件中换行符的差别导致的。

> 这里需要知道CRLF和LF的区别：

windows下的换行符是CRLF而Unix的换行符格式是LF。git默认支持LF。

上面的报错的意思是会把CRLF（也就是回车换行）转换成Unix格式（LF），这些是转换文件格式的警
告，不影响使用。

一般commit代码时git会把CRLF转LF，pull代码时LF换CRLF。

解决方案：

```bash
git rm -r --cached .
git config core.autocrlf false
```

然后重新上传代码即可。

为`true`时，Git会将你add的所有文件视为文本问价你，将结尾的CRLF转换为LF，而checkout时会再将文件的LF格式转为CRLF格式。

为`false`时，line endings不做任何改变，文本文件保持其原来的样子。

为`input`时，add时Git会把CRLF转换为LF，而check时仍旧为LF，所以Windows操作系统不建议设置此
值。

输入git status命令，提示如下:

```bash
@DESKTOP MINGW64 ~/Desktop/git-study (master)
$ touch b.txt
@DESKTOP MINGW64 ~/Desktop/git-study (master)
$ git add b.txt
$ git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
new file: b.txt

```

Untracked files 说明存在未跟踪的文件（引号中的内容）

所谓的“未跟踪”文件，是指那些新添加的并且未被加入到暂存区域或提交的文件。它们处于一个逍遥法外的状态，但你一旦将它们加入暂存区域或提交到 Git 仓库，它们就开始受到 Git 的“跟踪”。

> git log 查看历史操作记录

```bash
$ git log
commit 5da78a44017dda030d1fe273e2a470792534ba9a (HEAD -> master)
Author: zhangnan <510180298@qq.com>
Date: Sat Mar 13 16:01:01 2021 +0800

    123

commit c7c0e3bf6d64404e3e68632c24ca13eac38b02e2
Author: zhangnan <510180298@qq.com>
Date: Sat Mar 13 15:53:38 2021 +0800

    first

```

结果中：有head代表当前所处的分之，master代表当前是master分支。两次的提交记录看到了。--pretty=oneline

head git 中的分支，其实本质上仅仅是个指向 commit 对象的可变指针。git 是如何知道你当前在哪个
分支上工作的呢

其实答案也很简单，它保存着一个名为 HEAD 的特别指针。在 git 中，它是一个指向你正在工作中的本
地分支的指针，可以将 HEAD 想象为当前分支的别名。

## 时光回退

有关回退的命令有两个：`reset` 和 `checkout`

### 回滚快照

*注：快照即提交的版本，每个版本我们称之为一个快照*

现在我们利用 reset 命令回滚快照，并看看 Git 仓库和三棵树分别发生了什么

执行 `git reset HEAD~` 命令：

*注：HEAD 表示最新提交的快照，而 HEAD~ 表示 HEAD 的上一个快照，HEAD~~表示上上个快照，如果表示上10个快照，则可以用HEAD ~10*

此时我们的快找回滚到了第二棵数（暂存区域）

*记住：head永远指向当前分支的当前快照*

```bash
$ git --hard reset head~
```

[![g4kIc6.png](https://z3.ax1x.com/2021/05/19/g4kIc6.png)](https://imgtu.com/i/g4kIc6)

> 参数选择

* --hard : 回退版本库，暂存区，工作区。（因此我们修改过的代码就没了，需要谨慎使用）

* reset 不仅移动 HEAD 的指向，将快照回滚动到暂存区域，它还将暂存区域的文件还原到工作目录

* --mixed: 回退版本库，暂存区。(--mixed为git reset的默认参数，即当任何参数都不加的时候的参数)

* --soft: 回退版本库，就相当于只移动 HEAD 的指向，但并不会将快照回滚到暂存区域。相当于撤消了上一次的提交（commit）

[![g4k7nO.png](https://z3.ax1x.com/2021/05/19/g4k7nO.png)](https://imgtu.com/i/g4k7nO)

### 回滚指定快照

`reset` 不仅可以回滚指定快照，还可以回滚个别文件

命令格式为：

```bash
git reset --hard c7c0e3bf6d64404e3e68632c24ca13eac38b02e2
```

这样，它就会将忽略移动 HEAD 的指向这一步（因为你只是回滚快照的部分内容，并不是整个快照，所以 HEAD 的指向不应该发生改变），直接将指定快照的指定文件回滚到暂存区域

**不仅可以往回滚，还可以往前滚！**

这里需要强调的是：reset 不仅是一个“复古”的命令，它不仅可以回到过去，还可以去到“未来”

唯一的一个前提条件是：你需要知道指定快照的 ID 号

**那如果不记得ID号怎么办？**

```bash
git reflog
```

列出Git记录的每一次操作的版本号

```bash
$ git reset --hard 7ce4954
```

回退指定版本号

## 版本对比

### 暂存区与工作树

> 目的：对比版本之间有哪些不同

在已经存在的文件b.txt中添加内容：

```bash
$ git diff
diff --git a/b.txt b/b.txt
index 9ab39d5..4d37a8a 100644
--- a/b.txt
+++ b/b.txt
@@ -2,3 +2,4 @@
1212
123123123
234234234
+手动阀手动阀
```

现在来解释一下上面每一行的含义：
**第一行**：diff --git a/b.txt b/b.txt

表示对比的是存放在暂存区域和工作目录的b.txt

**第二行**：index 9ab39d5..4d37a8a 100644

表示对应文件的 ID 分别是 9ab39d5和 4d37a8a，左边暂存区域，后边当前目录。最后的 100644 是指
定文件的类型和权限

**第三行**：--- a/b.txt

--- 表示该文件是旧文件（存放在暂存区域）

**第四行**：+++ b/b.txt

+++ 表示该文件是新文件（存放在工作区域）

**第五行**：@@ -2,3 +2,4 @@

以 @@ 开头和结束，中间的“-”表示旧文件，“+”表示新文件，后边的数字表示“开始行号，显示行数”
内容：+代表新增的行 -代表少了的行

直接执行 `git diff` 命令是比较暂存区域与工作目录的文件内容：

### 工作树和最新提交