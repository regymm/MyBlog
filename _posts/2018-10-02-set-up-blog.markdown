---
layout:	post
title:	"第一篇博客：使用Github Pages搭建博客"
date:	2018-10-02 14:44:00 +0800
categories: [Tutorial, Chinese]
---
听说Github Pages有搭建博客的功能，在这国庆节清闲之时搭了一个，感觉还不错，准备用来记录我学习和折腾的经历以及心得体会。本人能力有限，内容若有错误或不当之处还请指教，联系方式见About页面。   
# 用Github Pages搭建博客过程简述  
[Github Pages](https://pages.github.com)大概就是个在github上托管静态网页的东西，可以用来做项目的网站等等。网页可以直接自己写放上去，也可以用Jekyll生成，一般博客属于后一种。只要在github上建一个叫yourusername.github.io的repo，在里面放上网页，就直接可以在yourusername.github.io访问了。  
博客的话，简单的说，把Jekyll在本地的repo里配置好，然后推上去，剩下的事github会帮你做好。博客文章可以用html或markdown写，Jekyll可玩性还是很高的，但一些复杂的功能我应该也用不上。  
### 1.  新建github仓库
建一个(你的用户名).github.io的仓库，github会自动把它作为Github Pages的代码库。在新建的仓库中建一个index.html，写几个字进去测试一下，在(你的用户名).github.io这个域名下应该能看到你的index.html。  
如果连不上请自行解决网络问题，就算你访问一个不存在的pages网站，github也会给一个404而不会连不上。 
### 2.  安装Jekyll
以下内容均在Linux系统下。  
安装ruby和ruby-dev。一般发行版会带ruby，但ruby-dev可能不带，我的debian9上要`apt install ruby-dev`装一下。  
用gem安装jekyll。按照[官网](https://jekyllrb.com/docs/)的quickstart，`gem install jekyll bundler`，记得以root身份或者sudo。下载可能比较慢，可能需要解决网络问题，耐心等待安装完成，注意如果ruby-dev没有安装会提示没有头文件的错误。  
### 3.  Jekyll新建网站
把github仓库clone下来，进入目录，输入`jekyll new .`，等一会，一个网站就生成好了。  
看到里面有一些如404.html，about.md之类的可配置的页面，还有主配置文件\_config.yml。  
运行，用指令`bundle exec jekyll serve`，然后在http://localhost:4000就能看到内容了。  
### 4.  配置和新文章的编写
简易配置：在\_config.yml中设置站名、联系方式等信息；about.md设置about内容。  
新文章：在\_posts目录中可以看到一个示例博客，按这个规范用markdown写就好了，写好之后可以看到网页里Posts中有了你的新文章。Jekyll生成的静态网页在\_site目录下，这个目录默认以经加在.gitignore里了，不必特殊处理。  
我的目录结构： 
```
petergu@localhost:~/coding/ustcpetergu.github.io$ tree
.
├── 404.html
├── about.md
├── _config.yml
├── Gemfile
├── Gemfile.lock
├── index.html.bak
├── index.md
├── _posts
│   ├── 2018-10-01-welcome-to-jekyll.markdown
│   └── 2018-10-02-set-up-blog.markdown
├── README.md
└── _site
    ├── 2018
    │   └── 10
    │       └── 02
    │           └── set-up-blog.html
    ├── 404.html
    ├── about
    │   └── index.html
    ├── assets
    │   ├── main.css
    │   └── minima-social-icons.svg
    ├── feed.xml
    ├── index.html
    ├── index.html.bak
    ├── jekyll
    │   └── update
    │       └── 2018
    │           └── 10
    │               └── 01
    │                   └── welcome-to-jekyll.html
    └── README.md

12 directories, 20 files
```
### 5.  发布
说是发布，其实提交然后推到github上就完成了，在github.io上应该直接能看到做好的博客网站。 
### 6.  其他
也可以在github网站上直接编写markdown  
github设置里有很多主题可以选择  
可以绑定自己的域名  
[markdown](https://daringfireball.net/projects/markdown/syntax)简单易学 

### The END
第一次写博客，希望能坚持下去！
