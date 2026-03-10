<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AskData // Neural Analytics Interface</title>
    
    <!-- Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;600;700;800;900&family=Rajdhani:wght@300;400;500;600;700&family=Share+Tech+Mono&display=swap" rel="stylesheet">
    
    <!-- Tailwind -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Three.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <!-- SheetJS -->
    <script src="https://cdn.sheetjs.com/xlsx-0.20.1/package/dist/xlsx.full.min.js"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        orbitron: ['Orbitron', 'sans-serif'],
                        rajdhani: ['Rajdhani', 'sans-serif'],
                        mono: ['Share Tech Mono', 'monospace'],
                    },
                    colors: {
                        neon: {
                            cyan: '#00f3ff',
                            pink: '#ff00ff',
                            purple: '#b026ff',
                            green: '#39ff14',
                            yellow: '#ffff00',
                            orange: '#ff6600',
                        },
                        cyber: {
                            dark: '#0a0a0f',
                            darker: '#050508',
                            panel: '#0f0f1a',
                            border: '#1a1a2e',
                        }
                    },
                    animation: {
                        'pulse-fast': 'pulse 1s cubic-bezier(0.4, 0, 0.6, 1) infinite',
                        'spin-slow': 'spin 8s linear infinite',
                        'glitch': 'glitch 1s linear infinite',
                        'scan': 'scan 4s linear infinite',
                        'float': 'float 6s ease-in-out infinite',
                    },
                    keyframes: {
                        glitch: {
                            '0%, 100%': { transform: 'translate(0)' },
                            '20%': { transform: 'translate(-2px, 2px)' },
                            '40%': { transform: 'translate(-2px, -2px)' },
                            '60%': { transform: 'translate(2px, 2px)' },
                            '80%': { transform: 'translate(2px, -2px)' },
                        },
                        scan: {
                            '0%': { transform: 'translateY(-100%)' },
                            '100%': { transform: 'translateY(100%)' },
                        },
                        float: {
                            '0%, 100%': { transform: 'translateY(0px)' },
                            '50%': { transform: 'translateY(-20px)' },
                        }
                    }
                }
            }
        }
    </script>
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background: #050508;
            color: #e0e0e0;
            font-family: 'Rajdhani', sans-serif;
            overflow: hidden;
        }
        
        /* CRT Scanline Effect */
        .crt::before {
            content: " ";
            display: block;
            position: absolute;
            top: 0;
            left: 0;
            bottom: 0;
            right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), 
                        linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
            background-size: 100% 2px, 3px 100%;
            pointer-events: none;
            z-index: 1000;
        }
        
        /* Neon Glow Effects */
        .neon-text-cyan {
            color: #00f3ff;
            text-shadow: 0 0 10px #00f3ff, 0 0 20px #00f3ff, 0 0 40px #00f3ff;
        }
        
        .neon-text-pink {
            color: #ff00ff;
            text-shadow: 0 0 10px #ff00ff, 0 0 20px #ff00ff, 0 0 40px #ff00ff;
        }
        
        .neon-border-cyan {
            border: 1px solid #00f3ff;
            box-shadow: 0 0 10px rgba(0, 243, 255, 0.5), inset 0 0 10px rgba(0, 243, 255, 0.1);
        }
        
        .neon-border-pink {
            border: 1px solid #ff00ff;
            box-shadow: 0 0 10px rgba(255, 0, 255, 0.5), inset 0 0 10px rgba(255, 0, 255, 0.1);
        }
        
        /* Glassmorphism with Cyberpunk twist */
        .cyber-glass {
            background: rgba(15, 15, 26, 0.7);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(0, 243, 255, 0.3);
            box-shadow: 0 0 20px rgba(0, 243, 255, 0.2);
        }
        
        .cyber-glass-pink {
            background: rgba(15, 15, 26, 0.7);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 0, 255, 0.3);
            box-shadow: 0 0 20px rgba(255, 0, 255, 0.2);
        }
        
        /* Animated Grid Background */
        .grid-bg {
            background-image: 
                linear-gradient(rgba(0, 243, 255, 0.1) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 243, 255, 0.1) 1px, transparent 1px);
            background-size: 50px 50px;
            animation: gridMove 20s linear infinite;
        }
        
        @keyframes gridMove {
            0% { transform: perspective(500px) rotateX(60deg) translateY(0); }
            100% { transform: perspective(500px) rotateX(60deg) translateY(50px); }
        }
        
        /* Hexagon Pattern */
        .hex-bg {
            background-color: #050508;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='28' height='49' viewBox='0 0 28 49'%3E%3Cg fill-rule='evenodd'%3E%3Cg fill='%2300f3ff' fill-opacity='0.05'%3E%3Cpath d='M13.99 9.25l13 7.5v15l-13 7.5L1 31.75v-15l12.99-7.5zM3 17.9v12.7l10.99 6.34 11-6.35V17.9l-11-6.34L3 17.9zM0 15l12.98-7.5V0h-2v6.35L0 12.69v2.3zm0 18.5L12.98 41v8h-2v-6.85L0 35.81v-2.3zM15 0v7.5L27.99 15H28v-2.31h-.01L17 6.35V0h-2zm0 49v-7.5L27.99 34H28v2.31h-.01L17 42.65V49h-2z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
        }
        
        /* Loading Animation */
        .loader-ring {
            display: inline-block;
            width: 80px;
            height: 80px;
        }
        
        .loader-ring:after {
            content: " ";
            display: block;
            width: 64px;
            height: 64px;
            margin: 8px;
            border-radius: 50%;
            border: 6px solid #00f3ff;
            border-color: #00f3ff transparent #00f3ff transparent;
            animation: ring 1.2s linear infinite;
            box-shadow: 0 0 20px #00f3ff;
        }
        
        @keyframes ring {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        /* Terminal Cursor */
        .cursor-blink::after {
            content: '█';
            animation: blink 1s step-end infinite;
            color: #00f3ff;
        }
        
        @keyframes blink {
            50% { opacity: 0; }
        }
        
        /* Data Stream Effect */
        .data-stream {
            font-family: 'Share Tech Mono', monospace;
            color: #39ff14;
            font-size: 12px;
            line-height: 1.4;
            opacity: 0.6;
        }
        
        /* Holographic Card */
        .holo-card {
            background: linear-gradient(135deg, rgba(0, 243, 255, 0.1) 0%, rgba(255, 0, 255, 0.1) 100%);
            border: 1px solid rgba(0, 243, 255, 0.5);
            position: relative;
            overflow: hidden;
        }
        
        .holo-card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(
                45deg,
                transparent 30%,
                rgba(0, 243, 255, 0.1) 50%,
                transparent 70%
            );
            animation: holoShine 3s ease-in-out infinite;
        }
        
        @keyframes holoShine {
            0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
            100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
        }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        
        ::-webkit-scrollbar-track {
            background: #0a0a0f;
        }
        
        ::-webkit-scrollbar-thumb {
            background: #00f3ff;
            border-radius: 3px;
            box-shadow: 0 0 10px #00f3ff;
        }
        
        /* Button Effects */
        .cyber-btn {
            position: relative;
            overflow: hidden;
            transition: all 0.3s;
            clip-path: polygon(
                0 0,
                calc(100% - 10px) 0,
                100% 10px,
                100% 100%,
                10px 100%,
                0 calc(100% - 10px)
            );
        }
        
        .cyber-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
            transition: left 0.5s;
        }
        
        .cyber-btn:hover::before {
            left: 100%;
        }
        
        /* Glitch Text */
        .glitch {
            position: relative;
        }
        
        .glitch::before,
        .glitch::after {
            content: attr(data-text);
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        
        .glitch::before {
            left: 2px;
            text-shadow: -2px 0 #ff00ff;
            clip: rect(24px, 550px, 90px, 0);
            animation: glitch-anim 2s infinite linear alternate-reverse;
        }
        
        .glitch::after {
            left: -2px;
            text-shadow: -2px 0 #00f3ff;
            clip: rect(85px, 550px, 140px, 0);
            animation: glitch-anim 2s infinite linear alternate-reverse;
        }
        
        @keyframes glitch-anim {
            0% { clip: rect(10px, 9999px, 85px, 0); }
            20% { clip: rect(63px, 9999px, 130px, 0); }
            40% { clip: rect(25px, 9999px, 5px, 0); }
            60% { clip: rect(76px, 9999px, 100px, 0); }
            80% { clip: rect(15px, 9999px, 170px, 0); }
            100% { clip: rect(50px, 9999px, 90px, 0); }
        }
        
        /* Scanning Line */
        .scan-line {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 2px;
            background: #00f3ff;
            box-shadow: 0 0 10px #00f3ff, 0 0 20px #00f3ff;
            animation: scan 3s linear infinite;
            z-index: 100;
        }
        
        /* Dashboard Panel Slide */
        .dashboard-panel {
            transform: translateX(100%);
            transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .dashboard-panel.active {
            transform: translateX(0);
        }
        
        /* Chart Container */
        .chart-container {
            position: relative;
            height: 300px;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(0, 243, 255, 0.3);
        }
        
        /* 3D Canvas */
        #canvas-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
        }
        
        /* UI Layer */
        #ui-layer {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 10;
            pointer-events: none;
        }
        
        #ui-layer > * {
            pointer-events: auto;
        }
    </style>
</head>
<body class="crt hex-bg">

    <!-- Boot Sequence Overlay -->
    <div id="boot-sequence" class="fixed inset-0 bg-black z-[9999] flex flex-col items-center justify-center font-mono text-green-400 p-8">
        <div id="boot-text" class="text-sm md:text-base max-w-2xl w-full"></div>
        <div class="mt-8 w-64 h-2 bg-gray-800 rounded overflow-hidden">
            <div id="boot-progress" class="h-full bg-green-400 w-0 transition-all duration-100"></div>
        </div>
    </div>

    <!-- 3D Canvas -->
    <div id="canvas-container">
        <canvas id="three-canvas"></canvas>
    </div>

    <!-- UI Layer -->
    <div id="ui-layer">
        
        <!-- Top HUD -->
        <header class="absolute top-0 left-0 right-0 p-4 md:p-6 flex items-center justify-between">
            <!-- Logo -->
            <div class="cyber-glass rounded-lg px-6 py-3 flex items-center gap-4">
                <div class="relative">
                    <div class="w-12 h-12 bg-gradient-to-br from-neon-cyan to-neon-purple rounded flex items-center justify-center font-orbitron font-bold text-xl text-black animate-pulse-fast">
                        A
                    </div>
                    <div class="absolute -inset-1 bg-gradient-to-r from-neon-cyan to-neon-purple rounded blur opacity-50"></div>
                </div>
                <div>
                    <h1 class="font-orbitron font-bold text-xl tracking-wider glitch" data-text="ASKDATA_3D">ASKDATA_3D</h1>
                    <p class="font-mono text-xs text-neon-cyan tracking-widest">NEURAL_ANALYTICS_INTERFACE v2.0</p>
                </div>
            </div>

            <!-- Status Indicators -->
            <div class="hidden md:flex items-center gap-4">
                <div class="cyber-glass rounded-lg px-4 py-2 flex items-center gap-3">
                    <div class="flex items-center gap-2">
                        <span class="w-2 h-2 bg-neon-green rounded-full animate-pulse"></span>
                        <span class="font-mono text-xs text-gray-400">SYSTEM</span>
                    </div>
                    <span class="font-mono text-sm text-neon-green">ONLINE</span>
                </div>
                
                <div id="data-status" class="cyber-glass-pink rounded-lg px-4 py-2 flex items-center gap-3 hidden">
                    <div class="flex items-center gap-2">
                        <span class="w-2 h-2 bg-neon-pink rounded-full animate-pulse"></span>
                        <span class="font-mono text-xs text-gray-400">DATA_STREAM</span>
                    </div>
                    <span class="font-mono text-sm text-neon-pink" id="data-info">NO_SIGNAL</span>
                </div>

                <button onclick="toggleDashboard()" class="cyber-btn bg-gradient-to-r from-neon-cyan/20 to-neon-purple/20 hover:from-neon-cyan/40 hover:to-neon-purple/40 text-neon-cyan font-orbitron text-sm px-6 py-3 rounded border border-neon-cyan/50">
                    [ DASHBOARD ]
                </button>
            </div>
        </header>

        <!-- Left Control Panel -->
        <div class="absolute top-32 left-4 md:left-8 w-64 md:w-80 space-y-4">
            
            <!-- Neural Link Status -->
            <div class="cyber-glass rounded-lg p-4">
                <div class="flex items-center justify-between mb-3">
                    <span class="font-orbitron text-xs text-neon-cyan tracking-wider">NEURAL_LINK</span>
                    <span class="w-2 h-2 bg-neon-green rounded-full animate-pulse"></span>
                </div>
                <div class="space-y-2 font-mono text-xs">
                    <div class="flex justify-between">
                        <span class="text-gray-500">BANDWIDTH</span>
                        <span class="text-neon-cyan">12.4 TB/s</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-gray-500">LATENCY</span>
                        <span class="text-neon-cyan">0.3 ms</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-gray-500">NODES</span>
                        <span class="text-neon-cyan">2,847</span>
                    </div>
                </div>
                <div class="mt-3 h-1 bg-gray-800 rounded overflow-hidden">
                    <div class="h-full bg-gradient-to-r from-neon-cyan to-neon-purple w-3/4 animate-pulse"></div>
                </div>
            </div>

            <!-- Quick Commands -->
            <div class="cyber-glass rounded-lg p-4">
                <h3 class="font-orbitron text-xs text-neon-cyan tracking-wider mb-3">QUICK_COMMANDS</h3>
                <div class="space-y-2">
                    <button onclick="quickAsk('Revenue analysis by sector')" class="cyber-btn w-full text-left p-3 bg-black/30 hover:bg-neon-cyan/10 border border-neon-cyan/30 rounded font-mono text-xs text-neon-cyan transition-all group">
                        <span class="group-hover:animate-pulse">></span> REVENUE_SECTOR_ANALYSIS
                    </button>
                    <button onclick="quickAsk('Customer churn prediction')" class="cyber-btn w-full text-left p-3 bg-black/30 hover:bg-neon-pink/10 border border-neon-pink/30 rounded font-mono text-xs text-neon-pink transition-all group">
                        <span class="group-hover:animate-pulse">></span> CHURN_PREDICTION_MODEL
                    </button>
                    <button onclick="quickAsk('Inventory optimization')" class="cyber-btn w-full text-left p-3 bg-black/30 hover:bg-neon-green/10 border border-neon-green/30 rounded font-mono text-xs text-neon-green transition-all group">
                        <span class="group-hover:animate-pulse">></span> INVENTORY_NEURAL_OPT
                    </button>
                    <button onclick="quickAsk('Market trend forecast')" class="cyber-btn w-full text-left p-3 bg-black/30 hover:bg-neon-yellow/10 border border-neon-yellow/30 rounded font-mono text-xs text-neon-yellow transition-all group">
                        <span class="group-hover:animate-pulse">></span> TREND_FORECAST_2024
                    </button>
                </div>
            </div>

            <!-- Data Upload -->
            <div class="cyber-glass-pink rounded-lg p-4">
                <h3 class="font-orbitron text-xs text-neon-pink tracking-wider mb-3">DATA_INJECTION</h3>
                <label class="block p-4 border-2 border-dashed border-neon-pink/30 rounded-lg hover:border-neon-pink hover:bg-neon-pink/5 cursor-pointer transition-all text-center group">
                    <div class="text-3xl mb-2 group-hover:scale-110 transition-transform">📡</div>
                    <p class="font-mono text-xs text-neon-pink">UPLOAD_DATA_STREAM</p>
                    <p class="text-xs text-gray-500 mt-1">CSV | XLSX | JSON</p>
                    <input type="file" id="fileInput" class="hidden" accept=".csv,.xlsx,.xls,.json">
                </label>
                <div class="mt-3 flex gap-2">
                    <button onclick="loadSample('sales')" class="flex-1 py-2 bg-neon-cyan/10 hover:bg-neon-cyan/20 border border-neon-cyan/30 rounded font-mono text-xs text-neon-cyan">SALES_DEMO</button>
                    <button onclick="loadSample('customers')" class="flex-1 py-2 bg-neon-pink/10 hover:bg-neon-pink/20 border border-neon-pink/30 rounded font-mono text-xs text-neon-pink">CUST_DEMO</button>
                </div>
            </div>

            <!-- Live Data Stream -->
            <div class="cyber-glass rounded-lg p-3 h-32 overflow-hidden">
                <h3 class="font-orbitron text-xs text-neon-cyan tracking-wider mb-2">LIVE_STREAM</h3>
                <div id="data-stream" class="data-stream h-full overflow-hidden"></div>
            </div>
        </div>

        <!-- Center Reticle -->
        <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 pointer-events-none">
            <div class="relative w-64 h-64 opacity-30">
                <div class="absolute inset-0 border border-neon-cyan/30 rounded-full animate-spin-slow"></div>
                <div class="absolute inset-4 border border-neon-pink/30 rounded-full animate-spin-slow" style="animation-direction: reverse;"></div>
                <div class="absolute inset-8 border border-neon-cyan/20 rounded-full"></div>
                <div class="absolute top-1/2 left-0 w-full h-px bg-neon-cyan/30"></div>
                <div class="absolute top-0 left-1/2 w-px h-full bg-neon-cyan/30"></div>
            </div>
        </div>

        <!-- Bottom Command Line -->
        <div class="absolute bottom-8 left-1/2 transform -translate-x-1/2 w-full max-w-3xl px-4">
            <div class="cyber-glass rounded-lg p-1">
                <div class="flex items-end gap-2 bg-black/50 rounded p-3">
                    <span class="font-mono text-neon-cyan text-lg">></span>
                    <textarea id="userInput" rows="1" placeholder="ENTER_NEURAL_QUERY..." 
                        class="flex-1 bg-transparent border-none resize-none max-h-32 py-1 focus:outline-none font-mono text-neon-cyan placeholder-neon-cyan/30"
                        onkeydown="handleKeyDown(event)"></textarea>
                    <button onclick="sendMessage()" class="cyber-btn bg-neon-cyan/20 hover:bg-neon-cyan/40 text-neon-cyan font-orbitron px-6 py-2 rounded border border-neon-cyan">
                        EXECUTE
                    </button>
                </div>
                <div class="flex items-center justify-between px-3 py-2 text-xs font-mono text-gray-500">
                    <span>STATUS: <span class="text-neon-green">READY</span></span>
                    <span>MODE: <span class="text-neon-cyan">NEURAL_ANALYTICS</span></span>
                    <span>v2.0.4_BUILD_892</span>
                </div>
            </div>
        </div>

        <!-- Right Info Panel -->
        <div class="absolute top-32 right-4 md:right-8 w-48 space-y-4 hidden md:block">
            <div class="cyber-glass rounded-lg p-3 text-center">
                <p class="font-orbitron text-2xl text-neon-cyan" id="clock">00:00:00</p>
                <p class="font-mono text-xs text-gray-500" id="date">YYYY-MM-DD</p>
            </div>
            
            <div class="cyber-glass-pink rounded-lg p-3">
                <p class="font-orbitron text-xs text-neon-pink mb-2">CPU_LOAD</p>
                <div class="space-y-1">
                    <div class="flex justify-between font-mono text-xs">
                        <span class="text-gray-400">CORE_01</span>
                        <span class="text-neon-pink">34%</span>
                    </div>
                    <div class="h-1 bg-gray-800 rounded">
                        <div class="h-full bg-neon-pink w-[34%] animate-pulse"></div>
                    </div>
                    <div class="flex justify-between font-mono text-xs">
                        <span class="text-gray-400">CORE_02</span>
                        <span class="text-neon-pink">67%</span>
                    </div>
                    <div class="h-1 bg-gray-800 rounded">
                        <div class="h-full bg-neon-pink w-[67%]"></div>
                    </div>
                </div>
            </div>

            <div class="cyber-glass rounded-lg p-3">
                <p class="font-orbitron text-xs text-neon-cyan mb-2">MEMORY</p>
                <div class="relative h-20 bg-gray-900 rounded overflow-hidden">
                    <div class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-neon-cyan/30 to-transparent h-3/4 animate-pulse"></div>
                    <div class="absolute inset-0 flex items-center justify-center font-mono text-lg text-neon-cyan">
                        12.4 TB
                    </div>
                </div>
            </div>
        </div>

        <!-- Toast Container -->
        <div id="toastContainer" class="fixed top-24 right-4 z-50 space-y-2"></div>
    </div>

    <!-- Dashboard Panel (Holographic) -->
    <div id="dashboard-panel" class="dashboard-panel fixed top-0 right-0 w-full md:w-[700px] h-full bg-cyber-darker/95 backdrop-blur-xl border-l border-neon-cyan/30 z-50 flex flex-col">
        <div class="scan-line"></div>
        
        <!-- Panel Header -->
        <div class="p-6 border-b border-neon-cyan/30 flex items-center justify-between bg-gradient-to-r from-neon-cyan/5 to-transparent">
            <div>
                <h2 class="font-orbitron text-2xl text-neon-cyan tracking-wider glitch" data-text="ANALYSIS_DASHBOARD">ANALYSIS_DASHBOARD</h2>
                <p class="font-mono text-xs text-neon-pink mt-1" id="dashboard-subtitle">AWAITING_DATA_STREAM...</p>
            </div>
            <div class="flex items-center gap-3">
                <button onclick="exportDashboard()" class="cyber-btn bg-neon-cyan/10 hover:bg-neon-cyan/20 text-neon-cyan font-mono text-xs px-4 py-2 rounded border border-neon-cyan/50">
                    [EXPORT]
                </button>
                <button onclick="toggleDashboard()" class="cyber-btn bg-neon-pink/10 hover:bg-neon-pink/20 text-neon-pink font-mono text-xs px-4 py-2 rounded border border-neon-pink/50">
                    [CLOSE]
                </button>
            </div>
        </div>

        <!-- Panel Content -->
        <div class="flex-1 overflow-y-auto p-6 space-y-6 font-mono">
            
            <!-- Empty State -->
            <div id="dashboard-empty" class="h-full flex flex-col items-center justify-center text-center p-8 border border-neon-cyan/20 rounded-lg bg-neon-cyan/5">
                <div class="w-32 h-32 border-2 border-neon-cyan rounded-full flex items-center justify-center mb-6 relative">
                    <div class="absolute inset-0 border border-neon-cyan rounded-full animate-ping"></div>
                    <span class="text-5xl animate-pulse">📊</span>
                </div>
                <h3 class="font-orbitron text-xl text-neon-cyan mb-2">NO_ACTIVE_ANALYSIS</h3>
                <p class="text-gray-400 text-sm mb-6 max-w-md">Initialize neural query sequence or inject data stream to begin holographic analysis visualization.</p>
                
                <div class="grid grid-cols-2 gap-3 w-full max-w-sm">
                    <button onclick="quickAsk('Revenue by region Q3')" class="cyber-btn p-3 bg-neon-cyan/5 hover:bg-neon-cyan/10 border border-neon-cyan/30 rounded font-mono text-xs text-neon-cyan">
                        Q3_REVENUE_SECTORS
                    </button>
                    <button onclick="quickAsk('Customer lifetime value')" class="cyber-btn p-3 bg-neon-pink/5 hover:bg-neon-pink/10 border border-neon-pink/30 rounded font-mono text-xs text-neon-pink">
                        CLV_PREDICTION
                    </button>
                    <button onclick="quickAsk('Inventory turnover')" class="cyber-btn p-3 bg-neon-green/5 hover:bg-neon-green/10 border border-neon-green/30 rounded font-mono text-xs text-neon-green">
                        INVENTORY_CYCLES
                    </button>
                    <button onclick="quickAsk('Growth forecast 2024')" class="cyber-btn p-3 bg-neon-yellow/5 hover:bg-neon-yellow/10 border border-neon-yellow/30 rounded font-mono text-xs text-neon-yellow">
                        GROWTH_FORECAST
                    </button>
                </div>
            </div>

            <!-- Metrics Grid -->
            <div id="metrics-grid" class="grid grid-cols-2 gap-4 hidden"></div>

            <!-- Charts -->
            <div id="charts-container" class="space-y-4 hidden"></div>

            <!-- Data Table -->
            <div id="data-table" class="hidden border border-neon-cyan/30 rounded-lg overflow-hidden bg-black/30">
                <div class="p-4 border-b border-neon-cyan/30 flex items-center justify-between bg-neon-cyan/5">
                    <h4 class="font-orbitron text-neon-cyan">RAW_DATA_STREAM</h4>
                    <button onclick="downloadData()" class="font-mono text-xs text-neon-cyan hover:text-white">[DOWNLOAD_CSV]</button>
                </div>
                <div class="overflow-x-auto max-h-64">
                    <table class="w-full text-xs">
                        <thead class="bg-neon-cyan/10 text-neon-cyan font-mono" id="table-head"></thead>
                        <tbody class="divide-y divide-neon-cyan/10 font-mono text-gray-300" id="table-body"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- 2D Fallback Mode -->
    <div id="fallback-2d" class="hidden fixed inset-0 bg-cyber-darker z-[9998] overflow-auto font-mono">
        <div class="min-h-screen p-8">
            <header class="max-w-6xl mx-auto mb-8 flex items-center justify-between border-b border-neon-cyan/30 pb-4">
                <div class="flex items-center gap-4">
                    <div class="w-12 h-12 bg-neon-cyan rounded flex items-center justify-center font-orbitron font-bold text-black text-xl">A</div>
                    <div>
                        <h1 class="font-orbitron text-2xl text-neon-cyan">ASKDATA_2D</h1>
                        <p class="text-xs text-gray-500">FALLBACK_MODE // NEURAL_LINK_OFFLINE</p>
                    </div>
                </div>
                <button onclick="location.reload()" class="cyber-btn bg-neon-cyan/20 text-neon-cyan px-4 py-2 rounded border border-neon-cyan">
                    RETRY_3D_LINK
                </button>
            </header>
            
            <main class="max-w-6xl mx-auto">
                <div class="cyber-glass rounded-lg p-8 mb-6">
                    <h2 class="font-orbitron text-xl text-neon-cyan mb-4">DATA_INJECTION</h2>
                    <div class="flex gap-4 mb-6">
                        <button onclick="loadSample('sales')" class="cyber-btn flex-1 py-3 bg-neon-cyan/10 border border-neon-cyan/30 rounded text-neon-cyan">LOAD_SALES_MATRIX</button>
                        <button onclick="loadSample('customers')" class="cyber-btn flex-1 py-3 bg-neon-pink/10 border border-neon-pink/30 rounded text-neon-pink">LOAD_CUSTOMER_MATRIX</button>
                    </div>
                    <div class="flex gap-2">
                        <input type="text" id="fallback-input" placeholder="ENTER_QUERY_STRING..." class="flex-1 bg-black/50 border border-neon-cyan/30 rounded px-4 py-3 text-neon-cyan font-mono focus:outline-none focus:border-neon-cyan">
                        <button onclick="fallbackQuery()" class="cyber-btn px-6 py-3 bg-neon-cyan/20 border border-neon-cyan rounded text-neon-cyan font-orbitron">EXECUTE</button>
                    </div>
                </div>
                <div id="fallback-results" class="space-y-4"></div>
            </main>
        </div>
    </div>

    <script>
        // ==========================================
        // BOOT SEQUENCE
        // ==========================================
        const bootText = [
            "INITIALIZING_NEURAL_INTERFACE...",
            "LOADING_QUANTUM_PROCESSORS... [OK]",
            "ESTABLISHING_HOLOGRID_CONNECTION... [OK]",
            "CALIBRATING_3D_RENDERING_ENGINE... [OK]",
            "SYNCING_WITH_DATA_STREAM... [OK]",
            "MOUNTING_CYBERNETIC_UI... [OK]",
            "ASKDATA_3D v2.0 READY_FOR_NEURAL_INPUT"
        ];

        async function runBootSequence() {
            const container = document.getElementById('boot-text');
            const progress = document.getElementById('boot-progress');
            
            for (let i = 0; i < bootText.length; i++) {
                const line = document.createElement('div');
                line.className = 'mb-1';
                line.textContent = bootText[i];
                container.appendChild(line);
                
                progress.style.width = `${((i + 1) / bootText.length) * 100}%`;
                
                await new Promise(r => setTimeout(r, 400));
            }
            
            await new Promise(r => setTimeout(r, 500));
            document.getElementById('boot-sequence').style.opacity = '0';
            setTimeout(() => {
                document.getElementById('boot-sequence').style.display = 'none';
                initThreeJS();
            }, 500);
        }

        // ==========================================
        // THREE.JS FUTURISTIC SCENE
        // ==========================================
        let scene, camera, renderer, controls;
        let floatingOrbs = [];
        let particleSystem, gridHelper;
        let raycaster, mouse;
        let currentData = null;
        let currentFileName = '';
        let isProcessing = false;
        let charts = [];

        function initThreeJS() {
            const canvas = document.getElementById('three-canvas');
            
            // Scene with fog
            scene = new THREE.Scene();
            scene.fog = new THREE.FogExp2(0x050508, 0.02);
            
            // Camera
            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(0, 2, 12);
            
            // Renderer
            renderer = new THREE.WebGLRenderer({ 
                canvas, 
                antialias: true, 
                alpha: true,
                powerPreference: "high-performance"
            });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
            
            // Lighting - Cyberpunk style
            const ambientLight = new THREE.AmbientLight(0x404040, 0.5);
            scene.add(ambientLight);
            
            // Cyan light from left
            const light1 = new THREE.PointLight(0x00f3ff, 2, 100);
            light1.position.set(-10, 10, 10);
            scene.add(light1);
            
            // Pink light from right
            const light2 = new THREE.PointLight(0xff00ff, 2, 100);
            light2.position.set(10, -10, 10);
            scene.add(light2);
            
            // Purple light from below
            const light3 = new THREE.PointLight(0xb026ff, 1.5, 100);
            light3.position.set(0, -10, 5);
            scene.add(light3);
            
            // Animated grid floor
            createHoloGrid();
            
            // Floating data crystals
            createDataCrystals();
            
            // Particle data stream
            createDataParticles();
            
            // Raycaster
            raycaster = new THREE.Raycaster();
            mouse = new THREE.Vector2();
            
            // Events
            window.addEventListener('resize', onWindowResize);
            canvas.addEventListener('mousemove', onMouseMove);
            canvas.addEventListener('click', onMouseClick);
            
            // Mouse controls
            setupMouseControls();
            
            // Start animation
            animate();
            
            // Update clock
            setInterval(updateClock, 1000);
            updateClock();
            
            // Start data stream
            startDataStream();
        }

        function createHoloGrid() {
            // Main grid
            const gridHelper = new THREE.GridHelper(100, 100, 0x00f3ff, 0x00f3ff);
            gridHelper.position.y = -4;
            gridHelper.material.opacity = 0.2;
            gridHelper.material.transparent = true;
            scene.add(gridHelper);
            
            // Secondary grid (rotated)
            const grid2 = new THREE.GridHelper(60, 60, 0xff00ff, 0xff00ff);
            grid2.position.y = -4;
            grid2.rotation.x = Math.PI / 2;
            grid2.material.opacity = 0.1;
            grid2.material.transparent = true;
            scene.add(grid2);
        }

        function createDataCrystals() {
            const crystalData = [
                { color: 0x00f3ff, pos: [-6, 1, -3], label: 'REVENUE', icon: '💎' },
                { color: 0xff00ff, pos: [6, 0, -4], label: 'CUSTOMERS', icon: '👤' },
                { color: 0x39ff14, pos: [-4, -2, -2], label: 'INVENTORY', icon: '📦' },
                { color: 0xffff00, pos: [4, -1, -3], label: 'ANALYTICS', icon: '📊' },
                { color: 0xff6600, pos: [0, 3, -5], label: 'FORECAST', icon: '🔮' }
            ];
            
            crystalData.forEach((data, i) => {
                // Main crystal - Icosahedron for futuristic look
                const geometry = new THREE.IcosahedronGeometry(0.8, 0);
                const material = new THREE.MeshPhysicalMaterial({
                    color: data.color,
                    metalness: 0.9,
                    roughness: 0.1,
                    transmission: 0.6,
                    thickness: 1,
                    emissive: data.color,
                    emissiveIntensity: 0.4,
                    clearcoat: 1
                });
                
                const crystal = new THREE.Mesh(geometry, material);
                crystal.position.set(...data.pos);
                crystal.userData = { ...data, id: i, originalY: data.pos[1] };
                
                // Outer wireframe glow
                const wireGeo = new THREE.IcosahedronGeometry(1.2, 0);
                const wireMat = new THREE.MeshBasicMaterial({
                    color: data.color,
                    wireframe: true,
                    transparent: true,
                    opacity: 0.3
                });
                const wireframe = new THREE.Mesh(wireGeo, wireMat);
                crystal.add(wireframe);
                
                // Rotating ring
                const ringGeo = new THREE.TorusGeometry(1.5, 0.02, 16, 100);
                const ringMat = new THREE.MeshBasicMaterial({ 
                    color: data.color, 
                    transparent: true, 
                    opacity: 0.6 
                });
                const ring = new THREE.Mesh(ringGeo, ringMat);
                ring.rotation.x = Math.PI / 2;
                crystal.add(ring);
                
                // Vertical light beam
                const beamGeo = new THREE.CylinderGeometry(0.05, 0.05, 8, 8);
                const beamMat = new THREE.MeshBasicMaterial({
                    color: data.color,
                    transparent: true,
                    opacity: 0.2
                });
                const beam = new THREE.Mesh(beamGeo, beamMat);
                beam.position.y = -4;
                crystal.add(beam);
                
                scene.add(crystal);
                floatingOrbs.push(crystal);
            });
        }

        function createDataParticles() {
            const geometry = new THREE.BufferGeometry();
            const count = 1000;
            const positions = new Float32Array(count * 3);
            const colors = new Float32Array(count * 3);
            
            for (let i = 0; i < count; i++) {
                positions[i * 3] = (Math.random() - 0.5) * 50;
                positions[i * 3 + 1] = (Math.random() - 0.5) * 30;
                positions[i * 3 + 2] = (Math.random() - 0.5) * 50;
                
                // Cyan and pink particles
                const isCyan = Math.random() > 0.5;
                colors[i * 3] = isCyan ? 0 : 1;
                colors[i * 3 + 1] = isCyan ? 0.95 : 0;
                colors[i * 3 + 2] = isCyan ? 1 : 1;
            }
            
            geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
            geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3));
            
            const material = new THREE.PointsMaterial({
                size: 0.1,
                vertexColors: true,
                transparent: true,
                opacity: 0.8,
                blending: THREE.AdditiveBlending
            });
            
            particleSystem = new THREE.Points(geometry, material);
            scene.add(particleSystem);
        }

        function setupMouseControls() {
            const canvas = document.getElementById('three-canvas');
            let isDragging = false;
            let previousMousePosition = { x: 0, y: 0 };
            
            canvas.addEventListener('mousedown', (e) => {
                isDragging = true;
                previousMousePosition = { x: e.clientX, y: e.clientY };
            });
            
            canvas.addEventListener('mouseup', () => isDragging = false);
            
            canvas.addEventListener('mousemove', (e) => {
                if (isDragging) {
                    const deltaMove = {
                        x: e.clientX - previousMousePosition.x,
                        y: e.clientY - previousMousePosition.y
                    };
                    
                    scene.rotation.y += deltaMove.x * 0.005;
                    scene.rotation.x += deltaMove.y * 0.005;
                    
                    previousMousePosition = { x: e.clientX, y: e.clientY };
                }
            });
            
            canvas.addEventListener('wheel', (e) => {
                camera.position.z += e.deltaY * 0.01;
                camera.position.z = Math.max(5, Math.min(20, camera.position.z));
            });
        }

        function animate() {
            requestAnimationFrame(animate);
            
            const time = Date.now() * 0.001;
            
            // Animate crystals
            floatingOrbs.forEach((crystal, i) => {
                crystal.rotation.x += 0.01;
                crystal.rotation.y += 0.02;
                crystal.position.y = crystal.userData.originalY + Math.sin(time + i) * 0.5;
                
                // Pulse emissive
                crystal.material.emissiveIntensity = 0.4 + Math.sin(time * 2 + i) * 0.2;
                
                // Rotate ring
                crystal.children[1].rotation.z += 0.02;
                crystal.children[1].rotation.x = Math.PI / 2 + Math.sin(time) * 0.2;
            });
            
            // Animate particles
            if (particleSystem) {
                particleSystem.rotation.y += 0.001;
                const positions = particleSystem.geometry.attributes.position.array;
                for (let i = 0; i < positions.length; i += 3) {
                    positions[i + 1] += Math.sin(time + positions[i]) * 0.01;
                }
                particleSystem.geometry.attributes.position.needsUpdate = true;
            }
            
            // Gentle camera drift
            camera.position.x = Math.sin(time * 0.2) * 0.5;
            
            renderer.render(scene, camera);
        }

        function onWindowResize() {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        }

        function onMouseMove(event) {
            mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
            mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
            
            raycaster.setFromCamera(mouse, camera);
            const intersects = raycaster.intersectObjects(floatingOrbs);
            
            floatingOrbs.forEach(orb => {
                orb.material.emissiveIntensity = 0.4;
                orb.scale.setScalar(1);
            });
            
            if (intersects.length > 0) {
                const orb = intersects[0].object;
                orb.material.emissiveIntensity = 0.8;
                orb.scale.setScalar(1.2);
                document.body.style.cursor = 'pointer';
            } else {
                document.body.style.cursor = 'default';
            }
        }

        function onMouseClick(event) {
            raycaster.setFromCamera(mouse, camera);
            const intersects = raycaster.intersectObjects(floatingOrbs);
            
            if (intersects.length > 0) {
                const orb = intersects[0].object;
                showToast(`ACCESSING: ${orb.userData.label}`, 'info');
                
                switch(orb.userData.label) {
                    case 'REVENUE': quickAsk('Revenue analysis by sector'); break;
                    case 'CUSTOMERS': quickAsk('Customer churn prediction'); break;
                    case 'INVENTORY': quickAsk('Inventory optimization'); break;
                    case 'ANALYTICS': quickAsk('Market trend forecast'); break;
                    case 'FORECAST': quickAsk('Growth forecast 2024'); break;
                }
            }
        }

        // ==========================================
        // UI & DATA FUNCTIONS
        // ==========================================
        function updateClock() {
            const now = new Date();
            document.getElementById('clock').textContent = now.toLocaleTimeString('en-US', { hour12: false });
            document.getElementById('date').textContent = now.toISOString().split('T')[0];
        }

        function startDataStream() {
            const stream = document.getElementById('data-stream');
            const lines = [
                'PACKET_8923: 0x4F2A... OK',
                'SYNC_NODE_47: HANDSHAKE_COMPLETE',
                'BUFFER_ALLOC: 12.4TB FREE',
                'NEURAL_WEIGHT: 0.8473',
                'LATENCY_CHK: 0.3ms AVG',
                'ENCRYPTION: AES-4096 ACTIVE',
                'STREAM_HASH: a7f3...9d2e',
                'QUANTUM_BIT: STABLE',
                'CORE_TEMP: 34.2°C NOMINAL'
            ];
            
            setInterval(() => {
                const line = document.createElement('div');
                line.textContent = `> ${lines[Math.floor(Math.random() * lines.length)]}`;
                line.style.color = Math.random() > 0.8 ? '#ff00ff' : '#39ff14';
                stream.appendChild(line);
                if (stream.children.length > 8) stream.removeChild(stream.firstChild);
                stream.scrollTop = stream.scrollHeight;
            }, 800);
        }

        function handleFileUpload(e) {
            const file = e.target.files[0];
            if (!file) return;

            showToast(`INJECTING: ${file.name}`, 'info');
            
            const reader = new FileReader();
            reader.onload = (event) => {
                try {
                    let data;
                    if (file.name.endsWith('.csv')) {
                        data = parseCSV(event.target.result);
                    } else if (file.name.endsWith('.json')) {
                        data = JSON.parse(event.target.result);
                        if (!Array.isArray(data)) data = [data];
                    } else if (file.name.match(/\.xlsx?$/)) {
                        const workbook = XLSX.read(event.target.result, {type: 'binary'});
                        const firstSheet = workbook.Sheets[workbook.SheetNames[0]];
                        data = XLSX.utils.sheet_to_json(firstSheet);
                    }
                    
                    if (data && data.length > 0) {
                        loadData(data, file.name);
                        showToast(`DATA_INJECTED: ${data.length} RECORDS`, 'success');
                    }
                } catch (err) {
                    showToast(`ERROR: ${err.message}`, 'error');
                }
            };
            
            if (file.name.match(/\.xlsx?$/)) {
                reader.readAsBinaryString(file);
            } else {
                reader.readAsText(file);
            }
        }

        function parseCSV(text) {
            const lines = text.trim().split('\n');
            if (lines.length < 2) return [];
            
            const headers = lines[0].split(',').map(h => h.trim().replace(/^"|"$/g, ''));
            const data = [];
            
            for (let i = 1; i < lines.length; i++) {
                if (!lines[i].trim()) continue;
                const values = lines[i].split(',').map(v => v.trim().replace(/^"|"$/g, ''));
                const obj = {};
                headers.forEach((h, idx) => {
                    let val = values[idx] || '';
                    if (!isNaN(val) && val !== '') val = Number(val);
                    obj[h] = val;
                });
                data.push(obj);
            }
            return data;
        }

        function loadData(data, filename) {
            currentData = data;
            currentFileName = filename;
            
            document.getElementById('data-status').classList.remove('hidden');
            document.getElementById('data-info').textContent = `${data.length} RECORDS`;
            document.getElementById('dashboard-subtitle').textContent = `ANALYZING: ${filename.toUpperCase()}`;
        }

        function loadSample(type) {
            let data;
            switch(type) {
                case 'sales':
                    data = generateSalesData();
                    break;
                case 'customers':
                    data = generateCustomerData();
                    break;
            }
            loadData(data, `${type}_demo.csv`);
        }

        function generateSalesData() {
            const regions = ['N_AMERICA', 'EUROPE', 'ASIA_PAC', 'LATAM'];
            const months = ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN'];
            const data = [];
            for (let i = 0; i < 200; i++) {
                data.push({
                    MONTH: months[Math.floor(Math.random() * months.length)],
                    REGION: regions[Math.floor(Math.random() * regions.length)],
                    REVENUE: Math.floor(Math.random() * 50000) + 10000,
                    UNITS: Math.floor(Math.random() * 1000) + 100,
                    GROWTH: Number((Math.random() * 40 - 10).toFixed(1))
                });
            }
            return data;
        }

        function generateCustomerData() {
            const segments = ['ENTERPRISE', 'SMB', 'STARTUP'];
            const data = [];
            for (let i = 0; i < 150; i++) {
                data.push({
                    ID: `CUST_${10000 + i}`,
                    SEGMENT: segments[Math.floor(Math.random() * segments.length)],
                    MRR: Math.floor(Math.random() * 5000) + 500,
                    CHURN_RISK: Math.random() > 0.7 ? 'HIGH' : 'LOW',
                    NPS: Math.floor(Math.random() * 50) + 30
                });
            }
            return data;
        }

        function handleKeyDown(e) {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendMessage();
            }
        }

        function quickAsk(text) {
            document.getElementById('userInput').value = text;
            sendMessage();
        }

        async function sendMessage() {
            const input = document.getElementById('userInput');
            const text = input.value.trim();
            if (!text || isProcessing) return;
            
            if (!currentData) {
                loadSample('sales');
                await new Promise(r => setTimeout(r, 500));
            }

            isProcessing = true;
            input.value = '';
            
            showToast('NEURAL_PROCESSING...', 'info');
            
            await new Promise(r => setTimeout(r, 1500));
            
            const dashboard = generateDashboard(text, currentData);
            renderDashboard(dashboard);
            
            document.getElementById('dashboard-panel').classList.add('active');
            isProcessing = false;
        }

        function generateDashboard(query, data) {
            const q = query.toLowerCase();
            const headers = Object.keys(data[0]);
            const numericCols = headers.filter(h => typeof data[0][h] === 'number');
            const categoricalCols = headers.filter(h => typeof data[0][h] === 'string');
            
            const dashboard = {
                title: query,
                metrics: [],
                charts: [],
                table: data.slice(0, 20)
            };

            if (numericCols.length > 0) {
                const col = numericCols[0];
                const values = data.map(d => d[col]).filter(v => !isNaN(v));
                const total = values.reduce((a, b) => a + b, 0);
                
                dashboard.metrics = [
                    { title: 'TOTAL_' + col.toUpperCase(), value: formatNumber(total), change: '+12.5%', color: 'cyan' },
                    { title: 'RECORD_COUNT', value: data.length.toLocaleString(), change: '100%', color: 'pink' },
                    { title: 'CATEGORIES', value: new Set(data.map(d => d[categoricalCols[0]])).size, change: 'ACTIVE', color: 'green' },
                    { title: 'AVG_' + col.toUpperCase(), value: formatNumber(total / values.length), change: '+5.2%', color: 'yellow' }
                ];
            }

            if (q.includes('region') || q.includes('by')) {
                const grouped = aggregateData(data, categoricalCols[0], numericCols[0]);
                dashboard.charts.push({
                    type: 'bar',
                    title: `${numericCols[0]}_BY_${categoricalCols[0]}`,
                    data: grouped,
                    xKey: categoricalCols[0],
                    yKey: numericCols[0]
                });
            }
            
            if (q.includes('trend') || q.includes('month')) {
                const timeCol = categoricalCols.find(c => c.includes('MONTH')) || categoricalCols[0];
                const timeData = aggregateData(data, timeCol, numericCols[0]);
                dashboard.charts.push({
                    type: 'line',
                    title: 'TEMPORAL_ANALYSIS',
                    data: timeData,
                    xKey: timeCol,
                    yKey: numericCols[0]
                });
            }
            
            if (dashboard.charts.length === 0) {
                const grouped = aggregateData(data, categoricalCols[0], numericCols[0], 5);
                dashboard.charts.push({
                    type: 'doughnut',
                    title: 'DISTRIBUTION_MATRIX',
                    data: grouped,
                    key: categoricalCols[0],
                    value: numericCols[0]
                });
            }

            return dashboard;
        }

        function aggregateData(data, groupCol, valueCol, limit = 10) {
            const groups = {};
            data.forEach(row => {
                const key = row[groupCol];
                if (!groups[key]) groups[key] = { [groupCol]: key, [valueCol]: 0 };
                groups[key][valueCol] += Number(row[valueCol]) || 0;
            });
            return Object.values(groups)
                .sort((a, b) => b[valueCol] - a[valueCol])
                .slice(0, limit);
        }

        function renderDashboard(dashboard) {
            document.getElementById('dashboard-empty').classList.add('hidden');
            
            const metricsGrid = document.getElementById('metrics-grid');
            metricsGrid.classList.remove('hidden');
            metricsGrid.innerHTML = dashboard.metrics.map(m => {
                const colorClass = m.color === 'cyan' ? 'text-neon-cyan border-neon-cyan' :
                                  m.color === 'pink' ? 'text-neon-pink border-neon-pink' :
                                  m.color === 'green' ? 'text-neon-green border-neon-green' :
                                  'text-neon-yellow border-neon-yellow';
                
                return `
                    <div class="holo-card rounded-lg p-4 border ${colorClass}">
                        <div class="flex items-center justify-between mb-2">
                            <span class="font-mono text-xs text-gray-400">${m.title}</span>
                            <span class="font-mono text-xs bg-${m.color}-500/20 px-2 py-1 rounded ${colorClass}">${m.change}</span>
                        </div>
                        <p class="font-orbitron text-2xl ${colorClass}">${m.value}</p>
                    </div>
                `;
            }).join('');

            const chartsContainer = document.getElementById('charts-container');
            chartsContainer.classList.remove('hidden');
            chartsContainer.innerHTML = dashboard.charts.map((chart, index) => `
                <div class="chart-container rounded-lg p-4">
                    <h4 class="font-orbitron text-neon-cyan text-sm mb-4 tracking-wider">${chart.title}</h4>
                    <canvas id="chart-${index}"></canvas>
                </div>
            `).join('');

            charts.forEach(c => c.destroy());
            charts = [];

            dashboard.charts.forEach((chart, index) => {
                const ctx = document.getElementById(`chart-${index}`).getContext('2d');
                const newChart = createFuturisticChart(ctx, chart);
                charts.push(newChart);
            });

            document.getElementById('data-table').classList.remove('hidden');
            const headers = Object.keys(dashboard.table[0]);
            document.getElementById('table-head').innerHTML = `
                <tr>${headers.map(h => `<th class="px-4 py-3 text-left font-orbitron text-xs">${h}</th>`).join('')}</tr>
            `;
            document.getElementById('table-body').innerHTML = dashboard.table.map(row => `
                <tr class="hover:bg-neon-cyan/5 transition-colors">${headers.map(h => `<td class="px-4 py-3">${row[h]}</td>`).join('')}</tr>
            `).join('');
        }

        function createFuturisticChart(ctx, config) {
            Chart.defaults.color = '#a0a0a0';
            Chart.defaults.borderColor = 'rgba(0, 243, 255, 0.1)';
            
            const neonColors = ['#00f3ff', '#ff00ff', '#39ff14', '#ffff00', '#ff6600', '#b026ff'];
            
            if (config.type === 'line') {
                return new Chart(ctx, {
                    type: 'line',
                    data: {
                        labels: config.data.map(d => d[config.xKey]),
                        datasets: [{
                            label: config.yKey,
                            data: config.data.map(d => d[config.yKey]),
                            borderColor: '#00f3ff',
                            backgroundColor: (ctx) => {
                                const gradient = ctx.chart.ctx.createLinearGradient(0, 0, 0, 300);
                                gradient.addColorStop(0, 'rgba(0, 243, 255, 0.3)');
                                gradient.addColorStop(1, 'rgba(0, 243, 255, 0)');
                                return gradient;
                            },
                            tension: 0.4,
                            fill: true,
                            pointBackgroundColor: '#00f3ff',
                            pointBorderColor: '#fff',
                            pointHoverBackgroundColor: '#fff',
                            pointHoverBorderColor: '#00f3ff'
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: { legend: { display: false } },
                        scales: {
                            y: { 
                                grid: { color: 'rgba(0, 243, 255, 0.1)' },
                                ticks: { color: '#00f3ff', font: { family: 'Share Tech Mono' } }
                            },
                            x: { 
                                grid: { display: false },
                                ticks: { color: '#a0a0a0', font: { family: 'Share Tech Mono' } }
                            }
                        }
                    }
                });
            } else if (config.type === 'bar') {
                return new Chart(ctx, {
                    type: 'bar',
                    data: {
                        labels: config.data.map(d => d[config.xKey]),
                        datasets: [{
                            label: config.yKey,
                            data: config.data.map(d => d[config.yKey]),
                            backgroundColor: neonColors,
                            borderRadius: 4,
                            borderWidth: 0
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: { legend: { display: false } },
                        scales: {
                            y: { 
                                grid: { color: 'rgba(0, 243, 255, 0.1)' },
                                ticks: { color: '#00f3ff', font: { family: 'Share Tech Mono' } }
                            },
                            x: { 
                                grid: { display: false },
                                ticks: { color: '#a0a0a0', font: { family: 'Share Tech Mono' } }
                            }
                        }
                    }
                });
            } else {
                return new Chart(ctx, {
                    type: 'doughnut',
                    data: {
                        labels: config.data.map(d => d[config.key]),
                        datasets: [{
                            data: config.data.map(d => d[config.value]),
                            backgroundColor: neonColors,
                            borderWidth: 2,
                            borderColor: '#0a0a0f'
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: { 
                                position: 'right',
                                labels: { 
                                    color: '#a0a0a0',
                                    font: { family: 'Rajdhani' }
                                }
                            }
                        }
                    }
                });
            }
        }

        // ==========================================
        // UTILITY FUNCTIONS
        // ==========================================
        function toggleDashboard() {
            document.getElementById('dashboard-panel').classList.toggle('active');
        }

        function toggleMode() {
            document.getElementById('canvas-container').classList.toggle('hidden');
            document.getElementById('fallback-2d').classList.toggle('hidden');
        }

        function fallbackQuery() {
            const input = document.getElementById('fallback-input');
            document.getElementById('userInput').value = input.value;
            sendMessage();
            toggleMode();
        }

        function showToast(message, type) {
            const container = document.getElementById('toastContainer');
            const toast = document.createElement('div');
            
            const colors = {
                success: 'border-neon-green text-neon-green',
                error: 'border-neon-pink text-neon-pink',
                info: 'border-neon-cyan text-neon-cyan'
            };
            
            toast.className = `cyber-glass rounded-lg px-6 py-3 border-l-4 ${colors[type]} transform transition-all duration-300 translate-x-full`;
            toast.innerHTML = `
                <div class="flex items-center gap-3">
                    <span class="font-mono text-lg">${type === 'success' ? '✓' : type === 'error' ? '✕' : '>'}</span>
                    <span class="font-mono text-sm">${message}</span>
                </div>
            `;
            
            container.appendChild(toast);
            requestAnimationFrame(() => toast.classList.remove('translate-x-full'));
            
            setTimeout(() => {
                toast.classList.add('translate-x-full');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        function formatNumber(num) {
            if (num >= 1000000) return (num / 1000000).toFixed(1) + 'M';
            if (num >= 1000) return (num / 1000).toFixed(1) + 'K';
            return num.toFixed(0);
        }

        function exportDashboard() {
            if (!currentData) return;
            const csv = convertToCSV(currentData);
            downloadBlob(csv, 'neural_export.csv', 'text/csv');
        }

        function downloadData() {
            if (!currentData) return;
            const csv = convertToCSV(currentData);
            downloadBlob(csv, currentFileName || 'data_stream.csv', 'text/csv');
        }

        function convertToCSV(data) {
            const headers = Object.keys(data[0]);
            const rows = data.map(row => headers.map(h => `"${row[h]}"`).join(','));
            return [headers.join(','), ...rows].join('\n');
        }

        function downloadBlob(content, filename, type) {
            const blob = new Blob([content], { type });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = filename;
            a.click();
            URL.revokeObjectURL(url);
        }

        // ==========================================
        // INITIALIZATION
        // ==========================================
        document.addEventListener('DOMContentLoaded', () => {
            // Check WebGL
            try {
                const canvas = document.createElement('canvas');
                const gl = canvas.getContext('webgl') || canvas.getContext('experimental-webgl');
                if (!gl) throw new Error('No WebGL');
                
                // Start boot sequence
                runBootSequence();
            } catch (e) {
                console.log('WebGL unavailable, switching to 2D');
                document.getElementById('boot-sequence').style.display = 'none';
                document.getElementById('fallback-2d').classList.remove('hidden');
            }
            
            // Event listeners
            document.getElementById('fileInput').addEventListener('change', handleFileUpload);
            document.getElementById('userInput').addEventListener('input', function() {
                this.style.height = 'auto';
                this.style.height = Math.min(this.scrollHeight, 128) + 'px';
            });
        });
    </script>
</body>
</html>
