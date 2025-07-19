<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Examinations Timetable – August 2025</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
        --bg-gradient-start: #1a0134;
        --bg-gradient-mid: #3b0266;
        --bg-gradient-end: #57008a;
        --text-primary: #f1f5f9;
        --text-secondary: #cbd5e1;
        --text-accent: #b4a3c1;
        --glass-bg-card: rgba(59, 2, 102, 0.6);
        --glass-border: rgba(255, 255, 255, 0.1);
        --blue-accent: #a855f7;
        --purple-accent: #d946ef;
    }

    body {
        font-family: 'Inter', sans-serif;
        background-image: linear-gradient(135deg, var(--bg-gradient-start) 0%, var(--bg-gradient-mid) 50%, var(--bg-gradient-end) 100%);
        color: var(--text-primary);
        background-attachment: fixed;
    }
    
    .gradient-text {
        background-image: linear-gradient(to right, #e879f9, #a855f7, #6366f1);
        -webkit-background-clip: text;
        background-clip: text;
        color: transparent;
    }

    .main-card {
        background: var(--glass-bg-card);
        backdrop-filter: blur(10px);
        -webkit-backdrop-filter: blur(10px);
        border: 1px solid var(--glass-border);
    }
    
    .filter-btn {
        padding: 0.5rem 1.5rem;
        border-radius: 9999px;
        font-weight: 600;
        color: var(--text-secondary);
        background-color: transparent;
        border: 1px solid var(--glass-border);
        transition: all 0.2s ease-in-out;
        cursor: pointer;
    }
    .filter-btn.active, .filter-btn:hover {
        color: white;
        background-image: linear-gradient(to right, var(--blue-accent), var(--purple-accent));
        box-shadow: 0 4px 15px rgba(168, 85, 247, 0.3);
        border-color: transparent;
    }

    .date-link-btn {
        padding: 0.25rem 0.75rem;
        border-radius: 9999px;
        font-size: 0.875rem;
        font-weight: 600;
        color: var(--text-secondary);
        background-color: rgba(0,0,0,0.2);
        border: 1px solid var(--glass-border);
        transition: all 0.2s ease-in-out;
        cursor: pointer;
    }
    .date-link-btn:hover {
        color: var(--text-primary);
        transform: translateY(-2px);
    }
    .date-link-btn.theory-date:hover {
        border-color: var(--blue-accent);
    }
    .date-link-btn.practical-date:hover {
        border-color: var(--purple-accent);
    }

    .exam-section-card {
        background-color: rgba(0,0,0,0.1);
        border: 1px solid var(--glass-border);
    }

    .exam-item-card {
        background-color: rgba(0,0,0,0.2);
        border-left-width: 4px;
        transition: all 0.2s ease-in-out;
    }
    .exam-item-card:hover {
        background-color: rgba(0,0,0,0.3);
        transform: scale(1.02);
    }
    .exam-item-card.theory-item { border-color: var(--blue-accent); }
    .exam-item-card.practical-item { border-color: var(--purple-accent); }

  </style>
</head>

<body class="p-4 sm:p-6 md:p-8">
  <div class="min-h-screen flex flex-col items-center">
    <div class="main-card rounded-2xl shadow-lg w-full max-w-4xl p-4 sm:p-6 md:p-8">

      <div class="text-center mb-6">
        <h1 class="text-2xl sm:text-3xl font-semibold gradient-text">University Exam Time Table</h1>
      </div>

      <div id="filter-container" class="flex flex-wrap justify-center gap-2 mb-4 p-1 rounded-full" style="background-color: rgba(0,0,0,0.1);">
        <button id="btn-theory" onclick="filterContent('theory')" class="filter-btn active">Theory</button>
        <button id="btn-practical" onclick="filterContent('practical')" class="filter-btn">Practical</button>
      </div>

      <div class="flex flex-wrap justify-center gap-2 mb-6">
        <button onclick="scrollToExam('date-18', 'theory')" class="date-link-btn theory-date">18</button>
        <button onclick="scrollToExam('date-20', 'theory')" class="date-link-btn theory-date">20</button>
        <button onclick="scrollToExam('date-22', 'theory')" class="date-link-btn theory-date">22</button>
        <button onclick="scrollToExam('date-23', 'theory')" class="date-link-btn theory-date">23</button>
        <button onclick="scrollToExam('date-25', 'practical')" class="date-link-btn practical-date">25</button>
        <button onclick="scrollToExam('date-26', 'practical')" class="date-link-btn practical-date">26</button>
        <button onclick="scrollToExam('date-28', 'practical')" class="date-link-btn practical-date">28</button>
        <button onclick="scrollToExam('date-29', 'practical')" class="date-link-btn practical-date">29</button>
      </div>

      <div class="space-y-6">
        <div class="theory exam-section-card p-4 rounded-xl shadow">
          <h2 class="font-semibold text-lg mb-2" style="color: var(--blue-accent);">Theory Examinations</h2>
          <ul class="space-y-4 text-sm sm:text-base">
            <li id="date-18" class="exam-item-card theory-item p-4 rounded-lg">
              <strong>18.08.2025 (Monday)</strong><br>
              PHAR 205T – Pharmacology (I & II)<br>
              PATH 210T – Pathology (I & II) (Including Genetics)<br>
              <strong class="text-text-accent">Time:</strong> 10:00 A.M. – 1:00 P.M.
            </li>
            <li id="date-20" class="exam-item-card theory-item p-4 rounded-lg">
              <strong>20.08.2025 (Wednesday)</strong><br>
              N-AHN 225T – Adult Health Nursing II with Integrated Pathophysiology, Geriatric Nursing & Palliative Care<br>
              <strong class="text-text-accent">Time:</strong> 10:00 A.M. – 1:00 P.M.
            </li>
            <li id="date-22" class="exam-item-card theory-item p-4 rounded-lg">
              <strong>22.08.2025 (Friday)</strong><br>
              PROF 230 – Professionalism, Professional Values & Ethics (Including Bioethics)<br>
              <strong class="text-text-accent">Time:</strong> 10:00 A.M. – 1:00 P.M.
            </li>
            <li id="date-23" class="exam-item-card theory-item p-4 rounded-lg">
              <strong>23.08.2025 (Saturday)</strong><br>
              ELEC 1 – Human Values (Elective)<br>
              <strong class="text-text-accent">Time:</strong> 10:00 A.M. – 1:00 P.M.
            </li>
          </ul>
        </div>

        <div class="practical exam-section-card p-4 rounded-xl shadow" style="display: none;">
          <h2 class="font-semibold text-lg mb-2" style="color: var(--purple-accent);">Practical Examinations</h2>
          <ul class="space-y-4 text-sm sm:text-base">
            <li id="date-25" class="exam-item-card practical-item p-4 rounded-lg">
              <strong>25.08.2025 (Monday)</strong><br>
              N-AHN 225P – Adult Health Nursing II Practical
            </li>
            <li id="date-26" class="exam-item-card practical-item p-4 rounded-lg">
              <strong>26.08.2025 (Tuesday)</strong><br>
              N-AHN 225P – Adult Health Nursing II Practical
            </li>
            <li id="date-28" class="exam-item-card practical-item p-4 rounded-lg">
              <strong>28.08.2025 (Thursday)</strong><br>
              N-AHN 225P – Adult Health Nursing II Practical
            </li>
            <li id="date-29" class="exam-item-card practical-item p-4 rounded-lg">
              <strong>29.08.2025 (Friday)</strong><br>
              N-AHN 225P – Adult Health Nursing II Practical
            </li>
          </ul>
        </div>
      </div>

    </div>
  </div>

  <script>
    function filterContent(type) {
      const theorySection = document.querySelector('.theory');
      const practicalSection = document.querySelector('.practical');
      const theoryBtn = document.getElementById('btn-theory');
      const practicalBtn = document.getElementById('btn-practical');

      if (type === 'theory') {
        theorySection.style.display = 'block';
        practicalSection.style.display = 'none';
        theoryBtn.classList.add('active');
        practicalBtn.classList.remove('active');
      } else if (type === 'practical') {
        theorySection.style.display = 'none';
        practicalSection.style.display = 'block';
        theoryBtn.classList.remove('active');
        practicalBtn.classList.add('active');
      }
    }

    function scrollToExam(id, type) {
      filterContent(type);
      // Use a short timeout to ensure the section is visible before scrolling
      setTimeout(() => {
        const element = document.getElementById(id);
        if (element) {
          element.scrollIntoView({
            behavior: 'smooth',
            block: 'center'
          });
          // Add a temporary highlight effect
          element.style.transition = 'background-color 0.2s ease-in-out';
          element.style.backgroundColor = 'rgba(255, 255, 255, 0.1)';
          setTimeout(() => {
            element.style.backgroundColor = '';
          }, 1000);
        }
      }, 100);
    }
    
    // Set initial state on load
    document.addEventListener('DOMContentLoaded', () => {
        filterContent('theory');
    });
  </script>
</body>

</html>
