---
title: 配置自己的工作环境3：Sublime Text 2 的安装与配置
categories: 瞎折腾
tags:
  - 工作环境
  - Sublime Text 2
cover: 'https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109210220204.png'
abbrlink: e1b8bbc5
date: 2014-05-06 11:25:00
updated: 2014-05-06 11:25:00
---
写在前面的话：

> Windows下的文本（代码）编辑器多不胜数，如Notepad++、Editplus等等。但选择哪一款在于自己的习惯，博主在这里选择的是号称性感无比的代码编辑器Sublime Text 2 （[via](http://www.iplaysoft.com/sublimetext.html)）

对于性感的软件，博主的定义有3：

1. 界面美观漂亮
2. 启动&操作速度顺畅
3. 可扩展（插件及主题）

很幸运的，Sublime Text 2全都符合，下面开始：

## 下载及安装
Sublime Text 2 是一款商业收费软件，价格甚至高达59美刀。不过不用担心，作者很厚道的提供了免费无限制无限期的试用权，也就是说，除了偶尔（频率非常低）会弹出提示你没有购买之外，你可以永久免费的使用该软件的全功能没有任何问题。

官方下载地址：[http://www.sublimetext.com/2 ](http://www.sublimetext.com/2)（Sublime Text 3 目前还在测试版，现在推荐2）

下载完成后正常安装即可，养成修改安装路径的好习惯，我习惯安装在`D:\Program Files\` 目录下。

安装完毕后即可打开 Sublime Text 2 软件。咦，是英文界面？那就对了…

中文语言包及汉化教程下载：[http://pan.baidu.com/s/1i340ikX](http://pan.baidu.com/s/1i340ikX)

PS：这个语言包将插件翻译为了“程序包”，其实这才是最准确的，原单词为 Package 。

安装语言包后再打开，是中文了吧。安装结束，体验吧！

## 技巧及插件
这些也是 Sublime Text 2 最性感的地方了。

- 我常用的颜色主题：Monokai
- 安装Package Control
    1. 点击查看-显示控制台（快捷键Ctrl+`）
    2. 粘贴以下代码到底部命令行并回车
    
        ```
        import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
        ```
    3. 重启Sublime Text 2
    4. 如果在Perferences->package settings中看到package control这一项，则安装成功。
- 使用Package Control
    1. 按下Ctrl+Shift+P调出命令面板
    2. 输入install 调出 Install Package 选项并回车，然后在列表中选中要安装的插件。
    3. 即可以在线查找及安装插件了
    
ok，下面开始介绍插件，由于网上存在很多的插件介绍，博主这里只介绍自己使用的一些：

- Gist：Sublime Text 2 + Gist = 代码片段管理器（[via](http://lucifr.com/2012/03/07/sublime-text-2-plus-gist-equal-snippet-manager/)）

    > PS：原始的填入 Gist 账户名及密码来验证GitHub账户的方式已失效 （[via](https://github.com/condemil/Gist#generating-access-token)）新的验证方式步骤如下：
    > 1. 进入[https://github.com/settings/applications](https://github.com/settings/applications)
    > 2. 点击 Create New Token 创建一个新的Token，可以使用Gist for Sublime Text 2这样d描述来区分
    > 3. 打开 Sublime Text 2 里的Gist插件配置页面，填入token:XXXXXXXXXXXXX，保存即可
    
- WordPress：集成一些WordPress的函数，对于像我这种经常要写WP模版和插件的人特别有用！
- Markdown Preview：通过sublime text2来编辑markdown（[via](http://www.1008a.com/post/679.html)）

    > Sublime Text 2自带的主题风格中没有Markdown语法的高亮功能
    > Markdown语法高亮主题下载：[http://pan.baidu.com/s/1eQoXpz8](http://pan.baidu.com/s/1eQoXpz8)
    
- ……先列举这么多

ok，关于 Sublime Text 2 的介绍和使用教程到此结束！