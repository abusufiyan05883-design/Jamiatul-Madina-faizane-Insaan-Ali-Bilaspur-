<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>
      Monthly Test Sheet (Teacher View)
    </title>
    <script src="https://cdn.tailwindcss.com">
    </script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" />
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&amp;display=swap" rel="stylesheet" />
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js">
    </script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js">
    </script>
    <script src="jameel-font.js">
    </script>
    <style>
      @font-face { 
            font-family: &#39;Jameel Noori Nastaleeq&#39;; 
            src: url(&#39;https://cdn.jsdelivr.net/gh/sajjadsaeed/UrduFontsWeb@master/fonts/JameelNooriNastaleeq.woff2&#39;) format(&#39;woff2&#39;); 
            font-display: swap; 
        }
        body { font-family: &#39;Poppins&#39;, &#39;Jameel Noori Nastaleeq&#39;, sans-serif; background-color: #f0fdfa; }
        .urdu-font { font-family: &#39;Jameel Noori Nastaleeq&#39;, serif; line-height: 1.6; }
        .mark-input { text-align: center; }

        /* Grades Colors */
        .grade-mumtaz-sharf { background-color: #15803d; color: white; } 
        .grade-mumtaz { background-color: #22c55e; color: white; } 
        .grade-jayyid { background-color: #eab308; color: white; } 
        .grade-maqbul { background-color: #f97316; color: white; } 
        .grade-nakam { background-color: #ef4444; color: white; } 
        .grade-absent { background-color: #6b7280; color: white; }

        /* Input Validation Colors */
        .bg-fail-input { background-color: #fee2e2 !important; color: #991b1b; font-weight: bold; }
        
        .subject-checkbox:checked + div { background-color: #0d9488; color: white; border-color: #0d9488; }

        /* Summary Table Borders */
        #summary-view-area table th, 
        #summary-view-area table td {
            border: 1px solid #d1d5db;
        }
    </style>
  </head>
  <body class="p-4">
    <div class="max-w-7xl mx-auto bg-white shadow-2xl rounded-xl overflow-hidden border border-teal-100">
      <div class="bg-teal-800 text-white p-6 text-center relative">
        <h1 class="text-3xl font-bold urdu-font mb-2" id="display-jamia">
          Loading...
        </h1>
        <p class="text-teal-200 text-lg" id="display-month">
          ...
        </p>
        <span id="pdf-jamia" class="hidden"></span>
        <span id="pdf-month" class="hidden"></span>
        <p class="text-xs text-teal-300 mt-2">
          Teacher/Principal Portal
        </p>
      </div>
      <div id="loader" class="text-center py-10">
        <i class="fas fa-spinner fa-spin text-4xl text-teal-600"></i>
        Loading data...
      </div>
      <div id="main-content" class="p-6 hidden">
        <div class="flex justify-between items-center mb-6 print:hidden">
          <h2 class="text-2xl font-bold text-teal-800 urdu-font">
            Result Entry Panel
          </h2>
          <div class="flex gap-2">
            <button onclick="toggleSummary()" id="btn-view-summary" class="bg-purple-700 hover:bg-purple-800 text-white px-4 py-2 rounded-lg shadow flex items-center gap-2">
              <i class="fas fa-chart-bar"></i>
              View Summary
            </button>
            <button onclick="toggleSummary()" id="btn-back-entry" class="hidden bg-gray-600 hover:bg-gray-700 text-white px-4 py-2 rounded-lg shadow flex items-center gap-2">
              <i class="fas fa-arrow-left"></i>
              Back to Entry
            </button>
          </div>
        </div>
        <div id="summary-view-area" class="hidden">
          <div class="bg-white shadow-lg rounded-lg overflow-hidden border border-purple-200 mb-8">
            <div class="bg-purple-700 text-white text-center py-2 font-bold text-xl font-serif">
              Result Summary (All Subjects)
            </div>
            <div class="overflow-x-auto">
              <table class="w-full text-center border-collapse">
                <thead>
                  <tr class="bg-purple-100 text-purple-900 text-sm font-bold">
                    <th class="p-2">
                      S.No.
                    </th>
                    <th class="p-2 text-left">
                      Class
                    </th>
                    <th class="p-2">
                      Total Students
                    </th>
                    <th class="p-2">
                      Present
                    </th>
                    <th class="p-2">
                      Pass
                    </th>
                    <th class="p-2">
                      Fail
                    </th>
                    <th class="p-2">
                      Percent
                    </th>
                    <th class="p-2">
                      Grade
                    </th>
                  </tr>
                </thead>
                <tbody id="summary-body-main" class="text-sm">
                </tbody>
                <tfoot id="summary-foot-main" class="font-bold text-white">
                </tfoot>
              </table>
            </div>
          </div>
          <div class="bg-white shadow-lg rounded-lg overflow-hidden border border-purple-200 mb-8">
            <div class="bg-purple-700 text-white text-center py-2 font-bold text-xl font-serif">
              (Asri) Result Summary (English &amp; Maths)
            </div>
            <div class="overflow-x-auto">
              <table class="w-full text-center border-collapse">
                <thead>
                  <tr class="bg-purple-100 text-purple-900 text-sm font-bold">
                    <th class="p-2">
                      S.No.
                    </th>
                    <th class="p-2 text-left">
                      Class
                    </th>
                    <th class="p-2">
                      Total Students
                    </th>
                    <th class="p-2">
                      Present (In Asri)
                    </th>
                    <th class="p-2">
                      Pass
                    </th>
                    <th class="p-2">
                      Fail
                    </th>
                    <th class="p-2">
                      Percent
                    </th>
                    <th class="p-2">
                      Grade
                    </th>
                  </tr>
                </thead>
                <tbody id="summary-body-asri" class="text-sm">
                </tbody>
                <tfoot id="summary-foot-asri" class="font-bold text-white">
                </tfoot>
              </table>
            </div>
          </div>
        </div>
        <div id="data-entry-area">
          <div class="mb-8 bg-gray-50 p-6 rounded-lg border border-gray-200">
            <h3 id="step-heading" class="text-lg font-bold text-gray-700 mb-4 border-b pb-2">
              Step 1: Class &amp; Subjects Select Karein
            </h3>
            <div class="mb-4">
              <label class="block text-gray-600 font-semibold mb-1">
                Class:
              </label>
              <div id="class-tabs" class="flex flex-wrap gap-2 mt-4">
              </div>
              <select id="class-select" class="hidden">
                <option value="">
                  -- Select Class --
                </option>
              </select>
            </div>
            <p id="step-instruction" class="text-xs text-gray-500 mt-1">
              Ek class save karne ke baad dusri select karein.
            </p>
            <div id="subject-selection-area" class="hidden mt-4">
              <p class="text-sm text-gray-500 mb-2">
                Is Class ki kitabon me se koi
                <strong>                3 Kitabein
</strong>
                select karein:
              </p>
              <div class="flex gap-4 mb-4">
                <label class="flex items-center gap-2 cursor-pointer">
                  <input type="checkbox" id="include-english" checked="" />
                  <span class="font-semibold">                  English
</span>
                </label>
                <label class="flex items-center gap-2 cursor-pointer">
                  <input type="checkbox" id="include-maths" checked="" />
                  <span class="font-semibold">                  Maths
</span>
                </label>
              </div>
              <div id="checkbox-container" class="grid grid-cols-2 md:grid-cols-4 gap-3 mb-4">
              </div>
              <div class="bg-blue-50 p-3 rounded border border-blue-200 mb-4 text-sm text-blue-800 flex items-center">
                <i class="fas fa-info-circle mr-2"></i>
                Note:
                <strong>                English (25)
</strong>
                aur
                <strong>                Maths (25)
</strong>
                automatically shamil rahenge. Baqi 100 number ke honge.
              </div>
              <button onclick="generateSheet()" class="bg-teal-600 text-white px-6 py-3 rounded-lg font-bold hover:bg-teal-700 transition w-full md:w-auto">
                Result Sheet Banayein (Total 350)
              </button>
            </div>
          </div>
          <div id="result-area" class="hidden">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-4 gap-4">
              <h2 class="text-xl font-bold text-gray-800 border-b-2 border-teal-500 pb-1">
                Result Sheet
              </h2>
              <div class="flex gap-2">
                <button onclick="pasteFromExcel()" class="bg-teal-600 text-white px-4 py-2 rounded shadow hover:bg-teal-700 transition flex items-center gap-2">
                  <i class="fas fa-paste"></i>
                  Paste Excel
                </button>
                <button onclick="addStudentRow()" class="bg-blue-600 text-white px-4 py-2 rounded shadow hover:bg-blue-700 transition flex items-center gap-2">
                  <i class="fas fa-plus"></i>
                  Add Student
                </button>
              </div>
            </div>
            <div class="overflow-x-auto border rounded-lg shadow-sm" id="print-area">
              <table class="w-full text-sm text-left bg-white border-collapse">
                <thead class="bg-teal-50 text-teal-900 uppercase font-bold" id="table-head">
                </thead>
                <tbody id="students-body" class="divide-y divide-gray-200">
                </tbody>
              </table>
            </div>
            <div class="mt-8 flex flex-col md:flex-row gap-4 justify-end border-t pt-6">
              <button onclick="saveResult()" class="bg-emerald-600 hover:bg-emerald-700 text-white px-8 py-4 rounded-xl font-bold text-lg shadow-lg flex justify-center items-center gap-2">
                <i class="fas fa-save"></i>
                Save Result Sheet
              </button>
              <button onclick="downloadPDF()" class="bg-red-600 hover:bg-red-700 text-white px-6 py-4 rounded-xl font-bold text-lg shadow-lg flex justify-center items-center gap-2">
                <i class="fas fa-file-pdf"></i>
                Download PDF
              </button>
              <button id="download-all-btn" onclick="downloadAllClassesPDF()" class="bg-purple-700 hover:bg-purple-800 text-white px-6 py-4 rounded-xl font-bold text-lg shadow-lg flex justify-center items-center gap-2">
                <i class="fas fa-layer-group"></i>
                Download ALL Classes
              </button>
              <button onclick="downloadReportCards()" class="bg-blue-800 hover:bg-blue-900 text-white px-6 py-4 rounded-xl font-bold text-lg shadow-lg flex justify-center items-center gap-2">
                <i class="fas fa-id-card"></i>
                Report Cards
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div id="report-card-container" style="position: absolute; left: -9999px; top: 0; width: 800px;">
      <div id="report-card-template" style="width: 794px; height: 1123px; padding: 20px; background-color: #fff; box-sizing: border-box; font-family: &#39;Poppins&#39;, sans-serif;">
        <div style="border: 5px solid #b0c4de; height: 100%; padding: 10px; position: relative;">
          <div style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); opacity: 0.1; z-index: 0;">
            <img src="https://upload.wikimedia.org/wikipedia/commons/2/2f/Jamia_logo.png" style="width: 400px;" />
          </div>
          <div style="text-align: center; position: relative; z-index: 10;">
            <h1 style="color: #2c5282; font-weight: 900; font-size: 36px; margin: 0; text-transform: uppercase;">
              Monthly Test Result
            </h1>
            <h2 style="color: #2c5282; font-weight: 700; font-size: 28px; margin: 5px 0;" id="rc-month">
              MONTH 2025
            </h2>
            <h3 style="color: #2b6cb0; font-weight: 800; font-size: 24px; margin: 5px 0;">
              JAMIA TUL MADINA INDIA
            </h3>
          </div>
          <div style="position: absolute; top: 20px; right: 20px; width: 100px;">
            <img src="https://upload.wikimedia.org/wikipedia/commons/2/2f/Jamia_logo.png" style="width: 100%;" />
          </div>
          <br />
          <div style="display: flex; gap: 20px; margin-top: 20px; position: relative; z-index: 10;">
            <table style="width: 50%; border-collapse: collapse; border: 2px solid #000;">
              <tbody>
                <tr>
                  <td style="background: #b0c4de; font-weight: bold; padding: 8px; border: 1px solid #000; text-align: center;">
                    Class
                  </td>
                  <td style="padding: 8px; border: 1px solid #000; font-weight: bold;" id="rc-class">
                    ...
                  </td>
                </tr>
                <tr>
                  <td style="background: #b0c4de; font-weight: bold; padding: 8px; border: 1px solid #000; text-align: center; vertical-align: middle;">
                    Name :
                  </td>
                  <td style="padding: 8px; border: 1px solid #000; height: 60px; font-family: &#39;Jameel Noori Nastaleeq&#39;; font-size: 22px; font-weight: bold; text-align: center;" id="rc-name">
                    ...
                  </td>
                </tr>
              </tbody>
            </table>
            <table style="width: 50%; border-collapse: collapse; border: 2px solid #000;">
              <tbody>
                <tr>
                  <td style="background: #b0c4de; font-weight: bold; padding: 8px; border: 1px solid #000; text-align: center;">
                    Roll No :
                  </td>
                  <td style="padding: 8px; border: 1px solid #000; font-weight: bold;" id="rc-roll">
                    ...
                  </td>
                </tr>
                <tr>
                  <td style="background: #b0c4de; font-weight: bold; padding: 8px; border: 1px solid #000; text-align: center;">
                    BRANCH NAME:
                  </td>
                  <td style="padding: 8px; border: 1px solid #000; font-family: &#39;Jameel Noori Nastaleeq&#39;; font-size: 20px; text-align: right;" id="rc-branch">
                    ...
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div style="margin-top: 20px; position: relative; z-index: 10;">
            <table style="width: 100%; border-collapse: collapse; border: 2px solid #000; text-align: center;">
              <thead>
                <tr style="background: #b0c4de;">
                  <th style="border: 1px solid #000; padding: 10px; width: 40%;">
                    Subjects
                  </th>
                  <th style="border: 1px solid #000; padding: 10px; width: 20%;">
                    Total Marks
                  </th>
                  <th style="border: 1px solid #000; padding: 10px; width: 20%;">
                    Marks Obtained
                  </th>
                  <th style="border: 1px solid #000; padding: 10px; width: 20%; font-family: &#39;Jameel Noori Nastaleeq&#39;;">
                    مضمون
                  </th>
                </tr>
              </thead>
              <tbody id="rc-marks-body">
              </tbody>
            </table>
          </div>
          <div style="display: flex; gap: 20px; margin-top: 0; position: relative; z-index: 10;">
            <table style="width: 50%; border-collapse: collapse; border: 2px solid #000; border-top: none;">
              <tbody>
                <tr>
                  <td style="background: #b0c4de; font-weight: bold; padding: 8px; border: 1px solid #000;">
                    Total Marks :
                  </td>
                  <td style="padding: 8px; border: 1px solid #000; font-weight: bold;" id="rc-grand-total">
                    300
                  </td>
                </tr>
                <tr>
                  <td style="background: #b0c4de; font-weight: bold; padding: 8px; border: 1px solid #000;">
                    Passing Marks
                  </td>
                  <td style="padding: 8px; border: 1px solid #000;">
                    40/Subject
                  </td>
                </tr>
              </tbody>
            </table>
            <table style="width: 50%; border-collapse: collapse; border: 2px solid #000; border-top: none;">
              <tbody>
                <tr>
                  <td style="background: #b0c4de; font-weight: bold; padding: 8px; border: 1px solid #000;">
                    Obtained Marks
                  </td>
                  <td style="padding: 8px; border: 1px solid #000; font-weight: bold;" id="rc-obtained">
                    160
                  </td>
                </tr>
                <tr>
                  <td style="background: #b0c4de; font-weight: bold; padding: 8px; border: 1px solid #000;">
                    Percentage
                  </td>
                  <td style="padding: 8px; border: 1px solid #000; font-weight: bold;" id="rc-percentage">
                    53.33
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div style="text-align: center; margin: 10px 0; font-weight: bold; font-size: 18px;">
            You Are
            <span id="rc-status" style="margin-left: 10px;">            Pass
</span>
          </div>
          <div style="margin-top: 10px; border: 2px solid #000; display: flex;">
            <div style="flex: 1; border-right: 1px solid #000;">
              <div style="background: #b0c4de; border-bottom: 1px solid #000; padding: 5px; font-weight: bold; text-align: center;">
                Grade
              </div>
              <div style="padding: 10px; text-align: center; font-weight: bold; font-size: 20px;" id="rc-grade">
                B
              </div>
            </div>
            <div style="flex: 1; border-right: 1px solid #000;">
              <div style="background: #b0c4de; border-bottom: 1px solid #000; padding: 5px; font-weight: bold; text-align: center;">
                Rank
              </div>
              <div style="padding: 10px; text-align: center; font-weight: bold; font-size: 20px;" id="rc-rank">
                4
              </div>
            </div>
            <div style="flex: 2;">
              <div style="background: #b0c4de; border-bottom: 1px solid #000; padding: 5px; font-weight: bold; text-align: center;">
                Rank In Jamia
              </div>
              <div style="padding: 10px; text-align: center;">
              </div>
            </div>
          </div>
          <div style="margin-top: 0px; border: 2px solid #000; border-top: none; height: 60px; padding: 10px; text-align: center; color: #555;">
            Any Additional Information
          </div>
          <div style="margin-top: 80px; display: flex; justify-content: space-between; align-items: flex-end;">
            <div style="text-align: center; width: 200px;">
              <div style="font-family: &#39;Dancing Script&#39;, cursive; font-size: 24px; margin-bottom: 5px;">
                Principal Sign
              </div>
              <div style="border-top: 2px solid #000; padding-top: 5px;">
                Principal Signature
              </div>
            </div>
            <div style="text-align: center; width: 200px;">
              <div style="font-family: &#39;Dancing Script&#39;, cursive; font-size: 24px; margin-bottom: 5px;">
                Taleemi Zimm.
              </div>
              <div style="border-top: 2px solid #000; padding-top: 5px;">
                Signature Taleemi Zimmedar
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <script type="module">
      import { initializeApp } from &#34;https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js&#34;;
        import { getFirestore, doc, getDoc, setDoc, serverTimestamp } from &#34;https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js&#34;;

        const firebaseConfig = {
            apiKey: &#34;AIzaSyDhPuyX0y1gb3uXoIRcdcQPkd5Q4KJFgl0&#34;,
            authDomain: &#34;data-f15ab.firebaseapp.com&#34;,
            projectId: &#34;data-f15ab&#34;,
            storageBucket: &#34;data-f15ab.appspot.com&#34;,
            messagingSenderId: &#34;449137002393&#34;,
            appId: &#34;1:449137002393:web:6a23721d00b1b0daeb3508&#34;
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let adminUid = &#34;&#34;;
        let jamiaName = &#34;&#34;;
        let currentSheetData = [];
        let isEditingMode = false;
        let monthKey = &#34;&#34;;
        let globalUserData = {};
        let finalSubjectsList = [];
        let subjectTranslationMap = {};
        let totalMax = 350; 
        let classBooksMap = {}; 
        let selectedClassName = &#39;&#39;;

        window.onload = async () =&gt; {
            const params = new URLSearchParams(window.location.search);
            // Fallback support for &#39;id&#39; if &#39;uid&#39; is missing (as seen in older links)
            adminUid = params.get(&#39;uid&#39;) || params.get(&#39;id&#39;);
            jamiaName = params.get(&#39;jamia&#39;);
            monthKey = params.get(&#39;month&#39;);

            if (!adminUid || !jamiaName || !monthKey) {
                alert(&#34;Link Invalid hai. Link me ID, Jamia, ya Month missing hai.&#34;); return;
            }

            document.getElementById(&#39;display-jamia&#39;).textContent = jamiaName;
            document.getElementById(&#39;display-month&#39;).textContent = `Month: ${monthKey}`;
            
            // Set hidden fields for PDF
            if(document.getElementById(&#39;pdf-jamia&#39;)) document.getElementById(&#39;pdf-jamia&#39;).textContent = jamiaName;
            if(document.getElementById(&#39;pdf-month&#39;)) document.getElementById(&#39;pdf-month&#39;).textContent = monthKey;

            await loadTeachingData();
        };

        function getAcademicYear(key) {
            if (!key) return null;
            const [yStr, mStr] = key.split(&#34;-&#34;);
            const yearNum  = parseInt(yStr);
            const monthNum = parseInt(mStr) - 1; 
            return monthNum &gt;= 3 ? `${yearNum}-${yearNum+1}` : `${yearNum-1}-${yearNum}`;
        }

        async function loadTeachingData() {
            try {
                // --- 1. ADMIN KI GLOBAL CONFIGURATION SE SUBJECT MAPPING AUTO-LOAD KAREIN ---
                try {
                    const configDoc = await getDoc(doc(db, &#34;settings&#34;, &#34;academic_config&#34;));
                    if (configDoc.exists()) {
                        const configData = configDoc.exists() ? configDoc.data() : {};
                        const adminClasses = configData.classes || [];
                        
                        // Reset map pehle se loaded data ke liye
                        subjectTranslationMap = {};
                        
                        // Har class ke andar se subjects ki English aur Urdu names nikal kar map me save karein
                        adminClasses.forEach(c =&gt; {
                            if (Array.isArray(c.subjects)) {
                                c.subjects.forEach(s =&gt; {
                                    if (s.urdu &amp;&amp; s.eng) {
                                        // Key Urdu naam rahega aur Value uska English naam banega
                                        subjectTranslationMap[s.urdu.trim()] = s.eng.trim();
                                    }
                                });
                            }
                        });
                        console.log(&#34;Admin Subject Names Auto-Loaded:&#34;, subjectTranslationMap);
                    }
                } catch (err) {
                    console.warn(&#34;Admin academic_config load nahi ho saki (Using Fallbacks):&#34;, err);
                }

                // --- 2. PURANA TEACHING DATA STRUCTURE LOAD KAREIN ---
                const acYear = getAcademicYear(monthKey);
                const userDoc = await getDoc(doc(db, &#34;users&#34;, adminUid));
                if (!userDoc.exists()) { alert(&#34;User data nahi mila&#34;); return; }
                const userData = userDoc.data();
                globalUserData = userData;
                const years = userData.academicYears || {};
                const yearData = years[acYear];
                let jamiaStruct = null;
                
                if (yearData &amp;&amp; Array.isArray(yearData.karkardagiStructure)) {
                    jamiaStruct = yearData.karkardagiStructure.find(j =&gt; j.jamiaName === jamiaName);
                }
                if (!jamiaStruct &amp;&amp; Array.isArray(userData.academicStructure)) {
                    jamiaStruct = userData.academicStructure.find(j =&gt; j.jamiaName === jamiaName);
                }
                if (!jamiaStruct) { alert(&#34;Setup nahi mila. Admin se rabta karein.&#34;); return; }

                classBooksMap = {};
                if (Array.isArray(jamiaStruct.teachers)) {
                    jamiaStruct.teachers.forEach(t =&gt; {
                        if (Array.isArray(t.periods)) {
                            t.periods.forEach(p =&gt; {
                                const cName = p.className || p.class || p.darja;
                                const bName = p.bookName || p.book || p.kitab || p.subject;
                                if (cName &amp;&amp; bName) {
                                    if (!classBooksMap[cName]) classBooksMap[cName] = new Set();
                                    classBooksMap[cName].add(bName);
                                }
                            });
                        }
                    });
                }

                const tabs = document.getElementById(&#39;class-tabs&#39;);
                const select = document.getElementById(&#39;class-select&#39;);
                tabs.innerHTML = &#39;&#39;;
                select.innerHTML = &#39;&lt;option value=&#34;&#34;&gt;-- Select Class --&lt;/option&gt;&#39;;

                const sortedClasses = Object.keys(classBooksMap).sort();
                sortedClasses.forEach(cls =&gt; {
                    const opt = document.createElement(&#39;option&#39;);
                    opt.value = cls;
                    opt.textContent = cls;
                    select.appendChild(opt);

                    const btn = document.createElement(&#39;button&#39;);
                    btn.type = &#39;button&#39;;
                    btn.textContent = cls;
                    btn.dataset.class = cls;
                    let baseClass = &#34;px-4 py-2 rounded-lg border text-sm font-semibold transition&#34;;
                    btn.className = baseClass + &#34; bg-white hover:bg-teal-600 hover:text-white&#34;;
                    
                    btn.onclick = () =&gt; {
                        document.querySelectorAll(&#39;#class-tabs button&#39;).forEach(b =&gt; {
                            b.classList.remove(&#39;bg-teal-600&#39;, &#39;text-white&#39;);
                            b.classList.add(&#39;bg-white&#39;, &#39;text-gray-800&#39;);
                        });
                        btn.classList.remove(&#39;bg-white&#39;, &#39;text-gray-800&#39;);
                        btn.classList.add(&#39;bg-teal-600&#39;, &#39;text-white&#39;);
                        select.value = cls;
                        handleClassChange();
                    };
                    tabs.appendChild(btn);
                });

                document.getElementById(&#39;loader&#39;).classList.add(&#39;hidden&#39;);
                document.getElementById(&#39;main-content&#39;).classList.remove(&#39;hidden&#39;);
            } catch (error) { console.error(&#34;Error loading data:&#34;, error); }
        }

        // --- UPDATED UI FOR SUBJECT SELECTION ---
window.handleClassChange = async () =&gt; {
    const clsName = document.getElementById(&#39;class-select&#39;).value;
    const container = document.getElementById(&#39;checkbox-container&#39;);
    const selectionArea = document.getElementById(&#39;subject-selection-area&#39;);
    const resultArea = document.getElementById(&#39;result-area&#39;);

    container.innerHTML = &#39;&#39;;
    resultArea.classList.add(&#39;hidden&#39;); 
    document.getElementById(&#39;students-body&#39;).innerHTML = &#39;&#39;; 

    if (!clsName) { 
        selectionArea.classList.add(&#39;hidden&#39;); 
        return; 
    }

    const rawSubjectsSet = classBooksMap[clsName];
    if (!rawSubjectsSet) return;
    
    const filteredSubjects = Array.from(rawSubjectsSet).filter(s =&gt; {
        const lower = s.toLowerCase();
        return lower !== &#39;english&#39; &amp;&amp; lower !== &#39;maths&#39;;
    });

    // Checkbox container mein subjects load karna
    filteredSubjects.forEach((sub) =&gt; {
        const wrapper = document.createElement(&#39;div&#39;);
        wrapper.className = &#34;bg-white border rounded shadow-sm p-3 hover:shadow-md transition flex items-center justify-between&#34;;

        wrapper.innerHTML = `
            &lt;span class=&#34;font-bold text-teal-700 urdu-font text-xl&#34;&gt;${sub}&lt;/span&gt;
            &lt;input type=&#34;checkbox&#34; value=&#34;${sub}&#34; class=&#34;subject-checkbox w-5 h-5 text-teal-600 cursor-pointer&#34; onchange=&#34;toggleSubjectInput(this)&#34;&gt;
        `;
        container.appendChild(wrapper);
    });

    // Pura area visible karna
    selectionArea.classList.remove(&#39;hidden&#39;);
    
    // Firestore se purana data dhoondhne ke liye await lagana laazmi hai
    await window.checkExistingForClass(clsName);
};

// Helper function to toggle input visibility
window.toggleSubjectInput = (checkbox) =&gt; {
    if (checkbox.checked) {
        checkLimit(checkbox); // Sirf 3 subjects ki limit check karega
    }
};

        window.checkLimit = (elem) =&gt; {
            const checked = document.querySelectorAll(&#39;.subject-checkbox:checked&#39;);
            if (checked.length &gt; 3) {
                elem.checked = false;
                alert(&#34;Sirf 3 kitabein select kar sakte hain.&#34;);
            }
        };

        window.generateSheet = () =&gt; {
            const checkedBoxes = document.querySelectorAll(&#39;.subject-checkbox:checked&#39;);
            if (document.querySelectorAll(&#39;.subject-checkbox&#39;).length &gt; 0 &amp;&amp; checkedBoxes.length !== 3) {
                if (!confirm(`Apne sirf ${checkedBoxes.length} kitabein select ki hain. Kya aap aage badhna chahte hain?`)) return;
            }
            const selected3 = Array.from(checkedBoxes).map(cb =&gt; cb.value);
finalSubjectsList = [...selected3];

// 🔥 AUTOMATIC ENGLISH MAPPING (BINA INPUT BOX KE) 🔥
checkedBoxes.forEach(cb =&gt; {
    const sub = cb.value;
    // Agar database se pehle se koi English naam loaded hai to wahi rahega,
    // warna fallback me same Urdu naam hi chala jayega.
    if (!subjectTranslationMap[sub]) {
        subjectTranslationMap[sub] = sub; 
    }
});
            if (document.getElementById(&#39;include-english&#39;)?.checked) finalSubjectsList.push(&#39;English&#39;);
            if (document.getElementById(&#39;include-maths&#39;)?.checked) finalSubjectsList.push(&#39;Maths&#39;);

            totalMax = 0;
            finalSubjectsList.forEach(sub =&gt; {
                if (sub.toLowerCase() === &#39;english&#39; || sub.toLowerCase() === &#39;maths&#39;) totalMax += 25;
                else totalMax += 100;
            });
            renderTableHeaders();
            document.getElementById(&#39;result-area&#39;).classList.remove(&#39;hidden&#39;);
        };

        function renderTableHeaders() {
            const head = document.getElementById(&#39;table-head&#39;);
            let html = `&lt;tr&gt;&lt;th class=&#34;px-4 py-3 border border-gray-300 w-12&#34;&gt;#&lt;/th&gt;&lt;th class=&#34;px-4 py-3 border border-gray-300 text-left w-1/4&#34;&gt;Talib-e-Ilm&lt;/th&gt;`;
            finalSubjectsList.forEach(sub =&gt; {
                const isSmall = sub.toLowerCase() === &#39;english&#39; || sub.toLowerCase() === &#39;maths&#39;;
                const max = isSmall ? 25 : 100;
                html += `&lt;th class=&#34;px-4 py-3 border border-gray-300 text-center&#34;&gt;${sub} &lt;br&gt;&lt;span class=&#34;text-xs font-normal text-gray-200&#34;&gt;/${max}&lt;/span&gt;&lt;/th&gt;`;
            });
            html += `&lt;th class=&#34;px-4 py-3 border border-gray-300 text-center bg-gray-100&#34;&gt;Total (${totalMax})&lt;/th&gt;&lt;th class=&#34;px-4 py-3 border border-gray-300 text-center bg-gray-100&#34;&gt;%&lt;/th&gt;&lt;th class=&#34;px-4 py-3 border border-gray-300 text-center bg-gray-100&#34;&gt;Grade&lt;/th&gt;&lt;th class=&#34;px-2 py-3 border border-gray-300 text-center text-red-500 no-print&#34;&gt;&lt;i class=&#34;fas fa-trash&#34;&gt;&lt;/i&gt;&lt;/th&gt;&lt;/tr&gt;`;
            head.innerHTML = html;
        }

        window.addStudentRow = (name = &#34;&#34;, marks = {}) =&gt; {
            const tbody = document.getElementById(&#39;students-body&#39;);
            const index = tbody.rows.length + 1;
            const tr = document.createElement(&#39;tr&#39;);
            tr.className = &#34;hover:bg-gray-50&#34;;
            let inputs = &#34;&#34;;
            finalSubjectsList.forEach(sub =&gt; {
                const val = marks[sub] !== undefined ? marks[sub] : &#34;&#34;;
                const isSmall = sub.toLowerCase() === &#39;english&#39; || sub.toLowerCase() === &#39;maths&#39;;
                const max = isSmall ? 25 : 100;
                inputs += `&lt;td class=&#34;border border-gray-200 p-1&#34;&gt;&lt;input type=&#34;text&#34; data-sub=&#34;${sub}&#34; data-max=&#34;${max}&#34; value=&#34;${val}&#34; class=&#34;w-full p-2 text-center outline-none mark-input focus:bg-blue-50&#34; oninput=&#34;calcRow(this)&#34; placeholder=&#34;-&#34;&gt;&lt;/td&gt;`;
            });
            tr.innerHTML = `&lt;td class=&#34;border border-gray-200 p-2 text-center text-gray-500&#34;&gt;${index}&lt;/td&gt;&lt;td class=&#34;border border-gray-200 p-1&#34;&gt;&lt;input type=&#34;text&#34; value=&#34;${name}&#34; placeholder=&#34;Naam likhein&#34; class=&#34;w-full p-2 font-semibold urdu-font outline-none std-name&#34;&gt;&lt;/td&gt;${inputs}&lt;td class=&#34;border border-gray-200 p-2 text-center font-bold text-gray-700 total-val&#34;&gt;0&lt;/td&gt;&lt;td class=&#34;border border-gray-200 p-2 text-center font-bold text-blue-600 perc-val&#34;&gt;0%&lt;/td&gt;&lt;td class=&#34;border border-gray-200 p-2 text-center font-bold text-xs grade-val&#34;&gt;-&lt;/td&gt;&lt;td class=&#34;border border-gray-200 p-2 text-center cursor-pointer text-red-400 hover:text-red-600 no-print&#34; onclick=&#34;this.parentElement.remove()&#34;&gt;×&lt;/td&gt;`;
            tbody.appendChild(tr);
            if(name) calcRow(tr.querySelector(&#39;input&#39;));
        };

        // --- TABLE ROW CALCULATION (FIXED STYLING) ---
window.calcRow = (el) =&gt; {
    const row = el.closest(&#39;tr&#39;);
    const inputs = row.querySelectorAll(&#39;.mark-input&#39;);
    
    let obtained = 0;
    let totalMax = 0;
    
    let failCount = 0;       // Kitne subjects me fail hai
    let subjectCount = 0;    // Total subjects count
    let absentCount = 0;     // Kitne me absent hai

    inputs.forEach(inp =&gt; {
        subjectCount++;
        const subName = inp.dataset.sub ? inp.dataset.sub.toLowerCase() : &#34;&#34;;
        let val = inp.value.trim().toUpperCase();
        
        // 1. Max Marks Tay Karein
        let max = 100;
        if (subName.includes(&#39;english&#39;) || subName.includes(&#39;math&#39;) || subName.includes(&#39;hisab&#39;)) {
            max = 25;
        }
        totalMax += max;

        // 2. Marks Calculation &amp; Fail Check
        let numVal = 0;
        let isFail = false; // Is specific box ke liye check

        if (val === &#39;A&#39;) {
            // Absent Logic
            absentCount++;
            failCount++; 
            isFail = true;
        } 
        else if (val === &#39;&#39;) {
            // Empty Logic - Kuch mat karo (Reset niche hoga)
        }
        else {
            numVal = parseFloat(val);
            
            if (!isNaN(numVal)) {
                if (numVal &gt; max) { 
                    alert(`Maximum marks ${max} hain.`); 
                    inp.value = max; 
                    numVal = max; 
                }

                obtained += numVal;
                
                // 🔥 FAIL CHECK (Passing Marks = 40%)
                // 100 me se 40, 25 me se 10 pass hain
                const passingMarks = max * 0.40;
                
                // Agar marks passing se kam hain (0 bhi isme aayega)
                if (numVal &lt; passingMarks) {
                    failCount++; 
                    isFail = true;
                }
            }
        }

        // 🔥 APPLY STYLING USING CSS CLASS (Ye Important Hai)
        if (isFail) {
            // Agar Fail, 0 ya A hai to class add karo
            inp.classList.add(&#39;bg-fail-input&#39;); 
            // Inline style bhi laga dete hain safety ke liye
            inp.style.color = &#39;#991b1b&#39;; 
        } else {
            // Agar Pass hai ya Khali hai to class hata do
            inp.classList.remove(&#39;bg-fail-input&#39;);
            inp.style.color = &#39;&#39;; // Reset to default
        }
    });

    // 3. Percentage Nikalein
    let percentage = 0;
    if (totalMax &gt; 0) percentage = (obtained / totalMax) * 100;

    // 4. Grade Logic
    let grade = &#34;Nakam&#34;;
    let badgeClass = &#34;bg-red-600 text-white&#34;; // Default Fail Style

    // Rule A: Agar poori tarah Absent hai
    if (subjectCount &gt; 0 &amp;&amp; absentCount === subjectCount) {
        grade = &#34;Absent&#34;;
        badgeClass = &#34;bg-gray-500 text-white&#34;; 
    }
    // Rule B: 🔥 Agar 3 ya ziyada subjects me Fail/Absent hai to NAKAM
    else if (failCount &gt;= 3) {
        grade = &#34;Nakam&#34;;
        badgeClass = &#34;bg-red-600 text-white&#34;;
    }
    // Rule C: Percentage Base Grading
    else {
        if (percentage &gt;= 80) { 
            grade = &#34;Mumtaz Ma Sharf&#34;; 
            badgeClass = &#34;bg-teal-600 text-white&#34;; 
        }
        else if (percentage &gt;= 70) { 
            grade = &#34;Mumtaz&#34;; 
            badgeClass = &#34;bg-green-600 text-white&#34;; 
        }
        else if (percentage &gt;= 60) { 
            grade = &#34;Jayyid Jiddan&#34;; 
            badgeClass = &#34;bg-blue-600 text-white&#34;; 
        }
        else if (percentage &gt;= 50) { 
            grade = &#34;Jayyid&#34;; 
            badgeClass = &#34;bg-indigo-500 text-white&#34;; 
        }
        else if (percentage &gt;= 40) { 
            grade = &#34;Maqbul&#34;; 
            badgeClass = &#34;bg-yellow-500 text-white&#34;; 
        }
        else { 
            grade = &#34;Nakam&#34;; 
            badgeClass = &#34;bg-red-600 text-white&#34;; 
        }
    }

    // 5. Update UI
    row.querySelector(&#39;.total-val&#39;).innerText = obtained;
    row.querySelector(&#39;.perc-val&#39;).innerText = percentage.toFixed(2) + &#39;%&#39;;
    
    const gradeCell = row.querySelector(&#39;.grade-val&#39;);
    gradeCell.innerText = grade;
    
    // Badge Styling apply karein
    gradeCell.className = `grade-val px-2 py-1 text-center font-bold text-xs border rounded shadow-sm ${badgeClass}`;
};

        // --- FINAL SAVE RESULT (FIXED: allClasses Error &amp; Supports Empty Table) ---
window.saveResult = async (isAutoSave = false) =&gt; {
    const rows = document.querySelectorAll(&#39;#students-body tr&#39;);
    
    // AutoSave ke waqt button change na karein taaki user disturb na ho
    const btn = document.querySelector(&#39;button[onclick=&#34;saveResult()&#34;]&#39;);
    let originalText = &#34;Save Result Sheet&#34;;
    
    if(btn &amp;&amp; !isAutoSave) { 
        originalText = btn.innerText;
        btn.innerText = &#34;Saving...&#34;; 
        btn.disabled = true; 
    }

    const rawClass = document.getElementById(&#39;class-select&#39;).value;
    if(!rawClass &amp;&amp; !isAutoSave) { 
        alert(&#34;Class select nahi hai.&#34;); 
        if(btn) btn.disabled=false; 
        return; 
    }
    
    const cleanClass = rawClass.split(&#39;_&#39;)[0]; 
    const docId = `${adminUid}_${jamiaName.replace(/\s/g, &#39;_&#39;)}_${monthKey}_${cleanClass.replace(/\s/g, &#39;_&#39;)}`; 
    const selectedClass = rawClass; 
    const students = [];

    // Students ka data jama karein (Agar table me koi hai to)
    if (rows.length &gt; 0) {
        rows.forEach(row =&gt; {
            const name = row.querySelector(&#39;.std-name&#39;).value.trim();
            if(!name) return;
            const marks = {};
            row.querySelectorAll(&#39;.mark-input&#39;).forEach(inp =&gt; {
                const val = inp.value.trim().toUpperCase();
                marks[inp.dataset.sub] = (val === &#39;A&#39;) ? &#39;A&#39; : (val === &#39;&#39; ? 0 : parseFloat(val));
            });
            const total = parseFloat(row.querySelector(&#39;.total-val&#39;).textContent);
            const perc = parseFloat(row.querySelector(&#39;.perc-val&#39;).textContent);
            students.push({ name, marks, total, percentage: perc });
        });
    }

    try {
        // Inside saveResult function...
        await setDoc(doc(db, &#34;monthly_test_sheets&#34;, docId), {
            adminUid, jamiaName, month: monthKey, class: selectedClass,
            subjects: finalSubjectsList, 
            subjectMap: subjectTranslationMap, // 🔥 YE LINE ADD KARNI HAI
            students, 
            updatedAt: serverTimestamp()
        });
        
        // 2. Aggregation Logic (Corrected to match Summary)
        let grandTotalStudents = 0; 
        let grandPassedStudents = 0;
        
        const allClasses = Object.keys(classBooksMap);
        
        for (const cls of allClasses) {
            const clsClean = cls.split(&#39;_&#39;)[0];
            const sheetId = `${adminUid}_${jamiaName.replace(/\s/g, &#39;_&#39;)}_${monthKey}_${clsClean.replace(/\s/g, &#39;_&#39;)}`;
            
            let sStudents = [];
            let sSubjects = [];

            // Agar current class save ho rahi hai to taza data use karein
            if (cls === rawClass) {
                sStudents = students;
                sSubjects = finalSubjectsList;
            } else {
                // Dusri classes ka data DB se layein
                const sheetSnap = await getDoc(doc(db, &#34;monthly_test_sheets&#34;, sheetId));
                if (sheetSnap.exists()) {
                    const sData = sheetSnap.data();
                    sStudents = sData.students || [];
                    sSubjects = sData.subjects || [];
                }
            }
            
            sStudents.forEach(std =&gt; {
                // 1. Check Full Absent (All subjects &#39;A&#39;)
                const subKeys = Object.keys(std.marks);
                const isFullAbsent = subKeys.length &gt; 0 &amp;&amp; subKeys.every(k =&gt; std.marks[k] === &#39;A&#39;);

                if (!isFullAbsent) {
                    grandTotalStudents++; // Count as Present
                    
                    let isFail = false;
                    
                    // Rule A: Percentage Check (&lt; 40%)
                    if (parseFloat(std.percentage) &lt; 40) isFail = true;
                    
                    // Rule B: Partial Absent (Agar kisi bhi subject me &#39;A&#39; hai)
                    if (!isFail) {
                        // Check if any subject in the subject list is marked &#39;A&#39;
                        sSubjects.forEach(sub =&gt; {
                            if (std.marks[sub] === &#39;A&#39;) isFail = true;
                        });
                    }
                    
                    // Rule C: Fail Count (Fail in 3 or more subjects)
                    if (!isFail) {
                        let failCount = 0;
                        sSubjects.forEach(sub =&gt; {
                            const subLower = sub.toLowerCase();
                            // Max marks logic fix (Summary jaisa)
                            let max = (subLower.includes(&#39;english&#39;) || subLower.includes(&#39;math&#39;) || subLower.includes(&#39;hisab&#39;)) ? 25 : 100;
                            let passMarks = max * 0.40; // 40% passing
                            
                            let m = std.marks[sub];
                            // Agar marks number hain aur passing se kam hain
                            if (typeof m === &#39;number&#39; &amp;&amp; m &lt; passMarks) {
                                failCount++;
                            }
                        });
                        
                        if(failCount &gt;= 3) isFail = true;
                    }

                    if (!isFail) grandPassedStudents++;
                }
            });
        }

        let jamiaPassPerc = &#34;0%&#34;;
        if (grandTotalStudents &gt; 0) {
            jamiaPassPerc = ((grandPassedStudents / grandTotalStudents) * 100).toFixed(1) + &#39;%&#39;;
        }

        // 3. Admin Report Update
        const [yStr, mStr] = monthKey.split(&#39;-&#39;);
        const y = parseInt(yStr);
        const m = parseInt(mStr) - 1;
        const acYear = m &gt;= 3 ? `${y}-${y + 1}` : `${y - 1}-${y}`;
        const reportId = `${adminUid}_${acYear}_${y}_${m}`;
        const reportRef = doc(db, &#39;monthly_reports&#39;, reportId);
        
        // Report update safely
        try {
            const reportSnap = await getDoc(reportRef);
            let testData = reportSnap.exists() ? (reportSnap.data().monthlyTest?.data || []) : [];
            
            // Purani entry hata kar nayi update karein
            testData = testData.filter(t =&gt; t.name !== jamiaName);
            testData.push({ 
                name: jamiaName, 
                status: &#39;Test Hua&#39;, 
                month: monthKey, 
                percentage: jamiaPassPerc, 
                wajah: &#39;&#39; 
            });
            
            await setDoc(reportRef, { monthlyTest: { status: &#39;Done&#39;, data: testData } }, { merge: true });
        } catch (repErr) {
            console.warn(&#34;Report update failed (Minor error):&#34;, repErr);
        }

        if (!isAutoSave) {
            // Agar Admin ne columns update kiye hain aur students 0 hain
            if (students.length === 0) {
                alert(&#34;Subjects/Columns update ho gaye hain!&#34;);
            } else {
                alert(`Result Save ho gaya!\n\nJamia Total Pass: ${grandPassedStudents} / ${grandTotalStudents} (${jamiaPassPerc})`);
            }
        }

    } catch (error) { 
        console.error(&#34;Save Error:&#34;, error); 
        if(!isAutoSave) alert(&#34;Error: &#34; + error.message); 
    } 
    finally { 
        if(btn &amp;&amp; !isAutoSave) { btn.innerText = originalText; btn.disabled = false; }
    }
};

    // --- LOAD DATA AND CHECK EXISTING STATUS (FIXED) ---
        window.checkExistingForClass = async (clsName) =&gt; {
            const cleanClass = clsName.split(&#39;_&#39;)[0]; 
            const docId = `${adminUid}_${jamiaName.replace(/\s/g, &#39;_&#39;)}_${monthKey}_${cleanClass.replace(/\s/g, &#39;_&#39;)}`;
            const resultArea = document.getElementById(&#39;result-area&#39;);
            const studentsBody = document.getElementById(&#39;students-body&#39;);
            const classSelect = document.getElementById(&#39;class-select&#39;);
            const selectionArea = document.getElementById(&#39;subject-selection-area&#39;); 

            finalSubjectsList = [];
            studentsBody.innerHTML = &#39;&#39;;
            resultArea.classList.add(&#39;hidden&#39;); 
            document.getElementById(&#39;table-head&#39;).innerHTML = &#39;&#39;;
            
            const checkboxesList = document.querySelectorAll(&#39;.subject-checkbox&#39;);
            checkboxesList.forEach(chk =&gt; { chk.checked = false; });
            classSelect.disabled = false; 

            const params = new URLSearchParams(window.location.search);
            const isViewMode = params.get(&#39;mode&#39;) === &#39;view&#39;;

            try {
                const snap = await getDoc(doc(db, &#34;monthly_test_sheets&#34;, docId));
                if (snap.exists()) {
                    const data = snap.data(); 
                    
                    // Firestore se saved custom translations pehle load karein
                    if (data.subjectMap) {
                        subjectTranslationMap = { ...subjectTranslationMap, ...data.subjectMap };
                    }
                    
                    // 🔥 VARIABLE SUBJECTS KO YAHA DEFINE KIYA HAI (ERROR FIX)
                    const savedVariableSubjects = data.subjects ? data.subjects.filter(s =&gt; s !== &#39;English&#39; &amp;&amp; s !== &#39;Maths&#39;) : [];
                    
                    checkboxesList.forEach(chk =&gt; { 
                        if (savedVariableSubjects.includes(chk.value)) {
                            chk.checked = true;
                        }
                    });
                    
                    finalSubjectsList = data.subjects || [];
                    totalMax = 0;
                    finalSubjectsList.forEach(sub =&gt; {
                        if (sub.toLowerCase() === &#39;english&#39; || sub.toLowerCase() === &#39;maths&#39;) totalMax += 25; else totalMax += 100;
                    });
                    
                    renderTableHeaders(); 
                    resultArea.classList.remove(&#39;hidden&#39;); 
                    selectionArea.classList.add(&#39;hidden&#39;); 
                    
                    document.getElementById(&#39;students-body&#39;).innerHTML = &#39;&#39;; 
                    data.students.forEach(s =&gt; addStudentRow(s.name, s.marks)); 
                } 
                else {
                    if (isViewMode) {
                        resultArea.classList.remove(&#39;hidden&#39;);
                        document.getElementById(&#39;table-head&#39;).innerHTML = &#39;&lt;tr&gt;&lt;td colspan=&#34;10&#34; class=&#34;text-center p-4 text-gray-500 italic&#34;&gt;Is class ka data abhi tak save nahi hua hai. Upar &#34;Enable Editing&#34; dabakar subjects add karein.&lt;/td&gt;&lt;/tr&gt;&#39;;
                    }
                }

                if (isViewMode) {
                    const headerTitle = document.querySelector(&#39;h1&#39;);
                    if(headerTitle &amp;&amp; !document.getElementById(&#39;edit-mode-btn&#39;)) {
                        const badge = document.getElementById(&#39;view-badge&#39;);
                        if(!badge) headerTitle.innerHTML += ` &lt;span id=&#34;view-badge&#34; class=&#34;text-xl bg-yellow-200 text-yellow-800 px-2 py-1 rounded ml-2 align-middle&#34;&gt;View Only&lt;/span&gt;`;
                        headerTitle.innerHTML += `
                            &lt;button id=&#34;edit-mode-btn&#34; onclick=&#34;enableEditMode()&#34; class=&#34;ml-4 text-sm bg-blue-600 hover:bg-blue-700 text-white px-3 py-1 rounded shadow align-middle&#34;&gt;
                                &lt;i class=&#34;fas fa-edit&#34;&gt;&lt;/i&gt; Enable Editing
                            &lt;/button&gt;`;
                    }

                    [&#39;step-heading&#39;, &#39;step-instruction&#39;, &#39;class-select&#39;].forEach(id =&gt; { 
                        const el = document.getElementById(id); if(el) el.classList.add(&#39;hidden&#39;); 
                    });
                    
                    const addBtn = document.querySelector(&#39;button[onclick=&#34;addStudentRow()&#34;]&#39;);
                    const saveBtn = document.querySelector(&#39;button[onclick=&#34;saveResult()&#34;]&#39;);
                    if(addBtn) { addBtn.style.display = &#39;none&#39;; addBtn.classList.add(&#39;hidden-in-view&#39;); }
                    if(saveBtn) { saveBtn.style.display = &#39;none&#39;; saveBtn.classList.add(&#39;hidden-in-view&#39;); }

                    document.querySelectorAll(&#39;#students-body input&#39;).forEach(inp =&gt; {
                        inp.disabled = true;
                        inp.classList.add(&#39;view-mode-input&#39;);
                        inp.style.backgroundColor = &#39;transparent&#39;;
                        inp.style.border = &#39;none&#39;;
                        inp.style.textAlign = &#39;center&#39;;
                        inp.style.fontWeight = &#39;bold&#39;;
                        inp.style.color = &#39;#1f2937&#39;; 
                    });

                    document.querySelectorAll(&#39;.no-print&#39;).forEach(btn =&gt; btn.style.display = &#39;none&#39;);
                    const headerTrash = document.querySelector(&#39;#table-head th:last-child&#39;);
                    if(headerTrash) headerTrash.style.display = &#39;none&#39;;
                }
                
                if (typeof isEditingMode !== &#39;undefined&#39; &amp;&amp; isEditingMode) {
                    enableEditMode(true); 
                }
            } catch(e) { console.log(&#34;New sheet or error loading:&#34;, e); }
        };

        // --- SUMMARY FEATURE LOGIC ---
        window.toggleSummary = async () =&gt; {
            const entryArea = document.getElementById(&#39;data-entry-area&#39;);
            const summaryArea = document.getElementById(&#39;summary-view-area&#39;);
            const btnView = document.getElementById(&#39;btn-view-summary&#39;);
            const btnBack = document.getElementById(&#39;btn-back-entry&#39;);

            if (summaryArea.classList.contains(&#39;hidden&#39;)) {
                entryArea.classList.add(&#39;hidden&#39;);
                summaryArea.classList.remove(&#39;hidden&#39;);
                btnView.classList.add(&#39;hidden&#39;);
                btnBack.classList.remove(&#39;hidden&#39;);
                await loadSummaryData();
            } else {
                summaryArea.classList.add(&#39;hidden&#39;);
                entryArea.classList.remove(&#39;hidden&#39;);
                btnBack.classList.add(&#39;hidden&#39;);
                btnView.classList.remove(&#39;hidden&#39;);
            }
        };

        async function loadSummaryData() {
            const mainBody = document.getElementById(&#39;summary-body-main&#39;);
            const asriBody = document.getElementById(&#39;summary-body-asri&#39;);
            const mainFoot = document.getElementById(&#39;summary-foot-main&#39;);
            const asriFoot = document.getElementById(&#39;summary-foot-asri&#39;);

            mainBody.innerHTML = &#39;&lt;tr&gt;&lt;td colspan=&#34;8&#34; class=&#34;p-4 text-gray-500&#34;&gt;Loading data...&lt;/td&gt;&lt;/tr&gt;&#39;;
            asriBody.innerHTML = &#39;&lt;tr&gt;&lt;td colspan=&#34;8&#34; class=&#34;p-4 text-gray-500&#34;&gt;Loading data...&lt;/td&gt;&lt;/tr&gt;&#39;;

            try {
                const allClasses = Object.keys(classBooksMap).sort();
                let mainRowsHTML = &#34;&#34;;
                let asriRowsHTML = &#34;&#34;;
                let gMain = { total: 0, present: 0, pass: 0, fail: 0 };
                let gAsri = { total: 0, present: 0, pass: 0, fail: 0 };
                let sn = 1;

                for (const clsName of allClasses) {
                    const cleanClass = clsName.split(&#39;_&#39;)[0];
                    const docId = `${adminUid}_${jamiaName.replace(/\s/g, &#39;_&#39;)}_${monthKey}_${cleanClass.replace(/\s/g, &#39;_&#39;)}`;
                    const snap = await getDoc(doc(db, &#34;monthly_test_sheets&#34;, docId));
                    if (!snap.exists()) continue;
                    
                    const data = snap.data();
                    const students = data.students || [];
                    const sheetSubjects = data.subjects || []; // Saved subjects list
                    
                    if (students.length === 0) continue;

                    let mTotal = students.length; let mPresent = 0; let mPass = 0; let mFail = 0;
                    let aTotal = students.length; let aPresent = 0; let aPass = 0; let aFail = 0;
                    let hasAsriSubjects = false;

                    students.forEach(std =&gt; {
                        // --- Main Calculation (Fixed Logic) ---
                        const marksKeys = Object.keys(std.marks);
                        // Check if ALL subjects are &#39;A&#39;
                        const isFullAbsent = marksKeys.length &gt; 0 &amp;&amp; marksKeys.every(sub =&gt; std.marks[sub] === &#39;A&#39;);

                        if (!isFullAbsent) {
                            mPresent++;
                            let isFailMain = false;
                            
                            // Rule 1: Percentage Check
                            if (parseFloat(std.percentage) &lt; 40) isFailMain = true;
                            
                            // Rule 2: Partial Absent Check
                            sheetSubjects.forEach(sub =&gt; { if(std.marks[sub] === &#39;A&#39;) isFailMain = true; });
                            
                            // Rule 3: Fail in 3 Subjects Check (Ye missing tha)
                            if (!isFailMain) {
                                let subjectFailCount = 0;
                                sheetSubjects.forEach(sub =&gt; {
                                    const subLow = sub.toLowerCase();
                                    const max = (subLow.includes(&#39;english&#39;) || subLow.includes(&#39;math&#39;) || subLow.includes(&#39;hisab&#39;)) ? 25 : 100;
                                    const passMarks = max * 0.40;
                                    const m = std.marks[sub];
                                    
                                    if (typeof m === &#39;number&#39; &amp;&amp; m &lt; passMarks) {
                                        subjectFailCount++;
                                    }
                                });
                                if (subjectFailCount &gt;= 3) isFailMain = true;
                            }
                            
                            if (isFailMain) mFail++; else mPass++;
                        }

                        // --- Asri Calculation (Same as before) ---
                        let engMark = std.marks[&#39;English&#39;];
                        let mathMark = std.marks[&#39;Maths&#39;];
                        if (engMark !== undefined || mathMark !== undefined) {
                            hasAsriSubjects = true;
                            
                            // Asri Full Absent Check
                            let asriFullAbsent = false;
                            if (engMark !== undefined &amp;&amp; mathMark !== undefined) {
                                if (engMark === &#39;A&#39; &amp;&amp; mathMark === &#39;A&#39;) asriFullAbsent = true;
                            } else if (engMark !== undefined &amp;&amp; engMark === &#39;A&#39;) {
                                asriFullAbsent = true;
                            } else if (mathMark !== undefined &amp;&amp; mathMark === &#39;A&#39;) {
                                asriFullAbsent = true;
                            }

                            if (!asriFullAbsent) {
                                aPresent++;
                                let isFailAsri = false;
                                if (engMark === &#39;A&#39; || mathMark === &#39;A&#39;) isFailAsri = true;
                                if (engMark !== undefined &amp;&amp; typeof engMark === &#39;number&#39; &amp;&amp; engMark &lt; 10) isFailAsri = true;
                                if (mathMark !== undefined &amp;&amp; typeof mathMark === &#39;number&#39; &amp;&amp; mathMark &lt; 10) isFailAsri = true;
                                if (isFailAsri) aFail++; else aPass++;
                            }
                        }
                    });

                    let mPerc = mPresent &gt; 0 ? ((mPass / mPresent) * 100).toFixed(1) : &#34;0.0&#34;;
                    let mGrade = getSummaryGrade(mPerc);
                    gMain.total += mTotal; gMain.present += mPresent; gMain.pass += mPass; gMain.fail += mFail;

                    mainRowsHTML += `&lt;tr class=&#34;odd:bg-white even:bg-gray-50 hover:bg-purple-50&#34;&gt;&lt;td class=&#34;p-2 border&#34;&gt;${sn}&lt;/td&gt;&lt;td class=&#34;p-2 border text-left font-semibold&#34;&gt;${clsName}&lt;/td&gt;&lt;td class=&#34;p-2 border&#34;&gt;${mTotal}&lt;/td&gt;&lt;td class=&#34;p-2 border&#34;&gt;${mPresent}&lt;/td&gt;&lt;td class=&#34;p-2 border text-green-700 font-bold&#34;&gt;${mPass}&lt;/td&gt;&lt;td class=&#34;p-2 border text-red-600 font-bold&#34;&gt;${mFail}&lt;/td&gt;&lt;td class=&#34;p-2 border font-bold&#34;&gt;${mPerc}%&lt;/td&gt;&lt;td class=&#34;p-2 border&#34;&gt;&lt;span class=&#34;${getGradeColor(mGrade)} px-2 py-1 rounded text-xs text-white&#34;&gt;${mGrade}&lt;/span&gt;&lt;/td&gt;&lt;/tr&gt;`;

                    if (hasAsriSubjects) {
                        let aPerc = aPresent &gt; 0 ? ((aPass / aPresent) * 100).toFixed(1) : &#34;0.0&#34;;
                        let aGrade = getSummaryGrade(aPerc);
                        gAsri.total += aTotal; gAsri.present += aPresent; gAsri.pass += aPass; gAsri.fail += aFail;
                        asriRowsHTML += `&lt;tr class=&#34;odd:bg-white even:bg-gray-50 hover:bg-purple-50&#34;&gt;&lt;td class=&#34;p-2 border&#34;&gt;${sn}&lt;/td&gt;&lt;td class=&#34;p-2 border text-left font-semibold&#34;&gt;${clsName}&lt;/td&gt;&lt;td class=&#34;p-2 border&#34;&gt;${aTotal}&lt;/td&gt;&lt;td class=&#34;p-2 border&#34;&gt;${aPresent}&lt;/td&gt;&lt;td class=&#34;p-2 border text-green-700 font-bold&#34;&gt;${aPass}&lt;/td&gt;&lt;td class=&#34;p-2 border text-red-600 font-bold&#34;&gt;${aFail}&lt;/td&gt;&lt;td class=&#34;p-2 border font-bold&#34;&gt;${aPerc}%&lt;/td&gt;&lt;td class=&#34;p-2 border&#34;&gt;&lt;span class=&#34;${getGradeColor(aGrade)} px-2 py-1 rounded text-xs text-white&#34;&gt;${aGrade}&lt;/span&gt;&lt;/td&gt;&lt;/tr&gt;`;
                    } else {
                        asriRowsHTML += `&lt;tr&gt;&lt;td class=&#34;p-2 border&#34;&gt;${sn}&lt;/td&gt;&lt;td class=&#34;p-2 border text-left font-semibold&#34;&gt;${clsName}&lt;/td&gt;&lt;td colspan=&#34;6&#34; class=&#34;p-2 border text-gray-400&#34;&gt;Asri subjects nahi hain&lt;/td&gt;&lt;/tr&gt;`;
                    }
                    sn++;
                }

                mainBody.innerHTML = mainRowsHTML || &#39;&lt;tr&gt;&lt;td colspan=&#34;8&#34; class=&#34;p-4&#34;&gt;No data found&lt;/td&gt;&lt;/tr&gt;&#39;;
                asriBody.innerHTML = asriRowsHTML || &#39;&lt;tr&gt;&lt;td colspan=&#34;8&#34; class=&#34;p-4&#34;&gt;No data found&lt;/td&gt;&lt;/tr&gt;&#39;;

                let gMainPerc = gMain.present &gt; 0 ? ((gMain.pass / gMain.present) * 100).toFixed(2) : &#34;0.00&#34;;
                mainFoot.innerHTML = `&lt;tr class=&#34;bg-green-600&#34;&gt;&lt;td colspan=&#34;2&#34; class=&#34;p-2 border text-right text-lg&#34;&gt;Total&lt;/td&gt;&lt;td class=&#34;p-2 border bg-purple-300 text-black&#34;&gt;${gMain.total}&lt;/td&gt;&lt;td class=&#34;p-2 border bg-purple-300 text-black&#34;&gt;${gMain.present}&lt;/td&gt;&lt;td class=&#34;p-2 border bg-purple-300 text-black&#34;&gt;${gMain.pass}&lt;/td&gt;&lt;td class=&#34;p-2 border bg-purple-300 text-black&#34;&gt;${gMain.fail}&lt;/td&gt;&lt;td class=&#34;p-2 border bg-purple-300 text-black&#34;&gt;${gMainPerc}%&lt;/td&gt;&lt;td class=&#34;p-2 border bg-red-600&#34;&gt;${getSummaryGrade(gMainPerc)}&
