# 基于 Jekyll 构建的个人博客模板

Fork 自 [mzlogin/mzlogin.github.io](https://github.com/mzlogin/mzlogin.github.io) 并完成 **98 项个性化功能迭代**，适配 GitHub Pages 一键部署，支持博客文章、维基知识库、碎片笔记、图片画廊等多形态内容展示，已稳定运行并持续更新。 

博客线上地址：[https://zxy6.indevs.in](https://zxy6.indevs.in)  
欢迎 Star / Fork 二次开发，保留原作者协议即可。

---

## ✨ 核心特性

- **多内容形态支持**：博客文章（`_posts`）、草稿（`_drafts`）、维基知识库（`_wiki`）、技术碎片笔记（`_fragments`）、图片画廊（`gallery`）
- **个性化定制**：支持首页横幅背景自定义、侧边栏二维码配置、自定义导航链接、CNAME 域名绑定
- **性能优化**：集成 jsDelivr CDN 加速、静态资源自动压缩、搜索数据按需刷新
- **开发友好**：本地实时预览、Markdown 语法全支持、代码高亮、UTF-8 中文无乱码
- **扩展功能**：访问统计、文章分类 / 标签、社交媒体分享、自定义侧边栏组件
- **部署便捷**：适配 GitHub Pages 自动构建，无需额外服务器，支持自定义域名

---

## 🛠 技术栈

| 技术       | 占比    | 用途                         |
|------------|---------|------------------------------|
| HTML       | 65.9%   | 页面结构与组件开发           |
| CSS        | 23.3%   | 页面样式与响应式布局         |
| JavaScript | 10.6%   | 交互功能与动态效果           |
| Ruby       | 0.2%    | Jekyll 构建与依赖管理        |

- **核心框架**：Jekyll  
- **依赖管理**：Bundler  
- **CDN 加速**：jsDelivr  
- **静态部署**：GitHub Pages  

---

## 🚀 快速开始

### 1.  部署上线
方式 1：GitHub Pages 自动部署（推荐）

将项目推送到自己的 GitHub 仓库，仓库名必须为 [你的用户名].github.io

进入仓库「Settings」→「Pages」，选择「main」分支作为构建源，点击「Save」

等待 1-5 分钟，博客即可通过 https://[你的用户名].github.io 访问

方式 2：自定义域名部署

修改项目根目录的 CNAME 文件，替换为你的自定义域名（如：zxy6.indevs.in）

在域名解析后台添加「CNAME 记录」，指向 [你的用户名].github.io

按照方式 1 完成 GitHub Pages 部署，等待解析生效即可

方式 3：本地构建后部署到其他服务器

# 本地构建生成静态文件，输出到 _site 目录
bundle exec jekyll build
将 _site 目录下的所有文件上传到任意支持静态网站的服务器（如 Nginx、Apache、Netlify 等）。

📁 目录结构
核心目录 / 文件说明，其余为 Jekyll 默认配置，无需修改：

wyx8999.github.io/
├── _data/          # 静态数据配置（导航链接、侧边栏等）
│   └── links.yml   # 自定义导航链接配置
├── _drafts/        # 博客草稿（未发布文章）
├── _fragments/     # 技术碎片笔记（短知识点、解决方案）
├── _includes/      # 可复用组件（侧边栏、二维码、头部/底部）
├── _layouts/       # 页面模板（博客、维基、画廊、首页等）
├── _posts/         # 正式博客文章（Markdown格式，命名规范：YYYY-MM-DD-title.md）
├── _wiki/          # 维基知识库（结构化技术文档、学习笔记）
├── assets/         # 静态资源（CSS、JS、字体、图标等）
├── images/         # 图片资源（文章配图、画廊图片、头像等）
├── pages/          # 自定义页面（关于页、相册页、分类页等）
├── _config.yml     # 博客核心配置（必改，含个人信息、功能开关）
├── CNAME           # 自定义域名配置文件
├── Gemfile         # Ruby 依赖配置
└── index.html      # 博客首页（支持横幅背景自定义）
✏️ 内容创作指南
1. 发布博客文章
在 _posts 目录下创建 Markdown 文件，文件名必须遵循规范：

text
YYYY-MM-DD-文章标题.md
文件头部添加 YAML 前置配置（必填，示例）：

---
layout: post
title: "博客使用指南"
subtitle: "wyx8999.github.io 快速上手"
date: 2026-03-25
author: wyx8999
header-img: /images/bg-post.jpg
catalog: true
tags:
  - Jekyll
  - GitHub Pages
  - 博客搭建
categories:
  - 技术教程
---

# 文章正文
这里使用 Markdown 语法编写正文，支持代码高亮、图片、表格等。
2. 编写维基知识库
在 _wiki 目录下创建 Markdown 文件，用于整理结构化、长文型技术文档（如：Python 学习笔记、前端开发手册），支持分类和索引，配置与博客文章一致。

3. 记录碎片笔记
在 _fragments 目录下创建 Markdown 文件，用于记录简短技术知识点、问题解决方案、开发小技巧，自动生成时间线展示，无需复杂配置，仅需添加标题和正文。

4. 新增自定义页面
在 pages 目录下创建 Markdown/HTML 文件，如 photos.md（相册页），通过修改 _config.yml 配置导航链接，即可在首页顶部菜单展示。

⚙️ 核心配置（_config.yml）
所有个性化配置均在项目根目录 _config.yml 文件中。

🐛 常见问题解决
1. 本地启动失败
检查 Ruby 版本是否 ≥2.5.0，低于版本请升级

执行 bundle update 更新依赖包

确保项目目录无中文 / 特殊字符

2. 中文显示乱码
确保所有 Markdown 文件编码为 UTF-8

检查 _config.yml 中已配置 encoding: "utf-8"

3. 图片无法加载
图片路径使用相对路径，如：/images/xxx.jpg（根目录为项目目录）

确保图片文件存在于 images 目录下，无大小写错误

4. CDN 加速不生效
检查 _config.yml 中 cdn.jsdelivr.enabled: true

静态资源路径使用 {{ assets_base_url }} 变量（模板已默认配置）

5. 自定义域名无法访问
确认 CNAME 文件中的域名无拼写错误

域名解析的 CNAME 记录已生效（可通过 nslookup 你的域名 验证）

等待 GitHub Pages 缓存刷新（约 10 分钟）

📄 许可证
本项目基于 MIT License 开源，详见项目根目录 LICENSE 文件。
Fork / 二次开发时，请保留原作者信息和许可证协议。


text
