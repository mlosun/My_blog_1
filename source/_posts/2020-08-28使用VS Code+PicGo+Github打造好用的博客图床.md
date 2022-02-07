---
title: 使用VS Code+PicGo+Github打造好用的博客图床
categories: 瞎折腾
tags:
  - vscode
  - Github
  - PicGo
  - 图床
cover: 'https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109210305088.png'
abbrlink: c6ccae09
date: 2020-08-28 15:25:00
updated: 2020-08-28 15:25:00
---
> Markdown图片的管理一直是个难题，而使用图床是一个当前比较流行的做法。但又不想使用过多的第三方服务，搜索了一圈发现当前这个方案似乎比较适合自己，配置起来又不会过于繁琐。这里也将整体的方案记录下来。

> **2020-08-31更新**：有点尴尬的是，刚写完本文第二天我的Macbook中的VS Code PicGo扩展快捷键`Cmd + Opt + U`不知何故失效了，初步判断是和某个App的快捷键冲突了，找了一圈无果。故转而使用[PicGo独立应用](https://picgo.github.io/PicGo-Doc/zh/)而非[PicGO扩展](https://github.com/PicGo/vs-picgo)了。
> **两个切换Tips**：
> #### 1.[PicGo独立应用](https://picgo.github.io/PicGo-Doc/zh/)展示[PicGO扩展](https://github.com/PicGo/vs-picgo)的历史上传记录
> - [PicGo独立应用](https://picgo.github.io/PicGo-Doc/zh/)的历史上传记录在`~/Library/Application Support/picgo/data.json`
> - [PicGO扩展](https://github.com/PicGo/vs-picgo)的历史上传记录在`your_home_dir/vs-picgo-data.json`
> - 两者格式一致，手动复制过来即可
> - 注：[picgo-plugin-vscode-migrator](https://github.com/upupming/picgo-plugin-vscode-migrator)已失效无法使用
> #### 2.[PicGo独立应用](https://picgo.github.io/PicGo-Doc/zh/)和[PicGO扩展](https://github.com/PicGo/vs-picgo)的时间戳重命名区别
> - [PicGo独立应用](https://picgo.github.io/PicGo-Doc/zh/)的时间戳重命名格式为YYYYMMDD
> - [PicGO扩展](https://github.com/PicGo/vs-picgo)YYYY-MM-DD
> 
> 好在本方案刚刚启用，已上传的图片并不多，手动修正了上述两个小问题后，后续还是老老实实使用[PicGo独立应用](https://picgo.github.io/PicGo-Doc/zh/)吧~ε(┬┬﹏┬┬)3


## 一、方案亮点

使用[Github](https://github.com)作为图床的托管，一方面经济、稳定，另一方面也能少一些托管服务商，方便整体的备份。至于速度问题，可以使用[jsDelivr](https://www.jsdelivr.com)来解决.

使用[VS Code](https://code.visualstudio.com)+[PicGO扩展](https://github.com/PicGo/vs-picgo)，来解决使用时的便捷性问题，毕竟少一个应用软件，就能少分心一些。

综上，本方案具备的特点是：免费、快速、稳定、易管理。

## 二、Github仓库配置

1.在Github上新建一个名为`My_images`的仓库，并初始化，初始化时记得设置默认分支为`main`（[不使用master分支的原因？](https://weibo.com/ttarticle/p/show?id=2309404516196870390640)）

2.在Github中的[创建一个Token](https://github.com/settings/tokens)（记得在页面关闭前保存下来，页面一旦关闭就再也看不到了）

只需要这两步，就可以关闭Github的页面了。

## 三、VS Code +PicGo配置

1.在安装好VS Code的情况下，搜索扩展`PicGo`，并安装

![](https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109151744219.png)

2.打开扩展`PicGo`的配置页面，依据自己的情况完善即可

![](https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109151745401.png)

- **Picgo: Custom Output Format** 自定义输出格式，我使用`![](${url})`
- **Picgo: Custom Upload Name** 自定义上传文件名，我使用`${dateTime}${extName}`
- **Picgo › Pic Bed: Current** 使用的图床服务，我使用Github
- **Picgo › Pic Bed › Github: Branch** Github的分支，我使用`main`
- **Picgo › Pic Bed › Github: Custom Url** 自定义Url前缀，我使用`https://cdn.jsdelivr.net/gh/mlosun/My_images@main`
- **Picgo › Pic Bed › Github: Path** Github的文件路径，我使用`BlogImages/`
- **Picgo › Pic Bed › Github: Repo** Github的用户名/仓库名，我使用`mlosun/My_images`
- **Picgo › Pic Bed › Github: Token** Github的Token值，填入之前在Github生成的Token即可

也只需这两部，整个方案就已配置完毕。

## 四、使用方案

| 系统           | 从剪贴板上传图片               | 从资源管理器上传图片                  | 从输入框上传图片               |
| ------------ | ----------------------------------------------- | ----------------------------------------------- | ----------------------------------------------- |
| Windows/Unix | Ctrl + Alt + U | Ctrl + Alt + E | Ctrl + Alt + O |
| MacOS          | Cmd + Opt + U  | Cmd + Opt + E  | Cmd + Opt + O  |

## 五、配置补充说明

配置项比较简单，都没什么好说的，唯有以下三个需要稍微补充下

### 1.自定义输出格式
配置项：`Picgo: Custom Output Format`，这里决定了将图片上传后，输出什么样的内容到VS Code的，一般使用Markdown的格式`![]()`即可，可选参数：
- `${url}`url地址
- `${uploadedName}`上传后的文件名（即时下面自定义上传的文件名中包含了扩展名，也不带扩展名）

### 2.自定义上传文件名
配置项：`Picgo: Custom Upload Name`，这里决定了图片上传后的名称，可选参数：
- `${fileName}`本地的文件名
- `${extName}`本地的文件扩展名
- `${mdFileName}`Markdown文件的名称
- `${date}`当日日期（格式2020-08-28）
- `${dateTime}`当前日期时间（格式2020-08-28-12-30-10）

### 3.自定义Url
配置项：`Picgo › Pic Bed › Github: Custom Url`，这里决定了图片的自定义Url地址前缀

1. Github上图片的默认地址，可填入`https://raw.githubusercontent.com/用户名/仓库名/分支名`
2. 若将Github仓库绑定域名后，可填入`https://绑定的域名`
3. 若需要使用jsDelivr加速服务，可填入`https://cdn.jsdelivr.net/gh/用户名/仓库名@分支名`

我需要使用jsDelivr服务，同时也绑定了域名，所以三种方式都可以访问到图片资源，示例：

```
https://raw.githubusercontent.com/mlosun/My_images/main/BlogImages/20200828130012.png
https://images.mlosun.com/BlogImages/20200828130012.png
https://cdn.jsdelivr.net/gh/mlosun/My_images@main/BlogImages/20200828130012.png
```

## 六、参考资料
- [VSCode + Github + Picgo + jsDelivr 搭建稳定快速高效图床](https://shanya.world/archives/2fb4d43a.html)
- [Visual Studio Code 图床工具插件你值得拥有](https://nsoft.vip/2019/03/14/190314-图床工具介绍/)
- [PicGo-图片上传、管理新体验](https://picgo.github.io/PicGo-Doc/zh/)