---
title: "关于我"
# description: "MowMark"
date: 2026-05-15T13:48:00+08:00
image: 
math: 
license: 
# comments: true
draft: false
build:
    list: always    # Change to "never" to hide the page from the list
---

> 本博客由**MowMark**搭建, 在 **[**Liu Houliang**的Stack主题模板](https://github.com/liu-houliang/hugo-stack-starter)** 上进行了一定改动, 暂未定型.

这里是**MowMark**, 一名在读研究生, 喜欢折腾, 学习机器人算法中, 可能扩大折腾范围.

## 更新计划

- [ ] IBus Fcitx5 输入法及 Rime 的使用方法
- [ ] Ventoy 的使用方法
- [ ] Bash, Csh, Zsh
- [ ] Vim, Neovim
- [ ] v2ray, v2rayA 及 v2rayN
- [ ] Clash, Clash For Windows, Clash Verge Rev
- [ ] CherryStudio
- [ ] OpenCode

## 更新中内容

- [x] [Hugo 个人博客搭建指南]({{< ref "/website/hugo" >}})
- [x] [双系统安装指南]({{< ref "/computer/linux" >}})
- [x] [多机协同工作方案]({{< ref "/computer/hardware/000_MultipleComputer" >}})
<!-- README.md>

# MowMark 的个人博客

## 博客介绍

这是一个基于 [Hugo](https://gohugo.io/), [Hugo Theme Stack](https://github.com/CaiJimmy/hugo-theme-stack) 和  构建的个人博客系统。

### 🌟 核心特性与架构

本博客在原生 Stack 主题的基础上进行了一系列深度定制，主要特点包括：

- **自动化侧边栏路由**：支持无限扩展的顶级侧边栏导航，自动识别目录下子文件夹生成“分类卡片”。
- **全自动更新日期 (Lastmod)**：打通 Git 提交记录，自动根据文件的最后一次 `git commit` 生成并展示“更新于”标签。
- **智能标签提取**：无需手动在 Markdown 中写入 `categories`，系统自动根据文章所在的物理文件夹名称生成分类标签。
- **紧凑型归档列表**：重写了二级分类目录的展示逻辑，完美复刻年度归档页的 UI 设计。

### 📁 核心目录结构

本项目对内容（Content）进行了模块化切分，并非所有文章都挤在默认的 `post` 目录下。

```text
.
├── config/_default/       # Hugo 多文件配置目录 (config.toml, params.zh.toml, menu.zh.toml 等)
├── content/
│   ├── post/              # 默认文章目录
│   ├── os/                # 自定义顶级板块：操作系统
│   ├── robot/             # 自定义顶级板块：机器人
│   └── website/           # 自定义顶级板块：网站
├── layouts/               # 本地主题覆写目录 (Theme Overrides)
│   ├── os/list.html       # 定制的二级归档列表模板
│   ├── _partials/         # 覆写的原生组件 (例如修改标签颜色、添加更新时间的 details.html)
│   └── ...
└── public/                # 编译后自动生成的静态文件 (无需手动修改)
```

### 🛠️ 新增顶级侧边栏板块 (SOP)

如果你想要在左侧导航栏新增一个板块（例如：人工智能 `AI`），请严格遵循以下 4 步标准操作流程：

1. 创建目录与索引文件
在 `content/` 目录下创建 `ai/` 文件夹，并在其中新建一个极其简单的 `_index.md`：

```markdown
---
title: "人工智能"
---
```

> ⚠️ **注意：** 绝对不要写 `layout: archives`，我们会用本地覆写的模板来渲染它。

1. 复制归档列表模板
在根目录下的 `layouts/` 文件夹中新建对应目录并复制模板：

```bash
mkdir -p layouts/ai
cp layouts/os/list.html layouts/ai/list.html
```

这一步利用了 Hugo 的路由机制，使得该顶级板块完美继承定制的“紧凑型分类卡片 + 年份归档”样式。

1. 在配置文件中注册该板块
为了让该板块下的最新文章能够正常在“首页信息流”和“全站归档”中显示，必须修改参数配置。
编辑 `config/_default/params.zh.toml`（如有英文版也需同步修改）：

```toml
# 找到 mainSections 数组，加入你的新目录名称 (小写)
mainSections = ["post", "os", "robot", "website", "ai"]
```

1. 添加入口到侧边栏菜单
编辑 `config/_default/menu.zh.toml`，在文件末尾新增该导航项：

```toml
[[main]]
    identifier = "ai"
    name = "人工智能"
    url = "/ai/"
    weight = 5       # 决定它在菜单里的上下顺序
    [main.params]
      icon = "brain" # 图标名称 (需提前将 SVG 放入 assets/icons/ 目录下)
```

完成这四步后，新的侧边栏板块就搭建完毕了。你可以直接在 `content/ai/` 目录下建立子文件夹（例如 `content/ai/deep-learning/`），文章放在里面即可自动被系统识别出标签和分类。

### 📝 写作与部署指南

- 本地开发命令

```bash
# 启动本地开发服务器，-D 参数确保能够预览草稿 (Draft)
hugo server -D -p 1313
```

- 全自动 Lastmod (更新日期)
你**不需要**在 Markdown 头部手动维护 `lastmod: yyyy-mm-dd`。
系统已配置 `enableGitInfo = true` 以及前置 Git 解析优先级。你只需要正常修改文章并执行 `git commit`，列表页和文章标题下方就会自动渲染出最新的 `[更新于 YYYY年MM月DD日]` 标签。

- 部署方式
博客已配置 GitHub Actions (`.github/workflows/deploy.yml`)。
你只需要将 Markdown 源码 `git push` 到 `main` 分支，系统将自动使用 Hugo 进行云端构建，并将其发布至 GitHub Pages。

## 🙏 特别致谢

在搭建和调整本博客复杂的路由结构与主题重写逻辑时，特别感谢 **GitHub Copilot (OpenCode)** 以及其背后强大的 **Gemini 3.1 Pro** 模型的协作。它协助我厘清了 Hugo 的模块化缓存机制，完美复刻了 Stack 主题的归档组件，并实现了纯自动化的标签与更新日期系统。
