<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>萬用網頁與遊戲一鍵修改工具</title>
  <style>
    * { box-sizing: border-box; }
    body { font-family: -apple-system, BlinkMacSystemFont, sans-serif; margin: 0; display: flex; flex-direction: column; height: 100vh; background: #1e2227; }
    /* 頂部控制列（針對 iPad 觸控放大設計） */
    .toolbar {
      background: #24292e; color: white; padding: 12px 16px;
      display: flex; gap: 10px; align-items: center; flex-wrap: wrap;
      border-bottom: 1px solid #444d56;
    }
    .url-group { display: flex; gap: 8px; flex: 1; min-width: 280px; }
    input[type="url"] {
      flex: 1; padding: 10px 14px; border-radius: 6px; border: none;
      font-size: 16px; background: #ffffff; color: #000;
    }
    button {
      background: #2ea44f; color: white; border: none; padding: 10px 16px;
      border-radius: 6px; cursor: pointer; font-weight: bold; font-size: 15px;
      white-space: nowrap;
    }
    button:active { opacity: 0.8; }
    button.blue { background: #0366d6; }
    button.purple { background: #6f42c1; }
    button.orange { background: #d97706; }
    button.active-mode { background: #e36209 !important; box-shadow: 0 0 0 2px #fff; }
    .color-label { display: flex; align-items: center; gap: 6px; font-size: 15px; background: #2f363d; padding: 6px 10px; border-radius: 6px; }
    input[type="color"] { border: none; width: 28px; height: 28px; padding: 0; background: none; cursor: pointer; }
    #status { width: 100%; font-size: 13px; color: #ffdf5d; margin-top: 2px; }
    /* 網頁預覽畫布 */
    iframe { flex: 1; width: 100%; border: none; background: #ffffff; }
  </style>
</head>
<body>

  <div class="toolbar">
    <!-- 網址輸入區 -->
    <div class="url-group">
      <input type="url" id="targetUrl" placeholder="貼上老師的網頁或遊戲網址 (https://...)">
      <button onclick="loadExternalPage()">1. 載入網頁</button>
    </div>

    <!-- 觸控修改按鈕區 -->
    <button id="btn-edit" class="blue" onclick="toggleTextEdit()">改文字</button>
    <button id="btn-img" class="purple" onclick="toggleImgEdit()">換圖片</button>
    <button class="orange" onclick="addFloatingText()">圖片加字</button>
    <label class="color-label">背景：<input type="color" id="bgColor" value="#ffffff" onchange="changeBgColor()"></label>
    <button onclick="downloadModifiedHTML()">下載成品</button>

    <div id="status">請在上方輸入框貼上網址，並點擊「1. 載入網頁」</div>
    <input type="file" id="imgUploader" accept="image/*" style="display:none;">
  </div>

  <!-- 載入後的網頁顯示區 -->
  <iframe id="previewFrame"></iframe>

  <script>
    const iframe = document.getElementById('previewFrame');
    const statusText = document.getElementById('status');
    const uploader = document.getElementById('imgUploader');
    let editMode = false;
    let imgMode = false;
    let targetImg = null;

    // 雙線路自動抓取外部網頁原始碼
    async function loadExternalPage() {
      const url = document.getElementById('targetUrl').value.trim();
      if (!url.startsWith('http')) {
        alert('請輸入包含 http:// 或 https:// 的完整網址');
        return;
      }

      statusText.textContent = '正在讀取網頁原始碼，請稍候...';

      // 備援代理清單
      const proxies = [
        async (target) => {
          const res = await fetch(`https://api.allorigins.win/get?url=${encodeURIComponent(target)}`);
          const data = await res.json();
          return data.contents;
        },
        async (target) => {
          const res = await fetch(`https://corsproxy.io/?${encodeURIComponent(target)}`);
          return await res.text();
        }
      ];

      let htmlContent = null;
      for (let fetchProxy of proxies) {
        try {
          htmlContent = await fetchProxy(url);
          if (htmlContent && htmlContent.length > 50) break;
        } catch (e) {
          console.log('切換備援線路...');
        }
      }

      if (!htmlContent) {
        statusText.textContent = '讀取失敗：該網站有防抓取保護，或網址無法連線。';
        alert('無法直接抓取此網址（可能受網站安全限制）。建議改用電腦瀏覽器「另存完整網頁」後再上傳到 GitHub。');
        return;
      }

      // 注入 <base> 標籤確保圖片、CSS 與相對路徑資源正常顯示
      const baseTag = `<base href="${url}">`;
      if (htmlContent.includes('<head>')) {
        htmlContent = htmlContent.replace('<head>', `<head>\n${baseTag}`);
      } else {
        htmlContent = baseTag + htmlContent;
      }

      // 寫入同源 iframe 中，解除跨網域編輯限制
      const doc = iframe.contentDocument || iframe.contentWindow.document;
      doc.open();
      doc.write(htmlContent);
      doc.close();

      // 綁定 iframe 內部的點擊換圖監聽器
      setTimeout(() => bindIframeEvents(doc), 500);
      statusText.textContent = '載入成功！現在可以使用上方的按鈕修改內容了。';
    }

    function bindIframeEvents(doc) {
      doc.addEventListener('click', function(e) {
        if (!imgMode) return;
        e.preventDefault();
        e.stopPropagation();
        if (e.target.tagName === 'IMG') {
          targetImg = e.target;
          uploader.click();
        } else {
          alert('你點到的不是標準圖片（可能是背景圖或遊戲畫布），請改用「圖片加字」功能覆蓋！');
        }
      }, true);
    }

    // 切換點擊改字模式
    function toggleTextEdit() {
      const doc = iframe.contentDocument || iframe.contentWindow.document;
      if (!doc || !doc.body) return alert('請先載入網頁！');
      
      editMode = !editMode;
      doc.designMode = editMode ? 'on' : 'off';
      const btn = document.getElementById('btn-edit');
      btn.textContent = editMode ? '停止改字' : '改文字';
      btn.classList.toggle('active-mode', editMode);
      statusText.textContent = editMode ? '已開啟改字模式：直接點擊下方畫面文字即可編輯。' : '已關閉改字模式。';
    }

    // 切換點擊換圖片模式
    function toggleImgEdit() {
      const doc = iframe.contentDocument || iframe.contentWindow.document;
      if (!doc || !doc.body) return alert('請先載入網頁！');

      imgMode = !imgMode;
      const btn = document.getElementById('btn-img');
      btn.textContent = imgMode ? '請點圖片...' : '換圖片';
      btn.classList.toggle('active-mode', imgMode);
      if (imgMode) statusText.textContent = '請用手指點選下方畫面中要替換的圖片！';
    }

    // 處理相簿選圖並轉為 Base64 內嵌
    uploader.onchange = function(e) {
      const file = e.target.files[0];
      if (!file || !targetImg) return;
      const reader = new FileReader();
      reader.onload = function(ev) {
        targetImg.src = ev.target.result;
        targetImg.srcset = '';
        imgMode = false;
        const btn = document.getElementById('btn-img');
        btn.textContent = '換圖片';
        btn.classList.remove('active-mode');
        statusText.textContent = '圖片替換成功！';
      };
      reader.readAsDataURL(file);
    };

    // 在圖片或畫布上方新增可拖曳覆蓋文字框
    function addFloatingText() {
      const doc = iframe.contentDocument || iframe.contentWindow.document;
      if (!doc || !doc.body) return alert('請先載入網頁！');

      const txt = prompt('輸入要蓋在圖片上的新文字：', '新標題');
      if (!txt) return;

      const box = doc.createElement('div');
      box.textContent = txt;
      box.contentEditable = 'true';
      box.style.cssText = 'position:fixed;top:80px;left:40px;background:#ffffff;color:#000000;border:2px dashed #d97706;padding:10px 16px;font-size:22px;font-weight:bold;z-index:2147483646;border-radius:8px;box-shadow:0 2px 8px rgba(0,0,0,0.3);cursor:move;';
      
      // 支援 iPad 手指拖曳與電腦滑鼠拖曳
      box.ontouchmove = function(e) {
        const t = e.touches[0];
        box.style.left = (t.clientX - 40) + 'px';
        box.style.top = (t.clientY - 20) + 'px';
      };
      let isDrag = false, ox, oy;
      box.onmousedown = function(e) { isDrag = true; ox = e.clientX - box.offsetLeft; oy = e.clientY - box.offsetTop; };
      doc.onmousemove = function(e) { if (isDrag) { box.style.left = (e.clientX - ox) + 'px'; box.style.top = (e.clientY - oy) + 'px'; } };
      doc.onmouseup = function() { isDrag = false; };

      doc.body.appendChild(box);
      statusText.textContent = '已新增文字方塊，用手指拖曳到圖片文字上方遮蓋即可！';
    }

    // 更換背景顏色
    function changeBgColor() {
      const doc = iframe.contentDocument || iframe.contentWindow.document;
      if (!doc || !doc.body) return;
      doc.body.style.backgroundColor = document.getElementById('bgColor').value;
    }

    // 匯出修改後的完整 HTML 檔案
    function downloadModifiedHTML() {
      const doc = iframe.contentDocument || iframe.contentWindow.document;
      if (!doc || !doc.body) return alert('請先載入網頁！');

      doc.designMode = 'off';
      const fullHTML = "<!DOCTYPE html>\n" + doc.documentElement.outerHTML;
      const a = document.createElement('a');
      a.href = URL.createObjectURL(new Blob([fullHTML], { type: 'text/html' }));
      a.download = 'modified_game.html';
      a.click();
      statusText.textContent = '已匯出 modified_game.html！';
    }
  </script>
</body>
</html>
