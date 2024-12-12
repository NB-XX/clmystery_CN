在命令行上处理文本
=====================================

命令行（也称为命令行界面，简称CLI，或有时候称为终端），是一个基于纯文本的界面，用于在计算机上执行命令。如果你曾经看过1980年代关于黑客的电影，如 *WarGames*，他们盯着黑屏上的提示符逐个输入命令，基本上就是这样。

你有一个提示符，你可以输入一个命令并按“Enter”键执行它。一个例子命令是：

	touch newfile.txt

这个命令将创建一个名为`newfile.txt`的文件。

如何访问命令行
------------------------------

**Mac OS X：** 前往 /Applications/Utilities 并点击“Terminal”或在 Spotlight 中搜索“Terminal”。

**Desktop Linux：** 你可以从 Dash 中搜索“Terminal”应用程序。不过老实说，如果你在使用 Linux，你可能不需要这个教程。

**Windows：** Windows 有些特殊。如果你前往开始菜单点击“运行”，然后输入“cmd”并按回车，它将打开 Windows 版的命令行。不幸的是，Windows 版的命令行有其自己的系统，所以为了跟随这些示例，你会想要安装 Cygwin，这将允许你模拟 Linux 风格的命令行：

http://www.cygwin.com/

更多细节
--------------------

命令通常采取的格式是：

	[命令名称] [选项] [选项] [选项] ...

提示符还会显示你当前所在的目录。每当你执行命令时，你都是从特定的目录中执行它。这很重要，因为当你执行涉及文件名或目录名的命令时，你可以用两种方式之一指定它们：

#### 相对路径

指定一个文件或目录为相对路径意味着你正在指定它相对于你所在目录的位置。例如，假设你在 `files` 目录的 `videos` 子目录中。你会看到这个提示符：

	/files/videos$

如果你执行像 `touch newfile.txt` 这样的命令，它将在当前目录内创建`newfile.txt`。相对路径不以斜杠开头。

#### 绝对路径

指定一个文件或目录为绝对路径意味着你正在指定它在计算机上的绝对位置，从顶层开始。例如，假设你再次位于 `files` 目录的 `videos` 子目录中。

	/files/videos$

如果你执行像 `touch /files/music/newfile.txt` 这样的命令，它将在不同的文件夹中创建`newfile.txt`，即 `files` 文件夹的 `music` 子文件夹。*绝对路径以斜杠开头。*

如果你使用绝对路径，无论你从哪个目录执行命令，命令都会做同样的事情。

所以这两个命令在 `/files/videos` 目录下将有相同的结果：

	/files/videos$ rm video.mp4
	（这将从当前目录删除文件`video.mp4`）

	/files/videos$ rm /files/videos/video.mp4
	（这将从/files/videos/目录删除`video.mp4`，恰好是当前目录）

如果你在不同的目录中，相同的两个命令将不会有相同的结果：

	/files/text$ rm video.mp4
	（这将尝试从'text'子目录中删除文件video.mp4，因为那是当前目录）

	/files/text$ rm /files/videos/video.mp4
	（这将从/files/videos/目录删除文件，即使它不是当前目录）

记住：

**以斜杠开头的路径** 表示你想要给出完整的路径并忽略你当前所在的目录。
**不以斜杠开头的路径** 表示你想要从你所在的目录开始给出路径。

如果你不确定自己处于什么目录，你可以使用 `pwd`（打印工作目录）命令来获取当前目录的绝对路径。

	~$ pwd
	/Users/Noah

文件模式
-------------

在大多数情况下，当你需要指定文件名或目录名时，你也可以指定一个通用的**模式**，它可能匹配多个文件。这方面有许多细节，但最基本的版本是使用星号(*)，它匹配任何内容。它也被称为通配符。

	删除当前目录中的任何文件
	/files$ rm *

	删除任何以'.txt'结尾的文件
	/files$ rm *.txt

	删除任何以'data'开头的文件
	/files$ rm data*

导航
----------

用于导航提示符所在目录的两个核心命令是 `cd` 和 `ls`。

`cd` 是一个更改当前目录的命令，必须跟一个你想要切换到的目录。你可以提供一个绝对或相对路径。

	这将把你带到 /files/videos
	/files$ cd videos	
	/files/videos$

	这将把你带到 /videos，然后是 vines 子目录
	/files$ cd /videos
	/videos$ cd vines
	/videos/vines$

如果你想要，你可以一次跳转多个级别。

	这将把你带到 /files/videos/short
	/files$ cd videos/short

你可以使用 `cd ..` 移动到父目录的上一级。

	这将把你带到 /files
	/files/videos$ cd ..

`ls` 将列出当前目录中的文件。它有助于弄清楚你在哪里，存在什么文件和什么子文件夹。

	/photos$ ls	
	thumbnails  photo1.jpg  photo2.jpg

使用 `ls -l` 将以垂直方式打印列表，还有关于文件大小、权限和最后修改日期的许多其他额外信息：

	/photos$ ls -l
	-rw-rw-r-- 1 noah noah 58133 Oct 22 17:13 photo1.jpg
	-rw-rw-r-- 1 noah noah 75640 Oct 22 17:13 photo2.jpg
	drwxrwxr-x 2 noah noah 4096  Oct 22 17:13 thumbnails

当输入一个目录或文件名时，你可以按“Tab”键进行自动补全（如果可能的话）。例如，在 /photos 文件夹中，如果你输入：

	/photos$ cd thu

并按“Tab”，它将填写剩余部分并显示：

	/photos$ cd thumbnails

然而，如果有多个可能的文件/目录与你到目前为止所输入的匹配，它不会起作用。如果你输入：

	/photos$ rm pho

并按“Tab”，因为你可能要删除 `photo1.jpg` 或 `photo2.jpg`，所以什么也不会发生。

命令输出
--------------

我们将要讨论的命令都将以文本形式输出其结果。当你通过按“Enter”键执行命令时，它会在提示符下方的额外行上打印出一堆输出。例如，`head [file]` 将打印出一个文件的前10行。

	/files$ head names.txt
	Dan Sinker
	Erika Owens
	Noah Veltman
	Annabel Church
	Friedrich Lindenberg
	Sonya Song
	Mike Tigas
	Brian Abelson
	Manuel Aristaran
	Stijn Debrouwere
	/files$

注意，在它打印出输出后，它会回到给你一个新的提示符。如果你只是随便看看，这种方式得到输出是有用的，但通常你想要做两件事之一：**将输出发送到文件**，或**将输出作为输入发送到另一个命令**。

### 将输出发送到文件

你可以这样将输出发送到新文件：

	/files$ head names.txt > first10names.txt

如果 first10names.txt 不存在，它将被创建。如果它已经存在，它将被覆盖。

你可以这样将输出附加到现有文件的末尾：

	/files$ head names.txt >> allnames.txt

这将在 allnames.txt 的末尾添加10行新输出。

### 将输出作为输入发送到另一个命令

你可以使用管道符号(|)将输出发送到另一个命令。`grep` 命令搜索某些文本中的匹配项（稍后详细介绍），所以你可以这样做来获取文件的前10行，然后搜索这10行中的"Steve"：

	/files$ head names.txt | grep "Steve"

这基本上与这样做相同：

	/files$ head names.txt > temporaryfile.txt
	/files$ grep "Steve" temporaryfile.txt

但是，你可以直接从第一个命令中将输出管道到第二个命令，而不是先将输出发送到文件然后再对该文件运行第二个命令。你可以将许多这样的命令串在一起：

	/files$ grep "United States" addresses.csv | grep "California" | head

这将搜索文件 addresses.csv 中包含短语 "United States" 的行，然后搜索结果中包含单词 "California" 的行，然后打印出这些匹配项的前10行。

Grep
----

`grep` 命令将让你搜索一个文件（或多个文件）中的短语。默认情况下，它将打印出每个匹配你搜索的行。

打印出包含单词 "darkwing" 的行：

	/files$ grep "darkwing" famousducks.txt

与上述相同，但搜索不区分大小写：

	/files$ grep -i "darkwing" famousducks.txt

在文件中查找单词 "Donald" 的精确匹配 - 包含 "Donald" 的词，如 "McDonald"，不计算在内：

	grep -w "Donald" famousducks.txt

在当前目录的每个文件中查找 "McDuck" 的匹配：

	grep "McDuck" *

在当前目录及其所有子目录中查找 "McDuck" 的匹配，一直向下搜索：

	grep -r "McDuck" *

对于每个 "Howard" 的匹配，打印出该行及其后的4行（共5行）：

	grep -A 4 "Howard" famousducks.txt

对于每个 "Howard" 的匹配，打印出该行及其前的4行（共5行）：

	grep -B 4 "Howard" famousducks.txt

对于每个 "Howard" 的匹配，打印出该行及其前后各4行（共9行）：

	grep -C 4 "Howard" famousducks.txt

不打印出匹配的行，而是打印出匹配搜索的文件名：

	grep -l "Daffy" *

仅获取匹配的数量：

	grep -c "Daffy" *

在匹配行旁边显示行号：

	grep -n "Daffy" famousducks.txt

Cat
---

`cat` 命令将合并多个文件。这将按顺序打印三个文件，就像它们是一个文件一样：

	cat turkey.txt duck.txt chicken.txt

记住，这只会将输出打印到你的终端。更有可能的是，你想要创建一个合并它们的新文件：

	cat turkey.txt duck.txt chicken.txt > turducken.txt

turducken.txt 将包含 turkey.txt 中的所有行，然后是 duck.txt 中的所有行，然后是 chicken.txt 中的所有行。

如果你想要合并目录中的所有文件，你可以使用通配符：

	cat * > allfilescombined.txt

Head
----

`head` 命令将打印出一个文件的前10行：

	/files$ head names.txt

你也可以指定不同的行数。这将打印出一个文件的前15行：

	/files$ head -n 15 names.txt

或者，如果你想打印整个文件但留下最后15行，则可以给出一个负数：

	/files$ head -n -15 names.txt

head 的一个好用法是快速查看大文本文件中的内容，而不必等待文本编辑器加载它。当你谈到一个1 GB的文件时，这就成了一件大事！

Tail
----

`tail` 命令与 head 相反。它将打印出一个文件的最后10行：

	/files$ tail names.txt

这将打印出一个文件的最后15行：

	/files$ tail -n 15 names.txt

或者，如果你想打印整个文件但留下前15行，则可以加上一个加号：

	/files$ tail -n +16 names.txt

这在你想要，比如说，从 CSV 文件中移除一个标题行时很有帮助：

	/files$ tail -n +2 names.txt > names-no-header.txt

杂项
-------------

如果你只想将一个文件的整个内容打印到你的终端，你可以使用 `cat` 并且不将其与任何东西结合使用。这有点违背 `cat` 的整个用途，但是是一个方便的技巧。

	/files$ cat address.txt
	1600 Pennsylvania Avenue
	Washington, DC 20500

如果你想要认真地在内置于你的终端的文本编辑器中打开一个文件，你可以尝试 `nano`：

	/files$ nano address.txt

names.txt 中有多少行？

	/files$ wc -l names.txt
	18

正则表达式
-------------------

当使用像 `grep` 这样的东西进行搜索时，你可以搜索一个简单的带有字母、数字和空格的术语。但是如果你想要搜索一个模式，你可以使用所谓的**正则表达式**。正则表达式使用特殊字符来表示模式，比如“任何数字”、“任何字母”、“X或Y”、“至少三个小写字母”等等。

我们现在不需要担心这些细节，但一个有用的操作符是句点(.)。在正则表达式中，这意味着“任何一个字符”。因此，你可以搜索像这样的东西：

	/files$ grep -i "car.s" dictionary.txt

这将匹配像 `cards`,`carts`,`cares` 等词。它也会匹配短语 "scar story" 中的部分 (CAR S)，因为“任何字符”意味着任何字符，包括空格或标点符号。

再举一个例子：

	/files$ grep -i ".e.st" dictionary.txt

这将匹配像 `least`,`beast` 和 `heist` 这样的词。

有多种方法可以做同一件事
------------------------------

通常有许多同样合理的命令或命令组合来达到同样的目的。

例子：

	/files$ head -n 12 names.txt | tail -n 5
	（打印出前12行，然后打印出那之中的最后5行）

	与

	/files$ tail -n +8 names.txt | head -n 5
	（打印出从第8行开始的所有内容，然后打印出那之中的前5行）

	几乎相同：

	/files$ tail -n +8 names.txt > temporaryfile.txt
	/files$ head -n 5 temporaryfile.txt
	/files$ rm temporaryfile.txt
	（将从第8行开始的所有内容保存到一个临时文件中，然后打印出那之中的前5行，然后删除临时文件）

---

## 问题/评论/建议 ##
Noah Veltman  
网站: http://noahveltman.com  
推特: [@veltman](http://twitter.com/veltman)  
电邮: [noah@noahveltman.com](mailto:noah@noahveltman.com)  
