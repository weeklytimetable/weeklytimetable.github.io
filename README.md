<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weekly Timetable</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f0f2f5; /* Light grey background */
            color: #333;
            line-height: 1.6;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        .container {
            max-width: 1300px;
            margin: 20px auto;
            background-color: #ffffff;
            border-radius: 12px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
            padding: 30px;
            overflow-x: auto; /* For responsive table scrolling */
        }

        .header-minimal {
            text-align: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid #e0e0e0;
        }

        .header-minimal h1 {
            font-size: 2.5em;
            color: #004d40; /* Dark Teal */
            margin-bottom: 10px;
            letter-spacing: 1px;
            font-weight: 700;
        }

        .header-minimal h2 {
            font-size: 1.5em;
            color: #00796b; /* Medium Teal */
            margin-top: 0;
            font-weight: 500;
        }

        .timetable-wrapper {
            overflow-x: auto; /* Ensures table is scrollable on small screens */
            -webkit-overflow-scrolling: touch;
        }

        .timetable-table {
            width: 100%;
            min-width: 900px; /* Ensure table doesn't get too squished */
            border-collapse: collapse;
            margin-top: 20px;
            background-color: #fff;
            border-radius: 8px;
            overflow: hidden; /* For rounded corners on the table */
        }

        .timetable-table th, .timetable-table td {
            padding: 12px 15px;
            text-align: center;
            border: 1px solid #e0e0e0;
            vertical-align: middle;
        }

        .timetable-table thead th {
            background-color: #00796b; /* Medium Teal */
            color: #ffffff;
            font-weight: 600;
            white-space: nowrap; /* Keep time slots in one line */
            position: sticky;
            top: 0;
            z-index: 10;
        }

        .timetable-table tbody tr:nth-child(even) {
            background-color: #f9fbfb; /* Slightly different background for even rows */
        }

        .timetable-table tbody tr:hover {
            background-color: #e0f2f1; /* Light teal on hover */
            cursor: default;
        }

        .timetable-table td {
            font-size: 0.95em;
            color: #444;
        }

        .timetable-table td strong {
            color: #004d40; /* Dark Teal for strong text */
            font-weight: 500;
        }

        .timetable-table td span {
            display: block; /* Ensures names are on new lines */
            font-size: 0.9em;
            color: #555;
        }

        .break-cell {
            background-color: #ffc107; /* Amber for break */
            color: #333;
            font-weight: 700;
            letter-spacing: 1px;
            text-transform: uppercase;
        }

        .self-study-cell {
            background-color: #e0e0e0; /* Light grey for self study */
            color: #666;
            font-style: italic;
        }

        .third-saturday-cell {
            background-color: #cfd8dc; /* Blue grey for third saturday */
            color: #555;
            font-weight: 500;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .empty-cell {
            background-color: #fbfbfb;
        }

        /* Responsive adjustments */
        @media (max-width: 1024px) {
            .timetable-table {
                min-width: 800px; /* Adjust min-width for slightly smaller screens */
            }
        }

        @media (max-width: 768px) {
            body {
                padding: 10px;
            }
            .container {
                padding: 15px;
                border-radius: 8px;
            }
            .header-minimal h1 {
                font-size: 1.8em;
            }
            .header-minimal h2 {
                font-size: 1.2em;
            }
            .timetable-table th, .timetable-table td {
                padding: 10px;
            }
        }

        @media (max-width: 480px) {
            .header-minimal h1 {
                font-size: 1.5em;
            }
            .header-minimal h2 {
                font-size: 1.1em;
            }
            .timetable-table {
                min-width: 600px; /* Keep a minimum width for readability */
            }
            .timetable-table th, .timetable-table td {
                font-size: 0.85em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header-minimal">
            <h1>WEEKLY TIMETABLE</h1>
            <h2>14.07.2025 TO 19.07.2025</h2>
        </div>

        <div class="timetable-wrapper">
            <table class="timetable-table">
                <thead>
                    <tr>
                        <th>DAYS</th>
                        <th>9:00AM to 10:00 AM</th>
                        <th>10:00AM to 11:00 AM</th>
                        <th>11:00AM to 12:00 AM</th>
                        <th>12:00PM to 12:30 PM</th>
                        <th>12:30PM to 01:00 P.M</th>
                        <th>1:00 PM to 2:00 PM</th>
                        <th>2:00 PM to 3:00 PM</th>
                        <th>3:00 PM to 4:00 PM</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>MONDAY</td>
                        <td>AHN II <br><span>Mrs. Vimala Rani V</span></td>
                        <td>Pharmacology II</td>
                        <td>AHN II <br><span>Ms.Janani Shivaji S</span></td>
                        <td class="self-study-cell">SELF STUDY</td>
                        <td class="empty-cell"></td>
                        <td>AHN II <br><span>Mr. Ebin Samuel J</span></td>
                        <td>Pharmacology II</td>
                        <td>AHN II <br><span>Mrs. Ruth Saranya R</span></td>
                    </tr>
                    <tr>
                        <td>TUESDAY</td>
                        <td>AHN II <br><span>Mr. Ebin Samuel J</span></td>
                        <td>Pharmacology II</td>
                        <td>Pharmacology II <br><span>Mrs. Ruth Saranya</span></td>
                        <td class="empty-cell"></td>
                        <td class="empty-cell"></td>
                        <td>AHN II <br><span>Mr. Pondian B</span></td>
                        <td>Pharmacology II</td>
                        <td>AHN II <br><span>Mr. Ebin Samuel J</span></td>
                    </tr>
                    <tr>
                        <td>WEDNESDAY</td>
                        <td>AHN II <br><span>Ms.Janani Shivaji S</span></td>
                        <td>Pharmacology II</td>
                        <td>AHN II <br><span>Mr. Ebin Samuel J</span></td>
                        <td class="empty-cell"></td>
                        <td class="break-cell" rowspan="3">B R E A K</td>
                        <td>AHN II <br><span>Mrs. Ruth Saranya R</span></td>
                        <td>Pharmacology II</td>
                        <td>AHN II <br><span>Mrs. Mahalakshmi M</span></td>
                    </tr>
                    <tr>
                        <td>THURSDAY</td>
                        <td>AHN II <br><span>Mr. Ebin Samuel J</span></td>
                        <td>Pharmacology II</td>
                        <td>Pharmacology II <br><span>Mrs. Ruth Saranya</span></td>
                        <td class="empty-cell"></td>
                        <td>PROFESSIONALISM <br><span>Mr. Ebin Samuel J</span></td>
                        <td>Pharmacology II</td>
                        <td>AHN II <br><span>Mrs. Vimala Rani V</span></td>
                    </tr>
                    <tr>
                        <td>FRIDAY</td>
                        <td>AHN II <br><span>Mrs. Punitha K R</span></td>
                        <td>Pharmacology II</td>
                        <td>AHN II <br><span>Ms.Janani Shivaji S</span></td>
                        <td class="self-study-cell">SELF STUDY</td>
                        <td>AHN II <br><span>Mr. Ebin Samuel J</span></td>
                        <td>Pharmacology II</td>
                        <td>AHN II <br><span>Mrs. Mahalakshmi M</span></td>
                    </tr>
                    <tr>
                        <td>SATURDAY</td>
                        <td class="third-saturday-cell" colspan="4">THIRD SATURDAY</td>
                        <td class="empty-cell"></td>
                        <td colspan="3" class="third-saturday-cell">THIRD SATURDAY</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</body>
</html>
