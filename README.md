# Ry
# 我有多爱你 ❤️ 小网站代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <title>我有多爱你</title>
  <style>
    body {
      background: #ffe6f0;
      font-family: "Comic Sans MS", cursive, sans-serif;
      text-align: center;
      color: #cc3366;
      padding: 40px;
    }
    .page {
      display: none;
    }
    .page.active {
      display: block;
    }
    button {
      background: #ff66a3;
      border: none;
      border-radius: 30px;
      color: white;
      padding: 12px 25px;
      margin: 8px;
      font-size: 16px;
      cursor: pointer;
    }
    button:hover {
      background: #ff3385;
    }
    textarea {
      width: 80%;
      height: 100px;
      border-radius: 10px;
      border: 2px solid #ff99cc;
      padding: 10px;
      font-size: 16px;
    }
    .note-box {
      background: #fff0f5;
      border: 2px dashed #ff99cc;
      border-radius: 10px;
      padding: 20px;
      margin-top: 30px;
      text-align: left;
      max-width: 600px;
      margin-left: auto;
      margin-right: auto;
    }
    .note-box ul {
      list-style: none;
      padding-left: 0;
      margin: 0;
    }
    .note-box li {
      background: #ffcee3;
      margin-bottom: 8px;
      padding: 8px 12px;
      border-radius: 8px;
      word-break: break-word;
    }
  </style>
</head>
<body>

<div id="page1" class="page active">
  <h2>猜猜我有多爱你？</h2>
  <button onclick="checkAnswer('wrong')">你给我走开</button>
  <button onclick="checkAnswer('wrong')">一点也不爱</button>
  <button onclick="checkAnswer('wrong')">一般般</button>
  <button onclick="checkAnswer('wrong')">爱你</button>
  <button onclick="checkAnswer('right')">超级无敌宇宙第一爱</button>
  <p id="message" style="color:red;"></p>
</div>

<div id="page2" class="page">
  <h2>Ry已经</h2>
  <h1 id="daysTogether">加载中...</h1>
  <p>天</p>
  <button onclick="showPage('page3')">去看看小纸条 💌</button>
</div>

<div id="page3" class="page">
  <h2>给对方留一句话吧 💌</h2>
  <textarea id="noteInput" placeholder="写下你想说的话..."></textarea><br>
  <button onclick="saveNote()">发送小纸条</button>
  <div class="note-box">
    <h3>最近的小纸条 ✉️</h3>
    <div id="latestNote">这里是空的 去留言！</div>
  </div>
</div>

<script>
  function showPage(id) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(id).classList.add('active');
  }

  function checkAnswer(answer) {
    const msg = document.getElementById('message');
    if(answer === 'right') {
      msg.innerText = '';
      showPage('page2');
      updateDays();
    } else {
      msg.innerText = "不对！！再想想";
    }
  }

  function updateDays() {
    const startDate = new Date("2025-01-19");
    const today = new Date();
    const diffTime = today - startDate;
    const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24));
    document.getElementById("daysTogether").innerText = `第 ${diffDays} 天`;
  }

  function saveNote() {
    const noteInput = document.getElementById('noteInput');
    const note = noteInput.value.trim();
    if (!note) {
      alert("不要留空白");
      return;
    }

    let notes = JSON.parse(localStorage.getItem('loveNotes') || '[]');
    notes.unshift(note);
    if (notes.length > 10) notes = notes.slice(0, 10);
    localStorage.setItem('loveNotes', JSON.stringify(notes));
    displayNotes(notes);
    noteInput.value = '';
    alert("小纸条送出成功！");
  }

  function displayNotes(notes) {
    const container = document.getElementById('latestNote');
    if (notes.length === 0) {
      container.innerHTML = '还没有留言呢~';
      return;
    }
    const html = notes.map(n => `<li>${escapeHtml(n)}</li>`).join('');
    container.innerHTML = `<ul>${html}</ul>`;
  }

  function escapeHtml(text) {
    return text.replace(/[&<>"']/g, function(m) {
      return {'&':'&amp;', '<':'&lt;', '>':'&gt;', '"':'&quot;', "'":'&#39;'}[m];
    });
  }

  window.onload = function () {
    let notes = JSON.parse(localStorage.getItem('loveNotes') || '[]');
    displayNotes(notes);
  }
</script>

</body>
</html>
