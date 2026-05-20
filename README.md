<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trash Tiger — Premium Circular Economy Dashboard</title>
    
    <meta name="description" content="A premium SaaS platform for streamlined recycling management and sustainable material procurement.">
    <meta name="theme-color" content="#10b981">
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=Plus+Jakarta+Sans:wght@500;700;800&display=swap" rel="stylesheet">
    
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <style>
        /* ==========================================================================
           1. PREMIUM DESIGN TOKENS & DESIGN SYSTEM
           ========================================================================== */
        :root {
            --font-sans: 'Inter', sans-serif;
            --font-heading: 'Plus Jakarta Sans', sans-serif;

            /* Dark Palette (Default Futuristic Silicon Valley Theme) */
            --bg-main: #060814;
            --bg-surface: rgba(13, 18, 38, 0.45);
            --bg-surface-hover: rgba(20, 28, 58, 0.65);
            --border-color: rgba(255, 255, 255, 0.06);
            --border-glow: rgba(16, 185, 129, 0.25);
            
            /* High-End Brand Gradients */
            --primary: #10b981;
            --primary-gradient: linear-gradient(135deg, #10b981 0%, #06b6d4 100%);
            --accent-glow: radial-gradient(circle at 50% 50%, rgba(16, 185, 129, 0.15), transparent 60%);
            
            /* Typography Scale */
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --text-inverse: #060814;

            /* System Feedback Statuses */
            --success: #10b981;
            --error: #ef4444;
            --warning: #fbbf24;

            /* Shadow Layers & Motion Profiles */
            --shadow-sm: 0 4px 12px rgba(0, 0, 0, 0.5);
            --shadow-md: 0 12px 32px -4px rgba(0, 0, 0, 0.7);
            --shadow-lg: 0 24px 64px -12px rgba(0, 0, 0, 0.9);
            --transition-smooth: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
            --transition-bounce: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
        }

        /* Light Theme Layer Overrides */
        [data-theme="light"] {
            --bg-main: #f4f7f6;
            --bg-surface: rgba(255, 255, 255, 0.75);
            --bg-surface-hover: rgba(240, 244, 248, 0.9);
            --border-color: rgba(15, 23, 42, 0.08);
            --border-glow: rgba(16, 185, 129, 0.15);
            --text-main: #0f172a;
            --text-muted: #62728d;
            --text-inverse: #ffffff;
            --shadow-md: 0 12px 24px -4px rgba(15, 23, 42, 0.06);
            --shadow-lg: 0 24px 48px -12px rgba(15, 23, 42, 0.12);
        }

        /* ==========================================================================
           2. GLOBAL APERIODIC STYLES & LAYOUT RESET
           ========================================================================== */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }

        body {
            font-family: var(--font-sans);
            background-color: var(--bg-main);
            color: var(--text-main);
            overflow-x: hidden;
            min-height: 100vh;
            line-height: 1.6;
            transition: background-color var(--transition-smooth);
        }

        input, select, button {
            font-family: inherit;
            color: inherit;
            background: none;
            border: none;
            outline: none;
        }

        /* Premium Minimal Scrollbars */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: var(--bg-main);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--border-color);
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: var(--primary);
        }

        /* Ambient Dynamic Light Orbs */
        .ambient-orb-1 {
            position: fixed;
            top: -20%;
            left: -10%;
            width: 70vw;
            height: 70vh;
            background: radial-gradient(circle, rgba(16, 185, 129, 0.08) 0%, transparent 65%);
            z-index: -1;
            filter: blur(120px);
            pointer-events: none;
            animation: orbitalFloat 25s ease-in-out infinite alternate;
        }

        .ambient-orb-2 {
            position: fixed;
            bottom: -20%;
            right: -10%;
            width: 60vw;
            height: 60vh;
            background: radial-gradient(circle, rgba(6, 182, 212, 0.06) 0%, transparent 70%);
            z-index: -1;
            filter: blur(140px);
            pointer-events: none;
            animation: orbitalFloat 30s ease-in-out infinite alternate-reverse;
        }

        @keyframes orbitalFloat {
            0% { transform: translate(0, 0) scale(1); }
            100% { transform: translate(6%, 8%) scale(1.15); }
        }

        /* ==========================================================================
           3. AUTHENTICATION GATE (GLASSMORPHIC SYSTEM)
           ========================================================================== */
        .auth-overlay {
            position: fixed;
            inset: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            background: rgba(3, 5, 12, 0.6);
            backdrop-filter: blur(24px);
            -webkit-backdrop-filter: blur(24px);
            transition: opacity var(--transition-smooth), transform var(--transition-smooth);
            padding: 24px;
        }

        .auth-overlay.hidden {
            opacity: 0;
            pointer-events: none;
            transform: scale(1.03);
        }

        .auth-container {
            background: var(--bg-surface);
            border: 1px solid var(--border-color);
            border-radius: 28px;
            width: 100%;
            max-width: 420px;
            padding: 48px 40px;
            box-shadow: var(--shadow-lg);
            position: relative;
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            animation: paneEntrance 0.7s cubic-bezier(0.16, 1, 0.3, 1) forwards;
        }

        @keyframes paneEntrance {
            from { opacity: 0; transform: translateY(40px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .auth-brand {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            font-family: var(--font-heading);
            font-weight: 800;
            font-size: 26px;
            letter-spacing: -0.5px;
            background: linear-gradient(135deg, #fff 0%, var(--text-muted) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 12px;
        }
        
        [data-theme="light"] .auth-brand {
            background: var(--primary-gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .auth-tagline {
            color: var(--text-muted);
            font-size: 14px;
            text-align: center;
            margin-bottom: 36px;
        }

        /* Core Inputs & Forms */
        .input-group {
            position: relative;
            margin-bottom: 24px;
        }

        .input-label {
            display: block;
            font-size: 13px;
            font-weight: 600;
            color: var(--text-muted);
            margin-bottom: 8px;
            padding-left: 2px;
        }

        .field-shell {
            position: relative;
            display: flex;
            align-items: center;
        }

        .field-shell i {
            position: absolute;
            left: 16px;
            color: var(--text-muted);
            pointer-events: none;
            transition: var(--transition-smooth);
            width: 18px;
            height: 18px;
        }

        .premium-input, .premium-select {
            width: 100%;
            padding: 14px 16px 14px 48px;
            background: rgba(0, 0, 0, 0.25);
            border: 1px solid var(--border-color);
            border-radius: 14px;
            font-size: 14px;
            color: var(--text-main);
            transition: var(--transition-smooth);
        }

        .premium-select {
            padding-left: 16px;
            appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='none' viewBox='0 0 24 24' stroke='%2394a3b8'%3E%3Cpath stroke-linecap='round' stroke-linejoin='round' stroke-width='2' d='M19 9l-7 7-7-7'/%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 16px center;
            background-size: 16px;
            cursor: pointer;
        }

        [data-theme="light"] .premium-input, [data-theme="light"] .premium-select {
            background: rgba(255, 255, 255, 0.8);
        }

        .premium-input:focus, .premium-select:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 4px var(--border-glow);
            background: rgba(0, 0, 0, 0.4);
        }

        .premium-input:focus + i {
            color: var(--primary);
        }

        /* High-End Motion Buttons */
        .btn-action {
            position: relative;
            width: 100%;
            padding: 16px;
            background: var(--primary-gradient);
            color: white;
            font-size: 14px;
            font-weight: 600;
            border-radius: 14px;
            cursor: pointer;
            overflow: hidden;
            box-shadow: 0 4px 20px rgba(16, 185, 129, 0.25);
            transition: var(--transition-bounce);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .btn-action:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 28px rgba(16, 185, 129, 0.4);
        }

        .btn-action:active {
            transform: translateY(1px);
        }

        /* ==========================================================================
           4. MAIN SaaS CONTAINER LAYOUT ARCHITECTURE
           ========================================================================== */
        .app-shell {
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            opacity: 0;
            transition: opacity 0.6s ease-in-out;
        }

        .app-shell.ready {
            opacity: 1;
        }

        /* Premium Blurry Glass Top Bar Navigation */
        .navbar {
            position: sticky;
            top: 0;
            background: rgba(6, 8, 20, 0.7);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border-bottom: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 18px 40px;
            z-index: 1000;
            transition: var(--transition-smooth);
        }

        [data-theme="light"] .navbar {
            background: rgba(244, 247, 246, 0.7);
        }

        .nav-brand {
            display: flex;
            align-items: center;
            gap: 10px;
            font-family: var(--font-heading);
            font-weight: 800;
            font-size: 20px;
            letter-spacing: -0.5px;
        }

        .nav-actions {
            display: flex;
            align-items: center;
            gap: 16px;
        }

        .icon-utility-btn {
            width: 42px;
            height: 42px;
            border-radius: 12px;
            background: var(--bg-surface);
            border: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: var(--transition-smooth);
            color: var(--text-muted);
        }

        .icon-utility-btn:hover {
            color: var(--text-main);
            background: var(--bg-surface-hover);
            transform: translateY(-2px);
        }

        .btn-install {
            background: rgba(16, 185, 129, 0.1);
            color: var(--primary);
            border: 1px solid rgba(16, 185, 129, 0.2);
            font-size: 13px;
            font-weight: 600;
            padding: 10px 18px;
            border-radius: 12px;
            cursor: pointer;
            transition: var(--transition-smooth);
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .btn-install:hover {
            background: var(--primary-gradient);
            color: white;
            transform: translateY(-2px);
        }

        /* Core Workspace Wrapper */
        .workspace-grid {
            max-width: 1300px;
            width: 100%;
            margin: 0 auto;
            padding: 40px;
            display: flex;
            flex-direction: column;
            gap: 32px;
            flex-grow: 1;
        }

        .welcome-hero {
            margin-bottom: 12px;
        }

        .welcome-hero h1 {
            font-family: var(--font-heading);
            font-size: 36px;
            font-weight: 800;
            letter-spacing: -1px;
            margin-bottom: 6px;
        }

        .welcome-hero p {
            color: var(--text-muted);
            font-size: 15px;
        }

        /* Responsive Adaptive Grid Systems */
        .responsive-panel-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 24px;
        }

        .premium-card {
            background: var(--bg-surface);
            border: 1px solid var(--border-color);
            border-radius: 24px;
            padding: 28px;
            position: relative;
            overflow: hidden;
            box-shadow: var(--shadow-sm);
            transition: var(--transition-bounce);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
        }

        .premium-card:hover {
            transform: translateY(-6px);
            box-shadow: var(--shadow-md);
            border-color: var(--border-glow);
        }

        .product-meta-header {
            display: flex;
            justify-content: space-between;
            align-items: start;
            margin-bottom: 16px;
        }

        .product-tag {
            font-size: 11px;
            text-transform: uppercase;
            letter-spacing: 1px;
            font-weight: 700;
            color: var(--primary);
            background: rgba(16, 185, 129, 0.1);
            padding: 4px 10px;
            border-radius: 20px;
        }

        .price-tag {
            font-family: var(--font-heading);
            font-weight: 700;
            font-size: 20px;
            color: var(--text-main);
        }

        /* Interactive Sections Styling Split layout */
        .split-dashboard-layout {
            display: grid;
            grid-template-columns: 1fr 1.3fr;
            gap: 24px;
        }

        @media (max-width: 1024px) {
            .split-dashboard-layout {
                grid-template-columns: 1fr;
            }
        }

        /* Modernized Execution Data Tables */
        .table-viewport {
            width: 100%;
            overflow-x: auto;
            margin-top: 16px;
        }

        .saas-table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
        }

        .saas-table th {
            padding: 14px 16px;
            font-size: 11px;
            font-weight: 600;
            color: var(--text-muted);
            text-transform: uppercase;
            border-bottom: 1px solid var(--border-color);
            letter-spacing: 0.5px;
        }

        .saas-table td {
            padding: 16px;
            font-size: 14px;
            border-bottom: 1px solid var(--border-color);
        }

        .saas-table tr:last-child td {
            border-bottom: none;
        }

        .empty-indicator {
            text-align: center;
            color: var(--text-muted);
            padding: 32px 0;
            font-size: 14px;
        }

        .revenue-badge {
            margin-top: 24px;
            padding: 16px 20px;
            border-radius: 16px;
            background: rgba(16, 185, 129, 0.08);
            border: 1px solid rgba(16, 185, 129, 0.15);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* ==========================================================================
           5. SYSTEM NOTIFICATION FEEDS (TOAST PIPELINES)
           ========================================================================== */
        #notification-center {
            position: fixed;
            bottom: 32px;
            right: 32px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            z-index: 3000;
        }

        .live-toast {
            background: rgba(13, 18, 38, 0.85);
            border: 1px solid var(--border-color);
            padding: 16px 24px;
            border-radius: 16px;
            box-shadow: var(--shadow-lg);
            display: flex;
            align-items: center;
            gap: 12px;
            min-width: 320px;
            transform: translateX(130%);
            transition: var(--transition-bounce);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
        }

        .live-toast.visible {
            transform: translateX(0);
        }

        .toast-status-icon.success { color: var(--success); }
        .toast-status-icon.error { color: var(--error); }

        /* ==========================================================================
           6. COMPLIANT ENTERPRISE FOOTER SPECIFICATION
           ========================================================================== */
        .saas-footer {
            margin-top: auto;
            text-align: center;
            font-size: 13px;
            color: var(--text-muted);
            padding: 32px 40px;
            border-top: 1px solid var(--border-color);
            backdrop-filter: blur(8px);
        }

        .saas-footer span {
            font-weight: 600;
            background: var(--primary-gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* Responsive Breakpoint Adaptability */
        @media (max-width: 768px) {
            .navbar { padding: 16px 24px; }
            .workspace-grid { padding: 24px; gap: 24px; }
            .welcome-hero h1 { font-size: 28px; }
        }
    </style>
</head>
<body>

    <div class="ambient-orb-1"></div>
    <div class="ambient-orb-2"></div>

    <div id="notification-center"></div>

    <div class="auth-overlay" id="authGate">
        <div class="auth-container">
            <div class="auth-brand">
                <i data-lucide="aperture" style="width: 28px; height: 28px; color: var(--primary);"></i> Trash Tiger
            </div>
            <p class="auth-tagline">Access the premium circular economy dashboard</p>

            <form id="gateForm" onsubmit="executeSessionAuth(event)">
                <div class="input-group">
                    <label class="input-label">Operator Key Identifier</label>
                    <div class="field-shell">
                        <input type="text" id="operatorName" class="premium-input" placeholder="Enter name or identity..." required>
                        <i data-lucide="shield"></i>
                    </div>
                </div>

                <button type="submit" class="btn-action">
                    <span>Initialize Interface</span>
                    <i data-lucide="arrow-right" style="width: 16px; height: 16px;"></i>
                </button>
            </form>
        </div>
    </div>

    <div class="app-shell" id="appInterface">
        
        <nav class="navbar">
            <div class="nav-brand">
                <i data-lucide="aperture" style="width: 24px; height: 24px; color: var(--primary);"></i>
                <span>Trash Tiger</span>
            </div>
            <div class="nav-actions">
                <button id="pwaInstallBtn" class="btn-install" style="display: none;">
                    <i data-lucide="download"></i> Install Hub App
                </button>
                <button class="icon-utility-btn" onclick="toggleColorPalette()" title="Shift Visual Spectrum">
                    <i data-lucide="sun" id="paletteIcon"></i>
                </button>
                <button class="icon-utility-btn" onclick="terminateSession()" title="Revoke Authentication Session">
                    <i data-lucide="log-out"></i>
                </button>
            </div>
        </nav>

        <main class="workspace-grid">
            
            <header class="welcome-hero">
                <h1 id="userGreeting">System Environment Overview</h1>
                <p>Track metrics, analyze outputs, and order verified circular ecosystem commodities seamlessly.</p>
            </header>

            <section>
                <h2 style="font-family: var(--font-heading); font-size: 20px; margin-bottom: 20px;">Premium Commodities</h2>
                <div id="productGridPipeline" class="responsive-panel-grid"></div>
            </section>

            <div class="split-dashboard-layout">
                
                <section class="premium-card" style="height: fit-content;">
                    <h3 style="font-family: var(--font-heading); font-size: 18px; margin-bottom: 20px;">Procure Allocation</h3>
                    <form onsubmit="handleOrderSubmission(event)">
                        <div class="input-group">
                            <label class="input-label">Select Resource Node</label>
                            <select id="orderProductSelect" class="premium-select"></select>
                        </div>
                        <div class="input-group">
                            <label class="input-label">Target Quantity (Units)</label>
                            <div class="field-shell">
                                <input type="number" id="orderQuantity" class="premium-input" style="padding-left: 16px;" placeholder="0" min="1" required>
                            </div>
                        </div>
                        <button type="submit" class="btn-action">
                            <i data-lucide="shopping-bag" style="width: 16px; height: 16px;"></i>
                            <span>Transmit via WhatsApp</span>
                        </button>
                    </form>
                </section>

                <section class="premium-card">
                    <h3 style="font-family: var(--font-heading); font-size: 18px;">Telemetry Stream Logs</h3>
                    
                    <div class="table-viewport">
                        <table class="saas-table">
                            <thead>
                                <tr>
                                    <th>Commodity</th>
                                    <th>Volume</th>
                                    <th>Value Estimation</th>
                                </tr>
                            </thead>
                            <tbody id="telemetryStreamBody"></tbody>
                        </table>
                    </div>

                    <div class="revenue-badge">
                        <span style="font-size: 14px; color: var(--text-muted); font-weight: 500;">Aggregated Allocation Stream</span>
                        <span id="revenueMetricDisplay" style="font-family: var(--font-heading); font-weight: 800; font-size: 18px; color: var(--primary);">GHS 0.00</span>
                    </div>
                </section>

            </div>

        </main>

        <footer class="saas-footer">
            <p>Trash Tiger Logistics Core Systems Enterprise &copy; 2026. Powered by <span>Suleiman Alhassan.</span></p>
        </footer>

    </div>

    <script>
        // High-Quality Global Products Pipeline State Configuration
        const productMatrix = [
            { id: "egg_trays", name: "Premium Egg Trays", price: 15 },
            { id: "paper", name: "Recycled Industrial Paper", price: 10 },
            { id: "toilet_paper", name: "Eco Fine Toilet Paper", price: 8 },
            { id: "paint", name: "Refined Reclaimed Paint", price: 25 }
        ];

        // Core App Context Instantiation Object
        let sessionOperator = null;

        document.addEventListener("DOMContentLoaded", () => {
            lucide.createIcons();
            verifyActiveSessionLog();
        });

        /**
         * Validates operational session validation layers
         */
        function verifyActiveSessionLog() {
            const currentSession = localStorage.getItem("trash_tiger_session");
            if (currentSession) {
                sessionOperator = currentSession;
                bootDashboardShell();
            }
        }

        /**
         * Transitions UI state gracefully into operational hub viewport
         */
        function executeSessionAuth(event) {
            event.preventDefault();
            const operatorVal = document.getElementById("operatorName").value.trim();
            if (!operatorVal) return;

            localStorage.setItem("trash_tiger_session", operatorVal);
            sessionOperator = operatorVal;
            
            dispatchSystemToast("Identity verification metrics validated successfully.", "success");
            bootDashboardShell();
        }

        /**
         * Initializes structural layout components inside active dashboard shell
         */
        function bootDashboardShell() {
            document.getElementById("authGate").classList.add("hidden");
            const mainInterface = document.getElementById("appInterface");
            mainInterface.classList.add("ready");

            document.getElementById("userGreeting").innerText = `Welcome back, ${sessionOperator}`;
            
            renderProductCatalog();
            rehydrateTelemetryStream();
        }

        /**
         * Revokes active authorization matrix tokens and triggers environment view resets
         */
        function terminateSession() {
            localStorage.removeItem("trash_tiger_session");
            sessionOperator = null;
            
            document.getElementById("authGate").classList.remove("hidden");
            document.getElementById("appInterface").classList.remove("ready");
            dispatchSystemToast("Secure session lifecycle completed.", "success");
        }

        /**
         * Compiles and outputs micro-interaction nodes safely inside semantic components
         */
        function renderProductCatalog() {
            const gridContainer = document.getElementById("productGridPipeline");
            const selectDropdown = document.getElementById("orderProductSelect");
            
            let gridBuffer = "";
            let selectBuffer = "";

            productMatrix.forEach(item => {
                gridBuffer += `
                    <div class="premium-card">
                        <div class="product-meta-header">
                            <span class="product-tag">Verified Resource</span>
                            <span class="price-tag">GHS ${item.price}</span>
                        </div>
                        <h4 style="font-family: var(--font-heading); font-size: 16px; font-weight:700;">${item.name}</h4>
                        <p style="color: var(--text-muted); font-size: 12px; margin-top:
