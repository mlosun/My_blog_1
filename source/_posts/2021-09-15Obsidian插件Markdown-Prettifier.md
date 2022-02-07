---
title: Obsidian插件——Markdown Prettifier
categories: 瞎折腾
tags:
  - Obsidian
  - 插件
  - Markdown Prettifier
cover: 'https://cdn.jsdelivr.net/gh/mlosun/cdn.mlosun.com/img/202109210435285.png'
abbrlink: '386e7395'
date: 2021-09-15 10:36:52
updated: 2021-09-21 10:36:52
---
## Markdown Prettifier
> - 简介：美化Markdown格式并添加修改日期到Front matter
> - 更新：2021-09-15
> - 版本：0.08
> - 安装方式：Obsidian 内第三方插件
> - Github：https://github.com/cristianvasquez/obsidian-prettify

## 介绍

每个人使用 Markdown 的习惯不同，例如无序列表可使用`* 项目`、`+ 项目`或者`- 项目`；加粗文本可使用`*加粗文本*`或`_加粗文本_`，这种对于有强迫症的人就是一个灾难。Markdown Prettifier 可以将这些格式统一起来，让阅读 Markdown 文件时更舒适。

另外最值得推荐的是新增/更新 [Front matter](https://publish.obsidian.md/help-zh/%E9%AB%98%E7%BA%A7%E7%94%A8%E6%B3%95/YAML+front+matter)，设置好快捷键，方便在编辑了笔记后新增/更新日期到Front matter。

## 中文翻译
> 结合自己理解的机翻。如有疑问请留言

安装好社区插件原版后，下载压缩包解压后覆盖 Obsidian 库内的`.obsidian/plugins/`内对应文件

[汉化版Markdown Prettifier下载](https://github.com/mlosun/cdn.mlosun.com/raw/main/files/markdown-prettifier.zip)


## 我的应用
- Front matter 模版设置为`update: {{date:YYYY/MM/DD}}`（使用`YYYY-MM-DD`会出现莫名bug，[来源](https://forum-zh.obsidian.md/t/topic/389)）
- 快捷键设置为`Command+S`
  - 替换原有的保存当前文件`Command+S`快捷键
  - 多年伏案工作早已养成了随时`Command+S`的好习惯