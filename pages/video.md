---
layout: video
title: 视频展示
description: 我的私人视频集
keywords: 视频,影音
comments: false
copyright: false
menu: 视频
permalink: /video/
---

> 藏时光一隅，盼君轻启。

<style>
#content { display: none; }
/* 主布局：和照片/维基页统一 */
.video-container {
  margin-top: 20px;
  display: flex;
  gap: 30px;
  width: 100%;
}
.video-main {
  flex: 1;
}
/* 右侧边栏：完全匹配维基页Search+目录 */
.video-sidebar {
  width: 260px;
  flex-shrink: 0;
  font-size: 14px;
}
/* 搜索框：1:1匹配维基页 */
.video-search {
  margin-bottom: 20px;
}
.video-search label {
  display: block;
  font-weight: bold;
  margin-bottom: 8px;
  color: #333;
}
.video-search input {
  width: 100%;
  padding: 6px 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  outline: none;
  font-size: 14px;
}
/* 标签/合集标题 */
.tag-title, .album-title {
  font-size: 16px;
  font-weight: bold;
  margin: 20px 0 10px;
  color: #333;
}
/* 标签样式 */
.tag-list {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 15px;
}
.tag {
  padding: 4px 9px;
  background: #f3f3f3;
  border-radius: 12px;
  font-size: 13px;
  cursor: pointer;
}
.tag.active, .tag:hover {
  background: #555;
  color: #fff;
}
/* 合集列表：匹配维基页目录 */
.album-list {
  padding-left: 0;
  margin: 0;
  list-style: none;
}
.album-item {
  padding: 4px 0;
  cursor: pointer;
  font-size: 14px;
  color: #0645ad;
}
.album-item.active, .album-item:hover {
  text-decoration: underline;
  color: #0b0080;
}
/* 视频网格：修复画面显示不全问题 */
.video-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
}
.video-card {
  background: #fff;
  border-radius: 6px;
  overflow: hidden;
  box-shadow: 0 1px 4px rgba(0,0,0,0.05);
}
/* 关键修复：取消固定高度，让视频按原生比例显示 */
.video-card video {
  width: 100%;
  height: auto; /* 核心：自动适配比例，画面完整显示 */
  object-fit: contain; /* 确保画面不裁剪、不拉伸 */
}
.video-info {
  padding: 10px 12px;
  font-size: 14px;
  color: #333;
}
/* 密码弹窗 */
dialog {
  border: none;
  border-radius: 8px;
  padding: 25px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}
dialog::backdrop {
  background: rgba(0,0,0,0.4);
}
#errorTip {
  color: red;
  font-size: 12px;
  display: none;
  margin: 8px 0;
}
#passwordInput {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin: 10px 0;
}
.dialog-buttons {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}
.dialog-buttons button {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.dialog-buttons .confirm {
  background: #555;
  color: #fff;
}
</style>

<div id="content">
  <div class="video-container">
    <!-- 左侧视频展示区 -->
    <div class="video-main">
      <div class="video-grid" id="videoGrid"></div>
    </div>
    
    <!-- 右侧：Search + 标签 + 合集（和维基页对齐） -->
    <div class="video-sidebar">
      <!-- 搜索框 -->
      <div class="video-search">
        <label>Search</label>
        <input type="text" id="searchInput" placeholder="Search">
      </div>

      <!-- 标签（Search下方） -->
      <div class="tag-title">标签</div>
      <div class="tag-list" id="tagList"></div>

      <!-- 合集（标签下方） -->
      <div class="album-title">合集</div>
      <ul class="album-list" id="albumList"></ul>
    </div>
  </div>
</div>

<!-- 密码弹窗 -->
<dialog id="passwordDialog">
  <div>请输入密码查看视频</div>
  <div id="errorTip">密码错误！</div>
  <input type="password" id="passwordInput">
  <div class="dialog-buttons">
    <button onclick="closeDialog()">取消</button>
    <button class="confirm" onclick="verifyPassword()">确定</button>
  </div>
</dialog>

<script>
// 视频数据配置
const videos = [
  { src: "/assets/video/文明你我他1.mp4", title: "文明你我他", album: "生活", tags: ["日常","生活"] },
  { src: "/assets/video/冷酷的文儿.mp4", title: "冷酷的文儿", album: "人物", tags: ["人物","随拍"] },
  { src: "/assets/video/(玖贰陆（佛系更新）)🥯_780367cb71435e4823f91c8347435de1.mp4", title: "玖贰陆", album: "日常", tags: ["生活","随拍"] }
];

// DOM元素
const videoGrid = document.getElementById("videoGrid");
const tagList = document.getElementById("tagList");
const albumList = document.getElementById("albumList");
const searchInput = document.getElementById("searchInput");

// 筛选条件
let currentFilter = { album: "all", tag: "all", search: "" };

// 渲染视频（修复画面显示问题）
function renderVideos() {
  videoGrid.innerHTML = "";
  // 筛选逻辑
  const filteredVideos = videos.filter(video => {
    const matchAlbum = currentFilter.album === "all" || video.album === currentFilter.album;
    const matchTag = currentFilter.tag === "all" || video.tags.includes(currentFilter.tag);
    const matchSearch = !currentFilter.search || 
      video.title.toLowerCase().includes(currentFilter.search.toLowerCase()) ||
      video.album.toLowerCase().includes(currentFilter.search.toLowerCase()) ||
      video.tags.some(tag => tag.toLowerCase().includes(currentFilter.search.toLowerCase()));
    return matchAlbum && matchTag && matchSearch;
  });

  // 生成视频DOM
  filteredVideos.forEach(video => {
    const card = document.createElement("div");
    card.className = "video-card";
    // 核心修复：video标签取消固定高度，画面完整显示
    card.innerHTML = `
      <video controls src="${video.src}" preload="metadata">
        您的浏览器不支持视频播放
      </video>
      <div class="video-info">${video.title}</div>
    `;
    videoGrid.appendChild(card);
  });
}

// 获取所有合集
function getAlbums() {
  const albumSet = new Set(videos.map(video => video.album));
  return ["all", ...Array.from(albumSet)];
}

// 获取所有标签
function getTags() {
  const tagSet = new Set();
  videos.forEach(video => video.tags.forEach(tag => tagSet.add(tag)));
  return ["all", ...Array.from(tagSet)];
}

// 渲染标签
function renderTags() {
  tagList.innerHTML = "";
  getTags().forEach(tag => {
    const tagEl = document.createElement("div");
    tagEl.className = `tag ${currentFilter.tag === tag ? "active" : ""}`;
    tagEl.innerText = tag === "all" ? "全部" : tag;
    tagEl.onclick = () => {
      currentFilter.tag = tag;
      renderTags();
      renderVideos();
    };
    tagList.appendChild(tagEl);
  });
}

// 渲染合集（列表样式）
function renderAlbums() {
  albumList.innerHTML = "";
  getAlbums().forEach(album => {
    const albumEl = document.createElement("li");
    albumEl.className = `album-item ${currentFilter.album === album ? "active" : ""}`;
    albumEl.innerText = album === "all" ? "全部合集" : album;
    albumEl.onclick = () => {
      currentFilter.album = album;
      renderAlbums();
      renderVideos();
    };
    albumList.appendChild(albumEl);
  });
}

// 搜索监听
searchInput.addEventListener("input", (e) => {
  currentFilter.search = e.target.value.trim().toLowerCase();
  renderVideos();
});

// 密码验证逻辑
const dialog = document.getElementById('passwordDialog');
const content = document.getElementById('content');
const errorTip = document.getElementById('errorTip');
const passwordInput = document.getElementById('passwordInput');

window.onload = () => dialog.showModal();
function closeDialog() { dialog.close(); }
function verifyPassword() {
  if (passwordInput.value === '123') {
    dialog.close();
    content.style.display = 'block';
  } else {
    errorTip.style.display = 'block';
    passwordInput.value = '';
    setTimeout(() => errorTip.style.display = 'none', 2000);
  }
}

// 初始化渲染
renderVideos();
renderTags();
renderAlbums();
</script>
