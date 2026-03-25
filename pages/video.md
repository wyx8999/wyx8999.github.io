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
/* 基础布局 */
#content { display: none; }
.video-container {
  display: flex;
  gap: 30px;
  margin-top: 30px;
  flex-wrap: wrap;
}
.video-main {
  flex: 1;
  min-width: 600px;
}
.video-sidebar {
  width: 260px;
  position: sticky;
  top: 40px;
  height: fit-content;
  padding: 20px;
  background: #fafafa;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0,0,0,0.06);
}

/* 搜索 */
.video-search {
  margin-bottom: 20px;
}
.video-search input {
  width: 100%;
  padding: 10px 14px;
  border-radius: 8px;
  border: 1px solid #eee;
  outline: none;
  font-size: 14px;
}
.video-search input:focus {
  border-color: #888;
}

/* 标签云 */
.tag-title, .album-title {
  font-size: 16px;
  font-weight: 600;
  margin: 20px 0 10px;
}
.tag-list {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 20px;
}
.tag {
  padding: 5px 10px;
  background: #f0f0f0;
  border-radius: 20px;
  font-size: 13px;
  cursor: pointer;
  transition: 0.2s;
}
.tag:hover, .tag.active {
  background: #555;
  color: #fff;
}

/* 合集列表 */
.album-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.album-item {
  padding: 8px 12px;
  background: #f6f6f6;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  transition: 0.2s;
}
.album-item:hover, .album-item.active {
  background: #555;
  color: #fff;
}

/* 视频网格（统一大小、美观卡片） */
.video-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
}
.video-card {
  background: #fff;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0,0,0,0.08);
  transition: 0.3s;
}
.video-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.12);
}
.video-card video {
  width: 100%;
  height: 180px;
  object-fit: cover;
  display: block;
}
.video-info {
  padding: 12px 14px;
  font-size: 14px;
  color: #333;
}

/* 密码弹窗 */
dialog {
  border: none;
  border-radius: 12px;
  padding: 30px;
  width: 340px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
}
dialog::backdrop {
  background: rgba(0,0,0,0.4);
}
.dialog-input {
  width: 100%;
  padding: 12px;
  margin: 14px 0;
  border-radius: 8px;
  border: 1px solid #eee;
  outline: none;
}
.dialog-buttons {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 10px;
}
.dialog-btn {
  padding: 8px 16px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  transition: 0.2s;
}
.confirm {
  background: #444;
  color: #fff;
}
.confirm:hover {
  background: #222;
}
.error-tip {
  color: #f56c6c;
  font-size: 13px;
  display: none;
}
</style>

<div id="content">
  <div class="video-container">
    <!-- 左侧视频区域 -->
    <div class="video-main">
      <div class="video-grid" id="videoGrid">
        <!-- 视频自动渲染 -->
      </div>
    </div>

    <!-- 右侧边栏：搜索 + 标签 + 合集 -->
    <div class="video-sidebar">
      <div class="video-search">
        <input type="text" id="searchInput" placeholder="搜索视频描述/标签/合集">
      </div>

      <div class="tag-title">标签</div>
      <div class="tag-list" id="tagList"></div>

      <div class="album-title">合集</div>
      <div class="album-list" id="albumList"></div>
    </div>
  </div>
</div>

<dialog id="passwordDialog">
  <div>请输入密码查看视频</div>
  <div class="error-tip" id="errorTip">密码错误！</div>
  <input type="password" class="dialog-input" id="passwordInput">
  <div class="dialog-buttons">
    <button class="dialog-btn" onclick="closeDialog()">取消</button>
    <button class="dialog-btn confirm" onclick="verifyPassword()">确定</button>
  </div>
</dialog>

<script>
// ====================== 视频配置区：只改这里 ======================
const videos = [
  {
    src: "/assets/video/文明你我他1.mp4",
    title: "文明你我他",
    album: "文明你我他",
    tags: ["眼镜", "反差"]
  },
  {
    src: "/assets/video/冷酷的文儿.mp4",
    title: "冷酷的文儿",
    album: "严厉的文老师",
    tags: ["老师", "反差","闷骚"]
  },
  {
    src: "/assets/video/(玖贰陆（佛系更新）)🥯_780367cb71435e4823f91c8347435de1.mp4",
    title: "玖贰陆",
    album: "玖贰陆",
    tags: ["眼镜"]
  }
];
// ==================================================================

const videoGrid = document.getElementById("videoGrid");
const tagList = document.getElementById("tagList");
const albumList = document.getElementById("albumList");
const searchInput = document.getElementById("searchInput");

let currentFilter = { album: "all", tag: "all", search: "" };

// 渲染视频
function renderVideos() {
  videoGrid.innerHTML = "";
  const filtered = videos.filter(item => {
    const matchAlbum = currentFilter.album === "all" || item.album === currentFilter.album;
    const matchTag = currentFilter.tag === "all" || item.tags.includes(currentFilter.tag);
    const matchSearch = !currentFilter.search ||
      item.title.toLowerCase().includes(currentFilter.search.toLowerCase()) ||
      item.album.toLowerCase().includes(currentFilter.search.toLowerCase()) ||
      item.tags.some(t => t.toLowerCase().includes(currentFilter.search.toLowerCase()));
    return matchAlbum && matchTag && matchSearch;
  });
  filtered.forEach(item => {
    const card = document.createElement("div");
    card.className = "video-card";
    card.innerHTML = `
      <video controls src="${item.src}"></video>
      <div class="video-info">${item.title}</div>
    `;
    videoGrid.appendChild(card);
  });
}

// 获取所有合集
function getAlbums() {
  const set = new Set();
  videos.forEach(v => set.add(v.album));
  return ["all", ...Array.from(set)];
}

// 获取所有标签
function getTags() {
  const set = new Set();
  videos.forEach(v => v.tags.forEach(t => set.add(t)));
  return ["all", ...Array.from(set)];
}

// 渲染标签
function renderTags() {
  tagList.innerHTML = "";
  getTags().forEach(tag => {
    const el = document.createElement("div");
    el.className = "tag" + (currentFilter.tag === tag ? " active" : "");
    el.innerText = tag === "all" ? "全部" : tag;
    el.onclick = () => {
      currentFilter.tag = tag;
      renderTags();
      renderVideos();
    };
    tagList.appendChild(el);
  });
}

// 渲染合集
function renderAlbums() {
  albumList.innerHTML = "";
  getAlbums().forEach(album => {
    const el = document.createElement("div");
    el.className = "album-item" + (currentFilter.album === album ? " active" : "");
    el.innerText = album === "all" ? "全部合集" : album;
    el.onclick = () => {
      currentFilter.album = album;
      renderAlbums();
      renderVideos();
    };
    albumList.appendChild(el);
  });
}

// 搜索
searchInput.addEventListener("input", e => {
  currentFilter.search = e.target.value.trim();
  renderVideos();
});

// 初始化
renderVideos();
renderTags();
renderAlbums();

// 密码逻辑
const dialog = document.getElementById('passwordDialog');
const content = document.getElementById('content');
window.onload = () => dialog.showModal();
function closeDialog() { dialog.close(); }
function verifyPassword() {
  if (passwordInput.value === '123') {
    dialog.close();
    content.style.display = 'block';
  } else {
    errorTip.style.display = 'block';
    passwordInput.value = '';
  }
}
</script>
