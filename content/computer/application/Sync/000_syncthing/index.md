---
title: "使用Syncthing进行文件同步"
description: ""
date: 2026-07-14T14:18:28+08:00
image:
math:
license:
tags: ["文件同步", "Syncthing", "Sync"]
comments: true
draft: false
build:
    list: always    # Change to "never" to hide the page from the list
---

## 什么是Syncthing?

简而言之, Syncthing 是一款专门用于文件同步的开源软件. 它自主设计了一套名为 BEP (Block Exchange Protocol) 的协议, 并基于此搭建了 P2P 架构, 每一端都能独立进行文件的发送与接收.

需要说明的是, 文件内容本身是点对点直连传输的. 但两台设备要"找到彼此", 默认还是依赖官方维护的 discovery server (设备发现) 和 relay server (NAT 穿透失败时的中继). 这两个服务器不经手你的文件内容, 只负责牵线搭桥. 如果你对隐私要求极高, 也可以自建这两个服务, 但对大多数人来说没有必要.

## 为什么要使用 Syncthing?

日常跨设备同步文件, 常见的做法无非是丢云盘或插移动硬盘倒来倒去. 但这两种方式各有各的问题: 云盘要经过第三方服务器, 大文件同步慢, 免费额度有限; 移动硬盘完全依赖人工操作, 谈不上"同步".

Syncthing 的优势在于:

- **去中心化**: 不经过任何第三方存储, 文件只在你自己的设备之间流转.
- **增量同步**: 只传输变化的部分, 不用每次全量拷贝.
- **跨平台**: Windows, Linux, macOS, Android 都有对应客户端.
- **开源免费**: 没有容量限制, 也不用担心某天服务商跑路.

如果你有多台常驻设备 (比如台式机 + 笔记本 + NAS), 且不想依赖云盘, Syncthing 是一个相当稳妥的选择.

## 步骤

1. **下载**: 前往 [Syncthing](https://syncthing.net/downloads/) 下载适合的架构版本, 解压或安装运行.
    - 如果你的发行版软件源收录了 Syncthing, 也可以用包管理器安装 (比如 Ubuntu/Debian: `sudo apt install syncthing`). 好处是系统自带了 systemd 服务模板, 后面开机启动省事很多. 坏处是版本会落后一些.
> Windows 用户直接从官网下载安装包, 双击安装即可, 后续的 Web UI 操作和 Linux 完全一致, 不再赘述.

2. **打开Web UI**: 启动后, 进入浏览器, 访问`http://localhost:8384`.

3. **添加设备**: 点击"添加远程设备", 填入另一台设备的 Device ID. Device ID 可以在对方的 Web UI 里点击右上角"操作 → 显示 ID"查到, 是一长串字符或者一个二维码. 添加之后, 需要在两台设备上分别完成这个操作 (互相添加对方), 并在对方的界面上手动接受连接请求, 双方才算配对成功.

4. **添加并共享文件夹**: 点击"添加文件夹", 填写文件夹路径和文件夹标签 (Folder ID). 标签是两台设备之间识别同步关系的唯一标识, 两边必须一致. 保存后, 记得在文件夹的"共享"选项卡中勾选刚才配对好的目标设备. 对方会收到一条共享邀请, 接受后选择本地存放路径, 同步才会真正开始. 只加设备不共享文件夹, 或者加了文件夹不勾目标设备, 都不会触发任何同步.

在另一台电脑上重复上述操作即可.

## 设置开机启动

Linux 通过 systemd 管理系统后台服务, 控制程序何时启动、以什么身份运行、异常退出后如何处理. Windows 则通过系统服务和任务计划程序管理. 下面按平台和安装方式分别说明.

### 通过包管理器安装 (Linux)

如果你是用 `apt` / `dnf` 等包管理器安装的, 系统已经自带了 `syncthing@<user>.service` 模板服务, 一条命令搞定:

```bash
sudo systemctl enable --now syncthing@<user>.service
```

不需要手写 unit 文件, 手动安装的读者请继续往下看.

### 手动安装 (Linux)

如果你是手动下载的二进制文件, 才需要走完下面完整的流程:

1. **创建软链接**: 有些教程在 `/usr/bin/` 下创建软链接, 但那是发行版包管理器管辖的目录, 自己放文件容易和系统更新冲突. 更推荐放在 `/usr/local/bin/` 下

```bash
sudo ln -s <你的Syncthing可执行文件的绝对路径> /usr/local/bin/syncthing
```

2. **编辑 service 服务文件**:

```bash
sudo vim /etc/systemd/system/syncthing.service
```

3. **粘贴如下内容**:

```
[Unit]
Description=Syncthing - Open Source Continuous File Synchronization
After=network.target

[Service]
User=<user>
ExecStart=/usr/local/bin/syncthing
Restart=on-failure
SuccessExitStatus=3 4
RestartForceExitStatus=3 4

[Install]
WantedBy=multi-user.target
```

> `Restart=on-failure`: 服务异常退出时自动重启.
>
> `SuccessExitStatus=3 4` + `RestartForceExitStatus=3 4`: Syncthing 自动升级或手动重启时以退出码 3 或 4 退出. 不加这两行, systemd 会把正常重启当成崩溃, 触发不必要的报错.
>
> `WantedBy=multi-user.target`: 在多用户模式下启动.

记得把 `User` 换成你自己的用户名, 用 root 跑 Syncthing 没有必要, 也不安全.

4. **重新加载 daemon**:

```bash
sudo systemctl daemon-reload
```

5. **设置开机启动**:

```bash
sudo systemctl enable syncthing
```

6. **手动启动**:

```bash
sudo systemctl start syncthing
```

如果是包管理器安装的, 则用 `sudo systemctl start syncthing@<user>.service` 启动.

完成后, 用 `sudo systemctl status syncthing` (或 `syncthing@<user>`) 确认服务为 `active (running)` 状态即可.

### 安装版 (Windows)

Windows 安装包默认将 Syncthing 注册为系统服务. 打开 `Win+R` 输入 `services.msc`, 找到 `Syncthing` 服务, 确认启动类型为"自动"即可.

### 便携版 (Windows)

便携版不会注册系统服务. 可以在任务计划程序中创建一条基本任务: 触发器选"当前用户登录时", 操作选"启动程序", 填入 `syncthing.exe` 的完整路径. 一般情况用安装版就够了.

## 补充说明

### 忽略不需要同步的文件

有些文件夹或文件类型不需要也不应该被同步. 在同步文件夹根目录创建 `.stignore`, 按行写入忽略模式, 语法和 `.gitignore` 类似. 一个常见的配置如下:

```
# 版本控制
.git/

# 依赖目录
node_modules/
__pycache__/

# 编译产物
*.o
*.class

# 临时文件
*.tmp
*.swp
~*
```

配置后 Syncthing 会自动跳过这些路径, 不再传输和扫描.

### 文件版本控制

日常使用中误删或误覆盖的情况难以完全避免. 在 Web UI 中点击对应文件夹 → 编辑 → File Versioning, 可以启用版本控制. 推荐使用 `Simple File Versioning`, 设置保留最近 N 个旧版本. 当文件被修改或删除时, 旧版本会被保留下来, 而不是被彻底丢弃.

### 冲突处理

两台设备同时在同一个文件上做了修改, Syncthing 不会静默覆盖任意一方. 它会保留最后一次同步的版本作为基础, 另一端的修改另存为冲突副本. 副本命名格式为 `文件名.sync-conflict-YYYYMMDD-HHMMSS.ext`. 你发现冲突后手动核对两个版本的内容, 保留正确的, 删除多余的那个即可.

### 纯局域网模式

如果所有设备都在同一个局域网内, 不需要经过公网的 discovery 和 relay 服务器. 进入 `操作 → 高级 → 连接`, 取消勾选 `Global Discovery` 和 `Enable Relaying`, 只保留 `LAN Discovery`. 这样 Syncthing 只会在局域网内广播和连接, 不走公网, 速度更快, 隐私性也更高.
