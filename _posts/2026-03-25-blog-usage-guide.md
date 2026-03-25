---
layout: post
title: 博客使用指南
categories: 教程
tags: 教程
topmost: true
---

# 一、博客介绍
本博客基于 **Jekyll + GitHub Pages** 构建，是纯静态博客。
所有内容通过修改仓库文件实现，**无需后台、无需数据库、改完提交即生效**。

你只需要会：新建文件、编辑文件、提交，就能完全掌控整个博客。

---

# 二、核心文件路径（必须记住）
所有功能都对应固定的文件，记住这几条条，博客你随便改：

- 新增/修改文章 → `_posts/`
- 未发布的博客文章 → `_drafts`
- 已发布的 wiki 页面 → `_wiki`
- 已发布的短文片段 → `_fragments`
- 修改独立页面 → `pages/`
- 修改网站标题/头像/昵称 → `_config.yml`
- 修改「关于」页面 → `pages/about.md`
- 上传图片/视频 → `images/`、`assets/`
- 页面模板 → `_layouts`
- 界面设计 → `pages`

---

# 三、新增一篇博客文章（最常用）

## 1. 进入仓库
打开 `wyx8999.github.io` → 进入 `_posts` 文件夹

## 2. 新建文件
点击 `Add file` → `Create new file`

## 3. 文件名必须严格按格式
**YYYY-MM-DD-文章标题英文或拼音.md**例：2026-03-24-my-first-blog.md

下面用 **Markdown**写正文

 # 四、新增一篇文章后，会自动出现在哪里？
只要你在 _posts/ 里新建了文章，系统会自动把它放到这些位置，你完全不用手动搬：

- 首页（最顶部）最新文章永远排在博客首页第一条
- 文章列表 / 归档（Archives）所有文章按时间倒序全部显示
- 分类页面（Categories）按你写的 categories: 自动归类
- 标签页面（Tags）按你写的 tags: 自动归类

也就是说：你只管写文章，位置自动生成 ✅
