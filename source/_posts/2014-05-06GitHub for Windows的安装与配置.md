---
title: GitHub for Windows的安装与配置
categories: 瞎折腾
tags:
  - Github
cover: 'https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109210228186.png'
abbrlink: 562a5dc0
date: 2014-05-06 15:25:00
updated: 2014-05-06 15:25:00
---
初接触git，看了很多`msysgit`的使用教程，对于没太接触过相关内容的我深感糊涂。虽然最后也能使用[git](http://git-scm.com/)配合[GitHub](https://github.com/)进行简单的操作，但还是感到过程对于我来说甚是麻烦。

突然不小心发现了GitHub有Windows客户端，顿时感觉被解放了。下载之看了一些教程，感觉蛮舒服的。那么就在这里汇总和验证下安装和配置的过程当作教程好了。

## Setup.1 下载安装

为什么下载安装还需要单独说明？因为习惯firefox和chrome配合使用的我开始就是卡在这一步了。

1. 使用浏览器打开 [https://windows.github.com/](https://windows.github.com/) 后点击Download按钮，会下载一个`GitHubSetup.exe`文件，双击该文件。

2. 双击后会发现安装出错，别着急，这是正常现象，点击`Details`按钮出现下图所示：
![](https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109151735086.jpg)

3. 复制其中的`http://github-windows.s3.amazonaws.com/GitHub.application`，使用IE浏览器打开此链接，无法打开的请自行科学上网。
    **直接忽略前两步从此处开始也可。 这里切记使用的浏览器一定要是IE，如果使用firefox或者chrome会导致直接用内置下载器下载该文件，但这不是我们所想要的。**

4. 很快就出现了安装界面，如下图：
![](https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109151735211.jpg)

5. 点击安装，等待下载与安装。

6. 剩余的时间，静静等待进度条到达100%即可安装成功（下载服务器在国外，略慢）

7. 等待安装结束后会自动打开GitHub for Windows的登陆界面，如下：
![](https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109151735102.jpg)

8. 输入Username or Email 以及 Password进行登陆就ok了

## Setup.2 基础配置
好了，接下来说一说该如何使用GitHub for Windows。
1. 登陆GitHub账号后会自动创建ssh的密钥并且在GitHub的web端配置好，需要做的仅仅是点击中上方的tools ===> Options 打开设置选项。
![](https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109151735278.jpg)

2. 设置你的全局name和email，选择默认存储文件夹，以及默认的命令行工具。然后点击Update更新。

3. 配置完毕后，就可以开始你的GitHub之旅了！

-------

> **Do I need to install anything extra?**
> GitHub for Windows includes a fully functional version of msysGit — no need to install anything extra. You can pull up a PowerShell console within the context of any repository. GitHub for Windows even includes the amazing posh-git utility for your command line pleasure.
> ![](https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109151736293.png)