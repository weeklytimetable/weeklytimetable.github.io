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
            /* Light Theme */
            --bg-color: #eef2ff; /* indigo-50 */
            --bg-color-2: #e0e7ff; /* indigo-100 */
            --text-primary: #1e1b4b; /* indigo-950 */
            --text-secondary: #4338ca; /* indigo-700 */
            --accent-color: #4f46e5; /* indigo-600 */
            --accent-glow: rgba(79, 70, 229, 0.4);
            --card-bg: rgba(255, 255, 255, 0.6);
            --border-color: rgba(199, 210, 254, 0.8); /* indigo-200 */
            --soft-shadow-1: 6px 6px 12px #d9ddef;
            --soft-shadow-2: -6px -6px 12px #ffffff;
            --icon-color: #312e81; /* indigo-900 */
            --icon-color-active: #4f46e5;
            --success-color: #16a34a; /* green-600 */
            --danger-color: #dc2626; /* red-600 */
            --success-bg: rgba(34, 197, 94, 0.1);
            --danger-bg: rgba(220, 38, 38, 0.1);
        }

        [data-theme="dark"] {
            --bg-color: #111827; /* gray-900 */
            --bg-color-2: #1f2937; /* gray-800 */
            --text-primary: #f9fafb; /* gray-50 */
            --text-secondary: #9ca3af; /* gray-400 */
            --accent-color: #818cf8; /* indigo-400 */
            --accent-glow: rgba(129, 140, 248, 0.4);
            --card-bg: rgba(55, 65, 81, 0.5); /* gray-700 with alpha */
            --border-color: rgba(75, 85, 99, 0.6); /* gray-600 with alpha */
            --soft-shadow-1: 6px 6px 12px #0b0f1a;
            --soft-shadow-2: -6px -6px 12px #172134;
            --icon-color: #d1d5db; /* gray-300 */
            --icon-color-active: #818cf8;
            --success-color: #4ade80; /* green-400 */
            --danger-color: #f87171; /* red-400 */
            --success-bg: rgba(74, 222, 128, 0.1);
            --danger-bg: rgba(248, 113, 113, 0.1);
        }
        
        /* Base Styles */
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-primary);
            transition: background-color 0.3s ease, color 0.3s ease;
        }
        
        .app-container {
            padding-bottom: 9rem; /* Increased space for bottom nav */
        }
        
        /* Glassmorphism & Soft UI Components */
        .glass-card {
            background: var(--card-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border-radius: 1.25rem;
            border: 1px solid var(--border-color);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.1);
            padding: 1.5rem;
        }

        .soft-btn {
            background: var(--bg-color);
            color: var(--text-secondary);
            border: 1px solid transparent;
            border-radius: 0.75rem;
            box-shadow: var(--soft-shadow-1), var(--soft-shadow-2);
            transition: all 0.2s ease-in-out;
            cursor: pointer;
            font-weight: 600;
            padding: 0.75rem 1rem;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }
        .soft-btn:hover {
            color: var(--accent-color);
            border-color: var(--border-color);
        }
        .soft-btn:active, .soft-btn.active {
            font-weight: 700;
            color: var(--accent-color);
            background-color: var(--bg-color-2);
            box-shadow: none;
            outline: 2px solid var(--accent-glow);
        }
        .soft-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            box-shadow: none;
        }
        
        input[type="text"], input[type="number"], input[type="file"], select {
            background-color: var(--bg-color-2);
            border: 1px solid transparent;
            border-radius: 0.75rem;
            padding: 0.75rem;
            width: 100%;
            color: var(--text-primary);
            box-shadow: inset 2px 2px 4px color-mix(in srgb, var(--bg-color) 70%, black 30%), inset -2px -2px 4px color-mix(in srgb, var(--bg-color) 70%, white 30%);
            transition: box-shadow 0.2s, border-color 0.2s;
        }
        input:focus, select:focus {
            outline: none;
            border-color: var(--accent-color);
            box-shadow: 0 0 0 2px var(--accent-glow);
        }
        
        /* Layout: Header & Bottom Nav */
        .app-header {
            background: var(--card-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
        }

        .bottom-nav {
            box-shadow: 0 -4px 30px rgba(0, 0, 0, 0.1);
        }
        .nav-btn {
            display: flex; flex-direction: column; align-items: center; gap: 4px;
            color: var(--icon-color); font-weight: 500; font-size: 0.75rem;
            transition: all 0.2s ease; position: relative; padding: 8px; border-radius: 12px;
        }
        .nav-btn.active {
            color: var(--icon-color-active);
            font-weight: 700;
        }
        .nav-btn.active::after {
            content: '';
            position: absolute; bottom: -4px;
            width: 8px; height: 8px;
            background-color: var(--accent-color);
            border-radius: 50%;
            box-shadow: 0 0 10px var(--accent-glow);
        }

        /* Functional Summary Buttons */
        .summary-btn {
            padding: 0.5rem;
            border-radius: 0.75rem;
            cursor: pointer;
            transition: all 0.2s ease;
            border: 2px solid transparent;
            background-color: var(--bg-color-2);
        }
        .summary-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
        .summary-btn.active {
            font-weight: 700;
            box-shadow: 0 0 0 3px var(--accent-glow);
            transform: translateY(-2px);
        }
        .summary-btn[data-status-filter="present"].active { border-color: var(--success-color); }
        .summary-btn[data-status-filter="absent"].active { border-color: var(--danger-color); }

        /* Animations */
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .fade-in { animation: fadeIn 0.5s ease-out forwards; }
        
        /* Custom scrollbar */
        .custom-scrollbar::-webkit-scrollbar { width: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: transparent; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: var(--accent-color); border-radius: 3px; }
        
        /* Modal Styles */
        .modal-overlay {
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(0, 0, 0, 0.4);
            backdrop-filter: blur(8px); -webkit-backdrop-filter: blur(8px);
            display: flex; justify-content: center; align-items: center;
            z-index: 100; opacity: 0; pointer-events: none; transition: opacity 0.3s;
        }
        .modal-overlay.visible { opacity: 1; pointer-events: auto; }
        .modal-content {
            transform: scale(0.95); transition: transform 0.3s;
            max-width: 500px; width: 90%;
        }
        .modal-overlay.visible .modal-content { transform: scale(1); }

        /* Hidden div for exporting */
        .export-container { position: absolute; left: -9999px; top: auto; width: 850px; }
        
        /* Styles for professional document export/view */
        .export-document {
            font-family: Arial, sans-serif;
            padding: 2rem;
            background-color: #ffffff;
            border: 1px solid #ddd;
        }
        .export-document, .export-document *, .export-document p, .export-document div, .export-document td, .export-document th {
            color: #333 !important;
        }
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
        
        /* Redesign Styles */
        .control-group {
            border: 1px solid var(--border-color);
            border-radius: 1rem;
            padding: 1rem;
            display: flex;
            flex-direction: column;
            gap: 0.75rem;
        }
        .search-container {
            position: relative;
            display: flex;
            align-items: center;
            width: 100%;
        }
        .search-container input {
            padding-right: 8.5rem !important;
        }
        .search-icons {
            position: absolute;
            right: 0.5rem;
            top: 50%;
            transform: translateY(-50%);
            display: flex;
            align-items: center;
            gap: 0.25rem;
            background-color: var(--bg-color-2);
        }
        .search-clear-btn {
            position: absolute;
            right: 6.25rem;
            top: 50%;
            transform: translateY(-50%);
            color: var(--icon-color);
            padding: 0.25rem;
            border-radius: 99px;
            cursor: pointer;
            background-color: transparent;
            border: none;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }
        .search-clear-btn:hover {
            background-color: var(--bg-color);
        }
        .search-mode-btn-inner {
            background: transparent;
            color: var(--text-secondary);
            border: 1px solid transparent;
            border-radius: 0.5rem;
            box-shadow: none;
            transition: all 0.2s ease-in-out;
            cursor: pointer;
            font-weight: 600;
            padding: 0.5rem;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }
        .search-mode-btn-inner:hover {
            color: var(--accent-color);
            background-color: var(--bg-color);
        }
        .search-mode-btn-inner.active {
            color: var(--accent-color);
            background-color: var(--bg-color);
            box-shadow: inset 2px 2px 4px color-mix(in srgb, var(--bg-color) 70%, black 30%), inset -2px -2px 4px color-mix(in srgb, var(--bg-color) 70%, white 30%);
        }
        #reset-attendance-btn svg, #reset-payments-btn svg {
            color: var(--danger-color);
        }
    </style>
</head>
<body class="antialiased">

    <div id="app-container" class="relative min-h-screen">
        <!-- App Header (Sticky) -->
        <header class="app-header sticky top-0 z-40 p-4">
            <div class="container mx-auto flex justify-between items-center">
                <div>
                    <h1 id="app-title" class="text-xl font-bold transition-all duration-300">Attendance</h1>
                    <p id="total-students-header" class="text-sm text-gray-500">Available: 0</p>
                </div>
                <div class="flex items-center gap-2">
                    <button id="undo-history-btn" class="soft-btn rounded-full !p-2.5 disabled:opacity-50" disabled title="Undo">
                        <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M9 15L3 9m0 0l6-6M3 9h12a6 6 0 010 12h-3" /></svg>
                    </button>
                    <button id="redo-history-btn" class="soft-btn rounded-full !p-2.5 disabled:opacity-50" disabled title="Redo">
                        <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M15 15l6-6m0 0l-6-6m6 6H9a6 6 0 000 12h3" /></svg>
                    </button>
                    <button id="theme-toggle" class="soft-btn rounded-full !p-2.5">
                        <svg id="theme-icon-sun" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" /></svg>
                        <svg id="theme-icon-moon" class="h-5 w-5 hidden" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" /></svg>
                    </button>
                    <div class="relative">
                        <button id="menu-btn" class="soft-btn rounded-full !p-2.5">
                            <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M12 5v.01M12 12v.01M12 19v.01M12 6a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2z" /></svg>
                        </button>
                        <div id="menu-options" class="hidden absolute right-0 mt-2 w-56 glass-card p-2 z-50 rounded-2xl">
                             <a href="#" id="export-pdf" class="block w-full text-left px-4 py-2 text-sm rounded-lg hover:bg-[var(--bg-color-2)]">Export Page as PDF</a>
                             <a href="#" id="export-png" class="block w-full text-left px-4 py-2 text-sm rounded-lg hover:bg-[var(--bg-color-2)]">Export Page as PNG</a>
                             <a href="#" id="export-excel" class="block w-full text-left px-4 py-2 text-sm rounded-lg hover:bg-[var(--bg-color-2)]">Export Page as Excel</a>
                             <div class="border-t my-2 border-[var(--border-color)]"></div>
                             <button id="import-json-btn-menu" class="w-full text-left px-4 py-2 text-sm rounded-lg hover:bg-[var(--bg-color-2)]">Import App Data (JSON)</button>
                             <a href="#" id="export-json" class="block w-full text-left px-4 py-2 text-sm rounded-lg hover:bg-[var(--bg-color-2)]">Export App Data (JSON)</a>
                             <div class="border-t my-2 border-[var(--border-color)]"></div>
                             <button id="reset-app-btn" class="w-full text-left px-4 py-2 text-sm rounded-lg text-[var(--danger-color)] hover:bg-[var(--danger-bg)]">Reset All Data</button>
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
    <nav class="bottom-nav fixed bottom-4 left-1/2 -translate-x-1/2 w-[90%] max-w-sm h-16 glass-card flex justify-around items-center z-50 rounded-full">
        <button class="nav-btn" data-tab="attendance">
            <svg class="h-6 w-6 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
            <span>Attendance</span>
        </button>
        <button class="nav-btn" data-tab="payments">
            <svg class="h-6 w-6 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M2.25 18.75a60.07 60.07 0 0115.797 2.101c.727.198 1.453-.342 1.453-1.096V18.75M3.75 4.5v.75A.75.75 0 013 6h-.75m0 0v-.375c0-.621.504-1.125 1.125-1.125H20.25M2.25 6v9m18-10.5v.75c0 .414.336.75.75.75h.75m-1.5-1.5h.375c.621 0 1.125.504 1.125 1.125v9.75c0 .621-.504 1.125-1.125-1.125h-15c-.621 0-1.125-.504-1.125-1.125v-9.75c0-.621.504-1.125 1.125-1.125h1.5" /></svg>
            <span>Payments</span>
        </button>
        <button class="nav-btn" data-tab="history">
            <svg class="h-6 w-6 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M12 6v6h4.5m4.5 0a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
            <span>History</span>
        </button>
    </nav>
    
    <!-- Modal for Confirmation -->
    <div id="confirmation-modal" class="modal-overlay">
        <div class="modal-content glass-card text-center">
            <p id="modal-message" class="mb-6 text-lg">Are you sure?</p>
            <div class="flex justify-center gap-4">
                <button id="modal-confirm" class="soft-btn flex-1 text-[var(--danger-color)] hover:bg-[var(--danger-bg)]">Confirm</button>
                <button id="modal-cancel" class="soft-btn flex-1">Cancel</button>
            </div>
        </div>
    </div>
    
    <!-- Modal for Payments Filter -->
    <div id="payments-filter-modal" class="modal-overlay">
        <div class="modal-content glass-card !p-4 space-y-4">
            <div class="flex justify-between items-center">
                <h3 class="text-xl font-bold">Filters</h3>
                <button id="close-payments-filter-modal-btn" class="soft-btn !rounded-full !p-2 !text-lg">&times;</button>
            </div>
            <div id="payments-filter-modal-content" class="space-y-4 max-h-[60vh] overflow-y-auto custom-scrollbar pr-2">
                <!-- Content generated by JS -->
            </div>
             <div class="flex justify-end gap-4 pt-4 border-t border-[var(--border-color)]">
                <button id="reset-payments-filters-btn" class="soft-btn text-[var(--danger-color)] hover:bg-[var(--danger-bg)]">Reset</button>
                <button id="apply-payments-filters-btn" class="soft-btn active">Apply</button>
            </div>
        </div>
    </div>
    
    <!-- Input for single session import (moved here to be persistent) -->
    <input type="file" id="import-session-input" class="hidden" accept=".json">
    
    <!-- Hidden container for generating exports -->
    <div class="export-container">
        <div id="export-content-wrapper"></div>
    </div>

    <!-- Main Application Script -->
    <script>
    document.addEventListener('DOMContentLoaded', () => {
        // --- INITIAL DATA & STATE ---
        const initialStudents = [
            { reg: '232131104001', name: 'AARCHA D' }, { reg: '232131104002', name: 'AARTHI A' }, { reg: '232131104003', name: 'ABITHA S' },
            { reg: '232131104005', name: 'AHAMMAD AFSAL' }, { reg: '232131104006', name: 'AJAI J' }, { reg: '232131104007', name: 'AKSHAYA R' },
            { reg: '232131104008', name: 'ALPHY ANN THOMAS' }, { reg: '232131104009', name: 'AMAYA PRAKASH' }, { reg: '232131104010', name: 'ANUSIYA R' },
            { reg: '232131104011', name: 'ARUL JOSHVA F' }, { reg: '232131104012', name: 'ASHIJA S' }, { reg: '232131104013', name: 'ASHLY THOMAS T' },
            { reg: '232131104014', name: 'ASHWIN KUMAR S' }, { reg: '232131104015', name: 'ATCHAYA S' }, { reg: '232131104016', name: 'BENSI B' },
            { reg: '232131104019', name: 'BIJO S L' }, { reg: '232131104020', name: 'BOOMIKA M' }, { reg: '232131104021', name: 'BUGAN T' },
            { reg: '232131104022', name: 'CAROLIN QUETINΑ Η' }, { reg: '232131104023', name: 'DEEPAK KUMAR' }, { reg: '232131104024', name: 'DINAKARAN I' },
            { reg: '232131104025', name: 'DIVYA DHARSHINI A' }, { reg: '232131104026', name: 'DIVYA PRASIKILLA E' }, { reg: '232131104027', name: 'DOMINIC DEVADHAS' },
            { reg: '232131104028', name: 'FATHIMA BEGUM H' }, { reg: '232131104029', name: 'GAYATHRI P' }, { reg: '232131104030', name: 'GOPIKA M' },
            { reg: '232131104031', name: 'GOWRINATHAN S' }, { reg: '232131104032', name: 'HALID HASAN H' }, { reg: '232131104033', name: 'HANA RITHIKA R' },
            { reg: '232131104034', name: 'ILAYA SURIYAN N' }, { reg: '232131104035', name: 'JAGADESH KUMAR K' }, { reg: '232131104036', name: 'JASMIN JINS' },
            { reg: '232131104037', name: 'JAYA PRIYA M' }, { reg: '232131104038', name: 'JEBIN N J' }, { reg: '232131104039', name: 'JONAN JOSHUA' },
            { reg: '232131104040', name: 'JONATHAN NORBERT D ROZARIO' }, { reg: '232131104041', name: 'JOTHILAKSHMI S' }, { reg: '232131104042', name: 'KALAISELVAN M' },
            { reg: '232131104043', name: 'KARTHICK C M' }, { reg: '232131104044', name: 'ΚAVIYA R' }, { reg: '232131104045', name: 'KISHON ANTONY A' },
            { reg: '232131104046', name: 'KOLLAPAN R' }, { reg: '232131104048', name: 'LIGHTSON N' }, { reg: '232131104049', name: 'LILLIAN PAIPPAD SUNIL' },
            { reg: '232131104050', name: 'P LOKESH KUMAR' }, { reg: '232131104051', name: 'LUSSIAN PAIPPAD SUNIL' }, { reg: '232131104052', name: 'MAADHESHWARAN S P' },
            { reg: '232131104053', name: 'MAHARAJA B' }, { reg: '232131104054', name: 'MANOJ J' }, { reg: '232131104055', name: 'MEGALA S' },
            { reg: '232131104056', name: 'MOHAMED ARSHATH S' }, { reg: '232131104057', name: 'MOHAMED SHAKEEL M' }, { reg: '232131104058', name: 'MOHAMMED AIYAS B' },
            { reg: '232131104059', name: 'MOHAMMED FAYAZ F' }, { reg: '232131104060', name: 'MOHAMMED SHAHEEM K' }, { reg: '232131104061', name: 'MUSHAKIR BASHA D' },
            { reg: '232131104062', name: 'NIVETHA D' }, { reg: '232131104063', name: 'PAVITHRA E' }, { reg: '232131104064', name: 'PRIYA NANDHANA S' },
            { reg: '232131104065', name: 'PRIYADHARSHINI E' }, { reg: '232131104066', name: 'PRIYANTH P' }, { reg: '232131104067', name: 'RAGAVAN V' },
            { reg: '232131104068', name: 'RAGUL RAVI R U' }, { reg: '232131104069', name: 'RAKSHAN S' }, { reg: '232131104070', name: 'RENUGAMBAL M' },
            { reg: '232131104071', name: 'RIYAS AHAMED R' }, { reg: '232131104072', name: 'RIYAS MOHAMAD A' }, { reg: '232131104073', name: 'RODRIC DELSIN ARUL LEO THOMAS' },
            { reg: '232131104074', name: 'ROMAL ROY' }, { reg: '232131104075', name: 'SAHANA V' }, { reg: '232131104076', name: 'SALONI B' },
            { reg: '232131104077', name: 'SAMSON SMITH M' }, { reg: '232131104078', name: 'SANDHIYA I' }, { reg: '232131104079', name: 'SANDHIYA J (23.03.2005)' },
            { reg: '232131104080', name: 'SANDHIYA J (24.05.2005)' }, { reg: '232131104081', name: 'SANDHIYA L' }, { reg: '232131104082', name: 'SANDHIYA T' },
            { reg: '232131104083', name: 'SARU MILSHA S' }, { reg: '232131104084', name: 'SAVARI LENSI V' }, { reg: '232131104085', name: 'SAVEETHA S' },
            { reg: '232131104086', name: 'SHALINI N' }, { reg: '232131104087', name: 'SHANU R P' }, { reg: '232131104088', name: 'SHARAN ELIZABETH R' },
            { reg: '232131104089', name: 'SHARMIKA K' }, { reg: '232131104090', name: 'SIBINRAJ D' }, { reg: '232131104091', name: 'SOORAJ SURESH' },
            { reg: '232131104092', name: 'STEFI BJ' }, { reg: '232131104093', name: 'SUJITHA U' }, { reg: '232131104094', name: 'SUMITHRA L' },
            { reg: '232131104095', name: 'SURAYAVEL V S' }, { reg: '232131104096', name: 'THEERTHIGA E' }, { reg: '232131104097', name: 'VENGAT VINAYAGAM P' },
            { reg: '232131104098', name: 'VINODHINI G' }, { reg: '232131104099', name: 'VISHNU PRIYA J' }
        ];

        let state = {};
        let stateHistory = [];
        let historyPointer = -1;
        let savedScrollPositions = { attendance: 0, payments: 0 };

        const defaultState = {
            students: [],
            attendanceHistory: [],
            paymentsHistory: [],
            currentTab: 'attendance',
            ui: {
                attendance: { filteredStudents: [], searchText: '', searchMode: 'name', filterType: 'all', filterRangePreset: 'all', customRange: '', statusFilter: 'all' },
                payments: { filteredStudents: [], searchText: '', searchMode: 'name', filterType: 'all', filterRangePreset: 'all', customRange: '', statusFilter: 'all', paymentMethodFilter: 'all', purpose: '', defaultAmount: '', collector: '' },
                history: { viewingItem: null, searchText: '', editingSession: { type: null, id: null } }
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
            const workInProgress = {
                attendance: state.students.map(s => ({ reg: s.reg, present: s.present })),
                payments: state.students.map(s => ({ reg: s.reg, payment: s.payment })),
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
            const appData = {
                students: state.students.map(({ id, present, payment, ...rest }) => rest),
                attendanceHistory: state.attendanceHistory,
                paymentsHistory: state.paymentsHistory,
            };
            localStorage.setItem('studentMarkerData', JSON.stringify(appData));
        }

        function loadState() {
            const savedData = localStorage.getItem('studentMarkerData');
            state = deepClone(defaultState);
            if (savedData) {
                const parsedData = JSON.parse(savedData);
                state.students = (parsedData.students || initialStudents).map((s, i) => ({ ...s, id: i + 1, present: undefined, payment: null }));
                state.attendanceHistory = (parsedData.attendanceHistory || []).map(h => ({ ...h, isImported: h.isImported || false }));
                state.paymentsHistory = (parsedData.paymentsHistory || parsedData.moneyHistory || []).map(h => ({ ...h, isImported: h.isImported || false })); // Backward compatibility
            } else {
                state.students = initialStudents.map((s, i) => ({ ...s, id: i + 1, present: undefined, payment: null }));
            }

            const workInProgressData = localStorage.getItem('studentMarkerWorkInProgress');
            if (workInProgressData) {
                const workInProgress = JSON.parse(workInProgressData);
                if (workInProgress.attendance) {
                    workInProgress.attendance.forEach(wipStudent => {
                        const student = state.students.find(s => s.reg === wipStudent.reg);
                        if (student) student.present = wipStudent.present;
                    });
                }
                if (workInProgress.payments) {
                    workInProgress.payments.forEach(wipStudent => {
                        const student = state.students.find(s => s.reg === wipStudent.reg);
                        if (student) student.payment = wipStudent.payment;
                    });
                }
                if (workInProgress.paymentsUi) {
                    state.ui.payments.purpose = workInProgress.paymentsUi.purpose || '';
                    state.ui.payments.defaultAmount = workInProgress.paymentsUi.defaultAmount || '';
                    state.ui.payments.collector = workInProgress.paymentsUi.collector || '';
                }
            }
        }
        
        function init() {
            applyInitialTheme();
            loadState();
            recordState(); // Initial state for history
            renderApp();
            setupEventListeners();
        }

        function renderApp() {
            totalStudentsHeader.textContent = `Available: ${state.students.length}`;
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
                case 'attendance': content = getAttendanceHTML(); break;
                case 'payments': content = getPaymentsHTML(); break;
                case 'history': content = getHistoryHTML(); break;
            }
            mainContent.innerHTML = content;
            // Post-render updates
            if (state.currentTab === 'attendance') updateAttendanceList();
            if (state.currentTab === 'payments') updatePaymentsList();
            if (state.currentTab === 'history') renderHistoryLists();
        }

        function saveCurrentScrollPositions() {
            const attendanceListEl = document.getElementById('attendance-list');
            if (attendanceListEl) savedScrollPositions.attendance = attendanceListEl.scrollTop;

            const paymentsListEl = document.getElementById('payments-list');
            if (paymentsListEl) savedScrollPositions.payments = paymentsListEl.scrollTop;
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

        // --- TEMPLATES (Rewritten for new UI) ---
        
        function getEditingBannerHTML(session) {
            return `
                <div class="bg-[var(--accent-glow)] p-4 rounded-2xl mb-4 flex justify-between items-center text-sm">
                    <div>
                        <p class="font-bold">Editing a past session from ${session.date}.</p>
                        <p>Changes will overwrite the saved record.</p>
                    </div>
                    <button id="cancel-edit-btn" class="soft-btn !rounded-lg !px-3 !py-1.5 !text-xs">Cancel</button>
                </div>
            `;
        }

        function getSearchComponentHTML(tab) {
            const { searchText, searchMode } = state.ui[tab];
            const placeholder = searchMode === 'name' ? "Search by name..." : "Search by Reg No. ...";
            return `
                <div class="search-container">
                    <input id="${tab}-search-input" type="text" placeholder="${placeholder}" value="${searchText}" autocomplete="off">
                    <button class="search-clear-btn" id="${tab}-search-clear-btn" title="Clear search" style="display: ${searchText ? 'inline-flex' : 'none'};">
                        <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                          <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z" clip-rule="evenodd" />
                        </svg>
                    </button>
                    <div class="search-icons">
                        <button class="search-mode-btn search-mode-btn-inner ${searchMode === 'name' ? 'active' : ''}" data-search-mode="name" title="Search by Name">
                            <svg class="h-5 w-5 pointer-events-none" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z" clip-rule="evenodd" /></svg>
                        </button>
                        <button class="search-mode-btn search-mode-btn-inner ${searchMode === 'reg' ? 'active' : ''}" data-search-mode="reg" title="Search by Reg No.">
                             <svg class="h-5 w-5 pointer-events-none" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M7 20l4-16m2 16l4-16M6 9h14M4 15h14" /></svg>
                        </button>
                    </div>
                </div>`;
        }
        
        function getFilterComponentHTML(tab) {
            const { filterType, filterRangePreset, customRange } = state.ui[tab];
            const isRangeTypeSelected = filterType === 'reg' || filterType === 'index';

            let range2_label = '51-100', range2_value = '51-100';
            if (filterType === 'reg') { range2_label = '51-99'; range2_value = '51-99'; }
            else if (filterType === 'index') { range2_label = '51-95'; range2_value = '51-95'; }

            return `
                <div class="space-y-3">
                    <div class="flex flex-wrap items-center gap-2">
                        <button class="soft-btn text-sm flex-1 ${filterType === 'all' ? 'active' : ''}" data-filter-type="all">All</button>
                        <button class="soft-btn text-sm flex-1 ${filterType === 'reg' ? 'active' : ''}" data-filter-type="reg">Reg No.</button>
                        <button class="soft-btn text-sm flex-1 ${filterType === 'index' ? 'active' : ''}" data-filter-type="index">Index No.</button>
                    </div>
                    <div class="space-y-3 ${isRangeTypeSelected ? '' : 'hidden'}">
                        <div class="flex flex-wrap items-center gap-2 border-t pt-3 border-[var(--border-color)]">
                             <button class="soft-btn text-xs flex-1 ${filterRangePreset === 'all' ? 'active' : ''}" data-filter-range="all">All</button>
                             <button class="soft-btn text-xs flex-1 ${filterRangePreset === '1-50' ? 'active' : ''}" data-filter-range="1-50">1-50</button>
                             <button class="soft-btn text-xs flex-1 ${filterRangePreset === range2_value ? 'active' : ''}" data-filter-range="${range2_value}">${range2_label}</button>
                             <button class="soft-btn text-xs flex-1 ${filterRangePreset === 'custom' ? 'active' : ''}" data-filter-range="custom">Custom</button>
                        </div>
                        <input id="${tab}-custom-range" type="text" placeholder="e.g., 1,5,10-15" class="${filterRangePreset === 'custom' ? '' : 'hidden'}" value="${customRange}">
                    </div>
                </div>`;
        }

        function getAttendanceHTML() {
            const isEditing = state.ui.history.editingSession.type === 'attendance';
            const session = isEditing ? state.attendanceHistory.find(s => s.id === state.ui.history.editingSession.id) : null;
            const takenByValue = session ? session.takenBy : '';
            const subjectValue = session ? session.subject : '';
            const bannerHTML = isEditing ? getEditingBannerHTML(session) : '';
            
            return `
                <div class="space-y-6">
                    ${bannerHTML}
                    <div class="glass-card space-y-4">
                        <div class="flex justify-between items-center">
                            <h2 class="text-xl font-bold">Control Panel</h2>
                            <div class="flex items-center gap-2">
                                <button id="reset-attendance-btn" class="soft-btn !rounded-full !p-2.5" title="Reset Current Marks">
                                    <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M1 4v6h6" />
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10" />
                                    </svg>
                                </button>
                                <button id="save-attendance-btn" class="soft-btn !rounded-full !p-2.5" title="${isEditing ? 'Update Session' : 'Save Session'}">
                                     <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z" />
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M17 21V13H7v8" />
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M7 3v5h8" />
                                     </svg>
                                </button>
                            </div>
                        </div>
                        <div class="control-group">
                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                                <input id="taken-by-input" type="text" placeholder="Taken by..." value="${takenByValue}">
                                <input id="subject-input" type="text" placeholder="Subject / Purpose" value="${subjectValue}">
                            </div>
                        </div>
                        <div class="control-group">
                            ${getSearchComponentHTML('attendance')}
                            ${getFilterComponentHTML('attendance')}
                        </div>
                    </div>
                    <div class="glass-card">
                        <div id="attendance-summary" class="grid grid-cols-3 gap-3 mb-4 text-center"></div>
                        <div class="flex gap-2 mb-4">
                            <button id="mark-all-visible-present" class="soft-btn text-sm flex-1">Mark All</button>
                            <button id="clear-all-visible" class="soft-btn text-sm flex-1">Clear All</button>
                        </div>
                        <div id="attendance-list" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3 max-h-[50vh] overflow-y-auto p-1 custom-scrollbar"></div>
                    </div>
                </div>`;
        }
        
        function getPaymentsHTML() {
            const isEditing = state.ui.history.editingSession.type === 'payments';
            const session = isEditing ? state.paymentsHistory.find(s => s.id === state.ui.history.editingSession.id) : null;
            const purposeValue = session ? session.purpose : state.ui.payments.purpose;
            const defaultAmountValue = state.ui.payments.defaultAmount;
            const collectorValue = session ? session.collector : state.ui.payments.collector;
            const bannerHTML = isEditing ? getEditingBannerHTML(session) : '';

            return `
                <div class="space-y-6">
                    ${bannerHTML}
                    <div class="glass-card space-y-4">
                        <div class="flex justify-between items-center">
                            <h2 class="text-xl font-bold">Control Panel</h2>
                            <div class="flex items-center gap-2">
                                <button id="reset-payments-btn" class="soft-btn !rounded-full !p-2.5" title="Reset Current Collection">
                                    <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M1 4v6h6" />
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10" />
                                    </svg>
                                </button>
                                <button id="save-payments-btn" class="soft-btn !rounded-full !p-2.5" title="${isEditing ? 'Update Collection' : 'Save Collection'}">
                                     <svg class="h-5 w-5 pointer-events-none" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z" />
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M17 21V13H7v8" />
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M7 3v5h8" />
                                     </svg>
                                </button>
                            </div>
                        </div>
                        <div class="control-group">
                            <input id="payments-purpose" type="text" placeholder="Purpose (e.g., Field Visit)" value="${purposeValue}">
                            <div class="grid grid-cols-2 gap-3">
                                <input id="payments-amount" type="number" placeholder="Default Amount (₹)" value="${defaultAmountValue}">
                                <input id="payments-collector" type="text" placeholder="Collector" value="${collectorValue}">
                            </div>
                        </div>
                        <div class="control-group">
                            <div class="flex gap-2 items-center">
                                <div class="flex-grow">${getSearchComponentHTML('payments')}</div>
                                <button id="open-payments-filter-modal-btn" class="soft-btn flex-shrink-0">
                                    <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M2.628 1.601C5.028 1.206 7.49 1 10 1s4.973.206 7.372.601a.75.75 0 01.628.74v2.288a2.25 2.25 0 01-.659 1.59l-4.682 4.683a2.25 2.25 0 00-.659 1.59v3.037c0 .684-.31 1.33-.844 1.757l-1.937 1.55A.75.75 0 0110 18v-5.968a2.25 2.25 0 00-.659-1.59L4.659 5.77a2.25 2.25 0 01-.659-1.59V2.34a.75.75 0 01.628-.74z" clip-rule="evenodd" /></svg>
                                    <span>Filter</span>
                                </button>
                            </div>
                        </div>
                    </div>
                    <div class="glass-card">
                        <div id="payments-summary" class="grid grid-cols-2 md:grid-cols-3 gap-3 mb-4 text-center"></div>
                        <div id="payments-list" class="grid grid-cols-1 sm:grid-cols-2 gap-3 max-h-[50vh] overflow-y-auto p-1 custom-scrollbar"></div>
                    </div>
                </div>`;
        }

        function getHistoryHTML() {
            return `
                <div class="space-y-6">
                    <div class="flex flex-wrap gap-2 items-center">
                        <input id="history-search-input" type="text" placeholder="Search history..." class="flex-grow" value="${state.ui.history.searchText}">
                        <button id="import-session-btn" class="soft-btn">
                            <svg class="h-5 w-5 pointer-events-none" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" /></svg>
                            <span>Import Session</span>
                        </button>
                    </div>
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                        <div>
                            <h2 class="text-xl font-bold mb-4">Attendance</h2>
                            <h3 class="text-lg font-semibold mb-2 text-[var(--text-secondary)]">Saved</h3>
                            <div id="history-attendance-saved-list" class="space-y-4 max-h-[60vh] overflow-y-auto custom-scrollbar pr-2"></div>
                            <h3 class="text-lg font-semibold mt-6 mb-2 text-[var(--text-secondary)]">Imported</h3>
                            <div id="history-attendance-imported-list" class="space-y-4 max-h-[60vh] overflow-y-auto custom-scrollbar pr-2"></div>
                        </div>
                        <div>
                            <h2 class="text-xl font-bold mb-4">Payments History</h2>
                            <h3 class="text-lg font-semibold mb-2 text-[var(--text-secondary)]">Saved</h3>
                            <div id="history-payments-saved-list" class="space-y-4 max-h-[60vh] overflow-y-auto custom-scrollbar pr-2"></div>
                            <h3 class="text-lg font-semibold mt-6 mb-2 text-[var(--text-secondary)]">Imported</h3>
                            <div id="history-payments-imported-list" class="space-y-4 max-h-[60vh] overflow-y-auto custom-scrollbar pr-2"></div>
                        </div>
                    </div>
                    <div id="history-detail-view" class="mt-6"></div>
                </div>`;
        }
        
        function getPaymentsFilterModalHTML() {
            const tab = 'payments';
            const { filterType, filterRangePreset, customRange, statusFilter, paymentMethodFilter } = state.ui[tab];
            const isRangeTypeSelected = filterType === 'reg' || filterType === 'index';
            
            let range2_label = '51-100', range2_value = '51-100';
            if (filterType === 'reg') { range2_label = '51-99'; range2_value = '51-99'; }
            else if (filterType === 'index') { range2_label = '51-95'; range2_value = '51-95'; }

            return `
                <div class="space-y-3">
                    <label class="font-semibold">Filter by Number</label>
                    <div class="flex flex-wrap items-center gap-2" data-filter-group="filterType">
                        <button class="soft-btn text-sm flex-1 ${filterType === 'all' ? 'active' : ''}" data-value="all">All</button>
                        <button class="soft-btn text-sm flex-1 ${filterType === 'reg' ? 'active' : ''}" data-value="reg">Reg No.</button>
                        <button class="soft-btn text-sm flex-1 ${filterType === 'index' ? 'active' : ''}" data-value="index">Index No.</button>
                    </div>
                    <div class="space-y-3 ${isRangeTypeSelected ? '' : 'hidden'}">
                        <div class="flex flex-wrap items-center gap-2 border-t pt-3 border-[var(--border-color)]" data-filter-group="filterRangePreset">
                             <button class="soft-btn text-xs flex-1 ${filterRangePreset === 'all' ? 'active' : ''}" data-value="all">All</button>
                             <button class="soft-btn text-xs flex-1 ${filterRangePreset === '1-50' ? 'active' : ''}" data-value="1-50">1-50</button>
                             <button class="soft-btn text-xs flex-1 ${filterRangePreset === range2_value ? 'active' : ''}" data-value="${range2_value}">${range2_label}</button>
                             <button class="soft-btn text-xs flex-1 ${filterRangePreset === 'custom' ? 'active' : ''}" data-value="custom">Custom</button>
                        </div>
                        <input id="payments-modal-custom-range" type="text" placeholder="e.g., 1,5,10-15" class="${filterRangePreset === 'custom' ? '' : 'hidden'}" value="${customRange}">
                    </div>
                </div>
                <div class="space-y-3 border-t pt-3 border-[var(--border-color)]">
                    <label class="font-semibold">Payment Status</label>
                    <div class="flex flex-wrap items-center gap-2" data-filter-group="statusFilter">
                        <button class="soft-btn text-sm flex-1 ${statusFilter === 'all' ? 'active' : ''}" data-value="all">All</button>
                        <button class="soft-btn text-sm flex-1 ${statusFilter === 'paid' ? 'active' : ''}" data-value="paid">Paid</button>
                        <button class="soft-btn text-sm flex-1 ${statusFilter === 'unpaid' ? 'active' : ''}" data-value="unpaid">Unpaid</button>
                    </div>
                </div>
                <div class="space-y-3 border-t pt-3 border-[var(--border-color)]">
                    <label class="font-semibold">Payment Method</label>
                    <div class="flex flex-wrap items-center gap-2" data-filter-group="paymentMethodFilter">
                        <button class="soft-btn text-sm flex-1 ${paymentMethodFilter === 'all' ? 'active' : ''}" data-value="all">All</button>
                        <button class="soft-btn text-sm flex-1 ${paymentMethodFilter === 'Cash' ? 'active' : ''}" data-value="Cash">Cash</button>
                        <button class="soft-btn text-sm flex-1 ${paymentMethodFilter === 'ePay' ? 'active' : ''}" data-value="ePay">ePay</button>
                    </div>
                </div>
            `;
        }

        function getHistoryCardHTML(item) {
            let statsHTML;
            if (item.type === 'attendance') {
                const present = item.records.filter(r => r.present).length;
                const absent = item.records.length - present;
                statsHTML = `
                    <div class="flex justify-around text-center text-sm">
                        <div><p class="font-bold text-lg text-[var(--success-color)]">${present}</p><p class="text-xs text-[var(--text-secondary)]">Present</p></div>
                        <div><p class="font-bold text-lg text-[var(--danger-color)]">${absent}</p><p class="text-xs text-[var(--text-secondary)]">Absent</p></div>
                        <div><p class="font-bold text-lg">${item.records.length}</p><p class="text-xs text-[var(--text-secondary)]">Total</p></div>
                    </div>`;
            } else { // payments
                const paidRecords = item.records.filter(r => r.payment);
                const totalCollected = paidRecords.reduce((sum, r) => sum + parseFloat(r.payment.amount || 0), 0);
                statsHTML = `
                     <div class="flex justify-around text-center text-sm">
                        <div><p class="font-bold text-lg">${paidRecords.length}</p><p class="text-xs text-[var(--text-secondary)]">Paid</p></div>
                        <div><p class="font-bold text-lg">₹${totalCollected.toFixed(2)}</p><p class="text-xs text-[var(--text-secondary)]">Collected</p></div>
                    </div>`;
            }

            return `
                <div class="glass-card !p-3 flex flex-col gap-3" data-id="${item.id}" data-type="${item.type}">
                    <div>
                        <p class="font-bold truncate">${item.subject || item.purpose}</p>
                        <p class="text-xs text-[var(--text-secondary)]">${item.date} at ${item.time}</p>
                    </div>
                    <div class="bg-[var(--bg-color-2)] p-2 rounded-xl">${statsHTML}</div>
                    <div class="flex flex-wrap gap-2 pt-3 border-t border-[var(--border-color)]">
                        <button class="view-history-btn flex-1 text-sm soft-btn !py-1.5 !px-3 !text-xs">View</button>
                        <button class="edit-history-btn flex-1 text-sm soft-btn !py-1.5 !px-3 !text-xs">Edit</button>
                        <div class="relative flex-1">
                            <button class="export-history-btn w-full text-sm soft-btn !py-1.5 !px-3 !text-xs">Export</button>
                            <div class="export-history-options hidden absolute right-0 bottom-full mb-2 w-36 glass-card !p-1 z-20">
                                <a href="#" class="export-history-option block px-3 py-1.5 text-xs rounded-lg hover:bg-[var(--bg-color-2)]" data-format="pdf">PDF</a>
                                <a href="#" class="export-history-option block px-3 py-1.5 text-xs rounded-lg hover:bg-[var(--bg-color-2)]" data-format="png">PNG</a>
                                <a href="#" class="export-history-option block px-3 py-1.5 text-xs rounded-lg hover:bg-[var(--bg-color-2)]" data-format="excel">Excel</a>
                                <div class="border-t my-1 border-[var(--border-color)]"></div>
                                <a href="#" class="export-history-option block px-3 py-1.5 text-xs rounded-lg hover:bg-[var(--bg-color-2)]" data-format="session_json">Share (JSON)</a>
                            </div>
                        </div>
                        <button class="delete-history-btn flex-none soft-btn !p-1.5 !rounded-lg text-[var(--danger-color)] hover:bg-[var(--danger-bg)]" title="Delete">
                            <svg class="h-4 w-4 pointer-events-none" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg>
                        </button>
                    </div>
                </div>`;
        }
        
        function getStudentCardHTML(student) {
            const statusIndicator = student.present === true
                ? 'bg-[var(--success-bg)] border-[var(--success-color)]' // Present
                : 'border-[var(--border-color)]'; // Unmarked

            const filterType = state.ui.attendance.filterType;
            let circleContent = '';
            switch(filterType) {
                case 'reg': circleContent = parseInt(student.reg.slice(-3)); break;
                case 'index': circleContent = student.id; break;
                default: circleContent = student.name.split(' ').map(n => n[0]).join('').substring(0, 2).toUpperCase(); break;
            }

            return `
                <div class="student-card flex items-center p-2 border-2 rounded-2xl cursor-pointer ${statusIndicator}" data-reg="${student.reg}">
                    <div class="w-10 h-10 rounded-full bg-[var(--accent-glow)] flex items-center justify-center mr-3 flex-shrink-0 font-bold text-[var(--accent-color)]">${circleContent}</div>
                    <div class="flex-grow min-w-0"><p class="font-semibold truncate">${student.name}</p></div>
                </div>`;
        }
        
        function getPaymentsStudentCardHTML(student) {
             let statusIndicator = student.payment ? 'bg-[var(--success-bg)]' : 'border border-[var(--border-color)]';
            const amount = student.payment ? student.payment.amount : (state.ui.payments.defaultAmount || '');
            const method = student.payment ? student.payment.method : 'Cash';

            return `
                <div class="student-card-payments flex flex-col p-3 rounded-2xl transition-colors ${statusIndicator}">
                    <div class="flex items-center mb-2">
                        <div class="flex-grow min-w-0"><p class="font-semibold truncate">${student.name}</p></div>
                        <input type="checkbox" class="payments-student-checkbox h-5 w-5 ml-2 flex-shrink-0" data-reg="${student.reg}" ${student.payment ? 'checked' : ''}>
                    </div>
                    <div class="flex items-center gap-2 mt-1">
                        <input type="number" placeholder="Amt" class="payments-amount-input !text-sm !p-2" data-reg="${student.reg}" value="${amount}" ${!student.payment ? 'disabled' : ''}>
                        <select class="payments-method-select !text-sm !p-2" data-reg="${student.reg}" ${!student.payment ? 'disabled' : ''}>
                            <option value="Cash" ${method === 'Cash' ? 'selected' : ''}>Cash</option>
                            <option value="ePay" ${method === 'ePay' ? 'selected' : ''}>ePay</option>
                        </select>
                    </div>
                </div>`;
        }

        // --- RENDERERS & UPDATERS ---
        function applyStaggeredAnimation(listEl, selector) {
            const cards = listEl.querySelectorAll(selector);
            cards.forEach((card, index) => {
                card.style.animationDelay = `${index * 40}ms`;
                card.classList.add('fade-in');
            });
        }
        
        function updateAttendanceList() {
            const studentsToDisplay = getFilteredStudents('attendance');
            const listEl = document.getElementById('attendance-list');
            if (!listEl) return;
            listEl.innerHTML = studentsToDisplay.length > 0
                ? studentsToDisplay.map(s => getStudentCardHTML(s)).join('')
                : `<p class="col-span-full text-center text-[var(--text-secondary)] py-8">No students match.</p>`;
            
            updateAttendanceSummary();
            applyStaggeredAnimation(listEl, '.student-card');
            if (listEl && savedScrollPositions.attendance > 0) listEl.scrollTop = savedScrollPositions.attendance;
        }

        function updatePaymentsList() {
             const studentsToDisplay = getFilteredStudents('payments');
             const listEl = document.getElementById('payments-list');
             if (!listEl) return;
             listEl.innerHTML = studentsToDisplay.length > 0
                ? studentsToDisplay.map(s => getPaymentsStudentCardHTML(s)).join('')
                : `<p class="col-span-full text-center text-[var(--text-secondary)] py-8">No students match.</p>`;

             updatePaymentsSummary();
             applyStaggeredAnimation(listEl, '.student-card-payments');
             if (listEl && savedScrollPositions.payments > 0) listEl.scrollTop = savedScrollPositions.payments;
        }

        function renderHistoryLists() {
            const lists = {
                attSaved: document.getElementById('history-attendance-saved-list'),
                attImported: document.getElementById('history-attendance-imported-list'),
                paySaved: document.getElementById('history-payments-saved-list'),
                payImported: document.getElementById('history-payments-imported-list')
            };
            const lowerSearchText = state.ui.history.searchText.toLowerCase().trim();

            let fullAtt = state.attendanceHistory.filter(item => !lowerSearchText || Object.values(item).some(val => String(val).toLowerCase().includes(lowerSearchText)));
            let fullPay = state.paymentsHistory.filter(item => !lowerSearchText || Object.values(item).some(val => String(val).toLowerCase().includes(lowerSearchText)));
            
            const attSaved = fullAtt.filter(i => !i.isImported);
            const attImported = fullAtt.filter(i => i.isImported);
            const paySaved = fullPay.filter(i => !i.isImported);
            const payImported = fullPay.filter(i => i.isImported);

            if(lists.attSaved) lists.attSaved.innerHTML = attSaved.length > 0 ? attSaved.map(getHistoryCardHTML).join('') : `<p class="text-center text-[var(--text-secondary)] p-2">No saved attendance history.</p>`;
            if(lists.attImported) lists.attImported.innerHTML = attImported.length > 0 ? attImported.map(getHistoryCardHTML).join('') : `<p class="text-center text-[var(--text-secondary)] p-2">No imported attendance history.</p>`;
            if(lists.paySaved) lists.paySaved.innerHTML = paySaved.length > 0 ? paySaved.map(getHistoryCardHTML).join('') : `<p class="text-center text-[var(--text-secondary)] p-2">No saved payments history.</p>`;
            if(lists.payImported) lists.payImported.innerHTML = payImported.length > 0 ? payImported.map(getHistoryCardHTML).join('') : `<p class="text-center text-[var(--text-secondary)] p-2">No imported payments history.</p>`;

            // Handle visibility of the detail view based on state
            const detailView = document.getElementById('history-detail-view');
            if (detailView) {
                if (state.ui.history.viewingItem) {
                    detailView.innerHTML = `<div class="glass-card"><div class="flex justify-between items-center mb-4 border-b border-[var(--border-color)] pb-2"><h2 class="text-2xl font-bold">Details</h2><button id="close-history-detail" class="soft-btn !rounded-full !p-2 !text-2xl">&times;</button></div><div class="max-h-[60vh] overflow-y-auto custom-scrollbar p-1"><div class="export-document">${getExportHTML(state.ui.history.viewingItem)}</div></div></div>`;
                    detailView.scrollIntoView({ behavior: 'smooth' });
                } else {
                    detailView.innerHTML = '';
                }
            }
        }
        
        function updateAttendanceSummary() {
            const summaryEl = document.getElementById('attendance-summary');
            if(!summaryEl) return;
            const relevantStudents = getRangeFilteredStudents('attendance');
            const totalCount = relevantStudents.length;
            const presentCount = relevantStudents.filter(s => s.present === true).length;
            const absentCount = totalCount - presentCount;
            const activeFilter = state.ui.attendance.statusFilter;

            summaryEl.innerHTML = `
                <button class="summary-btn ${activeFilter === 'all' ? 'active' : ''}" data-status-filter="all">
                    <p class="font-bold">Total</p><span class="text-xl">${totalCount}</span>
                </button>
                <button class="summary-btn ${activeFilter === 'present' ? 'active' : ''}" data-status-filter="present">
                    <p class="font-bold text-[var(--success-color)]">Present</p><span class="text-xl text-[var(--success-color)]">${presentCount}</span>
                </button>
                <button class="summary-btn ${activeFilter === 'absent' ? 'active' : ''}" data-status-filter="absent">
                    <p class="font-bold text-[var(--danger-color)]">Absent</p><span class="text-xl text-[var(--danger-color)]">${absentCount}</span>
                </button>`;
        }

        function updatePaymentsSummary() {
            const summaryEl = document.getElementById('payments-summary');
            if(!summaryEl) return;
            
            const studentsInView = getFilteredStudents('payments');
            const defaultAmount = parseFloat(document.getElementById('payments-amount')?.value) || 0;

            const paidStudents = studentsInView.filter(s => s.payment);
            const paidCount = paidStudents.length;
            const unpaidCount = studentsInView.length - paidCount;

            const totalReceived = paidStudents.reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
            const totalExpected = defaultAmount > 0 ? defaultAmount * studentsInView.length : totalReceived;
            const dueAmount = totalExpected - totalReceived;

            const cashTotal = paidStudents.filter(s => s.payment.method === 'Cash').reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
            const ePayTotal = paidStudents.filter(s => s.payment.method === 'ePay').reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
            
            summaryEl.innerHTML = `
                <div class="bg-[var(--bg-color-2)] p-2 rounded-xl"><p class="text-xs font-medium">Budget</p><p class="text-lg font-bold">₹${totalExpected.toFixed(2)}</p></div>
                <div class="bg-[var(--bg-color-2)] p-2 rounded-xl"><p class="text-xs font-medium text-[var(--success-color)]">Received</p><p class="text-lg font-bold text-[var(--success-color)]">₹${totalReceived.toFixed(2)}</p></div>
                <div class="bg-[var(--bg-color-2)] p-2 rounded-xl"><p class="text-xs font-medium text-[var(--danger-color)]">Due</p><p class="text-lg font-bold text-[var(--danger-color)]">₹${Math.max(0, dueAmount).toFixed(2)}</p></div>
                <div class="bg-[var(--bg-color-2)] p-2 rounded-xl"><p class="text-xs font-medium">Paid/Unpaid</p><p class="text-lg font-bold"><span class="text-[var(--success-color)]">${paidCount}</span> / <span class="text-[var(--danger-color)]">${unpaidCount}</span></p></div>
                <div class="bg-[var(--bg-color-2)] p-2 rounded-xl"><p class="text-xs font-medium">Cash</p><p class="text-lg font-bold">₹${cashTotal.toFixed(2)}</p></div>
                <div class="bg-[var(--bg-color-2)] p-2 rounded-xl"><p class="text-xs font-medium">ePay</p><p class="text-lg font-bold">₹${ePayTotal.toFixed(2)}</p></div>
            `;
        }
        
        // --- NEW PAYMENTS FILTER MODAL FUNCTIONS ---
        function openPaymentsFilterModal() {
            renderPaymentsFilterModalContent();
            document.getElementById('payments-filter-modal').classList.add('visible');
        }

        function closePaymentsFilterModal() {
            document.getElementById('payments-filter-modal').classList.remove('visible');
        }
        
        function renderPaymentsFilterModalContent() {
            const contentEl = document.getElementById('payments-filter-modal-content');
            if (contentEl) {
                contentEl.innerHTML = getPaymentsFilterModalHTML();
            }
        }

        // --- FILTER & PARSING LOGIC (UNCHANGED) ---
        function parseRange(rangeStr) { /* ... original code ... */ }
        function getRangeFilteredStudents(tab) { /* ... original code ... */ }
        function getFilteredStudents(tab) { /* ... original code ... */ }
        // Copying the original functions to avoid breaking logic
        parseRange = function(rangeStr) {
            const result = new Set();
            if (!rangeStr) return result;
            const parts = rangeStr.split(',').map(s => s.trim());
            for (const part of parts) {
                if (part.includes('-')) {
                    const [start, end] = part.split('-').map(Number);
                    if (!isNaN(start) && !isNaN(end)) {
                        for (let i = start; i <= end; i++) result.add(i);
                    }
                } else {
                    const num = Number(part);
                    if (!isNaN(num)) result.add(num);
                }
            }
            return result;
        };
        getRangeFilteredStudents = function(tab) {
            let studentsToDisplay = [...state.students];
            const uiState = state.ui[tab];
            if (uiState.searchText) {
                const lowerSearchText = uiState.searchText.toLowerCase();
                studentsToDisplay = studentsToDisplay.filter(student => uiState.searchMode === 'name' ? student.name.toLowerCase().includes(lowerSearchText) : student.reg.endsWith(uiState.searchText));
            }
            const { filterType, filterRangePreset, customRange } = uiState;
            if (filterType === 'all' || filterRangePreset === 'all') return studentsToDisplay;
            let rangeSet = null;
            if (filterRangePreset === '1-50') rangeSet = parseRange('1-50');
            else if (filterRangePreset === '51-99') rangeSet = parseRange('51-99');
            else if (filterRangePreset === '51-95') rangeSet = parseRange('51-95');
            else if (filterRangePreset === 'custom' && customRange) rangeSet = parseRange(customRange);
            if (rangeSet && rangeSet.size > 0) {
                studentsToDisplay = studentsToDisplay.filter(student => {
                    const valueToCheck = (filterType === 'index') ? student.id : parseInt(student.reg.slice(-3));
                    return rangeSet.has(valueToCheck);
                });
            } else if (filterRangePreset !== 'custom') { studentsToDisplay = []; }
            return studentsToDisplay;
        };
        getFilteredStudents = function(tab) {
            const uiState = state.ui[tab];
            if (tab === 'attendance') {
                let studentsToDisplay = getRangeFilteredStudents('attendance');
                const { statusFilter } = uiState;
                if (statusFilter === 'present') studentsToDisplay = studentsToDisplay.filter(s => s.present === true);
                else if (statusFilter === 'absent') studentsToDisplay = studentsToDisplay.filter(s => s.present !== true);
                uiState.filteredStudents = studentsToDisplay;
                return studentsToDisplay;
            } else if (tab === 'payments') {
                const { statusFilter, paymentMethodFilter } = state.ui.payments;
                let studentsToDisplay = getRangeFilteredStudents('payments').filter(student => {
                    const isPaid = !!student.payment;
                    const method = isPaid ? student.payment.method : null;
                    const statusMatch = (statusFilter === 'all') || (statusFilter === 'paid' && isPaid) || (statusFilter === 'unpaid' && !isPaid);
                    if (!statusMatch) return false;
                    const methodMatch = (paymentMethodFilter === 'all') || !isPaid || (isPaid && method === paymentMethodFilter);
                    if (!methodMatch) return false;
                    return true;
                });
                uiState.filteredStudents = studentsToDisplay;
                return studentsToDisplay;
            }
            let students = getRangeFilteredStudents(tab);
            uiState.filteredStudents = students;
            return students;
        };
        
        
        // --- EVENT HANDLERS (Adapted for new UI) ---
        function setupEventListeners() {
            document.querySelectorAll('.nav-btn').forEach(btn => btn.addEventListener('click', () => switchTab(btn.dataset.tab)));
            document.getElementById('reset-app-btn').addEventListener('click', () => showConfirmation("This will delete all data. Are you sure?", () => {
                localStorage.clear();
                init();
            }));
            
            document.getElementById('import-json-btn-menu').addEventListener('click', () => document.getElementById('import-json-input').click());
            document.getElementById('import-json-input').addEventListener('change', handleImportData);
            
            menuBtn.addEventListener('click', (e) => { e.stopPropagation(); menuOptions.classList.toggle('hidden'); });
            
            document.getElementById('export-pdf').addEventListener('click', (e) => { e.preventDefault(); handleExport('pdf'); });
            document.getElementById('export-png').addEventListener('click', (e) => { e.preventDefault(); handleExport('png'); });
            document.getElementById('export-excel').addEventListener('click', (e) => { e.preventDefault(); handleExport('excel'); });
            document.getElementById('export-json').addEventListener('click', (e) => { e.preventDefault(); handleExport('json'); });

            undoBtn.addEventListener('click', undo);
            redoBtn.addEventListener('click', redo);
            document.getElementById('theme-toggle').addEventListener('click', toggleTheme);

            document.getElementById('modal-cancel').addEventListener('click', hideConfirmation);
            window.addEventListener('click', (e) => {
                if (!menuBtn.parentElement.contains(e.target)) menuOptions.classList.add('hidden');
                document.querySelectorAll('.export-history-options').forEach(el => el.classList.add('hidden'));
            });

            mainContent.addEventListener('input', handleMainContentInput);
            mainContent.addEventListener('click', handleMainContentClick);
            mainContent.addEventListener('change', handleMainContentChange);
            
            // Payments filter modal listeners
            const filterModal = document.getElementById('payments-filter-modal');
            document.getElementById('close-payments-filter-modal-btn').addEventListener('click', closePaymentsFilterModal);
            document.getElementById('apply-payments-filters-btn').addEventListener('click', () => {
                closePaymentsFilterModal();
                updatePaymentsList();
            });
            document.getElementById('reset-payments-filters-btn').addEventListener('click', () => {
                const defaults = defaultState.ui.payments;
                state.ui.payments.filterType = defaults.filterType;
                state.ui.payments.filterRangePreset = defaults.filterRangePreset;
                state.ui.payments.customRange = defaults.customRange;
                state.ui.payments.statusFilter = defaults.statusFilter;
                state.ui.payments.paymentMethodFilter = defaults.paymentMethodFilter;
                renderPaymentsFilterModalContent();
                updatePaymentsList();
            });
            filterModal.addEventListener('click', (e) => {
                const btn = e.target.closest('button[data-value]');
                if (!btn) return;
                const group = btn.parentElement.dataset.filterGroup;
                const value = btn.dataset.value;
                if (group && state.ui.payments[group] !== value) {
                    state.ui.payments[group] = value;
                    if (group === 'filterType') { state.ui.payments.filterRangePreset = 'all'; }
                    renderPaymentsFilterModalContent();
                }
            });
            filterModal.addEventListener('input', (e) => {
                if(e.target.id === 'payments-modal-custom-range') {
                    state.ui.payments.customRange = e.target.value;
                }
            });

            // New listener for single session import
            const importSessionInput = document.getElementById('import-session-input');
            if (importSessionInput) {
                importSessionInput.addEventListener('change', handleImportSession);
            }
        }

        function handleMainContentInput(e) {
            const tab = state.currentTab;
            const target = e.target;
            const targetId = target.id;

            if (targetId === `${tab}-search-input`) {
                state.ui[tab].searchText = target.value;
                const clearBtn = document.getElementById(`${tab}-search-clear-btn`);
                if (clearBtn) clearBtn.style.display = target.value ? 'inline-flex' : 'none';
                
                if (tab === 'attendance') updateAttendanceList();
                else if (tab === 'payments') updatePaymentsList();
                return;
            }
            if (targetId === 'history-search-input') {
                state.ui.history.searchText = target.value;
                renderHistoryLists();
                return;
            }
            if (targetId === `${tab}-custom-range`) {
                state.ui[tab].customRange = target.value;
                if (state.ui[tab].filterRangePreset === 'custom') {
                    if (tab === 'attendance') updateAttendanceList();
                    else if (tab === 'payments') updatePaymentsList();
                }
                return;
            }
            if (target.matches('#payments-purpose, #payments-amount, #payments-collector')) {
                state.ui.payments.purpose = document.getElementById('payments-purpose')?.value;
                state.ui.payments.defaultAmount = document.getElementById('payments-amount')?.value;
                state.ui.payments.collector = document.getElementById('payments-collector')?.value;
                debouncedAutosave();
                if (targetId === 'payments-amount') {
                    updatePaymentsSummary();
                }
            }
        }

        function handleMainContentClick(e) {
            const tab = state.currentTab;
            let stateChanged = false;

            const clearBtn = e.target.closest('.search-clear-btn');
            if (clearBtn) {
                state.ui[tab].searchText = '';
                const inputEl = document.getElementById(`${tab}-search-input`);
                if (inputEl) inputEl.value = '';
                clearBtn.style.display = 'none';

                if (tab === 'attendance') updateAttendanceList();
                else if (tab === 'payments') updatePaymentsList();
                return;
            }

            const filterTypeBtn = e.target.closest('[data-filter-type]');
            if (filterTypeBtn) {
                saveCurrentScrollPositions();
                const newFilterType = filterTypeBtn.dataset.filterType;
                if(state.ui[tab].filterType !== newFilterType) {
                    state.ui[tab].filterType = newFilterType;
                    state.ui[tab].filterRangePreset = 'all';
                }
                renderCurrentTab(); return;
            }
            const filterRangeBtn = e.target.closest('[data-filter-range]');
            if (filterRangeBtn) {
                saveCurrentScrollPositions();
                state.ui[tab].filterRangePreset = filterRangeBtn.dataset.filterRange;
                renderCurrentTab(); return;
            }
            
            const searchModeBtn = e.target.closest('.search-mode-btn');
            if (searchModeBtn) {
                saveCurrentScrollPositions();
                state.ui[tab].searchMode = searchModeBtn.dataset.searchMode;
                renderCurrentTab(); return;
            }

            const statusFilterBtn = e.target.closest('.summary-btn[data-status-filter]');
            if(statusFilterBtn) {
                saveCurrentScrollPositions();
                state.ui.attendance.statusFilter = statusFilterBtn.dataset.statusFilter;
                updateAttendanceList();
                // Update button active states without full re-render
                document.querySelectorAll('.summary-btn[data-status-filter]').forEach(btn => btn.classList.remove('active'));
                statusFilterBtn.classList.add('active');
                return;
            }

            if (e.target.closest('#open-payments-filter-modal-btn')) {
                openPaymentsFilterModal();
                return;
            }

            const studentCard = e.target.closest('.student-card');
            if (studentCard && tab === 'attendance') {
                const student = state.students.find(s => s.reg === studentCard.dataset.reg);
                if (student) {
                    student.present = student.present === true ? undefined : true;
                    studentCard.classList.remove('bg-[var(--success-bg)]', 'border-[var(--success-color)]', 'border-[var(--border-color)]');
                    if (student.present === true) {
                        studentCard.classList.add('bg-[var(--success-bg)]', 'border-[var(--success-color)]');
                    } else {
                        studentCard.classList.add('border-[var(--border-color)]');
                    }
                    recordState(); debouncedAutosave(); updateAttendanceSummary();
                }
                return;
            }

            const targetId = e.target.closest('button')?.id;
            switch (targetId) {
                case 'save-attendance-btn': saveAttendance(); break;
                case 'save-payments-btn': savePaymentsCollection(); break;
                case 'reset-attendance-btn': {
                    state.students.forEach(s => { s.present = undefined; });
                    const wip = JSON.parse(localStorage.getItem('studentMarkerWorkInProgress') || '{}');
                    delete wip.attendance;
                    localStorage.setItem('studentMarkerWorkInProgress', JSON.stringify(wip));
                    stateChanged = true;
                    break;
                }
                case 'reset-payments-btn': {
                    state.students.forEach(s => s.payment = null);
                    const wip = JSON.parse(localStorage.getItem('studentMarkerWorkInProgress') || '{}');
                    delete wip.payments; delete wip.paymentsUi;
                    localStorage.setItem('studentMarkerWorkInProgress', JSON.stringify(wip));
                    stateChanged = true;
                    break;
                }
                case 'mark-all-visible-present':
                case 'clear-all-visible':
                    const markPresent = targetId === 'mark-all-visible-present';
                    state.ui.attendance.filteredStudents.forEach(fs => {
                        const student = state.students.find(s => s.reg === fs.reg);
                        if(student) student.present = markPresent ? true : undefined;
                    });
                    stateChanged = true;
                    break;
                case 'cancel-edit-btn': cancelEditing(); break;
                case 'import-session-btn': document.getElementById('import-session-input').click(); break;
                case 'close-history-detail':
                    state.ui.history.viewingItem = null;
                    renderHistoryLists();
                    break;
            }

            if (tab === 'history') {
                const historyCard = e.target.closest('[data-id]');
                if (!historyCard) return;
                const { id, type } = historyCard.dataset;
                if (e.target.closest('.view-history-btn')) viewHistoryDetail(id, type);
                if (e.target.closest('.delete-history-btn')) deleteHistoryItem(id, type);
                if (e.target.closest('.edit-history-btn')) startEditingHistoryItem(id, type);
                if (e.target.closest('.export-history-btn')) {
                    e.stopPropagation();
                    historyCard.querySelector('.export-history-options')?.classList.toggle('hidden');
                }
                const exportOption = e.target.closest('.export-history-option');
                if (exportOption) {
                    e.preventDefault();
                    const item = type === 'attendance' ? state.attendanceHistory.find(i => i.id === id) : state.paymentsHistory.find(i => i.id === id);
                    if (item) handleExport(exportOption.dataset.format, item);
                }
            }

            if(stateChanged) { saveCurrentScrollPositions(); recordState(); debouncedAutosave(); renderCurrentTab(); }
        }

        function handleMainContentChange(e) {
            let stateChanged = false;
            if (e.target.classList.contains('payments-student-checkbox')) {
                const student = state.students.find(s => s.reg === e.target.dataset.reg);
                if (student) {
                    if (e.target.checked) {
                        const defaultAmount = document.getElementById('payments-amount')?.value || 0;
                        student.payment = { amount: defaultAmount, method: 'Cash' };
                    } else { student.payment = null; }
                    stateChanged = true;
                }
            }
            if (e.target.classList.contains('payments-amount-input') || e.target.classList.contains('payments-method-select')) {
                const student = state.students.find(s => s.reg === e.target.dataset.reg);
                if (student && student.payment) {
                    const amountInput = mainContent.querySelector(`.payments-amount-input[data-reg="${student.reg}"]`);
                    const methodSelect = mainContent.querySelector(`.payments-method-select[data-reg="${student.reg}"]`);
                    if (amountInput) student.payment.amount = amountInput.value;
                    if (methodSelect) student.payment.method = methodSelect.value;
                    stateChanged = true;
                }
            }
            
            if(stateChanged) { saveCurrentScrollPositions(); recordState(); debouncedAutosave(); renderCurrentTab(); }
        }
        
        // --- CORE LOGIC ---
        function showConfirmation(message, onConfirm) {
            modalMessage.textContent = message;
            modal.classList.add('visible');
            const confirmBtn = document.getElementById('modal-confirm');
            const confirmHandler = () => { onConfirm(); hideConfirmation(); };
            confirmBtn.addEventListener('click', confirmHandler, { once: true });
            document.getElementById('modal-cancel').addEventListener('click', () => { hideConfirmation(); confirmBtn.removeEventListener('click', confirmHandler); }, {once: true});
        };
        function hideConfirmation() { modal.classList.remove('visible'); };

        function saveAttendance() {
            const now = new Date();
            const takenByEl = document.getElementById('taken-by-input');
            const subjectEl = document.getElementById('subject-input');
            const recordsToSave = state.students.map(({ reg, name, present }) => ({ reg, name, present: present === true }));
            if(recordsToSave.filter(r => r.present).length === 0) { alert("No students marked as Present."); return; }
            const { type, id } = state.ui.history.editingSession;
            if (type === 'attendance' && id) {
                const sessionIndex = state.attendanceHistory.findIndex(s => s.id === id);
                if (sessionIndex !== -1) {
                    state.attendanceHistory[sessionIndex] = { ...state.attendanceHistory[sessionIndex], takenBy: takenByEl.value || 'N/A', subject: subjectEl.value || 'N/A', records: recordsToSave };
                    alert("Attendance session updated!");
                }
                cancelEditing(false);
            } else {
                const session = { id: `att-${Date.now()}`, type: 'attendance', date: now.toLocaleDateString(), time: now.toLocaleTimeString(), takenBy: takenByEl.value || 'N/A', subject: subjectEl.value || 'N/A', records: recordsToSave, isImported: false };
                state.attendanceHistory.unshift(session);
                alert("Attendance saved!");
            }
            const wip = JSON.parse(localStorage.getItem('studentMarkerWorkInProgress') || '{}');
            delete wip.attendance;
            localStorage.setItem('studentMarkerWorkInProgress', JSON.stringify(wip));
            saveCurrentScrollPositions();
            state.students.forEach(s => { s.present = undefined; });
            saveState(); recordState(); renderApp();
        };

        function savePaymentsCollection() {
            const now = new Date();
            const studentsInView = getFilteredStudents('payments');
            const recordsForSession = studentsInView.map(s => ({ reg: s.reg, name: s.name, payment: s.payment ? deepClone(s.payment) : null }));
            
            if (recordsForSession.filter(r => r.payment).length === 0) { alert("No payments recorded."); return; }

            const defaultAmount = document.getElementById('payments-amount')?.value || 0;
            const filterDescription = getPaymentsFilterDescription();

            const sessionContext = {
                totalStudentsInView: studentsInView.length,
                defaultAmount: defaultAmount,
                filterDescription: filterDescription,
            };
            
            const { type, id } = state.ui.history.editingSession;
            if (type === 'payments' && id) {
                 const sessionIndex = state.paymentsHistory.findIndex(s => s.id === id);
                 if (sessionIndex !== -1) {
                    state.paymentsHistory[sessionIndex] = { 
                        ...state.paymentsHistory[sessionIndex], 
                        collector: state.ui.payments.collector || 'N/A', 
                        purpose: state.ui.payments.purpose || 'General Collection', 
                        records: recordsForSession,
                        context: sessionContext 
                    };
                     alert("Payments collection updated!");
                 }
                cancelEditing(false);
            } else {
                const session = { 
                    id: `pay-${Date.now()}`, 
                    type: 'payments', 
                    date: now.toLocaleDateString(), 
                    time: now.toLocaleTimeString(), 
                    collector: state.ui.payments.collector || 'N/A', 
                    purpose: state.ui.payments.purpose || 'General Collection', 
                    records: recordsForSession, 
                    context: sessionContext,
                    isImported: false 
                };
                state.paymentsHistory.unshift(session);
                alert("Payments collection saved!");
            }
            const wip = JSON.parse(localStorage.getItem('studentMarkerWorkInProgress') || '{}');
            delete wip.payments; delete wip.paymentsUi;
            localStorage.setItem('studentMarkerWorkInProgress', JSON.stringify(wip));
            saveCurrentScrollPositions();
            state.students.forEach(s => { s.payment = null; });
            saveState(); recordState(); renderApp();
        };
        
        function handleImportData(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const data = JSON.parse(e.target.result);
                    if(!data.students || !data.attendanceHistory) throw new Error("Invalid format");
                    state.students = (data.students || initialStudents).map((s, i) => ({ ...s, id: i + 1, present: undefined, payment: null }));
                    state.attendanceHistory = (data.attendanceHistory || []).map(h => ({ ...h, isImported: h.isImported || false }));
                    state.paymentsHistory = (data.paymentsHistory || data.moneyHistory || []).map(h => ({ ...h, isImported: h.isImported || false }));
                    saveState(); recordState(); renderApp();
                    alert('Data imported successfully!');
                } catch (error) { alert('Failed to import data.'); console.error("Import error:", error); }
            };
            reader.readAsText(file);
        };
        
        function handleImportSession(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const item = JSON.parse(e.target.result);
                    if (!item.id || !item.type || !item.records) throw new Error("Invalid session file format.");
                    
                    item.isImported = true;
                    item.id = `imp-${item.type}-${Date.now()}`; // Assign new unique ID

                    if (item.type === 'attendance') {
                        state.attendanceHistory.unshift(item);
                    } else if (item.type === 'payments') {
                        state.paymentsHistory.unshift(item);
                    } else {
                        throw new Error("Unknown session type in file.");
                    }
                    saveState(); recordState(); renderHistoryLists();
                    alert('Session imported successfully!');
                } catch (error) {
                    alert(`Failed to import session: ${error.message}`);
                    console.error("Session import error:", error);
                } finally {
                    event.target.value = '';
                }
            };
            reader.readAsText(file);
        }

        function viewHistoryDetail(id, type) {
            const item = type === 'attendance' ? state.attendanceHistory.find(i => i.id === id) : state.paymentsHistory.find(i => i.id === id);
            if (!item) return;
            state.ui.history.viewingItem = item;
            renderHistoryLists();
        };

        function deleteHistoryItem(id, type) {
            showConfirmation("Delete this record permanently?", () => {
                if (type === 'attendance') state.attendanceHistory = state.attendanceHistory.filter(item => item.id !== id);
                else state.paymentsHistory = state.paymentsHistory.filter(item => item.id !== id);
                saveState(); recordState(); renderApp();
            });
        };

        function startEditingHistoryItem(id, type) {
            const session = type === 'attendance' ? state.attendanceHistory.find(i => i.id === id) : state.paymentsHistory.find(i => i.id === id);
            if (!session) return;
            state.ui.history.editingSession = { type, id };
            state.students.forEach(s => { s.present = undefined; s.payment = null; });
            session.records.forEach(record => {
                const student = state.students.find(s => s.reg === record.reg);
                if (student) {
                    if (type === 'attendance') student.present = record.present;
                    if (type === 'payments') student.payment = deepClone(record.payment);
                }
            });
            if (type === 'payments') { state.ui.payments.purpose = session.purpose; state.ui.payments.collector = session.collector; }
            switchTab(type);
        };

        function cancelEditing(shouldSwitchTab = true) {
            state.ui.history.editingSession = { type: null, id: null };
            state.students.forEach(s => { s.present = undefined; s.payment = null; });
             if (shouldSwitchTab) switchTab(state.currentTab);
             else renderApp();
        };

        function getFilterDescription(tab) {
            const { filterType, filterRangePreset, customRange } = state.ui[tab];
            if (filterType === 'all') return 'All Students';
            const typeText = filterType === 'reg' ? 'Reg No.' : 'Index No.';
            let rangeText = '';
            switch(filterRangePreset) {
                case 'all': rangeText = 'All'; break;
                case 'custom': rangeText = `Custom (${customRange || 'N/A'})`; break;
                default: rangeText = filterRangePreset; break;
            }
            return `${typeText} - ${rangeText}`;
        };

        function getPaymentsFilterDescription() {
            const uiState = state.ui.payments;
            const parts = [];
            parts.push(getFilterDescription('payments')); // The number part
            if (uiState.statusFilter !== 'all') {
                parts.push(`Status: ${uiState.statusFilter.charAt(0).toUpperCase() + uiState.statusFilter.slice(1)}`);
            }
            if (uiState.paymentMethodFilter !== 'all') {
                parts.push(`Method: ${uiState.paymentMethodFilter}`);
            }
            return parts.join('; ');
        }

        function handleExport(format, specificItem = null) {
            const itemToExport = specificItem || getCurrentTabDataForExport();
            if (!itemToExport) { alert("No data to export."); return; }
            
            const exportContentWrapper = document.getElementById('export-content-wrapper');
            exportContentWrapper.innerHTML = `<div class="export-document">${getExportHTML(itemToExport)}</div>`;
            const exportContentEl = exportContentWrapper.firstChild;

            const filename = `${itemToExport.type}-${(itemToExport.subject || itemToExport.purpose).replace(/ /g, '_')}-${itemToExport.date.replace(/\//g, '-')}`;

            if (format === 'pdf') {
                html2canvas(exportContentEl, { scale: 3, useCORS: true, backgroundColor: '#ffffff' }).then(canvas => {
                    const imgData = canvas.toDataURL('image/png');
                    const pdf = new jsPDF('p', 'mm', 'a4');
                    const pdfWidth = pdf.internal.pageSize.getWidth();
                    const pageHeight = pdf.internal.pageSize.getHeight();
                    const imgHeight = canvas.height * pdfWidth / canvas.width;
                    let heightLeft = imgHeight, position = 0;
                    pdf.addImage(imgData, 'PNG', 0, position, pdfWidth, imgHeight); heightLeft -= pageHeight;
                    while (heightLeft > 0) { position -= pageHeight; pdf.addPage(); pdf.addImage(imgData, 'PNG', 0, position, pdfWidth, imgHeight); heightLeft -= pageHeight; }
                    pdf.save(`${filename}.pdf`);
                });
            } else if (format === 'png') {
                 html2canvas(exportContentEl, { scale: 2, useCORS: true, backgroundColor: '#ffffff' }).then(canvas => { const link = document.createElement('a'); link.href = canvas.toDataURL('image/png'); link.download = `${filename}.png`; link.click(); });
            } else if (format === 'excel') {
                const wb = XLSX.utils.book_new();
                let ws_data = [];
                const sheetName = (itemToExport.subject || itemToExport.purpose || 'Report').substring(0, 31);
                if (itemToExport.type === 'attendance') {
                    const presentRecords = itemToExport.records.filter(r => r.present), absentRecords = itemToExport.records.filter(r => !r.present);
                    ws_data.push([`Attendance: ${itemToExport.subject || 'Current View'}`], [`Date: ${itemToExport.date}`, `Time: ${itemToExport.time}`]);
                    if (itemToExport.filterDescription) ws_data.push([`Filter: ${itemToExport.filterDescription}`]);
                    ws_data.push([], [`Total: ${itemToExport.records.length}`, `Present: ${presentRecords.length}`, `Absent: ${absentRecords.length}`], []);
                    if (presentRecords.length > 0) { ws_data.push(['Present Students'], ['Reg No', 'Name']); presentRecords.forEach(r => ws_data.push([r.reg, r.name])); ws_data.push([]); }
                    if (absentRecords.length > 0) { ws_data.push(['Absent Students'], ['Reg No', 'Name']); absentRecords.forEach(r => ws_data.push([r.reg, r.name])); }
                } else if (itemToExport.type === 'payments') {
                    const records = itemToExport.records || itemToExport.studentRecords; // Normalize
                    const paidRecords = records.filter(r => r.payment);
                    const unpaidRecords = records.filter(r => !r.payment);
                    const totalCollected = paidRecords.reduce((sum, r) => sum + parseFloat(r.payment.amount || 0), 0);
                    let totalExpected, unpaidCount, dueAmount, filterDesc;

                    unpaidCount = unpaidRecords.length;
                    filterDesc = itemToExport.context?.filterDescription || itemToExport.filterDescription;
                    if (itemToExport.context) { // New history format
                        const defaultAmount = parseFloat(itemToExport.context.defaultAmount || 0);
                        totalExpected = defaultAmount > 0 ? itemToExport.context.totalStudentsInView * defaultAmount : totalCollected;
                    } else if (itemToExport.defaultAmount !== undefined) { // Current view format
                        const defaultAmount = parseFloat(itemToExport.defaultAmount || 0);
                        totalExpected = defaultAmount > 0 ? records.length * defaultAmount : totalCollected;
                    } else { totalExpected = totalCollected; unpaidCount = 0; } // Legacy history
                    
                    dueAmount = Math.max(0, totalExpected - totalCollected);
                    const cashTotal = paidRecords.filter(s => s.payment.method === 'Cash').reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
                    const ePayTotal = paidRecords.filter(s => s.payment.method === 'ePay').reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);

                    ws_data.push([`Payments: ${itemToExport.purpose || 'Current Collection'}`], [`Date: ${itemToExport.date}`, `Time: ${itemToExport.time}`]);
                    if (filterDesc) ws_data.push([`Filter: ${filterDesc}`]);
                    ws_data.push([], ["Summary"]);
                    ws_data.push(["Budget", `₹${totalExpected.toFixed(2)}`]);
                    ws_data.push(["Received", `₹${totalCollected.toFixed(2)}`]);
                    ws_data.push(["Due", `₹${dueAmount.toFixed(2)}`]);
                    ws_data.push(["Paid/Unpaid", `${paidRecords.length} / ${unpaidCount}`]);
                    ws_data.push(["Cash", `₹${cashTotal.toFixed(2)}`]);
                    ws_data.push(["ePay", `₹${ePayTotal.toFixed(2)}`]);
                    ws_data.push([]);

                    if (paidRecords.length > 0) { ws_data.push(['Paid Students'], ['Reg No', 'Name', 'Amount (₹)', 'Method']); paidRecords.forEach(r => ws_data.push([r.reg, r.name, r.payment.amount, r.payment.method])); ws_data.push([]); }
                    if (unpaidRecords.length > 0) { ws_data.push(['Unpaid Students'], ['Reg No', 'Name']); unpaidRecords.forEach(r => ws_data.push([r.reg, r.name])); }
                }
                const ws = XLSX.utils.aoa_to_sheet(ws_data); XLSX.utils.book_append_sheet(wb, ws, sheetName); XLSX.writeFile(wb, `${filename}.xlsx`);
            } else if (format === 'json') {
                const appData = { students: state.students.map(({ id, present, payment, ...rest }) => rest), attendanceHistory: state.attendanceHistory, paymentsHistory: state.paymentsHistory };
                const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(appData, null, 2));
                const a = document.createElement('a'); a.href = dataStr; a.download = "student_marker_backup.json"; a.click(); a.remove();
            } else if (format === 'session_json') {
                const sessionData = JSON.stringify(itemToExport, null, 2);
                const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(sessionData);
                const a = document.createElement('a'); a.href = dataStr; a.download = `${filename}_session.json`; a.click(); a.remove();
            }
            menuOptions.classList.add('hidden');
        };

        function getCurrentTabDataForExport() {
            const now = new Date(); const tab = state.currentTab;
            if (tab === 'attendance' || tab === 'payments') {
                const filteredStudents = getRangeFilteredStudents(tab);
                if (tab === 'attendance') {
                     return { type: 'attendance', date: now.toLocaleDateString(), time: now.toLocaleTimeString(), takenBy: document.getElementById('taken-by-input')?.value || 'N/A', subject: document.getElementById('subject-input')?.value || 'Current View', records: filteredStudents.map(s => ({ reg: s.reg, name: s.name, present: s.present === true })), filterDescription: getFilterDescription('attendance') };
                } else {
                    return { type: 'payments', date: now.toLocaleDateString(), time: now.toLocaleTimeString(), collector: state.ui.payments.collector || 'N/A', purpose: state.ui.payments.purpose || 'Current Collection', records: filteredStudents.map(s => ({ reg: s.reg, name: s.name, payment: s.payment })), defaultAmount: state.ui.payments.defaultAmount || 0, filterDescription: getPaymentsFilterDescription() };
                }
            } 
            return null;
        };

        function getExportHTML(item) {
            const records = item.records || item.studentRecords; // Normalize
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
                content += `<div class="summary-section">
                    <div class="summary-item"><div class="label">Total</div><div class="value">${records.length}</div></div>
                    <div class="summary-item"><div class="label">Present</div><div class="value present">${present}</div></div>
                    <div class="summary-item"><div class="label">Absent</div><div class="value absent">${absent}</div></div>
                </div>`;
                content += `<h3>Present (${present})</h3>`;
                if(present > 0) { content += `<table><thead><tr><th>Reg No</th><th>Name</th></tr></thead><tbody>${records.filter(r => r.present).map(r => `<tr><td>${r.reg}</td><td>${r.name}</td></tr>`).join('')}</tbody></table>`; } else { content += `<p>None</p>`; }
                content += `<h3>Absent (${absent})</h3>`;
                if(absent > 0) { content += `<table><thead><tr><th>Reg No</th><th>Name</th></tr></thead><tbody>${records.filter(r => !r.present).map(r => `<tr><td>${r.reg}</td><td>${r.name}</td></tr>`).join('')}</tbody></table>`; } else { content += `<p>None</p>`; }
            } else if (item.type === 'payments') {
                const paidRecords = records.filter(r => r.payment);
                const unpaidRecords = records.filter(r => !r.payment);
                const totalCollected = paidRecords.reduce((sum, r) => sum + parseFloat(r.payment.amount || 0), 0);
                let totalExpected, unpaidCount, dueAmount;

                unpaidCount = unpaidRecords.length;
                if (item.context) { // New history format with full context
                    const defaultAmount = parseFloat(item.context.defaultAmount || 0);
                    totalExpected = defaultAmount > 0 ? item.context.totalStudentsInView * defaultAmount : totalCollected;
                } else if (item.defaultAmount !== undefined) { // "Current View" export
                    const defaultAmount = parseFloat(item.defaultAmount || 0);
                    totalExpected = defaultAmount > 0 ? records.length * defaultAmount : totalCollected;
                } else { // Legacy history item
                    totalExpected = totalCollected;
                    unpaidCount = item.context?.totalStudentsInView ? item.context.totalStudentsInView - paidRecords.length : 0; // Best guess for older items
                }
                dueAmount = Math.max(0, totalExpected - totalCollected);
                const cashTotal = paidRecords.filter(s => s.payment.method === 'Cash').reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
                const ePayTotal = paidRecords.filter(s => s.payment.method === 'ePay').reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);

                content += `<div class="summary-section">
                    <div class="summary-item"><div class="label">Budget</div><div class="value">₹${totalExpected.toFixed(2)}</div></div>
                    <div class="summary-item"><div class="label">Received</div><div class="value received">₹${totalCollected.toFixed(2)}</div></div>
                    <div class="summary-item"><div class="label">Due</div><div class="value due">₹${dueAmount.toFixed(2)}</div></div>
                    <div class="summary-item"><div class="label">Paid/Unpaid</div><div class="value"><span class="present">${paidRecords.length}</span> / <span class="absent">${unpaidCount}</span></div></div>
                    <div class="summary-item"><div class="label">Cash</div><div class="value">₹${cashTotal.toFixed(2)}</div></div>
                    <div class="summary-item"><div class="label">ePay</div><div class="value">₹${ePayTotal.toFixed(2)}</div></div>
                </div>`;
                
                content += `<h3>Paid (${paidRecords.length})</h3>`;
                if(paidRecords.length > 0) { content += `<table><thead><tr><th>Reg No</th><th>Name</th><th>Amount (₹)</th><th>Method</th></tr></thead><tbody>${paidRecords.map(r => `<tr><td>${r.reg}</td><td>${r.name}</td><td>${r.payment.amount}</td><td>${r.payment.method}</td></tr>`).join('')}</tbody></table>`; } else { content += `<p>None</p>`; }
                
                content += `<h3>Pending (${unpaidCount})</h3>`;
                if (unpaidRecords.length > 0) {
                     content += `<table><thead><tr><th>Reg No</th><th>Name</th></tr></thead><tbody>${unpaidRecords.map(r => `<tr><td>${r.reg}</td><td>${r.name}</td></tr>`).join('')}</tbody></table>`;
                } else if (unpaidCount > 0 && unpaidRecords.length === 0) {
                    content += `<p>The list of pending students is not available for this legacy record.</p>`;
                } else {
                    content += `<p>None</p>`;
                }
            }
            content += `<div class="footer">Generated by ATTEND. & MC app on ${new Date().toLocaleString()}</div>`;
            return content;
        }
        
        // --- FINAL BOOTSTRAP ---
        init();
    });
    </script>
</body>
</html>
