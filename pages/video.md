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
  .video-item { margin: 20px 0; }
  video { width: 100%; border-radius: 6px; }
  dialog {
    border: none;
    border-radius: 10px;
    padding: 25px;
    width: 320px;
    box-shadow: 0 5px 20px rgba(0,0,0,0.2);
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  dialog::backdrop { background: rgba(0,0,0,0.4); }
  .dialog-input { width: 100%; padding: 10px; margin: 10px 0; border-radius: 6px; border: 1px solid #ddd; }
  .dialog-buttons { display: flex; justify-content: flex-end; gap: 10px; }
  .dialog-btn { padding: 8px 14px; border-radius: 6px; border: none; cursor: pointer; }
  .confirm { background: #555; color: #fff; }
  .error-tip { color: red; font-size: 12px; display: none; }
</style>

<div id="content">
  <div class="video-item">
    <video controls src="/assets/video/米修米修1.mp4"></video>
  </div>
  <div class="video-item">
    <video controls src="/assets/video/文明你我他1.mp4"></video>
  </div>
  <div class="video-item">
    <video controls src="/assets/video/玖贰陆1.mp4"></video>
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
  const dialog = document.getElementById('passwordDialog');
  const content = document.getElementById('content');
  window.onload = () => dialog.showModal();
  function closeDialog() { dialog.close(); }
  function verifyPassword() {
    if(passwordInput.value === '123') {
      dialog.close();
      content.style.display = 'block';
    } else {
      errorTip.style.display = 'block';
      passwordInput.value = '';
    }
  }
</script>
