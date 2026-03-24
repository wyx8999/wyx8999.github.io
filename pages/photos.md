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
  .photo-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    margin-top: 20px;
  }
  .photo-grid img {
    width: 100%;
    height: auto;
    border-radius: 6px;
  }
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
  <div class="photo-grid">
    <img src="/assets/photos/hello_hebe1.jpg" alt="">
    <img src="/assets/photos/吃饭很香1.jpg" alt="">
    <img src="/assets/photos/馨雅1.jpg" alt="">
  </div>
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
