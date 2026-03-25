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
#content { display: none; }
.photo-container {
  margin-top: 20px;
  display: flex;
  gap: 25px;
  flex-wrap: wrap;
}
.photo-main {
  flex: 1;
  min-width: 550px;
}
.photo-sidebar {
  width: 240px;
  padding: 15px 20px;
  background: #fafafa;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.05);
  font-size: 14px;
}
.photo-search input {
  width: 100%;
  padding: 8px 12px;
  border: 1px solid #eee;
  border-radius: 6px;
  outline: none;
  font-size: 14px;
}
.photo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
  gap: 14px;
}
.photo-item {
  aspect-ratio: 1;
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
  object-fit: cover;
}
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
  <div class="photo-container">
    <div class="photo-main">
      <div class="photo-grid" id="photoGrid"></div>
    </div>
    <div class="photo-sidebar">
      <div class="photo-search">
        <input type="text" id="searchInput" placeholder="搜索照片/标签/合集">
      </div>
      <div class="tag-title">标签</div>
      <div class="tag-list" id="tagList"></div>
      <div class="album-title">合集</div>
      <div class="album-list" id="albumList"></div>
    </div>
  </div>
</div>

<div class="photo-modal" id="photoModal" onclick="closeModal()">
  <img id="modalImg">
</div>

<dialog id="passwordDialog" style="border:none; border-radius:8px; padding:25px;">
  请输入密码查看照片
  <div id="errorTip" style="color:red; font-size:12px; display:none;">密码错误</div>
  <input type="password" id="passwordInput" style="width:100%; margin:10px 0; padding:8px; border:1px solid #ddd; border-radius:6px;">
  <div style="display:flex; justify-content:flex-end; gap:10px;">
    <button onclick="closeDialog()">取消</button>
    <button onclick="verifyPassword()">确定</button>
  </div>
</dialog>

<script>
const photos = [
  { src: "/assets/photos/hello_hebe1.jpg", alt: "Hebe", album: "生活", tags: ["人物","日常"] },
  { src: "/assets/photos/吃饭很香1.jpg", alt: "美食", album: "美食", tags: ["美食","生活"] },
  { src: "/assets/photos/馨雅1.jpg", alt: "馨雅", album: "生活", tags: ["人物","旅行"] }
];

const photoGrid = document.getElementById("photoGrid");
const tagList = document.getElementById("tagList");
const albumList = document.getElementById("albumList");
const searchInput = document.getElementById("searchInput");
const photoModal = document.getElementById("photoModal");
const modalImg = document.getElementById("modalImg");
let currentFilter = { album: "all", tag: "all", search: "" };

function openModal(src) { modalImg.src = src; photoModal.classList.add("show"); }
function closeModal() { photoModal.classList.remove("show"); }

function renderPhotos() {
  photoGrid.innerHTML = "";
  photos.filter(p =>
    (currentFilter.album === "all" || p.album === currentFilter.album) &&
    (currentFilter.tag === "all" || p.tags.includes(currentFilter.tag)) &&
    (!currentFilter.search || p.alt.toLowerCase().includes(currentFilter.search.toLowerCase()))
  ).forEach(p => {
    const div = document.createElement("div");
    div.className = "photo-item";
    div.innerHTML = `<img src="${p.src}" alt="${p.alt}">`;
    div.onclick = () => openModal(p.src);
    photoGrid.appendChild(div);
  });
}

function getAlbums() { return ["all", ...new Set(photos.map(p => p.album))]; }
function getTags() { return ["all", ...new Set(photos.flatMap(p => p.tags))]; }

function renderTags() {
  tagList.innerHTML = "";
  getTags().forEach(tag => {
    const el = document.createElement("div");
    el.className = "tag" + (currentFilter.tag === tag ? " active" : "");
    el.innerText = tag === "all" ? "全部" : tag;
    el.onclick = () => { currentFilter.tag = tag; renderTags(); renderPhotos(); };
    tagList.appendChild(el);
  });
}

function renderAlbums() {
  albumList.innerHTML = "";
  getAlbums().forEach(album => {
    const el = document.createElement("div");
    el.className = "album-item" + (currentFilter.album === album ? " active" : "");
    el.innerText = album === "all" ? "全部合集" : album;
    el.onclick = () => { currentFilter.album = album; renderAlbums(); renderPhotos(); };
    albumList.appendChild(el);
  });
}

searchInput.addEventListener("input", e => {
  currentFilter.search = e.target.value.trim();
  renderPhotos();
});

renderPhotos(); renderTags(); renderAlbums();

const dialog = document.getElementById('passwordDialog');
const content = document.getElementById('content');
window.onload = () => dialog.showModal();
function closeDialog() { dialog.close(); }
function verifyPassword() {
  if (passwordInput.value === '123') { dialog.close(); content.style.display = 'block'; }
  else { errorTip.style.display = 'block'; passwordInput.value = ''; }
}
</script>
