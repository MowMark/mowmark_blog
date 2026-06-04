---
title: "使用 Terminal 主题美化博客"
description: "Terminal 主题的修改方法" 
date: 2026-05-22T09:12:24+08:00
image: 
math: 
license: 
tags: ["Hugo", "建站", "博客"]
comments: true
draft: false
build:
    list: always    # Change to "never" to hide the page from the list
---

承接上一篇文章: [使用 GitHub Pages 部署博客]({{< ref "001_GitHubPages" >}}), 这篇文章我将分享使用 `Terminal` 主题美化博客的个人经验. 由于个人精力有限, 可能有一些地方并没有涉及到, 欢迎讨论交流.

## Hugo 配置文件的基本要求

在正式开始之前, 我们需要先梳理 Hugo 主题的基本结构, 而这又绕不开 Hugo 项目的核心机制: **配置文件的覆写机制**

### 配置文件的覆写机制

简单来说, 在新版Hugo中, **`config/_default/`目录下的配置文件优先级大于根目录下的配置文件大于主题目录(`themes/`)下的配置文件**, 此外, 主题目录如果随 `Git` 自动更新, 我们在里面做的任何修改都会被直接覆盖. 基于这两点, 如果我们要修改主题的配置, 就不能在主题目录下进行, 而要在根目录或者 `config/` 目录下操作.

### 配置文件的基本要求

除此之外, 配置文件的格式, 内容和作用域也需要厘清一下:

1. Hugo 早期要求配置文件名字应该为 `config`, 后来变成了 `hugo`, 目前版本是两种名字都可以接受, 而格式则可以为 `yaml`, `toml` 或者 `json` 当中的任意一种;
2. Hugo 支持将所有配置信息都写在同一个文件中, 也可以拆分开来, 作为不同的配置文件, 但名称必须为 `params`, `menu` 或者 `languages` 已经规定好的一级目录;
3. Hugo 项目的全局配置控制整个网站的样式, 如网站主标题, 导航菜单, 默认语言等, 这些信息都存储在 `hugo.toml` 或者从中拆分出来的配置文件里, 可能存放在根目录或者 `config/` 目录下;
4. 写在每篇文章头部的 `Front Matter` 控制这篇文章的元数据, 如标题, 日期, 标签等, 其优先级高于全局配置. 在使用 `hugo new ...` 命令新建文章时, Hugo 会根据存放在 `archetypes/` 目录下的文章模板, 自动创建新文章的 `Front Matter`.

基于上述几个理由, 读者在使用不同主题时, 有可能会在根目录或者 `config/` 目录下看到不同类型的配置文件

---

## Terminal 主题的配置

Hugo 官方对主题结构没有严格规定, 所以每个主题的组织方式都不太一样, 以下内容基于 `Hugo Extended v0.161.1` 和 `Terminal 2024-11-22`, 仅供参考.

### 复制初始文件

使用 `git submodule ...` 命令安装好主题后, 将 `themes/terminal/exampleSite/` 目录下的文件复制到根目录, 作为配置的初始模板. 作者给出的默认配置相当长, 但实际上大部分参数保持默认即可, 只有少数几个参数需要留意.

### 修改配置文件

打开根目录的 `hugo.toml`:

```toml
baseurl = "https://<Your-UserName>.github.io/"  # 改成你的实际域名
languageCode = "zh-cn"               # 当前 Hugo 规定的语言代码, 中文用户记得改这里
theme = "terminal"
paginate = 5

[params]
  # 这个参数对应 content/ 目录下存放文章的文件夹名称
  # 如果你的文章放在 content/blog/ 下，这里就要改成 "blog"
  # 不改的话首页会加载不出任何文章，是比较容易踩的坑
  contentTypeName = "posts"

  # 首页显示的菜单项数量，超出的部分会折叠进右上角
  # 如果你想让页面干净一点，设成 0 也没问题
  showMenuItems = 2
```

---

## 实际修改主题文件

前面提到不能在主题目录里改文件, 那如果我们确实需要修改主题的某个模板, 应该怎么做?

以修改页脚为例. 假设你想在底部加上备案号, 或者就是单纯不喜欢默认的版权格式, 直觉上可能会去 `themes/terminal/layouts/partials/footer.html` 里改, 但这样做下次主题更新就会丢失.

正确的做法是:

1. 在根目录下创建相同的路径 `layouts/partials/`;
2. 把 `themes/terminal/layouts/partials/footer.html` 复制到这个目录下;
3. 修改根目录下的这个副本.

Hugo 在构建时会优先读取根目录下的 `layouts/` 目录, 主题里的原版文件不会被动, 下次更新主题也不会影响你的改动. 其他模板文件的修改方式都是一样的道理.

---

## 小结

这篇文章到这里基本就结束了. Terminal 主题本身并不复杂, 配置文件改几个参数就能跑起来, 真正需要花时间理解的反而是 Hugo 本身的复写机制, 这个搞清楚了, 后续不管换什么主题, 修改起来都会顺手很多.

如果读者想要使用 Stack 主题, 后面我会更新相关文章, Stack 主题的结构比 Terminal 复杂不少, 需要花费比较多的时间进行调试.
