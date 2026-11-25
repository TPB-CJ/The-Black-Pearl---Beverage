# The-Black-Pearl---Beverage
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>The Black Pearl | Cellar & Bar Digital Menu</title>
  <script src="https://sheetjs.com"></script>
  <style>
    :root {
      --bg: #0b0d10; --panel: #13171d; --card: #181d25; --hover: #1f2631;
      --gold: #d4af37; --gold-l: #f3d982; --gold-bg: rgba(212,175,55,0.15);
      --text: #f4f5f7; --muted: #9aa3b2; --border: #28303d;
      --green: #2ecc71; --amber: #f39c12; --red: #e74c3c;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "PingFang SC", "Microsoft YaHei", sans-serif; background: var(--bg); color: var(--text); line-height: 1.5; }
    header { background: linear-gradient(180deg, #141820, #0b0d10); border-bottom: 1px solid var(--border); padding: 14px 20px; position: sticky; top: 0; z-index: 99; }
    .nav { max-width: 1380px; margin: 0 auto; display: flex; flex-wrap: wrap; align-items: center; justify-content: space-between; gap: 12px; }
    .brand { display: flex; align-items: center; gap: 12px; }
    .pearl { width: 36px; height: 36px; border-radius: 50%; background: radial-gradient(circle at 30% 30%, #fff, #7a8599 45%, #11151c 85%); border: 1px solid var(--gold); box-shadow: 0 0 12px rgba(212,175,55,0.4); }
    .brand h1 { font-size: 1.4rem; letter-spacing: 2px; color: var(--gold); text-transform: uppercase; }
    .brand small { display: block; font-size: 0.72rem; color: var(--muted); }
    .btns { display: flex; gap: 8px; flex-wrap: wrap; align-items: center; }
    .btn { background: var(--card); color: var(--text); border: 1px solid var(--border); padding: 7px 13px; border-radius: 8px; cursor: pointer; font-size: 0.82rem; transition: .2s; }
    .btn:hover { border-color: var(--gold); color: var(--gold-l); }
    .btn-gold { background: var(--gold-bg); border-color: var(--gold); color: var(--gold-l); font-weight: 600; }
    .btn-ok { background: rgba(46,204,113,0.16); border-color: var(--green); color: #7bed9f; }

    /* Admin Panel */
    #adminBox { display: none; max-width: 1380px; margin: 14px auto 0; padding: 16px; background: #141922; border: 1px solid var(--gold); border-radius: 12px; }
    #adminBox.show { display: block; }
    .admin-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 12px; margin-top: 10px; }
    .abox { background: var(--bg); padding: 12px; border-radius: 8px; border: 1px solid var(--border); }
    .abox label { display: block; font-size: 0.78rem; color: var(--gold-l); margin-bottom: 6px; }
    .abox input { width: 100%; background: var(--card); border: 1px solid var(--border); color: var(--text); padding: 7px 10px; border-radius: 6px; font-size: 0.82rem; margin-bottom: 6px; }

    /* Search & Filter */
    .wrap { max-width: 1380px; margin: 18px auto; padding: 0 20px; }
    .search-row { display: grid; grid-template-columns: 2fr 1fr 1fr; gap: 10px; margin-bottom: 12px; }
    @media (max-width: 768px) { .search-row { grid-template-columns: 1fr; } }
    .inp { background: var(--panel); border: 1px solid var(--border); color: var(--text); padding: 11px 14px; border-radius: 10px; font-size: 0.9rem; outline: none; width: 100%; }
    .inp:focus { border-color: var(--gold); }
    .pills { display: flex; gap: 7px; flex-wrap: wrap; margin-bottom: 12px; }
    .pill { padding: 5px 13px; border-radius: 20px; background: var(--panel); border: 1px solid var(--border); color: var(--muted); font-size: 0.8rem; cursor: pointer; }
    .pill.active, .pill:hover { background: var(--gold); color: #0b0d10; border-color: var(--gold); font-weight: 600; }
    .meta-bar { display: flex; justify-content: space-between; font-size: 0.8rem; color: var(--muted); padding-bottom: 8px; border-bottom: 1px solid var(--border); margin-bottom: 16px; flex-wrap: wrap; gap: 8px; }

    /* Cards */
    .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(330px, 1fr)); gap: 16px; margin-bottom: 40px; }
    .card { background: var(--card); border: 1px solid var(--border); border-radius: 14px; overflow: hidden; display: flex; flex-direction: column; transition: .2s; }
    .card:hover { transform: translateY(-2px); border-color: var(--gold); background: var(--hover); }
    .c-top { display: flex; gap: 12px; padding: 14px; border-bottom: 1px solid rgba(255,255,255,0.05); }
    .bottle { width: 72px; height: 110px; border-radius: 10px; background: radial-gradient(circle, #252d3a, #101318); border: 1px solid var(--border); display: flex; flex-direction: column; align-items: center; justify-content: center; flex-shrink: 0; position: relative; cursor: pointer; text-align: center; padding: 4px; }
    .bottle img { width: 100%; height: 100%; object-fit: contain; }
    .bottle-tip { font-size: 0.65rem; color: var(--gold-l); margin-top: 4px; }
    .c-info { flex: 1; min-width: 0; }
    .badges { display: flex; justify-content: space-between; align-items: center; gap: 6px; margin-bottom: 5px; }
    .code { font-family: monospace; font-size: 0.76rem; background: var(--gold-bg); color: var(--gold-l); padding: 2px 7px; border-radius: 5px; border: 1px solid rgba(212,175,55,0.35); }
    .stk { font-size: 0.74rem; padding: 2px 8px; border-radius: 12px; font-weight: 600; }
    .stk-in { background: rgba(46,204,113,0.15); color: var(--green); }
    .stk-low { background: rgba(243,156,18,0.18); color: var(--amber); }
    .stk-out { background: rgba(231,76,60,0.18); color: var(--red); }
    .title { font-size: 0.98rem; font-weight: 700; color: #fff; margin-bottom: 3px; }
    .sub { font-size: 0.78rem; color: var(--muted); }

    .prices { display: flex; background: #11151b; padding: 9px 14px; gap: 12px; border-bottom: 1px solid rgba(255,255,255,0.05); }
    .pbox { flex: 1; }
    .plabel { font-size: 0.7rem; color: var(--muted); }
    .pval { font-size: 0.95rem; font-weight: 700; color: var(--gold-l); }

    .c-body { padding: 12px 14px; font-size: 0.8rem; flex: 1; display: flex; flex-direction: column; gap: 6px; }
    .kv { display: flex; gap: 6px; }
    .k { color: var(--muted); min-width: 68px; flex-shrink: 0; }
    .v { color: var(--text); }
    .mixer-tags { display: flex; flex-wrap: wrap; gap: 4px; margin-top: 2px; }
    .mtag { background: #222936; color: #cbd5e1; padding: 2px 7px; border-radius: 4px; font-size: 0.72rem; border: 1px solid #313b4d; }

    .c-foot { padding: 9px 14px; background: rgba(0,0,0,0.2); display: flex; justify-content: space-between; align-items: center; gap: 8px; border-top: 1px solid rgba(255,255,255,0.04); }
    .link-btn { color: var(--gold-l); text-decoration: none; font-size: 0.76rem; }
    .link-btn:hover { text-decoration: underline; }
    .admin-edit { display: none; align-items: center; gap: 4px; }
    body.unlocked .admin-edit { display: flex; }
    .admin-edit input { width: 54px; background: var(--bg); border: 1px solid var(--gold); color: #fff; padding: 2px 6px; border-radius: 4px; font-size: 0.78rem; text-align: center; }

    /* Modal */
    .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.75); z-index: 200; align-items: center; justify-content: center; padding: 16px; }
    .modal.show { display: flex; }
    .mcard { background: var(--panel); border: 1px solid var(--gold); border-radius: 12px; padding: 20px; width: 100%; max-width: 380px; }
    .mcard h3 { color: var(--gold); margin-bottom: 10px; }
  </style>
</head>
<body>

<header>
  <div class="nav">
    <div class="brand">
      <div class="pearl"></div>
      <div>
        <h1>The Black Pearl</h1>
        <small data-i18n="subtitle">Cellar & Bar Digital Inventory · 酒窖与酒吧智能酒单</small>
      </div>
    </div>
    <div class="btns">
      <button class="btn" onclick="syncRemoteExcel()" id="btnSync">🔄 <span data-i18n="syncBtn">同步库存</span></button>
      <button class="btn" onclick="toggleLang()">🌐 <span id="langLabel">English</span></button>
      <button class="btn btn-gold" onclick="openAdminModal()" id="btnLock">🔒 <span data-i18n="adminBtn">管理员解锁</span></button>
    </div>
  </div>

  <!-- 管理员控制台（仅解锁后可见） -->
  <div id="adminBox">
    <div style="display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:8px;">
      <strong style="color:var(--gold-l);" data-i18n="adminTitle">👑 管理员专属控制台（仅您可修改库存与数据源）</strong>
      <div style="display:flex;gap:8px;">
        <button class="btn btn-gold" onclick="exportExcel()" data-i18n="exportBtn">📥 导出最新 Excel</button>
        <button class="btn" onclick="lockAdmin()" data-i18n="lockBtn">🔒 退出锁定</button>
      </div>
    </div>
    <div class="admin-grid">
      <div class="abox">
        <label data-i18n="onedriveLabel">1. 绑定 OneDrive / Google Sheets 在线表格直链：</label>
        <input type="text" id="sheetUrl" placeholder="粘贴 Excel 公开下载链接或 CSV 链接..." />
        <button class="btn btn-gold" style="width:100%" onclick="saveAndSyncUrl()" data-i18n="saveSync">保存链接并立即同步</button>
      </div>
      <div class="abox">
        <label data-i18n="uploadLabel">2. 本地上传 Excel (.xlsx / .csv) 实时更新库存：</label>
        <input type="file" id="excelFile" accept=".xlsx,.xls,.csv" onchange="handleLocalExcel(event)" />
        <small style="color:var(--muted);font-size:0.74rem;" data-i18n="uploadTip">表格只需包含 Code 和 Stock (库存) 列即可自动匹配。</small>
      </div>
      <div class="abox">
        <label data-i18n="pwdLabel">3. 修改管理员密码：</label>
        <input type="password" id="newPwd" placeholder="输入新密码..." />
        <button class="btn" style="width:100%" onclick="changePwd()" data-i18n="updatePwd">更新密码</button>
      </div>
    </div>
  </div>
</header>

<div class="wrap">
  <div class="search-row">
    <input type="text" id="qSearch" class="inp" placeholder="🔍 搜索酒名、编号 Code (如 B241 / L020)、产区、葡萄品种或原料..." oninput="render()" />
    <select id="qStock" class="inp" onchange="render()">
