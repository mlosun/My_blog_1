---
title: 使用Hugo+Github Actions实现自动化部署博客
categories: 瞎折腾
tags:
  - 博客
  - Github
  - Hugo
cover: 'https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109210302332.png'
abbrlink: beeeabdb
date: 2020-08-27 15:25:00
updated: 2020-08-27 15:25:00
---
> 很早就想搭建一个自己的博客了，因为某些原因一直为付诸行动。直到近期自己对建博的需求愈发强烈，在尝试过了各类流行的方案后，选择Hugo+Github Action来实现静态博客的自动化部署。这里也将整理过的流程记录下来，也作为新博客的第一篇文章。

## 一、环境准备

安装[Git](https://git-scm.com)、[Hugo](https://gohugo.io)，注册[Github](https://github.com)账号，网上教程很多就不再赘述，我使用MacOS系统，Windows系统用户可自行调整。

注：请自行将下文中的`My_blog`、`mlosun`替换为自己的仓库名、站点名、文件夹名、用户名

## 二、初始化Hugo项目

### 1.新建站点
本地安装好各类环境后，终端通过`hugo new site My_blog`新建站点，生成hugo站点的默认目录，目录树如下：

```
My_blog  
├─ archetypes     
│  └─ default.md  
├─ content        
├─ data           
├─ layouts        
├─ static         
├─ themes         
└─ config.toml    
```

### 2.安装主题

Hugo默认不带主题，可以在[官方主题仓库](https://themes.gohugo.io)选择喜欢的主题，本次我使用了国产的[Zozo主题](https://themes.gohugo.io/hugo-theme-zozo/)，在主题页面下载好主题文件后，将主题文件复制到`themes`文件夹下，并重命名为`zozo`，并将主题的示例文件`exampleSite`目录下的示例文件复制到项目根目录下替换原有文件：
```
My_blog
├─ archetypes                          
│  └─ default.md                       
├─ content                             
│  ├─ about                            
│  │  └─ index.md                      
│  └─ posts                            
│     ├─ chinese-preview.md            
│     ├─ english-preview.md                 
│     ├─ japanese-preview.md           
│     └─ theme-preview.md              
├─ data                                
├─ layouts                                                 
├─ static                              
│  ├─ 100.jpg                                                    
├─ themes                              
│  └─ zozo                                          
│     └─ 主题文件                   
├─ config.toml                         
└─ netlify.toml                        
```

终端进入项目目录`cd My_blog`，输入`hugo server -D -w`命令后，在浏览器中访问[http://localhost:1313](http://localhost:1313)进行预览。

若没有问题，初始化Hugo项目完成。

## 三、Git仓库配置
在Github上创建名为`My_blog`的公开仓库，创建时无需进行初始化

依次在终端中执行以下命令：

1. `git init`初始化仓库
2. `git remote add origin https://github.com/mlosun/My_blog.git`连接远端仓库
3. `git checkout -b develop`切换至develop分支（[不使用master分支的原因？](https://weibo.com/ttarticle/p/show?id=2309404516196870390640)）
4. `git add .`暂存全部文件
5. `git commit -m "first commit"`提交
6. `git push -u origin develop`推送到远端仓库

执行完毕后去Github上看下文件是否都已上传，顺手在Github上新建一个`gh-pages`分支

## 四、自动化部署
在Github中的[创建一个Token](https://github.com/settings/tokens)(记得在页面关闭前保存下来，页面一旦关闭就再也看不到了),回到`My_blog`项目仓库中在`Settings/Secrets`创建一个`Secrets`，标题命名为`personal_token`，内容填入刚刚创建的`Token`。

本地`My_blog`目录中新建`.github/workflows/deploy.yml`，内容填入以下代码：

```
name: Deploy Hugo

on:
  push:
    branches:
      - develop

jobs:
  build-deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v1  # v2 does not have submodules option now
        # with:
          # submodules: true

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: latest
          # extended: true

      - name: Build
        run: hugo

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          personal_token: ${{ secrets.personal_token }} # 这里的 ACTIONS_DEPLOY_KEY 则是上面设置 Private Key的变量名
          PUBLISH_BRANCH: gh-pages
          PUBLISH_DIR: ./public
          commit_message: ${{ github.event.head_commit.message }}
```

依次在终端中执行以下命令：
1. `git add .`暂存全部文件
2. `git commit -m "Github Action自动化部署"`提交
3. `git push -u origin develop`推送到远端仓库

等待Github仓库中Actions`Deploy Hugo`执行完毕后，看下`gh-pages`分支是否已生成HTML文件。

## 五、其他补充内容

### 1.绑定域名

在本地`static`目录下新建`CNAME`文件，内容填入绑定的域名`www.mlosun.com`，同时也需要将域名解析至`mlosun.github.io.`

后依次在终端中执行：
1. `git add .`暂存全部文件
2. `git commit -m "绑定域名"`提交
3. `git push -u origin develop`推送到远端仓库

### 2.修改主题配置

修改`confit.toml`文件，之前从[Zozo主题](https://themes.gohugo.io/hugo-theme-zozo/)复制过来的`My_blog/confit.toml`文件中对大部分配置都有详细的注释说明，根据自己情况修改即可

注：`baseURL`字段记得必须修改，否则页面会无法加载样式

后依次在终端中执行：
1. `git add .`暂存全部文件
2. `git commit -m "修改主题配置"`提交
3. `git push -u origin develop`推送到远端仓库

### 3.开启Valine评论

在`confit.toml`文件中找到`[params.valine]`部分，填入从[LeanCloud](https://www.leancloud.cn)获取到的AppID和AppKey，将`enable`更改为`true`即可

```
[params.valine]
  enable = true
  appId = ""
  appKey = ""
  placeholder = "期待你一针见血的评论，Come on！"
  visitor = true
```

后依次在终端中执行：
1. `git add .`暂存全部文件
2. `git commit -m "开启Valine评论"`提交
3. `git push -u origin develop`推送到远端仓库

### 3.访问测试
尝试访问[www.mlosun.com](https://www.mlosun.com)，看是否能顺利打开，页面显示是否正常等.

## 六、参考资料
- [Hugo + Github Actions 实现自动化部署](https://immmmm.com/hugo-github-actions/)
- [用 Hugo 配合 Github Actions 自动构建我的博客](https://www.nashome.cn/posts/hugo-github-actions/)
- [Deploy Hugo on Github Pages](https://piggy.site/posts/deploy-hugo-on-github-pages/)