<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>生産日報</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: sans-serif; background: #f5f7fb; min-height: 100vh; }
  .header { background: #1a6fd4; color: #fff; padding: 16px 20px; }
  .header h1 { font-size: 18px; }
  .header p { font-size: 12px; opacity: 0.8; }
  .tabs { background: #fff; border-bottom: 1px solid #e0e0e0; display: flex; }
  .tab { padding: 10px 20px; border: none; border-bottom: 3px solid transparent; background: none; cursor: pointer; font-size: 14px; color: #555; }
  .tab.active { border-bottom-color: #1a6fd4; color: #1a6fd4; font-weight: 700; }
  .content { padding: 16px; max-width: 520px; margin: 0 auto; }
  .card { background: #fff; border-radius: 10px; padding: 16px; box-shadow: 0 1px 4px rgba(0,0,0,0.08); margin-bottom: 12px; }
  label { display: block; font-size: 12px; color: #555; margin-bottom: 3px; margin-top: 10px; }
  input, select, textarea { width: 100%; padding: 7px 10px; border: 1px solid #ccc; border-radius: 6px; font-size: 14px; }
  .row { display: flex; gap: 8px; }
  .row > div { flex: 1; }
  .pair { display: flex; gap: 8px; margin-top: 10px; }
  .pair > div { flex: 1; }
  .section-title { font-size: 12px; color: #888; margin: 12px 0 4px; border-top: 1px solid #eee; padding-top: 10px; }
  .btn { width: 100%; padding: 12px; background: #1a6fd4; color: #fff; border: none; border-radius: 8px; font-size: 16px; font-weight: 700; cursor: pointer; margin-top: 12px; }
  .btn:disabled { background: #aaa; cursor: not-allowed; }
  .btn-outline { background: #fff; border: 1px solid #1a6fd4; color: #1a6fd4; padding: 8px 16px; border-radius: 6px; cursor: pointer; font-size: 13px; margin-bottom: 12px; }
  .msg-ok { text-align: center; color: #2e7d32; margin-top: 10px; font-weight: 600; }
  .msg-err { text-align: center; color: #c62828; margin-top: 10px; font-weight: 600; }
  .badge { background: #1a6fd4; color: #fff; border-radius: 10px; padding: 1px 7px; font-size: 11px; margin-left: 4px; }
  .record-header { display: flex; justify-content: space-between; align-items: flex-start; }
  .record-date { font-weight: 700; color: #1a6fd4; }
  .record-process { background: #e3f0ff; color: #1a6fd4; border-radius: 4px; padding: 2px 8px; font-size: 12px; margin-left: 8px; }
  .grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 6px; margin-top: 8px; }
  .cell { background: #f5f7fb; border-radius: 6px; padding: 6px 8px; }
  .cell-label { font-size: 10px; color: #888; }
  .cell-value { font-size: 13px; font-weight: 600; }
  .note-box { margin-top: 8px; font-size: 12px; color: #555; background: #fffde7; border-radius: 6px; padding: 6px 8px; }
  .del-btn { border: none; background: none; color: #ccc; cursor: pointer; font-size: 18px; }
  .empty { text-align: center; color: #aaa; padding: 40px; }
  textarea { height: 70px; resize: vertical; }
</style>
</head>
<body>

<div class="header">
  <h1>📋 生産日報</h1>
  <p>現場入力アプリ</p>
</div>

<div class="tabs">
  <button class="tab active" onclick="showTab('input')">入力</button>
  <button class="tab" onclick="showTab('list')" id="list-tab">一覧</button>
</div>

<div class="content">
  <!-- 入力タブ -->
  <div id="tab-input">
    <div class="card">
      <label>📅 生産日 *</label>
      <input type="date" id="date">

      <label>⚙️ 工程 *</label>
      <select id="process">
        <option value="">選択してください</option>
        <option>工程A</option>
        <option>工程B</option>
        <option>工程C</option>
        <option>工程D</option>
        <option>工程E</option>
      </select>

      <div class="row">
        <div><label>📦 生産数</label><input type="number" id="production" value="0" min="0"></div>
        <div><label>👥 総人員</label><input type="number" id="total_staff" value="0" min="0"></div>
        <div><label>⏱ 時間(h)</label><input type="number" id="hours" value="0" min="0" step="0.5"></div>
      </div>

      <div class="section-title">詳細</div>

      <label>🌙 残業</label>
      <div class="pair">
        <div><input type="number" id="overtime_staff" value="0" min="0" placeholder="人数"></div>
        <div><input type="number" id="overtime_hours" value="0" min="0" step="0.5" placeholder="時間(h)"></div>
      </div>

      <label>🤝 応援受け入れ</label>
      <div class="pair">
        <div><input type="number" id="support_in_staff" value="0" min="0" placeholder="人数"></div>
        <div><input type="number" id="support_in_hours" value="0" min="0" step="0.5" placeholder="時間(h)"></div>
      </div>

      <label>➡️ 応援出し</label>
      <div class="pair">
        <div><input type="number" id="support_out_staff" value="0" min="0" placeholder="人数"></div>
        <div><input type="number" id="support_out_hours" value="0" min="0" step="0.5" placeholder="時間(h)"></div>
      </div>

      <label>⏸ 停止</label>
      <div class="pair">
        <div><input type="number" id="stop_staff" value="0" min="0" placeholder="人数"></div>
        <div><input type="number" id="stop_hours" value="0" min="0" step="0.5" placeholder="時間(h)"></div>
      </div>

      <label>📝 備考</label>
      <textarea id="note" placeholder="コメントを入力..."></textarea>

      <button class="btn" id="submit-btn" onclick="submitForm()">登録する</button>
      <div id="msg"></div>
    </div>
  </div>

  <!-- 一覧タブ -->
  <div id="tab-list" style="display:none;">
    <button class="btn-outline" onclick="exportTSV()">📥 Excelダウンロード（TSV）</button>
    <div id="records"></div>
  </div>
</div>

<script>
const GAS_URL = "https://script.google.com/macros/s/AKfycbzKUrk_ITe4oJXsZu3axaoodcGHqUgGAMrh7lpgLSk5_CExwANA1usGNzMi839wiphL/exec";
let records = [];

// 今日の日付をセット
document.getElementById("date").value = new Date().toISOString().slice(0, 10);

function showTab(name) {
  document.getElementById("tab-input").style.display = name === "input" ? "" : "none";
  document.getElementById("tab-list").style.display = name === "list" ? "" : "none";
  document.querySelectorAll(".tab").forEach((t, i) => {
    t.classList.toggle("active", (i === 0 && name === "input") || (i === 1 && name === "list"));
  });
  if (name === "list") renderRecords();
}

function getForm() {
  return {
    date: document.getElementById("date").value,
    process: document.getElementById("process").value,
    production: Number(document.getElementById("production").value),
    total_staff: Number(document.getElementById("total_staff").value),
    hours: Number(document.getElementById("hours").value),
    overtime_staff: Number(document.getElementById("overtime_staff").value),
    overtime_hours: Number(document.getElementById("overtime_hours").value),
    support_in_staff: Number(document.getElementById("support_in_staff").value),
    support_in_hours: Number(document.getElementById("support_in_hours").value),
    support_out_staff: Number(document.getElementById("support_out_staff").value),
    support_out_hours: Number(document.getElementById("support_out_hours").value),
    stop_staff: Number(document.getElementById("stop_staff").value),
    stop_hours: Number(document.getElementById("stop_hours").value),
    note: document.getElementById("note").value,
  };
}

function resetForm(date, process) {
  const fields = ["production","total_staff","hours","overtime_staff","overtime_hours",
    "support_in_staff","support_in_hours","support_out_staff","support_out_hours",
    "stop_staff","stop_hours"];
  fields.forEach(f => document.getElementById(f).value = 0);
  document.getElementById("note").value = "";
  document.getElementById("date").value = date;
  document.getElementById("process").value = process;
}

async function submitForm() {
  const data = getForm();
  if (!data.date || !data.process) { alert("日付と工程は必須です"); return; }

  const btn = document.getElementById("submit-btn");
  const msg = document.getElementById("msg");
  btn.disabled = true;
  btn.textContent = "送信中...";
  msg.innerHTML = "";

  try {
    await fetch(GAS_URL, {
  method: "POST",
  mode: "no-cors",
  headers: { "Content-Type": "text/plain" },
  body: JSON.stringify(data),
});
    records.unshift(data);
    msg.innerHTML = '<div class="msg-ok">✅ スプレッドシートに送信しました！</div>';
    resetForm(data.date, data.process);
    document.getElementById("list-tab").innerHTML = `一覧<span class="badge">${records.length}</span>`;
  } catch(e) {
    msg.innerHTML = '<div class="msg-err">⚠️ 送信に失敗しました。ネットワークを確認してください。</div>';
  } finally {
    btn.disabled = false;
    btn.textContent = "登録する";
    setTimeout(() => msg.innerHTML = "", 3000);
  }
}

function deleteRecord(i) {
  records.splice(i, 1);
  renderRecords();
  document.getElementById("list-tab").innerHTML = records.length
    ? `一覧<span class="badge">${records.length}</span>` : "一覧";
}

function renderRecords() {
  const el = document.getElementById("records");
  if (records.length === 0) { el.innerHTML = '<div class="empty">まだ登録データがありません</div>'; return; }
  el.innerHTML = records.map((r, i) => `
    <div class="card">
      <div class="record-header">
        <div><span class="record-date">${r.date}</span><span class="record-process">${r.process}</span></div>
        <button class="del-btn" onclick="deleteRecord(${i})">×</button>
      </div>
      <div class="grid">
        ${[["生産数",r.production,"個"],["総人員",r.total_staff,"人"],["時間",r.hours,"h"],
           ["残業",`${r.overtime_staff}人/${r.overtime_hours}h`,""],
           ["応援受",`${r.support_in_staff}人/${r.support_in_hours}h`,""],
           ["応援出",`${r.support_out_staff}人/${r.support_out_hours}h`,""],
           ["停止",`${r.stop_staff}人/${r.stop_hours}h`,""]]
          .map(([k,v,u]) => `<div class="cell"><div class="cell-label">${k}</div><div class="cell-value">${v}<span style="font-size:10px;color:#888">${u}</span></div></div>`).join("")}
      </div>
      ${r.note ? `<div class="note-box">📝 ${r.note}</div>` : ""}
    </div>
  `).join("");
}

function exportTSV() {
  const h = ["日付","工程","生産数","総人員","時間(h)","残業人数","残業時間(h)","応援受入人数","応援受入時間(h)","応援出し人数","応援出し時間(h)","停止人数","停止時間(h)","備考"];
  const rows = records.map(r => [r.date,r.process,r.production,r.total_staff,r.hours,
    r.overtime_staff,r.overtime_hours,r.support_in_staff,r.support_in_hours,
    r.support_out_staff,r.support_out_hours,r.stop_staff,r.stop_hours,
    (r.note||"").replace(/\t/g," ")]);
  const tsv = [h,...rows].map(r => r.join("\t")).join("\r\n");
  const blob = new Blob(["\uFEFF"+tsv], {type:"text/tab-separated-values;charset=utf-8"});
  const a = document.createElement("a");
  a.href = URL.createObjectURL(blob);
  a.download = "生産日報.tsv";
  a.click();
}
</script>
</body>
</html>
