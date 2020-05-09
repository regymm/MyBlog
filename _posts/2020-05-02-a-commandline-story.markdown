---
layout:	post
title:	"命令行故事"
date:	2020-05-02 23:00:00 +0800
categories: [Experience]
---
> 或许当你成为熟练的命令行用户之后，却发现自己为其所困，在使用命令行时从“提高生产力和快乐”变成了“因为种种原因生产力降低、失去快乐”：至少我有时会这样。或许...

> 故事80%以上为虚构。

你早已习惯了使用终端，你在你的Linux主机和WSL命令行中如履平地。有一天，你终于闲下来，打开好几个月没碰的博客。你娴熟地打开终端，`bundle exec jekyll serve`。可是一堆乱七八糟的报错之后你的博客服务并没有启动。唉，这个问题也不是第一次出现了。你厌恶地看了一眼报错，感觉这个根本没必要上网查啊，`bundle update`, 噫，卡住了, `export http_proxy=http://xxxxxx;https_proxy=$http_proxy`, 还是不行？`export http_proxy=socks5://xxxx https_proxy=socks5://xxxx`, 这回好了, `gem install jekyll`, 你不会ruby，但这无所谓，似乎只是版本旧了, `apt upgrade`, 哦，不在学校，换个离家近点的源吧，`sudo vim /etc/apt/sources.list`, 又忘了这个配置怎么改了，上镜像站看一眼。一顿熟悉的操作后不灵，于是被迫找上Arch Wiki，哦，它的方法好像和我的不太一样，没关系，老司机不care这些的。于是你混用gem和系统包管理器。你遇到了文件冲突，强制移除了`/usr/bin`中某些文件，可是冲突太多了，你只能 `--overwrite`。你有些心虚，但安装总算是过了。哦，要干啥来着？博客。再次`jekyll serve`, 不灵，看错误好像是包没装上？明明装了啊，继续查吧，askubuntu，linuxquestions，superuser，你不等网页加载完就不耐烦的关掉了页面：我只是想写个博客啊！哦，想起来了，`jekyll serve`前头要加`bundle exec`的，这里不能偷懒，你一年前在另一个操作系统里就碰到过。。。哦，输出变了：怎么看着有点熟悉。。。好吧，是又回到原点了。

你喝了一大口水后打开Google，搜linux install jekyll。哦，还是Arch Wiki。这次你决定跟着它来。`gem update`, `gem install jekyll`, `bundle update --bundler`。 一片绿色的输出稍稍缓解了你焦虑的心情。你用麻木的手再次敲下`bundle exec jekyll serve`，哦，一次就过了，可是你的心中并没有喜悦。呼，结束了。你看着浏览器里的打开的博客预览这样想。

你干涩的眼睛提醒你现在已经是睡觉时间，可是。。。你很不甘心地打开了一篇没写完的博客，回顾了一下内容，字写了五行又删了三行。明天再写吧。你扣上电脑，草草洗漱就睡了。这20分钟里，你压根没有看浏览器里的预览一眼。

第二天，你看都没看屏幕上一片狼藉的博客、终端和浏览器窗口，就熟练地切换到一个新的工作区，干别的去了。直到5天之后重启电脑，你也没有回到那个博客的工作区一次。

三个月后，你再次想起了那篇没写完的博客。你娴熟地打开终端，`bundle exec jekyll serve`。

