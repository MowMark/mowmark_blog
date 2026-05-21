---
title: "使用 Hugo 搭建个人博客"
description: "零前端基础也能上手：用 Hugo 搭建属于自己的免费独立博客"
date: 2026-05-13T20:53:02+08:00
# comments: true
draft: false
tags: ["Hugo"]
build:
    list: always    # Change to "never" to hide the page from the list
---

## 前言

由于学业原因, 最近一直在Ubuntu下学习SLAM算法以及开发, 有相当多操作学了又忘, 忘了再学. 为了打破这个循环, 我决定写一个个人博客进行技术沉淀与知识巩固, 顺便作为一个经验分享与交流的地方.

第一篇文章, 就从**如何搭建个人博客**开始!

## 博客搭建

本质上, 搭建博客就是部署一个轻量化的个人网站, 如果要求不高, 完全可以使用[博客园](https://www.cnblogs.com)、[CSDN](https://www.csdn.net) 等现成的内容平台, 但作为一个**喜欢折腾**的人, 我更希望能够拥有一个自己的独立站点.

### 技术方案对比

目前主流的独立博客方案大概分为两派：**传统动态 CMS**（如 WordPress、Halo）和 **静态站点生成器**（如 Hugo、Hexo）。由于静态站点不需要维护数据库，部署成本极低（甚至免费），我最终锁定了后者。

常见的静态框架对比如下：

| 框架 | 底层语言 | 编译速度 | 特点与优势 |
| :--- | :--- | :--- | :--- |
| **Hugo** | Go | 极快 🚀 | 上手快，零依赖，适合喜欢专注内容写作的人 |
| **Hexo** | Node.js | 中等 | 中文社区庞大，插件丰富，但环境配置略繁琐 |
| **Astro**| Astro.js | 快 | 现代前端新宠，组件化强，适合有一定前端基础的玩家 |
| **VitePress**| Vue/Vite | 快 | Vue 生态，文档风格利落，非常适合做技术文档 |

由于我对复杂的前端开发不太感冒, 且相对看重编译速度和本地环境的纯净度, **Hugo** 成为了我的首选。

## 环境准备与依赖安装

Hugo依赖于Go语言, 但从`v0.121.0`起, Hugo已内置了Go工具链, 且包管理器能够自动处理依赖, 所以**不管是手动安装二进制包还是使用包管理器安装的用户, 都可以跳过 Go 语言的安装部分** (除非需要安装其他依赖 Go 语言的软件)

但是后续的代码托管与发布一定离不开Git, 因此, **在安装 Hugo 之前, 我们必须先准备好Git**, 已经安装好了的读者则可以跳过这个部分.

> **提示**: 以下部分网站可能需要科学上网环境, 请自行准备所需工具.

### 安装Go语言

1. **Windows系统**: 打开[Go 官网](https://go.dev/dl/), 下载`*.msi`安装包, 一路 Next 即可. 如果习惯使用包管理器`Scoop`或者`Chocolatey`, 也可以通过执行以下命令来安装

```bash
scoop install go
# 或者
choco install golang
```

1. **Linux系统**: 官方推荐使用`snap`安装, 会自动处理依赖, 该步骤可跳过.

### 安装Git

1. **Windows系统**: 前往[Git 官网](https://git-scm.com/install/windows), 根据系统架构下载合适的安装包, 运行即可. 或使用包管理器:

```bash
winget install Git.Git
# 或者
scoop install git
# 或者
choco install git

```

1. **Linux系统**: 根据你的发行版执行对应命令:

```bash
# 基于Debian
sudo apt update && sudo apt install git
# 基于Fedora
sudo dnf update && sudo dnf install git
# 基于Arch
sudo pacman -Syu git
```

## 搭建 Hugo 博客环境

### 安装 Hugo 本体

与上面类似, 我们依然可以通过安装包或者包管理器安装 Hugo, 需要注意一点, Hugo本身不带样式, 需要安装主题, 而*部分主题依赖于Extended版的Hugo*, 所以在这一步中, **推荐安装Extended版本的Hugo**

1. **Windows环境**: 前往[Hugo的Git仓库](https://github.com/gohugoio/hugo/releases), 下载Extended版本的Hugo即可. 或使用包管理器快速安装:

```bash
winget install Hugo.Hugo.Extended
# 或者
scoop install hugo-extended
# 或者
choco install hugo-extended
```

1. **Linux环境**: 使用snap安装<!--, 如果没有安装snap, 则请查看[这一篇文章]() -->

```bash
sudo snap install hugo --channel=extended
```

安装完成后, 在终端里运行`hugo version`, 只要输出版本号信息, 就说明大功告成!

### 初始化项目

找一个合适的目录, 进入终端执行以下命令:

```bash
hugo new site my-blog
cd my-blog
git init
```

此时, Hugo 已经生成了标准的目录结构:

```text
my-blog/
├── archetypes # 文章默认模板
│   └── default.md
├── assets/ # 资源目录
├── content/ # 博客文章存放目录 ← 最重要
├── data/
├── hugo.toml # 主配置文件 ← 最重要
├── i18n/ # 语言翻译
├── layouts/ # 自定义布局, 优先级高于主题
├── static/ # 静态资源(图片, 字体等)
└── themes/ # 主题目录

9 directories, 2 files
```

### 安装与配置主题

Hugo 本身不带样式, 需要安装主题. 这里以极简风格的`Terminal`主题为例:

将主题作为 Git 子模块引入(强烈推荐这种方式, 方便后续更新):

```bash
git submodule add https://github.com/panr/hugo-theme-terminal.git themes/terminal
```

接着, 打开根目录下的`hugo.toml`配置文件, 指定我们刚刚安装的主题, 并顺便修改一下基础配置:

```toml
baseURL = "http://localhost:1313" # 沿用默认的本地调试地址
defaultContentLanguage = "zh-cn" # 博客语言
title = "MowMark 的技术博客" # 你的博客名称
theme = "terminal" # 必须与 themes 目录下的文件夹名称一致
```

到这一步, 博客的底层搭建已经完成了99%

## 本地预览

在项目根目录下执行:

```bash
hugo server -D # -D 参数表示渲染草稿文章(draft: true)
```

> `-D` 参数表示渲染草稿文章(draft: true)

> `hugo server` 对应的常用参数还有 `--port 端口号`, `--baseURL 域名`等, 具体参数可以查看[hugo server](https://hugo.opendocs.io/commands/hugo_server/)

打开浏览器, 访问`http://localhost:1313`, 就能看到个人博客已经在本地成功运行了!

当然, 在本地运行是不够的, **如何将它部署到公网上, 让所有人都能访问**?

这就需要用到GitHub Pages 和自动化的构建工具了.
由于篇幅原因, 这部分内容我会在[使用GitHubPages部署博客](../001_GitHubPages)中讲解.
