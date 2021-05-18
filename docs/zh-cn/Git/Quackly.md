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

