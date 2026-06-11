<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Điều Hành Bù Gap - HCM 1</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body { background-color: #f8fafc; color: #1e293b; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; overflow-x: hidden; }
        .card { background-color: #ffffff; border: 1px solid #e2e8f0; border-radius: 0.75rem; box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05); transition: all 0.3s ease; }
        .card:hover { border-color: #cbd5e1; }
        input[type="number"]::-webkit-inner-spin-button, input[type="number"]::-webkit-outer-spin-button { -webkit-appearance: none; margin: 0; }
        .btn-step { background-color: #f1f5f9; border: 1px solid #cbd5e1; color: #475569; transition: all 0.2s; }
        .btn-step:hover { background-color: #e2e8f0; color: #0f172a; }
        .btn-step:active { transform: scale(0.9); }
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
    </style>
</head>
<body class="p-4 md:p-6 text-sm">

<div class="container mx-auto max-w-[1400px] space-y-6">
    
    <div class="card p-5 flex flex-wrap justify-between items-center">
        <div class="flex items-center space-x-3 mb-4 md:mb-0">
            <span class="w-4 h-4 bg-teal-500 rounded-full animate-pulse"></span>
            <div>
                <h1 class="text-2xl md:text-3xl font-extrabold text-slate-800 uppercase tracking-tight">HCM 1 - HỆ THỐNG ĐIỀU HÀNH BÙ GAP</h1>
                <p class="text-slate-500 mt-1 text-xs md:text-sm italic">Thiết kế chuẩn Light Mode - Có dán dữ liệu và nút điều chỉnh ngày mượt mà</p>
            </div>
        </div>
        
        <div class="flex space-x-4 md:space-x-8 items-center bg-slate-50 p-3 rounded-xl border border-slate-200 shadow-sm">
            <div class="text-center">
                <p class="text-slate-500 text-[10px] font-bold uppercase mb-2">Số ngày đã qua</p>
                <div class="flex items-center space-x-1">
                    <button onclick="changeDays('passed', -1)" class="btn-step w-8 h-8 rounded-lg font-bold text-lg">-</button>
                    <input id="inputDaysPassed" type="number" value="10" oninput="updateTimeConfig()" class="w-12 bg-white border border-slate-300 rounded-lg py-1 text-xl font-bold text-slate-800 text-center focus:outline-none focus:border-teal-500 shadow-inner">
                    <button onclick="changeDays('passed', 1)" class="btn-step w-8 h-8 rounded-lg font-bold text-lg">+</button>
                </div>
            </div>
            
            <div class="text-center">
                <p class="text-slate-500 text-[10px] font-bold uppercase mb-2">Tổng ngày LV</p>
                <div class="flex items-center space-x-1">
                    <button onclick="changeDays('total', -1)" class="btn-step w-8 h-8 rounded-lg font-bold text-lg">-</button>
                    <input id="inputTotalDays" type="number" value="30" oninput="updateTimeConfig()" class="w-12 bg-white border border-slate-300 rounded-lg py-1 text-xl font-bold text-slate-800 text-center focus:outline-none focus:border-teal-500 shadow-inner">
                    <button onclick="changeDays('total', 1)" class="btn-step w-8 h-8 rounded-lg font-bold text-lg">+</button>
                </div>
            </div>

            <div class="border-l border-slate-300 pl-4 md:pl-8 text-center">
                <p class="text-slate-500 text-[10px] font-bold uppercase mb-2">Ngày còn lại</p>
                <div id="daysRemainingText" class="text-2xl font-black text-teal-600">20</div>
            </div>
        </div>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div class="card p-5 bg-slate-800 text-white shadow-xl">
            <h2 class="font-bold uppercase mb-1 text-xs flex items-center text-amber-400 tracking-wider">
                <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"></path></svg>
                BƯỚC 1: DÁN DỮ LIỆU ĐẦU NGÀY
            </h2>
            <p class="text-slate-400 text-[11px] mb-3 leading-tight">Copy bảng từ hệ thống/excel (3 cột) dán vào đây để cập nhật Lũy kế cho HCM 1</p>
            <textarea id="pasteArea" class="w-full h-24 bg-slate-900 border border-slate-700 rounded-lg p-3 text-slate-300 text-sm focus:outline-none focus:border-amber-400 mb-3 resize-none shadow-inner" placeholder="Dán dữ liệu thực tế tại đây..."></textarea>
            <button onclick="handlePasteData()" class="w-full bg-teal-600 hover:bg-teal-500 text-white font-bold py-2 px-4 rounded-lg text-sm transition-all flex justify-center items-center">
                LỌC DỮ LIỆU & TÍNH LẠI GAP
            </button>
        </div>

        <div class="card p-6 flex flex-col justify-between border-l-4 border-l-amber-400">
            <div class="flex justify-between items-start">
                <h2 class="text-slate-500 font-bold uppercase text-xs tracking-widest">Trạng thái lũy kế vùng</h2>
                <div id="regionGapCell" class="px-2 py-1 rounded text-[11px] font-black uppercase">Đang tính...</div>
            </div>
            <div class="mt-4">
                <p class="text-slate-400 text-xs font-semibold uppercase">Tổng Lũy Kế Thực Hiện:</p>
                <div id="totalTHCell" class="text-5xl font-black text-slate-800 mt-1 tracking-tight">0 <span class="text-lg font-bold text-slate-400">Trđ</span></div>
            </div>
            <div class="mt-4 border-t border-slate-100 pt-4 flex justify-between">
                <p class="text-slate-500 text-xs tracking-wide uppercase">Mục tiêu tháng: <span id="totalTargetCell" class="text-slate-800 font-black">0 Trđ</span></p>
            </div>
        </div>

        <div class="card p-6 border-t-4 border-teal-500 flex flex-col justify-between bg-teal-50/20">
            <div>
                <h2 class="text-teal-700 font-bold uppercase text-xs tracking-widest mb-1">MT Ngày mới toàn vùng</h2>
                <p class="text-slate-500 text-[11px] italic">Số tiền trung bình phải GN mỗi ngày kể từ hôm nay</p>
            </div>
            <div id="totalNewDailyTargetCell" class="text-6xl font-black text-teal-600 text-center my-2 tracking-tighter">0 <span class="text-xl font-bold text-slate-300">Trđ</span></div>
            <p class="text-slate-400 text-[10px] text-right font-medium">* Chiến thuật Bù Gap: Hụt bù - Vượt giữ nguyên áp lực</p>
        </div>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <div class="card p-0 lg:col-span-2 overflow-hidden flex flex-col border-slate-200">
            <div class="p-4 border-b border-slate-100 bg-slate-50/80">
                <h2 class="text-slate-700 font-bold uppercase text-xs flex items-center tracking-wider">
                    <svg class="w-4 h-4 mr-2 text-teal-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 3.055A9.001 9.001 0 1020.945 13H11V3.055z"></path></svg>
                    Theo dõi tiến độ bù gap từng khu vực - HCM 1
                </h2>
            </div>
            <div class="overflow-x-auto p-2">
                <table class="w-full text-left text-xs md:text-sm">
                    <thead>
                        <tr class="uppercase border-b border-slate-200">
                            <th class="py-3 px-2 text-slate-500 text-[10px]">QLKV / KHU VỰC</th>
                            <th class="py-3 px-2 text-right text-slate-500 text-[10px]">KH THÁNG</th>
                            <th class="py-3 px-2 text-right text-slate-500 text-[10px]">MT GỐC</th>
                            <th class="py-3 px-2 text-right text-slate-500 text-[10px]">LK KH</th>
                            <th class="py-3 px-2 text-right text-amber-600 text-[10px] bg-amber-50">THỰC TẾ LK</th>
                            <th class="py-3 px-2 text-right text-slate-500 text-[10px]">GAP</th>
                            <th class="py-3 px-2 text-right text-teal-600 text-[10px] font-bold">MT NGÀY MỚI</th>
                        </tr>
                    </thead>
                    <tbody id="targetTableBody"></tbody>
                </table>
            </div>
        </div>

        <div class="card p-0 flex flex-col shadow-lg border-slate-300">
            <div class="p-4 bg-slate-800 text-white flex justify-between items-center rounded-t-lg">
                <h2 class="font-bold uppercase text-xs flex items-center tracking-widest">
                    <svg class="w-4 h-4 mr-2 text-teal-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                    TRẠM CHECKPOINT
                </h2>
                <div class="text-[10px] font-bold text-teal-300">MT: <span id="cpDailyMTText">0</span> Trđ</div>
            </div>
            <div class="p-5 space-y-4">
                <div class="bg-slate-50 border border-slate-200 rounded-xl p-4 text-center">
                    <p class="text-slate-400 text-[10px] font-bold uppercase mb-1">Lũy kế giải ngân hôm nay:</p>
                    <div id="cpTotalGNCell" class="text-4xl font-black text-amber-500 tracking-tight">0 <span class="text-sm font-bold text-slate-300">Trđ</span></div>
                    <div id="cpTotalProgressText" class="text-sm font-black text-teal-600 mt-1">Tiến độ: 0%</div>
                </div>

                <button onclick="recordCheckpoint()" class="w-full bg-teal-600 hover:bg-teal-700 text-white font-bold py-3 px-4 rounded-xl shadow-lg transition-all text-sm active:scale-95">
                    GHI NHẬN CHECKPOINT
                </button>

                <div class="flex-1">
                    <h3 class="text-slate-500 text-[10px] font-bold uppercase mb-2">Nhật ký hôm nay:</h3>
                    <div class="max-h-[140px] overflow-y-auto pr-1">
                        <table class="w-full text-[11px] text-left">
                            <thead class="bg-slate-100 sticky top-0">
                                <tr class="text-slate-500 font-bold">
                                    <th class="py-1 px-1">Lần</th>
                                    <th class="py-1 px-1">Giờ</th>
                                    <th class="py-1 px-1 text-right">Đã GN</th>
                                    <th class="py-1 px-1 text-right">%</th>
                                </tr>
                            </thead>
                            <tbody id="checkpointLogBody"></tbody>
                        </table>
                    </div>
                </div>
                <div class="h-28"><canvas id="checkpointChart"></canvas></div>
            </div>
        </div>
    </div>

    <div class="card p-0 overflow-hidden border-slate-200">
        <div class="p-4 border-b border-slate-100 bg-slate-50/80">
            <h2 class="text-slate-700 font-bold uppercase text-xs flex items-center tracking-wider">
                <span class="w-2 h-4 bg-teal-500 mr-2 rounded-full"></span>
                Tiến độ thực hiện realtime từng Quản lý khu vực
            </h2>
        </div>
        <div class="overflow-x-auto p-4">
            <table class="w-full text-left text-sm table-fixed">
                <thead>
                    <tr class="uppercase border-b border-slate-200 text-slate-500 text-[10px] font-bold">
                        <th class="py-3 px-2 w-[220px]">QLKV / KHU VỰC</th>
                        <th class="py-3 px-2 text-right">MỤC TIÊU NGÀY</th>
                        <th class="py-3 px-2 text-center text-amber-600">ĐÃ GN <br><span class="text-[9px] font-normal normal-case italic text-slate-400">(Nhập số tại đây)</span></th>
                        <th class="py-3 px-2 text-center">TIẾN ĐỘ</th>
                        <th class="py-3 px-2 text-right">CÒN LẠI</th>
                        <th class="py-3 px-2 w-[250px]">NHỊP ĐỘ CHẠY SỐ</th>
                    </tr>
                </thead>
                <tbody id="checkpointTableBody"></tbody>
            </table>
        </div>
    </div>

    <div class="card overflow-hidden border border-slate-300 shadow-xl">
        <div class="bg-slate-100 px-5 py-3 flex justify-between items-center border-b border-slate-200">
            <h2 class="text-slate-700 font-bold uppercase text-xs flex items-center tracking-widest">
                <svg class="w-4 h-4 mr-2 text-rose-500" fill="currentColor" viewBox="0 0 20 20"><path d="M10 2a6 6 0 00-6 6v3.586l-.707.707A1 1 0 004 14h12a1 1 0 00.707-1.707L16 11.586V8a6 6 0 00-6 6zM10 18a3 3 0 01-3-3h6a3 3 0 01-3 3z"></path></svg>
                MẪU LỆNH CHỈ ĐẠO GỬI NHÓM GAPO (V6.0)
            </h2>
            <button onclick="copyTemplate()" class="bg-teal-600 hover:bg-teal-700 text-white text-xs font-black py-2 px-6 rounded-lg transition-all shadow-md active:scale-95 uppercase">
                COPY LỆNH
            </button>
        </div>
        <div class="p-6 bg-slate-50">
            <pre id="directiveTemplateArea" class="text-slate-700 font-mono text-[13px] whitespace-pre-wrap leading-relaxed bg-white p-5 rounded-lg border border-slate-200 shadow-inner"></pre>
        </div>
    </div>

</div>

<script>
    // DỮ LIỆU HCM 1
    let regionData = [
        { name: "Phan Ngọc Tường Vy", monthTarget: 33021, actualTH: 9659 },
        { name: "Huỳnh Ngọc Cường", monthTarget: 24191, actualTH: 8540 },
        { name: "Mã Trấn Hưng", monthTarget: 25200, actualTH: 6710 },
        { name: "Nguyễn Huy Hoàng", monthTarget: 28153, actualTH: 7392 },
        { name: "Nguyễn Thị Thùy Linh", monthTarget: 37866, actualTH: 11830 },
        { name: "Hồ Xuân Dũng", monthTarget: 51873, actualTH: 13016 }
    ];

    let totalDays = 30;
    let daysPassed = 10;
    let daysRemaining = 20;
    let computedData = [];
    let checkpointChart;
    const checkpointLogs = [];

    const formatTrd = (num) => Math.round(num).toLocaleString('vi-VN');

    // Logic Tăng/Giảm ngày
    window.changeDays = (type, delta) => {
        if (type === 'passed') {
            const el = document.getElementById('inputDaysPassed');
            let val = parseInt(el.value) + delta;
            el.value = Math.max(0, val);
        } else {
            const el = document.getElementById('inputTotalDays');
            let val = parseInt(el.value) + delta;
            el.value = Math.max(1, val);
        }
        updateTimeConfig();
    }

    const calculateData = () => {
        daysRemaining = Math.max(0, totalDays - daysPassed);
        document.getElementById('daysRemainingText').innerText = daysRemaining;

        computedData = regionData.map((row, index) => {
            const safeTotalDays = totalDays > 0 ? totalDays : 1; 
            const mtOriginDaily = row.monthTarget / safeTotalDays;
            const lkKH = mtOriginDaily * daysPassed;
            const gap = row.actualTH - lkKH;
            
            let newDailyTarget = mtOriginDaily;
            if (gap < 0 && daysRemaining > 0) {
                newDailyTarget = mtOriginDaily + (Math.abs(gap) / daysRemaining);
            } else if (daysRemaining === 0) {
                newDailyTarget = Math.max(0, -gap);
            }

            const currentGN = (computedData[index] && computedData[index].currentGN) || 0;
            const progressCP = newDailyTarget > 0 ? (currentGN / newDailyTarget * 100) : 0;
            const remainCP = newDailyTarget - currentGN;

            return {
                ...row, mtOriginDaily, lkKH, gap, newDailyTarget, currentGN, progressCP, remainCP
            };
        });
    }

    // Logic Dán dữ liệu
    window.handlePasteData = () => {
        const pasteText = document.getElementById('pasteArea').value.trim();
        if(!pasteText) return;
        const lines = pasteText.split('\n');
        let updatedCount = 0;
        lines.forEach(line => {
            const parts = line.split('\t');
            if(parts.length >= 2) {
                let actualStr = parts[parts.length-1].replace(/,/g, '').replace(/\./g,'').trim();
                let num = parseFloat(actualStr);
                let namePart = parts[0].toLowerCase();
                let matchedIndex = regionData.findIndex(r => namePart.includes(r.name.split(' ').pop().toLowerCase()));
                if(!isNaN(num)) {
                    if(matchedIndex !== -1) {
                        regionData[matchedIndex].actualTH = num;
                        updatedCount++;
                    } else if (updatedCount < regionData.length) {
                        regionData[updatedCount].actualTH = num;
                        updatedCount++;
                    }
                }
            }
        });
        if(updatedCount > 0) {
            calculateData(); renderAll();
            alert(`Đã cập nhật số liệu thực tế cho ${updatedCount} KV của HCM 1.`);
            document.getElementById('pasteArea').value = '';
        }
    }

    const updateRegionTotals = () => {
        const totalActualTH = computedData.reduce((sum, row) => sum + row.actualTH, 0);
        const totalMonthTarget = computedData.reduce((sum, row) => sum + row.monthTarget, 0);
        const totalGap = computedData.reduce((sum, row) => sum + row.gap, 0);
        const totalNewDailyTarget = computedData.reduce((sum, row) => sum + row.newDailyTarget, 0);
        const totalCurrentGN = computedData.reduce((sum, row) => sum + (Number(row.currentGN) || 0), 0);
        const totalProgress = totalNewDailyTarget ? (totalCurrentGN / totalNewDailyTarget * 100).toFixed(1) : 0;

        document.getElementById('totalTHCell').innerHTML = `${formatTrd(totalActualTH)} <span class="text-sm font-bold text-slate-400 uppercase">Trđ</span>`;
        document.getElementById('totalTargetCell').innerText = `${formatTrd(totalMonthTarget)} Trđ`;
        document.getElementById('totalNewDailyTargetCell').innerHTML = `${formatTrd(totalNewDailyTarget)} <span class="text-xl font-bold text-slate-300">Trđ</span>`;
        
        const gapCell = document.getElementById('regionGapCell');
        const isSurplus = totalGap >= 0;
        gapCell.innerText = `${isSurplus ? 'Vượt Vùng: +' : 'Hụt Vùng: '}${formatTrd(totalGap)} Trđ`;
        gapCell.className = `px-2 py-1 rounded text-[10px] font-black border ${isSurplus ? 'bg-emerald-50 text-emerald-600 border-emerald-200' : 'bg-rose-50 text-rose-600 border-rose-200'}`;

        document.getElementById('cpDailyMTText').innerText = formatTrd(totalNewDailyTarget);
        document.getElementById('cpTotalGNCell').innerHTML = `${formatTrd(totalCurrentGN)} <span class="text-sm font-bold text-slate-300">Trđ</span>`;
        document.getElementById('cpTotalProgressText').innerHTML = `Tiến độ: <span class="${totalProgress>=100 ? 'text-emerald-500' : 'text-teal-600'}">${totalProgress}%</span>`;
        updateTemplate();
    }

    const renderBar = (computedRow) => {
        const progress = Math.min(computedRow.progressCP, 100);
        const barColor = computedRow.progressCP >= 100 ? 'bg-emerald-500' : 'bg-teal-500';
        return `<div class="w-full bg-slate-100 rounded-full h-2 overflow-hidden border border-slate-200 shadow-inner">
                    <div class="h-full rounded-full transition-all duration-700 ease-in-out ${barColor}" style="width: ${progress}%"></div>
                </div>`;
    }

    const renderTargetTable = () => {
        document.getElementById('targetTableBody').innerHTML = computedData.map(row => `
            <tr class="border-b border-slate-100 transition hover:bg-slate-50/50">
                <td class="py-4 px-2 font-black text-slate-700">${row.name}</td>
                <td class="py-4 px-2 text-right font-medium">${formatTrd(row.monthTarget)}</td>
                <td class="py-4 px-2 text-right text-slate-400">${formatTrd(row.mtOriginDaily)}</td>
                <td class="py-4 px-2 text-right text-slate-400">${formatTrd(row.lkKH)}</td>
                <td class="py-4 px-2 text-right font-black text-amber-600 bg-amber-50/30">${formatTrd(row.actualTH)}</td>
                <td class="py-4 px-2 text-right font-bold ${row.gap >= 0 ? 'text-emerald-600' : 'text-rose-500'}">${row.gap >= 0 ? '+' : ''}${formatTrd(row.gap)}</td>
                <td class="py-4 px-2 text-right font-black text-teal-600">${formatTrd(row.newDailyTarget)}</td>
            </tr>`).join('');
    }

    const renderCheckpointTableRows = () => {
        document.getElementById('checkpointTableBody').innerHTML = computedData.map((row, index) => `
            <tr data-index="${index}" class="border-b border-slate-100 transition hover:bg-slate-50/50">
                <td class="py-4 px-2 font-black text-slate-700">${row.name}</td>
                <td class="py-4 px-2 text-right font-bold text-slate-500">${formatTrd(row.newDailyTarget)}</td>
                <td class="py-4 px-2 text-center">
                    <input type="number" step="0.1" value="${row.currentGN || ''}" class="w-24 bg-white border border-slate-300 rounded-lg px-2 py-1.5 text-amber-600 font-black text-center focus:outline-none focus:border-teal-500 shadow-sm" oninput="updateCheckpointInput(this)" />
                </td>
                <td class="py-4 px-2 text-center progressCP">
                    <span class="px-2 py-1 rounded text-[11px] font-black ${row.progressCP >= 100 ? 'text-emerald-600 bg-emerald-50' : 'text-slate-700'}">${row.progressCP.toFixed(1)}%</span>
                </td>
                <td class="py-4 px-2 text-right text-[11px] remainCP font-black ${row.remainCP <= 0 ? 'text-emerald-500' : 'text-slate-400'}">${formatTrd(row.remainCP)}</td>
                <td class="py-4 px-2 barBar">${renderBar(row)}</td>
            </tr>`).join('');
    }

    window.updateTimeConfig = () => {
        totalDays = parseFloat(document.getElementById('inputTotalDays').value) || 0;
        daysPassed = parseFloat(document.getElementById('inputDaysPassed').value) || 0;
        calculateData(); renderAll();
    }

    window.updateCheckpointInput = (input) => {
        const row = input.closest('tr');
        const index = row.dataset.index;
        const value = parseFloat(input.value) || 0;
        computedData[index].currentGN = value;
        const mtNgàyMới = computedData[index].newDailyTarget;
        const progress = mtNgàyMới > 0 ? (value / mtNgàyMới * 100) : 0;
        computedData[index].progressCP = progress;
        computedData[index].remainCP = mtNgàyMới - value;

        row.querySelector('.progressCP').innerHTML = `<span class="px-2 py-1 rounded text-[11px] font-black ${progress >= 100 ? 'text-emerald-600 bg-emerald-50' : 'text-slate-700'}">${progress.toFixed(1)}%</span>`;
        row.querySelector('.remainCP').innerHTML = `<span class="font-black ${computedData[index].remainCP <= 0 ? 'text-emerald-500' : 'text-slate-400'}">${formatTrd(computedData[index].remainCP)}</span>`;
        row.querySelector('.barBar').innerHTML = renderBar(computedData[index]);
        updateRegionTotals();
    }

    const renderAll = () => { renderTargetTable(); renderCheckpointTableRows(); updateRegionTotals(); }

    const updateTemplate = () => {
        const now = new Date();
        const time = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
        const totalGN = computedData.reduce((sum, row) => sum + (Number(row.currentGN) || 0), 0);
        const totalMT = computedData.reduce((sum, row) => sum + row.newDailyTarget, 0);
        const progress = totalMT ? (totalGN / totalMT * 100).toFixed(1) : 0;
        let txt = `[CHECKPOINT TIẾN ĐỘ GIẢI NGÂN - VÙNG HCM 1]\n`;
        txt += `⏰ Thời gian: ${time} (Check lần ${checkpointLogs.length + 1})\n`;
        txt += `🎯 TỔNG VÙNG: ${formatTrd(totalGN)} / ${formatTrd(totalMT)} Trđ (${progress}%)\n\n`;
        txt += `🔥 TIẾN ĐỘ CHI TIẾT TỪNG KHU VỰC:\n`;
        computedData.forEach(row => {
            const gn = Number(row.currentGN) || 0;
            const res = row.remainCP > 0 ? `👉 Còn ${formatTrd(row.remainCP)} Trđ` : `✅ ĐÃ ĐẠT!`;
            txt += `- ${row.name}: ${formatTrd(gn)} / ${formatTrd(row.newDailyTarget)} Trđ (${row.progressCP.toFixed(1)}%) -> ${res}\n`;
        });
        document.getElementById('directiveTemplateArea').innerText = txt;
    }

    window.copyTemplate = () => {
        navigator.clipboard.writeText(document.getElementById('directiveTemplateArea').innerText).then(() => alert('Đã copy mẫu lệnh HCM 1!'));
    }

    window.recordCheckpoint = () => {
        const now = new Date();
        const time = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
        const totalGN = computedData.reduce((sum, row) => sum + (Number(row.currentGN) || 0), 0);
        const totalMT = computedData.reduce((sum, row) => sum + row.newDailyTarget, 0);
        const progress = totalMT ? (totalGN / totalMT * 100).toFixed(1) : 0;
        checkpointLogs.push({ times: time, amount: totalGN, progress: progress });
        document.getElementById('checkpointLogBody').innerHTML = checkpointLogs.map((log, i) => `
            <tr class="border-b border-slate-100 text-slate-600">
                <td class="py-2 px-1 font-bold">${i + 1}</td>
                <td class="py-2 px-1 text-teal-600 font-mono">${log.times}</td>
                <td class="py-2 px-1 text-right text-amber-600 font-black">${formatTrd(log.amount)}</td>
                <td class="py-2 px-1 text-right font-black text-slate-400">${log.progress}%</td>
            </tr>`).join('');
        if (checkpointChart) {
            checkpointChart.data.labels.push(time);
            checkpointChart.data.datasets[0].data.push(totalGN);
            checkpointChart.update();
        }
        updateTemplate();
    }

    const initChart = () => {
        const ctx = document.getElementById('checkpointChart').getContext('2d');
        checkpointChart = new Chart(ctx, {
            type: 'line',
            data: { labels: [], datasets: [{ data: [], borderColor: '#0d9488', backgroundColor: 'rgba(20, 184, 166, 0.05)', borderWidth: 3, pointRadius: 2, fill: true, tension: 0.4 }] },
            options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { x: { display: false }, y: { display: false, beginAtZero: true } } }
        });
    }

    initChart(); calculateData(); renderAll();
</script>
</body>
</html>
