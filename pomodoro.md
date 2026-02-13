---
layout: null
permalink: /pomodoro/
title: pomodoro
---
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Focus Flow v2.1</title>
    <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;700&family=Fira+Code:wght@400;600&display=swap" rel="stylesheet">
    <style>
        :root {
            /* --- Theme 1: Pastel Flow --- */
            --bg-pastel: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
            --glass-pastel: rgba(255, 255, 255, 0.3);
            --text-pastel: #4a4a4a;
            --accent-work-pastel: #ff9a9e;
            --accent-break-pastel: #66a6ff;
            --font-main: 'Quicksand', sans-serif;

            /* --- Theme 2: Hacker Pink Terminal (UPDATED) --- */
            --bg-hacker: #0a0a0a;
            --glass-hacker: rgba(20, 20, 20, 0.9);
            /* UPDATED: Lighter, softer pink */
            --text-hacker: #ff80ab; 
            --accent-work-hacker: #ff80ab;
            --accent-break-hacker: #00f2ff; /* Cyan Break */
            --font-hacker: 'Fira Code', monospace;
        }

        body {
            transition: background 0.5s ease, color 0.5s ease;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0;
            padding: 20px;
        }

        /* Base Styles for Themes */
        body.theme-pastel { background: var(--bg-pastel); color: var(--text-pastel); font-family: var(--font-main); }
        body.theme-hacker { background: var(--bg-hacker); color: var(--text-hacker); font-family: var(--font-hacker); }

        .glass-container {
            max-width: 850px;
            width: 100%;
            backdrop-filter: blur(15px);
            border-radius: 35px;
            padding: 30px;
            text-align: center;
            transition: all 0.5s ease;
        }

        body.theme-pastel .glass-container { 
            background: var(--glass-pastel); 
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
        }
        
        body.theme-hacker .glass-container { 
            background: var(--glass-hacker); 
            border: 1px solid rgba(255, 128, 171, 0.3); /* Lighter pink border */
            border-radius: 12px;
            /* UPDATED: Reduced container glow */
            box-shadow: 0 0 15px rgba(255, 128, 171, 0.1);
        }

        .video-wrapper {
            border-radius: 20px;
            overflow: hidden;
            position: relative;
            padding-bottom: 56.25%;
            height: 0;
            background: #000;
            margin-bottom: 20px;
            transition: border-radius 0.5s ease;
        }
        body.theme-hacker .video-wrapper { border-radius: 8px; }

        .video-wrapper iframe { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }

        .timer-display { font-size: 5.5rem; font-weight: 700; margin: 10px 0; transition: all 0.5s; }
        
        /* Dynamic Colors & Glows for Timer */
        body.theme-pastel.work-mode .timer-display { color: var(--accent-work-pastel); }
        body.theme-pastel.break-mode .timer-display { color: var(--accent-break-pastel); }
        
        /* UPDATED: Reduced glow (text-shadow) for Hacker mode */
        body.theme-hacker.work-mode .timer-display { 
            color: var(--accent-work-hacker); 
            text-shadow: 0 0 5px rgba(255, 128, 171, 0.6);
        }
        body.theme-hacker.break-mode .timer-display { 
            color: var(--accent-break-hacker); 
            text-shadow: 0 0 5px rgba(0, 242, 255, 0.6);
        }

        button {
            padding: 10px 24px;
            border-radius: 50px;
            border: none;
            cursor: pointer;
            margin: 5px;
            font-weight: 700;
            font-family: inherit;
            transition: all 0.3s ease;
            font-size: 0.9rem;
        }

        /* Patel Buttons */
        body.theme-pastel button { background: rgba(255,255,255,0.5); color: var(--text-pastel); }
        body.theme-pastel button:hover { background: #fff; box-shadow: 0 5px 15px rgba(0,0,0,0.1); }

        /* Hacker Buttons */
        body.theme-hacker button { 
            background: transparent; 
            border: 1px solid var(--text-hacker); 
            color: var(--text-hacker); 
            border-radius: 4px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        body.theme-hacker button:hover { 
            background: var(--text-hacker); 
            color: var(--bg-hacker); /* Dark text on pink background */
            box-shadow: 0 0 10px rgba(255, 128, 171, 0.4);
        }

        .settings-grid, .theme-switcher {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 20px;
            text-align: left;
            padding: 25px;
            border-radius: 25px;
            transition: all 0.5s ease;
        }

        body.theme-pastel .settings-grid, body.theme-pastel .theme-switcher { background: rgba(255,255,255,0.2); }
        body.theme-hacker .settings-grid, body.theme-hacker .theme-switcher { background: rgba(0,0,0,0.3); border-radius: 12px; border: 1px solid rgba(255, 128, 171, 0.1);}

        .hidden { display: none !important; }

        input {
            width: 100%; padding: 10px; border-radius: 12px; border: 1px solid rgba(255,255,255,0.3);
            background: rgba(255,255,255,0.2); color: inherit; margin-top: 8px; font-family: inherit; box-sizing: border-box;
            transition: all 0.3s ease;
        }
        
        body.theme-hacker input {
            border-radius: 4px;
            border: 1px solid rgba(255, 128, 171, 0.3);
            background: rgba(0,0,0,0.5);
        }
        body.theme-hacker input:focus { outline: none; border-color: var(--text-hacker); }

        .focus-btn { position: fixed; top: 20px; right: 20px; z-index: 100; opacity: 0.6; }
        .focus-btn:hover { opacity: 1; }
        
        #status { font-weight: bold; letter-spacing: 2px; text-transform: uppercase; margin-bottom: 10px; display: block;}
    </style>
</head>
<body class="theme-pastel work-mode">

<button class="focus-btn" onclick="toggleFocus()">👁️ Focus Mode</button>

<div class="glass-container" id="mainContainer">
    <div id="header-area">
        <div class="theme-switcher">
            <button onclick="setTheme('pastel')">🌸 Pastel Flow</button>
            <button onclick="setTheme('hacker')">⌨️ Hacker Pink</button>
        </div>
    </div>

    <span id="status">Ready?</span>
    <div class="timer-display" id="timer">25:00</div>

    <div class="video-wrapper">
        <div id="player"></div>
    </div>

    <div class="controls">
        <button id="startBtn">START SESSION</button>
        <button id="pauseBtn">PAUSE</button>
        <button id="resetBtn">RESET</button>
    </div>

    <div class="settings-grid" id="settings-area">
        <div>
            <strong>WORK CONFIG</strong>
            <input type="text" id="workUrl" value="https://www.youtube.com/watch?v=jfKfPfyJRdk" onchange="saveSettings()">
            <input type="number" id="workMins" value="25" onchange="saveSettings()">
        </div>
        <div>
            <strong>BREAK CONFIG</strong>
            <input type="text" id="breakUrl" value="https://www.youtube.com/watch?v=5qap5aO4i9A" onchange="saveSettings()">
            <div style="margin-top: 10px;">
                 <input type="checkbox" id="syncBreak" onchange="saveSettings()" style="width: auto; margin-right: 10px;"> Sync Length
            </div>
            <input type="number" id="breakMins" value="5" onchange="saveSettings()">
        </div>
    </div>
</div>

<script>
    let player, timerInterval, timeLeft, isPaused = true;
    let currentState = 'IDLE', nextState = 'WORKING';
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

    function setTheme(theme) {
        document.body.classList.remove('theme-pastel', 'theme-hacker');
        document.body.classList.add('theme-' + theme);
        saveSettings();
    }

    function toggleFocus() {
        document.getElementById('header-area').classList.toggle('hidden');
        document.getElementById('settings-area').classList.toggle('hidden');
    }

    function saveSettings() {
        const settings = {
            workUrl: document.getElementById('workUrl').value,
            workMins: document.getElementById('workMins').value,
            breakUrl: document.getElementById('breakUrl').value,
            breakMins: document.getElementById('breakMins').value,
            syncBreak: document.getElementById('syncBreak').checked,
            theme: document.body.classList.contains('theme-hacker') ? 'hacker' : 'pastel'
        };
        localStorage.setItem('pomodoroSettingsV2', JSON.stringify(settings));
    }

    function loadSettings() {
        const s = JSON.parse(localStorage.getItem('pomodoroSettingsV2'));
        if (s) {
            document.getElementById('workUrl').value = s.workUrl;
            document.getElementById('workMins').value = s.workMins;
            document.getElementById('breakUrl').value = s.breakUrl;
            document.getElementById('breakMins').value = s.breakMins;
            document.getElementById('syncBreak').checked = s.syncBreak;
            setTheme(s.theme || 'pastel');
        }
    }

    function playChime(freq, type = 'sine') {
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.type = type;
        osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 1);
        osc.connect(gain); gain.connect(audioCtx.destination);
        osc.start(); osc.stop(audioCtx.currentTime + 1);
    }

    var tag = document.createElement('script');
    tag.src = "https://www.youtube.com/iframe_api";
    document.head.appendChild(tag);

    function onYouTubeIframeAPIReady() {
        loadSettings();
        player = new YT.Player('player', {
            videoId: extractId(document.getElementById('workUrl').value),
            playerVars: { 'origin': window.location.origin, 'rel': 0 },
            events: { 'onReady': () => { timeLeft = document.getElementById('workMins').value * 60; updateDisplay(); } }
        });
    }

    function extractId(url) {
        const m = url.match(/^.*((youtu.be\/)|(v\/)|(\/u\/\w\/)|(embed\/)|(watch\?))\??v?=?([^#&?]*).*/);
        return (m && m[7].length == 11) ? m[7] : "jfKfPfyJRdk";
    }

    function updateDisplay() {
        const m = Math.floor(timeLeft / 60);
        const s = timeLeft % 60;
        document.getElementById('timer').innerText = `${m}:${s.toString().padStart(2, '0')}`;
    }

    function setMode(mode) {
        currentState = mode;
        const b = document.body;
        const status = document.getElementById('status');
        b.classList.remove('work-mode', 'break-mode', 'warmup-mode');
        if (mode === 'WARMUP') { 
            b.classList.add('warmup-mode'); 
            status.innerText = "GET READY...";
            timeLeft = 10; player.pauseVideo(); 
        }
        else if (mode === 'WORKING') { 
            b.classList.add('work-mode'); 
            status.innerText = "/// WORK SESSION ///";
            timeLeft = document.getElementById('workMins').value * 60; player.loadVideoById(extractId(document.getElementById('workUrl').value)); 
        }
        else if (mode === 'BREAK') { 
            b.classList.add('break-mode'); 
            status.innerText = "/// BREAK TIME ///";
            player.loadVideoById(extractId(document.getElementById('breakUrl').value)); 
            if (document.getElementById('syncBreak').checked) setTimeout(() => { timeLeft = Math.floor(player.getDuration()); updateDisplay(); }, 1000);
            else timeLeft = document.getElementById('breakMins').value * 60;
        }
    }

    function startTimer() {
        if (!isPaused) return; isPaused = false; audioCtx.resume();
        if (currentState === 'IDLE') setMode('WARMUP');
        timerInterval = setInterval(() => {
            timeLeft--; updateDisplay();
            if (timeLeft === 3) playChime(660, 'triangle');
            if (timeLeft <= 0) {
                clearInterval(timerInterval); playChime(880);
                if (currentState === 'WARMUP') setMode(nextState);
                else if (currentState === 'WORKING') { nextState = 'BREAK'; setMode('WARMUP'); }
                else { nextState = 'WORKING'; setMode('WARMUP'); }
                isPaused = true; startTimer();
            }
        }, 1000);
    }

    document.getElementById('startBtn').addEventListener('click', startTimer);
    document.getElementById('pauseBtn').addEventListener('click', () => { isPaused = true; clearInterval(timerInterval); player.pauseVideo(); });
    document.getElementById('resetBtn').addEventListener('click', () => { isPaused = true; clearInterval(timerInterval); currentState = 'IDLE'; timeLeft = document.getElementById('workMins').value * 60; updateDisplay(); document.getElementById('status').innerText="READY?"; });
</script>
</body>
