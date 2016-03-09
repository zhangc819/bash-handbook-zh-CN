# bash-handbook-zh-CN

谨以此文档献给那些想学习Bash又无需钻研太深的人。

> **Tip**: 不妨尝试 [**learnyoubash**](https://git.io/learnyoubash) — 一个基于本文档的交互式学习平台！

# TODO Node

# TODO 目录

# 前言

如果你是一个程序员，时间的价值想必心中有数。持续优化工作流是你最重要的工作之一。

在通往高效和高生产力的路上，我们经常不得不做一些一遍又一遍重复的劳动，比如：

* 对屏幕截图，并把截图上传到服务器上
* 处理各种各种的文本
* 在不同格式之间转换文件
* 格式化一个程序的输出

就让**Bash**来拯救我们吧。

Bash是一个Unix Shell，作为[Bourne shell](https://en.wikipedia.org/wiki/Bourne_shell)的free software替代品，由[Brian Fox][]为GNU项目编写。它发布于1989年，在很长一段时间，Linux系统和OS X系统都把Bash作为默认的shell。

[Brian Fox]: https://en.wikipedia.org/wiki/Brian_Fox_(computer_programmer)
<!-- 一些MD处理器不能很好的处理URL里面的 '()'，因此这个链接采用这种格式 -->

那么，我们学习这个有着30多年历史的东西意义何在呢？答案很简单：这个所谓的 _东西_，是当今最强大、可移植性最好的，为所有基于Unix的系统编写高效率脚本的工具之一。这就是你需要学习bash的原因。句号。

在本文中，我会用例子来描述在bash中最重要的思想。希望这篇概略性的文章对你有帮助。

# Shells与模式

bash shell有交互和非交互两种模式。

## 交互模式

Ubuntu用户都知道，在Ubuntu中有7个虚拟终端。
桌面环境接管了第7个虚拟终端，于是按下`Ctrl-Alt-F7`，便可以去到一个操作友好的图形用户界面。

即便如此，还是可以通过`Ctrl-Alt-F1`，来打开shell。打开后，熟悉的图形用户界面消失了，一个虚拟终端展现出来。

看到形如下面的东西，说明shell处于交互模式下：

    user@host:~$

接着，便可以输入一系列Unix命令，比如`ls`，`grep`，`cd`，`mkdir`，`rm`，来看它们的执行结果。

之所以把这种模式叫做交互式，是因为shell直接与用户进行交互。

直接使用虚拟终端其实并不是很方便。设想一下，当你想编辑一个文档，与此同时又想执行另一个命令，这种情况下，下面的虚拟终端模拟器可能更适合(**TODO**)：

- [GNOME Terminal](https://en.wikipedia.org/wiki/GNOME_Terminal)
- [Terminator](https://en.wikipedia.org/wiki/Terminator_(terminal_emulator))
- [iTerm2](https://en.wikipedia.org/wiki/ITerm2)
- [ConEmu](https://en.wikipedia.org/wiki/ConEmu)

## 非交互模式

在非交互模式下，shell从文件或者管道中读取命令并执行。当shell解释器执行完文件中的最后一个命令，进程终止，回到父进程(**TODO**)。

可以使用下面的命令让shell以非交互模式运行：

    sh /path/to/script.sh
    bash /path/to/script.sh

上面的例子中，`script.sh`是一个包含shell解释器可以识别并执行的命令的普通文本文件，`sh`和`bash`是shell解释器程序。你可以使用任何喜欢的编辑器创建`script.sh`（vim，nano，Sublime Text, Atom等等）。

除此之外，你还可以通过`chmod`命令给文件添加可执行的权限，来直接执行脚本文件：

    chmod +x /path/to/script.sh

这种方式要求脚本文件的第一行必须指明运行该脚本的程序，比如：

```bash
#!/bin/bash
echo "Hello, world!"
```
如果你更喜欢用`sh`，把`#!/bin/bash`改成`#!/bin/sh`即可。`#!`被称作[shebang](https://zh.wikipedia.org/wiki/Shebang)。之后，就能这样来运行脚本了：

    /path/to/script.sh

上面的例子中，我们使用了`echo`来输出字符串到屏幕上。

我们还可以这样来使用shebang：

```bash
#!/usr/bin/env bash
echo "Hello, world!"
```

这样做的好处是，系统会自动在`PATH`环境变量中查找你指定的程序（本例中的`bash`）。相比第一种写法，你应该尽量用这种写法，因为程序的路径是不确定的。这样写还有一个好处，操作系统的`PATH`变量有可能被配置为指向程序的另一个版本。比如，安装完新版本的`bash`，我们可能将其路径添加到`PATH`中，来“隐藏”老版本。如果直接用`#!/bin/bash`，那么系统会选择老版本的`bash`来执行脚本，如果用`#!/usr/bin/env bash`，则会使用新版本。
