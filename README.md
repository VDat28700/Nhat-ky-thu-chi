[index.html](https://github.com/user-attachments/files/34846903/index.html)<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nhật ký thu chi</title>
    <!-- Tải Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Tải Google Fonts: Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background-color: #fafafa;
            color: #111827;
        }
        /* Tùy chỉnh radio button tròn đen tối giản giống ảnh mẫu */
        .custom-radio {
            appearance: none;
            -webkit-appearance: none;
            width: 18px;
            height: 18px;
            border: 1.5px solid #d1d5db;
            border-radius: 50%;
            outline: none;
            cursor: pointer;
            position: relative;
            transition: all 0.15s ease-in-out;
            background-color: #ffffff;
            flex-shrink: 0;
        }
        .custom-radio:hover {
            border-color: #9ca3af;
        }
        .custom-radio:checked {
            border-color: #000000;
            background-color: #000000;
        }
        .custom-radio:checked::after {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 6px;
            height: 6px;
            border-radius: 50%;
            background-color: #ffffff;
        }
        /* Ẩn nút tăng giảm mặc định của input number */
        input[type=number]::-webkit-inner-spin-button, 
        input[type=number]::-webkit-outer-spin-button { 
            -webkit-appearance: none; 
            margin: 0; 
        }
        input[type=number] {
            -moz-appearance: textfield;
        }
    </style>
</head>
<body class="min-h-screen py-6 px-4 sm:px-6 flex justify-center items-start">

    <!-- Container chính -->
    <main class="w-full max-w-[540px]">
        <!-- Tiêu đề chính & Nút tiện ích nhỏ -->
        <div class="flex items-center justify-between mb-4 px-1">
            <h1 class="text-xl font-bold tracking-tight text-gray-900">Nhật ký thu chi</h1>
            <div class="flex items-center gap-2">
                <button type="button" onclick="copyDailySummary()" title="Sao chép báo cáo ngày" class="text-xs font-medium text-gray-500 hover:text-gray-900 bg-white border border-gray-200 hover:border-gray-300 px-2.5 py-1.5 rounded-full transition flex items-center gap-1.5 shadow-2xs">
                    <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg>
                    <span>Sao chép</span>
                </button>
                <button type="button" onclick="openResetModal()" title="Đặt lại dữ liệu mẫu" class="text-xs font-medium text-gray-400 hover:text-red-600 bg-white border border-gray-200 hover:border-red-200 px-2.5 py-1.5 rounded-full transition">
                    Đặt lại
                </button>
            </div>
        </div>

        <!-- Khung thẻ (Card) chính -->
        <div class="bg-white rounded-3xl border border-gray-200/90 shadow-xs p-5 sm:p-6 space-y-5">
            
            <!-- 1. Phần Ngày & Số dư hiện có -->
            <section class="space-y-4">
                <!-- Chọn ngày -->
                <div>
                    <label for="entryDate" class="block text-sm font-semibold text-gray-900 mb-1.5">Ngày</label>
                    <div class="relative">
                        <input type="date" id="entryDate" 
                            class="w-full h-11 px-4 rounded-full border border-gray-200 bg-white text-sm text-gray-800 font-medium focus:outline-none focus:border-gray-900 focus:ring-1 focus:ring-gray-900 transition cursor-pointer">
                    </div>
                </div>

                <!-- 2 cột Tiền mặt & Ngân hàng -->
                <div class="grid grid-cols-2 gap-3.5">
                    <div>
                        <label for="cashBalance" class="block text-sm font-semibold text-gray-900 mb-1.5">Tiền mặt</label>
                        <div class="relative">
                            <input type="text" inputmode="numeric" id="cashBalance" placeholder="0"
                                class="w-full h-11 px-4 pr-8 rounded-full border border-gray-200 bg-white text-sm font-medium text-gray-900 focus:outline-none focus:border-gray-900 focus:ring-1 focus:ring-gray-900 transition">
                            <span class="absolute right-3.5 top-1/2 -translate-y-1/2 text-xs text-gray-400 pointer-events-none">đ</span>
                        </div>
                    </div>
                    <div>
                        <label for="bankBalance" class="block text-sm font-semibold text-gray-900 mb-1.5">Ngân hàng</label>
                        <div class="relative">
                            <input type="text" inputmode="numeric" id="bankBalance" placeholder="0"
                                class="w-full h-11 px-4 pr-8 rounded-full border border-gray-200 bg-white text-sm font-medium text-gray-900 focus:outline-none focus:border-gray-900 focus:ring-1 focus:ring-gray-900 transition">
                            <span class="absolute right-3.5 top-1/2 -translate-y-1/2 text-xs text-gray-400 pointer-events-none">đ</span>
                        </div>
                    </div>
                </div>
            </section>

            <hr class="border-gray-100">

            <!-- 2. Phần Thu -->
            <section class="space-y-3.5">
                <div>
                    <label for="incomeAmount" class="block text-sm font-semibold text-gray-900 mb-1.5">Thu</label>
                    <input type="text" inputmode="numeric" id="incomeAmount" placeholder="Số tiền thu"
                        class="w-full h-11 px-4 rounded-full border border-gray-200 bg-white text-sm text-gray-900 placeholder-gray-400 focus:outline-none focus:border-gray-900 focus:ring-1 focus:ring-gray-900 transition">
                </div>

                <!-- Nguồn thu -->
                <div>
                    <span class="block text-sm font-semibold text-gray-900 mb-2">Nguồn thu</span>
                    <div class="flex flex-wrap items-center gap-x-5 gap-y-2.5">
                        <label class="inline-flex items-center gap-2 cursor-pointer select-none">
                            <input type="radio" name="incomeSource" value="PM (Pullman)" class="custom-radio" checked onchange="toggleCustomSource()">
                            <span class="text-sm text-gray-700">PM (Pullman)</span>
                        </label>
                        <label class="inline-flex items-center gap-2 cursor-pointer select-none">
                            <input type="radio" name="incomeSource" value="PL (Phúc Long)" class="custom-radio" onchange="toggleCustomSource()">
                            <span class="text-sm text-gray-700">PL (Phúc Long)</span>
                        </label>
                        <label class="inline-flex items-center gap-2 cursor-pointer select-none">
                            <input type="radio" name="incomeSource" value="Khác" class="custom-radio" onchange="toggleCustomSource()">
                            <span class="text-sm text-gray-700">Khác</span>
                        </label>
                    </div>
                    <!-- Ô nhập ghi chú nguồn thu khi chọn "Khác" -->
                    <div id="customSourceContainer" class="hidden mt-2.5">
                        <input type="text" id="customSourceNote" placeholder="Nhập nguồn thu khác (VD: Lương, Thưởng, Bán đồ...)"
                            class="w-full h-10 px-4 rounded-full border border-gray-200 bg-gray-50/70 text-sm text-gray-900 placeholder-gray-400 focus:bg-white focus:outline-none focus:border-gray-900 transition">
                    </div>
                </div>

                <!-- Hình thức nhận / Thu vào -->
                <div>
                    <span class="block text-sm font-semibold text-gray-900 mb-2">Thu vào</span>
                    <div class="flex flex-wrap items-center gap-x-6 gap-y-2">
                        <label class="inline-flex items-center gap-2 cursor-pointer select-none">
                            <input type="radio" name="incomeMethod" value="Tiền mặt" class="custom-radio" checked>
                            <span class="text-sm text-gray-700">Tiền mặt</span>
                        </label>
                        <label class="inline-flex items-center gap-2 cursor-pointer select-none">
                            <input type="radio" name="incomeMethod" value="Ngân hàng" class="custom-radio">
                            <span class="text-sm text-gray-700">Ngân hàng</span>
                        </label>
                    </div>
                </div>
            </section>

            <hr class="border-gray-100">

            <!-- 3. Phần Chi -->
            <section class="space-y-3.5">
                <div>
                    <label for="expenseAmount" class="block text-sm font-semibold text-gray-900 mb-1.5">Chi</label>
                    <input type="text" inputmode="numeric" id="expenseAmount" placeholder="Số tiền chi"
                        class="w-full h-11 px-4 rounded-full border border-gray-200 bg-white text-sm text-gray-900 placeholder-gray-400 focus:outline-none focus:border-gray-900 focus:ring-1 focus:ring-gray-900 transition">
                </div>

                <!-- Hình thức chi / Chi từ -->
                <div>
                    <span class="block text-sm font-semibold text-gray-900 mb-2">Chi từ</span>
                    <div class="flex flex-wrap items-center gap-x-6 gap-y-2">
                        <label class="inline-flex items-center gap-2 cursor-pointer select-none">
                            <input type="radio" name="expenseMethod" value="Tiền mặt" class="custom-radio" checked>
                            <span class="text-sm text-gray-700">Tiền mặt</span>
                        </label>
                        <label class="inline-flex items-center gap-2 cursor-pointer select-none">
                            <input type="radio" name="expenseMethod" value="Ngân hàng" class="custom-radio">
                            <span class="text-sm text-gray-700">Ngân hàng</span>
                        </label>
                    </div>
                </div>

                <!-- Nội dung chi -->
                <div>
                    <label for="expenseNote" class="block text-sm font-semibold text-gray-900 mb-1.5">Nội dung chi</label>
                    <textarea id="expenseNote" rows="2" placeholder="Chi tiêu cho việc gì?"
                        class="w-full p-3.5 rounded-2xl border border-gray-200 bg-white text-sm text-gray-900 placeholder-gray-400 focus:outline-none focus:border-gray-900 focus:ring-1 focus:ring-gray-900 transition resize-none"></textarea>
                </div>

                <!-- Nút lưu màu đen bo tròn toàn chiều rộng -->
                <div class="pt-1">
                    <button type="button" onclick="saveEntry()"
                        class="w-full h-11 bg-black hover:bg-neutral-800 active:scale-[0.99] text-white font-semibold text-sm rounded-full transition flex items-center justify-center gap-2 shadow-xs cursor-pointer">
                        <span>Ghi nhận thu chi</span>
                    </button>
                </div>
            </section>

            <hr class="border-gray-100">

            <!-- 4. Tổng kết trong ngày -->
            <section class="space-y-2.5">
                <h2 class="text-sm font-semibold text-gray-900">Tổng kết trong ngày</h2>
                <div class="space-y-2 text-sm">
                    <div class="flex items-center justify-between">
                        <span class="text-gray-500">Tổng thu</span>
                        <span id="summaryIncome" class="font-semibold text-gray-900">0đ</span>
                    </div>
                    <div class="flex items-center justify-between">
                        <span class="text-gray-500">Tổng chi</span>
                        <span id="summaryExpense" class="font-semibold text-gray-900">0đ</span>
                    </div>
                    <div class="flex items-center justify-between pt-1">
                        <span class="font-semibold text-gray-900">Chênh lệch thu - chi</span>
                        <span id="summaryDiff" class="text-base font-bold text-emerald-600">0đ</span>
                    </div>
                </div>
            </section>

            <hr class="border-gray-100">

            <!-- 5. Các giao dịch đã ghi -->
            <section class="space-y-3">
                <div class="flex items-center justify-between">
                    <h2 class="text-sm font-semibold text-gray-900">Các giao dịch đã ghi</h2>
                    <span id="transactionCount" class="text-xs text-gray-400">0 giao dịch</span>
                </div>

                <!-- Danh sách giao dịch -->
                <div id="transactionsList" class="space-y-2.5">
                    <!-- Nội dung được render bằng JS -->
                </div>
            </section>
        </div>
    </main>

    <!-- Thông báo nổi (Toast Notification) -->
    <div id="toast" class="fixed bottom-5 left-1/2 -translate-x-1/2 z-50 pointer-events-none opacity-0 transition-all duration-200 translate-y-2">
        <div id="toastMessage" class="bg-gray-900 text-white text-xs font-medium px-4 py-2.5 rounded-full shadow-lg">
            Đã lưu giao dịch
        </div>
    </div>

    <!-- Hộp thoại xác nhận đặt lại dữ liệu (thay thế confirm()) -->
    <div id="resetModal" class="fixed inset-0 z-50 hidden items-center justify-center bg-black/40 backdrop-blur-[1px] p-4">
        <div class="bg-white rounded-3xl max-w-sm w-full p-6 shadow-xl border border-gray-100 space-y-4">
            <h3 class="text-base font-bold text-gray-900">Đặt lại dữ liệu?</h3>
            <p class="text-sm text-gray-600 leading-relaxed">
                Bạn muốn khôi phục lại dữ liệu mẫu ban đầu hay xóa trắng toàn bộ sổ thu chi?
            </p>
            <div class="flex flex-col gap-2 pt-1">
                <button type="button" onclick="resetToSample()" class="w-full py-2.5 px-4 bg-black hover:bg-neutral-800 text-white text-sm font-medium rounded-full transition">
                    Khôi phục dữ liệu mẫu
                </button>
                <button type="button" onclick="clearAllData()" class="w-full py-2.5 px-4 bg-red-50 hover:bg-red-100 text-red-600 text-sm font-medium rounded-full transition">
                    Xóa trắng tất cả
                </button>
                <button type="button" onclick="closeResetModal()" class="w-full py-2 px-4 text-gray-500 hover:text-gray-800 text-sm font-medium rounded-full transition">
                    Hủy
                </button>
            </div>
        </div>
    </div>

    <script>
        // Khóa lưu trữ trong localStorage
        const STORAGE_KEY = 'nhat_ky_thu_chi_v1';

        // Lấy ngày hôm nay theo định dạng YYYY-MM-DD (giờ địa phương)
        function getTodayISO() {
            const now = new Date();
            const offset = now.getTimezoneOffset() * 60000;
            return new Date(now.getTime() - offset).toISOString().split('T')[0];
        }

        // Định dạng số tiền Việt Nam (VD: 200000 -> 200.000đ)
        function formatCurrency(amount) {
            const num = Number(amount) || 0;
            const sign = num < 0 ? '-' : '';
            return sign + Math.abs(num).toLocaleString('vi-VN') + 'đ';
        }

        // Định dạng số có dấu chấm phân cách hàng nghìn khi nhập liệu
        function formatNumberInput(val) {
            const digits = String(val).replace(/\D/g, '');
            if (!digits) return '';
            return Number(digits).toLocaleString('vi-VN');
        }

        // Chuyển chuỗi số đã định dạng về số nguyên
        function parseNumberInput(val) {
            const digits = String(val).replace(/\D/g, '');
            return digits ? parseInt(digits, 10) : 0;
        }

        // Dữ liệu mẫu ban đầu khớp với ảnh chụp màn hình
        function getDefaultState() {
            const today = getTodayISO();
            return {
                cashBalance: 200000,
                bankBalance: 719202,
                transactions: [
                    {
                        id: 'sample-1',
                        date: today,
                        type: 'income',
                        amount: 200000,
                        source: 'PM (Pullman)',
                        customNote: '',
                        method: 'Tiền mặt',
                        note: ''
                    },
                    {
                        id: 'sample-2',
                        date: today,
                        type: 'expense',
                        amount: 15000,
                        source: '',
                        customNote: '',
                        method: 'Tiền mặt',
                        note: 'Bánh mì sandwich'
                    }
                ]
            };
        }

        // Trạng thái ứng dụng
        let appState = loadState();

        function loadState() {
            try {
                const raw = localStorage.getItem(STORAGE_KEY);
                if (raw) {
                    const parsed = JSON.parse(raw);
                    if (parsed && Array.isArray(parsed.transactions)) {
                        return parsed;
                    }
                }
            } catch (e) {
                console.error('Lỗi đọc localStorage:', e);
            }
            const initial = getDefaultState();
            saveStateToStorage(initial);
            return initial;
        }

        function saveStateToStorage(state = appState) {
            try {
                localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
            } catch (e) {
                console.error('Lỗi lưu localStorage:', e);
            }
        }

        // Hiển thị thông báo ngắn (Toast)
        let toastTimer = null;
        function showToast(msg) {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toastMessage');
            toastMsg.textContent = msg;
            toast.classList.remove('opacity-0', 'translate-y-2');
            toast.classList.add('opacity-100', 'translate-y-0');

            clearTimeout(toastTimer);
            toastTimer = setTimeout(() => {
                toast.classList.remove('opacity-100', 'translate-y-0');
                toast.classList.add('opacity-0', 'translate-y-2');
            }, 2400);
        }

        // Ẩn/hiện ô nhập ghi chú nguồn thu khi chọn "Khác"
        function toggleCustomSource() {
            const selected = document.querySelector('input[name="incomeSource"]:checked')?.value;
            const container = document.getElementById('customSourceContainer');
            if (selected === 'Khác') {
                container.classList.remove('hidden');
                document.getElementById('customSourceNote').focus();
            } else {
                container.classList.add('hidden');
            }
        }

        // Gắn tự động format dấu chấm nghìn cho các ô nhập tiền
        function setupCurrencyInputs() {
            const moneyIds = ['cashBalance', 'bankBalance', 'incomeAmount', 'expenseAmount'];
            moneyIds.forEach(id => {
                const el = document.getElementById(id);
                el.addEventListener('input', (e) => {
                    const formatted = formatNumberInput(e.target.value);
                    e.target.value = formatted;

                    // Nếu người dùng sửa trực tiếp số dư Tiền mặt / Ngân hàng thì cập nhật & lưu ngay
                    if (id === 'cashBalance') {
                        appState.cashBalance = parseNumberInput(formatted);
                        saveStateToStorage();
                    } else if (id === 'bankBalance') {
                        appState.bankBalance = parseNumberInput(formatted);
                        saveStateToStorage();
                    }
                });
            });
        }

        // Cập nhật hiển thị số dư hiện có lên 2 ô input
        function renderBalances() {
            document.getElementById('cashBalance').value = appState.cashBalance ? appState.cashBalance.toLocaleString('vi-VN') : '0';
            document.getElementById('bankBalance').value = appState.bankBalance ? appState.bankBalance.toLocaleString('vi-VN') : '0';
        }

        // Lưu giao dịch mới (có thể nhập Thu, Chi, hoặc cả hai cùng lúc)
        function saveEntry() {
            const selectedDate = document.getElementById('entryDate').value || getTodayISO();
            const incomeVal = parseNumberInput(document.getElementById('incomeAmount').value);
            const expenseVal = parseNumberInput(document.getElementById('expenseAmount').value);

            if (incomeVal <= 0 && expenseVal <= 0) {
                showToast('Vui lòng nhập số tiền Thu hoặc Chi');
                document.getElementById('incomeAmount').focus();
                return;
            }

            let addedCount = 0;

            // 1. Xử lý khoản Thu (nếu có)
            if (incomeVal > 0) {
                const source = document.querySelector('input[name="incomeSource"]:checked')?.value || 'PM (Pullman)';
                const customNote = source === 'Khác' ? document.getElementById('customSourceNote').value.trim() : '';
                const method = document.querySelector('input[name="incomeMethod"]:checked')?.value || 'Tiền mặt';

                appState.transactions.unshift({
                    id: 'tx-' + Date.now() + '-in',
                    date: selectedDate,
                    type: 'income',
                    amount: incomeVal,
                    source: source,
                    customNote: customNote,
                    method: method,
                    note: ''
                });

                // Cộng vào số dư tương ứng
                if (method === 'Tiền mặt') {
                    appState.cashBalance += incomeVal;
                } else {
                    appState.bankBalance += incomeVal;
                }
                addedCount++;
            }

            // 2. Xử lý khoản Chi (nếu có)
            if (expenseVal > 0) {
                const method = document.querySelector('input[name="expenseMethod"]:checked')?.value || 'Tiền mặt';
                const note = document.getElementById('expenseNote').value.trim();

                appState.transactions.unshift({
                    id: 'tx-' + Date.now() + '-out',
                    date: selectedDate,
                    type: 'expense',
                    amount: expenseVal,
                    source: '',
                    customNote: '',
                    method: method,
                    note: note
                });

                // Trừ khỏi số dư tương ứng
                if (method === 'Tiền mặt') {
                    appState.cashBalance -= expenseVal;
                } else {
                    appState.bankBalance -= expenseVal;
                }
                addedCount++;
            }

            // Lưu và làm mới giao diện
            saveStateToStorage();
            renderBalances();
            renderDailyView();

            // Xóa trắng các ô nhập liệu giao dịch vừa ghi
            document.getElementById('incomeAmount').value = '';
            document.getElementById('customSourceNote').value = '';
            document.getElementById('expenseAmount').value = '';
            document.getElementById('expenseNote').value = '';

            showToast(addedCount > 1 ? 'Đã ghi nhận khoản thu và chi' : 'Đã lưu giao dịch');
        }

        // Xóa một giao dịch và hoàn tác lại số dư Tiền mặt / Ngân hàng
        function deleteTransaction(id) {
            const idx = appState.transactions.findIndex(t => t.id === id);
            if (idx === -1) return;

            const tx = appState.transactions[idx];
            if (tx.type === 'income') {
                if (tx.method === 'Tiền mặt') {
                    appState.cashBalance -= tx.amount;
                } else {
                    appState.bankBalance -= tx.amount;
                }
            } else if (tx.type === 'expense') {
                if (tx.method === 'Tiền mặt') {
                    appState.cashBalance += tx.amount;
                } else {
                    appState.bankBalance += tx.amount;
                }
            }

            appState.transactions.splice(idx, 1);
            saveStateToStorage();
            renderBalances();
            renderDailyView();
            showToast('Đã xóa giao dịch và hoàn tác số dư');
        }

        // Hiển thị tổng kết và danh sách giao dịch của ngày đang chọn
        function renderDailyView() {
            const selectedDate = document.getElementById('entryDate').value || getTodayISO();
            const dailyTx = appState.transactions.filter(t => t.date === selectedDate);

            let totalIncome = 0;
            let totalExpense = 0;

            dailyTx.forEach(t => {
                if (t.type === 'income') totalIncome += t.amount;
                if (t.type === 'expense') totalExpense += t.amount;
            });

            const diff = totalIncome - totalExpense;

            // Cập nhật phần Tổng kết trong ngày
            document.getElementById('summaryIncome').textContent = formatCurrency(totalIncome);
            document.getElementById('summaryExpense').textContent = formatCurrency(totalExpense);

            const diffEl = document.getElementById('summaryDiff');
            diffEl.textContent = formatCurrency(diff);
            if (diff >= 0) {
                diffEl.className = 'text-base font-bold text-emerald-600';
            } else {
                diffEl.className = 'text-base font-bold text-red-600';
            }

            // Cập nhật số lượng & danh sách giao dịch đã ghi
            document.getElementById('transactionCount').textContent = `${dailyTx.length} giao dịch`;
            const listContainer = document.getElementById('transactionsList');

            if (dailyTx.length === 0) {
                listContainer.innerHTML = `
                    <div class="bg-gray-50/80 border border-dashed border-gray-200 rounded-2xl p-5 text-center">
                        <p class="text-xs text-gray-400">Chưa có giao dịch nào được ghi trong ngày này.</p>
                    </div>
                `;
                return;
            }

            listContainer.innerHTML = dailyTx.map(tx => {
                if (tx.type === 'income') {
                    const sourceLabel = tx.source === 'Khác' && tx.customNote
                        ? `Khác (${escapeHTML(tx.customNote)})`
                        : escapeHTML(tx.source);
                    return `
                        <div class="group bg-gray-100/80 hover:bg-gray-100 rounded-2xl px-4 py-3 flex items-center justify-between gap-3 transition">
                            <div class="min-w-0">
                                <div class="flex flex-wrap items-center gap-1.5 text-sm font-semibold text-gray-900">
                                    <span class="text-emerald-700">Thu ${formatCurrency(tx.amount)}</span>
                                    <span class="text-gray-400 font-normal">•</span>
                                    <span class="text-gray-700 font-medium">${sourceLabel}</span>
                                    <span class="text-gray-400 font-normal">•</span>
                                    <span class="text-xs font-medium px-2 py-0.5 rounded-full bg-white text-gray-600 border border-gray-200/80">${escapeHTML(tx.method)}</span>
                                </div>
                            </div>
                            <button type="button" onclick="deleteTransaction('${tx.id}')" title="Xóa giao dịch"
                                class="p-1.5 text-gray-400 hover:text-red-600 hover:bg-white rounded-full transition shrink-0 cursor-pointer">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                            </button>
                        </div>
                    `;
                } else {
                    const desc = tx.note ? escapeHTML(tx.note) : 'Không có ghi chú';
                    return `
                        <div class="group bg-gray-100/80 hover:bg-gray-100 rounded-2xl px-4 py-3 flex items-center justify-between gap-3 transition">
                            <div class="min-w-0">
                                <div class="flex flex-wrap items-center gap-1.5 text-sm font-semibold text-gray-900">
                                    <span>Chi ${formatCurrency(tx.amount)}</span>
                                    <span class="text-gray-400 font-normal">•</span>
                                    <span class="text-xs font-medium px-2 py-0.5 rounded-full bg-white text-gray-600 border border-gray-200/80">${escapeHTML(tx.method)}</span>
                                </div>
                                <p class="text-xs text-gray-500 mt-0.5 truncate">${desc}</p>
                            </div>
                            <button type="button" onclick="deleteTransaction('${tx.id}')" title="Xóa giao dịch"
                                class="p-1.5 text-gray-400 hover:text-red-600 hover:bg-white rounded-full transition shrink-0 cursor-pointer">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                            </button>
                        </div>
                    `;
                }
            }).join('');
        }

        // Chống XSS khi hiển thị ghi chú người dùng nhập
        function escapeHTML(str) {
            return String(str)
                .replace(/&/g, '&amp;')
                .replace(/</g, '&lt;')
                .replace(/>/g, '&gt;')
                .replace(/"/g, '&quot;')
                .replace(/'/g, '&#039;');
        }

        // Sao chép tóm tắt thu chi trong ngày vào clipboard
        function copyDailySummary() {
            const selectedDate = document.getElementById('entryDate').value || getTodayISO();
            const [y, m, d] = selectedDate.split('-');
            const dateFormatted = `${d}/${m}/${y}`;
            const dailyTx = appState.transactions.filter(t => t.date === selectedDate);

            let totalIncome = 0;
            let totalExpense = 0;
            const lines = [];

            dailyTx.forEach(t => {
                if (t.type === 'income') {
                    totalIncome += t.amount;
                    const src = t.source === 'Khác' && t.customNote ? `Khác (${t.customNote})` : t.source;
                    lines.push(`+ Thu ${formatCurrency(t.amount)} [${src} - ${t.method}]`);
                } else {
                    totalExpense += t.amount;
                    lines.push(`- Chi ${formatCurrency(t.amount)} [${t.method}]${t.note ? ': ' + t.note : ''}`);
                }
            });

            const text = [
                `NHẬT KÝ THU CHI (${dateFormatted})`,
                `• Tiền mặt hiện có: ${formatCurrency(appState.cashBalance)}`,
                `• Ngân hàng hiện có: ${formatCurrency(appState.bankBalance)}`,
                `------------------------`,
                ...lines,
                `------------------------`,
                `Tổng thu: ${formatCurrency(totalIncome)}`,
                `Tổng chi: ${formatCurrency(totalExpense)}`,
                `Chênh lệch: ${formatCurrency(totalIncome - totalExpense)}`
            ].join('\n');

            const textarea = document.createElement('textarea');
            textarea.value = text;
            document.body.appendChild(textarea);
            textarea.select();
            try {
                document.execCommand('copy');
                showToast('Đã sao chép báo cáo ngày');
            } catch (err) {
                showToast('Không thể sao chép');
            }
            document.body.removeChild(textarea);
        }

        // Điều khiển modal đặt lại dữ liệu
        function openResetModal() {
            const modal = document.getElementById('resetModal');
            modal.classList.remove('hidden');
            modal.classList.add('flex');
        }

        function closeResetModal() {
            const modal = document.getElementById('resetModal');
            modal.classList.add('hidden');
            modal.classList.remove('flex');
        }

        function resetToSample() {
            appState = getDefaultState();
            document.getElementById('entryDate').value = getTodayISO();
            saveStateToStorage();
            renderBalances();
            renderDailyView();
            closeResetModal();
            showToast('Đã khôi phục dữ liệu mẫu');
        }

        function clearAllData() {
            appState = {
                cashBalance: 0,
                bankBalance: 0,
                transactions: []
            };
            saveStateToStorage();
            renderBalances();
            renderDailyView();
            closeResetModal();
            showToast('Đã xóa trắng toàn bộ dữ liệu');
        }

        // Khởi tạo khi tải trang
        window.addEventListener('DOMContentLoaded', () => {
            const dateInput = document.getElementById('entryDate');
            dateInput.value = getTodayISO();
            dateInput.addEventListener('change', renderDailyView);

            setupCurrencyInputs();
            renderBalances();
            renderDailyView();
        });
    </script>
</body>
</html>
