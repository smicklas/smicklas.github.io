---
layout: null
permalink: /pomodoro/
title: pomodoro
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pastel Focus Flow</title>
    <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-gradient: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
            --work-accent: #ff9a9e;     
            --break-accent: #66a6ff;    
            --warmup-accent: #ffcc33;   
            --text-dark: #4a4a4a;
            --glass-bg: rgba(255, 255, 255, 0.3);
            --glass-border: 1px solid rgba(255, 255, 255, 0.4);
        }

        body {
            font-family: 'Quicksand', sans-serif;
            background: var(--bg-gradient);
            background-attachment: fixed;
            color: var(--text-dark);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }

        .glass-container {
            max-width: 850px;
            width: 100%;
            background: var(--glass-bg);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border-radius: 35px;
            border: var(--glass-border);
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
            padding: 30px;
            text-align: center;
            transition: all 0.3s ease;
        }

        .video-wrapper {
            border-radius: 20px;
            overflow: hidden;
            position: relative;
            padding-bottom: 56.25%;
            height: 0;
            background: #000;
            margin-bottom: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        }

        .video-wrapper iframe {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
        }

        .video-loader {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.6);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 10;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }

        .video-loader.active {
            opacity: 1;
            pointer-events: all;
        }

        .spinner {
            width: 50px;
            height: 50px;
            border: 5px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: #fff;
            animation: spin 1s ease-in-out infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        .timer-display {
            font-size: 5.5rem;
            font-weight: 700;
            margin: 10px 0;
            transition: color 0.5s ease;
        }

        .status-pill {
            display: inline-block;
            padding: 5px 20px;
            border-radius: 50px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 2px;
            font-size: 0.9rem;
            margin-bottom: 10px;
            background: rgba(255,255,255,0.4);
        }

        .glass-container.work-mode .timer-display { color: var(--work-accent); }
        .glass-container.break-mode .timer-display { color: var(--break-accent); }
        .glass-container.warmup-mode .timer-display { color: var(--warmup-accent); }

        button {
            padding: 12px 30px;
            font-family: 'Quicksand', sans-serif;
            font-weight: 700;
            border-radius: 50px;
            border: none;
            cursor: pointer;
            margin: 5px;
            transition: all 0.2s;
            font-size: 1rem;
        }

        #mainBtn { background: #fff; color: var(--text-dark); box-shadow: 0 4px 15px rgba(0,0,0,0.1); width: 200px;}
        #mainBtn:hover { transform: scale(1.05); }
        
        #mainBtn.paused-state {
            background: rgba(255, 255, 255, 0.8);
            color: var(--text-dark);
        }

        .settings-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 30px;
            text-align: left;
            background: rgba(255,255,255,0.2);
            padding: 20px;
            border-radius: 20px;
        }

        label { font-size: 0.85rem; font-weight: 700; display: block; margin-bottom: 5px; }
        input[type="text"], input[type="number"] {
            width: 100%; padding: 10px; border-radius: 10px; border: 1px solid rgba(255,255,255,0.5);
            background: rgba(255,255,255,0.5); margin-bottom: 10px; box-sizing: border-box;
        }

        .sync-row { display: flex; align-items: center; gap: 10px; margin-top: 10px; }
        .sync-row input { width: auto; margin-bottom: 0; }
        
        #error-banner {
            background: #ff6b6b;
            color: white;
            padding: 10px;
            border-radius: 10px;
            margin-bottom: 15px;
            display: none;
            font-size: 0.9rem;
        }
    </style>
</head>
<body class="work-mode">

<div class="glass-container work-mode" id="mainContainer">
    <div id="error-banner">⚠️ <strong>Video Error:</strong> This video cannot be played here.</div>
    
    <div class="status-pill" id="status">Ready?</div>
    <div class="timer-display" id="timer">25:00</div>

    <div class="video-wrapper">
        <div class="video-loader" id="videoLoader">
            <div class="spinner"></div>
        </div>
        <div id="player"></div>
    </div>

    <div class="controls">
        <button id="mainBtn">Start Session ✨</button>
        <button id="resetBtn" style="background: rgba(255,255,255,0.3)">Reset</button>
    </div>

    <div class="settings-grid">
        <div>
            <h3 style="color: var(--work-accent); margin-top: 0;">🌸 Work</h3>
            <label>YouTube URL (Update triggers preview)</label>
            <input type="text" id="workUrl" value="https://www.youtube.com/watch?v=jfKfPfyJRdk">
            <label>Minutes</label>
            <input type="number" id="workMins" value="25">
            
            <div class="sync-row">
                <input type="checkbox" id="titleToggle">
                <label for="titleToggle" style="margin-bottom:0">Show timer in Tab</label>
            </div>
        </div>
        <div>
            <h3 style="color: var(--break-accent); margin-top: 0;">🐬 Break</h3>
            <label>YouTube URL</label>
            <input type="text" id="breakUrl" value="https://www.youtube.com/watch?v=5qap5aO4i9A">
            
            <div id="breakTimeInput">
                <label>Minutes</label>
                <input type="number" id="breakMins" value="5">
            </div>

            <div class="sync-row">
                <input type="checkbox" id="syncBreak">
                <label for="syncBreak" style="margin-bottom:0">Match video length</label>
            </div>
        </div>
    </div>
</div>

<script>
    // --- 1. LOCAL STORAGE LOGIC (LOAD FIRST) ---
    function loadPreferences() {
        if(localStorage.getItem('pastel_workUrl')) document.getElementById('workUrl').value = localStorage.getItem('pastel_workUrl');
        if(localStorage.getItem('pastel_workMins')) document.getElementById('workMins').value = localStorage.getItem('pastel_workMins');
        if(localStorage.getItem('pastel_breakUrl')) document.getElementById('breakUrl').value = localStorage.getItem('pastel_breakUrl');
        if(localStorage.getItem('pastel_breakMins')) document.getElementById('breakMins').value = localStorage.getItem('pastel_breakMins');
        
        if(localStorage.getItem('pastel_syncBreak') !== null) {
            const isChecked = localStorage.getItem('pastel_syncBreak') === 'true';
            document.getElementById('syncBreak').checked = isChecked;
            document.getElementById('breakTimeInput').style.opacity = isChecked ? "0.3" : "1";
        }
        if(localStorage.getItem('pastel_titleToggle') !== null) {
             document.getElementById('titleToggle').checked = localStorage.getItem('pastel_titleToggle') === 'true';
        }
    }

    function savePreferences() {
        localStorage.setItem('pastel_workUrl', document.getElementById('workUrl').value);
        localStorage.setItem('pastel_workMins', document.getElementById('workMins').value);
        localStorage.setItem('pastel_breakUrl', document.getElementById('breakUrl').value);
        localStorage.setItem('pastel_breakMins', document.getElementById('breakMins').value);
        localStorage.setItem('pastel_syncBreak', document.getElementById('syncBreak').checked);
        localStorage.setItem('pastel_titleToggle', document.getElementById('titleToggle').checked);
    }

    // Load saved settings immediately before Player loads
    loadPreferences();
    
    // Attach save listeners to all inputs
    ['workUrl', 'workMins', 'breakUrl', 'breakMins', 'syncBreak', 'titleToggle'].forEach(id => {
        document.getElementById(id).addEventListener('change', savePreferences);
    });

    // --- 2. MAIN APP VARIABLES ---
    let player;
    let timerInterval;
    let timeLeft;
    let currentState = 'IDLE'; 
    let nextState = 'WORKING'; 
    let isPaused = true; 
    const defaultTitle = document.title;

    // --- 3. AUDIO SYSTEM ---
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    
    function playChime(freq = 440, type = 'sine') {
        if(audioCtx.state === 'suspended') audioCtx.resume();
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.type = type;
        osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
        gain.gain.setValueAtTime(0, audioCtx.currentTime);
        gain.gain.linearRampToValueAtTime(0.1, audioCtx.currentTime + 0.01);
        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 1);
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        osc.start();
        osc.stop(audioCtx.currentTime + 1);
    }

    // --- 4. YOUTUBE API ---
    var tag = document.createElement('script');
    tag.src = "https://www.youtube.com/iframe_api";
    var firstScriptTag = document.getElementsByTagName('script')[0];
    firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

    function onYouTubeIframeAPIReady() {
        player = new YT.Player('player', {
            height: '100%', width: '100%',
            videoId: extractId(document.getElementById('workUrl').value),
            playerVars: { 
                'rel': 0 // removed 'origin' to help with local file playback
            },
            events: { 
                'onReady': () => { 
                    timeLeft = document.getElementById('workMins').value * 60; 
                    updateDisplay(); 
                },
                'onError': onPlayerError,
                'onStateChange': onPlayerStateChange
            }
        });
    }

    function onPlayerStateChange(event) {
        const loader = document.getElementById('videoLoader');
        if (event.data === YT.PlayerState.BUFFERING || event.data === -1) {
            loader.classList.add('active');
        } else if (event.data === YT.PlayerState.PLAYING || event.data === YT.PlayerState.PAUSED || event.data === YT.PlayerState.CUED) {
            loader.classList.remove('active');
        }
    }

    function onPlayerError(e) {
        if([101, 150, 153].includes(e.data)){
            document.getElementById('error-banner').style.display = 'block';
            document.getElementById('videoLoader').classList.remove('active');
        }
    }

    function extractId(url) {
        if (!url) return null;
        const match = url.match(/^.*((youtu.be\/)|(v\/)|(\/u\/\w\/)|(embed\/)|(watch\?))\??v?=?([^#&?]*).*/);
        return (match && match[7].length == 11) ? match[7] : null;
    }

    // --- 5. LOGIC & EVENTS ---

    // Immediate URL Updates
    document.getElementById('workUrl').addEventListener('change', (e) => {
        const newId = extractId(e.target.value);
        if (newId && player && typeof player.cueVideoById === 'function') {
            player.cueVideoById(newId); 
            document.getElementById('videoLoader').classList.add('active'); 
        }
    });

    document.getElementById('breakUrl').addEventListener('change', (e) => {
        if (currentState === 'BREAK') {
            const newId = extractId(e.target.value);
            if (newId && player) player.cueVideoById(newId);
        }
    });

    function updateDisplay() {
        const mins = Math.floor(timeLeft / 60);
        const secs = timeLeft % 60;
        const formattedTime = `${mins}:${secs.toString().padStart(2, '0')}`;
        document.getElementById('timer').innerText = formattedTime;

        if(document.getElementById('titleToggle').checked && currentState !== 'IDLE') {
            let emoji = currentState === 'WORKING' ? "🌸" : (currentState === 'BREAK' ? "🐬" : "✨");
            document.title = `(${formattedTime}) ${emoji} ${currentState}`;
        } else {
            document.title = defaultTitle;
        }
    }

    function setMode(mode) {
        currentState = mode;
        const container = document.getElementById('mainContainer');
        const status = document.getElementById('status');
        
        container.classList.remove('work-mode', 'break-mode', 'warmup-mode');

        if (mode === 'WARMUP') {
            container.classList.add('warmup-mode');
            status.innerText = "Get Ready... ✨";
            timeLeft = 10;
            player.pauseVideo();
        } else if (mode === 'WORKING') {
            container.classList.add('work-mode');
            status.innerText = "Deep Work 🌸";
            timeLeft = document.getElementById('workMins').value * 60;
            
            const vidId = extractId(document.getElementById('workUrl').value);
            document.getElementById('videoLoader').classList.add('active');
            player.loadVideoById(vidId);
            
        } else if (mode === 'BREAK') {
            container.classList.add('break-mode');
            status.innerText = "Rest & Recharge 🐬";
            
            const breakVidId = extractId(document.getElementById('breakUrl').value);
            document.getElementById('videoLoader').classList.add('active');
            player.loadVideoById(breakVidId);

            if (document.getElementById('syncBreak').checked) {
                setTimeout(() => {
                    timeLeft = Math.floor(player.getDuration());
                    updateDisplay();
                }, 1000);
            } else {
                timeLeft = document.getElementById('breakMins').value * 60;
            }
        }
        updateDisplay();
    }

    function toggleTimer() {
        const btn = document.getElementById('mainBtn');

        if (isPaused) {
            // ACTION: START
            isPaused = false;
            
            if(audioCtx.state === 'suspended') audioCtx.resume();
            playChime(1200, 'sine'); 
            
            btn.innerText = "Pause ⏸";
            btn.classList.add('paused-state');
            document.getElementById('error-banner').style.display = 'none';

            if (currentState === 'IDLE') setMode('WARMUP');

            updateDisplay(); 
            
            timerInterval = setInterval(() => {
                timeLeft--;
                updateDisplay();

                if (timeLeft === 3) playChime(660, 'triangle'); 

                if (timeLeft <= 0) {
                    clearInterval(timerInterval);
                    playChime(880, 'sine'); 
                    
                    if (currentState === 'WARMUP') {
                        setMode(nextState);
                        isPaused = true; toggleTimer(); 
                    } else if (currentState === 'WORKING') {
                        nextState = 'BREAK';
                        setMode('WARMUP');
                        isPaused = true; toggleTimer();
                    } else if (currentState === 'BREAK') {
                        nextState = 'WORKING';
                        setMode('WARMUP');
                        isPaused = true; toggleTimer();
                    }
                }
            }, 1000);

        } else {
            // ACTION: PAUSE
            isPaused = true;
            clearInterval(timerInterval);
            player.pauseVideo();
            
            document.getElementById('status').innerText = "Paused ⏸";
            btn.innerText = "Resume ▶";
            btn.classList.remove('paused-state');
        }
    }

    document.getElementById('mainBtn').addEventListener('click', toggleTimer);
    document.getElementById('resetBtn').addEventListener('click', () => location.reload());

    document.getElementById('syncBreak').addEventListener('change', (e) => {
        document.getElementById('breakTimeInput').style.opacity = e.target.checked ? "0.3" : "1";
    });
    
    document.getElementById('titleToggle').addEventListener('change', updateDisplay);
</script>
</body>
</html>
