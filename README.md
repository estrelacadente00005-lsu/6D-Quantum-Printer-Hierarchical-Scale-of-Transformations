
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Impressora Quântica 6D - Escala Hierárquica de Transformações</title>
    <!-- Tailwind CSS for Layout and Utility -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        :root {
            --bg-dark: #02040a;
            --panel-bg: rgba(6, 12, 28, 0.85);
            --border-cyan: rgba(0, 243, 255, 0.3);
            --neon-cyan: #00f3ff;
            --neon-purple: #b700ff;
            --neon-gold: #ffc400;
            --neon-green: #00ff73;
            --neon-pink: #ff0055;
            --text-main: #d0f2ff;
            --font-code: 'Courier New', Courier, monospace;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: var(--font-code);
            user-select: none;
        }

        body {
            background-color: var(--bg-dark);
            color: var(--text-main);
            min-height: 100vh;
            background-image: 
                radial-gradient(circle at 10% 10%, rgba(183, 0, 255, 0.12) 0%, transparent 40%),
                radial-gradient(circle at 90% 90%, rgba(0, 243, 255, 0.1) 0%, transparent 45%),
                radial-gradient(circle at 50% 50%, rgba(255, 0, 85, 0.05) 0%, transparent 60%);
            overflow-x: hidden;
        }

        /* Glassmorphism Panel Styling */
        .hud-panel {
            background: var(--panel-bg);
            border: 1px solid var(--border-cyan);
            border-radius: 8px;
            backdrop-filter: blur(10px);
            box-shadow: 0 0 20px rgba(0, 243, 255, 0.08), inset 0 0 15px rgba(0, 243, 255, 0.03);
            transition: all 0.3s ease;
        }

        .hud-panel:hover {
            border-color: rgba(0, 243, 255, 0.5);
            box-shadow: 0 0 25px rgba(0, 243, 255, 0.15), inset 0 0 20px rgba(0, 243, 255, 0.05);
        }

        .hud-title {
            font-size: 0.85rem;
            color: var(--neon-cyan);
            border-bottom: 1px solid var(--border-cyan);
            padding-bottom: 6px;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            font-weight: bold;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* Scrollbars */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(2, 6, 16, 0.8);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--border-cyan);
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: var(--neon-cyan);
        }

        /* Rank Buttons / Cards */
        .rank-card {
            background: rgba(3, 10, 24, 0.75);
            border: 1px solid rgba(0, 243, 255, 0.2);
            border-radius: 6px;
            padding: 8px 10px;
            cursor: pointer;
            transition: all 0.25s ease;
            position: relative;
            overflow: hidden;
        }

        .rank-card:hover {
            border-color: var(--neon-cyan);
            background: rgba(0, 243, 255, 0.1);
            transform: translateX(3px);
        }

        .rank-card.active {
            border-color: var(--neon-green);
            background: linear-gradient(90deg, rgba(0, 255, 115, 0.15), rgba(0, 243, 255, 0.05));
            box-shadow: 0 0 15px rgba(0, 255, 115, 0.3);
        }

        .rank-card.cosmic-active {
            border-color: var(--neon-purple);
            background: linear-gradient(90deg, rgba(183, 0, 255, 0.2), rgba(255, 0, 85, 0.05));
            box-shadow: 0 0 15px rgba(183, 0, 255, 0.4);
        }

        .rank-card.omni-active {
            border-color: var(--neon-gold);
            background: linear-gradient(90deg, rgba(255, 196, 0, 0.25), rgba(183, 0, 255, 0.1));
            box-shadow: 0 0 20px rgba(255, 196, 0, 0.5);
        }

        /* Custom Range Inputs */
        input[type="range"] {
            appearance: none;
            -webkit-appearance: none;
            width: 100%;
            height: 6px;
            background: #08162b;
            border-radius: 3px;
            outline: none;
            border: 1px solid rgba(0, 243, 255, 0.2);
        }

        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: var(--neon-cyan);
            cursor: pointer;
            box-shadow: 0 0 10px var(--neon-cyan);
            transition: all 0.2s ease;
        }

        input[type="range"]::-webkit-slider-thumb:hover {
            transform: scale(1.2);
            background: #fff;
        }

        /* Action Button */
        .btn-transmute {
            background: linear-gradient(135deg, rgba(0, 243, 255, 0.2), rgba(183, 0, 255, 0.2));
            border: 1px solid var(--neon-cyan);
            color: #fff;
            font-weight: bold;
            letter-spacing: 2px;
            text-transform: uppercase;
            padding: 12px;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 0 15px rgba(0, 243, 255, 0.2);
        }

        .btn-transmute:hover {
            background: linear-gradient(135deg, var(--neon-cyan), var(--neon-purple));
            color: #000;
            box-shadow: 0 0 25px rgba(0, 243, 255, 0.6);
            transform: translateY(-2px);
        }

        .btn-transmute:active {
            transform: translateY(0);
        }

        /* Terminal Logging */
        .terminal-box {
            background: rgba(1, 4, 12, 0.9);
            border: 1px solid rgba(0, 243, 255, 0.2);
            border-radius: 6px;
            font-size: 0.72rem;
            line-height: 1.4;
            color: #72bbfd;
        }

        .log-item {
            margin-bottom: 3px;
            animation: fadeIn 0.2s ease-in;
        }
        .log-item.info { color: var(--neon-cyan); }
        .log-item.warn { color: var(--neon-gold); }
        .log-item.alert { color: var(--neon-pink); }
        .log-item.success { color: var(--neon-green); }
        .log-item.omni { color: var(--neon-purple); font-weight: bold; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(2px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Canvas Overlays & Tags */
        .canvas-container {
            position: relative;
            width: 100%;
            height: 100%;
            background: #010309;
            border-radius: 6px;
            overflow: hidden;
            border: 1px solid rgba(0, 243, 255, 0.25);
        }

        .canvas-tag {
            position: absolute;
            top: 10px;
            left: 12px;
            background: rgba(0, 0, 0, 0.75);
            border: 1px solid var(--border-cyan);
            padding: 4px 10px;
            font-size: 0.68rem;
            color: var(--neon-cyan);
            border-radius: 4px;
            letter-spacing: 1px;
            pointer-events: none;
            backdrop-filter: blur(4px);
        }

        .canvas-hud-info {
            position: absolute;
            bottom: 10px;
            right: 12px;
            background: rgba(0, 0, 0, 0.85);
            border: 1px solid rgba(183, 0, 255, 0.3);
            padding: 6px 12px;
            font-size: 0.65rem;
            color: #b0d8ff;
            border-radius: 4px;
            text-align: right;
            pointer-events: none;
            backdrop-filter: blur(4px);
        }
    </style>
</head>
<body class="p-3 md:p-5 flex flex-col gap-4">

    <!-- HEADER FUTURISTA -->
    <header class="hud-panel p-4 flex flex-col md:flex-row justify-between items-center gap-3">
        <div>
            <div class="flex items-center gap-2">
                <span class="w-3 h-3 rounded-full bg-cyan-400 animate-pulse"></span>
                <h1 class="text-xl font-bold tracking-widest text-cyan-300">IMPRESSORA QUÂNTICA 6D</h1>
            </div>
            <p class="text-xs text-slate-400 mt-1">Simulação Hierárquica de Transformações Biomáquina (MHD + SAMS + SROS + Singularidade Direcionada)</p>
        </div>
        <div class="flex flex-wrap gap-2 items-center text-xs">
            <span class="px-3 py-1 rounded-full bg-cyan-950/80 border border-cyan-500/40 text-cyan-300">ESTADO: OPERACIONAL</span>
            <span class="px-3 py-1 rounded-full bg-purple-950/80 border border-purple-500/40 text-purple-300" id="topTechStatus">TECNOLOGIA: SROS MULTIVERSO</span>
            <span class="px-3 py-1 rounded-full bg-yellow-950/80 border border-yellow-500/40 text-yellow-300" id="topCoherenceTag">COERÊNCIA: 6D HILBERT</span>
        </div>
    </header>

    <!-- CONTAINER PRINCIPAL (GRID 3 COLUNAS) -->
    <main class="grid grid-cols-1 lg:grid-cols-12 gap-4 flex-1">

        <!-- COLUNA 1: LISTA HIERÁRQUICA SELEÇÃO (POSIÇÕES 1 A 9) -->
        <section class="lg:col-span-3 hud-panel p-3 flex flex-col gap-3 max-h-[85vh] overflow-y-auto">
            <div class="hud-title">
                <span>ESCALA HIERÁRQUICA (1-9)</span>
                <span class="text-xs text-cyan-400" id="selectedRankTag">POS : 1</span>
            </div>
            <p class="text-[0.68rem] text-slate-400">Selecione o nível de transmutação para calibrar a matriz 6D:</p>

            <div class="flex flex-col gap-2" id="rankListContainer">
                <!-- Cards renderizados dinamicamente via JS -->
            </div>
        </section>

        <!-- COLUNA 2: DUAL CANVAS ENGINE & SIMULAÇÃO GRAFICA -->
        <section class="lg:col-span-6 flex flex-col gap-4">
            
            <!-- CANVAS 1: BERÇO DE IMPRESSÃO QUÂNTICA 6D -->
            <div class="hud-panel p-2 flex flex-col gap-2 h-[340px]">
                <div class="canvas-container">
                    <div class="canvas-tag">CANVAS 1: BERÇO DE IMPRESSÃO SAMS & FEIXES SROS</div>
                    <canvas id="canvasChamber"></canvas>
                    <div class="canvas-hud-info" id="hudChamberInfo">
                        CAMPO SAMS: 100.0% | REATOR MHD: 1.2 GW<br>
                        MUTAÇÃO: ESCALA MACROMOLECULAR
                    </div>
                </div>
            </div>

            <!-- CANVAS 2: VISUALIZADOR DA ESCALA DA REALIDADE / ESPAÇO-TEMPO 11D -->
            <div class="hud-panel p-2 flex flex-col gap-2 h-[260px]">
                <div class="canvas-container">
                    <div class="canvas-tag">CANVAS 2: MALHA DO ESPAÇO-TEMPO & FENDA DE BRANAS</div>
                    <canvas id="canvasSpace"></canvas>
                    <div class="canvas-hud-info" id="hudSpaceInfo">
                        DENSIDADE DE DISTORÇÃO: LOW<br>
                        DIMENSÕES ATIVAS: 3D / 4D
                    </div>
                </div>
            </div>

            <!-- PAINEL DE TELEMETRIA E LOGS DE OPERAÇÃO -->
            <div class="hud-panel p-3 flex flex-col gap-2">
                <div class="hud-title">
                    <span>TELEMETRIA & TERMINAL DE REGISTROS (PT-BR)</span>
                    <span class="text-[0.65rem] text-green-400">LOGS AO VIVO</span>
                </div>
                <div class="terminal-box p-2 h-28 overflow-y-auto" id="terminalLog">
                    <div class="log-item info">> Sistema da Impressora Quântica 6D inicializado.</div>
                    <div class="log-item success">> Estabilizador SAMS pronto. Aguardando seleção de parâmetro...</div>
                </div>
            </div>
        </section>

        <!-- COLUNA 3: ESTATÍSTICAS E MODULADORES INTERATIVOS -->
        <section class="lg:col-span-3 hud-panel p-3 flex flex-col gap-4 justify-between">
            
            <!-- PAINEL DE ESTATÍSTICAS DO NÍVEL SELECIONADO -->
            <div class="flex flex-col gap-2">
                <div class="hud-title">
                    <span>ESTATÍSTICAS DA MUTAÇÃO</span>
                </div>

                <div class="bg-slate-950/80 p-2.5 rounded border border-cyan-500/20 flex flex-col gap-2">
                    <div>
                        <div class="text-[0.65rem] text-slate-400">PERSONAGEM / MODELO:</div>
                        <div class="text-sm font-bold text-cyan-300" id="statName">Batman / Iron Man</div>
                    </div>
                    <div>
                        <div class="text-[0.65rem] text-slate-400">ENGENHARIA REQUERIDA:</div>
                        <div class="text-xs text-yellow-300" id="statEng">Cibernética e Metalurgia Exótica</div>
                    </div>
                    <div>
                        <div class="text-[0.65rem] text-slate-400">NÍVEL DE ATUAÇÃO 6D:</div>
                        <div class="text-xs text-purple-300" id="statLevel">Escala Macromolecular (3D/4D)</div>
                    </div>
                    <div class="grid grid-cols-2 gap-2 pt-1 border-t border-cyan-900/50">
                        <div>
                            <div class="text-[0.6rem] text-slate-400">FREQUÊNCIA SPIN:</div>
                            <div class="text-xs font-bold text-cyan-400" id="statSpin">440.0 GHz</div>
                        </div>
                        <div>
                            <div class="text-[0.6rem] text-slate-400">POTÊNCIA HD-MHD:</div>
                            <div class="text-xs font-bold text-pink-400" id="statPower">1.2 GW</div>
                        </div>
                    </div>
                    <div>
                        <div class="text-[0.6rem] text-slate-400">INTERFERÊNCIA NA REALIDADE:</div>
                        <div class="w-full bg-slate-900 h-2 rounded overflow-hidden mt-1 border border-cyan-500/30">
                            <div class="bg-gradient-to-r from-cyan-400 via-purple-500 to-pink-500 h-full transition-all duration-300" id="statInterferenceBar" style="width: 5%;"></div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- CONTROLES E SLIDERS INTERATIVOS -->
            <div class="flex flex-col gap-3">
                <div class="hud-title">
                    <span>MODULADORES QUÂNTICOS</span>
                </div>

                <!-- SLIDER 1: FREQUÊNCIA DE FASE QUÂNTICA -->
                <div class="flex flex-col gap-1">
                    <div class="flex justify-between text-[0.68rem]">
                        <span class="text-slate-300">Frequência de Fase Quântica:</span>
                        <span class="text-cyan-400 font-bold" id="lblFreqVal">440.0 GHz</span>
                    </div>
                    <input type="range" id="sliderFreq" min="10" max="2000" value="440">
                </div>

                <!-- SLIDER 2: DENSIDADE DE POTÊNCIA HD-MHD -->
                <div class="flex flex-col gap-1">
                    <div class="flex justify-between text-[0.68rem]">
                        <span class="text-slate-300">Potência Reator HD-MHD:</span>
                        <span class="text-pink-400 font-bold" id="lblMhdVal">1.2 GW</span>
                    </div>
                    <input type="range" id="sliderMhd" min="1" max="1000" value="12">
                </div>

                <!-- SLIDER 3: COERÊNCIA DIMENSIONAL -->
                <div class="flex flex-col gap-1">
                    <div class="flex justify-between text-[0.68rem]">
                        <span class="text-slate-300">Coerência Dimensional:</span>
                        <span class="text-purple-400 font-bold" id="lblDimVal">3D / 4D</span>
                    </div>
                    <input type="range" id="sliderDim" min="3" max="11" value="4">
                </div>

                <!-- SLIDER 4: ESTABILIZADOR SAMS -->
                <div class="flex flex-col gap-1">
                    <div class="flex justify-between text-[0.68rem]">
                        <span class="text-slate-300">Estabilizador SAMS (Contenção):</span>
                        <span class="text-green-400 font-bold" id="lblSamsVal">99.8%</span>
                    </div>
                    <input type="range" id="sliderSams" min="10" max="100" value="100">
                </div>
            </div>

            <!-- BOTÃO PRINCIPAL DE TRANSMUTAÇÃO -->
            <button class="btn-transmute w-full" id="btnStartTransmutation" onclick="executeTransmutation()">
                INICIAR TRANSMUTAÇÃO DE ESCALA
            </button>
        </section>

    </main>

    <!-- SCRIPT DA APLICAÇÃO & DUAL CANVAS ENGINE -->
    <script>
        // DATASET COMPLETO DAS 9 POSIÇÕES DA ESCALA
        const TRANSFORMATION_SCALE = [
            {
                rank: 1,
                name: "Batman / Iron Man",
                engineering: "Cibernética e metalurgia exótica via levitação magnética.",
                level: "Escala Macromolecular (3D/4D)",
                defaultSpin: "440.0 GHz",
                defaultPower: "1.2 GW",
                interference: 5,
                dim: 4,
                color: "#00f3ff",
                type: "bio",
                description: "Reestruturação macromolecular de armaduras exóticas e suporte cibernético."
            },
            {
                rank: 2,
                name: "Cyborg (DC)",
                engineering: "Fusão biônica celular e computação quântica integrada ao sistema nervoso.",
                level: "Escala Nanométrica",
                defaultSpin: "1.8 THz",
                defaultPower: "8.5 GW",
                interference: 15,
                dim: 4,
                color: "#00a2ff",
                type: "bio",
                description: "Reconfiguração de nanobots biônicos integrados à sinapse cerebral."
            },
            {
                rank: 3,
                name: "Wolverine / Deadpool",
                engineering: "Reconfiguração genética de telômeros e nanobots de regeneração biológica.",
                level: "Escala Genética / Atômica",
                defaultSpin: "12.5 THz",
                defaultPower: "45.0 GW",
                interference: 28,
                dim: 4,
                color: "#00ff73",
                type: "bio",
                description: "Re-impressão celular instantânea e regeneração tecidual quântica."
            },
            {
                rank: 4,
                name: "Flash (DC)",
                engineering: "Campo de amortecimento cinético (SAMS) para anular o atrito e manipulação de spin quântico.",
                level: "Escala Subatômica (Fase)",
                defaultSpin: "180.0 THz",
                defaultPower: "350.0 GW",
                interference: 45,
                dim: 5,
                color: "#ffc400",
                type: "speed",
                description: "Deslocamento de fase quântica do corpo para atravessar matéria sólida."
            },
            {
                rank: 5,
                name: "Superman (DC)",
                engineering: "Densidade atômica hiper-compactada e bio-reatores baseados na absorção de radiação estelar.",
                level: "Escala Métrica Local",
                defaultSpin: "1.5 PHz",
                defaultPower: "12.0 TW",
                interference: 62,
                dim: 5,
                color: "#ff3300",
                type: "cosmic",
                description: "Compactação quântica da matéria biológica com tolerância a fotossíntese estelar."
            },
            {
                rank: 6,
                name: "Capitã Marvel (Marvel)",
                engineering: "Absorção de energia de singularidades e conversão total de massa em fótons estáveis.",
                level: "Escala Cósmica",
                defaultSpin: "85.0 PHz",
                defaultPower: "450.0 TW",
                interference: 78,
                dim: 6,
                color: "#ff0088",
                type: "cosmic",
                description: "Morfogênese puramente fotônica sustentada por micro-singularidades direcionadas."
            },
            {
                rank: 7,
                name: "Doutor Manhattan (DC)",
                engineering: "Desintegração física completa e conversão em função de onda macroscópica consciente.",
                level: "Escala Dimensional (5D)",
                defaultSpin: "1.2 EHz",
                defaultPower: "980.0 PW",
                interference: 90,
                dim: 7,
                color: "#b700ff",
                type: "quantum",
                description: "Corpo transformado em função de onda intrínseca sem necessidade de órgãos."
            },
            {
                rank: 8,
                name: "Galactus (Marvel)",
                engineering: "Reconstrução do indivíduo como uma força cosmológica viva que se alimenta diretamente da entropia de planetas.",
                level: "Escala Multiversal / Branas",
                defaultSpin: "999.0 EHz",
                defaultPower: "50.0 ZW",
                interference: 98,
                dim: 9,
                color: "#ff0055",
                type: "multiverse",
                description: "Ancoragem do organismo em branas cósmicas para absorção de entropia."
            },
            {
                rank: 9,
                name: "Tribunal Vivo / Spectre",
                engineering: "Consciência fragmentada e ancorada em todas as realidades simultaneamente via SROS Multiverso.",
                level: "Escala Omniversal (Oniverso Total)",
                defaultSpin: "∞ INFINITO",
                defaultPower: "∞ YOTTA-WATTS",
                interference: 100,
                dim: 11,
                color: "#ffea00",
                type: "omni",
                description: "Dissolução total em todos os planos dimensionais simultâneos."
            }
        ];

        // ESTADO GLOBAL DO APLICATIVO
        let currentRankIndex = 0;
        let isTransmuting = false;
        let transmutationProgress = 0;

        // ELEMENTOS DOM
        const canvasChamber = document.getElementById('canvasChamber');
        const ctxChamber = canvasChamber.getContext('2d');
        const canvasSpace = document.getElementById('canvasSpace');
        const ctxSpace = canvasSpace.getContext('2d');

        const rankListContainer = document.getElementById('rankListContainer');
        const terminalLog = document.getElementById('terminalLog');

        const statName = document.getElementById('statName');
        const statEng = document.getElementById('statEng');
        const statLevel = document.getElementById('statLevel');
        const statSpin = document.getElementById('statSpin');
        const statPower = document.getElementById('statPower');
        const statInterferenceBar = document.getElementById('statInterferenceBar');
        const selectedRankTag = document.getElementById('selectedRankTag');

        const sliderFreq = document.getElementById('sliderFreq');
        const sliderMhd = document.getElementById('sliderMhd');
        const sliderDim = document.getElementById('sliderDim');
        const sliderSams = document.getElementById('sliderSams');

        const lblFreqVal = document.getElementById('lblFreqVal');
        const lblMhdVal = document.getElementById('lblMhdVal');
        const lblDimVal = document.getElementById('lblDimVal');
        const lblSamsVal = document.getElementById('lblSamsVal');

        const hudChamberInfo = document.getElementById('hudChamberInfo');
        const hudSpaceInfo = document.getElementById('hudSpaceInfo');

        // AJUSTAR DENSIDADE E REDIMENSIONAMENTO DOS CANVASES
        function resizeCanvases() {
            canvasChamber.width = canvasChamber.parentElement.clientWidth;
            canvasChamber.height = canvasChamber.parentElement.clientHeight;
            canvasSpace.width = canvasSpace.parentElement.clientWidth;
            canvasSpace.height = canvasSpace.parentElement.clientHeight;
        }
        window.addEventListener('resize', resizeCanvases);

        // LOGS NO TERMINAL
        function addLog(msg, type = 'info') {
            const entry = document.createElement('div');
            entry.className = `log-item ${type}`;
            const time = new Date().toLocaleTimeString();
            entry.textContent = `[${time}] ${msg}`;
            terminalLog.appendChild(entry);
            terminalLog.scrollTop = terminalLog.scrollHeight;
        }

        // INITIALIZE RANK CARDS MENU
        function renderRankList() {
            rankListContainer.innerHTML = '';
            TRANSFORMATION_SCALE.forEach((item, index) => {
                const card = document.createElement('div');
                
                let activeClass = '';
                if (index === currentRankIndex) {
                    if (item.rank >= 8) activeClass = 'omni-active';
                    else if (item.rank >= 6) activeClass = 'cosmic-active';
                    else activeClass = 'active';
                }

                card.className = `rank-card ${activeClass}`;
                card.onclick = () => selectRank(index);

                card.innerHTML = `
                    <div class="flex justify-between items-center">
                        <span class="text-xs font-bold" style="color: ${item.color}">${item.rank}. ${item.name}</span>
                        <span class="text-[0.6rem] px-1.5 py-0.5 rounded bg-slate-900 border border-slate-700 text-slate-300">POS ${item.rank}</span>
                    </div>
                    <div class="text-[0.65rem] text-slate-400 mt-1 truncate">${item.level}</div>
                `;

                rankListContainer.appendChild(card);
            });
        }

        // SELECIONAR NÍVEL HIERÁRQUICO
        function selectRank(index) {
            currentRankIndex = index;
            const rankData = TRANSFORMATION_SCALE[index];

            selectedRankTag.textContent = `POS : ${rankData.rank}`;
            statName.textContent = rankData.name;
            statEng.textContent = rankData.engineering;
            statLevel.textContent = rankData.level;
            statSpin.textContent = rankData.defaultSpin;
            statPower.textContent = rankData.defaultPower;
            statInterferenceBar.style.width = `${rankData.interference}%`;

            sliderDim.value = rankData.dim;
            lblDimVal.textContent = `${rankData.dim}D Hilbert`;

            renderRankList();

            let logStyle = 'info';
            if (rankData.rank >= 8) logStyle = 'omni';
            else if (rankData.rank >= 6) logStyle = 'alert';
            else if (rankData.rank >= 4) logStyle = 'warn';

            addLog(`Mapeamento atualizado para Posição ${rankData.rank}: ${rankData.name}`, logStyle);
            addLog(`Parâmetro 6D: ${rankData.description}`, 'info');

            updateHudInfos();
        }

        // ATUALIZAÇÃO DOS EVENTOS DE SLIDER
        sliderFreq.addEventListener('input', (e) => {
            lblFreqVal.textContent = (e.target.value * 1.0).toFixed(1) + " GHz";
            updateHudInfos();
        });
        sliderMhd.addEventListener('input', (e) => {
            lblMhdVal.textContent = (e.target.value * 0.1).toFixed(1) + " GW";
            updateHudInfos();
        });
        sliderDim.addEventListener('input', (e) => {
            lblDimVal.textContent = `${e.target.value}D Hilbert`;
            updateHudInfos();
        });
        sliderSams.addEventListener('input', (e) => {
            lblSamsVal.textContent = `${e.target.value}.0%`;
            updateHudInfos();
        });

        function updateHudInfos() {
            const current = TRANSFORMATION_SCALE[currentRankIndex];
            hudChamberInfo.innerHTML = `
                CAMPO SAMS: ${sliderSams.value}.0% | REATOR MHD: ${(sliderMhd.value * 0.1).toFixed(1)} GW<br>
                MUTAÇÃO: ${current.level.toUpperCase()}
            `;

            hudSpaceInfo.innerHTML = `
                DISTORÇÃO ESPAÇO-TEMPO: ${current.interference}%<br>
                DIMENSÕES ATIVAS: ${sliderDim.value}D VÁCUO QUÂNTICO
            `;
        }

        // TRIGGER DE TRANSMUTAÇÃO
        function executeTransmutation() {
            if (isTransmuting) return;
            isTransmuting = true;
            transmutationProgress = 0;

            const rankData = TRANSFORMATION_SCALE[currentRankIndex];
            addLog(`=== INICIANDO PROTOCOLO DE TRANSMUTAÇÃO 6D [POS ${rankData.rank}] ===`, 'alert');
            addLog(`Ativando Feixes SROS em Frequência ${sliderFreq.value} GHz...`, 'warn');
            addLog(`Carregando Anel Magnético HD-MHD em ${(sliderMhd.value * 0.1).toFixed(1)} GW...`, 'info');
            
            if (rankData.rank >= 7) {
                addLog(`AVISO DE SEGURANÇA SAMS: Flutuação de vácuo iniciada! Injeção de fase ${sliderDim.value}D!`, 'omni');
            }
        }

        // -------------------------------------------------------------
        // ENGINE GRÁFICO - CANVAS 1: BERÇO DE IMPRESSÃO SAMS & FEIXES SROS
        // -------------------------------------------------------------
        function drawChamberCanvas() {
            const w = canvasChamber.width;
            const h = canvasChamber.height;
            const cx = w / 2;
            const cy = h / 2;
            const current = TRANSFORMATION_SCALE[currentRankIndex];

            ctxChamber.clearRect(0, 0, w, h);

            // Fundo escuro com linhas de grade cibernética
            ctxChamber.strokeStyle = 'rgba(0, 243, 255, 0.05)';
            ctxChamber.lineWidth = 1;
            for (let x = 0; x < w; x += 25) {
                ctxChamber.beginPath(); ctxChamber.moveTo(x, 0); ctxChamber.lineTo(x, h); ctxChamber.stroke();
            }
            for (let y = 0; y < h; y += 25) {
                ctxChamber.beginPath(); ctxChamber.moveTo(0, y); ctxChamber.lineTo(w, y); ctxChamber.stroke();
            }

            const time = Date.now() * 0.003;

            // Anéis de contenção SAMS & MHD
            const ringCount = 3;
            for (let i = 1; i <= ringCount; i++) {
                ctxChamber.save();
                ctxChamber.translate(cx, cy);
                ctxChamber.rotate(time * (i % 2 === 0 ? 1 : -1) * 0.5);
                ctxChamber.strokeStyle = current.color;
                ctxChamber.lineWidth = 2;
                ctxChamber.shadowColor = current.color;
                ctxChamber.shadowBlur = 10;
                
                ctxChamber.beginPath();
                ctxChamber.ellipse(0, 0, 80 + i * 22, 35 + i * 10, 0, 0, Math.PI * 2);
                ctxChamber.stroke();
                ctxChamber.restore();
            }

            // Feixes de Lasers SROS Verticais
            ctxChamber.strokeStyle = isTransmuting ? '#00ff73' : 'rgba(0, 243, 255, 0.4)';
            ctxChamber.lineWidth = isTransmuting ? 3 : 1;
            ctxChamber.shadowColor = '#00ff73';
            ctxChamber.shadowBlur = isTransmuting ? 15 : 0;

            for (let offset of [-60, -20, 20, 60]) {
                ctxChamber.beginPath();
                ctxChamber.moveTo(cx + offset, 20);
                ctxChamber.lineTo(cx + offset, h - 20);
                ctxChamber.stroke();
            }

            // DESENHO DO CORPO DO VOLUNTÁRIO / ENTIDADE CONFORME A POSIÇÃO DA ESCALA
            ctxChamber.save();
            ctxChamber.translate(cx, cy);

            ctxChamber.fillStyle = current.color;
            ctxChamber.shadowColor = current.color;
            ctxChamber.shadowBlur = 20;

            if (current.rank <= 3) {
                // Corpo humanoide / biômetro com envoltório cibernético
                ctxChamber.beginPath(); ctxChamber.arc(0, -30, 10, 0, Math.PI * 2); ctxChamber.fill(); // Cabeça
                ctxChamber.fillRect(-10, -18, 20, 38); // Tronco
                ctxChamber.fillRect(-18, -18, 6, 32); ctxChamber.fillRect(12, -18, 6, 32); // Braços
                ctxChamber.fillRect(-8, 20, 6, 35); ctxChamber.fillRect(2, 20, 6, 35); // Pernas
            } else if (current.rank <= 6) {
                // Corpo envolto por aura de energia densa (Flash/Superman/Capitã Marvel)
                ctxChamber.beginPath(); ctxChamber.arc(0, -30, 12, 0, Math.PI * 2); ctxChamber.fill();
                ctxChamber.fillRect(-12, -16, 24, 40);
                ctxChamber.fillRect(-22, -16, 8, 35); ctxChamber.fillRect(14, -16, 8, 35);
                ctxChamber.fillRect(-10, 24, 8, 40); ctxChamber.fillRect(2, 24, 8, 40);

                // Aura pulsatória
                ctxChamber.strokeStyle = '#ffffff';
                ctxChamber.lineWidth = 2;
                ctxChamber.beginPath();
                ctxChamber.arc(0, 0, 45 + Math.sin(time * 5) * 5, 0, Math.PI * 2);
                ctxChamber.stroke();
            } else if (current.rank <= 8) {
                // Função de onda macroscópica / Galactus / Manhattan (Nuvem de partículas)
                for (let p = 0; p < 40; p++) {
                    const angle = p * 0.2 + time;
                    const rad = 20 + (p * 1.5);
                    const px = Math.cos(angle) * rad;
                    const py = Math.sin(angle) * rad;
                    ctxChamber.beginPath();
                    ctxChamber.arc(px, py, 3 + (p % 3), 0, Math.PI * 2);
                    ctxChamber.fill();
                }
            } else {
                // Rank 9: Tribunal Vivo / Spectre (Fraturas de rostos e nós de realidade)
                for (let head = -1; head <= 1; head++) {
                    ctxChamber.beginPath();
                    ctxChamber.arc(head * 35, -20, 14, 0, Math.PI * 2);
                    ctxChamber.fill();
                }
                ctxChamber.strokeStyle = '#ffea00';
                ctxChamber.lineWidth = 2;
                ctxChamber.beginPath();
                ctxChamber.arc(0, 10, 60 + Math.cos(time * 3) * 8, 0, Math.PI * 2);
                ctxChamber.stroke();
            }

            ctxChamber.restore();

            // ANIMAÇÃO DE TRANSMUTAÇÃO EM ANDAMENTO
            if (isTransmuting) {
                transmutationProgress += 0.8;

                // Anel de varredura laser
                const scanY = (h * (transmutationProgress / 100));
                ctxChamber.strokeStyle = '#00ff73';
                ctxChamber.lineWidth = 4;
                ctxChamber.shadowColor = '#00ff73';
                ctxChamber.shadowBlur = 20;
                ctxChamber.beginPath();
                ctxChamber.moveTo(30, scanY);
                ctxChamber.lineTo(w - 30, scanY);
                ctxChamber.stroke();

                if (transmutationProgress >= 100) {
                    isTransmuting = false;
                    addLog(`>>> TRANSMUTAÇÃO DA POSIÇÃO ${current.rank} CONCLUÍDA COM SUCESSO! <<<`, 'success');
                }
            }
        }

        // -------------------------------------------------------------
        // ENGINE GRÁFICO - CANVAS 2: ESPAÇO-TEMPO & FENDA DE BRANAS
        // -------------------------------------------------------------
        function drawSpaceCanvas() {
            const w = canvasSpace.width;
            const h = canvasSpace.height;
            const current = TRANSFORMATION_SCALE[currentRankIndex];

            ctxSpace.clearRect(0, 0, w, h);
            ctxSpace.fillStyle = '#010309';
            ctxSpace.fillRect(0, 0, w, h);

            const time = Date.now() * 0.002;
            const gridSpacing = 20;
            const cols = Math.floor(w / gridSpacing) + 1;
            const rows = Math.floor(h / gridSpacing) + 1;

            ctxSpace.strokeStyle = 'rgba(0, 243, 255, 0.15)';
            ctxSpace.lineWidth = 1;

            // Distorção de malha com base no nível de interferência (1-100)
            const warpFactor = current.interference * 0.35;

            for (let r = 0; r < rows; r++) {
                ctxSpace.beginPath();
                for (let c = 0; c < cols; c++) {
                    const x = c * gridSpacing;
                    const y = r * gridSpacing;

                    // Cálculo da deformação gravitacional/dimensional no centro do canvas
                    const dx = x - (w / 2);
                    const dy = y - (h / 2);
                    const dist = Math.sqrt(dx * dx + dy * dy);
                    const offset = Math.sin((dist * 0.05) - time * 2) * (warpFactor * (100 / (dist + 20)));

                    const finalX = x + (dx / (dist + 1)) * offset;
                    const finalY = y + (dy / (dist + 1)) * offset;

                    if (c === 0) ctxSpace.moveTo(finalX, finalY);
                    else ctxSpace.lineTo(finalX, finalY);
                }
                ctxSpace.stroke();
            }

            // SE O NÍVEL FOR 7, 8 OU 9, DESENHA FENDAS MULTIVERSAIS E BRANAS
            if (current.rank >= 7) {
                ctxSpace.save();
                ctxSpace.translate(w / 2, h / 2);

                ctxSpace.strokeStyle = current.color;
                ctxSpace.lineWidth = 2;
                ctxSpace.shadowColor = current.color;
                ctxSpace.shadowBlur = 25;

                const branes = current.rank === 9 ? 7 : (current.rank === 8 ? 4 : 2);
                for (let b = 0; b < branes; b++) {
                    ctxSpace.rotate((Math.PI / branes) + time * 0.2);
                    ctxSpace.beginPath();
                    ctxSpace.moveTo(-w * 0.4, 0);
                    ctxSpace.bezierCurveTo(-w * 0.1, -40 * b, w * 0.1, 40 * b, w * 0.4, 0);
                    ctxSpace.stroke();
                }
                ctxSpace.restore();
            }
        }

        // ANIMATION LOOP
        function animate() {
            drawChamberCanvas();
            drawSpaceCanvas();
            requestAnimationFrame(animate);
        }

        // INICIALIZAÇÃO DO APP
        window.onload = () => {
            resizeCanvases();
            selectRank(0);
            animate();
        };
    </script>
</body>
</html>
