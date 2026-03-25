---
layout: photos
title: 照片展示
description: 我的私人照片集
keywords: 照片,相册
comments: false
copyright: false
menu: 照片
permalink: /photos/
---

> 私藏岁月，静赏芳华。

<style>
#content { display: none; }
/* 主布局：左侧内容，右侧搜索+标签+合集（和维基页一致） */
.photo-container {
  margin-top: 20px;
  display: flex;
  gap: 30px;
  width: 100%;
}
.photo-main {
  flex: 1; /* 左侧占满剩余宽度 */
}
/* 右侧边栏：完全匹配维基页Search+目录布局 */
.photo-sidebar {
  width: 260px;
  flex-shrink: 0; /* 固定宽度不收缩 */
  font-size: 14px;
}
/* 搜索框样式：1:1匹配维基页 */
.photo-search {
  margin-bottom: 20px;
}
.photo-search label {
  display: block;
  font-weight: bold;
  margin-bottom: 8px;
  color: #333;
}
.photo-search input {
  width: 100%;
  padding: 6px 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  outline: none;
  font-size: 14px;
}
/* 标签/合集标题：匹配维基页Table of Contents样式 */
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
/* 合集列表：匹配维基页目录样式 */
.album-list {
  padding-left: 0;
  margin: 0;
  list-style: none;
}
.album-item {
  padding: 4px 0;
  cursor: pointer;
  font-size: 14px;
  color: #0645ad; /* 匹配维基页链接蓝色 */
}
.album-item.active, .album-item:hover {
  text-decoration: underline;
  color: #0b0080;
}
/* 照片网格：统一大小+美观 */
.photo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 16px;
}
.photo-item {
  aspect-ratio: 1; /* 正方形统一大小 */
  border-radius: 6px;
  overflow: hidden;
  cursor: pointer;
  box-shadow: 0 1px 4px rgba(0,0,0,0.05);
  transition: transform 0.2s;
}
.photo-item:hover {
  transform: scale(1.02);
}
.photo-item img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* 保持比例不拉伸 */
}
/* 照片预览弹窗 */
.photo-modal {
  display: none;
  position: fixed;
  top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.75);
  z-index: 9999;
  align-items: center;
  justify-content: center;
}
.photo-modal.show { display: flex; }
.photo-modal img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 8px;
}
/* 密码弹窗：匹配全站风格 */
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
  <div class="photo-container">
    <!-- 左侧照片展示区 -->
    <div class="photo-main">
      <div class="photo-grid" id="photoGrid"></div>
    </div>
    
    <!-- 右侧：Search + 标签 + 合集（和维基页完全对齐） -->
    <div class="photo-sidebar">
      <!-- 搜索框（匹配维基页Search样式） -->
      <div class="photo-search">
        <label>Search</label>
        <input type="text" id="searchInput" placeholder="Search">
      </div>

      <!-- 标签（Search下方） -->
      <div class="tag-title">标签</div>
      <div class="tag-list" id="tagList"></div>

      <!-- 合集（标签下方，匹配目录样式） -->
      <div class="album-title">合集</div>
      <ul class="album-list" id="albumList"></ul>
    </div>
  </div>
</div>

<!-- 照片预览弹窗 -->
<div class="photo-modal" id="photoModal" onclick="closeModal()">
  <img id="modalImg">
</div>

<!-- 密码弹窗 -->
<dialog id="passwordDialog">
  <div>请输入密码查看照片</div>
  <div id="errorTip">密码错误！</div>
  <input type="password" id="passwordInput">
  <div class="dialog-buttons">
    <button onclick="closeDialog()">取消</button>
    <button class="confirm" onclick="verifyPassword()">确定</button>
  </div>
</dialog>

<script>
// 照片数据配置
const photos = [
  { src: "/assets/photos/hello_hebe1.jpg", alt: "Hebe", album: "生活", tags: ["人物","日常"] },
  { src: "/assets/photos/吃饭很香1.jpg", alt: "美食", album: "美食", tags: ["美食","生活"] },
  { src: "/assets/photos/馨雅1.jpg", alt: "馨雅", album: "生活", tags: ["人物","旅行"] }
];

// DOM元素
const photoGrid = document.getElementById("photoGrid");
const tagList = document.getElementById("tagList");
const albumList = document.getElementById("albumList");
const searchInput = document.getElementById("searchInput");
const photoModal = document.getElementById("photoModal");
const modalImg = document.getElementById("modalImg");

// 筛选条件
let currentFilter = { album: "all", tag: "all", search: "" };

// 预览弹窗控制
function openModal(src) { 
  modalImg.src = src; 
  photoModal.classList.add("show"); 
}
function closeModal() { 
  photoModal.classList.remove("show"); 
}

// 渲染照片
function renderPhotos() {
  photoGrid.innerHTML = "";
  // 筛选逻辑
  const filteredPhotos = photos.filter(photo => {
    const matchAlbum = currentFilter.album === "all" || photo.album === currentFilter.album;
    const matchTag = currentFilter.tag === "all" || photo.tags.includes(currentFilter.tag);
    const matchSearch = !currentFilter.search || 
      photo.alt.toLowerCase().includes(currentFilter.search.toLowerCase()) ||
      photo.album.toLowerCase().includes(currentFilter.search.toLowerCase()) ||
      photo.tags.some(tag => tag.toLowerCase().includes(currentFilter.search.toLowerCase()));
    return matchAlbum && matchTag && matchSearch;
  });

  // 生成照片DOM
  filteredPhotos.forEach(photo => {
    const item = document.createElement("div");
    item.className = "photo-item";
    item.innerHTML = `<img src="${photo.src}" alt="${photo.alt}">`;
    item.onclick = () => openModal(photo.src);
    photoGrid.appendChild(item);
  });
}

// 获取所有合集
function getAlbums() {
  const albumSet = new Set(photos.map(photo => photo.album));
  return ["all", ...Array.from(albumSet)];
}

// 获取所有标签
function getTags() {
  const tagSet = new Set();
  photos.forEach(photo => photo.tags.forEach(tag => tagSet.add(tag)));
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
      renderPhotos();
    };
    tagList.appendChild(tagEl);
  });
}

// 渲染合集（改为列表项，匹配维基页目录）
function renderAlbums() {
  albumList.innerHTML = "";
  getAlbums().forEach(album => {
    const albumEl = document.createElement("li");
    albumEl.className = `album-item ${currentFilter.album === album ? "active" : ""}`;
    albumEl.innerText = album === "all" ? "全部合集" : album;
    albumEl.onclick = () => {
      currentFilter.album = album;
      renderAlbums();
      renderPhotos();
    };
    albumList.appendChild(albumEl);
  });
}

// 搜索监听
searchInput.addEventListener("input", (e) => {
  currentFilter.search = e.target.value.trim().toLowerCase();
  renderPhotos();
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
renderPhotos();
renderTags();
renderAlbums();
</script>
