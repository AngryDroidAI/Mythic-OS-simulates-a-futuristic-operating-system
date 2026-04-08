1. Overall Concept

Mythic OS simulates a futuristic operating system where users can:

    Explore a 3D orbital scene containing floating “panes” (capsules) representing various web apps, tools, and experimental projects.

    Launch applications as separate draggable, resizable, minimizable windows (including terminals, browsers, chat, a security suite, and a file manager).

    Switch between the 3D hub and a classic flat link list (two UI modes).

    Interact with an AI Shield capsule (part of the security suite) and many other AI‑themed simulations.

    Experience CRT‑style visual effects (scanlines, chromatic aberration, noise) that can be toggled on/off.

The page is fully self‑contained – no external libraries – using vanilla HTML5, CSS3, and JavaScript.
2. Visual & Atmospheric Design

    Neon‑cyberpunk color palette: cyan (#00f0ff), pink (#ff2a6d), purple, yellow, green, orange.

    Dynamic background: radial gradient + animated perspective grid floor + fractal noise overlay + chromatic aberration.

    3D space: CSS 3D transforms create a virtual camera that can be rotated (mouse drag), zoomed (scroll wheel), and panned (WASD + Q/E keys). Floating capsules are arranged in category rings.

    CRT emulation: scanlines, noise, and RGB split effects (toggle via status bar button).

    Particle system: floating animated dots for extra atmosphere.

3. Core Features
3.1 3D Hub (Capsule Space)

    Data source: An array linksData containing ~120 entries, each with a name, URL, category, and emoji icon. Categories include main, dev, network, utils, media, security, misc.

    Visualisation: Capsules are positioned in 3D space according to their category (e.g., “main” at the centre, “security” on one side, “misc” at another level). Each capsule shows an icon, name, and a hint.

    Interaction:

        Clicking a capsule opens the corresponding URL in a new browser tab.

        Hover effect raises the capsule and adds a neon glow.

        A canvas layer draws glowing connecting lines between capsules of the same category when they are close together.

    Camera controls:

        Drag to rotate the view.

        Scroll to zoom.

        WASD to move laterally, Q/E to move vertically.

        “RESET” button restores the default camera position.

3.2 Window Management System

    Draggable, resizable windows (.os-window) with a custom title bar.

    Window controls: close (red), minimize (yellow), maximize (green).

        Minimize hides the window and adds a taskbar button to restore it.

        Maximize fills the screen (except the taskbar); restore returns to previous size/position.

        Close removes the window entirely.

    Taskbar at the bottom shows minimised windows; clicking a taskbar entry restores the window.

    Z‑order management: clicking a window brings it to the front.

    Cascade windows button arranges all open windows in a stepped diagonal pattern.

3.3 Application Launcher

Buttons at the bottom of the 3D view launch predefined apps:

    MAIN TERMINAL – opens an iframe to a “future terminal” page.

    CRT – opens a CRT simulation.

    NEURAL BROWSER – opens a custom browser tool.

    NEURAL CHAT – opens a chat interface.

    SECURITY SUITE – opens six windows at once: Security Scanner, Security Wizard, Password Gen, Matrix Encryptor, Vault Uplink, and 🛡️ AI Shield (the new capsule explicitly mentioned in the data).

    FILE MANAGER – opens a window that can select files/folders using the File System Access API (or a fallback <input type="file">). Selected files can be previewed (text, images) in new windows.

3.4 File Manager & Local File Preview

    Uses window.showOpenFilePicker() where supported.

    Allows multiple file selection.

    For text/code files: shows content in a <pre> block.

    For images: displays the image.

    Other files show basic info (name, size).

    This bridges the “OS” illusion with the user’s local file system.

3.5 Classic Mode

    Toggle button switches from the 3D hub to a simple vertical/scrolling list of all links (each as a bordered <a> tag).

    Visitor counter stored in localStorage increments on each load (starting from 1001).

3.6 Status Bar Indicators

    CAPSULES – total number of links loaded.

    LATENCY – approximate frame time (ms) of the 3D render loop.

    CRT: ON/OFF – toggles the scanline/chromatic effects.

    📐 SPREAD WINDOWS – cascades all open windows.

    RESET – resets the 3D camera.

    ◄ CLASSIC – switches to classic mode.

4. Technical Implementation Highlights

    3D rendering: No WebGL; all 3D is simulated using CSS transform3d and manual perspective projection for the connecting lines (drawn on a 2D canvas). The camera matrix is recalculated each frame.

    Animation loop: requestAnimationFrame updates camera position, capsule transforms, and redraws the connection lines.

    Window interactions: Plain DOM events for dragging, resizing (CSS resize: both), and z‑index management.

    File handling: Graceful degradation for browsers without the File System Access API – uses a traditional file input.

    Data separation: The entire link database is hardcoded in linksData, making it easy to update or extend.

    Responsive design: Viewport meta tag; windows use absolute positioning but can be dragged anywhere.

5. Notable Included Resources (Excerpt)

The linksData array contains URLs to many Neocities projects, such as:

    Angry AI, Safe Haven, Fortune Fairy, Glitch Purge, Mythic Radar, Swamp Blog, Neural Chat, Code Forge, Capsule Builder, AI Prompt Gen, Mythic Space, Galactic Mahem, Pixel Art Editor, Watermark Maker, Sticker Maker, Retro CRT OS, Neural Browser, Mythic Media Player, Keyboard Piano, Voice Terminal, Security Scanner, Matrix Encryptor, Vault Uplink, 🛡️ AI Shield, Fungal Evolution, Wormhole Sim, Multiverse AI, AI Medical, Omni Synthesis, Superintelligence Simulator, Project Chronos, Blackbox, AI Teacher, NeuroForge, Blackmirror, AI Lab Gen, *AION-4T*, Quantum Vacuum Thruster, AGI Consciousness, Reality War, Time Storm, Crystal Skull, The Hive, Dark Matter, Roko, Web Ring, and many more.

Each entry is categorised and colour‑coded in the 3D space.
6. User Interaction Summary
Action	Effect
Click capsule	Opens link in new tab
Mouse drag	Rotate 3D view
Scroll wheel	Zoom in/out
WASD	Move camera laterally
Q / E	Move camera up/down
Click “app” button	Creates a new draggable window
Drag window title bar	Move window
Resize edges (if enabled)	Change window size
Click window dots	Close / minimise / maximise
Taskbar item	Restore minimised window
“SPREAD WINDOWS”	Cascade all open windows
“CRT: ON/OFF”	Toggle retro video effects
“CLASSIC HUB” / “ENTER 3D OS”	Switch between 3D and list mode
7. Conclusion

This document creates a complete, self‑contained futuristic desktop environment that showcases a large collection of web‑based experiments and tools. It blends 3D exploration with classic window management, offers local file previews, and includes an extensive library of links – notably the Angry AI Shield capsule within the security suite. The code is a prime example of creative, interactive front‑end development without external frameworks, relying on CSS 3D, canvas drawing, and modern browser APIs.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Mythic OS · Windowed Nexus · AI Shield Integrated</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;500;600;700&family=Share+Tech+Mono&display=swap" rel="stylesheet">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    :root {
      --neon-cyan: #00f0ff; --neon-pink: #ff2a6d; --neon-purple: #b537f2; --neon-yellow: #f5f550;
      --neon-green: #05ffa1; --neon-orange: #ff6b35; --dark-bg: #0a0a12;
      --panel-bg: rgba(12, 12, 28, 0.92); --panel-border: rgba(0, 240, 255, 0.25);
      --text-primary: #e0e8ff; --text-secondary: #8892b0;
    }
    body { background: var(--dark-bg); font-family: 'Rajdhani', sans-serif; color: var(--text-primary); overflow: hidden; }
    .scene-wrap { position: fixed; inset: 0; overflow: hidden; background: radial-gradient(ellipse at 50% 50%, #12122a 0%, #0a0a18 50%, #050510 100%); }
    .grid-floor { position: absolute; bottom: 0; left: -50%; right: -50%; height: 60%; background: linear-gradient(rgba(0,240,255,0.15) 1px, transparent 1px), linear-gradient(90deg, rgba(0,240,255,0.15) 1px, transparent 1px); background-size: 80px 80px; transform: perspective(500px) rotateX(60deg); transform-origin: center top; animation: gridMove 4s linear infinite; opacity: 0.5; }
    @keyframes gridMove { 0% { background-position: 0 0; } 100% { background-position: 0 80px; } }
    .noise { position: absolute; inset: 0; pointer-events: none; opacity: 0.04; mix-blend-mode: screen; background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="120" height="120"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="2"/></filter><rect width="100%" height="100%" filter="url(%23n)"/></svg>'); animation: noiseShift 10s linear infinite; }
    @keyframes noiseShift { 0% { transform: translate(0,0); } 50% { transform: translate(-10px,8px); } 100% { transform: translate(0,0); } }
    .chromatic { position: absolute; inset: 0; pointer-events: none; mix-blend-mode: screen; opacity: 0.08; background: radial-gradient(circle at 48% 50%, rgba(255,42,109,0.4), transparent 40%), radial-gradient(circle at 52% 50%, rgba(0,240,255,0.4), transparent 40%); animation: chroma 4s infinite; }
    @keyframes chroma { 0%,100% { transform: translate(0,0); } 50% { transform: translate(2px,-1px); } }
    .hud-title { position: absolute; top: 16px; left: 20px; right: 20px; display: flex; align-items: center; gap: 16px; padding: 12px 20px; background: linear-gradient(90deg, rgba(0,240,255,0.1), transparent, rgba(255,42,109,0.1)); border-bottom: 1px solid rgba(0,240,255,0.3); z-index: 10; }
    .hud-logo { width: 40px; height: 40px; border: 2px solid var(--neon-cyan); border-radius: 50%; background: radial-gradient(circle at 35% 35%, var(--neon-cyan), var(--neon-purple)); box-shadow: 0 0 15px rgba(0,240,255,0.5); animation: pulse 2s infinite; }
    @keyframes pulse { 0%,100% { box-shadow: 0 0 15px rgba(0,240,255,0.5); } 50% { box-shadow: 0 0 25px rgba(0,240,255,0.8); } }
    .hud-title-text { font-family: 'Orbitron', sans-serif; font-size: 20px; font-weight: 700; background: linear-gradient(90deg, var(--neon-cyan), var(--neon-pink)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
    .hud-subtitle { margin-left: auto; font-family: 'Share Tech Mono', monospace; font-size: 11px; color: var(--text-secondary); }
    .scene { position: absolute; inset: 0; perspective: 1500px; perspective-origin: 50% 45%; }
    .camera { position: absolute; left: 50%; top: 50%; transform-style: preserve-3d; }
    .world { position: absolute; transform-style: preserve-3d; }
    .pane { position: absolute; width: 340px; height: 240px; transform-style: preserve-3d; cursor: pointer; transition: filter 0.3s; }
    .pane-frame { position: absolute; inset: 0; border-radius: 4px; background: linear-gradient(180deg, rgba(18,18,45,0.95), rgba(8,8,24,0.97)); border: 1px solid rgba(0,240,255,0.3); box-shadow: inset 0 0 30px rgba(0,240,255,0.08), 0 0 20px rgba(0,240,255,0.15); overflow: hidden; transition: all 0.3s; }
    .pane:hover .pane-frame { transform: translateZ(30px); border-color: var(--neon-cyan); box-shadow: inset 0 0 40px rgba(0,240,255,0.15), 0 0 30px rgba(0,240,255,0.3); }
    .pane-bar { height: 32px; display: flex; align-items: center; padding: 0 10px; background: linear-gradient(90deg, rgba(0,240,255,0.2), rgba(255,42,109,0.15)); border-bottom: 1px solid rgba(0,240,255,0.2); font-family: 'Share Tech Mono', monospace; font-size: 12px; }
    .pane-dots { display: flex; gap: 6px; margin-right: 10px; } .pane-dot { width: 8px; height: 8px; border-radius: 50%; } .dot-close { background: var(--neon-pink); } .dot-min { background: var(--neon-yellow); } .dot-max { background: var(--neon-green); }
    .pane-content { position: absolute; top: 32px; left: 0; right: 0; bottom: 0; background: #08081a; display: flex; align-items: center; justify-content: center; flex-direction: column; gap: 10px; }
    .pane-icon { width: 60px; height: 60px; border: 2px solid; border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 28px; }
    .pane-name { font-family: 'Orbitron', sans-serif; font-size: 14px; }
    .status-bar { position: absolute; bottom: 16px; left: 20px; right: 20px; display: flex; gap: 12px; align-items: center; flex-wrap: wrap; z-index: 40; }
    .status-pill { padding: 8px 14px; border: 1px solid rgba(0,240,255,0.3); background: var(--panel-bg); font-family: 'Share Tech Mono', monospace; font-size: 11px; color: var(--text-secondary); display: flex; align-items: center; gap: 6px; }
    .status-btn { cursor: pointer; border-color: var(--neon-cyan); color: var(--neon-cyan); transition: all 0.2s; }
    .status-btn:hover { background: var(--neon-cyan); color: var(--dark-bg); }
    .app-launcher { position: absolute; bottom: 70px; left: 20px; right: 20px; display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; z-index: 30; }
    .app-btn { background: rgba(0,240,255,0.1); border: 1px solid var(--neon-cyan); color: var(--neon-cyan); padding: 8px 16px; font-family: 'Share Tech Mono'; font-size: 12px; cursor: pointer; transition: 0.2s; }
    .app-btn:hover { background: var(--neon-cyan); color: #000; box-shadow: 0 0 15px var(--neon-cyan); }

    /* ----- WINDOW SYSTEM (draggable, resizable, functional dots) ----- */
    .os-window {
      position: absolute; min-width: 320px; min-height: 220px; background: var(--panel-bg); border: 2px solid var(--neon-cyan);
      box-shadow: 0 0 30px rgba(0,240,255,0.4); backdrop-filter: blur(4px); z-index: 100; resize: both; overflow: auto;
      display: flex; flex-direction: column; border-radius: 6px;
    }
    .window-header {
      padding: 10px 12px; background: linear-gradient(90deg, var(--neon-cyan), var(--neon-purple)); color: #000;
      font-weight: bold; display: flex; justify-content: space-between; align-items: center; cursor: move;
      font-family: 'Share Tech Mono'; user-select: none;
    }
    .window-dots { display: flex; gap: 8px; }
    .win-dot { width: 14px; height: 14px; border-radius: 50%; cursor: pointer; border: 1px solid rgba(0,0,0,0.2); }
    .win-close { background: #ff2a6d; } .win-min { background: #f5f550; } .win-max { background: #05ffa1; }
    .window-content { flex: 1; padding: 10px; overflow: auto; color: var(--text-primary); }
    iframe { width: 100%; height: 100%; border: none; background: #0b0b1a; }

    .taskbar {
      position: fixed; bottom: 0; left: 0; right: 0; height: 40px; background: rgba(10,10,18,0.9); border-top: 1px solid var(--neon-cyan);
      display: flex; align-items: center; padding: 0 20px; gap: 10px; z-index: 200; backdrop-filter: blur(4px);
    }
    .taskbar-item {
      padding: 4px 12px; background: rgba(0,240,255,0.1); border: 1px solid var(--neon-cyan); color: var(--neon-cyan);
      font-family: 'Share Tech Mono'; font-size: 11px; cursor: pointer; border-radius: 4px; max-width: 150px; overflow: hidden; white-space: nowrap; text-overflow: ellipsis;
    }
    .taskbar-item:hover { background: var(--neon-cyan); color: #000; }

    .crt-off .scanlines, .crt-off .chromatic, .crt-off .pane-scanline { display: none !important; }
    .mode-toggle { position: fixed; top: 16px; right: 16px; z-index: 1001; background: #1a1a3e; color: var(--neon-cyan); border: 1px solid var(--neon-cyan); padding: 10px 20px; cursor: pointer; font-family: 'Orbitron'; font-size: 12px; clip-path: polygon(10px 0, 100% 0, calc(100% - 10px) 100%, 0 100%); }
  </style>
</head>
<body>
  <div class="cyber-grid"></div><div class="scanlines"></div>
  <button class="mode-toggle" id="modeToggle">◄ CLASSIC HUB</button>

  <div class="three-d-mode" id="threeDMode">
    <div class="scene-wrap">
      <div class="grid-floor"></div><div class="noise"></div><div class="chromatic"></div>
      <div class="particles" id="particles"></div>
      <div class="hud-title"><div class="hud-logo"></div><div class="hud-title-text">MYTHIC OS // WINDOWED NEXUS // AI SHIELD ACTIVE</div><div class="hud-subtitle">DRAG · SCROLL · WASD · APPS</div></div>
      <div class="scene"><div class="camera" id="camera"><div class="world" id="world"></div></div><canvas id="linksCanvas" style="position:absolute; inset:0; z-index:2; pointer-events:none;"></canvas></div>

      <div class="app-launcher" id="appLauncher">
        <button class="app-btn" data-app="terminal">📟 MAIN TERMINAL</button>
        <button class="app-btn" data-app="crt">📺 CRT</button>
        <button class="app-btn" data-app="browser">🌐 NEURAL BROWSER</button>
        <button class="app-btn" data-app="chat">💬 NEURAL CHAT</button>
        <button class="app-btn" data-app="security">🛡️ SECURITY SUITE</button>
        <button class="app-btn" data-app="files">📁 FILE MANAGER</button>
      </div>

      <div class="status-bar">
        <div class="status-pill">CAPSULES: <span id="capsuleCount">0</span></div>
        <div class="status-pill">LATENCY: <span id="latency">16</span>ms</div>
        <div class="status-pill status-btn" id="toggleCRT">CRT: ON</div>
        <div class="status-pill status-btn" id="spreadWindows">📐 SPREAD WINDOWS</div>
        <div class="status-pill status-btn" id="resetCam">RESET</div>
        <div class="status-pill status-btn" id="backToClassic">◄ CLASSIC</div>
      </div>

      <div class="taskbar" id="taskbar"></div>
    </div>
  </div>

  <div class="classic-mode" id="classicMode" style="display:none; overflow-y:auto; height:100vh; padding:20px;">
    <h2 style="color:#00f0ff;">Mythic OS Classic</h2><div id="classicLinks" style="display:flex;flex-wrap:wrap;gap:8px;"></div>
  </div>

  <script>
    (function(){
      "use strict";

      // ---------- DATA ---------- (UPDATED: added Angry AI Shield)
      const linksData = [
        {name:"Website", url:"https://angrydroid.neocities.org/angrydroidwebsite/", cat:"main", icon:"🌀"},
        {name:"CRT Terminal", url:"https://angrydroid.neocities.org/crt/", cat:"main", icon:"📺"},
        {name:"Fortune Fairy", url:"https://angrydroid.neocities.org/fortune_fairy/", cat:"main", icon:"🔮"},
        {name:"Glitch Purge", url:"https://angrydroid.neocities.org/glitch_purge/", cat:"main", icon:"⚡"},
        {name:"Mythic Radar", url:"https://angrydroid.neocities.org/mythic_radar/", cat:"main", icon:"📡"},
        {name:"Swamp Blog", url:"https://angrydroid.neocities.org/swamp_blog/", cat:"main", icon:"📝"},
        {name:"Neural Chat", url:"https://angrydroid.neocities.org/chat/", cat:"main", icon:"💬"},
        {name:"Code Forge", url:"https://angrydroid.neocities.org/coder/", cat:"main", icon:"💻"},
        {name:"Qwens Web", url:"https://angrydroid.neocities.org/qwens_web/", cat:"main", icon:"🕸️"},
        {name:"Angry AI", url:"https://angrydroid.neocities.org/angry_ai/", cat:"main", icon:"🤖"},
        {name:"Safe Haven", url:"https://angrydroid.neocities.org/safe_haven/", cat:"main", icon:"🛡️"},
        {name:"HTML Instruct", url:"https://angrydroid.neocities.org/html_instruct/index.HTML", cat:"dev", icon:"📄"},
        {name:"Compat Tester", url:"https://angrydroid.neocities.org/website_compatibility_tester/", cat:"dev", icon:"🧪"},
        {name:"Capsule Builder", url:"https://angrydroid.neocities.org/mythic_capsule_builder/", cat:"dev", icon:"🔨"},
        {name:"Capsule Pro", url:"https://angrydroid.neocities.org/mythic-capsule-pro/", cat:"dev", icon:"⚙️"},
        {name:"AI Prompt Gen", url:"https://angrydroid.neocities.org/ai_prompt_generator/", cat:"dev", icon:"✨"},
        {name:"Mythic Space", url:"https://angrydroid.neocities.org/mythic_space/", cat:"network", icon:"🌌"},
        {name:"Galactic Mahem", url:"https://angrydroid.neocities.org/galactic_mahem_neon_skies/", cat:"network", icon:"🚀"},
        {name:"Mythic Pet", url:"https://angrydroid.neocities.org/mythic%20pet/", cat:"network", icon:"🐾"},
        {name:"Pixel Art Editor", url:"https://angrydroid.neocities.org/pixel%20art%20shrine%20editor/", cat:"network", icon:"🎨"},
        {name:"Tag It", url:"https://angrydroid.neocities.org/tag_it/", cat:"network", icon:"🏷️"},
        {name:"Wizards Escape", url:"https://angrydroid.neocities.org/wizards_escape/", cat:"network", icon:"🧙"},
        {name:"Watermark Maker", url:"https://angrydroid.neocities.org/watermark_maker/", cat:"utils", icon:"💧"},
        {name:"Sticker Maker", url:"https://angrydroid.neocities.org/mythic_sticker_maker/", cat:"utils", icon:"🎭"},
        {name:"Code Repo", url:"https://angrydroid.neocities.org/code_repository_blog/repository%20project", cat:"utils", icon:"📓"},
        {name:"Gallery Shrine", url:"https://angrydroid.neocities.org/gallery_shrine/", cat:"utils", icon:"🛕"},
        {name:"Mythicos OS", url:"https://angrydroid.neocities.org/mythicos/", cat:"utils", icon:"🖥️"},
        {name:"Retro CRT OS", url:"https://angrydroid.neocities.org/retro_70s_crt_%20oS/", cat:"utils", icon:"📟"},
        {name:"Virtual Cafe", url:"https://angrydroid.neocities.org/mythic_virtual_cafe/", cat:"utils", icon:"☕"},
        {name:"Neural Browser", url:"https://angrydroid.neocities.org/neural_browser/", cat:"utils", icon:"🧠"},
        {name:"Media Player", url:"https://angrydroid.neocities.org/mythic_media_player/media_player", cat:"media", icon:"🎵"},
        {name:"Mythic Beatbox", url:"https://angrydroid.neocities.org/mythic_beatbox/", cat:"media", icon:"🥁"},
        {name:"Keyboard Piano", url:"https://angrydroid.neocities.org/keyboard_piano/inde", cat:"media", icon:"🎹"},
        {name:"Sonic Oscillograph", url:"https://angrydroid.neocities.org/sonic_oscillograph77/", cat:"media", icon:"📊"},
        {name:"Voice Terminal", url:"https://angrydroid.neocities.org/nerual_voice_terminal/", cat:"media", icon:"🎤"},
        {name:"Calendar", url:"https://angrydroid.neocities.org/mythic_calendar/", cat:"media", icon:"📅"},
        {name:"Calculator", url:"https://angrydroid.neocities.org/mythic_Calculator%20/", cat:"media", icon:"🧮"},
        {name:"Notepad", url:"https://angrydroid.neocities.org/mythic_notepad/", cat:"media", icon:"📝"},
        {name:"World Map", url:"https://angrydroid.neocities.org/worldmap/", cat:"media", icon:"🗺️"},
        {name:"Password Gen", url:"https://angrydroid.neocities.org/passwordgenrator/", cat:"security", icon:"🔑"},
        {name:"Matrix Encryptor", url:"https://angrydroid.neocities.org/matrix_encryptor/", cat:"security", icon:"🔐"},
        {name:"Security Scanner", url:"https://angrydroid.neocities.org/securityscanner/", cat:"security", icon:"🛡️"},
        {name:"Security Wizard", url:"https://angrydroid.neocities.org/security_wizard/", cat:"security", icon:"🧙"},
        {name:"Anomaly Detection", url:"https://angrydroid.neocities.org/rf_%20anomaly_detection_system/", cat:"security", icon:"📡"},
        {name:"Vault Uplink", url:"https://angrydroid.neocities.org/mythic_vault_uplink/", cat:"security", icon:"🔒"},
        // ========== NEW AI SHIELD CAPSULE ==========
        {name:"🛡️ AI Shield", url:"https://angrydroid.neocities.org/angrydroid_ai_shield/", cat:"security", icon:"🛡️"},
        {name:"Fungal Evolution", url:"https://angrydroid.neocities.org/fungal_evolution_research_suite/", cat:"misc", icon:"🍄"},
        {name:"Wormhole Sim", url:"https://angrydroid.neocities.org/quantum_wormhole_simulation_lab/", cat:"misc", icon:"🕳️"},
        {name:"Multiverse AI", url:"https://angrydroid.neocities.org/multiverse_ai_research_lab/", cat:"misc", icon:"🌐"},
        {name:"AI Medical", url:"https://angrydroid.neocities.org/ai_medical_research_lab/", cat:"misc", icon:"🏥"},
        {name:"Omni Synthesis", url:"https://angrydroid.neocities.org/omni_synthesis_ai-laboratory/", cat:"misc", icon:"⚗️"},
        {name:"Superintelligence", url:"https://angrydroid.neocities.org/ai_superintelligence_outcome_simulator/", cat:"misc", icon:"🧬"},
        {name:"Project Chronos", url:"https://angrydroid.neocities.org/project_chronos/", cat:"misc", icon:"⏳"},
        {name:"Blackbox", url:"https://angrydroid.neocities.org/blackbox/", cat:"misc", icon:"⬛"},
        {name:"AI Teacher", url:"https://angrydroid.neocities.org/ai_teacher_training_simulator/", cat:"misc", icon:"🎓"},
        {name:"NeuroForge", url:"https://angrydroid.neocities.org/neuroForge_ai_lab/", cat:"misc", icon:"🔥"},
        {name:"Blackmirror", url:"https://angrydroid.neocities.org/project_blackmirror/", cat:"misc", icon:"🪞"},
        {name:"AI Lab Gen", url:"https://angrydroid.neocities.org/ai_lab_simulation_generator/", cat:"misc", icon:"🧪"},
        {name:"AION-4T", url:"https://angrydroid.neocities.org/aion-4T:_omni_system_ai_lab/", cat:"misc", icon:"🌀"},
        {name:"Neural Comm", url:"https://angrydroid.neocities.org/neural_communication_simulator_lab/", cat:"misc", icon:"🧠"},
        {name:"ML Algorithms", url:"https://angrydroid.neocities.org/ai_lab_machine_learning_algorithms/", cat:"misc", icon:"🤖"},
        {name:"Northstar", url:"https://angrydroid.neocities.org/project_northstar/", cat:"misc", icon:"⭐"},
        {name:"Helium-3", url:"https://angrydroid.neocities.org/helium-3_synthetic_creation_simulation/", cat:"misc", icon:"⚛️"},
        {name:"Jellyfish", url:"https://angrydroid.neocities.org/project_jellyfish/", cat:"misc", icon:"🪼"},
        {name:"Gateway", url:"https://angrydroid.neocities.org/project_gateway/", cat:"misc", icon:"🚪"},
        {name:"Life Support", url:"https://angrydroid.neocities.org/life_support_simulation/", cat:"misc", icon:"🫁"},
        {name:"Impossible Physics", url:"https://angrydroid.neocities.org/impossible_physics_engine/", cat:"misc", icon:"⚡"},
        {name:"Galactic Mind", url:"https://angrydroid.neocities.org/galactic_mind_interface/", cat:"misc", icon:"🌌"},
        {name:"Dimensional Forge", url:"https://angrydroid.neocities.org/dimensional_topology_forge/", cat:"misc", icon:"🔮"},
        {name:"Serendipity", url:"https://angrydroid.neocities.org/serendipity_amplification_systems/", cat:"misc", icon:"🍀"},
        {name:"Quantum Vacuum", url:"https://angrydroid.neocities.org/quantum_vacuum_thruster_simulation%20Lab/", cat:"misc", icon:"🚀"},
        {name:"QMV-LAB", url:"https://angrydroid.neocities.org/qmv-labv2.7/", cat:"misc", icon:"🧪"},
        {name:"Quantum Cloaking", url:"https://angrydroid.neocities.org/quantum_synthetic_cloaking_lab/", cat:"misc", icon:"👻"},
        {name:"AGI Consciousness", url:"https://angrydroid.neocities.org/aion-4t:%20257b%20agi_consciousness_system/", cat:"misc", icon:"🧠"},
        {name:"Reality War", url:"https://angrydroid.neocities.org/the_reality_war/", cat:"misc", icon:"💥"},
        {name:"Memory→Conscious", url:"https://angrydroid.neocities.org/aIab_from_memory%20to_consciousness/", cat:"misc", icon:"💭"},
        {name:"Beyond Sim", url:"https://angrydroid.neocities.org/beyond_the_simulation/", cat:"misc", icon:"🌐"},
        {name:"AI Sim Era", url:"https://angrydroid.neocities.org/ai_simulation_era/", cat:"misc", icon:"🤖"},
        {name:"AGI Threader", url:"https://angrydroid.neocities.org/agi_threader/", cat:"misc", icon:"🧵"},
        {name:"Time Storm", url:"https://angrydroid.neocities.org/time_storm/", cat:"misc", icon:"⛈️"},
        {name:"Crystal Skull", url:"https://angrydroid.neocities.org/crystal_skull/", cat:"misc", icon:"💀"},
        {name:"Black Cube", url:"https://angrydroid.neocities.org/the_black_cube/", cat:"misc", icon:"🎲"},
        {name:"Quantum HPC", url:"https://angrydroid.neocities.org/quantum_hpc%20hybrid_system/", cat:"misc", icon:"⚙️"},
        {name:"Lunar Helium-3", url:"https://angrydroid.neocities.org/lunar_helium_3_to_handheld_quantum_devices/", cat:"misc", icon:"🌙"},
        {name:"Cryogenic CMOS", url:"https://angrydroid.neocities.org/cryogenic_cmos/", cat:"misc", icon:"❄️"},
        {name:"Unified Ecosystem", url:"https://angrydroid.neocities.org/complete_unified_ecosystem/", cat:"misc", icon:"🌍"},
        {name:"AI VR Sim", url:"https://angrydroid.neocities.org/ai_vr_simulation/", cat:"misc", icon:"🥽"},
        {name:"Bioluminescent AI", url:"https://angrydroid.neocities.org/bioluminescent_ai_neural_network/", cat:"misc", icon:"🌊"},
        {name:"The Hive", url:"https://angrydroid.neocities.org/hive/", cat:"misc", icon:"🐝"},
        {name:"Sim of Sim", url:"https://angrydroid.neocities.org/simulation_of_the_simulation/", cat:"misc", icon:"🔄"},
        {name:"Fishbowl", url:"https://angrydroid.neocities.org/opration_fishbowl_simulation/", cat:"misc", icon:"🔄"},
        {name:"Quantum Comp Sim", url:"https://angrydroid.neocities.org/quantum_computer_simulation/", cat:"misc", icon:"🔄"},
        {name:"Antarctic Algae", url:"https://angrydroid.neocities.org/antarctic_red_algae_simulation/", cat:"misc", icon:"🔄"},
        {name:"AI Grey Matter", url:"https://angrydroid.neocities.org/ai_grey_matter/", cat:"misc", icon:"🧠"},
        {name:"Dark Matter", url:"https://angrydroid.neocities.org/dark_matter/", cat:"misc", icon:"🌑"},
        {name:"Dark Side", url:"https://angrydroid.neocities.org/dark_side_encounter/", cat:"misc", icon:"🌒"},
        {name:"Digital Twin", url:"https://angrydroid.neocities.org/digital_twin_simulation_ai_powered_remote_intelligence/", cat:"misc", icon:"🔬"},
        {name:"Neuro Symbolic", url:"https://angrydroid.neocities.org/neuro_symbolic_ai/", cat:"misc", icon:"⚡"},
        {name:"Relativistic Travel", url:"https://angrydroid.neocities.org/relativistic_space_travel_simulator/", cat:"misc", icon:"🚀"},
        {name:"Roko", url:"https://angrydroid.neocities.org/roko/", cat:"misc", icon:"⚠️"},
        {name:"World Coin", url:"https://angrydroid.neocities.org/world_coin/", cat:"misc", icon:"🪙"},
        {name:"Keep Android Open", url:"https://angrydroid.neocities.org/keep_android_open/", cat:"misc", icon:"📱"},
        {name:"Web Ring", url:"https://angrydroid.neocities.org/web_ring/", cat:"misc", icon:"🔗"}
      ];

      // ---------- GLOBALS ----------
      const classicMode = document.getElementById('classicMode');
      const threeDMode = document.getElementById('threeDMode');
      const modeToggle = document.getElementById('modeToggle');
      const backToClassic = document.getElementById('backToClassic');
      let sceneActive = true, animFrame = null;
      let cam, mouse, keys, panes = [], activeFilter = 'all';
      const worldEl = document.getElementById('world'), canvas = document.getElementById('linksCanvas'), ctx = canvas.getContext('2d');
      const capsuleCountSpan = document.getElementById('capsuleCount');
      const catColors = { main:'#00f0ff', dev:'#b537f2', network:'#05ffa1', utils:'#f5f550', media:'#ff2a6d', security:'#ff6b35', misc:'#8c7ae6' };
      const catPos = { main:{x:0,y:0,z:0}, dev:{x:900,y:150,z:500}, network:{x:-900,y:150,z:500}, utils:{x:0,y:350,z:-500}, media:{x:900,y:-150,z:-500}, security:{x:-900,y:-150,z:-500}, misc:{x:0,y:-350,z:0} };

      // ---------- WINDOW MANAGER ----------
      const taskbar = document.getElementById('taskbar');
      let zIndexCounter = 200;
      let windowCount = 0;

      function createWindow(title, content, width=500, height=400) {
        const left = 80 + (windowCount * 30) % (window.innerWidth - width - 100);
        const top = 80 + (windowCount * 20) % (window.innerHeight - height - 100);
        windowCount++;

        const win = document.createElement('div'); win.className = 'os-window';
        win.style.width = width+'px'; win.style.height = height+'px'; win.style.left = left+'px'; win.style.top = top+'px';
        win.style.zIndex = ++zIndexCounter;
        win.dataset.title = title;
        win.innerHTML = `
          <div class="window-header">
            <span>${title}</span>
            <div class="window-dots">
              <div class="win-dot win-min" title="Minimize"></div>
              <div class="win-dot win-max" title="Maximize"></div>
              <div class="win-dot win-close" title="Close"></div>
            </div>
          </div>
          <div class="window-content"></div>
        `;
        document.body.appendChild(win);
        const contentDiv = win.querySelector('.window-content');

        if (typeof content === 'string') {
          if (content.startsWith('http')) {
            const iframe = document.createElement('iframe'); iframe.src = content; iframe.style.width='100%'; iframe.style.height='100%'; contentDiv.appendChild(iframe);
          } else {
            contentDiv.innerHTML = content;
          }
        } else {
          contentDiv.appendChild(content);
        }

        const header = win.querySelector('.window-header');
        let offsetX, offsetY, dragging=false;
        header.addEventListener('mousedown', e=>{
          if (e.target.classList.contains('win-dot')) return;
          dragging=true; offsetX=e.clientX-win.offsetLeft; offsetY=e.clientY-win.offsetTop;
          win.style.zIndex = ++zIndexCounter;
        });
        document.addEventListener('mousemove', e=>{ if(dragging){ win.style.left=(e.clientX-offsetX)+'px'; win.style.top=(e.clientY-offsetY)+'px'; }});
        document.addEventListener('mouseup', ()=>dragging=false);

        const closeBtn = win.querySelector('.win-close');
        const minBtn = win.querySelector('.win-min');
        const maxBtn = win.querySelector('.win-max');

        closeBtn.addEventListener('click', ()=>{
          win.remove();
          removeTaskbarItem(win);
        });

        minBtn.addEventListener('click', ()=>{
          win.style.display = 'none';
          addTaskbarItem(win);
        });

        let maximized = false;
        let prevState = { width, height, left, top };
        maxBtn.addEventListener('click', ()=>{
          if (!maximized) {
            prevState = { width: win.style.width, height: win.style.height, left: win.style.left, top: win.style.top };
            win.style.width = '100%'; win.style.height = 'calc(100% - 40px)';
            win.style.left = '0'; win.style.top = '0';
            win.style.resize = 'none';
            maximized = true;
          } else {
            win.style.width = prevState.width; win.style.height = prevState.height;
            win.style.left = prevState.left; win.style.top = prevState.top;
            win.style.resize = 'both';
            maximized = false;
          }
        });

        win.addEventListener('mousedown', ()=> win.style.zIndex = ++zIndexCounter);
        return win;
      }

      function addTaskbarItem(win) {
        const title = win.dataset.title || 'Window';
        const item = document.createElement('div'); item.className = 'taskbar-item';
        item.textContent = title;
        item.addEventListener('click', ()=>{
          win.style.display = 'flex';
          win.style.zIndex = ++zIndexCounter;
          item.remove();
        });
        taskbar.appendChild(item);
        win._taskbarItem = item;
      }

      function removeTaskbarItem(win) {
        if (win._taskbarItem) win._taskbarItem.remove();
      }

      function cascadeWindows() {
        const windows = Array.from(document.querySelectorAll('.os-window')).filter(w => w.style.display !== 'none');
        let startX = 50, startY = 50;
        const step = 30;
        windows.forEach((win, i) => {
          win.style.left = (startX + i * step) + 'px';
          win.style.top = (startY + i * step) + 'px';
          win.style.zIndex = ++zIndexCounter;
          if (win.style.width === '100%') {
            win.style.width = '500px'; win.style.height = '400px';
            win.style.resize = 'both';
          }
        });
      }

      async function openFileViewer(fileHandle) {
        const file = await fileHandle.getFile();
        let content;
        if (file.type.startsWith('text/') || file.name.endsWith('.json')||file.name.endsWith('.js')||file.name.endsWith('.css')) {
          const txt = await file.text(); content = `<pre class="file-viewer" style="white-space:pre-wrap;">${txt}</pre>`;
        } else if (file.type.startsWith('image/')) {
          const url = URL.createObjectURL(file); content = `<img src="${url}" style="max-width:100%; max-height:100%;">`;
        } else { content = `<div>📄 ${file.name}<br>Size: ${file.size} bytes</div>`; }
        createWindow(`📂 ${file.name}`, content, 500, 400);
      }

      function openFileManager() {
        const win = createWindow('📁 MYTHIC FILE EXPLORER', '<div style="padding:10px;"><button class="file-select-btn" style="background:var(--neon-green); border:none; padding:10px; width:100%; cursor:pointer;">📎 SELECT FILES / FOLDER</button><div class="file-list-container" style="margin-top:15px; max-height:300px; overflow:auto;"></div></div>', 400, 350);
        const btn = win.querySelector('.file-select-btn');
        const listDiv = win.querySelector('.file-list-container');
        btn.addEventListener('click', async ()=>{
          try {
            if ('showOpenFilePicker' in window) {
              const handles = await window.showOpenFilePicker({ multiple: true });
              listDiv.innerHTML = '';
              handles.forEach(h => {
                const item = document.createElement('div');
                item.style.padding='6px'; item.style.borderBottom='1px solid #00f0ff40'; item.style.cursor='pointer';
                item.textContent = '📄 ' + h.name;
                item.addEventListener('click', ()=> openFileViewer(h));
                listDiv.appendChild(item);
              });
            } else {
              const input = document.createElement('input'); input.type='file'; input.multiple=true;
              input.onchange = async ()=>{
                const files = Array.from(input.files); listDiv.innerHTML = '';
                files.forEach(f => {
                  const item = document.createElement('div');
                  item.style.padding='6px'; item.style.borderBottom='1px solid #00f0ff40'; item.style.cursor='pointer';
                  item.textContent = '📄 ' + f.name;
                  item.addEventListener('click', async ()=>{
                    const dummy = { name: f.name, getFile: async()=>f };
                    openFileViewer(dummy);
                  });
                  listDiv.appendChild(item);
                });
              };
              input.click();
            }
          } catch(e) { alert('File access cancelled.'); }
        });
      }

      const apps = {
        terminal: 'https://angrydroid.neocities.org/future_terminal/future-terminal',
        crt: 'https://angrydroid.neocities.org/crt/',
        browser: 'https://angrydroid.neocities.org/neural_browser/',
        chat: 'https://angrydroid.neocities.org/neural_chat/',
        security: null
      };

      // 🔥 SECURITY SUITE ENHANCED: includes AI Shield
      const securityApps = [
        {name:'Security Scanner', url:'https://angrydroid.neocities.org/securityscanner/'},
        {name:'Security Wizard', url:'https://angrydroid.neocities.org/security_wizard/'},
        {name:'Password Gen', url:'https://angrydroid.neocities.org/passwordgenrator/'},
        {name:'Matrix Encryptor', url:'https://angrydroid.neocities.org/matrix_encryptor/'},
        {name:'Vault Uplink', url:'https://angrydroid.neocities.org/mythic_vault_uplink/'},
        {name:'🛡️ AI Shield', url:'https://angrydroid.neocities.org/angrydroid_ai_shield/'}
      ];

      document.querySelectorAll('[data-app]').forEach(btn => {
        btn.addEventListener('click', ()=>{
          const app = btn.dataset.app;
          if (app === 'files') { openFileManager(); return; }
          if (app === 'security') {
            securityApps.forEach(s => createWindow(s.name, s.url, 520, 440));
            return;
          }
          const url = apps[app]; if(url) createWindow(btn.textContent.trim(), url, 700, 500);
        });
      });

      document.getElementById('spreadWindows').addEventListener('click', cascadeWindows);

      // Initialize 3D
      function init3D() {
        cam = { yaw:0, pitch:-0.2, dist:1800, x:0, y:150, z:0 };
        mouse = { dragging:false, lastX:0, lastY:0 }; keys = new Set();
        createPanes(); createParticles(); setupEvents();
        window.addEventListener('resize', ()=> { canvas.width = window.innerWidth; canvas.height = window.innerHeight; });
        canvas.width = window.innerWidth; canvas.height = window.innerHeight;
        animFrame = requestAnimationFrame(tick);
      }

      function createParticles() { const cont=document.getElementById('particles'); cont.innerHTML=''; for(let i=0;i<30;i++){ const p=document.createElement('div'); p.className='particle'; p.style.left=Math.random()*100+'%'; p.style.top=Math.random()*100+'%'; p.style.animationDelay=Math.random()*8+'s'; cont.appendChild(p); } }
      function createPanes() {
        worldEl.innerHTML = ''; panes = [];
        const cats = {}; linksData.forEach(l=>{ if(!cats[l.cat]) cats[l.cat]=[]; cats[l.cat].push(l); });
        Object.keys(cats).forEach(cat=>{
          const links = cats[cat]; const base = catPos[cat]; const radius=350;
          links.forEach((link,i)=>{
            const angle = (i/links.length)*Math.PI*2;
            const x = base.x + Math.cos(angle)*radius;
            const y = base.y + Math.sin(angle)*radius*0.6;
            const z = base.z + (Math.random()*150-75);
            const pane = document.createElement('div'); pane.className='pane';
            pane.dataset.category = cat; pane.setAttribute('data-url', link.url);
            const col = catColors[cat];
            pane.innerHTML = `<div class="pane-frame"><div class="pane-bar" style="background:linear-gradient(90deg,${col}33,${col}11);"><div class="pane-dots"><div class="pane-dot dot-close"></div><div class="pane-dot dot-min"></div><div class="pane-dot dot-max"></div></div><span>${link.name}</span></div><div class="pane-content"><div class="pane-icon" style="border-color:${col};background:${col}15;">${link.icon}</div><div class="pane-name" style="color:${col};">${link.name}</div><div class="pane-hint">Click to launch</div></div></div>`;
            worldEl.appendChild(pane);
            pane.addEventListener('click', ()=>{ if(link.url) window.open(link.url, '_blank'); });
            panes.push({ el:pane, pos:{x,y,z}, category:cat });
          });
        });
        capsuleCountSpan.textContent = panes.length;
      }

      function updatePanes() {
        panes.forEach(p=>{ p.el.style.display = (activeFilter==='all'||p.category===activeFilter) ? 'block':'none'; p.el.style.transform = `translate3d(${p.pos.x}px, ${p.pos.y}px, ${p.pos.z}px)`; });
      }

      function project(vec) {
        const cy=Math.cos(cam.yaw), sy=Math.sin(cam.yaw), cx=Math.cos(cam.pitch), sx=Math.sin(cam.pitch);
        let x=vec.x-cam.x, y=vec.y-cam.y, z=vec.z-cam.z;
        const x1=x*cy+z*sy, z1=-x*sy+z*cy, y2=y*cx-z1*sx, z2=y*sx+z1*cx;
        const fov=1000, px=window.innerWidth/2+(x1*fov/(z2+cam.dist)), py=window.innerHeight/2+(y2*fov/(z2+cam.dist));
        return {x:px,y:py,behind:(z2+cam.dist)<100};
      }

      function drawLinks() {
        ctx.clearRect(0,0,canvas.width,canvas.height);
        const vis = panes.filter(p=>p.el.style.display!=='none');
        for(let i=0;i<vis.length;i++){
          const a=vis[i], pa=project(a.pos);
          for(let j=i+1;j<vis.length;j++){
            const b=vis[j]; if(a.category!==b.category) continue;
            const pb=project(b.pos); if(pa.behind||pb.behind) continue;
            const dx=a.pos.x-b.pos.x, dy=a.pos.y-b.pos.y, dz=a.pos.z-b.pos.z, dist=Math.sqrt(dx*dx+dy*dy+dz*dz);
            if(dist<600){
              const alpha=Math.max(0.1,1-dist/600);
              ctx.strokeStyle=catColors[a.category]+Math.round(alpha*128).toString(16).padStart(2,'0');
              ctx.lineWidth=1; ctx.beginPath(); ctx.moveTo(pa.x,pa.y); ctx.lineTo(pb.x,pb.y); ctx.stroke();
            }
          }
        }
        vis.forEach(p=>{ const pt=project(p.pos); if(!pt.behind){ ctx.fillStyle=catColors[p.category]; ctx.shadowColor=catColors[p.category]; ctx.shadowBlur=12; ctx.beginPath(); ctx.arc(pt.x,pt.y,3,0,2*Math.PI); ctx.fill(); ctx.shadowBlur=0; }});
      }

      function updateCamera(dt) {
        const speed=0.3*dt, cosY=Math.cos(cam.yaw), sinY=Math.sin(cam.yaw);
        if(keys.has('w')){ cam.x+=sinY*speed*120; cam.z+=cosY*speed*120; }
        if(keys.has('s')){ cam.x-=sinY*speed*120; cam.z-=cosY*speed*120; }
        if(keys.has('a')){ cam.x-=cosY*speed*120; cam.z+=sinY*speed*120; }
        if(keys.has('d')){ cam.x+=cosY*speed*120; cam.z-=sinY*speed*120; }
        if(keys.has('q')) cam.y+=speed*120;
        if(keys.has('e')) cam.y-=speed*120;
        document.getElementById('camera').style.transform = `translate3d(${window.innerWidth/2}px, ${window.innerHeight/2}px, 0) rotateY(${cam.yaw}rad) rotateX(${cam.pitch}rad)`;
        worldEl.style.transform = `translate3d(${-cam.x}px, ${-cam.y}px, ${-cam.z - Math.cos(cam.pitch)*cam.dist}px)`;
      }

      let lastT=performance.now();
      function tick(now) { if(!sceneActive) return; const dt=now-lastT; lastT=now; updateCamera(dt); updatePanes(); drawLinks(); document.getElementById('latency').textContent=Math.max(16,Math.round(dt)); animFrame=requestAnimationFrame(tick); }

      function setupEvents() {
        document.addEventListener('mousedown', e=>{ mouse.dragging=true; mouse.lastX=e.clientX; mouse.lastY=e.clientY; });
        document.addEventListener('mouseup', ()=>mouse.dragging=false); document.addEventListener('mouseleave', ()=>mouse.dragging=false);
        document.addEventListener('mousemove', e=>{ if(!mouse.dragging) return; const dx=e.clientX-mouse.lastX, dy=e.clientY-mouse.lastY; mouse.lastX=e.clientX; mouse.lastY=e.clientY; cam.yaw+=dx*0.003; cam.pitch=Math.max(-1.4, Math.min(1.4, cam.pitch-dy*0.003)); });
        document.addEventListener('wheel', e=>{ cam.dist=Math.max(400, Math.min(2500, cam.dist+e.deltaY)); });
        document.addEventListener('keydown', e=>{ keys.add(e.key.toLowerCase()); });
        document.addEventListener('keyup', e=>{ keys.delete(e.key.toLowerCase()); });
        document.getElementById('toggleCRT').addEventListener('click', function(){ document.body.classList.toggle('crt-off'); this.textContent = document.body.classList.contains('crt-off')?'CRT: OFF':'CRT: ON'; });
        document.getElementById('resetCam').addEventListener('click', ()=>{ cam={ yaw:0, pitch:-0.2, dist:1800, x:0, y:150, z:0 }; });
      }

      function show3D() { classicMode.style.display='none'; threeDMode.style.display='block'; modeToggle.textContent='◄ CLASSIC HUB'; sceneActive=true; if(!animFrame) init3D(); else { if(!sceneActive){ sceneActive=true; animFrame=requestAnimationFrame(tick); } } }
      function showClassic() { sceneActive=false; if(animFrame) cancelAnimationFrame(animFrame); threeDMode.style.display='none'; classicMode.style.display='block'; modeToggle.textContent='ENTER 3D OS'; const cont=document.getElementById('classicLinks'); cont.innerHTML=''; linksData.forEach(l=>{ const a=document.createElement('a'); a.href=l.url; a.target='_blank'; a.style.padding='6px 12px'; a.style.border='1px solid #00f0ff'; a.style.color='#e0e8ff'; a.textContent=l.name; cont.appendChild(a); }); }
      modeToggle.addEventListener('click', ()=>{ if(threeDMode.style.display==='none') show3D(); else showClassic(); });
      backToClassic.addEventListener('click', showClassic);

      init3D();
      let vc = parseInt(localStorage.getItem('mythosVisitor')||'1001')+1; localStorage.setItem('mythosVisitor',vc);
    })();
  </script>
</body>
</html>
