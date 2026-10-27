
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema Tático Acoustic Air-Gap - Zero Trust Comm</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Orbitron & JetBrains Mono -->
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;800&family=Orbitron:wght@500;700;900&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        hud: {
                            bg: '#060a0f',
                            panel: '#0d1520',
                            border: '#1b2a3a',
                            green: '#00ff66',
                            cyan: '#00e5ff',
                            amber: '#ffaa00',
                            red: '#ff2a55',
                            purple: '#a855f7'
                        }
                    },
                    fontFamily: {
                        orbitron: ['Orbitron', 'sans-serif'],
                        mono: ['JetBrains Mono', 'monospace']
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #04070a;
            color: #d1d5db;
            font-family: 'JetBrains Mono', monospace;
            overflow-x: hidden;
            user-select: none;
        }
        /* Custom Scanline Effect */
        .scanlines {
            background: linear-gradient(
                to bottom,
                rgba(255,255,255,0),
                rgba(255,255,255,0) 50%,
                rgba(0, 0, 0, 0.3) 50%,
                rgba(0, 0, 0, 0.3)
            );
            background-size: 100% 4px;
        }
        /* Glow Effects */
        .glow-green { text-shadow: 0 0 8px rgba(0, 255, 102, 0.6); }
        .glow-cyan { text-shadow: 0 0 8px rgba(0, 229, 255, 0.6); }
        .glow-red { text-shadow: 0 0 8px rgba(255, 42, 85, 0.6); }
        .border-glow { box-shadow: 0 0 15px rgba(0, 229, 255, 0.15); }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #060a0f; }
        ::-webkit-scrollbar-thumb { background: #1b2a3a; border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: #00e5ff; }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between scanlines text-xs md:text-sm">

    <!-- HEADER / HUD NAV -->
    <header class="border-b border-hud-border bg-hud-panel/90 px-4 py-3 backdrop-blur sticky top-0 z-50">
        <div class="max-w-7xl mx-auto flex flex-wrap justify-between items-center gap-3">
            <div class="flex items-center gap-3">
                <div class="w-3 h-3 rounded-full bg-hud-green animate-ping"></div>
                <div>
                    <h1 class="font-orbitron font-extrabold text-base md:text-xl text-hud-cyan tracking-wider flex items-center gap-2">
                        <i class="fa-solid font-black fa-shield-halved text-hud-green"></i> ZERO-TRUST AIR-GAP COMM
                    </h1>
                    <p class="text-[10px] text-gray-400">ARQUITETURA TÁTICA ISOLADA ACOUSTIC AIR-GAP & FIBRA ÓPTICA</p>
                </div>
            </div>

            <!-- STATUS INDICATORS TOP HEADER -->
            <div class="flex items-center gap-2 md:gap-6 flex-wrap">
                <div class="bg-hud-bg/80 px-3 py-1.5 rounded border border-hud-border flex items-center gap-2">
                    <span class="text-gray-400 text-[10px]">CYBER BARRIER:</span>
                    <span id="headerCyberStatus" class="text-hud-green font-bold text-xs flex items-center gap-1">
                        <i class="fa-solid fa-lock"></i> 100% AIR-GAPPED
                    </span>
                </div>
                <div class="bg-hud-bg/80 px-3 py-1.5 rounded border border-hud-border flex items-center gap-2">
                    <span class="text-gray-400 text-[10px]">LATÊNCIA FIM-A-FIM:</span>
                    <span id="headerLatency" class="text-hud-cyan font-bold font-mono">142 ms</span>
                </div>
                <div class="bg-hud-bg/80 px-3 py-1.5 rounded border border-hud-border flex items-center gap-2">
                    <span class="text-gray-400 text-[10px]">INTEGRIDADE:</span>
                    <span id="headerIntegrity" class="text-hud-green font-bold font-mono">99.8%</span>
                </div>
            </div>
        </div>
    </header>

    <!-- MAIN SYSTEM CONTENT -->
    <main class="max-w-7xl w-full mx-auto p-3 md:p-6 grid grid-cols-1 lg:grid-cols-12 gap-4 flex-grow">

        <!-- LEFT PANEL: CANVAS SIMULATOR & AUDIO VISUALIZER (8 COLS) -->
        <div class="lg:col-span-8 flex flex-col gap-4">

            <!-- MAIN TOPOLOGY CANVAS -->
            <div class="bg-hud-panel rounded-lg border border-hud-border p-3 flex flex-col gap-2 relative overflow-hidden border-glow">
                <div class="flex justify-between items-center px-2 py-1 border-b border-hud-border/50">
                    <span class="font-orbitron font-semibold text-hud-cyan text-xs flex items-center gap-2">
                        <i class="fa-solid fa-diagram-project"></i> ANIMAÇÃO TOPOLÓGICA DA CADEIA TÁTICA
                    </span>
                    <span class="text-[10px] text-gray-400 font-mono" id="canvasFrameRate">FPS: 60</span>
                </div>

                <!-- CANVAS CONTAINER -->
                <div class="relative w-full h-[320px] sm:h-[400px] bg-hud-bg rounded border border-hud-border/80 overflow-hidden">
                    <canvas id="topologyCanvas" class="w-full h-full block"></canvas>

                    <!-- FLOATING HUD OVERLAYS ON CANVAS -->
                    <div class="absolute top-2 left-2 bg-hud-panel/90 p-2 rounded border border-hud-border text-[10px] backdrop-blur max-w-[200px]">
                        <div class="text-hud-amber font-bold border-b border-gray-700 pb-1 mb-1">LEGENDA DO SISTEMA</div>
                        <div class="flex items-center gap-1.5 mb-1"><span class="w-2.5 h-2.5 rounded-full bg-hud-green inline-block"></span> Áudio Válido (Voz)</div>
                        <div class="flex items-center gap-1.5 mb-1"><span class="w-2.5 h-2.5 rounded-full bg-hud-red inline-block"></span> Jamming / Ruído Ext.</div>
                        <div class="flex items-center gap-1.5 mb-1"><span class="w-2.5 h-2.5 rounded-full bg-hud-cyan inline-block"></span> Dados Cripto Fibra</div>
                        <div class="flex items-center gap-1.5"><span class="w-2.5 h-2.5 rounded-full bg-hud-purple inline-block"></span> Telemetria Satélite/Fio</div>
                    </div>

                    <!-- THREAT WARNING OVERLAY -->
                    <div id="threatOverlay" class="hidden absolute inset-0 bg-hud-red/10 backdrop-blur-[1px] flex items-center justify-center pointer-events-none">
                        <div class="bg-hud-bg/95 border-2 border-hud-red p-4 rounded-lg text-center max-w-sm shadow-2xl animate-pulse">
                            <i class="fa-solid fa-triangle-exclamation text-hud-red text-4xl mb-2"></i>
                            <h3 id="threatTitle" class="font-orbitron font-bold text-hud-red text-base">AMEAÇA DETECTADA</h3>
                            <p id="threatDesc" class="text-xs text-gray-300 mt-1">Interferência de RF detectada na antena receptora HQ.</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- AUDIO FREQUENCY SPECTRUM / DSP REALTIME GRAPH -->
            <div class="bg-hud-panel rounded-lg border border-hud-border p-3 flex flex-col gap-2">
                <div class="flex justify-between items-center px-2 py-1 border-b border-hud-border/50">
                    <span class="font-orbitron font-semibold text-hud-green text-xs flex items-center gap-2">
                        <i class="fa-solid fa-wave-square"></i> PROCESSAMENTO DE ÁUDIO & ESPECTROGRAMA DSP
                    </span>
                    <div class="flex gap-4 text-[11px] font-mono">
                        <span id="dspNoiseStatus" class="text-hud-green"><i class="fa-solid fa-filter"></i> FILTRO IA: ATIVO</span>
                        <span id="dspGainStatus" class="text-hud-cyan">AGC: 1.2x</span>
                    </div>
                </div>
                <div class="w-full h-24 bg-hud-bg rounded border border-hud-border/80 overflow-hidden relative">
                    <canvas id="spectrumCanvas" class="w-full h-full block"></canvas>
                    <div class="absolute bottom-1 right-2 text-[9px] text-gray-500 font-mono">0 Hz - 8 kHz (DSP Realtime)</div>
                </div>
            </div>

        </div>

        <!-- RIGHT PANEL: CONTROLS & AI DIAGNOSTICS (4 COLS) -->
        <div class="lg:col-span-4 flex flex-col gap-4">

            <!-- CONTROL CENTER -->
            <div class="bg-hud-panel rounded-lg border border-hud-border p-4 flex flex-col gap-3">
                <h2 class="font-orbitron font-bold text-sm text-hud-cyan border-b border-hud-border pb-2 flex items-center gap-2">
                    <i class="fa-solid fa-sliders"></i> CONTROLES OPERACIONAIS
                </h2>

                <!-- TRANSMISSION TOGGLE -->
                <button id="btnToggleTransmit" class="w-full py-2.5 px-4 rounded font-orbitron font-bold text-xs bg-hud-green/20 text-hud-green border border-hud-green hover:bg-hud-green/30 transition flex items-center justify-center gap-2">
                    <i class="fa-solid fa-microphone"></i> INICIAR TRANSMISSÃO DE ÁUDIO
                </button>

                <!-- SIMULATION THREAT BUTTONS -->
                <div class="grid grid-cols-2 gap-2">
                    <button id="btnJamming" class="py-2 px-3 rounded font-mono text-[11px] bg-hud-red/20 text-hud-red border border-hud-red/60 hover:bg-hud-red/40 transition flex items-center justify-center gap-1.5">
                        <i class="fa-solid fa-radio"></i> SIMULAR JAMMING
                    </button>
                    <button id="btnSpoofing" class="py-2 px-3 rounded font-mono text-[11px] bg-hud-amber/20 text-hud-amber border border-hud-amber/60 hover:bg-hud-amber/40 transition flex items-center justify-center gap-1.5">
                        <i class="fa-solid fa-user-ninja"></i> VOZ FALSA (SPOOF)
                    </button>
                </div>

                <hr class="border-hud-border my-1">

                <!-- ARCHITECTURE SELECTORS -->
                <div class="flex flex-col gap-2">
                    <label class="text-[11px] text-gray-300 font-semibold flex justify-between">
                        <span>MEIO CENTRAL (CAIXA ➔ SERVIDOR):</span>
                        <span id="labelCentralLink" class="text-hud-cyan font-bold">FIBRA ÓPTICA</span>
                    </label>
                    <div class="grid grid-cols-2 gap-2">
                        <button id="btnLinkFiber" class="py-1.5 px-2 rounded text-[11px] font-mono border border-hud-cyan bg-hud-cyan/20 text-hud-cyan font-bold">
                            <i class="fa-solid fa-cable-car"></i> FIBRA ÓPTICA
                        </button>
                        <button id="btnLinkWifi" class="py-1.5 px-2 rounded text-[11px] font-mono border border-hud-border bg-hud-bg text-gray-400 hover:text-white">
                            <i class="fa-solid fa-wifi"></i> WI-FI (VULNERÁVEL)
                        </button>
                    </div>
                </div>

                <div class="flex flex-col gap-2 mt-1">
                    <label class="text-[11px] text-gray-300 font-semibold flex justify-between">
                        <span>LINK LINHA DE FRONTE:</span>
                        <span id="labelFrontLink" class="text-hud-purple font-bold">SATÉLITE LEO</span>
                    </label>
                    <div class="grid grid-cols-2 gap-2">
                        <button id="btnFrontSat" class="py-1.5 px-2 rounded text-[11px] font-mono border border-hud-purple bg-hud-purple/20 text-hud-purple font-bold">
                            <i class="fa-solid fa-satellite"></i> SATÉLITE LEO
                        </button>
                        <button id="btnFrontWire" class="py-1.5 px-2 rounded text-[11px] font-mono border border-hud-border bg-hud-bg text-gray-400 hover:text-white">
                            <i class="fa-solid fa-circle-nodes"></i> MICROFIO TÁTICO
                        </button>
                    </div>
                </div>

                <hr class="border-hud-border my-1">

                <!-- SLIDERS -->
                <div class="flex flex-col gap-2">
                    <div class="flex justify-between text-[10px]">
                        <span class="text-gray-400">GANHO DE ÁUDIO DA CAIXA (AGC):</span>
                        <span id="valGain" class="text-hud-cyan font-mono">1.2x</span>
                    </div>
                    <input type="range" id="sliderGain" min="0.5" max="3.0" step="0.1" value="1.2" class="w-full accent-hud-cyan bg-hud-bg h-1 rounded cursor-pointer">

                    <div class="flex justify-between text-[10px] mt-1">
                        <span class="text-gray-400">RUÍDO DE AMBIENTE / ESTÁTICA:</span>
                        <span id="valNoise" class="text-hud-amber font-mono">15%</span>
                    </div>
                    <input type="range" id="sliderNoise" min="0" max="100" value="15" class="w-full accent-hud-amber bg-hud-bg h-1 rounded cursor-pointer">
                </div>
            </div>

            <!-- AI ENGINE & BIOMETRICS PANEL -->
            <div class="bg-hud-panel rounded-lg border border-hud-border p-4 flex flex-col gap-3 flex-grow">
                <h2 class="font-orbitron font-bold text-sm text-hud-green border-b border-hud-border pb-2 flex items-center justify-between">
                    <span><i class="fa-solid fa-brain"></i> ENGINE DE IA CENTRAL</span>
                    <span class="text-[10px] font-mono text-hud-green bg-hud-green/10 px-2 py-0.5 rounded border border-hud-green/30">ONLINE</span>
                </h2>

                <div class="flex flex-col gap-2 text-[11px]">
                    <!-- BIOMETRIC VERIFICATION STATUS -->
                    <div class="bg-hud-bg/80 p-2.5 rounded border border-hud-border flex flex-col gap-1">
                        <div class="flex justify-between text-gray-400">
                            <span>BIOMETRIA VOCAL COMANDO:</span>
                            <span id="biometricScore" class="text-hud-green font-mono font-bold">98.4% MATCH</span>
                        </div>
                        <div class="w-full bg-hud-border h-2 rounded-full overflow-hidden">
                            <div id="biometricBar" class="bg-hud-green h-full w-[98%] transition-all duration-300"></div>
                        </div>
                        <div id="biometricStatusText" class="text-[10px] text-hud-green mt-0.5">
                            <i class="fa-solid fa-check-circle"></i> Assinatura vocal do Comandante Autenticada.
                        </div>
                    </div>

                    <!-- AIR GAP SECURITY STATUS -->
                    <div class="bg-hud-bg/80 p-2.5 rounded border border-hud-border flex flex-col gap-1">
                        <span class="text-gray-400">BARREIRA CIBERNÉTICA REVERSA:</span>
                        <div id="airgapStatus" class="text-hud-green font-bold flex items-center gap-1.5">
                            <i class="fa-solid fa-shield-virus"></i> ISOLAMENTO FISICO ATIVO (0 BIT PASS-THROUGH)
                        </div>
                        <p class="text-[10px] text-gray-500 mt-0.5">Malwares e vírus de RF não conseguem propagar pelo som da caixa abafadora.</p>
                    </div>

                    <!-- LOG EVENT CONTAINER -->
                    <div class="bg-hud-bg p-2 rounded border border-hud-border/80 flex flex-col gap-1">
                        <span class="text-[10px] text-gray-400 font-bold border-b border-gray-800 pb-1">LOG DE TELEMETRIA EM TEMPO REAL:</span>
                        <div id="logContainer" class="h-24 overflow-y-auto font-mono text-[10px] flex flex-col gap-1 text-gray-300 pr-1">
                            <div class="text-hud-cyan">[SYS] Inicializando protocolo Zero Trust...</div>
                            <div class="text-hud-green">[AIR-GAP] Conexão acústica selada e calibrada.</div>
                        </div>
                    </div>
                </div>
            </div>

        </div>

    </main>

    <!-- FOOTER INFO BAR -->
    <footer class="border-t border-hud-border bg-hud-panel px-4 py-2 text-[10px] text-gray-500 flex flex-wrap justify-between items-center max-w-7xl mx-auto w-full">
        <div class="flex gap-4">
            <span>SISTEMA TÁTICO MILITAR V4.8-ZERO-TRUST</span>
            <span class="hidden md:inline">|</span>
            <span class="hidden md:inline">CRIPTOGRAFIA: AES-GCM-256 (PQC-READY)</span>
        </div>
        <div>
             ARQUITETURA ACOUSTIC AIR-GAP ACOPLADA À FIBRA ÓPTICA
        </div>
    </footer>

    <!-- JAVASCRIPT LOGIC -->
    <script>
        // ==========================================
        // STATE MANAGEMENT
        // ==========================================
        const state = {
            transmitting: false,
            jamming: false,
            spoofing: false,
            centralLink: 'fiber', // 'fiber' or 'wifi'
            frontLink: 'sat', // 'sat' or 'wire'
            gain: 1.2,
            noise: 15,
            latency: 142, // ms
            integrity: 99.8, // %
            fps: 60,
            frameCounter: 0,
            lastFpsUpdate: performance.now(),
            audioTime: 0
        };

        // ==========================================
        // DOM ELEMENTS
        // ==========================================
        const elements = {
            topologyCanvas: document.getElementById('topologyCanvas'),
            spectrumCanvas: document.getElementById('spectrumCanvas'),
            btnToggleTransmit: document.getElementById('btnToggleTransmit'),
            btnJamming: document.getElementById('btnJamming'),
            btnSpoofing: document.getElementById('btnSpoofing'),
            btnLinkFiber: document.getElementById('btnLinkFiber'),
            btnLinkWifi: document.getElementById('btnLinkWifi'),
            btnFrontSat: document.getElementById('btnFrontSat'),
            btnFrontWire: document.getElementById('btnFrontWire'),
            sliderGain: document.getElementById('sliderGain'),
            sliderNoise: document.getElementById('sliderNoise'),
            valGain: document.getElementById('valGain'),
            valNoise: document.getElementById('valNoise'),
            labelCentralLink: document.getElementById('labelCentralLink'),
            labelFrontLink: document.getElementById('labelFrontLink'),
            logContainer: document.getElementById('logContainer'),
            threatOverlay: document.getElementById('threatOverlay'),
            threatTitle: document.getElementById('threatTitle'),
            threatDesc: document.getElementById('threatDesc'),
            biometricScore: document.getElementById('biometricScore'),
            biometricBar: document.getElementById('biometricBar'),
            biometricStatusText: document.getElementById('biometricStatusText'),
            headerCyberStatus: document.getElementById('headerCyberStatus'),
            headerLatency: document.getElementById('headerLatency'),
            headerIntegrity: document.getElementById('headerIntegrity'),
            canvasFrameRate: document.getElementById('canvasFrameRate'),
            dspNoiseStatus: document.getElementById('dspNoiseStatus'),
            dspGainStatus: document.getElementById('dspGainStatus')
        };

        const ctxTopo = elements.topologyCanvas.getContext('2d');
        const ctxSpec = elements.spectrumCanvas.getContext('2d');

        // ==========================================
        // LOGGING UTILITY
        // ==========================================
        function addLog(msg, type = 'info') {
            const time = new Date().toLocaleTimeString('pt-BR', { hour12: false, hour: '2-digit', minute: '2-digit', second: '2-digit' });
            const div = document.createElement('div');
            
            let colorClass = 'text-gray-300';
            if (type === 'warn') colorClass = 'text-hud-amber';
            if (type === 'danger') colorClass = 'text-hud-red';
            if (type === 'success') colorClass = 'text-hud-green';
            if (type === 'cyan') colorClass = 'text-hud-cyan';

            div.className = colorClass;
            div.innerHTML = `<span class="opacity-50">[${time}]</span> ${msg}`;
            
            elements.logContainer.appendChild(div);
            elements.logContainer.scrollTop = elements.logContainer.scrollHeight;
        }

        // ==========================================
        // AUDIO SYNTHESIS & SPECTRUM (WEB AUDIO API)
        // ==========================================
        let audioCtx = null;
        let oscillator = null;
        let noiseNode = null;
        let masterGain = null;

        function initAudio() {
            if (audioCtx) return;
            try {
                const AudioContext = window.AudioContext || window.webkitAudioContext;
                audioCtx = new AudioContext();
            } catch(e) {
                console.warn('Web Audio API not supported');
            }
        }

        // ==========================================
        // CANVAS RESIZING
        // ==========================================
        function resizeCanvases() {
            const rectTopo = elements.topologyCanvas.parentElement.getBoundingClientRect();
            elements.topologyCanvas.width = rectTopo.width;
            elements.topologyCanvas.height = rectTopo.height;

            const rectSpec = elements.spectrumCanvas.parentElement.getBoundingClientRect();
            elements.spectrumCanvas.width = rectSpec.width;
            elements.spectrumCanvas.height = rectSpec.height;
        }
        window.addEventListener('resize', resizeCanvases);
        resizeCanvases();

        // ==========================================
        // PARTICLE / PACKET SIMULATION FOR CANVAS
        // ==========================================
        const packets = [];

        function spawnPacket(sourceX, sourceY, targetX, targetY, color, speed = 2, type = 'normal') {
            packets.push({
                x: sourceX,
                y: sourceY,
                startX: sourceX,
                startY: sourceY,
                targetX: targetX,
                targetY: targetY,
                progress: 0,
                speed: speed,
                color: color,
                type: type
            });
        }

        // ==========================================
        // TOPOLOGY RENDER ENGINE (CANVAS 2D)
        // ==========================================
        function renderTopology(timestamp) {
            const width = elements.topologyCanvas.width;
            const height = elements.topologyCanvas.height;

            ctxTopo.clearRect(0, 0, width, height);

            // Grid background
            ctxTopo.strokeStyle = 'rgba(27, 42, 58, 0.4)';
            ctxTopo.lineWidth = 1;
            const gridSize = 30;
            for (let x = 0; x < width; x += gridSize) {
                ctxTopo.beginPath();
                ctxTopo.moveTo(x, 0);
                ctxTopo.lineTo(x, height);
                ctxTopo.stroke();
            }
            for (let y = 0; y < height; y += gridSize) {
                ctxTopo.beginPath();
                ctxTopo.moveTo(0, y);
                ctxTopo.lineTo(width, y);
                ctxTopo.stroke();
            }

            // Node Coordinates
            const nodes = {
                hqAntenna: { x: width * 0.12, y: height * 0.35, label: "Antena HQ (DF)" },
                radioChamber: { x: width * 0.30, y: height * 0.35, label: "Rádio HQ" },
                micChamber: { x: width * 0.42, y: height * 0.35, label: "Celular Captação" },
                serverAI: { x: width * 0.65, y: height * 0.35, label: "Servidor / IA Core" },
                frontNode: { x: width * 0.88, y: height * 0.35, label: state.frontLink === 'sat' ? "Satélite LEO" : "Hub Microfios" },
                soldiers: [
                    { x: width * 0.82, y: height * 0.75, label: "Agente Delta 1" },
                    { x: width * 0.94, y: height * 0.75, label: "Agente Delta 2" }
                ]
            };

            // Chamber Box Bounds
            const boxX = width * 0.25;
            const boxY = height * 0.18;
            const boxW = width * 0.23;
            const boxH = height * 0.42;

            // 1. DRAW ACOUSTIC AIR-GAP CHAMBER
            ctxTopo.fillStyle = 'rgba(13, 21, 32, 0.85)';
            ctxTopo.strokeStyle = '#00e5ff';
            ctxTopo.lineWidth = 2;
            ctxTopo.fillRect(boxX, boxY, boxW, boxH);
            ctxTopo.strokeRect(boxX, boxY, boxW, boxH);

            // Chamber Acoustic Padding Lines
            ctxTopo.strokeStyle = 'rgba(0, 229, 255, 0.2)';
            ctxTopo.lineWidth = 1;
            for (let i = boxX + 5; i < boxX + boxW; i += 12) {
                ctxTopo.beginPath();
                ctxTopo.moveTo(i, boxY);
                ctxTopo.lineTo(i, boxY + 8);
                ctxTopo.stroke();
                ctxTopo.beginPath();
                ctxTopo.moveTo(i, boxY + boxH);
                ctxTopo.lineTo(i, boxY + boxH - 8);
                ctxTopo.stroke();
            }

            // Label for Chamber
            ctxTopo.fillStyle = '#00e5ff';
            ctxTopo.font = '10px Orbitron';
            ctxTopo.textAlign = 'center';
            ctxTopo.fillText("CAIXA ABAFADORA (AIR-GAP ACOUSTIC)", boxX + boxW / 2, boxY - 8);

            // 2. CONNECTING LINES / LINKS
            // Link 1: Antenna to Radio
            ctxTopo.beginPath();
            ctxTopo.moveTo(nodes.hqAntenna.x, nodes.hqAntenna.y);
            ctxTopo.lineTo(nodes.radioChamber.x, nodes.radioChamber.y);
            ctxTopo.strokeStyle = state.jamming ? '#ff2a55' : '#00ff66';
            ctxTopo.lineWidth = state.jamming ? 3 : 2;
            if (state.jamming) ctxTopo.setLineDash([4, 4]);
            ctxTopo.stroke();
            ctxTopo.setLineDash([]);

            // Link 2: Air-Gap Sound Waves inside box
            if (state.transmitting) {
                const waveCenterX = (nodes.radioChamber.x + nodes.micChamber.x) / 2;
                const radius = (timestamp * 0.08) % 35;
                ctxTopo.strokeStyle = state.spoofing ? '#ffaa00' : 'rgba(0, 255, 102, 0.6)';
                ctxTopo.lineWidth = 2;
                ctxTopo.beginPath();
                ctxTopo.arc(nodes.radioChamber.x + 15, nodes.radioChamber.y, radius, -Math.PI / 3, Math.PI / 3);
                ctxTopo.stroke();
            }

            // Link 3: Cellphone to Server (Fiber vs Wi-Fi)
            ctxTopo.beginPath();
            ctxTopo.moveTo(nodes.micChamber.x, nodes.micChamber.y);
            ctxTopo.lineTo(nodes.serverAI.x, nodes.serverAI.y);
            
            if (state.centralLink === 'fiber') {
                ctxTopo.strokeStyle = '#00e5ff'; // Cyan for Fiber
                ctxTopo.lineWidth = 3;
                ctxTopo.stroke();
                // Pass-through Seal Marker
                ctxTopo.fillStyle = '#00e5ff';
                ctxTopo.fillRect(boxX + boxW - 3, nodes.micChamber.y - 6, 6, 12);
            } else {
                // Wi-Fi (Suscetível)
                ctxTopo.strokeStyle = state.jamming ? '#ff2a55' : '#ffaa00';
                ctxTopo.lineWidth = 2;
                ctxTopo.setLineDash([6, 6]);
                ctxTopo.stroke();
                ctxTopo.setLineDash([]);
            }

            // Link 4: Server to Frontline Node
            ctxTopo.beginPath();
            ctxTopo.moveTo(nodes.serverAI.x, nodes.serverAI.y);
            ctxTopo.lineTo(nodes.frontNode.x, nodes.frontNode.y);
            ctxTopo.strokeStyle = '#a855f7';
            ctxTopo.lineWidth = 2;
            ctxTopo.stroke();

            // Link 5: Frontline Node to Soldiers
            nodes.soldiers.forEach(s => {
                ctxTopo.beginPath();
                ctxTopo.moveTo(nodes.frontNode.x, nodes.frontNode.y);
                ctxTopo.lineTo(s.x, s.y);
                ctxTopo.strokeStyle = state.frontLink === 'sat' ? '#a855f7' : '#00ff66';
                ctxTopo.lineWidth = 1.5;
                if (state.frontLink === 'sat') ctxTopo.setLineDash([3, 3]);
                ctxTopo.stroke();
                ctxTopo.setLineDash([]);
            });

            // 3. DRAW NODES (ICONS & SHAPES)
            // Helper for Node Styling
            function drawNode(node, iconClass, color, size = 18) {
                ctxTopo.fillStyle = '#0d1520';
                ctxTopo.strokeStyle = color;
                ctxTopo.lineWidth = 2;
                ctxTopo.beginPath();
                ctxTopo.arc(node.x, node.y, size, 0, Math.PI * 2);
                ctxTopo.fill();
                ctxTopo.stroke();

                ctxTopo.fillStyle = color;
                ctxTopo.font = '10px Orbitron';
                ctxTopo.textAlign = 'center';
                ctxTopo.fillText(node.label, node.x, node.y + size + 14);
            }

            drawNode(nodes.hqAntenna, '', state.jamming ? '#ff2a55' : '#00ff66');
            drawNode(nodes.radioChamber, '', '#00ff66');
            drawNode(nodes.micChamber, '', '#00e5ff');
            drawNode(nodes.serverAI, '', state.spoofing ? '#ffaa00' : '#00ff66', 22);
            drawNode(nodes.frontNode, '', '#a855f7');
            nodes.soldiers.forEach(s => drawNode(s, '', '#00ff66', 14));

            // Icons inside nodes
            ctxTopo.fillStyle = '#ffffff';
            ctxTopo.font = '12px "Font Awesome 6 Free"';
            ctxTopo.fontWeight = '900';
            ctxTopo.textAlign = 'center';
            ctxTopo.textBaseline = 'middle';

            ctxTopo.fillText('\uf1eb', nodes.hqAntenna.x, nodes.hqAntenna.y); // Antenna
            ctxTopo.fillText('\uf8d1', nodes.radioChamber.x, nodes.radioChamber.y); // Radio
            ctxTopo.fillText('\uf3cd', nodes.micChamber.x, nodes.micChamber.y); // Mobile
            ctxTopo.fillText('\uf5dc', nodes.serverAI.x, nodes.serverAI.y); // Brain/AI
            ctxTopo.fillText(state.frontLink === 'sat' ? '\uf7c0' : '\uf6ff', nodes.frontNode.x, nodes.frontNode.y); // Sat or Wire
            
            nodes.soldiers.forEach(s => {
                ctxTopo.fillText('\uf501', s.x, s.y); // Soldier/User
            });

            // 4. ANIMATED PACKET SPAWNING & UPDATING
            if (state.transmitting && Math.random() < 0.3) {
                // Spawn wave from antenna to radio
                const packetColor = state.jamming ? '#ff2a55' : (state.spoofing ? '#ffaa00' : '#00ff66');
                spawnPacket(nodes.hqAntenna.x, nodes.hqAntenna.y, nodes.radioChamber.x, nodes.radioChamber.y, packetColor, 0.02);
            }

            // Process Packets
            for (let i = packets.length - 1; i >= 0; i--) {
                const p = packets[i];
                p.progress += p.speed;

                p.x = p.startX + (p.targetX - p.startX) * p.progress;
                p.y = p.startY + (p.targetY - p.startY) * p.progress;

                ctxTopo.fillStyle = p.color;
                ctxTopo.beginPath();
                ctxTopo.arc(p.x, p.y, 4, 0, Math.PI * 2);
                ctxTopo.fill();

                // Chain Progression
                if (p.progress >= 1) {
                    if (p.startX === nodes.hqAntenna.x && !state.jamming) {
                        // Forward from Mic to Server via Fiber/Wifi
                        if (state.centralLink === 'fiber' || !state.jamming) {
                            spawnPacket(nodes.micChamber.x, nodes.micChamber.y, nodes.serverAI.x, nodes.serverAI.y, '#00e5ff', 0.025);
                        }
                    } else if (p.startX === nodes.micChamber.x && !state.spoofing) {
                        // Forward from Server to Frontline Node
                        spawnPacket(nodes.serverAI.x, nodes.serverAI.y, nodes.frontNode.x, nodes.frontNode.y, '#a855f7', 0.03);
                    } else if (p.startX === nodes.serverAI.x) {
                        // Forward to Soldiers
                        nodes.soldiers.forEach(s => {
                            spawnPacket(nodes.frontNode.x, nodes.frontNode.y, s.x, s.y, '#00ff66', 0.035);
                        });
                    }
                    packets.splice(i, 1);
                }
            }
        }

        // ==========================================
        // REALTIME SPECTRUM RENDER ENGINE
        // ==========================================
        function renderSpectrum() {
            const width = elements.spectrumCanvas.width;
            const height = elements.spectrumCanvas.height;

            ctxSpec.clearRect(0, 0, width, height);

            // Draw Base Spectrum Grid
            ctxSpec.strokeStyle = 'rgba(27, 42, 58, 0.5)';
            ctxSpec.lineWidth = 1;
            ctxSpec.beginPath();
            ctxSpec.moveTo(0, height / 2);
            ctxSpec.lineTo(width, height / 2);
            ctxSpec.stroke();

            if (!state.transmitting) {
                // Flatline baseline
                ctxSpec.strokeStyle = '#1b2a3a';
                ctxSpec.beginPath();
                ctxSpec.moveTo(0, height / 2);
                ctxSpec.lineTo(width, height / 2);
                ctxSpec.stroke();
                return;
            }

            state.audioTime += 0.05;
            ctxSpec.beginPath();
            ctxSpec.lineWidth = 2;

            if (state.jamming) {
                ctxSpec.strokeStyle = '#ff2a55'; // Red chaotic wave
            } else if (state.spoofing) {
                ctxSpec.strokeStyle = '#ffaa00'; // Amber warning wave
            } else {
                ctxSpec.strokeStyle = '#00ff66'; // Green pristine wave
            }

            const numPoints = width;
            for (let x = 0; x < numPoints; x += 2) {
                let y = height / 2;

                if (state.jamming) {
                    // Random noise overlay
                    y += (Math.random() - 0.5) * (state.noise * 0.8);
                } else if (state.spoofing) {
                    // Synthetic unnatural harmonic curves
                    y += Math.sin(x * 0.05 + state.audioTime) * 15 * state.gain;
                    y += Math.cos(x * 0.1) * 10;
                } else {
                    // Clean voice formants simulation
                    y += Math.sin(x * 0.03 + state.audioTime) * 18 * state.gain;
                    y += Math.sin(x * 0.08 - state.audioTime * 2) * 8 * state.gain;
                    y += (Math.random() - 0.5) * (state.noise * 0.15);
                }

                if (x === 0) ctxSpec.moveTo(x, y);
                else ctxSpec.lineTo(x, y);
            }
            ctxSpec.stroke();
        }

        // ==========================================
        // MAIN ANIMATION LOOP
        // ==========================================
        function animate(timestamp) {
            // FPS calculation
            state.frameCounter++;
            if (timestamp - state.lastFpsUpdate >= 1000) {
                state.fps = state.frameCounter;
                state.frameCounter = 0;
                state.lastFpsUpdate = timestamp;
                elements.canvasFrameRate.innerText = `FPS: ${state.fps}`;
            }

            renderTopology(timestamp);
            renderSpectrum();

            requestAnimationFrame(animate);
        }

        // ==========================================
        // STATE RECALCULATION & UI UPDATE
        // ==========================================
        function updateSystemState() {
            // Calculate Latency & Integrity
            let lat = 120; // base ms
            let integrity = 99.8;

            if (state.centralLink === 'wifi') lat += 35;
            if (state.frontLink === 'sat') lat += 25;
            if (state.frontLink === 'wire') lat += 5;

            if (state.jamming) {
                integrity -= 45.0;
                if (state.centralLink === 'wifi') integrity -= 40.0; // Central Wi-Fi collapses under jamming
            }

            if (state.spoofing) {
                integrity -= 15.0;
            }

            state.latency = lat;
            state.integrity = Math.max(0, integrity).toFixed(1);

            // Update UI Counters
            elements.headerLatency.innerText = `${state.latency} ms`;
            elements.headerIntegrity.innerText = `${state.integrity}%`;

            // Cyber status update
            if (state.centralLink === 'wifi' && state.jamming) {
                elements.headerCyberStatus.className = 'text-hud-red font-bold text-xs flex items-center gap-1';
                elements.headerCyberStatus.innerHTML = '<i class="fa-solid fa-triangle-exclamation"></i> LINK CENTRAL CAÍDO (WI-FI JAMMED)';
            } else {
                elements.headerCyberStatus.className = 'text-hud-green font-bold text-xs flex items-center gap-1';
                elements.headerCyberStatus.innerHTML = '<i class="fa-solid fa-lock"></i> 100% AIR-GAPPED (IMUNE)';
            }

            // Biometric AI UI Updates
            if (state.spoofing) {
                elements.biometricScore.innerText = '12.1% MATCH (SPOOF DETECTED)';
                elements.biometricScore.className = 'text-hud-red font-mono font-bold';
                elements.biometricBar.className = 'bg-hud-red h-full w-[12%] transition-all duration-300';
                elements.biometricStatusText.className = 'text-[10px] text-hud-red mt-0.5';
                elements.biometricStatusText.innerHTML = '<i class="fa-solid fa-ban"></i> FRAUDE VOCAL DETECTADA: Transmissão bloqueada!';
            } else if (state.jamming && state.centralLink === 'wifi') {
                elements.biometricScore.innerText = '0.0% SINAL PERDIDO';
                elements.biometricScore.className = 'text-hud-amber font-mono font-bold';
                elements.biometricBar.className = 'bg-hud-amber h-full w-[0%] transition-all duration-300';
                elements.biometricStatusText.className = 'text-[10px] text-hud-amber mt-0.5';
                elements.biometricStatusText.innerHTML = '<i class="fa-solid fa-wave-square"></i> Ruído excessivo / Jamming RF na recepção Wi-Fi.';
            } else {
                elements.biometricScore.innerText = '98.4% MATCH';
                elements.biometricScore.className = 'text-hud-green font-mono font-bold';
                elements.biometricBar.className = 'bg-hud-green h-full w-[98%] transition-all duration-300';
                elements.biometricStatusText.className = 'text-[10px] text-hud-green mt-0.5';
                elements.biometricStatusText.innerHTML = '<i class="fa-solid fa-check-circle"></i> Assinatura vocal do Comandante Autenticada.';
            }

            // Threat Overlay
            if (state.jamming || state.spoofing) {
                elements.threatOverlay.classList.remove('hidden');
                if (state.jamming) {
                    elements.threatTitle.innerText = "ATAQUE DE JAMMING DE RF";
                    elements.threatDesc.innerText = state.centralLink === 'fiber' 
                        ? "Bloqueio de rádio externo detectado. A Central de Fibra e a Caixa Air-Gap continuam 100% imunes ao ataque cibernético!" 
                        : "Sinal Wi-Fi suscetível e rádio foram derrubados! Mude o meio central para FIBRA ÓPTICA para imunidade total.";
                } else {
                    elements.threatTitle.innerText = "TENTATIVA DE SPOOFING DE VOZ";
                    elements.threatDesc.innerText = "A IA de Biometria Vocal detectou clonagem sintética e bloqueou os pacotes antes da distribuição para a Linha de Fronte.";
                }
            } else {
                elements.threatOverlay.classList.add('hidden');
            }
        }

        // ==========================================
        // EVENT HANDLERS & INTERACTIVITY
        // ==========================================
        elements.btnToggleTransmit.addEventListener('click', () => {
            initAudio();
            state.transmitting = !state.transmitting;
            if (state.transmitting) {
                elements.btnToggleTransmit.className = 'w-full py-2.5 px-4 rounded font-orbitron font-bold text-xs bg-hud-green text-hud-bg border border-hud-green shadow-lg transition flex items-center justify-center gap-2';
                elements.btnToggleTransmit.innerHTML = '<i class="fa-solid fa-stop"></i> PARAR TRANSMISSÃO';
                addLog('Transmissão de voz iniciada do Alto Escalão.', 'success');
            } else {
                elements.btnToggleTransmit.className = 'w-full py-2.5 px-4 rounded font-orbitron font-bold text-xs bg-hud-green/20 text-hud-green border border-hud-green hover:bg-hud-green/30 transition flex items-center justify-center gap-2';
                elements.btnToggleTransmit.innerHTML = '<i class="fa-solid fa-microphone"></i> INICIAR TRANSMISSÃO DE ÁUDIO';
                addLog('Transmissão encerrada.', 'info');
            }
            updateSystemState();
        });

        elements.btnJamming.addEventListener('click', () => {
            state.jamming = !state.jamming;
            if (state.jamming) state.spoofing = false; // Mutually exclusive for clear demo
            
            elements.btnJamming.className = state.jamming 
                ? 'py-2 px-3 rounded font-mono text-[11px] bg-hud-red text-white border border-hud-red font-bold flex items-center justify-center gap-1.5'
                : 'py-2 px-3 rounded font-mono text-[11px] bg-hud-red/20 text-hud-red border border-hud-red/60 hover:bg-hud-red/40 transition flex items-center justify-center gap-1.5';

            elements.btnSpoofing.className = 'py-2 px-3 rounded font-mono text-[11px] bg-hud-amber/20 text-hud-amber border border-hud-amber/60 hover:bg-hud-amber/40 transition flex items-center justify-center gap-1.5';

            if (state.jamming) {
                addLog('ALERTA: Interferência pesada de RF (Jamming) injetada na freq. de comando!', 'danger');
            } else {
                addLog('Interferência de RF cessada. Frequência limpa.', 'success');
            }
            updateSystemState();
        });

        elements.btnSpoofing.addEventListener('click', () => {
            state.spoofing = !state.spoofing;
            if (state.spoofing) state.jamming = false;

            elements.btnSpoofing.className = state.spoofing
                ? 'py-2 px-3 rounded font-mono text-[11px] bg-hud-amber text-black border border-hud-amber font-bold flex items-center justify-center gap-1.5'
                : 'py-2 px-3 rounded font-mono text-[11px] bg-hud-amber/20 text-hud-amber border border-hud-amber/60 hover:bg-hud-amber/40 transition flex items-center justify-center gap-1.5';

            elements.btnJamming.className = 'py-2 px-3 rounded font-mono text-[11px] bg-hud-red/20 text-hud-red border border-hud-red/60 hover:bg-hud-red/40 transition flex items-center justify-center gap-1.5';

            if (state.spoofing) {
                addLog('ALERTA: Tentativa de Injeção de Áudio Falso (Deepfake Voice Spoof) detectada!', 'warn');
            } else {
                addLog('Transmissão retornou à voz biométrica válida.', 'success');
            }
            updateSystemState();
        });

        // Link Switchers
        elements.btnLinkFiber.addEventListener('click', () => {
            state.centralLink = 'fiber';
            elements.btnLinkFiber.className = 'py-1.5 px-2 rounded text-[11px] font-mono border border-hud-cyan bg-hud-cyan/20 text-hud-cyan font-bold';
            elements.btnLinkWifi.className = 'py-1.5 px-2 rounded text-[11px] font-mono border border-hud-border bg-hud-bg text-gray-400 hover:text-white';
            elements.labelCentralLink.innerText = 'FIBRA ÓPTICA';
            elements.labelCentralLink.className = 'text-hud-cyan font-bold';
            addLog('Link Central alternado para FIBRA ÓPTICA (100% Imune Eletromagneticamente).', 'cyan');
            updateSystemState();
        });

        elements.btnLinkWifi.addEventListener('click', () => {
            state.centralLink = 'wifi';
            elements.btnLinkWifi.className = 'py-1.5 px-2 rounded text-[11px] font-mono border border-hud-amber bg-hud-amber/20 text-hud-amber font-bold';
            elements.btnLinkFiber.className = 'py-1.5 px-2 rounded text-[11px] font-mono border border-hud-border bg-hud-bg text-gray-400 hover:text-white';
            elements.labelCentralLink.innerText = 'WI-FI (SUSCETÍVEL)';
            elements.labelCentralLink.className = 'text-hud-amber font-bold';
            addLog('AVISO: Link Central alternado para WI-FI. Suscetível a ataques de Deauth e Jamming!', 'warn');
            updateSystemState();
        });

        elements.btnFrontSat.addEventListener('click', () => {
            state.frontLink = 'sat';
            elements.btnFrontSat.className = 'py-1.5 px-2 rounded text-[11px] font-mono border border-hud-purple bg-hud-purple/20 text-hud-purple font-bold';
            elements.btnFrontWire.className = 'py-1.5 px-2 rounded text-[11px] font-mono border border-hud-border bg-hud-bg text-gray-400 hover:text-white';
            elements.labelFrontLink.innerText = 'SATÉLITE LEO';
            elements.labelFrontLink.className = 'text-hud-purple font-bold';
            addLog('Distribuição para linha de fronte alterada para Redundância Satelital LEO.', 'info');
            updateSystemState();
        });

        elements.btnFrontWire.addEventListener('click', () => {
            state.frontLink = 'wire';
            elements.btnFrontWire.className = 'py-1.5 px-2 rounded text-[11px] font-mono border border-hud-green bg-hud-green/20 text-hud-green font-bold';
            elements.btnFrontSat.className = 'py-1.5 px-2 rounded text-[11px] font-mono border border-hud-border bg-hud-bg text-gray-400 hover:text-white';
            elements.labelFrontLink.innerText = 'MICROFIO TÁTICO';
            elements.labelFrontLink.className = 'text-hud-green font-bold';
            addLog('Distribuição de campo alterada para Microfios Táticos Ultra-resistentes.', 'info');
            updateSystemState();
        });

        // Sliders
        elements.sliderGain.addEventListener('input', (e) => {
            state.gain = parseFloat(e.target.value);
            elements.valGain.innerText = `${state.gain.toFixed(1)}x`;
            elements.dspGainStatus.innerText = `AGC: ${state.gain.toFixed(1)}x`;
        });

        elements.sliderNoise.addEventListener('input', (e) => {
            state.noise = parseInt(e.target.value);
            elements.valNoise.innerText = `${state.noise}%`;
        });

        // ==========================================
        // INITIALIZATION ON WINDOW LOAD
        // ==========================================
        window.onload = function() {
            resizeCanvases();
            updateSystemState();
            requestAnimationFrame(animate);
            addLog('Sistema Acoustic Air-Gap inicializado com sucesso.', 'success');
        };
    </script>
</body>
</html>
