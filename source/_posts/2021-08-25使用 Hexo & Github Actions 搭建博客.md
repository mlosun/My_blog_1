---
title: 使用 Hexo & Github Actions 搭建博客
date: 2021-08-25 19:37
updated: 2021-08-25 19:37
categories: 瞎折腾
tags:
  - Hexo
  - Github
  - Github Actions
cover: https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109210256825.png
abbrlink: 29b33eae
---
## 方案概述
一直认为在 Github 上搭建静态博客是一件很省~~钱~~心的事情，不需要去操心服务器到期、各种配置，并且 Github 也已经到了大而不倒（至少很难）的地步了吧。试过不少静态博客方案，也看过不少 Github Actions 的教程，最终结合自己的思路整理出来一套超简单的 Hexo & Github Actions 搭建博客方案。

### 提供两种方案
- 单库方案：Hexo源文件和生成的静态文件在同一个 Github 仓库中管理，便于管理维护
- 双库方案：Hexo源文件存放在私有仓库中，生成的静态文件存放在公有仓库中，便于保护隐私

### 步骤简述如下
1. 设置 Github 仓库 & 添加 SSH key
2. 本地搭建 Hexo博客
3. 本地配置 Hexo部署
4. 本地配置 Github Actions
5. 部署到 Github 仓库
6. 设置 Github Pages


### 详细步骤中替换内容
>- `mlosun`——Github 用户名
>- `HexoBlog`——单库方案中的 Github 仓库
>- `HexoBlogPublic`——双库方案中的 Github **公有**仓库
>- `HexoBlogPrivate`——双库方案中的 Github **私有**仓库

## 安装程序

在安装Hexo之前，需要先安装依赖程序 [Node.js](http://nodejs.org/) & [Git](http://git-scm.com/)，在官网上下载最新版本安装即可。

随后只需要打开终端，使用npm命令一键安装Hexo即可。
```
npm install -g hexo-cli
```

### 单库方案

#### Step.1 Github 设置

登录[Github](https://github.com/)，创建一个新的项目仓库（例如：`https://github.com/mlosun/HexoBlog.git`）。

随后在终端使用命令 `ssh-keygen -t rsa -C "mlosun"`创建新的 SSH key 一对公钥私钥，创建后可以在`~/.ssh`目录下找到它们，带有`.pub`的是公钥，另一个是私钥。
- 在 Github 上的 Settings - SSH and GPG keys 中，创建一个新的 SSH key ，名称随意，将**公钥**内容复制进去
- 在当前项目仓库的 Settings - Secrets 中，创建一个新的 secret ，命名为`ACCESS_TOKEN`，**私钥**内容复制进去

#### Step.2 搭建博客

回到本地，在你的工作目录新建存放 Hexo 文件的文件夹，进入该文件夹并初始化 Hexo 。
```
hexo init HexoBlog
cd HexoBlog
npm install
```

#### Step.3 配置Hexo部署
打开 HexoBlog 目录下的配置文件 `_config.yml`，配置deploy部分的内容如下。
```
deploy:  
 type: git  
 repo: https://github.com/mlosun/HexoBlog.git # 你的 Github 仓库地址  
 branch: source
```

#### Step.4 配置 Github Actions
在 HexoBlog 目录下新建文件夹`.github/workflows`（隐藏文件夹），新建文件`deploy.yml`，内容如下。
```
name: Hexo Blog CI & CD

on:
  push:
    branches:
      - source  # 存放 Hexo 源文件的分支

jobs:
  pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js 12.x
        uses: actions/setup-node@v1
        with:
          node-version: '12.x'
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }} # 无需修改
          deploy_key: ${{ secrets.ACCESS_TOKEN }}  # 添加 ACCESS_TOKEN
          publish_dir: ./public  # hexo generate 生成的博客文件默认存放在 /public 目录下
          publish_branch: main  # 存放展示的博客文件的分支
```

#### Step.5 部署到 Github Repositories
终端切换到 HexoBlog 目录下，依次执行以下命令。
```
git init  # 初始化git
git add .  # 添加全部文件到暂存区
git commit -m "first commit"  # 提交暂存区文件
git branch -M source  # 分支重命名为 source
git remote add origin https://github.com/mlosun/HexoBlog.git  # 连接远端仓库
git push -u origin source  # 推送到远端仓库
```

#### Step.6 配置 Github Pages 
回到 Github 仓库中，在 Settings - Pages 中，选择`main`分支
>若要绑定自有域名，需要创建 `HexoBlog/source/CNAME`文件，内容填写域名即可。在 Settings - Pages 中绑定的域名在下一次自动部署后会被覆盖。

到此，查看 Github 的仓库，已经有了两个分支`main` 和 `source`，并且后续在`source`分支做的更新都会自动部署到`main` 分支内。而`main` 则通过 Github Pages 服务展示你的博客。

### 双库方案

#### Step.1 Github 设置

登录[Github](https://github.com/)，创建两个新的项目仓库，例如：

- 公有仓库：`https://github.com/mlosun/HexoBlogPublic.git`
- 私有仓库：`https://github.com/mlosun/HexoBlogPrivate.git`

随后在终端使用命令 `ssh-keygen -t rsa -C "mlosun"`创建新的 SSH key 一对公钥私钥，创建后可以在`~/.ssh`目录下找到它们，带有`.pub`的是公钥，另一个是私钥。
- 在`HexoBlogPublic`仓库的 Settings - Deploy keys 中，创建一个新的 deploy key ，名称随意，将**公钥**内容复制进去，记得勾选**Allow write access**
- 在`HexoBlogPrivate`仓库的 Settings - Secrets 中，创建一个新的 secret ，命名为`ACCESS_TOKEN`，**私钥**内容复制进去

#### Step.2 搭建博客

回到本地，在你的工作目录新建存放 Hexo 文件的文件夹，进入该文件夹并初始化 Hexo 。
```
hexo init HexoBlog
cd HexoBlog
npm install
```

#### Step.3 配置Hexo部署
打开 HexoBlog 目录下的配置文件 `_config.yml`，配置deploy部分的内容如下。
```
deploy:  
 type: git  
 repo: https://github.com/mlosun/HexoBlogPrivate.git # 你的私有 Github 仓库地址  
 branch: source
```

#### Step.4 配置 Github Actions
在 HexoBlog 目录下新建文件夹`.github/workflows`（隐藏文件夹），新建文件`deploy.yml`，内容如下。
```
name: Hexo Blog CI & CD

on:
  push:
    branches:
      - source  # 存放 Hexo 源文件的分支

jobs:
  pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js 12.x
        uses: actions/setup-node@v1
        with:
          node-version: '12.x'
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }} # 无需修改
          deploy_key: ${{ secrets.ACCESS_TOKEN }}  # 添加 ACCESS_TOKEN
          publish_dir: ./public  # hexo generate 生成的博客文件默认存放在 /public 目录下
          publish_branch: main  # 存放展示的博客文件的分支
          external_repository: mlosun/HexoBlogPublic  # 文件发布到公有仓库，记得改为自己的地址
```

#### Step.5 部署到 Github Repositories
终端切换到 HexoBlog 目录下，依次执行以下命令。
```
git init  # 初始化git
git add .  # 添加全部文件到暂存区
git commit -m "first commit"  # 提交暂存区文件
git branch -M source  # 分支重命名为 source
git remote add origin https://github.com/mlosun/HexoBlogPrivate.git  # 连接远端仓库
git push -u origin source  # 推送到远端仓库
```

#### Step.6 配置 Github Pages 
回到 Github 仓库中，在公有仓库`HexoBlogPublic`的 Settings - Pages 中，选择`main`分支

>若要绑定自有域名，需要在私有仓库中创建 `HexoBlogPrivate/source/CNAME`文件，内容填写域名即可。在 Settings - Pages 中绑定的域名在下一次自动部署后会被覆盖。

到此，查看 Github 的仓库：

- 公有仓库：`HexoBlogPublic` 内分支`main`，通过 Github Pages 服务展示你的博客。
- 私有仓库：`HexoBlogPrivate` 内分支`source`，发生更新后会自动部署到公有仓库内。

## 日常更新
### 简单更新
可以直接在对应 Github 的仓库中编辑 `source`分支，保存提交后Github Actions 会自动部署。

### 复杂更新
为了减少一些莫名其妙的事情发生，建议每次在进行复杂一些的更新时，删除掉本地的仓库文件，从 Github 上重新 clone 仓库到本地：
```
git clone https://github.com/mlosun/HexoBlog.git 
git checkout source	# 切换到 source 分支
# 双库是`https://github.com/mlosun/HexoBlogPrivate.git`
```
更新时可以在本地使用Hexo服务器进行预览：
```
hexo d	# 生成静态文件
hexo s	# 启动Hexo服务器，默认预览地址http://localhost:4000
```
更新后推送到 Github 仓库时，建议清除缓存文件 (`db.json`) 和已生成的静态文件 (`public`)
```
hexo clean	# 清除缓存
git add .  # 添加全部文件到暂存区
git commit -m "update"  # 提交暂存区文件
git push -u origin source  # 推送到远端仓库
```

## 参考资料
- [Hello Hexo World](https://blog.towind.fun/2019/12/26/hello-hexo-world/)
- [使用 Github Actions 持续集成与部署 Hexo 博客](https://blog.towind.fun/2021/02/18/hexo-github-actions-ci-cd/)
- [GitHub Pages action](https://github.com/marketplace/actions/github-pages-action)