---
layout: post
title: Git 与 Gitee/Github 结合使用：从安装到代码提交全流程
categories: Git
description: 在第一次使用gitee交作业中，了解使用gitee
keyword: Git, ssh
---

在日常开发中，Git 是必不可少的版本控制工具，Gitee（码云）则是国内稳定高效的代码托管平台。本文将带你一步步完成 **Git 安装、SSH 配置、Gitee 仓库创建、本地代码提交与推送**，实现本地与远程仓库的无缝协作。同样适用于Github。

## 一、Git 安装与环境配置

### 1.1 下载安装 Git

访问 Git 官网下载对应系统版本：[https://git-scm.com/download](https://git-scm.com/download)  
Windows 版本直接双击安装包，一路**下一步**即可完成安装。

### 1.2 验证安装与环境变量配置

安装完成后，按下 `Win + R` 打开 cmd，输入以下命令验证：

git --version  

若显示版本号，说明安装与环境变量配置成功。

若提示 “git 不是内部或外部命令”，手动配置环境变量：

1.  找到 Git 安装目录（如 `D:\Program Files\Git`）
2.  复制 `cmd` 文件夹完整路径（`D:\Program Files\Git\cmd`）
3.  右键此电脑 → 属性 → 高级系统设置 → 环境变量
4.  编辑系统变量 `Path`，新建并粘贴上述路径，保存后重新打开 cmd 验证。

## 二、配置 SSH 公钥（免密推送必备）

使用 SSH 协议可避免每次推送代码输入账号密码，提升效率。

### 2.1 生成 SSH 公钥

桌面右键选择 **Git Bash Here**，打开 Git 命令行，执行以下命令（邮箱替换为自己的，非必须）：

ssh-keygen -t ed25519 -C "your\_email@example.com"  

连续回车，默认保存路径无需修改，公钥默认生成在 `C:\Users\用户名\.ssh` 目录下。

### 2.2 配置 Gitee SSH 公钥

1.  打开 `.ssh` 文件夹，用记事本打开 `id_ed25519.pub`，复制全部内容
2.  登录 Gitee → 右上角头像 → 设置 → SSH 公钥
3.  将复制的公钥粘贴到输入框，添加并验证 Gitee 密码即可完成配置。

验证 SSH 连接：

ssh -T git@gitee.com  

出现成功提示，说明配置生效。

## 三、Gitee 远程仓库创建

1.  登录 Gitee，点击右上角 + 号 → 新建仓库
2.  填写仓库名称（路径自动生成），按需选择公开 / 私有
3.  无需勾选初始化 README、.gitignore 等文件，直接点击**创建**。

创建完成后，复制仓库的 **SSH 地址**（格式：`git@gitee.com:用户名/仓库名.git`），后续关联本地仓库使用。

## 四、本地仓库初始化与代码提交

### 4.1 配置 Git 全局用户信息

打开 Git Bash，执行以下命令配置用户名和邮箱（与 Gitee 账号一致）。如开启不公开邮箱设置，请使用隐藏后的形式 **……+……@user.noreply.gitee.com**：

git config --global user.name "你的Gitee用户名"  
git config --global user.email "你的Gitee绑定邮箱"  

`--global` 表示全局配置，所有仓库生效。

### 4.2 初始化本地仓库

进入本地项目根目录，右键打开 Git Bash，执行初始化命令：

git init  

执行后目录生成 `.git` 隐藏文件夹，标志本地仓库创建成功。

### 4.3 添加文件到暂存区

将项目文件添加到 Git 暂存区，两种常用方式：

\# 添加指定文件  
git add 文件名  
  
\# 添加当前目录所有文件（推荐）  
git add .  

### 4.4 提交到本地仓库

添加完成后，执行提交命令，`-m` 后为提交说明（必填，清晰描述本次修改）：

git commit -m "第一次提交：项目初始化"  

## 五、关联远程仓库并推送代码

### 5.1 关联远程 Gitee 仓库

将本地仓库与 Gitee 远程仓库绑定，替换为自己的仓库 SSH 地址：

git remote add origin git@gitee.com:用户名/仓库名.git  

可通过 `git remote -v` 验证关联是否成功。

### 5.2 推送代码到远程仓库

执行推送命令，将本地代码上传到 Gitee：

git push origin master  

-   `origin`：远程仓库别名
-   `master`：默认主分支（新 Gitee 仓库也可能是 `main` 分支，按实际修改）。

首次推送可能需确认连接，输入 `yes` 回车即可。推送完成后，刷新 Gitee 仓库页面，可看到本地提交的文件。

## 六、克隆远程仓库到本地

若需在其他设备获取 Gitee 代码，使用克隆命令：

git clone git@gitee.com:用户名/仓库名.git  

执行后自动在当前目录创建仓库文件夹并下载全部代码。

## 七、完整流程总结

1.  安装 Git 并配置环境变量
2.  生成并配置 Gitee SSH 公钥，实现免密登录
3.  Gitee 创建远程仓库，复制 SSH 地址
4.  配置 Git 全局用户名、邮箱
5.  本地项目执行 `git init` 初始化仓库
6.  `git add .` 添加文件 → `git commit -m` 提交本地
7.  `git remote add origin` 关联远程 → `git push origin master` 推送代码
8.  远程仓库克隆使用 `git clone` 命令

建议优先使用 SSH 协议，避免重复输入账号密码，提升开发效率。按照以上步骤，即可轻松完成 Git 与 Gitee 的结合使用，实现代码版本管理与远程托管。
