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
/* ① 先隐藏内容，等弹窗关闭后再显示 */
#content { 
  display: none; 
  overflow-y: auto;   /* 让滚动条仅在内容区出现 */
  padding: 20px;
}

/* ② 主容器 */
.photo-container {
  display: flex;
  flex-wrap: wrap;            /* 允许换行（手机端） */
  gap: 20px;
}

/* ③ 照片区域（左侧） */
.photo-main {
  flex: 1 1 0;               /* 可伸缩，优先占据剩余空间 */
  min-width: 0;              /* 让 flex 子元素可完全收缩 */
  padding: 0;
}

/* ④ 右侧搜索/标签/合集容器 */
.photo-sidebar {
  width: 260px;             /* 固定宽度，桌面端右侧 */
  flex-shrink: 0;          /* 不收缩 */
  padding: 15px 20px;
  background: #fafafa;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,.05);
}

/* ⑤ 搜索框 */
.photo-search input {
  width: 100%;
  padding: 8px 12px;
  border: 1px solid #eee;
  border-radius: 6px;
  outline: none;
  font-size: 14px;
}

/* ⑥ 统一标题样式 */
.tag-title,
.album-title {
  font-size: 15px;
  font-weight: 500;
  margin: 14px 0 8px;
}

/* ⑦ 标签列表 */
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
.tag.active,
.tag:hover {
  background: #555;
  color: #fff;
}

/* ⑧ 合集列表 */
.album-item {
  padding: 6px 10px;
  background: #f7f7f7;
  border-radius: 6px;
  margin-bottom: 6px;
  cursor: pointer;
  font-size: 14px;
}
.album-item.active,
.album-item:hover {
  background: #555;
  color: #fff;
}

/* ⑨ 照片网格 */
.photo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
  gap: 14px;
}
.photo-item {
  aspect-ratio: 1;
  border-radius: 6px;
  overflow: hidden;
  cursor: pointer;
  box-shadow: 0 1px 4px rgba(0,0,0,.05);
  transition: transform .2s;
}
.photo-item:hover { transform: scale(1.02); }
.photo-item img {
  width: 100%; height: 100%; object-fit: cover;
}

/* ⑩ 模态窗 */
.photo-modal { 
  display: none; 
  position: fixed; top: 0; left: 0; right: 0; bottom: 0; 
  background: rgba(0,0,0,.75); z-index: 9999; 
  align-items:center; justify-content:center; 
}
.photo-modal.show { display: flex; }
.photo-modal img { max-width:90%; max-height:90%; border-radius:8px;}

/* ⑪ 密码弹窗样式的细调（保持与原 dialog 兼容） */
#passwordDialog { 
  border: none; border-radius:8px; padding:25px; 
}

/* ⑫ 媒体查询：在 768px 以下做垂直排布 */
@media (max-width:768px) {
  .photo-container { flex-direction: column; }
  .photo-sidebar { width: 100%; }
  .photo-main { width: 100%; }
  .photo-grid { grid-template-columns: repeat(auto-fill, minmax(120px, 1fr)); }
}
</style>

<div id="content">
  <div class="photo-container">
    <!-- ① 照片主区 -->
    <div class="photo-main">
      <div class="photo-grid" id="photoGrid"></div>
    </div>
    <!-- ② 右侧搜索、标签、合集 -->
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

<!-- ③ 模态窗 -->
<div class="photo-modal" id="photoModal" onclick="closeModal()">
  <img id="modalImg">
</div>

<!-- ④ 密码弹窗 -->
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
// --- ① 数据定义（可按需自行扩展） ---
const photos = [
  { src: "/assets/photos/hello_hebe1.jpg", alt: "Hebe", album: "生活", tags: ["人物","日常"] },
  { src: "/assets/photos/吃饭很香1.jpg", alt: "美食", album: "美食", tags: ["美食","生活"] },
  { src: "/assets/photos/馨雅1.jpg", alt: "馨雅", album: "生活", tags: ["人物","旅行"] },
  // 继续添加...
];

// --- ② 取引用 ---
const photoGrid   = document.getElementById("photoGrid");
const tagList     = document.getElementById("tagList");
const albumList   = document.getElementById("albumList");
const searchInput = document.getElementById("searchInput");
const photoModal  = document.getElementById("photoModal");
const modalImg    = document.getElementById("modalImg");
const dialog      = document.getElementById('passwordDialog');
const content     = document.getElementById('content');
const errorTip    = document.getElementById('errorTip');
const passwordInput = document.getElementById('passwordInput');

// --- ③ 过滤状态 ---
let currentFilter = { album:"all", tag:"all", search:"" };

// --- ④ 模态窗 ---
function openModal(src){ modalImg.src = src; photoModal.classList.add('show'); }
function closeModal(){ photoModal.classList.remove('show'); }

// --- ⑤ 渲染照片 ---
function renderPhotos(){
  photoGrid.innerHTML = "";
  photos.filter(p =>
      (currentFilter.album === "all" || p.album === currentFilter.album) &&
      (currentFilter.tag === "all" || p.tags.includes(currentFilter.tag)) &&
      (!currentFilter.search || p.alt.toLowerCase().includes(currentFilter.search.toLowerCase()))
  ).forEach(p=>{
      const div   = document.createElement("div");
      div.class   = "photo-item";
      div.innerHTML = `<img src="${p.src}" alt="${p.alt}">`;
      div.onclick = ()=>openModal(p.src);
      photoGrid.appendChild(div);
  });
}

// --- ⑥ 获取唯一合集与标签 ---
function getAlbums(){ return ["all", ...new Set(photos.map(p=>p.album))]; }
function getTags(){   return ["all", ...new Set(photos.flatMap(p=>p.tags))]; }

// --- ⑦ 渲染标签 ---
function renderTags(){
  tagList.innerHTML = "";
  getTags().forEach(tag=>{
    const el = document.createElement("div");
    el.className = "tag" + (currentFilter.tag===tag?" active":"");
    el.innerText = tag==="all"?"全部":tag;
    el.onclick = ()=>{
        currentFilter.tag = tag;
        renderTags(); renderPhotos();
    };
    tagList.appendChild(el);
  });
}

// --- ⑧ 渲染合集 ---
function renderAlbums(){
  albumList.innerHTML = "";
  getAlbums().forEach(album=>{
    const el = document.createElement("div");
    el.className = "album-item" + (currentFilter.album===album?" active":"");
    el.innerText = album==="all"?"全部合集":album;
    el.onclick = ()=>{
        currentFilter.album = album;
        renderAlbums(); renderPhotos();
    };
    albumList.appendChild(el);
  });
}

// --- ⑨ 搜索输入 ---
searchInput.addEventListener("input", e=>{
  currentFilter.search = e.target.value.trim();
  renderPhotos();
});

// --- ⑩ 初始化 ---
renderPhotos(); renderTags(); renderAlbums();

// --- ⑪ 密码弹窗 ---
window.onload = () => dialog.showModal();

function closeDialog(){ dialog.close(); }
function verifyPassword(){
    if(passwordInput.value==='123'){  // 演示用密码，实际请改为安全方案
        dialog.close();
        content.style.display = 'block';
    }else{
        errorTip.style.display='block';
        passwordInput.value='';
    }
}
</script>
