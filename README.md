# Shipment Tracking Application

A fully functional FedEx-style real-time shipment tracking interface with live map animations, dark mode, and realistic tracking updates.

## Features

- 🌍 Live GPS tracking with animated route visualization
- 📦 Real-time shipment status updates
- 🗺️ Interactive SVG world map
- 🌓 Dark/Light mode toggle
- 🔔 Sound alerts for tracking updates
- 📊 Shipment progress tracking
- 🌐 Multi-language support (EN/TR)
- 📱 Fully responsive design

## Demo Shipments

- **FX-7748293-TR**: Turkey 🇹🇷 to Germany 🇩🇪
- **FX-3300125-AS**: China 🇨🇳 to Turkey 🇹🇷
- **FX-4829301-US**: USA 🇺🇸 to Germany 🇩🇪
- **FX-9912847-EU**: Germany 🇩🇪 to UK 🇬🇧

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FedEx — Track Your Shipment</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<style>
  :root {
    --fedex-purple: #4D148C;
    --fedex-orange: #FF6600;
    --fedex-purple-dark: #3a0f6b;
    --fedex-purple-light: #6a2db8;
    --bg: #f4f4f6;
    --card: #ffffff;
    --text: #111120;
    --text-light: #6b6b85;
    --border: #e0e0e8;
    --success: #10b981;
    --warning: #f59e0b;
    --error: #ef4444;
  }
  [data-theme="dark"] {
    --bg: #0a0a12;
    --card: #16161f;
    --text: #f0f0f4;
    --text-light: #a0a0b8;
    --border: #2a2a38;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { 
    font-family: 'Inter', system-ui, sans-serif; 
    background: var(--bg); 
    color: var(--text); 
    min-height: 100vh; 
    font-size: 14px; 
    line-height: 1.5;
    transition: background 0.4s ease;
  }

  /* HEADER */
  .header-top { 
    background: var(--fedex-purple); 
    padding: 8px 0; 
    font-size: 12px;
    color: white;
  }
  .header-top-inner {
    max-width: 1400px;
    margin: 0 auto;
    padding: 0 24px;
    display: flex;
    gap: 24px;
    justify-content: flex-end;
  }
  .header-top-inner a {
    color: white;
    text-decoration: none;
    opacity: 0.9;
    transition: opacity 0.2s;
  }
  .header-top-inner a:hover { opacity: 1; }

  .header-main { 
    background: var(--card); 
    border-bottom: 1px solid var(--border); 
    box-shadow: 0 2px 12px rgba(0,0,0,0.1); 
    position: sticky; 
    top: 0; 
    z-index: 100; 
  }
  .header-inner {
    max-width: 1400px;
    margin: 0 auto;
    padding: 12px 24px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 40px;
  }

  .fedex-wordmark {
    font-size: 2.25rem;
    font-weight: 900;
    letter-spacing: -2px;
    font-style: italic;
    text-decoration: none;
    display: flex;
    align-items: center;
    gap: 2px;
  }
  .fedex-wordmark .fed { color: var(--fedex-purple); }
  .fedex-wordmark .ex  { color: var(--fedex-orange); }
  .fedex-wordmark .sub { 
    font-size: 0.7em; 
    margin-left: 4px; 
    font-weight: 600; 
    color: var(--text-light);
    font-style: normal;
  }

  nav {
    display: flex;
    gap: 32px;
    flex: 1;
  }
  nav a {
    color: var(--text-light);
    text-decoration: none;
    font-weight: 500;
    transition: all 0.2s;
    position: relative;
  }
  nav a:hover { color: var(--text); }
  nav a.active {
    color: var(--fedex-purple);
    font-weight: 600;
  }
  nav a.active::after {
    content: '';
    position: absolute;
    bottom: -12px;
    left: 0;
    right: 0;
    height: 3px;
    background: var(--fedex-purple);
  }

  .h-btns {
    display: flex;
    gap: 8px;
  }

  /* BUTTONS */
  .btn-ol, .btn-or {
    padding: 8px 16px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 600;
    font-size: 13px;
    transition: all 0.2s;
    font-family: inherit;
  }
  .btn-ol {
    background: transparent;
    border: 1.5px solid var(--border);
    color: var(--text);
  }
  .btn-ol:hover {
    border-color: var(--fedex-purple);
    color: var(--fedex-purple);
  }
  .btn-or {
    background: var(--fedex-orange);
    color: white;
  }
  .btn-or:hover {
    background: #e55a00;
    transform: translateY(-1px);
  }

  /* SERVICES BAR */
  .svc-bar {
    background: var(--card);
    border-bottom: 1px solid var(--border);
    overflow-x: auto;
  }
  .svc-inner {
    max-width: 1400px;
    margin: 0 auto;
    padding: 0 24px;
    display: flex;
    gap: 0;
  }
  .svc-tab {
    padding: 16px 20px;
    cursor: pointer;
    color: var(--text-light);
    border-bottom: 3px solid transparent;
    transition: all 0.2s;
    white-space: nowrap;
  }
  .svc-tab:hover {
    color: var(--text);
    background: rgba(77, 20, 140, 0.05);
  }
  .svc-tab.on {
    color: var(--fedex-purple);
    border-bottom-color: var(--fedex-purple);
  }

  /* HERO */
  .hero { 
    background: linear-gradient(135deg, var(--fedex-purple-dark) 0%, var(--fedex-purple) 65%, var(--fedex-purple-light) 100%); 
    padding: 52px 24px 76px; 
    position: relative; 
    overflow: hidden;
  }
  .hero::before { 
    content:''; 
    position:absolute; 
    inset:0; 
    background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='%23ffffff' fill-opacity='0.04'%3E%3Crect x='28' y='28' width='4' height='4'/%3E%3C/g%3E%3C/svg%3E"); 
  }
  .hero-inner {
    max-width: 1400px;
    margin: 0 auto;
    position: relative;
    z-index: 1;
  }
  .hero h1 {
    font-size: 2.5rem;
    color: white;
    margin-bottom: 12px;
    font-weight: 900;
  }
  .hero > .hero-inner > p {
    font-size: 1.1rem;
    color: rgba(255,255,255,0.9);
    margin-bottom: 32px;
    max-width: 500px;
  }

  .tbox { 
    background: var(--card); 
    border-radius: 14px; 
    padding: 28px 32px; 
    box-shadow: 0 20px 55px rgba(0,0,0,0.18), 0 8px 20px rgba(77,20,140,0.15);
    max-width: 600px;
  }
  .tbox-label {
    font-size: 13px;
    font-weight: 600;
    color: var(--text-light);
    margin-bottom: 12px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }
  .tinput-row {
    display: flex;
    gap: 8px;
    margin-bottom: 16px;
  }
  .tinput {
    flex: 1;
    padding: 12px 16px;
    background: var(--card);
    border: 2px solid var(--border);
    border-radius: 8px;
    color: var(--text);
    font-size: 14px;
    font-family: 'Space Mono', monospace;
    transition: border-color 0.2s;
  }
  .tinput:focus {
    outline: none;
    border-color: var(--fedex-purple);
  }
  .tinput::placeholder { color: var(--text-light); }

  .tbtn {
    padding: 12px 32px;
    background: var(--fedex-purple);
    color: white;
    border: none;
    border-radius: 8px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.2s;
    font-family: inherit;
  }
  .tbtn:hover {
    background: var(--fedex-purple-light);
    transform: translateY(-1px);
  }

  .qrow {
    display: flex;
    gap: 12px;
    align-items: center;
    flex-wrap: wrap;
  }
  .qlabel {
    font-size: 12px;
    color: var(--text-light);
    font-weight: 600;
  }
  .qchip {
    display: inline-block;
    padding: 6px 12px;
    background: rgba(77,20,140,0.1);
    border: 1px solid rgba(77,20,140,0.2);
    border-radius: 20px;
    font-size: 12px;
    cursor: pointer;
    transition: all 0.2s;
    color: var(--fedex-purple);
  }
  .qchip:hover {
    background: rgba(77,20,140,0.2);
    transform: scale(1.05);
  }

  /* CONTENT */
  .content {
    max-width: 1400px;
    margin: 0 auto;
    padding: 40px 24px;
  }

  .empty {
    text-align: center;
    padding: 80px 40px;
  }
  .empty-icon {
    font-size: 80px;
    margin-bottom: 24px;
  }
  .empty-title {
    font-size: 1.5rem;
    font-weight: 700;
    margin-bottom: 8px;
  }
  .empty-sub {
    color: var(--text-light);
    font-size: 1rem;
  }

  #dash.on { display: block !important; }
  #dash { display: none; }

  /* CARDS */
  .card, .sc, .pc {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 12px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.1);
    transition: all 0.3s ease;
    margin-bottom: 24px;
  }
  .card:hover, .sc:hover, .pc:hover { box-shadow: 0 15px 40px rgba(0,0,0,0.14); }
  .card.full { grid-column: 1 / -1; }

  /* STATUS CARD */
  .sc {
    padding: 24px;
  }
  .sc-hd {
    display: flex;
    justify-content: space-between;
    gap: 32px;
    margin-bottom: 20px;
  }
  .sc-left, .sc-right {
    display: flex;
    align-items: center;
    gap: 16px;
  }
  .sc-icon {
    font-size: 2.5rem;
  }
  .sc-lbl {
    font-size: 12px;
    color: var(--text-light);
    text-transform: uppercase;
    font-weight: 600;
    letter-spacing: 0.5px;
  }
  .sc-val {
    font-size: 1.3rem;
    font-weight: 700;
    color: var(--text);
  }
  .sc-eta-lbl {
    font-size: 11px;
    color: var(--text-light);
    text-transform: uppercase;
    font-weight: 600;
  }
  .sc-eta-date {
    font-size: 1.4rem;
    font-weight: 700;
  }
  .sc-eta-cd {
    font-size: 12px;
    color: var(--success);
    font-weight: 600;
  }

  .sc-body {
    border-top: 1px solid var(--border);
    padding-top: 20px;
  }
  .sc-trow {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 16px;
  }
  .sc-id-lbl {
    font-size: 11px;
    color: var(--text-light);
    text-transform: uppercase;
    font-weight: 600;
    margin-bottom: 4px;
  }
  .sc-id-val {
    font-size: 1.1rem;
    font-weight: 700;
    font-family: 'Space Mono', monospace;
  }

  .sc-badge {
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .sc-logo {
    font-size: 1.5rem;
    font-weight: 900;
    letter-spacing: -1px;
  }
  .sc-logo .fed { color: var(--fedex-purple); }
  .sc-logo .ex { color: var(--fedex-orange); }

  .sc-svc {
    font-weight: 600;
    color: var(--text);
  }
  .sc-sub {
    font-size: 12px;
    color: var(--text-light);
  }

  /* PROGRESS CARD */
  .pc {
    padding: 24px;
  }
  .pc-top {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
  .sec-hd {
    font-size: 1.1rem;
    font-weight: 700;
  }
  .pct-pill {
    background: rgba(77,20,140,0.1);
    color: var(--fedex-purple);
    padding: 4px 12px;
    border-radius: 20px;
    font-weight: 700;
    font-size: 13px;
  }

  .ptrack {
    width: 100%;
    height: 8px;
    background: var(--border);
    border-radius: 10px;
    overflow: hidden;
    margin-bottom: 24px;
  }
  .pfill {
    height: 100%;
    background: linear-gradient(90deg, var(--fedex-purple), var(--fedex-orange));
    transition: width 0.6s ease;
  }

  .stages {
    display: flex;
    justify-content: space-between;
  }
  .stage {
    flex: 1;
    text-align: center;
    position: relative;
  }
  .stage::after {
    content: '';
    position: absolute;
    top: 20px;
    left: 50%;
    right: -50%;
    height: 2px;
    background: var(--border);
    z-index: 0;
  }
  .stage:last-child::after { display: none; }

  .stage-dot {
    width: 40px;
    height: 40px;
    margin: 0 auto 8px;
    border-radius: 50%;
    background: var(--border);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    position: relative;
    z-index: 1;
    transition: all 0.3s;
  }
  .stage.active .stage-dot {
    background: var(--fedex-orange);
    transform: scale(1.2);
    box-shadow: 0 0 0 4px rgba(255,102,0,0.2);
  }
  .stage.done .stage-dot {
    background: var(--success);
  }

  .stage-name {
    font-size: 11px;
    color: var(--text-light);
    font-weight: 600;
    text-transform: uppercase;
  }

  /* MAP */
  .card-hd {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 24px;
    border-bottom: 1px solid var(--border);
  }
  .badge {
    background: rgba(16,185,129,0.1);
    color: var(--success);
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 11px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }

  .mapwrap { 
    height: 300px; 
    background: linear-gradient(135deg, #1e2a3a 0%, #0f1620 100%);
    position: relative; 
    overflow: hidden; 
  }
  [data-theme="dark"] .mapwrap { background: linear-gradient(135deg, #0f1620 0%, #050b15 100%); }

  .scanline {
    position: absolute;
    height: 2px;
    width: 100%;
    background: linear-gradient(90deg, transparent, rgba(255,102,0,0.5), transparent);
    animation: scan 5s linear infinite;
    opacity: 0.18;
    z-index: 2;
  }
  @keyframes scan { 0% { top: -20px; } 100% { top: 100%; } }

  .maploc {
    position: absolute;
    top: 20px;
    left: 20px;
    z-index: 3;
  }
  .maploc-main {
    font-size: 13px;
    font-weight: 700;
    color: white;
  }
  .maploc-sub {
    font-size: 11px;
    color: rgba(255,255,255,0.7);
  }

  .maplive {
    position: absolute;
    bottom: 20px;
    right: 20px;
    display: flex;
    align-items: center;
    gap: 6px;
    background: rgba(16,185,129,0.2);
    border: 1px solid var(--success);
    color: var(--success);
    padding: 6px 12px;
    border-radius: 20px;
    font-weight: 700;
    font-size: 12px;
    z-index: 3;
  }
  .lring {
    width: 8px;
    height: 8px;
    background: var(--success);
    border-radius: 50%;
    animation: pulse 1.5s ease-in-out infinite;
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.3; }
  }

  /* TIMELINE */
  .timeline {
    padding: 24px;
  }
  .timeline-item {
    display: flex;
    gap: 16px;
    margin-bottom: 20px;
    position: relative;
  }
  .timeline-item:last-child { margin-bottom: 0; }
  .timeline-item::before {
    content: '';
    position: absolute;
    left: 19px;
    top: 40px;
    bottom: -20px;
    width: 2px;
    background: var(--border);
  }
  .timeline-item:last-child::before { display: none; }

  .tl-dot {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: var(--border);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 16px;
    flex-shrink: 0;
    position: relative;
    z-index: 1;
  }
  .timeline-item.active .tl-dot {
    background: var(--fedex-orange);
    box-shadow: 0 0 0 4px rgba(255,102,0,0.2);
  }
  .timeline-item.done .tl-dot {
    background: var(--success);
  }

  .tl-content {
    flex: 1;
    padding-top: 4px;
  }
  .tl-status {
    font-weight: 700;
    font-size: 0.95rem;
  }
  .tl-location {
    color: var(--text-light);
    font-size: 0.9rem;
    margin: 4px 0;
  }
  .tl-time {
    font-size: 12px;
    color: var(--text-light);
  }
  .tl-badge {
    display: inline-block;
    background: rgba(255,102,0,0.1);
    color: var(--fedex-orange);
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 11px;
    font-weight: 600;
    margin-left: 8px;
  }

  /* PACKAGE INFO */
  .pkg-info {
    padding: 24px;
  }
  .info-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
    margin-bottom: 24px;
  }
  .info-row:last-child { margin-bottom: 0; }

  .info-item {
    border-bottom: 1px solid var(--border);
    padding-bottom: 16px;
  }
  .info-item:last-child { border-bottom: none; }

  .info-label {
    font-size: 11px;
    color: var(--text-light);
    text-transform: uppercase;
    font-weight: 600;
    margin-bottom: 4px;
    letter-spacing: 0.5px;
  }
  .info-value {
    font-size: 0.95rem;
    font-weight: 600;
  }

  /* ADDRESSES */
  .addr {
    padding: 24px;
  }
  .addr-pair {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 32px;
  }
  .addr-block {
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 16px;
  }
  .addr-label {
    font-size: 12px;
    color: var(--text-light);
    text-transform: uppercase;
    font-weight: 700;
    margin-bottom: 12px;
    letter-spacing: 1px;
  }
  .addr-name {
    font-weight: 700;
    font-size: 1rem;
    margin-bottom: 4px;
  }
  .addr-text {
    font-size: 0.9rem;
    line-height: 1.6;
    color: var(--text-light);
  }

  /* NOTIFICATIONS */
  .notif {
    position: fixed;
    top: 20px;
    right: 20px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 16px;
    max-width: 300px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.2);
    animation: slideIn 0.3s ease;
    z-index: 1000;
  }
  @keyframes slideIn {
    from { transform: translateX(400px); opacity: 0; }
    to { transform: translateX(0); opacity: 1; }
  }
  .notif-title {
    font-weight: 700;
    margin-bottom: 4px;
  }
  .notif-msg {
    font-size: 0.9rem;
    color: var(--text-light);
  }

  /* MODAL */
  #mbg {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.6);
    z-index: 2000;
    animation: fadeIn 0.2s ease;
  }
  #mbg.on { display: flex; }
  @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

  #mbg {
    align-items: center;
    justify-content: center;
  }

  .modal {
    background: var(--card);
    border-radius: 12px;
    width: 90%;
    max-width: 600px;
    max-height: 80vh;
    display: flex;
    flex-direction: column;
    animation: slideUp 0.3s ease;
  }
  @keyframes slideUp {
    from { transform: translateY(50px); opacity: 0; }
    to { transform: translateY(0); opacity: 1; }
  }

  .mhd {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px;
    border-bottom: 1px solid var(--border);
  }
  .mtitle {
    font-size: 1.1rem;
    font-weight: 700;
  }
  .mclose {
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
    color: var(--text-light);
    transition: color 0.2s;
  }
  .mclose:hover { color: var(--text); }

  #mbody {
    flex: 1;
    overflow-y: auto;
    padding: 20px;
  }

  .scan-log {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }
  .scan-entry {
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 12px;
    background: rgba(77,20,140,0.05);
  }
  .scan-time {
    font-weight: 700;
    font-size: 0.9rem;
  }
  .scan-text {
    font-size: 0.85rem;
    color: var(--text-light);
    margin: 4px 0;
  }

  @media (max-width: 768px) {
    nav { gap: 16px; font-size: 0.9rem; }
    .h-btns { flex-wrap: wrap; }
    .hero h1 { font-size: 1.8rem; }
    .sc-hd { flex-direction: column; gap: 16px; }
    .addr-pair { grid-template-columns: 1fr; }
    .info-row { grid-template-columns: 1fr; }
    .stages { flex-wrap: wrap; }
    .stage { flex: 0 0 calc(50% - 12px); }
    .stage::after { display: none !important; }
  }
</style>
</head>
<body data-theme="light">

<div id="nwrap"></div>

<div id="mbg">
  <div class="modal">
    <div class="mhd">
      <div class="mtitle">📦 Package Details & Scan Log</div>
      <button class="mclose" onclick="closeModal()">✕</button>
    </div>
    <div id="mbody"></div>
  </div>
</div>

<!-- HEADER -->
<div class="header-top">
  <div class="header-top-inner">
    <a href="#">FedEx Global Home</a>
    <a href="#">Customer Support</a>
    <a href="#">🇹🇷 Türkiye</a>
    <a href="#">EN | TR</a>
  </div>
</div>

<div class="header-main">
  <div class="header-inner">
    <a class="fedex-wordmark" href="#">
      <span class="fed">Fed</span><span class="ex">Ex</span><span class="sub">Express</span>
    </a>
    <nav>
      <a href="#" class="active">Track</a>
      <a href="#">Ship</a>
      <a href="#">Manage</a>
      <a href="#">Learn</a>
      <a href="#">Locations</a>
      <a href="#">Support</a>
    </nav>
    <div class="h-btns">
      <button class="btn-ol" onclick="openModal()">View Details</button>
      <button class="btn-ol" onclick="toggleTheme()">🌗</button>
      <button class="btn-or" id="soundBtn" onclick="toggleSound()">🔇 Alerts</button>
    </div>
  </div>
</div>

<!-- SERVICES BAR -->
<div class="svc-bar">
  <div class="svc-inner">
    <div class="svc-tab on">Track Shipments</div>
    <div class="svc-tab">Ship Now</div>
    <div class="svc-tab">FedEx Express</div>
    <div class="svc-tab">International</div>
    <div class="svc-tab">🇹🇷 Turkey Routes</div>
  </div>
</div>

<!-- HERO -->
<div class="hero">
  <div class="hero-inner">
    <h1>Track Your Shipment</h1>
    <p>Real-time tracking worldwide — including Turkey 🇹🇷 and all major global hubs.</p>
    <div class="tbox">
      <div class="tbox-label">Enter Tracking Number(s)</div>
      <div class="tinput-row">
        <input class="tinput" id="trackInput" type="text" placeholder="e.g. FX-7748293-TR" maxlength="30"/>
        <button class="tbtn" onclick="doTrack()">TRACK</button>
      </div>
      <div class="qrow">
        <span class="qlabel">Demo shipments:</span>
        <span class="qchip" onclick="qt('FX-7748293-TR')">FX-7748293-TR 🇹🇷→🇩🇪</span>
        <span class="qchip" onclick="qt('FX-3300125-AS')">FX-3300125-AS 🇨🇳→🇹🇷</span>
        <span class="qchip" onclick="qt('FX-4829301-US')">FX-4829301-US 🇺🇸→🇩🇪</span>
        <span class="qchip" onclick="qt('FX-9912847-EU')">FX-9912847-EU 🇩🇪→🇬🇧</span>
      </div>
    </div>
  </div>
</div>

<!-- CONTENT -->
<div class="content">
  <div class="empty" id="emptyState">
    <div class="empty-icon">📦</div>
    <div class="empty-title">No shipment tracked yet</div>
    <div class="empty-sub">Enter a FedEx tracking number or select a demo above.</div>
  </div>

  <div id="dash">
    <!-- STATUS CARD -->
    <div class="sc">
      <div class="sc-hd">
        <div class="sc-left">
          <div class="sc-icon" id="scIcon">✈️</div>
          <div>
            <div class="sc-lbl">Current Status</div>
            <div class="sc-val" id="scStatus">In Transit</div>
          </div>
        </div>
        <div class="sc-right">
          <div class="sc-eta-lbl">Estimated Delivery</div>
          <div class="sc-eta-date" id="scEta">—</div>
          <div class="sc-eta-cd" id="scCd">Calculating…</div>
        </div>
      </div>
      <div class="sc-body">
        <div class="sc-trow">
          <div>
            <div class="sc-id-lbl">FedEx Tracking ID</div>
            <div class="sc-id-val" id="scId">—</div>
          </div>
          <div class="sc-badge">
            <div class="sc-logo"><span class="fed">Fed</span><span class="ex">Ex</span></div>
            <div><div class="sc-svc" id="scSvc">Express International</div></div>
          </div>
        </div>
        <div class="sc-sub" id="scSub">Last scanned just now</div>
      </div>
    </div>

    <!-- PROGRESS -->
    <div class="pc">
      <div class="pc-top">
        <div class="sec-hd">Shipment Progress</div>
        <div class="pct-pill" id="pctPill">0%</div>
      </div>
      <div class="ptrack"><div class="pfill" id="pfill" style="width:0%"></div></div>
      <div class="stages" id="stagesRow"></div>
    </div>

    <!-- MAP -->
    <div class="card full">
      <div class="card-hd">
        <span class="sec-hd">🌍 Live Route Map</span>
        <span class="badge">GPS TRACKING ACTIVE</span>
      </div>
      <div class="mapwrap">
        <div class="scanline"></div>
        <svg id="mapSvg" viewBox="0 0 900 380" preserveAspectRatio="xMidYMid meet">
          <rect width="900" height="380" fill="#c5ddef"/>
          <path d="M0,0 L900,0 L900,380 L0,380 Z" fill="url(#oceanGradient)"/>
          
          <!-- Major Countries -->
          <path d="M720,150 L780,140 L800,180 L750,200 Z" fill="#e0e0e0" stroke="#999" stroke-width="1"/>
          <text x="750" y="170" font-family="Arial" font-size="8" fill="#333">USA</text>
          
          <path d="M320,120 L380,100 L390,180 L340,200 Z" fill="#e0e0e0" stroke="#999" stroke-width="1"/>
          <text x="340" y="160" font-family="Arial" font-size="8" fill="#333">EU</text>
          
          <path d="M466,84 L530,78 L550,92 L548,118 L514,126 L468,116 Z" fill="#ffe5cc" stroke="#FF6600" stroke-width="2"/>
          <text x="488" y="106" font-family="Arial" font-size="9" font-weight="bold" fill="#FF6600">🇹🇷 TR</text>
          
          <path d="M550,80 L650,60 L680,120 L600,150 Z" fill="#e0e0e0" stroke="#999" stroke-width="1"/>
          <text x="600" y="110" font-family="Arial" font-size="8" fill="#333">ASIA</text>

          <!-- Route path -->
          <defs>
            <linearGradient id="rg" x1="0%" y1="0%" x2="100%" y2="0%">
              <stop offset="0%" stop-color="#4D148C"/>
              <stop offset="100%" stop-color="#FF6600"/>
            </linearGradient>
            <linearGradient id="oceanGradient" x1="0%" y1="0%" x2="0%" y2="100%">
              <stop offset="0%" stop-color="#c5ddef"/>
              <stop offset="100%" stop-color="#a8c5dd"/>
            </linearGradient>
          </defs>

          <path id="routePath" d="M750,180 L680,150 L600,120 L500,100 L488,100" fill="none" stroke="url(#rg)" stroke-width="3.5" stroke-dasharray="12 6" opacity="0.9"/>
          
          <g id="vg" transform="translate(488, 100)">
            <circle r="12" fill="rgba(255,102,0,0.2)"/>
            <circle r="6" fill="#FF6600"/>
          </g>

          <g id="og" transform="translate(750, 180)"><circle r="7" fill="#4D148C"/></g>
          <g id="dg" transform="translate(488, 100)"><circle r="7" fill="#FF6600"/></g>
        </svg>
        <div class="maploc">
          <div class="maploc-main" id="mapLocMain">Current Location</div>
          <div class="maploc-sub" id="mapLocSub">FedEx Live GPS • Real-time</div>
        </div>
        <div class="maplive"><div class="lring"></div>LIVE</div>
      </div>
    </div>

    <!-- TIMELINE -->
    <div class="card full">
      <div style="padding: 24px;">
        <div class="sec-hd" style="margin-bottom: 20px;">📋 Event History</div>
      </div>
      <div class="timeline" id="timeline"></div>
    </div>

    <!-- PACKAGE INFO -->
    <div class="card full">
      <div style="padding: 24px; border-bottom: 1px solid var(--border);">
        <div class="sec-hd">📦 Package Information</div>
      </div>
      <div class="pkg-info">
        <div class="info-row">
          <div class="info-item">
            <div class="info-label">Weight</div>
            <div class="info-value" id="pkgWeight">—</div>
          </div>
          <div class="info-item">
            <div class="info-label">Dimensions</div>
            <div class="info-value" id="pkgDim">—</div>
          </div>
        </div>
        <div class="info-row">
          <div class="info-item">
            <div class="info-label">Service Type</div>
            <div class="info-value" id="pkgService">—</div>
          </div>
          <div class="info-item">
            <div class="info-label">Reference Number</div>
            <div class="info-value" id="pkgRef">—</div>
          </div>
        </div>
      </div>
    </div>

    <!-- ADDRESSES -->
    <div class="card full">
      <div style="padding: 24px; border-bottom: 1px solid var(--border);">
        <div class="sec-hd">📍 Shipping Addresses</div>
      </div>
      <div class="addr">
        <div class="addr-pair">
          <div class="addr-block">
            <div class="addr-label">From</div>
            <div class="addr-name" id="addrFromName">—</div>
            <div class="addr-text" id="addrFromText">—</div>
          </div>
          <div class="addr-block">
            <div class="addr-label">To</div>
            <div class="addr-name" id="addrToName">—</div>
            <div class="addr-text" id="addrToText">—</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
// ==================== SHIPMENT DATA ====================
const SSTATUS = {
  0: { label: 'Created', icon: '📦' },
  1: { label: 'Picked Up', icon: '🚚' },
  2: { label: 'At Facility', icon: '🏭' },
  3: { label: 'Customs', icon: '🛂' },
  4: { label: 'In Transit', icon: '✈️' },
  5: { label: 'Out for Delivery', icon: '🚴' },
  6: { label: 'Delivered', icon: '✅' }
};

const SHIPS = {
  'FX-7748293-TR': {
    id: 'FX-7748293-TR',
    from: 'Istanbul, Turkey',
    to: 'Berlin, Germany',
    cs: 4,
    w: '2.5 kg',
    dim: '30×20×15 cm',
    svc: 'FedEx Express International',
    ref: 'ORD-891234-TR',
    eta: '2026-05-10',
    route: 'M750,180 L680,150 L600,120 L500,100 L488,100',
    evs: [
      { s: 'Shipment created', l: 'Istanbul Terminal', i: '📦', t: '2 days ago', type: 'done' },
      { s: 'Picked up from sender', l: 'Fatih District, Istanbul', i: '🚚', t: '2 days ago', type: 'done' },
      { s: 'Arrived at facility', l: 'Istanbul International Hub', i: '🏭', t: '1 day ago', type: 'done' },
      { s: 'Customs clearance', l: 'Turkish Customs', i: '✅', t: '1 day ago', type: 'done' },
      { s: 'In transit to Europe', l: 'European Air Route', i: '✈️', t: '4 hours ago', type: 'active' },
      { s: 'Processing at facility', l: 'Leipzig Hub, Germany', i: '🏭', t: 'Est. in 2 hours', type: 'pending' },
      { s: 'Out for delivery', l: 'Berlin Local Station', i: '🚴', t: 'Est. tomorrow', type: 'pending' }
    ]
  },
  'FX-3300125-AS': {
    id: 'FX-3300125-AS',
    from: 'Shanghai, China',
    to: 'Ankara, Turkey',
    cs: 3,
    w: '5.8 kg',
    dim: '45×35×25 cm',
    svc: 'FedEx Express International',
    ref: 'ORD-556789-CN',
    eta: '2026-05-12',
    route: 'M600,150 L500,100 L488,100',
    evs: [
      { s: 'Order placed', l: 'Shanghai', i: '📦', t: '5 days ago', type: 'done' },
      { s: 'Picked up from warehouse', l: 'Pudong District', i: '🚚', t: '4 days ago', type: 'done' },
      { s: 'At Shanghai sorting facility', l: 'Shanghai Hub', i: '🏭', t: '4 days ago', type: 'done' },
      { s: 'Customs processing', l: 'Shanghai Customs', i: '🛂', t: '2 days ago', type: 'active' },
      { s: 'International transit', l: 'Trans-Asian Route', i: '✈️', t: 'Est. in 3 days', type: 'pending' }
    ]
  },
  'FX-4829301-US': {
    id: 'FX-4829301-US',
    from: 'New York, USA',
    to: 'Frankfurt, Germany',
    cs: 4,
    w: '1.2 kg',
    dim: '25×20×10 cm',
    svc: 'FedEx Express International',
    ref: 'ORD-234567-US',
    eta: '2026-05-09',
    route: 'M750,180 L600,120 L500,100',
    evs: [
      { s: 'Shipment picked up', l: 'Manhattan, NY', i: '🚚', t: '3 days ago', type: 'done' },
      { s: 'Sorted at facility', l: 'New York Hub', i: '🏭', t: '3 days ago', type: 'done' },
      { s: 'Departed USA', l: 'JFK International', i: '✈️', t: '2 days ago', type: 'done' },
      { s: 'In transit over Atlantic', l: 'International Airspace', i: '✈️', t: '6 hours ago', type: 'active' },
      { s: 'Expected at destination', l: 'Frankfurt', i: '🏭', t: 'Est. tomorrow', type: 'pending' }
    ]
  },
  'FX-9912847-EU': {
    id: 'FX-9912847-EU',
    from: 'Munich, Germany',
    to: 'London, UK',
    cs: 5,
    w: '0.8 kg',
    dim: '20×15×8 cm',
    svc: 'FedEx Express',
    ref: 'ORD-789123-DE',
    eta: '2026-05-08',
    route: 'M400,120 L380,100 L320,130',
    evs: [
      { s: 'Package picked up', l: 'Munich, Germany', i: '🚚', t: '1 day ago', type: 'done' },
      { s: 'Arrived at distribution', l: 'Munich Hub', i: '🏭', t: '1 day ago', type: 'done' },
      { s: 'Sorted and dispatched', l: 'Munich Facility', i: '📦', t: '20 hours ago', type: 'done' },
      { s: 'In transit to UK', l: 'Channel Route', i: '🚚', t: '8 hours ago', type: 'done' },
      { s: 'Out for delivery today', l: 'London Depot', i: '🚴', t: 'This morning', type: 'active' }
    ]
  }
};

const STAGES = ['Created','Picked Up','At Facility','Customs','In Transit','Out for Delivery','Delivered'];
let cur = null, liveT = null, cdT = null, soundOn = false;

function qt(id){ 
  document.getElementById('trackInput').value = id; 
  doTrack(); 
}

function doTrack(){
  const id = document.getElementById('trackInput').value.trim().toUpperCase();
  if(!id) return notify('⚠️','Missing ID','Please enter a tracking number.');
  
  if(!SHIPS[id]) return notify('❌','Not Found', `Tracking number "${id}" not found. Try the demos above.`);
  
  cur = SHIPS[id];
  renderAll(cur, id);
}

function renderAll(s, id){
  document.getElementById('emptyState').style.display = 'none';
  document.getElementById('dash').classList.add('on');
  
  renderStatus(s);
  renderProgress(s);
  renderTimeline(s);
  renderMap(s);
  renderPackageInfo(s);
  renderAddresses(s);
  
  startLiveUpdates(s);
  notify('📦','Shipment Found', `${id} — ${SSTATUS[s.cs].label}`);
}

function renderStatus(s){
  const status = SSTATUS[s.cs];
  document.getElementById('scIcon').textContent = status.icon;
  document.getElementById('scStatus').textContent = status.label;
  document.getElementById('scId').textContent = s.id;
  document.getElementById('scSvc').textContent = s.svc;
  document.getElementById('scEta').textContent = s.eta;
  document.getElementById('scSub').textContent = `Last scanned ${s.evs[s.evs.length-1].t || 'just now'}`;
  
  const daysLeft = Math.ceil((new Date(s.eta) - new Date()) / (1000*60*60*24));
  document.getElementById('scCd').textContent = daysLeft > 0 ? `${daysLeft} days left` : 'Today';
}

function renderProgress(s){
  const pct = Math.round((s.cs / 6) * 100);
  document.getElementById('pfill').style.width = pct + '%';
  document.getElementById('pctPill').textContent = pct + '%';
  
  let stagesHtml = '';
  for(let i = 0; i < STAGES.length; i++){
    const stageClass = i < s.cs ? 'done' : i === s.cs ? 'active' : '';
    stagesHtml += `
      <div class="stage ${stageClass}">
        <div class="stage-dot">${STAGES[i][0]}</div>
        <div class="stage-name">${STAGES[i]}</div>
      </div>
    `;
  }
  document.getElementById('stagesRow').innerHTML = stagesHtml;
}

function renderTimeline(s){
  let tlHtml = '';
  s.evs.forEach((ev, idx) => {
    const isActive = ev.type === 'active';
    const isDone = ev.type === 'done';
    const cls = isActive ? 'active' : isDone ? 'done' : '';
    tlHtml += `
      <div class="timeline-item ${cls}">
        <div class="tl-dot">${ev.i}</div>
        <div class="tl-content">
          <div class="tl-status">${ev.s}${isActive ? '<span class="tl-badge">LIVE</span>' : ''}</div>
          <div class="tl-location">📍 ${ev.l}</div>
          <div class="tl-time">${ev.t}</div>
        </div>
      </div>
    `;
  });
  document.getElementById('timeline').innerHTML = tlHtml;
}

function renderMap(s){
  document.getElementById('routePath').setAttribute('d', s.route);
  document.getElementById('mapLocMain').textContent = SSTATUS[s.cs].label;
  document.getElementById('mapLocSub').textContent = '📍 ' + (s.evs[s.evs.length-1]?.l || 'In Transit');
  
  animateVehicleAlongPath(s);
}

function animateVehicleAlongPath(s){
  const vehicle = document.getElementById('vg');
  const path = document.getElementById('routePath');
  if(!path || !vehicle) return;

  const length = path.getTotalLength();
  let start = Date.now();

  function move(){
    if(!cur || cur.id !== s.id) return;
    const progress = ((Date.now() - start) % 25000) / 25000;
    const point = path.getPointAtLength(progress * length);
    
    vehicle.setAttribute('transform', `translate(${point.x} ${point.y})`);
    requestAnimationFrame(move);
  }
  move();
}

function renderPackageInfo(s){
  document.getElementById('pkgWeight').textContent = s.w;
  document.getElementById('pkgDim').textContent = s.dim;
  document.getElementById('pkgService').textContent = s.svc;
  document.getElementById('pkgRef').textContent = s.ref;
}

function renderAddresses(s){
  const [fromCity, fromCountry] = s.from.split(', ');
  const [toCity, toCountry] = s.to.split(', ');
  
  document.getElementById('addrFromName').textContent = 'FedEx Pickup';
  document.getElementById('addrFromText').textContent = s.from + '\nTurkey';
  document.getElementById('addrToName').textContent = 'Recipient';
  document.getElementById('addrToText').textContent = s.to + '\nDestination Country';
}

function startLiveUpdates(s){
  if(liveT) clearInterval(liveT);
  liveT = setInterval(() => liveUpdate(s), 8500);
}

function liveUpdate(s){
  if(s.cs >= 6) return;
  
  const updates = [
    {s:'Scanned at transit facility', l:'Leipzig Hub, Germany', i:'🏭'},
    {s:'In transit • On schedule', l:'European Air Route', i:'✈️'},
    {s:'Customs cleared successfully', l:'Düsseldorf Gateway', i:'✅'},
    {s:'Package on vehicle', l:'Local delivery zone', i:'🚴'},
    {s:'Next stop processing', l:'Regional distribution', i:'🚚'}
  ];
  const u = updates[Math.floor(Math.random()*updates.length)];
  s.evs.push({...u, t:'Just now', type:'active', isNew:true});
  if(s.evs.length > 10) s.evs.shift();
  
  renderTimeline(s);
  renderMap(s);
  notify('📡','Live Tracking Update', u.s);
  if(soundOn) beep();
}

function openModal(){
  const s = cur;
  if(!s) return notify('⚠️', 'No Data', 'Track a shipment first');
  
  let html = '<div class="scan-log">';
  s.evs.forEach(ev => {
    html += `
      <div class="scan-entry">
        <div class="scan-time">${ev.t}</div>
        <div class="scan-text"><strong>${ev.s}</strong></div>
        <div class="scan-text">📍 ${ev.l}</div>
      </div>
    `;
  });
  html += '</div>';
  
  document.getElementById('mbody').innerHTML = html;
  document.getElementById('mbg').classList.add('on');
}

function closeModal(){
  document.getElementById('mbg').classList.remove('on');
}

function toggleTheme(){
  const body = document.body;
  const theme = body.getAttribute('data-theme') === 'dark' ? 'light' : 'dark';
  body.setAttribute('data-theme', theme);
  localStorage.setItem('theme', theme);
}

function toggleSound(){
  soundOn = !soundOn;
  document.getElementById('soundBtn').textContent = soundOn ? '🔊 Alerts' : '🔇 Alerts';
  notify('🔔', soundOn ? 'Alerts Enabled' : 'Alerts Disabled', soundOn ? 'You will hear notifications' : 'Silent mode');
}

function notify(icon, title, msg){
  const n = document.createElement('div');
  n.className = 'notif';
  n.innerHTML = `<div class="notif-title">${icon} ${title}</div><div class="notif-msg">${msg}</div>`;
  document.getElementById('nwrap').appendChild(n);
  setTimeout(() => n.remove(), 4000);
}

function beep(){
  try {
    const ctx = new (window.AudioContext || window.webkitAudioContext)();
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    osc.connect(gain);
    gain.connect(ctx.destination);
    osc.frequency.value = 800;
    gain.gain.setValueAtTime(0.3, ctx.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.1);
    osc.start(ctx.currentTime);
    osc.stop(ctx.currentTime + 0.1);
  } catch(e) {}
}

document.getElementById('trackInput').addEventListener('keydown', e => {
  if(e.key === 'Enter') doTrack();
});

// Load theme preference
const savedTheme = localStorage.getItem('theme') || 'light';
document.body.setAttribute('data-theme', savedTheme);

// Auto demo load
setTimeout(() => {
  document.getElementById('trackInput').value = 'FX-7748293-TR';
  doTrack();
}, 1000);
</script>
</body>
</html>
```
