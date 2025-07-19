<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weekly Timetable</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
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
            font-family: 'Roboto', sans-serif;
            margin: 0;
            padding: 20px;
            background-image: linear-gradient(135deg, var(--bg-gradient-start) 0%, var(--bg-gradient-mid) 50%, var(--bg-gradient-end) 100%);
            color: var(--text-primary);
            line-height: 1.6;
            background-attachment: fixed;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        .container {
            max-width: 1300px;
            margin: 20px auto;
            background: var(--glass-bg-card);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border: 1px solid var(--glass-border);
            border-radius: 1rem;
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
            padding: 30px;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid var(--glass-border);
        }

        .header h1 {
            font-size: 2.25em;
            margin-bottom: 5px;
            font-weight: 700;
            background-image: linear-gradient(to right, #e879f9, #a855f7, #6366f1);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .header h2 {
            font-size: 1.25em;
            color: var(--text-secondary);
            margin-top: 0;
            font-weight: 500;
        }

        .timetable-wrapper {
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
        }
        .timetable-wrapper::-webkit-scrollbar { height: 6px; }
        .timetable-wrapper::-webkit-scrollbar-track { background: rgba(0,0,0,0.2); border-radius: 3px; }
        .timetable-wrapper::-webkit-scrollbar-thumb { background: var(--purple-accent); border-radius: 3px; }

        .timetable-table {
            width: 100%;
            min-width: 900px;
            border-collapse: collapse;
            margin-top: 20px;
        }

        .timetable-table th, .timetable-table td {
            padding: 12px 15px;
            text-align: center;
            border: 1px solid var(--glass-border);
            vertical-align: middle;
        }

        .timetable-table thead th {
            background-color: rgba(0, 0, 0, 0.2);
            color: var(--text-accent);
            font-weight: 600;
            white-space: nowrap;
            position: sticky;
            top: 0;
            z-index: 10;
        }

        .timetable-table tbody tr {
            transition: background-color 0.2s ease-in-out;
        }

        .timetable-table tbody tr:hover {
            background-color: rgba(255, 255, 255, 0.05);
        }

        .timetable-table td {
            font-size: 0.95em;
            color: var(--text-secondary);
        }

        .timetable-table td strong {
            color: var(--text-primary);
            font-weight: 500;
        }

        .timetable-table td span {
            display: block;
            font-size: 0.85em;
            color: var(--text-accent);
        }

        .break-cell {
            background-color: rgba(217, 70, 239, 0.2);
            color: #f0abfc;
            font-weight: 700;
            letter-spacing: 1px;
            text-transform: uppercase;
        }

        .self-study-cell {
            background-color: rgba(0, 0, 0, 0.15);
            color: var(--text-accent);
            font-style: italic;
        }

        .third-saturday-cell {
            background-color: rgba(0, 0, 0, 0.25);
            color: var(--text-secondary);
            font-weight: 500;
            text-transform: uppercase;
        }
        
        @media (max-width: 768px) {
            body { padding: 10px; }
            .container { padding: 15px; }
            .header h1 { font-size: 1.8em; }
            .header h2 { font-size: 1.1em; }
            .timetable-table th, .timetable-table td { padding: 10px; }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>WEEKLY TIMETABLE</h1>
            <h2>14.07.2025 TO 19.07.2025</h2>
        </div>

        <div class="timetable-wrapper">
            <table class="timetable-table">
                <thead>
                    <tr>
                        <th>DAYS</th>
                        <th>9:00AM to 10:00AM</th>
                        <th>10:00AM to 11:00AM</th>
                        <th>11:00AM to 12:00PM</th>
                        <th>12:00PM to 12:30PM</th>
                        <th>12:30PM to 1:00PM</th>
                        <th>1:00PM to 2:00PM</th>
                        <th>2:00PM to 3:00PM</th>
                        <th>3:00PM to 4:00PM</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td><strong>MONDAY</strong></td>
                        <td><strong>AHN II</strong><br><span>Mrs. Vimala Rani V</span></td>
                        <td><strong>Pharmacology II</strong></td>
                        <td><strong>AHN II</strong><br><span>Ms. Janani Shivaji S</span></td>
                        <td class="self-study-cell">SELF STUDY</td>
                        <td rowspan="3" class="break-cell">B<br>R<br>E<br>A<br>K</td>
                        <td><strong>AHN II</strong><br><span>Mr. Ebin Samuel J</span></td>
                        <td><strong>Pharmacology II</strong></td>
                        <td><strong>AHN II</strong><br><span>Mrs. Ruth Saranya R</span></td>
                    </tr>
                    <tr>
                        <td><strong>TUESDAY</strong></td>
                        <td><strong>AHN II</strong><br><span>Mr. Ebin Samuel J</span></td>
                        <td><strong>Pharmacology II</strong></td>
                        <td><strong>Pharmacology II</strong><br><span>Mrs. Ruth Saranya</span></td>
                        <td></td>
                        <td><strong>AHN II</strong><br><span>Mr. Pondian B</span></td>
                        <td><strong>Pharmacology II</strong></td>
                        <td><strong>AHN II</strong><br><span>Mr. Ebin Samuel J</span></td>
                    </tr>
                    <tr>
                        <td><strong>WEDNESDAY</strong></td>
                        <td><strong>AHN II</strong><br><span>Ms. Janani Shivaji S</span></td>
                        <td><strong>Pharmacology II</strong></td>
                        <td><strong>AHN II</strong><br><span>Mr. Ebin Samuel J</span></td>
                        <td></td>
                        <td><strong>AHN II</strong><br><span>Mrs. Ruth Saranya R</span></td>
                        <td><strong>Pharmacology II</strong></td>
                        <td><strong>AHN II</strong><br><span>Mrs. Mahalakshmi M</span></td>
                    </tr>
                    <tr>
                        <td><strong>THURSDAY</strong></td>
                        <td><strong>AHN II</strong><br><span>Mr. Ebin Samuel J</span></td>
                        <td><strong>Pharmacology II</strong></td>
                        <td><strong>Pharmacology II</strong><br><span>Mrs. Ruth Saranya</span></td>
                        <td></td>
                        <td class="break-cell">B<br>R<br>E<br>A<br>K</td>
                        <td><strong>PROFESSIONALISM</strong><br><span>Mr. Ebin Samuel J</span></td>
                        <td><strong>Pharmacology II</strong></td>
                        <td><strong>AHN II</strong><br><span>Mrs. Vimala Rani V</span></td>
                    </tr>
                    <tr>
                        <td><strong>FRIDAY</strong></td>
                        <td><strong>AHN II</strong><br><span>Mrs. Punitha K R</span></td>
                        <td><strong>Pharmacology II</strong></td>
                        <td><strong>AHN II</strong><br><span>Ms. Janani Shivaji S</span></td>
                        <td class="self-study-cell">SELF STUDY</td>
                        <td class="break-cell">B<br>R<br>E<br>A<br>K</td>
                        <td><strong>AHN II</strong><br><span>Mr. Ebin Samuel J</span></td>
                        <td><strong>Pharmacology II</strong></td>
                        <td><strong>AHN II</strong><br><span>Mrs. Mahalakshmi M</span></td>
                    </tr>
                    <tr>
                        <td><strong>SATURDAY</strong></td>
                        <td class="third-saturday-cell" colspan="8">THIRD SATURDAY - HOLIDAY</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</body>
</html>
