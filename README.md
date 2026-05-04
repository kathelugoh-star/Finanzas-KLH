<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Finanzas · KLH</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --cream: #FAF7F2; --sand: #EDE8DF; --mocha: #8B7355; --espresso: #4A3728;
    --sage: #7C9A7E; --rose: #C4857A; --amber: #D4A853; --sky: #7BA7BC;
    --danger: #C4514A; --text: #2C2420; --text-soft: #7A6E65; --white: #FFFFFF;
    --shadow: 0 4px 24px rgba(74,55,40,0.10); --shadow-lg: 0 8px 40px rgba(74,55,40,0.15);
    --radius: 16px; --radius-sm: 10px;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body { font-family:'DM Sans',sans-serif; background:var(--cream); color:var(--text); min-height:100vh; font-size:14px; }

  .app-header { background:var(--espresso); padding:18px 20px 14px; display:flex; align-items:center; justify-content:space-between; position:sticky; top:0; z-index:100; box-shadow:0 2px 16px rgba(74,55,40,0.25); }
  .app-header h1 { font-family:'Playfair Display',serif; color:var(--cream); font-size:18px; font-weight:600; }
  .app-header .subtitle { color:var(--mocha); font-size:10px; font-weight:300; letter-spacing:1.5px; text-transform:uppercase; margin-top:2px; }
  .header-date { color:var(--sand); font-size:11px; text-align:right; line-height:1.6; }
  .header-date span { display:block; color:var(--amber); font-weight:600; font-size:13px; }

  .tabs { display:flex; background:var(--espresso); padding:0 16px; gap:2px; overflow-x:auto; }
  .tab-btn { background:none; border:none; color:var(--mocha); font-family:'DM Sans',sans-serif; font-size:12px; font-weight:500; padding:10px 14px; cursor:pointer; border-bottom:2px solid transparent; transition:all 0.2s; white-space:nowrap; }
  .tab-btn.active { color:var(--amber); border-bottom-color:var(--amber); }

  .main { padding:16px; max-width:820px; margin:0 auto; }
  .tab-panel { display:none; }
  .tab-panel.active { display:block; }

  /* INCOME OVERVIEW */
  .income-card { background:linear-gradient(135deg,var(--espresso),#6B4C38); border-radius:var(--radius); padding:18px 20px; margin-bottom:16px; color:var(--cream); box-shadow:var(--shadow-lg); }
  .income-card .income-label { font-size:10px; text-transform:uppercase; letter-spacing:1.5px; color:var(--mocha); margin-bottom:4px; }
  .income-card .income-amount { font-family:'Playfair Display',serif; font-size:28px; font-weight:700; color:var(--amber); }
  .income-card .income-sub { font-size:11px; color:rgba(250,247,242,0.6); margin-top:2px; }
  .income-grid { display:grid; grid-template-columns:1fr 1fr 1fr; gap:10px; margin-top:14px; }
  .income-stat { background:rgba(255,255,255,0.08); border-radius:10px; padding:10px 12px; }
  .income-stat .s-label { font-size:9px; text-transform:uppercase; letter-spacing:1px; color:var(--mocha); margin-bottom:3px; }
  .income-stat .s-val { font-family:'Playfair Display',serif; font-size:15px; font-weight:700; }
  .income-stat .s-val.red { color:#E8907A; }
  .income-stat .s-val.green { color:#8EC190; }
  .income-stat .s-val.yellow { color:var(--amber); }

  .summary-row { display:grid; grid-template-columns:repeat(3,1fr); gap:10px; margin-bottom:16px; }
  .summary-card { background:var(--white); border-radius:var(--radius); padding:14px; box-shadow:var(--shadow); border-left:4px solid transparent; }
  .summary-card.urgent { border-left-color:var(--rose); }
  .summary-card.soon { border-left-color:var(--amber); }
  .summary-card.ok { border-left-color:var(--sage); }
  .summary-card .label { font-size:9px; text-transform:uppercase; letter-spacing:1.2px; color:var(--text-soft); margin-bottom:5px; font-weight:600; }
  .summary-card .value { font-family:'Playfair Display',serif; font-size:20px; font-weight:700; color:var(--espresso); line-height:1; }
  .summary-card .sub { font-size:10px; color:var(--text-soft); margin-top:3px; }

  .section-title { font-family:'Playfair Display',serif; font-size:15px; color:var(--espresso); margin-bottom:12px; display:flex; align-items:center; gap:8px; }
  .section-title::after { content:''; flex:1; height:1px; background:var(--sand); }

  .payment-list { display:flex; flex-direction:column; gap:8px; margin-bottom:22px; }
  .payment-card { background:var(--white); border-radius:var(--radius); padding:13px 15px; box-shadow:var(--shadow); display:grid; grid-template-columns:36px 1fr auto; gap:12px; align-items:center; cursor:pointer; transition:all 0.2s; border:1.5px solid transparent; position:relative; overflow:hidden; }
  .payment-card::before { content:''; position:absolute; left:0; top:0; bottom:0; width:4px; }
  .payment-card.status-urgent { border-color:rgba(196,133,122,0.25); }
  .payment-card.status-urgent::before { background:var(--rose); }
  .payment-card.status-soon { border-color:rgba(212,168,83,0.25); }
  .payment-card.status-soon::before { background:var(--amber); }
  .payment-card.status-ok { border-color:rgba(124,154,126,0.25); }
  .payment-card.status-ok::before { background:var(--sage); }
  .payment-card.status-paid::before { background:#ccc; }
  .payment-card.status-paid { opacity:0.55; }
  .payment-card:hover { transform:translateX(3px); box-shadow:var(--shadow-lg); }
  .payment-card.pending-info { border-color:rgba(212,168,83,0.4); background:#FFFBF0; }
  .payment-card.pending-info::before { background:var(--amber); }

  .payment-icon { width:36px; height:36px; border-radius:9px; display:flex; align-items:center; justify-content:center; font-size:16px; flex-shrink:0; }
  .payment-info .name { font-weight:600; font-size:13px; color:var(--espresso); margin-bottom:2px; }
  .payment-info .meta { font-size:10px; color:var(--text-soft); display:flex; gap:8px; flex-wrap:wrap; }

  .payment-right { text-align:right; }
  .payment-right .amount { font-family:'Playfair Display',serif; font-size:14px; font-weight:700; color:var(--espresso); }
  .payment-right .days-badge { display:inline-block; padding:2px 7px; border-radius:20px; font-size:9px; font-weight:600; margin-top:3px; letter-spacing:0.3px; }
  .badge-urgent { background:rgba(196,133,122,0.15); color:var(--rose); }
  .badge-soon { background:rgba(212,168,83,0.15); color:#B8852A; }
  .badge-ok { background:rgba(124,154,126,0.15); color:#4A7A4C; }
  .badge-paid { background:rgba(0,0,0,0.06); color:#999; }
  .badge-pending { background:rgba(212,168,83,0.2); color:#8B6000; }

  .cal-hint { background:linear-gradient(135deg,var(--espresso),#6B4C38); border-radius:var(--radius); padding:14px 16px; color:var(--cream); margin-bottom:16px; display:flex; align-items:center; gap:12px; box-shadow:var(--shadow-lg); flex-wrap:wrap; }
  .cal-hint .icon { font-size:24px; flex-shrink:0; }
  .cal-hint h3 { font-size:12px; font-weight:600; margin-bottom:2px; }
  .cal-hint p { font-size:10px; color:var(--mocha); line-height:1.5; }
  .cal-actions { display:flex; gap:6px; margin-left:auto; }
  .btn-cal { background:var(--amber); color:var(--espresso); border:none; padding:7px 12px; border-radius:8px; font-family:'DM Sans',sans-serif; font-size:11px; font-weight:600; cursor:pointer; white-space:nowrap; transition:all 0.2s; }
  .btn-cal:hover { background:#E8BA5A; }

  .add-section { background:var(--white); border-radius:var(--radius); padding:18px; box-shadow:var(--shadow); margin-bottom:18px; }
  .form-grid { display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-bottom:12px; }
  .form-grid.three { grid-template-columns:1fr 1fr 1fr; }
  .form-group label { display:block; font-size:9px; font-weight:600; text-transform:uppercase; letter-spacing:1px; color:var(--text-soft); margin-bottom:4px; }
  .form-group input, .form-group select { width:100%; padding:8px 10px; border:1.5px solid var(--sand); border-radius:8px; font-family:'DM Sans',sans-serif; font-size:12px; color:var(--text); background:var(--cream); transition:border-color 0.2s; outline:none; }
  .form-group input:focus, .form-group select:focus { border-color:var(--mocha); }
  .btn-add { background:var(--espresso); color:var(--cream); border:none; padding:9px 20px; border-radius:8px; font-family:'DM Sans',sans-serif; font-size:12px; font-weight:600; cursor:pointer; transition:all 0.2s; }
  .btn-add:hover { background:var(--mocha); }
  .btn-danger { background:#C4514A; color:white; border:none; padding:5px 10px; border-radius:6px; font-size:10px; font-weight:600; cursor:pointer; margin-top:6px; }

  .budget-bar-container { background:var(--white); border-radius:var(--radius); padding:16px; box-shadow:var(--shadow); margin-bottom:12px; }
  .budget-header { display:flex; justify-content:space-between; align-items:flex-end; margin-bottom:10px; }
  .budget-header .category-name { font-weight:600; font-size:13px; color:var(--espresso); }
  .budget-header .amounts { font-size:11px; color:var(--text-soft); }
  .budget-header .amounts strong { color:var(--espresso); font-family:'Playfair Display',serif; font-size:14px; }
  .bar-track { height:9px; background:var(--sand); border-radius:20px; overflow:hidden; margin-bottom:5px; }
  .bar-fill { height:100%; border-radius:20px; transition:width 0.6s cubic-bezier(0.4,0,0.2,1); }
  .bar-fill.safe { background:linear-gradient(90deg,var(--sage),#8EC190); }
  .bar-fill.warning { background:linear-gradient(90deg,var(--amber),#E8BA5A); }
  .bar-fill.danger { background:linear-gradient(90deg,var(--rose),var(--danger)); }
  .bar-note { font-size:10px; color:var(--text-soft); display:flex; justify-content:space-between; }

  .expense-log { background:var(--white); border-radius:var(--radius); padding:16px; box-shadow:var(--shadow); margin-bottom:16px; }
  .expense-item { display:flex; align-items:center; gap:10px; padding:8px 0; border-bottom:1px solid var(--sand); }
  .expense-item:last-child { border-bottom:none; }
  .expense-dot { width:7px; height:7px; border-radius:50%; flex-shrink:0; }
  .expense-item .exp-info { flex:1; }
  .expense-item .exp-name { font-size:12px; font-weight:500; color:var(--espresso); }
  .expense-item .exp-meta { font-size:10px; color:var(--text-soft); }
  .expense-item .exp-amount { font-family:'Playfair Display',serif; font-size:13px; font-weight:700; color:var(--espresso); }
  .exp-delete { background:none; border:none; color:var(--text-soft); cursor:pointer; font-size:12px; opacity:0.4; transition:opacity 0.2s; }
  .exp-delete:hover { opacity:1; color:var(--danger); }

  .alert-banner { border-radius:var(--radius-sm); padding:10px 14px; margin-bottom:12px; font-size:11px; display:flex; align-items:center; gap:8px; font-weight:500; }
  .alert-warning { background:rgba(212,168,83,0.15); color:#8B6000; border:1px solid rgba(212,168,83,0.3); }
  .alert-danger { background:rgba(196,81,74,0.12); color:var(--danger); border:1px solid rgba(196,81,74,0.25); }
  .alert-ok { background:rgba(124,154,126,0.15); color:#2E6B30; border:1px solid rgba(124,154,126,0.3); }
  .alert-info { background:rgba(123,167,188,0.15); color:#2A5F78; border:1px solid rgba(123,167,188,0.3); }

  .modal-overlay { position:fixed; inset:0; background:rgba(44,36,32,0.6); z-index:200; display:none; align-items:center; justify-content:center; padding:16px; backdrop-filter:blur(4px); }
  .modal-overlay.open { display:flex; }
  .modal { background:var(--white); border-radius:20px; padding:22px; max-width:400px; width:100%; box-shadow:var(--shadow-lg); animation:modalIn 0.25s cubic-bezier(0.34,1.56,0.64,1); max-height:90vh; overflow-y:auto; }
  @keyframes modalIn { from{opacity:0;transform:scale(0.9) translateY(10px)} to{opacity:1;transform:scale(1) translateY(0)} }
  .modal h2 { font-family:'Playfair Display',serif; font-size:17px; color:var(--espresso); margin-bottom:3px; }
  .modal .modal-amount { font-size:11px; color:var(--text-soft); margin-bottom:16px; }
  .info-row { display:flex; justify-content:space-between; align-items:center; padding:7px 10px; background:var(--cream); border-radius:7px; margin-bottom:6px; }
  .info-row .key { font-size:10px; color:var(--text-soft); font-weight:500; }
  .info-row .val { font-size:12px; color:var(--espresso); font-weight:600; }
  .strategy-box { background:linear-gradient(135deg,rgba(124,154,126,0.1),rgba(124,154,126,0.05)); border:1px solid rgba(124,154,126,0.3); border-radius:10px; padding:12px; margin:14px 0; }
  .strategy-box h4 { font-size:10px; text-transform:uppercase; letter-spacing:1px; color:var(--sage); font-weight:700; margin-bottom:5px; }
  .strategy-box p { font-size:11px; color:var(--text); line-height:1.6; }
  .modal-actions { display:flex; gap:7px; flex-wrap:wrap; }
  .btn-modal { flex:1; padding:9px; border-radius:8px; font-family:'DM Sans',sans-serif; font-size:11px; font-weight:600; cursor:pointer; transition:all 0.2s; border:none; min-width:80px; }
  .btn-modal.primary { background:var(--espresso); color:var(--cream); }
  .btn-modal.outline { background:transparent; border:1.5px solid var(--sand); color:var(--text-soft); }
  .btn-modal.success { background:var(--sage); color:white; }
  .btn-modal.del { background:#C4514A; color:white; }

  .toast { position:fixed; bottom:20px; left:50%; transform:translateX(-50%) translateY(80px); background:var(--espresso); color:var(--cream); padding:10px 20px; border-radius:30px; font-size:12px; font-weight:500; box-shadow:var(--shadow-lg); transition:transform 0.4s cubic-bezier(0.34,1.56,0.64,1); z-index:999; white-space:nowrap; }
  .toast.show { transform:translateX(-50%) translateY(0); }

  .pending-badge { background:rgba(212,168,83,0.2); color:#8B6000; font-size:9px; font-weight:700; padding:2px 7px; border-radius:10px; margin-left:6px; }

  @media(max-width:500px) {
    .summary-row { grid-template-columns:1fr 1fr; }
    .form-grid { grid-template-columns:1fr; }
    .form-grid.three { grid-template-columns:1fr 1fr; }
    .income-grid { grid-template-columns:1fr 1fr; }
  }
</style>
</head>
<body>

<header class="app-header">
  <div>
    <h1>💰 Finanzas KLH</h1>
    <div class="subtitle">Gestión Personal de Pagos</div>
  </div>
  <div class="header-date">
    <span id="today-display"></span>
    <span id="month-display" style="font-size:10px;color:var(--mocha);font-weight:400;"></span>
  </div>
</header>

<div class="tabs">
  <button class="tab-btn active" onclick="switchTab('pagos')">📅 Pagos</button>
  <button class="tab-btn" onclick="switchTab('resumen')">📊 Resumen</button>
  <button class="tab-btn" onclick="switchTab('gastos')">💸 Gastos</button>
  <button class="tab-btn" onclick="switchTab('agregar')">＋ Agregar</button>
</div>

<div class="main">

  <!-- TAB: PAGOS -->
  <div class="tab-panel active" id="tab-pagos">
    <div class="summary-row" id="summary-cards"></div>
    <div class="cal-hint">
      <div class="icon">🗓️</div>
      <div style="flex:1;">
        <h3>Exportar al calendario</h3>
        <p>Descarga .ics con recordatorios automáticos para Google Calendar o iPhone.</p>
      </div>
      <div class="cal-actions">
        <button class="btn-cal" onclick="exportICS()">⬇ .ics</button>
      </div>
    </div>
    <div id="urgent-section"><div class="section-title">🔴 Urgente</div><div class="payment-list" id="list-urgent"></div></div>
    <div id="soon-section"><div class="section-title">🟡 Esta semana</div><div class="payment-list" id="list-soon"></div></div>
    <div id="ok-section"><div class="section-title">🟢 Tranquila — Tiempo disponible</div><div class="payment-list" id="list-ok"></div></div>
    <div id="pending-section"><div class="section-title">⚠️ Pendiente de confirmar</div><div class="payment-list" id="list-pending"></div></div>
    <div id="paid-section"><div class="section-title">✅ Pagados</div><div class="payment-list" id="list-paid"></div></div>
  </div>

  <!-- TAB: RESUMEN -->
  <div class="tab-panel" id="tab-resumen">
    <div class="income-card">
      <div class="income-label">Ingreso neto mensual</div>
      <div class="income-amount">$3.674.468</div>
      <div class="income-sub">Ya descontado préstamo Davivienda ($700.000)</div>
      <div class="income-grid" id="income-stats"></div>
    </div>
    <div id="resumen-detail"></div>
  </div>

  <!-- TAB: GASTOS -->
  <div class="tab-panel" id="tab-gastos">
    <div id="budget-alerts"></div>
    <div id="budget-bars"></div>
    <div class="expense-log">
      <div class="section-title">📋 Este mes</div>
      <div id="expense-list-display"></div>
    </div>
    <div class="add-section">
      <div class="section-title" style="margin-bottom:12px;">➕ Registrar gasto</div>
      <div class="form-grid">
        <div class="form-group" style="grid-column:span 2">
          <label>Descripción</label>
          <input type="text" id="exp-name" placeholder="Ej: Mercado, Gasolina..." />
        </div>
      </div>
      <div class="form-grid three">
        <div class="form-group">
          <label>Monto ($)</label>
          <input type="number" id="exp-amount" placeholder="0" />
        </div>
        <div class="form-group">
          <label>Categoría</label>
          <select id="exp-category">
            <option>Alimentación</option><option>Transporte</option><option>Salud</option>
            <option>Entretenimiento</option><option>Ropa</option><option>Personal</option><option>Otros</option>
          </select>
        </div>
        <div class="form-group">
          <label>Fecha</label>
          <input type="date" id="exp-date" />
        </div>
      </div>
      <button class="btn-add" onclick="addExpense()">Registrar</button>
    </div>
  </div>

  <!-- TAB: AGREGAR -->
  <div class="tab-panel" id="tab-agregar">
    <div class="add-section">
      <div class="section-title" style="margin-bottom:14px;">📌 Nueva obligación</div>
      <div class="form-grid">
        <div class="form-group" style="grid-column:span 2">
          <label>Nombre</label>
          <input type="text" id="new-name" placeholder="Ej: Tarjeta Bancolombia..." />
        </div>
      </div>
      <div class="form-grid three">
        <div class="form-group">
          <label>Monto ($)</label>
          <input type="number" id="new-amount" placeholder="0" />
        </div>
        <div class="form-group">
          <label>Día límite</label>
          <input type="number" id="new-due-day" placeholder="Ej: 15" min="1" max="31" />
        </div>
        <div class="form-group">
          <label>Tipo</label>
          <select id="new-type">
            <option value="💳">💳 Tarjeta</option>
            <option value="🏦">🏦 Préstamo</option>
            <option value="🏠">🏠 Arriendo/Admin</option>
            <option value="📱">📱 Telefonía</option>
            <option value="⚡">⚡ Servicios</option>
            <option value="🐾">🐾 Mascota</option>
            <option value="💊">💊 Salud</option>
            <option value="📦">📦 Otro</option>
          </select>
        </div>
      </div>
      <div class="form-grid">
        <div class="form-group">
          <label>Nota (opcional)</label>
          <input type="text" id="new-note" placeholder="Ej: descuento nómina" />
        </div>
        <div class="form-group">
          <label>¿Pendiente de confirmar?</label>
          <select id="new-pending">
            <option value="false">No, tengo el dato</option>
            <option value="true">Sí, falta averiguar</option>
          </select>
        </div>
      </div>
      <button class="btn-add" onclick="addPayment()">Agregar obligación</button>
    </div>

    <div class="add-section">
      <div class="section-title" style="margin-bottom:12px;">📊 Presupuestos mensuales</div>
      <p style="font-size:11px;color:var(--text-soft);margin-bottom:12px;">Ajusta cuánto quieres gastar por categoría.</p>
      <div id="budget-inputs"></div>
    </div>
  </div>

</div>

<!-- MODAL -->
<div class="modal-overlay" id="modal-overlay" onclick="closeModal(event)">
  <div class="modal">
    <h2 id="m-name"></h2>
    <div class="modal-amount" id="m-amount"></div>
    <div id="m-grid"></div>
    <div class="strategy-box">
      <h4>💡 Estrategia de pago</h4>
      <p id="m-strategy"></p>
    </div>
    <div class="modal-actions">
      <button class="btn-modal outline" onclick="closeModalDirect()">Cerrar</button>
      <button class="btn-modal success" id="m-mark-paid" onclick="markPaidFromModal()">✓ Pagado</button>
      <button class="btn-modal primary" onclick="exportSingleICS()">📅 Calendario</button>
      <button class="btn-modal del" onclick="deleteFromModal()">🗑</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const INCOME = 3674468;
const DAVIVIENDA = 700000; // ya descontado de nómina

let payments = JSON.parse(localStorage.getItem('klh_payments_v3') || 'null') || [
  { id:1,  name:'Hipoteca',             amount:758000,  dueDay:30, type:'🏠', note:'Pago más importante del mes', paid:false, pending:false },
  { id:2,  name:'Claro Celular',        amount:56500,   dueDay:30, type:'📱', note:'', paid:false, pending:false },
  { id:3,  name:'Claro Hogar',          amount:121000,  dueDay:30, type:'📱', note:'', paid:false, pending:false },
  { id:4,  name:'Internet Mamá',        amount:170000,  dueDay:30, type:'📡', note:'Pago a favor de mamá', paid:false, pending:false },
  { id:5,  name:'Recibo Gas',           amount:40000,   dueDay:4,  type:'🔥', note:'Vence el 4 de cada mes', paid:false, pending:false },
  { id:6,  name:'Recibo Luz',           amount:150000,  dueDay:15, type:'⚡', note:'', paid:false, pending:false },
  { id:7,  name:'Recibo Agua',          amount:130000,  dueDay:10, type:'💧', note:'Vence el 10 de cada mes', paid:false, pending:false },
  { id:8,  name:'Comida Prince 🐾',     amount:250000,  dueDay:15, type:'🐾', note:'Comida del mes para Prince', paid:false, pending:false },
  { id:9,  name:'Administración',       amount:200000,  dueDay:15, type:'🏢', note:'', paid:false, pending:false },
  { id:10, name:'Gasolina (quincena 1)',amount:150000,  dueDay:15, type:'⛽', note:'Primera quincena', paid:false, pending:false },
  { id:11, name:'Gasolina (quincena 2)',amount:150000,  dueDay:30, type:'⛽', note:'Segunda quincena', paid:false, pending:false },
  { id:12, name:'Medicina Prepagada',   amount:350000,  dueDay:30, type:'💊', note:'', paid:false, pending:false },
  { id:13, name:'Plan Premium Papás',   amount:120000,  dueDay:30, type:'👨‍👩‍👧', note:'Pago a favor de papás', paid:false, pending:false },
  { id:14, name:'Tarjeta Rappi (mes actual)',  amount:90700,   dueDay:30, type:'💳', note:'Refinanciada. Próximo mes: $279.000. Buscar compra de cartera.', paid:false, pending:false },

  { id:16, name:'Tarjeta Itaú',         amount:0,       dueDay:30, type:'💳', note:'⚠️ Averiguar pago mínimo y fecha exacta. Saldo total: $8.300.000', paid:false, pending:true },
];

let expenses = JSON.parse(localStorage.getItem('klh_expenses_v3') || '[]');
let budgets  = JSON.parse(localStorage.getItem('klh_budgets_v3')  || 'null') || {
  'Alimentación':350000,'Transporte':200000,'Salud':80000,
  'Entretenimiento':80000,'Ropa':80000,'Personal':80000,'Otros':100000
};
let currentModalId = null;

function save() {
  localStorage.setItem('klh_payments_v3', JSON.stringify(payments));
  localStorage.setItem('klh_expenses_v3', JSON.stringify(expenses));
  localStorage.setItem('klh_budgets_v3',  JSON.stringify(budgets));
}

function today() { return new Date(); }

function daysUntilDue(dueDay) {
  const now = today();
  const thisMonth = new Date(now.getFullYear(), now.getMonth(), dueDay);
  let diff = Math.ceil((thisMonth - now) / 86400000);
  if (diff < -3) {
    const nextMonth = new Date(now.getFullYear(), now.getMonth()+1, dueDay);
    diff = Math.ceil((nextMonth - now) / 86400000);
  }
  return diff;
}

function strategicPayDate(dueDay) {
  const now = today();
  let base = new Date(now.getFullYear(), now.getMonth(), dueDay - 2);
  if (base <= now) base = new Date(now.getFullYear(), now.getMonth()+1, dueDay - 2);
  return base;
}

function formatDate(d) {
  const ds=['Dom','Lun','Mar','Mié','Jue','Vie','Sáb'];
  const ms=['ene','feb','mar','abr','may','jun','jul','ago','sep','oct','nov','dic'];
  return `${ds[d.getDay()]} ${d.getDate()} ${ms[d.getMonth()]}`;
}

function getStatus(p) {
  if (p.pending) return 'pending';
  if (p.paid) return 'paid';
  const d = daysUntilDue(p.dueDay);
  if (d <= 2) return 'urgent';
  if (d <= 7) return 'soon';
  return 'ok';
}

function strategyText(p) {
  if (p.pending) return '⚠️ Este pago tiene información pendiente de confirmar. Averigua el monto mínimo y la fecha exacta antes de que genere mora.';
  if (p.note && (p.note.toLowerCase().includes('nómina') || p.note.toLowerCase().includes('descuento')))
    return 'Este pago se descuenta automáticamente de nómina. Verifica que tu cuenta tenga saldo suficiente antes del día indicado.';
  const days = daysUntilDue(p.dueDay);
  const pd = strategicPayDate(p.dueDay);
  if (days <= 0) return '🚨 ¡Vence hoy! Paga ahora mismo. Los pagos en línea pueden tardar hasta 24h en aplicarse.';
  if (days <= 2) return `⚠️ ¡Paga hoy o mañana! Queda muy poco margen antes del vencimiento (día ${p.dueDay}).`;
  if (days <= 7) return `Fecha óptima para pagar: ${formatDate(pd)}. Esto te da 2 días de margen para que el pago procese antes del vencimiento (día ${p.dueDay}).`;
  return `Fecha óptima para pagar: ${formatDate(pd)} — 2 días antes del vencimiento (día ${p.dueDay}). Así tienes margen si el pago online tarda 24h en aplicarse.`;
}

// ===== RENDER PAGOS =====
function renderPagos() {
  const urgent=[], soon=[], ok=[], pending=[], paid=[];
  payments.forEach(p => {
    const s = getStatus(p);
    if (s==='urgent') urgent.push(p);
    else if (s==='soon') soon.push(p);
    else if (s==='ok') ok.push(p);
    else if (s==='pending') pending.push(p);
    else paid.push(p);
  });

  renderList('list-urgent', urgent);
  renderList('list-soon', soon);
  renderList('list-ok', ok);
  renderList('list-pending', pending);
  renderList('list-paid', paid);

  document.getElementById('urgent-section').style.display  = urgent.length  ? 'block':'none';
  document.getElementById('soon-section').style.display    = soon.length    ? 'block':'none';
  document.getElementById('ok-section').style.display      = ok.length      ? 'block':'none';
  document.getElementById('pending-section').style.display = pending.length ? 'block':'none';
  document.getElementById('paid-section').style.display    = paid.length    ? 'block':'none';

  const totalPending = payments.filter(p=>!p.paid&&!p.pending).reduce((s,p)=>s+p.amount,0);
  document.getElementById('summary-cards').innerHTML = `
    <div class="summary-card urgent">
      <div class="label">Urgente/Próximo</div>
      <div class="value">${urgent.length+soon.length}</div>
      <div class="sub">por pagar pronto</div>
    </div>
    <div class="summary-card soon">
      <div class="label">Total a pagar</div>
      <div class="value" style="font-size:15px;">$${Math.round(totalPending/1000)}k</div>
      <div class="sub">este mes</div>
    </div>
    <div class="summary-card ok">
      <div class="label">Pagados</div>
      <div class="value">${payments.filter(p=>p.paid).length}/${payments.length}</div>
      <div class="sub">obligaciones</div>
    </div>
  `;
}

function renderList(containerId, list) {
  const el = document.getElementById(containerId);
  if (!list.length) { el.innerHTML='<p style="font-size:11px;color:var(--text-soft);padding:6px 0;">Ninguna 🎉</p>'; return; }
  el.innerHTML = list.map(p => {
    const status = getStatus(p);
    const days = p.pending ? null : daysUntilDue(p.dueDay);
    const pd = p.pending ? null : strategicPayDate(p.dueDay);
    let badgeClass='badge-ok', badgeText=`${days}d`;
    if (status==='urgent') { badgeClass='badge-urgent'; badgeText=days<=0?'¡HOY!':days+'d'; }
    else if (status==='soon') badgeClass='badge-soon';
    else if (status==='paid') { badgeClass='badge-paid'; badgeText='✓'; }
    else if (status==='pending') { badgeClass='badge-pending'; badgeText='❓'; }

    const extraClass = p.pending ? ' pending-info' : '';
    const amountDisplay = p.pending ? '❓ Por confirmar' : '$'+p.amount.toLocaleString();

    return `<div class="payment-card status-${status}${extraClass}" onclick="openModal(${p.id})">
      <div class="payment-icon" style="background:rgba(74,55,40,0.07)">${p.type}</div>
      <div class="payment-info">
        <div class="name">${p.name}${p.pending?'<span class="pending-badge">PENDIENTE</span>':''}</div>
        <div class="meta">
          ${p.pending ? '<span>⚠️ Falta averiguar info</span>' : `<span>📅 Vence día ${p.dueDay}</span><span>💡 Pagar: ${formatDate(pd)}</span>`}
          ${p.note&&!p.pending?`<span>· ${p.note}</span>`:''}
        </div>
      </div>
      <div class="payment-right">
        <div class="amount">${amountDisplay}</div>
        <div class="days-badge ${badgeClass}">${badgeText}</div>
      </div>
    </div>`;
  }).join('');
}

// ===== MODAL =====
function openModal(id) {
  const p = payments.find(x=>x.id===id);
  currentModalId = id;
  const days = p.pending ? null : daysUntilDue(p.dueDay);
  const pd   = p.pending ? null : strategicPayDate(p.dueDay);

  document.getElementById('m-name').textContent   = `${p.type} ${p.name}`;
  document.getElementById('m-amount').textContent = p.pending ? 'Monto pendiente de confirmar' : `$${p.amount.toLocaleString()} COP mensuales`;
  document.getElementById('m-grid').innerHTML = `
    ${!p.pending?`
    <div class="info-row"><span class="key">📅 Vence</span><span class="val">Día ${p.dueDay} de cada mes</span></div>
    <div class="info-row"><span class="key">💡 Pagar óptimo</span><span class="val">${formatDate(pd)}</span></div>
    <div class="info-row"><span class="key">⏳ Días restantes</span><span class="val">${days>0?days+' días':'¡Hoy!'}</span></div>`:''}
    <div class="info-row"><span class="key">📊 Estado</span><span class="val">${p.paid?'✅ Pagado':p.pending?'⚠️ Pendiente de confirmar':getStatus(p)==='urgent'?'🔴 Urgente':getStatus(p)==='soon'?'🟡 Próximo':'🟢 Tranquila'}</span></div>
    ${p.note?`<div class="info-row"><span class="key">📝 Nota</span><span class="val">${p.note}</span></div>`:''}
  `;
  document.getElementById('m-strategy').textContent = strategyText(p);
  document.getElementById('m-mark-paid').textContent = p.paid ? '↩ Desmarcar' : '✓ Marcar pagado';
  document.getElementById('modal-overlay').classList.add('open');
}

function closeModal(e) { if(e.target===document.getElementById('modal-overlay')) closeModalDirect(); }
function closeModalDirect() { document.getElementById('modal-overlay').classList.remove('open'); currentModalId=null; }

function markPaidFromModal() {
  const p = payments.find(x=>x.id===currentModalId);
  p.paid = !p.paid;
  save(); renderPagos(); renderResumen(); closeModalDirect();
  showToast(p.paid?'✅ ¡Pago registrado!':'Desmarcado como pendiente');
}

function deleteFromModal() {
  if(!confirm('¿Eliminar esta obligación?')) return;
  payments = payments.filter(x=>x.id!==currentModalId);
  save(); renderPagos(); renderResumen(); closeModalDirect();
  showToast('🗑 Eliminado');
}

// ===== RESUMEN FINANCIERO =====
function renderResumen() {
  const totalObligaciones = payments.filter(p=>!p.pending).reduce((s,p)=>s+p.amount,0) + DAVIVIENDA;
  const disponible = INCOME - totalObligaciones;
  const pct = Math.round(totalObligaciones/INCOME*100);

  document.getElementById('income-stats').innerHTML = `
    <div class="income-stat">
      <div class="s-label">Obligaciones fijas</div>
      <div class="s-val red">$${Math.round(totalObligaciones/1000)}k</div>
    </div>
    <div class="income-stat">
      <div class="s-label">Disponible real</div>
      <div class="s-val ${disponible>0?'green':'red'}">$${Math.round(disponible/1000)}k</div>
    </div>
    <div class="income-stat">
      <div class="s-label">% comprometido</div>
      <div class="s-val ${pct>80?'red':pct>60?'yellow':'green'}">${pct}%</div>
    </div>
  `;

  const grupos = [
    { titulo:'🏠 Vivienda y servicios', items: payments.filter(p=>[1,5,6,7,9].includes(p.id)) },
    { titulo:'📱 Comunicaciones', items: payments.filter(p=>[2,3,4,13].includes(p.id)) },
    { titulo:'💳 Deudas y tarjetas', items: payments.filter(p=>[14,16].includes(p.id)), extra: [{name:'Préstamo Davivienda (incluye compra cartera Bogotá)',amount:DAVIVIENDA,note:'Descuento automático nómina',type:'🏦'}] },
    { titulo:'🏥 Salud', items: payments.filter(p=>[12].includes(p.id)) },
    { titulo:'🐾 Otros gastos fijos', items: payments.filter(p=>[8,10,11].includes(p.id)) },
  ];

  let html = '';
  grupos.forEach(g => {
    const allItems = [...g.items, ...(g.extra||[])];
    const subtotal = allItems.reduce((s,p)=>s+(p.amount||0),0);
    html += `<div class="budget-bar-container">
      <div class="budget-header">
        <div class="category-name">${g.titulo}</div>
        <div class="amounts"><strong>$${subtotal.toLocaleString()}</strong></div>
      </div>
      ${allItems.map(p=>`
        <div style="display:flex;justify-content:space-between;font-size:11px;padding:3px 0;border-bottom:1px solid var(--sand);">
          <span style="color:var(--text-soft)">${p.type||''} ${p.name}</span>
          <span style="font-weight:600;color:var(--espresso)">${p.pending?'❓':p.amount===0?'❓':'$'+p.amount.toLocaleString()}</span>
        </div>`).join('')}
    </div>`;
  });

  const alertColor = disponible < 0 ? 'alert-danger' : disponible < 300000 ? 'alert-warning' : 'alert-ok';
  const alertMsg = disponible < 0
    ? `🔴 Tus obligaciones superan tu ingreso en $${Math.abs(disponible).toLocaleString()}. Es urgente revisar qué se puede ajustar.`
    : disponible < 300000
    ? `🟡 Te quedan $${disponible.toLocaleString()} después de obligaciones. Margen muy ajustado — cuidado con gastos variables.`
    : `🟢 Te quedan $${disponible.toLocaleString()} disponibles después de todas las obligaciones fijas.`;

  document.getElementById('resumen-detail').innerHTML =
    `<div class="alert-banner ${alertColor}" style="margin-bottom:16px;">${alertMsg}</div>
     <div class="alert-banner alert-info">ℹ️ La Tarjeta Itaú aún no tiene monto confirmado — el disponible real puede ser menor.</div>
     ${html}`;
}

// ===== ICS =====
function generateICS(list) {
  const lines=['BEGIN:VCALENDAR','VERSION:2.0','PRODID:-//KLH Finanzas//ES','CALSCALE:GREGORIAN'];
  const now = today();
  list.filter(p=>!p.pending&&!p.paid).forEach(p=>{
    for(let m=0;m<12;m++){
      const pd = new Date(now.getFullYear(),now.getMonth()+m,p.dueDay-2);
      const dd = new Date(now.getFullYear(),now.getMonth()+m,p.dueDay);
      lines.push('BEGIN:VEVENT');
      lines.push(`UID:klh-${p.id}-${m}-${Date.now()}@finanzas`);
      lines.push(`DTSTART;VALUE=DATE:${pd.toISOString().slice(0,10).replace(/-/g,'')}`);
      lines.push(`DTEND;VALUE=DATE:${dd.toISOString().slice(0,10).replace(/-/g,'')}`);
      lines.push(`SUMMARY:💰 Pagar ${p.name} - $${p.amount.toLocaleString()}`);
      lines.push(`DESCRIPTION:Vence día ${p.dueDay}. Paga hoy para que procese a tiempo.\\nNota: ${p.note||'Sin nota'}`);
      lines.push('BEGIN:VALARM\nTRIGGER:-PT0S\nACTION:DISPLAY');
      lines.push(`DESCRIPTION:⏰ Pagar ${p.name} hoy\nEND:VALARM`);
      lines.push('BEGIN:VALARM\nTRIGGER:-P1D\nACTION:DISPLAY');
      lines.push(`DESCRIPTION:⚠️ Mañana: Pagar ${p.name}\nEND:VALARM`);
      lines.push('END:VEVENT');
    }
  });
  lines.push('END:VCALENDAR');
  return lines.join('\r\n');
}

function exportICS() {
  const blob=new Blob([generateICS(payments)],{type:'text/calendar;charset=utf-8'});
  const a=document.createElement('a'); a.href=URL.createObjectURL(blob);
  a.download='KLH_Pagos.ics'; a.click();
  showToast('📅 Descargado. Ábrelo para importar al calendario.');
}

function exportSingleICS() {
  const p=payments.find(x=>x.id===currentModalId);
  if(!p||p.pending) return;
  const blob=new Blob([generateICS([p])],{type:'text/calendar;charset=utf-8'});
  const a=document.createElement('a'); a.href=URL.createObjectURL(blob);
  a.download=`KLH_${p.name.replace(/\s+/g,'_')}.ics`; a.click();
  showToast('📅 Evento exportado.');
}

// ===== ADD PAYMENT =====
function addPayment() {
  const name=document.getElementById('new-name').value.trim();
  const amount=parseInt(document.getElementById('new-amount').value)||0;
  const dueDay=parseInt(document.getElementById('new-due-day').value);
  const type=document.getElementById('new-type').value;
  const note=document.getElementById('new-note').value.trim();
  const pending=document.getElementById('new-pending').value==='true';
  if(!name||!dueDay){showToast('⚠️ Completa nombre y día');return;}
  payments.push({id:Date.now(),name,amount,dueDay,type,note,paid:false,pending});
  save(); renderPagos(); renderResumen();
  showToast('✅ Obligación agregada');
  ['new-name','new-amount','new-due-day','new-note'].forEach(id=>document.getElementById(id).value='');
  switchTab('pagos');
}

// ===== EXPENSES =====
function addExpense() {
  const name=document.getElementById('exp-name').value.trim();
  const amount=parseInt(document.getElementById('exp-amount').value);
  const category=document.getElementById('exp-category').value;
  const date=document.getElementById('exp-date').value||today().toISOString().slice(0,10);
  if(!name||!amount){showToast('⚠️ Completa nombre y monto');return;}
  expenses.unshift({id:Date.now(),name,amount,category,date});
  if(expenses.length>100) expenses=expenses.slice(0,100);
  save(); renderExpenses();
  showToast('💸 Gasto registrado');
  document.getElementById('exp-name').value='';
  document.getElementById('exp-amount').value='';
}

function deleteExpense(id) {
  expenses=expenses.filter(e=>e.id!==id);
  save(); renderExpenses();
}

function getMonthExpenses() {
  const m=today().getMonth(),y=today().getFullYear();
  return expenses.filter(e=>{const d=new Date(e.date);return d.getMonth()===m&&d.getFullYear()===y;});
}

function renderExpenses() {
  const monthExp=getMonthExpenses();
  const totals={};
  Object.keys(budgets).forEach(c=>totals[c]=0);
  monthExp.forEach(e=>{if(totals[e.category]!==undefined)totals[e.category]+=e.amount;else totals['Otros']+=e.amount;});

  let alertsHTML='';
  Object.entries(budgets).forEach(([cat,budget])=>{
    const spent=totals[cat]||0, pct=budget>0?spent/budget:0;
    if(pct>=1) alertsHTML+=`<div class="alert-banner alert-danger">🔴 <strong>${cat}:</strong> ¡Superaste el presupuesto! $${spent.toLocaleString()} / $${budget.toLocaleString()}</div>`;
    else if(pct>=0.8) alertsHTML+=`<div class="alert-banner alert-warning">🟡 <strong>${cat}:</strong> Al ${Math.round(pct*100)}%. Quedan $${(budget-spent).toLocaleString()}</div>`;
  });
  if(!alertsHTML) alertsHTML=`<div class="alert-banner alert-ok">✅ Todos los presupuestos bajo control este mes</div>`;
  document.getElementById('budget-alerts').innerHTML=alertsHTML;

  document.getElementById('budget-bars').innerHTML=Object.entries(budgets).map(([cat,budget])=>{
    const spent=totals[cat]||0,pct=Math.min(budget>0?spent/budget:0,1);
    const cls=pct>=1?'danger':pct>=0.8?'warning':'safe';
    return `<div class="budget-bar-container">
      <div class="budget-header"><div class="category-name">${cat}</div><div class="amounts"><strong>$${spent.toLocaleString()}</strong> / $${budget.toLocaleString()}</div></div>
      <div class="bar-track"><div class="bar-fill ${cls}" style="width:${pct*100}%"></div></div>
      <div class="bar-note"><span>${Math.round(pct*100)}% usado</span><span>Quedan $${Math.max(budget-spent,0).toLocaleString()}</span></div>
    </div>`;
  }).join('');

  const COLORS={'Alimentación':'#7C9A7E','Transporte':'#7BA7BC','Salud':'#C4857A','Entretenimiento':'#D4A853','Ropa':'#B07CC6','Personal':'#C46E99','Otros':'#8B7355'};
  document.getElementById('expense-list-display').innerHTML=monthExp.length
    ?monthExp.slice(0,25).map(e=>`<div class="expense-item">
        <div class="expense-dot" style="background:${COLORS[e.category]||'#888'}"></div>
        <div class="exp-info"><div class="exp-name">${e.name}</div><div class="exp-meta">${e.category} · ${e.date}</div></div>
        <div class="exp-amount">$${e.amount.toLocaleString()}</div>
        <button class="exp-delete" onclick="deleteExpense(${e.id})">✕</button>
      </div>`).join('')
    :'<p style="font-size:11px;color:var(--text-soft);padding:10px 0;">No hay gastos registrados este mes.</p>';
}

function renderBudgetInputs() {
  document.getElementById('budget-inputs').innerHTML=Object.entries(budgets).map(([cat,val])=>`
    <div style="display:flex;align-items:center;gap:10px;margin-bottom:8px;">
      <label style="font-size:12px;font-weight:500;color:var(--espresso);flex:1;">${cat}</label>
      <input type="number" value="${val}" style="width:130px;padding:7px 10px;border:1.5px solid var(--sand);border-radius:8px;font-family:'DM Sans',sans-serif;font-size:12px;background:var(--cream);outline:none;" onchange="updateBudget('${cat}',this.value)" />
    </div>`).join('');
}
function updateBudget(cat,val) { budgets[cat]=parseInt(val)||0; save(); renderExpenses(); }

function switchTab(tab) {
  document.querySelectorAll('.tab-btn').forEach((b,i)=>{
    b.classList.toggle('active',['pagos','resumen','gastos','agregar'][i]===tab);
  });
  document.querySelectorAll('.tab-panel').forEach(p=>p.classList.remove('active'));
  document.getElementById('tab-'+tab).classList.add('active');
}

function showToast(msg) {
  const t=document.getElementById('toast');
  t.textContent=msg; t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),2800);
}

function init() {
  const d=today();
  const ds=['Domingo','Lunes','Martes','Miércoles','Jueves','Viernes','Sábado'];
  const ms=['Enero','Febrero','Marzo','Abril','Mayo','Junio','Julio','Agosto','Septiembre','Octubre','Noviembre','Diciembre'];
  document.getElementById('today-display').textContent=`${ds[d.getDay()]} ${d.getDate()}`;
  document.getElementById('month-display').textContent=`${ms[d.getMonth()]} ${d.getFullYear()}`;
  document.getElementById('exp-date').value=d.toISOString().slice(0,10);
  renderPagos(); renderResumen(); renderExpenses(); renderBudgetInputs();
}

init();
</script>
</body>
</html>
