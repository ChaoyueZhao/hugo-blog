+++
title = "Hogo建站入门"
date = 2016-04-20
lastmod = 2018-01-13T00:00:00
draft = false
tags = []
+++

Hugo是时下特别流行的静态网站生成器，据统计排名仅次于Jekyll，它使用Go语言开发而成，Hugo最大的特点就是“快”，它的官网上毫不谦虚的炫耀着：“The world's fastest framework for buliding websites"。

我是去年11月发现Hugo的，当时对Wordpress已经丧失了最初的激情，随着自己技术的增长，希望能搞一个更好玩的网站，于是开始了解Hugo。不过由于种种原因，直到今年过完元旦我才正式开始研究它，花了两天时间，踩了不少坑，终于算是初步搞定了。下面就记录下我用Hugo建站的过程。

## 一、快速开始 ##

首先，按照官网上 **[Quick Start] (http://gohugo.io/getting-started/quick-start/)** 的步骤走就行，一切正常的话，5分钟就搞定了，

Mac上，打开terminal，按照以下步骤进行：

1. 安装Hugo

    ```bash
    brew install hugo
    ```

2. 创建一个新站

    ```bash
    hugo new site hugo-blog
    ```
 
3. 增加一个主题（皮肤）

    ```bash
    cd hugo-blog;
    git init;
    git submodule add https/:github.com/budparr/gohugo-theme-ananke.git themes/ananke;
    ```

4. 修改根目录的toml文件。
  
    ```bash
    echo 'theme = "ananke"' >> config.toml
    ```

5. 创建一篇文章，比如first-post.md

    ```bash
    hugo new posts/first-post.md
    ```

6. 下来就可以搭建一个本地服务器啦，用-D来生成显示草稿（默认新文章都是草稿）
 
    ```bash
    hugo server -D
    ```

7. 访问localhost地址，这样就看到网站啦，这里有点小激动，毕竟距离成功已经不远啦。



### 小结 ###
如果顺利的话，以上步骤5分钟就可以搞定了，我当时忘记修改config.toml文件里的theme，怎么也无法生成网站。其实手动打开文件输入代码也可以，感觉用terminal反而不习惯。


***


## 二、快速部署 ##
Hugo官网推荐了9种部署方式，我使用的是在GitHub上的User Page，步骤如下：

1.  在GitHub上创建一个repo用来放Hugo的主体文件。比如：

    ```bash
    hugo-blog
    ```

2. 在GitHub上创建一个自己用户名的repo，我的是:

    ```bash
    ChaoyueZhao.github.io
    ```
    
4. 这个repo用来放Hugo生成好的网站文件，利用GitHub自带的网站功能，自动形成repo名的免费网址。

3. 用terminal，cd到之前用Hugo创建的本地文件夹里，我的是hugo-blog, 用


    ```bash
    git remote add origin <第一步的repo地址>
    ```

    这里是把GitHub上的新建的repo作为本地hugo-blog的远程仓库，注意，为了下一步省事，在第一第二步建立GitHub的repo的时候，不要自动生成README.md。
  

4. 然后把本地hugo-blog仓库push到远程仓库（第一步创建的repo），代码是:


    ```bash
    git push origin master
     ```

5. 现在看看hugo-blog根目录有没有public，如果没有的话，在terminal里输入

     ```bash
     hugo
     ```
 
    会立刻生成一个public文件夹，这个是Hugo默认的网站发布路径。然后:
   
    ```bash
    cd public
    ```

6. 这一步很关键，需要把public文件夹作为第二步repo的子模块，代码是:

    ```bash
    git submodule add <第二步repo的地址>
    ```
 
     这样的好处是把Hugo建站工具文件（hugo-blog）和生成好的网站文件（public）分开，由于hugo-blog本身就是一个git repo，所以在它内部的public文件夹必须要变成一个submodule（子模块）才能生成repo。

7. 这样每次写了新文章，确定更新了，就输入:

    ```bash
    hugo
    ```
 
    Hugo会自动生成新的public文件夹
 
    然后，先把hugo-blog文件夹同步到GitHub上，依次进行以下步骤：
  
    ```bash
    git add .
    git commit
    git push
    ```
 
    注意这里git会显示自动忽略public文件夹里的内容，因为public文件夹是一个子模块。
 
8. 接着同步public文件夹：
 
    ```bash
    cd public
    git add .
    git commit 
    git push
    ```
 
    这样就把public 推到了GitHub，然后过一小会，网站内容就出现在<usersname>.github.io这个网址了。
 
### 小结 ###
 
这部分的难点是git的熟练使用，我在这里耽误了不少时间，各种报错，想了想还是自己对git不熟悉。我的心得是，初学者一定要搞清楚本地repo和远程repo（GitHub）的交互方法，如何push，如何pull， 这些基本的东西一定要掌握。
 
此外，还需要了解submodule（子模块）的含义，我就一度把submodule和remote搞混了。