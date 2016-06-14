---
title: windows下使用hexo搭建GitHub Pages
toc: true
date: 2016-06-08 10:57:09
categories: hexo
tags:
	- hexo
	- GitHub
description: 本文讲述如何一步一步使用hexo搭建GitHub Pages.
---
# 软件预装
1. [Node.js](https://nodejs.org)
    国内由于网络环境，某些npm的软件无法下载，推荐用淘宝搭建的[npm](https://npm.taobao.org/)。
    `$ npm install -g cnpm --registry=https://registry.npm.taobao.org #以后就可以用cnpm代替npm`
2. [Git](https://git-scm.com/download)

# hexo
## 安装
```
$ npm install -g hexo-cli
```
## 建站
在一个工作目录下，执行如下命令：
```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
  此时，npm会将建站必须的资源拉入**folder**下,站的框架已经基本搭成。folder目录如下:
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
其中**_config.yml**是hexo的[配置文件](https://hexo.io/zh-cn/docs/configuration.html)，以后的许多配置都是通过它来完成。

## 运行项目
### 编译
```
$ hexo generate
```
### 运行
```
$ hexo server
```
### 访问
此时在浏览器中访问[http://127.0.0.1:4000/](http://127.0.0.1:4000/),即可看到本地搭建好的hexo初始博客站点。

## hexo命令(参见:[指令](https://hexo.io/zh-cn/docs/commands.html))
```
hexo g == hexo generate
hexo d == hexo deploy
hexo s == hexo server
hexo n == hexo new
还能组合使用，如：
hexo s -g
hexo d -g
```

# hexo高级功能

## 更换主题
folder目录下有个themes目录，此目录保存博客的主题(默认是landscape，在hexo配置文件的theme配置项)。hexo有自己的配置文件，即主目录下的_config.yml；主题也有自己的配置文件，即主题目录下的_config.yml。为了区别，我们将前者叫做站点配置文件，后者叫做主题配置文件。
如果想更换其他主题，比如本博的maupassant：
1. 下载主题到theme目录
```
$ cd <folder>
$ git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
```
2. 安装hexo主题插件
```
$ npm install hexo-renderer-jade --save
$ npm install hexo-renderer-sass --save
```
3. 修改站点配置文件
```
language: zh-CN
theme: maupassant
```

4. 重新编译运行
`hexo s -g`后再次访问[http://127.0.0.1:4000/](http://127.0.0.1:4000/)，就可以看到新的主题已经生效了。

## 使标签、分类等生效
```
$ hexo new page tags
$ hexo new page archives
$ hexo new page categories
$ hexo new page about
```
这样，下次`hexo g`的时候，会将标签、分类等信息编译进相应的目录下（相应的插件在安装hexo时已经默认安装了），并生效。

## 使搜索框的本地搜索生效
1. 安装hexo插件`npm install hexo-generator-search --save`
2. 修改主题配置文件的self_search
```
self_search: true ## Use a jQuery-based local search engine, true/false.
```
3. 重新编译启动hexo即可。

## 添加评论功能
maupassant使用的是第三方评论平台[多说](http://duoshuo.com/)。
1. 登陆多说并创建站点
    ![](http://7xv60m.com1.z0.glb.clouddn.com/%E5%A4%9A%E8%AF%B4%E5%88%9B%E5%BB%BA%E7%AB%99%E7%82%B9.jpg)
2. 编辑主题配置文件，将上图红色方框里的多说短域名设置为duoshuo的值。
```
duoshuo: xxxx ## Your duoshuo_shortname, e.g. username
```
3. OK,重新编译启动hexo后，即可看到评论功能。评论后就能在创建的多说站点里，管理所有的评论啦。

## 404页面
在主目录的source目录下创建**404.html**或**404.md**(注意文件需要UTF-8编码)，内容如下：
```
---
layout: false
title: "页面不存在"
---

<!DOCTYPE html>
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html;charset=utf-8">
        <script type="text/javascript" src="http://www.qq.com/404/search_children.js" charset="utf-8" homePageUrl="http://xxx.github.io" homePageName="回到首页"></script>
    </head>
    <body>
    </body>
</html>
```

## RSS订阅
1. 安装hexo插件`npm install hexo-generator-feed --save`
2. 修改站点配置文件,增加如下设置
``` 
Plugins:
- hexo-generator-feed
#Feed Atom
feed:
  type: atom
  path: atom.xml
  limit: 20
```
3. 重新编译启动hexo即可。

## 部署到GitHub Page
1. 在github创建一个username.github.io的库（username为自己github的用户名，**必须按照这种格式**）;
2. 这时通过username.github.io就可以看到一个空的博客站点了；
3. 修改站点配置文件
```
deploy:
  type: git
  repo: https://github.com/<username>/<username>.github.io.git
  branch: master
```
4. 本地博客内容同步到github
```
hexo d -g
```

## 加入百度统计
1. 注册[百度统计](http://tongji.baidu.com/web/welcome/login)
	在域名填入自己的站点
2. 获取百度统计号
![](http://7xv60m.com1.z0.glb.clouddn.com/%E7%99%BE%E5%BA%A6%E7%BB%9F%E8%AE%A1%E4%BB%A3%E7%A0%81%E8%8E%B7%E5%8F%96.jpg)
3. 修改主题配置文件的baidu_analytics配置项
``` 
baidu_analytics: xxxxxxxxb516a46cxxxxxxxx ## Your Baidu Analytics tracking id, e.g. 8006843039519956000
```
4. 重新编译启动hexo即可。

## 加入Google分析
1. 注册[Google Analytics](https://analytics.google.com/analytics/web/?hl=zh-CN&pli=1#report/defaultid/a79273564w118388251p123837186/),会生成一个跟踪ID，形如UA-42425684-2.
2. 修改主题配置文件的google_analytics配置项
``` 
google_analytics: UA-42425684-2 ## Your Google Analytics tracking id, e.g. UA-42425684-2
```
3. 重新编译启动hexo即可。

## 使用七牛图床
GitHub Pages只有300MB的空间，所以图片最好还是放在第三方，比如七牛。
1. 注册七牛-->添加资源-->对象存储，添加一个存储库。
2. 上传图片(内容管理-->上传文件)，获取外链(操作-->复制外链)。

# 创建文章
OK，经过前边的设置，一个功能齐全的博客站点已经形成了。下面要做的就是编写博客了。

1. 创建文章
```
hexo n "title"
```
2. 在source/_post下找到相应的md文件，使用编辑器(推荐[Cmd Markdown](https://www.zybuluo.com/mdeditor#402917))进行编写。
3. 编写过程中本地预览
```
hexo s -g --debug
```
	不用每次都重新编译重启hexo，它是热生效的。
4. 博客编写完成后，同步到GitHub.
```
hexo d -g
```

