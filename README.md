<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Student List Marker</title>
    <!-- External Libraries via CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;900&display=swap" rel="stylesheet">
    <!-- Inline Styles -->
    <style>
        :root {
            --bg-start: #f3f4f6;
            --bg-end: #e5e7eb;
            --text-primary: #111827;
            --text-secondary: #4b5563;
            --card-bg: rgba(255, 255, 255, 0.6);
            --card-border: rgba(255, 255, 255, 0.8);
            --shadow-light: rgba(255, 255, 255, 0.7);
            --shadow-dark: rgba(0, 0, 0, 0.1);
            --primary-accent: #4f46e5;
            --primary-accent-light: #e0e7ff;
            --primary-accent-text: #ffffff;
            --nav-btn-active-text: #3730a3;
            --header-bg: rgba(255, 255, 255, 0.3);
        }

        body.dark-mode {
            --bg-start: #111827;
            --bg-end: #1f2937;
            --text-primary: #f9fafb;
            --text-secondary: #9ca3af;
            --card-bg: rgba(31, 41, 55, 0.5);
            --card-border: rgba(55, 65, 81, 0.7);
            --shadow-light: rgba(255, 255, 255, 0.05);
            --shadow-dark: rgba(0, 0, 0, 0.4);
            --primary-accent: #818cf8;
            --primary-accent-light: rgba(79, 70, 229, 0.3);
            --primary-accent-text: #111827;
            --nav-btn-active-text: var(--primary-accent);
            --header-bg: rgba(31, 41, 55, 0.3);
        }

        body {
            font-family: 'Inter', sans-serif;
            background-image: linear-gradient(135deg, var(--bg-start) 0%, var(--bg-end) 100%);
            color: var(--text-primary);
            transition: background-color 0.3s ease, color 0.3s ease;
            -webkit-tap-highlight-color: transparent;
        }

        .app-container {
            max-width: 1000px;
            margin: 0 auto;
            padding: 1rem 1rem 6rem 1rem; /* Padding bottom for bottom nav */
        }
        
        /* Glassmorphism & Soft UI */
        .glass-card {
            background: var(--card-bg);
            -webkit-backdrop-filter: blur(12px);
            backdrop-filter: blur(12px);
            border: 1px solid var(--card-border);
            border-radius: 1.5rem; /* 24px */
            box-shadow: 0 8px 24px 0 var(--shadow-dark);
            transition: all 0.3s ease;
        }
        
        .soft-btn {
            background-color: var(--card-bg);
            border: 1px solid var(--card-border);
            border-radius: 9999px;
            box-shadow: 4px 4px 8px var(--shadow-dark), -4px -4px 8px var(--shadow-light);
            transition: all 0.2s ease-in-out;
            color: var(--text-secondary);
        }
        .soft-btn:active, .soft-btn.active {
            box-shadow: inset 4px 4px 8px var(--shadow-dark), inset -4px -4px 8px var(--shadow-light);
            color: var(--primary-accent);
            font-weight: 600;
        }

        /* Header */
        .app-header {
            position: sticky;
            top: 0;
            z-index: 50;
            padding: 1rem;
            margin: 0 -1rem;
            background: var(--header-bg);
            -webkit-backdrop-filter: blur(12px);
            backdrop-filter: blur(12px);
        }

        /* Bottom Navigation */
        .bottom-nav {
            position: fixed;
            bottom: 1rem;
            left: 50%;
            transform: translateX(-50%);
            z-index: 50;
            width: calc(100% - 2rem);
            max-width: 400px;
        }
        .bottom-nav-inner {
            display: flex;
            justify-content: space-around;
            padding: 0.5rem;
        }
        .nav-btn {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 0.5rem 0;
            color: var(--text-secondary);
            border-radius: 1rem;
            transition: all 0.2s ease;
        }
        .nav-btn.active {
            color: var(--nav-btn-active-text);
            background: var(--primary-accent-light);
        }
        .nav-btn span { font-size: 0.75rem; font-weight: 500; margin-top: 4px; }
        .nav-btn svg { width: 1.5rem; height: 1.5rem; }

        /* Student Cards */
        .student-card {
            border-width: 2px;
            border-color: transparent;
            background-color: var(--card-bg);
            transition: all 0.2s ease-in-out;
            border-radius: 1rem;
        }
        .student-card.present {
            border-color: #34d399;
            background-color: rgba(16, 185, 129, 0.1);
        }
         .student-card-money.paid {
            border-color: var(--primary-accent);
            background-color: var(--primary-accent-light);
        }
        
        /* Modals */
        .modal-overlay {
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(0, 0, 0, 0.5);
            -webkit-backdrop-filter: blur(8px);
            backdrop-filter: blur(8px);
            display: flex;
            justify-content: center;
            z-index: 100;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }
        .modal-overlay.visible { opacity: 1; pointer-events: auto; }
        
        .modal-content {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            max-height: 90vh;
            padding: 1rem 1rem 2rem 1rem;
            border-top-left-radius: 1.5rem;
            border-top-right-radius: 1.5rem;
            transform: translateY(100%);
            transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
            overflow-y: auto;
        }
        .modal-overlay.visible .modal-content { transform: translateY(0); }

        .modal-handle {
            width: 4rem; height: 5px; background-color: var(--card-border);
            border-radius: 2.5px; margin: 0 auto 1.5rem auto;
        }
        
        .modal-center .modal-content {
            bottom: auto;
            top: 50%;
            left: 50%;
            width: 90%;
            max-width: 400px;
            max-height: 80vh;
            border-radius: 1.5rem;
            transform: translate(-50%, -50%) scale(0.9);
            transition: transform 0.3s ease, opacity 0.3s ease;
        }
        .modal-overlay.visible .modal-center .modal-content { transform: translate(-50%, -50%) scale(1); }
        .actions-btn { 
            display: block; 
            width: 100%; 
            text-align: left; 
            padding: 0.75rem 1rem; 
            border-radius: 0.75rem; 
            font-weight: 500; 
            transition: background-color 0.2s ease;
        } 
        .actions-btn:hover { background-color: rgba(0,0,0,0.05); } 
        body.dark-mode .actions-btn:hover { background-color: rgba(255,255,255,0.05); }


        /* Animations */
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .fade-in { animation: fadeIn 0.5s ease-out forwards; }
        .stagger-in > * {
            opacity: 0;
            animation: fadeIn 0.4s ease-out forwards;
        }

        /* Custom scrollbar */
        .custom-scrollbar::-webkit-scrollbar { width: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: transparent; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: var(--text-secondary); border-radius: 3px; }

        /* Hidden div for exporting */
        .export-container { position: absolute; left: -9999px; top: auto; width: 800px; z-index: -1; }
        #export-content { font-family: 'Times New Roman', Times, serif; color: #000; padding: 1rem; background-color: #fff; }
        #export-content h1 { font-size: 24px; font-weight: bold; text-align: center; margin-bottom: 20px; }
        #export-content h2 { font-size: 18px; font-weight: bold; border-bottom: 2px solid #333; padding-bottom: 5px; margin-top: 20px; margin-bottom: 10px; }
        #export-content .meta-info { text-align: center; margin-bottom: 20px; font-size: 12px; color: #555; }
        #export-content .summary-item { font-size: 14px; margin-bottom: 5px; }
        #export-content table { width: 100%; border-collapse: collapse; margin-top: 15px; page-break-inside: auto; }
        #export-content tr { page-break-inside: avoid; page-break-after: auto; }
        #export-content th, #export-content td { border: 1px solid #ccc; padding: 8px; text-align: left; font-size: 12px; }
        #export-content thead { display: table-header-group; } /* Ensures header repeats on each page */
        #export-content th { background-color: #e9ecef; font-weight: bold; }
        #export-content .footer { text-align: center; margin-top: 30px; font-size: 10px; color: #777; }
    </style>
</head>
<body>

    <div class="app-container">
        <!-- App Header -->
        <header class="app-header flex justify-between items-center mb-6">
            <div>
                <h1 class="text-2xl sm:text-3xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-indigo-500 to-purple-500 dark:from-indigo-400 dark:to-purple-400">Marker</h1>
                <p id="total-students-header" class="text-sm text-gray-600 dark:text-gray-400 mt-1">Available: 0</p>
            </div>
            <div class="flex items-center gap-2">
                <button id="theme-toggle-btn" class="p-2 rounded-full hover:bg-black/10 dark:hover:bg-white/10 transition-colors" aria-label="Toggle theme">
                    <svg id="theme-icon-sun" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" /></svg>
                    <svg id="theme-icon-moon" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 hidden" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" /></svg>
                </button>
                <button id="actions-menu-btn" class="p-2 rounded-full hover:bg-black/10 dark:hover:bg-white/10 transition-colors" aria-label="Actions menu">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 5v.01M12 12v.01M12 19v.01M12 6a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2z" /></svg>
                </button>
            </div>
        </header>
        
        <!-- Main Content Area -->
        <main id="main-content">
            <!-- Sections will be dynamically rendered here -->
        </main>

        <!-- Bottom Navigation -->
        <nav class="bottom-nav">
             <div class="bottom-nav-inner glass-card">
                <button class="nav-btn" data-tab="dashboard" aria-label="Dashboard">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6A2.25 2.25 0 0 1 6 3.75h2.25A2.25 2.25 0 0 1 10.5 6v2.25a2.25 2.25 0 0 1-2.25 2.25H6a2.25 2.25 0 0 1-2.25-2.25V6ZM3.75 15.75A2.25 2.25 0 0 1 6 13.5h2.25a2.25 2.25 0 0 1 2.25 2.25v2.25A2.25 2.25 0 0 1 8.25 21H6a2.25 2.25 0 0 1-2.25-2.25v-2.25ZM13.5 6a2.25 2.25 0 0 1 2.25-2.25H18A2.25 2.25 0 0 1 20.25 6v2.25A2.25 2.25 0 0 1 18 10.5h-2.25a2.25 2.25 0 0 1-2.25-2.25V6ZM13.5 15.75a2.25 2.25 0 0 1 2.25-2.25H18a2.25 2.25 0 0 1 2.25 2.25v2.25A2.25 2.25 0 0 1 18 21h-2.25A2.25 2.25 0 0 1 13.5 18.75v-2.25Z" /></svg>
                    <span>Dashboard</span>
                </button>
                <button class="nav-btn" data-tab="attendance" aria-label="Attendance">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" /></svg>
                    <span>Attendance</span>
                </button>
                <button class="nav-btn" data-tab="money" aria-label="Money">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M2.25 18.75a60.07 60.07 0 0 1 15.797 2.101c.727.198 1.453-.342 1.453-1.096V18.75M3.75 4.5v.75A.75.75 0 0 1 3 6h-.75m0 0v-.375c0-.621.504-1.125 1.125-1.125H20.25M2.25 6v9m18-10.5v.75c0 .414.336.75.75.75h.75m-1.5-1.5h.375c.621 0 1.125.504 1.125 1.125v9.75c0 .621-.504 1.125-1.125 1.125h-15c-.621 0-1.125-.504-1.125-1.125V8.25m18-3.75v9m-18-9v9" /></svg>
                    <span>Money</span>
                </button>
            </div>
        </nav>
    </div>

    <!-- Modals Container -->
    <div id="modal-container"></div>
    
    <!-- Hidden container for generating exports -->
    <div class="export-container">
        <div id="export-content"></div>
    </div>

    <!-- Main Application Script -->
    <script>
    /*****************************************************************************************
     * README: Student List Marker Application - v2 (Modern UI)
     *****************************************************************************************
     * This is a refactored version with a modern, mobile-first UI, including glassmorphism,
     * dark mode, and improved user interactions. All original functionality is preserved.
     *****************************************************************************************/
    document.addEventListener('DOMContentLoaded', () => {
        // --- INITIAL DATA ---
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
        let savedScrollPositions = { attendance: 0, money: 0 };
        const { jsPDF } = window.jspdf;

        const defaultState = {
            students: [],
            attendanceHistory: [],
            moneyHistory: [],
            currentTab: 'dashboard',
            ui: {
                attendance: { filteredStudents: [], searchText: '', searchMode: 'name', filterType: 'all', filterRangePreset: 'all', customRange: '', statusFilter: 'all' },
                money: { filteredStudents: [], searchText: '', searchMode: 'name', filterType: 'all', filterRangePreset: 'all', customRange: '', statusFilter: 'all', paymentMethodFilter: 'all', purpose: '', defaultAmount: '', collector: '' },
                history: { viewingItem: null, searchText: '', editingSession: { type: null, id: null } }
            }
        };

        const mainContent = document.getElementById('main-content');
        const modalContainer = document.getElementById('modal-container');
        const totalStudentsHeader = document.getElementById('total-students-header');

        // --- All Function Definitions ---

        function deepClone(obj) { return JSON.parse(JSON.stringify(obj)); }
        function debounce(func, delay) {
            let timeout;
            return function(...args) {
                clearTimeout(timeout);
                timeout = setTimeout(() => func.apply(this, args), delay);
            };
        }
        function autosaveCurrentWork() {
            const workInProgress = {
                attendance: state.students.map(s => ({ reg: s.reg, present: s.present })),
                money: state.students.map(s => ({ reg: s.reg, payment: s.payment })),
                moneyUi: {
                    purpose: state.ui.money.purpose,
                    defaultAmount: state.ui.money.defaultAmount,
                    collector: state.ui.money.collector,
                }
            };
            localStorage.setItem('studentMarkerWorkInProgress', JSON.stringify(workInProgress));
        }
        const debouncedAutosave = debounce(autosaveCurrentWork, 1500);

        function recordState(shouldAutosave = true) {
            if (historyPointer < stateHistory.length - 1) {
                stateHistory = stateHistory.slice(0, historyPointer + 1);
            }
            stateHistory.push(deepClone(state));
            historyPointer++;
            if (shouldAutosave) {
                debouncedAutosave();
            }
        }

        function undo() {
            if (historyPointer > 0) {
                historyPointer--;
                loadStateFromHistory();
            }
        }

        function redo() {
            if (historyPointer < stateHistory.length - 1) {
                historyPointer++;
                loadStateFromHistory();
            }
        }
        
        function loadStateFromHistory() {
            state = deepClone(stateHistory[historyPointer]);
            renderApp();
        }

        function saveState() {
            const appData = {
                students: state.students.map(({ id, present, payment, ...rest }) => rest),
                attendanceHistory: state.attendanceHistory,
                moneyHistory: state.moneyHistory,
            };
            localStorage.setItem('studentMarkerData', JSON.stringify(appData));
        }

        function loadState() {
            const savedData = localStorage.getItem('studentMarkerData');
            state = deepClone(defaultState);
            if (savedData) {
                const parsedData = JSON.parse(savedData);
                state.students = (parsedData.students || initialStudents).map((s, i) => ({ ...s, id: i + 1, present: undefined, payment: null }));
                state.attendanceHistory = parsedData.attendanceHistory || [];
                state.moneyHistory = parsedData.moneyHistory || [];
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
                if (workInProgress.money) {
                    workInProgress.money.forEach(wipStudent => {
                        const student = state.students.find(s => s.reg === wipStudent.reg);
                        if (student) student.payment = wipStudent.payment;
                    });
                }
                if (workInProgress.moneyUi) {
                    state.ui.money.purpose = workInProgress.moneyUi.purpose || '';
                    state.ui.money.defaultAmount = workInProgress.moneyUi.defaultAmount || '';
                    state.ui.money.collector = workInProgress.moneyUi.collector || '';
                }
            }
        }
        
        // --- THEME ---
        function applyTheme(theme) {
            const sunIcon = document.getElementById('theme-icon-sun');
            const moonIcon = document.getElementById('theme-icon-moon');
            if (theme === 'dark') {
                document.body.classList.add('dark-mode');
                sunIcon.classList.add('hidden');
                moonIcon.classList.remove('hidden');
            } else {
                document.body.classList.remove('dark-mode');
                sunIcon.classList.remove('hidden');
                moonIcon.classList.add('hidden');
            }
        }
        function toggleTheme() {
            const currentTheme = document.body.classList.contains('dark-mode') ? 'dark' : 'light';
            const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
            localStorage.setItem('studentMarkerTheme', newTheme);
            applyTheme(newTheme);
        }

        function init() {
            loadState();
            recordState(false);
            const savedTheme = localStorage.getItem('studentMarkerTheme') || (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
            applyTheme(savedTheme);
            renderApp();
            setupEventListeners();
        }

        function renderApp() {
            totalStudentsHeader.textContent = `Available: ${state.students.length}`;
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.toggle('active', btn.dataset.tab === state.currentTab);
            });
            renderCurrentTab();
        }

        function switchTab(tabName) {
            if (state.currentTab === tabName) return;
            state.ui.history.viewingItem = null;
            state.currentTab = tabName;
            renderApp();
        }

        function renderCurrentTab() {
            mainContent.innerHTML = '';
            let content = '';
            switch (state.currentTab) {
                case 'dashboard': content = getDashboardHTML(); break;
                case 'attendance': content = getAttendanceHTML(); break;
                case 'money': content = getMoneyHTML(); break;
            }
            mainContent.innerHTML = content;

            // Post-render updates
            if (state.currentTab === 'attendance') updateAttendanceList();
            if (state.currentTab === 'money') updateMoneyList();
            if (state.currentTab === 'dashboard') renderHistoryLists();
        }

        function saveCurrentScrollPositions() {
            const listEl = document.querySelector('.student-list-container');
            if (listEl) {
                savedScrollPositions[state.currentTab] = listEl.scrollTop;
            }
        }
        
        function getEditingBannerHTML(session) {
            return `<div class="p-4 rounded-2xl mb-4 flex justify-between items-center bg-yellow-400/20 text-yellow-800 dark:text-yellow-300">
                <div>
                    <p class="font-bold">Editing a past session</p>
                    <p class="text-sm">Changes will overwrite the record from ${session.date}.</p>
                </div>
                <button id="cancel-edit-btn" class="bg-black/10 dark:bg-white/10 p-2 rounded-full">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
                </button>
            </div>`;
        }
        
        // --- TEMPLATES (get...HTML functions) ---
        
        function getCommonControlsHTML(tab) {
            const { searchText, searchMode } = state.ui[tab];
            const placeholder = searchMode === 'name' ? "Search by name..." : "Search by Reg No. ...";
            return `
                <div class="glass-card p-4 space-y-4">
                    <div class="relative">
                        <input id="${tab}-search-input" type="text" placeholder="${placeholder}" class="w-full py-3 px-4 bg-transparent border-2 border-gray-300 dark:border-gray-600 rounded-xl focus:ring-2 focus:ring-primary-accent focus:border-transparent outline-none" value="${searchText}">
                        <div class="absolute right-2 top-1/2 -translate-y-1/2 flex items-center gap-1 bg-[var(--bg-end)] p-1 rounded-lg">
                             <button class="search-mode-btn p-1.5 rounded-md ${searchMode === 'name' ? 'bg-white dark:bg-gray-900 shadow' : ''}" data-search-mode="name" title="Search by Name">
                                 <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 pointer-events-none" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z" clip-rule="evenodd" /></svg>
                             </button>
                             <button class="search-mode-btn p-1.5 rounded-md ${searchMode === 'reg' ? 'bg-white dark:bg-gray-900 shadow' : ''}" data-search-mode="reg" title="Search by Reg No.">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 pointer-events-none" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M7 20l4-16m2 16l4-16M6 9h14M4 15h14" /></svg>
                            </button>
                        </div>
                    </div>
                    <button id="open-filter-modal-btn" class="w-full text-center py-3 font-semibold text-indigo-600 dark:text-indigo-400 bg-indigo-500/10 rounded-xl">
                        Filter Students
                    </button>
                </div>
            `;
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
                    ${getCommonControlsHTML('attendance')}
                    <div class="glass-card p-4 space-y-4">
                        <div id="attendance-summary" class="grid grid-cols-3 gap-2 text-center"></div>
                        <div class="grid grid-cols-2 gap-2">
                            <button id="mark-all-visible-present" class="py-2 bg-green-500/20 text-green-700 dark:text-green-300 font-semibold rounded-lg">Mark Visible Present</button>
                            <button id="clear-all-visible" class="py-2 bg-gray-500/20 text-gray-700 dark:text-gray-300 font-semibold rounded-lg">Clear Visible</button>
                        </div>
                    </div>
                    <div id="attendance-list" class="student-list-container grid grid-cols-1 sm:grid-cols-2 gap-3 max-h-[50vh] overflow-y-auto custom-scrollbar p-1"></div>
                    <div class="glass-card p-4 space-y-4">
                        <input id="taken-by-input" type="text" placeholder="Taken by (e.g., Dr. Smith)" class="w-full bg-transparent p-3 border-2 border-gray-300 dark:border-gray-600 rounded-xl" value="${takenByValue}">
                        <input id="subject-input" type="text" placeholder="Subject / Purpose" class="w-full bg-transparent p-3 border-2 border-gray-300 dark:border-gray-600 rounded-xl" value="${subjectValue}">
                        <div class="flex gap-2">
                             <button id="reset-attendance-btn" class="w-full bg-yellow-500 text-white mt-2 py-3 rounded-xl font-semibold transition-transform active:scale-95">Reset</button>
                            <button id="save-attendance-btn" class="w-full bg-indigo-600 text-white mt-2 py-3 rounded-xl font-semibold transition-transform active:scale-95">${isEditing ? 'Update Session' : 'Save Attendance'}</button>
                        </div>
                    </div>
                </div>
            `;
        }
        
        function getMoneyHTML() {
            const isEditing = state.ui.history.editingSession.type === 'money';
            const session = isEditing ? state.moneyHistory.find(s => s.id === state.ui.history.editingSession.id) : null;
            const purposeValue = session ? session.purpose : state.ui.money.purpose;
            const defaultAmountValue = state.ui.money.defaultAmount;
            const collectorValue = session ? session.collector : state.ui.money.collector;
            const bannerHTML = isEditing ? getEditingBannerHTML(session) : '';

            return `
                 <div class="space-y-6">
                    ${bannerHTML}
                    <div class="glass-card p-4 space-y-3">
                         <input id="money-purpose" type="text" placeholder="Purpose (e.g., Field Trip)" class="w-full bg-transparent p-3 border-2 border-gray-300 dark:border-gray-600 rounded-xl" value="${purposeValue}">
                         <div class="grid grid-cols-2 gap-3">
                            <input id="money-amount" type="number" placeholder="Default Amt (₹)" class="w-full bg-transparent p-3 border-2 border-gray-300 dark:border-gray-600 rounded-xl" value="${defaultAmountValue}">
                            <input id="money-collector" type="text" placeholder="Collector" class="w-full bg-transparent p-3 border-2 border-gray-300 dark:border-gray-600 rounded-xl" value="${collectorValue}">
                        </div>
                    </div>
                    ${getCommonControlsHTML('money')}
                    <div id="money-summary" class="glass-card p-4 grid grid-cols-2 md:grid-cols-3 gap-2 text-center"></div>
                    <div id="money-list" class="student-list-container grid grid-cols-1 sm:grid-cols-2 gap-3 max-h-[50vh] overflow-y-auto custom-scrollbar p-1"></div>
                    <div class="glass-card p-4 flex gap-2">
                         <button id="reset-money-btn" class="w-full bg-yellow-500 text-white py-3 rounded-xl font-semibold transition-transform active:scale-95">Reset Payments</button>
                        <button id="save-money-btn" class="w-full bg-indigo-600 text-white py-3 rounded-xl font-semibold transition-transform active:scale-95">${isEditing ? 'Update Collection' : 'Save Collection'}</button>
                    </div>
                </div>
            `;
        }

        function getDashboardHTML() {
            const totalAttendance = state.attendanceHistory.length;
            const totalMoney = state.moneyHistory.length;
            const totalStudents = state.students.length;

            const recentHistory = [...state.attendanceHistory, ...state.moneyHistory]
                .sort((a, b) => b.id.split('-')[1] - a.id.split('-')[1])
                .slice(0, 3);

            return `
                <div class="space-y-6">
                    <!-- Quick Stats -->
                    <div class="grid grid-cols-3 gap-4 text-center">
                        <div class="glass-card p-4">
                            <p class="font-bold text-3xl text-indigo-500 dark:text-indigo-400">${totalStudents}</p>
                            <p class="text-sm text-secondary">Students</p>
                        </div>
                        <div class="glass-card p-4">
                            <p class="font-bold text-3xl text-indigo-500 dark:text-indigo-400">${totalAttendance}</p>
                            <p class="text-sm text-secondary">Attendances</p>
                        </div>
                        <div class="glass-card p-4">
                            <p class="font-bold text-3xl text-indigo-500 dark:text-indigo-400">${totalMoney}</p>
                            <p class="text-sm text-secondary">Collections</p>
                        </div>
                    </div>

                    <!-- Recent Activity -->
                    <div>
                        <h2 class="text-xl font-bold mb-4">Recent Activity</h2>
                        <div class="space-y-4">
                            ${recentHistory.length > 0 ? recentHistory.map(getHistoryCardHTML).join('') : '<p class="text-gray-500 p-4 text-center glass-card rounded-2xl">No recent activity.</p>'}
                        </div>
                    </div>

                    <!-- Full History Section -->
                    <div class="relative mt-6">
                        <input id="history-search-input" type="text" placeholder="Search all history..." class="w-full py-3 px-4 bg-transparent border-2 border-gray-300 dark:border-gray-600 rounded-xl focus:ring-2 focus:ring-primary-accent focus:border-transparent outline-none" value="${state.ui.history.searchText}">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 absolute right-4 top-1/2 -translate-y-1/2 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" /></svg>
                    </div>
                    <div class="grid grid-cols-1 gap-6">
                        <div>
                            <h2 class="text-xl font-bold mb-4">All Attendance Sessions</h2>
                            <div id="history-attendance-list" class="space-y-4"></div>
                        </div>
                        <div>
                            <h2 class="text-xl font-bold mb-4">All Money Collections</h2>
                            <div id="history-money-list" class="space-y-4"></div>
                        </div>
                    </div>
                </div>
            `;
        }
        
        function getStudentCardHTML(student) {
            const isPresent = student.present === true;
            const filterType = state.ui.attendance.filterType;
            let circleContent = '';
            
            switch(filterType) {
                case 'all': circleContent = student.name.split(' ').map(n => n[0]).join('').substring(0, 2).toUpperCase(); break;
                case 'reg': circleContent = parseInt(student.reg.slice(-3)).toString().padStart(2, '0'); break;
                case 'index': circleContent = student.id.toString().padStart(2, '0'); break;
            }

            return `
                <div class="student-card flex items-center p-3 cursor-pointer ${isPresent ? 'present' : ''}" data-reg="${student.reg}" style="--stagger-index: ${student.id}">
                    <div class="w-10 h-10 rounded-full bg-indigo-200 dark:bg-indigo-900 flex items-center justify-center mr-3 flex-shrink-0 font-bold text-indigo-800 dark:text-indigo-300">${circleContent}</div>
                    <p class="font-semibold truncate flex-grow">${student.name}</p>
                </div>
            `;
        }

        function getMoneyStudentCardHTML(student) {
            const isPaid = !!student.payment;
            const amount = isPaid ? student.payment.amount : (state.ui.money.defaultAmount || '');
            const method = isPaid ? student.payment.method : 'Cash';
            const filterType = state.ui.money.filterType;
            let circleContent = '';
            const circleClasses = isPaid
                ? 'bg-indigo-500 dark:bg-indigo-600 text-white dark:text-indigo-100'
                : 'bg-indigo-200 dark:bg-indigo-900 text-indigo-800 dark:text-indigo-300';

            switch(filterType) {
                case 'all': circleContent = student.name.split(' ').map(n => n[0]).join('').substring(0, 2).toUpperCase(); break;
                case 'reg': circleContent = parseInt(student.reg.slice(-3)).toString().padStart(2, '0'); break;
                case 'index': circleContent = student.id.toString().padStart(2, '0'); break;
            }

            return `
                <div class="student-card-money flex flex-col p-3 rounded-xl border-2 border-transparent ${isPaid ? 'paid' : 'bg-[var(--card-bg)]'}">
                    <div class="flex items-center mb-2">
                        <div class="w-10 h-10 rounded-full flex items-center justify-center mr-3 flex-shrink-0 font-bold ${circleClasses}">${circleContent}</div>
                        <p class="font-semibold truncate flex-grow">${student.name}</p>
                        <input type="checkbox" class="money-student-checkbox h-6 w-6 ml-2 flex-shrink-0 rounded-md accent-[var(--primary-accent)]" data-reg="${student.reg}" ${isPaid ? 'checked' : ''}>
                    </div>
                    <div class="flex items-center gap-2 mt-1 ${isPaid ? '' : 'hidden'}">
                        <input type="number" placeholder="Amt" class="money-amount-input w-full p-2 bg-transparent border-2 border-gray-300 dark:border-gray-600 rounded-lg text-sm" data-reg="${student.reg}" value="${amount}">
                        <select class="money-method-select p-2 bg-transparent border-2 border-gray-300 dark:border-gray-600 rounded-lg text-sm" data-reg="${student.reg}">
                            <option value="Cash" ${method === 'Cash' ? 'selected' : ''}>Cash</option>
                            <option value="ePay" ${method === 'ePay' ? 'selected' : ''}>ePay</option>
                        </select>
                    </div>
                </div>
            `;
        }
        
        function getHistoryCardHTML(item) {
            let statsHTML = '';
            if (item.type === 'attendance') {
                const present = item.records.filter(r => r.present).length;
                const absent = item.records.length - present;
                statsHTML = `<div class="flex justify-around text-center text-sm">
                    <div><span class="font-bold text-lg text-green-600 dark:text-green-400">${present}</span><p>Present</p></div>
                    <div><span class="font-bold text-lg text-red-600 dark:text-red-400">${absent}</span><p>Absent</p></div>
                    <div><span class="font-bold text-lg">${item.records.length}</span><p>Total</p></div>
                </div>`;
            } else { // money
                const paid = item.records.length;
                const totalCollected = item.records.reduce((sum, r) => sum + parseFloat(r.payment.amount || 0), 0);
                statsHTML = `<div class="flex justify-around text-center text-sm">
                    <div><span class="font-bold text-lg text-blue-600 dark:text-blue-400">${paid}</span><p>Paid</p></div>
                    <div><span class="font-bold text-lg">₹${totalCollected.toFixed(2)}</span><p>Collected</p></div>
                </div>`;
            }

            return `
                <div class="glass-card p-4 flex flex-col gap-3" data-id="${item.id}" data-type="${item.type}">
                    <div>
                        <p class="font-bold text-lg">${item.subject || item.purpose}</p>
                        <p class="text-xs text-gray-500 dark:text-gray-400">${item.date} at ${item.time}</p>
                    </div>
                    <div class="bg-black/5 dark:bg-white/5 p-2 rounded-lg">${statsHTML}</div>
                    <div class="grid grid-cols-4 gap-2 pt-3 border-t border-gray-200 dark:border-gray-700">
                        <button class="view-history-btn text-sm bg-gray-500/20 py-2 rounded-lg" title="View">View</button>
                        <button class="edit-history-btn text-sm bg-blue-500/20 text-blue-800 dark:text-blue-300 py-2 rounded-lg" title="Edit">Edit</button>
                        <button class="export-history-btn text-sm bg-indigo-500/20 text-indigo-800 dark:text-indigo-300 py-2 rounded-lg" title="Export">Export</button>
                        <button class="delete-history-btn p-2 rounded-lg text-red-600 hover:bg-red-500/20" title="Delete">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mx-auto pointer-events-none" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg>
                        </button>
                    </div>
                </div>`;
        }
        
        // --- RENDERERS & UPDATERS ---
        function updateAttendanceList() {
            const listEl = document.getElementById('attendance-list');
            if (!listEl) return;

            const studentsToDisplay = getFilteredStudents('attendance');
            if (studentsToDisplay.length === 0) {
                listEl.innerHTML = `<p class="col-span-full text-center text-gray-500 py-8">No students match the current filter.</p>`;
            } else {
                listEl.innerHTML = studentsToDisplay.map(s => getStudentCardHTML(s)).join('');
                listEl.querySelectorAll('.student-card').forEach((card, index) => {
                    card.style.animationDelay = `${index * 0.03}s`;
                });
            }
            updateAttendanceSummary();
            
            if (savedScrollPositions.attendance > 0) {
                listEl.scrollTop = savedScrollPositions.attendance;
                savedScrollPositions.attendance = 0;
            }
        }
        
        function updateMoneyList() {
            const listEl = document.getElementById('money-list');
            if (!listEl) return;

            const studentsToDisplay = getFilteredStudents('money');
             if (studentsToDisplay.length === 0) {
                listEl.innerHTML = `<p class="col-span-full text-center text-gray-500 py-8">No students match the current filter.</p>`;
            } else {
                listEl.innerHTML = studentsToDisplay.map(s => getMoneyStudentCardHTML(s)).join('');
                 listEl.querySelectorAll('.student-card-money').forEach((card, index) => {
                    card.style.animationDelay = `${index * 0.03}s`;
                });
            }
            updateMoneySummary();
            
            if (savedScrollPositions.money > 0) {
                listEl.scrollTop = savedScrollPositions.money;
                savedScrollPositions.money = 0;
            }
        }
        
        function renderHistoryLists() {
            const attendanceListEl = document.getElementById('history-attendance-list');
            const moneyListEl = document.getElementById('history-money-list');
            const { searchText } = state.ui.history;
            const lowerSearchText = searchText.toLowerCase().trim();

            let filteredAttendance = state.attendanceHistory.filter(item => !lowerSearchText || (item.subject && item.subject.toLowerCase().includes(lowerSearchText)) || (item.takenBy && item.takenBy.toLowerCase().includes(lowerSearchText)) || item.date.includes(lowerSearchText));
            let filteredMoney = state.moneyHistory.filter(item => !lowerSearchText || (item.purpose && item.purpose.toLowerCase().includes(lowerSearchText)) || (item.collector && item.collector.toLowerCase().includes(lowerSearchText)) || item.date.includes(lowerSearchText));
            
            if (attendanceListEl) {
                attendanceListEl.innerHTML = filteredAttendance.length > 0 ? filteredAttendance.map(getHistoryCardHTML).join('') : `<p class="text-gray-500 p-2 text-center">No attendance history found.</p>`;
            }
            if (moneyListEl) {
                moneyListEl.innerHTML = filteredMoney.length > 0 ? filteredMoney.map(getHistoryCardHTML).join('') : `<p class="text-gray-500 p-2 text-center">No money collection history found.</p>`;
            }
        }

        function updateAttendanceSummary() {
            const summaryEl = document.getElementById('attendance-summary');
            if(!summaryEl) return;
            
            const relevantStudents = getRangeFilteredStudents('attendance');
            const totalCount = relevantStudents.length;
            const presentCount = relevantStudents.filter(s => s.present === true).length;
            const absentCount = totalCount - presentCount;

            summaryEl.innerHTML = `
                <div><p class="font-bold text-2xl">${totalCount}</p><p class="text-sm text-secondary">Total</p></div>
                <div><p class="font-bold text-2xl text-green-600 dark:text-green-400">${presentCount}</p><p class="text-sm text-secondary">Present</p></div>
                <div><p class="font-bold text-2xl text-red-600 dark:text-red-400">${absentCount}</p><p class="text-sm text-secondary">Absent</p></div>
            `;
        }

        function updateMoneySummary() {
            const summaryEl = document.getElementById('money-summary');
            if(!summaryEl) return;

            const relevantStudents = getRangeFilteredStudents('money');
            const paidStudents = relevantStudents.filter(s => s.payment);
            const totalReceived = paidStudents.reduce((sum, s) => sum + (parseFloat(s.payment.amount) || 0), 0);
            
            summaryEl.innerHTML = `
                 <div><p class="text-sm text-secondary">Students</p><p class="font-bold text-xl">${relevantStudents.length}</p></div>
                 <div><p class="text-sm text-secondary">Paid</p><p class="font-bold text-xl text-green-600 dark:text-green-400">${paidStudents.length}</p></div>
                 <div><p class="text-sm text-secondary">Unpaid</p><p class="font-bold text-xl text-red-600 dark:text-red-400">${relevantStudents.length - paidStudents.length}</p></div>
                 <div class="col-span-3"><p class="text-sm text-secondary">Total Collected</p><p class="font-bold text-3xl text-green-600 dark:text-green-400">₹${totalReceived.toFixed(2)}</p></div>
            `;
        }

        // --- FILTER & PARSING LOGIC ---
        function parseRange(rangeStr) {
            const result = new Set();
            if (!rangeStr) return result;
            rangeStr.split(',').map(s => s.trim()).forEach(part => {
                if (part.includes('-')) {
                    const [start, end] = part.split('-').map(Number);
                    if (!isNaN(start) && !isNaN(end)) {
                        for (let i = start; i <= end; i++) result.add(i);
                    }
                } else {
                    const num = Number(part);
                    if (!isNaN(num)) result.add(num);
                }
            });
            return result;
        }

        function getRangeFilteredStudents(tab) {
            let studentsToDisplay = [...state.students];
            const uiState = state.ui[tab];

            if (uiState.searchText) {
                const lowerSearchText = uiState.searchText.toLowerCase();
                studentsToDisplay = studentsToDisplay.filter(student => uiState.searchMode === 'name' ? student.name.toLowerCase().includes(lowerSearchText) : student.reg.endsWith(uiState.searchText));
            }
            
            const { filterType, filterRangePreset, customRange } = uiState;
            if (filterType === 'all' || filterRangePreset === 'all') {
                return studentsToDisplay;
            }

            let rangeSet = null;
            if (filterRangePreset === '1-50') rangeSet = parseRange('1-50');
            else if (filterRangePreset === '51-99' || filterRangePreset === '51-95') rangeSet = parseRange(filterRangePreset);
            else if (filterRangePreset === 'custom' && customRange) rangeSet = parseRange(customRange);

            if (rangeSet && rangeSet.size > 0) {
                studentsToDisplay = studentsToDisplay.filter(student => {
                    const valueToCheck = (filterType === 'index') ? student.id : parseInt(student.reg.slice(-3));
                    return rangeSet.has(valueToCheck);
                });
            } else if (filterRangePreset !== 'custom') {
                return [];
            }
            return studentsToDisplay;
        }

        function getFilteredStudents(tab) {
            const uiState = state.ui[tab];
            let studentsToDisplay = getRangeFilteredStudents(tab);
            
            if (tab === 'attendance') {
                const { statusFilter } = uiState;
                if (statusFilter === 'present') studentsToDisplay = studentsToDisplay.filter(s => s.present === true);
                else if (statusFilter === 'absent') studentsToDisplay = studentsToDisplay.filter(s => s.present !== true);
            } else if (tab === 'money') {
                const { statusFilter, paymentMethodFilter } = uiState;
                studentsToDisplay = studentsToDisplay.filter(student => {
                    const isPaid = !!student.payment;
                    const method = isPaid ? student.payment.method : null;
                    const statusMatch = (statusFilter === 'all') || (statusFilter === 'paid' && isPaid) || (statusFilter === 'unpaid' && !isPaid);
                    if (!statusMatch) return false;
                    const methodMatch = (paymentMethodFilter === 'all') || !isPaid || (isPaid && method === paymentMethodFilter);
                    if (!methodMatch) return false;
                    return true;
                });
            }
            uiState.filteredStudents = studentsToDisplay;
            return studentsToDisplay;
        }

        // --- EVENT HANDLERS ---
        function setupEventListeners() {
            document.getElementById('theme-toggle-btn').addEventListener('click', toggleTheme);
            document.getElementById('actions-menu-btn').addEventListener('click', showActionsModal);
            document.querySelector('.bottom-nav').addEventListener('click', (e) => {
                const navBtn = e.target.closest('.nav-btn');
                if (navBtn) switchTab(navBtn.dataset.tab);
            });
            document.body.addEventListener('input', handleBodyInput);
            document.body.addEventListener('click', handleBodyClick);
            document.body.addEventListener('change', handleBodyChange);
        }

        function handleBodyInput(e) {
            const tab = state.currentTab;
            if (e.target.id === `${tab}-search-input`) {
                state.ui[tab].searchText = e.target.value;
                if (tab === 'attendance') updateAttendanceList();
                if (tab === 'money') updateMoneyList();
            } else if (e.target.id === 'history-search-input') {
                state.ui.history.searchText = e.target.value;
                renderHistoryLists();
            } else if (e.target.id === `${tab}-custom-range`) {
                state.ui[tab].customRange = e.target.value;
            } else if (e.target.matches('#money-purpose, #money-amount, #money-collector')) {
                state.ui.money.purpose = document.getElementById('money-purpose')?.value;
                state.ui.money.defaultAmount = document.getElementById('money-amount')?.value;
                state.ui.money.collector = document.getElementById('money-collector')?.value;
                debouncedAutosave();
                if (e.target.id === 'money-amount') updateMoneySummary();
            } else if (e.target.matches('.money-amount-input')) {
                 const student = state.students.find(s => s.reg === e.target.dataset.reg);
                 if (student && student.payment) {
                    student.payment.amount = e.target.value;
                    recordState();
                    updateMoneySummary();
                 }
            }
        }

        function handleBodyClick(e) {
            const tab = state.currentTab;
            
            const studentCard = e.target.closest('.student-card');
            if (studentCard && tab === 'attendance') {
                saveCurrentScrollPositions();
                const student = state.students.find(s => s.reg === studentCard.dataset.reg);
                if (student) student.present = student.present === true ? undefined : true;
                recordState();
                updateAttendanceList();
                return;
            }

            const searchModeBtn = e.target.closest('.search-mode-btn');
            if (searchModeBtn && state.ui[tab] && state.ui[tab].searchMode !== searchModeBtn.dataset.searchMode) {
                state.ui[tab].searchMode = searchModeBtn.dataset.searchMode;
                renderCurrentTab();
                return;
            }

            const modalTarget = e.target.closest('.modal-overlay');
            if (modalTarget) handleModalClick(e);

            const historyCard = e.target.closest('[data-id][data-type]');
            if(historyCard && tab === 'dashboard') {
                const { id, type } = historyCard.dataset;
                const item = type === 'attendance' ? state.attendanceHistory.find(i=>i.id===id) : state.moneyHistory.find(i=>i.id===id);

                if (e.target.closest('.view-history-btn')) viewHistoryDetail(item);
                if (e.target.closest('.delete-history-btn')) deleteHistoryItem(item);
                if (e.target.closest('.edit-history-btn')) startEditingHistoryItem(item);
                if (e.target.closest('.export-history-btn')) showExportOptionsModal(item);
                return;
            }
            
            const targetId = e.target.id;
            let stateChanged = false;
             switch (targetId) {
                case 'save-attendance-btn': saveAttendance(); break;
                case 'save-money-btn': saveMoneyCollection(); break;
                case 'reset-attendance-btn': 
                    state.students.forEach(s => s.present = undefined);
                    stateChanged = true;
                    break;
                case 'reset-money-btn':
                    state.students.forEach(s => s.payment = null);
                    stateChanged = true;
                    break;
                case 'mark-all-visible-present':
                    state.ui.attendance.filteredStudents.forEach(fs => {
                        const student = state.students.find(s => s.reg === fs.reg);
                        if(student) student.present = true;
                    });
                    stateChanged = true;
                    break;
                case 'clear-all-visible':
                    state.ui.attendance.filteredStudents.forEach(fs => {
                        const student = state.students.find(s => s.reg === fs.reg);
                        if(student) student.present = undefined;
                    });
                    stateChanged = true;
                    break;
                case 'cancel-edit-btn': cancelEditing(); break;
                case 'open-filter-modal-btn': showFilterModal(); break;
            }

            if(stateChanged) {
                saveCurrentScrollPositions();
                recordState();
                renderApp();
            }
        }
        
        function handleBodyChange(e) {
            let stateChanged = false;
            if (e.target.classList.contains('money-student-checkbox')) {
                const student = state.students.find(s => s.reg === e.target.dataset.reg);
                if (student) {
                    if (e.target.checked) {
                        const defaultAmount = document.getElementById('money-amount')?.value || 0;
                        student.payment = { amount: defaultAmount, method: 'Cash' };
                    } else {
                        student.payment = null;
                    }
                    stateChanged = true;
                }
            }
            if (e.target.classList.contains('money-method-select')) {
                const student = state.students.find(s => s.reg === e.target.dataset.reg);
                if (student && student.payment) {
                    student.payment.method = e.target.value;
                    stateChanged = true;
                }
            }
            
            if(stateChanged) {
                saveCurrentScrollPositions();
                recordState();
                if (state.currentTab === 'money') {
                    updateMoneyList();
                }
            }
        }
        
        function handleModalClick(e) {
             if (e.target.closest('.close-modal-btn') || e.target.classList.contains('modal-overlay')) {
                closeCurrentModal();
            }
            const filterTypeBtn = e.target.closest('[data-filter-type]');
            if (filterTypeBtn) {
                state.ui[state.currentTab].filterType = filterTypeBtn.dataset.filterType;
                state.ui[state.currentTab].filterRangePreset = 'all'; 
                showFilterModal();
            }
            const filterRangeBtn = e.target.closest('[data-filter-range]');
            if (filterRangeBtn) {
                 state.ui[state.currentTab].filterRangePreset = filterRangeBtn.dataset.filterRange;
                 showFilterModal();
            }
            const statusFilterBtn = e.target.closest('[data-status-filter]');
            if (statusFilterBtn) {
                state.ui[state.currentTab].statusFilter = statusFilterBtn.dataset.statusFilter;
                showFilterModal();
            }
            const methodFilterBtn = e.target.closest('[data-method-filter]');
            if (methodFilterBtn) {
                state.ui[state.currentTab].paymentMethodFilter = methodFilterBtn.dataset.methodFilter;
                showFilterModal();
            }
            if(e.target.id === 'apply-filters-btn') {
                closeCurrentModal();
                renderApp();
            }
        }

        // --- CORE LOGIC (save, import, export etc.) ---
        function saveAttendance() {
            const now = new Date();
            const takenBy = document.getElementById('taken-by-input')?.value.trim() || 'N/A';
            const subject = document.getElementById('subject-input')?.value.trim() || 'General';
            const recordsToSave = state.students.map(({ reg, name, present }) => ({ reg, name, present: present === true }));

            if(recordsToSave.filter(r => r.present).length === 0) {
                showModal({ title: "No Students Marked", content: "Please mark at least one student as present before saving.", isCenter: true });
                return;
            }
            
            const { type, id } = state.ui.history.editingSession;
            if (type === 'attendance' && id) {
                const sessionIndex = state.attendanceHistory.findIndex(s => s.id === id);
                if (sessionIndex !== -1) {
                    state.attendanceHistory[sessionIndex] = { ...state.attendanceHistory[sessionIndex], takenBy, subject, records: recordsToSave };
                }
                cancelEditing(false);
            } else {
                const session = { id: `att-${Date.now()}`, type: 'attendance', date: now.toLocaleDateString(), time: now.toLocaleTimeString(), takenBy, subject, records: recordsToSave };
                state.attendanceHistory.unshift(session);
            }

            localStorage.removeItem('studentMarkerWorkInProgress');
            state.students.forEach(s => { s.present = undefined; });
            saveState();
            recordState(false);
            renderApp();
        }
        
        function saveMoneyCollection() {
            const now = new Date();
            const recordsToSave = state.students.filter(s => s.payment).map(({ reg, name, payment }) => ({ reg, name, payment }));

            if (recordsToSave.length === 0) {
                 showModal({ title: "No Payments Recorded", content: "Please record at least one payment before saving.", isCenter: true });
                return;
            }
            
            const { type, id } = state.ui.history.editingSession;
            if (type === 'money' && id) {
                 const sessionIndex = state.moneyHistory.findIndex(s => s.id === id);
                if (sessionIndex !== -1) {
                    state.moneyHistory[sessionIndex] = { ...state.moneyHistory[sessionIndex], collector: state.ui.money.collector, purpose: state.ui.money.purpose, records: recordsToSave };
                }
                cancelEditing(false);
            } else {
                const session = { id: `mon-${Date.now()}`, type: 'money', date: now.toLocaleDateString(), time: now.toLocaleTimeString(), collector: state.ui.money.collector, purpose: state.ui.money.purpose, records: recordsToSave };
                state.moneyHistory.unshift(session);
            }
            
            localStorage.removeItem('studentMarkerWorkInProgress');
            state.students.forEach(s => { s.payment = null; });
            saveState();
            recordState(false);
            renderApp();
        }

        function deleteHistoryItem(item) {
            showConfirmation(`Are you sure you want to delete the session for "${item.subject || item.purpose}"? This cannot be undone.`, () => {
                if (item.type === 'attendance') {
                    state.attendanceHistory = state.attendanceHistory.filter(h => h.id !== item.id);
                } else {
                    state.moneyHistory = state.moneyHistory.filter(h => h.id !== item.id);
                }
                saveState();
                recordState(false);
                renderApp();
            });
        }

        function startEditingHistoryItem(item) {
            state.ui.history.editingSession = { type: item.type, id: item.id };
            // Clear current work
            state.students.forEach(s => { 
                s.present = undefined;
                s.payment = null;
            });
            // Load history data into current state
            if (item.type === 'attendance') {
                item.records.forEach(rec => {
                    const student = state.students.find(s => s.reg === rec.reg);
                    if (student) student.present = rec.present;
                });
                switchTab('attendance');
            } else { // money
                 item.records.forEach(rec => {
                    const student = state.students.find(s => s.reg === rec.reg);
                    if (student) student.payment = rec.payment;
                });
                state.ui.money.purpose = item.purpose;
                state.ui.money.collector = item.collector;
                state.ui.money.defaultAmount = '';
                switchTab('money');
            }
        }
        
        function cancelEditing(shouldRender = true) {
            state.ui.history.editingSession = { type: null, id: null };
            // Clear current work
            state.students.forEach(s => {
                s.present = undefined;
                s.payment = null;
            });
            // Reload any other work-in-progress if necessary, or just clear
            loadState(); // Simplest way to reset to a clean slate
            if (shouldRender) renderApp();
        }

        // --- MODAL MANAGEMENT ---
        function showModal({ id = 'generic-modal', title, content, isCenter = false }) {
            closeCurrentModal();
            const modalClass = isCenter ? 'modal-center' : '';
            modalContainer.innerHTML = `
                <div id="${id}" class="modal-overlay ${modalClass}">
                    <div class="modal-content glass-card">
                        ${isCenter ? '' : '<div class="modal-handle"></div>'}
                        <div class="flex justify-between items-center mb-4">
                            <h2 class="text-xl font-bold">${title}</h2>
                            <button class="close-modal-btn p-2 rounded-full hover:bg-black/10 dark:hover:bg-white/10 text-2xl leading-none">&times;</button>
                        </div>
                        <div class="custom-scrollbar" style="max-height: 60vh; overflow-y: auto;">${content}</div>
                    </div>
                </div>
            `;
            setTimeout(() => document.getElementById(id)?.classList.add('visible'), 10);
        }
        
        function closeCurrentModal() {
            const modal = modalContainer.querySelector('.modal-overlay');
            if (modal) {
                modal.classList.remove('visible');
                setTimeout(() => { modalContainer.innerHTML = ''; }, 300);
            }
        }
        
        // --- Show Specific Modals ---

        function showFilterModal() {
            const tab = state.currentTab;
            const uiState = state.ui[tab];
            const { filterType, filterRangePreset, customRange } = uiState;
            const isRangeTypeSelected = filterType === 'reg' || filterType === 'index';

            let range2_label = (filterType === 'reg') ? '51-99' : '51-95';
            
            let otherFiltersHTML = '';
            if (tab === 'money') {
                const { statusFilter, paymentMethodFilter } = uiState;
                otherFiltersHTML = `
                    <h3 class="font-semibold mt-4">Status</h3>
                    <div class="grid grid-cols-3 gap-2 mt-2">
                        <button class="soft-btn py-2 ${statusFilter === 'all' ? 'active' : ''}" data-status-filter="all">All</button>
                        <button class="soft-btn py-2 ${statusFilter === 'paid' ? 'active' : ''}" data-status-filter="paid">Paid</button>
                        <button class="soft-btn py-2 ${statusFilter === 'unpaid' ? 'active' : ''}" data-status-filter="unpaid">Unpaid</button>
                    </div>
                    <h3 class="font-semibold mt-4">Payment Mode</h3>
                    <div class="grid grid-cols-3 gap-2 mt-2">
                        <button class="soft-btn py-2 ${paymentMethodFilter === 'all' ? 'active' : ''}" data-method-filter="all">All</button>
                        <button class="soft-btn py-2 ${paymentMethodFilter === 'Cash' ? 'active' : ''}" data-method-filter="Cash">Cash</button>
                        <button class="soft-btn py-2 ${paymentMethodFilter === 'ePay' ? 'active' : ''}" data-method-filter="ePay">ePay</button>
                    </div>
                `;
            } else { // Attendance specific filter
                 const { statusFilter } = uiState;
                 otherFiltersHTML = `
                    <h3 class="font-semibold mt-4">Status</h3>
                    <div class="grid grid-cols-3 gap-2 mt-2">
                        <button class="soft-btn py-2 ${statusFilter === 'all' ? 'active' : ''}" data-status-filter="all">All</button>
                        <button class="soft-btn py-2 ${statusFilter === 'present' ? 'active' : ''}" data-status-filter="present">Present</button>
                        <button class="soft-btn py-2 ${statusFilter === 'absent' ? 'active' : ''}" data-status-filter="absent">Absent</button>
                    </div>
                 `;
            }

            const content = `
                <div class="space-y-4">
                    <h3 class="font-semibold">Filter By</h3>
                    <div class="grid grid-cols-3 gap-2">
                        <button class="soft-btn py-2 ${filterType === 'all' ? 'active' : ''}" data-filter-type="all">All</button>
                        <button class="soft-btn py-2 ${filterType === 'reg' ? 'active' : ''}" data-filter-type="reg">Reg No.</button>
                        <button class="soft-btn py-2 ${filterType === 'index' ? 'active' : ''}" data-filter-type="index">Index No.</button>
                    </div>
                    
                    <div class="${isRangeTypeSelected ? '' : 'hidden'}">
                        <h3 class="font-semibold mt-4">Range</h3>
                        <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 mt-2">
                            <button class="soft-btn py-2 ${filterRangePreset === 'all' ? 'active' : ''}" data-filter-range="all">All</button>
                            <button class="soft-btn py-2 ${filterRangePreset === '1-50' ? 'active' : ''}" data-filter-range="1-50">1-50</button>
                            <button class="soft-btn py-2 ${filterRangePreset === range2_label ? 'active' : ''}" data-filter-range="${range2_label}">${range2_label}</button>
                            <button class="soft-btn py-2 ${filterRangePreset === 'custom' ? 'active' : ''}" data-filter-range="custom">Custom</button>
                        </div>
                        <input id="${tab}-custom-range" type="text" placeholder="e.g., 1,5,10-15" class="mt-2 w-full bg-transparent p-3 border-2 border-gray-300 dark:border-gray-600 rounded-xl ${filterRangePreset === 'custom' ? '' : 'hidden'}" value="${customRange}">
                    </div>
                    
                    ${otherFiltersHTML}
                    
                    <button id="apply-filters-btn" class="w-full bg-indigo-600 text-white mt-4 py-3 rounded-xl font-semibold">Apply Filters</button>
                </div>
            `;
            showModal({ id: 'filter-modal', title: 'Filter Options', content });
        }
        
        function showActionsModal() {
            const content = `
                <div class="space-y-2">
                    <button class="actions-btn" id="action-import">Import App Data (JSON)</button>
                    <button class="actions-btn" id="action-export-json">Export App Data (JSON)</button>
                    <hr class="my-2 border-gray-200 dark:border-gray-700">
                    <button class="actions-btn" id="action-undo" ${historyPointer <= 0 ? 'disabled' : ''}>Undo</button>
                    <button class="actions-btn" id="action-redo" ${historyPointer >= stateHistory.length - 1 ? 'disabled' : ''}>Redo</button>
                    <hr class="my-2 border-gray-200 dark:border-gray-700">
                    <button class="actions-btn text-red-600 dark:text-red-400" id="action-reset">Reset All Data</button>
                </div>
            `;
            showModal({ id: 'actions-modal', title: 'Actions', content });
            document.getElementById('action-import').addEventListener('click', () => { closeCurrentModal(); document.getElementById('import-json-input-hidden').click(); });
            document.getElementById('action-export-json').addEventListener('click', () => { closeCurrentModal(); handleExport('json-app', state); });
            document.getElementById('action-undo').addEventListener('click', () => { closeCurrentModal(); undo(); });
            document.getElementById('action-redo').addEventListener('click', () => { closeCurrentModal(); redo(); });
            document.getElementById('action-reset').addEventListener('click', () => {
                 closeCurrentModal();
                 showConfirmation("This will delete all student lists and history. Are you sure?", () => {
                    localStorage.clear();
                    window.location.reload();
                 });
            });
        }
        
        function viewHistoryDetail(item) {
            let listHTML;
            if (item.type === 'attendance') {
                listHTML = item.records.map(r => `
                    <div class="flex justify-between items-center py-2 border-b border-gray-200 dark:border-gray-700">
                        <span>${r.name} (${r.reg})</span>
                        <span class="font-semibold ${r.present ? 'text-green-600' : 'text-red-600'}">${r.present ? 'Present' : 'Absent'}</span>
                    </div>`).join('');
            } else { // money
                listHTML = item.records.map(r => `
                    <div class="flex justify-between items-center py-2 border-b border-gray-200 dark:border-gray-700">
                        <span>${r.name} (${r.reg})</span>
                        <span class="font-semibold text-green-600">₹${r.payment.amount} (${r.payment.method})</span>
                    </div>`).join('');
            }
            showModal({ id: 'history-detail-modal', title: item.subject || item.purpose, content: listHTML });
        }
        
        function showExportOptionsModal(item) {
            const content = `
                <p class="mb-4">Select format to export report for "${item.subject || item.purpose}":</p>
                <div class="space-y-2">
                    <button class="actions-btn" data-export-type="pdf">Export as PDF</button>
                    <button class="actions-btn" data-export-type="png">Export as PNG Image</button>
                    <button class="actions-btn" data-export-type="excel">Export as Excel (.xlsx)</button>
                </div>
            `;
            showModal({ id: 'export-options-modal', title: 'Export Session', content });
            document.querySelector('#export-options-modal').addEventListener('click', (e) => {
                const exportBtn = e.target.closest('[data-export-type]');
                if (exportBtn) {
                    closeCurrentModal();
                    handleExport(exportBtn.dataset.exportType, item);
                }
            });
        }

        function showConfirmation(message, onConfirm) {
            const content = `
                <p class="text-center text-lg mb-6">${message}</p>
                <div class="flex justify-center gap-4">
                    <button id="modal-cancel" class="flex-1 soft-btn py-3">Cancel</button>
                    <button id="modal-confirm" class="flex-1 bg-red-600 text-white py-3 rounded-xl font-semibold">Confirm</button>
                </div>
            `;
            showModal({id: 'confirm-modal', title: 'Are you sure?', content, isCenter: true});
            document.getElementById('modal-confirm').onclick = () => { onConfirm(); closeCurrentModal(); };
            document.getElementById('modal-cancel').onclick = closeCurrentModal;
        }

        // --- IMPORT / EXPORT LOGIC ---
        function handleImportData(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const data = JSON.parse(e.target.result);
                    if (!data.students || !data.attendanceHistory || !data.moneyHistory) {
                        throw new Error("Invalid data file format.");
                    }
                    showConfirmation("This will overwrite all current data. Continue?", () => {
                        localStorage.setItem('studentMarkerData', JSON.stringify(data));
                        window.location.reload();
                    });
                } catch (error) {
                    showModal({ title: 'Import Error', content: `Failed to import data: ${error.message}`, isCenter: true });
                }
            };
            reader.readAsText(file);
            event.target.value = ''; // Reset input
        }

        async function handleExport(type, item) {
            const exportContentEl = document.getElementById('export-content');
            const filename = `${(item.subject || item.purpose || "app_data").replace(/\s/g, '_')}_${item.date || new Date().toISOString().split('T')[0]}`;
            
            if (type === 'json-app') {
                const dataStr = JSON.stringify(item, null, 2);
                const blob = new Blob([dataStr], { type: 'application/json' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = 'student_marker_backup.json';
                a.click();
                URL.revokeObjectURL(url);
                return;
            }
            
            exportContentEl.innerHTML = getExportHTML(item);

            if (type === 'pdf') {
                const canvas = await html2canvas(exportContentEl);
                const imgData = canvas.toDataURL('image/png');
                const pdf = new jsPDF({ orientation: 'p', unit: 'mm', format: 'a4' });
                const pdfWidth = pdf.internal.pageSize.getWidth();
                const pdfHeight = (canvas.height * pdfWidth) / canvas.width;
                pdf.addImage(imgData, 'PNG', 0, 0, pdfWidth, pdfHeight);
                pdf.save(`${filename}.pdf`);
            } else if (type === 'png') {
                const canvas = await html2canvas(exportContentEl);
                const a = document.createElement('a');
                a.href = canvas.toDataURL('image/png');
                a.download = `${filename}.png`;
                a.click();
            } else if (type === 'excel') {
                generateExcel(item, filename);
            }
            exportContentEl.innerHTML = ''; // Clear after use
        }
        
        function generateExcel(item, filename) {
            const ws_data = [];
            // Header
            ws_data.push([item.subject || item.purpose]);
            ws_data.push([`Date: ${item.date}`, `Time: ${item.time}`]);
            ws_data.push([item.type === 'attendance' ? `Taken By: ${item.takenBy}`: `Collector: ${item.collector}`]);
            ws_data.push([]); // Spacer

            // Table headers
            if(item.type === 'attendance') {
                ws_data.push(['Reg No', 'Name', 'Status']);
                item.records.forEach(r => {
                    ws_data.push([r.reg, r.name, r.present ? 'Present' : 'Absent']);
                });
            } else { // money
                ws_data.push(['Reg No', 'Name', 'Amount (₹)', 'Method']);
                item.records.forEach(r => {
                    ws_data.push([r.reg, r.name, r.payment.amount, r.payment.method]);
                });
                const total = item.records.reduce((sum, r) => sum + parseFloat(r.payment.amount || 0), 0);
                ws_data.push([]);
                ws_data.push(['', 'Total Collected:', total]);
            }
            
            const wb = XLSX.utils.book_new();
            const ws = XLSX.utils.aoa_to_sheet(ws_data);
            XLSX.utils.book_append_sheet(wb, ws, 'Report');
            XLSX.writeFile(wb, `${filename}.xlsx`);
        }
        
        function getExportHTML(item) {
             let summary, tableRows;
             if (item.type === 'attendance') {
                const presentCount = item.records.filter(r => r.present).length;
                const absentCount = item.records.length - presentCount;
                summary = `<div class="meta-info">Taken by: ${item.takenBy}</div>
                           <h2>Summary</h2>
                           <p class="summary-item">Total Students: ${item.records.length}</p>
                           <p class="summary-item">Present: ${presentCount}</p>
                           <p class="summary-item">Absent: ${absentCount}</p>`;
                tableRows = item.records.map(r => `<tr><td>${r.reg}</td><td>${r.name}</td><td>${r.present ? 'Present' : 'Absent'}</td></tr>`).join('');
            } else { // money
                const totalCollected = item.records.reduce((sum, r) => sum + parseFloat(r.payment.amount || 0), 0);
                summary = `<div class="meta-info">Collected by: ${item.collector}</div>
                           <h2>Summary</h2>
                           <p class="summary-item">Total Payments: ${item.records.length}</p>
                           <p class="summary-item">Total Collected: ₹${totalCollected.toFixed(2)}</p>`;
                tableRows = item.records.map(r => `<tr><td>${r.reg}</td><td>${r.name}</td><td>₹${r.payment.amount}</td><td>${r.payment.method}</td></tr>`).join('');
            }

            return `
                <h1>${item.subject || item.purpose}</h1>
                <div class="meta-info">${item.date} at ${item.time}</div>
                ${summary}
                <h2>Details</h2>
                <table>
                    <thead><tr><th>Reg No</th><th>Name</th><th>${item.type === 'attendance' ? 'Status' : 'Details'}</th></tr></thead>
                    <tbody>${tableRows}</tbody>
                </table>
                <div class="footer">Report generated by Student Marker App</div>
            `;
        }

        // Add hidden input for file import
        const hiddenInput = document.createElement('input');
        hiddenInput.type = 'file';
        hiddenInput.id = 'import-json-input-hidden';
        hiddenInput.className = 'hidden';
        hiddenInput.accept = '.json';
        document.body.appendChild(hiddenInput);
        hiddenInput.addEventListener('change', handleImportData);

        // --- FINAL BOOTSTRAP ---
        init();
    });
    </script>

</body>
</html>
