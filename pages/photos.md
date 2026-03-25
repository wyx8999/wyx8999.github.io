---
layout: wiki
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
/* 基础布局 */
#content { display: none; }
.photo-container {
  display: flex;
  gap: 30px;
  margin-top: 30px;
  flex-wrap: wrap;
}
.photo-main {
  flex: 1;
  min-width: 600px;
}
.photo-sidebar {
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
.photo-search {
  margin-bottom: 20px;
}
.photo-search input {
  width: 100%;
  padding: 10px 14px;
  border-radius: 8px;
  border: 1px solid #eee;
  outline: none;
  font-size: 14px;
}
.photo-search input:focus {
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

/* 照片网格（统一大小） */
.photo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 16px;
}
.photo-item {
  position: relative;
  overflow: hidden;
  border-radius: 10px;
  aspect-ratio: 1 / 1;
  cursor: pointer;
  transition: 0.3s;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
}
.photo-item:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.12);
}
.photo-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* 大图预览弹窗 */
.photo-modal {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.8);
  z-index: 9999;
  align-items: center;
  justify-content: center;
  padding: 20px;
}
.photo-modal.show {
  display: flex;
}
.photo-modal img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 8px;
  object-fit: contain;
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
  <div class="photo-container">
    <!-- 左侧照片区域 -->
    <div class="photo-main">
      <div class="photo-grid" id="photoGrid">
        <!-- 照片会被JS自动渲染 -->
      </div>
    </div>

    <!-- 右侧边栏 -->
    <div class="photo-sidebar">
      <div class="photo-search">
        <input type="text" id="searchInput" placeholder="搜索照片描述/标签/合集">
      </div>

      <div class="tag-title">标签</div>
      <div class="tag-list" id="tagList"></div>

      <div class="album-title">合集</div>
      <div class="album-list" id="albumList"></div>
    </div>
  </div>
</div>

<!-- 照片预览弹窗 -->
<div class="photo-modal" id="photoModal" onclick="closeModal()">
  <img id="modalImg">
</div>

<dialog id="passwordDialog">
  <div>请输入密码查看照片</div>
  <div class="error-tip" id="errorTip">密码错误！</div>
  <input type="password" class="dialog-input" id="passwordInput">
  <div class="dialog-buttons">
    <button class="dialog-btn" onclick="closeDialog()">取消</button>
    <button class="dialog-btn confirm" onclick="verifyPassword()">确定</button>
  </div>
</dialog>

<script>
// ====================== 配置区：你只需要改这里 ======================
const photos = [
  {
    src: "/assets/photos/hello_hebe1.jpg",
    alt: "Hebe",
    album: "生活",
    tags: ["人物", "日常"]
  },
  {
    src: "/assets/photos/吃饭很香1.jpg",
    alt: "美食",
    album: "美食",
    tags: ["美食", "生活"]
  },
  {
    src: "/assets/photos/馨雅1.jpg",
    alt: "馨雅",
    album: "生活",
    tags: ["人物", "旅行"]
  }
];
// ==================================================================

const photoGrid = document.getElementById("photoGrid");
const tagList = document.getElementById("tagList");
const albumList = document.getElementById("albumList");
const searchInput = document.getElementById("searchInput");
const photoModal = document.getElementById("photoModal");
const modalImg = document.getElementById("modalImg");

let currentFilter = { album: "all", tag: "all", search: "" };

// 打开预览
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
  const filtered = photos.filter(item => {
    const matchAlbum = currentFilter.album === "all" || item.album === currentFilter.album;
    const matchTag = currentFilter.tag === "all" || item.tags.includes(currentFilter.tag);
    const matchSearch = !currentFilter.search ||
      item.alt.toLowerCase().includes(currentFilter.search.toLowerCase()) ||
      item.album.toLowerCase().includes(currentFilter.search.toLowerCase()) ||
      item.tags.some(t => t.toLowerCase().includes(currentFilter.search.toLowerCase()));
    return matchAlbum && matchTag && matchSearch;
  });
  filtered.forEach(item => {
    const div = document.createElement("div");
    div.className = "photo-item";
    div.innerHTML = `<img src="${item.src}" alt="${item.alt}">`;
    div.onclick = () => openModal(item.src);
    photoGrid.appendChild(div);
  });
}

// 获取所有合集
function getAlbums() {
  const set = new Set();
  photos.forEach(p => set.add(p.album));
  return ["all", ...Array.from(set)];
}

// 获取所有标签
function getTags() {
  const set = new Set();
  photos.forEach(p => p.tags.forEach(t => set.add(t)));
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
      renderPhotos();
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
      renderPhotos();
    };
    albumList.appendChild(el);
  });
}

// 搜索
searchInput.addEventListener("input", e => {
  currentFilter.search = e.target.value.trim();
  renderPhotos();
});

// 初始化
renderPhotos();
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
