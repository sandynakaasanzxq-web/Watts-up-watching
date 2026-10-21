# Watts-up-watching
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบเช็คอิน / เช็คเอาท์ เข้าใช้งานห้อง Innovation</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons CDN -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Font Kanit -->
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        body {
            font-family: 'Kanit', sans-serif;
            background: linear-gradient(135deg, #f0f4f8 0%, #e2e8f0 100%);
        }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-center p-4">

    <div class="w-full max-w-2xl bg-white rounded-3xl shadow-xl overflow-hidden border border-slate-100">
        
        <!-- Header & Real-time Digital Clock Section -->
        <div class="bg-gradient-to-r from-blue-600 to-indigo-700 p-6 sm:p-8 text-white text-center relative">
            <div class="inline-block p-3 bg-white/10 rounded-2xl backdrop-blur-md mb-3">
                <i class="fa-solid font-bold fa-clock text-3xl text-amber-300"></i>
            </div>
            <h1 class="text-2xl sm:text-3xl font-bold tracking-wide">ระบบบันทึกการเข้า-ออกห้อง</h1>
            <p class="text-blue-100 text-sm mt-1 font-medium">ห้อง Innovation</p>
            
            <div class="mt-6 bg-black/20 rounded-2xl p-4 backdrop-blur-sm border border-white/10">
                <div id="date-display" class="text-blue-200 text-sm font-medium mb-1">กำลังโหลดวันที่...</div>
                <div id="clock-display" class="text-4xl sm:text-5xl font-extrabold tracking-widest text-white font-mono">00:00:00</div>
            </div>
        </div>

        <!-- Form Input & Action Buttons Section -->
        <div class="p-6 sm:p-8 space-y-6">
            
            <div class="grid grid-cols-1 gap-4">
                <div>
                    <label class="block text-sm font-semibold text-slate-700 mb-2">
                        <i class="fa-solid fa-user text-indigo-500 mr-1"></i> ชื่อ-นามสกุล / ผู้ใช้งาน <span class="text-slate-400 font-normal">(ไม่ระบุก็ได้)</span>
                    </label>
                    <input type="text" id="userName" placeholder="ระบุชื่อผู้เข้าใช้ (ถ้ามี)" 
                        class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 transition-all text-slate-800">
                </div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 pt-2">
                <button onclick="handleAction('CHECK_IN')" id="btnCheckIn"
                    class="flex items-center justify-center gap-2 w-full py-4 bg-emerald-500 hover:bg-emerald-600 active:scale-95 text-white font-bold rounded-2xl shadow-lg shadow-emerald-500/30 transition-all cursor-pointer">
                    <i class="fa-solid fa-right-to-bracket text-xl"></i>
                    <span>ลงชื่อเข้า (Check In)</span>
                </button>

                <button onclick="handleAction('CHECK_OUT')" id="btnCheckOut"
                    class="flex items-center justify-center gap-2 w-full py-4 bg-rose-500 hover:bg-rose-600 active:scale-95 text-white font-bold rounded-2xl shadow-lg shadow-rose-500/30 transition-all cursor-pointer">
                    <i class="fa-solid fa-right-from-bracket text-xl"></i>
                    <span>ลงชื่อออก (Check Out)</span>
                </button>
            </div>

            <div class="border-t border-slate-100 pt-4">
                <details class="group">
                    <summary class="flex justify-between items-center font-medium cursor-pointer list-none text-xs text-slate-500 hover:text-indigo-600 transition-colors">
                        <span><i class="fa-solid fa-gear mr-1"></i> ตั้งค่าการเชื่อมต่อ Google Sheet</span>
                        <span class="transition group-open:rotate-180">
                            <i class="fa-solid fa-chevron-down"></i>
                        </span>
                    </summary>
                    <div class="mt-3 bg-slate-50 p-4 rounded-xl border border-slate-200 text-xs space-y-2">
                        <label class="block font-semibold text-slate-700">Google Apps Script Web App URL:</label>
                        <input type="text" id="scriptUrl" placeholder="https://script.google.com/macros/s/.../exec"
                            class="w-full px-3 py-2 rounded-lg border border-slate-300 font-mono text-xs focus:ring-1 focus:ring-indigo-500">
                        <p class="text-slate-500">วาง URL ที่ได้จากการเลย์เอาท์ (Deploy) Google Apps Script เพื่อบันทึกข้อมูลเข้า Google Sheet ของคุณ</p>
                    </div>
                </details>
            </div>

            <div class="mt-6">
                <h3 class="text-sm font-semibold text-slate-700 mb-3 flex items-center justify-between">
                    <span><i class="fa-solid fa-history text-indigo-500 mr-1"></i> ประวัติกิจกรรมล่าสุด (ห้อง Innovation)</span>
                </h3>
                <div class="overflow-x-auto bg-slate-50 rounded-xl border border-slate-200 max-h-48 overflow-y-auto">
                    <table class="w-full text-left text-xs text-slate-600">
                        <thead class="bg-slate-100 text-slate-700 uppercase font-semibold sticky top-0">
                            <tr>
                                <th class="p-3">ประเภท</th>
                                <th class="p-3">ชื่อ</th>
                                <th class="p-3">ห้อง</th>
                                <th class="p-3">เวลา</th>
                            </tr>
                        </thead>
                        <tbody id="historyTable" class="divide-y divide-slate-200">
                            <tr>
                                <td colspan="4" class="p-4 text-center text-slate-400">ยังไม่มีข้อมูลในเซสชั่นนี้</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

        </div>
    </div>

    <div class="mt-6 text-center">
        <button onclick="toggleModal(true)" class="text-xs text-indigo-600 hover:text-indigo-800 underline font-medium cursor-pointer">
            <i class="fa-solid fa-circle-info mr-1"></i> วิธีตั้งค่า Google Sheet เพื่อรับข้อมูล
        </button>
    </div>

    <!-- Modal Box for Google Sheet Setup -->
    <div id="guideModal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm hidden flex items-center justify-center p-4 z-50">
        <div class="bg-white rounded-2xl max-w-lg w-full p-6 space-y-4 max-h-[85vh] overflow-y-auto shadow-2xl">
            <div class="flex justify-between items-center border-b pb-3">
                <h3 class="font-bold text-slate-800 text-lg"><i class="fa-solid fa-file-csv text-emerald-600 mr-2"></i>วิธีเชื่อมต่อกับ Google Sheet</h3>
                <button onclick="toggleModal(false)" class="text-slate-400 hover:text-slate-600 text-xl font-bold cursor-pointer">&times;</button>
            </div>
            <div class="text-sm text-slate-600 space-y-3">
                <p><strong>ขั้นตอนการทำ (ทำเพียงครั้งเดียว):</strong></p>
                <ol class="list-decimal list-inside space-y-2 bg-slate-50 p-3 rounded-lg border text-xs leading-relaxed">
                    <li>เปิด <a href="https://sheets.google.com" target="_blank" class="text-indigo-600 underline">Google Sheets</a> สร้างสเปรดชีตใหม่</li>
                    <li>ตั้งชื่อหัวคอลัมน์แถวแรก ดังนี้: <br><code class="bg-slate-200 px-1 py-0.5 rounded text-indigo-800 font-mono">Timestamp | Type | Name | Room</code></li>
                    <li>ไปที่เมนู <strong>ส่วนขยาย (Extensions)</strong> &gt; <strong>Apps Script</strong></li>
                    <li>วางโค้ดแบบนี้ลงไป:
                        <pre class="bg-slate-800 text-emerald-400 p-2 rounded mt-1 overflow-x-auto text-[10px]">function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  sheet.appendRow([data.timestamp, data.type, data.name, data.room]);
  return ContentService.createTextOutput(JSON.stringify({"result": "success"}))
    .setMimeType(ContentService.MimeType.JSON);
}</pre>
                    </li>
                    <li>กด <strong>ทำให้ใช้งานได้อย่างเป็นทางการ (Deploy)</strong> &gt; <strong>การทำให้ใช้งานได้ใหม่ (New deployment)</strong></li>
                    <li>เลือกประเภทเป็น <strong>เว็บแอป (Web App)</strong></li>
                    <li>กำหนดสิทธิ์ <i>"ผู้ที่มีสิทธิ์เข้าถึง (Who has access)"</i> เป็น <strong>ทุกคน (Anyone)</strong></li>
                    <li>กด <strong>ทำให้ใช้งานได้ (Deploy)</strong> แล้วคัดลอก **URL เว็บแอป** มาวางในช่องตั้งค่าในหน้านี้</li>
                </ol>
            </div>
            <div class="pt-2 text-right">
                <button onclick="toggleModal(false)" class="px-4 py-2 bg-indigo-600 text-white rounded-xl text-sm font-semibold hover:bg-indigo-700 cursor-pointer">เข้าใจแล้ว</button>
            </div>
        </div>
    </div>

    <div id="toast" class="fixed bottom-5 right-5 transform translate-y-20 opacity-0 transition-all duration-300 bg-slate-800 text-white px-5 py-3 rounded-xl shadow-xl flex items-center gap-3 z-50 pointer-events-none">
        <i id="toastIcon" class="fa-solid fa-circle-check text-emerald-400"></i>
        <span id="toastMessage" class="text-sm">ข้อความแจ้งเตือน</span>
    </div>

    <script>
        function updateClock() {
            const now = new Date();
            
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            const seconds = String(now.getSeconds()).padStart(2, '0');
            document.getElementById('clock-display').textContent = hours + ':' + minutes + ':' + seconds;

            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('date-display').textContent = now.toLocaleDateString('th-TH', options);
        }

        setInterval(updateClock, 1000);
        updateClock();

        // Local State
        const sessionHistory = [];

        function cleanText(str) {
            if (!str) return '';
            return String(str)
                .replace(/&/g, '&amp;')
                .replace(/</g, '&lt;')
                .replace(/>/g, '&gt;')
                .replace(/"/g, '&quot;')
                .replace(/'/g, '&#039;');
        }

        async function handleAction(actionType) {
            const nameInput = document.getElementById('userName');
            const scriptUrlInput = document.getElementById('scriptUrl');

            const room = "ห้อง Innovation";
            const rawName = nameInput.value.trim();
            const name = rawName !== "" ? rawName : "ไม่ระบุชื่อ";
            
            const scriptUrl = scriptUrlInput.value.trim();

            const now = new Date();
            const timestamp = now.toLocaleString('th-TH');
            const actionLabel = actionType === 'CHECK_IN' ? 'Check In' : 'Check Out';

            toggleButtons(true);

            const payload = {
                timestamp: timestamp,
                type: actionLabel,
                name: name,
                room: room
            };

            if (scriptUrl) {
                try {
                    await fetch(scriptUrl, {
                        method: 'POST',
                        mode: 'no-cors',
                        headers: {
                            'Content-Type': 'application/json'
                        },
                        body: JSON.stringify(payload)
                    });
                } catch (error) {
                    console.error('Error saving to Google Sheet:', error);
                }
            }

            addHistoryRecord(payload);
            toggleButtons(false);
            nameInput.value = '';
            
            if (scriptUrl) {
                showToast('บันทึก ' + actionLabel + ' สำเร็จ! (ส่งเข้า Google Sheet เรียบร้อย)', 'success');
            } else {
                showToast('บันทึก ' + actionLabel + ' สำเร็จ! (ยังไม่ได้ใส่ URL Google Sheet)', 'info');
            }
        }

        function addHistoryRecord(record) {
            sessionHistory.unshift(record);
            const tbody = document.getElementById('historyTable');
            tbody.innerHTML = '';

            sessionHistory.forEach(item => {
                const tr = document.createElement('tr');
                const badgeColor = item.type === 'Check In' 
                    ? 'bg-emerald-100 text-emerald-700 border-emerald-200' 
                    : 'bg-rose-100 text-rose-700 border-rose-200';
                
                tr.innerHTML = `
                    <td class="p-3">
                        <span class="px-2 py-0.5 text-[10px] font-bold rounded-md border ${badgeColor}">
                            ${item.type}
                        </span>
                    </td>
                    <td class="p-3 font-medium text-slate-800">${cleanText(item.name)}</td>
                    <td class="p-3 text-slate-600">${cleanText(item.room)}</td>
                    <td class="p-3 text-slate-500 font-mono text-[11px]">${item.timestamp}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function toggleButtons(disabled) {
            const btnIn = document.getElementById('btnCheckIn');
            const btnOut = document.getElementById('btnCheckOut');
            
            btnIn.disabled = disabled;
            btnOut.disabled = disabled;
            
            if (disabled) {
                btnIn.classList.add('opacity-50', 'cursor-not-allowed');
                btnOut.classList.add('opacity-50', 'cursor-not-allowed');
            } else {
                btnIn.classList.remove('opacity-50', 'cursor-not-allowed');
                btnOut.classList.remove('opacity-50', 'cursor-not-allowed');
            }
        }

        function showToast(message, type) {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toastMessage');
            const toastIcon = document.getElementById('toastIcon');

            toastMsg.textContent = message;

            if (type === 'success') {
                toastIcon.className = 'fa-solid fa-circle-check text-emerald-400 text-lg';
            } else if (type === 'error') {
                toastIcon.className = 'fa-solid fa-circle-exclamation text-rose-400 text-lg';
            } else {
                toastIcon.className = 'fa-solid fa-circle-info text-blue-400 text-lg';
            }

            toast.classList.remove('translate-y-20', 'opacity-0');
            toast.classList.add('translate-y-0', 'opacity-100');

            setTimeout(() => {
                toast.classList.remove('translate-y-0', 'opacity-100');
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 4000);
        }

        // Modal Controls
        function toggleModal(show) {
            const modal = document.getElementById('guideModal');
            if (show) {
                modal.classList.remove('hidden');
            } else {
                modal.classList.add('hidden');
            }
        }
    </script>
</body>
</html>for 
