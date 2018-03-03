---
title: hexo+github搭建属于自己的在线博客
date: 2018-03-01 00:13:43
tags: hexo
---


## 简介 ##
hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
<!-- more -->
## 总览步骤： ##


 1. 安装NodeJs软件，用于使用npm工具；
 2. 安装Git软件，用于与Github连接；
 3. 安装hexo博客框架，使用NodeJs的npm工具；
 4. 部署本地hexo博客以及博文编辑；
 5. 部署基于github的在线博客；
 6. 优化部署与管理；
 7. 附录。

## 一、安装NodeJs ##

进入[nodejs中文网][1]进行下载安装，速度还是可以的，安装过程一路下一步即可，这里不再赘述。


## 二、安装Git ##

在Windows上使用Git，可以从[Git官网][2]直接下载安装程序，（网速慢的同学请移步[国内镜像][3]），也是一路下一步即可。

## 三、安装Hexo ##

当然，我们可以使用nodejs自带的npm命令进行下载，但速度上会比较慢，可能还会出现下载包的不完整，所以我们使用淘宝的npm镜像进行安装。

**3.1、安装淘宝npm镜像**

``` bash
npm --registry https://registry.npm.taobao.org info underscore
```

**3.2、使用cnpm命令安装hexo**

``` bash
`cnpm install hexo-cli -g`
`cnpm install hexo -g`
--- 其中，hexo-cli是客户端，hexo是服务端，-g代表全局安装
```
至此，环境准备告一段落，下面开始正式部署和优化

## 四、本地Hexo的使用 ##

**4.1   部署**

 - 新建一个存放博客的文件夹，例如：blog
 - 双击进入blog文件夹，右键选择`Git Bash Here`命令行界面
 - 初始化blog，输入`hexo init`
 - 模块安装，输入`cnpm install`，会检查node_modules目录，并自动安装
 - 输入`hexo g`，生成静态文件
 - 输入`hexo s`，启动服务器
 - 在浏览器中打开`localhost:4000`，成功的话就可以看到博客页面了

**4.2   新建博文**

输入`hexo new "文章题目"`，确认后，source/_posts目录中多了一个`.md文件`，这就是我们刚才新建的博文。

> 注:新建博文后，需要执行`hexo s -g`，重新生成静态文件并启动服务器。

**4.3   编辑博文**

用编辑器打开`.md文件`就可以正经写博文了，但你会发现根本无从下手，你缺少的是强有力的辅助`markdown编辑器`，它可以帮助你快速的写博文，无须关注样式的实现，当然，你也可以使用传统的html+css。

**4.4   markdown的使用**

首先，你必须得了解markdown的[基本语法][4]，同时借助hexo-admin插件即可。

``` bash
cnpm install hexo-admin --save
--- 本地安装hexo-admin插件
hexo s -g
--- 重新生成静态文件，并开启服务器
```

现在，浏览器访问`localhost:4000/admin`即可编辑发布。

> 虽然现在可以快速编辑博文了，但还不够完美，hexo-admin编辑markdown没有快捷键！

网上有很多印象笔记支持快捷键操作，比如：马克飞象，简书等。
本人使用的是[作业部落](https://www.zybuluo.com/mdeditor)里的在线markdown,它每隔3s左右就会自动保存，你不使用的时候，直接关闭网页就可以了，很方便！！

**4.5   博客优化**
 
4.5.1、主题yilia的使用

> 安装：输入以下命令，下载yilia主题
`git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia`
配置：修改_config.yml文件，将theme: **landscape**（修改为**yilia**）

修改完毕后，`hexo s -g`，呃...修改了静态文件，必须要有这步操作。

 
## 五、部署基于github的在线博客 ##

**5.1   创建github账号**

要想使用github第一步当然是注册github账号了， github官网地址：https://github.com/。 之后就可以创建仓库了。

**5.2   创建公共库**

登陆github账户后，点击右上角`+`号，Create a New Repository，填好名称后Create，库名称的格式为，你的username.github.io，例如，我的username为showver，所以公共库名就为，showver.github.io。

**5.3   连接hexo与github**

有了github公共库，我们就可以将hexo代码托管到远端（remote）。
配置：打开配置文件_config.yml，拉到底部
<pre>
deploy:
  type: git
  repo: https://github.com/showver/showver.github.io.git
  branch: master
</pre>

    注意：冒号后面有个空格，并且空格为半角空格。

为了能够使Hexo部署到GitHub上，需要安装一个插件：

> `cnpm install hexo-deployer-git --save`
--- 其中，--save代表本地安装

这时，就可以使用`hexo d`，将代码部署到github。


## 六、优化部署与管理 ##

**6.1   概述**

Hexo部署到GitHub上的文件，是.md（你的博文）转化之后的.html（静态网页）。因此，当你重装电脑或者想在不同电脑上修改博客时，就不可能了（除非你自己写html o(^▽^)o ）。

其实，Hexo生成的网站文件中有.gitignore文件，因此它的本意也是想我们将Hexo生成的网站文件存放到GitHub上进行管理的（而不是用U盘或者云备份啦(╬▔皿▔)凸）。这样，不仅解决了上述的问题，还可以通过git的版本控制追踪你的博文的修改过程，是极赞的。

但是，如果每一个GitHub Pages都需要创建一个额外的仓库来存放Hexo网站文件，我感觉很麻烦（10个项目需要20个仓库(ˉ▽ˉ；)…）。

所以，我利用了分支！！！

简单地说，每个想建立GitHub Pages的仓库，起码有两个分支，一个用来存放Hexo网站的文件，一个用来发布网站。

下面以我的博客作为例子详细地讲述。

**6.2   git与github的连接**

了解了能够在本地使用git管理github上的项目，需要进行一些配置，这里介绍SSH的方法

> 1、检查电脑是否已经有ssh key；
`ls -al ~/.ssh`
2、如果没有SSH KEY，则生成新的SSH KEY，需要注册github账号时用的邮箱；
`ssh-keygen -t rsa -C "your_email@youremail.com"`
之后一路回车确认；
3、将拷贝的id_rsa.pub内容导入到github；
在GitHub右上方点击头像，选择”Settings”，在右边的”Personal settings”侧边栏选择”SSH Keys”。接着粘贴key，点击”Add key”按钮
4、最后，测试链接；
`ssh -T git@github.com`
如果是第一次的会提示是否continue，输入yes就会看到：You've successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。

接下来我们要做的就是把本地仓库传到github上去，在此之前还需要设置username和email，因为github每次commit都会记录他们。

> `git config --global user.name "your name"`
`git config --global user.email "your_email@youremail.com"`


**6.3   博客搭建流程**

1、在博客仓库创建分支hexo，并设置为默认分支；

2、将博客仓库clone至本地，使用git命令拷贝仓库
`git clone git@github.com:CrazyMilk/CrazyMilk.github.io.git`

3、将本地搭建的hexo文件夹里的_config.yml，themes/，source，scffolds/，package.json，.gitignore文件复制到你克隆下来的仓库文件夹，即Username.github.io；（Username是你自己的用户名）

4、将themes/yilia/(本文用的是yilia主题)中的.git/删除，否则无法将主题文件夹push；

5、在Username.github.io文件夹执行npm install，npm install hexo-deployer-git(这里可以看看分支是不是显示为Hexo)；

6、执行git add .，git commit -m "提交文件"，git push origin hexo，来提交hexo网站源文件；

7、执行hexo g -d 将生成静态网页部署到github上。

这样一来，在GitHub上的showver.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。无敌！！！


**6.4   博客管理流程**

在本地对博客修改（包括修改主题样式、发布新文章等）后

1、执行git add，git commit -m "提交文件"，git push origin hexo来提交hexo网站源文件；

2、执行hexo g -d 生成静态网页部署到github上；
（每次发布重复这两步，它们之间没有严格的顺序）

**6.5   博客恢复流程**

换电脑想改博客：
1、安装git；
2、安装Nodejs和npm；
3、使用克隆命令将仓库拷贝至本地；
4、在文件夹内执行命令npm install hexo-cli -g、npm install、npm install hexo-deployer-git；

## 七、附录 ##

**7.1   常用的操作**

 - `hexo g` == hexo generate， 生成静态文件
 - `hexo s` == hexo server，启动服务器
 - `hexo d` == hexo deploy，部署网站
 - `hexo n` == hexo new，新建博文
 - `hexo clear` ，清除缓存文件


**7.2   目录说明**

 - _config.yml站点的配置文件，需要拷贝；
 - themes/主题文件夹，需要拷贝；
 - source/博客文章的.md文件，需要拷贝；
 - scaffolds/文章的模板，需要拷贝；
 - package.json安装包的名称，需要拷贝；
 - .gitignore限定在push时哪些文件可以忽略，需要拷贝；
 - .git/主题和站点都有，标志这是一个git项目，不需要拷贝；
 - node_modules/是安装包的目录，在执行npm install的时候会重新生成，不需要拷贝；
 - public是hexo g生成的静态网页，不需要拷贝；
 - .deploy_git同上，hexo g也会生成，不需要拷贝；
 - db.json文件，不需要拷贝；
 





 



 










 
 


 


  [1]: http://nodejs.cn/
  [2]: https://git-scm.com/downloads
  [3]: https://pan.baidu.com/s/1kU5OCOB?errno=0&errmsg=Auth%20Login%20Sucess&&bduss=&ssnerror=0&traceid=#list/path=/pub/git
  [4]: https://www.jianshu.com/p/b03a8d7b1719