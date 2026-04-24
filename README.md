# ArtificialQuest.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Artificial Quest | Space Mission</title>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Orbitron:wght@400;700;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --space-bg:    #03001C;
            --deep:        #070B34;
            --nebula1:     #301B5F;
            --nebula2:     #5C2D91;
            --star:        #E8E8FF;
            --glow-purple: rgba(140,60,255,0.6);
            --glow-cyan:   rgba(0,230,255,0.5);
            --accent:      #C084FC;
            --accent2:     #38BDF8;
            --gold:        #FBBF24;
            --danger:      #F43F5E;
            --text:        #F1F5F9;
            --dim:         rgba(241,245,249,0.55);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-user-select: none;
            user-select: none;
        }

        body {
            background-color: var(--space-bg);
            color: var(--text);
            font-family: 'Inter', sans-serif;
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            position: relative;
        }

        /* ===== BACKGROUND ELEMENTS ===== */
        #space-canvas {
            position: fixed;
            inset: 0;
            z-index: -1;
            background: radial-gradient(circle at center, #1a0533 0%, #03001C 100%);
        }

        .star {
            position: absolute;
            background: white;
            border-radius: 50%;
            pointer-events: none;
            box-shadow: 0 0 4px white;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.3; transform: scale(0.8); }
            50% { opacity: 1; transform: scale(1.2); }
        }

        .nebula {
            position: fixed;
            border-radius: 50%;
            filter: blur(80px);
            z-index: -1;
            pointer-events: none;
            opacity: 0.6;
        }

        .neb-1 { top: -10%; left: -10%; width: 400px; height: 400px; background: rgba(92,45,145,0.25); }
        .neb-2 { bottom: -5%; right: -5%; width: 350px; height: 350px; background: rgba(0,180,255,0.15); }
        .neb-3 { top: 40%; right: -5%; width: 300px; height: 300px; background: rgba(192,132,252,0.12); }

        .shooting-star {
            position: fixed;
            width: 2px;
            height: 80px;
            background: linear-gradient(to bottom, white, transparent);
            z-index: -1;
            opacity: 0;
            pointer-events: none;
        }

        @keyframes shooting {
            0% { transform: translate(0, 0) rotate(45deg); opacity: 0; }
            10% { opacity: 1; }
            30% { transform: translate(400px, 400px) rotate(45deg); opacity: 0; }
            100% { transform: translate(400px, 400px) rotate(45deg); opacity: 0; }
        }

        /* ===== SCREENS ===== */
        .screen {
            position: absolute;
            inset: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            transition: opacity 0.6s ease, visibility 0.6s;
            z-index: 10;
        }

        .screen.hidden {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
        }

        /* ===== INTRO SCREEN ===== */
        #intro-screen h1 {
            font-family: 'Orbitron', sans-serif;
            font-weight: 900;
            font-size: clamp(2.5rem, 6vw, 5rem);
            background: linear-gradient(135deg, #C084FC, #38BDF8, #FBBF24);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-align: center;
            animation: pulseGlow 3s ease-in-out infinite alternate;
        }

        @keyframes pulseGlow {
            from { filter: drop-shadow(0 0 20px rgba(192,132,252,0.4)); }
            to { filter: drop-shadow(0 0 50px rgba(192,132,252,0.8)) drop-shadow(0 0 80px rgba(56,189,248,0.4)); }
        }

        .subtitle {
            margin-top: 0.8rem;
            font-size: 0.95rem;
            letter-spacing: 3px;
            color: var(--dim);
            text-transform: uppercase;
            text-align: center;
        }

        .orbit-container {
            margin: 2rem auto;
            width: 180px;
            height: 180px;
            border: 2px solid rgba(192,132,252,0.3);
            border-radius: 50%;
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .orbit-dot {
            width: 10px;
            height: 10px;
            background: var(--accent);
            border-radius: 50%;
            position: absolute;
            box-shadow: 0 0 12px var(--accent);
            offset-path: circle(90px at 50% 50%);
            animation: rotateOrbit 8s linear infinite;
        }

        @keyframes rotateOrbit {
            from { offset-distance: 0%; }
            to { offset-distance: 100%; }
        }

        .launch-btn {
            background: linear-gradient(135deg, #7C3AED, #0EA5E9);
            border: none;
            border-radius: 12px;
            padding: 16px 48px;
            font-family: 'Orbitron', sans-serif;
            font-weight: 700;
            font-size: 1.1rem;
            color: white;
            cursor: pointer;
            box-shadow: 0 0 30px rgba(124,58,237,0.5), 0 0 60px rgba(14,165,233,0.2);
            transition: all 0.25s cubic-bezier(.34,1.4,.64,1);
            margin-top: 2rem;
            text-transform: uppercase;
        }

        .launch-btn:hover {
            transform: scale(1.06);
            box-shadow: 0 0 50px rgba(124,58,237,0.7), 0 0 80px rgba(14,165,233,0.4);
        }

        .tagline {
            position: absolute;
            bottom: 24px;
            font-size: 0.75rem;
            color: var(--dim);
            letter-spacing: 2px;
            text-transform: uppercase;
        }

        /* ===== MAP SCREEN ===== */
        #map-screen {
            justify-content: flex-start;
        }

        #map-container {
            width: 100%;
            height: 100%;
            position: relative;
            overflow: hidden;
        }

        #map-svg {
            position: absolute;
            inset: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        .path-glow {
            stroke: rgba(56,189,248,0.1);
            stroke-width: 24;
            fill: none;
            filter: blur(4px);
        }

        .path-main {
            stroke: rgba(192,132,252,0.35);
            stroke-width: 8;
            fill: none;
            stroke-dasharray: 16 10;
            filter: drop-shadow(0 0 8px rgba(192,132,252,0.4));
        }

        /* ===== NODES ===== */
        .node {
            position: absolute;
            z-index: 5;
            transform: translate(-50%, -50%);
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .node-circle {
            width: 72px;
            height: 72px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: 'Orbitron', sans-serif;
            font-weight: 900;
            font-size: 1.2rem;
            color: white;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
        }

        .node.unlocked .node-circle {
            background: radial-gradient(circle, #7C3AED, #4C1D95);
            border: 3px solid rgba(192,132,252,0.8);
            box-shadow: 0 0 20px rgba(192,132,252,0.5), 0 0 40px rgba(192,132,252,0.2);
            animation: breathe 2s infinite alternate;
        }

        .node.unlocked .node-circle:hover {
            transform: scale(1.15);
            box-shadow: 0 0 40px rgba(192,132,252,0.8);
        }

        @keyframes breathe {
            from { box-shadow: 0 0 20px rgba(192,132,252,0.5); }
            to { box-shadow: 0 0 40px rgba(192,132,252,0.8), 0 0 60px rgba(192,132,252,0.3); }
        }

        .node.locked .node-circle {
            background: rgba(30,20,60,0.7);
            border: 3px solid rgba(100,80,150,0.3);
            cursor: not-allowed;
            color: rgba(255,255,255,0.3);
        }

        .node.completed .node-circle {
            background: radial-gradient(circle, #059669, #064E3B);
            border: 3px solid rgba(52,211,153,0.8);
            box-shadow: 0 0 20px rgba(52,211,153,0.5);
        }

        .node-label {
            margin-top: 12px;
            background: rgba(0,0,0,0.6);
            border: 1px solid rgba(192,132,252,0.25);
            border-radius: 8px;
            padding: 4px 12px;
            font-size: 0.7rem;
            font-weight: 600;
            color: var(--accent);
            white-space: nowrap;
            backdrop-filter: blur(4px);
        }

        /* ===== PLAYER TOKEN ===== */
        #player-token {
            position: absolute;
            width: 48px;
            height: 48px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            z-index: 10;
            transform: translate(-50%, -50%);
            transition: left 0.8s cubic-bezier(.34,1.4,.64,1), top 0.8s cubic-bezier(.34,1.4,.64,1);
            filter: drop-shadow(0 0 16px rgba(251,191,36,0.8));
        }

        #player-token::after {
            content: '';
            position: absolute;
            inset: -4px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(251,191,36,0.4) 0%, transparent 70%);
            animation: tokenPulse 1.5s infinite alternate;
        }

        @keyframes tokenPulse {
            from { transform: scale(1); opacity: 0.5; }
            to { transform: scale(1.4); opacity: 0.8; }
        }

        .trail-dot {
            position: absolute;
            width: 6px;
            height: 6px;
            background: var(--gold);
            border-radius: 50%;
            pointer-events: none;
            z-index: 9;
            animation: fadeOutTrail 0.6s forwards;
        }

        @keyframes fadeOutTrail {
            to { opacity: 0; transform: scale(0); }
        }

        /* ===== HUD ===== */
        .hud-pill {
            position: fixed;
            background: rgba(0,0,0,0.5);
            backdrop-filter: blur(10px);
            border-radius: 100px;
            padding: 8px 20px;
            font-family: 'Orbitron', sans-serif;
            font-weight: 700;
            font-size: 0.9rem;
            display: flex;
            align-items: center;
            gap: 10px;
            z-index: 50;
            transition: transform 0.3s cubic-bezier(.34,1.4,.64,1);
        }

        #score-display {
            top: 24px;
            right: 24px;
            color: var(--gold);
            border: 1px solid rgba(251,191,36,0.3);
        }

        #rounds-counter {
            top: 24px;
            left: 24px;
            color: var(--accent);
            border: 1px solid rgba(192,132,252,0.3);
            display: none;
        }

        .hud-pulse {
            transform: scale(1.2);
        }

        /* ===== TOOLTIP ===== */
        #node-tooltip {
            position: absolute;
            background: rgba(10,5,30,0.92);
            border: 1px solid rgba(192,132,252,0.3);
            border-radius: 12px;
            padding: 12px 16px;
            min-width: 220px;
            z-index: 100;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.3s;
            transform: translate(-50%, -120%);
        }

        #node-tooltip h4 {
            font-family: 'Orbitron', sans-serif;
            color: var(--accent);
            font-size: 0.8rem;
            margin-bottom: 4px;
            text-transform: uppercase;
        }

        #node-tooltip .tt-name {
            font-weight: 700;
            font-size: 1rem;
            margin-bottom: 8px;
            display: block;
        }

        #node-tooltip p {
            font-size: 0.75rem;
            color: var(--dim);
            line-height: 1.4;
        }

        /* ===== MODAL ===== */
        #modal-overlay {
            position: fixed;
            inset: 0;
            background: rgba(3,0,28,0.85);
            backdrop-filter: blur(12px);
            z-index: 200;
            display: flex;
            align-items: center;
            justify-content: center;
            opacity: 0;
            visibility: hidden;
            transition: all 0.4s ease;
        }

        #modal-overlay.active {
            opacity: 1;
            visibility: visible;
        }

        .modal-card {
            background: linear-gradient(135deg, rgba(30,10,70,0.95), rgba(10,5,40,0.98));
            border: 1px solid rgba(192,132,252,0.3);
            border-radius: 20px;
            width: 90%;
            max-width: 520px;
            padding: 2.5rem;
            position: relative;
            box-shadow: 0 0 60px rgba(124,58,237,0.3);
            transform: translateY(20px);
            transition: transform 0.4s ease;
        }

        #modal-overlay.active .modal-card {
            transform: translateY(0);
        }

        .modal-close {
            position: absolute;
            top: 1.5rem;
            right: 1.5rem;
            background: none;
            border: none;
            color: var(--dim);
            font-size: 1.5rem;
            cursor: pointer;
            transition: color 0.2s;
        }

        .modal-close:hover { color: white; }

        .round-badge {
            font-family: 'Orbitron', sans-serif;
            color: var(--accent);
            letter-spacing: 3px;
            font-size: 0.8rem;
            margin-bottom: 0.5rem;
            display: block;
        }

        .round-title {
            font-family: 'Orbitron', sans-serif;
            font-weight: 700;
            font-size: clamp(1.4rem, 3vw, 2rem);
            background: linear-gradient(90deg, var(--accent), var(--accent2));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 1rem;
        }

        .round-desc {
            color: var(--dim);
            line-height: 1.6;
            font-size: 0.95rem;
            margin-bottom: 1.5rem;
        }

        .divider {
            height: 1px;
            background: rgba(192,132,252,0.15);
            margin: 1.5rem 0;
        }

        .question-box {
            margin-top: 1.5rem;
        }

        .ascii-art {
            background: rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.1);
            padding: 10px;
            font-family: monospace;
            white-space: pre;
            font-size: 0.8rem;
            color: var(--accent2);
            text-align: center;
            margin-bottom: 1.5rem;
            border-radius: 8px;
        }

        .code-block {
            background: #0d1117;
            padding: 12px;
            border-radius: 8px;
            font-family: 'Courier New', Courier, monospace;
            font-size: 0.85rem;
            color: #e6edf3;
            margin-bottom: 1.5rem;
            border: 1px solid rgba(255,255,255,0.05);
            overflow-x: auto;
        }

        .answers-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }

        .answer-btn {
            background: rgba(255,255,255,0.05);
            border: 1px solid rgba(192,132,252,0.25);
            border-radius: 10px;
            padding: 12px 16px;
            font-family: 'Inter', sans-serif;
            font-weight: 600;
            color: var(--text);
            cursor: pointer;
            transition: all 0.2s;
            text-align: left;
        }

        .answer-btn:hover {
            background: rgba(124,58,237,0.2);
            border-color: var(--accent);
        }

        .answer-btn.correct {
            background: rgba(52,211,153,0.3) !important;
            border-color: #34d399 !important;
        }

        .answer-btn.wrong {
            background: rgba(244,63,94,0.3) !important;
            border-color: var(--danger) !important;
        }

        .float-points {
            position: absolute;
            font-family: 'Orbitron', sans-serif;
            font-weight: 900;
            pointer-events: none;
            animation: floatUpPoints 1.2s forwards;
            z-index: 300;
        }

        @keyframes floatUpPoints {
            0% { transform: translateY(0); opacity: 0; }
            20% { opacity: 1; }
            100% { transform: translateY(-50px); opacity: 0; }
        }

        .shake {
            animation: shakeScreen 0.3s linear 3;
        }

        @keyframes shakeScreen {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-6px); }
            75% { transform: translateX(6px); }
        }

        /* ===== WIN SCREEN ===== */
        #win-screen {
            background: radial-gradient(circle at center, #1a0533 0%, #03001C 100%);
            z-index: 500;
            text-align: center;
        }

        .win-title {
            font-family: 'Orbitron', sans-serif;
            font-weight: 900;
            font-size: clamp(2rem, 5vw, 4rem);
            background: linear-gradient(135deg, #FBBF24, #C084FC);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 1rem;
            animation: winGlow 2s infinite alternate;
        }

        @keyframes winGlow {
            from { filter: drop-shadow(0 0 20px rgba(251,191,36,0.5)); }
            to { filter: drop-shadow(0 0 50px rgba(251,191,36,1)); }
        }

        .final-score {
            font-family: 'Orbitron', sans-serif;
            font-weight: 700;
            font-size: 2.5rem;
            color: var(--gold);
            margin: 1.5rem 0;
        }

        .confetti {
            position: absolute;
            width: 8px;
            height: 8px;
            border-radius: 2px;
            pointer-events: none;
            z-index: 1000;
        }
    </style>
</head>
<body>

    <!-- Background Layer -->
    <div id="space-canvas"></div>
    <div class="nebula neb-1"></div>
    <div class="nebula neb-2"></div>
    <div class="nebula neb-3"></div>

    <!-- HUD Elements -->
    <div id="score-display" class="hud-pill">
        <span>⭐</span> <span id="score-val">0</span> <span>PTS</span>
    </div>
    <div id="rounds-counter" class="hud-pill">
        <span>🏆</span> <span id="rounds-val">0</span><span>/5 ROUNDS</span>
    </div>

    <!-- Screen 1: Intro -->
    <section id="intro-screen" class="screen">
        <div class="orbit-container">
            <div class="orbit-dot"></div>
        </div>
        <h1>ARTIFICIAL QUEST</h1>
        <p class="subtitle">A multi-round challenge for the next-gen coders</p>
        
        <button class="launch-btn" onclick="startGame()">Launch Mission 🚀</button>
        
        <p class="tagline">Powered by Team Artificial Quest · CS Dept · 2025</p>
    </section>

    <!-- Screen 2: Map -->
    <section id="map-screen" class="screen hidden">
        <div id="map-container">
            <svg id="map-svg" viewBox="0 0 1000 1000">
                <path id="path-glow" class="path-glow" d=""></path>
                <path id="path-main" class="path-main" d=""></path>
            </svg>

            <div id="nodes-container"></div>
            
            <div id="player-token">🚀</div>

            <div id="node-tooltip">
                <h4 id="tt-round">ROUND 1</h4>
                <span class="tt-name" id="tt-name">GLITCH VISION</span>
                <p id="tt-desc">Description goes here...</p>
            </div>
        </div>
    </section>

    <!-- Modal Overlay -->
    <div id="modal-overlay">
        <div class="modal-card">
            <button class="modal-close" onclick="closeModal()">✕</button>
            <span class="round-badge" id="modal-badge">ROUND 1</span>
            <h2 class="round-title" id="modal-title">GLITCH VISION</h2>
            <p class="round-desc" id="modal-desc">Spot the glitch. Find hidden patterns, errors, or clues.</p>
            
            <div class="divider"></div>

            <div class="question-box" id="modal-question-box">
                <!-- Dynamic Question Content -->
            </div>
        </div>
    </div>

    <!-- Win Screen -->
    <section id="win-screen" class="screen hidden">
        <h1 class="win-title">🏆 MISSION COMPLETE</h1>
        <p style="color: var(--dim); font-size: 1.2rem;">You completed Artificial Quest!</p>
        <div class="final-score">⭐ <span id="final-score-val">0</span> PTS</div>
        <button class="launch-btn" onclick="resetGame()">Play Again</button>
    </section>

    <script>
        // ===== GAME STATE =====
        let score = 0;
        let currentLevel = 1; // 1 to 5
        let completedLevels = [];
        let isMoving = false;

        const nodePositions = [
            { id: 1, x: 120, y: 780, name: "GLITCH VISION", desc: "Spot the glitch. Find hidden patterns, errors, or clues. No tech skills needed — just eyes." },
            { id: 2, x: 380, y: 580, name: "DECODE MODE", desc: "Binary. Ciphers. QR codes. Decode it before your rivals do." },
            { id: 3, x: 620, y: 380, name: "BUG HUNT", desc: "Broken code. Wrong logic. Fix it, predict it, own it." },
            { id: 4, x: 380, y: 220, name: "MEME MATRIX", desc: "Decode the meme. The clue is in the format. Big brain energy required." },
            { id: 5, x: 640, y: 100, name: "FINAL BOSS", desc: "Everything you've learned. One massive puzzle. Team up or lose." }
        ];

        const questions = {
            1: {
                type: 'ascii',
                content: "  /$$$$$$  /$$$$$$$$ /$$$$$$ \n /$$__  $$|__  $$__/|_  $$_/ \n| $$  \\__/   | $$     | $$   \n| $$         | $$     | $$   \n| $$    $$   | $$     | $$   \n|  $$$$$$/   | $$    /$$$$$$ \n \\______/    |__/   |______/ \n\nWhat is the hidden word?",
                options: ["CTI", "ATI", "CTI", "DTI"],
                correct: 0
            },
            2: {
                type: 'text',
                content: "Decode the message: <br><br><b>01001000 01101001</b>",
                options: ["Ho", "Hi", "Ha", "He"],
                correct: 1
            },
            3: {
                type: 'code',
                content: "def check_even(num):\n    if num % 2 = 0:\n        return True\n    return False\n\nWhat is the bug?",
                options: ["Missing Colon", "Assignment vs Equality", "Indentation Error", "Variable naming"],
                correct: 1
            },
            4: {
                type: 'text',
                content: "Meme Analysis: <br><br><b>'When you [BLANK] but the compiler says no 💀'</b>",
                options: ["Fix the bug", "Think it's fixed", "Delete the file", "Go to sleep"],
                correct: 1
            },
            5: {
                type: 'text',
                content: "Final Integration: <br><br><b>[CTI] + [==] = ?</b>",
                options: ["CTI == True", "CTI == Boss", "CTI == 100", "CTI == Done"],
                correct: 0
            }
        };

        // ===== INITIALIZATION =====
        document.addEventListener('DOMContentLoaded', () => {
            initStars();
            createShootingStar();
            drawPath();
            renderNodes();
            updatePlayerPosition(false);
            
            // Random shooting stars
            setInterval(triggerShootingStar, 6000);
        });

        function initStars() {
            const container = document.getElementById('space-canvas');
            for (let i = 0; i < 200; i++) {
                const star = document.createElement('div');
                star.className = 'star';
                const size = Math.random() * 2 + 1;
                star.style.width = `${size}px`;
                star.style.height = `${size}px`;
                star.style.left = `${Math.random() * 100}%`;
                star.style.top = `${Math.random() * 100}%`;
                star.style.animation = `twinkle ${2 + Math.random() * 3}s infinite ${Math.random() * 5}s`;
                container.appendChild(star);
            }
        }

        function createShootingStar() {
            const ss = document.createElement('div');
            ss.className = 'shooting-star';
            ss.id = 'shooting-star-el';
            document.body.appendChild(ss);
        }

        function triggerShootingStar() {
            const ss = document.getElementById('shooting-star-el');
            ss.style.top = `${Math.random() * 50}%`;
            ss.style.left = `${Math.random() * 80}%`;
            ss.style.animation = 'none';
            void ss.offsetWidth; // Trigger reflow
            ss.style.animation = 'shooting 2s forwards';
        }

        function drawPath() {
            const pathMain = document.getElementById('path-main');
            const pathGlow = document.getElementById('path-glow');
            
            // Generate Cubic Bezier Path
            let d = `M ${nodePositions[0].x} ${nodePositions[0].y}`;
            for (let i = 0; i < nodePositions.length - 1; i++) {
                const start = nodePositions[i];
                const end = nodePositions[i+1];
                const midY = (start.y + end.y) / 2;
                d += ` C ${start.x} ${midY}, ${end.x} ${midY}, ${end.x} ${end.y}`;
            }
            
            pathMain.setAttribute('d', d);
            pathGlow.setAttribute('d', d);
        }

        function renderNodes() {
            const container = document.getElementById('nodes-container');
            container.innerHTML = '';
            
            nodePositions.forEach(node => {
                const nodeEl = document.createElement('div');
                nodeEl.className = 'node';
                nodeEl.id = `node-${node.id}`;
                nodeEl.style.left = `${node.x / 10}%`;
                nodeEl.style.top = `${node.y / 10}%`;
                
                // State Logic
                if (completedLevels.includes(node.id)) {
                    nodeEl.classList.add('completed');
                } else if (node.id === currentLevel) {
                    nodeEl.classList.add('unlocked');
                } else {
                    nodeEl.classList.add('locked');
                }

                const circle = document.createElement('div');
                circle.className = 'node-circle';
                
                if (completedLevels.includes(node.id)) {
                    circle.innerText = '✓';
                } else if (node.id === currentLevel) {
                    circle.innerText = node.id;
                } else {
                    circle.innerText = '🔒';
                }

                circle.onclick = () => handleNodeClick(node.id);
                circle.onmouseenter = (e) => showTooltip(e, node);
                circle.onmouseleave = hideTooltip;

                const label = document.createElement('div');
                label.className = 'node-label';
                label.innerText = `👁️ ${node.name}`;

                nodeEl.appendChild(circle);
                nodeEl.appendChild(label);
                container.appendChild(nodeEl);
            });
        }

        function showTooltip(e, node) {
            if (completedLevels.includes(node.id) || node.id === currentLevel) {
                const tt = document.getElementById('node-tooltip');
                const ttRound = document.getElementById('tt-round');
                const ttName = document.getElementById('tt-name');
                const ttDesc = document.getElementById('tt-desc');
                
                ttRound.innerText = `ROUND ${node.id}`;
                ttName.innerText = node.name;
                ttDesc.innerText = node.desc;
                
                tt.style.left = `${node.x / 10}%`;
                tt.style.top = `${node.y / 10}%`;
                tt.style.opacity = '1';
            }
        }

        function hideTooltip() {
            document.getElementById('node-tooltip').style.opacity = '0';
        }

        // ===== GAME LOGIC =====
        function startGame() {
            document.getElementById('intro-screen').classList.add('hidden');
            document.getElementById('map-screen').classList.remove('hidden');
            document.getElementById('rounds-counter').style.display = 'flex';
        }

        function handleNodeClick(id) {
            if (id !== currentLevel || isMoving) return;
            openModal(id);
        }

        function openModal(id) {
            const node = nodePositions.find(n => n.id === id);
            const q = questions[id];
            
            document.getElementById('modal-badge').innerText = `ROUND ${id}`;
            document.getElementById('modal-title').innerText = node.name;
            document.getElementById('modal-desc').innerText = node.desc;
            
            const qBox = document.getElementById('modal-question-box');
            qBox.innerHTML = '';
            
            if (q.type === 'ascii') {
                const ascii = document.createElement('div');
                ascii.className = 'ascii-art';
                ascii.innerText = q.content;
                qBox.appendChild(ascii);
            } else if (q.type === 'code') {
                const code = document.createElement('pre');
                code.className = 'code-block';
                code.innerText = q.content;
                qBox.appendChild(code);
            } else {
                const text = document.createElement('p');
                text.style.color = 'var(--text)';
                text.style.marginBottom = '1.5rem';
                text.innerHTML = q.content;
                qBox.appendChild(text);
            }
            
            const grid = document.createElement('div');
            grid.className = 'answers-grid';
            q.options.forEach((opt, idx) => {
                const btn = document.createElement('button');
                btn.className = 'answer-btn';
                btn.innerText = opt;
                btn.onclick = (e) => checkAnswer(e, idx, q.correct);
                grid.appendChild(btn);
            });
            qBox.appendChild(grid);
            
            document.getElementById('modal-overlay').classList.add('active');
        }

        function closeModal() {
            document.getElementById('modal-overlay').classList.remove('active');
        }

        function checkAnswer(e, selected, correct) {
            const btn = e.target;
            const card = document.querySelector('.modal-card');
            
            if (selected === correct) {
                btn.classList.add('correct');
                updateScore(10, btn);
                setTimeout(() => {
                    closeModal();
                    completeRound();
                }, 1200);
            } else {
                btn.classList.add('wrong');
                updateScore(-5, btn);
                document.body.classList.add('shake');
                setTimeout(() => document.body.classList.remove('shake'), 300);
            }
        }

        function updateScore(delta, btn) {
            score = Math.max(0, score + delta);
            document.getElementById('score-val').innerText = score;
            
            // HUD Animation
            const pill = document.getElementById('score-display');
            pill.classList.add('hud-pulse');
            setTimeout(() => pill.classList.remove('hud-pulse'), 300);
            
            // Floating Points
            const float = document.createElement('div');
            float.className = 'float-points';
            float.innerText = delta > 0 ? `+${delta} PTS` : `${delta} PTS`;
            float.style.color = delta > 0 ? 'var(--gold)' : 'var(--danger)';
            
            const rect = btn.getBoundingClientRect();
            float.style.left = `${rect.left + rect.width / 2}px`;
            float.style.top = `${rect.top}px`;
            document.body.appendChild(float);
            
            setTimeout(() => float.remove(), 1200);
        }

        function completeRound() {
            if (completedLevels.includes(currentLevel)) return;
            
            completedLevels.push(currentLevel);
            document.getElementById('rounds-val').innerText = completedLevels.length;
            
            if (currentLevel < 5) {
                currentLevel++;
                renderNodes();
                updatePlayerPosition(true);
            } else {
                renderNodes();
                setTimeout(showWinScreen, 1500);
            }
        }

        function updatePlayerPosition(animate) {
            const token = document.getElementById('player-token');
            const pos = nodePositions.find(n => n.id === (completedLevels.length + 1) || n.id === currentLevel);
            
            if (!pos) return;

            if (animate) {
                isMoving = true;
                const startX = parseFloat(token.style.left) || (nodePositions[0].x / 10);
                const startY = parseFloat(token.style.top) || (nodePositions[0].y / 10);
                
                // Spawn trail
                let trailInterval = setInterval(() => {
                    const rect = token.getBoundingClientRect();
                    spawnTrail(rect.left + rect.width/2, rect.top + rect.height/2);
                }, 50);
                
                setTimeout(() => {
                    clearInterval(trailInterval);
                    isMoving = false;
                }, 800);
            }

            token.style.left = `${pos.x / 10}%`;
            token.style.top = `${pos.y / 10}%`;
        }

        function spawnTrail(x, y) {
            const dot = document.createElement('div');
            dot.className = 'trail-dot';
            dot.style.left = `${x}px`;
            dot.style.top = `${y}px`;
            document.body.appendChild(dot);
            setTimeout(() => dot.remove(), 600);
        }

        function showWinScreen() {
            document.getElementById('map-screen').classList.add('hidden');
            document.getElementById('rounds-counter').style.display = 'none';
            document.getElementById('final-score-val').innerText = score;
            document.getElementById('win-screen').classList.remove('hidden');
            spawnConfetti();
        }

        function spawnConfetti() {
            const colors = ['#C084FC', '#38BDF8', '#FBBF24', '#7C3AED', '#0EA5E9'];
            for (let i = 0; i < 60; i++) {
                const conf = document.createElement('div');
                conf.className = 'confetti';
                conf.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                conf.style.left = '50%';
                conf.style.top = '50%';
                
                const angle = Math.random() * Math.PI * 2;
                const dist = 100 + Math.random() * 300;
                const tx = Math.cos(angle) * dist;
                const ty = Math.sin(angle) * dist;
                
                conf.animate([
                    { transform: 'translate(0, 0) scale(1)', opacity: 1 },
                    { transform: `translate(${tx}px, ${ty}px) scale(0)`, opacity: 0 }
                ], {
                    duration: 1500 + Math.random() * 1000,
                    easing: 'cubic-bezier(.17,.67,.83,.67)'
                }).onfinish = () => conf.remove();
                
                document.body.appendChild(conf);
            }
        }

        function resetGame() {
            score = 0;
            currentLevel = 1;
            completedLevels = [];
            isMoving = false;
            
            document.getElementById('score-val').innerText = '0';
            document.getElementById('rounds-val').innerText = '0';
            
            document.getElementById('win-screen').classList.add('hidden');
            document.getElementById('intro-screen').classList.remove('hidden');
            
            renderNodes();
            updatePlayerPosition(false);
        }
    </script>
</body>
</html>
