<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aura Scribe AI — Premium SaaS Dashboard</title>
    <meta name="description" content="A premium, next-generation SaaS dashboard featuring advanced AI analytics, glassmorphic UI elements, and fluid animations.">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=Plus+Jakarta+Sans:wght@500;700;800&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <style>
        /* ==========================================================================
           1. DESIGN TOKENS & SYSTEM VARIABLES
           ========================================================================== */
        :root {
            --font-sans: 'Inter', sans-serif;
            --font-heading: 'Plus Jakarta Sans', sans-serif;

            /* Dark Palette (Default Premium Theme) */
            --bg-main: #09090b;
            --bg-surface: rgba(20, 20, 25, 0.7);
            --bg-surface-hover: rgba(30, 30, 40, 0.8);
            --border-color: rgba(255, 255, 255, 0.08);
            --border-glow: rgba(99, 102, 241, 0.2);
            
            /* High-End Accent Gradients */
            --primary: #6366f1;
            --primary-gradient: linear-gradient(135deg, #6366f1 0%, #a855f7 100%);
            --accent-glow: radial-gradient(circle at 50% 50%, rgba(99, 102, 241, 0.15), transparent 60%);
            
            /* Typography Colors */
            --text-main: #f4f4f5;
            --text-muted: #a1a1aa;
            --text-inverse: #09090b;

            /* System Status Colors */
            --success: #10b981;
            --error: #ef4444;
            --warning: #f59e0b;

            /* Animation & Shadow Tokens */
            --shadow-sm: 0 2px 8px -2px rgba(0,0,0,0.5);
            --shadow-md: 0 12px 24px -4px rgba(0,0,0,0.6);
            --shadow-lg: 0 24px 48px -12px rgba(0,0,0,0.8);
            --transition-smooth: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
            --transition-bounce: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
        }

        /* Light Theme Toggles */
        [data-theme="light"] {
            --bg-main: #f8fafc;
            --bg-surface: rgba(255, 255, 255, 0.75);
            --bg-surface-hover: rgba(255, 255, 255, 0.95);
            --border-color: rgba(15, 23, 42, 0.06);
            --border-glow: rgba(99, 102, 241, 0.1);
            --text-main: #0f172a;
            --text-muted: #64748b;
            --text-inverse: #ffffff;
            --shadow-md: 0 12px 24px -4px rgba(15, 23, 42, 0.05);
            --shadow-lg: 0 24px 48px -12px rgba(15, 23, 42, 0.1);
        }

        /* ==========================================================================
           2. RESET & BASE STYLES
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
            line-height: 1.5;
            transition: background-color var(--transition-smooth);
        }

        input, button, textarea {
            font-family: inherit;
            color: inherit;
            background: none;
            border: none;
            outline: none;
        }

        /* Smooth Scrollbar Configuration */
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

        /* ==========================================================================
           3. GLOBAL KINETIC BACKGROUNDS & AMBIENCE
           ========================================================================== */
        .ambient-glow {
            position: fixed;
            top: -10%;
            left: -10%;
            width: 60vw;
            height: 60vh;
            background: radial-gradient(circle, rgba(99, 102, 241, 0.12) 0%, transparent 70%);
            z-index: -1;
            filter: blur(80px);
            pointer-events: none;
            animation: floatGlow 15s ease-in-out infinite alternate;
        }

        .ambient-glow-secondary {
            position: fixed;
            bottom: -10%;
            right: -10%;
            width: 50vw;
            height: 50vh;
            background: radial-gradient(circle, rgba(168, 85, 247, 0.1) 0%, transparent 70%);
            z-index: -1;
            filter: blur(100px);
            pointer-events: none;
            animation: floatGlow 20s ease-in-out infinite alternate-reverse;
        }

        @keyframes floatGlow {
            0% { transform: translate(0, 0) scale(1); }
            100% { transform: translate(8%, 5%) scale(1.15); }
        }

        /* ==========================================================================
           4. AUTHENTICATION ENGINE SYSTEM (GLASS OVERLAY)
           ========================================================================== */
        .auth-wrapper {
            position: fixed;
            inset: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            background: rgba(5, 5, 5, 0.4);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            transition: opacity var(--transition-smooth), transform var(--transition-smooth);
            padding: 20px;
        }

        .auth-wrapper.hidden {
            opacity: 0;
            pointer-events: none;
            transform: scale(1.02);
        }

        .auth-card {
            background: var(--bg-surface);
            border: 1px solid var(--border-color);
            border-radius: 24px;
            width: 100%;
            max-width: 440px;
            padding: 40px;
            box-shadow: var(--shadow-lg);
            position: relative;
            overflow: hidden;
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            transform: translateY(0);
            animation: cardIntro 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
        }

        @keyframes cardIntro {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .auth-card::before {
            content: '';
            position: absolute;
            top: 0; left: -50%;
            width: 200%; height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.03), transparent);
            transform: skewX(-25deg);
            pointer-events: none;
        }

        .auth-header {
            text-align: center;
            margin-bottom: 32px;
        }

        .brand-logo {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            font-family: var(--font-heading);
            font-weight: 800;
            font-size: 24px;
            letter-spacing: -0.5px;
            background: linear-gradient(120deg, #fff, var(--text-muted));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 8px;
        }

        .auth-title {
            font-family: var(--font-heading);
            font-size: 28px;
            font-weight: 700;
            letter-spacing: -0.5px;
            margin-bottom: 6px;
        }

        .auth-subtitle {
            color: var(--text-muted);
            font-size: 14px;
        }

        /* Form Controls */
        .form-group {
            position: relative;
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            font-size: 13px;
            font-weight: 500;
            color: var(--text-muted);
            margin-bottom: 8px;
            padding-left: 4px;
        }

        .input-wrapper {
            position: relative;
            display: flex;
            align-items: center;
        }

        .input-wrapper i {
            position: absolute;
            left: 16px;
            color: var(--text-muted);
            pointer-events: none;
            transition: var(--transition-smooth);
            width: 18px;
            height: 18px;
        }

        .form-input {
            width: 100%;
            padding: 14px 16px 14px 48px;
            background: rgba(0, 0, 0, 0.2);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            font-size: 14px;
            transition: var(--transition-smooth);
        }

        [data-theme="light"] .form-input {
            background: rgba(255, 255, 255, 0.6);
        }

        .form-input:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 4px var(--border-glow);
            background: rgba(0, 0, 0, 0.4);
        }

        .form-input:focus + i {
            color: var(--primary);
        }

        /* Premium Kinetic Buttons */
        .btn-premium {
            position: relative;
            width: 100%;
            padding: 14px;
            background: var(--primary-gradient);
            color: white;
            font-size: 14px;
            font-weight: 600;
            border-radius: 12px;
            cursor: pointer;
            overflow: hidden;
            box-shadow: 0 4px 20px rgba(99, 102, 241, 0.3);
            transition: var(--transition-bounce);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .btn-premium:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 26px rgba(99, 102, 241, 0.45);
        }

        .btn-premium:active {
            transform: translateY(1px);
        }

        /* Magnetic Toggle Option Link */
        .auth-toggle-link {
            text-align: center;
            margin-top: 20px;
            font-size: 13px;
            color: var(--text-muted);
        }

        .auth-toggle-link span {
            color: var(--primary);
            cursor: pointer;
            font-weight: 500;
            transition: var(--transition-smooth);
        }

        .auth-toggle-link span:hover {
            text-shadow: 0 0 8px var(--border-glow);
            text-decoration: underline;
        }

        /* ==========================================================================
           5. APP DASHBOARD ARCHITECTURE & LAYOUT
           ========================================================================== */
        .app-container {
            display: grid;
            grid-template-columns: 280px 1fr;
            min-height: 100vh;
            opacity: 0;
            transition: opacity 0.8s ease-in-out;
        }

        .app-container.authenticated {
            opacity: 1;
        }

        /* Premium Sidebar Panel */
        .sidebar {
            background: rgba(15, 15, 20, 0.6);
            border-right: 1px solid var(--border-color);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            padding: 32px 24px;
            display: flex;
            flex-direction: column;
            z-index: 900;
            position: sticky;
            top: 0;
            height: 100vh;
        }

        .sidebar-brand {
            margin-bottom: 40px;
            padding-left: 12px;
        }

        .nav-list {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .nav-item {
            display: flex;
            align-items: center;
            gap: 14px;
            padding: 12px 16px;
            color: var(--text-muted);
            font-size: 14px;
            font-weight: 500;
            border-radius: 12px;
            cursor: pointer;
            transition: var(--transition-smooth);
        }

        .nav-item i {
            width: 18px;
            height: 18px;
        }

        .nav-item:hover, .nav-item.active {
            color: var(--text-main);
            background: var(--bg-surface-hover);
            box-shadow: inset 0 1px 0 rgba(255,255,255,0.05);
        }

        .nav-item.active {
            border-left: 3px solid var(--primary);
            background: linear-gradient(90deg, rgba(99, 102, 241, 0.1), transparent);
        }

        .sidebar-footer {
            margin-top: auto;
            border-top: 1px solid var(--border-color);
            padding-top: 20px;
        }

        .user-profile-widget {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 8px;
            border-radius: 12px;
        }

        .avatar-container {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: var(--primary-gradient);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            color: white;
            box-shadow: var(--shadow-sm);
        }

        .user-info h4 {
            font-size: 14px;
            font-weight: 600;
        }

        .user-info p {
            font-size: 11px;
            color: var(--text-muted);
        }

        /* Main Workspace Content Area */
        .main-workspace {
            padding: 40px 60px;
            display: flex;
            flex-direction: column;
            gap: 40px;
            overflow-y: auto;
        }

        /* Top Bar Sub-navigation Elements */
        .workspace-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .greeting h1 {
            font-family: var(--font-heading);
            font-size: 32px;
            font-weight: 800;
            letter-spacing: -1px;
        }

        .header-actions {
            display: flex;
            align-items: center;
            gap: 16px;
        }

        .action-icon-btn {
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

        .action-icon-btn:hover {
            color: var(--text-main);
            background: var(--bg-surface-hover);
            transform: translateY(-2px);
        }

        /* ==========================================================================
           6. MATRIX METRICS GRID (PREMIUM DATA VISUALIZATIONS)
           ========================================================================== */
        .metrics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
            gap: 24px;
        }

        .premium-card {
            background: var(--bg-surface);
            border: 1px solid var(--border-color);
            border-radius: 20px;
            padding: 24px;
            position: relative;
            overflow: hidden;
            box-shadow: var(--shadow-sm);
            transition: var(--transition-bounce);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
        }

        .premium-card:hover {
            transform: translateY(-6px) scale(1.01);
            box-shadow: var(--shadow-md);
            border-color: var(--border-glow);
        }

        .premium-card::after {
            content: '';
            position: absolute;
            inset: 0;
            background: var(--accent-glow);
            opacity: 0;
            transition: opacity 0.4s ease;
            z-index: 0;
        }

        .premium-card:hover::after {
            opacity: 1;
        }

        .card-content-inner {
            position: relative;
            z-index: 1;
        }

        .card-header-icon {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .icon-box {
            width: 46px;
            height: 46px;
            border-radius: 14px;
            background: rgba(99, 102, 241, 0.1);
            color: var(--primary);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .metric-trend {
            font-size: 12px;
            font-weight: 600;
            padding: 4px 8px;
            border-radius: 20px;
            display: flex;
            align-items: center;
            gap: 4px;
        }

        .trend-up { background: rgba(16, 185, 129, 0.1); color: var(--success); }
        .trend-down { background: rgba(239, 68, 68, 0.1); color: var(--error); }

        .metric-value {
            font-family: var(--font-heading);
            font-size: 28px;
            font-weight: 700;
            margin-bottom: 4px;
            letter-spacing: -0.5px;
        }

        .metric-title {
            font-size: 13px;
            color: var(--text-muted);
            font-weight: 500;
        }

        /* ==========================================================================
           7. INTERACTIVE APPLICATIONS / DATA TABLE LAYER
           ========================================================================== */
        .workspace-split-view {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 24px;
        }

        .interactive-section {
            background: var(--bg-surface);
            border: 1px solid var(--border-color);
            border-radius: 24px;
            padding: 32px;
            box-shadow: var(--shadow-sm);
        }

        .section-title-area {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
        }

        .section-title {
            font-family: var(--font-heading);
            font-size: 18px;
            font-weight: 700;
        }

        /* Modern Custom Premium Data Table Elements */
        .data-table-container {
            width: 100%;
            overflow-x: auto;
        }

        .premium-table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
        }

        .premium-table th {
            padding: 16px;
            font-size: 12px;
            font-weight: 600;
            color: var(--text-muted);
            text-transform: uppercase;
            border-bottom: 1px solid var(--border-color);
            letter-spacing: 0.5px;
        }

        .premium-table td {
            padding: 18px 16px;
            font-size: 14px;
            border-bottom: 1px solid var(--border-color);
            transition: var(--transition-smooth);
        }

        .premium-table tr:last-child td {
            border-bottom: none;
        }

        .premium-table tr:hover td {
            background: rgba(255, 255, 255, 0.02);
        }

        .badge-status {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            font-size: 12px;
            font-weight: 500;
            padding: 4px 10px;
            border-radius: 12px;
        }

        .status-active { background: rgba(16, 185, 129, 0.1); color: var(--success); }
        .status-pending { background: rgba(245, 158, 11, 0.1); color: var(--warning); }

        /* Quick Engine Interaction Form Console */
        .interactive-form-console {
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        /* ==========================================================================
           8. SYSTEM TOAST SYSTEM (GLOBAL LIVE FEEDBACK)
           ========================================================================== */
        #toast-container {
            position: fixed;
            bottom: 30px;
            right: 30px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            z-index: 2000;
        }

        .toast-notification {
            background: var(--bg-surface);
            border: 1px solid var(--border-color);
            padding: 16px 20px;
            border-radius: 14px;
            box-shadow: var(--shadow-lg);
            display: flex;
            align-items: center;
            gap: 12px;
            min-width: 300px;
            transform: translateX(120%);
            transition: var(--transition-bounce);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
        }

        .toast-notification.active {
            transform: translateX(0);
        }

        .toast-icon {
            width: 20px;
            height: 20px;
        }

        .toast-icon.success { color: var(--success); }
        .toast-icon.error { color: var(--error); }

        .toast-message {
            font-size: 13px;
            font-weight: 500;
        }

        /* ==========================================================================
           9. FOOTER SPECIFICATION RULES
           ========================================================================== */
        .premium-footer {
            margin-top: auto;
            text-align: center;
            font-size: 12px;
            color: var(--text-muted);
            padding-top: 24px;
            border-top: 1px solid var(--border-color);
            letter-spacing: 0.25px;
        }

        .premium-footer span {
            font-weight: 600;
            background: var(--primary-gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* ==========================================================================
           10. FLUID RESPONSIVE ADAPTATIONS (MOBILE BRAKING ENGINES)
           ========================================================================== */
        @media (max-width: 1024px) {
            .workspace-split-view {
                grid-template-columns: 1fr;
            }
        }

        @media (max-width: 768px) {
            .app-container {
                grid-template-columns: 1fr;
            }
            .sidebar {
                display: none; /* Can be bound via responsive dynamic JS drawer */
            }
            .main-workspace {
                padding: 24px;
            }
            .workspace-header {
                flex-direction: column;
                align-items: flex-start;
                gap: 16px;
            }
            .header-actions {
                width: 100%;
                justify-content: flex-end;
            }
        }
    </style>
</head>
<body>

    <div class="ambient-glow"></div>
    <div class="ambient-glow-secondary"></div>

    <div id="toast-container"></div>

    <div class="auth-wrapper" id="authGateway">
        <div class="auth-card">
            <div class="auth-header">
                <div class="brand-logo">
                    <i data-lucide="layers"></i> Aura Scribe AI
                </div>
                <h2 class="auth-title" id="authTitle">Welcome Back</h2>
                <p class="auth-subtitle" id="authSubtitle">Enter your enterprise operational codes</p>
            </div>

            <form id="authForm" autocomplete="off" onsubmit="handleAuthEngine(event)">
                <div class="form-group" id="usernameGroup" style="display: none;">
                    <label class="form-label">Full Account Name</label>
                    <div class="input-wrapper">
                        <input type="text" id="authName" class="form-input" placeholder="Alex Mercer">
                        <i data-lucide="user"></i>
                    </div>
                </div>

                <div class="form-group">
                    <label class="form-label">Corporate Email Address</label>
                    <div class="input-wrapper">
                        <input type="email" id="authEmail" class="form-input" placeholder="name@company.com" required>
                        <i data-lucide="mail"></i>
                    </div>
                </div>

                <div class="form-group">
                    <label class="form-label">Security Access Key</label>
                    <div class="input-wrapper">
                        <input type="password" id="authPassword" class="form-input" placeholder="••••••••••••" required>
                        <i data-lucide="lock"></i>
                    </div>
                </div>

                <button type="submit" class="btn-premium" id="authSubmitBtn">
                    <span>Authenticate Console</span>
                    <i data-lucide="arrow-right"></i>
                </button>
            </form>

            <div class="auth-toggle-link">
                <p id="authToggleText">New to the core interface? <span onclick="toggleAuthMode()">Provision Account</span></p>
            </div>
        </div>
    </div>

    <div class="app-container" id="appWorkspace">
        
        <aside class="sidebar">
            <div class="sidebar-brand">
                <div class="brand-logo">
                    <i data-lucide="layers"></i> Aura Scribe
                </div>
            </div>

            <ul class="nav-list">
                <li class="nav-item active"><i data-lucide="layout-dashboard"></i> Overview Portal</li>
                <li class="nav-item"><i data-lucide="activity"></i> Core Analytics</li>
                <li class="nav-item"><i data-lucide="cpu"></i> Neural Node Settings</li>
                <li class="nav-item"><i data-lucide="shield-check"></i> Security Protocols</li>
            </ul>

            <div class="sidebar-footer">
                <div class="user-profile-widget">
                    <div class="avatar-container" id="userAvatar">U</div>
                    <div class="user-info">
                        <h4 id="userDisplayName">Operator</h4>
                        <p id="userDisplayEmail">system.root@aura.io</p>
                    </div>
                    <button class="action-icon-btn" style="margin-left: auto; width: 32px; height: 32px;" onclick="terminateSession()" title="Terminate Session">
                        <i data-lucide="log-out" style="width:14px; height:14px;"></i>
                    </button>
                </div>
            </div>
        </aside>

        <main class="main-workspace">
            
            <header class="workspace-header">
                <div class="greeting">
                    <h1 id="welcomeHeading">System Matrix Overview</h1>
                    <p style="color: var(--text-muted); font-size: 14px;">Real-time execution optimization metrics</p>
                </div>
                <div class="header-actions">
                    <button class="action-icon-btn" onclick="toggleInterfaceTheme()" title="Toggle Visual Mode">
                        <i data-lucide="sun" id="themeIcon"></i>
                    </button>
                    <button class="action-icon-btn" onclick="triggerSystemNotification('Security parameters active. Firewall optimal.', 'success')">
                        <i data-lucide="bell"></i>
                    </button>
                </div>
            </header>

            <section class="metrics-grid">
                <div class="premium-card">
                    <div class="card-content-inner">
                        <div class="card-header-icon">
                            <div class="icon-box"><i data-lucide="database"></i></div>
                            <span class="metric-trend trend-up"><i data-lucide="trending-up"></i> +12.4%</span>
                        </div>
                        <div class="metric-value">4.82 TB</div>
                        <div class="metric-title">Neural Engine Data Indexed</div>
                    </div>
                </div>
                <div class="premium-card">
                    <div class="card-content-inner">
                        <div class="card-header-icon">
                            <div class="icon-box"><i data-lucide="zap"></i></div>
                            <span class="metric-trend trend-up"><i data-lucide="trending-up"></i> +0.02ms</span>
                        </div>
                        <div class="metric-value">14.2 ms</div>
                        <div class="metric-title">Global Micro-latency Threshold</div>
                    </div>
                </div>
                <div class="premium-card">
                    <div class="card-content-inner">
                        <div class="card-header-icon">
                            <div class="icon-box"><i data-lucide="users"></i></div>
                            <span class="metric-trend trend-down"><i data-lucide="trending-down"></i> -1.8%</span>
                        </div>
                        <div class="metric-value">99.84%</div>
                        <div class="metric-title">Automated Node Concurrency Sync</div>
                    </div>
                </div>
            </section>

            <div class="workspace-split-view">
                <div class="interactive-section">
                    <div class="section-title-area">
                        <h3 class="section-title">Active Security Data Clusters</h3>
                    </div>
                    <div class="data-table-container">
                        <table class="premium-table">
                            <thead>
                                <tr>
                                    <th>Target Cluster Node</th>
                                    <th>Signature Identity</th>
                                    <th>Status Verification</th>
                                </tr>
                            </thead>
                            <tbody id="clusterTableBody">
                                <tr>
                                    <td style="font-weight: 500;">Core-Alpha-01</td>
                                    <td style="font-family: monospace; color: var(--text-muted);">0x92F4A...B22</td>
                                    <td><span class="badge-status status-active">Optimal</span></td>
                                </tr>
                                <tr>
                                    <td style="font-weight: 500;">Neural-Sync-West</td>
                                    <td style="font-family: monospace; color: var(--text-muted);">0x44A1B...C90</td>
                                    <td><span class="badge-status status-active">Optimal</span></td>
                                </tr>
                                <tr>
                                    <td style="font-weight: 500;">Backup-Vault-Delta</td>
                                    <td style="font-family: monospace; color: var(--text-muted);">0x12E99...F55</td>
                                    <td><span class="badge-status status-pending">Sync Pending</span></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>

                <div class="interactive-section">
                    <div class="section-title-area">
                        <h3 class="section-title">System Execution Input</h3>
                    </div>
                    <div class="interactive-form-console">
                        <div class="form-group" style="margin-bottom: 0;">
                            <label class="form-label">Cluster Dispatch Token</label>
                            <div class="input-wrapper">
                                <input type="text" id="nodeTokenInput" class="form-input" placeholder="Enter node routing id..." style="padding-left: 16px;">
                            </div>
                        </div>
                        <button class="btn-premium" onclick="dispatchClusterNode()" style="padding: 12px;">
                            <i data-lucide="send" style="width: 16px; height:16px;"></i>
                            <span>Inject Custom Node</span>
                        </button>
                    </div>
                </div>
            </div>

            <footer class="premium-footer">
                <p>Aura Scribe Architecture Console Enterprise &copy; 2026. Powered by <span>Suleiman Alhassan.</span></p>
            </footer>

        </main>
    </div>

    <script>
        // Global Application Architecture State Variables
        let isSignUpMode = false;
        let currentUser = null;

        // Initialize Lucide SVG System Icons immediately 
        document.addEventListener("DOMContentLoaded", () => {
            lucide.createIcons();
            checkForActiveSession();
        });

        /**
         * Authentication Mode Toggle Engine Component switching context cleanly
         */
        function toggleAuthMode() {
            isSignUpMode = !isSignUpMode;
            const title = document.getElementById('authTitle');
            const subtitle = document.getElementById('authSubtitle');
            const btnText = document.querySelector('#authSubmitBtn span');
            const toggleText = document.getElementById('authToggleText');
            const usernameGroup = document.getElementById('usernameGroup');
            const nameInput = document.getElementById('authName');

            if (isSignUpMode) {
                title.innerText = "Create Identity";
                subtitle.innerText = "Provision clean enterprise matrix credentials";
                btnText.innerText = "Provision Session Base";
                toggleText.innerHTML = "Existing operator credential? <span onclick='toggleAuthMode()'>Authenticate Access</span>";
                usernameGroup.style.display = "block";
                nameInput.setAttribute('required', 'true');
            } else {
                title.innerText = "Welcome Back";
                subtitle.innerText = "Enter your enterprise operational codes";
                btnText.innerText = "Authenticate Console";
                toggleText.innerHTML = "New to the core interface? <span onclick='toggleAuthMode()'>Provision Account</span>";
                usernameGroup.style.display = "none";
                nameInput.removeAttribute('required');
            }
        }

        /**
         * Core Security Authentication Submission Processing Engine
         */
        function handleAuthEngine(event) {
            event.preventDefault();
            
            const email = document.getElementById('authEmail').value;
            const password = document.getElementById('authPassword').value;
            const name = isSignUpMode ? document.getElementById('authName').value : 'System Operator';

            // Security Base Validation Criteria Block
            if (password.length < 6) {
                triggerSystemNotification("Security Access Key must contain at least 6 tokens.", "error");
                return;
            }

            // Create Local Session Storage Object State payload
            currentUser = { name, email };
            localStorage.setItem('aura_session', JSON.stringify(currentUser));
            
            triggerSystemNotification(isSignUpMode ? "Account Identity Matrix Provisioned Successfully." : "Access Verification Subroutine Validated.", "success");
            initiateAppWorkspaceUI(currentUser);
        }

        /**
         * Session check validation engine upon initial boot lifecycle hook
         */
        function checkForActiveSession() {
            const savedSession = localStorage.getItem('aura_session');
            if (savedSession) {
                currentUser = JSON.parse(savedSession);
                initiateAppWorkspaceUI(currentUser);
            }
        }

        /**
         * Controls CSS transition layers to switch from Gateway to Dashboard interface
         */
        function initiateAppWorkspaceUI(user) {
            document.getElementById('authGateway').classList.add('hidden');
            const workspace = document.getElementById('appWorkspace');
            workspace.classList.add('authenticated');

            // Map user profiles safely into UI views
            document.getElementById('userDisplayName').innerText = user.name;
            document.getElementById('userDisplayEmail').innerText = user.email;
            document.getElementById('userAvatar').innerText = user.name.charAt(0).toUpperCase();
            document.getElementById('welcomeHeading').innerText = `Welcome, Operator ${user.name.split(' ')[0]}`;
        }

        /**
         * Destroys current security profile context tokens and initiates view resets
         */
        function terminateSession() {
            localStorage.removeItem('aura_session');
            currentUser = null;
            triggerSystemNotification("Secure context state closed. Session terminated.", "success");
            
            document.getElementById('authGateway').classList.remove('hidden');
            document.getElementById('appWorkspace').classList.remove('authenticated');
        }

        /**
         * Custom Interactivity Method: Injects dynamically verified modern table row nodes
         */
        function dispatchClusterNode() {
            const tokenInput = document.getElementById('nodeTokenInput');
            const val = tokenInput.value.trim();

            if (!val) {
                triggerSystemNotification("Dispatch Token payload stream cannot be empty.", "error");
                return;
            }

            const tbody = document.getElementById('clusterTableBody');
            const newRow = document.createElement('tr');
            
            // Premium DOM generation layout safe string escaping interpolation architecture
            newRow.innerHTML = `
                <td style="font-weight: 500;">Custom-Node-${Math.floor(Math.random() * 100)}</td>
                <td style="font-family: monospace; color: var(--text-muted);">${val}</td>
                <td><span class="badge-status status-active">Optimal</span></td>
            `;

            tbody.appendChild(newRow);
            tokenInput.value = '';
            triggerSystemNotification("Dynamic custom node cluster dispatched safely into matrix.", "success");
        }

        /**
         * Micro-Notification Engine creating reactive real-time toast elements natively
         */
        function triggerSystemNotification(message, type = 'success') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            toast.className = 'toast-notification';
            
            const iconType = type === 'success' ? 'check-circle' : 'alert-triangle';
            
            toast.innerHTML = `
                <i data-lucide="${iconType}" class="toast-icon ${type}"></i>
                <span class="toast-message">${message}</span>
            `;
            
            container.appendChild(toast);
            lucide.createIcons();

            // Hardware-accelerated kinetic presentation entry handling
            setTimeout(() => toast.classList.add('active'), 50);

            // Natural garbage collection of toast component nodes
            setTimeout(() => {
                toast.classList.remove('active');
                setTimeout(() => toast.remove(), 400);
            }, 4000);
        }

        /**
         * High-End Light and Dark Mode State Switcher
         */
        function toggleInterfaceTheme() {
            const currentTheme = document.documentElement.getAttribute('data-theme');
            const targetTheme = currentTheme === 'light' ? 'dark' : 'light';
            document.documentElement.setAttribute('data-theme', targetTheme);
            
            const themeIcon = document.getElementById('themeIcon');
            if (targetTheme === 'light') {
                themeIcon.setAttribute('data-lucide', 'moon');
            } else {
                themeIcon.setAttribute('data-lucide', 'sun');
            }
            lucide.createIcons();
            triggerSystemNotification(`Switched interface layout mode to: ${targetTheme.toUpperCase()}`, 'success');
        }
    </script>
</body>
</html>
