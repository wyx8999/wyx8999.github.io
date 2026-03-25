---
layout: wiki
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
.video-container {
  margin-top: 20px;
  display: flex;
  gap: 25px;
  flex-wrap: wrap;
}
.video-main {
  flex: 1;
  min-width: 550px;
}
.video-sidebar {
  width: 240px;
  padding: 15px 20px;
  background: #fafafa;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.05);
  font-size: 14px;
}
.video-search input {
  width: 100%;
  padding: 8px 12px;
  border: 1px solid #eee;
  border-radius: 6px;
  outline: none;
  font-size: 14px;
}
.video-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 16px;
}
.video-card {
  background: #fff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 1px 4px rgba(0,0,0,0.05);
}
.video-card video {
  width: 100%;
  height: 160px;
  object-fit: cover;
}
.video-info {
  padding: 10px 12px;
  font-size: 14px;
}
.tag-title, .album-title {
  font-size: 15px;
  font-weight: 500;
  margin: 14px 0 8px;
}
.tag-list {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 14px;
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
.album-item {
  padding: 6px 10px;
  background: #f7f7f7;
  border-radius: 6px;
  margin-bottom: 6px;
  cursor: pointer;
  font-size: 14px;
}
.album-item.active, .album-item:hover {
  background: #555;
  color: #fff;
}
</style>

<div id="content">
  <div class="video-container">
    <div class="video-main">
      <div class="video-grid" id="videoGrid"></div>
    </div>
    <div class="video-sidebar">
      <div class="video-search">
        <input type="text" id="searchInput" placeholder="搜索视频/标签/合集">
      </div>
      <div class="tag-title">标签</div>
      <div class="tag-list" id="tagList"></div>
      <div class="album-title">合集</div>
      <div class="album-list" id="albumList"></div>
    </div>
  </div>
</div>

<dialog id="passwordDialog" style="border:none; border-radius:8px; padding:25px;">
  请输入密码查看视频
  <div id="errorTip" style="color:red; font-size:12px; display:none;">密码错误</div>
  <input type="password" id="passwordInput" style="width:100%; margin:10px 0; padding:8px; border:1px solid #ddd; border-radius:6px;">
  <div style="display:flex; justify-content:flex-end; gap:10px;">
    <button onclick="closeDialog()">取消</button>
    <button onclick="verifyPassword()">确定</button>
  </div>
</dialog>

<script>
const videos = [
  { src: "/assets/video/文明你我他1.mp4", title: "文明你我他", album: "生活", tags: ["日常","生活"] },
  { src: "/assets/video/冷酷的文儿.mp4", title: "冷酷的文儿", album: "人物", tags: ["人物","随拍"] },
  { src: "/assets/video/(玖贰陆（佛系更新）)🥯_780367cb71435e4823f91c8347435de1.mp4", title: "玖贰陆", album: "日常", tags: ["生活","随拍"] }
];

const videoGrid = document.getElementById("videoGrid");
const tagList = document.getElementById("tagList");
const albumList = document.getElementById("albumList");
const searchInput = document.getElementById("searchInput");
let currentFilter = { album: "all", tag: "all", search: "" };

function renderVideos() {
  videoGrid.innerHTML = "";
  videos.filter(v =>
    (currentFilter.album === "all" || v.album === currentFilter.album) &&
    (currentFilter.tag === "all" || v.tags.includes(currentFilter.tag)) &&
    (!currentFilter.search || v.title.toLowerCase().includes(currentFilter.search.toLowerCase()))
  ).forEach(v => {
    const card = document.createElement("div");
    card.className = "video-card";
    card.innerHTML = `<video controls src="${v.src}"></video><div class="video-info">${v.title}</div>`;
    videoGrid.appendChild(card);
  });
}

function getAlbums() { return ["all", ...new Set(videos.map(v => v.album))]; }
function getTags() { return ["all", ...new Set(videos.flatMap(v => v.tags))]; }

function renderTags() {
  tagList.innerHTML = "";
  getTags().forEach(tag => {
    const el = document.createElement("div");
    el.className = "tag" + (currentFilter.tag === tag ? " active" : "");
    el.innerText = tag === "all" ? "全部" : tag;
    el.onclick = () => { currentFilter.tag = tag; renderTags(); renderVideos(); };
    tagList.appendChild(el);
  });
}

function renderAlbums() {
  albumList.innerHTML = "";
  getAlbums().forEach(album => {
    const el = document.createElement("div");
    el.className = "album-item" + (currentFilter.album === album ? " active" : "");
    el.innerText = album === "all" ? "全部合集" : album;
    el.onclick = () => { currentFilter.album = album; renderAlbums(); renderVideos(); };
    albumList.appendChild(el);
  });
}

searchInput.addEventListener("input", e => {
  currentFilter.search = e.target.value.trim();
  renderVideos();
});

renderVideos(); renderTags(); renderAlbums();

const dialog = document.getElementById('passwordDialog');
const content = document.getElementById('content');
window.onload = () => dialog.showModal();
function closeDialog() { dialog.close(); }
function verifyPassword() {
  if (passwordInput.value === '123') { dialog.close(); content.style.display = 'block'; }
  else { errorTip.style.display = 'block'; passwordInput.value = ''; }
}
</script>
