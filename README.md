<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dimensionamento Elétrico Residencial — Passo a Passo</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  :root {
    --azul-escuro: #1a3a5c;
    --azul-medio: #2563a8;
    --azul-claro: #dbeafe;
    --laranja: #d97706;
    --laranja-bg: #fef3c7;
    --verde: #15803d;
    --verde-bg: #dcfce7;
    --vermelho: #b91c1c;
    --vermelho-bg: #fee2e2;
    --cinza-bg: #f8fafc;
    --cinza-borda: #e5e7eb;
    --cinza-texto: #6b7280;
    --texto: #1e293b;
  }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    background: #f3f4f6;
    color: var(--texto);
    line-height: 1.5;
    padding: 0;
    min-height: 100vh;
  }
  header {
    background: var(--azul-escuro);
    color: white;
    padding: 18px 28px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  header h1 {
    font-size: 20px;
    font-weight: 700;
  }
  header .sub {
    font-size: 13px;
    color: var(--azul-claro);
    margin-top: 2px;
  }

  .container {
    max-width: 1000px;
    margin: 24px auto;
    padding: 0 16px;
  }

  /* ----- Steps indicator ----- */
  .steps {
    display: flex;
    background: white;
    border-radius: 10px;
    padding: 14px;
    margin-bottom: 20px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.06);
    gap: 4px;
    overflow-x: auto;
  }
  .step {
    flex: 1;
    min-width: 110px;
    text-align: center;
    padding: 10px 6px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 12px;
    transition: all 0.2s;
    border: 2px solid transparent;
    background: var(--cinza-bg);
    color: var(--cinza-texto);
  }
  .step.active {
    background: var(--azul-medio);
    color: white;
    font-weight: 600;
  }
  .step.done {
    background: var(--verde-bg);
    color: var(--verde);
    font-weight: 500;
  }
  .step .num {
    display: block;
    font-size: 16px;
    font-weight: 700;
    margin-bottom: 2px;
  }

  /* ----- Cards / Sections ----- */
  .card {
    background: white;
    border-radius: 10px;
    padding: 24px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.06);
    margin-bottom: 18px;
  }
  .card h2 {
    color: var(--azul-escuro);
    font-size: 18px;
    margin-bottom: 6px;
    border-left: 4px solid var(--azul-medio);
    padding-left: 10px;
  }
  .card .desc {
    color: var(--cinza-texto);
    font-size: 14px;
    margin-bottom: 18px;
  }
  .card h3 {
    color: var(--azul-escuro);
    font-size: 15px;
    margin: 18px 0 10px;
  }

  /* ----- Forms ----- */
  .grid-2 {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 14px;
  }
  .grid-3 {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 14px;
  }
  @media (max-width: 700px) {
    .grid-2, .grid-3 { grid-template-columns: 1fr; }
  }
  .field { display: flex; flex-direction: column; gap: 4px; }
  .field label {
    font-size: 12.5px;
    font-weight: 600;
    color: var(--azul-escuro);
  }
  .field input, .field select, .field textarea {
    padding: 9px 11px;
    border: 1.5px solid var(--cinza-borda);
    border-radius: 6px;
    font-size: 14px;
    font-family: inherit;
    transition: border-color 0.15s;
  }
  .field input:focus, .field select:focus, .field textarea:focus {
    outline: none;
    border-color: var(--azul-medio);
  }
  .field .hint {
    font-size: 11.5px;
    color: var(--cinza-texto);
  }

  /* ----- Buttons ----- */
  button {
    cursor: pointer;
    border: none;
    border-radius: 6px;
    font-size: 14px;
    font-weight: 600;
    padding: 10px 18px;
    transition: all 0.15s;
    font-family: inherit;
  }
  .btn-primary {
    background: var(--azul-medio);
    color: white;
  }
  .btn-primary:hover { background: var(--azul-escuro); }
  .btn-secondary {
    background: var(--cinza-bg);
    color: var(--azul-escuro);
    border: 1.5px solid var(--cinza-borda);
  }
  .btn-secondary:hover { background: var(--cinza-borda); }
  .btn-danger {
    background: var(--vermelho-bg);
    color: var(--vermelho);
  }
  .btn-danger:hover { background: #fecaca; }
  .btn-small {
    padding: 5px 10px;
    font-size: 12px;
  }
  .btn-add {
    background: var(--verde-bg);
    color: var(--verde);
    border: 1.5px dashed var(--verde);
  }
  .btn-add:hover { background: #bbf7d0; }

  .actions {
    display: flex;
    gap: 10px;
    justify-content: space-between;
    margin-top: 24px;
    flex-wrap: wrap;
  }

  /* ----- Lists (ambientes, equipamentos) ----- */
  .item-list { display: flex; flex-direction: column; gap: 10px; margin-top: 8px; }
  .item-row {
    background: var(--cinza-bg);
    border: 1px solid var(--cinza-borda);
    border-radius: 8px;
    padding: 12px;
    display: grid;
    grid-template-columns: 2fr 1fr 1fr 1fr auto;
    gap: 10px;
    align-items: end;
  }
  .item-row.equip {
    grid-template-columns: 2fr 1fr 1fr 1fr auto;
  }
  @media (max-width: 700px) {
    .item-row, .item-row.equip { grid-template-columns: 1fr; }
  }
  .item-row .field input, .item-row .field select { padding: 7px 9px; font-size: 13px; }

  /* ----- Tables ----- */
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13.5px;
    margin: 10px 0;
  }
  th {
    background: var(--azul-escuro);
    color: white;
    padding: 9px 10px;
    text-align: left;
    font-weight: 600;
    font-size: 13px;
  }
  td {
    padding: 8px 10px;
    border-bottom: 1px solid var(--cinza-borda);
  }
  tr:nth-child(even) td { background: var(--cinza-bg); }
  td.num { text-align: right; font-variant-numeric: tabular-nums; }
  td.center { text-align: center; }
  td.bold { font-weight: 600; color: var(--azul-escuro); }

  /* ----- Boxes ----- */
  .box-info, .box-warn, .box-ok, .box-fail {
    border-radius: 8px;
    padding: 12px 14px;
    margin: 12px 0;
    border-left: 4px solid;
    font-size: 14px;
  }
  .box-info { background: var(--azul-claro); border-color: var(--azul-medio); color: #1e3a8a; }
  .box-warn { background: var(--laranja-bg); border-color: var(--laranja); color: #78350f; }
  .box-ok   { background: var(--verde-bg);   border-color: var(--verde);   color: #14532d; }
  .box-fail { background: var(--vermelho-bg);border-color: var(--vermelho);color: #7f1d1d; }
  .box-info b, .box-warn b, .box-ok b, .box-fail b { display: block; margin-bottom: 4px; }

  /* ----- Equação rendering ----- */
  .eq {
    background: var(--cinza-bg);
    padding: 10px 14px;
    border-radius: 6px;
    text-align: center;
    margin: 8px 0;
    font-family: "Cambria Math", "Times New Roman", serif;
    font-size: 16px;
    color: var(--azul-escuro);
  }
  .eq sub { font-size: 0.7em; }
  .eq sup { font-size: 0.7em; }

  /* ----- Resumo cards ----- */
  .stats {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 12px;
    margin: 16px 0;
  }
  .stat {
    background: var(--cinza-bg);
    border-left: 4px solid var(--azul-medio);
    padding: 12px;
    border-radius: 6px;
  }
  .stat .label { font-size: 12px; color: var(--cinza-texto); margin-bottom: 4px; }
  .stat .value { font-size: 20px; font-weight: 700; color: var(--azul-escuro); }
  .stat .unit { font-size: 13px; color: var(--cinza-texto); }

  /* ----- Diagrama unifilar SVG ----- */
  #diagrama-container {
    background: white;
    border: 1px solid var(--cinza-borda);
    border-radius: 8px;
    padding: 16px;
    overflow-x: auto;
    margin: 12px 0;
  }
  #diagrama-container svg {
    display: block;
    margin: 0 auto;
    max-width: 100%;
    height: auto;
  }

  /* ----- Tabs (modo ambiente vs lista) ----- */
  .tabs {
    display: flex;
    gap: 4px;
    margin-bottom: 16px;
    border-bottom: 2px solid var(--cinza-borda);
  }
  .tab {
    padding: 8px 16px;
    background: transparent;
    border: none;
    border-bottom: 3px solid transparent;
    cursor: pointer;
    font-weight: 600;
    color: var(--cinza-texto);
    font-size: 14px;
    margin-bottom: -2px;
  }
  .tab.active {
    color: var(--azul-medio);
    border-bottom-color: var(--azul-medio);
  }

  .hidden { display: none !important; }

  .progress-help {
    background: var(--azul-claro);
    border-radius: 8px;
    padding: 10px 14px;
    margin: 12px 0;
    font-size: 13px;
    color: #1e3a8a;
  }
  .progress-help b { color: var(--azul-escuro); }

  /* Print */
  @media print {
    header, .steps, .actions, .tabs { display: none; }
    .card { box-shadow: none; page-break-inside: avoid; }
    body { background: white; }
  }
</style>
</head>
<body>

<header>
  <h1>⚡ Dimensionamento Elétrico Residencial</h1>
  <div class="sub">Da previsão de cargas ao diagrama unifilar — passo a passo conforme NBR 5410:2004</div>
</header>

<div class="container">

  <!-- ============= NAVEGAÇÃO DE ETAPAS ============= -->
  <div class="steps" id="steps">
    <div class="step active" data-step="1"><span class="num">1</span>Projeto</div>
    <div class="step" data-step="2"><span class="num">2</span>Ambientes</div>
    <div class="step" data-step="3"><span class="num">3</span>Equipamentos</div>
    <div class="step" data-step="4"><span class="num">4</span>Resultados</div>
    <div class="step" data-step="5"><span class="num">5</span>Obra</div>
  </div>

  <!-- ============= ETAPA 1: PROJETO ============= -->
  <div class="card etapa" data-etapa="1">
    <h2>Etapa 1 — Dados do Projeto</h2>
    <p class="desc">Informações gerais que servem de base para todos os cálculos.</p>

    <div class="grid-2">
      <div class="field">
        <label>Nome / identificação do projeto</label>
        <input type="text" id="proj-nome" placeholder="Ex.: Residência João da Silva">
      </div>
      <div class="field">
        <label>Cidade / UF</label>
        <input type="text" id="proj-cidade" placeholder="Ex.: Cascavel/PR">
      </div>
      <div class="field">
        <label>Tensão de alimentação (V)</label>
        <select id="proj-tensao">
          <option value="220">220 V — bifásico</option>
          <option value="127">127 V — monofásico</option>
          <option value="380">380 V — trifásico</option>
        </select>
        <div class="hint">No Paraná o padrão Copel mais comum é 220 V bifásico</div>
      </div>
      <div class="field">
        <label>Fator de conversão ANEEL</label>
        <input type="number" id="proj-aneel" value="0.92" step="0.01" min="0.5" max="1">
        <div class="hint">Padrão = 0,92 (kW = kVA × 0,92)</div>
      </div>
    </div>

    <div class="progress-help">
      <b>Como funciona:</b> nas próximas etapas você informa os ambientes (com área e perímetro) e/ou
      uma lista simples de equipamentos. O app aplica os critérios da NBR 5410 para calcular
      automaticamente a TUG, TUE, iluminação, divisão de circuitos, dimensionamento de cabos e
      o diagrama unifilar.
    </div>

    <div class="actions">
      <button class="btn-danger btn-small" onclick="resetProjeto()">🗑 Limpar tudo</button>
      <button class="btn-primary" onclick="irPara(2)">Próximo: Ambientes →</button>
    </div>
  </div>

  <!-- ============= ETAPA 2: AMBIENTES ============= -->
  <div class="card etapa hidden" data-etapa="2">
    <h2>Etapa 2 — Ambientes da Edificação</h2>
    <p class="desc">Adicione os ambientes com sua área e perímetro. O app aplica automaticamente os critérios de TUG mínima e número de pontos de iluminação.</p>

    <div class="box-info">
      <b>💡 Critérios NBR 5410 aplicados:</b>
      • Banheiros e ambientes ≤ 6 m²: mínimo 1 TUG de 100 VA<br>
      • Demais ambientes: 1 TUG de 100 VA + 1 a cada 5 m de perímetro<br>
      • Cozinha, copa e área de serviço: as 3 primeiras a cada 3,5 m com 600 VA cada<br>
      • Iluminação: 1 ponto de 100 W (ajustável por ambiente)
    </div>

    <h3>Ambientes adicionados</h3>
    <div class="item-list" id="lista-ambientes"></div>

    <div style="margin-top: 12px;">
      <button class="btn-add" onclick="adicionarAmbiente()">+ Adicionar ambiente</button>
    </div>

    <div class="actions">
      <button class="btn-secondary" onclick="irPara(1)">← Voltar</button>
      <button class="btn-primary" onclick="irPara(3)">Próximo: Equipamentos →</button>
    </div>
  </div>

  <!-- ============= ETAPA 3: EQUIPAMENTOS (TUE) ============= -->
  <div class="card etapa hidden" data-etapa="3">
    <h2>Etapa 3 — Equipamentos de Uso Específico</h2>
    <p class="desc">Liste todos os equipamentos fixos de maior potência (chuveiros, ar-condicionado, fornos, máquinas de lavar etc.). Cada um vira um circuito exclusivo no quadro.</p>

    <div class="box-info">
      <b>💡 Sobre ar-condicionado:</b> informe a potência em BTU/h e o app converte para watts considerando
      o COP típico (BTU/h × 0,293 ÷ COP). Para splits residenciais é usado COP = 3,2 por padrão.
    </div>

    <h3>Equipamentos cadastrados</h3>
    <div class="item-list" id="lista-equipamentos"></div>

    <div style="margin-top: 12px;">
      <button class="btn-add" onclick="adicionarEquipamento()">+ Adicionar equipamento</button>
      <button class="btn-secondary btn-small" onclick="exemploEquipamentos()">📋 Carregar exemplo</button>
    </div>

    <div class="actions">
      <button class="btn-secondary" onclick="irPara(2)">← Voltar</button>
      <button class="btn-primary" onclick="calcularEIr(4)">Calcular e ver resultados →</button>
    </div>
  </div>

  <!-- ============= ETAPA 4: RESULTADOS ============= -->
  <div class="card etapa hidden" data-etapa="4">
    <h2>Etapa 4 — Memorial de Cálculo</h2>
    <p class="desc">Resultados completos do dimensionamento conforme NBR 5410:2004.</p>

    <div id="resultados-container">
      <!-- preenchido por JS -->
    </div>

    <div class="actions">
      <button class="btn-secondary" onclick="irPara(3)">← Voltar</button>
      <button class="btn-primary" onclick="irPara(5)">Próximo: Guia de Obra →</button>
    </div>
  </div>

  <!-- ============= ETAPA 5: RECOMENDAÇÕES DE OBRA ============= -->
  <div class="card etapa hidden" data-etapa="5">
    <h2>Etapa 5 — Guia de Execução em Obra</h2>
    <p class="desc">Recomendações práticas para o eletricista e para a vistoria do engenheiro responsável.</p>

    <div id="obra-container">
      <!-- preenchido por JS -->
    </div>

    <div class="actions">
      <button class="btn-secondary" onclick="irPara(4)">← Voltar</button>
      <button class="btn-primary" onclick="window.print()">🖨 Imprimir / Salvar PDF</button>
    </div>
  </div>

</div>

<script>
/* ============================================================
   ESTADO E PERSISTÊNCIA (localStorage)
   ============================================================ */
const STORAGE_KEY = "dim_eletrico_v1";

let state = {
  projeto: { nome: "", cidade: "", tensao: 220, aneel: 0.92 },
  ambientes: [],
  equipamentos: [],
  iluminacaoExtra: {}, // por ambiente, pontos extras
};

function salvar() {
  // sincroniza inputs do form com state
  state.projeto.nome   = document.getElementById("proj-nome").value;
  state.projeto.cidade = document.getElementById("proj-cidade").value;
  state.projeto.tensao = parseFloat(document.getElementById("proj-tensao").value);
  state.projeto.aneel  = parseFloat(document.getElementById("proj-aneel").value);

  localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
}

function carregar() {
  const raw = localStorage.getItem(STORAGE_KEY);
  if (!raw) return;
  try {
    state = JSON.parse(raw);
    document.getElementById("proj-nome").value   = state.projeto.nome   || "";
    document.getElementById("proj-cidade").value = state.projeto.cidade || "";
    document.getElementById("proj-tensao").value = state.projeto.tensao || 220;
    document.getElementById("proj-aneel").value  = state.projeto.aneel  || 0.92;
  } catch (e) {
    console.warn("Erro ao carregar estado:", e);
  }
}

function resetProjeto() {
  if (!confirm("Tem certeza? Todos os dados serão apagados.")) return;
  localStorage.removeItem(STORAGE_KEY);
  location.reload();
}

/* ============================================================
   NAVEGAÇÃO ENTRE ETAPAS
   ============================================================ */
let etapaAtual = 1;

function irPara(n) {
  salvar();
  document.querySelectorAll(".etapa").forEach(el => el.classList.add("hidden"));
  document.querySelector(`.etapa[data-etapa="${n}"]`).classList.remove("hidden");
  document.querySelectorAll(".step").forEach(s => {
    const sn = parseInt(s.dataset.step);
    s.classList.remove("active", "done");
    if (sn < n) s.classList.add("done");
    if (sn === n) s.classList.add("active");
  });
  etapaAtual = n;
  window.scrollTo({ top: 0, behavior: "smooth" });
}

document.querySelectorAll(".step").forEach(s => {
  s.addEventListener("click", () => irPara(parseInt(s.dataset.step)));
});

/* ============================================================
   AMBIENTES
   ============================================================ */
const TIPOS_AMBIENTE = [
  { key: "sala",     nome: "Sala (estar/jantar)", iluminacao: 2 },
  { key: "quarto",   nome: "Quarto / dormitório", iluminacao: 1 },
  { key: "cozinha",  nome: "Cozinha",             iluminacao: 1 },
  { key: "servico",  nome: "Área de serviço",     iluminacao: 1 },
  { key: "banheiro", nome: "Banheiro / lavabo",   iluminacao: 1 },
  { key: "corredor", nome: "Corredor / hall",     iluminacao: 1 },
  { key: "garagem",  nome: "Garagem / varanda",   iluminacao: 1 },
  { key: "outro",    nome: "Outro ambiente",      iluminacao: 1 },
];

function adicionarAmbiente(amb = null) {
  if (!amb) {
    amb = { id: Date.now() + Math.random(), tipo: "quarto", nome: "", area: 10, perimetro: 12, pontosIlum: 1 };
    state.ambientes.push(amb);
  }
  salvar();
  renderAmbientes();
}

function removerAmbiente(id) {
  state.ambientes = state.ambientes.filter(a => a.id !== id);
  salvar(); renderAmbientes();
}

function atualizarAmbiente(id, campo, valor) {
  const amb = state.ambientes.find(a => a.id === id);
  if (!amb) return;
  amb[campo] = valor;
  // Se mudou o tipo, sugere pontos de iluminação padrão se ainda for o default
  if (campo === "tipo") {
    const tipo = TIPOS_AMBIENTE.find(t => t.key === valor);
    if (tipo) amb.pontosIlum = tipo.iluminacao;
  }
  salvar();
}

function renderAmbientes() {
  const cont = document.getElementById("lista-ambientes");
  if (state.ambientes.length === 0) {
    cont.innerHTML = '<div style="color: var(--cinza-texto); font-size: 14px; padding: 12px; background: var(--cinza-bg); border-radius: 6px; text-align:center;">Nenhum ambiente adicionado ainda.</div>';
    return;
  }
  cont.innerHTML = state.ambientes.map(a => `
    <div class="item-row">
      <div class="field">
        <label>Tipo / Nome</label>
        <select onchange="atualizarAmbiente(${a.id}, 'tipo', this.value); renderAmbientes()">
          ${TIPOS_AMBIENTE.map(t => `<option value="${t.key}" ${t.key===a.tipo?'selected':''}>${t.nome}</option>`).join("")}
        </select>
      </div>
      <div class="field">
        <label>Área (m²)</label>
        <input type="number" min="0" step="0.1" value="${a.area}"
          onchange="atualizarAmbiente(${a.id}, 'area', parseFloat(this.value)||0)">
      </div>
      <div class="field">
        <label>Perímetro (m)</label>
        <input type="number" min="0" step="0.1" value="${a.perimetro}"
          onchange="atualizarAmbiente(${a.id}, 'perimetro', parseFloat(this.value)||0)">
      </div>
      <div class="field">
        <label>Pontos iluminação</label>
        <input type="number" min="0" step="1" value="${a.pontosIlum}"
          onchange="atualizarAmbiente(${a.id}, 'pontosIlum', parseInt(this.value)||0)">
      </div>
      <div class="field">
        <label>&nbsp;</label>
        <button class="btn-danger btn-small" onclick="removerAmbiente(${a.id})">Remover</button>
      </div>
    </div>
  `).join("");
}

/* ============================================================
   EQUIPAMENTOS (TUE)
   ============================================================ */
const TIPOS_EQUIPAMENTO = [
  { key: "chuveiro",  nome: "Chuveiro elétrico",      potencia: 5500, unidade: "W"   },
  { key: "torneira",  nome: "Torneira elétrica",      potencia: 3500, unidade: "W"   },
  { key: "aquecedor", nome: "Aquecedor de passagem",  potencia: 6000, unidade: "W"   },
  { key: "ar_split",  nome: "Ar-condicionado split",  potencia: 9000, unidade: "BTU" },
  { key: "forno",     nome: "Forno elétrico",         potencia: 1500, unidade: "W"   },
  { key: "microondas",nome: "Micro-ondas",            potencia: 2000, unidade: "W"   },
  { key: "lavadora",  nome: "Máquina de lavar roupa", potencia: 1000, unidade: "W"   },
  { key: "secadora",  nome: "Secadora de roupas",     potencia: 3500, unidade: "W"   },
  { key: "louca",     nome: "Máquina de lavar louça", potencia: 1500, unidade: "W"   },
  { key: "outro_w",   nome: "Outro (em watts)",       potencia: 1000, unidade: "W"   },
  { key: "outro_btu", nome: "Outro (em BTU/h)",       potencia: 9000, unidade: "BTU" },
];

// BTU/h → Watts (potência elétrica consumida)
// 1 BTU/h = 0,293 W térmico; COP típico residencial = 3,2
function btuParaWatts(btu, cop = 3.2) {
  return (btu * 0.293) / cop;
}

function adicionarEquipamento(eq = null) {
  if (!eq) {
    eq = { id: Date.now() + Math.random(), tipo: "chuveiro", quantidade: 1, potencia: 5500, unidade: "W", ambiente: "Banheiro" };
    state.equipamentos.push(eq);
  }
  salvar();
  renderEquipamentos();
}

function removerEquipamento(id) {
  state.equipamentos = state.equipamentos.filter(e => e.id !== id);
  salvar(); renderEquipamentos();
}

function atualizarEquipamento(id, campo, valor) {
  const eq = state.equipamentos.find(e => e.id === id);
  if (!eq) return;
  eq[campo] = valor;
  if (campo === "tipo") {
    const tipo = TIPOS_EQUIPAMENTO.find(t => t.key === valor);
    if (tipo) {
      eq.potencia = tipo.potencia;
      eq.unidade  = tipo.unidade;
    }
  }
  salvar();
}

function exemploEquipamentos() {
  state.equipamentos = [
    { id: 1, tipo: "chuveiro", quantidade: 1, potencia: 5500, unidade: "W",   ambiente: "Banheiro" },
    { id: 2, tipo: "ar_split", quantidade: 1, potencia: 9000, unidade: "BTU", ambiente: "Quarto" },
    { id: 3, tipo: "forno",    quantidade: 1, potencia: 1500, unidade: "W",   ambiente: "Cozinha" },
    { id: 4, tipo: "lavadora", quantidade: 1, potencia: 1000, unidade: "W",   ambiente: "Área de serviço" },
  ];
  salvar(); renderEquipamentos();
}

function renderEquipamentos() {
  const cont = document.getElementById("lista-equipamentos");
  if (state.equipamentos.length === 0) {
    cont.innerHTML = '<div style="color: var(--cinza-texto); font-size: 14px; padding: 12px; background: var(--cinza-bg); border-radius: 6px; text-align:center;">Nenhum equipamento adicionado ainda.</div>';
    return;
  }
  cont.innerHTML = state.equipamentos.map(e => {
    const watts = e.unidade === "BTU" ? btuParaWatts(e.potencia).toFixed(0) : e.potencia;
    return `
    <div class="item-row equip">
      <div class="field">
        <label>Equipamento</label>
        <select onchange="atualizarEquipamento(${e.id}, 'tipo', this.value); renderEquipamentos()">
          ${TIPOS_EQUIPAMENTO.map(t => `<option value="${t.key}" ${t.key===e.tipo?'selected':''}>${t.nome}</option>`).join("")}
        </select>
      </div>
      <div class="field">
        <label>Qtd</label>
        <input type="number" min="1" step="1" value="${e.quantidade}"
          onchange="atualizarEquipamento(${e.id}, 'quantidade', parseInt(this.value)||1)">
      </div>
      <div class="field">
        <label>Potência (${e.unidade})</label>
        <input type="number" min="0" step="50" value="${e.potencia}"
          onchange="atualizarEquipamento(${e.id}, 'potencia', parseFloat(this.value)||0)">
        ${e.unidade === "BTU" ? `<div class="hint">≈ ${watts} W elétricos</div>` : ''}
      </div>
      <div class="field">
        <label>Ambiente</label>
        <input type="text" value="${e.ambiente}" placeholder="Ex.: Banheiro"
          onchange="atualizarEquipamento(${e.id}, 'ambiente', this.value)">
      </div>
      <div class="field">
        <label>&nbsp;</label>
        <button class="btn-danger btn-small" onclick="removerEquipamento(${e.id})">Remover</button>
      </div>
    </div>`;
  }).join("");
}

/* ============================================================
   CÁLCULOS — núcleo do dimensionamento (NBR 5410:2004)
   ============================================================ */

// Calcula TUG de um ambiente conforme regras NBR 5410
function calcularTUG(amb) {
  const isCozinha = ["cozinha", "servico"].includes(amb.tipo);
  const isBanheiro = amb.tipo === "banheiro";
  let pontos, potencia, criterio;

  if (isCozinha) {
    // Cozinha/área de serviço: as 3 primeiras tomadas a cada 3,5 m, 600 VA cada
    // Demais (após 3 pontos): 100 VA cada
    const nTotal = Math.max(1, Math.ceil(amb.perimetro / 3.5));
    const n600 = Math.min(3, nTotal);
    const n100 = Math.max(0, nTotal - 3);
    pontos = nTotal;
    potencia = n600 * 600 + n100 * 100;
    criterio = `${n600}×600 VA${n100>0 ? ` + ${n100}×100 VA` : ''}`;
  } else if (isBanheiro || amb.area <= 6) {
    // Banheiro ou ambiente ≤ 6m²: 1 ponto de 100 VA
    pontos = 1;
    potencia = 100;
    criterio = `1×100 VA (mín. norma)`;
  } else {
    // Demais: 1 ponto + 1 a cada 5 m
    pontos = 1 + Math.floor(amb.perimetro / 5);
    potencia = pontos * 100;
    criterio = `${pontos}×100 VA`;
  }
  return { pontos, potencia, criterio };
}

// Fator de demanda para iluminação + TUG (NBR 5410, tabela)
function fatorDemandaIluminacaoTUG(potW) {
  if (potW <=  1000) return 0.86;
  if (potW <=  2000) return 0.75;
  if (potW <=  3000) return 0.66;
  if (potW <=  4000) return 0.59;
  if (potW <=  5000) return 0.52;
  if (potW <=  6000) return 0.45;
  if (potW <=  7000) return 0.40;
  if (potW <=  8000) return 0.35;
  if (potW <=  9000) return 0.31;
  if (potW <= 10000) return 0.27;
  return 0.24;
}

// Capacidade de condução de cabos PVC, método B1, 3 condutores carregados (NBR 5410)
const AMPACIDADE = {
  1.5:  15.5,
  2.5:  21,
  4:    28,
  6:    36,
  10:   50,
  16:   68,
  25:   89,
  35:  111,
  50:  134,
};

// Fator de correção por agrupamento (NBR 5410, tabela 42)
function fatorCorrecao(nCircuitos) {
  const tab = { 1: 1.00, 2: 0.80, 3: 0.70, 4: 0.65, 5: 0.60, 6: 0.57, 7: 0.54, 8: 0.52, 9: 0.50 };
  return tab[Math.min(nCircuitos, 9)] || 0.50;
}

// Disjuntores comerciais (valores nominais usuais)
const DISJUNTORES = [6, 10, 16, 20, 25, 32, 40, 50, 63, 70, 80, 100];

function disjuntorComercial(iProj) {
  return DISJUNTORES.find(d => d >= iProj) || 125;
}

// Eletroduto PVC comercial — diâmetros internos aproximados (mm)
const ELETRODUTOS = [
  { nominal: "20 mm (1/2\")",  diametro: 17.0 },
  { nominal: "25 mm (3/4\")",  diametro: 21.5 },
  { nominal: "32 mm (1\")",    diametro: 27.5 },
  { nominal: "40 mm (1.1/4\")",diametro: 35.5 },
  { nominal: "50 mm (1.1/2\")",diametro: 41.0 },
  { nominal: "60 mm (2\")",    diametro: 52.5 },
];

// Diâmetro externo aproximado dos cabos isolados em mm
const DIAMETRO_CABO = {
  1.5: 2.8, 2.5: 3.3, 4: 3.8, 6: 4.4, 10: 5.5, 16: 6.7, 25: 8.0, 35: 9.2, 50: 10.5,
};

// Escolhe eletroduto comercial considerando taxa de ocupação NBR 5410
function dimensionarEletroduto(bitolas) {
  // bitolas: array de mm² com TODOS os condutores (fase+neutro+terra de cada circuito)
  const areaTotal = bitolas.reduce((acc, b) => {
    const d = DIAMETRO_CABO[b] || 5;
    return acc + Math.PI * (d/2) ** 2;
  }, 0);

  // Taxa máxima: 1 cond = 53%, 2 cond = 31%, 3+ cond = 40%
  const taxa = bitolas.length === 1 ? 0.53 : bitolas.length === 2 ? 0.31 : 0.40;

  for (const eltd of ELETRODUTOS) {
    const areaUtil = Math.PI * (eltd.diametro/2) ** 2 * taxa;
    if (areaUtil >= areaTotal) {
      return { ...eltd, ocupacao: (areaTotal / (Math.PI * (eltd.diametro/2)**2) * 100).toFixed(1) };
    }
  }
  return { nominal: "≥ 75 mm (rever projeto)", diametro: 75, ocupacao: "—" };
}

function escolherBitola(iProj, fc, ambitoForca) {
  // Iluminação: mínimo 1,5 mm²; força (TUG/TUE): mínimo 2,5 mm²
  const minimo = ambitoForca ? 2.5 : 1.5;
  const bitolasOrdenadas = Object.keys(AMPACIDADE).map(Number).sort((a, b) => a - b);
  for (const bitola of bitolasOrdenadas) {
    if (bitola < minimo) continue;
    const iC = AMPACIDADE[bitola];
    const iZ = iC * fc;
    if (iZ >= iProj) return { bitola, iC, iZ: parseFloat(iZ.toFixed(2)) };
  }
  return { bitola: 50, iC: 134, iZ: parseFloat((134*fc).toFixed(2)) };
}

// FUNÇÃO PRINCIPAL DE DIMENSIONAMENTO
function dimensionar() {
  const V = state.projeto.tensao;
  const fpAneel = state.projeto.aneel;

  // ----- 1) Calcular TUG e iluminação por ambiente -----
  const tugPorAmbiente = state.ambientes.map(a => {
    const tug = calcularTUG(a);
    const ilum = a.pontosIlum * 100;
    return { ...a, tug, iluminacao: ilum };
  });

  const potTUG  = tugPorAmbiente.reduce((s, a) => s + a.tug.potencia, 0);
  const potIlum = tugPorAmbiente.reduce((s, a) => s + a.iluminacao,   0);

  // ----- 2) Calcular TUE total -----
  let potTUE = 0;
  const equipExpandidos = [];
  state.equipamentos.forEach(e => {
    const wattsUnit = e.unidade === "BTU" ? btuParaWatts(e.potencia) : e.potencia;
    for (let i = 0; i < e.quantidade; i++) {
      equipExpandidos.push({
        tipo: e.tipo, ambiente: e.ambiente,
        potencia: wattsUnit, original: `${e.potencia} ${e.unidade}`,
        nome: TIPOS_EQUIPAMENTO.find(t => t.key === e.tipo)?.nome || e.tipo
      });
      potTUE += wattsUnit;
    }
  });

  const potTotal = potTUG + potIlum + potTUE;
  const potTotalKW = potTotal * fpAneel / 1000;

  // ----- 3) Divisão de circuitos -----
  const circuitos = [];

  // Iluminação geral (corrente ≤ 16 A, senão divide)
  if (potIlum > 0) {
    const i = potIlum / V;
    if (i <= 16) {
      circuitos.push({
        tipo: "iluminacao", nome: "Iluminação geral",
        potencia: potIlum, corrente: i, ambitoCozinha: false,
      });
    } else {
      // Divide em dois
      circuitos.push({
        tipo: "iluminacao", nome: "Iluminação — Setor 1",
        potencia: potIlum/2, corrente: (potIlum/2)/V, ambitoCozinha: false,
      });
      circuitos.push({
        tipo: "iluminacao", nome: "Iluminação — Setor 2",
        potencia: potIlum/2, corrente: (potIlum/2)/V, ambitoCozinha: false,
      });
    }
  }

  // TUG por ambiente — agrupa ambientes "molhados" e "secos"
  const tugCozinha  = tugPorAmbiente.filter(a => a.tipo === "cozinha");
  const tugServico  = tugPorAmbiente.filter(a => a.tipo === "servico");
  const tugSocial   = tugPorAmbiente.filter(a => !["cozinha", "servico"].includes(a.tipo));

  const potTugCozinha = tugCozinha.reduce((s,a) => s + a.tug.potencia, 0);
  const potTugServico = tugServico.reduce((s,a) => s + a.tug.potencia, 0);
  const potTugSocial  = tugSocial.reduce((s,a) => s + a.tug.potencia, 0);

  if (potTugCozinha > 0) {
    circuitos.push({
      tipo: "tug", nome: "TUG — Cozinha",
      potencia: potTugCozinha, corrente: potTugCozinha/V, ambitoCozinha: true,
    });
  }
  if (potTugServico > 0) {
    circuitos.push({
      tipo: "tug", nome: "TUG — Área de serviço",
      potencia: potTugServico, corrente: potTugServico/V, ambitoCozinha: true,
    });
  }
  // TUG social — se > 16 A divide
  if (potTugSocial > 0) {
    const iSocial = potTugSocial / V;
    if (iSocial <= 16) {
      circuitos.push({
        tipo: "tug", nome: "TUG — Social (sala/quartos/banheiros)",
        potencia: potTugSocial, corrente: iSocial, ambitoCozinha: false,
      });
    } else {
      // divide pela metade
      const n = Math.ceil(iSocial / 14); // margem de segurança
      const pPorCircuito = potTugSocial / n;
      for (let k = 1; k <= n; k++) {
        circuitos.push({
          tipo: "tug", nome: `TUG — Social ${k}`,
          potencia: pPorCircuito, corrente: pPorCircuito/V, ambitoCozinha: false,
        });
      }
    }
  }

  // TUE — cada equipamento vira um circuito exclusivo
  equipExpandidos.forEach((e, idx) => {
    circuitos.push({
      tipo: "tue", nome: `TUE — ${e.nome}${e.ambiente ? ` (${e.ambiente})` : ''}`,
      potencia: e.potencia, corrente: e.potencia/V, ambitoCozinha: false,
      original: e.original,
    });
  });

  // ----- 4) Dimensionar disjuntor, bitola, Fc para cada circuito -----
  // Agrupamento: assume que TUG/iluminação ficam num mesmo eletroduto (ramo)
  // e cada TUE em eletroduto exclusivo
  const circuitosNaoTUE = circuitos.filter(c => c.tipo !== "tue").length;
  const fcAgrupado = fatorCorrecao(circuitosNaoTUE);

  circuitos.forEach((c, idx) => {
    const fc = c.tipo === "tue" ? 1.00 : fcAgrupado;
    // Iluminação aceita 1,5 mm²; TUG e TUE exigem mínimo de 2,5 mm² (força)
    const ehForca = c.tipo !== "iluminacao";
    const { bitola, iC, iZ } = escolherBitola(c.corrente, fc, ehForca);
    const disjuntor = disjuntorComercial(c.corrente);
    c.fc = fc;
    c.bitola = bitola;
    c.iC = iC;
    c.iZ = iZ;
    c.disjuntor = disjuntor;
    c.aprovado  = disjuntor <= iZ && c.corrente <= iZ;
    c.id = `C${idx+1}`;
  });

  // ----- 5) Eletrodutos -----
  // Eletroduto principal (TUG + iluminação)
  const cabosTUG = [];
  circuitos.filter(c => c.tipo !== "tue").forEach(c => {
    cabosTUG.push(c.bitola); // fase
    cabosTUG.push(c.bitola); // neutro
    cabosTUG.push(Math.min(c.bitola, 2.5)); // terra (mín. 2,5 mm² em residências)
  });
  const eletrodutoPrincipal = cabosTUG.length > 0 ? dimensionarEletroduto(cabosTUG) : null;

  const eletrodutosTUE = circuitos.filter(c => c.tipo === "tue").map(c => {
    const cabos = [c.bitola, c.bitola, Math.min(c.bitola, 2.5)];
    return { circuito: c, eletroduto: dimensionarEletroduto(cabos) };
  });

  // ----- 6) Demanda -----
  const fdIlumTUG = fatorDemandaIluminacaoTUG(potIlum + potTUG);
  const d1 = (potIlum + potTUG) * fdIlumTUG / 1000; // kVA
  const d2 = potTUE / 1000; // kVA (Fd = 1,00 para uso específico de aquecimento)
  const dTotal = d1 + d2;
  const cargaKW = dTotal * fpAneel;

  // ----- 7) Disjuntor geral -----
  const correnteGeral = (dTotal * 1000) / V;
  const disjuntorGeral = disjuntorComercial(correnteGeral * 1.1); // 10% de margem

  // ----- 8) Reservas (mínimo 2 ou 10% dos circuitos) -----
  const nReservas = Math.max(2, Math.ceil(circuitos.length * 0.10));

  return {
    tugPorAmbiente,
    potTUG, potIlum, potTUE, potTotal, potTotalKW,
    circuitos,
    eletrodutoPrincipal, eletrodutosTUE,
    fdIlumTUG, d1, d2, dTotal, cargaKW,
    correnteGeral, disjuntorGeral, nReservas,
    V, equipExpandidos,
  };
}

/* ============================================================
   RENDERIZAÇÃO DOS RESULTADOS
   ============================================================ */
function calcularEIr(n) {
  if (state.ambientes.length === 0 && state.equipamentos.length === 0) {
    alert("Adicione pelo menos um ambiente ou equipamento antes de calcular.");
    return;
  }
  const r = dimensionar();
  renderResultados(r);
  renderObra(r);
  irPara(n);
}

function fmt(n, casas = 2) {
  if (isNaN(n) || !isFinite(n)) return "—";
  return n.toLocaleString("pt-BR", { minimumFractionDigits: casas, maximumFractionDigits: casas });
}

function renderResultados(r) {
  const c = document.getElementById("resultados-container");

  c.innerHTML = `
    <h3>1) Resumo do dimensionamento</h3>
    <div class="stats">
      <div class="stat"><div class="label">Potência TUG</div><div class="value">${fmt(r.potTUG, 0)}</div><div class="unit">VA</div></div>
      <div class="stat"><div class="label">Iluminação</div><div class="value">${fmt(r.potIlum, 0)}</div><div class="unit">VA</div></div>
      <div class="stat"><div class="label">TUE</div><div class="value">${fmt(r.potTUE, 0)}</div><div class="unit">W</div></div>
      <div class="stat"><div class="label">Potência total</div><div class="value">${fmt(r.potTotal/1000, 2)}</div><div class="unit">kVA</div></div>
      <div class="stat"><div class="label">Carga p/ concessionária</div><div class="value">${fmt(r.potTotalKW, 2)}</div><div class="unit">kW</div></div>
    </div>

    <div class="eq">P<sub>total</sub> = P<sub>TUG</sub> + P<sub>ilum</sub> + P<sub>TUE</sub>
      = ${fmt(r.potTUG,0)} + ${fmt(r.potIlum,0)} + ${fmt(r.potTUE,0)}
      = ${fmt(r.potTotal,0)} VA</div>
    <div class="eq">P<sub>(kW)</sub> = ${fmt(r.potTotal/1000, 2)} × ${state.projeto.aneel}
      = ${fmt(r.potTotalKW, 2)} kW</div>

    ${ r.potTotalKW > 75 ? `
      <div class="box-fail"><b>⚠ Atenção:</b> a carga total ultrapassa 75 kW. Acima desse limite, a NBR exige projeto assinado por engenheiro eletricista. Reveja o projeto.</div>
    ` : `
      <div class="box-ok"><b>✓ Dentro do limite:</b> a carga total é inferior a 75 kVA, podendo ser assinada por engenheiro civil habilitado.</div>
    `}

    <h3>2) Previsão de cargas por ambiente</h3>
    <table>
      <thead>
        <tr><th>Ambiente</th><th>Área (m²)</th><th>Perím. (m)</th><th>Iluminação</th><th>TUG (critério)</th><th>Potência TUG</th></tr>
      </thead>
      <tbody>
        ${r.tugPorAmbiente.map(a => `
          <tr>
            <td class="bold">${TIPOS_AMBIENTE.find(t => t.key===a.tipo)?.nome || a.tipo}</td>
            <td class="num">${fmt(a.area, 1)}</td>
            <td class="num">${fmt(a.perimetro, 1)}</td>
            <td class="num">${a.pontosIlum}×100 W</td>
            <td>${a.tug.criterio}</td>
            <td class="num bold">${a.tug.potencia} VA</td>
          </tr>
        `).join("")}
        ${r.tugPorAmbiente.length === 0 ? '<tr><td colspan="6" style="text-align:center; color:var(--cinza-texto);">Nenhum ambiente cadastrado</td></tr>' : ''}
        <tr><td colspan="5" class="bold">TOTAL</td><td class="num bold">${fmt(r.potTUG, 0)} VA</td></tr>
      </tbody>
    </table>

    <h3>3) Tomadas de uso específico (TUE)</h3>
    ${r.equipExpandidos.length === 0 ? '<div class="box-info">Nenhum equipamento TUE cadastrado.</div>' : `
      <table>
        <thead>
          <tr><th>Equipamento</th><th>Ambiente</th><th>Original</th><th>Potência elétrica</th></tr>
        </thead>
        <tbody>
          ${r.equipExpandidos.map(e => `
            <tr>
              <td class="bold">${e.nome}</td>
              <td>${e.ambiente}</td>
              <td class="num">${e.original}</td>
              <td class="num bold">${fmt(e.potencia, 0)} W</td>
            </tr>
          `).join("")}
          <tr><td colspan="3" class="bold">TOTAL TUE</td><td class="num bold">${fmt(r.potTUE, 0)} W</td></tr>
        </tbody>
      </table>
    `}

    <h3>4) Divisão de circuitos e dimensionamento</h3>
    <p class="desc" style="margin-bottom:8px;">Cada linha mostra um circuito com seu disjuntor comercial, bitola escolhida e a verificação I<sub>proj</sub> ≤ I<sub>n</sub> ≤ I<sub>z</sub>.</p>
    <table>
      <thead>
        <tr>
          <th>Circ.</th><th>Identificação</th><th>Potência</th><th>I<sub>proj</sub></th>
          <th>F<sub>c</sub></th><th>Bitola</th><th>I<sub>z</sub></th><th>Disj.</th><th>OK?</th>
        </tr>
      </thead>
      <tbody>
        ${r.circuitos.map(c => `
          <tr>
            <td class="bold center">${c.id}</td>
            <td>${c.nome}</td>
            <td class="num">${fmt(c.potencia, 0)} ${c.tipo === "tue" ? "W" : "VA"}</td>
            <td class="num">${fmt(c.corrente, 1)} A</td>
            <td class="num">${fmt(c.fc, 2)}</td>
            <td class="num">${c.bitola} mm²</td>
            <td class="num">${fmt(c.iZ, 1)} A</td>
            <td class="num">${c.disjuntor} A</td>
            <td class="center" style="color: ${c.aprovado ? 'var(--verde)' : 'var(--vermelho)'};">${c.aprovado ? '✓' : '✗'}</td>
          </tr>
        `).join("")}
      </tbody>
    </table>

    <h3>5) Eletrodutos</h3>
    ${r.eletrodutoPrincipal ? `
      <div class="box-info">
        <b>Eletroduto principal</b> (TUG + Iluminação — ${r.circuitos.filter(c=>c.tipo!=='tue').length} circuitos)<br>
        Bitola comercial: <b>${r.eletrodutoPrincipal.nominal}</b> &nbsp;|&nbsp;
        Ocupação: ${r.eletrodutoPrincipal.ocupacao}% &nbsp;|&nbsp;
        Fator de correção aplicado: ${fmt(r.circuitos.find(c=>c.tipo!=='tue')?.fc || 1, 2)}
      </div>
    ` : ''}
    ${r.eletrodutosTUE.length > 0 ? `
      <table>
        <thead>
          <tr><th>Circuito TUE</th><th>Bitola do cabo</th><th>Eletroduto exclusivo</th><th>Ocupação</th></tr>
        </thead>
        <tbody>
          ${r.eletrodutosTUE.map(et => `
            <tr>
              <td>${et.circuito.nome}</td>
              <td class="num">${et.circuito.bitola} mm²</td>
              <td class="bold">${et.eletroduto.nominal}</td>
              <td class="num">${et.eletroduto.ocupacao}%</td>
            </tr>
          `).join("")}
        </tbody>
      </table>
    ` : ''}

    <h3>6) Cálculo de demanda</h3>
    <div class="eq">D<sub>1</sub> = (P<sub>ilum</sub> + P<sub>TUG</sub>) × F<sub>d</sub>
      = ${fmt(r.potIlum + r.potTUG, 0)} × ${fmt(r.fdIlumTUG, 2)}
      = ${fmt(r.d1, 2)} kVA</div>
    <div class="eq">D<sub>2</sub> = P<sub>TUE</sub> × 1,00 = ${fmt(r.d2, 2)} kVA</div>
    <div class="eq">D<sub>total</sub> = D<sub>1</sub> + D<sub>2</sub> = ${fmt(r.dTotal, 2)} kVA</div>
    <div class="eq">Carga para concessionária: ${fmt(r.cargaKW, 2)} kW
      &nbsp;&nbsp; (I<sub>geral</sub> = ${fmt(r.correnteGeral, 1)} A)</div>

    <h3>7) Quadro de Distribuição (QD) — diagrama unifilar</h3>
    <div id="diagrama-container">
      ${gerarDiagramaUnifilar(r)}
    </div>
  `;
}

/* ============================================================
   DIAGRAMA UNIFILAR — gerado em SVG
   ============================================================ */
function gerarDiagramaUnifilar(r) {
  const circs = r.circuitos;
  const reservas = r.nReservas;
  const nCircs = circs.length + reservas;

  // dimensões
  const W = 720;
  const H = 240 + nCircs * 50;
  const xBus = 80;
  const xDevices = 130;
  const xBox = 230;
  const yEntrada = 30;

  let svg = `<svg viewBox="0 0 ${W} ${H}" xmlns="http://www.w3.org/2000/svg">`;

  // Estilos
  svg += `<style>
    .lbl-bold { font: bold 13px sans-serif; fill: #1a3a5c; }
    .lbl      { font: 12px sans-serif; fill: #1e293b; }
    .lbl-sm   { font: 10.5px sans-serif; fill: #6b7280; }
    .wire     { stroke: #1f2937; stroke-width: 2.2; fill: none; }
    .box      { stroke-width: 1.4; fill: #f8fafc; }
    .disj     { stroke-width: 1.4; fill: white; }
  </style>`;

  // Entrada
  svg += `<text x="${xBus}" y="${yEntrada-6}" class="lbl-bold">ENTRADA (Padrão)</text>`;
  svg += `<line x1="${xBus}" y1="${yEntrada}" x2="${xBus}" y2="${yEntrada+20}" class="wire"/>`;

  // DPS
  svg += dispositivoSVG(xBus, yEntrada + 30, "DPS", "175 V — 8 kA", "#2563a8");
  // DR
  svg += `<line x1="${xBus}" y1="${yEntrada+62}" x2="${xBus}" y2="${yEntrada+78}" class="wire"/>`;
  svg += dispositivoSVG(xBus, yEntrada + 80, "DR", "30 mA — geral", "#2563a8");
  // Disjuntor Geral
  svg += `<line x1="${xBus}" y1="${yEntrada+112}" x2="${xBus}" y2="${yEntrada+128}" class="wire"/>`;
  svg += dispositivoSVG(xBus, yEntrada + 130, "Disj. Geral", `${r.disjuntorGeral} A`, "#1a3a5c");

  // Barramento vertical (busbar)
  const yBusStart = yEntrada + 165;
  const yBusEnd = yBusStart + nCircs * 50 + 20;
  svg += `<line x1="${xBus}" y1="${yBusStart}" x2="${xBus}" y2="${yBusEnd}" stroke="#1f2937" stroke-width="3.5"/>`;
  svg += `<text x="${xBus-22}" y="${(yBusStart+yBusEnd)/2}" class="lbl-bold" transform="rotate(-90, ${xBus-22}, ${(yBusStart+yBusEnd)/2})" text-anchor="middle">QD</text>`;

  // Circuitos
  const todos = [
    ...circs,
    ...Array.from({length: reservas}, (_, i) => ({
      id: `R${i+1}`, nome: "Reserva", potencia: 0, corrente: 0,
      bitola: 2.5, disjuntor: 10, tipo: "reserva", aprovado: true
    }))
  ];

  todos.forEach((c, i) => {
    const y = yBusStart + 20 + i * 50;
    const cor = c.tipo === "tue" ? "#d97706" : c.tipo === "reserva" ? "#9ca3af" : "#2563a8";

    // linha do barramento ao disjuntor
    svg += `<line x1="${xBus}" y1="${y}" x2="${xDevices}" y2="${y}" class="wire"/>`;
    // disjuntor (retângulo pequeno)
    svg += `<rect x="${xDevices}" y="${y-12}" width="40" height="24" rx="3" class="disj" stroke="${cor}"/>`;
    svg += `<text x="${xDevices+20}" y="${y-16}" class="lbl-sm" text-anchor="middle" fill="${cor}" style="font-weight:600;">${c.disjuntor} A</text>`;
    // linha disjuntor ao box
    svg += `<line x1="${xDevices+40}" y1="${y}" x2="${xBox}" y2="${y}" class="wire"/>`;
    // box do circuito
    svg += `<rect x="${xBox}" y="${y-18}" width="${W - xBox - 30}" height="36" rx="5" class="box" stroke="${cor}"/>`;
    svg += `<text x="${xBox + 12}" y="${y - 2}" class="lbl-bold">${c.id} — ${escape_(c.nome)}</text>`;
    const direita = `${c.bitola} mm²   ${c.potencia > 0 ? fmt(c.potencia, 0) + (c.tipo==='tue'?' W':' VA') : '—'}`;
    svg += `<text x="${W - 40}" y="${y + 12}" class="lbl-sm" text-anchor="end">${direita}</text>`;
    if (!c.aprovado) {
      svg += `<text x="${xBox + 12}" y="${y + 12}" class="lbl-sm" fill="#b91c1c">✗ verificar bitola/disjuntor</text>`;
    }
  });

  // Título
  svg += `<text x="${W/2}" y="${H - 12}" class="lbl-sm" text-anchor="middle">Diagrama Unifilar — ${escape_(state.projeto.nome || "Projeto residencial")}</text>`;

  svg += `</svg>`;
  return svg;
}

function dispositivoSVG(x, y, label, sub, cor) {
  const w = 100, h = 32;
  return `
    <rect x="${x - w/2}" y="${y}" width="${w}" height="${h}" rx="5" class="box" stroke="${cor}"/>
    <text x="${x}" y="${y + 14}" class="lbl-bold" text-anchor="middle">${label}</text>
    <text x="${x}" y="${y + 27}" class="lbl-sm" text-anchor="middle">${sub}</text>
  `;
}

function escape_(s) {
  if (s == null) return "";
  return String(s).replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;").replace(/"/g,"&quot;");
}

/* ============================================================
   ETAPA 5: GUIA DE OBRA
   ============================================================ */
function renderObra(r) {
  const c = document.getElementById("obra-container");
  const totalCabo15 = r.circuitos.filter(c => c.bitola === 1.5).length;
  const totalCabo25 = r.circuitos.filter(c => c.bitola === 2.5).length;
  const totalCabo4  = r.circuitos.filter(c => c.bitola === 4).length;
  const totalCabo6  = r.circuitos.filter(c => c.bitola === 6).length;
  const totalCabo10 = r.circuitos.filter(c => c.bitola === 10).length;

  c.innerHTML = `
    <h3>📋 Checklist do quadro de distribuição</h3>
    <div class="box-info">
      <b>Componentes obrigatórios no QD:</b>
      <div style="margin-top:6px;">☐ DPS classe II (175 V, 8 kA mínimo) — protege contra surtos</div>
      <div>☐ DR geral de 30 mA — proteção contra choque elétrico</div>
      <div>☐ Disjuntor geral de ${r.disjuntorGeral} A — corte total da unidade</div>
      <div>☐ ${r.circuitos.length} disjuntores de circuito + ${r.nReservas} reservas (total ${r.circuitos.length + r.nReservas} módulos)</div>
      <div>☐ Barramento de neutro (cor azul-claro) e barramento de terra (verde) separados</div>
      <div>☐ Identificação de cada circuito (etiqueta com sigla + ambiente + corrente)</div>
    </div>

    <h3>🔌 Lista de materiais (cabos e disjuntores)</h3>
    <table>
      <thead>
        <tr><th>Item</th><th>Especificação</th><th>Quantidade estimada</th></tr>
      </thead>
      <tbody>
        ${totalCabo15 > 0 ? `<tr><td>Cabo flexível 1,5 mm²</td><td>Iluminação — cores: fase preto/vermelho, neutro azul, terra verde</td><td class="num">${totalCabo15} circuito(s)</td></tr>` : ''}
        ${totalCabo25 > 0 ? `<tr><td>Cabo flexível 2,5 mm²</td><td>TUG — cores idem</td><td class="num">${totalCabo25} circuito(s)</td></tr>` : ''}
        ${totalCabo4 > 0  ? `<tr><td>Cabo flexível 4 mm²</td><td>TUE de média potência</td><td class="num">${totalCabo4} circuito(s)</td></tr>` : ''}
        ${totalCabo6 > 0  ? `<tr><td>Cabo flexível 6 mm²</td><td>TUE de alta potência</td><td class="num">${totalCabo6} circuito(s)</td></tr>` : ''}
        ${totalCabo10 > 0 ? `<tr><td>Cabo flexível 10 mm²</td><td>TUE de muito alta potência / alimentador</td><td class="num">${totalCabo10} circuito(s)</td></tr>` : ''}
        <tr><td>Disjuntores</td><td>${disjuntoresLista(r)}</td><td class="num">${r.circuitos.length + 1 + r.nReservas} unidade(s)</td></tr>
        <tr><td>DR</td><td>30 mA, In = ${r.disjuntorGeral} A</td><td class="num">1 unidade</td></tr>
        <tr><td>DPS</td><td>Classe II, 175 V, 8 kA</td><td class="num">1 unidade (ou 1 por fase)</td></tr>
      </tbody>
    </table>

    <h3>📏 Eletrodutos a comprar</h3>
    <div class="box-info">
      ${r.eletrodutoPrincipal ? `<div style="margin-bottom:6px;">• <b>Eletroduto principal</b> (TUG/Iluminação): <b>${r.eletrodutoPrincipal.nominal}</b></div>` : ''}
      ${r.eletrodutosTUE.map(et => `<div style="margin-bottom:4px;">• Eletroduto exclusivo para <b>${et.circuito.nome}</b>: <b>${et.eletroduto.nominal}</b></div>`).join('')}
    </div>

    <h3>🎨 Padronização de cores (obrigatório — NBR 5410)</h3>
    <table>
      <thead><tr><th>Condutor</th><th>Cor obrigatória</th><th>Observação</th></tr></thead>
      <tbody>
        <tr><td class="bold">Neutro (N)</td><td style="background:#bfdbfe;">Azul claro</td><td>Nunca usar para fase</td></tr>
        <tr><td class="bold">Terra / Proteção (PE)</td><td style="background:#bbf7d0;">Verde ou verde-amarelo</td><td>Nunca usar para outra função</td></tr>
        <tr><td class="bold">Fase (L1, L2, L3)</td><td>Qualquer cor (preto, vermelho, marrom, cinza...)</td><td>Exceto azul claro, verde e verde-amarelo</td></tr>
        <tr><td class="bold">Retorno (após interruptor)</td><td>Preferencialmente amarelo ou branco</td><td>Distingue dos demais</td></tr>
      </tbody>
    </table>

    <h3>⚠️ Cuidados durante a execução</h3>
    <div class="box-warn">
      <b>Antes de energizar:</b>
      <div style="margin-top:6px;">☐ Conferir aperto de todos os parafusos do QD (torque do fabricante)</div>
      <div>☐ Medir continuidade do condutor terra em todas as tomadas</div>
      <div>☐ Testar o DR pressionando o botão TEST após energização</div>
      <div>☐ Conferir polaridade fase/neutro em todas as tomadas com testador</div>
      <div>☐ Verificar que <u>nenhum</u> chuveiro ou torneira elétrica tem tomada — devem ser ligados diretamente à fiação</div>
    </div>

    <h3>🏗️ Pontos de fiscalização do engenheiro responsável</h3>
    <div class="box-warn">
      <b>Conferências críticas em vistoria:</b>
      <div style="margin-top:6px;">1. Disjuntor compatível com cabo: I<sub>proj</sub> ≤ I<sub>n</sub> ≤ I<sub>z</sub> — armadilha clássica é colocar disjuntor "com margem" maior que a capacidade do cabo</div>
      <div>2. Eletrodutos não excedendo a taxa máxima de ocupação (40% para 3+ condutores)</div>
      <div>3. Distância mínima de 60 cm entre tomadas e box do banheiro</div>
      <div>4. Altura padrão das tomadas: baixas a 30 cm, médias a 1,20 m, altas a 1,80 m do piso</div>
      <div>5. TUE de aquecimento de água SEM plugue — ligação direta na fiação</div>
      <div>6. Disjuntor geral instalado dentro do QD e acessível</div>
    </div>

    <h3>📐 Resumo executivo para a ART</h3>
    <table>
      <tbody>
        <tr><td class="bold">Projeto</td><td>${state.projeto.nome || "—"}</td></tr>
        <tr><td class="bold">Local</td><td>${state.projeto.cidade || "—"}</td></tr>
        <tr><td class="bold">Tensão de alimentação</td><td>${state.projeto.tensao} V</td></tr>
        <tr><td class="bold">Carga instalada</td><td>${fmt(r.potTotal/1000, 2)} kVA / ${fmt(r.potTotalKW, 2)} kW</td></tr>
        <tr><td class="bold">Demanda calculada</td><td>${fmt(r.dTotal, 2)} kVA / ${fmt(r.cargaKW, 2)} kW</td></tr>
        <tr><td class="bold">Corrente do disjuntor geral</td><td>${r.disjuntorGeral} A</td></tr>
        <tr><td class="bold">Nº de circuitos</td><td>${r.circuitos.length} (+ ${r.nReservas} reservas)</td></tr>
        <tr><td class="bold">Responsabilidade técnica</td><td>${r.potTotalKW <= 75 ? "Eng. civil habilitado (carga ≤ 75 kVA)" : "Eng. eletricista obrigatório (carga > 75 kVA)"}</td></tr>
        <tr><td class="bold">Norma de referência</td><td>NBR 5410:2004</td></tr>
      </tbody>
    </table>
  `;
}

function disjuntoresLista(r) {
  const c = {};
  r.circuitos.forEach(x => c[x.disjuntor] = (c[x.disjuntor]||0) + 1);
  c[r.disjuntorGeral] = (c[r.disjuntorGeral]||0) + 1;
  return Object.entries(c).sort((a,b)=>parseInt(a[0])-parseInt(b[0])).map(([d,n]) => `${n}× ${d} A`).join(", ");
}

/* ============================================================
   INICIALIZAÇÃO
   ============================================================ */
window.addEventListener("DOMContentLoaded", () => {
  carregar();
  renderAmbientes();
  renderEquipamentos();
  // auto-salva ao mudar inputs do projeto
  ["proj-nome", "proj-cidade", "proj-tensao", "proj-aneel"].forEach(id => {
    document.getElementById(id).addEventListener("change", salvar);
  });
});
</script>
</body>
</html># El-trica
