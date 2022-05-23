# Git教程

## [#](https://www.ydlclass.com/doc21xnv/basics/git/#第一章-git安装)第一章 Git安装

windows安装：进入网站https://git-scm.com/下载安装，然后在cmd命令行配置

直接去腾讯软件中心下载也可以！

```php
> git config --global user.name "itnanls"
> git config --global user.email "itnanls@163.com"
#检查信息是否写入成功
git config --list 
```

ubuntu配置：apt-get install git

centos配置：yum install git

## [#](https://www.ydlclass.com/doc21xnv/basics/git/#第二章-理论基础)第二章 理论基础

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#一、git-是什么)一、Git 是什么？

那么，简单地说，Git 究竟是怎样的一个系统呢？ 请注意接下来的内容非常重要，若你理解了 Git 的思想和基本工作原理，用起来就会知其所以然，游刃有余。 在学习 Git 时，请尽量理清你对其它版本管理系统已有的认识，如 CVS、Subversion 或 Perforce， 这样能帮助你使用工具时避免发生混淆。尽管 Git 用起来与其它的版本控制系统非常相似， 但它在对信息的存储和认知方式上却有很大差异，理解这些差异将有助于避免使用中的困惑。

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#二、三种状态)二、三种状态

现在请注意，如果你希望后面的学习更顺利，请记住下面这些关于 Git 的概念。 Git 有三种状态，你的文件可能处于其中之一： **已提交（committed）**、**已修改（modified）** 和 **已暂存（staged）**。

- 已修改表示修改了文件，但还没保存到数据库中。
- 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
- 已提交表示数据已经安全地保存在本地数据库中。

这会让我们的 Git 项目拥有三个阶段：工作区、暂存区以及 Git 目录。

![工作区、暂存区以及 Git 目录。](https://www.ydlclass.com/doc21xnv/assets/areas.86c16db0.png)

工作目录、暂存区域以及 Git 仓库.

工作区是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

暂存区是一个文件，保存了下次将要提交的文件列表信息，一般在 Git 仓库目录中。 按照 Git 的术语叫做“索引”，不过一般说法还是叫“暂存区”。

Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，复制的就是这里的数据。

基本的 Git 工作流程如下：

1. 在工作区中修改文件。
2. 将你想要下次提交的更改选择性地暂存，这样只会将更改的部分添加到暂存区。
3. 提交更新，找到暂存区的文件，将快照永久性存储到 Git 目录。

如果 Git 目录中保存着特定版本的文件，就属于 **已提交** 状态。 如果文件已修改并放入暂存区，就属于 **已暂存** 状态。 如果自上次检出后，作了修改但还没有放到暂存区域，就是 **已修改** 状态。 在 [Git 基础open in new window](https://git-scm.com/book/zh/v2/ch00/ch02-git-basics-chapter) 一章，你会进一步了解这些状态的细节， 并学会如何根据文件状态实施后续操作，以及怎样跳过暂存直接提交。

如上，如果每个版本中有文件发生变动，Git 会将整个文件复制并保存起来。这种设计看似会多消耗更多的空间，但在分支管理时却是带来了很多的益处和便利。

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#三、git-保证完整性)三、Git 保证完整性

Git 中所有的数据在存储前都计算校验和，然后以校验和来引用。 这意味着不可能在 Git 不知情时更改任何文件内容或目录内容。 这个功能建构在 Git 底层，是构成 Git 哲学不可或缺的部分。 若你在传送过程中丢失信息或损坏文件，Git 就能发现。

Git 用以计算校验和的机制叫做 SHA-1 散列（hash，哈希）。 这是一个由 40 个十六进制字符（0-9 和 a-f）组成的字符串，基于 Git 中文件的内容或目录结构计算出来。 SHA-1 哈希看起来是这样：

```text
24b9da6552252987aa493b52f8696cd6d3b00373
```

Git 中使用这种哈希值的情况很多，你将经常看到这种哈希值。 实际上，Git 数据库中保存的信息都是以文件内容的哈希值来索引，而不是文件名。

## [#](https://www.ydlclass.com/doc21xnv/basics/git/#第三章-实战)第三章 实战

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#一、初始化git)一、初始化Git

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1、初次运行-git-前的配置)1、初次运行 Git 前的配置

既然已经在系统上安装了 Git，你会想要做几件事来定制你的 Git 环境。 每台计算机上只需要配置一次，程序升级时会保留配置信息。 你可以在任何时候再次通过运行命令来修改它们。

Git 自带一个 `git config` 的工具来帮助设置控制 Git 外观和行为的配置变量。

在 Windows 系统中，Git 会查找 `$HOME` 目录下（一般情况下是 `C:\Users\$USER` ）的 `.gitconfig` 文件。

你可以通过以下命令查看所有的配置以及它们所在的文件：

```bash
$ git config --list --show-origin
```

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_2、用户信息)2、用户信息

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：

```console
$ git config --global user.name "itnanls"
$ git config --global user.email "510180298@qq.com"
```

再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 `--global` 选项的命令来配置。

很多 GUI 工具都会在第一次运行时帮助你配置这些信息。

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_3、检查配置信息)3、检查配置信息

如果想要检查你的配置，可以使用 `git config --list` 命令来列出所有 Git 当时能找到的配置。

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

你可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置（例如：`/etc/gitconfig` 与 `~/.gitconfig`）。 这种情况下，Git 会使用它找到的每一个变量的最后一个配置。

你可以通过输入 `git config <key>`： 来检查 Git 的某一项配置

```bash
$ git config user.name
John Doe
```

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_4、尝试)4、尝试

在自己方便的盘中新建一个文件夹，这里以MyProject为例，注意路径中不要含有中文字符。打开cmd命令窗口，操作如下：

找一个空文件夹：

点击鼠标右键-----》git bash here

```bash
// 初始化 仓库
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study
$ git init
Initialized empty Git repository in C:/Users/51018/Desktop/git-study/.git/
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
    
// 添加一个文件
$ touch a.txt
$ echo 123 > a.txt
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
    
// 提交至缓存区
$ git add a.txt
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)

// 提交到本地仓库
$ git commit -m 'first'
[master (root-commit) ac41d06] first
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
```

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_5、获取帮助)5、获取帮助

若你使用 Git 时需要获取帮助，有三种等价的方法可以找到 Git 命令的综合手册（manpage）：

```bash
$ git help <verb>
$ git <verb> --help
```

例如，要想获得 `git config` 命令的手册，执行

```bash
$ git help config
```

此外，如果你不需要全面的手册，只需要可用选项的快速参考，那么可以用 `-h` 选项获得更简明的 “help” 输出：

```bash
$ git add -h
usage: git add [<options>] [--] <pathspec>...

    -n, --dry-run         dry run
    -v, --verbose         be verbose

    -i, --interactive     interactive picking
    -p, --patch           select hunks interactively
    -e, --edit            edit current diff and apply
    -f, --force           allow adding otherwise ignored files
    -u, --update          update tracked files
    --renormalize         renormalize EOL of tracked files (implies -u)
    -N, --intent-to-add   record only the fact that the path will be added later
    -A, --all             add changes from all tracked and untracked files
    --ignore-removal      ignore paths removed in the working tree (same as --no-all)
    --refresh             don't add, only refresh the index
    --ignore-errors       just skip files which cannot be added because of errors
    --ignore-missing      check if - even missing - files are ignored in dry run
    --chmod (+|-)x        override the executable bit of the listed files
```

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#二、基础命令)二、基础命令

我们怎么知道哪些文件是新添加的，哪些文件已经加入了暂存区域呢？总不能让我们自己拿个本本记下来吧？ 当然不，作为世界上最伟大的版本控制系统，你能遇到的囧境，Git 早已有了相应的解决方案。随时随地都可以使用**git status**查看当前状态

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

```bash
$ git add b.txt
```

如果代码报错：git上传代码报错-The file will have its original line endings in your working directory

原因是因为文件中换行符的差别导致的。

> 这里需要知道CRLF和LF的区别：

windows下的换行符是CRLF而Unix的换行符格式是LF。git默认支持LF。

上面的报错的意思是会把CRLF（也就是回车换行）转换成Unix格式（LF），这些是转换文件格式的警告，不影响使用。

一般commit代码时git会把CRLF转LF，pull代码时LF换CRLF。

解决方案：

```bash
git rm -r --cached .
git config core.autocrlf false
```

然后重新上传代码即可。

为true时，Git会将你add的所有文件视为文本问价你，将结尾的CRLF转换为LF，而checkout时会再将文件的LF格式转为CRLF格式。

为false时，line endings不做任何改变，文本文件保持其原来的样子。

为input时，add时Git会把CRLF转换为LF，而check时仍旧为LF，所以Windows操作系统不建议设置此值。

输入**git status**命令，提示如下：

```bash
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ echo 1234 > b.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git add b.txt

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   b.txt


51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
```

**Untracked files** 说明存在未跟踪的文件（下边红色的那个）

所谓的“未跟踪”文件，是指那些新添加的并且未被加入到暂存区域或提交的文件。它们处于一个逍遥法外的状态，但你一旦将它们加入暂存区域或提交到 Git 仓库，它们就开始受到 Git 的“跟踪”。

这里圆括号中的英文是 git 给我们的建议：使用 git add file 命令将待提交的文件添加到暂存区域。

```csharp
F:\MyProject>git add LICENSE

F:\MyProject>git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   LICENSE(绿色)
```

再次添加到暂存区域，然后执行 git commit -m "b.txt" 命令：

修改数据

```rust
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ echo 123 > b.txt

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   b.txt

no changes added to commit (use "git add" and/or "git commit -a")

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
```

修改文件后，使用git status查看数据。

> git log 查看历史操作记录

```bash
$ git log
commit 5da78a44017dda030d1fe273e2a470792534ba9a (HEAD -> master)
Author: zhangnan <510180298@qq.com>
Date:   Sat Mar 13 16:01:01 2021 +0800

    123

commit c7c0e3bf6d64404e3e68632c24ca13eac38b02e2
Author: zhangnan <510180298@qq.com>
Date:   Sat Mar 13 15:53:38 2021 +0800

    first

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
```

```text
* d5a12d8a966da5bf36c1f4a080c5d507398f5f59 (HEAD -> master) first
```

结果中：有head代表当前所处的分之，master代表当前是master分支。可以按下不表。

两次的提交记录看到了。--pretty=oneline

head git 中的分支，其实本质上仅仅是个指向 commit 对象的可变指针。git 是如何知道你当前在哪个分支上工作的呢？ 其实答案也很简单，它保存着一个名为 HEAD 的特别指针。在 git 中，它是一个指向你正在工作中的本地分支的指针，可以将 HEAD 想象为当前分支的别名。

```bash
$ git log --graph
```

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#三、时光回退)三、时光回退

有关回退的命令有两个：**reset 和 checkout**

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1、回滚快照)1、回滚快照

*注：快照即提交的版本，每个版本我们称之为一个快照。*

现在我们利用 reset 命令回滚快照，并看看 Git 仓库和三棵树分别发生了什么。

执行 git reset HEAD~ 命令：

*注：HEAD 表示最新提交的快照，而 HEAD~ 表示 HEAD 的上一个快照，HEAD~~表示上上个快照，如果表示上10个快照，则可以用HEAD ~10*

此时我们的快找回滚到了第二棵数（暂存区域）

记住：head永远指向当前分支的当前快照

```bash
$ git  --hard reset head~

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git log
commit c7c0e3bf6d64404e3e68632c24ca13eac38b02e2 (HEAD -> master)
Author: zhangnan <510180298@qq.com>
Date:   Sat Mar 13 15:53:38 2021 +0800

    first

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
```

可以看到，只剩下一个记录了。

![image-20210316212457416](https://www.ydlclass.com/doc21xnv/assets/image-20210316212457416.07ed7c09.png)

> 参数选择

--hard : 回退版本库，暂存区，工作区。（因此我们修改过的代码就没了，需要谨慎使用）

reset 不仅移动 HEAD 的指向，将快照回滚动到暂存区域，它还将暂存区域的文件还原到工作目录。

--mixed: 回退版本库，暂存区。(--mixed为git reset的默认参数，即当任何参数都不加的时候的参数)

--soft: 回退版本库。

就相当于只移动 HEAD 的指向，但并不会将快照回滚到暂存区域。相当于撤消了上一次的提交（commit）。

![image-20210316214437985](https://www.ydlclass.com/doc21xnv/assets/image-20210316214437985.3e04547e.png)

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_2、回滚指定快照)2、回滚指定快照

reset 不仅可以回滚指定快照，还可以回滚个别文件。

命令格式为：

```bash
git reset --hard  c7c0e3bf6d64404e3e68632c24ca13eac38b02e2
```

这样，它就会将忽略移动 HEAD 的指向这一步（因为你只是回滚快照的部分内容，并不是整个快照，所以 HEAD 的指向不应该发生改变），直接将指定快照的指定文件回滚到暂存区域。

**不仅可以往回滚，还可以往前滚！**

这里需要强调的是：reset 不仅是一个“复古”的命令，它不仅可以回到过去，还可以去到“未来”。

唯一的一个前提条件是：你需要知道指定快照的 ID 号。

**那如果不小心把命令窗口关了不记得ID号怎么办？** 命令：

```bash
git reflog
```

Git记录的每一次操作的版本ID号

```bash
$ git reset --hard 7ce4954
```

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#四、版本对比)四、版本对比

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1、暂存区与工作树)1、暂存区与工作树

目的：对比版本之间有哪些不同

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

**第一行：**diff --git a/b.txt b/b.txt 表示对比的是存放在暂存区域和工作目录的b.txt

**第二行：**index 9ab39d5..4d37a8a 100644 表示对应文件的 ID 分别是 9ab39d5和 4d37a8a，左边暂存区域，后边当前目录。最后的 100644 是指定文件的类型和权限

**第三行：**--- a/b.txt

--- 表示该文件是旧文件（存放在暂存区域）

**第四行：**+++ b/b.txt +++ 表示该文件是新文件（存放在工作区域）

**第五行：**@@ -2,3 +2,4 @@ 以 @@ 开头和结束，中间的“-”表示旧文件，“+”表示新文件，后边的数字表示“开始行号，显示行数”

内容：+代表新增的行 -代表少了的行

直接执行 git diff 命令是比较暂存区域与工作目录的文件内容：

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_2、工作树和最新提交)2、工作树和最新提交

```text
$ git diff head
warning: LF will be replaced by CRLF in b.txt.
The file will have its original line endings in your working directory
diff --git a/b.txt b/b.txt
new file mode 100644
index 0000000..4d37a8a
--- /dev/null
+++ b/b.txt
@@ -0,0 +1,5 @@
+123
+1212
+123123123
+234234234
+手动阀手动阀

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
```

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_3、两个历史快照)3、两个历史快照

```bash
$ git diff 5da78a4 c7c0e3b
diff --git a/b.txt b/b.txt
deleted file mode 100644
index 81c545e..0000000
--- a/b.txt
+++ /dev/null
@@ -1 +0,0 @@
-1234
```

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_4、比较仓库和暂存区)4、比较仓库和暂存区

```bash
$ git diff --cached c7c0e3b
diff --git a/b.txt b/b.txt
new file mode 100644
index 0000000..9ab39d5
--- /dev/null
+++ b/b.txt
@@ -0,0 +1,4 @@
+123
+1212
+123123123
+234234234
```

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#五、删除文件)五、删除文件

> 不小心删除文件怎么办？

现在从工作目录中手动删除 b.txt 文件，然后执行 git status 命令：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    b.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

提醒使用 checkout 命令可以将暂存区域的文件恢复到工作目录：

```bash
$ git checkout -- b.txt
```

文件就会重新返回。

> 那么如何彻底删除一个文件呢？

假如你不小心把小黄图下载到了工作目录，然后又不小心提交到了 Git 仓库：

新增一个c.txt文件

```text
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ echo 123 > c.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git add .
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git commit -m 'third'
[master 3bd84d8] third
 1 file changed, 1 insertion(+)
 create mode 100644 c.txt
```

还有方法：

执行 git rm a.txt 命令：

```text
$ git rm c.txt
rm 'c.txt'
```

此时工作目录中的c.txt已经被删除……

```bash
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ ls
a.txt  b.txt  mintty.exe.stackdump
```

但执行 git status 命令，你仍然发现 Git 还不肯松手：

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        deleted:    c.txt
```

意思是说它在仓库的快照中发现有个叫 c.txt 的东西，但似乎在暂存区域和当前目录不见了！

此时可以执行 git reset --soft HEAD~ 命令将快照回滚到上一个位置，然后重新提交，就好了：

**注意：rm 命令删除的只是工作目录和暂存区域的文件（即取消跟踪，在下次提交时不纳入版本管理）**

> 缓冲区和工作树的内容不一致，怎么删除

1、修改b.txt 添加至缓冲区

2、再修改b.txt

3、git rm c.txt

```bash
$ echo 123 > b.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git add b.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ echo 123 > b.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git rm b.txt
error: the following file has changes staged in the index:
    b.txt
(use --cached to keep the file, or -f to force removal)
```

因为两个不同内容的同名文件，谁知道你是不是搞清楚了都要删掉？还是提醒一下好，别等一下出错了又要赖机器…… 根据提示，执行 git rm -f b.txt命令就可以把两个都删除。

> 我只想删除暂存区域的文件，保留工作目录的，应该怎么操作？

执行 git rm --cached 文件名 命令。

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#六、重命名文件)六、重命名文件

直接在工作目录重命名文件，执行git status出现错误：

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   b.txt

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    b.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        n.txt
```

正确的姿势应该是：

git mv 旧文件名 新文件名

```bash
$ git mv b.txt c.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        renamed:    b.txt -> c.txt
```

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#七、忽略文件)七、忽略文件

**如何让Git 识别某些格式的文件，然后自主不跟踪它们？** 比如工作目录中有三个文件1.temp、2.temp 和 3.temp，我们不希望后缀名为 temp 的文件被追踪，可是每次执行git status都会出现：

解决办法：在工作目录创建一个名为 .gitignore 的文件。

然后你发现 Windows 压根儿不允许你在文件管理器中创建以点（.）开头的文件。windows需要在命令行窗口创建（.）开头的文件。执行 echo *.temp > .gitignore 命令，创建一个 .gitignore 文件，并让 Git 忽略所有 .temp 后缀的文件：

```bash
$ echo *.temp > .gitignore
```

```bash
$ echo *.temp > .gitignore
```

在工作目录创建 a.temp

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        renamed:    b.txt -> c.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore


51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
```

好了，Git 已经忽略了所有的 *.temp 文件（你还可以把 .gitignore 文件也一并忽略）。

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#八、创建和切换分支)八、创建和切换分支

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1、分支是什么)1、分支是什么？

假设你的大项目已经上线了（有上百万人在使用），过了一段时间

你突然觉得应该添加一些新的功能，但是为了保险起见，你肯定不能在当前项目上直接进行开发，这时候你就有创建分支的需要了。

![image-20210316221252036](https://www.ydlclass.com/doc21xnv/assets/image-20210316221252036.657de827.png)

对于其它版本控制系统而言，创建分支常常需要完全创建一个源代码目录的副本，项目越大，耗费的时间就越多；而 Git 由于每一个结点都已经是一个完整的项目，所以只需要创建多一个“指针”（像 master）指向分支开始的位置即可。

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_2、创建分支)2、创建分支

执行git status查看状态：

```bash
$ git status
On branch master
```

创建分支，使用 git branch 分支名 命令：

```bash
$ git branch feature01
```

没有任何提示说明分支创建成功（一般也不会失败啦，除非创建了同名的分支会提醒你一下），此时可以执行 git log --decorate 命令查看：

*如果希望以“精简版”的方式显示，可以加上一个 --oneline 选项（即 git log --decorate --oneline），这样就只用一行来显示一个快照记录。*

```bash
$ git log
commit 432621d36faf270eae133cfe2e976fc99df479a5 (HEAD -> master, feature01)
Author: zhangnan <510180298@qq.com>
Date:   Sat Mar 13 17:43:53 2021 +0800

    1

commit 4c9e83b6d4ca3ca3d8b0b77bb5aca614dd755413
Author: zhangnan <510180298@qq.com>
Date:   Sat Mar 13 17:11:51 2021 +0800

    123
```

可以看到最新的快照后边多了一个 (HEAD -> master, feature01)

它的意思是：目前有两个分支，一个是主分支（master），一个是刚才我们创建的新分支（feature），然后 HEAD 指针仍然指向默认的 master 分支。

```bash
$ git log --decorate --oneline
432621d (HEAD -> master, feature01) 1
4c9e83b 123
8af2e68 secong
c7c0e3b first
```

所以目前仓库中的快照应该是这样：head --》 master feature01

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_3、切换分支)3、切换分支

现在我们需要将工作环境切换到新创建的分支（feature）上，使用的就是之前我们欲言又止的 checkout 命令。执行 git checkout feature 命令：

```bash
$ git checkout feature01
Switched to branch 'feature01'

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (feature01)
$ git status
On branch feature01
nothing to commit, working tree clean
```

现在我们进行一次提交（暂存区域还有一个更改的文件没有提交呢）：

创建d.txt文件

```text
$ git add d.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (feature01)
$ git commit -am 'feature01'
[feature01 f5e0b68] feature01
 1 file changed, 1 insertion(+)
 create mode 100644 d.txt
```

现在仓库中的快照应该是酱紫（提交的快照由当前HEAD指针指向的分支来管理）：

![image-20210316221845292](https://www.ydlclass.com/doc21xnv/assets/image-20210316221845292.08b8f813.png)

然后我们将 HEAD 指针切回 master 分支：

```bash
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (feature01)
$ ls
a.temp  a.txt  c.txt  d.txt  mintty.exe.stackdump

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (feature01)
$ git checkout master
Switched to branch 'master'

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ ls
a.temp  a.txt  c.txt  mintty.exe.stackdump
```

细心的朋友会发现上一次对 README.md 文件的修改已经荡然无存了，这是因为我们的工作目录已经回到 master 分支的状态中：

> 在不同的分支分别提交

```bash
$ git status
On branch master
nothing to commit, working tree clean

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ echo 333 >> c.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git add c.txt
warning: LF will be replaced by CRLF in c.txt.
The file will have its original line endings in your working directory

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git commit -m 'master'
[master baccb7f] master
 1 file changed, 1 insertion(+)

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git checkout feature01
Switched to branch 'feature01'

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (feature01)
$ echo 333 >> c.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (feature01)
$ git add c.txt
warning: LF will be replaced by CRLF in c.txt.
The file will have its original line endings in your working directory

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (feature01)
$ git commit -m 'feature01'
[feature01 b134862] feature01
 1 file changed, 1 insertion(+)

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (feature01)
$ got log --graph

| Author: zhangnan <510180298@qq.com>
| Date:   Sat Mar 13 18:00:03 2021 +0800
|
|     feature01
|
* commit f5e0b68217b66d959bf9eeed7ad7631e4365f355
| Author: zhangnan <510180298@qq.com>
| Date:   Sat Mar 13 17:50:20 2021 +0800
|
|     feature01
|
| * commit baccb7f29da8143adebc79ca4c10e15204e79411 (master)
|/  Author: zhangnan <510180298@qq.com>
|   Date:   Sat Mar 13 17:59:20 2021 +0800
|
|       master
|
* commit 432621d36faf270eae133cfe2e976fc99df479a5
| Author: zhangnan <510180298@qq.com>
| Date:   Sat Mar 13 17:43:53 2021 +0800
|
|     1
|
* commit 4c9e83b6d4ca3ca3d8b0b77bb5aca614dd755413
| Author: zhangnan <510180298@qq.com>
| Date:   Sat Mar 13 17:11:51 2021 +0800
|
|     123
|
```

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#九、合并分支)九、合并分支

新建一个仓库

```bash
// 初始化一个仓库
$ git init
// 创建一个a.txt 文件，并且修改他的内容
$ touch a.txt

// 提交该分支
$ git add a.txt
$ git commit -m 'master'

// 切出一个分支
$ git branch feature1
// 切换到该分支
$ git checkout feature1
Switched to branch 'feature1'
// 随意修改a.txt的内容
。。。
// 切换回主分支
$ git checkout master
Switched to branch 'master'
// 合并分支
$ git merge feature1
Updating 540e027..cae5dfc
Fast-forward
 a.txt | 8 +++++++-
 1 file changed, 7 insertions(+), 1 deletion(-)
```

![image-20210315100125472](https://www.ydlclass.com/doc21xnv/assets/image-20210315100125472.2c462a31.png)

当一个子分支的使命完结之后，它就应该回归到主分支中去。

```bash
$ git log --decorate --all --graph --online
fatal: unrecognized argument: --online

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (feature01)
$ git log --decorate --all --graph --oneline
* b134862 (HEAD -> feature01) feature01
* f5e0b68 feature01
| * baccb7f (master) master
|/
* 432621d 1
* 4c9e83b 123
* 8af2e68 secong
* c7c0e3b first
```

合并分支我们使用 merge 命令，执行 git merge feature01命令，将 feature 分支合并到 HEAD 所在的分支（master）上：

第一步 切出一个feature2分支，修改master分支中a.txt第一行数据，

```bash
// 先切出一个分支
$ git branch feature2

// 在master分支做修改，修改a.txt的第一行数据
$ git add a.txt

//提交master分支
$ git commit -m 'master'

// 切换到feature2 分支
$ git checkout feature2
Switched to branch 'feature2'

// 同样修改a.txt的第一行数据
$ git add a.txt
// 提交
$ git commit -m feature2
[feature2 0ebb84a] feature2
 1 file changed, 1 insertion(+), 1 deletion(-)

// 切换到master分支
$ git checkout master
Switched to branch 'master'

// 将feature2合并到master分支上
$ git merge feature2
// 发生了问题
Auto-merging a.txt
CONFLICT (content): Merge conflict in a.txt
Automatic merge failed; fix conflicts and then commit the result.
```

a.txt内容变成了如下：

```txt
<<<<<<< HEAD
123123
=======
123345
>>>>>>> feature2
```

意思是说现在你需要先解决冲突的问题，Git 才能进行合并操作。所谓冲突，无非就是像两个分支中存在同名但内容却不同的文件，Git 不知道你要舍弃哪一个或保留哪一个，所以需要你自己来决定。 此时执行 git status 命令也会显示需要你解决的冲突：

```bash
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

以“=======”为界，上到“<<<<<<< HEAD”的内容表示当前分支，下到“>>>>>>> feature”表示待合并的 feature 分支，之间的内容就是冲突的地方。

![image-20210315100412732](https://www.ydlclass.com/doc21xnv/assets/image-20210315100412732.a95cb134.png)

我们就需要手动修改：

```text
123123
```

1

```bash
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master|MERGING)
$ git add a.txt

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master|MERGING)
$ git commit -m '解决冲突'
[master 569943e] 解决冲突
```

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#十、删除分支)十、删除分支

当一个功能开发完成，并且成功合并到主分支，我们应该删除分支

使用 git branch -d 分支名 命令：

执行 git log --decorate --all --graph --oneline 命令：

由于 Git 的分支原理实际上只是通过一个指针记载，所以创建和删除分支都几乎是瞬间完成。

*注意：如果试图删除未合并的分支，Git 会提示你“该分支未完全合并，如果你确定要删除，请使用 git branch -D 分支名 命令。*

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#十一、变基)十一、变基

当我们开发一个功能时，可能会在本地有无数次commit，而你实际上在你的master分支上只想显示每一个功能测试完成后的一次完整提交记录就好了，其他的提交记录并不想将来全部保留在你的master分支上，那么rebase将会是一个好的选择，他可以在rebase时将本地多次的commit合并成一个commit，还可以修改commit的描述等

```text
// 合并前两次的commit
git  rebase -i head~~

// 合并此次commit在最新commit的提交
git rebaser -i hash值
```

## [#](https://www.ydlclass.com/doc21xnv/basics/git/#第四章-码云的使用)第四章 码云的使用

既然git是一个团队合作开发的工具，那本地的仓库肯定不能满足团队开发的需求！就必须要有一个远程仓库统一管理我们的代码。

这类的工具有很多，公网上的有github，国内的有码云 gitee

公司内部直接使用 gitlab （等学习了docker后我们部署一个gitlab）

![image-20210316223001173](https://www.ydlclass.com/doc21xnv/assets/image-20210316223001173.f8e3d9fe.png)

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#一、基本了解)一、基本了解

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1、github)1、github

![image-20210316141240499](https://www.ydlclass.com/doc21xnv/assets/image-20210316141240499.a40a7b38.png)

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_2、码云)2、码云

![image-20210316141308572](https://www.ydlclass.com/doc21xnv/assets/image-20210316141308572.271089e2.png)

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_3、gitlab独立部署)3、gitlab独立部署

![image-20210316141401006](https://www.ydlclass.com/doc21xnv/assets/image-20210316141401006.19f8e475.png)

但是基于网路的原因和学习成本，咱们使用码云。会一个就都会了。

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#二、基本使用)二、基本使用

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1、注册个账号)1、注册个账号

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_2、认识界面)2、认识界面

开源软件--》里边有很多的优秀的开源软件学习

![image-20210316142109047](https://www.ydlclass.com/doc21xnv/assets/image-20210316142109047.35dd766c.png)

> 我的控制台

![image-20210316142040365](https://www.ydlclass.com/doc21xnv/assets/image-20210316142040365.8bbc87fb.png)

![image-20210316142303377](https://www.ydlclass.com/doc21xnv/assets/image-20210316142303377.bbfde513.png)

> 贡献度和动态

![image-20210316142318619](https://www.ydlclass.com/doc21xnv/assets/image-20210316142318619.b069e8c7.png)

> 认识仓库

![image-20210316142705234](https://www.ydlclass.com/doc21xnv/assets/image-20210316142705234.0bcef3c6.png)

- pull Request 开发者在本地对源代码进行修改之后，想仓库提交请求合并的功能
- Wiki 该功能通常用作文档手册的编写当中
- Issues：是将一个任务或问题分配给一个issue进行跟踪和管理，可以当做bug管理系统使用，每一个功能的更正或修改都应该对应一个issue，只要看issues就能看到关于这个更改的所有信息
- 统计就是仓库各项数据的数据统计，devOPs是持续继承、持续交付的服务，服务：其他码云提供的一些服务。
- 管理：对仓库的一些修改删除等操作：

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_3、新建仓库)3、新建仓库

![image-20210316143811271](https://www.ydlclass.com/doc21xnv/assets/image-20210316143811271.330f6ba6.png)

![image-20210316143923380](https://www.ydlclass.com/doc21xnv/assets/image-20210316143923380.bf0502e4.png)

![image-20210316144205201](https://www.ydlclass.com/doc21xnv/assets/image-20210316144205201.d0096190.png)

> 第一次进入

![image-20210316144308897](https://www.ydlclass.com/doc21xnv/assets/image-20210316144308897.2002da30.png)

> 会让你输入密码

![image-20210316144512234](https://www.ydlclass.com/doc21xnv/assets/image-20210316144512234.10cc368d.png)

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_4、建立本地仓库)4、建立本地仓库

```bash
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study
$ git config --global user.name "Alm张楠"

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study
$ git config --global user.email "510180298@qq.com"

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study
$ cd git-study
bash: cd: git-study: No such file or directory
touch README.md
git add README.md
51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study
$ git init
Initialized empty Git repository in C:/Users/51018/Desktop/git-study/.git/

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ touch README.md

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git add README.md

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git commit -m "first commit"
[master (root-commit) d885e2e] first commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git remote add origin https://gitee.com/zhangnan716/git-study.git

51018@DESKTOP-6R8BLO2 MINGW64 ~/Desktop/git-study (master)
$ git push -u origin master
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 215 bytes | 215.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Powered by GITEE.COM [GNK-5.0]
To https://gitee.com/zhangnan716/git-study.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

> 第一个关键：添加一个远程仓库

```text
$ git remote add origin https://gitee.com/zhangnan716/git-study.git
```

> 把代码推送到远程仓库

```bash
$ git push -u origin master
```

> 列出所有的远程仓库

```text
git remote -v
```

> 显示某个远程仓库信息

```text
git remote show [remote]
```

> git push 的其他命令**

这几个常见的用法已足以满足我们日常开发的使用了，还有几个扩展的用法，如下：

```bash
 git push -u origin master 
```

如果当前分支与多个主机存在追踪关系，则可以使用 -u 参数指定一个默认主机，这样后面就可以不加任何参数使用git push，

不带任何参数的git push，默认只推送当前分支，这叫做simple方式，还有一种matching方式，会推送所有有对应的远程分支的本地分支， Git 2.0之前默认使用matching，现在改为simple方式

如果想更改设置，可以使用git config命令。git config --global push.default matching OR git config --global push.default simple；可以使用git config -l 查看配置

```bash
 git push --all origin 
```

当遇到这种情况就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要 -all 选项

```bash
 git push --force origin 
```

git push的时候需要本地先git pull更新到跟服务器版本一致，如果本地版本库比远程服务器上的低，那么一般会提示你git pull更新，如果一定要提交，那么可以使用这个命令。

![image-20210316144809258](https://www.ydlclass.com/doc21xnv/assets/image-20210316144809258.c175de5a.png)

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_5、其他功能)5、其他功能

> 打标签

![image-20210316144902740](https://www.ydlclass.com/doc21xnv/assets/image-20210316144902740.193b760b.png)

> 分支选择

![image-20210316144940684](https://www.ydlclass.com/doc21xnv/assets/image-20210316144940684.1e4cb7d5.png)

> 创建分支

![image-20210316145026969](https://www.ydlclass.com/doc21xnv/assets/image-20210316145026969.8e809286.png)

![image-20210316145048834](https://www.ydlclass.com/doc21xnv/assets/image-20210316145048834.231f716d.png)

> 在统计模块，你还可以发布发型版，提供下载

![image-20210316145150233](https://www.ydlclass.com/doc21xnv/assets/image-20210316145150233.3a655df0.png)

![image-20210316145202029](https://www.ydlclass.com/doc21xnv/assets/image-20210316145202029.76d92ba6.png)

### [#](https://www.ydlclass.com/doc21xnv/basics/git/#三、一般的协同开发流程)三、一般的协同开发流程

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1、仓库)1、仓库

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1-源仓库-线上版本库)（1）源仓库(线上版本库)

在项目的开始,项目的发起者构建起一个项目的最原始的仓库,称为`origin。`

源仓库的有两个作用：

- 汇总参与该项目的各个开发者的代码
- 存放趋于稳定和可发布的代码

源仓库应该是受保护的，开发者不应该直接对其进行开发工作。只有项目管理者能对其进行较高权限的操作。

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_2-开发者仓库-本地仓库)（2）开发者仓库(本地仓库)

任何开发者都不会对源仓库进行直接的操作，源仓库建立以后，每个开发者需要做的事情就是把源仓库的“复制”一份，作为自己日常开发的仓库。这个复制是gitlab上面的`fork`。

每个开发者所fork的仓库是完全独立的，互不干扰，甚至与源仓库都无关。每个开发者仓库相当于一个源仓库实体的影像，开发者在这个影像中进行编码，提交到自己的仓库中，这样就可以轻易地实现团队成员之间的并行开发工作。而开发工作完成以后，开发者可以向源仓库发送pull request，请求管理员把自己的代码合并到源仓库中，这样就实现了**分布式开发工作**和**集中式的管理**。

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1、分支划分-branch)1、分支划分（Branch）

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1-master-branch-主分支)（1）master branch：主分支

**master**：主分支从项目一开始便存在，它用于存放经过测试，已经完全稳定代码；在项目开发以后的任何时刻当中，`master`存放的代码应该是可作为产品供用户使用的代码。所以，应该随时保持`master`仓库代码的清洁和稳定，确保入库之前是通过完全测试和代码reivew的。master分支是所有分支中最不活跃的，大概每个月或每两个月更新一次，每一次master更新的时候都应该用`git`打上`tag`，来说明产品有新版本发布。

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_2-develop-branch-开发分支)（2）develop branch：开发分支

**develop**：开发分支，一开始从`master`分支中分离出来，用于开发者存放基本稳定代码。每个开发者的仓库相当于源仓库的一个镜像，每个开发者自己的仓库上也有master和develop。开发者把功能做好以后，是存放到自己的develop中，当测试完以后，可以向管理者发起一个pull request，请求把自己仓库的develop分支合并到源仓库的develop中。所有开发者开发好的功能会在源仓库的develop分支中进行汇总，当develop中的代码经过不断的测试，已经逐渐趋于稳定了，接近产品目标了。这时候，就可以把`develop`分支合并到`master`分支中，发布一个新版本。

注:任何人不应该向`master`直接进行无意义的合并、提交操作。正常情况下，`master`只应该接受`develop`的合并，也就是说，`master`所有代码更新应该源于合并`develop`的代码。

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_3-feature-branch-功能分支)（3）feature branch：功能分支

**feature**：功能性分支，是用于开发项目的功能的分支，是开发者主要战斗阵地。开发者在本地仓库从`develop`分支分出功能分支，在该分支上进行功能的开发，开发完成以后再合并到`develop`分支上，这时候功能性分支已经完成任务，可以删除。功能性分支的命名一般为`feature-*`，|*为需要开发的功能的名称。

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_4-协议选择)（4）协议选择

![image-20210316154759831](https://www.ydlclass.com/doc21xnv/assets/image-20210316154759831.f5926af7.png)

> http好还是ssh好

- git可以使用四种主要的协议来传输资料: 本地协议（Local），HTTP 协议，SSH（Secure Shell）协议及 git 协议。其中，本地协议由于目前大都是进行远程开发和共享代码所以一般不常用，而git协议由于缺乏授权机制且较难架设所以也不常用。
- 最常用的便是SSH和HTTP(S)协议。git关联远程仓库可以使用http协议或者ssh协议。

> HTTPS优缺点

- 优点1: 相比 SSH 协议，可以使用用户名／密码授权是一个很大的优势，这样用户就不必须在使用 Git 之前先在本地生成 SSH 密钥对再把公钥上传到服务器。 对非资深的使用者，或者系统上缺少 SSH 相关程序的使用者，HTTP 协议的可用性是主要的优势。 与 SSH 协议类似，HTTP 协议也非常快和高效
- 优点2: 企业防火墙一般会打开 80 和 443 这两个常见的http和https协议的端口，使用http和https的协议在架设了防火墙的企业里面就可以绕过安全限制正常使用git，非常方便
- 缺点: 使用http/https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令. 但是现在操作系统或者其他git工具都提供了 `keychain` 的功能，可以把你的账户密码记录在系统里，例如OSX 的 Keychain 或者 Windows 的凭证管理器。所以也只需要输一次密码就搞定了。

> SSH的优缺点

- 优点1: 架设 Git 服务器时常用 SSH 协议作为传输协议。 因为大多数环境下已经支持通过 SSH 访问 —— 即时没有也比较很容易架设。 SSH 协议也是一个验证授权的网络协议；并且，因为其普遍性，架设和使用都很容易。
- 缺点1: SSH服务端一般使用22端口，企业防火墙可能没有打开这个端口。
- 缺点2: SSH 协议的缺点在于你不能通过他实现匿名访问。 即便只要读取数据，使用者也要有通过 SSH 访问你的主机的权限，这使得 SSH 协议不利于开源的项目。 如果你只在公司网络使用，SSH 协议可能是你唯一要用到的协议。 如果你要同时提供匿名只读访问和 SSH 协议，那么你除了为自己推送架设 SSH 服务以外，还得架设一个可以让其他人访问的服务。

> 总结

HTTPS利于匿名访问，适合开源项目可以方便被别人克隆和读取(但他没有push权限)；毕竟为了克隆别人一个仓库学习一下你就要生成个ssh-key折腾一番还是比较麻烦，所以github除了支持ssh协议必然提供了https协议的支持。

而SSH协议使用公钥认证比较适合内部项目。 当然了现在的代码管理平台例如github、gitliab，两种协议都是支持的，基本上看自己喜好和需求来选择就可以了。

> 生成/添加SSH公钥

[SSH Keyopen in new window](https://gitee.com/help/labels/19) [SSH 公钥open in new window](https://gitee.com/help/labels/29)

Gitee 提供了基于SSH协议的Git服务，在使用SSH协议访问仓库仓库之前，需要先配置好账户/仓库的SSH公钥。

你可以按如下命令来生成 sshkey:

```text
ssh-keygen -t rsa -C "510180298@qq.com"  
# Generating public/private rsa key pair...
```

> 注意：这里的 `xxxxx@xxxxx.com` 只是生成的 sshkey 的名称，并不约束或要求具体命名为某个邮箱。 现网的大部分教程均讲解的使用邮箱生成，其一开始的初衷仅仅是为了便于辨识所以使用了邮箱。

按照提示完成三次回车，即可生成 ssh key。通过查看 `~/.ssh/id_rsa.pub` 文件内容，获取到你的 public key

```text
cat ~/.ssh/id_rsa.pub
# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6eNtGpNGwstc....
```

![SSH生成](https://www.ydlclass.com/doc21xnv/assets/170141_5aa5bc98_551147.5983bb6e.png)

复制生成后的 ssh key，通过仓库主页 **「管理」->「部署公钥管理」->「添加部署公钥」** ，添加生成的 public key 添加到仓库中。

![添加部署公钥](https://www.ydlclass.com/doc21xnv/assets/233212_29a62378_551147.9d04d8e4.png)

添加后，在终端（Terminal）中输入

```bash
ssh -T git@gitee.com
```

首次使用需要确认并添加主机到本机SSH可信列表。若返回 `Hi XXX! You've successfully authenticated, but Gitee.com does not provide shell access.` 内容，则证明添加成功。

![SSH添加提示](https://www.ydlclass.com/doc21xnv/assets/170837_4c5ef029_551147.20915a65.png)

添加成功后，就可以使用SSH协议对仓库进行操作了。

```bash
$ ssh -T git@gitee.com
Hi Alm张楠 (DeployKey)! You've successfully authenticated, but GITEE.COM does not provide shell access.
Note: Perhaps the current use is DeployKey.
Note: DeployKey only supports pull/fetch operations
```

> 仓库公钥和可部署公钥

为了便于用户在多个项目仓库下使用一套公钥，免于重复部署和管理的繁琐，Gitee 推出了「可部署公钥」功能，支持在一个仓库空间下使用当前账户名下/参与的另一个仓库空间的部署公钥，实现公钥共用。

部署公钥允许以只读的方式访问仓库，主要用于仓库在生产服务器的部署上，免去HTTP方式每次操作都要输入密码和普通SSH方式担心不小心修改仓库代码的麻烦。

部署公钥配置后的机器，只支持clone与pull等只读操作。如果您想要对仓库进行写操作，请 [添加个人公钥open in new window](https://gitee.com/profile/sshkeys)

部署公钥允许以只读的方式访问仓库，主要用于仓库在生产服务器的部署上，免去HTTP方式每次操作都要输入密码和普通SSH方式担心不小心修改仓库代码的麻烦。

部署公钥配置后的机器，只支持clone与pull等只读操作。如果您想要对仓库进行写操作，请 [添加个人公钥open in new window](https://gitee.com/profile/sshkeys)

个人公钥的添加地址：

![image-20210316173317900](https://www.ydlclass.com/doc21xnv/assets/image-20210316173317900.2bdba5d1.png)

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_3、实战)3、实战

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_1-多人合作开发流程)（1）多人合作开发流程

![image-20210318143710841](https://www.ydlclass.com/doc21xnv/assets/image-20210318143710841.2c291230.png)

1、首先项目经理初始化仓库建立好分支。一般会建立两个，一个master分支，一个develop分支。当然，也可能建立一个预发布版本的分支用于测试不如 realse分支。

2、对个分支设置保护行为。

3、添加项目成员。

> 小张的开发

1、将项目克隆到本地。

2、切换至开发分支

3、在开发分支上新建一个单独的功能分支，进行开发。

4、开发完成，合并到开发分支，如果功能分支没用了，可以删除。

5、先拉取新代码（git pull）,其实就是合并，发生冲突，解决冲突。

6、解决完冲突，将代码推送至代码托管平台。

> 1、新建仓库

![image-20210316161344242](https://www.ydlclass.com/doc21xnv/assets/image-20210316161344242.99345513.png)

> 选择项目分支保护

![image-20210316161514694](https://www.ydlclass.com/doc21xnv/assets/image-20210316161514694.f908097f.png)

> 添加开发者（如果你的仓库是公共的，直接搜也行）

![image-20210316162346682](https://www.ydlclass.com/doc21xnv/assets/image-20210316162346682.01561211.png)

> 此时开发者的账户汇出现该仓库

> 将项目克隆到本地，进行开发

> 开发完成推送至远程仓库

```bash
>>> git clone 仓库地址
>>> git checkout develop
# 切换到`develop`分支
>>> git checkout -b feature-discuss
# 分出一个功能性分支
>>> touch discuss.java
# 假装discuss.java就是我们要开发的功能
>>> git add .
>>> git commit -m 'finish discuss feature'
# 提交更改,多次测试以后
>>> git checkout develop
# 回到develop分支
>>> git merge feature-discuss
# 把做好的功能合并到develop中
>>> git branch -d feature-discuss
# 删除功能性分支
>>> git push origin develop
# 把develop提交到远程仓库中
```

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_2-跨团队合作开发)（2）跨团队合作开发

![image-20210318145518474](https://www.ydlclass.com/doc21xnv/assets/image-20210318145518474.6cce7562.png)

项目组成员的开发保持不变。

> 跨团队成员的合作方式

1、将代码fork到自己的仓库，同样可以进行相关的配置。

2、项目克隆到本地。

3、可以担任开发也可以多人开发。

4、开发完成后合并到自己的仓库

5、发起pull request请求给源仓库管理员

6、源仓库管理员进行code review（重新检查代码，审核代码），测试审核，通过则进行合并。

> 源仓库的构建

这一步通常由项目发起人(项目管理员)来操作，源仓库为op/Chanjet_Asset_Management,并初始化两个分支master和develop.

> 1、新建仓库

![image-20210316161344242](https://www.ydlclass.com/doc21xnv/assets/image-20210316161344242.99345513.png)

> 选择项目分支保护

![image-20210316161514694](https://www.ydlclass.com/doc21xnv/assets/image-20210316161514694.f908097f.png)

> 开发者fork仓库到自己的账户下，作为自己开发所用的仓库。

![image-20210316162529369](https://www.ydlclass.com/doc21xnv/assets/image-20210316162529369.89eebb82.png)

> 把自己开发者仓库clone到本地

```bash
>>> git clone https://gitee.com/zhao-dengkai/git-study.git
```

> 修改内容，并提交，这里直接在码云上修改

![image-20210316162711300](https://www.ydlclass.com/doc21xnv/assets/image-20210316162711300.794ee557.png)

> 构建功能分支进行开发

假设现在要开发一个“讨论”功能：

```bash
>>> git checkout develop
# 切换到`develop`分支
>>> git checkout -b feature-discuss
# 分出一个功能性分支
>>> touch discuss.java
# 假装discuss.java就是我们要开发的功能
>>> git add .
>>> git commit -m 'finish discuss feature'
# 提交更改,多次测试以后
>>> git checkout develop
# 回到develop分支
>>> git merge feature-discuss
# 把做好的功能合并到develop中
>>> git branch -d feature-discuss
# 删除功能性分支
>>> git push origin develop
# 把develop提交到自己的远程仓库中
```

此时，上自己gitlab的项目主页中`develop`分支中查看，已经有`discuss.java`这个文件了：

> 向项目经理提交pull request

> 提交pullrequest请求页面

![image-20210316162936698](https://www.ydlclass.com/doc21xnv/assets/image-20210316162936698.574f16d7.png)

![image-20210316162907857](https://www.ydlclass.com/doc21xnv/assets/image-20210316162907857.f8f42c87.png)

在完成了“讨论”功能（当然，也可能对自己的`develop`进行了多次合并，完成了多个功能）,经过测试以后，觉得没问题，就可以请求管理员把**自己仓库的**`develop`**分支**合并到**源仓库的**`develop`分支中。

> 管理员测试、合并

管理员登陆`gitlab`，看到了开发者对源仓库发起的`pull request``。`

管理员需要做的事情就是：对开发者的代码进行**`**review`。

在他的本地测试新建一个测试分支，测试开发者的代码：

```bash
>>> git checkout develop
# 进入管理员本地的develop分支
>>> git checkout -b manager-develop
# 从develop分支中分出一个叫manager-develop的测试分支测试开发者的代码
>>> git pull
http://gitlab.rd.chanjet.com/op/Chanjet_Asset_Management.git develop
# 把开发者的代码pull到测试分支中，进行测试
```

**判断是否同意合并到源仓库的**`develop`**中**，如果经过测试没问题，可以把开发者的代码合并到源仓库的`develop`中：

> 审核通过

![image-20210316163025733](https://www.ydlclass.com/doc21xnv/assets/image-20210316163025733.a57acff5.png)

> 接受请求

![image-20210316163045014](https://www.ydlclass.com/doc21xnv/assets/image-20210316163045014.dfa44fbe.png)

![image-20210316163136823](https://www.ydlclass.com/doc21xnv/assets/image-20210316163136823.925f94dd.png)

> 总结：

1、自己先fork代码到自己账户

2、拉倒本地，写代码

3、推到远程仓库

4、提交issue

5、管理员测试，review后统一，就合并了

6、如发生冲突，解决冲突即可

#### [#](https://www.ydlclass.com/doc21xnv/basics/git/#_4-重点)（4）重点

- 不要随便动别人的代码，即使要动也要商量！
- 不要随便动别人的代码，即使要动也要商量！
- 不要随便动别人的代码，即使要动也要商量！
- 记住一点，写代码和提交之前先拉去最新的代码！必须记住！
- 记住一点，写代和提交之码前先拉去最新的代码！必须记住！
- 记住一点，写代和提交之码前先拉去最新的代码！必须记住！

能够很大程度的避免冲突！

## [#](https://www.ydlclass.com/doc21xnv/basics/git/#第五章-idea使用git)第五章 idea使用git

> 配置

![image-20210316223514317](https://www.ydlclass.com/doc21xnv/assets/image-20210316223514317.e5ebf480.png)

可以下载码云插件 gitee

> 从远程仓库拉项目

![image-20210316223943198](https://www.ydlclass.com/doc21xnv/assets/image-20210316223943198.e938cc5b.png)

![image-20210316224009006](https://www.ydlclass.com/doc21xnv/assets/image-20210316224009006.380c58a5.png)

> 控制台查看分支提交等信息

![image-20210316223754144](https://www.ydlclass.com/doc21xnv/assets/image-20210316223754144.d07d1ad2.png)

> 提交代码

![image-20210316223853465](https://www.ydlclass.com/doc21xnv/assets/image-20210316223853465.8dca464a.png)

![image-20210316223911336](https://www.ydlclass.com/doc21xnv/assets/image-20210316223911336.c8762fc5.png)