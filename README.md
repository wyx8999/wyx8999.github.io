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

### 1. 环境准备
确保本地安装以下工具（版本要求：Ruby ≥ 2.5.0）：

```bash
# 检查 Ruby 版本
ruby -v

# 安装 Bundler
gem install bundler
2. 克隆项目
bash
# 克隆仓库到本地
git clone https://github.com/wyx8999/wyx8999.github.io.git

# 进入项目目录
cd wyx8999.github.io
3. 安装依赖
bash
bundle install
4. 本地预览
bash
# 启动本地服务，实时预览
bundle exec jekyll serve
# 或简写
jekyll s
打开浏览器访问：http://127.0.0.1:4000，修改代码后浏览器强刷即可实时查看效果。

5. 部署上线
方式 1：GitHub Pages 自动部署（推荐）

将项目推送到自己的 GitHub 仓库，仓库名必须为 [你的用户名].github.io

进入仓库「Settings」→「Pages」，选择「main」分支作为构建源，点击「Save」

等待 1-5 分钟，博客即可通过 https://[你的用户名].github.io 访问

方式 2：自定义域名部署

修改项目根目录的 CNAME 文件，替换为你的自定义域名（如：zxy6.indevs.in）

在域名解析后台添加「CNAME 记录」，指向 [你的用户名].github.io

按照方式 1 完成 GitHub Pages 部署，等待解析生效即可

方式 3：本地构建后部署到其他服务器

bash
# 本地构建生成静态文件，输出到 _site 目录
bundle exec jekyll build
将 _site 目录下的所有文件上传到任意支持静态网站的服务器（如 Nginx、Apache、Netlify 等）。

📁 目录结构
核心目录 / 文件说明，其余为 Jekyll 默认配置，无需修改：

text
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

markdown
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
所有个性化配置均在项目根目录 _config.yml 文件中，必改项如下：

yaml
# 博客基本信息
title: 你的博客名称
subtitle: 博客副标题
description: 博客描述（SEO 友好）
url: https://[你的用户名].github.io  # 博客根地址
author: 你的昵称
email: 你的邮箱
location: 城市

# 导航菜单（对应 _data/links.yml）
navs:
  - title: 首页
    url: /
  - title: 博客
    url: /posts/
  - title: 维基
    url: /wiki/
  - title: 关于
    url: /pages/about/

# 功能开关
cdn:
  jsdelivr:
    enabled: true  # 开启 jsDelivr CDN 加速
busuanzi: true    # 开启不蒜子访问统计
search: true      # 开启文章搜索

# 首页定制
index:
  banner:
    background: # 首页横幅背景色/图片，如：#2f4154 或 /images/bg-index.jpg

# 评论系统（支持 giscus/gitalk/disqus）
comments_provider: giscus
giscus:
  repo: 你的仓库名
  repo_id: 仓库ID
  category: 评论分类
  category_id: 分类ID
其余配置项参考文件内注释，按需修改即可。

🎨 个性化定制
1. 自定义首页横幅
修改 _config.yml 中 index.banner.background，支持十六进制颜色值或图片路径：

yaml
index:
  banner:
    background: /images/bg-index.jpg  # 图片路径
    # 或
    background: #1E90FF  # 颜色值
2. 添加侧边栏二维码
修改 _includes/sidebar-qrcode.html 文件，替换二维码图片路径和描述，用于展示个人微信 / 公众号二维码。

3. 配置导航链接
修改 _data/links.yml 文件，添加 / 删除导航项，示例：

yaml
- title: 相册
  url: /pages/photos/
- title: 碎片
  url: /fragments/
4. 更换网站图标
替换项目根目录的 favicon.ico 文件，建议尺寸：32×32px，格式：ico/png。

5. 自定义代码高亮主题
修改 _config.yml 中 highlight_theme，支持 GitHub、Solarized Light/Dark 等主题。

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

🔧 版本更新
本项目在原模板基础上完成 98 项功能迭代，核心更新如下：

新增「图片画廊」功能（_layouts/gallery）

优化侧边栏组件，支持自定义二维码

新增碎片笔记（_fragments）模块，自动生成时间线

支持首页横幅背景自定义配置

优化搜索功能，按需刷新 search_data.json

完善图片资源管理，新增 pages/photos 相册页

适配最新 Jekyll 版本，修复部分兼容性问题

优化移动端响应式布局，提升阅读体验

持续更新中，所有更新记录见项目 Commit 历史。

💡 温馨提示
建议定期备份 _posts、_wiki、images 等核心内容目录

发布文章前建议本地预览，检查格式和链接是否正常

如需关闭 GitHub Actions 自动构建，可修改 .github 目录下的配置文件

若需添加广告，可修改 ads.txt 文件（适配 Google AdSense）

欢迎提交 Issue 反馈问题，或 Fork 后提交 Pull Request 贡献代码。

最后更新：2026-03-25
维护者：wyx8999

text
