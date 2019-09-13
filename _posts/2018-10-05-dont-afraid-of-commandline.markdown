---
layout:	post
title:	"不要害怕命令行：一些个人的思考和理解(not finished)"
date:	2018-10-05 15:32:00 +0800
categories: [Experience]
---
### 以下内容仅个人观点，不保证完全正确，甚至可能有误导性
感觉很多人（包括编程的人）似乎对命令行和CLI都有一种恐惧，但了解命令行对于了解计算机，甚至是更好地写和调试代码都是很重要的。希望本文能帮助大家克服这种恐惧，打开终端迎接新世界。  
# 关于命令行
命令行(Commandline)，或者说命令提示符(Command Prompt)，有时候叫终端(Terminal)，是操作系统中进行命令输入并与系统进行直接交互的工具。Windows上的命令行程序是cmd.exe，Linux（也是一个优秀的操作系统，常用于服务器）和macOS里都有终端程序。安卓上其实也有命令行，只是系统本身不想让你用，必须装第三方软件来使用。  
特点不必多说，通常是暗色的背景，没有图形界面，只有几行文字和一个闪烁的光标，打一行字，敲一个回车，输出几行字。看起来很不友好，很难使用，甚至不像是一个优美的操作系统里应该有的东西。  
的确，对与很多人来说，这东西几乎用不上而又令人害怕。叫人“打开终端，输入xxxxxx”，多半会使人皱起眉头。  
放几张图：  
典型的Windows7命令提示符  
![Win7 cmd](/images/wincmd.png)

# 为什么让人恐惧
其实，让人恐惧的可能并不是命令行本身。以下几点都可能使第一次打开命令行的人感到恐惧并不愿接触：  
1. 没有活动空间  
除了一个闪动的光标和几个让人不想看的字，一个打开的终端实在没给你什么提示告诉你该干什么。如果打开一个如下的GUI（图形界面）软件：  
![Origin](/images/origin_overview.png)
你也会不知到该干什么（假设你没有用过这个软件），但至少你可以看看周围的图标，点点菜单和按钮，软件也会给你些积极的反馈，让你感到很新奇并想要继续学习。  
但在命令行里，你什么都干不了：输入些字符，敲个回车，看到一条好像是错误信息的东西：  
```
C:\Users\petergu>whatshouldIdo???
'whatshouldIdo???' is not recognized as an internal or external command,
operable program or batch file.
```
再输入些其他的字符，还是同样的结果，很令人挫败。
1. 输出信息量大  
可能，你听说了几条指令，比如`shutdown`，想尝试一下，结果：  
```
C:\Users\petergu>shutdown
Usage: shutdown [/i | /l | /s | /r | /g | /a | /p | /h | /e] [/f]
    [/m \\computer][/t xxx][/d [p|u:]xx:yy [/c "comment"]]

    No args    Display help. This is the same as typing /?.
    /?         Display help. This is the same as not typing any options.
    /i         Display the graphical user interface (GUI).
               This must be the first option.
    /l         Log off. This cannot be used with /m or /d options.
    /s         Shutdown the computer.
    /r         Shutdown and restart the computer.
    /g         Shutdown and restart the computer. After the system is
               rebooted, restart any registered applications.
---以下省略十几行---
```
你发现，爆出了大量的文字，却好像什么也没发生，使人不知所措，难以从中获得有用的信息，就算是想要查资料或者问别人，也不知道如何下手。  
1. 输入自由度过大  
一个软件，如果只有几个按钮，总不会让人感到可怕，因为点来点去也就那么几种输入情况；而如果一下子给你一堆选项和输入框（如图，这是Servers Ultimate Pro下一个服务器的配置），你便会无从下手，即使这是安卓的软件，不是命令行。  
![SUP demo](/images/sup_demo_small.jpg)
而命令行，直接给你一个光标，让你随便输入，甚至可以输入ctrl字符和Esc，于是这情况就可想而知了。  
1. 提示不友好  
在图形界面里，各种图形就是一种提示：比如打开文件、保存、回收站、撤销、重做等等各种图标让你一看就知道是什么，这是一种（可能被忽视但确实）很重要的提示。  
试想，如果在一个Word文档中，把所有的图标换成一个个字符，该怎么用？Word还是Word，却让你望而却步。如果把所有的提示都去掉，所有的操作都是字符指令并且都要你记在脑子里（这种东西真的存在，并且可能超级好用），该是什么一种感觉？  
友好的Word  
![Word menu](/images/word_menu.PNG)
似乎不太好用的nano，但下面提示怎么编辑  
![nano](/images/nano.png)
混乱邪恶的Vim，提示？不存在的！  
![Vim raw](/images/vimraw.png)
但事实上，这三种东西哪个好用，并不是很容易就能说清楚的，但哪个令人害怕，已经看得很明显了。  
这一点Linux上可能更加明显。  
下图是我电脑（我用Linux）关掉图形界面登录后的画面，只拍了左上角因为剩下的都是空的，除了一行字`root@localhost:~#`和一个光标什么都没有，所有的操作都要（也都可以）在这下面完成，这就是一个完整的操作系统。没有提示不意味着没有功能，但这确实可能令初学者感到畏惧。
![TTY](/images/ttysmall.jpg)
1. 心理因素  
命令行在一些电影中经常以黑客等人的高级工具的身份存在，这会让人感觉这东西很高级、很困难，形成先入为主的恐惧，不愿意接触。  
命令行也会令人联想到一些如Linux、服务器、程序员等名词，使得不是专业程序员但需要写代码或被迫使用~~折腾~~电脑系统的人感到不适。

这几种困难其实并不是独立的，而是相互关联甚至好像有些矛盾（如没有活动空间却自由度过大，提示不友好却有大量输入）的。知道了为什么会恐惧，才有可能逐一克服。

# 哪里有命令行？

命令行属于与系统底层交互的工具，在几乎所有的操作系统上都是有的。Windows上有经典的命令提示符和PowerShell，Linux和Mac OS X都有自己的终端，Android（以及iOS）系统希望用户好好地使用系统而不是玩这种东西，所以没有提供命令行应用给普通用户，但如果进行app开发，都是有如adb这种工具访问手机等设备的终端的。

如果不只看操作系统的终端，那么命令行的存在就更加广泛了。比如第三方程序也提供类似命令行的交互程序。最常见的可能就是Python的交互式解释器了：

```
Python 3.7.3 (default, Jun 24 2019, 04:54:02) 
[GCC 9.1.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 

```

其他的，如lvm管理，diskpart、fdisk分区，也都大同小异。当然这只是形式上相似，具体的使用方法（比如支持什么命令）都不相同。

如这个fdisk界面，输入了指令p查看分区表。

```
Welcome to fdisk (util-linux 2.34).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk /dev/sda: xxxx GiB, xxxx bytes, xxxx sectors
Disk model: xxxx
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: xxxx

Device          Start        End    Sectors   Size Type
/dev/sda1   	(此处略去)
/dev/sda2  
/dev/sda3  
/dev/sda4  

Command (m for help): 

```

### 这说明什么？

甚至只是想老实地写写代码，都不能完全避开使用命令行。所以命令行是学习计算机、编程等的基础。如果害怕不敢接触命令行，那么必然会处处受阻。

那命令行到底能干什么呢？知道了能干什么，就有了学习它的动力。

# 命令行能干什么？

#### 系统上最强大的工具



#### 认识并了解底层




# CLI和GUI


# 如何使用？新手Guide？

#### 阅读输出的文字
#### 借助网络
#### 有需求才有动力学习

#### 学好英语

# 注意事项

#### 不忘初心

#### 注意安全



