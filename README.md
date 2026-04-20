[index.html](https://github.com/user-attachments/files/26898445/index.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>YC 庫存盤點系統</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:-apple-system,BlinkMacSystemFont,"Microsoft JhengHei","Noto Sans TC",sans-serif;background:#f0f2f5;min-height:100vh;}
.nav{background:#1a1a2e;padding:12px 16px;display:flex;align-items:center;justify-content:space-between;}
.nav-title{color:#fff;font-size:17px;font-weight:700;}
.nav-badge{font-size:11px;padding:3px 10px;border-radius:99px;font-weight:600;}
.nav-badge.online{background:#d1fae5;color:#065f46;}
.nav-badge.offline{background:#fee2e2;color:#991b1b;}
.nav-badge.loading{background:#fef3c7;color:#92400e;}
.tabs{display:flex;background:#fff;border-bottom:1px solid #e5e7eb;}
.tab{flex:1;padding:12px 0;text-align:center;font-size:14px;font-weight:600;color:#888;cursor:pointer;border-bottom:3px solid transparent;transition:all 0.15s;}
.tab.active{color:#1D9E75;border-bottom-color:#1D9E75;}
.page{display:none;padding:16px;max-width:500px;margin:0 auto;}
.page.active{display:block;}
.card{background:#fff;border-radius:14px;padding:16px 18px;margin-bottom:14px;box-shadow:0 1px 4px rgba(0,0,0,0.08);}
.card-title{font-size:11px;font-weight:700;color:#888;letter-spacing:0.08em;text-transform:uppercase;margin-bottom:12px;}

.scanner-wrap{position:relative;width:100%;background:#000;border-radius:12px;overflow:hidden;margin-bottom:12px;display:none;}
#preview{width:100%;height:260px;object-fit:cover;display:block;}
.scanner-overlay{position:absolute;top:0;left:0;right:0;bottom:0;display:flex;align-items:center;justify-content:center;pointer-events:none;}
.scanner-frame{width:200px;height:200px;border:2px solid #1D9E75;border-radius:12px;box-shadow:0 0 0 9999px rgba(0,0,0,0.45);}
.scanner-line{position:absolute;width:180px;height:2px;background:linear-gradient(90deg,transparent,#1D9E75,transparent);animation:scan 2s linear infinite;}
@keyframes scan{0%{top:calc(50% - 90px)}100%{top:calc(50% + 90px)}}
.scanner-status{position:absolute;bottom:12px;left:0;right:0;text-align:center;color:#fff;font-size:13px;background:rgba(0,0,0,0.5);padding:6px;}
.btn-scan{width:100%;height:50px;font-size:16px;font-weight:700;border:none;border-radius:10px;cursor:pointer;transition:all 0.15s;margin-bottom:10px;}
.btn-scan.start{background:#1D9E75;color:#fff;}
.btn-scan.stop{background:#dc2626;color:#fff;}
.divider{display:flex;align-items:center;gap:10px;margin:10px 0;color:#aaa;font-size:13px;}
.divider::before,.divider::after{content:'';flex:1;height:1px;background:#e5e7eb;}
#manualInput{width:100%;height:52px;font-size:20px;font-family:"Courier New",monospace;border:2px solid #d1d5db;border-radius:10px;padding:0 14px;color:#1a1a1a;background:#fff;letter-spacing:0.04em;transition:border-color 0.15s;}
#manualInput:focus{outline:none;border-color:#1D9E75;box-shadow:0 0 0 3px rgba(29,158,117,0.15);}
#manualInput::placeholder{font-size:15px;letter-spacing:0;color:#aaa;font-family:inherit;}

.product-found{background:#f0fdf8;border:1.5px solid #6ee7b7;border-radius:10px;padding:14px;margin-bottom:12px;display:none;}
.pf-label{font-size:11px;color:#059669;margin-bottom:2px;font-weight:600;}
.pf-pn{font-size:18px;font-weight:700;font-family:"Courier New",monospace;color:#1a1a1a;}
.pf-name{font-size:15px;color:#374151;margin-top:6px;font-weight:600;}

.count-wrap{display:none;margin-bottom:12px;}
.count-label{font-size:14px;font-weight:600;color:#374151;margin-bottom:8px;}
#countInput{width:100%;height:64px;font-size:32px;font-weight:700;font-family:"Courier New",monospace;border:2px solid #d1d5db;border-radius:10px;padding:0 14px;color:#1a1a1a;background:#fff;text-align:center;transition:border-color 0.15s;}
#countInput:focus{outline:none;border-color:#2563eb;box-shadow:0 0 0 3px rgba(37,99,235,0.15);}
#countInput::placeholder{font-size:18px;font-weight:400;color:#bbb;}
#confirmBtn{width:100%;height:54px;margin-top:10px;font-size:19px;font-weight:700;color:#fff;background:#2563eb;border:none;border-radius:10px;cursor:pointer;transition:all 0.15s;}
#confirmBtn:hover{background:#1d4ed8;}
#confirmBtn:active{transform:scale(0.98);}

.result-empty{text-align:center;padding:24px;color:#bbb;font-size:15px;}
.result-notfound{text-align:center;padding:28px 16px;border-radius:10px;background:#fff1f1;border:2px solid #f87171;}
.notfound-icon{font-size:48px;margin-bottom:10px;}
.notfound-title{font-size:22px;font-weight:800;color:#b91c1c;margin-bottom:6px;}
.notfound-code{font-size:16px;color:#dc2626;font-family:"Courier New",monospace;background:#fee2e2;padding:4px 10px;border-radius:6px;display:inline-block;margin-bottom:6px;}
.notfound-sub{font-size:14px;color:#ef4444;}

.result-compare{border-radius:10px;overflow:hidden;border:1.5px solid #e5e7eb;}
.compare-header{padding:14px 16px;display:flex;align-items:center;gap:12px;}
.compare-header.ok{background:#d1fae5;border-bottom:1.5px solid #6ee7b7;}
.compare-header.diff{background:#fee2e2;border-bottom:1.5px solid #fca5a5;}
.compare-icon{font-size:32px;}
.compare-title{font-size:20px;font-weight:800;}
.compare-header.ok .compare-title{color:#065f46;}
.compare-header.diff .compare-title{color:#991b1b;}
.compare-sub{font-size:13px;margin-top:2px;}
.compare-header.ok .compare-sub{color:#059669;}
.compare-header.diff .compare-sub{color:#dc2626;}
.compare-body{padding:0 16px;}
.cr{display:flex;justify-content:space-between;align-items:center;padding:13px 0;border-bottom:1px solid #f3f4f6;}
.cr:last-child{border-bottom:none;}
.cr-key{font-size:14px;color:#666;}
.cr-val{font-size:15px;font-weight:700;text-align:right;}
.cr-val.big{font-size:30px;}
.cr-val.ok{color:#059669;}
.cr-val.diff{color:#dc2626;}
.cr-val.warn{color:#d97706;}
.cr-val.normal{color:#1a1a1a;}
.unit{font-size:16px;font-weight:400;color:#888;margin-left:4px;}

.safe-alert{background:#7f1d1d;border-radius:8px;padding:10px 14px;margin:10px 16px;display:flex;align-items:center;gap:10px;}
.safe-alert-text{font-size:13px;color:#fca5a5;font-weight:600;}

.btn-row{display:flex;gap:10px;margin-top:12px;}
.btn-next{flex:1;height:46px;font-size:15px;font-weight:700;color:#fff;background:#1D9E75;border:none;border-radius:10px;cursor:pointer;}
.btn-next:hover{background:#17876a;}

.summary{display:none;background:#1a1a2e;border-radius:12px;padding:14px 16px;margin-bottom:14px;}
.summary.visible{display:flex;}
.sum-item{flex:1;text-align:center;}
.sum-item+.sum-item{border-left:1px solid rgba(255,255,255,0.1);}
.sum-num{font-size:28px;font-weight:800;color:#fff;}
.sum-num.ok{color:#34d399;}
.sum-num.diff{color:#f87171;}
.sum-label{font-size:11px;color:rgba(255,255,255,0.5);margin-top:2px;}

.hr{display:flex;justify-content:space-between;align-items:center;padding:9px 6px;border-bottom:1px solid #f3f4f6;border-radius:8px;}
.hr:last-child{border-bottom:none;}
.hr-pn{font-size:13px;font-family:"Courier New",monospace;font-weight:600;color:#1a1a1a;}
.hr-name{font-size:11px;color:#888;margin-top:1px;}
.hr-badge{font-size:11px;padding:3px 10px;border-radius:99px;font-weight:700;}
.hr-badge.ok{background:#d1fae5;color:#065f46;}
.hr-badge.diff{background:#fee2e2;color:#991b1b;}

.upload-box{border:2px dashed #c8ccd0;border-radius:10px;padding:22px 16px;text-align:center;cursor:pointer;transition:all 0.15s;}
.upload-box:hover{border-color:#1D9E75;background:#f0fdf8;}
.upload-box.loaded{border:2px solid #1D9E75;background:#e8faf3;}
.upload-icon{font-size:30px;margin-bottom:6px;}
.upload-main{font-size:15px;font-weight:600;color:#333;}
.upload-sub{font-size:13px;color:#999;margin-top:4px;}
.upload-badge{display:inline-block;margin-top:8px;font-size:12px;padding:4px 12px;border-radius:99px;font-weight:600;}
.badge-ok{background:#d1fae5;color:#065f46;}
.badge-err{background:#fee2e2;color:#991b1b;}
.btn-upload{width:100%;height:50px;font-size:16px;font-weight:700;color:#fff;background:#2563eb;border:none;border-radius:10px;cursor:pointer;margin-top:12px;transition:all 0.15s;display:none;}
.btn-upload:hover{background:#1d4ed8;}
.btn-upload:disabled{background:#93c5fd;cursor:not-allowed;}
.info-box{background:#f0f9ff;border:1px solid #bae6fd;border-radius:10px;padding:12px 14px;margin-bottom:12px;font-size:13px;color:#0369a1;line-height:1.8;}

.records-table{width:100%;border-collapse:collapse;font-size:13px;margin-top:8px;}
.records-table th{background:#f8f9fa;padding:8px 10px;text-align:left;font-weight:600;color:#555;border-bottom:1px solid #e5e7eb;}
.records-table td{padding:8px 10px;border-bottom:1px solid #f3f4f6;color:#1a1a1a;}
.records-table tr:last-child td{border-bottom:none;}
.tag{font-size:11px;padding:2px 8px;border-radius:99px;font-weight:600;}
.tag.ok{background:#d1fae5;color:#065f46;}
.tag.diff{background:#fee2e2;color:#991b1b;}
.btn-export{width:100%;height:44px;font-size:14px;font-weight:700;color:#1D9E75;background:#fff;border:2px solid #1D9E75;border-radius:10px;cursor:pointer;margin-top:12px;}
.btn-export:hover{background:#f0fdf8;}

@media(max-width:400px){
  #preview{height:220px;}
  .scanner-frame{width:170px;height:170px;}
  #countInput{height:58px;font-size:28px;}
}
</style>
</head>
<body>

<div class="nav">
  <div class="nav-title">📦 YC 庫存盤點</div>
  <div class="nav-badge loading" id="connBadge">連線中...</div>
</div>

<div class="tabs">
  <div class="tab active" onclick="showPage('scan',this)">📱 盤點</div>
  <div class="tab" onclick="showPage('admin',this)">⚙️ 管理</div>
</div>

<div class="page active" id="page-scan">
  <div class="summary" id="summaryBar">
    <div class="sum-item"><div class="sum-num" id="sumTotal">0</div><div class="sum-label">已盤件數</div></div>
    <div class="sum-item"><div class="sum-num ok" id="sumOk">0</div><div class="sum-label">數量正確</div></div>
    <div class="sum-item"><div class="sum-num diff" id="sumDiff">0</div><div class="sum-label">有差異</div></div>
  </div>

  <div class="card">
    <div class="card-title">掃描條碼</div>
    <div class="scanner-wrap" id="scannerWrap">
      <video id="preview" playsinline autoplay muted></video>
      <div class="scanner-overlay"><div class="scanner-frame"></div><div class="scanner-line"></div></div>
      <div class="scanner-status" id="scannerStatus">對準條碼掃描...</div>
    </div>
    <button class="btn-scan start" id="btnStartScan" onclick="startScan()">📷 開啟鏡頭掃描</button>
    <button class="btn-scan stop" id="btnStopScan" onclick="stopScan()" style="display:none">✕ 關閉鏡頭</button>
    <div class="divider">或手動輸入</div>
    <input type="text" id="manualInput" placeholder="輸入產品編號..." autocomplete="off">
  </div>

  <div class="card" id="productCard" style="display:none">
    <div class="card-title">找到產品</div>
    <div class="product-found" id="productFound" style="display:block">
      <div class="pf-label">產品編號</div>
      <div class="pf-pn" id="pfPN"></div>
      <div class="pf-name" id="pfName"></div>
    </div>
    <div class="count-wrap" id="countWrap" style="display:block">
      <div class="count-label">請輸入實際盤點數量：</div>
      <input type="number" id="countInput" placeholder="輸入數量..." min="0" autocomplete="off">
      <button id="confirmBtn" onclick="confirmCount()">✔ 確認送出</button>
    </div>
  </div>

  <div class="card">
    <div class="card-title">比對結果</div>
    <div id="resultArea"><div class="result-empty">等待掃描...</div></div>
    <div class="btn-row" id="btnRow" style="display:none">
      <button class="btn-next" onclick="nextItem()">➡ 下一筆</button>
    </div>
  </div>

  <div class="card" id="histCard" style="display:none">
    <div class="card-title">本次盤點紀錄</div>
    <div id="histArea"></div>
  </div>
</div>

<div class="page" id="page-admin">
  <div class="info-box">
    📋 上傳最新庫存 Excel，員工端立即同步。<br>
    支援欄位：<b>產品編號、品名規格、現有數量、安全存量</b><br>
    ⚠️ 現有數量低於安全存量時會顯示<b style="color:#b91c1c">紅色警示</b>
  </div>

  <div class="card">
    <div class="card-title">上傳庫存 Excel</div>
    <div class="upload-box" id="uploadBox" onclick="document.getElementById('fileInput').click()">
      <div class="upload-icon">📋</div>
      <div class="upload-main" id="uploadMain">點此選擇 Excel 庫存檔</div>
      <div class="upload-sub" id="uploadSub">.xlsx / .xls 格式</div>
      <div id="uploadBadge"></div>
    </div>
    <input type="file" id="fileInput" accept=".xlsx,.xls,.csv" style="display:none">
    <button class="btn-upload" id="btnUpload" onclick="uploadInventory()">⬆ 上傳到雲端（員工立即可用）</button>
  </div>

  <div class="card" id="recordsCard" style="display:none">
    <div class="card-title">盤點結果 <span id="recordCount" style="font-weight:400;text-transform:none;font-size:12px;color:#888"></span></div>
    <div id="recordsArea"></div>
    <button class="btn-export" onclick="exportExcel()">⬇ 匯出 Excel</button>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://unpkg.com/@zxing/library@0.19.1/umd/index.min.js"></script>
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getFirestore, collection, getDocs, setDoc, doc, onSnapshot, writeBatch } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyB7yTHdpvxUf1AXZ3N24sZzKx__ipEQGD4",
  authDomain: "yc-inventory.firebaseapp.com",
  projectId: "yc-inventory",
  storageBucket: "yc-inventory.firebasestorage.app",
  messagingSenderId: "591513427308",
  appId: "1:591513427308:web:a981ebe0bfba56003b6749"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
let inventory = [];

async function loadInventory() {
  try {
    const snap = await getDocs(collection(db, 'inventory'));
    inventory = [];
    snap.forEach(d => inventory.push(d.data()));
    window._inventory = inventory;
    document.getElementById('connBadge').textContent = '已連線（' + inventory.length + ' 筆）';
    document.getElementById('connBadge').className = 'nav-badge online';
  } catch(e) {
    document.getElementById('connBadge').textContent = '連線失敗';
    document.getElementById('connBadge').className = 'nav-badge offline';
  }
}
loadInventory();
window._getInventory = () => inventory;
window._loadInventory = loadInventory;

onSnapshot(collection(db, 'records'), (snap) => {
  const records = [];
  snap.forEach(d => records.push({id: d.id, ...d.data()}));
  window._records = records;
  renderAdminRecords(records);
  updateSummary(records);
});

function renderAdminRecords(records) {
  const card = document.getElementById('recordsCard');
  const area = document.getElementById('recordsArea');
  const count = document.getElementById('recordCount');
  if (!records.length) { card.style.display='none'; return; }
  card.style.display = 'block';
  count.textContent = '共 ' + records.length + ' 筆';
  const sorted = [...records].sort((a,b) => (b.time||0)-(a.time||0));
  area.innerHTML = '<div style="overflow-x:auto"><table class="records-table"><thead><tr><th>產品編號</th><th>品名</th><th>系統</th><th>盤點</th><th>差異</th><th>結果</th></tr></thead><tbody>'
    + sorted.map(r => '<tr>'
      +'<td style="font-family:monospace">'+r.pn+'</td>'
      +'<td>'+r.name+'</td>'
      +'<td>'+(r.sysQty!==null?r.sysQty:'—')+'</td>'
      +'<td>'+r.counted+'</td>'
      +'<td>'+(r.diff!==null?(r.diff>0?'+'+r.diff:r.diff):'—')+'</td>'
      +'<td><span class="tag '+(r.isOk?'ok':'diff')+'">'+(r.isOk?'正確':'差異')+'</span></td>'
      +'</tr>'
    ).join('')+'</tbody></table></div>';
}

function updateSummary(records) {
  if (!records.length) return;
  document.getElementById('summaryBar').classList.add('visible');
  document.getElementById('sumTotal').textContent = records.length;
  document.getElementById('sumOk').textContent = records.filter(r=>r.isOk).length;
  document.getElementById('sumDiff').textContent = records.filter(r=>!r.isOk).length;
}

window._saveRecord = async function(rec) {
  await setDoc(doc(collection(db, 'records'), rec.pn), rec);
};

window.exportExcel = function() {
  const records = window._records || [];
  if (!records.length) { alert('尚無盤點資料'); return; }
  const wsData = [['產品編號','品名規格','系統庫存','盤點數量','差異','結果']];
  records.forEach(r => wsData.push([r.pn, r.name, r.sysQty??'', r.counted, r.diff??'', r.isOk?'正確':'有差異']));
  const ws = XLSX.utils.aoa_to_sheet(wsData);
  ws['!cols'] = [{wch:14},{wch:28},{wch:10},{wch:10},{wch:8},{wch:8}];
  const wb2 = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb2, ws, '盤點結果');
  const now = new Date();
  XLSX.writeFile(wb2, '盤點結果_'+now.getFullYear()+String(now.getMonth()+1).padStart(2,'0')+String(now.getDate()).padStart(2,'0')+'.xlsx');
};

window._uploadToFirebase = async function(inv) {
  const batch = writeBatch(db);
  inv.forEach((item, i) => {
    const ref = doc(collection(db, 'inventory'), 'item_' + i);
    batch.set(ref, item);
  });
  await batch.commit();
  await loadInventory();
};
</script>

<script>
function showPage(p, el) {
  document.querySelectorAll('.page').forEach(x => x.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(x => x.classList.remove('active'));
  document.getElementById('page-' + p).classList.add('active');
  el.classList.add('active');
}

let codeReader = null;
function startScan() {
  document.getElementById('scannerWrap').style.display = 'block';
  document.getElementById('btnStartScan').style.display = 'none';
  document.getElementById('btnStopScan').style.display = 'block';
  if (typeof ZXing === 'undefined') { setTimeout(startScan, 500); return; }
  codeReader = new ZXing.BrowserMultiFormatReader();
  codeReader.decodeFromVideoDevice(null, 'preview', (result) => {
    if (result) {
      document.getElementById('scannerStatus').textContent = '✓ ' + result.getText();
      stopScan();
      handleCode(result.getText());
    }
  });
}
function stopScan() {
  if (codeReader) { codeReader.reset(); codeReader = null; }
  document.getElementById('scannerWrap').style.display = 'none';
  document.getElementById('btnStartScan').style.display = 'block';
  document.getElementById('btnStopScan').style.display = 'none';
}

let manualTimer, currentItem = null;
document.getElementById('manualInput').addEventListener('input', function() {
  clearTimeout(manualTimer);
  const q = this.value.trim();
  if (q) manualTimer = setTimeout(() => handleCode(q), 500);
});
document.getElementById('manualInput').addEventListener('keydown', function(e) {
  if (e.key === 'Enter') { clearTimeout(manualTimer); handleCode(this.value.trim()); }
});

function handleCode(code) {
  if (!code) return;
  document.getElementById('manualInput').value = code;
  const inv = window._getInventory ? window._getInventory() : [];
  if (!inv.length) { document.getElementById('resultArea').innerHTML = '<div class="result-notfound"><div class="notfound-title">請管理員先上傳庫存</div></div>'; return; }
  const match = inv.find(r => String(r.pn||'').trim().toLowerCase() === code.toLowerCase());
  if (!match) {
    currentItem = null;
    document.getElementById('productCard').style.display = 'none';
    document.getElementById('btnRow').style.display = 'none';
    document.getElementById('resultArea').innerHTML =
      '<div class="result-notfound"><div class="notfound-icon">❌</div>'
      +'<div class="notfound-title">查無此料號</div>'
      +'<div class="notfound-code">'+code+'</div>'
      +'<div class="notfound-sub">請確認條碼是否正確</div></div>';
    return;
  }
  currentItem = match;
  document.getElementById('pfPN').textContent = match.pn;
  document.getElementById('pfName').textContent = match.name ? '📌 ' + match.name : '';
  document.getElementById('productCard').style.display = 'block';
  document.getElementById('countInput').value = '';
  document.getElementById('resultArea').innerHTML = '<div class="result-empty">請輸入盤點數量後送出</div>';
  document.getElementById('btnRow').style.display = 'none';
  document.getElementById('countInput').focus();
}

document.getElementById('countInput').addEventListener('keydown', function(e) {
  if (e.key === 'Enter') confirmCount();
});

async function confirmCount() {
  const val = document.getElementById('countInput').value.trim();
  if (val === '' || isNaN(val)) {
    document.getElementById('countInput').style.borderColor = '#ef4444';
    setTimeout(() => { document.getElementById('countInput').style.borderColor = '#d1d5db'; }, 1000);
    return;
  }
  const counted  = Math.round(parseFloat(val));
  const sysQty   = currentItem.qty !== null ? Math.round(parseFloat(currentItem.qty)||0) : null;
  const safeQty  = currentItem.safeQty !== null && currentItem.safeQty !== undefined ? Math.round(parseFloat(currentItem.safeQty)||0) : null;
  const isOk     = sysQty !== null && counted === sysQty;
  const diff     = sysQty !== null ? counted - sysQty : null;
  const isSafeDanger = safeQty !== null && counted < safeQty;

  const rec = { pn: currentItem.pn, name: currentItem.name||'', sysQty, counted, diff, isOk };

  const btn = document.getElementById('confirmBtn');
  btn.textContent = '儲存中...'; btn.disabled = true;
  try { await window._saveRecord(rec); } catch(e) {}
  btn.textContent = '✔ 確認送出'; btn.disabled = false;

  const cls  = diff===null?'normal':diff===0?'ok':diff>0?'diff':'warn';
  const tag  = diff===null?'—':diff===0?'± 0':diff>0?'多 '+diff+' 件':'少 '+Math.abs(diff)+' 件';

  document.getElementById('resultArea').innerHTML =
    '<div class="result-compare">'
    +'<div class="compare-header '+(isOk?'ok':'diff')+'">'
    +'<div class="compare-icon">'+(isOk?'✅':'⚠️')+'</div>'
    +'<div><div class="compare-title">'+(isOk?'數量正確！':'數量有差異！')+'</div>'
    +'<div class="compare-sub">'+(isOk?'系統與盤點一致':'請確認是否有誤計')+'</div></div>'
    +'</div>'
    +(isSafeDanger ? '<div class="safe-alert"><div style="font-size:20px">🚨</div><div class="safe-alert-text">盤點數量低於安全存量（'+safeQty+'件），請補貨！</div></div>' : '')
    +'<div class="compare-body">'
    +'<div class="cr"><span class="cr-key">品名規格</span><span class="cr-val normal">'+(currentItem.name||'—')+'</span></div>'
    +'<div class="cr"><span class="cr-key">系統庫存</span><span class="cr-val big normal">'+(sysQty!==null?sysQty:'—')+'<span class="unit">件</span></span></div>'
    +'<div class="cr"><span class="cr-key">盤點數量</span><span class="cr-val big '+(isOk?'ok':'diff')+'">'+counted+'<span class="unit">件</span></span></div>'
    +'<div class="cr"><span class="cr-key">差異</span><span class="cr-val big '+cls+'">'+tag+'</span></div>'
    +(safeQty!==null?'<div class="cr"><span class="cr-key">安全存量</span><span class="cr-val normal">'+safeQty+' 件</span></div>':'')
    +'</div></div>';

  document.getElementById('productCard').style.display = 'none';
  document.getElementById('btnRow').style.display = 'flex';
  addLocalHist(rec);
}

function nextItem() {
  document.getElementById('manualInput').value = '';
  document.getElementById('productCard').style.display = 'none';
  document.getElementById('btnRow').style.display = 'none';
  document.getElementById('resultArea').innerHTML = '<div class="result-empty">等待掃描...</div>';
  currentItem = null;
  document.getElementById('manualInput').focus();
}

let localHist = [];
function addLocalHist(rec) {
  localHist = localHist.filter(h => h.pn !== rec.pn);
  localHist.unshift(rec);
  if (localHist.length > 10) localHist.pop();
  document.getElementById('histCard').style.display = 'block';
  document.getElementById('histArea').innerHTML = localHist.map(h =>
    '<div class="hr"><div><div class="hr-pn">'+h.pn+'</div><div class="hr-name">'+h.name+'</div></div>'
    +'<span class="hr-badge '+(h.isOk?'ok':'diff')+'">'+(h.isOk?'✓ 正確':(h.diff>0?'多'+h.diff+'件':'少'+Math.abs(h.diff)+'件'))+'</span></div>'
  ).join('');
}

document.getElementById('fileInput').addEventListener('change', function(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = function(evt) {
    try {
      const wb = XLSX.read(new Uint8Array(evt.target.result), {type:'array', raw:false});
      const ws = wb.Sheets[wb.SheetNames[0]];
      const rows = XLSX.utils.sheet_to_json(ws, {header:1, raw:false});
      if (rows.length < 2) { setBadge('檔案內容不足', false); return; }
      const headers = rows[0].map(h => String(h||'').trim().replace(/\s+/g,''));
      const data = rows.slice(1).filter(r => r.some(c => c !== undefined && c !== ''));
      const colPN   = findCol(headers, ['產品編號','產品編','料號','品號','編號','barcode','code','sku']);
      const colName = findCol(headers, ['品名規格','品名','規格','名稱','name','description']);
      const colQty  = findCol(headers, ['現有數量','庫存數量','現有','庫存','數量','qty','quantity','stock']);
      const colSafe = findCol(headers, ['安全存量','安全庫存','最低庫存','最低存量','safestock','minstock','safety']);
      if (colPN < 0) { setBadge('找不到產品編號欄位', false); return; }
      window._pendingInventory = data.map(row => ({
        pn:      String(row[colPN]  || '').trim(),
        name:    colName >= 0 ? String(row[colName] || '').trim() : '',
        qty:     colQty  >= 0 ? (parseFloat(String(row[colQty] ||'').replace(/,/g,''))||0) : null,
        safeQty: colSafe >= 0 ? (parseFloat(String(row[colSafe]||'').replace(/,/g,''))||null) : null
      })).filter(r => r.pn);
      const hasSafe = colSafe >= 0;
      document.getElementById('uploadBox').classList.add('loaded');
      document.getElementById('uploadMain').textContent = file.name;
      document.getElementById('uploadSub').textContent = '共 ' + window._pendingInventory.length + ' 筆' + (hasSafe ? '（含安全存量✓）' : '');
      setBadge('讀取成功 ' + window._pendingInventory.length + ' 筆', true);
      document.getElementById('btnUpload').style.display = 'block';
    } catch(e) { setBadge('讀取失敗，請確認格式', false); }
  };
  reader.readAsArrayBuffer(file);
});

function findCol(headers, keys) { return headers.findIndex(h => keys.some(k => h.includes(k))); }
function setBadge(msg, ok) { document.getElementById('uploadBadge').innerHTML = '<span class="upload-badge '+(ok?'badge-ok':'badge-err')+'">'+msg+'</span>'; }

async function uploadInventory() {
  const inv = window._pendingInventory;
  if (!inv || !inv.length) return;
  const btn = document.getElementById('btnUpload');
  btn.disabled = true; btn.textContent = '上傳中...';
  try {
    await window._uploadToFirebase(inv);
    btn.textContent = '✓ 上傳成功！';
    btn.style.background = '#059669';
    setTimeout(() => { btn.textContent = '⬆ 上傳到雲端（員工立即可用）'; btn.disabled = false; btn.style.background = ''; }, 3000);
  } catch(e) { btn.textContent = '上傳失敗，請重試'; btn.disabled = false; }
}
</script>
</body>
</html>
