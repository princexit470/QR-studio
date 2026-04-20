<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>QR Studio • Professional</title>
    
    <script src="https://cdn.jsdelivr.net/npm/jsqr@1.4.0/dist/jsQR.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/qrcode-generator@1.4.4/qrcode.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        /* ========== 基础样式 + 动画增强 ========== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }

        body {
            background: #f5f5f7;
            min-height: 100vh;
            color: #1d1d1f;
            transition: background 0.3s ease;
        }

        body.dark {
            background: #1a1a1e;
            color: #e5e5e7;
        }

        /* 平滑过渡 */
        .header, .card, .btn, .tab, .content-type-btn, .style-item {
            transition: all 0.25s ease;
        }

        .header {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            padding: 16px 24px;
            position: sticky;
            top: 0;
            z-index: 50;
            border-bottom: 1px solid rgba(0,0,0,0.06);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        body.dark .header {
            background: rgba(26, 26, 30, 0.95);
            border-bottom: 1px solid rgba(255,255,255,0.06);
        }

        .header h1 {
            font-size: 1.5rem;
            font-weight: 600;
            background: linear-gradient(135deg, #0066cc, #5ac8fa);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .multi-badge {
            background: #0066cc;
            color: white;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 0.7rem;
            font-weight: 600;
            margin-left: 10px;
            transition: opacity 0.2s;
        }

        .header-actions {
            display: flex;
            align-items: center;
            gap: 16px;
            position: relative;
        }

        .menu-btn {
            background: none;
            border: none;
            font-size: 1.4rem;
            cursor: pointer;
            color: #1d1d1f;
            padding: 8px;
            border-radius: 50%;
            transition: background 0.2s, transform 0.2s;
        }

        .menu-btn:hover {
            background: rgba(0,0,0,0.05);
            transform: scale(0.95);
        }

        body.dark .menu-btn {
            color: #e5e5e7;
        }
        body.dark .menu-btn:hover {
            background: rgba(255,255,255,0.1);
        }

        .menu-dropdown {
            display: none;
            position: fixed;
            top: 70px;
            right: 24px;
            background: white;
            border-radius: 16px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.15);
            min-width: 200px;
            z-index: 200;
            border: 1px solid rgba(0,0,0,0.08);
            animation: fadeSlide 0.2s ease;
        }

        @keyframes fadeSlide {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        body.dark .menu-dropdown {
            background: #2c2c2e;
            border-color: rgba(255,255,255,0.08);
        }

        .menu-dropdown.active {
            display: block;
        }

        .menu-item {
            padding: 14px 20px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 12px;
            transition: background 0.15s;
        }

        .menu-item:hover {
            background: #f5f5f7;
        }

        body.dark .menu-item:hover {
            background: #3a3a3c;
        }

        .tabs {
            display: flex;
            gap: 8px;
            max-width: 400px;
            margin: 0 auto;
            padding: 16px 20px 0;
        }

        .tab {
            flex: 1;
            padding: 10px 20px;
            border: none;
            background: transparent;
            color: #86868b;
            font-size: 1rem;
            font-weight: 500;
            cursor: pointer;
            border-radius: 30px;
            transition: all 0.2s;
        }

        .tab.active {
            background: #1d1d1f;
            color: white;
        }

        body.dark .tab.active {
            background: #0066cc;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px 24px;
        }

        .page {
            display: none;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .page.active {
            display: block;
        }

        .generator-layout {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 24px;
        }

        .full-width {
            grid-column: span 2;
        }

        .card {
            background: white;
            border-radius: 20px;
            padding: 24px;
            box-shadow: 0 2px 12px rgba(0,0,0,0.04);
            border: 1px solid rgba(0,0,0,0.04);
            word-wrap: break-word;
            overflow: hidden;
            transition: box-shadow 0.3s, transform 0.2s;
        }

        .card:hover {
            box-shadow: 0 8px 24px rgba(0,0,0,0.08);
        }

        body.dark .card {
            background: #2c2c2e;
            border-color: rgba(255,255,255,0.04);
        }

        .card-title {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .content-type-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 8px;
            margin-bottom: 20px;
        }

        .content-type-btn {
            padding: 12px 16px;
            background: #f5f5f7;
            border: 1.5px solid transparent;
            border-radius: 14px;
            font-size: 0.9rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .content-type-btn:hover {
            background: #e8e8ed;
            transform: translateY(-2px);
        }

        body.dark .content-type-btn {
            background: #3a3a3c;
            color: #e5e5e7;
        }

        .content-type-btn.active {
            background: #0066cc;
            color: white;
        }

        .form-group {
            margin-bottom: 18px;
        }

        .form-group label {
            display: block;
            font-size: 0.75rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.3px;
            color: #86868b;
            margin-bottom: 6px;
        }

        .form-control {
            width: 100%;
            padding: 12px 16px;
            border: 1.5px solid #e5e5e7;
            border-radius: 14px;
            font-size: 0.95rem;
            background: white;
            transition: border 0.2s, box-shadow 0.2s;
        }

        .form-control:focus {
            outline: none;
            border-color: #0066cc;
            box-shadow: 0 0 0 3px rgba(0,102,204,0.1);
        }

        body.dark .form-control {
            background: #3a3a3c;
            border-color: #3a3a3c;
            color: white;
        }

        textarea.form-control {
            min-height: 80px;
            resize: vertical;
        }

        .row-2 {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }

        .style-label {
            font-size: 0.75rem;
            font-weight: 600;
            text-transform: uppercase;
            color: #86868b;
            margin: 16px 0 10px;
        }

        .gradient-grid, .texture-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 8px;
        }

        .style-item {
            aspect-ratio: 1;
            border-radius: 12px;
            cursor: pointer;
            border: 2px solid transparent;
            transition: all 0.2s;
        }

        .style-item:hover {
            transform: scale(0.95);
        }

        .style-item.active {
            border-color: #0066cc;
        }

        .color-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 16px;
            margin-top: 16px;
        }

        .color-picker {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .color-picker input[type="color"] {
            width: 50px;
            height: 40px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
        }

        .opacity-control {
            margin-top: 16px;
        }

        .qr-preview-section {
            text-align: center;
        }

        .qr-preview {
            background: #fafafa;
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 20px;
            transition: background 0.3s;
        }

        body.dark .qr-preview {
            background: #1c1c1e;
        }

        #qrcodeCanvas {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 250px;
        }

        #qrcodeCanvas canvas {
            max-width: 250px;
            border-radius: 12px;
            transition: transform 0.2s;
        }

        .action-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }

        .btn {
            padding: 14px 20px;
            border: none;
            border-radius: 40px;
            font-weight: 600;
            font-size: 0.95rem;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            transition: all 0.2s;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }

        .btn:active {
            transform: translateY(0);
        }

        .btn-primary {
            background: #0066cc;
            color: white;
        }

        .btn-primary:hover {
            background: #0055aa;
        }

        .btn-secondary {
            background: #f5f5f7;
            color: #1d1d1f;
        }

        body.dark .btn-secondary {
            background: #3a3a3c;
            color: #e5e5e7;
        }

        .btn-small {
            padding: 8px 16px;
            font-size: 0.85rem;
        }

        .upload-area {
            border: 2px dashed #c6c6c8;
            border-radius: 20px;
            padding: 40px;
            text-align: center;
            cursor: pointer;
            transition: border 0.2s, background 0.2s;
        }

        .upload-area:hover {
            border-color: #0066cc;
            background: rgba(0,102,204,0.02);
        }

        body.dark .upload-area {
            border-color: #3a3a3c;
        }

        .camera-fullscreen {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: black;
            z-index: 1000;
        }

        .camera-fullscreen.active {
            display: block;
        }

        #cameraVideo {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .scan-box {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 280px;
            height: 280px;
            border: 2px solid #00cc66;
            border-radius: 24px;
            box-shadow: 0 0 0 2000px rgba(0,0,0,0.6);
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { border-color: #00cc66; }
            50% { border-color: #66ffa6; }
            100% { border-color: #00cc66; }
        }

        .camera-close {
            position: absolute;
            top: 20px;
            left: 20px;
            background: rgba(0,0,0,0.5);
            color: white;
            border: none;
            width: 48px;
            height: 48px;
            border-radius: 50%;
            font-size: 1.4rem;
            cursor: pointer;
            z-index: 1001;
            transition: background 0.2s;
        }

        .history-panel {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 150;
            backdrop-filter: blur(5px);
        }

        .history-panel.active {
            display: block;
        }

        .history-content {
            position: absolute;
            top: 0;
            right: 0;
            width: 100%;
            max-width: 420px;
            height: 100%;
            background: white;
            padding: 24px;
            overflow-y: auto;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(100%); }
            to { transform: translateX(0); }
        }

        body.dark .history-content {
            background: #1c1c1e;
        }

        .history-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 20px;
        }

        .history-search {
            width: 100%;
            padding: 12px 16px;
            border: 1.5px solid #e5e5e7;
            border-radius: 30px;
            margin-bottom: 20px;
        }

        .history-item {
            padding: 16px;
            background: #f5f5f7;
            border-radius: 14px;
            margin-bottom: 10px;
            cursor: pointer;
            border: 1px solid transparent;
            transition: all 0.2s;
        }

        .history-item:hover {
            background: #e8e8ed;
            border-color: #0066cc;
            transform: translateX(4px);
        }

        body.dark .history-item {
            background: #2c2c2e;
        }

        .preview-card {
            background: #f5f5f7;
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 12px;
            animation: fadeIn 0.4s ease;
        }

        body.dark .preview-card {
            background: #3a3a3c;
        }

        .action-group {
            display: flex;
            gap: 10px;
            margin-top: 12px;
            flex-wrap: wrap;
        }

        .hidden {
            display: none !important;
        }

        @media (max-width: 768px) {
            .generator-layout {
                grid-template-columns: 1fr;
            }
            .full-width {
                grid-column: span 1;
            }
        }
    </style>
</head>
<body class="light">
    <!-- Header -->
    <div class="header">
        <div style="display: flex; align-items: center;">
            <h1><i class="fas fa-qrcode" style="margin-right: 8px;"></i>QR Studio</h1>
            <span class="multi-badge hidden" id="multiBadge">Multi</span>
        </div>
        <div class="header-actions">
            <button class="menu-btn" id="menuToggle"><i class="fas fa-bars"></i></button>
            <div class="menu-dropdown" id="menuDropdown">
                <div class="menu-item" id="darkModeToggle">
                    <i class="fas fa-moon"></i><span>Dark Mode</span>
                </div>
                <div class="menu-item" id="historyMenuBtn">
                    <i class="fas fa-history"></i><span>History</span>
                </div>
            </div>
        </div>
    </div>

    <!-- Tabs -->
    <div class="tabs">
        <button class="tab active" data-page="generator">Generate</button>
        <button class="tab" data-page="scanner">Scan</button>
    </div>

    <div class="container">
        <!-- GENERATOR PAGE -->
        <div class="page active" id="generatorPage">
            <div class="generator-layout">
                <div class="card">
                    <div class="card-title"><i class="fas fa-layer-group"></i> Content Types</div>
                    <div class="content-type-grid" id="contentTypeGrid">
                        <div class="content-type-btn active" data-type="url"><i class="fas fa-globe"></i> URL</div>
                        <div class="content-type-btn" data-type="text"><i class="fas fa-font"></i> Text</div>
                        <div class="content-type-btn" data-type="image"><i class="fas fa-image"></i> Image</div>
                        <div class="content-type-btn" data-type="location"><i class="fas fa-map-pin"></i> Location</div>
                        <div class="content-type-btn" data-type="contact"><i class="fas fa-user"></i> Contact</div>
                        <div class="content-type-btn" data-type="wifi"><i class="fas fa-wifi"></i> WiFi</div>
                        <div class="content-type-btn" data-type="email"><i class="fas fa-envelope"></i> Email</div>
                        <div class="content-type-btn" data-type="sms"><i class="fas fa-comment"></i> SMS</div>
                        <div class="content-type-btn" data-type="call"><i class="fas fa-phone"></i> Call</div>
                    </div>
                    <div id="contentPanels"></div>
                    <div class="form-group">
                        <label>Internal Header (hidden in scan)</label>
                        <input type="text" class="form-control" id="pageHeader" value="My QR Content">
                    </div>
                    <div class="form-group">
                        <label>QR Name (for history)</label>
                        <input type="text" class="form-control" id="qrName" value="My QR">
                    </div>
                </div>

                <div class="card">
                    <div class="card-title"><i class="fas fa-palette"></i> Style</div>
                    <div class="style-label">GRADIENTS</div>
                    <div class="gradient-grid" id="gradientGrid">
                        <div class="style-item active" data-gradient="solid" style="background:#000;"></div>
                        <div class="style-item" data-gradient="sunset" style="background:linear-gradient(135deg,#ff6b6b,#feca57);"></div>
                        <div class="style-item" data-gradient="ocean" style="background:linear-gradient(135deg,#0066cc,#00cc99);"></div>
                        <div class="style-item" data-gradient="purple" style="background:linear-gradient(135deg,#667eea,#764ba2);"></div>
                        <div class="style-item" data-gradient="forest" style="background:linear-gradient(135deg,#11998e,#38ef7d);"></div>
                    </div>
                    <div class="style-label">TEXTURES</div>
                    <div class="texture-grid" id="textureGrid">
                        <div class="style-item active" data-texture="none" style="background:#fff;border:1px solid #ddd;"></div>
                        <div class="style-item" data-texture="dots" style="background:radial-gradient(circle,#000 20%,transparent 20%);background-size:8px 8px;"></div>
                        <div class="style-item" data-texture="lines" style="background:repeating-linear-gradient(45deg,#000 0px,#000 2px,transparent 2px,transparent 8px);"></div>
                        <div class="style-item" data-texture="grid" style="background:linear-gradient(#000 1px,transparent 1px),linear-gradient(90deg,#000 1px,transparent 1px);background-size:8px 8px;"></div>
                        <div class="style-item" data-texture="cross" style="background:repeating-linear-gradient(45deg,#000 0px,#000 2px,transparent 2px,transparent 6px),repeating-linear-gradient(-45deg,#000 0px,#000 2px,transparent 2px,transparent 6px);"></div>
                    </div>
                    <div class="color-row">
                        <div class="color-picker">
                            <label>Foreground</label>
                            <input type="color" id="fgColor" value="#000000">
                        </div>
                        <div class="color-picker">
                            <label>Background</label>
                            <input type="color" id="bgColor" value="#ffffff">
                        </div>
                    </div>
                    <div class="opacity-control">
                        <label>Opacity: <span id="opacityVal">100</span>%</label>
                        <input type="range" id="opacitySlider" min="20" max="100" value="100" style="width:100%;">
                    </div>
                </div>

                <div class="card full-width qr-preview-section">
                    <div class="qr-preview">
                        <div id="qrcodeCanvas"></div>
                    </div>
                    <div class="action-buttons">
                        <button class="btn btn-primary" id="generateBtn"><i class="fas fa-sync-alt"></i> Generate</button>
                        <button class="btn btn-secondary" id="downloadBtn"><i class="fas fa-download"></i> Download PNG</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- SCANNER PAGE -->
        <div class="page" id="scannerPage">
            <div class="card">
                <div class="card-title"><i class="fas fa-camera"></i> Scan QR Code</div>
                <div class="upload-area" id="uploadArea">
                    <i class="fas fa-cloud-upload-alt"></i>
                    <h3>Upload Image</h3>
                    <p style="color:#86868b;">or click to select</p>
                    <input type="file" id="fileInput" accept="image/*" hidden>
                </div>
                <button class="btn btn-primary" id="openCameraBtn" style="width:100%; margin-top:16px;">
                    <i class="fas fa-camera"></i> Open Camera
                </button>
            </div>
            <div class="card hidden" id="analysisContainer">
                <div class="card-title"><i class="fas fa-info-circle"></i> Scan Result</div>
                <div id="analysisDetails"></div>
                <div id="scanActions" style="margin-top:20px;"></div>
            </div>
        </div>
    </div>

    <!-- Camera Fullscreen -->
    <div class="camera-fullscreen" id="cameraFullscreen">
        <video id="cameraVideo" autoplay playsinline></video>
        <div class="scan-box"></div>
        <button class="camera-close" id="closeCameraBtn"><i class="fas fa-arrow-left"></i></button>
    </div>

    <!-- History Panel -->
    <div class="history-panel" id="historyPanel">
        <div class="history-content">
            <div class="history-header">
                <h2>History</h2>
                <button class="menu-btn" id="closeHistoryBtn"><i class="fas fa-times"></i></button>
            </div>
            <input type="text" class="history-search" id="historySearch" placeholder="Search...">
            <div id="historyList"></div>
        </div>
    </div>

    <script>
    (function(){
        "use strict";
        
        let selectedTypes = new Set(['url']);
        let currentGradient = 'solid';
        let currentTexture = 'none';
        let qrCanvas = null;
        let cameraStream = null;
        let scanInterval = null;
        let qrHistory = JSON.parse(localStorage.getItem('qrHistory') || '[]');
        let imageBase64 = null;
        
        window.copyToClipboard = function(text) {
            navigator.clipboard?.writeText(text).then(() => {
                alert('Copied!');
            }).catch(() => {
                prompt('Copy manually:', text);
            });
        };
        
        // Dark mode
        document.getElementById('darkModeToggle').addEventListener('click', () => {
            document.body.classList.toggle('dark');
            const icon = document.getElementById('darkModeToggle').querySelector('i');
            icon.classList.toggle('fa-moon');
            icon.classList.toggle('fa-sun');
        });
        
        // Menu
        document.getElementById('menuToggle').addEventListener('click', (e) => {
            e.stopPropagation();
            document.getElementById('menuDropdown').classList.toggle('active');
        });
        document.addEventListener('click', () => document.getElementById('menuDropdown').classList.remove('active'));
        
        // History
        function saveHistory(name, data, types) {
            const entry = { id: Date.now(), name, types: Array.from(types).join(', '), data, date: new Date().toLocaleString() };
            qrHistory.unshift(entry);
            if (qrHistory.length > 50) qrHistory.pop();
            localStorage.setItem('qrHistory', JSON.stringify(qrHistory));
        }
        
        function renderHistory(search = '') {
            const list = document.getElementById('historyList');
            const filtered = search ? qrHistory.filter(h => h.name.toLowerCase().includes(search.toLowerCase()) || h.date.includes(search)) : qrHistory;
            if (filtered.length === 0) {
                list.innerHTML = '<p style="color:#86868b;text-align:center;padding:40px;">No history</p>';
                return;
            }
            list.innerHTML = filtered.map(h => `
                <div class="history-item" data-history-id="${h.id}">
                    <div class="history-name">${h.name}</div>
                    <div class="history-date">${h.date} • ${h.types}</div>
                </div>
            `).join('');
            
            document.querySelectorAll('.history-item').forEach(el => {
                el.addEventListener('click', () => {
                    const id = Number(el.dataset.historyId);
                    const entry = qrHistory.find(h => h.id === id);
                    if (entry) {
                        generateQRFromData(entry.data, entry.name);
                        document.getElementById('historyPanel').classList.remove('active');
                    }
                });
            });
        }
        
        document.getElementById('historyMenuBtn').addEventListener('click', () => {
            document.getElementById('menuDropdown').classList.remove('active');
            document.getElementById('historyPanel').classList.add('active');
            renderHistory();
        });
        document.getElementById('closeHistoryBtn').addEventListener('click', () => document.getElementById('historyPanel').classList.remove('active'));
        document.getElementById('historySearch').addEventListener('input', e => renderHistory(e.target.value));
        
        // Tabs
        document.querySelectorAll('.tab').forEach(tab => {
            tab.addEventListener('click', () => {
                document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
                document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
                tab.classList.add('active');
                document.getElementById(tab.dataset.page === 'generator' ? 'generatorPage' : 'scannerPage').classList.add('active');
                if (tab.dataset.page !== 'scanner') stopCamera();
            });
        });
        
        // Content panels
        const panels = {
            url: `<div class="form-group"><label>URL</label><input class="form-control" id="urlInput" value="https://google.com"></div>`,
            text: `<div class="form-group"><label>Text</label><textarea class="form-control" id="textInput">Hello World!</textarea></div>`,
            image: `<div class="form-group"><label>Image</label><input type="file" class="form-control" id="imageInput" accept="image/*"></div>`,
            location: `<div class="form-group"><label>Location Name</label><input class="form-control" id="locName" value="New Delhi"></div><div class="row-2"><div class="form-group"><input class="form-control" id="locLat" value="28.6139"></div><div class="form-group"><input class="form-control" id="locLng" value="77.2090"></div></div>`,
            contact: `<div class="form-group"><label>Name</label><input class="form-control" id="contactName" value="Raj"></div><div class="form-group"><label>Phone</label><input class="form-control" id="contactPhone" value="+919876543210"></div><div class="form-group"><label>Email</label><input class="form-control" id="contactEmail" value="raj@example.com"></div>`,
            wifi: `<div class="form-group"><label>SSID</label><input class="form-control" id="wifiSsid" value="MyWiFi"></div><div class="form-group"><label>Password</label><input class="form-control" id="wifiPassword" value="pass123"></div>`,
            email: `<div class="form-group"><label>To</label><input class="form-control" id="emailTo" value="hello@example.com"></div><div class="form-group"><label>Subject</label><input class="form-control" id="emailSubject" value="Hello"></div><div class="form-group"><label>Body</label><textarea class="form-control" id="emailBody">Test</textarea></div>`,
            sms: `<div class="form-group"><label>Phone</label><input class="form-control" id="smsPhone" value="+919876543210"></div><div class="form-group"><label>Message</label><textarea class="form-control" id="smsBody">Hello!</textarea></div>`,
            call: `<div class="form-group"><label>Phone</label><input class="form-control" id="callPhone" value="+919876543210"></div>`
        };
        
        function updatePanels() {
            let html = '';
            for (let t of selectedTypes) if (panels[t]) html += panels[t];
            document.getElementById('contentPanels').innerHTML = html;
            document.getElementById('multiBadge').classList.toggle('hidden', selectedTypes.size <= 1);
            if (selectedTypes.has('image')) {
                document.getElementById('imageInput')?.addEventListener('change', e => {
                    const f = e.target.files[0];
                    if (f) { const r = new FileReader(); r.onload = ev => imageBase64 = ev.target.result; r.readAsDataURL(f); }
                });
            }
        }
        
        document.querySelectorAll('.content-type-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                const t = btn.dataset.type;
                if (selectedTypes.has(t)) { if (selectedTypes.size > 1) { selectedTypes.delete(t); btn.classList.remove('active'); } }
                else { selectedTypes.add(t); btn.classList.add('active'); }
                updatePanels();
            });
        });
        
        // Style
        document.querySelectorAll('[data-gradient]').forEach(el => el.addEventListener('click', function(){ 
            document.querySelectorAll('[data-gradient]').forEach(e=>e.classList.remove('active')); 
            this.classList.add('active'); 
            currentGradient=this.dataset.gradient; 
        }));
        document.querySelectorAll('[data-texture]').forEach(el => el.addEventListener('click', function(){ 
            document.querySelectorAll('[data-texture]').forEach(e=>e.classList.remove('active')); 
            this.classList.add('active'); 
            currentTexture=this.dataset.texture; 
        }));
        document.getElementById('opacitySlider').addEventListener('input', e => document.getElementById('opacityVal').textContent = e.target.value);
        
        // Data
        function getMultiData() {
            const c = { _header: document.getElementById('pageHeader').value || 'My QR' };
            if (selectedTypes.has('url')) c.url = document.getElementById('urlInput')?.value || 'https://google.com';
            if (selectedTypes.has('text')) c.text = document.getElementById('textInput')?.value || 'Hello';
            if (selectedTypes.has('image') && imageBase64) c.image = imageBase64;
            if (selectedTypes.has('location')) c.location = { name: document.getElementById('locName')?.value || 'Loc', lat: document.getElementById('locLat')?.value, lng: document.getElementById('locLng')?.value };
            if (selectedTypes.has('contact')) c.contact = { name: document.getElementById('contactName')?.value, phone: document.getElementById('contactPhone')?.value, email: document.getElementById('contactEmail')?.value };
            if (selectedTypes.has('wifi')) c.wifi = { ssid: document.getElementById('wifiSsid')?.value, password: document.getElementById('wifiPassword')?.value };
            if (selectedTypes.has('email')) c.email = { to: document.getElementById('emailTo')?.value, subject: document.getElementById('emailSubject')?.value, body: document.getElementById('emailBody')?.value };
            if (selectedTypes.has('sms')) c.sms = { phone: document.getElementById('smsPhone')?.value, body: document.getElementById('smsBody')?.value };
            if (selectedTypes.has('call')) c.call = document.getElementById('callPhone')?.value;
            return JSON.stringify(c);
        }
        
        // QR Generation (optimized for scan reliability)
        async function generateQRCode() {
            const data = getMultiData();
            const qrName = document.getElementById('qrName').value || 'My QR';
            await generateQRFromData(data, qrName);
            saveHistory(qrName, data, selectedTypes);
        }
        
        async function generateQRFromData(dataString) {
            const container = document.getElementById('qrcodeCanvas');
            container.innerHTML = '<div style="padding:40px;"><i class="fas fa-spinner fa-pulse"></i></div>';
            
            try {
                const qr = qrcode(0, 'M');
                qr.addData(dataString);
                qr.make();
                
                const moduleCount = qr.getModuleCount();
                const cellSize = 10;  // 增大模块尺寸，提高清晰度
                const size = moduleCount * cellSize;
                
                const canvas = document.createElement('canvas');
                canvas.width = size;
                canvas.height = size;
                const ctx = canvas.getContext('2d');
                ctx.imageSmoothingEnabled = false;
                
                const bgColor = document.getElementById('bgColor').value;
                const fgColor = document.getElementById('fgColor').value;
                const opacity = parseInt(document.getElementById('opacitySlider').value) / 100;
                
                // 背景
                ctx.fillStyle = bgColor;
                ctx.fillRect(0, 0, size, size);
                
                // 前景样式
                let fgStyle = fgColor;
                if (currentGradient !== 'solid') {
                    let grad;
                    if (currentGradient === 'sunset') grad = ctx.createLinearGradient(0, 0, size, size);
                    else if (currentGradient === 'ocean') grad = ctx.createLinearGradient(0, 0, size, 0);
                    else if (currentGradient === 'purple') grad = ctx.createLinearGradient(size, 0, 0, size);
                    else if (currentGradient === 'forest') grad = ctx.createRadialGradient(size/2, size/2, 0, size/2, size/2, size/2);
                    else grad = ctx.createLinearGradient(0, 0, size, size);
                    
                    if (currentGradient === 'sunset') { grad.addColorStop(0, '#ff6b6b'); grad.addColorStop(1, '#feca57'); }
                    else if (currentGradient === 'ocean') { grad.addColorStop(0, '#0066cc'); grad.addColorStop(1, '#00cc99'); }
                    else if (currentGradient === 'purple') { grad.addColorStop(0, '#667eea'); grad.addColorStop(1, '#764ba2'); }
                    else if (currentGradient === 'forest') { grad.addColorStop(0, '#11998e'); grad.addColorStop(1, '#38ef7d'); }
                    else { grad.addColorStop(0, fgColor); grad.addColorStop(1, fgColor); }
                    fgStyle = grad;
                }
                
                ctx.globalAlpha = opacity;
                
                // 绘制模块
                for (let row = 0; row < moduleCount; row++) {
                    for (let col = 0; col < moduleCount; col++) {
                        if (qr.isDark(row, col)) {
                            const x = col * cellSize;
                            const y = row * cellSize;
                            
                            if (currentTexture !== 'none') {
                                ctx.save();
                                ctx.beginPath();
                                ctx.rect(x, y, cellSize, cellSize);
                                ctx.clip();
                                ctx.fillStyle = fgStyle;
                                ctx.fillRect(x, y, cellSize, cellSize);
                                
                                ctx.fillStyle = bgColor;
                                if (currentTexture === 'dots') {
                                    for (let i=0; i<cellSize; i+=4) for (let j=0; j<cellSize; j+=4) {
                                        if ((i+j) % 8 === 0) ctx.fillRect(x+i, y+j, 2, 2);
                                    }
                                } else if (currentTexture === 'lines') {
                                    ctx.strokeStyle = bgColor;
                                    ctx.lineWidth = 1.5;
                                    for (let k=-cellSize; k<cellSize*2; k+=6) {
                                        ctx.beginPath();
                                        ctx.moveTo(x+k, y);
                                        ctx.lineTo(x+k+cellSize, y+cellSize);
                                        ctx.stroke();
                                    }
                                } else if (currentTexture === 'grid') {
                                    ctx.strokeStyle = bgColor;
                                    ctx.lineWidth = 1;
                                    for (let k=0; k<=cellSize; k+=4) {
                                        ctx.beginPath(); ctx.moveTo(x+k, y); ctx.lineTo(x+k, y+cellSize); ctx.stroke();
                                        ctx.beginPath(); ctx.moveTo(x, y+k); ctx.lineTo(x+cellSize, y+k); ctx.stroke();
                                    }
                                } else if (currentTexture === 'cross') {
                                    ctx.strokeStyle = bgColor;
                                    ctx.lineWidth = 1.8;
                                    for (let k=2; k<cellSize; k+=6) {
                                        ctx.beginPath(); ctx.moveTo(x+k, y); ctx.lineTo(x+k+4, y+cellSize); ctx.stroke();
                                        ctx.beginPath(); ctx.moveTo(x, y+k); ctx.lineTo(x+cellSize, y+k+4); ctx.stroke();
                                    }
                                }
                                ctx.restore();
                            } else {
                                ctx.fillStyle = fgStyle;
                                ctx.fillRect(x, y, cellSize, cellSize);
                            }
                        }
                    }
                }
                ctx.globalAlpha = 1.0;
                
                container.innerHTML = '';
                container.appendChild(canvas);
                qrCanvas = canvas;
            } catch(e) {
                container.innerHTML = '<p style="color:red;">Error generating QR</p>';
            }
        }
        
        function downloadQR() {
            if (qrCanvas) {
                const a = document.createElement('a');
                a.download = `qr-${Date.now()}.png`;
                a.href = qrCanvas.toDataURL('image/png');
                a.click();
            } else {
                alert('Generate QR first!');
            }
        }
        
        // Camera & Scanner
        function stopCamera() {
            if (cameraStream) { cameraStream.getTracks().forEach(t => t.stop()); cameraStream = null; }
            if (scanInterval) { clearInterval(scanInterval); scanInterval = null; }
            document.getElementById('cameraFullscreen').classList.remove('active');
        }
        
        async function openCamera() {
            document.getElementById('cameraFullscreen').classList.add('active');
            try {
                cameraStream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'environment' } });
                const video = document.getElementById('cameraVideo');
                video.srcObject = cameraStream;
                video.setAttribute('playsinline', true);
                video.play();
                scanInterval = setInterval(captureAndScan, 500);
            } catch(e) { 
                alert('Camera access denied'); 
                stopCamera(); 
            }
        }
        
        function captureAndScan() {
            const v = document.getElementById('cameraVideo');
            if (!v.srcObject || v.readyState !== 4) return;
            
            const canvas = document.createElement('canvas');
            const w = v.videoWidth, h = v.videoHeight;
            if (!w || !h) return;
            canvas.width = w; canvas.height = h;
            const ctx = canvas.getContext('2d');
            ctx.drawImage(v, 0, 0, w, h);
            
            const imageData = ctx.getImageData(0, 0, w, h);
            let code = jsQR(imageData.data, w, h);
            
            if (!code) {
                const data = imageData.data;
                for (let i=0; i<data.length; i+=4) {
                    const gray = data[i]*0.299 + data[i+1]*0.587 + data[i+2]*0.114;
                    const val = gray > 128 ? 0 : 255;
                    data[i] = data[i+1] = data[i+2] = val;
                }
                code = jsQR(data, w, h);
            }
            
            if (code) {
                stopCamera();
                processScan(code.data);
            }
        }
        
        function decodeImage(file) {
            const reader = new FileReader();
            reader.onload = e => {
                const img = new Image();
                img.onload = () => {
                    const canvas = document.createElement('canvas');
                    let w = img.width, h = img.height;
                    if (w > 1200 || h > 1200) {
                        if (w > h) { h = (h/w)*1200; w = 1200; }
                        else { w = (w/h)*1200; h = 1200; }
                    }
                    canvas.width = w; canvas.height = h;
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(img, 0, 0, w, h);
                    let imageData = ctx.getImageData(0, 0, w, h);
                    let code = jsQR(imageData.data, w, h);
                    if (!code) {
                        const d = imageData.data;
                        for (let i=0; i<d.length; i+=4) {
                            const g = d[i]*0.299 + d[i+1]*0.587 + d[i+2]*0.114;
                            d[i] = d[i+1] = d[i+2] = g>128 ? 0 : 255;
                        }
                        code = jsQR(d, w, h);
                    }
                    if (code) processScan(code.data);
                    else alert('No QR code found');
                };
                img.src = e.target.result;
            };
            reader.readAsDataURL(file);
        }
        
        function processScan(data) {
            const detailsDiv = document.getElementById('analysisDetails');
            const actionsDiv = document.getElementById('scanActions');
            actionsDiv.innerHTML = '';
            
            try {
                const c = JSON.parse(data);
                let html = '';
                // 忽略 _header
                
                if (c.url) {
                    html += `<div class="preview-card">
                        <i class="fas fa-globe"></i> <strong>URL:</strong> ${c.url}
                        <div class="action-group">
                            <button class="btn btn-primary btn-small" onclick="window.open('${c.url}')">Open</button>
                            <button class="btn btn-secondary btn-small" onclick="copyToClipboard('${c.url}')">Copy</button>
                        </div>
                    </div>`;
                }
                if (c.text) {
                    html += `<div class="preview-card">
                        <i class="fas fa-font"></i> <strong>Text:</strong> ${c.text}
                        <div class="action-group">
                            <button class="btn btn-secondary btn-small" onclick="copyToClipboard('${c.text.replace(/'/g,"\\'")}')">Copy</button>
                        </div>
                    </div>`;
                }
                if (c.image) {
                    html += `<div class="preview-card">
                        <i class="fas fa-image"></i> <strong>Image:</strong><br>
                        <img src="${c.image}" style="max-width:100%; border-radius:12px; margin-top:8px;">
                    </div>`;
                }
                if (c.location) {
                    const mapsUrl = `https://maps.google.com/?q=${c.location.lat},${c.location.lng}`;
                    html += `<div class="preview-card">
                        <i class="fas fa-map-pin"></i> <strong>${c.location.name}</strong><br>
                        ${c.location.lat}, ${c.location.lng}
                        <div class="action-group">
                            <button class="btn btn-primary btn-small" onclick="window.open('${mapsUrl}')">Maps</button>
                            <button class="btn btn-secondary btn-small" onclick="copyToClipboard('${c.location.lat},${c.location.lng}')">Coords</button>
                        </div>
                    </div>`;
                }
                if (c.contact) {
                    html += `<div class="preview-card">
                        <i class="fas fa-user"></i> <strong>${c.contact.name}</strong><br>
                        📞 ${c.contact.phone}<br> ✉️ ${c.contact.email}
                        <div class="action-group">
                            <button class="btn btn-primary btn-small" onclick="window.location.href='tel:${c.contact.phone}'">Call</button>
                            <button class="btn btn-secondary btn-small" onclick="copyToClipboard('${c.contact.phone}')">Phone</button>
                            <button class="btn btn-secondary btn-small" onclick="copyToClipboard('${c.contact.email}')">Email</button>
                        </div>
                    </div>`;
                }
                if (c.wifi) {
                    html += `<div class="preview-card">
                        <i class="fas fa-wifi"></i> <strong>${c.wifi.ssid}</strong><br>
                        Password: ${c.wifi.password}
                        <div class="action-group">
                            <button class="btn btn-secondary btn-small" onclick="copyToClipboard('${c.wifi.password}')">Copy Password</button>
                        </div>
                    </div>`;
                }
                if (c.email) {
                    const mailto = `mailto:${c.email.to}?subject=${encodeURIComponent(c.email.subject)}&body=${encodeURIComponent(c.email.body)}`;
                    html += `<div class="preview-card">
                        <i class="fas fa-envelope"></i> <strong>To:</strong> ${c.email.to}<br>
                        Subject: ${c.email.subject}
                        <div class="action-group">
                            <button class="btn btn-primary btn-small" onclick="window.location.href='${mailto}'">Compose</button>
                            <button class="btn btn-secondary btn-small" onclick="copyToClipboard('${c.email.to}')">Copy</button>
                        </div>
                    </div>`;
                }
                if (c.sms) {
                    const smsLink = `sms:${c.sms.phone}?body=${encodeURIComponent(c.sms.body)}`;
                    html += `<div class="preview-card">
                        <i class="fas fa-comment"></i> <strong>SMS to:</strong> ${c.sms.phone}<br>
                        Message: ${c.sms.body}
                        <div class="action-group">
                            <button class="btn btn-primary btn-small" onclick="window.location.href='${smsLink}'">Send SMS</button>
                            <button class="btn btn-secondary btn-small" onclick="copyToClipboard('${c.sms.body}')">Copy</button>
                        </div>
                    </div>`;
                }
                if (c.call) {
                    html += `<div class="preview-card">
                        <i class="fas fa-phone"></i> <strong>Call:</strong> ${c.call}
                        <div class="action-group">
                            <button class="btn btn-primary btn-small" onclick="window.location.href='tel:${c.call}'">Dial</button>
                            <button class="btn btn-secondary btn-small" onclick="copyToClipboard('${c.call}')">Copy</button>
                        </div>
                    </div>`;
                }
                
                if (!html) html = '<p>No content found.</p>';
                detailsDiv.innerHTML = html;
                actionsDiv.innerHTML = `<button class="btn btn-secondary" onclick="copyToClipboard('${data.replace(/'/g,"\\'")}')">Copy Raw Data</button>`;
                
            } catch(e) {
                detailsDiv.innerHTML = `<div class="preview-card">${data}</div>`;
                actionsDiv.innerHTML = `
                    <div class="action-group">
                        <button class="btn btn-primary" onclick="copyToClipboard('${data.replace(/'/g,"\\'")}')">Copy</button>
                        ${data.startsWith('http') ? `<button class="btn btn-primary" onclick="window.open('${data}')">Open</button>` : ''}
                    </div>
                `;
            }
            document.getElementById('analysisContainer').classList.remove('hidden');
        }
        
        // Event listeners
        document.getElementById('generateBtn').addEventListener('click', generateQRCode);
        document.getElementById('downloadBtn').addEventListener('click', downloadQR);
        document.getElementById('uploadArea').addEventListener('click', () => document.getElementById('fileInput').click());
        document.getElementById('fileInput').addEventListener('change', e => { if(e.target.files[0]) decodeImage(e.target.files[0]); });
        document.getElementById('openCameraBtn').addEventListener('click', openCamera);
        document.getElementById('closeCameraBtn').addEventListener('click', stopCamera);
        
        updatePanels();
        setTimeout(generateQRCode, 1000000);
    })();
    </script>
</body>
</html>