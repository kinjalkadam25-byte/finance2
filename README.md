
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Finance Dashboard</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #0f1117;
      --surface: #1a1d27;
      --surface2: #22263a;
      --border: #2e3250;
      --accent: #6c63ff;
      --accent2: #ff6584;
      --green: #3ecf8e;
      --text: #e2e8f0;
      --muted: #7b82a8;
      --radius: 12px;
    }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Segoe UI', system-ui, sans-serif;
      min-height: 100vh;
      padding: 24px 16px;
    }

    h1 { font-size: 1.6rem; font-weight: 700; letter-spacing: -0.5px; }
    h2 { font-size: 1.1rem; font-weight: 600; margin-bottom: 16px; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; font-size: 0.75rem; }

    .header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 28px;
      flex-wrap: wrap;
      gap: 12px;
    }

    .month-nav {
      display: flex;
      align-items: center;
      gap: 10px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 8px 14px;
    }

    .month-nav button {
      background: none;
      border: none;
      color: var(--accent);
      font-size: 1.2rem;
      cursor: pointer;
      line-height: 1;
    }

    .month-nav span { font-weight: 600; min-width: 140px; text-align: center; }

    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      gap: 14px;
      margin-bottom: 28px;
    }

    .stat-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 18px 16px;
    }

    .stat-card .label { font-size: 0.72rem; color: var(--muted); text-transform: uppercase; letter-spacing: 0.8px; margin-bottom: 6px; }
    .stat-card .value { font-size: 1.5rem; font-weight: 700; }
    .stat-card .value.green { color: var(--green); }
    .stat-card .value.red { color: var(--accent2); }
    .stat-card .value.purple { color: var(--accent); }

    .layout {
      display: grid;
      grid-template-columns: 380px 1fr;
      gap: 20px;
      align-items: start;
    }

    @media (max-width: 820px) {
      .layout { grid-template-columns: 1fr; }
    }

    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 20px;
    }

    .form-group { margin-bottom: 14px; }
    label { display: block; font-size: 0.8rem; color: var(--muted); margin-bottom: 5px; }

    input, select, textarea {
      width: 100%;
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: 8px;
      color: var(--text);
      padding: 9px 12px;
      font-size: 0.9rem;
      outline: none;
      transition: border-color 0.2s;
    }

    input:focus, select:focus, textarea:focus { border-color: var(--accent); }
    select option { background: var(--surface2); }

    .btn {
      width: 100%;
      padding: 11px;
      background: var(--accent);
      color: #fff;
      border: none;
      border-radius: 8px;
      font-size: 0.95rem;
      font-weight: 600;
      cursor: pointer;
      transition: opacity 0.2s;
    }

    .btn:hover { opacity: 0.88; }

    /* Category breakdown */
    .cat-list { display: flex; flex-direction: column; gap: 10px; }

    .cat-row { display: flex; align-items: center; gap: 10px; }
    .cat-name { width: 110px; font-size: 0.83rem; flex-shrink: 0; }
    .bar-wrap { flex: 1; background: var(--surface2); border-radius: 20px; height: 8px; overflow: hidden; }
    .bar-fill { height: 100%; border-radius: 20px; transition: width 0.4s; }
    .cat-amt { width: 70px; text-align: right; font-size: 0.83rem; font-weight: 600; }

    /* Transactions table */
    .tx-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 14px;
      flex-wrap: wrap;
      gap: 8px;
    }

    .filter-row { display: flex; gap: 8px; flex-wrap: wrap; }

    .filter-row select, .filter-row input {
      width: auto;
      padding: 6px 10px;
      font-size: 0.8rem;
    }

    table { width: 100%; border-collapse: collapse; font-size: 0.87rem; }
    thead th {
      text-align: left;
      padding: 8px 10px;
      color: var(--muted);
      font-size: 0.72rem;
      text-transform: uppercase;
      letter-spacing: 0.7px;
      border-bottom: 1px solid var(--border);
    }

    tbody tr { border-bottom: 1px solid var(--border); transition: background 0.15s; }
    tbody tr:last-child { border-bottom: none; }
    tbody tr:hover { background: var(--surface2); }
    td { padding: 10px 10px; vertical-align: middle; }

    .badge {
      display: inline-block;
      padding: 3px 8px;
      border-radius: 20px;
      font-size: 0.72rem;
      font-weight: 600;
    }

    .del-btn {
      background: none;
      border: none;
      color: var(--muted);
      cursor: pointer;
      font-size: 1rem;
      line-height: 1;
      padding: 2px 6px;
      border-radius: 4px;
      transition: color 0.2s, background 0.2s;
    }
    .del-btn:hover { color: var(--accent2); background: rgba(255,101,132,0.1); }

    .empty { text-align: center; color: var(--muted); padding: 32px; font-size: 0.9rem; }

    .toast {
      position: fixed;
      bottom: 24px;
      right: 24px;
      background: var(--green);
      color: #0f1117;
      padding: 10px 18px;
      border-radius: 8px;
      font-weight: 600;
      font-size: 0.9rem;
      opacity: 0;
      transform: translateY(10px);
      transition: opacity 0.3s, transform 0.3s;
      pointer-events: none;
      z-index: 999;
    }
    .toast.show { opacity: 1; transform: translateY(0); }

    /* ── Login Portal ───────────────────────────────────────── */
    #loginScreen {
      position: fixed; inset: 0;
      background: var(--bg);
      z-index: 10000;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 24px;
    }

    .login-box {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 18px;
      padding: 40px 36px 36px;
      width: 100%;
      max-width: 400px;
      box-shadow: 0 24px 64px rgba(0,0,0,0.5);
      animation: loginPop 0.35s cubic-bezier(0.34,1.56,0.64,1) both;
    }

    @keyframes loginPop {
      from { opacity: 0; transform: scale(0.92) translateY(16px); }
      to   { opacity: 1; transform: scale(1) translateY(0); }
    }

    .login-logo {
      text-align: center;
      margin-bottom: 28px;
    }

    .login-logo .emoji { font-size: 2.8rem; display: block; margin-bottom: 10px; }
    .login-logo h2 {
      font-size: 1.4rem; font-weight: 800;
      color: var(--text);
      text-transform: none; letter-spacing: -0.5px;
      margin-bottom: 4px;
    }
    .login-logo p { font-size: 0.82rem; color: var(--muted); }

    .login-field { margin-bottom: 16px; }
    .login-field label {
      display: block; font-size: 0.78rem;
      color: var(--muted); margin-bottom: 6px; font-weight: 500;
    }
    .login-field input {
      width: 100%; padding: 11px 14px;
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: 9px; color: var(--text);
      font-size: 0.95rem; outline: none;
      transition: border-color 0.2s, box-shadow 0.2s;
    }
    .login-field input:focus {
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(108,99,255,0.18);
    }

    .login-error {
      background: rgba(255,101,132,0.12);
      border: 1px solid rgba(255,101,132,0.35);
      color: var(--accent2);
      border-radius: 8px;
      padding: 9px 14px;
      font-size: 0.83rem;
      margin-bottom: 14px;
      display: none;
    }

    .login-btn {
      width: 100%; padding: 12px;
      background: var(--accent);
      color: #fff; border: none;
      border-radius: 9px; font-size: 1rem;
      font-weight: 700; cursor: pointer;
      transition: opacity 0.2s, transform 0.1s;
      margin-top: 4px;
    }
    .login-btn:hover { opacity: 0.88; }
    .login-btn:active { transform: scale(0.98); }
    .login-btn:disabled { opacity: 0.5; cursor: default; }

    .login-footer {
      text-align: center; margin-top: 20px;
      font-size: 0.75rem; color: var(--muted);
    }

    /* Logout chip in header */
    .user-chip {
      display: flex; align-items: center; gap: 10px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 7px 14px;
      font-size: 0.82rem;
    }
    .user-chip .avatar {
      width: 26px; height: 26px;
      background: var(--accent);
      border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      font-size: 0.72rem; font-weight: 700; color: #fff;
      flex-shrink: 0;
    }
    .user-chip .uname { color: var(--text); font-weight: 600; }
    .logout-btn {
      background: none; border: none; color: var(--muted);
      cursor: pointer; font-size: 0.78rem;
      padding: 2px 6px; border-radius: 5px;
      transition: color 0.2s, background 0.2s;
    }
    .logout-btn:hover { color: var(--accent2); background: rgba(255,101,132,0.1); }

    /* Dashboard blur until login */
    #dashboardRoot { display: none; }

    .import-box {
      border: 2px dashed var(--border);
      border-radius: var(--radius);
      padding: 20px;
      text-align: center;
      cursor: pointer;
      transition: border-color 0.2s, background 0.2s;
      margin-bottom: 14px;
    }
    .import-box:hover { border-color: var(--accent); background: rgba(108,99,255,0.05); }
    .import-box input[type=file] { display: none; }
    .import-box .icon { font-size: 1.8rem; margin-bottom: 6px; }
    .import-box .hint { font-size: 0.78rem; color: var(--muted); margin-top: 4px; }

    .btn-outline {
      width: 100%;
      padding: 9px;
      background: transparent;
      color: var(--accent);
      border: 1px solid var(--accent);
      border-radius: 8px;
      font-size: 0.88rem;
      font-weight: 600;
      cursor: pointer;
      transition: background 0.2s;
      margin-top: 8px;
    }
    .btn-outline:hover { background: rgba(108,99,255,0.1); }

    .preview-table { font-size: 0.78rem; margin-top: 10px; max-height: 200px; overflow-y: auto; }
    .preview-table table { width: 100%; }
    .preview-table th { color: var(--muted); padding: 4px 6px; }
    .preview-table td { padding: 4px 6px; border-bottom: 1px solid var(--border); }

    /* Review modal */
    .modal-backdrop {
      position: fixed; inset: 0;
      background: rgba(0,0,0,0.7);
      z-index: 100;
      display: flex; align-items: center; justify-content: center;
      padding: 16px;
    }
    .modal {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      width: 100%; max-width: 860px;
      max-height: 90vh;
      display: flex; flex-direction: column;
      overflow: hidden;
    }
    .modal-header {
      padding: 18px 20px;
      border-bottom: 1px solid var(--border);
      display: flex; justify-content: space-between; align-items: center;
    }
    .modal-header h3 { font-size: 1rem; font-weight: 700; }
    .modal-body { overflow-y: auto; padding: 0; flex: 1; }
    .modal-footer {
      padding: 14px 20px;
      border-top: 1px solid var(--border);
      display: flex; gap: 10px; justify-content: flex-end;
    }
    .modal-footer .btn { width: auto; padding: 9px 22px; }
    .btn-ghost {
      padding: 9px 22px;
      background: transparent;
      color: var(--muted);
      border: 1px solid var(--border);
      border-radius: 8px;
      font-size: 0.9rem;
      cursor: pointer;
    }
    .review-table { width: 100%; border-collapse: collapse; font-size: 0.83rem; }
    .review-table th {
      position: sticky; top: 0;
      background: var(--surface);
      padding: 10px 12px;
      text-align: left;
      color: var(--muted);
      font-size: 0.7rem;
      text-transform: uppercase;
      letter-spacing: 0.6px;
      border-bottom: 1px solid var(--border);
    }
    .review-table td { padding: 8px 12px; border-bottom: 1px solid var(--border); vertical-align: middle; }
    .review-table tr:last-child td { border-bottom: none; }
    .review-table select {
      padding: 4px 8px;
      font-size: 0.78rem;
      border-radius: 6px;
      width: 130px;
    }
    .review-table .skip-cb { width: 16px; height: 16px; cursor: pointer; accent-color: var(--accent2); }
    .conf-dot {
      display: inline-block; width: 8px; height: 8px;
      border-radius: 50%; margin-right: 5px; vertical-align: middle;
    }
    .skipped td { opacity: 0.35; text-decoration: line-through; }
    .stats-row { display: flex; gap: 16px; flex-wrap: wrap; padding: 14px 20px; border-bottom: 1px solid var(--border); }
    .stats-row span { font-size: 0.82rem; color: var(--muted); }
    .stats-row strong { color: var(--text); }
  </style>
</head>
<body>

<!-- ── Login Screen ─────────────────────────────────────────── -->
<div id="loginScreen">
  <div class="login-box">
    <div class="login-logo">
      <span class="emoji">💰</span>
      <h2>Finance Dashboard</h2>
      <p>Sign in to manage your expenses</p>
    </div>
    <div class="login-error" id="loginError">Incorrect username or password.</div>
    <div class="login-field">
      <label>Username</label>
      <input type="text" id="loginUser" placeholder="Enter username" autocomplete="username" />
    </div>
    <div class="login-field">
      <label>Password</label>
      <input type="password" id="loginPass" placeholder="Enter password" autocomplete="current-password" />
    </div>
    <button class="login-btn" id="loginBtn" onclick="attemptLogin()">Sign In</button>
    <div class="login-footer">🔒 Your data is private and secured</div>
  </div>
</div>

<!-- ── Dashboard (hidden until logged in) ──────────────────── -->
<div id="dashboardRoot">

<div class="header">
  <h1>💰 Finance Dashboard</h1>
  <div style="display:flex;gap:10px;align-items:center;flex-wrap:wrap">
    <div class="month-nav">
      <button onclick="changeMonth(-1)">&#8592;</button>
      <span id="monthLabel"></span>
      <button onclick="changeMonth(1)">&#8594;</button>
    </div>
    <div class="user-chip" id="userChip">
      <div class="avatar" id="userAvatar"></div>
      <span class="uname" id="userNameDisplay"></span>
      <button class="logout-btn" onclick="logout()" title="Sign out">Sign out</button>
    </div>
  </div>
</div>

<!-- Summary stats -->
<div class="grid" id="statsGrid"></div>

<div class="layout">
  <!-- Add Expense Form -->
  <div>
    <div class="card section-gap">
      <h2>Add Expense</h2>
      <div class="form-group">
        <label>Date</label>
        <input type="date" id="expDate" />
      </div>
      <div class="form-group">
        <label>Category</label>
        <select id="expCat"></select>
      </div>
      <div class="form-group">
        <label>Amount (₹)</label>
        <input type="number" id="expAmt" placeholder="0.00" min="0" step="0.01" />
      </div>
      <div class="form-group">
        <label>Note (optional)</label>
        <input type="text" id="expNote" placeholder="e.g. Paid via UPI" />
      </div>
      <button class="btn" onclick="addExpense()">+ Add Expense</button>
    </div>

    <!-- Import from Excel/CSV -->
    <div class="card section-gap">
      <h2>Import from Excel / CSV</h2>
      <label class="import-box" id="dropZone">
        <input type="file" id="importFile" accept=".xlsx,.xls,.csv,.pdf" onchange="handleImport(this)" />
        <div class="icon">📂</div>
        <div>Click to select or drag &amp; drop file</div>
        <div class="hint">Supports .xlsx, .xls, .csv, .pdf &nbsp;|&nbsp; Bank statements auto-classified</div>
      </label>
      <div id="importPreview" style="font-size:0.8rem;color:var(--muted);text-align:center;padding:6px 0"></div>
    </div>

    <!-- Category Breakdown -->
    <div class="card">
      <h2>Category Breakdown</h2>
      <div class="cat-list" id="catBreakdown"></div>
    </div>
  </div>

  <!-- Transactions -->
  <div class="card">
    <div class="tx-header">
      <h2 style="margin-bottom:0">Transactions</h2>
      <div class="filter-row">
        <select id="filterCat" onchange="renderTransactions()">
          <option value="">All Categories</option>
        </select>
        <input type="text" id="filterSearch" placeholder="Search note..." oninput="renderTransactions()" style="width:140px" />
      </div>
    </div>
    <div id="txTable"></div>
  </div>
</div>

<!-- Review Modal -->
<div class="modal-backdrop" id="reviewModal" style="display:none">
  <div class="modal">
    <div class="modal-header">
      <h3>Review & Confirm Import</h3>
      <button class="btn-ghost" onclick="closeReview()" style="padding:4px 10px;border-radius:6px">✕</button>
    </div>
    <div class="stats-row" id="reviewStats"></div>
    <div class="modal-body">
      <table class="review-table" id="reviewTableEl">
        <thead>
          <tr>
            <th style="width:32px">Skip</th>
            <th>Date</th>
            <th>Description</th>
            <th>Amount</th>
            <th>Category</th>
            <th>Confidence</th>
          </tr>
        </thead>
        <tbody id="reviewBody"></tbody>
      </table>
    </div>
    <div class="modal-footer">
      <button class="btn-ghost" onclick="closeReview()">Cancel</button>
      <button class="btn" onclick="confirmImport()">Import Selected</button>
    </div>
  </div>
</div>

</div><!-- end #dashboardRoot -->

<div class="toast" id="toast"></div>

<!-- SheetJS for Excel reading -->
<script src="https://cdn.sheetjs.com/xlsx-0.20.3/package/dist/xlsx.full.min.js"></script>
<!-- PDF.js for bank statement PDFs -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<script>
  if (typeof pdfjsLib !== 'undefined') {
    pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';
  }
</script>

<script>
// ══════════════════════════════════════════════════════════════
//  LOGIN CREDENTIALS
//  Add or remove users here. Passwords are checked client-side.
//  For stronger security, move credential checking into
//  your Apps Script backend and use a token-based approach.
// ══════════════════════════════════════════════════════════════
const USERS = [
  { username: "Jitendra", password: "finance@2025" },
  { username: "Sheetal",  password: "sheetal@123"  },
  { username: "admin",    password: "admin@123"    },
];
// ══════════════════════════════════════════════════════════════

// ── Auth helpers ──────────────────────────────────────────────
function attemptLogin() {
  const u = document.getElementById('loginUser').value.trim();
  const p = document.getElementById('loginPass').value;
  const errEl = document.getElementById('loginError');
  const btn = document.getElementById('loginBtn');

  const match = USERS.find(x => x.username.toLowerCase() === u.toLowerCase() && x.password === p);
  if (!match) {
    errEl.style.display = 'block';
    document.getElementById('loginPass').value = '';
    document.getElementById('loginPass').focus();
    btn.disabled = false;
    return;
  }

  errEl.style.display = 'none';
  sessionStorage.setItem('fd_user', match.username);
  showDashboard(match.username);
}

function logout() {
  sessionStorage.removeItem('fd_user');
  document.getElementById('dashboardRoot').style.display = 'none';
  document.getElementById('loginScreen').style.display = 'flex';
  document.getElementById('loginUser').value = '';
  document.getElementById('loginPass').value = '';
  document.getElementById('loginError').style.display = 'none';
  document.getElementById('loginUser').focus();
}

function showDashboard(username) {
  document.getElementById('loginScreen').style.display = 'none';
  document.getElementById('dashboardRoot').style.display = 'block';
  // Set avatar (initials) and name
  document.getElementById('userAvatar').textContent = username.slice(0,2).toUpperCase();
  document.getElementById('userNameDisplay').textContent = username;
  init();
}

// Check session on page load — allow keyboard Enter on login fields
document.addEventListener('DOMContentLoaded', () => {
  const saved = sessionStorage.getItem('fd_user');
  if (saved && USERS.find(x => x.username === saved)) {
    showDashboard(saved);
  } else {
    document.getElementById('loginScreen').style.display = 'flex';
    setTimeout(() => document.getElementById('loginUser').focus(), 100);
  }

  // Enter key support on login inputs
  ['loginUser','loginPass'].forEach(id => {
    document.getElementById(id).addEventListener('keydown', e => {
      if (e.key === 'Enter') attemptLogin();
    });
  });
});


//  Paste your deployed Apps Script Web App URL below.
//  See Code.gs for step-by-step deployment instructions.
// ══════════════════════════════════════════════════════════════
const APPS_SCRIPT_URL = "YOUR_WEB_APP_URL_HERE";
// ══════════════════════════════════════════════════════════════

const CATEGORIES = [
  "Rent","Medical","Kishori","Light Bill","Gas",
  "General Store","Vegetables","Mobile Bills","Sheetal","Jitendra","Nishi"
];

const CAT_COLORS = [
  "#6c63ff","#ff6584","#3ecf8e","#f6a623","#38bdf8",
  "#e879f9","#fb923c","#34d399","#a78bfa","#f87171","#60a5fa"
];

const CAT_COLOR_MAP = {};
CATEGORIES.forEach((c, i) => CAT_COLOR_MAP[c] = CAT_COLORS[i % CAT_COLORS.length]);

let currentYear, currentMonth; // 0-indexed month

async function init() {
  const now = new Date();
  currentYear = now.getFullYear();
  currentMonth = now.getMonth();

  // Populate category selects
  const sel = document.getElementById('expCat');
  const filterSel = document.getElementById('filterCat');
  CATEGORIES.forEach(c => {
    sel.innerHTML += `<option value="${c}">${c}</option>`;
    filterSel.innerHTML += `<option value="${c}">${c}</option>`;
  });

  // Default date to today
  document.getElementById('expDate').value = toDateString(now);

  await render();
}

function toDateString(d) {
  return d.toISOString().split('T')[0];
}

// ── Google Sheets API helpers ─────────────────────────────────

// Simple in-memory cache so we don't re-fetch on every render
const _cache = {};

async function apiFetch(params) {
  const url = new URL(APPS_SCRIPT_URL);
  const isPost = ['addExpense','deleteExpense','bulkAdd'].includes(params.action);

  if (isPost) {
    const res = await fetch(APPS_SCRIPT_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'text/plain' }, // Apps Script requires text/plain for JSON POST
      body: JSON.stringify(params),
      redirect: 'follow',
    });
    return res.json();
  } else {
    Object.entries(params).forEach(([k,v]) => url.searchParams.set(k, v));
    const res = await fetch(url, { redirect: 'follow' });
    return res.json();
  }
}

async function getExpenses() {
  if (APPS_SCRIPT_URL === 'YOUR_WEB_APP_URL_HERE') {
    // Demo mode: fall back to localStorage so the UI is usable before setup
    return JSON.parse(localStorage.getItem(`expenses_${currentYear}_${String(currentMonth+1).padStart(2,'0')}`) || '[]');
  }
  const cacheKey = `${currentYear}_${String(currentMonth+1).padStart(2,'0')}`;
  if (_cache[cacheKey]) return _cache[cacheKey];

  try {
    setLoading(true);
    const data = await apiFetch({
      action: 'getExpenses',
      year:   currentYear,
      month:  String(currentMonth+1).padStart(2,'0'),
    });
    if (data.error) { showToast('Sheet error: ' + data.error, 'error'); return []; }
    _cache[cacheKey] = data.expenses || [];
    return _cache[cacheKey];
  } catch(e) {
    showToast('Could not reach Google Sheet', 'error');
    return [];
  } finally {
    setLoading(false);
  }
}

function invalidateCache() {
  const cacheKey = `${currentYear}_${String(currentMonth+1).padStart(2,'0')}`;
  delete _cache[cacheKey];
}

async function saveExpenseToSheet(expense) {
  if (APPS_SCRIPT_URL === 'YOUR_WEB_APP_URL_HERE') {
    // Demo mode: localStorage
    const key = `expenses_${currentYear}_${String(currentMonth+1).padStart(2,'0')}`;
    const arr = JSON.parse(localStorage.getItem(key) || '[]');
    arr.push(expense);
    arr.sort((a,b) => b.date.localeCompare(a.date));
    localStorage.setItem(key, JSON.stringify(arr));
    return;
  }
  const data = await apiFetch({ action: 'addExpense', ...expense });
  if (data.error) { showToast('Save failed: ' + data.error, 'error'); throw new Error(data.error); }
  invalidateCache();
}

async function deleteExpenseFromSheet(id) {
  if (APPS_SCRIPT_URL === 'YOUR_WEB_APP_URL_HERE') {
    const key = `expenses_${currentYear}_${String(currentMonth+1).padStart(2,'0')}`;
    const arr = JSON.parse(localStorage.getItem(key) || '[]').filter(e => e.id !== id);
    localStorage.setItem(key, JSON.stringify(arr));
    return;
  }
  const data = await apiFetch({ action: 'deleteExpense', id: String(id) });
  if (data.error) { showToast('Delete failed: ' + data.error, 'error'); throw new Error(data.error); }
  invalidateCache();
}

async function bulkAddToSheet(expenses) {
  if (APPS_SCRIPT_URL === 'YOUR_WEB_APP_URL_HERE') {
    // Demo mode: localStorage grouped by month
    const grouped = {};
    expenses.forEach(r => {
      const [y, m] = r.date.split('-');
      const key = `expenses_${y}_${m}`;
      if (!grouped[key]) grouped[key] = JSON.parse(localStorage.getItem(key) || '[]');
      grouped[key].push({ id: r.id, date: r.date, cat: r.cat, amt: r.amt, note: r.note });
    });
    Object.entries(grouped).forEach(([k, arr]) => {
      arr.sort((a,b) => b.date.localeCompare(a.date));
      localStorage.setItem(k, JSON.stringify(arr));
    });
    return;
  }
  const data = await apiFetch({ action: 'bulkAdd', expenses });
  if (data.error) { showToast('Import failed: ' + data.error, 'error'); throw new Error(data.error); }
  // invalidate all cached months since import may span multiple months
  Object.keys(_cache).forEach(k => delete _cache[k]);
}

// Loading indicator
function setLoading(on) {
  let el = document.getElementById('loadingBar');
  if (!el) {
    el = document.createElement('div');
    el.id = 'loadingBar';
    el.style.cssText = `
      position:fixed;top:0;left:0;height:3px;background:var(--accent);
      width:0;transition:width 0.3s;z-index:9999;pointer-events:none;
    `;
    document.body.appendChild(el);
  }
  el.style.width = on ? '70%' : '100%';
  if (!on) setTimeout(() => { el.style.width = '0'; }, 300);
}

async function changeMonth(dir) {
  currentMonth += dir;
  if (currentMonth > 11) { currentMonth = 0; currentYear++; }
  if (currentMonth < 0)  { currentMonth = 11; currentYear--; }
  await render();
}

function monthName(y, m) {
  return new Date(y, m, 1).toLocaleString('default', { month: 'long', year: 'numeric' });
}

async function addExpense() {
  const date = document.getElementById('expDate').value;
  const cat  = document.getElementById('expCat').value;
  const amt  = parseFloat(document.getElementById('expAmt').value);
  const note = document.getElementById('expNote').value.trim();

  if (!date) { showToast('Please select a date', 'error'); return; }
  if (!amt || amt <= 0) { showToast('Enter a valid amount', 'error'); return; }

  const btn = document.querySelector('.btn[onclick="addExpense()"]');
  if (btn) { btn.disabled = true; btn.textContent = 'Saving…'; }

  try {
    await saveExpenseToSheet({ id: Date.now(), date, cat, amt, note });
    document.getElementById('expAmt').value = '';
    document.getElementById('expNote').value = '';
    showToast('Expense added!');
    await render();
  } catch(_) {
    // error already shown in saveExpenseToSheet
  } finally {
    if (btn) { btn.disabled = false; btn.textContent = '+ Add Expense'; }
  }
}

async function deleteExpense(id) {
  try {
    await deleteExpenseFromSheet(id);
    await render();
  } catch(_) {}
}

async function render() {
  document.getElementById('monthLabel').textContent = monthName(currentYear, currentMonth);
  await renderStats();
  await renderCategoryBreakdown();
  await renderTransactions();
}

async function renderStats() {
  const expenses = await getExpenses();
  const total = expenses.reduce((s, e) => s + e.amt, 0);
  const count = expenses.length;
  const topCat = (() => {
    const m = {};
    expenses.forEach(e => m[e.cat] = (m[e.cat]||0) + e.amt);
    return Object.entries(m).sort((a,b)=>b[1]-a[1])[0]?.[0] || '—';
  })();
  const avgDay = (() => {
    const days = new Set(expenses.map(e=>e.date)).size;
    return days ? (total/days) : 0;
  })();

  document.getElementById('statsGrid').innerHTML = `
    <div class="stat-card">
      <div class="label">Total Spent</div>
      <div class="value red">₹${fmt(total)}</div>
    </div>
    <div class="stat-card">
      <div class="label">Transactions</div>
      <div class="value purple">${count}</div>
    </div>
    <div class="stat-card">
      <div class="label">Top Category</div>
      <div class="value" style="font-size:1.1rem">${topCat}</div>
    </div>
    <div class="stat-card">
      <div class="label">Avg per Day</div>
      <div class="value green">₹${fmt(avgDay)}</div>
    </div>
  `;
}

async function renderCategoryBreakdown() {
  const expenses = await getExpenses();
  const total = expenses.reduce((s,e)=>s+e.amt,0);
  const map = {};
  CATEGORIES.forEach(c => map[c] = 0);
  expenses.forEach(e => map[e.cat] = (map[e.cat]||0) + e.amt);

  const sorted = CATEGORIES.map(c => ({ cat: c, amt: map[c] }))
    .filter(x => x.amt > 0)
    .sort((a,b) => b.amt - a.amt);

  if (!sorted.length) {
    document.getElementById('catBreakdown').innerHTML = `<div class="empty">No data yet</div>`;
    return;
  }

  const max = sorted[0].amt;
  document.getElementById('catBreakdown').innerHTML = sorted.map(({ cat, amt }) => `
    <div class="cat-row">
      <span class="cat-name">${cat}</span>
      <div class="bar-wrap">
        <div class="bar-fill" style="width:${(amt/max*100).toFixed(1)}%;background:${CAT_COLOR_MAP[cat]}"></div>
      </div>
      <span class="cat-amt">₹${fmt(amt)}</span>
    </div>
  `).join('');
}

async function renderTransactions() {
  const filterCat = document.getElementById('filterCat').value;
  const search = document.getElementById('filterSearch').value.toLowerCase();

  let expenses = await getExpenses();
  if (filterCat) expenses = expenses.filter(e => e.cat === filterCat);
  if (search)    expenses = expenses.filter(e => e.note.toLowerCase().includes(search) || e.cat.toLowerCase().includes(search));

  if (!expenses.length) {
    document.getElementById('txTable').innerHTML = `<div class="empty">No transactions found</div>`;
    return;
  }

  document.getElementById('txTable').innerHTML = `
    <table>
      <thead>
        <tr>
          <th>Date</th>
          <th>Category</th>
          <th>Note</th>
          <th>Amount</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        ${expenses.map(e => `
          <tr>
            <td>${formatDate(e.date)}</td>
            <td>
              <span class="badge" style="background:${CAT_COLOR_MAP[e.cat]}22;color:${CAT_COLOR_MAP[e.cat]}">
                ${e.cat}
              </span>
            </td>
            <td style="color:var(--muted)">${e.note || '—'}</td>
            <td style="font-weight:600;color:var(--accent2)">₹${fmt(e.amt)}</td>
            <td><button class="del-btn" onclick="deleteExpense(${e.id})" title="Delete">✕</button></td>
          </tr>
        `).join('')}
      </tbody>
    </table>
  `;
}

function formatDate(str) {
  const d = new Date(str + 'T00:00:00');
  return d.toLocaleDateString('en-IN', { day: '2-digit', month: 'short' });
}

function fmt(n) {
  return n.toLocaleString('en-IN', { maximumFractionDigits: 0 });
}

function showToast(msg, type) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.style.background = type === 'error' ? 'var(--accent2)' : 'var(--green)';
  t.style.color = type === 'error' ? '#fff' : '#0f1117';
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2400);
}

// ── Auto-Classification Rules ─────────────────────────────
const CLASSIFY_RULES = [
  { cat: 'Rent',          keywords: ['rent','housing','flat','pg','lodge','landlord','house rent','niwas','awas'] },
  { cat: 'Medical',       keywords: ['pharmacy','medical','hospital','clinic','health','chemist','medicine','apollo','medplus','netmeds','1mg','wellness','diagnostic','lab','nursing','dispensary','pharma'] },
  { cat: 'Kishori',       keywords: ['kishori'] },
  { cat: 'Light Bill',    keywords: ['electricity','mseb','bescom','light bill','torrent power','bijli','electric','wbsedcl','tsspdcl','tneb','cesc','adani electric','bses','discom','power bill','energyبل','mahadiscom','mahavitaran','uppcl','apspdcl'] },
  { cat: 'Gas',           keywords: ['gas','lpg','indane','hp gas','bharat gas','cylinder','igl','mgl','mahanagar gas','piped gas','png','gas bill'] },
  { cat: 'General Store', keywords: ['kirana','general store','grocery','supermarket','d-mart','dmart','bigbasket','blinkit','zepto','jiomart','grofers','reliance fresh','more retail','nilgiris','spencers','hypermarket','departmental','provision','convenient store','easyday','spar','star bazaar','nature basket','walmart','metro cash'] },
  { cat: 'Vegetables',    keywords: ['vegetable','sabji','bhaji','sabzi','mandi','greengrocer','fresh produce','fruits','veggie','subzi'] },
  { cat: 'Mobile Bills',  keywords: ['airtel','jio','vi-','vodafone','idea cellular','bsnl','mobile bill','recharge','postpaid','prepaid','tata docomo','telecom','dtelecom','mtnl','cellphone','sim','broadband','internet bill','fiber','wifi bill'] },
  { cat: 'Sheetal',       keywords: ['sheetal'] },
  { cat: 'Jitendra',      keywords: ['jitendra','jitu'] },
  { cat: 'Nishi',         keywords: ['nishi','nishad'] },
];

// Returns { cat, confidence: 'high'|'medium'|'low' }
function autoClassify(description) {
  const desc = description.toLowerCase();
  for (const rule of CLASSIFY_RULES) {
    for (const kw of rule.keywords) {
      if (desc.includes(kw)) {
        const confidence = kw.length >= 5 ? 'high' : 'medium';
        return { cat: rule.cat, confidence };
      }
    }
  }
  return { cat: 'General Store', confidence: 'low' };
}

// ── Date Parsing ──────────────────────────────────────────
function parseDate(rawDate) {
  if (!rawDate) return '';
  rawDate = String(rawDate).trim();

  // Excel serial number
  const asNum = Number(rawDate);
  if (!isNaN(asNum) && asNum > 10000 && asNum < 100000) {
    const d = XLSX.SSF.parse_date_code(asNum);
    if (d) return `${d.y}-${String(d.m).padStart(2,'0')}-${String(d.d).padStart(2,'0')}`;
  }

  // DD/MM/YYYY or DD-MM-YYYY or DD.MM.YYYY
  const dmy = rawDate.match(/^(\d{1,2})[\/\-\.](\d{1,2})[\/\-\.](\d{2,4})$/);
  if (dmy) {
    let [, d, m, y] = dmy;
    if (y.length === 2) y = '20' + y;
    return `${y}-${m.padStart(2,'0')}-${d.padStart(2,'0')}`;
  }

  // YYYY-MM-DD
  const ymd = rawDate.match(/^(\d{4})[\/\-\.](\d{1,2})[\/\-\.](\d{1,2})$/);
  if (ymd) {
    const [, y, m, d] = ymd;
    return `${y}-${m.padStart(2,'0')}-${d.padStart(2,'0')}`;
  }

  // Fallback: JS Date parse
  const parsed = new Date(rawDate);
  if (!isNaN(parsed)) return parsed.toISOString().split('T')[0];

  return '';
}

// ── Import Logic ──────────────────────────────────────────
let pendingImportRows = [];

document.addEventListener('DOMContentLoaded', () => {
  const zone = document.getElementById('dropZone');
  zone.addEventListener('dragover', e => { e.preventDefault(); zone.style.borderColor = 'var(--accent)'; });
  zone.addEventListener('dragleave', () => { zone.style.borderColor = ''; });
  zone.addEventListener('drop', e => {
    e.preventDefault();
    zone.style.borderColor = '';
    const file = e.dataTransfer.files[0];
    if (file) processImportFile(file);
  });
});

function handleImport(input) {
  if (input.files[0]) processImportFile(input.files[0]);
}

function processImportFile(file) {
  const ext = file.name.split('.').pop().toLowerCase();
  if (ext === 'pdf') {
    parsePDF(file);
  } else {
    parseExcelCSV(file);
  }
}

// ── Excel / CSV Parser ────────────────────────────────────
function parseExcelCSV(file) {
  const reader = new FileReader();
  reader.onload = function(e) {
    try {
      const data = new Uint8Array(e.target.result);
      const wb = XLSX.read(data, { type: 'array', cellDates: false });
      const sheet = wb.Sheets[wb.SheetNames[0]];
      const rows = XLSX.utils.sheet_to_json(sheet, { defval: '', raw: true });
      if (!rows.length) { showToast('File is empty', 'error'); return; }

      const result = [];
      const errors = [];

      rows.forEach((row, i) => {
        const keys = Object.keys(row);
        const get = (names) => {
          const k = keys.find(k => names.some(n => k.toLowerCase().trim().includes(n.toLowerCase())));
          return k ? String(row[k]).trim() : '';
        };

        const rawDate = get(['date','dt','dated','transaction date','txn date','value date']);
        // For bank statements, look for debit/withdrawal columns first
        const rawDebit  = get(['debit','withdrawal','dr','debit amt','withdrawal amt','debit amount']);
        const rawCredit = get(['credit','deposit','cr','credit amt','deposit amt','credit amount']);
        const rawAmt    = get(['amount','amt','price','cost','expense','₹','rs','inr']);
        const rawDesc   = get(['description','narration','particulars','details','remarks','note','reference','desc','transaction details']);
        const rawCat    = get(['category','cat','type','head']);

        const date = parseDate(rawDate);
        if (!date) { errors.push(i + 2); return; }

        // For bank statements: only import debits (expenses)
        let amt = 0;
        if (rawDebit) {
          amt = parseFloat(String(rawDebit).replace(/[₹,\s]/g, ''));
        } else {
          amt = parseFloat(String(rawAmt).replace(/[₹,\s]/g, ''));
        }
        if (isNaN(amt) || amt <= 0) { errors.push(i + 2); return; }

        const description = rawDesc || rawCat || '';
        let cat, confidence;
        // If file has explicit category column that matches our list, use it
        const exactCat = CATEGORIES.find(c => c.toLowerCase() === rawCat.toLowerCase());
        if (exactCat) {
          cat = exactCat; confidence = 'high';
        } else {
          ({ cat, confidence } = autoClassify(description));
        }

        result.push({ id: Date.now() + i, date, cat, confidence, amt, note: description });
      });

      if (errors.length) console.warn(`${errors.length} rows skipped (invalid date/amount)`);
      if (!result.length) { showToast('No valid expense rows found', 'error'); return; }

      openReview(result);
    } catch(err) {
      showToast('Could not read file', 'error');
      console.error(err);
    }
  };
  reader.readAsArrayBuffer(file);
}

// ── PDF Parser (bank statements) ─────────────────────────
async function parsePDF(file) {
  showToast('Reading PDF…');
  try {
    const arrayBuffer = await file.arrayBuffer();
    const pdf = await pdfjsLib.getDocument({ data: arrayBuffer }).promise;
    let fullText = '';

    for (let p = 1; p <= pdf.numPages; p++) {
      const page = await pdf.getPage(p);
      const content = await page.getTextContent();
      // Reconstruct lines by grouping items with similar Y positions
      const lines = {};
      content.items.forEach(item => {
        const y = Math.round(item.transform[5]);
        if (!lines[y]) lines[y] = [];
        lines[y].push({ x: item.transform[4], text: item.str });
      });
      Object.keys(lines).sort((a,b)=>b-a).forEach(y => {
        const line = lines[y].sort((a,b)=>a.x-b.x).map(i=>i.text).join(' ');
        fullText += line + '\n';
      });
    }

    const transactions = extractTransactionsFromText(fullText);
    if (!transactions.length) {
      showToast('Could not parse transactions from PDF. Try Excel/CSV export.', 'error');
      return;
    }
    openReview(transactions);
  } catch(err) {
    showToast('PDF read failed. Try Excel/CSV instead.', 'error');
    console.error(err);
  }
}

function extractTransactionsFromText(text) {
  const results = [];
  const lines = text.split('\n').map(l => l.trim()).filter(Boolean);

  // Pattern: date at start of line, then description, then amount(s)
  // Covers SBI, HDFC, ICICI, Axis, Kotak, Canara, PNB, BOI formats
  const dateRe = /\b(\d{1,2}[\/\-\.]\d{1,2}[\/\-\.]\d{2,4})\b/;
  const amtRe  = /(\d{1,3}(?:,\d{3})*(?:\.\d{1,2})?)/g;

  lines.forEach((line, idx) => {
    const dateMatch = line.match(dateRe);
    if (!dateMatch) return;

    const date = parseDate(dateMatch[1]);
    if (!date) return;

    // Remove the date from the line
    let rest = line.replace(dateMatch[0], '').trim();

    // Find all numbers in the rest
    const nums = [...rest.matchAll(amtRe)].map(m => parseFloat(m[1].replace(/,/g, '')));
    if (!nums.length) return;

    // Heuristic: in bank statements last 1-2 numbers are balance; debit is before that
    // Try to detect debit: look for "Dr" or take second-to-last number if >0
    const isDr = /\bdr\b|\bdebit\b|\bwithdraw/i.test(rest);
    const isCr = /\bcr\b|\bcredit\b|\bdeposit/i.test(rest);
    if (isCr && !isDr) return; // skip credits/income

    // Clean up description: remove numbers and common noise words
    let desc = rest
      .replace(amtRe, '')
      .replace(/\b(dr|cr|neft|upi|imps|rtgs|atm|pos|bal|balance|ref|no|transaction|transfer|chq|clg|ach|ecs|nach|inb|mob|net)\b/gi, ' ')
      .replace(/[\/\-\|#*:]+/g, ' ')
      .replace(/\s{2,}/g, ' ')
      .trim();

    // Use the biggest number that's not the balance (last number usually is balance)
    const amt = nums.length >= 2 ? nums[nums.length - 2] : nums[0];
    if (!amt || amt <= 0) return;

    const { cat, confidence } = autoClassify(desc + ' ' + line);
    results.push({ id: Date.now() + idx, date, cat, confidence, amt, note: desc });
  });

  return results;
}

// ── Review Modal ──────────────────────────────────────────
function openReview(rows) {
  pendingImportRows = rows;
  const body = document.getElementById('reviewBody');
  const stats = document.getElementById('reviewStats');

  const high = rows.filter(r=>r.confidence==='high').length;
  const med  = rows.filter(r=>r.confidence==='medium').length;
  const low  = rows.filter(r=>r.confidence==='low').length;
  const total = rows.reduce((s,r)=>s+r.amt,0);

  stats.innerHTML = `
    <span><strong>${rows.length}</strong> transactions &nbsp;|&nbsp; ₹<strong>${fmt(total)}</strong> total</span>
    <span><span class="conf-dot" style="background:var(--green)"></span><strong>${high}</strong> high confidence</span>
    <span><span class="conf-dot" style="background:#f6a623"></span><strong>${med}</strong> medium</span>
    <span><span class="conf-dot" style="background:var(--accent2)"></span><strong>${low}</strong> low — review these</span>
  `;

  body.innerHTML = rows.map((r, i) => {
    const confColor = r.confidence==='high' ? 'var(--green)' : r.confidence==='medium' ? '#f6a623' : 'var(--accent2)';
    const catOptions = CATEGORIES.map(c =>
      `<option value="${c}" ${c===r.cat?'selected':''}>${c}</option>`
    ).join('');
    return `
      <tr id="rrow-${i}" class="">
        <td><input type="checkbox" class="skip-cb" onchange="toggleSkip(${i},this)" title="Skip this row"></td>
        <td>${formatDate(r.date)}</td>
        <td style="max-width:260px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap" title="${r.note}">${r.note||'—'}</td>
        <td style="font-weight:600;color:var(--accent2)">₹${fmt(r.amt)}</td>
        <td><select onchange="updateCat(${i},this.value)">${catOptions}</select></td>
        <td><span class="conf-dot" style="background:${confColor}"></span>${r.confidence}</td>
      </tr>
    `;
  }).join('');

  document.getElementById('reviewModal').style.display = 'flex';
}

function toggleSkip(i, cb) {
  const row = document.getElementById(`rrow-${i}`);
  if (cb.checked) row.classList.add('skipped');
  else row.classList.remove('skipped');
  pendingImportRows[i]._skip = cb.checked;
}

function updateCat(i, val) {
  pendingImportRows[i].cat = val;
  pendingImportRows[i].confidence = 'high'; // user confirmed
}

function closeReview() {
  document.getElementById('reviewModal').style.display = 'none';
  pendingImportRows = [];
  document.getElementById('importFile').value = '';
}

async function confirmImport() {
  const toImport = pendingImportRows.filter(r => !r._skip);
  if (!toImport.length) { showToast('Nothing to import', 'error'); return; }

  const btn = document.querySelector('#reviewModal .btn[onclick="confirmImport()"]');
  if (btn) { btn.disabled = true; btn.textContent = 'Importing…'; }

  try {
    const clean = toImport.map(r => ({ id: r.id, date: r.date, cat: r.cat, amt: r.amt, note: r.note }));
    await bulkAddToSheet(clean);
    closeReview();
    showToast(`${toImport.length} expenses imported!`);
    await render();
  } catch(_) {
    if (btn) { btn.disabled = false; btn.textContent = 'Import Selected'; }
  }
}

function showImportPreview() {}  // kept for compatibility


</script>
</body>
</html>
