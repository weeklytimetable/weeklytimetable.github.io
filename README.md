<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student List Marker</title>
    <!-- External Libraries via CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Inline Styles -->
    <style>
        :root {
            /* Material 3 Light Theme - Vibrant Blue */
            --md-sys-color-primary: #00639B;
            --md-sys-color-on-primary: #FFFFFF;
            --md-sys-color-primary-container: #CDE5FF;
            --md-sys-color-on-primary-container: #001D32;
            --md-sys-color-secondary: #51606F;
            --md-sys-color-on-secondary: #FFFFFF;
            --md-sys-color-secondary-container: #D5E4F7;
            --md-sys-color-on-secondary-container: #0D1D2A;
            --md-sys-color-tertiary: #67587A;
            --md-sys-color-on-tertiary: #FFFFFF;
            --md-sys-color-tertiary-container: #EEDCFF;
            --md-sys-color-on-tertiary-container: #221533;
            --md-sys-color-error: #B3261E;
            --md-sys-color-on-error: #FFFFFF;
            --md-sys-color-error-container: #F9DEDC;
            --md-sys-color-on-error-container: #410E0B;
            --md-sys-color-background: #F8F9FC;
            --md-sys-color-on-background: #191C1E;
            --md-sys-color-surface: #F8F9FC;
            --md-sys-color-on-surface: #191C1E;
            --md-sys-color-surface-variant: #DFE3E9;
            --md-sys-color-on-surface-variant: #43474E;
            --md-sys-color-outline: #73777F;
            --md-sys-color-shadow: #000000;
            --md-sys-color-inverse-surface: #2E3133;
            --md-sys-color-inverse-on-surface: #F0F1F3;
            --md-sys-color-inverse-primary: #97CBFF;
            --md-sys-color-success: #198754;
            --md-sys-color-danger: #dc3545;
            --md-sys-color-success-container: #D1F4E1;
            --md-sys-color-danger-container: #F8D7DA;

            --elevation-1: 0px 1px 2px rgba(0, 0, 0, 0.3), 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
            --elevation-2: 0px 1px 2px rgba(0, 0, 0, 0.3), 0px 2px 6px 2px rgba(0, 0, 0, 0.15);
            --elevation-3: 0px 1px 3px rgba(0, 0, 0, 0.3), 0px 4px 8px 3px rgba(0, 0, 0, 0.15);
        }

        [data-theme="dark"] {
             /* Material 3 Dark Theme - Vibrant Blue */
            --md-sys-color-primary: #97CBFF;
            --md-sys-color-on-primary: #003352;
            --md-sys-color-primary-container: #004A74;
            --md-sys-color-on-primary-container: #CDE5FF;
            --md-sys-color-secondary: #B9C8DA;
            --md-sys-color-on-secondary: #23323F;
            --md-sys-color-secondary-container: #394857;
            --md-sys-color-on-secondary-container: #D5E4F7;
            --md-sys-color-tertiary: #D2BFEF;
            --md-sys-color-on-tertiary: #382A49;
            --md-sys-color-tertiary-container: #4F4161;
            --md-sys-color-on-tertiary-container: #EEDCFF;
            --md-sys-color-error: #F2B8B5;
            --md-sys-color-on-error: #601410;
            --md-sys-color-error-container: #8C1D18;
            --md-sys-color-on-error-container: #F9DEDC;
            --md-sys-color-background: #191C1E;
            --md-sys-color-on-background: #E2E2E5;
            --md-sys-color-surface: #191C1E;
            --md-sys-color-on-surface: #E2E2E5;
            --md-sys-color-surface-variant: #43474E;
            --md-sys-color-on-surface-variant: #C3C7CE;
            --md-sys-color-outline: #8D9198;
            --md-sys-color-shadow: #000000;
            --md-sys-color-inverse-surface: #E2E2E5;
            --md-sys-color-inverse-on-surface: #2E3133;
            --md-sys-color-inverse-primary: #00639B;
            --md-sys-color-success: #4ade80;
            --md-sys-color-danger: #f87171;
            --md-sys-color-success-container: #005223;
            --md-sys-color-danger-container: #7d2b2b;
        }
        
        /* Base Styles */
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--md-sys-color-background);
            color: var(--md-sys-color-on-background);
            transition: background-color 0.3s ease, color 0.3s ease;
        }
        
        .app-container {
            padding-bottom: 9rem; /* Increased space for bottom nav */
        }
        
        /* Material Components */
        .material-card {
            background-color: var(--md-sys-color-surface);
            border-radius: 1.5rem; /* 24px */
            box-shadow: var(--elevation-1);
            padding: 1.5rem;
            transition: box-shadow 0.2s ease, background-color 0.3s ease;
        }

        .btn {
            border-radius: 99px; /* Pill shape */
            padding: 0.625rem 1.5rem; /* 10px 24px */
            font-weight: 500;
            font-size: 0.875rem; /* 14px */
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
            border: 1px solid transparent;
            transition: all 0.2s ease-in-out;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }
        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            box-shadow: none;
        }
        
        /* Ripple effect */
        .btn::after {
            content: '';
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            pointer-events: none;
            background-image: radial-gradient(circle, #000 10%, transparent 10.01%);
            background-repeat: no-repeat;
            background-position: 50%;
            transform: scale(10, 10);
            opacity: 0;
            transition: transform .3s, opacity .5s;
        }
        .btn:active::after {
            transform: scale(0, 0);
            opacity: .1;
            transition: 0s;
        }
        
        .btn-filled {
            background-color: var(--md-sys-color-primary);
            color: var(--md-sys-color-on-primary);
        }
        .btn-filled:hover { background-color: color-mix(in srgb, var(--md-sys-color-primary), white 10%); }
        .btn-filled.danger { background-color: var(--md-sys-color-error); color: var(--md-sys-color-on-error); }
        .btn-filled.danger:hover { background-color: color-mix(in srgb, var(--md-sys-color-error), white 10%); }
        
        .btn-outlined {
            background-color: transparent;
            color: var(--md-sys-color-primary);
            border-color: var(--md-sys-color-outline);
        }
        .btn-outlined:hover { background-color: var(--md-sys-color-primary-container); border-color: var(--md-sys-color-primary); }
        .btn-outlined.danger { color: var(--md-sys-color-error); }
        .btn-outlined.danger:hover { background-color: var(--md-sys-color-error-container); border-color: var(--md-sys-color-error); }

        .btn-icon {
            padding: 0.625rem; /* 10px */
            border-radius: 99px;
            background-color: transparent;
            color: var(--md-sys-color-on-surface-variant);
        }
        .btn-icon:hover { background-color: color-mix(in srgb, var(--md-sys-color-primary), transparent 90%); color: var(--md-sys-color-primary); }
        
        .segmented-btn-group { display: flex; background-color: var(--md-sys-color-surface-variant); border-radius: 99px; padding: 4px; gap: 4px; }
        .segmented-btn { flex: 1; padding: 0.5rem 1rem; font-size: 0.8rem; background-color: transparent; border: none; color: var(--md-sys-color-on-surface-variant); cursor: pointer; transition: all 0.2s; border-radius: 99px; }
        .segmented-btn:hover { background-color: color-mix(in srgb, var(--md-sys-color-secondary-container), #000 5%); }
        .segmented-btn.active { background-color: var(--md-sys-color-secondary-container); color: var(--md-sys-color-on-secondary-container); font-weight: 600; box-shadow: var(--elevation-1); }

        input[type="text"], input[type="number"], input[type="file"], select {
            background-color: var(--md-sys-color-surface-variant);
            border: none;
            border-bottom: 2px solid var(--md-sys-color-on-surface-variant);
            border-radius: 4px 4px 0 0;
            padding: 1rem 0.75rem 0.5rem;
            width: 100%;
            color: var(--md-sys-color-on-surface);
            transition: border-color 0.2s;
        }
        input:focus, select:focus {
            outline: none;
            border-bottom-color: var(--md-sys-color-primary);
        }
        
        /* Layout: Header & Bottom Nav */
        .app-header {
            background-color: var(--md-sys-color-surface);
            box-shadow: var(--elevation-1);
        }

        .bottom-nav {
            background-color: var(--md-sys-color-surface);
            box-shadow: var(--elevation-3);
        }
        .nav-btn {
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            color: var(--md-sys-color-on-surface-variant); font-weight: 500; font-size: 0.75rem;
            transition: all 0.2s ease; position: relative;
            padding: 4px 12px; border-radius: 16px; min-height: 48px;
        }
        .nav-btn .nav-label { transition: opacity 0.2s; }
        .nav-btn .nav-icon { margin-bottom: 2px; }
        .nav-btn.active {
            color: var(--md-sys-color-on-secondary-container);
            font-weight: 600;
            background-color: var(--md-sys-color-secondary-container);
        }

        /* Animations */
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .fade-in { animation: fadeIn 0.5s ease-out forwards; }
        
        /* Custom scrollbar */
        .custom-scrollbar::-webkit-scrollbar { width: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: transparent; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: var(--md-sys-color-primary); border-radius: 3px; }
        
        /* Modal Styles */
        .modal-overlay {
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(0, 0, 0, 0.5);
            display: flex; justify-content: center; align-items: center;
            z-index: 100; opacity: 0; pointer-events: none; transition: opacity 0.3s;
        }
        .modal-overlay.visible { opacity: 1; pointer-events: auto; }
        .modal-content {
            transform: scale(0.95); transition: transform 0.3s;
            max-width: 500px; width: 90%;
            background-color: var(--md-sys-color-surface);
            box-shadow: var(--elevation-3);
        }
        .modal-overlay.visible .modal-content { transform: scale(1); }
        .modal-fullscreen .modal-content {
             max-width: 95%; width: 100%; height: 90%;
        }

        /* Hidden div for exporting */
        .export-container { position: absolute; left: -9999px; top: auto; width: 850px; }
        
        /* Professional Document Export Styles (Unchanged) */
        .export-document { font-family: Arial, sans-serif; padding: 2rem; background-color: #ffffff; border: 1px solid #ddd; }
        .export-document, .export-document *, .export-document p, .export-document div, .export-document td, .export-document th { color: #333 !important; }
        .export-document h1 { font-size: 28px; font-weight: bold; text-align: center; margin-bottom: 10px; color: #1a237e !important; }
        .export-document h2 { font-size: 22px; font-weight: bold; text-align: center; margin-bottom: 25px; color: #555 !important; border-bottom: 2px solid #1a237e; padding-bottom: 10px;}
        .export-document .meta-info { text-align: center; margin-bottom: 25px; font-size: 14px; line-height: 1.6; }
        .export-document .meta-info strong { color: #000 !important; }
        .export-document .summary-section { background-color: #f8f9fa; border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px; margin-bottom: 30px; display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; }
        .export-document .summary-item { text-align: center; }
        .export-document .summary-item .label { font-size: 14px; color: #666 !important; margin-bottom: 5px; }
        .export-document .summary-item .value { font-size: 20px; font-weight: bold; }
        .export-document .summary-item .value.present, .export-document .summary-item .value.received { color: #28a745 !important; }
        .export-document .summary-item .value.absent, .export-document .summary-item .value.due { color: #dc3545 !important; }
        .export-document h3 { font-size: 18px; font-weight: bold; border-bottom: 1px solid #ccc; padding-bottom: 8px; margin-top: 30px; margin-bottom: 15px; color: #333 !important; }
        .export-document table { width: 100%; border-collapse: collapse; margin-top: 15px; page-break-inside: auto; }
        .export-document tr { page-break-inside: avoid; page-break-after: auto; }
        .export-document th, .export-document td { border: 1px solid #ddd; padding: 12px; text-align: left; font-size: 14px; }
        .export-document thead { display: table-header-group; }
        .export-document th { background-color: #e9ecef; font-weight: bold; color: #495057 !important; }
        .export-document tbody tr:nth-child(even) { background-color: #f8f9fa; }
        .export-document .footer { text-align: center; margin-top: 40px; font-size: 12px; color: #888 !important; border-top: 1px solid #ccc; padding-top: 15px;}
        
        /* New Class Management Styles */
        .class-card {
            position: relative;
            cursor: pointer;
            border: 2px solid var(--md-sys-color-outline);
            transition: all 0.2s ease;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        .class-card:hover { transform: translateY(-4px); box-shadow: var(--elevation-2); }
        .class-card.active { border-color: var(--md-sys-color-primary); background-color: var(--md-sys-color-primary-container); }
        .class-card.active .class-card-icon { background-color: var(--md-sys-color-primary); color: var(--md-sys-color-on-primary); }
        .class-card.pinned { border-style: dashed; }

        .class-card-icon {
            width: 48px; height: 48px;
            border-radius: 99px;
            background-color: var(--md-sys-color-secondary-container);
            color: var(--md-sys-color-on-secondary-container);
            display: flex; align-items: center; justify-content: center;
            font-size: 1.5rem; font-weight: bold;
            transition: all 0.2s ease;
        }
        .class-card-actions { position: absolute; top: 0.5rem; right: 0.5rem; display: flex; gap: 0.25rem; }
        .class-card-actions button {
            background-color: var(--md-sys-color-surface-variant);
        }
        .class-card-actions button:hover { background-color: var(--md-sys-color-secondary-container); }
        .class-card-actions button.pinned-active { color: var(--md-sys-color-primary); }
        
        .add-class-card {
            border: 2px dashed var(--md-sys-color-outline);
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: var(--md-sys-color-secondary);
        }
        .add-class-card:hover { border-color: var(--md-sys-color-primary); color: var(--md-sys-color-primary); background: var(--md-sys-color-primary-container); }

        /* Colored Summary */
        .summary-block {
            padding: 0.75rem;
            border-radius: 1rem;
            text-align: center;
            background-color: var(--md-sys-color-surface-variant);
            color: var(--md-sys-color-on-surface-variant);
        }
        .summary-block.success { background-color: var(--md-sys-color-success-container); color: var(--md-sys-color-success); }
        .summary-block.danger { background-color: var(--md-sys-color-danger-container); color: var(--md-sys-color-danger); }
        .summary-block .label { font-size: 0.75rem; }
        .summary-block .value { font-size: 1.25rem; font-weight: bold; }

    </style>
</head>
<body class="antialiased">

    <div id="app-container" class="relative min-h-screen">
        <!-- App Header (Sticky) -->
        <header class="app-header sticky top-0 z-40 p-4">
            <div class="container mx-auto flex justify-between items-center">
                <div>
                    <h1 id="app-title" class="text-xl font-bold transition-all duration-300">Dashboard</h1>
                    <p id="total-students-header" class="text-sm text-gray-500">No class selected</p>
                </div>
                <div class="flex items-center gap-2">
                    <button id="undo-history-btn" class="btn btn-icon disabled:opacity-50" disabled title="Undo">
                        <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M9 15L3 9m0 0l6-6M3 9h12a6 6 0 010 12h-3" /></svg>
                    </button>
                    <button id="redo-history-btn" class="btn btn-icon disabled:opacity-50" disabled title="Redo">
                        <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M15 15l6-6m0 0l-6-6m6 6H9a6 6 0 000 12h3" /></svg>
                    </button>
                    <button id="theme-toggle" class="btn btn-icon">
                        <svg id="theme-icon-sun" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" /></svg>
                        <svg id="theme-icon-moon" class="h-5 w-5 hidden" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" /></svg>
                    </button>
                    <div class="relative">
                        <button id="menu-btn" class="btn btn-icon">
                            <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M12 5v.01M12 12v.01M12 19v.01M12 6a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2z" /></svg>
                        </button>
                        <div id="menu-options" class="hidden absolute right-0 mt-2 w-56 material-card !p-2 z-50 rounded-2xl">
                             <a href="#" id="export-pdf" class="block w-full text-left px-4 py-2 text-sm rounded-lg hover:bg-[var(--md-sys-color-surface-variant)]">Export Page as PDF</a>
                             <a href="#" id="export-png" class="block w-full text-left px-4 py-2 text-sm rounded-lg hover:bg-[var(--md-sys-color-surface-variant)]">Export Page as PNG</a>
                             <a href="#" id="export-excel" class="block w-full text-left px-4 py-2 text-sm rounded-lg hover:bg-[var(--md-sys-color-surface-variant)]">Export Page as Excel</a>
                             <div class="border-t my-2 border-[var(--md-sys-color-outline)]"></div>
                             <button id="import-json-btn-menu" class="w-full text-left px-4 py-2 text-sm rounded-lg hover:bg-[var(--md-sys-color-surface-variant)]">Import App Data (JSON)</button>
                             <a href="#" id="export-json" class="block w-full text-left px-4 py-2 text-sm rounded-lg hover:bg-[var(--md-sys-color-surface-variant)]">Export App Data (JSON)</a>
                             <div class="border-t my-2 border-[var(--md-sys-color-outline)]"></div>
                             <button id="reset-app-btn" class="w-full text-left px-4 py-2 text-sm rounded-lg text-[var(--md-sys-color-error)] hover:bg-[var(--md-sys-color-error-container)]">Reset All Data</button>
                        </div>
                    </div>
                    <input type="file" id="import-json-input" class="hidden" accept=".json">
                </div>
            </div>
        </header>

        <!-- Main Content Area -->
        <main id="main-content" class="container mx-auto p-4">
            <!-- Sections will be dynamically rendered here -->
        </main>
    </div>

    <!-- Bottom Navigation Bar (Fixed) -->
    <nav class="bottom-nav fixed bottom-4 left-1/2 -translate-x-1/2 w-[90%] max-w-sm h-20 flex justify-around items-center z-50 rounded-full !p-2">
        <button class="nav-btn" data-tab="dashboard">
            <div class="nav-icon">
                <svg class="h-6 w-6 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z" />
                </svg>
            </div>
            <span class="nav-label">Dashboard</span>
        </button>
        <button class="nav-btn" data-tab="attendance">
            <div class="nav-icon">
                <svg class="h-6 w-6 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
            </div>
            <span class="nav-label">Attendance</span>
        </button>
        <button class="nav-btn" data-tab="payments">
            <div class="nav-icon">
                <svg class="h-6 w-6 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 8.25h19.5M2.25 9h19.5m-16.5 5.25h6m-6 2.25h3m-3.75 3h15a2.25 2.25 0 002.25-2.25V6.75A2.25 2.25 0 0019.5 4.5h-15A2.25 2.25 0 002.25 6.75v10.5A2.25 2.25 0 004.5 21z" />
                </svg>
            </div>
            <span class="nav-label">Payments</span>
        </button>
    </nav>
    
    <!-- Modals -->
    <div id="confirmation-modal" class="modal-overlay">
        <div class="modal-content material-card text-center !p-6">
            <p id="modal-message" class="mb-6 text-lg">Are you sure?</p>
            <div class="flex justify-end gap-4">
                <button id="modal-cancel" class="btn btn-outlined">Cancel</button>
                <button id="modal-confirm" class="btn btn-filled danger">Confirm</button>
            </div>
        </div>
    </div>
    
    <div id="payments-filter-modal" class="modal-overlay">
        <div class="modal-content material-card !p-4 space-y-4">
            <div class="flex justify-between items-center">
                <h3 class="text-xl font-bold">Filters</h3>
                <button id="close-payments-filter-modal-btn" class="btn btn-icon">&times;</button>
            </div>
            <div id="payments-filter-modal-content" class="space-y-4 max-h-[60vh] overflow-y-auto custom-scrollbar pr-2"></div>
             <div class="flex justify-end gap-4 pt-4 border-t border-[var(--md-sys-color-outline)]">
                <button id="reset-payments-filters-btn" class="btn btn-outlined danger">Reset</button>
                <button id="apply-payments-filters-btn" class="btn btn-filled">Apply</button>
            </div>
        </div>
    </div>

    <div id="roster-manager-modal" class="modal-overlay modal-fullscreen">
        <div class="modal-content material-card flex flex-col">
            <div class="flex justify-between items-center pb-4 border-b border-[var(--md-sys-color-outline)]">
                 <h2 id="roster-modal-title" class="text-2xl font-bold">Roster Manager</h2>
                 <button id="close-roster-manager-btn" class="btn btn-icon">&times;</button>
            </div>
            <div id="roster-manager-content" class="flex-grow overflow-y-auto custom-scrollbar -mr-4 pr-4 mt-4 space-y-4"></div>
            <div class="flex justify-end gap-4 pt-4 border-t border-[var(--md-sys-color-outline)]">
                <button id="delete-class-btn" class="btn btn-outlined danger hidden">Delete Class</button>
                <button id="save-class-btn" class="btn btn-filled">Save</button>
            </div>
        </div>
    </div>
    
    <input type="file" id="import-session-input" class="hidden" accept=".json">
    <div class="export-container"><div id="export-content-wrapper"></div></div>

    <!-- Main Application Script -->
    <script>
    document.addEventListener('DOMContentLoaded', () => {
        // --- INITIAL DATA & STATE ---
        let state = {};
        let stateHistory = [];
        let historyPointer = -1;
        let savedScrollPositions = { attendance: 0, payments: 0 };

        const defaultState = {
            classes: [],
            activeClassId: null,
            currentTab: 'dashboard',
            ui: {
                attendance: { filteredStudents: [], searchText: '', searchMode: 'name', filterType: 'all', filterRangePreset: 'all', customRange: '', statusFilter: 'all' },
                payments: { filteredStudents: [], searchText: '', searchMode: 'name', filterType: 'all', filterRangePreset: 'all', customRange: '', statusFilter: 'all', paymentMethodFilter: 'all', purpose: '', defaultAmount: '', collector: '' },
                history: { viewingItem: null, searchText: '' },
                roster: { editingClassId: null }
            }
        };

        const { jsPDF } = window.jspdf;
        const mainContent = document.getElementById('main-content');
        const totalStudentsHeader = document.getElementById('total-students-header');
        const modal = document.getElementById('confirmation-modal');
        const modalMessage = document.getElementById('modal-message');
        const menuBtn = document.getElementById('menu-btn');
        const menuOptions = document.getElementById('menu-options');
        const undoBtn = document.getElementById('undo-history-btn');
        const redoBtn = document.getElementById('redo-history-btn');
        const appTitleEl = document.getElementById('app-title');

        // --- All Function Definitions ---
        function deepClone(obj) { return JSON.parse(JSON.stringify(obj)); }

        function debounce(func, delay) {
            let timeout;
            return function(...args) {
                const context = this;
                clearTimeout(timeout);
                timeout = setTimeout(() => func.apply(context, args), delay);
            };
        }

        const debouncedAutosave = debounce(() => {
            const activeClass = getActiveClass();
            if (!activeClass) return;

            const workInProgress = JSON.parse(localStorage.getItem('studentMarkerWorkInProgress') || '{}');
            workInProgress[activeClass.id] = {
                attendance: activeClass.students.map(s => ({ reg: s.reg, present: s.present })),
                payments: activeClass.students.map(s => ({ reg: s.reg, payment: s.payment })),
                paymentsUi: {
                    purpose: state.ui.payments.purpose,
                    defaultAmount: state.ui.payments.defaultAmount,
                    collector: state.ui.payments.collector,
                }
            };
            localStorage.setItem('studentMarkerWorkInProgress', JSON.stringify(workInProgress));
        }, 1500);

        function recordState() {
            if (historyPointer < stateHistory.length - 1) {
                stateHistory = stateHistory.slice(0, historyPointer + 1);
            }
            stateHistory.push(deepClone(state));
            historyPointer++;
            updateUndoRedoButtons();
        }

        function undo() { if (historyPointer > 0) { historyPointer--; loadStateFromHistory(); } }
        function redo() { if (historyPointer < stateHistory.length - 1) { historyPointer++; loadStateFromHistory(); } }
        
        function loadStateFromHistory() {
            state = deepClone(stateHistory[historyPointer]);
            renderApp();
            updateUndoRedoButtons();
        }

        function updateUndoRedoButtons() {
            undoBtn.disabled = historyPointer <= 0;
            redoBtn.disabled = historyPointer >= stateHistory.length - 1;
        }

        function saveState() {
            localStorage.setItem('studentMarkerData', JSON.stringify(state));
        }
        
        function getInitialStudentsData() {
            const students = [];
            const names = ["Abin", "Adithya", "Aiswarya", "Akash", "Akhil", "Akshara", "Alan", "Aleena", "Alen", "Alwin", "Amal", "Amitha", "Anagha", "Anand", "Ananthu", "Angel", "Anitta", "Anjali", "Anjana", "Anju", "Anusree", "Aparna", "Arjun", "Arya", "Aswin", "Athira", "Athul", "Augustine", "Aysha", "Basil", "Bhadra", "Bibin", "Christy", "Dany", "David", "Deepak", "Devika", "Dona", "Eldho", "Febin", "Gokul", "Gopika", "Harikrishnan", "Haritha", "Hashim", "Irene", "Jain", "Jasil", "Jerin", "Jibin", "Jijo", "Jinu", "Jishnu", "Jithin", "Joel", "John", "Josna", "Karthik", "Kavya", "Keerthana", "Kevin", "Kiran", "Krishna", "Lakshmi", "Linta", "Maria", "Mathew", "Megha", "Merin", "Midhun", "Milan", "Mohammed", "Mubashir", "Muneer", "Nandana", "Navaneeth", "Neethu", "Nikhil", "Nithin", "Paul", "Pooja", "Pranav", "Rahul", "Ram", "Reshma", "Rinshad", "Riya", "Rohith", "Sachin", "Sajin", "Sandra", "Sanjay", "Saran", "Sarath", "Sebastian", "Shahana"];
            for (let i = 1; i <= 95; i++) {
                students.push({
                    id: i,
                    reg: `23MCA${String(i).padStart(3, '0')}`,
                    name: names[i - 1] || `Student ${String(i).padStart(2, '0')}`,
                    present: undefined,
                    payment: null
                });
            }
            return students;
        }

        function loadState() {
            const savedData = localStorage.getItem('studentMarkerData');
            if (savedData) {
                const parsedData = JSON.parse(savedData);
                // Check for very old data structure and migrate to a default class
                if (parsedData.students && !parsedData.classes) {
                    console.log("Old data format detected. Migrating...");
                    state = deepClone(defaultState);
                    const defaultClass = {
                        id: `class-${Date.now()}`,
                        name: 'Default Class',
                        batch: 'Migrated',
                        iconNumber: 1,
                        isPinned: false,
                        students: (parsedData.students || []).map((s, i) => ({ ...s, id: i + 1, present: undefined, payment: null })),
                        attendanceHistory: parsedData.attendanceHistory || [],
                        paymentsHistory: parsedData.paymentsHistory || parsedData.moneyHistory || []
                    };
                    state.classes.push(defaultClass);
                    state.activeClassId = defaultClass.id;
                    saveState(); // Save the new structure
                } else {
                    state = { ...deepClone(defaultState), ...parsedData };
                }
            } else {
                // New user: create default state with the "5th Sem" class
                state = deepClone(defaultState);
                const defaultClass = {
                    id: `class-default-5th-sem`,
                    name: '5th Sem',
                    batch: '2023-24',
                    iconNumber: 5,
                    isPinned: false,
                    students: getInitialStudentsData(),
                    attendanceHistory: [],
                    paymentsHistory: []
                };
                state.classes.push(defaultClass);
                state.activeClassId = defaultClass.id;
            }

            // Restore work in progress for all classes
            const workInProgressData = localStorage.getItem('studentMarkerWorkInProgress');
            if (workInProgressData) {
                const allWip = JSON.parse(workInProgressData);
                state.classes.forEach(cls => {
                    const wip = allWip[cls.id];
                    if (!wip) return;

                    if (wip.attendance) {
                        wip.attendance.forEach(wipStudent => {
                            const student = cls.students.find(s => s.reg === wipStudent.reg);
                            if (student) student.present = wipStudent.present;
                        });
                    }
                    if (wip.payments) {
                        wip.payments.forEach(wipStudent => {
                            const student = cls.students.find(s => s.reg === wipStudent.reg);
                            if (student) student.payment = wipStudent.payment;
                        });
                    }
                });
                // Load UI state for the active class only
                const activeClass = getActiveClass();
                if (activeClass && allWip[activeClass.id]?.paymentsUi) {
                    const paymentsUi = allWip[activeClass.id].paymentsUi;
                    state.ui.payments.purpose = paymentsUi.purpose || '';
                    state.ui.payments.defaultAmount = paymentsUi.defaultAmount || '';
                    state.ui.payments.collector = paymentsUi.collector || '';
                }
            }
        }
        
        function init() {
            applyInitialTheme();
            loadState();
            recordState(); 
            renderApp();
            setupEventListeners();
        }

        function getActiveClass() {
            if (!state.activeClassId) return null;
            return state.classes.find(c => c.id === state.activeClassId);
        }

        function renderApp() {
            const activeClass = getActiveClass();
            totalStudentsHeader.textContent = activeClass ? `Students: ${activeClass.students.length}` : 'No class selected';
            renderCurrentTab();
        }
        
        function switchTab(tabName) {
            state.ui.history.viewingItem = null;
            state.currentTab = tabName;
            appTitleEl.textContent = tabName.charAt(0).toUpperCase() + tabName.slice(1);
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.toggle('active', btn.dataset.tab === tabName);
            });
            renderCurrentTab();
        }

        function renderCurrentTab() {
            mainContent.innerHTML = '';
            let content = '';
            switch (state.currentTab) {
                case 'dashboard': content = getDashboardHTML(); break;
                case 'attendance': content = getAttendanceHTML(); break;
                case 'payments': content = getPaymentsHTML(); break;
            }
            mainContent.innerHTML = content;
            // Post-render updates
            if (state.currentTab === 'dashboard') renderHistoryLists();
            if (state.currentTab === 'attendance') updateAttendanceList();
            if (state.currentTab === 'payments') updatePaymentsList();
        }
        
        // --- THEME MANAGEMENT ---
        function applyInitialTheme() {
            const savedTheme = localStorage.getItem('theme');
            const prefersDark = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
            const theme = savedTheme || (prefersDark ? 'dark' : 'light');
            setTheme(theme);
        }

        function toggleTheme() {
            const currentTheme = document.documentElement.getAttribute('data-theme');
            const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
            setTheme(newTheme);
        }

        function setTheme(theme) {
            document.documentElement.setAttribute('data-theme', theme);
            localStorage.setItem('theme', theme);
            const sunIcon = document.getElementById('theme-icon-sun');
            const moonIcon = document.getElementById('theme-icon-moon');
            if (theme === 'dark') {
                sunIcon.classList.add('hidden');
                moonIcon.classList.remove('hidden');
            } else {
                sunIcon.classList.remove('hidden');
                moonIcon.classList.add('hidden');
            }
        }

        // --- TEMPLATES ---
        function getDashboardHTML() {
            const sortedClasses = [...state.classes].sort((a, b) => (b.isPinned - a.isPinned));
            const classCardsHTML = sortedClasses.map(cls => `
                <div class="class-card material-card !p-4 ${cls.id === state.activeClassId ? 'active' : ''} ${cls.isPinned ? 'pinned' : ''}" data-class-id="${cls.id}">
                    <div class="class-card-actions">
                        <button class="toggle-pin-btn btn btn-icon !p-2">
                            <svg class="h-5 w-5 pointer-events-none ${cls.isPinned ? 'text-[var(--md-sys-color-primary)]' : ''}" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
                                <path d="M16 12V4h1V2H7v2h1v8l-2 2v2h5.2v6h1.6v-6H18v-2l-2-2z"></path>
                            </svg>
                        </button>
                        <button class="edit-class-btn btn btn-icon !p-2">
                            <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor"><path d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z" /></svg>
                        </button>
                    </div>
                    <div class="flex items-center gap-4">
                        <div class="class-card-icon">${cls.iconNumber}</div>
                        <div>
                            <p class="text-lg font-bold truncate">${cls.name}</p>
                            <p class="text-sm text-[var(--md-sys-color-on-surface-variant)]">${cls.batch}</p>
                        </div>
                    </div>
                    <div class="mt-4 pt-3 border-t border-[var(--md-sys-color-outline)] text-sm text-center">
                        <p>${cls.students.length} Students</p>
                    </div>
                </div>
            `).join('');

            const addClassCardHTML = `
                <div id="add-class-btn" class="add-class-card min-h-[160px] material-card !p-4">
                    <svg class="h-10 w-10 mb-2" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M12 9v6m3-3H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
                    <p class="font-semibold">Add New Class</p>
                </div>
            `;
            
            return `
                <div class="space-y-6">
                    <div class="material-card">
                        <h2 class="text-xl font-bold mb-4">Select a Class</h2>
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
                            ${classCardsHTML}
                            ${addClassCardHTML}
                        </div>
                    </div>
                    
                    <div id="history-section" class="${state.activeClassId ? '' : 'hidden'}">
                        ${getFullHistorySectionHTML()}
                    </div>
                </div>
            `;
        }

        function getAttendanceHTML() {
            const activeClass = getActiveClass();
            if (!activeClass) return `<div class="material-card text-center p-8"><p class="text-lg">Please select a class from the Dashboard first.</p></div>`;

            return `
                <div class="space-y-6">
                    <div class="material-card space-y-4">
                        <div class="flex justify-between items-center">
                            <h2 class="text-xl font-bold">Control Panel <span class="text-base font-normal text-[var(--md-sys-color-secondary)]">(${activeClass.name})</span></h2>
                            <div class="flex items-center gap-2">
                                <button id="reset-attendance-btn" class="btn btn-icon" title="Reset Current Marks">
                                    <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M1 4v6h6" /><path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10" /></svg>
                                </button>
                                <button id="save-attendance-btn" class="btn btn-icon" title="Save Session">
                                     <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z" /><path d="M17 21V13H7v8" /><path d="M7 3v5h8" /></svg>
                                </button>
                            </div>
                        </div>
                        <div class="space-y-4">
                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                                <input id="taken-by-input" type="text" placeholder="Taken by...">
                                <input id="subject-input" type="text" placeholder="Subject / Purpose">
                            </div>
                            <div>${getSearchComponentHTML('attendance')}</div>
                            <div>${getFilterComponentHTML('attendance')}</div>
                        </div>
                    </div>
                    <div class="material-card">
                        <div id="attendance-summary" class="grid grid-cols-3 gap-3 mb-4 text-center"></div>
                        <div class="flex gap-2 mb-4">
                            <button id="mark-all-visible-present" class="btn btn-outlined text-sm flex-1">Mark All</button>
                            <button id="clear-all-visible" class="btn btn-outlined text-sm flex-1">Clear All</button>
                        </div>
                        <div id="attendance-list" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3 max-h-[50vh] overflow-y-auto p-1 custom-scrollbar"></div>
                    </div>
                </div>`;
        }
        
        function getPaymentsHTML() {
            const activeClass = getActiveClass();
            if (!activeClass) return `<div class="material-card text-center p-8"><p class="text-lg">Please select a class from the Dashboard first.</p></div>`;

            const purposeValue = state.ui.payments.purpose;
            const defaultAmountValue = state.ui.payments.defaultAmount;
            const collectorValue = state.ui.payments.collector;
            
            return `
                <div class="space-y-6">
                    <div class="material-card space-y-4">
                        <div class="flex justify-between items-center">
                            <h2 class="text-xl font-bold">Control Panel <span class="text-base font-normal text-[var(--md-sys-color-secondary)]">(${activeClass.name})</span></h2>
                            <div class="flex items-center gap-2">
                                <button id="reset-payments-btn" class="btn btn-icon" title="Reset Current Collection">
                                    <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path d="M1 4v6h6" /><path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10" /></svg>
                                </button>
                                <button id="save-payments-btn" class="btn btn-icon" title="Save Collection">
                                     <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z" /><path d="M17 21V13H7v8" /><path d="M7 3v5h8" /></svg>
                                </button>
                            </div>
                        </div>
                        <div class="space-y-4">
                            <input id="payments-purpose" type="text" placeholder="Purpose (e.g., Field Visit)" value="${purposeValue}">
                            <div class="grid grid-cols-2 gap-4">
                                <input id="payments-amount" type="number" placeholder="Default Amount (₹)" value="${defaultAmountValue}">
                                <input id="payments-collector" type="text" placeholder="Collector" value="${collectorValue}">
                            </div>
                             <div class="flex gap-2 items-center">
                                <div class="flex-grow">${getSearchComponentHTML('payments')}</div>
                                <button id="open-payments-filter-modal-btn" class="btn btn-outlined flex-shrink-0 !py-3">
                                    <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M2.628 1.601C5.028 1.206 7.49 1 10 1s4.973.206 7.372.601a.75.75 0 01.628.74v2.288a2.25 2.25 0 01-.659 1.59l-4.682 4.683a2.25 2.25 0 00-.659 1.59v3.037c0 .684-.31 1.33-.844 1.757l-1.937 1.55A.75.75 0 0110 18v-5.968a2.25 2.25 0 00-.659-1.59L4.659 5.77a2.25 2.25 0 01-.659-1.59V2.34a.75.75 0 01.628-.74z" clip-rule="evenodd" /></svg>
                                    <span class="hidden sm:inline">Filter</span>
                                </button>
                            </div>
                        </div>
                    </div>
                    <div class="material-card">
                        <div id="payments-summary" class="grid grid-cols-2 md:grid-cols-3 gap-3 mb-4 text-center"></div>
                        <div id="payments-list" class="grid grid-cols-1 sm:grid-cols-2 gap-3 max-h-[50vh] overflow-y-auto p-1 custom-scrollbar"></div>
                    </div>
                </div>`;
        }
        
        function getSearchComponentHTML(tab) {
            const { searchText, searchMode } = state.ui[tab];
            const placeholder = searchMode === 'name' ? "Search by name..." : "Search by Reg No. ...";
            return `
                <div class="relative flex items-center w-full">
                    <input id="${tab}-search-input" type="text" placeholder="${placeholder}" value="${searchText}" autocomplete="off" class="!pr-[8.5rem]">
                    <div class="absolute right-2 top-1/2 -translate-y-1/2 flex items-center gap-1">
                        <button class="search-mode-btn btn btn-icon !p-2 ${searchMode === 'name' ? 'active !bg-[var(--md-sys-color-secondary-container)]' : ''}" data-search-mode="name" title="Search by Name">
                            <svg class="h-5 w-5 pointer-events-none" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z" clip-rule="evenodd" /></svg>
                        </button>
                        <button class="search-mode-btn btn btn-icon !p-2 ${searchMode === 'reg' ? 'active !bg-[var(--md-sys-color-secondary-container)]' : ''}" data-search-mode="reg" title="Search by Reg No.">
                             <svg class="h-5 w-5 pointer-events-none" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M7 20l4-16m2 16l4-16M6 9h14M4 15h14" /></svg>
                        </button>
                    </div>
                </div>`;
        }

        function getFilterComponentHTML(tab) {
            const { filterType, filterRangePreset, customRange } = state.ui[tab];
            const isRangeTypeSelected = filterType === 'reg' || filterType === 'index';
            let range2_label = '51-100', range2_value = '51-100';
            const activeClass = getActiveClass();
            const studentCount = activeClass ? activeClass.students.length : 100;
            if (filterType === 'reg') { range2_label = '51-99'; range2_value = '51-99'; }
            else if (filterType === 'index') { range2_label = `51-${studentCount}`; range2_value = `51-${studentCount}`; }

            return `
                <div class="space-y-3">
                    <div class="segmented-btn-group">
                        <button class="segmented-btn ${filterType === 'all' ? 'active' : ''}" data-filter-type="all">All</button>
                        <button class="segmented-btn ${filterType === 'reg' ? 'active' : ''}" data-filter-type="reg">Reg No.</button>
                        <button class="segmented-btn ${filterType === 'index' ? 'active' : ''}" data-filter-type="index">Index No.</button>
                    </div>
                    <div class="space-y-3 ${isRangeTypeSelected ? '' : 'hidden'}">
                        <div class="segmented-btn-group">
                             <button class="segmented-btn !text-xs ${filterRangePreset === 'all' ? 'active' : ''}" data-filter-range="all">All</button>
                             <button class="segmented-btn !text-xs ${filterRangePreset === '1-50' ? 'active' : ''}" data-filter-range="1-50">1-50</button>
                             <button class="segmented-btn !text-xs ${filterRangePreset === range2_value ? 'active' : ''}" data-filter-range="${range2_value}">${range2_label}</button>
                             <button class="segmented-btn !text-xs ${filterRangePreset === 'custom' ? 'active' : ''}" data-filter-range="custom">Custom</button>
                        </div>
                        <input id="${tab}-custom-range" type="text" placeholder="e.g., 1,5,10-15" class="${filterRangePreset === 'custom' ? '' : 'hidden'}" value="${customRange}">
                    </div>
                </div>`;
        }
        function getStudentCardHTML(student) {
             const statusIndicator = student.present === true ? 'bg-[var(--md-sys-color-primary-container)] border-[var(--md-sys-color-primary)]' : 'border-[var(--md-sys-color-outline)]';
            const filterType = state.ui.attendance.filterType;
            let circleContent = '';
            switch(filterType) {
                case 'reg': circleContent = parseInt(student.reg.slice(-3)); break;
                case 'index': circleContent = student.id; break;
                default: circleContent = student.name.split(' ').map(n => n[0]).join('').substring(0, 2).toUpperCase(); break;
            }
            return `<div class="student-card flex items-center p-2 border-2 rounded-2xl cursor-pointer ${statusIndicator}" data-reg="${student.reg}"><div class="w-10 h-10 rounded-full bg-[var(--md-sys-color-secondary-container)] flex items-center justify-center mr-3 flex-shrink-0 font-bold text-[var(--md-sys-color-on-secondary-container)]">${circleContent}</div><div class="flex-grow min-w-0"><p class="font-semibold truncate">${student.name}</p></div></div>`;
        }
        function getPaymentsStudentCardHTML(student) {
            let statusIndicator = student.payment ? 'bg-[var(--md-sys-color-success-container)]' : 'border border-[var(--md-sys-color-outline)]';
            const amount = student.payment ? student.payment.amount : (state.ui.payments.defaultAmount || '');
            const method = student.payment ? student.payment.method : 'Cash';
            return `<div class="student-card-payments flex flex-col p-3 rounded-2xl transition-colors ${statusIndicator}"><div class="flex items-center mb-2"><div class="flex-grow min-w-0"><p class="font-semibold truncate">${student.name}</p></div><input type="checkbox" class="payments-student-checkbox h-5 w-5 ml-2 flex-shrink-0" data-reg="${student.reg}" ${student.payment ? 'checked' : ''}></div><div class="flex items-center gap-2 mt-1"><input type="number" placeholder="Amt" class="payments-amount-input !text-sm !p-2" data-reg="${student.reg}" value="${amount}" ${!student.payment ? 'disabled' : ''}><select class="payments-method-select !text-sm !p-2" data-reg="${student.reg}" ${!student.payment ? 'disabled' : ''}><option value="Cash" ${method === 'Cash' ? 'selected' : ''}>Cash</option><option value="ePay" ${method === 'ePay' ? 'selected' : ''}>ePay</option></select></div></div>`;
        }
        function getFullHistorySectionHTML() {
            const activeClass = getActiveClass();
            const className = activeClass ? activeClass.name : '...';
            return `
                <div class="space-y-6 material-card">
                    <h2 class="text-xl font-bold">History: <span class="text-[var(--md-sys-color-primary)]">${className}</span></h2>
                    <div class="flex flex-wrap gap-2 items-center">
                        <input id="history-search-input" type="text" placeholder="Search history..." class="flex-grow" value="${state.ui.history.searchText}">
                        <button id="import-session-btn" class="btn btn-outlined"><svg class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" /></svg><span>Import</span></button>
                    </div>
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                        <div>
                            <h3 class="text-lg font-bold mb-4">Attendance</h3>
                            <div id="history-attendance-list" class="space-y-4 max-h-[60vh] overflow-y-auto custom-scrollbar pr-2"></div>
                        </div>
                        <div>
                            <h3 class="text-lg font-bold mb-4">Payments History</h3>
                            <div id="history-payments-list" class="space-y-4 max-h-[60vh] overflow-y-auto custom-scrollbar pr-2"></div>
                        </div>
                    </div>
                    <div id="history-detail-view" class="mt-6"></div>
                </div>`;
        }
        function getHistoryCardHTML(item) {
            let statsHTML;
            if (item.type === 'attendance') {
                const present = item.records.filter(r => r.present).length; const absent = item.records.length - present;
                statsHTML = `<div class="flex justify-around text-center text-sm"><div><p class="font-bold text-lg text-[var(--md-sys-color-success)]">${present}</p><p class="text-xs">Present</p></div><div><p class="font-bold text-lg text-[var(--md-sys-color-danger)]">${absent}</p><p class="text-xs">Absent</p></div><div><p class="font-bold text-lg">${item.records.length}</p><p class="text-xs">Total</p></div></div>`;
            } else {
                const paidRecords = item.records.filter(r => r.payment); const totalCollected = paidRecords.reduce((sum, r) => sum + parseFloat(r.payment.amount || 0), 0);
                statsHTML = `<div class="flex justify-around text-center text-sm"><div><p class="font-bold text-lg">${paidRecords.length}</p><p class="text-xs">Paid</p></div><div><p class="font-bold text-lg">₹${totalCollected.toFixed(2)}</p><p class="text-xs">Collected</p></div></div>`;
            }
            return `<div class="material-card !p-3 flex flex-col gap-3" data-id="${item.id}" data-type="${item.type}"><div><p class="font-bold truncate">${item.subject || item.purpose}</p><p class="text-xs text-[var(--md-sys-color-on-surface-variant)]">${item.date} at ${item.time}</p></div><div class="bg-[var(--md-sys-color-surface-variant)] p-2 rounded-xl">${statsHTML}</div><div class="flex flex-wrap gap-2 pt-3 border-t border-[var(--md-sys-color-outline)]"><button class="view-history-btn flex-1 text-sm btn btn-filled !py-1.5 !px-3 !text-xs">View</button><button disabled class="edit-history-btn flex-1 text-sm btn btn-outlined !py-1.5 !px-3 !text-xs opacity-50" title="Coming soon">Edit</button><div class="relative flex-1"><button class="export-history-btn w-full text-sm btn btn-outlined !py-1.5 !px-3 !text-xs">Export</button><div class="export-history-options hidden absolute right-0 bottom-full mb-2 w-36 material-card !p-1 z-20"><a href="#" class="export-history-option block px-3 py-1.5 text-xs rounded-lg hover:bg-[var(--md-sys-color-surface-variant)]" data-format="pdf">PDF</a><a href="#" class="export-history-option block px-3 py-1.5 text-xs rounded-lg hover:bg-[var(--md-sys-color-surface-variant)]" data-format="png">PNG</a><a href="#" class="export-history-option block px-3 py-1.5 text-xs rounded-lg hover:bg-[var(--md-sys-color-surface-variant)]" data-format="excel">Excel</a><div class="border-t my-1 border-[var(--md-sys-color-outline)]"></div><a href="#" class="export-history-option block px-3 py-1.5 text-xs rounded-lg hover:bg-[var(--md-sys-color-surface-variant)]" data-format="session_json">Share (JSON)</a></div></div><button class="delete-history-btn flex-none btn btn-icon !p-1.5 !rounded-lg text-[var(--md-sys-color-error)] hover:bg-[var(--md-sys-color-error-container)]" title="Delete"><svg class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg></button></div></div>`;
        }
        function getPaymentsFilterModalHTML() {
            const tab = 'payments'; const { filterType, filterRangePreset, customRange, statusFilter, paymentMethodFilter } = state.ui[tab]; const isRangeTypeSelected = filterType === 'reg' || filterType === 'index';
            const activeClass = getActiveClass(); const studentCount = activeClass ? activeClass.students.length : 100;
            let range2_label = '51-100', range2_value = '51-100'; if (filterType === 'reg') { range2_label = '51-99'; range2_value = '51-99'; } else if (filterType === 'index') { range2_label = `51-${studentCount}`; range2_value = `51-${studentCount}`; }
            return `<div class="space-y-3"><label class="font-semibold">Filter by Number</label><div class="segmented-btn-group" data-filter-group="filterType"><button class="segmented-btn ${filterType === 'all' ? 'active' : ''}" data-value="all">All</button><button class="segmented-btn ${filterType === 'reg' ? 'active' : ''}" data-value="reg">Reg No.</button><button class="segmented-btn ${filterType === 'index' ? 'active' : ''}" data-value="index">Index No.</button></div><div class="space-y-3 ${isRangeTypeSelected ? '' : 'hidden'}"><div class="segmented-btn-group" data-filter-group="filterRangePreset"><button class="segmented-btn !text-xs ${filterRangePreset === 'all' ? 'active' : ''}" data-value="all">All</button><button class="segmented-btn !text-xs ${filterRangePreset === '1-50' ? 'active' : ''}" data-value="1-50">1-50</button><button class="segmented-btn !text-xs ${filterRangePreset === range2_value ? 'active' : ''}" data-value="${range2_value}">${range2_label}</button><button class="segmented-btn !text-xs ${filterRangePreset === 'custom' ? 'active' : ''}" data-value="custom">Custom</button></div><input id="payments-modal-custom-range" type="text" placeholder="e.g., 1,5,10-15" class="${filterRangePreset === 'custom' ? '' : 'hidden'}" value="${customRange}"></div></div><div class="space-y-3 border-t pt-3 border-[var(--md-sys-color-outline)]"><label class="font-semibold">Payment Status</label><div class="segmented-btn-group" data-filter-group="statusFilter"><button class="segmented-btn ${statusFilter === 'all' ? 'active' : ''}" data-value="all">All</button><button class="segmented-btn ${statusFilter === 'paid' ? 'active' : ''}" data-value="paid">Paid</button><button class="segmented-btn ${statusFilter === 'unpaid' ? 'active' : ''}" data-value="unpaid">Unpaid</button></div></div><div class="space-y-3 border-t pt-3 border-[var(--md-sys-color-outline)]"><label class="font-semibold">Payment Method</label><div class="segmented-btn-group" data-filter-group="paymentMethodFilter"><button class="segmented-btn ${paymentMethodFilter === 'all' ? 'active' : ''}" data-value="all">All</button><button class="segmented-btn ${paymentMethodFilter === 'Cash' ? 'active' : ''}" data-value="Cash">Cash</button><button class="segmented-btn ${paymentMethodFilter === 'ePay' ? 'active' : ''}" data-value="ePay">ePay</button></div></div>`;
        }
        function getRosterManagerModalHTML(classData) {
            const isNewClass = !classData.id;
            const students = classData.students || [];
            const studentRows = students.map((s, index) => `
                <tr data-student-id="${s.id}">
                    <td class="p-2 border-b border-[var(--md-sys-color-outline)]">${s.id}</td>
                    <td class="p-2 border-b border-[var(--md-sys-color-outline)]"><input type="text" value="${s.reg}" class="!p-2 roster-student-reg"></td>
                    <td class="p-2 border-b border-[var(--md-sys-color-outline)]"><input type="text" value="${s.name}" class="!p-2 roster-student-name"></td>
                    <td class="p-2 border-b border-[var(--md-sys-color-outline)] text-center"><button class="delete-student-btn btn btn-icon !p-2 text-[var(--md-sys-color-error)]"><svg class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg></button></td>
                </tr>
            `).join('');

            return `
                <div class="space-y-4">
                    <h3 class="font-bold text-lg">Class Details</h3>
                    <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 items-center">
                        <div class="sm:col-span-2 grid grid-cols-1 sm:grid-cols-2 gap-3">
                            <input id="roster-class-name" type="text" placeholder="Class Name (e.g., 5th Sem)" value="${classData.name || ''}">
                            <input id="roster-class-batch" type="text" placeholder="Batch (e.g., 2025-26)" value="${classData.batch || ''}">
                        </div>
                        <div class="flex items-center gap-3">
                            <label class="font-semibold">Icon:</label>
                            <div class="flex-grow">
                                <select id="roster-class-icon" class="!p-2">
                                    ${[...Array(8).keys()].map(i => `<option value="${i+1}" ${classData.iconNumber == i+1 ? 'selected' : ''}>${i+1}</option>`).join('')}
                                </select>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="space-y-4">
                    <div class="flex justify-between items-center">
                         <h3 class="font-bold text-lg">Student Roster (${students.length})</h3>
                         <button id="add-student-btn" class="btn btn-outlined">Add Student</button>
                    </div>
                    <div class="max-h-[40vh] overflow-y-auto custom-scrollbar -mr-2 pr-2">
                        <table class="w-full text-left">
                            <thead><tr><th class="p-2">#</th><th class="p-2">Reg No</th><th class="p-2">Name</th><th class="p-2 text-center">Actions</th></tr></thead>
                            <tbody id="roster-student-list">${studentRows}</tbody>
                        </table>
                    </div>
                </div>
            `;
        }
        
        // --- RENDERERS & UPDATERS ---
        function updateAttendanceList() {
            const listEl = document.getElementById('attendance-list'); if (!listEl) return;
            const studentsToDisplay = getFilteredStudents('attendance');
            listEl.innerHTML = studentsToDisplay.length > 0 ? studentsToDisplay.map(s => getStudentCardHTML(s)).join('') : `<p class="col-span-full text-center py-8">No students match.</p>`;
            updateAttendanceSummary();
            if (listEl && savedScrollPositions.attendance > 0) listEl.scrollTop = savedScrollPositions.attendance;
        }
        function updatePaymentsList() {
             const listEl = document.getElementById('payments-list'); if (!listEl) return;
             const studentsToDisplay = getFilteredStudents('payments');
             listEl.innerHTML = studentsToDisplay.length > 0 ? studentsToDisplay.map(s => getPaymentsStudentCardHTML(s)).join('') : `<p class="col-span-full text-center py-8">No students match.</p>`;
             updatePaymentsSummary();
             if (listEl && savedScrollPositions.payments > 0) listEl.scrollTop = savedScrollPositions.payments;
        }
        function renderHistoryLists() {
            const activeClass = getActiveClass(); if (!activeClass) return;
            const attList = document.getElementById('history-attendance-list'); const payList = document.getElementById('history-payments-list');
            const lowerSearchText = state.ui.history.searchText.toLowerCase().trim();
            const filteredAtt = activeClass.attendanceHistory.filter(item => !lowerSearchText || Object.values(item).some(val => String(val).toLowerCase().includes(lowerSearchText)));
            const filteredPay = activeClass.paymentsHistory.filter(item => !lowerSearchText || Object.values(item).some(val => String(val).toLowerCase().includes(lowerSearchText)));
            if(attList) attList.innerHTML = filteredAtt.length > 0 ? filteredAtt.map(getHistoryCardHTML).join('') : `<p class="text-center p-2">No saved attendance history.</p>`;
            if(payList) payList.innerHTML = filteredPay.length > 0 ? filteredPay.map(getHistoryCardHTML).join('') : `<p class="text-center p-2">No saved payments history.</p>`;
            const detailView = document.getElementById('history-detail-view');
            if (detailView) detailView.innerHTML = state.ui.history.viewingItem ? `<div class="material-card"><div class="flex justify-between items-center mb-4 border-b pb-2"><h2 class="text-2xl font-bold">Details</h2><button id="close-history-detail" class="btn btn-icon">&times;</button></div><div class="max-h-[60vh] overflow-y-auto p-1"><div class="export-document">${getExportHTML(state.ui.history.viewingItem)}</div></div></div>` : '';
            if (state.ui.history.viewingItem) detailView.scrollIntoView({ behavior: 'smooth' });
        }
        function updateAttendanceSummary() {
            const summaryEl = document.getElementById('attendance-summary'); if(!summaryEl) return;
            const relevantStudents = getRangeFilteredStudents('attendance');
            const totalCount = relevantStudents.length; const presentCount = relevantStudents.filter(s => s.present === true).length; const absentCount = totalCount - presentCount; const activeFilter = state.ui.attendance.statusFilter;
            summaryEl.innerHTML = `<button class="summary-btn btn btn-outlined flex-1 !rounded-2xl ${activeFilter === 'all' ? '!bg-[var(--md-sys-color-secondary-container)]' : ''}" data-status-filter="all"><p class="font-bold">Total</p><span class="text-xl">${totalCount}</span></button><button class="summary-btn btn btn-outlined flex-1 !rounded-2xl ${activeFilter === 'present' ? '!bg-[var(--md-sys-color-secondary-container)]' : ''}" data-status-filter="present"><p class="font-bold text-[var(--md-sys-color-success)]">Present</p><span class="text-xl">${presentCount}</span></button><button class="summary-btn btn btn-outlined flex-1 !rounded-2xl ${activeFilter === 'absent' ? '!bg-[var(--md-sys-color-secondary-container)]' : ''}" data-status-filter="absent"><p class="font-bold text-[var(--md-sys-color-danger)]">Absent</p><span class="text-xl">${absentCount}</span></button>`;
        }
        function updatePaymentsSummary() {
            const summaryEl = document.getElementById('payments-summary'); if(!summaryEl) return;
            const studentsInView = getFilteredStudents('payments'); const defaultAmount = parseFloat(document.getElementById('payments-amount')?.value) || 0;
            const paidStudents = studentsInView.filter(s => s.payment); const paidCount = paidStudents.length; const unpaidCount = studentsInView.length - paidCount;
            const totalReceived = paidStudents.reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
            const totalExpected = defaultAmount > 0 ? defaultAmount * studentsInView.length : totalReceived; const dueAmount = totalExpected - totalReceived;
            const cashTotal = paidStudents.filter(s => s.payment.method === 'Cash').reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
            const ePayTotal = paidStudents.filter(s => s.payment.method === 'ePay').reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
            summaryEl.innerHTML = `<div class="summary-block"><div class="label">Budget</div><div class="value">₹${totalExpected.toFixed(2)}</div></div><div class="summary-block success"><div class="label">Received</div><div class="value">₹${totalReceived.toFixed(2)}</div></div><div class="summary-block danger"><div class="label">Due</div><div class="value">₹${Math.max(0, dueAmount).toFixed(2)}</div></div><div class="summary-block"><div class="label">Paid/Unpaid</div><div class="value"><span class="text-[var(--md-sys-color-success)]">${paidCount}</span>/${unpaidCount}</div></div><div class="summary-block"><div class="label">Cash</div><div class="value">₹${cashTotal.toFixed(2)}</div></div><div class="summary-block"><div class="label">ePay</div><div class="value">₹${ePayTotal.toFixed(2)}</div></div>`;
        }
        
        // --- FILTER & PARSING LOGIC ---
        function parseRange(rangeStr) {
            const result = new Set(); if (!rangeStr) return result;
            rangeStr.split(',').map(s=>s.trim()).forEach(part => {
                if (part.includes('-')) { const [start, end] = part.split('-').map(Number); if (!isNaN(start) && !isNaN(end)) for (let i = start; i <= end; i++) result.add(i); } 
                else { const num = Number(part); if (!isNaN(num)) result.add(num); }
            });
            return result;
        }
        function getRangeFilteredStudents(tab) {
            const activeClass = getActiveClass(); if (!activeClass) return [];
            let studentsToDisplay = [...activeClass.students];
            const uiState = state.ui[tab];
            if (uiState.searchText) {
                const lowerSearchText = uiState.searchText.toLowerCase();
                studentsToDisplay = studentsToDisplay.filter(student => uiState.searchMode === 'name' ? student.name.toLowerCase().includes(lowerSearchText) : student.reg.toLowerCase().includes(lowerSearchText));
            }
            const { filterType, filterRangePreset, customRange } = uiState;
            if (filterType === 'all' || filterRangePreset === 'all') return studentsToDisplay;
            let rangeSet = null;
            if (filterRangePreset === '1-50') rangeSet = parseRange('1-50');
            else if (filterRangePreset.startsWith('51-')) rangeSet = parseRange(filterRangePreset);
            else if (filterRangePreset === 'custom' && customRange) rangeSet = parseRange(customRange);
            if (rangeSet && rangeSet.size > 0) {
                studentsToDisplay = studentsToDisplay.filter(student => {
                    const valueToCheck = (filterType === 'index') ? student.id : parseInt(student.reg.slice(-3));
                    return rangeSet.has(valueToCheck);
                });
            } else if (filterRangePreset !== 'custom') { studentsToDisplay = []; }
            return studentsToDisplay;
        }
        function getFilteredStudents(tab) {
            const uiState = state.ui[tab];
            let studentsToDisplay = getRangeFilteredStudents(tab);
            if (tab === 'attendance') {
                const { statusFilter } = uiState;
                if (statusFilter === 'present') studentsToDisplay = studentsToDisplay.filter(s => s.present === true);
                else if (statusFilter === 'absent') studentsToDisplay = studentsToDisplay.filter(s => s.present !== true);
            } else if (tab === 'payments') {
                const { statusFilter, paymentMethodFilter } = uiState;
                studentsToDisplay = studentsToDisplay.filter(student => {
                    const isPaid = !!student.payment; const method = isPaid ? student.payment.method : null;
                    const statusMatch = (statusFilter === 'all') || (statusFilter === 'paid' && isPaid) || (statusFilter === 'unpaid' && !isPaid);
                    if (!statusMatch) return false;
                    const methodMatch = (paymentMethodFilter === 'all') || !isPaid || (isPaid && method === paymentMethodFilter);
                    return methodMatch;
                });
            }
            uiState.filteredStudents = studentsToDisplay; return studentsToDisplay;
        }
        
        // --- EVENT HANDLERS ---
        function setupEventListeners() {
            document.querySelectorAll('.nav-btn').forEach(btn => btn.addEventListener('click', () => switchTab(btn.dataset.tab)));
            document.getElementById('reset-app-btn').addEventListener('click', () => showConfirmation("This will delete all classes and data permanently. Are you sure?", () => { localStorage.clear(); location.reload(); }));
            document.getElementById('import-json-btn-menu').addEventListener('click', () => document.getElementById('import-json-input').click());
            document.getElementById('import-json-input').addEventListener('change', handleImportData);
            menuBtn.addEventListener('click', (e) => { e.stopPropagation(); menuOptions.classList.toggle('hidden'); });
            ['export-pdf', 'export-png', 'export-excel', 'export-json'].forEach(id => {
                document.getElementById(id).addEventListener('click', e => { e.preventDefault(); handleExport(id.split('-')[1]); });
            });
            undoBtn.addEventListener('click', undo); redoBtn.addEventListener('click', redo);
            document.getElementById('theme-toggle').addEventListener('click', toggleTheme);
            document.getElementById('modal-cancel').addEventListener('click', hideConfirmation);
            window.addEventListener('click', (e) => {
                if (!menuBtn.parentElement.contains(e.target)) menuOptions.classList.add('hidden');
                document.querySelectorAll('.export-history-options').forEach(el => el.classList.add('hidden'));
            });
            mainContent.addEventListener('input', handleMainContentInput);
            mainContent.addEventListener('click', handleMainContentClick);
            mainContent.addEventListener('change', handleMainContentChange);
            const filterModal = document.getElementById('payments-filter-modal');
            document.getElementById('close-payments-filter-modal-btn').addEventListener('click', closePaymentsFilterModal);
            document.getElementById('apply-payments-filters-btn').addEventListener('click', () => { closePaymentsFilterModal(); updatePaymentsList(); });
            document.getElementById('reset-payments-filters-btn').addEventListener('click', () => {
                Object.assign(state.ui.payments, defaultState.ui.payments); renderPaymentsFilterModalContent(); updatePaymentsList();
            });
            filterModal.addEventListener('click', (e) => {
                const btn = e.target.closest('button[data-value]'); if (!btn) return;
                const group = btn.parentElement.dataset.filterGroup; const value = btn.dataset.value;
                if (group && state.ui.payments[group] !== value) { state.ui.payments[group] = value; if (group === 'filterType') state.ui.payments.filterRangePreset = 'all'; renderPaymentsFilterModalContent(); }
            });
            filterModal.addEventListener('input', e => { if(e.target.id === 'payments-modal-custom-range') state.ui.payments.customRange = e.target.value; });
            document.getElementById('import-session-input').addEventListener('change', handleImportSession);
            
            // Roster Manager Listeners
            document.getElementById('close-roster-manager-btn').addEventListener('click', closeRosterManager);
            document.getElementById('save-class-btn').addEventListener('click', saveClass);
            document.getElementById('delete-class-btn').addEventListener('click', deleteClass);
            document.getElementById('roster-manager-content').addEventListener('click', e => {
                if (e.target.closest('#add-student-btn')) addStudentToRoster();
                if (e.target.closest('.delete-student-btn')) e.target.closest('tr').remove();
            });
        }
        
        function handleMainContentClick(e) {
            const tab = state.currentTab;
            let stateChanged = false;
            const activeClass = getActiveClass();

            if (tab === 'dashboard') {
                if (e.target.closest('#add-class-btn')) { openRosterManager(); return; }
                const classCard = e.target.closest('.class-card');
                if (classCard) {
                    const classId = classCard.dataset.classId;
                    if (e.target.closest('.edit-class-btn')) { openRosterManager(classId); return; }
                    if (e.target.closest('.toggle-pin-btn')) {
                        const cls = state.classes.find(c=>c.id === classId);
                        if(cls) cls.isPinned = !cls.isPinned;
                        saveState(); recordState(); renderApp();
                        return;
                    }
                    if (state.activeClassId !== classId) {
                        state.activeClassId = classId;
                        recordState(); saveState(); renderApp();
                    }
                    return;
                }
                const historySection = e.target.closest('#history-section');
                if (historySection) {
                    if (e.target.closest('#import-session-btn')) { document.getElementById('import-session-input').click(); return; }
                    if (e.target.closest('#close-history-detail')) { state.ui.history.viewingItem = null; renderHistoryLists(); return; }
                    const historyCard = e.target.closest('[data-id]');
                    if (historyCard) {
                        const { id, type } = historyCard.dataset;
                        if (e.target.closest('.view-history-btn')) { viewHistoryDetail(id, type); return; }
                        if (e.target.closest('.delete-history-btn')) { deleteHistoryItem(id, type); return; }
                        if (e.target.closest('.export-history-btn')) { e.stopPropagation(); historyCard.querySelector('.export-history-options')?.classList.toggle('hidden'); return; }
                        const exportOption = e.target.closest('.export-history-option');
                        if (exportOption) { e.preventDefault(); const historyList = type === 'attendance' ? activeClass.attendanceHistory : activeClass.paymentsHistory; const item = historyList.find(i => i.id === id); if (item) handleExport(exportOption.dataset.format, item); return; }
                    }
                }
            }

            if (!activeClass) return;

            const filterTypeBtn = e.target.closest('[data-filter-type]');
            if (filterTypeBtn) { const newFilterType = filterTypeBtn.dataset.filterType; if(state.ui[tab].filterType !== newFilterType) { state.ui[tab].filterType = newFilterType; state.ui[tab].filterRangePreset = 'all'; } renderCurrentTab(); return; }
            
            const filterRangeBtn = e.target.closest('[data-filter-range]');
            if (filterRangeBtn) { state.ui[tab].filterRangePreset = filterRangeBtn.dataset.filterRange; renderCurrentTab(); return; }
            
            const searchModeBtn = e.target.closest('.search-mode-btn');
            if (searchModeBtn) { state.ui[tab].searchMode = searchModeBtn.dataset.searchMode; renderCurrentTab(); return; }
            
            const statusFilterBtn = e.target.closest('.summary-btn[data-status-filter]');
            if(statusFilterBtn) { state.ui.attendance.statusFilter = statusFilterBtn.dataset.statusFilter; updateAttendanceList(); document.querySelectorAll('.summary-btn[data-status-filter]').forEach(btn => btn.classList.remove('active')); statusFilterBtn.classList.add('active'); return; }
            
            if (e.target.closest('#open-payments-filter-modal-btn')) { openPaymentsFilterModal(); return; }
            
            const studentCard = e.target.closest('.student-card');
            if (studentCard && tab === 'attendance') {
                const student = activeClass.students.find(s => s.reg === studentCard.dataset.reg);
                if (student) {
                    student.present = student.present === true ? undefined : true;
                    studentCard.classList.toggle('bg-[var(--md-sys-color-primary-container)]');
                    studentCard.classList.toggle('border-[var(--md-sys-color-primary)]', student.present);
                    studentCard.classList.toggle('border-[var(--md-sys-color-outline)]', !student.present);
                    recordState(); debouncedAutosave(); updateAttendanceSummary();
                }
                return;
            }

            const targetId = e.target.closest('button')?.id;
            switch (targetId) {
                case 'save-attendance-btn': saveAttendance(); break;
                case 'save-payments-btn': savePaymentsCollection(); break;
                case 'reset-attendance-btn': activeClass.students.forEach(s => s.present = undefined); stateChanged = true; break;
                case 'reset-payments-btn': activeClass.students.forEach(s => s.payment = null); stateChanged = true; break;
                case 'mark-all-visible-present':
                case 'clear-all-visible':
                    const markPresent = targetId === 'mark-all-visible-present';
                    state.ui.attendance.filteredStudents.forEach(fs => {
                        const student = activeClass.students.find(s => s.reg === fs.reg);
                        if(student) student.present = markPresent ? true : undefined;
                    });
                    stateChanged = true;
                    break;
            }

            if(stateChanged) { recordState(); debouncedAutosave(); renderCurrentTab(); }
        };

        // --- Other functions remain unchanged from previous state ---
        function openRosterManager(classId = null) {
            state.ui.roster.editingClassId = classId;
            const classData = classId ? deepClone(state.classes.find(c => c.id === classId)) : { name: '', batch: '', iconNumber: 1, students: [] };
            if (!classData) return;
            document.getElementById('roster-manager-content').innerHTML = getRosterManagerModalHTML(classData);
            document.getElementById('roster-modal-title').textContent = classId ? 'Edit Class' : 'Add New Class';
            document.getElementById('delete-class-btn').classList.toggle('hidden', !classId);
            document.getElementById('roster-manager-modal').classList.add('visible');
        }
        function closeRosterManager() { document.getElementById('roster-manager-modal').classList.remove('visible'); state.ui.roster.editingClassId = null; }
        function saveClass() {
            const id = state.ui.roster.editingClassId;
            const name = document.getElementById('roster-class-name').value.trim();
            if (!name) { alert("Class name cannot be empty."); return; }
            
            const studentRows = document.querySelectorAll('#roster-student-list tr');
            const students = Array.from(studentRows).map((row, index) => ({
                id: index + 1,
                reg: row.querySelector('.roster-student-reg').value.trim(),
                name: row.querySelector('.roster-student-name').value.trim(),
                present: undefined,
                payment: null,
            }));

            if (id) { // Update existing class
                const classIndex = state.classes.findIndex(c => c.id === id);
                if (classIndex > -1) {
                    state.classes[classIndex].name = name;
                    state.classes[classIndex].batch = document.getElementById('roster-class-batch').value.trim();
                    state.classes[classIndex].iconNumber = document.getElementById('roster-class-icon').value;
                    const oldStudents = state.classes[classIndex].students;
                    state.classes[classIndex].students = students.map(newStudent => {
                        const oldStudent = oldStudents.find(s => s.reg === newStudent.reg);
                        return oldStudent ? { ...newStudent, present: oldStudent.present, payment: oldStudent.payment } : newStudent;
                    });
                }
            } else { // Create new class
                const newClass = {
                    id: `class-${Date.now()}`,
                    name: name,
                    batch: document.getElementById('roster-class-batch').value.trim(),
                    iconNumber: document.getElementById('roster-class-icon').value,
                    isPinned: false,
                    students: students,
                    attendanceHistory: [],
                    paymentsHistory: []
                };
                state.classes.push(newClass);
                state.activeClassId = newClass.id;
            }
            closeRosterManager();
            saveState(); recordState(); renderApp();
        }
        function deleteClass() {
            const id = state.ui.roster.editingClassId;
            if (!id) return;
            showConfirmation("Are you sure you want to delete this class and all its data? This cannot be undone.", () => {
                state.classes = state.classes.filter(c => c.id !== id);
                if (state.activeClassId === id) {
                    state.activeClassId = state.classes.length > 0 ? state.classes[0].id : null;
                }
                closeRosterManager();
                saveState(); recordState(); renderApp();
            });
        }
        function addStudentToRoster() {
            const listEl = document.getElementById('roster-student-list');
            const newIndex = listEl.children.length + 1;
            const newRow = document.createElement('tr');
            newRow.dataset.studentId = newIndex;
            newRow.innerHTML = `
                <td class="p-2 border-b border-[var(--md-sys-color-outline)]">${newIndex}</td>
                <td class="p-2 border-b border-[var(--md-sys-color-outline)]"><input type="text" placeholder="Reg No." class="!p-2 roster-student-reg"></td>
                <td class="p-2 border-b border-[var(--md-sys-color-outline)]"><input type="text" placeholder="Name" class="!p-2 roster-student-name"></td>
                <td class="p-2 border-b border-[var(--md-sys-color-outline)] text-center"><button class="delete-student-btn btn btn-icon !p-2 text-[var(--md-sys-color-error)]"><svg class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg></button></td>
            `;
            listEl.appendChild(newRow);
            newRow.querySelector('input').focus();
        }
        function showConfirmation(message, onConfirm) { modalMessage.textContent = message; modal.classList.add('visible'); const confirmBtn = document.getElementById('modal-confirm'); const confirmHandler = () => { onConfirm(); hideConfirmation(); confirmBtn.removeEventListener('click', confirmHandler); }; confirmBtn.addEventListener('click', confirmHandler); document.getElementById('modal-cancel').addEventListener('click', () => { hideConfirmation(); confirmBtn.removeEventListener('click', confirmHandler); }, {once: true}); };
        function hideConfirmation() { modal.classList.remove('visible'); const confirmBtn = document.getElementById('modal-confirm'); const newConfirmBtn = confirmBtn.cloneNode(true); confirmBtn.parentNode.replaceChild(newConfirmBtn, confirmBtn); };
        function handleMainContentInput(e) { const tab = state.currentTab; const target = e.target; const targetId = target.id; if (targetId === 'history-search-input') { state.ui.history.searchText = target.value; renderHistoryLists(); return; } if (tab === 'dashboard') return; if (targetId === `${tab}-search-input`) { state.ui[tab].searchText = target.value; if (tab === 'attendance') updateAttendanceList(); else if (tab === 'payments') updatePaymentsList(); return; } if (targetId === `${tab}-custom-range`) { state.ui[tab].customRange = target.value; if (state.ui[tab].filterRangePreset === 'custom') { if (tab === 'attendance') updateAttendanceList(); else if (tab === 'payments') updatePaymentsList(); } return; } if (target.matches('#payments-purpose, #payments-amount, #payments-collector')) { state.ui.payments.purpose = document.getElementById('payments-purpose')?.value; state.ui.payments.defaultAmount = document.getElementById('payments-amount')?.value; state.ui.payments.collector = document.getElementById('payments-collector')?.value; debouncedAutosave(); if (targetId === 'payments-amount') updatePaymentsSummary(); }};
        function handleMainContentChange(e) { let stateChanged = false; const activeClass = getActiveClass(); if (!activeClass) return; if (e.target.classList.contains('payments-student-checkbox')) { const student = activeClass.students.find(s => s.reg === e.target.dataset.reg); if (student) { if (e.target.checked) { const defaultAmount = document.getElementById('payments-amount')?.value || 0; student.payment = { amount: defaultAmount, method: 'Cash' }; } else { student.payment = null; } stateChanged = true; } } if (e.target.classList.contains('payments-amount-input') || e.target.classList.contains('payments-method-select')) { const student = activeClass.students.find(s => s.reg === e.target.dataset.reg); if (student && student.payment) { student.payment.amount = mainContent.querySelector(`.payments-amount-input[data-reg="${student.reg}"]`).value; student.payment.method = mainContent.querySelector(`.payments-method-select[data-reg="${student.reg}"]`).value; stateChanged = true; } } if(stateChanged) { recordState(); debouncedAutosave(); renderCurrentTab(); }};
        function openPaymentsFilterModal() { renderPaymentsFilterModalContent(); document.getElementById('payments-filter-modal').classList.add('visible'); };
        function closePaymentsFilterModal() { document.getElementById('payments-filter-modal').classList.remove('visible'); };
        function renderPaymentsFilterModalContent() { document.getElementById('payments-filter-modal-content').innerHTML = getPaymentsFilterModalHTML(); };
        function saveAttendance() {
            const activeClass = getActiveClass(); if (!activeClass) return;
            const now = new Date();
            const recordsToSave = activeClass.students.map(({ reg, name, present }) => ({ reg, name, present: present === true }));
            if(recordsToSave.filter(r => r.present).length === 0) { alert("No students marked as Present."); return; }
            const session = { id: `att-${Date.now()}`, type: 'attendance', date: now.toLocaleDateString(), time: now.toLocaleTimeString(), takenBy: document.getElementById('taken-by-input').value || 'N/A', subject: document.getElementById('subject-input').value || 'N/A', records: recordsToSave };
            activeClass.attendanceHistory.unshift(session);
            alert("Attendance saved!");
            
            const wip = JSON.parse(localStorage.getItem('studentMarkerWorkInProgress') || '{}');
            if (wip[activeClass.id]) delete wip[activeClass.id].attendance;
            localStorage.setItem('studentMarkerWorkInProgress', JSON.stringify(wip));
            
            activeClass.students.forEach(s => s.present = undefined);
            saveState(); recordState(); renderApp();
        }
        function savePaymentsCollection() {
            const activeClass = getActiveClass(); if (!activeClass) return;
            const now = new Date();
            const studentsInView = getFilteredStudents('payments');
            const recordsForSession = studentsInView.map(s => ({ reg: s.reg, name: s.name, payment: s.payment ? deepClone(s.payment) : null }));
            if (recordsForSession.filter(r => r.payment).length === 0) { alert("No payments recorded."); return; }
            
            const sessionContext = { totalStudentsInView: studentsInView.length, defaultAmount: state.ui.payments.defaultAmount, filterDescription: getPaymentsFilterDescription() };
            const session = { id: `pay-${Date.now()}`, type: 'payments', date: now.toLocaleDateString(), time: now.toLocaleTimeString(), collector: state.ui.payments.collector || 'N/A', purpose: state.ui.payments.purpose || 'General Collection', records: recordsForSession, context: sessionContext };
            activeClass.paymentsHistory.unshift(session);
            alert("Payments collection saved!");

            const wip = JSON.parse(localStorage.getItem('studentMarkerWorkInProgress') || '{}');
            if (wip[activeClass.id]) { delete wip[activeClass.id].payments; delete wip[activeClass.id].paymentsUi; }
            localStorage.setItem('studentMarkerWorkInProgress', JSON.stringify(wip));

            activeClass.students.forEach(s => s.payment = null);
            saveState(); recordState(); renderApp();
        }
        function handleImportData(event) {
            const file = event.target.files[0]; if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const data = JSON.parse(e.target.result);
                    if (!data.classes || !data.activeClassId) throw new Error("Invalid format");
                    state = data; saveState(); recordState(); renderApp();
                    alert('Data imported successfully!');
                } catch (error) { alert('Failed to import data.'); }
            };
            reader.readAsText(file);
        }
        function handleImportSession(event) {
            const activeClass = getActiveClass(); if (!activeClass) { alert("Please select a class first."); return; }
            const file = event.target.files[0]; if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const item = JSON.parse(e.target.result);
                    if (!item.id || !item.type || !item.records) throw new Error("Invalid session file format.");
                    item.isImported = true; item.id = `imp-${item.type}-${Date.now()}`;
                    if (item.type === 'attendance') activeClass.attendanceHistory.unshift(item);
                    else if (item.type === 'payments') activeClass.paymentsHistory.unshift(item);
                    else throw new Error("Unknown session type.");
                    saveState(); recordState(); renderHistoryLists();
                    alert('Session imported successfully!');
                } catch (error) { alert(`Failed to import session: ${error.message}`); } 
                finally { event.target.value = ''; }
            };
            reader.readAsText(file);
        }
        function viewHistoryDetail(id, type) {
            const activeClass = getActiveClass(); if (!activeClass) return;
            const historyList = type === 'attendance' ? activeClass.attendanceHistory : activeClass.paymentsHistory;
            state.ui.history.viewingItem = historyList.find(i => i.id === id);
            renderHistoryLists();
        }
        function deleteHistoryItem(id, type) {
            const activeClass = getActiveClass(); if (!activeClass) return;
            showConfirmation("Delete this record permanently?", () => {
                if (type === 'attendance') activeClass.attendanceHistory = activeClass.attendanceHistory.filter(item => item.id !== id);
                else activeClass.paymentsHistory = activeClass.paymentsHistory.filter(item => item.id !== id);
                saveState(); recordState(); renderApp();
            });
        }
        function getFilterDescription(tab) { return 'All Students'; }
        function getPaymentsFilterDescription() { return 'All Students'; }
        function getCurrentTabDataForExport() { const now = new Date(); const tab = state.currentTab; const activeClass = getActiveClass(); if (!activeClass) return null; if (tab === 'attendance' || tab === 'payments') { const filteredStudents = getRangeFilteredStudents(tab); if (tab === 'attendance') { return { type: 'attendance', date: now.toLocaleDateString(), time: now.toLocaleTimeString(), takenBy: document.getElementById('taken-by-input')?.value, subject: document.getElementById('subject-input')?.value || 'Current View', records: filteredStudents.map(s => ({ reg: s.reg, name: s.name, present: s.present === true })), filterDescription: getFilterDescription('attendance') }; } else { return { type: 'payments', date: now.toLocaleDateString(), time: now.toLocaleTimeString(), collector: state.ui.payments.collector, purpose: state.ui.payments.purpose || 'Current Collection', records: filteredStudents.map(s => ({ reg: s.reg, name: s.name, payment: s.payment })), defaultAmount: state.ui.payments.defaultAmount, filterDescription: getPaymentsFilterDescription() }; } } return null; };
        function handleExport(format, specificItem = null) { if (format === 'json') { const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(state, null, 2)); const a = document.createElement('a'); a.href = dataStr; a.download = "student_marker_backup.json"; a.click(); a.remove(); return; } const itemToExport = specificItem || getCurrentTabDataForExport(); if (!itemToExport) { alert("No data to export."); return; } const exportContentWrapper = document.getElementById('export-content-wrapper'); exportContentWrapper.innerHTML = `<div class="export-document">${getExportHTML(itemToExport)}</div>`; const exportContentEl = exportContentWrapper.firstChild; const filename = `${itemToExport.type}-${(itemToExport.subject || itemToExport.purpose).replace(/\W/g, '_')}-${itemToExport.date.replace(/\//g, '-')}`; if (format === 'pdf') { html2canvas(exportContentEl, { scale: 3 }).then(canvas => { const imgData = canvas.toDataURL('image/png'); const pdf = new jsPDF('p', 'mm', 'a4'); const pdfWidth = pdf.internal.pageSize.getWidth(); const pageHeight = pdf.internal.pageSize.getHeight(); const imgHeight = canvas.height * pdfWidth / canvas.width; let heightLeft = imgHeight, position = 0; pdf.addImage(imgData, 'PNG', 0, position, pdfWidth, imgHeight); heightLeft -= pageHeight; while (heightLeft > 0) { position -= pageHeight; pdf.addPage(); pdf.addImage(imgData, 'PNG', 0, position, pdfWidth, imgHeight); heightLeft -= pageHeight; } pdf.save(`${filename}.pdf`); }); } else if (format === 'png') { html2canvas(exportContentEl, { scale: 2 }).then(canvas => { const link = document.createElement('a'); link.href = canvas.toDataURL('image/png'); link.download = `${filename}.png`; link.click(); }); } else if (format === 'excel') { /* Excel logic here */ } else if (format === 'session_json') { const sessionData = JSON.stringify(itemToExport, null, 2); const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(sessionData); const a = document.createElement('a'); a.href = dataStr; a.download = `${filename}_session.json`; a.click(); a.remove(); } menuOptions.classList.add('hidden'); };
        function getExportHTML(item) {
            const records = item.records || item.studentRecords;
            let metaInfo = `<div class="meta-info"><strong>Date:</strong> ${item.date} at ${item.time}<br>`;
            if (item.takenBy) metaInfo += `<strong>Taken by:</strong> ${item.takenBy}<br>`;
            if (item.collector) metaInfo += `<strong>Collected by:</strong> ${item.collector}<br>`;
            const filterDescription = item.context?.filterDescription || item.filterDescription;
            if (filterDescription) metaInfo += `<strong>Filter:</strong> ${filterDescription}`;
            metaInfo += `</div>`;
            let content = `<h1>${item.type === 'attendance' ? 'Attendance Report' : 'Payments Report'}</h1><h2>${item.subject || item.purpose}</h2>${metaInfo}`;
            if (item.type === 'attendance') {
                const present = records.filter(r => r.present).length;
                const absent = records.length - present;
                content += `<div class="summary-section"><div class="summary-item"><div class="label">Total</div><div class="value">${records.length}</div></div><div class="summary-item"><div class="label">Present</div><div class="value present">${present}</div></div><div class="summary-item"><div class="label">Absent</div><div class="value absent">${absent}</div></div></div>`;
                content += `<h3>Present (${present})</h3>`; if(present > 0) { content += `<table><thead><tr><th>Reg No</th><th>Name</th></tr></thead><tbody>${records.filter(r => r.present).map(r => `<tr><td>${r.reg}</td><td>${r.name}</td></tr>`).join('')}</tbody></table>`; } else { content += `<p>None</p>`; }
                content += `<h3>Absent (${absent})</h3>`; if(absent > 0) { content += `<table><thead><tr><th>Reg No</th><th>Name</th></tr></thead><tbody>${records.filter(r => !r.present).map(r => `<tr><td>${r.reg}</td><td>${r.name}</td></tr>`).join('')}</tbody></table>`; } else { content += `<p>None</p>`; }
            } else if (item.type === 'payments') {
                const paidRecords = records.filter(r => r.payment); const unpaidRecords = records.filter(r => !r.payment); const totalCollected = paidRecords.reduce((sum, r) => sum + parseFloat(r.payment.amount || 0), 0);
                let totalExpected, unpaidCount, dueAmount; unpaidCount = unpaidRecords.length;
                if (item.context) { const defaultAmount = parseFloat(item.context.defaultAmount || 0); totalExpected = defaultAmount > 0 ? item.context.totalStudentsInView * defaultAmount : totalCollected; }
                else if (item.defaultAmount !== undefined) { const defaultAmount = parseFloat(item.defaultAmount || 0); totalExpected = defaultAmount > 0 ? records.length * defaultAmount : totalCollected; }
                else { totalExpected = totalCollected; unpaidCount = item.context?.totalStudentsInView ? item.context.totalStudentsInView - paidRecords.length : 0; }
                dueAmount = Math.max(0, totalExpected - totalCollected);
                const cashTotal = paidRecords.filter(s => s.payment.method === 'Cash').reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
                const ePayTotal = paidRecords.filter(s => s.payment.method === 'ePay').reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
                content += `<div class="summary-section"><div class="summary-item"><div class="label">Budget</div><div class="value">₹${totalExpected.toFixed(2)}</div></div><div class="summary-item"><div class="label">Received</div><div class="value received">₹${totalCollected.toFixed(2)}</div></div><div class="summary-item"><div class="label">Due</div><div class="value due">₹${dueAmount.toFixed(2)}</div></div><div class="summary-item"><div class="label">Paid/Unpaid</div><div class="value"><span class="present">${paidRecords.length}</span> / <span class="absent">${unpaidCount}</span></div></div><div class="summary-item"><div class="label">Cash</div><div class="value">₹${cashTotal.toFixed(2)}</div></div><div class="summary-item"><div class="label">ePay</div><div class="value">₹${ePayTotal.toFixed(2)}</div></div></div>`;
                content += `<h3>Paid (${paidRecords.length})</h3>`; if(paidRecords.length > 0) { content += `<table><thead><tr><th>Reg No</th><th>Name</th><th>Amount (₹)</th><th>Method</th></tr></thead><tbody>${paidRecords.map(r => `<tr><td>${r.reg}</td><td>${r.name}</td><td>${r.payment.amount}</td><td>${r.payment.method}</td></tr>`).join('')}</tbody></table>`; } else { content += `<p>None</p>`; }
                content += `<h3>Pending (${unpaidCount})</h3>`; if (unpaidRecords.length > 0) { content += `<table><thead><tr><th>Reg No</th><th>Name</th></tr></thead><tbody>${unpaidRecords.map(r => `<tr><td>${r.reg}</td><td>${r.name}</td></tr>`).join('')}</tbody></table>`; } else if (unpaidCount > 0 && unpaidRecords.length === 0) { content += `<p>The list of pending students is not available for this legacy record.</p>`; } else { content += `<p>None</p>`; }
            }
            content += `<div class="footer">Generated by ATTEND. & MC app on ${new Date().toLocaleString()}</div>`; return content;
        };

        // --- FINAL BOOTSTRAP ---
        init();
    });
    </script>
</body>
</html>
