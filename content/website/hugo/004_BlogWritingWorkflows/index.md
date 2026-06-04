---
title: "博客写作工作流"
description: "日常撰写博客文章的工作流以及版本控制"
date: 2026-05-22T09:13:11+08:00
image: 
math: 
license: 
comments: true
draft: true
build:
    list: always    # Change to "never" to hide the page from the list
---

## 博客写作的日常工作流

网站发布后，你未来的日常任务就只剩下纯粹的“写作与发布”了。

### 1. 创建文章

推荐使用**页面捆绑包 (Page Bundles)** 的方式创建文章，这会让图片管理变得非常轻松：

```bash
# 这会生成一个包含 index.md 的同名文件夹
hugo new content/posts/my-first-post/index.md
```

如果你有图片要插入，直接把图片（比如 `pic.png`）丢到 `my-first-post` 文件夹里，然后在 `index.md` 中这样引用即可，非常直观：

```markdown
![这是一张演示图](pic.png)
```

### 2. 撰写与发布

你可以使用任何你喜欢的编辑器（Vim, Nano 甚至 VSCode ）打开 `index.md` 进行写作。
文章写好并在本地使用 `hugo server -D` 预览无误后，只需几步即可发布到公网：

1. 将文章顶部的 `baseURL` 和 `draft: true` 分别修改为你的域名和 `draft: false` (解除草稿状态).
2. 执行 `git add .`
3. 执行 `git commit -m <commit>`
4. 执行 `git push` 推送到 GitHub。

剩下的编译和发布工作，GitHub Actions 都会默默帮你在一分钟内自动搞定。

开始享受自由写作的乐趣吧！
