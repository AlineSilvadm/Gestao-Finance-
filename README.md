<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gestão Financeira - Neumorphism</title>
    <script src="https://unpkg.com/lucide@latest"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --bg-color: #E9EDFO;
            --card-bg: #E9EDFO;
            --shadow-light: #FFFFFF;
            --shadow-dark: #D3DBE6;
            --text-main: #2C3E50;
            --text-secondary: #7F8C8D;
            --accent-coral: #FF7272;
            --accent-blue: #489CC1;
            --accent-green: #21A87D;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            display: flex;
            height: 100vh;
            overflow: hidden;
        }

        /* Neumorphic Shadows */
        .neu-box {
            background: var(--card-bg);
            box-shadow: 10px 10px 20px var(--shadow-dark), -10px -10px 20px var(--shadow-light);
            border-radius: 20px;
        }

        .neu-box-inset {
            background: var(--card-bg);
            box-shadow: inset 5px 5px 10px var(--shadow-dark), inset -5px -5px 10px var(--shadow-light);
            border-radius: 14px;
        }

        .neu-btn {
            background: var(--card-bg);
            box-shadow: 5px 5px 10px var(--shadow-dark), -5px -5px 10px var(--shadow-light);
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.2s ease;
            color: var(--text-main);
            font-weight: 600;
        }

        .neu-btn:active, .neu-btn.active {
            box-shadow: inset 3px 3px 6px var(--shadow-dark), inset -3px -3px 6px var(--shadow-light);
            color: var(--accent-green);
        }

        /* SIDEBAR */
        aside {
            width: 270px;
            background-color: var(--bg-color);
            border-right: 1px solid rgba(255,255,255,0.4);
            display: flex;
            flex-direction: column;
            padding: 24px 16px;
            gap: 20px;
            overflow-y: auto;
        }

        .brand {
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 18px;
            font-weight: 700;
            color: var(--text-main);
            padding-left: 8px;
        }

        .brand-icon {
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 12px;
            color: var(--accent-blue);
        }

        .nav-list {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .nav-item button {
            width: 100%;
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 12px 14px;
            border-radius: 14px;
            color: var(--text-secondary);
            background: transparent;
            border: none;
            text-align: left;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }

        .nav-item button.active, .nav-item button:hover {
            color: var(--text-main);
            box-shadow: inset 4px 4px 8px var(--shadow-dark), inset -4px -4px 8px var(--shadow-light);
            background: var(--bg-color);
        }

        /* MAIN CONTENT */
        main {
            flex: 1;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        header {
            padding: 20px 32px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        header h1 {
            font-size: 22px;
            font-weight: 700;
        }

        .header-right {
            display: flex;
            align-items: center;
            gap: 16px;
        }

        .icon-neu {
            width: 44px;
            height: 44px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            background: var(--bg-color);
            box-shadow: 6px 6px 12px var(--shadow-dark), -6px -6px 12px var(--shadow-light);
            cursor: pointer;
            color: var(--text-secondary);
        }

        .user-pill {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 6px 16px 6px 6px;
            border-radius: 30px;
            box-shadow: 6px 6px 12px var(--shadow-dark), -6px -6px 12px var(--shadow-light);
        }

        .user-pill img {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            object-fit: cover;
        }

        /* VIEWS CONTAINER */
        .view-container {
            flex: 1;
            padding: 20px 32px 32px 32px;
            overflow-y: auto;
            display: none;
        }

        .view-container.active {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 24px;
        }

        .view-container.full-width {
            display: block;
        }

        .dashboard-col {
            display: flex;
            flex-direction: column;
            gap: 24px;
        }

        /* METRICS TOP ROW */
        .metrics-row {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 16px;
        }

        .metric-card {
            padding: 16px;
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .metric-title {
            font-size: 12px;
            color: var(--text-secondary);
            font-weight: 600;
        }

        .metric-val {
            font-size: 18px;
            font-weight: 700;
            color: var(--text-main);
        }

        /* SECTION TABLES / BLOCKS */
        .section-card {
            padding: 24px;
            margin-bottom: 20px;
        }

        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 16px;
        }

        .section-title {
            font-size: 16px;
            font-weight: 700;
        }

        /* TABLES */
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 13px;
        }

        th {
            text-align: left;
            color: var(--text-secondary);
            padding: 10px;
            border-bottom: 1px solid var(--shadow-dark);
            font-weight: 600;
        }

        td {
            padding: 12px 10px;
            border-bottom: 1px solid rgba(211, 219, 230, 0.4);
        }

        .badge {
            padding: 4px 10px;
            border-radius: 8px;
            font-size: 11px;
            font-weight: 600;
        }
        .badge-success { background: rgba(33, 168, 125, 0.15); color: var(--accent-green); }
        .badge-pending { background: rgba(255, 114, 114, 0.15); color: var(--accent-coral); }

        /* PROGRESS CARDS */
        .progress-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 14px;
        }

        .progress-box {
            padding: 14px;
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .prog-bar-bg {
            height: 6px;
            background: var(--bg-color);
            box-shadow: inset 2px 2px 4px var(--shadow-dark), inset -2px -2px 4px var(--shadow-light);
            border-radius: 4px;
            overflow: hidden;
            margin-top: 4px;
        }

        .prog-bar-fill {
            height: 100%;
            background: var(--accent-green);
            border-radius: 4px;
        }

        /* BANCO CARDS */
        .bank-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
        }

        .bank-card {
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .bank-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* MODAL */
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(44, 60, 80, 0.4);
            backdrop-filter: blur(4px);
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }

        .modal-content {
            background: var(--bg-color);
            width: 440px;
            padding: 30px;
            border-radius: 24px;
            box-shadow: 15px 15px 30px var(--shadow-dark), -15px -15px 30px var(--shadow-light);
        }

        .form-group {
            margin-bottom: 16px;
        }

        .form-group label {
            display: block;
            font-size: 12px;
            font-weight: 600;
            margin-bottom: 6px;
            color: var(--text-secondary);
        }

        .form-group input, .form-group select {
            width: 100%;
            padding: 12px;
            border: none;
            outline: none;
            background: var(--bg-color);
            box-shadow: inset 4px 4px 8px var(--shadow-dark), inset -4px -4px 8px var(--shadow-light);
            border-radius: 12px;
            color: var(--text-main);
            font-size: 14px;
        }
    </style>
</head>
<body>

    <!-- SIDEBAR -->
    <aside class="neu-box" style="box-shadow: none; border-radius:0; border-right: 2px solid var(--shadow-dark);">
        <div class="brand">
            <div class="neu-box icon-neu"><i data-lucide="pie-chart"></i></div>
            Gestão Financeira
        </div>
        <ul class="nav-list">
            <li class="nav-item"><button class="active" onclick="switchTab('dashboard', this)"><i data-lucide="layout-dashboard"></i> Dashboard</button></li>
            <li class="nav-item"><button onclick="switchTab('resumo', this)"><i data-lucide="bar-chart-3"></i> Resumo do Mês</button></li>
            <li class="nav-item"><button onclick="switchTab('orcamentos', this)"><i data-lucide="sliders"></i> Orçamentos</button></li>
            <li class="nav-item"><button onclick="switchTab('contas', this)"><i data-lucide="landmark"></i> Contas Bancárias</button></li>
            <li class="nav-item"><button onclick="switchTab('ganhos', this)"><i data-lucide="arrow-down-circle"></i> Ganhos</button></li>
            <li class="nav-item"><button onclick="switchTab('gastos', this)"><i data-lucide="arrow-up-circle"></i> Gastos</button></li>
            <li class="nav-item"><button onclick="switchTab('transferencias', this)"><i data-lucide="repeat"></i> Transferências</button></li>
            <li class="nav-item"><button onclick="switchTab('faturas', this)"><i data-lucide="credit-card"></i> Faturas / Cartões</button></li>
        </ul>

        <div class="neu-box" style="padding: 16px; text-align: center; margin-top: auto;">
            <p style="font-size: 11px; color: var(--text-secondary); font-style: italic; margin-bottom: 8px;">“Disciplina hoje, liberdade amanhã!”</p>
            <button class="neu-btn" style="width: 100%; padding: 10px; font-size: 12px; color: var(--accent-green);" onclick="openModal()">+ Nova Transação</button>
        </div>
    </aside>

    <!-- MAIN -->
    <main>
        <header>
            <h1 id="page-title">Dashboard - Novembro / 2026</h1>
            <div class="header-right">
                <button class="icon-neu" onclick="openModal()" title="Adicionar Transação"><i data-lucide="plus"></i></button>
                <div class="user-pill neu-box">
                    <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=100" alt="User">
                    <span style="font-size: 14px; font-weight: 600;">Aline Silva</span>
                </div>
            </div>
        </header>

        <!-- TAB 1: DASHBOARD -->
        <div id="tab-dashboard" class="view-container active">
            <div class="dashboard-col">
                <div class="metrics-row">
                    <div class="neu-box metric-card">
                        <span class="metric-title">Total Entradas</span>
                        <span class="metric-val" style="color: var(--accent-green);">R$ 1.250,09</span>
                    </div>
                    <div class="neu-box metric-card">
                        <span class="metric-title">Despesas Fixas</span>
                        <span class="metric-val" style="color: var(--accent-coral);">R$ 71,90</span>
                    </div>
                    <div class="neu-box metric-card">
                        <span class="metric-title">Despesas Variáveis</span>
                        <span class="metric-val" style="color: var(--accent-coral);">R$ 548,77</span>
                    </div>
                    <div class="neu-box metric-card">
                        <span class="metric-title">Sobra do Mês</span>
                        <span class="metric-val" style="color: var(--accent-blue);">R$ 629,42</span>
                    </div>
                </div>

                <div class="neu-box section-card">
                    <div class="section-header">
                        <span class="section-title">Contas a Pagar & Recorrentes</span>
                        <span style="font-size: 12px; color: var(--text-secondary);">Novembro</span>
                    </div>
                    <table>
                        <thead>
                            <tr>
                                <th>Descrição</th>
                                <th>Categoria</th>
                                <th>Valor</th>
                                <th>Status</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>Empréstimo Banco BV</td>
                                <td>Dívidas / Parcelamentos</td>
                                <td>R$ 503,09</td>
                                <td><span class="badge badge-success">Pago</span></td>
                            </tr>
                            <tr>
                                <td>Empréstimo Nubank (Parcela)</td>
                                <td>Dívidas / Parcelamentos</td>
                                <td>R$ 239,13</td>
                                <td><span class="badge badge-pending">Pendente</span></td>
                            </tr>
                            <tr>
                                <td>Pets (Alimentação e Cuidados)</td>
                                <td>Outros Controles</td>
                                <td>R$ 135,00</td>
                                <td><span class="badge badge-pending">Pendente</span></td>
                            </tr>
                        </tbody>
                    </table>
                </div>

                <div class="neu-box section-card">
                    <div class="section-header">
                        <span class="section-title">Últimas Movimentações</span>
                    </div>
                    <table>
                        <thead>
                            <tr><th>Descrição</th><th>Categoria</th><th>Data</th><th>Valor</th><th>Status</th></tr>
                        </thead>
                        <tbody id="dash-recent-tbody">
                            <tr><td>Salário / Pix Recebido</td><td>Entradas</td><td>02/11/2026</td><td style="color:var(--accent-green);font-weight:600;">+ R$ 1.197,09</td><td><span class="badge badge-success">Concluído</span></td></tr>
                            <tr><td>Constru Silva (Material)</td><td>Despesas Variáveis</td><td>08/11/2026</td><td style="color:var(--accent-coral);font-weight:600;">- R$ 50,00</td><td><span class="badge badge-success">Concluído</span></td></tr>
                            <tr><td>Drogarias Ultrapopula</td><td>Despesas Variáveis</td><td>07/11/2026</td><td style="color:var(--accent-coral);font-weight:600;">- R$ 137,27</td><td><span class="badge badge-success">Concluído</span></td></tr>
                            <tr><td>Netflix.Com</td><td>Despesas Fixas</td><td>08/11/2026</td><td style="color:var(--accent-coral);font-weight:600;">- R$ 71,90</td><td><span class="badge badge-success">Concluído</span></td></tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <div class="dashboard-col">
                <div class="neu-box section-card">
                    <div class="section-header">
                        <span class="section-title">Bancos Vinculados</span>
                        <button class="neu-btn" style="padding: 4px 10px; font-size:11px;" onclick="syncBanks()">Atualizar</button>
                    </div>
                    <div style="display:flex; flex-direction:column; gap:12px;">
                        <div class="neu-box-inset" style="padding:12px; display:flex; justify-content:space-between; align-items:center;">
                            <div>
                                <div style="font-weight:700; font-size:13px;">Nubank (Conta Principal)</div>
                                <div style="font-size:11px; color:var(--text-secondary);">Ag 0001 • CC 89286769-3</div>
                            </div>
                            <div style="font-weight:700; color:var(--accent-green);">R$ 0,01</div>
                        </div>
                        <div class="neu-box-inset" style="padding:12px; display:flex; justify-content:space-between; align-items:center;">
                            <div>
                                <div style="font-weight:700; font-size:13px;">Banco do Brasil</div>
                                <div style="font-size:11px; color:var(--text-secondary);">Ag 230 • CC 51003789-7</div>
                            </div>
                            <div style="font-weight:700; color:var(--accent-green);">R$ 200,00</div>
                        </div>
                    </div>
                </div>

                <div class="neu-box section-card">
                    <div class="section-header">
                        <span class="section-title">Metas & Objetivos</span>
                    </div>
                    <div style="display: flex; flex-direction: column; gap: 12px;">
                        <div>
                            <div style="display: flex; justify-content: space-between; font-size: 12px; font-weight:600; margin-bottom: 4px;">
                                <span>Reserva de Emergência</span><span>45%</span>
                            </div>
                            <div class="prog-bar-bg"><div class="prog-bar-fill" style="width: 45%; background: var(--accent-blue);"></div></div>
                        </div>
                        <div>
                            <div style="display: flex; justify-content: space-between; font-size: 12px; font-weight:600; margin-bottom: 4px;">
                                <span>Casa / Terreno</span><span>20%</span>
                            </div>
                            <div class="prog-bar-bg"><div class="prog-bar-fill" style="width: 20%; background: var(--accent-blue);"></div></div>
                        </div>
                        <div>
                            <div style="display: flex; justify-content: space-between; font-size: 12px; font-weight:600; margin-bottom: 4px;">
                                <span>Construção das Kitnets</span><span>10%</span>
                            </div>
                            <div class="prog-bar-bg"><div class="prog-bar-fill" style="width: 10%; background: var(--accent-blue);"></div></div>
                        </div>
                    </div>
                </div>

                <div class="neu-box section-card">
                    <div class="section-header"><span class="section-title">Balanço do Mês</span></div>
                    <div style="position: relative; height: 160px;"><canvas id="neuChart"></canvas></div>
                </div>
            </div>
        </div>

        <!-- TAB 2: RESUMO DO MÊS -->
        <div id="tab-resumo" class="view-container full-width">
            <div class="neu-box section-card">
                <div class="section-header"><span class="section-title">📊 Resumo Consolidado de Novembro / 2026</span></div>
                <div style="display:grid; grid-template-columns: repeat(3, 1fr); gap:16px; margin-bottom:24px;">
                    <div class="neu-box-inset" style="padding:16px;">
                        <div style="font-size:12px; color:var(--text-secondary);">Total Entradas</div>
                        <div style="font-size:20px; font-weight:700; color:var(--accent-green);">R$ 1.250,09</div>
                    </div>
                    <div class="neu-box-inset" style="padding:16px;">
                        <div style="font-size:12px; color:var(--text-secondary);">Total Saídas e Despesas</div>
                        <div style="font-size:20px; font-weight:700; color:var(--accent-coral);">R$ 620,67</div>
                    </div>
                    <div class="neu-box-inset" style="padding:16px;">
                        <div style="font-size:12px; color:var(--text-secondary);">Saldo Líquido / Sobra</div>
                        <div style="font-size:20px; font-weight:700; color:var(--accent-blue);">R$ 629,42</div>
                    </div>
                </div>
                <table>
                    <thead><tr><th>Categoria do Orçamento</th><th>Previsto / Orçado</th><th>Gasto Real</th><th>Diferença</th></tr></thead>
                    <tbody>
                        <tr><td>Despesas Fixas</td><td>R$ 100,00</td><td>R$ 71,90</td><td style="color:var(--accent-green);">+ R$ 28,10</td></tr>
                        <tr><td>Despesas Variáveis</td><td>R$ 600,00</td><td>R$ 548,77</td><td style="color:var(--accent-green);">+ R$ 51,23</td></tr>
                        <tr><td>Investimentos / RDB</td><td>R$ 100,00</td><td>R$ 65,30</td><td style="color:var(--accent-blue);">R$ 34,70</td></tr>
                        <tr><td>Outros Controles (Pets)</td><td>R$ 150,00</td><td>R$ 135,00</td><td style="color:var(--accent-green);">+ R$ 15,00</td></tr>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- TAB 3: ORÇAMENTOS -->
        <div id="tab-orcamentos" class="view-container full-width">
            <div class="neu-box section-card">
                <div class="section-header"><span class="section-title">⚙️ Gerenciamento de Orçamentos por Categoria</span></div>
                <div class="progress-grid" style="grid-template-columns: repeat(3, 1fr); gap: 20px;">
                    <div class="neu-box-inset progress-box" style="padding:20px;">
                        <span style="font-size: 14px; font-weight:700;">Alimentação & Mercado</span>
                        <span style="font-size: 12px; color: var(--text-secondary);">Gasto: R$ 0,00 / Limite: R$ 800,00</span>
                        <div class="prog-bar-bg" style="height:10px;"><div class="prog-bar-fill" style="width: 0%;"></div></div>
                    </div>
                    <div class="neu-box-inset progress-box" style="padding:20px;">
                        <span style="font-size: 14px; font-weight:700;">Farmácia & Saúde</span>
                        <span style="font-size: 12px; color: var(--text-secondary);">Gasto: R$ 137,27 / Limite: R$ 300,00</span>
                        <div class="prog-bar-bg" style="height:10px;"><div class="prog-bar-fill" style="width: 45%;"></div></div>
                    </div>
                    <div class="neu-box-inset progress-box" style="padding:20px;">
                        <span style="font-size: 14px; font-weight:700;">Pets</span>
                        <span style="font-size: 12px; color: var(--text-secondary);">Gasto: R$ 135,00 / Limite: R$ 200,00</span>
                        <div class="prog-bar-bg" style="height:10px;"><div class="prog-bar-fill" style="width: 67%;"></div></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- TAB 4: CONTAS BANCÁRIAS -->
        <div id="tab-contas" class="view-container full-width">
            <div class="neu-box section-card">
                <div class="section-header">
                    <span class="section-title">🏛️ Suas Contas Bancárias Vinculadas (Open Finance)</span>
                    <button class="neu-btn" style="padding: 6px 14px; font-size:12px;" onclick="syncBanks()">Sincronizar Bancos</button>
                </div>
                <div class="bank-grid">
                    <div class="neu-box-inset bank-card">
                        <div class="bank-header">
                            <span style="font-weight:700; font-size:16px;">Nubank</span>
                            <span class="badge badge-success">Conectado</span>
                        </div>
                        <div style="font-size:13px; color:var(--text-secondary);">Agência: 0001 | Conta: 89286769-3</div>
                        <div style="font-size:22px; font-weight:800; color:var(--text-main);">R$ 0,01</div>
                        <button class="neu-btn" style="padding:8px; font-size:12px;" onclick="alert('Extrato Nubank atualizado com sucesso!')">Ver Extrato Completo</button>
                    </div>
                    <div class="neu-box-inset bank-card">
                        <div class="bank-header">
                            <span style="font-weight:700; font-size:16px;">Banco do Brasil</span>
                            <span class="badge badge-success">Conectado</span>
                        </div>
                        <div style="font-size:13px; color:var(--text-secondary);">Agência: 0230 | Conta: 51003789-7</div>
                        <div style="font-size:22px; font-weight:800; color:var(--text-main);">R$ 200,00</div>
                        <button class="neu-btn" style="padding:8px; font-size:12px;" onclick="alert('Extrato Banco do Brasil atualizado!')">Ver Extrato Completo</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- TAB 5: GANHOS -->
        <div id="tab-ganhos" class="view-container full-width">
            <div class="neu-box section-card">
                <div class="section-header"><span class="section-title">📈 Relatório de Ganhos / Entradas</span></div>
                <table>
                    <thead><tr><th>Descrição</th><th>Banco / Origem</th><th>Data</th><th>Valor</th></tr></thead>
                    <tbody>
                        <tr><td>Pix Recebido (CloudWalk)</td><td>Nubank</td><td>03/11/2026</td><td style="color:var(--accent-green);font-weight:600;">+ R$ 842,09</td></tr>
                        <tr><td>Pix Recebido (BCO Brasil)</td><td>Banco do Brasil</td><td>02/11/2026</td><td style="color:var(--accent-green);font-weight:600;">+ R$ 200,00</td></tr>
                        <tr><td>Resgate RDB</td><td>Nubank</td><td>06/11/2026</td><td style="color:var(--accent-green);font-weight:600;">+ R$ 32,00</td></tr>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- TAB 6: GASTOS -->
        <div id="tab-gastos" class="view-container full-width">
            <div class="neu-box section-card">
                <div class="section-header"><span class="section-title">🛒 Relatório de Gastos / Saídas</span></div>
                <table>
                    <thead><tr><th>Descrição</th><th>Categoria</th><th>Data</th><th>Valor</th></tr></thead>
                    <tbody>
                        <tr><td>Constru Silva (Material)</td><td>Despesas Variáveis</td><td>08/11/2026</td><td style="color:var(--accent-coral);font-weight:600;">- R$ 50,00</td></tr>
                        <tr><td>Drogarias Ultrapopula</td><td>Farmácia</td><td>07/11/2026</td><td style="color:var(--accent-coral);font-weight:600;">- R$ 137,27</td></tr>
                        <tr><td>Netflix.Com</td><td>Assinaturas</td><td>08/11/2026</td><td style="color:var(--accent-coral);font-weight:600;">- R$ 71,90</td></tr>
                        <tr><td>Pets (Alimentação / Cuidados)</td><td>Outros Controles</td><td>Novembro</td><td style="color:var(--accent-coral);font-weight:600;">- R$ 135,00</td></tr>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- TAB 7: TRANSFERÊNCIAS -->
        <div id="tab-transferencias" class="view-container full-width">
            <div class="neu-box section-card">
                <div class="section-header"><span class="section-title">🔁 Relatório de Transferências & Pix Enviados</span></div>
                <table>
                    <thead><tr><th>Favorecido / Destino</th><th>Banco</th><th>Data</th><th>Valor</th></tr></thead>
                    <tbody>
                        <tr><td>Isadora Pereira Almeida</td><td>Nubank</td><td>02/11/2026</td><td>R$ 200,00</td></tr>
                        <tr><td>Marla Pollyane Novais Gomes</td><td>Banco do Brasil</td><td>04/11/2026</td><td>R$ 15,00</td></tr>
                        <tr><td>João Xavier Coutinho</td><td>Nubank</td><td>06/11/2026</td><td>R$ 7,00</td></tr>
                        <tr><td>Manoel Pimentel Souza</td><td>PicPay</td><td>07/11/2026</td><td>R$ 105,00</td></tr>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- TAB 8: FATURAS -->
        <div id="tab-faturas" class="view-container full-width">
            <div class="neu-box section-card">
                <div class="section-header"><span class="section-title">💳 Faturas de Cartão de Crédito</span></div>
                <table>
                    <thead><tr><th>Cartão</th><th>Vencimento</th><th>Valor Fatura</th><th>Status</th></tr></thead>
                    <tbody>
                        <tr><td>Nubank Platinum / Master</td><td>10/11/2026</td><td>R$ 323,19</td><td><span class="badge badge-success">Pago</span></td></tr>
                        <tr><td>Pix no Crédito (Marla)</td><td>03/11/2026</td><td>R$ 16,47</td><td><span class="badge badge-success">Pago</span></td></tr>
                    </tbody>
                </table>
            </div>
        </div>
    </main>

    <!-- MODAL -->
    <div class="modal" id="txModal">
        <div class="modal-content neu-box">
            <h3 style="margin-bottom: 16px;">Adicionar Lançamento</h3>
            <div class="form-group"><label>Descrição</label><input type="text" id="m-desc" placeholder="Ex: Mercado, Conta..."></div>
            <div class="form-group">
                <label>Categoria</label>
                <select id="m-cat">
                    <option>Despesas Fixas</option><option>Despesas Variáveis</option><option>Entradas</option><option>Cartão de Crédito</option><option>Investimentos</option><option>Pets</option>
                </select>
            </div>
            <div class="form-group"><label>Valor (R$)</label><input type="number" id="m-val" placeholder="0.00"></div>
            <div class="form-group">
                <label>Tipo</label>
                <select id="m-type"><option value="income">Entrada (+)</option><option value="expense">Saída (-)</option></select>
            </div>
            <button class="neu-btn" style="width: 100%; padding: 12px; background: var(--bg-color); color: var(--accent-green);" onclick="addTx()">Salvar Transação</button>
            <button class="neu-btn" style="width: 100%; padding: 10px; margin-top: 8px; color: var(--accent-coral);" onclick="closeModal()">Cancelar</button>
        </div>
    </div>

    <script>
        lucide.createIcons();

        // Chart setup
        const ctx = document.getElementById('neuChart').getContext('2d');
        new Chart(ctx, {
            type: 'doughnut',
            data: {
                labels: ['Fixas', 'Variáveis', 'Investimentos'],
                datasets: [{
                    data: [71.90, 548.77, 65.30],
                    backgroundColor: ['#FF7272', '#489CC1', '#21A87D'],
                    borderWidth: 0
                }]
            },
            options: {
                responsive: true, maintainAspectRatio: false, cutout: '70%',
                plugins: { legend: { position: 'bottom', labels: { boxWidth: 12, font: { size: 11 } } } }
            }
        });

        function switchTab(tabId, btn) {
            document.querySelectorAll('.view-container').forEach(el => el.classList.remove('active'));
            document.getElementById('tab-' + tabId).classList.add('active');
            
            document.querySelectorAll('.nav-list button').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');

            const titles = {
                'dashboard': 'Dashboard - Novembro / 2026',
                'resumo': 'Resumo Consolidado do Mês',
                'orcamentos': 'Orçamentos e Limites',
                'contas': 'Contas Bancárias (Open Finance)',
                'ganhos': 'Relatório de Ganhos',
                'gastos': 'Relatório de Gastos',
                'transferencias': 'Transferências e Pix',
                'faturas': 'Faturas de Cartão'
            };
            document.getElementById('page-title').innerText = titles[tabId];
        }

        function openModal() { document.getElementById('txModal').style.display = 'flex'; }
        function closeModal() { document.getElementById('txModal').style.display = 'none'; }
        function syncBanks() { alert('Sincronização bancária Open Finance realizada com sucesso! Saldos atualizados.'); }

        function addTx() {
            const desc = document.getElementById('m-desc').value;
            const cat = document.getElementById('m-cat').value;
            const val = parseFloat(document.getElementById('m-val').value);
            const type = document.getElementById('m-type').value;

            if(!desc || isNaN(val)) { alert('Preencha todos os campos!'); return; }

            const tbody = document.getElementById('dash-recent-tbody');
            const tr = document.createElement('tr');
            const isInc = type === 'income';
            tr.innerHTML = `
                <td>${desc}</td>
                <td>${cat}</td>
                <td>Hoje</td>
                <td style="color: ${isInc ? 'var(--accent-green)' : 'var(--accent-coral)'}; font-weight:600;">${isInc ? '+' : '-'} R$ ${val.toFixed(2)}</td>
                <td><span class="badge badge-success">Concluído</span></td>
            `;
            tbody.insertBefore(tr, tbody.firstChild);
            closeModal();
            document.getElementById('m-desc').value = '';
            document.getElementById('m-val').value = '';
            alert('Transação adicionada com sucesso!');
        }
    </script>
</body>
</html>

