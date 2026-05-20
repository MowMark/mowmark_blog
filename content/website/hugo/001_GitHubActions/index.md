---
title: "使用GitHub Actions发布个人博客"
description: "如何使用GitHub Actions部署静态网站"
date: 2026-05-14T22:01:19+08:00
image: 
math: 
license: 
comments: true
draft: true
build:
    list: always    # Change to "never" to hide the page from the list
---

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



