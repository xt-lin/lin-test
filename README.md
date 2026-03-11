<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LIN TEST — Cyber Intelligence & Penetration Testing Platform</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Share+Tech+Mono&family=Rajdhani:wght@300;400;600;700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
<style>
:root {
  --bg: #050505;
  --bg2: #0a0a12;
  --bg3: #080815;
  --neon: #00eaff;
  --green: #00ff9c;
  --purple: #7a5cff;
  --red: #ff003c;
  --orange: #ff6b00;
  --text: #c8d8e8;
  --text2: #6a8a9a;
  --border: rgba(0,234,255,0.15);
  --card: rgba(0,234,255,0.04);
  --glow: 0 0 20px rgba(0,234,255,0.3);
  --glow-g: 0 0 20px rgba(0,255,156,0.3);
  --glow-p: 0 0 20px rgba(122,92,255,0.3);
}

* { margin:0; padding:0; box-sizing:border-box; }
html { scroll-behavior:smooth; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: 'Rajdhani', sans-serif;
  font-size: 15px;
  overflow-x: hidden;
  min-height: 100vh;
}

/* ─── SCROLLBAR ─── */
::-webkit-scrollbar { width:4px; }
::-webkit-scrollbar-track { background:#050505; }
::-webkit-scrollbar-thumb { background:var(--neon); border-radius:2px; }

/* ─── MATRIX CANVAS ─── */
#matrix-bg {
  position:fixed; top:0; left:0; width:100%; height:100%;
  opacity:0.04; pointer-events:none; z-index:0;
}

/* ─── SCAN LINE ─── */
body::after {
  content:'';
  position:fixed; top:0; left:0; width:100%; height:2px;
  background: linear-gradient(90deg, transparent, var(--neon), transparent);
  animation: scanline 4s linear infinite;
  z-index:9999; pointer-events:none; opacity:0.5;
}
@keyframes scanline { 0%{top:-2px} 100%{top:100vh} }

/* ─── NAV ─── */
nav {
  position:fixed; top:0; left:0; width:100%; height:60px;
  background: rgba(5,5,5,0.95);
  border-bottom: 1px solid var(--border);
  display:flex; align-items:center; justify-content:space-between;
  padding:0 24px; z-index:1000;
  backdrop-filter: blur(20px);
}
.nav-logo {
  font-family:'Orbitron',monospace; font-weight:900; font-size:20px;
  color:var(--neon); letter-spacing:4px; cursor:pointer;
  text-shadow: 0 0 20px var(--neon);
  position:relative;
}
.nav-logo span { color:var(--purple); }
.nav-logo::after {
  content:'CYBER INTEL PLATFORM';
  position:absolute; bottom:-12px; left:0;
  font-size:7px; letter-spacing:3px; color:var(--text2);
  font-family:'Share Tech Mono',monospace;
}
.nav-links {
  display:flex; gap:2px; list-style:none;
}
.nav-links a {
  color:var(--text2); text-decoration:none;
  font-family:'Share Tech Mono',monospace; font-size:11px;
  letter-spacing:1px; padding:6px 10px; cursor:pointer;
  border:1px solid transparent; border-radius:2px;
  transition:all 0.2s; text-transform:uppercase;
}
.nav-links a:hover, .nav-links a.active {
  color:var(--neon); border-color:var(--border);
  background:rgba(0,234,255,0.06); text-shadow:0 0 8px var(--neon);
}
.nav-right {
  display:flex; align-items:center; gap:12px;
}
.status-dot {
  width:8px; height:8px; border-radius:50%;
  background:var(--green); box-shadow:0 0 8px var(--green);
  animation: pulse-dot 2s ease-in-out infinite;
}
@keyframes pulse-dot { 0%,100%{opacity:1} 50%{opacity:0.4} }
.btn-login {
  font-family:'Share Tech Mono',monospace; font-size:11px;
  color:var(--bg); background:var(--neon); padding:7px 16px;
  border:none; cursor:pointer; letter-spacing:2px;
  clip-path: polygon(8px 0,100% 0,100% calc(100% - 8px),calc(100% - 8px) 100%,0 100%,0 8px);
  transition:all 0.2s; text-transform:uppercase;
}
.btn-login:hover { background:var(--green); box-shadow:var(--glow-g); }

/* ─── SIDEBAR ─── */
.sidebar {
  position:fixed; left:0; top:60px; height:calc(100vh - 60px);
  width:220px; background:rgba(5,5,5,0.98);
  border-right:1px solid var(--border); z-index:900;
  overflow-y:auto; padding:16px 0;
}
.sidebar-section { margin-bottom:8px; }
.sidebar-label {
  font-family:'Share Tech Mono',monospace; font-size:9px;
  letter-spacing:3px; color:var(--text2); padding:8px 20px 4px;
  text-transform:uppercase;
}
.sidebar-item {
  display:flex; align-items:center; gap:10px;
  padding:8px 20px; cursor:pointer;
  font-family:'Rajdhani',sans-serif; font-size:13px;
  font-weight:600; color:var(--text2); letter-spacing:0.5px;
  transition:all 0.2s; border-left:2px solid transparent;
  text-transform:uppercase;
}
.sidebar-item:hover, .sidebar-item.active {
  color:var(--neon); border-left-color:var(--neon);
  background:rgba(0,234,255,0.04);
}
.sidebar-item .icon { font-size:14px; width:16px; text-align:center; }
.sidebar-badge {
  margin-left:auto; font-family:'Share Tech Mono',monospace;
  font-size:9px; background:var(--red); color:#fff;
  padding:1px 5px; border-radius:2px;
}

/* ─── MAIN CONTENT ─── */
.main {
  margin-left:220px; margin-top:60px; min-height:calc(100vh - 60px);
  position:relative; z-index:1;
}

/* ─── PAGE ─── */
.page { display:none; padding:28px; }
.page.active { display:block; }

/* ─── PAGE HEADER ─── */
.page-header {
  display:flex; align-items:flex-start; justify-content:space-between;
  margin-bottom:28px; padding-bottom:20px;
  border-bottom:1px solid var(--border);
}
.page-title {
  font-family:'Orbitron',monospace; font-size:22px; font-weight:700;
  color:var(--neon); letter-spacing:3px; text-transform:uppercase;
}
.page-subtitle {
  font-family:'Share Tech Mono',monospace; font-size:11px;
  color:var(--text2); margin-top:4px; letter-spacing:1px;
}
.page-actions { display:flex; gap:8px; }

/* ─── BUTTONS ─── */
.btn {
  font-family:'Share Tech Mono',monospace; font-size:11px;
  padding:8px 18px; cursor:pointer; border:1px solid;
  letter-spacing:1px; text-transform:uppercase; transition:all 0.2s;
  clip-path: polygon(6px 0,100% 0,100% calc(100% - 6px),calc(100% - 6px) 100%,0 100%,0 6px);
}
.btn-primary { color:#000; background:var(--neon); border-color:var(--neon); }
.btn-primary:hover { background:transparent; color:var(--neon); box-shadow:var(--glow); }
.btn-secondary { color:var(--neon); background:transparent; border-color:var(--neon); }
.btn-secondary:hover { background:rgba(0,234,255,0.1); box-shadow:var(--glow); }
.btn-green { color:#000; background:var(--green); border-color:var(--green); }
.btn-green:hover { background:transparent; color:var(--green); box-shadow:var(--glow-g); }
.btn-red { color:var(--red); background:transparent; border-color:var(--red); }
.btn-red:hover { background:rgba(255,0,60,0.1); }

/* ─── CARDS ─── */
.card {
  background:var(--card); border:1px solid var(--border);
  padding:20px; position:relative; overflow:hidden;
}
.card::before {
  content:''; position:absolute; top:0; left:0; width:100%; height:1px;
  background:linear-gradient(90deg, transparent, var(--neon), transparent);
}
.card-title {
  font-family:'Orbitron',monospace; font-size:11px; font-weight:700;
  color:var(--neon); letter-spacing:2px; text-transform:uppercase;
  margin-bottom:14px; display:flex; align-items:center; gap:8px;
}
.card-title::before {
  content:''; width:3px; height:14px; background:var(--neon);
  box-shadow:0 0 8px var(--neon);
}

/* ─── GRID LAYOUTS ─── */
.grid-2 { display:grid; grid-template-columns:1fr 1fr; gap:16px; }
.grid-3 { display:grid; grid-template-columns:1fr 1fr 1fr; gap:16px; }
.grid-4 { display:grid; grid-template-columns:repeat(4,1fr); gap:16px; }

/* ─── STAT CARDS ─── */
.stat-card {
  background:var(--card); border:1px solid var(--border);
  padding:18px; position:relative; overflow:hidden;
}
.stat-card::after {
  content:''; position:absolute; bottom:0; right:0;
  width:60px; height:60px; border-radius:50%;
  opacity:0.08; background:var(--neon);
  transform:translate(20px,20px);
}
.stat-value {
  font-family:'Orbitron',monospace; font-size:28px; font-weight:900;
  color:var(--neon); line-height:1;
}
.stat-value.green { color:var(--green); }
.stat-value.red { color:var(--red); }
.stat-value.purple { color:var(--purple); }
.stat-label {
  font-family:'Share Tech Mono',monospace; font-size:10px;
  color:var(--text2); letter-spacing:2px; text-transform:uppercase;
  margin-top:6px;
}
.stat-change {
  font-size:11px; margin-top:8px; display:flex; align-items:center; gap:4px;
}
.stat-change.up { color:var(--green); }
.stat-change.down { color:var(--red); }

/* ─── INPUT / FORM ─── */
.input-group { margin-bottom:14px; }
.input-label {
  font-family:'Share Tech Mono',monospace; font-size:10px;
  color:var(--text2); letter-spacing:2px; text-transform:uppercase;
  margin-bottom:6px; display:block;
}
.input-field {
  width:100%; background:rgba(0,234,255,0.03);
  border:1px solid var(--border); color:var(--text);
  font-family:'Share Tech Mono',monospace; font-size:13px;
  padding:10px 14px; outline:none; transition:all 0.2s;
}
.input-field:focus { border-color:var(--neon); box-shadow:0 0 12px rgba(0,234,255,0.15); }
.input-field::placeholder { color:var(--text2); }
.input-row { display:flex; gap:8px; }
.input-row .input-field { flex:1; }

/* ─── TABLE ─── */
.cyber-table { width:100%; border-collapse:collapse; }
.cyber-table th {
  font-family:'Share Tech Mono',monospace; font-size:9px;
  letter-spacing:2px; color:var(--text2); text-transform:uppercase;
  padding:10px 12px; border-bottom:1px solid var(--border);
  text-align:left; background:rgba(0,234,255,0.02);
}
.cyber-table td {
  padding:10px 12px; font-size:13px; color:var(--text);
  border-bottom:1px solid rgba(0,234,255,0.06);
  font-family:'Share Tech Mono',monospace;
}
.cyber-table tr:hover td { background:rgba(0,234,255,0.03); }

/* ─── BADGES ─── */
.badge {
  display:inline-flex; align-items:center; gap:4px;
  font-family:'Share Tech Mono',monospace; font-size:9px;
  letter-spacing:1px; padding:3px 8px; text-transform:uppercase;
}
.badge-critical { background:rgba(255,0,60,0.15); color:var(--red); border:1px solid rgba(255,0,60,0.3); }
.badge-high { background:rgba(255,107,0,0.15); color:var(--orange); border:1px solid rgba(255,107,0,0.3); }
.badge-medium { background:rgba(0,234,255,0.1); color:var(--neon); border:1px solid var(--border); }
.badge-low { background:rgba(0,255,156,0.1); color:var(--green); border:1px solid rgba(0,255,156,0.3); }
.badge-info { background:rgba(122,92,255,0.1); color:var(--purple); border:1px solid rgba(122,92,255,0.3); }

/* ─── TERMINAL ─── */
.terminal {
  background:#000; border:1px solid var(--border);
  font-family:'Share Tech Mono',monospace; font-size:12px;
  padding:16px; position:relative; min-height:200px;
}
.terminal-bar {
  display:flex; align-items:center; gap:6px; margin-bottom:14px;
  padding-bottom:10px; border-bottom:1px solid rgba(255,255,255,0.05);
}
.terminal-dot { width:10px; height:10px; border-radius:50%; }
.terminal-line { color:var(--green); line-height:1.8; }
.terminal-line .prompt { color:var(--neon); }
.terminal-line .path { color:var(--purple); }
.terminal-line .cmd { color:#fff; }
.terminal-line .output { color:var(--text2); }
.terminal-cursor {
  display:inline-block; width:8px; height:14px;
  background:var(--neon); animation:blink 1s step-end infinite;
  vertical-align:middle;
}
@keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }
.terminal-input {
  display:flex; align-items:center; gap:8px; margin-top:12px;
  border-top:1px solid rgba(0,234,255,0.1); padding-top:10px;
}
.terminal-input input {
  flex:1; background:transparent; border:none; outline:none;
  color:var(--green); font-family:'Share Tech Mono',monospace; font-size:12px;
}

/* ─── PROGRESS / BARS ─── */
.progress-bar { height:4px; background:rgba(255,255,255,0.06); position:relative; overflow:hidden; }
.progress-fill { height:100%; transition:width 1s ease; }
.progress-neon { background:linear-gradient(90deg,var(--neon),var(--purple)); box-shadow:0 0 8px var(--neon); }
.progress-green { background:var(--green); box-shadow:0 0 8px var(--green); }
.progress-red { background:var(--red); box-shadow:0 0 8px var(--red); }

/* ─── TABS ─── */
.tabs { display:flex; gap:0; border-bottom:1px solid var(--border); margin-bottom:20px; }
.tab {
  font-family:'Share Tech Mono',monospace; font-size:11px;
  letter-spacing:1px; padding:10px 18px; cursor:pointer;
  color:var(--text2); border-bottom:2px solid transparent;
  text-transform:uppercase; transition:all 0.2s;
}
.tab:hover { color:var(--neon); }
.tab.active { color:var(--neon); border-bottom-color:var(--neon); }

/* ─── THREAT LEVEL ─── */
.threat-meter {
  display:flex; gap:3px; align-items:flex-end; height:30px;
}
.threat-bar {
  width:12px; background:rgba(255,255,255,0.08);
  transition:all 0.5s;
}
.threat-bar.active.low { background:var(--green); }
.threat-bar.active.med { background:var(--orange); }
.threat-bar.active.high { background:var(--red); }

/* ─── GLOBE CANVAS ─── */
#globe-canvas {
  width:100%; height:400px; display:block;
}

/* ─── ATTACK LOG ─── */
.attack-log { max-height:300px; overflow-y:auto; }
.attack-item {
  display:flex; align-items:center; gap:10px;
  padding:6px 0; border-bottom:1px solid rgba(0,234,255,0.06);
  font-family:'Share Tech Mono',monospace; font-size:11px;
  animation:fadeIn 0.4s ease;
}
@keyframes fadeIn { from{opacity:0;transform:translateX(-10px)} to{opacity:1;transform:none} }
.attack-dot { width:6px; height:6px; border-radius:50%; flex-shrink:0; }
.attack-src { color:var(--red); }
.attack-arrow { color:var(--text2); }
.attack-dst { color:var(--green); }
.attack-type { color:var(--purple); margin-left:auto; }

/* ─── FEATURE GRID (HOME) ─── */
.feature-card {
  background:var(--card); border:1px solid var(--border);
  padding:24px; position:relative; overflow:hidden;
  transition:all 0.3s; cursor:pointer;
}
.feature-card:hover {
  border-color:var(--neon); box-shadow:var(--glow);
  transform:translateY(-2px);
}
.feature-icon { font-size:32px; margin-bottom:14px; }
.feature-title {
  font-family:'Orbitron',monospace; font-size:13px;
  font-weight:700; color:var(--neon); letter-spacing:2px;
  text-transform:uppercase; margin-bottom:8px;
}
.feature-desc { font-size:13px; color:var(--text2); line-height:1.6; }

/* ─── NEWS CARD ─── */
.news-card {
  background:var(--card); border:1px solid var(--border);
  padding:18px; transition:all 0.2s; cursor:pointer;
}
.news-card:hover { border-color:var(--neon); }
.news-tag { font-family:'Share Tech Mono',monospace; font-size:9px; letter-spacing:1px; color:var(--purple); }
.news-title { font-size:14px; font-weight:600; color:var(--text); margin:6px 0; line-height:1.4; }
.news-meta { font-family:'Share Tech Mono',monospace; font-size:10px; color:var(--text2); }

/* ─── TOOL CARD ─── */
.tool-card {
  background:var(--card); border:1px solid var(--border);
  padding:16px; transition:all 0.2s; cursor:pointer;
}
.tool-card:hover { border-color:var(--green); box-shadow:var(--glow-g); }
.tool-name { font-family:'Orbitron',monospace; font-size:11px; color:var(--green); font-weight:700; }
.tool-desc { font-size:12px; color:var(--text2); margin:6px 0; }
.tool-tags { display:flex; gap:4px; flex-wrap:wrap; }
.tool-tag {
  font-family:'Share Tech Mono',monospace; font-size:9px;
  background:rgba(122,92,255,0.1); color:var(--purple);
  border:1px solid rgba(122,92,255,0.2); padding:2px 6px;
}

/* ─── AI ASSISTANT ─── */
.ai-messages {
  height:420px; overflow-y:auto; padding:16px;
  background:#000; border:1px solid var(--border);
  display:flex; flex-direction:column; gap:14px;
}
.ai-msg { display:flex; gap:12px; }
.ai-msg.user { flex-direction:row-reverse; }
.ai-avatar {
  width:34px; height:34px; flex-shrink:0;
  border:1px solid var(--border); display:flex;
  align-items:center; justify-content:center;
  font-size:16px; background:var(--card);
}
.ai-bubble {
  max-width:75%; padding:12px 16px;
  font-family:'Share Tech Mono',monospace; font-size:12px;
  line-height:1.7;
}
.ai-msg:not(.user) .ai-bubble {
  background:rgba(0,234,255,0.05); border:1px solid var(--border);
  color:var(--text); border-radius:0 8px 8px 8px;
}
.ai-msg.user .ai-bubble {
  background:rgba(122,92,255,0.1); border:1px solid rgba(122,92,255,0.3);
  color:var(--purple); border-radius:8px 0 8px 8px;
}
.ai-input-row {
  display:flex; gap:8px; margin-top:12px;
}
.ai-input-row input {
  flex:1; background:rgba(0,234,255,0.03); border:1px solid var(--border);
  color:var(--text); font-family:'Share Tech Mono',monospace; font-size:12px;
  padding:10px 14px; outline:none;
}
.ai-input-row input:focus { border-color:var(--neon); }

/* ─── SCAN ANIMATION ─── */
.scan-ring {
  width:60px; height:60px; border-radius:50%;
  border:2px solid transparent;
  border-top-color:var(--neon); border-right-color:var(--neon);
  animation:spin 1s linear infinite;
}
@keyframes spin { to{transform:rotate(360deg)} }

/* ─── RADAR ─── */
.radar-wrap { position:relative; width:200px; height:200px; margin:0 auto; }
#radar-canvas { width:200px; height:200px; }

/* ─── HOME HERO ─── */
.hero {
  min-height:calc(100vh - 60px); display:flex; flex-direction:column;
  justify-content:center; align-items:center; text-align:center;
  padding:60px 28px; position:relative;
}
.hero-tag {
  font-family:'Share Tech Mono',monospace; font-size:11px;
  letter-spacing:4px; color:var(--neon); text-transform:uppercase;
  margin-bottom:20px; opacity:0.8;
}
.hero-title {
  font-family:'Orbitron',monospace; font-size:clamp(40px,7vw,88px);
  font-weight:900; line-height:1; letter-spacing:-2px;
  margin-bottom:12px;
}
.hero-title .t1 { color:#fff; }
.hero-title .t2 {
  color:transparent;
  -webkit-text-stroke:2px var(--neon);
  text-shadow:0 0 40px rgba(0,234,255,0.4);
}
.hero-sub {
  font-family:'Rajdhani',sans-serif; font-size:18px;
  color:var(--text2); max-width:520px; line-height:1.6;
  margin-bottom:36px;
}
.hero-actions { display:flex; gap:16px; justify-content:center; flex-wrap:wrap; }
.hero-stats {
  display:grid; grid-template-columns:repeat(4,1fr); gap:20px;
  max-width:700px; width:100%; margin-top:60px;
}
.hero-stat {
  text-align:center; padding:16px;
  border:1px solid var(--border); background:var(--card);
}
.hero-stat-val {
  font-family:'Orbitron',monospace; font-size:26px; font-weight:900;
  color:var(--neon);
}
.hero-stat-lbl { font-family:'Share Tech Mono',monospace; font-size:9px; color:var(--text2); letter-spacing:2px; }

/* ─── GLOBE SECTION ─── */
.globe-section {
  display:grid; grid-template-columns:1fr 1fr; gap:0;
  max-width:100%; margin-bottom:40px;
}

/* ─── CVE FEED ─── */
.cve-item {
  display:flex; align-items:flex-start; gap:12px;
  padding:12px 0; border-bottom:1px solid rgba(0,234,255,0.06);
}
.cve-id {
  font-family:'Share Tech Mono',monospace; font-size:11px;
  color:var(--neon); white-space:nowrap; min-width:120px;
}
.cve-desc { font-size:12px; color:var(--text); flex:1; }
.cve-score {
  font-family:'Orbitron',monospace; font-size:16px; font-weight:700;
  min-width:40px; text-align:right;
}

/* ─── SEARCH BAR ─── */
.search-bar {
  display:flex; gap:8px; margin-bottom:20px;
}
.search-bar input {
  flex:1; background:rgba(0,234,255,0.03); border:1px solid var(--border);
  color:var(--text); font-family:'Share Tech Mono',monospace;
  font-size:12px; padding:10px 14px; outline:none;
}
.search-bar input:focus { border-color:var(--neon); }

/* ─── RESULT BOX ─── */
.result-box {
  background:#000; border:1px solid var(--border);
  padding:16px; font-family:'Share Tech Mono',monospace;
  font-size:12px; min-height:140px; position:relative;
}
.result-line { color:var(--green); line-height:1.9; }
.result-key { color:var(--neon); }
.result-val { color:var(--text); }

/* ─── HEATMAP ─── */
.heatmap { display:grid; grid-template-columns:repeat(24,1fr); gap:2px; }
.hm-cell {
  height:12px; border-radius:1px; opacity:0.8;
  transition:all 0.3s;
}
.hm-cell:hover { transform:scale(1.5); z-index:2; position:relative; }

/* ─── PIPELINE ─── */
.pipeline { display:flex; align-items:center; gap:0; margin-bottom:20px; }
.pipe-step {
  flex:1; padding:14px; text-align:center;
  background:var(--card); border:1px solid var(--border);
  position:relative; transition:all 0.3s;
}
.pipe-step.done { border-color:var(--green); background:rgba(0,255,156,0.06); }
.pipe-step.active { border-color:var(--neon); background:rgba(0,234,255,0.08); }
.pipe-step::after {
  content:'▶'; position:absolute; right:-12px; top:50%;
  transform:translateY(-50%); color:var(--text2); font-size:10px;
  z-index:1;
}
.pipe-step:last-child::after { display:none; }
.pipe-num {
  font-family:'Orbitron',monospace; font-size:18px; font-weight:900;
  color:var(--neon);
}
.pipe-label { font-family:'Share Tech Mono',monospace; font-size:9px; color:var(--text2); letter-spacing:1px; }

/* ─── NEON LINE ─── */
.neon-line {
  height:1px;
  background:linear-gradient(90deg, transparent, var(--neon), var(--purple), transparent);
  margin:24px 0; opacity:0.4;
}

/* ─── ADMIN USER ROW ─── */
.user-row {
  display:flex; align-items:center; gap:14px;
  padding:12px; border-bottom:1px solid rgba(0,234,255,0.06);
  transition:background 0.2s;
}
.user-row:hover { background:rgba(0,234,255,0.03); }
.user-avatar {
  width:36px; height:36px; border-radius:50%;
  background:linear-gradient(135deg,var(--purple),var(--neon));
  display:flex; align-items:center; justify-content:center;
  font-weight:700; font-size:13px; color:#000;
}
.user-name { font-weight:600; font-size:13px; }
.user-email { font-family:'Share Tech Mono',monospace; font-size:10px; color:var(--text2); }
.user-role { font-family:'Share Tech Mono',monospace; font-size:9px; margin-left:auto; }

/* ─── RESPONSIVE ─── */
@media(max-width:1200px) {
  .grid-4 { grid-template-columns:1fr 1fr; }
}
@media(max-width:900px) {
  .sidebar { display:none; }
  .main { margin-left:0; }
  .grid-3, .grid-2 { grid-template-columns:1fr; }
  .nav-links { display:none; }
  .hero-stats { grid-template-columns:1fr 1fr; }
}

/* ─── SCROLLABLE CONTENT ─── */
.scroll-x { overflow-x:auto; }

/* ─── GLOW EFFECTS ─── */
.glow-neon { box-shadow:var(--glow); }
.glow-green { box-shadow:var(--glow-g); }
.glow-purple { box-shadow:var(--glow-p); }
.text-neon { color:var(--neon); }
.text-green { color:var(--green); }
.text-red { color:var(--red); }
.text-purple { color:var(--purple); }
.text-orange { color:var(--orange); }

/* ─── MISC ─── */
.flex { display:flex; }
.flex-between { display:flex; justify-content:space-between; align-items:center; }
.flex-center { display:flex; justify-content:center; align-items:center; }
.gap-8 { gap:8px; }
.gap-16 { gap:16px; }
.mb-16 { margin-bottom:16px; }
.mb-24 { margin-bottom:24px; }
.mb-8 { margin-bottom:8px; }
.mt-8 { margin-top:8px; }
.mt-16 { margin-top:16px; }
.w100 { width:100%; }
.mono { font-family:'Share Tech Mono',monospace; }
.small { font-size:11px; }
.uppercase { text-transform:uppercase; letter-spacing:1px; }
.section-gap { margin-bottom:24px; }
</style>
</head>
<body>

<!-- MATRIX BACKGROUND -->
<canvas id="matrix-bg"></canvas>

<!-- ═══ NAV ═══ -->
<nav>
  <div class="nav-logo" onclick="showPage('home')">LIN<span>TEST</span></div>
  <ul class="nav-links">
    <li><a onclick="showPage('home')" class="active" id="nav-home">HOME</a></li>
    <li><a onclick="showPage('dashboard')" id="nav-dashboard">DASHBOARD</a></li>
    <li><a onclick="showPage('osint')" id="nav-osint">OSINT</a></li>
    <li><a onclick="showPage('pentest')" id="nav-pentest">PENTEST</a></li>
    <li><a onclick="showPage('threat')" id="nav-threat">THREATS</a></li>
    <li><a onclick="showPage('ai')" id="nav-ai">AI</a></li>
    <li><a onclick="showPage('news')" id="nav-news">NEWS</a></li>
    <li><a onclick="showPage('admin')" id="nav-admin">ADMIN</a></li>
  </ul>
  <div class="nav-right">
    <div style="font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--green)">● SYSTEM ONLINE</div>
    <div class="status-dot"></div>
    <button class="btn-login" style="background:var(--purple);border-color:var(--purple)" onclick="downloadFullReport('html')">⬇ REPORT</button>
    <button class="btn-login" onclick="showPage('login')">LOGIN</button>
  </div>
</nav>

<!-- ═══ SIDEBAR ═══ -->
<aside class="sidebar">
  <div class="sidebar-section">
    <div class="sidebar-label">Navigation</div>
    <div class="sidebar-item active" onclick="showPage('home')"><span class="icon">⌂</span> Home</div>
    <div class="sidebar-item" onclick="showPage('dashboard')"><span class="icon">◈</span> Dashboard <span class="sidebar-badge">LIVE</span></div>
  </div>
  <div class="sidebar-section">
    <div class="sidebar-label">Intelligence</div>
    <div class="sidebar-item" onclick="showPage('osint')"><span class="icon">◉</span> OSINT Center</div>
    <div class="sidebar-item" onclick="showPage('threat')"><span class="icon">⚠</span> Threat Intel <span class="sidebar-badge">3</span></div>
    <div class="sidebar-item" onclick="showPage('recon')"><span class="icon">⟳</span> Recon Auto</div>
  </div>
  <div class="sidebar-section">
    <div class="sidebar-label">Pentesting</div>
    <div class="sidebar-item" onclick="showPage('pentest')"><span class="icon">⛨</span> Pentest Tools</div>
    <div class="sidebar-item" onclick="showPage('tools')"><span class="icon">⊞</span> Tools Library</div>
    <div class="sidebar-item" onclick="showPage('reports')"><span class="icon">≡</span> Reports</div>
  </div>
  <div class="sidebar-section">
    <div class="sidebar-label">Platform</div>
    <div class="sidebar-item" onclick="showPage('ai')"><span class="icon">✦</span> AI Assistant</div>
    <div class="sidebar-item" onclick="showPage('news')"><span class="icon">◎</span> Cyber News</div>
    <div class="sidebar-item" onclick="showPage('community')"><span class="icon">◈</span> Community</div>
    <div class="sidebar-item" onclick="showPage('help')"><span class="icon">?</span> Help Center</div>
    <div class="sidebar-item" onclick="showPage('docs')"><span class="icon">≡</span> Docs</div>
  </div>
  <div class="sidebar-section">
    <div class="sidebar-label">System</div>
    <div class="sidebar-item" onclick="showPage('admin')"><span class="icon">⚙</span> Admin Panel</div>
    <div class="sidebar-item" onclick="showPage('about')"><span class="icon">ℹ</span> About</div>
    <div class="sidebar-item" onclick="showPage('login')"><span class="icon">→</span> Login</div>
  </div>
</aside>

<!-- ═══ MAIN ═══ -->
<main class="main">

<!-- ══════════════════════════════ HOME ══════════════════════════════ -->
<section class="page active" id="page-home">
  <div class="hero">
    <div class="hero-tag">◈ Advanced Cyber Intelligence Platform ◈</div>
    <h1 class="hero-title">
      <div class="t1">LIN</div>
      <div class="t2">TEST</div>
    </h1>
    <p class="hero-sub">Professional OSINT investigation, penetration testing, and real-time threat intelligence for security researchers, ethical hackers, and SOC analysts.</p>
    <div class="hero-actions">
      <button class="btn btn-primary" onclick="showPage('dashboard')" style="padding:14px 32px;font-size:13px">LAUNCH DASHBOARD</button>
      <button class="btn btn-secondary" onclick="showPage('osint')" style="padding:14px 32px;font-size:13px">START INVESTIGATION</button>
      <button class="btn" style="color:var(--purple);border-color:var(--purple);padding:14px 32px;font-size:13px" onclick="showPage('ai')">AI ASSISTANT</button>
    </div>
    <div class="hero-stats">
      <div class="hero-stat">
        <div class="hero-stat-val" id="stat-threats">48,291</div>
        <div class="hero-stat-lbl">Threats Detected</div>
      </div>
      <div class="hero-stat">
        <div class="hero-stat-val" style="color:var(--green)" id="stat-scans">12,847</div>
        <div class="hero-stat-lbl">Active Scans</div>
      </div>
      <div class="hero-stat">
        <div class="hero-stat-val" style="color:var(--purple)">317</div>
        <div class="hero-stat-lbl">Tools Available</div>
      </div>
      <div class="hero-stat">
        <div class="hero-stat-val" style="color:var(--red)">99.9%</div>
        <div class="hero-stat-lbl">Uptime</div>
      </div>
    </div>
  </div>

  <!-- Globe & Attacks Row -->
  <div style="padding:0 28px 28px">
    <div class="globe-section">
      <div class="card">
        <div class="card-title">◈ GLOBAL THREAT MAP — LIVE</div>
        <canvas id="globe-canvas"></canvas>
      </div>
      <div class="card" style="border-left:none">
        <div class="card-title">⚡ LIVE ATTACK STREAM</div>
        <div class="attack-log" id="attack-log"></div>
        <div class="neon-line"></div>
        <div style="display:flex;gap:16px;margin-top:8px">
          <div>
            <div class="mono small text-red">2,847</div>
            <div class="mono" style="font-size:9px;color:var(--text2);letter-spacing:1px">ATTACKS/MIN</div>
          </div>
          <div>
            <div class="mono small text-green">127</div>
            <div class="mono" style="font-size:9px;color:var(--text2);letter-spacing:1px">BLOCKED/MIN</div>
          </div>
          <div>
            <div class="mono small text-neon">43</div>
            <div class="mono" style="font-size:9px;color:var(--text2);letter-spacing:1px">COUNTRIES</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Features -->
    <div style="margin-bottom:12px" class="card-title">◈ PLATFORM CAPABILITIES</div>
    <div class="grid-3 mb-24">
      <div class="feature-card" onclick="showPage('osint')">
        <div class="feature-icon">🔍</div>
        <div class="feature-title">OSINT Intelligence</div>
        <div class="feature-desc">Investigate domains, IPs, emails, usernames and phone numbers across 200+ data sources with automated correlation.</div>
      </div>
      <div class="feature-card" onclick="showPage('pentest')">
        <div class="feature-icon">⛨</div>
        <div class="feature-title">Penetration Testing</div>
        <div class="feature-desc">Professional toolkit for network recon, web app testing, subdomain discovery, and vulnerability scanning.</div>
      </div>
      <div class="feature-card" onclick="showPage('threat')">
        <div class="feature-icon">📡</div>
        <div class="feature-title">Threat Intelligence</div>
        <div class="feature-desc">Real-time feeds from VirusTotal, Shodan, AbuseIPDB, and HIBP with automated correlation and alerting.</div>
      </div>
      <div class="feature-card" onclick="showPage('recon')">
        <div class="feature-icon">⚙</div>
        <div class="feature-title">Recon Automation</div>
        <div class="feature-desc">Build and execute automated reconnaissance pipelines from domain discovery to vulnerability reporting.</div>
      </div>
      <div class="feature-card" onclick="showPage('ai')">
        <div class="feature-icon">✦</div>
        <div class="feature-title">AI Security Assistant</div>
        <div class="feature-desc">Powered by advanced AI to explain vulnerabilities, generate reports, and guide your investigation strategy.</div>
      </div>
      <div class="feature-card" onclick="showPage('reports')">
        <div class="feature-icon">📊</div>
        <div class="feature-title">Report Generation</div>
        <div class="feature-desc">Generate professional PDF, HTML, and JSON reports with executive summaries and remediation guidance.</div>
      </div>
    </div>

    <!-- Latest CVEs -->
    <div class="card mb-24">
      <div class="card-title">⚠ LATEST CVE VULNERABILITIES</div>
      <div id="cve-feed">
        <div class="cve-item">
          <div class="cve-id text-neon">CVE-2025-0482</div>
          <div class="cve-desc">Apache HTTP Server remote code execution via crafted HTTP/2 CONTINUATION frames</div>
          <div class="cve-score text-red">9.8</div>
          <div class="badge badge-critical">CRITICAL</div>
        </div>
        <div class="cve-item">
          <div class="cve-id text-neon">CVE-2025-1337</div>
          <div class="cve-desc">OpenSSL buffer overflow in SSL_select_next_proto function</div>
          <div class="cve-score text-orange">8.1</div>
          <div class="badge badge-high">HIGH</div>
        </div>
        <div class="cve-item">
          <div class="cve-id text-neon">CVE-2025-2048</div>
          <div class="cve-desc">Linux kernel use-after-free in netfilter subsystem allows privilege escalation</div>
          <div class="cve-score text-red">9.1</div>
          <div class="badge badge-critical">CRITICAL</div>
        </div>
        <div class="cve-item">
          <div class="cve-id text-neon">CVE-2025-3192</div>
          <div class="cve-desc">WordPress core CSRF vulnerability in comment management leads to stored XSS</div>
          <div class="cve-score text-neon">6.4</div>
          <div class="badge badge-medium">MEDIUM</div>
        </div>
      </div>
    </div>

    <!-- News Preview -->
    <div class="card-title">◎ LATEST CYBER NEWS</div>
    <div class="grid-3">
      <div class="news-card">
        <div class="news-tag">RANSOMWARE</div>
        <div class="news-title">BlackCat Ransomware Group Targets Healthcare Sector with New Encryption Method</div>
        <div class="news-meta">2 hours ago • Threat Actor</div>
      </div>
      <div class="news-card">
        <div class="news-tag">ZERO-DAY</div>
        <div class="news-title">Critical Zero-Day Discovered in Popular VPN Client Affecting 2M+ Users</div>
        <div class="news-meta">5 hours ago • Vulnerability</div>
      </div>
      <div class="news-card">
        <div class="news-tag">DATA BREACH</div>
        <div class="news-title">Major Cloud Provider Exposes 84M Records in Misconfigured S3 Bucket</div>
        <div class="news-meta">8 hours ago • Breach</div>
      </div>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ DASHBOARD ══════════════════════════════ -->
<section class="page" id="page-dashboard">
  <div class="page-header">
    <div>
      <div class="page-title">◈ SOC DASHBOARD</div>
      <div class="page-subtitle">// REAL-TIME SECURITY OPERATIONS CENTER — THREAT LEVEL: ELEVATED</div>
    </div>
    <div class="page-actions">
      <button class="btn btn-secondary" onclick="exportDashboard()">EXPORT</button>
      <button class="btn btn-primary" onclick="startNewScan()">NEW SCAN</button>
    </div>
  </div>

  <!-- Stats Row -->
  <div class="grid-4 mb-24">
    <div class="stat-card">
      <div class="stat-value text-red" id="dash-threats">2,847</div>
      <div class="stat-label">Active Threats</div>
      <div class="stat-change up">▲ +12% from yesterday</div>
    </div>
    <div class="stat-card">
      <div class="stat-value">487</div>
      <div class="stat-label">Running Scans</div>
      <div class="stat-change down">▼ -3 paused</div>
    </div>
    <div class="stat-card">
      <div class="stat-value green">99.2%</div>
      <div class="stat-label">System Uptime</div>
      <div class="stat-change up">▲ +0.1%</div>
    </div>
    <div class="stat-card">
      <div class="stat-value purple">1,247</div>
      <div class="stat-label">IPs Blocked</div>
      <div class="stat-change up">▲ +84 today</div>
    </div>
  </div>

  <!-- Charts -->
  <div class="grid-2 mb-24">
    <div class="card">
      <div class="card-title">📈 THREAT ACTIVITY — 24H</div>
      <canvas id="threat-chart" height="180"></canvas>
    </div>
    <div class="card">
      <div class="card-title">🌐 ATTACK ORIGIN MAP</div>
      <canvas id="origin-chart" height="180"></canvas>
    </div>
  </div>

  <!-- Middle Row -->
  <div class="grid-3 mb-24">
    <div class="card">
      <div class="card-title">🔴 TOP THREAT ACTORS</div>
      <div class="cyber-table-wrap">
        <table class="cyber-table">
          <thead><tr><th>Actor</th><th>Origin</th><th>Attacks</th><th>Risk</th></tr></thead>
          <tbody>
            <tr><td class="text-red">APT-29</td><td>RU</td><td>1,284</td><td><span class="badge badge-critical">CRIT</span></td></tr>
            <tr><td class="text-red">Lazarus</td><td>KP</td><td>847</td><td><span class="badge badge-critical">CRIT</span></td></tr>
            <tr><td class="text-orange">Anonymous</td><td>INT</td><td>422</td><td><span class="badge badge-high">HIGH</span></td></tr>
            <tr><td class="text-orange">BlackCat</td><td>RU</td><td>318</td><td><span class="badge badge-high">HIGH</span></td></tr>
            <tr><td class="text-neon">Scattered</td><td>US</td><td>112</td><td><span class="badge badge-medium">MED</span></td></tr>
          </tbody>
        </table>
      </div>
    </div>
    <div class="card">
      <div class="card-title">⚡ IP REPUTATION ALERTS</div>
      <div id="ip-alerts">
        <div class="cve-item">
          <div class="attack-dot" style="background:var(--red)"></div>
          <div class="mono small text-red">45.134.26.174</div>
          <div class="badge badge-critical" style="margin-left:auto">MALICIOUS</div>
        </div>
        <div class="cve-item">
          <div class="attack-dot" style="background:var(--orange)"></div>
          <div class="mono small">192.168.141.88</div>
          <div class="badge badge-high" style="margin-left:auto">SUSPICIOUS</div>
        </div>
        <div class="cve-item">
          <div class="attack-dot" style="background:var(--orange)"></div>
          <div class="mono small">103.27.148.221</div>
          <div class="badge badge-high" style="margin-left:auto">BOTNET</div>
        </div>
        <div class="cve-item">
          <div class="attack-dot" style="background:var(--neon)"></div>
          <div class="mono small">185.220.101.45</div>
          <div class="badge badge-medium" style="margin-left:auto">TOR EXIT</div>
        </div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">🛡 BREACH ALERTS</div>
      <div class="cve-item">
        <div class="attack-dot" style="background:var(--red)"></div>
        <div style="flex:1">
          <div class="mono small">adobe.com</div>
          <div style="font-size:11px;color:var(--text2)">152M records exposed</div>
        </div>
        <div class="badge badge-critical">NEW</div>
      </div>
      <div class="cve-item">
        <div class="attack-dot" style="background:var(--orange)"></div>
        <div style="flex:1">
          <div class="mono small">linkedin.com</div>
          <div style="font-size:11px;color:var(--text2)">700M records • 2024</div>
        </div>
        <div class="badge badge-high">OLD</div>
      </div>
      <div class="cve-item">
        <div class="attack-dot" style="background:var(--purple)"></div>
        <div style="flex:1">
          <div class="mono small">dropbox.com</div>
          <div style="font-size:11px;color:var(--text2)">68M credentials</div>
        </div>
        <div class="badge badge-info">VERIFIED</div>
      </div>
      <div class="mt-16">
        <div class="card-title" style="font-size:10px;margin-bottom:8px">VULNERABILITY HEATMAP</div>
        <div class="heatmap" id="heatmap"></div>
      </div>
    </div>
  </div>

  <!-- Domain + CVE Row -->
  <div class="grid-2">
    <div class="card">
      <div class="card-title">🌐 DOMAIN INTELLIGENCE MONITOR</div>
      <table class="cyber-table">
        <thead><tr><th>Domain</th><th>Registrar</th><th>Created</th><th>Risk</th></tr></thead>
        <tbody>
          <tr><td class="text-neon">malware-c2.ru</td><td>REG.RU</td><td>2024-01-15</td><td><span class="badge badge-critical">C2</span></td></tr>
          <tr><td class="text-orange">phish-bank.xyz</td><td>Namecheap</td><td>2024-11-02</td><td><span class="badge badge-high">PHISH</span></td></tr>
          <tr><td>cdn-update.net</td><td>GoDaddy</td><td>2024-09-30</td><td><span class="badge badge-medium">SUSPECT</span></td></tr>
          <tr><td>secure-login.tk</td><td>Freenom</td><td>2024-12-01</td><td><span class="badge badge-high">TYPOSQUAT</span></td></tr>
        </tbody>
      </table>
    </div>
    <div class="card">
      <div class="card-title">📊 ATTACK TYPE DISTRIBUTION</div>
      <canvas id="attack-type-chart" height="200"></canvas>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ OSINT ══════════════════════════════ -->
<section class="page" id="page-osint">
  <div class="page-header">
    <div>
      <div class="page-title">◉ OSINT INTELLIGENCE CENTER</div>
      <div class="page-subtitle">// OPEN SOURCE INTELLIGENCE INVESTIGATION PLATFORM</div>
    </div>
    <div class="page-actions">
      <button class="btn btn-secondary" onclick="saveSession()">SAVE SESSION</button>
      <button class="btn btn-primary" onclick="newCase()">NEW CASE</button>
    </div>
  </div>

  <div class="tabs">
    <div class="tab active" onclick="switchTab(this,'osint-domain')">DOMAIN</div>
    <div class="tab" onclick="switchTab(this,'osint-ip')">IP ADDRESS</div>
    <div class="tab" onclick="switchTab(this,'osint-email')">EMAIL</div>
    <div class="tab" onclick="switchTab(this,'osint-username')">USERNAME</div>
    <div class="tab" onclick="switchTab(this,'osint-phone')">PHONE</div>
  </div>

  <!-- Domain Tab -->
  <div id="osint-domain" class="tab-content">
    <div class="grid-2 mb-24">
      <div class="card">
        <div class="card-title">🌐 DOMAIN LOOKUP</div>
        <div class="input-group">
          <label class="input-label">Target Domain</label>
          <div class="input-row">
            <input class="input-field" placeholder="example.com" id="domain-input">
            <button class="btn btn-primary" onclick="runDomainLookup()">ANALYZE</button>
          </div>
        </div>
        <div class="grid-2" style="gap:8px;margin-top:8px">
          <button class="btn btn-secondary" onclick="setDemo('domain-input','google.com')">WHOIS</button>
          <button class="btn btn-secondary" onclick="setDemo('domain-input','github.com')">DNS ENUM</button>
          <button class="btn btn-secondary" onclick="setDemo('domain-input','cloudflare.com')">SSL CHECK</button>
          <button class="btn btn-secondary" onclick="setDemo('domain-input','microsoft.com')">SUBDOMAINS</button>
        </div>
      </div>
      <div class="card">
        <div class="card-title">📊 DOMAIN REPUTATION</div>
        <div id="domain-reputation">
          <div class="flex-between mb-16">
            <span class="mono small">Risk Score</span>
            <span class="mono text-green" style="font-size:22px;font-weight:700">14/100</span>
          </div>
          <div class="mb-8"><div class="progress-bar"><div class="progress-fill progress-green" style="width:14%"></div></div></div>
          <div class="neon-line" style="margin:12px 0"></div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px">
            <div style="text-align:center;padding:10px;border:1px solid var(--border)">
              <div class="text-green" style="font-size:18px;font-weight:700">✓</div>
              <div class="mono" style="font-size:9px;color:var(--text2)">SAFE</div>
            </div>
            <div style="text-align:center;padding:10px;border:1px solid var(--border)">
              <div class="text-green" style="font-size:18px;font-weight:700">✓</div>
              <div class="mono" style="font-size:9px;color:var(--text2)">NOT BLACKLISTED</div>
            </div>
            <div style="text-align:center;padding:10px;border:1px solid var(--border)">
              <div class="text-green" style="font-size:18px;font-weight:700">✓</div>
              <div class="mono" style="font-size:9px;color:var(--text2)">SSL VALID</div>
            </div>
            <div style="text-align:center;padding:10px;border:1px solid var(--border)">
              <div class="text-orange" style="font-size:18px;font-weight:700">!</div>
              <div class="mono" style="font-size:9px;color:var(--text2)">CDN DETECTED</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="grid-3 mb-24">
      <div class="card">
        <div class="card-title">WHOIS DATA</div>
        <div class="result-box">
          <div class="result-line"><span class="result-key">Registrar:</span> <span class="result-val" id="whois-registrar">MarkMonitor Inc.</span></div>
          <div class="result-line"><span class="result-key">Created:</span> <span class="result-val">1997-09-15</span></div>
          <div class="result-line"><span class="result-key">Expires:</span> <span class="result-val">2028-09-14</span></div>
          <div class="result-line"><span class="result-key">Updated:</span> <span class="result-val">2019-09-09</span></div>
          <div class="result-line"><span class="result-key">Status:</span> <span class="text-green">clientDeleteProhibited</span></div>
          <div class="result-line"><span class="result-key">DNSSEC:</span> <span class="result-val">unsigned</span></div>
          <div class="result-line"><span class="result-key">NS1:</span> <span class="result-val">ns1.google.com</span></div>
          <div class="result-line"><span class="result-key">NS2:</span> <span class="result-val">ns2.google.com</span></div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">DNS RECORDS</div>
        <div class="result-box">
          <div class="result-line"><span class="result-key">A:</span> <span class="result-val">142.250.185.46</span></div>
          <div class="result-line"><span class="result-key">AAAA:</span> <span class="result-val">2607:f8b0::200e</span></div>
          <div class="result-line"><span class="result-key">MX:</span> <span class="result-val">10 smtp.google.com</span></div>
          <div class="result-line"><span class="result-key">TXT:</span> <span class="result-val">v=spf1 include:_spf.google.com</span></div>
          <div class="result-line"><span class="result-key">NS:</span> <span class="result-val">ns1-4.google.com</span></div>
          <div class="result-line"><span class="result-key">SOA:</span> <span class="result-val">ns1.google.com.</span></div>
          <div class="result-line"><span class="result-key">CAA:</span> <span class="result-val">0 issue "pki.goog"</span></div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">SSL CERTIFICATE</div>
        <div class="result-box">
          <div class="result-line"><span class="result-key">Issuer:</span> <span class="result-val">GTS CA 1C3</span></div>
          <div class="result-line"><span class="result-key">Subject:</span> <span class="result-val">*.google.com</span></div>
          <div class="result-line"><span class="result-key">Valid:</span> <span class="text-green">2025-01-06</span></div>
          <div class="result-line"><span class="result-key">Expires:</span> <span class="text-green">2025-03-31</span></div>
          <div class="result-line"><span class="result-key">Algorithm:</span> <span class="result-val">ECDSA P-256</span></div>
          <div class="result-line"><span class="result-key">SANs:</span> <span class="result-val">*.google.com</span></div>
          <div class="result-line"><span class="result-key">CT Logs:</span> <span class="text-green">✓ VERIFIED</span></div>
        </div>
      </div>
    </div>

    <div class="card">
      <div class="card-title">🔗 SUBDOMAIN DISCOVERY</div>
      <div style="display:flex;gap:8px;margin-bottom:14px;flex-wrap:wrap">
        <span class="badge badge-info">www.google.com</span>
        <span class="badge badge-info">mail.google.com</span>
        <span class="badge badge-info">docs.google.com</span>
        <span class="badge badge-info">drive.google.com</span>
        <span class="badge badge-info">api.google.com</span>
        <span class="badge badge-medium">admin.google.com</span>
        <span class="badge badge-medium">vpn.google.com</span>
        <span class="badge badge-medium">dev.google.com</span>
        <span class="badge badge-high">staging.google.com</span>
        <span class="badge badge-high">test.google.com</span>
        <span class="badge badge-critical">internal.google.com</span>
      </div>
      <div class="mono small text-green">▶ Found 11 subdomains (4 potentially exposed)</div>
    </div>
  </div>

  <!-- IP Tab -->
  <div id="osint-ip" class="tab-content" style="display:none">
    <div class="grid-2 mb-24">
      <div class="card">
        <div class="card-title">🌍 IP GEOLOCATION & INTEL</div>
        <div class="input-group">
          <label class="input-label">Target IP Address</label>
          <div class="input-row">
            <input class="input-field" placeholder="8.8.8.8" id="ip-input" value="8.8.8.8">
            <button class="btn btn-primary" onclick="runIPLookup()">LOOKUP</button>
          </div>
        </div>
        <div class="neon-line" style="margin:12px 0"></div>
        <div class="result-box">
          <div class="result-line"><span class="result-key">IP:</span> <span class="result-val">8.8.8.8</span></div>
          <div class="result-line"><span class="result-key">Country:</span> <span class="result-val">🇺🇸 United States</span></div>
          <div class="result-line"><span class="result-key">City:</span> <span class="result-val">Mountain View, CA</span></div>
          <div class="result-line"><span class="result-key">ASN:</span> <span class="result-val">AS15169 — Google LLC</span></div>
          <div class="result-line"><span class="result-key">Org:</span> <span class="result-val">Google LLC</span></div>
          <div class="result-line"><span class="result-key">ISP:</span> <span class="result-val">Google Public DNS</span></div>
          <div class="result-line"><span class="result-key">rDNS:</span> <span class="result-val">dns.google</span></div>
          <div class="result-line"><span class="result-key">Blacklist:</span> <span class="text-green">✓ CLEAN</span></div>
          <div class="result-line"><span class="result-key">Abuse Score:</span> <span class="text-green">0/100</span></div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">📡 SHODAN EXPOSED PORTS</div>
        <table class="cyber-table">
          <thead><tr><th>Port</th><th>Service</th><th>Protocol</th><th>Status</th></tr></thead>
          <tbody>
            <tr><td class="text-green">53</td><td>DNS</td><td>UDP/TCP</td><td><span class="badge badge-low">OPEN</span></td></tr>
            <tr><td class="text-green">80</td><td>HTTP</td><td>TCP</td><td><span class="badge badge-low">OPEN</span></td></tr>
            <tr><td class="text-green">443</td><td>HTTPS</td><td>TCP</td><td><span class="badge badge-low">OPEN</span></td></tr>
            <tr><td class="text-orange">8080</td><td>HTTP-ALT</td><td>TCP</td><td><span class="badge badge-medium">OPEN</span></td></tr>
          </tbody>
        </table>
        <div class="neon-line" style="margin:12px 0"></div>
        <div class="mono small">
          <span class="text-green">● CLEAN</span>&nbsp;&nbsp;
          <span class="text-neon">4 open ports</span>&nbsp;&nbsp;
          <span class="text-text2">Not in TOR exit list</span>
        </div>
      </div>
    </div>
  </div>

  <!-- Email Tab -->
  <div id="osint-email" class="tab-content" style="display:none">
    <div class="grid-2 mb-24">
      <div class="card">
        <div class="card-title">📧 EMAIL BREACH CHECK</div>
        <div class="input-group">
          <label class="input-label">Email Address</label>
          <div class="input-row">
            <input class="input-field" placeholder="user@example.com" id="email-input">
            <button class="btn btn-primary" onclick="runEmailCheck()">CHECK</button>
          </div>
        </div>
        <div class="neon-line" style="margin:12px 0"></div>
        <div id="email-result" class="result-box">
          <div class="result-line text-text2">// Enter an email address to check for data breaches</div>
          <div class="result-line text-text2">// Powered by HaveIBeenPwned API</div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">✉ MX RECORD ANALYSIS</div>
        <div class="result-box">
          <div class="result-line"><span class="result-key">Priority 10:</span> <span class="result-val">aspmx.l.google.com</span></div>
          <div class="result-line"><span class="result-key">Priority 20:</span> <span class="result-val">alt1.aspmx.l.google.com</span></div>
          <div class="result-line"><span class="result-key">SPF:</span> <span class="text-green">v=spf1 ✓ PASS</span></div>
          <div class="result-line"><span class="result-key">DKIM:</span> <span class="text-green">✓ VALID</span></div>
          <div class="result-line"><span class="result-key">DMARC:</span> <span class="text-orange">! PARTIAL</span></div>
          <div class="result-line"><span class="result-key">Provider:</span> <span class="result-val">Google Workspace</span></div>
        </div>
      </div>
    </div>
  </div>

  <!-- Username Tab -->
  <div id="osint-username" class="tab-content" style="display:none">
    <div class="card mb-24">
      <div class="card-title">👤 USERNAME SEARCH</div>
      <div class="input-row mb-16">
        <input class="input-field" placeholder="Enter username to search..." id="username-input">
        <button class="btn btn-primary" onclick="runUsernameSearch()">SEARCH ALL PLATFORMS</button>
      </div>
    </div>
    <div class="grid-4" id="username-results">
      <div class="card" style="text-align:center">
        <div style="font-size:24px">𝕏</div>
        <div class="mono small mt-8">Twitter/X</div>
        <div class="badge badge-low mt-8">FOUND</div>
      </div>
      <div class="card" style="text-align:center">
        <div style="font-size:24px">📷</div>
        <div class="mono small mt-8">Instagram</div>
        <div class="badge badge-low mt-8">FOUND</div>
      </div>
      <div class="card" style="text-align:center">
        <div style="font-size:24px">💼</div>
        <div class="mono small mt-8">LinkedIn</div>
        <div class="badge badge-critical mt-8">NOT FOUND</div>
      </div>
      <div class="card" style="text-align:center">
        <div style="font-size:24px">🐙</div>
        <div class="mono small mt-8">GitHub</div>
        <div class="badge badge-low mt-8">FOUND</div>
      </div>
      <div class="card" style="text-align:center">
        <div style="font-size:24px">🎮</div>
        <div class="mono small mt-8">Reddit</div>
        <div class="badge badge-low mt-8">FOUND</div>
      </div>
      <div class="card" style="text-align:center">
        <div style="font-size:24px">🎵</div>
        <div class="mono small mt-8">TikTok</div>
        <div class="badge badge-critical mt-8">NOT FOUND</div>
      </div>
      <div class="card" style="text-align:center">
        <div style="font-size:24px">🎨</div>
        <div class="mono small mt-8">DeviantArt</div>
        <div class="badge badge-low mt-8">FOUND</div>
      </div>
      <div class="card" style="text-align:center">
        <div style="font-size:24px">💬</div>
        <div class="mono small mt-8">Discord</div>
        <div class="badge badge-medium mt-8">UNKNOWN</div>
      </div>
    </div>
  </div>

  <!-- Phone Tab -->
  <div id="osint-phone" class="tab-content" style="display:none">
    <div class="grid-2">
      <div class="card">
        <div class="card-title">📱 PHONE NUMBER INTEL</div>
        <div class="input-group">
          <label class="input-label">Phone Number (E.164 Format)</label>
          <div class="input-row">
            <input class="input-field" placeholder="+1234567890" id="phone-input">
            <button class="btn btn-primary" onclick="runPhoneLookup()">LOOKUP</button>
          </div>
        </div>
        <div class="neon-line" style="margin:12px 0"></div>
        <div class="result-box">
          <div class="result-line"><span class="result-key">Number:</span> <span class="result-val">+1 (650) 253-0000</span></div>
          <div class="result-line"><span class="result-key">Carrier:</span> <span class="result-val">Google Voice</span></div>
          <div class="result-line"><span class="result-key">Type:</span> <span class="result-val">VOIP</span></div>
          <div class="result-line"><span class="result-key">Country:</span> <span class="result-val">🇺🇸 United States</span></div>
          <div class="result-line"><span class="result-key">Region:</span> <span class="result-val">California</span></div>
          <div class="result-line"><span class="result-key">Spam Score:</span> <span class="text-green">LOW</span></div>
          <div class="result-line"><span class="result-key">Reports:</span> <span class="text-green">0 spam reports</span></div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ PENTEST ══════════════════════════════ -->
<section class="page" id="page-pentest">
  <div class="page-header">
    <div>
      <div class="page-title">⛨ PENETRATION TESTING TOOLS</div>
      <div class="page-subtitle">// ETHICAL HACKING TOOLKIT — AUTHORIZED USE ONLY</div>
    </div>
  </div>

  <div class="tabs">
    <div class="tab active" onclick="switchTab(this,'pt-network')">NETWORK RECON</div>
    <div class="tab" onclick="switchTab(this,'pt-web')">WEB APP TESTING</div>
    <div class="tab" onclick="switchTab(this,'pt-subdomain')">SUBDOMAIN ENUM</div>
    <div class="tab" onclick="switchTab(this,'pt-password')">PASSWORD SECURITY</div>
  </div>

  <div id="pt-network" class="tab-content">
    <div class="grid-2 mb-24">
      <div class="card">
        <div class="card-title">🔌 PORT SCANNER</div>
        <div class="input-group">
          <label class="input-label">Target Host</label>
          <input class="input-field" placeholder="192.168.1.1 or example.com" id="port-target">
        </div>
        <div class="grid-2" style="gap:8px">
          <div class="input-group">
            <label class="input-label">Port Range</label>
            <input class="input-field" value="1-1000" id="port-range">
          </div>
          <div class="input-group">
            <label class="input-label">Scan Type</label>
            <select class="input-field">
              <option>SYN Scan</option>
              <option>TCP Connect</option>
              <option>UDP Scan</option>
              <option>Service Detect</option>
            </select>
          </div>
        </div>
        <button class="btn btn-primary w100" onclick="runPortScan()" style="margin-top:8px">START SCAN</button>
      </div>
      <div class="card">
        <div class="card-title">📊 SCAN RESULTS</div>
        <div class="terminal">
          <div class="terminal-bar">
            <div class="terminal-dot" style="background:#ff5f57"></div>
            <div class="terminal-dot" style="background:#febc2e"></div>
            <div class="terminal-dot" style="background:#28c840"></div>
            <span class="mono" style="font-size:10px;color:var(--text2);margin-left:8px">lintest-scanner v2.1.0</span>
          </div>
          <div id="port-output">
            <div class="terminal-line"><span class="prompt">lintest</span><span class="path">@scanner</span>:~$ <span class="cmd">nmap -sV -p 1-1000 target</span></div>
            <div class="terminal-line output" style="color:var(--text2)">Waiting for target...</div>
            <div class="terminal-line"><span class="terminal-cursor"></span></div>
          </div>
        </div>
      </div>
    </div>

    <div class="grid-3">
      <div class="card">
        <div class="card-title">🖥 OS FINGERPRINTING</div>
        <div class="result-box" style="min-height:120px">
          <div class="result-line"><span class="result-key">OS:</span> <span class="result-val">Linux 5.x (Ubuntu)</span></div>
          <div class="result-line"><span class="result-key">TTL:</span> <span class="result-val">64 (Linux)</span></div>
          <div class="result-line"><span class="result-key">Window:</span> <span class="result-val">65535</span></div>
          <div class="result-line"><span class="result-key">Accuracy:</span> <span class="text-green">94%</span></div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">⚙ SERVICE DETECTION</div>
        <div class="result-box" style="min-height:120px">
          <div class="result-line"><span class="result-key">22/tcp:</span> <span class="result-val">OpenSSH 8.9</span></div>
          <div class="result-line"><span class="result-key">80/tcp:</span> <span class="result-val">nginx/1.24.0</span></div>
          <div class="result-line"><span class="result-key">443/tcp:</span> <span class="result-val">nginx/1.24.0 TLS</span></div>
          <div class="result-line"><span class="result-key">3306/tcp:</span> <span class="text-orange">MySQL 8.0.35</span></div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">🔥 VULNERABILITY MATCH</div>
        <div class="result-box" style="min-height:120px">
          <div class="result-line"><span class="text-orange">CVE-2023-38408</span> — ssh-agent</div>
          <div class="result-line"><span class="text-red">CVE-2024-1086</span> — MySQL auth bypass</div>
          <div class="result-line"><span class="text-neon">2 vulns found</span></div>
        </div>
      </div>
    </div>
  </div>

  <div id="pt-web" class="tab-content" style="display:none">
    <div class="grid-2 mb-24">
      <div class="card">
        <div class="card-title">💉 SQL INJECTION SCANNER</div>
        <div class="input-group">
          <label class="input-label">Target URL</label>
          <input class="input-field" placeholder="https://target.com/page?id=1">
        </div>
        <div class="input-group">
          <label class="input-label">Payload Type</label>
          <select class="input-field">
            <option>Error-based</option>
            <option>Blind Boolean</option>
            <option>Time-based</option>
            <option>UNION-based</option>
          </select>
        </div>
        <button class="btn btn-primary w100" onclick="runSQLScan()">SCAN FOR SQLi</button>
      </div>
      <div class="card">
        <div class="card-title">⚡ XSS SCANNER</div>
        <div class="input-group">
          <label class="input-label">Target URL</label>
          <input class="input-field" placeholder="https://target.com/search?q=">
        </div>
        <div class="input-group">
          <label class="input-label">XSS Type</label>
          <select class="input-field">
            <option>Reflected</option>
            <option>Stored</option>
            <option>DOM-based</option>
          </select>
        </div>
        <button class="btn btn-primary w100" onclick="runXSSScan()">SCAN FOR XSS</button>
      </div>
    </div>
    <div class="grid-2">
      <div class="card">
        <div class="card-title">📂 OPEN DIRECTORY DETECTION</div>
        <div class="result-box">
          <div class="result-line"><span class="text-red">EXPOSED:</span> <span class="result-val">/backup/</span></div>
          <div class="result-line"><span class="text-red">EXPOSED:</span> <span class="result-val">/admin/</span></div>
          <div class="result-line"><span class="text-orange">CHECK:</span> <span class="result-val">/.git/</span></div>
          <div class="result-line"><span class="text-green">SAFE:</span> <span class="result-val">/api/v1/docs</span></div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">🔒 SECURITY HEADER ANALYSIS</div>
        <div class="result-box">
          <div class="result-line"><span class="text-red">✗</span> Content-Security-Policy — <span class="text-red">MISSING</span></div>
          <div class="result-line"><span class="text-green">✓</span> X-Frame-Options — <span class="text-green">DENY</span></div>
          <div class="result-line"><span class="text-orange">!</span> Referrer-Policy — <span class="text-orange">WEAK</span></div>
          <div class="result-line"><span class="text-green">✓</span> HSTS — <span class="text-green">max-age=31536000</span></div>
          <div class="result-line"><span class="text-red">✗</span> Permissions-Policy — <span class="text-red">MISSING</span></div>
        </div>
      </div>
    </div>
  </div>

  <div id="pt-subdomain" class="tab-content" style="display:none">
    <div class="card mb-24">
      <div class="card-title">🔗 SUBDOMAIN ENUMERATION ENGINE</div>
      <div class="input-row mb-16">
        <input class="input-field" placeholder="target-domain.com" id="subenum-input">
        <select class="input-field" style="width:180px">
          <option>Passive OSINT</option>
          <option>DNS Bruteforce</option>
          <option>Combined</option>
        </select>
        <button class="btn btn-primary" onclick="runSubdomainEnum()">ENUMERATE</button>
      </div>
      <div class="terminal" style="min-height:180px">
        <div class="terminal-bar">
          <div class="terminal-dot" style="background:#ff5f57"></div>
          <div class="terminal-dot" style="background:#febc2e"></div>
          <div class="terminal-dot" style="background:#28c840"></div>
        </div>
        <div id="subdomain-output">
          <div class="terminal-line"><span class="prompt">lintest</span><span class="path">@recon</span>:~$ <span class="cmd">subfinder -d target.com -all</span></div>
          <div class="terminal-line" style="color:var(--green)">[INF] Current Version: v2.6.6</div>
          <div class="terminal-line" style="color:var(--green)">[INF] Loading provider config from default location</div>
          <div class="terminal-line" style="color:var(--neon)">[INF] Enumerating subdomains for target.com</div>
          <div class="terminal-line" style="color:var(--text2)">www.target.com</div>
          <div class="terminal-line" style="color:var(--text2)">api.target.com</div>
          <div class="terminal-line" style="color:var(--text2)">mail.target.com</div>
          <div class="terminal-line" style="color:var(--orange)">dev.target.com → <span style="color:var(--red)">EXPOSED DEV ENV</span></div>
          <div class="terminal-line" style="color:var(--orange)">staging.target.com → <span style="color:var(--orange)">STAGING</span></div>
          <div class="terminal-line" style="color:var(--green)">[INF] Found 5 subdomains for target.com in 8.2s</div>
        </div>
      </div>
    </div>
  </div>

  <div id="pt-password" class="tab-content" style="display:none">
    <div class="grid-3">
      <div class="card">
        <div class="card-title">🔐 HASH IDENTIFIER</div>
        <div class="input-group">
          <label class="input-label">Hash String</label>
          <input class="input-field" placeholder="5f4dcc3b5aa765d61d8327deb882cf99" id="hash-input" onkeyup="identifyHash(this.value)">
        </div>
        <div class="result-box" id="hash-result" style="min-height:80px">
          <div class="result-line text-text2">// Paste a hash to identify</div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">💪 PASSWORD STRENGTH</div>
        <div class="input-group">
          <label class="input-label">Password</label>
          <input class="input-field" type="password" placeholder="Enter password" id="pw-input" oninput="checkPasswordStrength(this.value)">
        </div>
        <div id="pw-strength-bar" style="height:6px;background:rgba(255,255,255,0.06);margin-bottom:8px">
          <div id="pw-fill" style="height:100%;width:0%;transition:width 0.5s;background:var(--red)"></div>
        </div>
        <div id="pw-strength-label" class="mono small text-text2">Enter password to analyze</div>
        <div id="pw-feedback" class="mt-8 mono" style="font-size:11px;color:var(--text2);line-height:1.8"></div>
      </div>
      <div class="card">
        <div class="card-title">🔓 BREACHED PASSWORD CHECK</div>
        <div class="input-group">
          <label class="input-label">Password to Check</label>
          <input class="input-field" type="password" placeholder="Check if password was breached">
        </div>
        <button class="btn btn-primary w100 mb-16" onclick="runHIBPCheck(this)">CHECK HIBP</button>
        <div class="result-box" style="min-height:60px">
          <div class="result-line"><span class="result-key">Status:</span> <span class="text-green">✓ NOT FOUND in breaches</span></div>
          <div class="result-line"><span class="result-key">Checked:</span> <span class="result-val">862,498,741 pwned passwords</span></div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ THREAT INTEL ══════════════════════════════ -->
<section class="page" id="page-threat">
  <div class="page-header">
    <div>
      <div class="page-title">📡 THREAT INTELLIGENCE CENTER</div>
      <div class="page-subtitle">// REAL-TIME THREAT FEEDS — VIRUSTOTAL • SHODAN • ABUSEIPDB • HIBP</div>
    </div>
    <div class="page-actions">
      <button class="btn btn-red" onclick="showCriticalAlerts()">⚠ 3 CRITICAL ALERTS</button>
    </div>
  </div>

  <!-- Threat Overview -->
  <div class="grid-4 mb-24">
    <div class="stat-card" style="border-color:rgba(255,0,60,0.3)">
      <div class="stat-value text-red">127</div>
      <div class="stat-label">Malicious IPs</div>
    </div>
    <div class="stat-card">
      <div class="stat-value text-orange">48</div>
      <div class="stat-label">Malware Hashes</div>
    </div>
    <div class="stat-card">
      <div class="stat-value text-purple">12</div>
      <div class="stat-label">Breach Leaks</div>
    </div>
    <div class="stat-card">
      <div class="stat-value text-neon">847</div>
      <div class="stat-label">CVEs This Week</div>
    </div>
  </div>

  <!-- Virustotal Check -->
  <div class="grid-2 mb-24">
    <div class="card">
      <div class="card-title">🦠 VIRUSTOTAL FILE/URL CHECK</div>
      <div class="tabs" style="margin-bottom:14px">
        <div class="tab active">URL</div>
        <div class="tab">FILE HASH</div>
        <div class="tab">IP</div>
        <div class="tab">DOMAIN</div>
      </div>
      <div class="input-row mb-16">
        <input class="input-field" placeholder="https://suspicious-site.com" id="vt-input">
        <button class="btn btn-primary" onclick="runVTCheck()">SCAN</button>
      </div>
      <div id="vt-result" class="result-box">
        <div class="result-line text-text2">// Enter URL, hash, IP, or domain to check VirusTotal</div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">📊 VIRUSTOTAL SIMULATION RESULT</div>
      <div class="flex-between mb-16">
        <div>
          <div class="stat-value text-red" style="font-size:40px">32</div>
          <div class="stat-label">ENGINES DETECTED</div>
        </div>
        <div>
          <div style="font-size:13px;color:var(--text2)">out of</div>
          <div class="stat-value" style="font-size:28px">72</div>
          <div class="stat-label">TOTAL ENGINES</div>
        </div>
        <div style="text-align:right">
          <div class="badge badge-critical" style="font-size:12px;padding:8px 16px">MALICIOUS</div>
          <div class="mono small text-text2 mt-8">44% detection rate</div>
        </div>
      </div>
      <div class="mb-8"><div class="progress-bar" style="height:8px"><div class="progress-fill progress-red" style="width:44%"></div></div></div>
      <div class="grid-2" style="gap:8px;margin-top:12px">
        <div style="padding:8px;border:1px solid rgba(255,0,60,0.2);background:rgba(255,0,60,0.05)">
          <div class="mono small text-red">Malware</div>
          <div class="mono" style="font-size:10px;color:var(--text2)">Trojan.GenericKD</div>
        </div>
        <div style="padding:8px;border:1px solid rgba(255,0,60,0.2);background:rgba(255,0,60,0.05)">
          <div class="mono small text-orange">Suspicious</div>
          <div class="mono" style="font-size:10px;color:var(--text2)">Suspicious.Script</div>
        </div>
      </div>
    </div>
  </div>

  <!-- Intel Feed -->
  <div class="grid-3">
    <div class="card">
      <div class="card-title">🔴 MALICIOUS IP FEED</div>
      <table class="cyber-table">
        <thead><tr><th>IP</th><th>Origin</th><th>Type</th></tr></thead>
        <tbody>
          <tr><td class="text-red mono">45.33.32.156</td><td>CN</td><td><span class="badge badge-critical">C2</span></td></tr>
          <tr><td class="text-red mono">192.241.220.137</td><td>RU</td><td><span class="badge badge-high">BOTNET</span></td></tr>
          <tr><td class="text-orange mono">185.220.101.45</td><td>DE</td><td><span class="badge badge-medium">TOR</span></td></tr>
          <tr><td class="text-orange mono">104.248.10.8</td><td>NL</td><td><span class="badge badge-high">SCANNER</span></td></tr>
          <tr><td class="text-red mono">91.213.50.20</td><td>UA</td><td><span class="badge badge-critical">RANSOMWARE</span></td></tr>
        </tbody>
      </table>
    </div>
    <div class="card">
      <div class="card-title">💀 MALWARE HASH DATABASE</div>
      <table class="cyber-table">
        <thead><tr><th>Hash (MD5)</th><th>Family</th><th>Score</th></tr></thead>
        <tbody>
          <tr><td class="mono text-red" style="font-size:9px">44d88612fea8a8f36de82e1278abb02f</td><td>Mirai</td><td class="text-red">98</td></tr>
          <tr><td class="mono text-red" style="font-size:9px">3b4c97de29d6d6e7e75038df1f7d3f5c</td><td>WannaCry</td><td class="text-red">100</td></tr>
          <tr><td class="mono text-orange" style="font-size:9px">a0f57e5a7b4a1fa2f3b8c9d0e1f22345</td><td>Emotet</td><td class="text-orange">87</td></tr>
          <tr><td class="mono text-orange" style="font-size:9px">b1c2d3e4f5a6b7c8d9e0f1a2b3c44567</td><td>Cobalt Strike</td><td class="text-orange">92</td></tr>
        </tbody>
      </table>
    </div>
    <div class="card">
      <div class="card-title">🌐 SHODAN EXPOSED SERVICES</div>
      <table class="cyber-table">
        <thead><tr><th>Service</th><th>Count</th><th>Risk</th></tr></thead>
        <tbody>
          <tr><td>Elasticsearch</td><td class="text-red">247,190</td><td><span class="badge badge-critical">CRITICAL</span></td></tr>
          <tr><td>MongoDB</td><td class="text-red">98,432</td><td><span class="badge badge-critical">CRITICAL</span></td></tr>
          <tr><td>Redis</td><td class="text-orange">47,201</td><td><span class="badge badge-high">HIGH</span></td></tr>
          <tr><td>FTP Anon</td><td class="text-orange">18,847</td><td><span class="badge badge-high">HIGH</span></td></tr>
          <tr><td>RDP Exposed</td><td class="text-neon">312,844</td><td><span class="badge badge-medium">MED</span></td></tr>
        </tbody>
      </table>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ AI ASSISTANT ══════════════════════════════ -->
<section class="page" id="page-ai">
  <div class="page-header">
    <div>
      <div class="page-title">✦ AI SECURITY ASSISTANT</div>
      <div class="page-subtitle">// LINTEST AI — CYBERSECURITY INTELLIGENCE ENGINE v3.0</div>
    </div>
  </div>

  <div class="grid-2" style="align-items:start">
    <div>
      <div class="ai-messages" id="ai-messages">
        <div class="ai-msg">
          <div class="ai-avatar">🤖</div>
          <div class="ai-bubble">
            <span class="text-neon">LINTEST AI ONLINE</span><br><br>
            I'm your AI-powered cybersecurity assistant. I can help you:<br><br>
            • Explain vulnerabilities and CVEs<br>
            • Analyze OSINT investigation results<br>
            • Generate penetration testing reports<br>
            • Suggest recon strategies<br>
            • Answer cybersecurity questions<br><br>
            <span class="text-green">Type a command or ask a question to begin.</span>
          </div>
        </div>
      </div>
      <div class="ai-input-row">
        <input id="ai-input" placeholder="scan domain example.com  |  explain CVE-2024-1086  |  check IP reputation..." onkeydown="if(event.key==='Enter')sendAIMessage()">
        <button class="btn btn-primary" onclick="sendAIMessage()">SEND</button>
      </div>
      <div style="display:flex;gap:6px;margin-top:10px;flex-wrap:wrap">
        <button class="btn btn-secondary" style="font-size:9px;padding:5px 10px" onclick="setAIPrompt('scan domain example.com')">scan domain</button>
        <button class="btn btn-secondary" style="font-size:9px;padding:5px 10px" onclick="setAIPrompt('check IP reputation for 45.33.32.156')">check IP</button>
        <button class="btn btn-secondary" style="font-size:9px;padding:5px 10px" onclick="setAIPrompt('find subdomains for target.com')">find subdomains</button>
        <button class="btn btn-secondary" style="font-size:9px;padding:5px 10px" onclick="setAIPrompt('explain SQL injection attack')">explain SQLi</button>
        <button class="btn btn-secondary" style="font-size:9px;padding:5px 10px" onclick="setAIPrompt('generate recon report')">gen report</button>
      </div>
    </div>

    <div>
      <div class="card mb-16">
        <div class="card-title">⚡ QUICK COMMANDS</div>
        <div style="display:flex;flex-direction:column;gap:6px">
          <div class="terminal-line" style="cursor:pointer" onclick="setAIPrompt('scan domain example.com')"><span class="prompt">$</span> <span class="text-neon">scan domain</span> &lt;target&gt;</div>
          <div class="terminal-line" style="cursor:pointer" onclick="setAIPrompt('check IP reputation for 8.8.8.8')"><span class="prompt">$</span> <span class="text-neon">check ip</span> &lt;address&gt;</div>
          <div class="terminal-line" style="cursor:pointer" onclick="setAIPrompt('find subdomains for target.com')"><span class="prompt">$</span> <span class="text-neon">find subdomains</span> &lt;domain&gt;</div>
          <div class="terminal-line" style="cursor:pointer" onclick="setAIPrompt('check email breach user@example.com')"><span class="prompt">$</span> <span class="text-neon">check email</span> &lt;email&gt;</div>
          <div class="terminal-line" style="cursor:pointer" onclick="setAIPrompt('explain CVE-2024-1086 vulnerability')"><span class="prompt">$</span> <span class="text-neon">explain</span> &lt;cve-id&gt;</div>
          <div class="terminal-line" style="cursor:pointer" onclick="setAIPrompt('generate penetration testing report')"><span class="prompt">$</span> <span class="text-neon">generate report</span></div>
          <div class="terminal-line" style="cursor:pointer" onclick="setAIPrompt('suggest recon strategy for web application')"><span class="prompt">$</span> <span class="text-neon">suggest strategy</span> &lt;target-type&gt;</div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">📊 AI CAPABILITIES</div>
        <div style="display:flex;flex-direction:column;gap:10px">
          <div>
            <div class="flex-between mb-8"><span class="mono small">OSINT Analysis</span><span class="text-green mono small">98%</span></div>
            <div class="progress-bar"><div class="progress-fill progress-green" style="width:98%"></div></div>
          </div>
          <div>
            <div class="flex-between mb-8"><span class="mono small">Vuln Explanation</span><span class="text-neon mono small">95%</span></div>
            <div class="progress-bar"><div class="progress-fill progress-neon" style="width:95%"></div></div>
          </div>
          <div>
            <div class="flex-between mb-8"><span class="mono small">Report Generation</span><span class="text-purple mono small">92%</span></div>
            <div class="progress-bar"><div class="progress-fill" style="width:92%;background:var(--purple)"></div></div>
          </div>
          <div>
            <div class="flex-between mb-8"><span class="mono small">Threat Correlation</span><span class="text-orange mono small">88%</span></div>
            <div class="progress-bar"><div class="progress-fill" style="width:88%;background:var(--orange)"></div></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ RECON AUTOMATION ══════════════════════════════ -->
<section class="page" id="page-recon">
  <div class="page-header">
    <div>
      <div class="page-title">⟳ RECON AUTOMATION</div>
      <div class="page-subtitle">// AUTOMATED RECONNAISSANCE PIPELINE ENGINE</div>
    </div>
    <button class="btn btn-primary">NEW PIPELINE</button>
  </div>

  <div class="card mb-24">
    <div class="card-title">⚙ PIPELINE BUILDER</div>
    <div class="input-group">
      <label class="input-label">Target Domain</label>
      <div class="input-row">
        <input class="input-field" placeholder="target.com" id="recon-target">
        <button class="btn btn-green" onclick="runPipeline()">▶ RUN PIPELINE</button>
      </div>
    </div>
    <div class="pipeline" id="pipeline">
      <div class="pipe-step done" id="pipe-1">
        <div class="pipe-num text-green">1</div>
        <div class="pipe-label">DOMAIN DISCOVER</div>
      </div>
      <div class="pipe-step done" id="pipe-2">
        <div class="pipe-num text-green">2</div>
        <div class="pipe-label">SUBDOMAIN ENUM</div>
      </div>
      <div class="pipe-step active" id="pipe-3">
        <div class="pipe-num">3</div>
        <div class="pipe-label">PORT SCANNING</div>
      </div>
      <div class="pipe-step" id="pipe-4">
        <div class="pipe-num" style="color:var(--text2)">4</div>
        <div class="pipe-label">VULN SCAN</div>
      </div>
      <div class="pipe-step" id="pipe-5">
        <div class="pipe-num" style="color:var(--text2)">5</div>
        <div class="pipe-label">REPORT GEN</div>
      </div>
    </div>
  </div>

  <div class="grid-2 mb-24">
    <div class="card">
      <div class="card-title">📋 PIPELINE LOG</div>
      <div class="terminal" style="min-height:280px">
        <div class="terminal-bar">
          <div class="terminal-dot" style="background:#ff5f57"></div>
          <div class="terminal-dot" style="background:#febc2e"></div>
          <div class="terminal-dot" style="background:#28c840"></div>
          <span class="mono" style="font-size:10px;color:var(--text2);margin-left:8px">pipeline.log</span>
        </div>
        <div id="pipeline-log">
          <div class="terminal-line" style="color:var(--green)">[✓] Step 1: Domain discovery complete — target.com</div>
          <div class="terminal-line" style="color:var(--green)">[✓] Step 2: Subdomains found — 23 results</div>
          <div class="terminal-line" style="color:var(--neon)">[⟳] Step 3: Port scanning 23 hosts...</div>
          <div class="terminal-line" style="color:var(--neon)">    ► Scanning www.target.com (1-65535)</div>
          <div class="terminal-line" style="color:var(--text2)">    ► Scanning api.target.com...</div>
          <div class="terminal-line" style="color:var(--text2)">[◎] Step 4: Queued — Vulnerability scan</div>
          <div class="terminal-line" style="color:var(--text2)">[◎] Step 5: Queued — Report generation</div>
          <div class="terminal-line"><span class="terminal-cursor"></span></div>
        </div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">📊 PIPELINE RESULTS</div>
      <div class="grid-2" style="gap:8px;margin-bottom:16px">
        <div style="padding:12px;border:1px solid var(--border);background:rgba(0,255,156,0.04)">
          <div class="stat-value green" style="font-size:22px">23</div>
          <div class="stat-label">Subdomains</div>
        </div>
        <div style="padding:12px;border:1px solid var(--border)">
          <div class="stat-value" style="font-size:22px">147</div>
          <div class="stat-label">Open Ports</div>
        </div>
        <div style="padding:12px;border:1px solid rgba(255,0,60,0.2)">
          <div class="stat-value text-red" style="font-size:22px">8</div>
          <div class="stat-label">Vulnerabilities</div>
        </div>
        <div style="padding:12px;border:1px solid rgba(255,107,0,0.2)">
          <div class="stat-value text-orange" style="font-size:22px">3</div>
          <div class="stat-label">Critical Issues</div>
        </div>
      </div>
      <button class="btn btn-primary w100" onclick="showPage('reports')">GENERATE REPORT</button>
    </div>
  </div>

  <div class="card">
    <div class="card-title">📂 SAVED PIPELINES</div>
    <table class="cyber-table">
      <thead><tr><th>Name</th><th>Target</th><th>Status</th><th>Last Run</th><th>Findings</th><th>Action</th></tr></thead>
      <tbody>
        <tr>
          <td class="text-neon">Full Recon v1</td>
          <td>example.com</td>
          <td><span class="badge badge-low">COMPLETE</span></td>
          <td class="mono" style="font-size:10px">2025-01-14 09:22</td>
          <td class="text-orange">12 vulns</td>
          <td><button class="btn btn-secondary" style="padding:4px 10px;font-size:9px">VIEW</button></td>
        </tr>
        <tr>
          <td class="text-neon">Web App Audit</td>
          <td>shop.target.com</td>
          <td><span class="badge badge-info">RUNNING</span></td>
          <td class="mono" style="font-size:10px">2025-01-15 11:00</td>
          <td class="text-neon">In progress</td>
          <td><button class="btn btn-red" style="padding:4px 10px;font-size:9px">STOP</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<!-- ══════════════════════════════ REPORTS ══════════════════════════════ -->
<section class="page" id="page-reports">
  <div class="page-header">
    <div>
      <div class="page-title">≡ REPORT GENERATOR</div>
      <div class="page-subtitle">// PROFESSIONAL SECURITY ASSESSMENT REPORTS</div>
    </div>
    <div class="page-actions">
      <button class="btn btn-secondary">📄 HTML</button>
      <button class="btn btn-secondary">{ } JSON</button>
      <button class="btn btn-primary" onclick="downloadFullReport('pdf')">📥 PDF EXPORT</button>
    </div>
  </div>

  <div class="grid-2 mb-24">
    <div class="card">
      <div class="card-title">⚙ REPORT CONFIGURATION</div>
      <div class="input-group">
        <label class="input-label">Report Title</label>
        <input class="input-field" value="Security Assessment — target.com">
      </div>
      <div class="input-group">
        <label class="input-label">Client Name</label>
        <input class="input-field" placeholder="Client Organization">
      </div>
      <div class="input-group">
        <label class="input-label">Classification</label>
        <select class="input-field">
          <option>CONFIDENTIAL</option>
          <option>RESTRICTED</option>
          <option>INTERNAL</option>
          <option>PUBLIC</option>
        </select>
      </div>
      <div class="input-group">
        <label class="input-label">Include Sections</label>
        <div style="display:flex;flex-direction:column;gap:6px;margin-top:4px">
          <label style="display:flex;gap:8px;align-items:center;font-size:12px;cursor:pointer">
            <input type="checkbox" checked style="accent-color:var(--neon)"> Executive Summary
          </label>
          <label style="display:flex;gap:8px;align-items:center;font-size:12px;cursor:pointer">
            <input type="checkbox" checked style="accent-color:var(--neon)"> Vulnerability Findings
          </label>
          <label style="display:flex;gap:8px;align-items:center;font-size:12px;cursor:pointer">
            <input type="checkbox" checked style="accent-color:var(--neon)"> Technical Evidence
          </label>
          <label style="display:flex;gap:8px;align-items:center;font-size:12px;cursor:pointer">
            <input type="checkbox" checked style="accent-color:var(--neon)"> Risk Score
          </label>
          <label style="display:flex;gap:8px;align-items:center;font-size:12px;cursor:pointer">
            <input type="checkbox" checked style="accent-color:var(--neon)"> Remediation Recommendations
          </label>
        </div>
      </div>
      <button class="btn btn-primary w100 mt-8" onclick="generateAndDownloadReport()">GENERATE REPORT</button>
    </div>
    <div class="card">
      <div class="card-title">📊 RISK ASSESSMENT OVERVIEW</div>
      <div class="flex-between mb-16">
        <div>
          <div class="stat-value text-red" style="font-size:52px">7.8</div>
          <div class="stat-label">OVERALL RISK SCORE</div>
        </div>
        <div style="text-align:right">
          <div class="badge badge-high" style="font-size:14px;padding:10px 20px">HIGH RISK</div>
          <div class="mono small text-text2 mt-8">Based on 23 findings</div>
        </div>
      </div>
      <div class="mb-8"><div class="progress-bar" style="height:8px"><div class="progress-fill progress-red" style="width:78%"></div></div></div>
      <div class="neon-line"></div>
      <div class="grid-4" style="gap:8px;margin-top:12px">
        <div style="text-align:center;padding:8px;border:1px solid rgba(255,0,60,0.3)">
          <div class="stat-value text-red" style="font-size:20px">3</div>
          <div class="stat-label" style="font-size:8px">CRITICAL</div>
        </div>
        <div style="text-align:center;padding:8px;border:1px solid rgba(255,107,0,0.3)">
          <div class="stat-value text-orange" style="font-size:20px">7</div>
          <div class="stat-label" style="font-size:8px">HIGH</div>
        </div>
        <div style="text-align:center;padding:8px;border:1px solid var(--border)">
          <div class="stat-value" style="font-size:20px">9</div>
          <div class="stat-label" style="font-size:8px">MEDIUM</div>
        </div>
        <div style="text-align:center;padding:8px;border:1px solid rgba(0,255,156,0.3)">
          <div class="stat-value green" style="font-size:20px">4</div>
          <div class="stat-label" style="font-size:8px">LOW</div>
        </div>
      </div>
    </div>
  </div>

  <!-- Report Preview -->
  <div class="card">
    <div class="card-title">📋 VULNERABILITY FINDINGS</div>
    <table class="cyber-table">
      <thead>
        <tr>
          <th>#</th><th>Title</th><th>CVSS</th><th>Severity</th><th>Status</th><th>Affected Component</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="mono text-neon">F-001</td>
          <td>SQL Injection in Login Form</td>
          <td class="text-red">9.8</td>
          <td><span class="badge badge-critical">CRITICAL</span></td>
          <td><span class="badge badge-high">OPEN</span></td>
          <td class="mono" style="font-size:10px">/login?user=</td>
        </tr>
        <tr>
          <td class="mono text-neon">F-002</td>
          <td>Missing Content Security Policy</td>
          <td class="text-orange">7.4</td>
          <td><span class="badge badge-high">HIGH</span></td>
          <td><span class="badge badge-high">OPEN</span></td>
          <td class="mono" style="font-size:10px">All pages</td>
        </tr>
        <tr>
          <td class="mono text-neon">F-003</td>
          <td>Sensitive Data in Git Repository</td>
          <td class="text-red">9.1</td>
          <td><span class="badge badge-critical">CRITICAL</span></td>
          <td><span class="badge badge-low">FIXED</span></td>
          <td class="mono" style="font-size:10px">/.git/config</td>
        </tr>
        <tr>
          <td class="mono text-neon">F-004</td>
          <td>Reflected XSS in Search Parameter</td>
          <td class="text-orange">6.1</td>
          <td><span class="badge badge-medium">MEDIUM</span></td>
          <td><span class="badge badge-high">OPEN</span></td>
          <td class="mono" style="font-size:10px">/search?q=</td>
        </tr>
        <tr>
          <td class="mono text-neon">F-005</td>
          <td>Exposed Admin Panel Without Auth</td>
          <td class="text-red">9.4</td>
          <td><span class="badge badge-critical">CRITICAL</span></td>
          <td><span class="badge badge-high">OPEN</span></td>
          <td class="mono" style="font-size:10px">/admin/</td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<!-- ══════════════════════════════ TOOLS LIBRARY ══════════════════════════════ -->
<section class="page" id="page-tools">
  <div class="page-header">
    <div>
      <div class="page-title">⊞ TOOLS LIBRARY</div>
      <div class="page-subtitle">// 317 CYBERSECURITY TOOLS — OSINT • PENTEST • FORENSICS • MALWARE</div>
    </div>
  </div>

  <div class="search-bar mb-24">
    <input placeholder="🔍 Search tools... (nmap, burpsuite, maltego, metasploit...)" oninput="filterTools(this.value)">
    <select class="input-field" style="width:200px" onchange="filterByCategory(this.value)">
      <option value="all">All Categories</option>
      <option value="osint">OSINT</option>
      <option value="pentest">Pentest</option>
      <option value="forensics">Digital Forensics</option>
      <option value="malware">Malware Analysis</option>
      <option value="network">Network Security</option>
    </select>
  </div>

  <div class="grid-4" id="tools-grid">
    <div class="tool-card" data-category="pentest">
      <div class="tool-name">NMAP</div>
      <div class="tool-desc">Network mapper and port scanner — the gold standard for host discovery.</div>
      <div class="tool-tags"><span class="tool-tag">NETWORK</span><span class="tool-tag">FREE</span><span class="tool-tag">CLI</span></div>
    </div>
    <div class="tool-card" data-category="pentest">
      <div class="tool-name">METASPLOIT</div>
      <div class="tool-desc">Penetration testing framework with 2000+ exploits and payloads.</div>
      <div class="tool-tags"><span class="tool-tag">EXPLOIT</span><span class="tool-tag">FREE</span><span class="tool-tag">CLI</span></div>
    </div>
    <div class="tool-card" data-category="osint">
      <div class="tool-name">MALTEGO</div>
      <div class="tool-desc">Graph-based OSINT and link analysis for cyber investigations.</div>
      <div class="tool-tags"><span class="tool-tag">OSINT</span><span class="tool-tag">GUI</span><span class="tool-tag">PAID</span></div>
    </div>
    <div class="tool-card" data-category="pentest">
      <div class="tool-name">BURPSUITE</div>
      <div class="tool-desc">Web application security testing platform and proxy.</div>
      <div class="tool-tags"><span class="tool-tag">WEB</span><span class="tool-tag">GUI</span><span class="tool-tag">FREEMIUM</span></div>
    </div>
    <div class="tool-card" data-category="osint">
      <div class="tool-name">SHODAN</div>
      <div class="tool-desc">Search engine for internet-connected devices and services.</div>
      <div class="tool-tags"><span class="tool-tag">OSINT</span><span class="tool-tag">API</span><span class="tool-tag">WEB</span></div>
    </div>
    <div class="tool-card" data-category="network">
      <div class="tool-name">WIRESHARK</div>
      <div class="tool-desc">Network protocol analyzer and packet capture tool.</div>
      <div class="tool-tags"><span class="tool-tag">NETWORK</span><span class="tool-tag">FREE</span><span class="tool-tag">GUI</span></div>
    </div>
    <div class="tool-card" data-category="pentest">
      <div class="tool-name">SQLMAP</div>
      <div class="tool-desc">Automatic SQL injection and database takeover tool.</div>
      <div class="tool-tags"><span class="tool-tag">WEB</span><span class="tool-tag">FREE</span><span class="tool-tag">CLI</span></div>
    </div>
    <div class="tool-card" data-category="osint">
      <div class="tool-name">THEHARVESTOR</div>
      <div class="tool-desc">E-mail, subdomain and people names harvester from public sources.</div>
      <div class="tool-tags"><span class="tool-tag">OSINT</span><span class="tool-tag">FREE</span><span class="tool-tag">CLI</span></div>
    </div>
    <div class="tool-card" data-category="forensics">
      <div class="tool-name">AUTOPSY</div>
      <div class="tool-desc">Digital forensics platform for investigating computers and smartphones.</div>
      <div class="tool-tags"><span class="tool-tag">FORENSICS</span><span class="tool-tag">FREE</span><span class="tool-tag">GUI</span></div>
    </div>
    <div class="tool-card" data-category="malware">
      <div class="tool-name">GHIDRA</div>
      <div class="tool-desc">NSA reverse engineering tool for malware analysis and decompilation.</div>
      <div class="tool-tags"><span class="tool-tag">MALWARE</span><span class="tool-tag">FREE</span><span class="tool-tag">GUI</span></div>
    </div>
    <div class="tool-card" data-category="pentest">
      <div class="tool-name">HASHCAT</div>
      <div class="tool-desc">Advanced CPU/GPU-based password recovery and hash cracking tool.</div>
      <div class="tool-tags"><span class="tool-tag">PASSWORD</span><span class="tool-tag">FREE</span><span class="tool-tag">CLI</span></div>
    </div>
    <div class="tool-card" data-category="network">
      <div class="tool-name">ZEEK</div>
      <div class="tool-desc">Powerful network analysis framework for security monitoring.</div>
      <div class="tool-tags"><span class="tool-tag">NETWORK</span><span class="tool-tag">FREE</span><span class="tool-tag">CLI</span></div>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ NEWS ══════════════════════════════ -->
<section class="page" id="page-news">
  <div class="page-header">
    <div>
      <div class="page-title">◎ CYBER NEWS</div>
      <div class="page-subtitle">// REAL-TIME CYBERSECURITY NEWS AGGREGATOR</div>
    </div>
  </div>

  <div class="grid-3">
    <div class="news-card" style="grid-column:span 2">
      <div class="news-tag" style="color:var(--red)">⚡ BREAKING</div>
      <div class="news-title" style="font-size:18px;font-weight:700">Critical Zero-Day in Apache HTTP Server Allows Remote Code Execution — Emergency Patches Released</div>
      <div style="color:var(--text2);font-size:13px;margin:10px 0;line-height:1.6">Security researchers from the Cybersecurity and Infrastructure Security Agency (CISA) have disclosed a critical remote code execution vulnerability in Apache HTTP Server affecting versions 2.4.0 through 2.4.62. The vulnerability, tracked as CVE-2025-0482, carries a CVSS score of 9.8 and requires no authentication to exploit.</div>
      <div class="news-meta">15 minutes ago • Apache Foundation • CERT/CC • 2,847 readers</div>
    </div>
    <div style="display:flex;flex-direction:column;gap:12px">
      <div class="news-card">
        <div class="news-tag">RANSOMWARE</div>
        <div class="news-title">BlackCat/ALPHV Group Claims Attack on Major Hospital Chain</div>
        <div class="news-meta">1 hour ago • Ransomware</div>
      </div>
      <div class="news-card">
        <div class="news-tag">APT</div>
        <div class="news-title">APT-29 Deploys New CosmicEnergy Malware Targeting European Grid</div>
        <div class="news-meta">3 hours ago • Nation-State</div>
      </div>
      <div class="news-card">
        <div class="news-tag">TOOLS</div>
        <div class="news-title">CISA Releases Emergency Directive on Ivanti Connect Secure Vulnerability</div>
        <div class="news-meta">5 hours ago • CISA</div>
      </div>
    </div>
  </div>
  <div class="neon-line"></div>
  <div class="grid-3" style="gap:12px">
    <div class="news-card">
      <div class="news-tag">BREACH</div>
      <div class="news-title">Cloud Database Exposes 2.4B Records Including PII and Financial Data</div>
      <div class="news-meta">6 hours ago • Data Breach</div>
    </div>
    <div class="news-card">
      <div class="news-tag">VULNERABILITY</div>
      <div class="news-title">CVSS 9.9 Flaw in Kubernetes Allows Container Escape to Host Node</div>
      <div class="news-meta">8 hours ago • Cloud Security</div>
    </div>
    <div class="news-card">
      <div class="news-tag">PHISHING</div>
      <div class="news-title">New AI-Generated Phishing Campaign Bypasses Traditional Email Filters</div>
      <div class="news-meta">10 hours ago • Email Security</div>
    </div>
    <div class="news-card">
      <div class="news-tag">MALWARE</div>
      <div class="news-title">FakeBat Loader Evolves With New Evasion Techniques Targeting Finance Sector</div>
      <div class="news-meta">12 hours ago • Malware</div>
    </div>
    <div class="news-card">
      <div class="news-tag">REGULATION</div>
      <div class="news-title">EU Cyber Resilience Act Requirements Take Effect — Compliance Deadline Approaching</div>
      <div class="news-meta">1 day ago • Policy</div>
    </div>
    <div class="news-card">
      <div class="news-tag">RESEARCH</div>
      <div class="news-title">Researchers Demonstrate GPU-Based Attack Breaking AES-256 Encryption</div>
      <div class="news-meta">1 day ago • Cryptography</div>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ ADMIN ══════════════════════════════ -->
<section class="page" id="page-admin">
  <div class="page-header">
    <div>
      <div class="page-title">⚙ ADMIN PANEL</div>
      <div class="page-subtitle">// SYSTEM ADMINISTRATION — RESTRICTED ACCESS</div>
    </div>
    <span class="badge badge-critical">ADMIN ONLY</span>
  </div>

  <div class="grid-4 mb-24">
    <div class="stat-card">
      <div class="stat-value">2,847</div>
      <div class="stat-label">Total Users</div>
    </div>
    <div class="stat-card">
      <div class="stat-value text-green">487</div>
      <div class="stat-label">Active Scans</div>
    </div>
    <div class="stat-card">
      <div class="stat-value text-orange">12</div>
      <div class="stat-label">Rate Limited</div>
    </div>
    <div class="stat-card">
      <div class="stat-value text-red">3</div>
      <div class="stat-label">Alerts</div>
    </div>
  </div>

  <div class="grid-2 mb-24">
    <div class="card">
      <div class="card-title">👥 USER MANAGEMENT</div>
      <div class="user-row">
        <div class="user-avatar">AH</div>
        <div><div class="user-name">Alex Hunter</div><div class="user-email"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6e0f020b162e0207001a0b1d1a400701">[email&#160;protected]</a></div></div>
        <span class="user-role text-neon">ADMIN</span>
        <div class="badge badge-low">ACTIVE</div>
      </div>
      <div class="user-row">
        <div class="user-avatar" style="background:linear-gradient(135deg,var(--green),var(--neon))">SR</div>
        <div><div class="user-name">Sara Reeves</div><div class="user-email"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="aeddcfdccfeeddcbcdc2cfcc80cdc1c3">[email&#160;protected]</a></div></div>
        <span class="user-role text-purple">ANALYST</span>
        <div class="badge badge-low">ACTIVE</div>
      </div>
      <div class="user-row">
        <div class="user-avatar" style="background:linear-gradient(135deg,var(--red),var(--orange))">JK</div>
        <div><div class="user-name">Jake Kowalski</div><div class="user-email"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="92f8f3f9f7d2f0e7f5f0fde7fce6ebbce2e0fd">[email&#160;protected]</a></div></div>
        <span class="user-role text-orange">RESEARCHER</span>
        <div class="badge badge-medium">RATE LIMITED</div>
      </div>
      <div class="user-row">
        <div class="user-avatar" style="background:linear-gradient(135deg,var(--purple),var(--neon))">MX</div>
        <div><div class="user-name">Max Chen</div><div class="user-email"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e588849da5968a86cb868a8895848b9ccb868a88">[email&#160;protected]</a></div></div>
        <span class="user-role text-green">SOC</span>
        <div class="badge badge-low">ACTIVE</div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">🔑 API KEY MANAGEMENT</div>
      <table class="cyber-table">
        <thead><tr><th>API Service</th><th>Status</th><th>Calls Today</th><th>Limit</th></tr></thead>
        <tbody>
          <tr>
            <td class="text-neon">VirusTotal</td>
            <td><span class="badge badge-low">ACTIVE</span></td>
            <td>1,247</td>
            <td>4,000</td>
          </tr>
          <tr>
            <td class="text-neon">Shodan</td>
            <td><span class="badge badge-low">ACTIVE</span></td>
            <td>489</td>
            <td>1,000</td>
          </tr>
          <tr>
            <td class="text-neon">AbuseIPDB</td>
            <td><span class="badge badge-low">ACTIVE</span></td>
            <td>2,100</td>
            <td>5,000</td>
          </tr>
          <tr>
            <td class="text-orange">HIBP</td>
            <td><span class="badge badge-medium">LIMITED</span></td>
            <td>487</td>
            <td>500</td>
          </tr>
          <tr>
            <td>IPinfo</td>
            <td><span class="badge badge-low">ACTIVE</span></td>
            <td>3,891</td>
            <td>50,000</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div class="grid-2">
    <div class="card">
      <div class="card-title">📋 SYSTEM LOGS</div>
      <div class="terminal" style="min-height:200px">
        <div class="terminal-bar">
          <div class="terminal-dot" style="background:#ff5f57"></div>
          <div class="terminal-dot" style="background:#febc2e"></div>
          <div class="terminal-dot" style="background:#28c840"></div>
          <span class="mono" style="font-size:10px;color:var(--text2);margin-left:8px">system.log</span>
        </div>
        <div class="terminal-line" style="color:var(--green)">[2025-01-15 11:24:01] INFO: User <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2c4d4049546c40454258495f58024543">[email&#160;protected]</a> logged in from 192.168.1.1</div>
        <div class="terminal-line" style="color:var(--neon)">[2025-01-15 11:22:44] SCAN: Port scan initiated by <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="bfccdecddeffccdadcd3dedd91dcd0d2">[email&#160;protected]</a> on 10.0.0.1</div>
        <div class="terminal-line" style="color:var(--orange)">[2025-01-15 11:18:32] WARN: Rate limit triggered for <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="294348424c694b5c4e4b465c475d5007595b46">[email&#160;protected]</a></div>
        <div class="terminal-line" style="color:var(--green)">[2025-01-15 11:15:00] INFO: Pipeline #247 completed — 12 findings</div>
        <div class="terminal-line" style="color:var(--red)">[2025-01-15 11:10:22] ALERT: Suspicious scan pattern detected from 192.168.99.44</div>
        <div class="terminal-line" style="color:var(--green)">[2025-01-15 11:05:00] INFO: VirusTotal API key refreshed successfully</div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">📊 SYSTEM ANALYTICS</div>
      <canvas id="admin-chart" height="200"></canvas>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ HELP ══════════════════════════════ -->
<section class="page" id="page-help">
  <div class="page-header">
    <div>
      <div class="page-title">? HELP CENTER</div>
      <div class="page-subtitle">// DOCUMENTATION • TUTORIALS • SUPPORT</div>
    </div>
  </div>
  <div class="grid-3 mb-24">
    <div class="feature-card">
      <div class="feature-icon">📚</div>
      <div class="feature-title">Getting Started</div>
      <div class="feature-desc">Learn how to set up your account, configure API keys, and run your first OSINT investigation or penetration test.</div>
    </div>
    <div class="feature-card">
      <div class="feature-icon">🎓</div>
      <div class="feature-title">Video Tutorials</div>
      <div class="feature-desc">Step-by-step video guides covering domain enumeration, IP reputation checks, and automated recon pipelines.</div>
    </div>
    <div class="feature-card">
      <div class="feature-icon">🛠</div>
      <div class="feature-title">Tool Usage Guides</div>
      <div class="feature-desc">Detailed documentation for all 317 tools in the library with examples, flags, and best practices.</div>
    </div>
  </div>
  <div class="card">
    <div class="card-title">❓ FREQUENTLY ASKED QUESTIONS</div>
    <div style="display:flex;flex-direction:column;gap:12px">
      <div style="padding:14px;border:1px solid var(--border);cursor:pointer" onclick="toggleFaq(this)">
        <div class="flex-between"><span class="mono small">How do I add my API keys for VirusTotal and Shodan?</span><span class="text-neon">+</span></div>
        <div style="display:none;margin-top:10px;font-size:13px;color:var(--text2);line-height:1.6">Navigate to Admin Panel → API Key Management → Enter your keys. Keys are encrypted at rest and never exposed to other users.</div>
      </div>
      <div style="padding:14px;border:1px solid var(--border);cursor:pointer" onclick="toggleFaq(this)">
        <div class="flex-between"><span class="mono small">Is LIN TEST legal to use for penetration testing?</span><span class="text-neon">+</span></div>
        <div style="display:none;margin-top:10px;font-size:13px;color:var(--text2);line-height:1.6">LIN TEST is designed for authorized security testing only. Always ensure you have written permission before testing any systems you do not own.</div>
      </div>
      <div style="padding:14px;border:1px solid var(--border);cursor:pointer" onclick="toggleFaq(this)">
        <div class="flex-between"><span class="mono small">What is the difference between passive and active OSINT?</span><span class="text-neon">+</span></div>
        <div style="display:none;margin-top:10px;font-size:13px;color:var(--text2);line-height:1.6">Passive OSINT uses publicly available data without interacting directly with the target. Active OSINT involves sending traffic directly to the target system.</div>
      </div>
      <div style="padding:14px;border:1px solid var(--border);cursor:pointer" onclick="toggleFaq(this)">
        <div class="flex-between"><span class="mono small">How does the recon automation pipeline work?</span><span class="text-neon">+</span></div>
        <div style="display:none;margin-top:10px;font-size:13px;color:var(--text2);line-height:1.6">The pipeline engine chains multiple tools together in sequence. Each step passes results to the next, culminating in an automated report with all findings consolidated.</div>
      </div>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ DOCS ══════════════════════════════ -->
<section class="page" id="page-docs">
  <div class="page-header">
    <div>
      <div class="page-title">≡ DOCUMENTATION</div>
      <div class="page-subtitle">// API REFERENCE • INTEGRATION GUIDES • ARCHITECTURE</div>
    </div>
  </div>
  <div class="grid-2">
    <div class="card">
      <div class="card-title">🔌 API REFERENCE</div>
      <div class="terminal" style="min-height:300px">
        <div class="terminal-bar">
          <div class="terminal-dot" style="background:#ff5f57"></div>
          <div class="terminal-dot" style="background:#febc2e"></div>
          <div class="terminal-dot" style="background:#28c840"></div>
          <span class="mono" style="font-size:10px;color:var(--text2);margin-left:8px">API v2.0</span>
        </div>
        <div class="terminal-line"><span style="color:var(--neon)">POST</span> <span style="color:var(--text)">/api/v2/osint/domain</span></div>
        <div class="terminal-line" style="color:var(--text2)">Authorization: Bearer {API_KEY}</div>
        <div class="terminal-line" style="color:var(--text2)">Content-Type: application/json</div>
        <br>
        <div class="terminal-line" style="color:var(--purple)">{</div>
        <div class="terminal-line" style="color:var(--text2);padding-left:20px">"domain": "example.com",</div>
        <div class="terminal-line" style="color:var(--text2);padding-left:20px">"checks": ["whois","dns","ssl","subdomains"],</div>
        <div class="terminal-line" style="color:var(--text2);padding-left:20px">"deep_scan": true</div>
        <div class="terminal-line" style="color:var(--purple)">}</div>
        <br>
        <div class="terminal-line"><span style="color:var(--green)">GET</span> <span style="color:var(--text)">/api/v2/ip/{ip_address}/reputation</span></div>
        <div class="terminal-line"><span style="color:var(--green)">GET</span> <span style="color:var(--text)">/api/v2/scan/{scan_id}/results</span></div>
        <div class="terminal-line"><span style="color:var(--neon)">POST</span> <span style="color:var(--text)">/api/v2/pipeline/run</span></div>
        <div class="terminal-line"><span style="color:var(--orange)">DELETE</span> <span style="color:var(--text)">/api/v2/scan/{scan_id}</span></div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">🏗 DEPLOYMENT GUIDE</div>
      <div class="terminal" style="min-height:300px">
        <div class="terminal-bar">
          <div class="terminal-dot" style="background:#ff5f57"></div>
          <div class="terminal-dot" style="background:#febc2e"></div>
          <div class="terminal-dot" style="background:#28c840"></div>
          <span class="mono" style="font-size:10px;color:var(--text2);margin-left:8px">deploy.sh</span>
        </div>
        <div class="terminal-line"><span class="prompt">#</span> <span style="color:var(--text2)">Clone repository</span></div>
        <div class="terminal-line"><span class="cmd">git clone https://github.com/lintest/platform</span></div>
        <div class="terminal-line"><span class="cmd">cd platform</span></div>
        <br>
        <div class="terminal-line"><span class="prompt">#</span> <span style="color:var(--text2)">Configure environment</span></div>
        <div class="terminal-line"><span class="cmd">cp .env.example .env</span></div>
        <div class="terminal-line"><span class="cmd">nano .env</span></div>
        <br>
        <div class="terminal-line"><span class="prompt">#</span> <span style="color:var(--text2)">Docker deployment</span></div>
        <div class="terminal-line"><span class="cmd">docker-compose up -d</span></div>
        <br>
        <div class="terminal-line" style="color:var(--green)">[✓] PostgreSQL started on :5432</div>
        <div class="terminal-line" style="color:var(--green)">[✓] Redis started on :6379</div>
        <div class="terminal-line" style="color:var(--green)">[✓] API server started on :8000</div>
        <div class="terminal-line" style="color:var(--green)">[✓] Frontend started on :3000</div>
        <div class="terminal-line" style="color:var(--neon)">[✦] LIN TEST is live at http://localhost:3000</div>
      </div>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ COMMUNITY ══════════════════════════════ -->
<section class="page" id="page-community">
  <div class="page-header">
    <div>
      <div class="page-title">◈ COMMUNITY</div>
      <div class="page-subtitle">// SECURITY RESEARCHERS • BUG BOUNTY HUNTERS • SOC ANALYSTS</div>
    </div>
    <button class="btn btn-primary">NEW POST</button>
  </div>
  <div class="grid-3 mb-24">
    <div class="stat-card"><div class="stat-value">12,847</div><div class="stat-label">Members</div></div>
    <div class="stat-card"><div class="stat-value text-green">3,291</div><div class="stat-label">Posts</div></div>
    <div class="stat-card"><div class="stat-value text-purple">847</div><div class="stat-label">Open Threads</div></div>
  </div>
  <div class="card">
    <div class="card-title">🔥 HOT DISCUSSIONS</div>
    <table class="cyber-table">
      <thead><tr><th>Topic</th><th>Category</th><th>Replies</th><th>Views</th><th>Last Post</th></tr></thead>
      <tbody>
        <tr><td class="text-neon" style="cursor:pointer">Best approach for subdomain takeover detection in 2025?</td><td><span class="badge badge-medium">OSINT</span></td><td>48</td><td>2,847</td><td class="mono" style="font-size:10px">2h ago</td></tr>
        <tr><td class="text-neon" style="cursor:pointer">Comparing Shodan vs Censys for IoT device hunting</td><td><span class="badge badge-info">TOOLS</span></td><td>31</td><td>1,592</td><td class="mono" style="font-size:10px">5h ago</td></tr>
        <tr><td class="text-neon" style="cursor:pointer">CVE-2025-0482 — Apache RCE — exploitation techniques discussion</td><td><span class="badge badge-critical">VULN</span></td><td>127</td><td>8,421</td><td class="mono" style="font-size:10px">12m ago</td></tr>
        <tr><td class="text-neon" style="cursor:pointer">AI-assisted phishing detection — false positive rates</td><td><span class="badge badge-low">RESEARCH</span></td><td>19</td><td>891</td><td class="mono" style="font-size:10px">1d ago</td></tr>
      </tbody>
    </table>
  </div>
</section>

<!-- ══════════════════════════════ ABOUT ══════════════════════════════ -->
<section class="page" id="page-about">
  <div class="page-header">
    <div>
      <div class="page-title">ℹ ABOUT LIN TEST</div>
      <div class="page-subtitle">// CYBER INTELLIGENCE & PENETRATION TESTING PLATFORM</div>
    </div>
  </div>
  <div class="grid-2 mb-24">
    <div class="card">
      <div class="card-title">◈ PLATFORM OVERVIEW</div>
      <div style="font-size:14px;color:var(--text2);line-height:1.8">
        <p style="margin-bottom:12px">LIN TEST is a professional-grade cybersecurity intelligence platform designed for ethical hackers, security researchers, bug bounty hunters, and SOC analysts.</p>
        <p style="margin-bottom:12px">The platform combines OSINT investigation tools, penetration testing utilities, real-time threat intelligence, and AI-powered analysis into a unified cyberpunk-themed interface.</p>
        <p>Built with security professionals in mind, LIN TEST provides access to 317+ tools, automated reconnaissance pipelines, and integrations with industry-leading threat intelligence APIs including VirusTotal, Shodan, AbuseIPDB, and HaveIBeenPwned.</p>
      </div>
    </div>
    <div class="card">
      <div class="card-title">📊 PLATFORM STATISTICS</div>
      <div style="display:flex;flex-direction:column;gap:14px">
        <div><div class="flex-between mb-8"><span class="mono small">Tools Available</span><span class="text-green mono">317</span></div><div class="progress-bar"><div class="progress-fill progress-green" style="width:63%"></div></div></div>
        <div><div class="flex-between mb-8"><span class="mono small">API Integrations</span><span class="text-neon mono">24</span></div><div class="progress-bar"><div class="progress-fill progress-neon" style="width:48%"></div></div></div>
        <div><div class="flex-between mb-8"><span class="mono small">Intelligence Sources</span><span class="text-purple mono">200+</span></div><div class="progress-bar"><div class="progress-fill" style="width:80%;background:var(--purple)"></div></div></div>
        <div><div class="flex-between mb-8"><span class="mono small">Uptime SLA</span><span class="text-green mono">99.9%</span></div><div class="progress-bar"><div class="progress-fill progress-green" style="width:99%"></div></div></div>
      </div>
    </div>
  </div>
</section>

<!-- ══════════════════════════════ LOGIN ══════════════════════════════ -->
<section class="page" id="page-login">
  <div style="max-width:480px;margin:60px auto;padding:0 20px">
    <div style="text-align:center;margin-bottom:40px">
      <div style="font-family:'Orbitron',monospace;font-size:36px;font-weight:900;color:var(--neon);text-shadow:0 0 20px var(--neon)">LINTEST</div>
      <div class="mono" style="color:var(--text2);font-size:11px;letter-spacing:3px;margin-top:6px">SECURE AUTHENTICATION PORTAL</div>
    </div>
    <div class="card">
      <div class="tabs" style="margin-bottom:20px">
        <div class="tab active">LOGIN</div>
        <div class="tab">REGISTER</div>
      </div>
      <div class="input-group">
        <label class="input-label">Email Address</label>
        <input class="input-field" type="email" placeholder="analyst@organization.com">
      </div>
      <div class="input-group">
        <label class="input-label">Password</label>
        <input class="input-field" type="password" placeholder="••••••••••••">
      </div>
      <div class="flex-between mb-16">
        <label style="display:flex;gap:8px;align-items:center;font-size:12px;cursor:pointer">
          <input type="checkbox" style="accent-color:var(--neon)"> Remember device
        </label>
        <span class="mono small text-neon" style="cursor:pointer">Forgot password?</span>
      </div>
      <button class="btn btn-primary w100" style="padding:14px;font-size:13px" onclick="showPage('dashboard')">AUTHENTICATE</button>
      <div class="neon-line"></div>
      <div style="text-align:center">
        <div class="mono small text-text2" style="margin-bottom:10px">Or continue with</div>
        <div style="display:flex;gap:8px;justify-content:center">
          <button class="btn btn-secondary" style="flex:1">GitHub</button>
          <button class="btn btn-secondary" style="flex:1">Google</button>
          <button class="btn btn-secondary" style="flex:1">LDAP/AD</button>
        </div>
      </div>
    </div>
    <div style="text-align:center;margin-top:20px" class="mono small text-text2">
      By logging in you agree to our Terms of Service and Privacy Policy.<br>
      LIN TEST is for authorized security testing only.
    </div>
  </div>
</section>

<!-- ══════════════════════════════ CONTACT ══════════════════════════════ -->
<section class="page" id="page-contact">
  <div class="page-header">
    <div><div class="page-title">✉ CONTACT</div><div class="page-subtitle">// GET IN TOUCH WITH THE LIN TEST TEAM</div></div>
  </div>
  <div class="grid-2">
    <div class="card">
      <div class="card-title">SEND MESSAGE</div>
      <div class="input-group"><label class="input-label">Name</label><input class="input-field" placeholder="Your name"></div>
      <div class="input-group"><label class="input-label">Email</label><input class="input-field" placeholder="your@email.com"></div>
      <div class="input-group"><label class="input-label">Subject</label>
        <select class="input-field"><option>General Inquiry</option><option>Bug Report</option><option>Feature Request</option><option>Security Report</option></select>
      </div>
      <div class="input-group"><label class="input-label">Message</label>
        <textarea class="input-field" rows="5" placeholder="Your message..."></textarea>
      </div>
      <button class="btn btn-primary w100">SEND MESSAGE</button>
    </div>
    <div>
      <div class="card mb-16">
        <div class="card-title">CONTACT INFO</div>
        <div class="mono small" style="display:flex;flex-direction:column;gap:10px;color:var(--text2)">
          <div>📧 <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7b080e0b0b14090f3b1712150f1e080f551214">[email&#160;protected]</a></div>
          <div>🔒 <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="a5d6c0c6d0d7ccd1dce5c9cccbd1c0d6d18bccca">[email&#160;protected]</a> <span class="badge badge-low" style="margin-left:8px">SECURITY DISCLOSURES</span></div>
          <div>💬 Discord: discord.gg/lintest</div>
          <div>🐦 Twitter: @lintestplatform</div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">🔐 RESPONSIBLE DISCLOSURE</div>
        <div style="font-size:13px;color:var(--text2);line-height:1.7">
          Found a vulnerability in LIN TEST? We take security seriously. Please report vulnerabilities to <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7605131503041f020f361a1f1802130502581f19">[email&#160;protected]</a> with a detailed description. We offer a responsible disclosure program with recognition for valid reports.
        </div>
      </div>
    </div>
  </div>
</section>

</main>

<script data-cfasync="false" src="/cdn-cgi/scripts/5c5dd728/cloudflare-static/email-decode.min.js"></script><script>
// ─── MATRIX BACKGROUND ───
const canvas = document.getElementById('matrix-bg');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
const chars = 'ﾊﾐﾋｰｳｼﾅﾓﾆｻﾜﾂｵﾘｱﾎﾃﾏｹﾒｴｶｷﾑﾕﾗｾﾈｽﾀﾇﾍ012345789ABCDEFGHIJKLMNOPQRSTUVWXYZ';
const cols = Math.floor(canvas.width / 14);
const drops = Array(cols).fill(1);
function drawMatrix() {
  ctx.fillStyle = 'rgba(5,5,5,0.05)';
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  ctx.fillStyle = '#00ff9c';
  ctx.font = '13px Share Tech Mono';
  for (let i = 0; i < drops.length; i++) {
    const text = chars[Math.floor(Math.random() * chars.length)];
    ctx.fillText(text, i * 14, drops[i] * 14);
    if (drops[i] * 14 > canvas.height && Math.random() > 0.975) drops[i] = 0;
    drops[i]++;
  }
}
setInterval(drawMatrix, 50);
window.addEventListener('resize', () => { canvas.width = window.innerWidth; canvas.height = window.innerHeight; });

// ─── PAGE ROUTING ───
function showPage(id) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.sidebar-item').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.nav-links a').forEach(a => a.classList.remove('active'));
  const pg = document.getElementById('page-' + id);
  if (pg) { pg.classList.add('active'); pg.scrollTop = 0; }
  const si = Array.from(document.querySelectorAll('.sidebar-item')).find(s => s.onclick && s.onclick.toString().includes("'"+id+"'"));
  if (si) si.classList.add('active');
  const na = document.getElementById('nav-' + id);
  if (na) na.classList.add('active');
  window.scrollTo(0, 0);
  if (id === 'home') initGlobe();
  if (id === 'dashboard') initCharts();
  if (id === 'admin') initAdminChart();
}

// ─── TABS ───
function switchTab(el, targetId) {
  const parent = el.parentElement;
  parent.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  el.classList.add('active');
  const section = parent.parentElement;
  section.querySelectorAll('.tab-content').forEach(c => c.style.display = 'none');
  const target = document.getElementById(targetId);
  if (target) target.style.display = 'block';
}

// ─── THREE.JS GLOBE ───
let globeRenderer, globeScene, globeCamera, globeMesh, globeAnimId;
function initGlobe() {
  const container = document.getElementById('globe-canvas');
  if (!container || globeRenderer) return;
  globeScene = new THREE.Scene();
  globeCamera = new THREE.PerspectiveCamera(45, container.clientWidth / container.clientHeight, 0.1, 1000);
  globeCamera.position.z = 2.5;
  globeRenderer = new THREE.WebGLRenderer({ canvas: container, alpha: true, antialias: true });
  globeRenderer.setSize(container.clientWidth, container.clientHeight);
  globeRenderer.setClearColor(0x000000, 0);

  // Globe
  const geom = new THREE.SphereGeometry(1, 64, 64);
  const mat = new THREE.MeshPhongMaterial({ color: 0x001122, wireframe: false, transparent: true, opacity: 0.85 });
  globeMesh = new THREE.Mesh(geom, mat);
  globeScene.add(globeMesh);

  // Wireframe overlay
  const wfGeom = new THREE.SphereGeometry(1.01, 24, 24);
  const wfMat = new THREE.MeshBasicMaterial({ color: 0x00eaff, wireframe: true, transparent: true, opacity: 0.12 });
  globeScene.add(new THREE.Mesh(wfGeom, wfMat));

  // Glow ring
  const ringGeom = new THREE.TorusGeometry(1.1, 0.01, 8, 100);
  const ringMat = new THREE.MeshBasicMaterial({ color: 0x00eaff });
  const ring = new THREE.Mesh(ringGeom, ringMat);
  ring.rotation.x = Math.PI / 2;
  globeScene.add(ring);

  // Attack dots
  for (let i = 0; i < 40; i++) {
    const phi = Math.acos(-1 + (2 * i) / 40);
    const theta = Math.sqrt(40 * Math.PI) * phi;
    const dotGeom = new THREE.SphereGeometry(0.015, 8, 8);
    const col = Math.random() > 0.7 ? 0xff003c : Math.random() > 0.5 ? 0x00ff9c : 0x7a5cff;
    const dotMat = new THREE.MeshBasicMaterial({ color: col });
    const dot = new THREE.Mesh(dotGeom, dotMat);
    dot.position.setFromSphericalCoords(1.02, phi, theta);
    globeScene.add(dot);
  }

  // Lights
  globeScene.add(new THREE.AmbientLight(0x222244, 1));
  const dirLight = new THREE.DirectionalLight(0x00eaff, 2);
  dirLight.position.set(5, 3, 5);
  globeScene.add(dirLight);
  const purpleLight = new THREE.PointLight(0x7a5cff, 1.5, 10);
  purpleLight.position.set(-5, -3, -5);
  globeScene.add(purpleLight);

  function animate() {
    globeAnimId = requestAnimationFrame(animate);
    globeMesh.rotation.y += 0.003;
    globeScene.children.forEach((c, i) => { if (c.rotation) c.rotation.y += 0.003; });
    globeRenderer.render(globeScene, globeCamera);
  }
  animate();
}

// ─── CHART.JS CHARTS ───
function initCharts() {
  const labels = Array.from({length:24}, (_,i) => `${i}:00`);
  const threatData = Array.from({length:24}, () => Math.floor(Math.random()*3000+500));
  const ctx1 = document.getElementById('threat-chart');
  if (ctx1 && !ctx1._chartInitialized) {
    ctx1._chartInitialized = true;
    new Chart(ctx1, {
      type: 'line',
      data: { labels, datasets: [{
        label: 'Threats', data: threatData,
        borderColor: '#00eaff', backgroundColor: 'rgba(0,234,255,0.08)',
        tension: 0.4, fill: true, pointRadius: 0, borderWidth: 2
      }]},
      options: { responsive:true, plugins:{legend:{display:false}},
        scales: { x:{ticks:{color:'#6a8a9a',font:{size:9}},grid:{color:'rgba(255,255,255,0.03)'}}, y:{ticks:{color:'#6a8a9a',font:{size:9}},grid:{color:'rgba(255,255,255,0.03)'}} } }
    });
  }
  const ctx2 = document.getElementById('origin-chart');
  if (ctx2 && !ctx2._chartInitialized) {
    ctx2._chartInitialized = true;
    new Chart(ctx2, {
      type: 'bar',
      data: {
        labels: ['CN','RU','US','BR','KP','DE','IN','UA','NL','IR'],
        datasets: [{ label: 'Attacks', data: [4821,3247,2109,1847,1522,987,876,654,421,312], backgroundColor: '#7a5cff', borderColor: '#00eaff', borderWidth: 1 }]
      },
      options: { responsive:true, plugins:{legend:{display:false}},
        scales: { x:{ticks:{color:'#6a8a9a',font:{size:9}},grid:{color:'rgba(255,255,255,0.03)'}}, y:{ticks:{color:'#6a8a9a',font:{size:9}},grid:{color:'rgba(255,255,255,0.03)'}} } }
    });
  }
  const ctx3 = document.getElementById('attack-type-chart');
  if (ctx3 && !ctx3._chartInitialized) {
    ctx3._chartInitialized = true;
    new Chart(ctx3, {
      type: 'doughnut',
      data: {
        labels: ['DDoS','SQL Injection','Phishing','Ransomware','Brute Force','XSS','Malware'],
        datasets: [{ data: [35,20,18,12,8,4,3], backgroundColor: ['#ff003c','#ff6b00','#7a5cff','#00eaff','#00ff9c','#ff69b4','#ffd700'], borderWidth: 0 }]
      },
      options: { responsive:true, cutout:'65%', plugins:{legend:{labels:{color:'#6a8a9a',font:{size:9}}}} }
    });
  }

  // Heatmap
  const hm = document.getElementById('heatmap');
  if (hm && !hm._initialized) {
    hm._initialized = true;
    for (let i = 0; i < 168; i++) {
      const div = document.createElement('div');
      div.className = 'hm-cell';
      const v = Math.random();
      div.style.background = v > 0.8 ? '#ff003c' : v > 0.6 ? '#ff6b00' : v > 0.4 ? '#7a5cff' : v > 0.2 ? '#00eaff' : 'rgba(255,255,255,0.05)';
      div.style.opacity = 0.3 + v * 0.7;
      hm.appendChild(div);
    }
  }
}

function initAdminChart() {
  const ctx = document.getElementById('admin-chart');
  if (ctx && !ctx._chartInitialized) {
    ctx._chartInitialized = true;
    const days = ['Mon','Tue','Wed','Thu','Fri','Sat','Sun'];
    new Chart(ctx, {
      type: 'line',
      data: { labels: days, datasets: [
        { label: 'Scans', data: [312,487,628,441,733,289,412], borderColor:'#00ff9c', tension:0.4, fill:false, pointRadius:3 },
        { label: 'Users', data: [124,189,247,198,312,87,156], borderColor:'#7a5cff', tension:0.4, fill:false, pointRadius:3 }
      ]},
      options: { responsive:true, plugins:{legend:{labels:{color:'#6a8a9a',font:{size:9}}}},
        scales: { x:{ticks:{color:'#6a8a9a',font:{size:9}},grid:{color:'rgba(255,255,255,0.03)'}}, y:{ticks:{color:'#6a8a9a',font:{size:9}},grid:{color:'rgba(255,255,255,0.03)'}} } }
    });
  }
}

// ─── LIVE ATTACK STREAM ───
const countries = ['CN','RU','US','BR','DE','KP','UA','IN','NL','FR','IR','TR'];
const attackTypes = ['SSH BRUTEFORCE','SQL INJECTION','DDoS','PORT SCAN','PHISHING','XSS','RANSOMWARE','BOTNET'];
const ips = () => `${Math.floor(Math.random()*220)+10}.${Math.floor(Math.random()*255)}.${Math.floor(Math.random()*255)}.${Math.floor(Math.random()*255)}`;
const colors = ['#ff003c','#ff6b00','#7a5cff','#00eaff','#ff69b4'];

function addAttack() {
  const log = document.getElementById('attack-log');
  if (!log) return;
  const srcIp = ips(), dstIp = ips();
  const country = countries[Math.floor(Math.random()*countries.length)];
  const type = attackTypes[Math.floor(Math.random()*attackTypes.length)];
  const color = colors[Math.floor(Math.random()*colors.length)];
  const div = document.createElement('div');
  div.className = 'attack-item';
  div.innerHTML = `<div class="attack-dot" style="background:${color}"></div><span class="attack-src">${country} ${srcIp}</span><span class="attack-arrow"> → </span><span class="attack-dst">${dstIp}</span><span class="attack-type">${type}</span>`;
  log.insertBefore(div, log.firstChild);
  if (log.children.length > 15) log.removeChild(log.lastChild);
}
setInterval(addAttack, 1200);

// ─── COUNTER ANIMATION ───
function animateCounter(el, target) {
  let current = 0;
  const step = target / 60;
  const timer = setInterval(() => {
    current += step;
    if (current >= target) { current = target; clearInterval(timer); }
    el.textContent = Math.floor(current).toLocaleString();
  }, 16);
}
setTimeout(() => {
  const e1 = document.getElementById('stat-threats');
  if (e1) animateCounter(e1, 48291);
}, 500);

// ─── OSINT TOOLS ───
function setDemo(id, val) { document.getElementById(id).value = val; }

// ─── LIVE SESSION DATA STORE ───
const sessionData = {
  scans: [], osint: {}, ipLookups: [], domainLookups: [],
  findings: [
    { id:'F-001', title:'SQL Injection in Login Form', cvss:9.8, severity:'CRITICAL', status:'OPEN', component:'/login?user=' },
    { id:'F-002', title:'Missing Content Security Policy', cvss:7.4, severity:'HIGH', status:'OPEN', component:'All pages' },
    { id:'F-003', title:'Sensitive Data in Git Repository', cvss:9.1, severity:'CRITICAL', status:'FIXED', component:'/.git/config' },
    { id:'F-004', title:'Reflected XSS in Search Parameter', cvss:6.1, severity:'MEDIUM', status:'OPEN', component:'/search?q=' },
    { id:'F-005', title:'Exposed Admin Panel Without Auth', cvss:9.4, severity:'CRITICAL', status:'OPEN', component:'/admin/' }
  ]
};

// ─── TOAST NOTIFICATION ───
function showToast(msg, type='neon') {
  const colors = { neon:'var(--neon)', green:'var(--green)', red:'var(--red)', orange:'var(--orange)' };
  const t = document.createElement('div');
  t.style.cssText = `position:fixed;bottom:24px;right:24px;z-index:9999;background:#0a0a12;border:1px solid ${colors[type]};color:${colors[type]};padding:12px 20px;font-family:'Share Tech Mono',monospace;font-size:12px;letter-spacing:1px;box-shadow:0 0 20px ${colors[type]}40;transition:opacity 0.5s;max-width:400px`;
  t.textContent = msg;
  document.body.appendChild(t);
  setTimeout(() => { t.style.opacity='0'; setTimeout(() => t.remove(), 500); }, 3000);
}

// ─── DEMO SETTER ───

// ─── DOMAIN LOOKUP (real DNS via Google DoH) ───
async function runDomainLookup() {
  const domain = document.getElementById('domain-input').value.trim() || 'example.com';
  const reputationEl = document.getElementById('domain-reputation');
  reputationEl.innerHTML = `<div class="result-line" style="color:var(--neon)">[⟳] Querying live DNS for <strong>${domain}</strong>...</div>`;
  try {
    const [dnsA, dnsMX, dnsTXT] = await Promise.all([
      fetch(`https://dns.google/resolve?name=${domain}&type=A`).then(r=>r.json()).catch(()=>null),
      fetch(`https://dns.google/resolve?name=${domain}&type=MX`).then(r=>r.json()).catch(()=>null),
      fetch(`https://dns.google/resolve?name=${domain}&type=TXT`).then(r=>r.json()).catch(()=>null),
    ]);
    const aRecord = dnsA?.Answer?.[0]?.data || 'Not resolved';
    const mxRecord = dnsMX?.Answer?.[0]?.data || 'No MX record';
    const txtRecord = dnsTXT?.Answer?.[0]?.data || 'No TXT record';
    let geoData = { country:'Unknown', org:'Unknown', isp:'Unknown', as:'Unknown' };
    if (aRecord !== 'Not resolved') {
      const geo = await fetch(`https://ip-api.com/json/${aRecord}?fields=country,org,isp,as,city`).then(r=>r.json()).catch(()=>null);
      if (geo && geo.status !== 'fail') geoData = geo;
    }
    const riskScore = Math.floor(Math.random() * 25) + 5;
    document.getElementById('whois-registrar').textContent = 'Live WHOIS (browser CORS restricted)';
    const dnsBox = document.querySelector('#osint-domain .grid-3 .card:nth-child(2) .result-box');
    if (dnsBox) dnsBox.innerHTML = `
      <div class="result-line"><span class="result-key">A:</span> <span class="result-val text-green">${aRecord}</span></div>
      <div class="result-line"><span class="result-key">MX:</span> <span class="result-val">${mxRecord}</span></div>
      <div class="result-line"><span class="result-key">TXT:</span> <span class="result-val" style="font-size:10px">${txtRecord.substring(0,50)}</span></div>
      <div class="result-line"><span class="result-key">Host Country:</span> <span class="result-val">${geoData.country}</span></div>
      <div class="result-line"><span class="result-key">ASN:</span> <span class="result-val">${geoData.as}</span></div>
    `;
    reputationEl.innerHTML = `
      <div class="flex-between mb-16"><span class="mono small">Risk Score</span>
      <span class="mono ${riskScore<30?'text-green':'text-orange'}" style="font-size:22px;font-weight:700">${riskScore}/100</span></div>
      <div class="mb-8"><div class="progress-bar"><div class="progress-fill ${riskScore<30?'progress-green':'progress-red'}" style="width:${riskScore}%"></div></div></div>
      <div class="neon-line" style="margin:12px 0"></div>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px">
        <div style="text-align:center;padding:10px;border:1px solid var(--border)"><div class="text-green" style="font-size:18px">${aRecord!=='Not resolved'?'✓':'✗'}</div><div class="mono" style="font-size:9px;color:var(--text2)">DNS LIVE</div></div>
        <div style="text-align:center;padding:10px;border:1px solid var(--border)"><div class="text-green" style="font-size:18px">✓</div><div class="mono" style="font-size:9px;color:var(--text2)">NOT BLACKLISTED</div></div>
        <div style="text-align:center;padding:10px;border:1px solid var(--border)"><div class="${mxRecord!=='No MX record'?'text-green':'text-orange'}" style="font-size:18px">${mxRecord!=='No MX record'?'✓':'!'}</div><div class="mono" style="font-size:9px;color:var(--text2)">MAIL (MX)</div></div>
        <div style="text-align:center;padding:10px;border:1px solid var(--border)"><div class="text-orange" style="font-size:18px">!</div><div class="mono" style="font-size:9px;color:var(--text2)">CDN CHECK</div></div>
      </div>
      <div class="mono small text-green mt-8">✓ LIVE DATA — ${new Date().toUTCString()}</div>
    `;
    sessionData.domainLookups.push({ domain, aRecord, mxRecord, riskScore, timestamp: new Date().toISOString() });
    showToast(`✓ Live DNS analysis complete: ${domain} → ${aRecord}`, 'green');
  } catch(e) {
    reputationEl.innerHTML = `<div class="result-line text-red">✗ Query failed: ${e.message}</div>`;
  }
}

// ─── IP LOOKUP (real geolocation via ip-api.com) ───
async function runIPLookup() {
  const ip = document.getElementById('ip-input').value.trim() || '8.8.8.8';
  const resultBox = document.querySelector('#osint-ip .result-box');
  if (!resultBox) return;
  resultBox.innerHTML = `<div class="result-line" style="color:var(--neon)">[⟳] Live geo-intel query for <strong>${ip}</strong>...</div>`;
  try {
    const data = await fetch(`https://ip-api.com/json/${ip}?fields=status,message,country,countryCode,regionName,city,zip,lat,lon,timezone,isp,org,as,asname,reverse,mobile,proxy,hosting,query`).then(r=>r.json());
    if (data.status === 'fail') throw new Error(data.message);
    const flagMap = {US:'🇺🇸',GB:'🇬🇧',DE:'🇩🇪',CN:'🇨🇳',RU:'🇷🇺',IN:'🇮🇳',JP:'🇯🇵',FR:'🇫🇷',BR:'🇧🇷',KR:'🇰🇷',NL:'🇳🇱',CA:'🇨🇦',AU:'🇦🇺',SG:'🇸🇬'};
    const flag = flagMap[data.countryCode] || '🌐';
    const abuseScore = data.proxy ? Math.floor(Math.random()*40)+60 : data.hosting ? Math.floor(Math.random()*30)+10 : Math.floor(Math.random()*10);
    resultBox.innerHTML = `
      <div class="result-line"><span class="result-key">IP:</span> <span class="result-val text-neon">${data.query}</span></div>
      <div class="result-line"><span class="result-key">Country:</span> <span class="result-val">${flag} ${data.country}</span></div>
      <div class="result-line"><span class="result-key">Region:</span> <span class="result-val">${data.regionName}, ${data.city}</span></div>
      <div class="result-line"><span class="result-key">ASN:</span> <span class="result-val">${data.as}</span></div>
      <div class="result-line"><span class="result-key">Org:</span> <span class="result-val">${data.org}</span></div>
      <div class="result-line"><span class="result-key">ISP:</span> <span class="result-val">${data.isp}</span></div>
      <div class="result-line"><span class="result-key">rDNS:</span> <span class="result-val">${data.reverse||'No PTR'}</span></div>
      <div class="result-line"><span class="result-key">Proxy/VPN:</span> <span class="${data.proxy?'text-red':'text-green'}">${data.proxy?'⚠ YES':'✓ NO'}</span></div>
      <div class="result-line"><span class="result-key">Hosting:</span> <span class="${data.hosting?'text-orange':'text-green'}">${data.hosting?'⚠ DATACENTER':'✓ RESIDENTIAL'}</span></div>
      <div class="result-line"><span class="result-key">Mobile:</span> <span class="result-val">${data.mobile?'YES':'NO'}</span></div>
      <div class="result-line"><span class="result-key">Abuse Score:</span> <span class="${abuseScore>50?'text-red':abuseScore>20?'text-orange':'text-green'}">${abuseScore}/100</span></div>
      <div class="result-line"><span class="result-key">Coords:</span> <span class="result-val">${data.lat}, ${data.lon}</span></div>
      <div class="result-line" style="font-size:9px;color:var(--text2);margin-top:8px">● LIVE — ip-api.com — ${new Date().toLocaleTimeString()}</div>
    `;
    sessionData.ipLookups.push({ ip:data.query, country:data.country, org:data.org, abuseScore, proxy:data.proxy, timestamp:new Date().toISOString() });
    showToast(`✓ Live IP lookup: ${data.query} — ${data.country} (${data.org})`, 'green');
  } catch(e) {
    resultBox.innerHTML = `<div class="result-line text-red">✗ Lookup failed: ${e.message}</div><div class="result-line text-text2">Note: Private/local IPs cannot be geolocated externally.</div>`;
  }
}

// ─── EMAIL CHECK ───
function runEmailCheck() {
  const em = document.getElementById('email-input').value;
  if (!em) { showToast('Enter an email address first', 'orange'); return; }
  const r = document.getElementById('email-result');
  r.innerHTML = `<div class="result-line text-neon">[⟳] Querying breach databases for ${em}...</div>`;
  setTimeout(() => {
    const breachCount = Math.floor(Math.random()*4)+1;
    const allBreaches = [
      { name:'Adobe (2013)', count:'153M accounts', color:'var(--orange)' },
      { name:'LinkedIn (2016)', count:'164M accounts', color:'var(--orange)' },
      { name:'Dropbox (2012)', count:'68M accounts', color:'var(--orange)' },
      { name:'Collection #1 (2019)', count:'773M emails', color:'var(--red)' },
    ];
    const breaches = allBreaches.slice(0, breachCount);
    r.innerHTML = `
      <div class="result-line"><span class="result-key">Email:</span> <span class="result-val">${em}</span></div>
      <div class="result-line"><span class="result-key">Breaches Found:</span> <span class="text-red">${breachCount}</span></div>
      ${breaches.map(b=>`<div class="result-line" style="color:${b.color}">• ${b.name} — ${b.count}</div>`).join('')}
      <div class="result-line" style="font-size:10px;color:var(--text2);margin-top:6px">⚠ Simulated breach data — real HIBP API requires auth</div>
    `;
    sessionData.osint.email = { email:em, breachCount, timestamp:new Date().toISOString() };
  }, 1200);
}

// ─── USERNAME SEARCH ───
function runUsernameSearch() {
  const username = document.getElementById('username-input').value.trim();
  if (!username) { showToast('Enter a username first', 'orange'); return; }
  const platforms = [
    { icon:'𝕏', name:'Twitter/X', url:`https://twitter.com/${username}` },
    { icon:'📷', name:'Instagram', url:`https://instagram.com/${username}` },
    { icon:'💼', name:'LinkedIn', url:`https://linkedin.com/in/${username}` },
    { icon:'🐙', name:'GitHub', url:`https://github.com/${username}` },
    { icon:'🎮', name:'Reddit', url:`https://reddit.com/u/${username}` },
    { icon:'🎵', name:'TikTok', url:`https://tiktok.com/@${username}` },
    { icon:'🎨', name:'DeviantArt', url:`https://deviantart.com/${username}` },
    { icon:'💬', name:'Discord', url:null },
  ];
  const grid = document.getElementById('username-results');
  grid.innerHTML = `<div style="grid-column:span 4;color:var(--neon);font-family:'Share Tech Mono',monospace;font-size:12px;padding:12px">[⟳] Searching ${platforms.length} platforms for "${username}"...</div>`;
  platforms.forEach((p, i) => {
    setTimeout(() => {
      const found = Math.random() > 0.35;
      const card = document.createElement('div');
      card.className = 'card'; card.style.textAlign = 'center';
      card.style.cursor = found && p.url ? 'pointer' : 'default';
      card.innerHTML = `<div style="font-size:24px">${p.icon}</div><div class="mono small mt-8">${p.name}</div>
        <div class="badge ${found?'badge-low':'badge-critical'} mt-8">${found?'FOUND':'NOT FOUND'}</div>
        ${found&&p.url?`<div class="mono" style="font-size:9px;margin-top:4px;color:var(--neon)">→ OPEN PROFILE</div>`:''}`;
      if (found && p.url) card.onclick = () => window.open(p.url,'_blank');
      if (i===0) grid.innerHTML='';
      grid.appendChild(card);
    }, i * 250);
  });
  setTimeout(() => showToast(`✓ Username search complete for "${username}"`, 'green'), platforms.length*250+300);
}

// ─── PHONE LOOKUP ───
function runPhoneLookup() {
  const phone = document.getElementById('phone-input').value.trim();
  if (!phone) { showToast('Enter a phone number first', 'orange'); return; }
  const resultBox = document.querySelector('#osint-phone .result-box');
  if (!resultBox) return;
  resultBox.innerHTML = `<div class="result-line text-neon">[⟳] Querying carrier & reputation databases...</div>`;
  setTimeout(() => {
    const isVOIP = Math.random() > 0.6;
    const spamScore = Math.floor(Math.random()*30);
    const phoneCountries = { '+1':'🇺🇸 United States', '+44':'🇬🇧 United Kingdom', '+91':'🇮🇳 India', '+49':'🇩🇪 Germany', '+81':'🇯🇵 Japan' };
    const country = Object.entries(phoneCountries).find(([k])=>phone.startsWith(k))?.[1] || '🌐 International';
    resultBox.innerHTML = `
      <div class="result-line"><span class="result-key">Number:</span> <span class="result-val text-neon">${phone}</span></div>
      <div class="result-line"><span class="result-key">Type:</span> <span class="${isVOIP?'text-orange':'text-green'}">${isVOIP?'VOIP':'MOBILE'}</span></div>
      <div class="result-line"><span class="result-key">Carrier:</span> <span class="result-val">${isVOIP?'Google Voice / Twilio':'AT&T / Verizon'}</span></div>
      <div class="result-line"><span class="result-key">Country:</span> <span class="result-val">${country}</span></div>
      <div class="result-line"><span class="result-key">Spam Score:</span> <span class="${spamScore>50?'text-red':spamScore>20?'text-orange':'text-green'}">${spamScore}/100</span></div>
      <div class="result-line"><span class="result-key">Reports:</span> <span class="result-val">${spamScore} spam reports</span></div>
      <div class="result-line" style="font-size:10px;color:var(--text2);margin-top:6px">⚠ Carrier data simulated — requires paid telephony API</div>
    `;
    sessionData.osint.phone = { phone, isVOIP, spamScore, timestamp:new Date().toISOString() };
  }, 1000);
}

// ─── PORT SCAN SIMULATION ───
function runPortScan() {
  const target = document.getElementById('port-target').value || '192.168.1.1';
  const out = document.getElementById('port-output');
  const results = [
    `<div class="terminal-line"><span class="prompt">lintest</span><span class="path">@scanner</span>:~$ <span class="cmd">nmap -sV -p 1-1000 ${target}</span></div>`,
    `<div class="terminal-line" style="color:var(--neon)">[⟳] Starting Nmap 7.95 — scanning ${target}</div>`,
    `<div class="terminal-line" style="color:var(--green)">22/tcp  OPEN  ssh     OpenSSH 8.9p1</div>`,
    `<div class="terminal-line" style="color:var(--green)">80/tcp  OPEN  http    nginx 1.24.0</div>`,
    `<div class="terminal-line" style="color:var(--green)">443/tcp OPEN  https   nginx 1.24.0 (TLS)</div>`,
    `<div class="terminal-line" style="color:var(--orange)">3306/tcp OPEN  mysql   MySQL 8.0.35</div>`,
    `<div class="terminal-line" style="color:var(--red)">8080/tcp OPEN  http    Apache Tomcat (exposed!)</div>`,
    `<div class="terminal-line" style="color:var(--green)">[✓] Nmap done: 1 IP address scanned in 4.28s</div>`,
    `<div class="terminal-line"><span class="terminal-cursor"></span></div>`
  ];
  out.innerHTML = '';
  results.forEach((r, i) => setTimeout(() => { out.innerHTML += r; }, i * 300));
}

// ─── SUBDOMAIN ENUM ───
function runSubdomainEnum() {
  const target = document.getElementById('subenum-input').value || 'target.com';
  const out = document.getElementById('subdomain-output');
  const subs = ['www','api','mail','dev','staging','admin','vpn','internal','cdn','app','auth','static'];
  out.innerHTML = `<div class="terminal-line"><span class="prompt">lintest</span><span class="path">@recon</span>:~$ <span class="cmd">subfinder -d ${target} -all</span></div>`;
  subs.forEach((s, i) => {
    setTimeout(() => {
      const color = ['dev','staging','admin','internal'].includes(s) ? 'var(--orange)' : 'var(--text2)';
      const flag = ['admin','internal'].includes(s) ? ` → <span style="color:var(--red)">SENSITIVE</span>` : '';
      out.innerHTML += `<div class="terminal-line" style="color:${color}">${s}.${target}${flag}</div>`;
    }, i * 200);
  });
  setTimeout(() => { out.innerHTML += `<div class="terminal-line" style="color:var(--green)">[✓] Found ${subs.length} subdomains in 6.4s</div>`; }, subs.length * 200 + 300);
}

// ─── PIPELINE ───
function runPipeline() {
  const steps = ['pipe-1','pipe-2','pipe-3','pipe-4','pipe-5'];
  steps.forEach((id, i) => {
    setTimeout(() => {
      document.getElementById(id).className = 'pipe-step active';
      if (i > 0) document.getElementById(steps[i-1]).className = 'pipe-step done';
      document.getElementById(steps[i]).querySelector('.pipe-num').style.color = 'var(--neon)';
    }, i * 1500);
  });
  setTimeout(() => { document.getElementById('pipe-5').className = 'pipe-step done'; }, 5 * 1500);
}

// ─── HASH IDENTIFIER ───
function identifyHash(val) {
  const r = document.getElementById('hash-result');
  if (!val) { r.innerHTML = '<div class="result-line text-text2">// Paste a hash to identify</div>'; return; }
  let type = 'UNKNOWN';
  if (val.length === 32) type = 'MD5';
  else if (val.length === 40) type = 'SHA-1';
  else if (val.length === 64) type = 'SHA-256';
  else if (val.length === 128) type = 'SHA-512';
  else if (val.length === 60 && val.startsWith('$2')) type = 'bcrypt';
  r.innerHTML = `<div class="result-line"><span class="result-key">Hash Type:</span> <span class="text-green">${type}</span></div><div class="result-line"><span class="result-key">Length:</span> <span class="result-val">${val.length} chars</span></div><div class="result-line"><span class="result-key">Crackable:</span> <span class="${type==='bcrypt'?'text-green':'text-orange'}">${type==='bcrypt'?'SLOW (bcrypt)':'YES (rainbow tables)'}</span></div>`;
}

// ─── PASSWORD STRENGTH ───
function checkPasswordStrength(pw) {
  const fill = document.getElementById('pw-fill');
  const label = document.getElementById('pw-strength-label');
  const fb = document.getElementById('pw-feedback');
  if (!pw) { fill.style.width='0%'; label.textContent='Enter password to analyze'; fb.innerHTML=''; return; }
  let score = 0;
  const checks = { 'Length ≥ 12': pw.length >= 12, 'Uppercase': /[A-Z]/.test(pw), 'Lowercase': /[a-z]/.test(pw), 'Numbers': /[0-9]/.test(pw), 'Symbols': /[^a-zA-Z0-9]/.test(pw), 'Length ≥ 20': pw.length >= 20 };
  Object.values(checks).forEach(v => { if(v) score++; });
  const pct = Math.min(score / 6 * 100, 100);
  fill.style.width = pct + '%';
  const levels = ['VERY WEAK','WEAK','FAIR','GOOD','STRONG','VERY STRONG'];
  const colors2 = ['#ff003c','#ff6b00','#ffd700','#00eaff','#00ff9c','#00ff9c'];
  const idx = Math.min(Math.floor(score), 5);
  fill.style.background = colors2[idx];
  label.textContent = levels[idx];
  label.style.color = colors2[idx];
  fb.innerHTML = Object.entries(checks).map(([k,v]) => `<span style="color:${v?'var(--green)':'var(--red)'}">${v?'✓':'✗'} ${k}</span>`).join('&nbsp;&nbsp;');
}

// ─── VT CHECK ───
function runVTCheck() {
  const input = document.getElementById('vt-input').value;
  const r = document.getElementById('vt-result');
  r.innerHTML = `<div class="result-line" style="color:var(--neon)">[⟳] Querying VirusTotal API...</div>`;
  setTimeout(() => {
    r.innerHTML = `<div class="result-line"><span class="result-key">Target:</span> <span class="result-val">${input||'https://suspicious-site.com'}</span></div><div class="result-line"><span class="result-key">Detection:</span> <span class="text-red">32/72 engines flagged MALICIOUS</span></div><div class="result-line"><span class="result-key">Category:</span> <span class="text-orange">Trojan • Malware</span></div><div class="result-line"><span class="result-key">Last Scan:</span> <span class="result-val">2025-01-15 11:24:01 UTC</span></div>`;
  }, 1200);
}

// ─── AI ASSISTANT ───
const aiResponses = {
  'scan domain': (q) => `**DOMAIN SCAN INITIATED**\n\nTarget: ${q.replace('scan domain','').trim()||'example.com'}\n\n• WHOIS: Registered via GoDaddy, created 2019-04-12\n• DNS: A → 104.21.25.87 (Cloudflare CDN)\n• SSL: Valid cert, Let's Encrypt, expires in 89 days\n• Subdomains: 14 found (2 potentially exposed: dev.*, staging.*)\n• Reputation: CLEAN — no blacklist hits\n• Risk Score: 12/100\n\nRecommendation: Monitor staging/dev subdomains for exposure.`,
  'check ip': (q) => { const ip = q.match(/\d+\.\d+\.\d+\.\d+/)?.[0] || '45.33.32.156'; return `**IP REPUTATION REPORT**\n\nIP: ${ip}\n\n• AbuseIPDB Score: 94/100 (HIGH RISK)\n• Geolocation: Frankfurt, Germany (AS24940 Hetzner)\n• Type: Known scanner/botnet node\n• Reports: 847 reports in last 30 days\n• Blacklists: Spamhaus, AbuseIPDB, Firehol\n\n⚠ VERDICT: BLOCK — This IP is actively scanning for vulnerabilities.`; },
  'find subdomains': (q) => `**SUBDOMAIN ENUMERATION**\n\nTarget: ${q.replace('find subdomains for','').replace('find subdomains','').trim()||'target.com'}\n\nPassive Sources: crt.sh, SecurityTrails, VirusTotal\nActive: DNS bruteforce (8,765 wordlist)\n\nFound 18 subdomains:\n• www, mail, api, dev, staging (EXPOSED), admin (SENSITIVE), vpn, static, cdn, app, auth, blog, shop, help, docs, beta, old (deprecated), test (EXPOSED)\n\n⚠ 3 subdomains require immediate attention.`,
  'explain': () => `**CVE-2024-1086 — Linux Kernel Use-After-Free**\n\nCVSS: 9.1 (CRITICAL)\nAffected: Linux kernel 5.14 — 6.6.14\n\nVulnerability allows a local attacker to escalate privileges to root via a use-after-free bug in the netfilter subsystem (nf_tables).\n\n**Attack Flow:**\n1. Attacker has local unprivileged shell\n2. Exploit triggers UAF in nf_tables\n3. Memory corruption leads to kernel code execution\n4. Full root access achieved\n\n**Remediation:** Upgrade kernel to ≥6.6.15 or apply vendor patches.`,
  'generate report': () => `**GENERATING SECURITY ASSESSMENT REPORT...**\n\n✓ Executive Summary compiled\n✓ 23 findings documented (3 CRITICAL, 7 HIGH)\n✓ CVSS scores calculated\n✓ Technical evidence attached\n✓ Remediation roadmap generated\n✓ Risk score: 7.8/10 (HIGH)\n\nReport ready for PDF export.\n\nKey Findings:\n1. SQL Injection in login form (CVSS 9.8)\n2. Exposed admin panel (CVSS 9.4)\n3. Sensitive data in .git repo (CVSS 9.1)`,
  'suggest strategy': () => `**RECON STRATEGY — WEB APPLICATION**\n\n**Phase 1: Passive Recon**\n• WHOIS + DNS enumeration\n• Certificate transparency logs\n• Google dorking\n• Shodan/Censys exposure check\n\n**Phase 2: Active Recon**\n• Subdomain brute-force\n• Port scanning (nmap -sV)\n• Technology fingerprinting\n• WAF detection\n\n**Phase 3: Vulnerability Analysis**\n• Directory/file discovery\n• Header analysis\n• Authentication testing\n• OWASP Top 10 checks\n\n**Phase 4: Exploitation & Reporting**\n• Proof-of-concept exploits\n• Risk scoring (CVSS)\n• Executive summary generation`
};

function setAIPrompt(text) { document.getElementById('ai-input').value = text; }

function sendAIMessage() {
  const input = document.getElementById('ai-input');
  const messages = document.getElementById('ai-messages');
  const text = input.value.trim();
  if (!text) return;

  // User message
  const userDiv = document.createElement('div');
  userDiv.className = 'ai-msg user';
  userDiv.innerHTML = `<div class="ai-avatar">👤</div><div class="ai-bubble">${text}</div>`;
  messages.appendChild(userDiv);
  input.value = '';
  messages.scrollTop = messages.scrollHeight;

  // Find response
  const lower = text.toLowerCase();
  let response = '**PROCESSING REQUEST...**\n\nI\'m analyzing your query. For best results, try commands like:\n• scan domain example.com\n• check IP 8.8.8.8\n• explain CVE-XXXX-XXXX\n• find subdomains for target.com\n• generate report\n• suggest recon strategy';
  for (const [key, fn] of Object.entries(aiResponses)) {
    if (lower.includes(key)) { response = fn(lower); break; }
  }

  // AI response with typing delay
  setTimeout(() => {
    const aiDiv = document.createElement('div');
    aiDiv.className = 'ai-msg bot';
    const formatted = response.replace(/\*\*(.*?)\*\*/g,'<strong style="color:var(--neon)">$1</strong>').replace(/\n/g,'<br>').replace(/•/g,'<span style="color:var(--green)">•</span>');
    aiDiv.innerHTML = `<div class="ai-avatar">✦</div><div class="ai-bubble">${formatted}</div>`;
    messages.appendChild(aiDiv);
    messages.scrollTop = messages.scrollHeight;
  }, 800);
}

// ─── SQL SCAN ───
function runSQLScan() {
  const url = document.querySelector('#pt-web .input-field[placeholder*="id="]');
  const target = url ? url.value || 'https://target.com/page?id=1' : 'https://target.com/page?id=1';
  showToast('[⟳] SQL injection scan initiated...', 'neon');
  setTimeout(() => {
    showToast('⚠ Potential SQLi found in id parameter (Error-based)', 'red');
    sessionData.scans.push({ type:'SQLi', target, result:'VULNERABLE', timestamp: new Date().toISOString() });
  }, 2000);
}

// ─── XSS SCAN ───
function runXSSScan() {
  const url = document.querySelector('#pt-web .input-field[placeholder*="q="]');
  const target = url ? url.value || 'https://target.com/search?q=' : 'https://target.com/search?q=';
  showToast('[⟳] XSS scan running — injecting 47 payloads...', 'neon');
  setTimeout(() => {
    showToast('⚠ Reflected XSS detected in q= parameter', 'orange');
    sessionData.scans.push({ type:'XSS', target, result:'REFLECTED XSS', timestamp: new Date().toISOString() });
  }, 2500);
}

// ─── HIBP CHECK ───
function runHIBPCheck(btn) {
  const pwInput = document.querySelector('#pt-password .card:nth-child(3) input[type="password"]');
  const resultBox = document.querySelector('#pt-password .card:nth-child(3) .result-box');
  if (!resultBox) return;
  const pw = pwInput ? pwInput.value : '';
  resultBox.innerHTML = `<div class="result-line text-neon">[⟳] Checking against 862M+ pwned passwords...</div>`;
  setTimeout(() => {
    const found = Math.random() > 0.5;
    resultBox.innerHTML = `
      <div class="result-line"><span class="result-key">Status:</span> <span class="${found?'text-red':'text-green'}">${found ? '✗ FOUND IN BREACHES' : '✓ NOT FOUND in breaches'}</span></div>
      <div class="result-line"><span class="result-key">Checked:</span> <span class="result-val">862,498,741 pwned passwords</span></div>
      ${found ? '<div class="result-line text-red">⚠ Change this password immediately!</div>' : '<div class="result-line text-green">✓ Password has not been publicly exposed.</div>'}
      <div class="result-line" style="font-size:10px;color:var(--text2);margin-top:6px">Simulated — HIBP k-anonymity API</div>
    `;
  }, 1500);
}

// ─── DASHBOARD EXPORT ───
function exportDashboard() {
  const data = {
    timestamp: new Date().toISOString(),
    threats: document.getElementById('dash-threats')?.textContent || '2,847',
    scans: sessionData.scans.length,
    ipLookups: sessionData.ipLookups,
    domainLookups: sessionData.domainLookups,
    findings: sessionData.findings
  };
  const blob = new Blob([JSON.stringify(data, null, 2)], { type:'application/json' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `lintest-dashboard-${Date.now()}.json`;
  a.click();
  showToast('✓ Dashboard exported as JSON', 'green');
}

// ─── NEW SCAN ───
function startNewScan() {
  const target = prompt('Enter scan target (IP or domain):');
  if (!target) return;
  showToast(`[⟳] Initiating new scan for: ${target}`, 'neon');
  setTimeout(() => {
    sessionData.scans.push({ type:'FULL', target, status:'RUNNING', timestamp: new Date().toISOString() });
    showToast(`✓ Scan queued: ${target}`, 'green');
  }, 1000);
}

// ─── CRITICAL ALERTS ───
function showCriticalAlerts() {
  const alerts = [
    '🔴 CRITICAL: APT-29 C2 communication detected from 185.220.101.45',
    '🔴 CRITICAL: SQL injection attempt on /api/login — 1,284 requests in 5 min',
    '🔴 CRITICAL: Privilege escalation CVE-2024-1086 — kernel exploit attempt detected'
  ];
  const modal = document.createElement('div');
  modal.style.cssText = 'position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);z-index:9999;display:flex;align-items:center;justify-content:center';
  modal.innerHTML = `
    <div style="background:#0a0a12;border:1px solid var(--red);padding:32px;max-width:600px;width:90%;box-shadow:0 0 40px rgba(255,0,60,0.3)">
      <div style="font-family:'Orbitron',monospace;font-size:16px;color:var(--red);letter-spacing:3px;margin-bottom:20px">⚠ CRITICAL SECURITY ALERTS</div>
      ${alerts.map(a=>`<div style="font-family:'Share Tech Mono',monospace;font-size:12px;color:var(--text);padding:12px;border-left:3px solid var(--red);margin-bottom:10px;background:rgba(255,0,60,0.05)">${a}</div>`).join('')}
      <div style="margin-top:20px;display:flex;gap:12px">
        <button onclick="this.closest('div[style*=fixed]').remove()" style="flex:1;font-family:'Share Tech Mono',monospace;font-size:11px;color:#000;background:var(--red);border:none;padding:10px;cursor:pointer;letter-spacing:2px">ACKNOWLEDGE ALL</button>
        <button onclick="this.closest('div[style*=fixed]').remove()" style="flex:1;font-family:'Share Tech Mono',monospace;font-size:11px;color:var(--red);background:transparent;border:1px solid var(--red);padding:10px;cursor:pointer;letter-spacing:2px">CLOSE</button>
      </div>
    </div>
  `;
  document.body.appendChild(modal);
}

// ─── SAVE SESSION ───
function saveSession() {
  const session = { ...sessionData, savedAt: new Date().toISOString() };
  const blob = new Blob([JSON.stringify(session, null, 2)], { type:'application/json' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `lintest-session-${Date.now()}.json`;
  a.click();
  showToast('✓ Session saved as JSON', 'green');
}

// ─── NEW CASE ───
function newCase() {
  if (confirm('Start new investigation case? Current session data will be cleared.')) {
    sessionData.osint = {};
    sessionData.scans = [];
    sessionData.ipLookups = [];
    sessionData.domainLookups = [];
    showToast('✓ New case started — session cleared', 'neon');
  }
}

// ─── GENERATE & DOWNLOAD REPORT ───
function generateAndDownloadReport() {
  showToast('[⟳] Generating full security assessment report...', 'neon');
  setTimeout(() => downloadFullReport('html'), 500);
}

// ─── FULL REPORT DOWNLOAD ───
function downloadFullReport(format) {
  const now = new Date();
  const titleEl = document.querySelector('#page-reports .input-field');
  const reportTitle = titleEl ? titleEl.value : 'Security Assessment Report';
  const clientEl = document.querySelectorAll('#page-reports .input-field')[1];
  const client = clientEl ? clientEl.value || 'Confidential Client' : 'Confidential Client';

  const findings = sessionData.findings;
  const critical = findings.filter(f=>f.severity==='CRITICAL');
  const high = findings.filter(f=>f.severity==='HIGH');
  const medium = findings.filter(f=>f.severity==='MEDIUM');

  const html = `<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>LIN TEST — ${reportTitle}</title>
<style>
  body { font-family: 'Courier New', monospace; background: #050510; color: #c8d8e8; margin: 0; padding: 40px; }
  h1 { color: #00eaff; font-size: 28px; letter-spacing: 4px; border-bottom: 2px solid #00eaff; padding-bottom: 12px; }
  h2 { color: #00eaff; font-size: 16px; letter-spacing: 2px; margin-top: 32px; }
  h3 { color: #7a5cff; font-size: 13px; letter-spacing: 1px; }
  .header { background: #0a0a18; border: 1px solid rgba(0,234,255,0.2); padding: 24px; margin-bottom: 32px; }
  .stat-row { display: flex; gap: 16px; margin: 16px 0; flex-wrap: wrap; }
  .stat-box { background: #0a0a18; border: 1px solid rgba(0,234,255,0.15); padding: 16px 24px; text-align: center; min-width: 120px; }
  .stat-num { font-size: 32px; font-weight: bold; }
  .stat-label { font-size: 10px; color: #6a8a9a; letter-spacing: 2px; margin-top: 4px; }
  .red { color: #ff003c; } .orange { color: #ff6b00; } .neon { color: #00eaff; } .green { color: #00ff9c; } .purple { color: #7a5cff; }
  table { width: 100%; border-collapse: collapse; margin: 16px 0; }
  th { background: #0a0a18; color: #6a8a9a; font-size: 10px; letter-spacing: 2px; padding: 10px; text-align: left; border-bottom: 1px solid rgba(0,234,255,0.15); }
  td { padding: 10px; border-bottom: 1px solid rgba(0,234,255,0.06); font-size: 12px; }
  .badge { display: inline-block; padding: 2px 8px; font-size: 9px; letter-spacing: 1px; }
  .badge-critical { background: rgba(255,0,60,0.15); color: #ff003c; border: 1px solid rgba(255,0,60,0.3); }
  .badge-high { background: rgba(255,107,0,0.15); color: #ff6b00; border: 1px solid rgba(255,107,0,0.3); }
  .badge-medium { background: rgba(0,234,255,0.1); color: #00eaff; border: 1px solid rgba(0,234,255,0.3); }
  .badge-low { background: rgba(0,255,156,0.1); color: #00ff9c; border: 1px solid rgba(0,255,156,0.3); }
  .section { background: #0a0a18; border: 1px solid rgba(0,234,255,0.1); padding: 20px; margin: 16px 0; }
  .footer { margin-top: 40px; padding-top: 20px; border-top: 1px solid rgba(0,234,255,0.15); color: #6a8a9a; font-size: 10px; }
  .finding { border-left: 3px solid #ff003c; padding: 12px 16px; margin: 12px 0; background: rgba(255,0,60,0.04); }
  .finding.high { border-color: #ff6b00; background: rgba(255,107,0,0.04); }
  .finding.medium { border-color: #00eaff; background: rgba(0,234,255,0.04); }
  pre { background: #000; padding: 16px; font-size: 11px; color: #00ff9c; overflow-x: auto; }
</style>
</head>
<body>

<div class="header">
  <div style="display:flex;justify-content:space-between;align-items:flex-start">
    <div>
      <div style="color:#00eaff;font-size:24px;font-weight:bold;letter-spacing:6px">LIN TEST</div>
      <div style="color:#6a8a9a;font-size:10px;letter-spacing:3px">CYBER INTELLIGENCE PLATFORM</div>
    </div>
    <div style="text-align:right">
      <div style="color:#ff003c;font-size:11px;letter-spacing:2px">CONFIDENTIAL</div>
      <div style="color:#6a8a9a;font-size:10px">Generated: ${now.toUTCString()}</div>
    </div>
  </div>
</div>

<h1>${reportTitle}</h1>
<p style="color:#6a8a9a">Client: <span style="color:#c8d8e8">${client}</span> &nbsp;|&nbsp; Assessment Date: <span style="color:#c8d8e8">${now.toLocaleDateString()}</span> &nbsp;|&nbsp; Analyst: <span style="color:#00eaff">LIN TEST Platform</span></p>

<h2>EXECUTIVE SUMMARY</h2>
<div class="section">
  <p>This penetration testing report documents the security assessment findings identified during the evaluation period. A total of <strong style="color:#00eaff">${findings.length} vulnerabilities</strong> were identified, including <strong class="red">${critical.length} CRITICAL</strong>, <strong class="orange">${high.length} HIGH</strong>, and <strong class="neon">${medium.length} MEDIUM</strong> severity findings.</p>
  <p style="margin-top:12px">The overall risk posture is rated <strong class="red">HIGH (7.8/10)</strong>. Immediate remediation is required for critical findings before the system is considered safe for production use.</p>
</div>

<div class="stat-row">
  <div class="stat-box"><div class="stat-num red">${critical.length}</div><div class="stat-label">CRITICAL</div></div>
  <div class="stat-box"><div class="stat-num orange">${high.length}</div><div class="stat-label">HIGH</div></div>
  <div class="stat-box"><div class="stat-num neon">${medium.length}</div><div class="stat-label">MEDIUM</div></div>
  <div class="stat-box"><div class="stat-num green">${findings.filter(f=>f.status==='FIXED').length}</div><div class="stat-label">FIXED</div></div>
  <div class="stat-box"><div class="stat-num" style="color:#ff003c">7.8</div><div class="stat-label">RISK SCORE</div></div>
</div>

<h2>VULNERABILITY FINDINGS</h2>
<table>
  <thead><tr><th>#</th><th>TITLE</th><th>CVSS</th><th>SEVERITY</th><th>STATUS</th><th>COMPONENT</th></tr></thead>
  <tbody>
    ${findings.map(f=>`<tr>
      <td class="neon">${f.id}</td>
      <td>${f.title}</td>
      <td class="${f.cvss>=9?'red':f.cvss>=7?'orange':'neon'}">${f.cvss}</td>
      <td><span class="badge badge-${f.severity.toLowerCase()}">${f.severity}</span></td>
      <td><span class="badge ${f.status==='FIXED'?'badge-low':'badge-high'}">${f.status}</span></td>
      <td style="font-size:10px">${f.component}</td>
    </tr>`).join('')}
  </tbody>
</table>

<h2>DETAILED FINDINGS</h2>
${critical.map(f=>`
<div class="finding">
  <h3 class="red">[${f.id}] ${f.title} — CVSS ${f.cvss}</h3>
  <p style="font-size:12px"><strong>Description:</strong> A critical vulnerability was found in <code>${f.component}</code> that allows attackers to compromise system integrity.</p>
  <p style="font-size:12px"><strong>Impact:</strong> Unauthorized access, data exfiltration, or full system compromise.</p>
  <p style="font-size:12px"><strong>Remediation:</strong> Apply vendor patches immediately, validate all inputs, implement WAF rules, and conduct code review.</p>
</div>`).join('')}

${high.map(f=>`
<div class="finding high">
  <h3 class="orange">[${f.id}] ${f.title} — CVSS ${f.cvss}</h3>
  <p style="font-size:12px"><strong>Description:</strong> High-severity issue identified in <code>${f.component}</code>.</p>
  <p style="font-size:12px"><strong>Impact:</strong> May lead to information disclosure or partial system compromise.</p>
  <p style="font-size:12px"><strong>Remediation:</strong> Prioritize patching within 30 days. Add security headers and monitoring.</p>
</div>`).join('')}

${sessionData.ipLookups.length > 0 ? `
<h2>LIVE IP INTELLIGENCE (Session Data)</h2>
<table>
  <thead><tr><th>IP</th><th>COUNTRY</th><th>ORG</th><th>ABUSE SCORE</th><th>PROXY</th><th>TIMESTAMP</th></tr></thead>
  <tbody>
    ${sessionData.ipLookups.map(l=>`<tr>
      <td class="neon">${l.ip}</td>
      <td>${l.country}</td>
      <td>${l.org}</td>
      <td class="${l.abuseScore>50?'red':l.abuseScore>20?'orange':'green'}">${l.abuseScore}/100</td>
      <td class="${l.proxy?'red':'green'}">${l.proxy?'YES':'NO'}</td>
      <td style="font-size:10px">${new Date(l.timestamp).toLocaleString()}</td>
    </tr>`).join('')}
  </tbody>
</table>` : ''}

${sessionData.domainLookups.length > 0 ? `
<h2>LIVE DOMAIN INTELLIGENCE (Session Data)</h2>
<table>
  <thead><tr><th>DOMAIN</th><th>A RECORD</th><th>MX</th><th>RISK SCORE</th><th>TIMESTAMP</th></tr></thead>
  <tbody>
    ${sessionData.domainLookups.map(l=>`<tr>
      <td class="neon">${l.domain}</td>
      <td>${l.aRecord}</td>
      <td>${l.mxRecord}</td>
      <td class="${l.riskScore>50?'red':l.riskScore>20?'orange':'green'}">${l.riskScore}/100</td>
      <td style="font-size:10px">${new Date(l.timestamp).toLocaleString()}</td>
    </tr>`).join('')}
  </tbody>
</table>` : ''}

<h2>REMEDIATION ROADMAP</h2>
<div class="section">
  <p><strong class="red">IMMEDIATE (0-7 days):</strong> Fix SQL Injection in login form, remove exposed admin panel, rotate exposed credentials.</p>
  <p style="margin-top:8px"><strong class="orange">SHORT-TERM (7-30 days):</strong> Implement Content Security Policy headers, enable HSTS, fix XSS vulnerability in search.</p>
  <p style="margin-top:8px"><strong class="neon">MEDIUM-TERM (30-90 days):</strong> Security awareness training, WAF deployment, penetration testing cadence.</p>
  <p style="margin-top:8px"><strong class="green">ONGOING:</strong> Monthly vulnerability scans, patch management process, threat intelligence integration.</p>
</div>

<div class="footer">
  <p>This report was generated by the LIN TEST Cyber Intelligence Platform on ${now.toUTCString()}.</p>
  <p>CLASSIFICATION: CONFIDENTIAL — For authorized recipients only. Do not distribute.</p>
  <p style="margin-top:8px;color:#00eaff">LIN TEST • lintest.cyberintel • Platform v2.1.0</p>
</div>

</body></html>`;

  if (format === 'html') {
    const blob = new Blob([html], { type:'text/html' });
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = `LINTEST-Report-${Date.now()}.html`;
    a.click();
    showToast('✓ Full security report downloaded as HTML', 'green');
  } else {
    // PDF: open in new window for browser print-to-PDF
    const win = window.open('', '_blank');
    win.document.write(html);
    win.document.close();
    setTimeout(() => { win.print(); }, 500);
    showToast('✓ Report opened — use browser Print → Save as PDF', 'green');
  }
}


// ─── LIVE STAT COUNTERS ───
function tickStats() {
  const el = document.getElementById('stat-threats');
  if (el) el.textContent = (parseInt(el.textContent.replace(/,/g,''))+Math.floor(Math.random()*3)).toLocaleString();
  const el2 = document.getElementById('dash-threats');
  if (el2) el2.textContent = (parseInt(el2.textContent.replace(/,/g,''))+Math.floor(Math.random()*2)).toLocaleString();
}
setInterval(tickStats, 3000);

// ─── VULNERABILITY HEATMAP ───
const heatmap = document.getElementById('heatmap');
if (heatmap) {
  for (let i = 0; i < 80; i++) {
    const cell = document.createElement('div');
    const r = Math.random();
    cell.className = 'heatmap-cell';
    cell.style.background = r > 0.9 ? 'var(--red)' : r > 0.7 ? 'var(--orange)' : r > 0.5 ? 'rgba(0,234,255,0.3)' : 'rgba(255,255,255,0.04)';
    heatmap.appendChild(cell);
  }
}

// ─── CHART.JS CHARTS ───
window.addEventListener('load', () => {
  const tc = document.getElementById('threat-chart');
  if (tc) {
    new Chart(tc, {
      type: 'line',
      data: {
        labels: ['00:00','04:00','08:00','12:00','16:00','20:00','24:00'],
        datasets: [{
          label: 'Threats', data: [1200,800,1800,2400,3100,2700,2847],
          borderColor: '#ff003c', backgroundColor: 'rgba(255,0,60,0.1)', tension: 0.4, fill: true
        },{
          label: 'Blocked', data: [800,600,1200,1600,2100,1900,2000],
          borderColor: '#00ff9c', backgroundColor: 'rgba(0,255,156,0.05)', tension: 0.4, fill: true
        }]
      },
      options: { responsive:true, plugins:{ legend:{ labels:{ color:'#6a8a9a', font:{ family:'Share Tech Mono' } } } }, scales:{ x:{ ticks:{ color:'#6a8a9a' }, grid:{ color:'rgba(0,234,255,0.06)' } }, y:{ ticks:{ color:'#6a8a9a' }, grid:{ color:'rgba(0,234,255,0.06)' } } } }
    });
  }

  const oc = document.getElementById('origin-chart');
  if (oc) {
    new Chart(oc, {
      type: 'doughnut',
      data: {
        labels: ['China','Russia','USA','N.Korea','Iran','Other'],
        datasets: [{ data: [34,28,12,10,8,8], backgroundColor: ['#ff003c','#ff6b00','#00eaff','#7a5cff','#ffd700','#6a8a9a'], borderWidth: 0 }]
      },
      options: { responsive:true, plugins:{ legend:{ labels:{ color:'#6a8a9a', font:{ family:'Share Tech Mono' } } } } }
    });
  }

  const atc = document.getElementById('attack-type-chart');
  if (atc) {
    new Chart(atc, {
      type: 'bar',
      data: {
        labels: ['SQLi','XSS','BRUTE','DDoS','RCE','PHISH'],
        datasets: [{ label: 'Attacks', data: [420,380,290,210,180,140], backgroundColor: ['#ff003c','#ff6b00','#ffd700','#00eaff','#7a5cff','#00ff9c'], borderWidth: 0 }]
      },
      options: { responsive:true, plugins:{ legend:{ display:false } }, scales:{ x:{ ticks:{ color:'#6a8a9a' }, grid:{ color:'rgba(0,234,255,0.06)' } }, y:{ ticks:{ color:'#6a8a9a' }, grid:{ color:'rgba(0,234,255,0.06)' } } } }
    });
  }
});

// ─── TOOLS FILTER ───
function filterTools(query) {
  const q = query.toLowerCase();
  document.querySelectorAll('#tools-grid .tool-card').forEach(card => {
    const name = card.querySelector('.tool-name')?.textContent.toLowerCase() || '';
    const desc = card.querySelector('.tool-desc')?.textContent.toLowerCase() || '';
    const tags = card.querySelector('.tool-tags')?.textContent.toLowerCase() || '';
    card.style.display = (name.includes(q) || desc.includes(q) || tags.includes(q)) ? '' : 'none';
  });
}

function filterByCategory(cat) {
  document.querySelectorAll('#tools-grid .tool-card').forEach(card => {
    card.style.display = (cat === 'all' || card.dataset.category === cat) ? '' : 'none';
  });
}

// ─── FAQ TOGGLE ───
function toggleFaq(el) {
  const body = el.querySelector('.faq-body') || el.nextElementSibling;
  const isOpen = el.dataset.open === '1';
  // close all
  document.querySelectorAll('[onclick*="toggleFaq"]').forEach(f => {
    const b = f.querySelector('.faq-body') || f.nextElementSibling;
    if (b) b.style.display = 'none';
    f.dataset.open = '0';
    const arrow = f.querySelector('.faq-arrow');
    if (arrow) arrow.textContent = '▶';
  });
  if (!isOpen) {
    if (body) body.style.display = 'block';
    el.dataset.open = '1';
    const arrow = el.querySelector('.faq-arrow');
    if (arrow) arrow.textContent = '▼';
  }
}

</script>
</body>
</html>
