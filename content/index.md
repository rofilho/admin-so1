---
title: "🖥️ Admin. Sistemas Operacionais I"
cssclasses:
  - dashboard-layout
---

<style>
/* ── Oculta elementos padrão do Quartz nesta página ── */
.sidebar, .page-header, .article-title, .content-meta, footer,
.lesson-nav { display: none !important; }

/* ── Reset total de largura ── */
html, body, #quartz-root, #quartz-body, .page, .center, .center-content, article {
  display: block !important;
  max-width: 100% !important;
  width: 100% !important;
  margin: 0 !important;
  padding: 0 !important;
  overflow-x: hidden;
}

/* ── Container principal ── */
.dashboard-container {
  font-family: 'Outfit', sans-serif;
  background-color: #0f172a;
  color: #f1f5f9;
  padding: 2rem 5%;
  min-height: 100vh;
  box-sizing: border-box;
  width: 100%;
}
:root[saved-theme="light"] .dashboard-container {
  background-color: #f8fafc;
  color: #1e293b;
}

/* ── Hero banner ── */
.hero {
  position: relative;
  height: 350px;
  display: flex;
  align-items: center;
  padding: 2rem 3rem;
  background-image: linear-gradient(135deg, #0f172a 0%, #3b82f6 150%);
  background-size: cover;
  background-position: center;
  border-radius: 16px;
  margin-bottom: 3rem;
  color: white;
  box-shadow: 0 10px 30px rgba(0,0,0,0.3);
  overflow: hidden;
}
.hero::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  background: url('https://uniube.br/assets/images/logo.png') no-repeat right 30px center;
  background-size: contain;
  opacity: 0.1;
}
.tag {
  display: inline-block;
  padding: 4px 12px;
  background: rgba(255,255,255,0.1);
  border: 1px solid rgba(255,255,255,0.2);
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
  color: white;
  margin-bottom: 12px;
}
.hero h1 { font-size: 38px; margin: 0 0 10px; line-height: 1.1; color: white !important; -webkit-text-fill-color: white !important; }
.hero p  { font-size: 15px; color: #cbd5e1; max-width: 500px; margin-bottom: 20px; }
.btn {
  padding: 10px 24px;
  font-size: 15px;
  font-weight: 600;
  border-radius: 8px;
  border: none;
  cursor: pointer;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  transition: all 0.2s ease;
  background: white;
  color: #0f172a;
}
.btn:hover { background: #f1f5f9; transform: scale(1.05); }

/* ── Linhas de cards (módulos) ── */
.row-wrapper { margin-bottom: 2.5rem; }
.row-title   { font-size: 20px; font-weight: 700; margin-bottom: 15px; margin-left: 5px; opacity: 0.9; }
.row {
  display: flex;
  gap: 16px;
  padding: 10px 5px;
  overflow-x: auto;
  scroll-behavior: smooth;
  -ms-overflow-style: none;
  scrollbar-width: none;
}
.row::-webkit-scrollbar { display: none; }

/* ── Cards de aulas ── */
.card {
  flex: 0 0 calc(100% / 4 - 15px);
  min-width: 220px;
  background: #1e293b;
  border-radius: 12px;
  overflow: hidden;
  cursor: pointer;
  text-decoration: none;
  display: flex;
  flex-direction: column;
  border: 1px solid rgba(255,255,255,0.05);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
:root[saved-theme="light"] .card { background: #f1f5f9; border-color: #e2e8f0; }
.card:hover { transform: translateY(-5px) scale(1.02); box-shadow: 0 15px 30px rgba(0,0,0,0.15); border-color: #3b82f6; }

.card-thumb {
  height: 120px;
  background: #0f172a;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 40px;
  position: relative;
  overflow: hidden;
}
:root[saved-theme="light"] .card-thumb { background: #e2e8f0; }
.card-thumb::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, #3b82f640 0%, transparent 100%);
}
.card-progress-bar { position: absolute; bottom: 0; left: 0; height: 4px; background: rgba(0,0,0,0.2); width: 100%; }
.card-progress      { height: 100%; background: #3b82f6; }

.card-content { padding: 15px; flex: 1; display: flex; flex-direction: column; }
.card-title {
  font-size: 14px;
  font-weight: 600;
  margin: 0 0 8px;
  line-height: 1.3;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  color: #f1f5f9;
}
:root[saved-theme="light"] .card-title { color: #1e293b; }
.card-meta { font-size: 11px; color: #64748b; margin-top: auto; display: flex; justify-content: space-between; }

/* ── Responsivo ── */
@media (max-width: 768px) {
  .card { flex: 0 0 calc(50% - 15px); }
  .hero { padding: 1.5rem; height: 300px; }
  .hero h1 { font-size: 28px; }
}
@media (max-width: 480px) { .card { flex: 0 0 85%; } }
</style>

<div class="dashboard-container">

<div class="hero">
  <div style="position: relative; z-index: 10;">
    <span class="tag">▶ Continue Estudando</span>
    <h1>🖥️ Admin. Sistemas Operacionais I</h1>
    <p>Aprenda a administrar Linux, Virtualização e Serviços de Rede.</p>
    <a href="./Aula-04---Virtualizacao-e-Hyper-V-com-Ubuntu-Server" class="btn" data-spa>Retomar: 🖥️ 🟢 Aula 04: Virtualização com Hyper-V e Instalação do Ubuntu Server</a>
  </div>
</div>

<div class="row-wrapper">
  <div class="row-title">Conteúdo da Disciplina</div>
  <div class="row">
    <a href="./Aula-01---Introducao-e-Ementa" class="card" data-spa>
      <div class="card-thumb">🖥️<div class="card-progress-bar"><div class="card-progress" style="width: 71%"></div></div></div>
      <div class="card-content">
        <div class="card-title">🖥️ Aula  – Administração de Sistemas Operacionais em Redes I – Plano de Aulas</div>
        <div class="card-meta"><span>Acessar Aula</span><span></span></div>
      </div>
    </a>
    <a href="./Aula-02---Historia-do-Unix-e-Software-Livre" class="card" data-spa>
      <div class="card-thumb">🖥️<div class="card-progress-bar"><div class="card-progress" style="width: 63%"></div></div></div>
      <div class="card-content">
        <div class="card-title">🖥️ Aula  – História do Unix, do Software Livre, Open Source e Introdução ao Linux</div>
        <div class="card-meta"><span>Acessar Aula</span><span></span></div>
      </div>
    </a>
    <a href="./Aula-03---Ambiente-de-Trabalho-no-Linux" class="card" data-spa>
      <div class="card-thumb">🖥️<div class="card-progress-bar"><div class="card-progress" style="width: 79%"></div></div></div>
      <div class="card-content">
        <div class="card-title">🖥️ Aula  – Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões</div>
        <div class="card-meta"><span>Acessar Aula</span><span></span></div>
      </div>
    </a>
    <a href="./Aula-04---Virtualizacao-e-Hyper-V-com-Ubuntu-Server" class="card" data-spa>
      <div class="card-thumb">🖥️<div class="card-progress-bar"><div class="card-progress" style="width: 77%"></div></div></div>
      <div class="card-content">
        <div class="card-title">🖥️ 🟢 Aula 04: Virtualização com Hyper-V e Instalação do Ubuntu Server</div>
        <div class="card-meta"><span>Acessar Aula</span><span></span></div>
      </div>
    </a>

  </div>
</div>

</div>
