<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FinançasPRO</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0c0f;
    --surface: #111418;
    --surface2: #181c22;
    --border: #252a32;
    --accent: #00e5a0;
    --accent2: #ff4d6d;
    --accent3: #ffd166;
    --text: #e8ecf0;
    --muted: #5a6370;
    --radius: 12px;
  }

  body {
    font-family: 'DM Mono', monospace;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    padding: 0;
  }

  /* ---- LOGIN ---- */
  #loginCard {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: var(--bg);
    position: relative;
    overflow: hidden;
  }
  #loginCard::before {
    content: '';
    position: absolute;
    width: 600px; height: 600px;
    background: radial-gradient(circle, rgba(0,229,160,0.08) 0%, transparent 70%);
    top: -150px; left: -150px;
    pointer-events: none;
  }
  #loginCard::after {
    content: '';
    position: absolute;
    width: 400px; height: 400px;
    background: radial-gradient(circle, rgba(255,77,109,0.06) 0%, transparent 70%);
    bottom: -100px; right: -100px;
    pointer-events: none;
  }

  .login-box {
    width: 380px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 48px 40px;
    position: relative;
    z-index: 1;
    animation: slideUp 0.6s ease;
  }
  .login-logo {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 28px;
    color: var(--accent);
    margin-bottom: 6px;
    letter-spacing: -1px;
  }
  .login-sub {
    color: var(--muted);
    font-size: 12px;
    margin-bottom: 36px;
  }
  .login-box label {
    display: block;
    font-size: 11px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 6px;
  }
  .login-box input {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    color: var(--text);
    font-family: 'DM Mono', monospace;
    font-size: 14px;
    padding: 12px 14px;
    margin-bottom: 20px;
    outline: none;
    transition: border-color 0.2s;
  }
  .login-box input:focus { border-color: var(--accent); }
  .btn-login {
    width: 100%;
    background: var(--accent);
    color: #000;
    border: none;
    border-radius: 8px;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 15px;
    padding: 14px;
    cursor: pointer;
    transition: opacity 0.2s, transform 0.1s;
    margin-top: 4px;
  }
  .btn-login:hover { opacity: 0.85; }
  .btn-login:active { transform: scale(0.98); }

  /* ---- APP ---- */
  #app { display: none; }

  .topbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 18px 32px;
    border-bottom: 1px solid var(--border);
    background: var(--surface);
    position: sticky;
    top: 0;
    z-index: 100;
  }
  .topbar-logo {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 20px;
    color: var(--accent);
    letter-spacing: -1px;
  }
  .topbar-user {
    font-size: 12px;
    color: var(--muted);
  }
  .topbar-user span { color: var(--text); }
  .btn-logout {
    background: var(--surface2);
    color: var(--muted);
    border: 1px solid var(--border);
    border-radius: 7px;
    font-family: 'DM Mono', monospace;
    font-size: 12px;
    padding: 7px 14px;
    cursor: pointer;
    transition: color 0.2s, border-color 0.2s;
    margin-left: 16px;
  }
  .btn-logout:hover { color: var(--accent2); border-color: var(--accent2); }

  .main {
    max-width: 1100px;
    margin: 0 auto;
    padding: 32px 24px;
  }

  /* ---- CARDS SUMMARY ---- */
  .summary-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    margin-bottom: 28px;
  }
  .summary-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 22px 24px;
    position: relative;
    overflow: hidden;
    transition: transform 0.2s;
  }
  .summary-card:hover { transform: translateY(-2px); }
  .summary-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 3px;
  }
  .summary-card.receitas::before { background: var(--accent); }
  .summary-card.despesas::before { background: var(--accent2); }
  .summary-card.saldo::before { background: var(--accent3); }
  .summary-label {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 1.5px;
    color: var(--muted);
    margin-bottom: 10px;
  }
  .summary-value {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 28px;
    letter-spacing: -1px;
  }
  .summary-card.receitas .summary-value { color: var(--accent); }
  .summary-card.despesas .summary-value { color: var(--accent2); }
  .summary-card.saldo .summary-value { color: var(--accent3); }
  .summary-icon {
    position: absolute;
    right: 20px; top: 20px;
    font-size: 28px;
    opacity: 0.12;
  }

  /* ---- LAYOUT 2 colunas ---- */
  .content-grid {
    display: grid;
    grid-template-columns: 1fr 380px;
    gap: 20px;
    align-items: start;
  }

  /* ---- FORM ---- */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 24px;
    margin-bottom: 20px;
  }
  .card-title {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 16px;
    margin-bottom: 20px;
    color: var(--text);
  }

  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .form-group { margin-bottom: 14px; }
  .form-group label {
    display: block;
    font-size: 11px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 6px;
  }
  .form-group input,
  .form-group select {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    color: var(--text);
    font-family: 'DM Mono', monospace;
    font-size: 13px;
    padding: 10px 12px;
    outline: none;
    transition: border-color 0.2s;
    appearance: none;
  }
  .form-group input:focus,
  .form-group select:focus { border-color: var(--accent); }
  .form-group select option { background: var(--surface2); }

  .tipo-toggle {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
    margin-bottom: 16px;
  }
  .tipo-btn {
    padding: 10px;
    border-radius: 8px;
    border: 1px solid var(--border);
    background: var(--surface2);
    color: var(--muted);
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
    text-align: center;
  }
  .tipo-btn.active-receita {
    background: rgba(0,229,160,0.12);
    border-color: var(--accent);
    color: var(--accent);
  }
  .tipo-btn.active-despesa {
    background: rgba(255,77,109,0.12);
    border-color: var(--accent2);
    color: var(--accent2);
  }

  .btn-salvar {
    width: 100%;
    background: var(--accent);
    color: #000;
    border: none;
    border-radius: 8px;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 14px;
    padding: 13px;
    cursor: pointer;
    transition: opacity 0.2s, transform 0.1s;
    margin-top: 4px;
  }
  .btn-salvar:hover { opacity: 0.85; }
  .btn-salvar:active { transform: scale(0.99); }

  /* ---- LISTA ---- */
  .lista-wrapper { max-height: 420px; overflow-y: auto; }
  .lista-wrapper::-webkit-scrollbar { width: 4px; }
  .lista-wrapper::-webkit-scrollbar-track { background: transparent; }
  .lista-wrapper::-webkit-scrollbar-thumb { background: var(--border); border-radius: 2px; }

  .transacao-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 14px 0;
    border-bottom: 1px solid var(--border);
    animation: slideUp 0.3s ease;
  }
  .transacao-item:last-child { border-bottom: none; }
  .t-badge {
    width: 36px; height: 36px;
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px;
    flex-shrink: 0;
  }
  .t-badge.receita { background: rgba(0,229,160,0.12); }
  .t-badge.despesa { background: rgba(255,77,109,0.12); }
  .t-info { flex: 1; min-width: 0; }
  .t-cat {
    font-size: 13px;
    font-weight: 500;
    color: var(--text);
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
  }
  .t-desc {
    font-size: 11px;
    color: var(--muted);
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
  }
  .t-date { font-size: 11px; color: var(--muted); text-align: right; }
  .t-valor {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 15px;
    text-align: right;
    min-width: 90px;
  }
  .t-valor.receita { color: var(--accent); }
  .t-valor.despesa { color: var(--accent2); }
  .t-actions { display: flex; gap: 6px; }
  .t-btn {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 6px;
    color: var(--muted);
    font-size: 12px;
    padding: 5px 9px;
    cursor: pointer;
    transition: all 0.15s;
    font-family: 'DM Mono', monospace;
  }
  .t-btn:hover { color: var(--text); border-color: var(--text); }
  .t-btn.del:hover { color: var(--accent2); border-color: var(--accent2); }

  .empty-state {
    text-align: center;
    padding: 40px 20px;
    color: var(--muted);
    font-size: 13px;
  }
  .empty-state .emoji { font-size: 36px; margin-bottom: 12px; }

  /* ---- GRAFICO ---- */
  .chart-container {
    position: relative;
    height: 240px;
  }

  /* ---- FILTRO ---- */
  .filter-row {
    display: flex;
    gap: 8px;
    margin-bottom: 16px;
  }
  .filter-chip {
    background: var(--surface2);
    border: 1px solid var(--border);
    color: var(--muted);
    border-radius: 20px;
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    padding: 5px 12px;
    cursor: pointer;
    transition: all 0.15s;
  }
  .filter-chip.active {
    background: rgba(0,229,160,0.12);
    border-color: var(--accent);
    color: var(--accent);
  }

  /* ---- UTILS ---- */
  @keyframes slideUp {
    from { opacity: 0; transform: translateY(16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .toast {
    position: fixed;
    bottom: 28px; right: 28px;
    background: var(--surface2);
    border: 1px solid var(--accent);
    border-radius: 10px;
    padding: 14px 20px;
    font-size: 13px;
    color: var(--accent);
    z-index: 999;
    animation: slideUp 0.3s ease;
    box-shadow: 0 8px 32px rgba(0,229,160,0.15);
  }

  @media (max-width: 768px) {
    .summary-grid { grid-template-columns: 1fr; }
    .content-grid { grid-template-columns: 1fr; }
    .form-row { grid-template-columns: 1fr; }
    .topbar { padding: 14px 16px; }
    .main { padding: 20px 14px; }
    .login-box { width: 90%; padding: 36px 28px; }
  }
</style>
</head>
<body>

<!-- LOGIN -->
<div id="loginCard">
  <div class="login-box">
    <div class="login-logo">FinançasPRO</div>
    <div class="login-sub">Controle financeiro inteligente</div>
    <label>Usuário</label>
    <input id="usuario" placeholder="seu_usuario" autocomplete="username">
    <label>Senha</label>
    <input id="senha" type="password" placeholder="••••••••" autocomplete="current-password">
    <button class="btn-login" onclick="login()">Entrar →</button>
  </div>
</div>

<!-- APP -->
<div id="app">
  <div class="topbar">
    <div class="topbar-logo">FinançasPRO</div>
    <div style="display:flex;align-items:center;gap:8px;">
      <div class="topbar-user">Olá, <span id="nomeUsuario"></span></div>
      <button class="btn-logout" onclick="logout()">Sair</button>
    </div>
  </div>

  <div class="main">

    <!-- SUMMARY -->
    <div class="summary-grid">
      <div class="summary-card receitas">
        <div class="summary-icon">↑</div>
        <div class="summary-label">Receitas</div>
        <div class="summary-value">R$ <span id="totalReceita">0,00</span></div>
      </div>
      <div class="summary-card despesas">
        <div class="summary-icon">↓</div>
        <div class="summary-label">Despesas</div>
        <div class="summary-value">R$ <span id="totalDespesa">0,00</span></div>
      </div>
      <div class="summary-card saldo">
        <div class="summary-icon">◎</div>
        <div class="summary-label">Saldo</div>
        <div class="summary-value">R$ <span id="saldo">0,00</span></div>
      </div>
    </div>

    <div class="content-grid">

      <!-- ESQUERDA: lista + gráfico -->
      <div>
        <div class="card">
          <div class="card-title">Transações</div>
          <div class="filter-row">
            <button class="filter-chip active" onclick="setFiltro('todos', this)">Todos</button>
            <button class="filter-chip" onclick="setFiltro('receita', this)">Receitas</button>
            <button class="filter-chip" onclick="setFiltro('despesa', this)">Despesas</button>
          </div>
          <div class="lista-wrapper">
            <div id="lista"></div>
          </div>
        </div>

        <div class="card">
          <div class="card-title">Gastos por Categoria</div>
          <div class="chart-container">
            <canvas id="grafico"></canvas>
          </div>
        </div>
      </div>

      <!-- DIREITA: form -->
      <div>
        <div class="card" style="position:sticky;top:80px;">
          <div class="card-title" id="formTitle">Nova Transação</div>

          <div class="tipo-toggle">
            <button class="tipo-btn active-receita" id="btnReceita" onclick="setTipo('receita')">↑ Receita</button>
            <button class="tipo-btn" id="btnDespesa" onclick="setTipo('despesa')">↓ Despesa</button>
          </div>

          <div class="form-group">
            <label>Categoria</label>
            <input type="text" id="categoria" placeholder="Ex: Salário, iFood, Mercado">
          </div>
          <div class="form-group">
            <label>Descrição (opcional)</label>
            <input type="text" id="descricao" placeholder="Detalhes...">
          </div>
          <div class="form-row">
            <div class="form-group">
              <label>Valor (R$)</label>
              <input type="number" id="valor" placeholder="0,00" step="0.01" min="0">
            </div>
            <div class="form-group">
              <label>Data</label>
              <input type="date" id="data">
            </div>
          </div>

          <button class="btn-salvar" id="btnSalvar" onclick="salvar()">Salvar Transação</button>
        </div>
      </div>

    </div>
  </div>
</div>

<script>
let usuarioAtual = null;
let transacoes = [];
let tipoAtual = 'receita';
let filtroAtual = 'todos';
let editandoId = null;
let grafico = null;

// set today as default date
document.getElementById('data').valueAsDate = new Date();

function login() {
  const user = document.getElementById('usuario').value.trim();
  const senha = document.getElementById('senha').value;
  if (!user || !senha) { showToast('Preencha usuário e senha'); return; }

  usuarioAtual = user;
  transacoes = JSON.parse(localStorage.getItem('fp_' + user) || '[]');

  document.getElementById('loginCard').style.display = 'none';
  document.getElementById('app').style.display = 'block';
  document.getElementById('nomeUsuario').textContent = user;

  atualizar();
}

function logout() {
  usuarioAtual = null;
  document.getElementById('app').style.display = 'none';
  document.getElementById('loginCard').style.display = 'flex';
  document.getElementById('usuario').value = '';
  document.getElementById('senha').value = '';
}

function setTipo(tipo) {
  tipoAtual = tipo;
  const btnR = document.getElementById('btnReceita');
  const btnD = document.getElementById('btnDespesa');
  btnR.className = 'tipo-btn' + (tipo === 'receita' ? ' active-receita' : '');
  btnD.className = 'tipo-btn' + (tipo === 'despesa' ? ' active-despesa' : '');
}

function setFiltro(filtro, el) {
  filtroAtual = filtro;
  document.querySelectorAll('.filter-chip').forEach(c => c.classList.remove('active'));
  el.classList.add('active');
  renderLista();
}

function salvar() {
  const categoria = document.getElementById('categoria').value.trim();
  const descricao = document.getElementById('descricao').value.trim();
  const valor = parseFloat(document.getElementById('valor').value);
  const data = document.getElementById('data').value;

  if (!valor || valor <= 0) { showToast('Insira um valor válido'); return; }
  if (!categoria) { showToast('Informe uma categoria'); return; }

  if (editandoId) {
    transacoes = transacoes.filter(t => t.id !== editandoId);
    editandoId = null;
    document.getElementById('formTitle').textContent = 'Nova Transação';
    document.getElementById('btnSalvar').textContent = 'Salvar Transação';
  }

  transacoes.unshift({ id: Date.now(), tipo: tipoAtual, categoria, descricao, valor, data });
  salvarStorage();
  limparForm();
  atualizar();
  showToast(tipoAtual === 'receita' ? 'Receita adicionada!' : 'Despesa adicionada!');
}

function salvarStorage() {
  localStorage.setItem('fp_' + usuarioAtual, JSON.stringify(transacoes));
}

function limparForm() {
  document.getElementById('categoria').value = '';
  document.getElementById('descricao').value = '';
  document.getElementById('valor').value = '';
  document.getElementById('data').valueAsDate = new Date();
}

function excluir(id) {
  if (!confirm('Excluir esta transação?')) return;
  transacoes = transacoes.filter(t => t.id !== id);
  salvarStorage();
  atualizar();
  showToast('Transação excluída');
}

function editar(id) {
  const t = transacoes.find(t => t.id === id);
  if (!t) return;
  editandoId = id;
  setTipo(t.tipo);
  document.getElementById('categoria').value = t.categoria;
  document.getElementById('descricao').value = t.descricao || '';
  document.getElementById('valor').value = t.valor;
  document.getElementById('data').value = t.data;
  document.getElementById('formTitle').textContent = 'Editar Transação';
  document.getElementById('btnSalvar').textContent = 'Atualizar';
  document.querySelector('.content-grid').scrollIntoView({ behavior: 'smooth' });
}

function atualizar() {
  let receita = 0, despesa = 0;
  const categorias = {};

  transacoes.forEach(t => {
    if (t.tipo === 'receita') receita += t.valor;
    if (t.tipo === 'despesa') {
      despesa += t.valor;
      categorias[t.categoria] = (categorias[t.categoria] || 0) + t.valor;
    }
  });

  const fmt = n => n.toLocaleString('pt-BR', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
  document.getElementById('totalReceita').textContent = fmt(receita);
  document.getElementById('totalDespesa').textContent = fmt(despesa);
  const saldo = receita - despesa;
  document.getElementById('saldo').textContent = fmt(Math.abs(saldo));
  const saldoEl = document.getElementById('saldo');
  saldoEl.style.color = saldo < 0 ? 'var(--accent2)' : '';

  renderLista();
  renderGrafico(categorias);
}

function renderLista() {
  const lista = document.getElementById('lista');
  const filtered = filtroAtual === 'todos' ? transacoes : transacoes.filter(t => t.tipo === filtroAtual);

  if (!filtered.length) {
    lista.innerHTML = `<div class="empty-state"><div class="emoji">📊</div>Nenhuma transação encontrada.</div>`;
    return;
  }

  const fmt = n => n.toLocaleString('pt-BR', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
  const fmtDate = d => d ? new Date(d + 'T00:00:00').toLocaleDateString('pt-BR') : '';

  lista.innerHTML = filtered.map(t => `
    <div class="transacao-item">
      <div class="t-badge ${t.tipo}">${t.tipo === 'receita' ? '↑' : '↓'}</div>
      <div class="t-info">
        <div class="t-cat">${t.categoria}</div>
        <div class="t-desc">${t.descricao || '—'}</div>
      </div>
      <div>
        <div class="t-valor ${t.tipo}">R$ ${fmt(t.valor)}</div>
        <div class="t-date">${fmtDate(t.data)}</div>
      </div>
      <div class="t-actions">
        <button class="t-btn" onclick="editar(${t.id})">✎</button>
        <button class="t-btn del" onclick="excluir(${t.id})">✕</button>
      </div>
    </div>
  `).join('');
}

function renderGrafico(categorias) {
  const ctx = document.getElementById('grafico').getContext('2d');
  if (grafico) grafico.destroy();

  const keys = Object.keys(categorias);
  if (!keys.length) return;

  const colors = ['#00e5a0','#ff4d6d','#ffd166','#06d6a0','#ef476f','#ffd60a','#118ab2','#073b4c','#8ecae6','#219ebc'];

  grafico = new Chart(ctx, {
    type: 'doughnut',
    data: {
      labels: keys,
      datasets: [{
        data: Object.values(categorias),
        backgroundColor: colors.slice(0, keys.length),
        borderWidth: 2,
        borderColor: '#111418',
        hoverOffset: 6
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          position: 'right',
          labels: {
            color: '#5a6370',
            font: { family: 'DM Mono', size: 11 },
            boxWidth: 12,
            padding: 12
          }
        },
        tooltip: {
          callbacks: {
            label: ctx => ` R$ ${ctx.raw.toLocaleString('pt-BR', { minimumFractionDigits: 2 })}`
          }
        }
      },
      cutout: '60%'
    }
  });
}

function showToast(msg) {
  const old = document.querySelector('.toast');
  if (old) old.remove();
  const toast = document.createElement('div');
  toast.className = 'toast';
  toast.textContent = msg;
  document.body.appendChild(toast);
  setTimeout(() => toast.remove(), 2800);
}
</script>
</body>
</html>
