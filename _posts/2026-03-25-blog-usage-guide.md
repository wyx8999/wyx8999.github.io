---
layout: post
title: "博客使用教程"
date: 2026-03-25 21:00:00 +0800
categories: 博客教程
tags: Jekyll GitHubPages 使用教程 博客搭建
---

# wyx8999.github.io 博客使用完全指南（新增/修改/配置一站式教程）

## 一、博客介绍
本博客基于 **Jekyll + GitHub Pages** 构建，是纯静态博客。  
所有内容通过修改仓库文件实现，**无需后台、无需数据库、改完提交即生效**。  

你只需要会：新建文件、编辑文件、提交，就能完全掌控整个博客。

---

## 二、核心文件路径（必须记住）
所有功能都对应固定的文件，记住这 5 条，博客你随便改：

| 功能 | 文件路径 |
|------|---------|
| 新增/修改文章 | `_posts/` |
| 修改顶部导航 | `_data/navigation.yml` |
| 修改独立页面 | `pages/` |
| 修改网站标题/头像/昵称 | `_config.yml` |
| 上传图片/视频 | `images/`、`assets/` |

---

## 三、新增一篇博客文章（最常用）
### 1. 进入仓库
打开 `wyx8999.github.io` → 进入 `_posts` 文件夹

### 2. 新建文件
点击 `Add file` → `Create new file`

### 3. 文件名格式
```plaintext
YYYY-MM-DD-文章标题英文或拼音.md
示例：2026-03-25-my-first-blog.md
