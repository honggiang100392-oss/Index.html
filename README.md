<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Điều Hành Bù Gap - HCM 1 (Light Mode)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* Theme Sáng - Light Mode */
        body { background-color: #f8fafc; color: #1e293b; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; overflow-x: hidden; }
        .card { background-color: #ffffff; border: 1px solid #e2e8f0; border-radius: 0.75rem; box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05); transition: all 0.3s ease; }
        .card:hover { border-color: #cbd5e1; box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.05); }
        
        /* Ẩn mũi tên lên xuống của ô nhập số */
        input[type="number"]::-webkit-inner-spin-button, 
        input[type="number"]::-webkit-outer-spin-button { -webkit-appearance: none; margin: 0; }
        
        /* Tùy chỉnh thanh cuộn sáng */
        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
        
        /* Bảng */
        thead { background-color: #f8fafc; }
        th { font-weight: 600; color: #64748b; }
        tr.border-b { border-color: #e2e8f0; }
        tbody tr:hover { background-color: #f1f5f9; }
    </style>
</head>
<body class="p-4 md:p-6 text-sm">

<div class="container mx-auto max-w-[1400px] space-y-6">
    
    <div class="card p-5 flex flex-wrap justify-between items-center bg-white">
        <div class="flex items-center space-x-3 mb-4 md:mb-0">
            <span class="w-4 h-4 bg-teal-500 rounded-full animate-pulse shadow-sm"></span>
            <div>
                <h1 class="text-2xl md:text-3xl font-extrabold text-slate-800 uppercase tracking-tight">HCM 1 - Hệ Thống Điều Hành Bù Gap</h1>
                <p class="text-slate-500 mt-1 text-xs md:text-sm">Tự động bóc tách số liệu giải ngân trực tiếp từ hệ thống bằng cơ chế sao chép dán</p>
            </div>
        </div>
        <div class="flex space-x-4 md:space-x-8 text-center items-center bg-slate-50 p-3 rounded-lg border border-slate-200">
            <div>
                <p class="text-slate-500 text-xs font-semibold uppercase mb-1">Số ngày đã qua</p>
                <input id="inputDaysPassed" type="number" value="10" oninput="updateTimeConfig()" class="w-16 bg-white border border-slate-300 rounded px-2 py-1 text-lg font-bold text-slate-800 text-center focus:outline-none focus:border-teal-500 focus:ring-1 focus:ring-teal-500 transition shadow-sm">
            </div>
            <div>
                <p class="text-slate-500 text-xs font-semibold uppercase mb-1">Tổng ngày LV</p>
                <input id="inputTotalDays" type="number" value="30" oninput="updateTimeConfig()" class="w-16 bg-white border border-slate-300 rounded px-2 py-1 text-lg font-bold text-slate-800 text-center focus:outline-none focus:border-teal-500 focus:ring-1 focus:ring-teal-500 transition shadow-sm">
            </div>
            <div class="border-l border-slate-300 pl-4 md:pl-8">
                <p class="text-slate-500 text-xs font-semibold uppercase mb-1">Số ngày còn lại</p>
                <div id="daysRemainingText" class="text-xl font-extrabold text-teal-600 mt-1">20 Ngày</div>
            </div>
        </div>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div class="card p-5 bg-slate-800 text-white">
            <h2 class="font-bold uppercase mb-1 text-sm flex items-center text-amber-400">
                <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"></path></svg>
                BƯỚC 1: DÁN DỮ LIỆU ĐẦU NGÀY
            </h2>
            <p class="text-slate-400 text-xs mb-3">Copy bảng thực tế lũy kế từ Excel (3 cột) rồi dán vào đây</p>
            <textarea id="pasteArea" class="w-full h-24 bg-slate-900 border border-slate-600 rounded p-3 text-slate-300 text-sm focus:outline-none focus:border-amber-400 mb-3 resize-none" placeholder="Nhấp chuột vào đây và nhấn Ctrl + V..."></textarea>
            <button onclick="handlePasteData()" class="w-full bg-teal-500 hover:bg-teal-400 text-white font-bold py-2 px-4 rounded text-sm transition flex justify-center items-center">
                <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                TRÍCH XUẤT SỐ LŨY KẾ ĐẦU NGÀY
            </button>
        </div>

        <div class="card p-6 flex flex-col justify-between">
            <div class="flex justify-between items-start">
                <h2 class="text-slate-600 font-bold uppercase text-sm tracking-wide">Trạng thái lũy kế vùng</h2>
                <div id="regionGapCell" class="px-3 py-1 rounded text-xs font-bold bg-rose-100 text-rose-600 border border-rose-200">Hụt Vùng: -0 Trđ</div>
            </div>
            <div class="mt-4">
                <p class="text-slate-500 text-xs uppercase font-semibold">Tổng Lũy Kế Đã Đạt:</p>
                <div id="totalTHCell" class="text-4xl lg:text-5xl font-extrabold text-amber-500 mt-1">0 <span class="text-lg lg:text-xl font-semibold text-slate-400">Trđ</span></div>
            </div>
            <div class="flex justify-between items-end mt-6 border-t border-slate-100 pt-4">
                <p class="text-slate-500 text-xs">Mục tiêu tháng: <span id="totalTargetCell" class="text-slate-800 font-bold">0 Trđ</span></p>
            </div>
        </div>

        <div class="card p-6 border-t-4 border-teal-500 flex flex-col justify-between bg-teal-50/30">
            <div>
                <h2 class="text-teal-700 font-bold uppercase text-sm tracking-wide mb-1">Mục tiêu ngày mới toàn vùng</h2>
                <p class="text-slate-500 text-xs">Tổng chỉ tiêu sau khi áp gap chiến thuật</p>
            </div>
            <div id="totalNewDailyTargetCell" class="text-5xl lg:text-6xl font-extrabold text-teal-600 text-center my-4">0 <span class="text-xl font-semibold text-slate-400">Trđ</span></div>
            <p class="text-slate-400 text-[10px] italic text-right">* Chiến thuật: Hụt số phải bù gap - Vượt giữ nguyên áp lực gốc</p>
        </div>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <div class="card p-0 lg:col-span-2 overflow-hidden flex flex-col">
            <div class="p-4 border-b border-slate-200 bg-slate-50">
                <h2 class="text-slate-700 font-bold uppercase text-sm flex items-center">
                    <svg class="w-4 h-4 mr-2 text-slate-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 17v-2m3 2v-4m3 4v-6m2 10H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg>
                    Bổ Chỉ Tiêu Ngày Và Theo Dõi Tiến Độ Bù Gap Từng QLKV
                </h2>
            </div>
            <div class="overflow-x-auto flex-1 p-4">
                <table class="w-full text-left border-collapse text-xs md:text-sm">
                    <thead>
                        <tr class="uppercase border-b-2 border-slate-200">
                            <th class="py-3 px-2 text-slate-600">KV / QLKV</th>
                            <th class="py-3 px-2 text-right text-slate-600">KH THÁNG (TRĐ)</th>
                            <th class="py-3 px-2 text-right text-slate-600">MT GỐC/NGÀY</th>
                            <th class="py-3 px-2 text-right text-slate-600">LK KẾ HOẠCH</th>
                            <th class="py-3 px-2 text-right text-amber-600">THỰC TẾ LK</th>
                            <th class="py-3 px-2 text-right text-slate-600">GAP HIỆN TẠI</th>
                            <th class="py-3 px-2 text-right text-teal-600 font-bold">MT NGÀY MỚI</th>
                        </tr>
                    </thead>
                    <tbody id="targetTableBody">
                        </tbody>
                </table>
            </div>
        </div>

        <div class="card p-0 flex flex-col border-slate-300 shadow-md">
            <div class="p-4 border-b border-slate-200 bg-slate-800 flex justify-between items-center rounded-t-xl">
                <h2 class="text-white font-bold uppercase text-sm flex items-center">
                    <svg class="w-4 h-4 mr-2 text-teal-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                    TRẠM CHECKPOINT
                </h2>
                <span class="bg-teal-900 text-teal-300 text-xs px-2 py-1 rounded border border-teal-700">MT: <span id="cpDailyMTText">0</span> Trđ</span>
            </div>
            <div class="p-5 flex-1 flex flex-col space-y-4">
                <div class="bg-slate-50 border border-slate-200 rounded p-4 text-center">
                    <p class="text-slate-500 text-xs font-semibold mb-1 uppercase">Tổng Đã GN Ghi Nhận Hiện Tại:</p>
                    <div id="cpTotalGNCell" class="text-3xl font-extrabold text-amber-500">0 <span class="text-sm font-normal text-slate-400">Trđ</span></div>
                    <div id="cpTotalProgressText" class="text-sm font-bold text-teal-600 mt-1">Tiến độ: 0%</div>
                </div>

                <button onclick="recordCheckpoint()" class="w-full bg-teal-600 hover:bg-teal-700 text-white font-bold py-3 px-4 rounded shadow transition text-sm">
                    GHI NHẬN CHECKPOINT LIỀN TAY
                </button>

                <div class="flex-1">
                    <h3 class="text-slate-600 text-[10px] font-bold uppercase mb-2">Nhật ký Checkpoint hôm nay:</h3>
                    <div class="max-h-[120px] overflow-y-auto pr-1">
                        <table class="w-full text-xs text-left">
                            <thead class="bg-slate-100 sticky top-0">
                                <tr class="text-slate-500">
                                    <th class="py-1 px-1">Lần</th>
                                    <th class="py-1 px-1">Mốc Giờ</th>
                                    <th class="py-1 px-1 text-right">Đã GN</th>
                                    <th class="py-1 px-1 text-right">Tiến độ</th>
                                </tr>
                            </thead>
                            <tbody id="checkpointLogBody">
                            </tbody>
                        </table>
                    </div>
                </div>
                
                <div class="h-24">
                    <canvas id="checkpointChart"></canvas>
                </div>
            </div>
        </div>
    </div>

    <div class="card p-0 overflow-hidden">
        <div class="p-4 border-b border-slate-200 bg-slate-50">
            <h2 class="text-slate-700 font-bold uppercase text-sm flex items-center">
                <span class="w-3 h-4 bg-teal-500 mr-2 rounded-sm"></span>
                Tiến độ Checkpoint chi tiết từng QLKV
            </h2>
        </div>
        <div class="overflow-x-auto p-4">
            <table class="w-full text-left border-collapse table-fixed text-sm">
                <thead>
                    <tr class="uppercase border-b-2 border-slate-200">
                        <th class="py-3 px-2 w-[220px] text-slate-600">KV / QLKV CHI TIẾT</th>
                        <th class="py-3 px-2 text-right text-slate-600">MỤC TIÊU NGÀY</th>
                        <th class="py-3 px-2 text-center text-amber-600">ĐÃ GN (TRĐ) <br><span class="text-[10px] text-slate-400 font-normal normal-case">(Nhập trực tiếp)</span></th>
                        <th class="py-3 px-2 text-center text-slate-600">TIẾN ĐỘ</th>
                        <th class="py-3 px-2 text-right text-slate-600">CÒN PHẢI CHẠY</th>
                        <th class="py-3 px-2 w-[250px] text-slate-600">THANH NHỊP ĐỘ CHẠY SỐ</th>
                    </tr>
                </thead>
                <tbody id="checkpointTableBody">
                    </tbody>
            </table>
        </div>
    </div>

    <div class="card overflow-hidden border border-slate-300">
        <div class="bg-slate-100 px-5 py-3 flex justify-between items-center border-b border-slate-200">
            <h2 class="text-slate-700 font-bold uppercase text-sm flex items-center">
                <svg class="w-4 h-4 mr-2 text-rose-500" fill="currentColor" viewBox="0 0 20 20"><path d="M10 2a6 6 0 00-6 6v3.586l-.707.707A1 1 0 004 14h12a1 1 0 00.707-1.707L16 11.586V8a6 6 0 00-6 6zM10 18a3 3 0 01-3-3h6a3 3 0 01-3 3z"></path></svg>
                MẪU LỆNH CHỈ ĐẠO SAU CHECKPOINT V5.5 (GỬI GAPO)
            </h2>
            <button onclick="copyTemplate()" class="bg-teal-600 hover:bg-teal-700 text-white text-xs font-bold py-2 px-4 rounded flex items-center transition shadow-sm">
                <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"></path></svg>
                COPY LỆNH
            </button>
        </div>
        <div class="p-5 bg-white">
            <pre id="directiveTemplateArea" class="text-slate-700 font-mono text-sm whitespace-pre-wrap leading-relaxed bg-slate-50 p-4 rounded border border-slate-200"></pre>
        </div>
    </div>

</div>

<script>
    // Dữ liệu gốc (Khởi tạo sẵn số liệu của HCM1)
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

    // Format tiền tệ
    const formatTrd = (num) => Math.round(num).toLocaleString('vi-VN');

    // 1. TÍNH TOÁN DỮ LIỆU
    const calculateData = () => {
        daysRemaining = Math.max(0, totalDays - daysPassed);
        document.getElementById('daysRemainingText').innerText = daysRemaining + ' Ngày';

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

    // 2. XỬ LÝ DÁN DỮ LIỆU (BƯỚC 1)
    window.handlePasteData = () => {
        const pasteText = document.getElementById('pasteArea').value.trim();
        if(!pasteText) {
            alert('Vui lòng dán dữ liệu vào ô trước khi bấm trích xuất!');
            return;
        }

        const lines = pasteText.split('\n');
        let updatedCount = 0;

        lines.forEach(line => {
            const parts = line.split('\t');
            if(parts.length >= 2) {
                // Lấy phần tử cuối cùng làm số Thực tế (bỏ dấu phẩy/chấm)
                let actualStr = parts[parts.length-1].replace(/,/g, '').replace(/\./g,'').trim();
                let num = parseFloat(actualStr);
                
                // Cố gắng map theo tên QLKV (Tìm chuỗi tương đồng)
                let namePart = parts[0].toLowerCase();
                let matchedIndex = regionData.findIndex(r => namePart.includes(r.name.split(' ').pop().toLowerCase()));
                
                if(!isNaN(num)) {
                    if(matchedIndex !== -1) {
                        regionData[matchedIndex].actualTH = num;
                        updatedCount++;
                    } else if (updatedCount < regionData.length) {
                        // Nếu không tìm thấy tên, đổ tuần tự từ trên xuống
                        regionData[updatedCount].actualTH = num;
                        updatedCount++;
                    }
                }
            }
        });

        if(updatedCount > 0) {
            calculateData();
            renderAll();
            alert(`Thành công! Đã cập nhật số liệu Thực tế Lũy kế cho ${updatedCount} Khu vực.`);
            document.getElementById('pasteArea').value = ''; // Xóa ô dán
        } else {
            alert('Không nhận diện được số liệu. Hãy copy đủ cột Tên Khu Vực và Số Thực Tế (cách nhau bằng Tab).');
        }
    }

    // 3. CẬP NHẬT UI
    const updateRegionTotals = () => {
        const totalActualTH = computedData.reduce((sum, row) => sum + row.actualTH, 0);
        const totalMonthTarget = computedData.reduce((sum, row) => sum + row.monthTarget, 0);
        const totalGap = computedData.reduce((sum, row) => sum + row.gap, 0);
        const totalNewDailyTarget = computedData.reduce((sum, row) => sum + row.newDailyTarget, 0);
        const totalCurrentGN = computedData.reduce((sum, row) => sum + (Number(row.currentGN) || 0), 0);
        const totalProgress = totalNewDailyTarget ? (totalCurrentGN / totalNewDailyTarget * 100).toFixed(1) : 0;

        document.getElementById('totalTHCell').innerHTML = `${formatTrd(totalActualTH)} <span class="text-lg lg:text-xl font-semibold text-slate-400">Trđ</span>`;
        document.getElementById('totalTargetCell').innerText = `${formatTrd(totalMonthTarget)} Trđ`;
        document.getElementById('totalNewDailyTargetCell').innerHTML = `${formatTrd(totalNewDailyTarget)} <span class="text-xl font-semibold text-slate-400">Trđ</span>`;
        
        const gapCell = document.getElementById('regionGapCell');
        const isSurplus = totalGap >= 0;
        gapCell.innerText = `${isSurplus ? 'Vượt Vùng: +' : 'Hụt Vùng: '}${formatTrd(totalGap)} Trđ`;
        gapCell.className = `px-3 py-1 rounded text-xs font-bold border ${isSurplus ? 'bg-emerald-50 text-emerald-600 border-emerald-200' : 'bg-rose-50 text-rose-600 border-rose-200'}`;

        document.getElementById('cpDailyMTText').innerText = formatTrd(totalNewDailyTarget);
        document.getElementById('cpTotalGNCell').innerHTML = `${formatTrd(totalCurrentGN)} <span class="text-sm font-normal text-slate-400">Trđ</span>`;
        document.getElementById('cpTotalProgressText').innerHTML = `Tiến độ: <span class="${totalProgress>=100 ? 'text-emerald-500' : 'text-teal-600'}">${totalProgress}%</span>`;
        
        updateTemplate();
    }

    const renderBar = (computedRow) => {
        const progress = computedRow.progressCP;
        const roundedProgress = Math.min(progress, 100);
        const barColor = progress >= 100 ? 'bg-emerald-500' : 'bg-teal-400';
        return `<div class="w-full bg-slate-200 rounded-full h-2.5 overflow-hidden">
                    <div class="h-full rounded-full transition-all duration-500 ease-out ${barColor}" style="width: ${roundedProgress}%"></div>
                </div>`;
    }

    const renderTargetTable = () => {
        document.getElementById('targetTableBody').innerHTML = computedData.map(row => `
            <tr class="border-b transition hover:bg-slate-50 text-slate-700">
                <td class="py-3 px-2 font-bold text-slate-800">${row.name}</td>
                <td class="py-3 px-2 text-right">${formatTrd(row.monthTarget)}</td>
                <td class="py-3 px-2 text-right">${formatTrd(row.mtOriginDaily)}</td>
                <td class="py-3 px-2 text-right">${formatTrd(row.lkKH)}</td>
                <td class="py-3 px-2 text-right font-bold text-amber-600">${formatTrd(row.actualTH)}</td>
                <td class="py-3 px-2 text-right font-bold ${row.gap >= 0 ? 'text-emerald-600' : 'text-rose-600'}">${row.gap >= 0 ? '+' : ''}${formatTrd(row.gap)}</td>
                <td class="py-3 px-2 text-right font-bold text-teal-600">${formatTrd(row.newDailyTarget)}</td>
            </tr>`).join('');
    }

    const renderCheckpointTableRows = () => {
        document.getElementById('checkpointTableBody').innerHTML = computedData.map((row, index) => `
            <tr data-index="${index}" class="border-b transition hover:bg-slate-50">
                <td class="py-4 px-2 font-bold text-slate-800 truncate">${row.name}</td>
                <td class="py-4 px-2 text-right font-bold text-slate-600">${formatTrd(row.newDailyTarget)}</td>
                <td class="py-4 px-2 text-center">
                    <input type="number" step="0.1" value="${row.currentGN || ''}" placeholder="Nhập số" class="w-24 bg-white border border-slate-300 rounded px-2 py-1.5 text-amber-600 font-bold text-center focus:outline-none focus:border-teal-500 focus:ring-1 focus:ring-teal-500 shadow-inner" oninput="updateCheckpointInput(this)" />
                </td>
                <td class="py-4 px-2 text-center progressCP">
                    <span class="px-2 py-1 rounded text-sm font-bold ${row.progressCP >= 100 ? 'text-emerald-600 bg-emerald-50' : 'text-slate-700'}">${row.progressCP.toFixed(1)}%</span>
                </td>
                <td class="py-4 px-2 text-right text-sm remainCP font-semibold ${row.remainCP <= 0 ? 'text-emerald-500' : 'text-slate-500'}">${formatTrd(row.remainCP)}</td>
                <td class="py-4 px-2 barBar">${renderBar(row)}</td>
            </tr>`).join('');
    }

    // 4. INTERACTION LOGIC
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
        const remain = mtNgàyMới - value;

        computedData[index].progressCP = progress;
        computedData[index].remainCP = remain;

        row.querySelector('.progressCP').innerHTML = `<span class="px-2 py-1 rounded text-sm font-bold ${progress >= 100 ? 'text-emerald-600 bg-emerald-50' : 'text-slate-700'}">${progress.toFixed(1)}%</span>`;
        row.querySelector('.remainCP').innerHTML = `<span class="text-sm font-semibold ${remain <= 0 ? 'text-emerald-500' : 'text-slate-500'}">${formatTrd(remain)}</span>`;
        row.querySelector('.barBar').innerHTML = renderBar(computedData[index]);

        updateRegionTotals();
    }

    const renderAll = () => {
        renderTargetTable();
        renderCheckpointTableRows();
        updateRegionTotals();
    }

    // 5. MẪU LỆNH CHỈ ĐẠO
    const updateTemplate = () => {
        const now = new Date();
        const time = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
        const totalGN = computedData.reduce((sum, row) => sum + (Number(row.currentGN) || 0), 0);
        const totalMT = computedData.reduce((sum, row) => sum + row.newDailyTarget, 0);
        const progress = totalMT ? (totalGN / totalMT * 100).toFixed(1) : 0;

        let txt = `[CẬP NHẬT TIẾN ĐỘ CHECKPOINT - VÙNG HCM 1]\n`;
        txt += `⏰ Thời gian ghi nhận: ${time} (Check lần ${checkpointLogs.length + 1})\n`;
        txt += `🎯 TOÀN VÙNG: ${formatTrd(totalGN)} Trđ / ${formatTrd(totalMT)} Trđ (${progress}%)\n\n`;
        
        txt += `🔥 CHI TIẾT TIẾN ĐỘ TỪNG KHU VỰC:\n`;
        computedData.forEach(row => {
            const gn = Number(row.currentGN) || 0;
            const remains = row.remainCP > 0 ? `Cần kéo: ${formatTrd(row.remainCP)} Trđ` : 'Đã Đạt Chỉ Tiêu!';
            txt += `👉 ${row.name}: ${formatTrd(gn)} / ${formatTrd(row.newDailyTarget)} Trđ (${row.progressCP.toFixed(1)}%) => ${remains}\n`;
        });
        
        document.getElementById('directiveTemplateArea').innerText = txt;
    }

    window.copyTemplate = () => {
        const text = document.getElementById('directiveTemplateArea').innerText;
        navigator.clipboard.writeText(text).then(() => {
            alert('Đã copy mẫu lệnh! Dán (Ctrl+V) vào Gapo/Zalo ngay.');
        });
    }

    // 6. BIỂU ĐỒ & GHI NHẬN
    window.recordCheckpoint = () => {
        const now = new Date();
        const time = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
        const totalGN = computedData.reduce((sum, row) => sum + (Number(row.currentGN) || 0), 0);
        const totalMT = computedData.reduce((sum, row) => sum + row.newDailyTarget, 0);
        const progress = totalMT ? (totalGN / totalMT * 100).toFixed(1) : 0;

        checkpointLogs.push({ times: time, amount: totalGN, progress: progress });
        
        document.getElementById('checkpointLogBody').innerHTML = checkpointLogs.map((log, i) => `
            <tr class="border-b border-slate-200 text-slate-700">
                <td class="py-1 px-1 font-medium">${i + 1}</td>
                <td class="py-1 px-1 text-teal-600 font-mono text-xs">${log.times}</td>
                <td class="py-1 px-1 text-right text-amber-600 font-bold">${formatTrd(log.amount)}</td>
                <td class="py-1 px-1 text-right font-bold text-slate-500">${log.progress}%</td>
            </tr>
        `).join('');
        
        if (checkpointChart) {
            checkpointChart.data.labels.push(time);
            checkpointChart.data.datasets[0].data.push(totalGN);
            checkpointChart.update();
        }
        updateTemplate(); // Update text with new check count
    }

    const initChart = () => {
        const ctx = document.getElementById('checkpointChart').getContext('2d');
        checkpointChart = new Chart(ctx, {
            type: 'line',
            data: { labels: [], datasets: [{ label: 'Giải Ngân', data: [], borderColor: '#0d9488', backgroundColor: 'rgba(20, 184, 166, 0.1)', borderWidth: 2, pointBackgroundColor: '#f59e0b', fill: true, tension: 0.3 }] },
            options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { x: { grid: { color: '#e2e8f0' }, ticks: { color: '#64748b', font: {size: 10} } }, y: { grid: { color: '#e2e8f0' }, ticks: { color: '#64748b', font: {size: 10} }, beginAtZero: true } } }
        });
    }

    // INIT
    initChart();
    calculateData();
    renderAll();

</script>
</body>
</html># Index.html
