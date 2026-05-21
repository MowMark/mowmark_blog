---
title: "使用 GitHub Pages 发布个人博客"
description: "使用 GitHub Pages 服务和 GitHub Actions 部署个人博客"
date: 2026-05-19T22:01:19+08:00
image: 
math: 
license: 
# comments: true
draft: false
tags: ["Hugo"]
build:
    list: always    # Change to "never" to hide the page from the list
---

> 承接自上一篇文章: [使用Hugo创建个人博客](../000_HugoInit), 本文讲解 GitHub Pages 和 GitHub Actions 的概念与使用方法

上一篇文章中, 我们讲到了本地预览, 现在我们开始讲如何发布到公网上

## 网络发布

想要把网站发布到网络上, 需要满足几个条件:

1. 需要有托管代码平台, 一般是 GitHub, Gitee或者GitLab
2. 需要有自己的域名
3. 需要有托管服务

  > 其中, 域名可以从 GitHub 免费获取, 也可以从腾讯云, 阿里云等平台购买
  > 而托管服务则由 GitHub Pages 免费提供, 但也可以使用 Cloudflare Pages, 或者腾讯云和阿里云的, 后两者用的人不多, 喜欢挑战自己的伙伴可以尝试部署一下.

鉴于 GitHub 提供的一站式服务, 我更倾向于使用 GitHub 仓库 + GitHub Pages 发布到 GitHub 提供的域名上. 如果采用了其他公司提供的某些服务, 本文仅作参考, 不保证适用.

### 创建 GitHub 仓库

首先登录自己的[GitHub账号](https://github.com), 新建仓库, 设置权限为允许公开访问(`public`), 命名为`用户名.github.io`, 后续可以通过`https://用户名.github.io/`域名访问,

这里需要注意一点, 如果仓库命名格式不符合`用户名.github.io`的话, 后面访问博客的域名应该是`https://用户名.github.io/仓库名`, 详细信息可以查看[GitHub Pages 说明](https://githubdocs.cn/en/pages/getting-started-with-github-pages/what-is-github-pages)

### 上传 GitHub 仓库

如果前面有按照我的上一篇文章进行操作的话，到这一步为止, 本地项目应该是已经完成了`git init`， 所以接下来就是设置Git的配置邮箱和用户名即可:

```bash
# 在项目根目录下执行: 
git config user.email 邮箱
git config user.name 用户名
```

这里并不需要核验邮箱的真假, 所以随便填即可, 这种情况下的配置信息会存放在本地项目根目录下的 `.gitconfig` 文件中, 需要注意的另外一点是 `--global` 参数代表全局设置, 对应的配置信息存放在 `~/.gitconfig` 或 `%USERPROFILE%/.gitconfig` 文件中

然后将当前项目的所有文件加入暂存区并添加评论:

```bash
git add .
git commit -m "初始化仓库"
```

随后将本地项目与 GitHub 仓库关联起来, 并推送:

```bash
# 关联本地项目和远程仓库
git remote add origin https://github.com/your-username/your-repository.git
# 或者
git remote add origin git@github.com:your-username/your-repository.git
# 上面两种方式等价
# 将本地代码推送到远程仓库的 main 分支, 并设为默认推送分支
git push -u origin main
```

> **提示**: 这个部分其实网上有两种方案, 一是将整个项目根目录全部 `push` 到远程仓库, 也就是我在这篇文章中使用的方法, 二是在 `hugo` 编译生成 `public/` 目录后才将 `public/` 目录 `push` 到远程仓库, 两种方法其实都是可以接受的, 毕竟笔者的目的仅仅是为了部署个人博客, 记录一些自己平时的学习与开发经验.

### 使用 GitHub Pages 手动部署

这一步开始, 我们才开始正式使用 GitHub Pages 服务, 进入仓库的 `Settings` → `Pages`, 在 `Branch` 选项中选择 `main` 分支, 保存即可. GitHub就会自动帮你部署网站.

![GitHub Pages 手动部署](GitHub_Pages.png)

稍等一会就能看到网站的效果, 一般与本地预览的效果没有差别.

### 使用 GitHub Actions 自动化发布

1. 创建工作流

2. 使用 GitHub Actions 自动部署网站

## 编辑与发布文章

### 创建文章

此时, 在项目根目录下执行以下命令就能创建自己的第一篇文章:

```bash
hugo new content/posts/my-first-post.md
```

如果要在文章中插入图片, 则建议使用如下命令

```bash
hugo new content/posts/my-first-post/index.md
# 图片存放在 content/posts/my-first-post/ 目录下
# 在 index.md 中引用同级图片 pic.png
[name](pic.png)
```

### 编辑文章

在终端中执行以下命令:

```bash
vim content/posts/my-first-post.md
```

按下`i`键即可编辑文章, 适时输入`:w`(英文符号)及时保存, 同时可以在浏览器中查看文章的渲染状态

### 发布文章

文章写好并检查无误后, 可以将文章的`draft`字段改为`false`就会自动发布.

## 版本控制
