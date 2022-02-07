---
title: 配置自己的工作环境6：使用Github+hexo搭建静态博客
categories: 瞎折腾
tags:
  - 工作环境
  - Github
  - Hexo
cover: 'https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109210220204.png'
abbrlink: 93ba44ca
date: 2014-05-06 14:25:00
updated: 2014-05-06 14:25:00
---
搭建一个博客和配置自己的工作环境有什么关系？

博主认为，是为了更好的进行知识系统的管理，当然你也可以说我有印象笔记等等工具。但我想说的是在互联网上混，你离得开交流与互动么？

搭建博客的程序很多，Wordpress、Zblog、jekyll、Octopress、hexo等等。前两者是传统的博客程序，需要主机有php或者asp环境支持。但后三者是静态的博客程序，任何的静态服务器都可以搭建，我们今天就用Github提供的Pages服务来部署我们的Hexo博客。

**Why Hexo？**

> 不可思议的快速 ─ 只要一眨眼静态文件即生成完成 支持 Markdown
> 仅需一道指令即可部署到 GitHub Pages 和 Heroku
> 已移植 Octopress 插件 高扩展性、自订性 兼容于 Windows, Mac & Linux
> ——[tommy351](http://zespia.tw/blog/2012/10/11/hexo-debut/)（Hexo作者）

&

> 易用。不仅部署简单，平时使用中仅需要`hexo new` `hexo generate` `hexo server` `hexo deploy`四个命令。
> 轻。文件少、小，易理解，方便自定义。
> 用户多。虽然赶不上Jekyll和Octopress，但遇到什么问题都能搜索到答案，或者找到同样使用hexo的用户进行参考和咨询。
> ——[zippera](http://zipperary.com/2013/05/28/hexo-guide-1/)

## 搭建一个本地的Hexo博客
> 注意：本节教程只针对Windows用户。

### 安装 Git 和 [Node.js](http://nodejs.org/)
> Git 以及 TortoiseGit 的下载安装上节已经说过，这里不再重复

Node.js的安装也十分简单，只需要在官网上点击`Install`即可自动下载安装文件并执行即可安装。

### 安装Hexo
在任意位置点击鼠标右键，选择Git bash，然后输入以下命令并回车，即可安装
```
npm install -g hexo
```
### 创建Hexo和初始化
安装完成后，在你喜爱的文件夹下（我选择的是E:\MyWork\hexo），在是E:\MyWork\hexo内点击鼠标右键，选择Git bash，输入以下命令并回车，Hexo 即会自动在目标文件夹建立网站所需要的所有文件。
```
hexo init
```
你也可以在任意地方右键打开Git bash，输入
```
hexo init E:\MyWork\hexo
```
也是同样的效果（无须手动创建Hexo文件夹）

### 搭建完成&本地查看
ok，一个Hexo博客就已经在本地搭建完成了，是不是很简单？

接下来让我们在本地浏览一下Hexo博客吧，在E:\MyWork\hexo的Git bash里执行命令：
```
hexo generate
hexo server
```
在浏览器输入[http://localhost:4000/](http://localhost:4000/)就可以打开了。

现在我们的Hexo博客虽然搭建好了，但只是在本地，别人可看不到，所以我们需要将博客部署到Github。

## 部署到Github

### 创建Github帐户及仓库
注册一个[Github](http://www.github.com/)账号，新建一个repository（仓库）。

> 注意repository的命名应该是`mlosun.github.com`将mlosun替换为你的用户名。

### 配置Hexo博客
编辑_config.yml(在E:\MyWork\hexo下)。你在编辑时，要把下面的mlosun都换成你的账号名。

```
deploy:
  type: github
  repository: https://github.com/mlosun/mlosun.github.com.git
  branch: master
```

编辑完保存后在E:\MyWork\hexo下执行以下命令

```
hexo generate
hexo deploy
```

ok，略等10分钟（Github第一次部署网站需要10分钟配置时间）打开网址[http://mlosun.github.com](http://mlosun.github.com) 是不是发现你的网页已经部署上去了？

是不是很简单？

接下来再说几个要点。

### 绑定域名
现在你可以通过[http://mlosun.github.com](http://mlosun.github.com)来访问你的博客，但是我们想要通过[http://mlosun.com/](http://mlosun.com/)来访问怎么办？

在你的E:\MyWork\hexo\source创建名为`CNAME`的文件，内容里写入`mlosun.com`，保存后重新执行以下命令即可。

```
hexo generate
hexo deploy
```

同时把你的域名解析做CNAME解析到`mlosun.github.io.`把用户名换成你自己的

等解析生效后打开[http://mlosun.com/](http://mlosun.com/)看看是不是成功了？

## 使用Hexo
### _config.yml文件配置
`_config.yml`文件是Hexo博客的全局配置文件，里面每个参数都可以自行修改，具体代表的意义参考[http://hexo.io/docs/configuration.html](http://hexo.io/docs/configuration.html)

### 发表新文章

好了，站点配置好了，我想发表一篇文章，怎么做呢？

1. 在E:\MyWork\hexo路径下执行命令hexo new "my new post"

2. 在E:\MyWork\hexo\source_posts中打开这个文件，建议使用Sublime Text 2打开

3. 文件的开头配置格式（一定要配置，不然无法生成新文章）


```
title: my new post #可以改成中文的，如“新文章” 
date: 2014-05-06 07:56:29
#发表日期，一般不改动 
categories: blog #文章文类 
tags: [博客，文章] #文章标签，多于一项时用这种格式
---
#这里是正文，用markdown写
```

然后再
* hexo server，访问localhost:4000预览效果。（退出server用Ctrl+c）
* hexo deploy，同步到github。访问网站看看效果。

### 命令格式小技巧

hexo现在支持更加简单的命令格式了，比如：

* hexo g == hexo generate
* hexo d == hexo deploy
* hexo s == hexo server
* hexo n == hexo new

### 插件与主题

插件和主题可以去[https://github.com/tommy351/hexo/wiki](https://github.com/tommy351/hexo/wiki)这里找
如果有不明白的也可以给Hexo作者在Github发issues，作者是台湾人，也比较方便沟通。

ok，欢迎访问我的博客：[http://mlosun.com](http://mlosun.com)