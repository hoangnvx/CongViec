<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
  <title>Lịch Việc Pro</title>

  <!-- PWA Standalone cho iPhone -->
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="apple-mobile-web-app-title" content="Lịch Việc">

  <!-- ICON LỊCH CHO IPHONE & BROWSER -->
  <link rel="apple-touch-icon" id="appleTouchIcon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 180 180' width='180' height='180'%3E%3Crect width='180' height='180' fill='%23F5EBE0'/%3E%3Crect x='34' y='44' width='112' height='98' rx='18' fill='%235C3D2E'/%3E%3C/svg%3E">
  <link rel="icon" id="faviconIcon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 180 180' width='180' height='180'%3E%3Crect width='180' height='180' fill='%23F5EBE0'/%3E%3Crect x='34' y='44' width='112' height='98' rx='18' fill='%235C3D2E'/%3E%3C/svg%3E">

  <script>
    (function() {
      try {
        const c = document.createElement('canvas');
        c.width = 180; c.height = 180;
        const ctx = c.getContext('2d');
        ctx.fillStyle = '#F5EBE0';
        ctx.fillRect(0, 0, 180, 180);
        ctx.fillStyle = '#5C3D2E';
        const x = 34, y = 44, w = 112, h = 98, r = 18;
        ctx.beginPath();
        ctx.moveTo(x + r, y); ctx.lineTo(x + w - r, y);
        ctx.quadraticCurveTo(x + w, y, x + w, y + r);
        ctx.lineTo(x + w, y + h - r);
        ctx.quadraticCurveTo(x + w, y + h, x + w - r, y + h);
        ctx.lineTo(x + r, y + h);
        ctx.quadraticCurveTo(x, y + h, x, y + h - r);
        ctx.lineTo(x, y + r);
        ctx.quadraticCurveTo(x, y, x + r, y);
        ctx.closePath();
        ctx.fill();
        ctx.fillStyle = '#DDB892';
        ctx.fillRect(52, 30, 12, 24);
        ctx.fillRect(116, 30, 12, 24);
        ctx.strokeStyle = '#F5EBE0'; ctx.lineWidth = 4;
        ctx.beginPath(); ctx.moveTo(34, 72); ctx.lineTo(146, 72); ctx.stroke();
        ctx.fillStyle = '#F5EBE0';
        [[56,94],[90,94],[124,94],[56,120],[90,120],[124,120]].forEach(([dx, dy]) => {
          ctx.beginPath(); ctx.arc(dx, dy, 6, 0, Math.PI * 2); ctx.fill();
        });
        const pngUrl = c.toDataURL('image/png');
        document.getElementById('appleTouchIcon').href = pngUrl;
        document.getElementById('faviconIcon').href = pngUrl;
      } catch(e) {}
    })();
  </script>

  <!-- Tailwind CSS & Lucide Icons -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest"></script>

  <style>
    body {
      -webkit-tap-highlight-color: transparent;
      user-select: none;
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", Roboto, sans-serif;
      padding-top: env(safe-area-inset-top);
    }
    .hide-scrollbar::-webkit-scrollbar { display: none; }
    .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    .safe-bottom { padding-bottom: calc(env(safe-area-inset-bottom) + 85px); }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen">

  <div class="max-w-md mx-auto w-full min-h-screen flex flex-col safe-bottom">
    
    <!-- TOP HEADER (YÊU CẦU 3: CÓ NÚT RELOAD BÊN TRÁI) -->
    <header class="px-5 pt-4 pb-3 flex justify-between items-center bg-slate-50 sticky top-0 z-20">
      <div class="flex items-center gap-3">
        <button onclick="handleReloadPage()" title="Tải lại trang" class="w-10 h-10 bg-white border border-slate-200/90 rounded-2xl flex items-center justify-center text-slate-600 shadow-sm active:scale-90 transition hover:bg-slate-100">
          <i data-lucide="rotate-cw" id="reloadBtnIcon" class="w-4 h-4"></i>
        </button>
        <div>
          <p id="topSubtitle" class="text-[11px] font-bold tracking-widest text-slate-400 uppercase">LỊCH LÀM VIỆC</p>
          <h1 id="topTitle" class="text-2xl font-extrabold text-slate-900 tracking-tight">Hôm nay</h1>
        </div>
      </div>
      <button onclick="openModalForCreate()" class="w-10 h-10 bg-slate-900 text-white rounded-full flex items-center justify-center shadow-md active:scale-95 transition">
        <i data-lucide="plus" class="w-5 h-5"></i>
      </button>
    </header>

    <!-- ================= TAB 1: LỊCH THÁNG ================= -->
    <section id="tabCalendar" class="px-5 space-y-4">
      <div class="flex items-center justify-between bg-white p-2.5 rounded-2xl border border-slate-200/80 shadow-sm">
        <button onclick="changeMonth(-1)" class="p-2 rounded-xl text-slate-600 hover:bg-slate-100 active:scale-95 transition">
          <i data-lucide="chevron-left" class="w-5 h-5"></i>
        </button>
        <span id="calendarMonthLabel" class="font-bold text-sm text-slate-900">-- / ----</span>
        <button onclick="changeMonth(1)" class="p-2 rounded-xl text-slate-600 hover:bg-slate-100 active:scale-95 transition">
          <i data-lucide="chevron-right" class="w-5 h-5"></i>
        </button>
      </div>

      <div class="bg-white p-4 rounded-3xl border border-slate-200/80 shadow-sm">
        <div class="grid grid-cols-7 gap-1 text-center text-[11px] font-bold text-slate-400 mb-2">
          <span>CN</span><span>T2</span><span>T3</span><span>T4</span><span>T5</span><span>T6</span><span>T7</span>
        </div>
        <div id="monthGrid" class="grid grid-cols-7 gap-y-2 gap-x-1 text-center"></div>
      </div>

      <div>
        <div class="flex justify-between items-center mb-2 px-1">
          <span id="selectedDayLabel" class="text-xs font-bold text-slate-400 uppercase">CÔNG VIỆC TRONG NGÀY</span>
          <span id="selectedDayJobCount" class="text-xs font-semibold text-slate-500">0 việc</span>
        </div>
        <div id="dayJobList" class="space-y-3"></div>
      </div>
    </section>

    <!-- ================= TAB 2: DẠNG DANH SÁCH & LƯU TRỮ ================= -->
    <section id="tabList" class="px-5 space-y-3 hidden">
      <div class="flex bg-slate-200/70 p-1 rounded-2xl text-xs font-bold">
        <button id="subTabActiveBtn" onclick="switchListSubTab(false)" class="flex-1 py-2 rounded-xl bg-white text-slate-900 shadow-sm transition">Đang làm (<span id="activeJobCount">0</span>)</button>
        <button id="subTabArchiveBtn" onclick="switchListSubTab(true)" class="flex-1 py-2 rounded-xl text-slate-500 transition">Kho lưu trữ (<span id="archivedJobCount">0</span>)</button>
      </div>

      <div class="relative">
        <i data-lucide="search" class="w-4 h-4 absolute left-3.5 top-3.5 text-slate-400"></i>
        <input id="searchInput" oninput="renderAllJobsList()" type="text" placeholder="Tìm theo tên việc, ghi chú..." class="w-full bg-white border border-slate-200 rounded-2xl pl-10 pr-4 py-2.5 text-xs outline-none shadow-sm focus:border-slate-800">
      </div>

      <div id="listSourceFilter" class="flex gap-2 overflow-x-auto hide-scrollbar py-1"></div>
      <div id="allJobsContainer" class="space-y-3 pt-1"></div>
    </section>

    <!-- ================= TAB 3: BÁO CÁO DOANH THU (YÊU CẦU 4 ĐÃ TÁCH BIỆT) ================= -->
    <section id="tabStats" class="px-5 space-y-4 hidden">
      <!-- 1. TỔNG THU NHẬP ĐỘC LẬP TÁCH BIỆT -->
      <div class="bg-slate-900 text-white p-5 rounded-3xl shadow-lg shadow-slate-900/10">
        <p class="text-[11px] font-bold tracking-widest text-slate-400 uppercase">TỔNG THU NHẬP (TỔNG HỢP ĐỒNG)</p>
        <p id="statTotalIncome" class="text-3xl font-black mt-1 text-white">0 ₫</p>
      </div>

      <!-- 2. BA MỤC TÀI CHÍNH CHI TIẾT RIÊNG BIỆT: THU ĐỦ, CỌC, CÔNG NỢ -->
      <div class="grid grid-cols-3 gap-2.5">
        <div class="bg-white p-3.5 rounded-2xl border border-slate-200/70 shadow-sm">
          <p class="text-[10px] font-bold text-emerald-600 uppercase">Đã thu đủ</p>
          <p id="statPaidFull" class="font-black text-slate-900 text-xs sm:text-sm mt-1">0 ₫</p>
        </div>
        <div class="bg-white p-3.5 rounded-2xl border border-slate-200/70 shadow-sm">
          <p class="text-[10px] font-bold text-amber-500 uppercase">Cọc đã nhận</p>
          <p id="statTotalDeposit" class="font-black text-slate-900 text-xs sm:text-sm mt-1">0 ₫</p>
        </div>
        <div class="bg-white p-3.5 rounded-2xl border border-slate-200/70 shadow-sm">
          <p class="text-[10px] font-bold text-rose-500 uppercase">Nợ còn thiếu</p>
          <p id="statDebt" class="font-black text-rose-600 text-xs sm:text-sm mt-1">0 ₫</p>
        </div>
      </div>

      <!-- TỔNG SỐ LƯỢNG VIỆC -->
      <div class="grid grid-cols-2 gap-3">
        <div class="bg-white p-4 rounded-2xl border border-slate-100 shadow-sm">
          <p class="text-xs font-medium text-slate-400">Tổng Jobs</p>
          <p id="statTotalJobs" class="text-2xl font-bold text-slate-900 mt-0.5">0</p>
        </div>
        <div class="bg-white p-4 rounded-2xl border border-slate-100 shadow-sm">
          <p class="text-xs font-medium text-slate-400">Đã Hoàn Thành</p>
          <p id="statCompletedJobs" class="text-2xl font-bold text-slate-800 mt-0.5">0</p>
        </div>
      </div>

      <!-- 3. BÁO CÁO CÔNG VIỆC HOÀN THÀNH & TIỀN THEO HẠNG MỤC (YÊU CẦU 4) -->
      <div class="bg-white p-5 rounded-3xl border border-slate-100 shadow-sm">
        <p class="text-xs font-bold text-slate-400 tracking-wider uppercase mb-3">BÁO CÁO THEO HẠNG MỤC</p>
        <div id="statCategoryBreakdown" class="space-y-3 text-xs"></div>
      </div>

      <!-- TỔNG KẾT THEO NGUỒN -->
      <div class="bg-white p-5 rounded-3xl border border-slate-100 shadow-sm">
        <p class="text-xs font-bold text-slate-400 tracking-wider uppercase mb-3">TỔNG KẾT THEO NGUỒN</p>
        <div id="statSourceBreakdown" class="space-y-2 text-xs"></div>
      </div>
    </section>

    <!-- ================= TAB 4: CÀI ĐẶT & QUYỀN QUẢN TRỊ DATABASE ================= -->
    <section id="tabSettings" class="px-5 space-y-4 hidden">
      
      <!-- YÊU CẦU 1: TRUY CẬP DATABASE QUYỀN QUẢN TRỊ (MẬT KHẨU 1998) -->
      <div class="bg-white p-5 rounded-3xl border border-slate-200/80 shadow-sm space-y-3">
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-2">
            <i data-lucide="database" class="w-5 h-5 text-slate-800"></i>
            <h3 class="font-bold text-slate-900 text-base">Quản Trị Cơ Sở Dữ Liệu</h3>
          </div>
          <span id="adminStatusBadge" class="text-[10px] font-bold px-2 py-0.5 rounded-full bg-slate-100 text-slate-500">Đang khóa</span>
        </div>
        <p class="text-xs text-slate-400">Truy cập IndexedDB trực tiếp để thêm, sửa, xóa bản ghi gốc.</p>

        <!-- Khung nhập mật khẩu -->
        <div id="adminLockBox" class="space-y-2 pt-1">
          <div class="flex gap-2">
            <input id="adminPassInput" type="password" placeholder="Nhập mật khẩu (1998)..." class="flex-1 bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-xs outline-none focus:border-slate-800 font-bold">
            <button onclick="loginAdminDatabase()" class="bg-slate-900 text-white px-4 py-2 rounded-xl text-xs font-bold active:scale-95 transition">Vào Quản Trị</button>
          </div>
          <p id="adminPassError" class="text-[10px] text-rose-500 font-semibold hidden">Mật khẩu không đúng! Vui lòng thử lại.</p>
        </div>

        <!-- Khung quản trị khi đã mở khóa -->
        <div id="adminPanelBox" class="hidden space-y-3 pt-2 border-t border-slate-100">
          <div class="flex items-center justify-between bg-emerald-50 text-emerald-800 px-3 py-2 rounded-xl text-xs font-bold">
            <span>✓ Đã xác thực Quản Trị Viên</span>
            <button onclick="logoutAdminDatabase()" class="text-rose-600 underline text-[11px]">Đăng xuất</button>
          </div>

          <div class="flex gap-2">
            <button onclick="openModalForCreate()" class="flex-1 bg-slate-900 text-white py-2 rounded-xl text-xs font-bold active:scale-95 transition">+ Thêm bản ghi mới</button>
            <button onclick="adminClearEntireDB()" class="px-3 bg-rose-50 text-rose-600 border border-rose-200 py-2 rounded-xl text-xs font-bold active:scale-95 transition">Xóa trắng DB</button>
          </div>

          <div class="text-[11px] font-bold text-slate-400 uppercase pt-1">Dữ liệu gốc IndexedDB (<span id="adminRecordCount">0</span> bản ghi)</div>
          <div id="adminRecordList" class="space-y-2 max-h-80 overflow-y-auto hide-scrollbar pr-1"></div>
        </div>
      </div>

      <!-- YÊU CẦU 2: QUẢN LÝ THỂ LOẠI (CHỌN MÀU NHẬN DIỆN RIÊNG) -->
      <div class="bg-white p-5 rounded-3xl border border-slate-100 shadow-sm space-y-3">
        <div>
          <h3 class="font-bold text-slate-900 text-base">Quản lý Hạng mục & Màu</h3>
          <p class="text-xs text-slate-400 mt-0.5">Màu sẽ được đồng bộ trên Lịch tháng và Danh sách</p>
        </div>
        <div class="space-y-2">
          <div class="flex gap-2">
            <input id="inputNewCategory" type="text" placeholder="Tên hạng mục mới..." class="flex-1 bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-xs outline-none focus:border-slate-800 font-semibold">
            <button onclick="addNewCategory()" class="bg-slate-900 text-white px-4 py-2 rounded-xl text-xs font-bold active:scale-95 transition">Thêm</button>
          </div>
          <div class="flex items-center gap-2 pt-1">
            <span class="text-[11px] text-slate-400 font-semibold">Chọn màu:</span>
            <div id="categoryColorPicker" class="flex gap-2"></div>
          </div>
        </div>
        <div id="settingsCategoryList" class="space-y-2 pt-2 border-t border-slate-100"></div>
      </div>

      <!-- QUẢN LÝ NGUỒN VIỆC & MÀU -->
      <div class="bg-white p-5 rounded-3xl border border-slate-100 shadow-sm space-y-3">
        <div>
          <h3 class="font-bold text-slate-900 text-base">Nguồn việc & Màu nhận diện</h3>
          <p class="text-xs text-slate-400 mt-0.5">Tùy biến nguồn khách hàng</p>
        </div>
        <div class="space-y-2">
          <div class="flex gap-2">
            <input id="inputNewSource" type="text" placeholder="Tên nguồn mới..." class="flex-1 bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-xs outline-none focus:border-slate-800 font-semibold">
            <button onclick="addNewSource()" class="bg-slate-900 text-white px-4 py-2 rounded-xl text-xs font-bold active:scale-95 transition">Thêm</button>
          </div>
          <div class="flex items-center gap-2 pt-1">
            <span class="text-[11px] text-slate-400 font-semibold">Chọn màu:</span>
            <div id="sourceColorPicker" class="flex gap-2"></div>
          </div>
        </div>
        <div id="settingsSourceList" class="space-y-2 pt-2 border-t border-slate-100"></div>
      </div>

      <!-- SAO LƯU FILE DỰ PHÒNG -->
      <div class="bg-white p-5 rounded-3xl border border-slate-100 shadow-sm space-y-3">
        <h3 class="font-bold text-slate-900 text-base">Sao lưu & Khôi phục</h3>
        <div class="grid grid-cols-2 gap-2 pt-1">
          <button onclick="exportDataBackup()" class="flex items-center justify-center gap-1.5 bg-slate-100 text-slate-700 py-2.5 rounded-xl text-xs font-bold active:scale-95 transition">
            <i data-lucide="download" class="w-4 h-4"></i>
            <span>Xuất JSON</span>
          </button>
          <label class="flex items-center justify-center gap-1.5 bg-slate-100 text-slate-700 py-2.5 rounded-xl text-xs font-bold cursor-pointer active:scale-95 transition">
            <i data-lucide="upload" class="w-4 h-4"></i>
            <span>Nhập khôi phục</span>
            <input type="file" id="importFileInput" onchange="importDataBackup(event)" accept=".json" class="hidden">
          </label>
        </div>
      </div>
    </section>

    <!-- ================= BOTTOM NAVIGATION MENU ================= -->
    <nav class="fixed bottom-0 left-0 right-0 bg-white/95 backdrop-blur-md border-t border-slate-200/80 z-30 px-6 py-2.5 flex justify-between items-center max-w-md mx-auto">
      <button onclick="navigateTab('calendar')" id="navCalendar" class="flex flex-col items-center gap-1 text-slate-900">
        <i data-lucide="calendar" class="w-5 h-5"></i>
        <span class="text-[10px] font-bold">Lịch tháng</span>
      </button>
      <button onclick="navigateTab('list')" id="navList" class="flex flex-col items-center gap-1 text-slate-400">
        <i data-lucide="list" class="w-5 h-5"></i>
        <span class="text-[10px] font-semibold">Danh sách</span>
      </button>
      <button onclick="navigateTab('stats')" id="navStats" class="flex flex-col items-center gap-1 text-slate-400">
        <i data-lucide="pie-chart" class="w-5 h-5"></i>
        <span class="text-[10px] font-semibold">Báo cáo</span>
      </button>
      <button onclick="navigateTab('settings')" id="navSettings" class="flex flex-col items-center gap-1 text-slate-400">
        <i data-lucide="settings" class="w-5 h-5"></i>
        <span class="text-[10px] font-semibold">Cài đặt</span>
      </button>
    </nav>
  </div>

  <!-- ================= MODAL: THÊM & SỬA CÔNG VIỆC ================= -->
  <div id="jobModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 hidden flex items-end justify-center">
    <div class="bg-white w-full max-w-md rounded-t-3xl p-6 pb-10 space-y-3 animate-in slide-in-from-bottom duration-200 max-h-[92vh] overflow-y-auto">
      <div class="flex justify-between items-center pb-2 border-b border-slate-100">
        <h3 id="modalTitle" class="font-bold text-base text-slate-900">Thêm Lịch Làm Việc</h3>
        <button onclick="closeModal()" class="text-slate-400"><i data-lucide="x" class="w-5 h-5"></i></button>
      </div>

      <input type="hidden" id="editingJobId" value="">

      <div>
        <label class="text-[10px] font-bold text-slate-400 uppercase">Tên công việc</label>
        <input id="inputTitle" type="text" placeholder="vd: Đám cưới Tuấn & Thảo" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2.5 text-xs outline-none focus:border-slate-800 mt-1 font-semibold">
      </div>

      <div class="grid grid-cols-2 gap-2">
        <div>
          <label class="text-[10px] font-bold text-slate-400 uppercase">Hạng mục (Thể loại)</label>
          <select id="modalCategorySelect" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-xs outline-none font-semibold mt-1"></select>
        </div>
        <div>
          <label class="text-[10px] font-bold text-slate-400 uppercase">Nguồn việc</label>
          <select id="modalSourceSelect" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-xs outline-none font-semibold mt-1"></select>
        </div>
      </div>

      <div class="bg-slate-100/70 p-3 rounded-2xl border border-slate-200/60 space-y-2">
        <div class="grid grid-cols-2 gap-2">
          <div>
            <label class="text-[10px] font-bold text-slate-600 uppercase">Từ ngày (Ngày 1)</label>
            <input id="inputDate" type="date" class="w-full bg-white border border-slate-200 rounded-xl px-2.5 py-2 text-xs outline-none mt-1 font-bold">
          </div>
          <div>
            <label class="text-[10px] font-bold text-slate-600 uppercase">Đến ngày (Nếu có)</label>
            <input id="inputEndDate" type="date" class="w-full bg-white border border-slate-200 rounded-xl px-2.5 py-2 text-xs outline-none mt-1 font-bold text-slate-700">
          </div>
        </div>
      </div>

      <div>
        <label class="text-[10px] font-bold text-slate-400 uppercase">Giờ bắt đầu làm</label>
        <input id="inputTime" type="time" value="09:00" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-xs outline-none mt-1 font-semibold">
      </div>

      <div class="bg-slate-50 p-3.5 rounded-2xl border border-slate-200/80 space-y-2.5">
        <div class="grid grid-cols-2 gap-2">
          <div>
            <label class="text-[10px] font-bold text-slate-500 uppercase">Tổng hợp đồng (VND)</label>
            <input id="inputIncome" oninput="calculateBalance()" type="number" placeholder="vd: 8000000" class="w-full bg-white border border-slate-200 rounded-xl px-3 py-2 text-xs outline-none mt-1 font-bold">
          </div>
          <div>
            <label class="text-[10px] font-bold text-slate-500 uppercase">Tiền cọc nhận trước</label>
            <input id="inputDeposit" oninput="calculateBalance()" type="number" placeholder="vd: 3000000" class="w-full bg-white border border-slate-200 rounded-xl px-3 py-2 text-xs outline-none mt-1 font-bold text-amber-600">
          </div>
        </div>

        <div class="flex justify-between items-center pt-2 border-t border-slate-200/60">
          <span class="text-xs font-bold text-slate-600">Còn lại cần thu:</span>
          <span id="previewRemaining" class="text-sm font-extrabold text-rose-500">0 ₫</span>
        </div>

        <div class="flex items-center justify-between pt-1">
          <span class="text-xs font-semibold text-slate-700">Khách đã thanh toán đủ 100%?</span>
          <input id="inputIsPaid" onchange="calculateBalance()" type="checkbox" class="w-5 h-5 accent-slate-900 rounded cursor-pointer">
        </div>
      </div>

      <div>
        <label class="text-[10px] font-bold text-slate-400 uppercase">Ghi chú (Địa chỉ, lịch trình...)</label>
        <textarea id="inputNote" rows="2" placeholder="vd: 8h đón dâu nhà gái..." class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-xs outline-none mt-1"></textarea>
      </div>

      <button id="modalSubmitBtn" onclick="saveOrUpdateJob()" class="w-full bg-slate-900 text-white font-bold py-3.5 rounded-xl shadow-md text-xs active:scale-[0.98] transition">
        Lưu Công Việc
      </button>
    </div>
  </div>

  <!-- JAVASCRIPT: TOÀN BỘ LOGIC HỆ THỐNG -->
  <script>
    const DB_NAME = 'JobCalendarDB_v3';
    const STORE_NAME = 'jobs_store';
    let db = null;

    const PALETTE = ['#2563EB', '#059669', '#7C3AED', '#EA580C', '#DB2777', '#4B5563', '#0891B2', '#D97706'];
    let selectedColor = PALETTE[0];
    let selectedCategoryColor = PALETTE[4]; // Mặc định hồng/đỏ

    let sources = [
      { name: 'Facebook', color: '#2563EB' },
      { name: 'Khách quen', color: '#059669' },
      { name: 'Giới thiệu', color: '#7C3AED' },
      { name: 'TikTok', color: '#EA580C' }
    ];

    // Hạng mục mặc định đã có mã màu
    let categories = [
      { name: 'Wedding', color: '#DB2777' },
      { name: 'Event', color: '#2563EB' },
      { name: 'Quảng cáo TVC', color: '#7C3AED' },
      { name: 'Kỷ yếu', color: '#059669' },
      { name: 'Chân dung', color: '#D97706' }
    ];

    let jobs = [];
    let now = new Date();
    let viewDate = new Date(now.getFullYear(), now.getMonth(), 1);
    let selectedDate = formatDate(now);
    let currentTab = 'calendar';
    let isArchivedSubTab = false;
    let listFilterSource = 'Tất cả';
    let isAdminUnlocked = false; // Quản trị database

    function formatDate(d) {
      return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')}`;
    }

    function formatDateRange(startDateStr, endDateStr) {
      if (!endDateStr || endDateStr === startDateStr) {
        const parts = startDateStr.split('-');
        return `${parts[2]}/${parts[1]}`;
      }
      const [y1, m1, d1] = startDateStr.split('-');
      const [y2, m2, d2] = endDateStr.split('-');
      return m1 === m2 && y1 === y2 ? `${d1} - ${d2}/${m1}` : `${d1}/${m1} - ${d2}/${m2}`;
    }

    function isJobActiveOnDate(job, dateStr) {
      const start = job.date;
      const end = job.endDate || job.date;
      return dateStr >= start && dateStr <= end;
    }

    /* YÊU CẦU 3: RELOAD TRANG */
    function handleReloadPage() {
      const icon = document.getElementById('reloadBtnIcon');
      if (icon) icon.classList.add('animate-spin');
      setTimeout(() => {
        window.location.reload();
      }, 250);
    }

    /* DATABASE INDEXEDDB */
    function initDatabase() {
      return new Promise((resolve) => {
        const req = indexedDB.open(DB_NAME, 1);
        req.onupgradeneeded = (e) => {
          db = e.target.result;
          if (!db.objectStoreNames.contains(STORE_NAME)) {
            db.createObjectStore(STORE_NAME, { keyPath: 'id' });
          }
        };
        req.onsuccess = (e) => { db = e.target.result; resolve(); };
        req.onerror = () => resolve();
      });
    }

    async function loadAllData() {
      const savedSources = localStorage.getItem('PRO_SOURCES_COLOR');
      if (savedSources) sources = JSON.parse(savedSources);

      const savedCats = localStorage.getItem('PRO_CATEGORIES_OBJ');
      if (savedCats) {
        try {
          const parsed = JSON.parse(savedCats);
          if (Array.isArray(parsed) && parsed.length > 0) {
            // Tương thích ngược nếu trước đó lưu string[]
            if (typeof parsed[0] === 'string') {
              categories = parsed.map((catName, idx) => ({
                name: catName,
                color: PALETTE[idx % PALETTE.length]
              }));
            } else {
              categories = parsed;
            }
          }
        } catch(e) {}
      }

      if (!db) {
        jobs = JSON.parse(localStorage.getItem('PRO_JOBS')) || [];
        return;
      }

      return new Promise((resolve) => {
        const tx = db.transaction(STORE_NAME, 'readonly');
        const req = tx.objectStore(STORE_NAME).getAll();
        req.onsuccess = () => {
          jobs = req.result || [];
          localStorage.setItem('PRO_JOBS', JSON.stringify(jobs));
          resolve();
        };
      });
    }

    function persistJob(job) {
      if (db) {
        const tx = db.transaction(STORE_NAME, 'readwrite');
        tx.objectStore(STORE_NAME).put(job);
      }
      localStorage.setItem('PRO_JOBS', JSON.stringify(jobs));
    }

    function removeJobFromDB(id) {
      if (db) {
        const tx = db.transaction(STORE_NAME, 'readwrite');
        tx.objectStore(STORE_NAME).delete(id);
      }
      localStorage.setItem('PRO_JOBS', JSON.stringify(jobs));
    }

    async function init() {
      await initDatabase();
      await loadAllData();
      lucide.createIcons();
      renderColorPicker();
      renderCategoryColorPicker();
      updateHeader();
      renderMonthGrid();
      renderDayJobs();
      renderAllJobsList();
      renderStats();
      renderSettingsSources();
      renderSettingsCategories();
      populateDropdowns();
      document.getElementById('inputDate').value = selectedDate;
    }

    function calculateBalance() {
      const income = parseFloat(document.getElementById('inputIncome').value) || 0;
      const deposit = parseFloat(document.getElementById('inputDeposit').value) || 0;
      const isPaid = document.getElementById('inputIsPaid').checked;

      let remaining = 0;
      if (!isPaid) remaining = Math.max(0, income - deposit);
      document.getElementById('previewRemaining').innerText = new Intl.NumberFormat('vi-VN').format(remaining) + ' ₫';
    }

    function getJobAlertStatus(job) {
      if (job.isPaid) return { type: 'paid', text: 'Đã thu 100%', border: 'border-emerald-500', badgeBg: 'bg-emerald-500 text-white' };
      if (job.isCompleted) return { type: 'completed', text: 'Đã xong (Còn nợ)', border: 'border-slate-400', badgeBg: 'bg-slate-700 text-white' };

      const today = new Date();
      today.setHours(0,0,0,0);
      const targetDate = new Date(job.date + 'T00:00:00');
      const diffDays = Math.ceil((targetDate - today) / (1000 * 60 * 60 * 24));

      if (diffDays <= 1) {
        return { type: 'danger', text: diffDays < 0 ? 'Quá hạn' : (diffDays === 0 ? 'Hôm nay làm!' : 'Ngày mai làm!'), border: 'border-rose-500', badgeBg: 'bg-rose-500 text-white' };
      } else if (diffDays <= 3) {
        return { type: 'warning', text: `Còn ${diffDays} ngày`, border: 'border-amber-500', badgeBg: 'bg-amber-500 text-white' };
      }
      return { type: 'normal', text: 'Sắp tới', border: 'border-slate-200', badgeBg: 'bg-slate-100 text-slate-600' };
    }

    /* CARD CÔNG VIỆC: ĐỒNG BỘ MÀU HẠNG MỤC & NGUỒN (YÊU CẦU 2) */
    function createJobCard(job, showDate = false) {
      const alertStatus = getJobAlertStatus(job);
      const srcObj = sources.find(s => s.name === job.source) || { color: '#64748B' };
      const catObj = categories.find(c => c.name === job.category) || { color: '#4B5563' };
      const deposit = job.deposit || 0;
      const remaining = job.isPaid ? 0 : Math.max(0, job.income - deposit);
      const formattedDate = formatDateRange(job.date, job.endDate);

      const card = document.createElement('div');
      card.className = `p-4 rounded-2xl border-2 ${alertStatus.border} bg-white shadow-sm space-y-3 transition`;

      card.innerHTML = `
        <div class="flex justify-between items-start">
          <div class="flex-1 pr-2">
            <div class="flex flex-wrap items-center gap-1.5 text-xs font-semibold mb-1">
              ${showDate ? `<span class="text-slate-900 font-extrabold bg-slate-100 px-2 py-0.5 rounded-md border border-slate-200">${formattedDate}</span><span>•</span>` : ''}
              <span class="text-slate-500">${job.time}</span>

              <!-- Huy hiệu Hạng mục mang màu nhận diện -->
              <span style="background-color: ${catObj.color}15; color: ${catObj.color}; border: 1px solid ${catObj.color}40;" class="px-2 py-0.5 rounded text-[10px] font-extrabold">
                ${job.category || 'Chung'}
              </span>

              <!-- Huy hiệu Nguồn mang màu nhận diện -->
              <span style="background-color: ${srcObj.color}15; color: ${srcObj.color}; border: 1px solid ${srcObj.color}40;" class="px-2 py-0.5 rounded text-[10px] font-bold">
                ● ${job.source}
              </span>

              <span class="px-2 py-0.5 rounded text-[10px] font-extrabold ${alertStatus.badgeBg}">
                ${alertStatus.text}
              </span>
            </div>

            <h4 class="font-extrabold text-slate-900 text-sm mt-1 leading-tight">${job.title}</h4>
            ${job.note ? `<p class="text-xs text-slate-500 mt-1">${job.note}</p>` : ''}
          </div>

          <div class="flex items-center gap-1">
            <button onclick="toggleArchive('${job.id}')" title="${job.isArchived ? 'Bỏ lưu trữ' : 'Lưu trữ'}" class="text-slate-400 hover:text-slate-700 p-1.5 rounded-lg active:bg-slate-200 transition">
              <i data-lucide="${job.isArchived ? 'archive-restore' : 'archive'}" class="w-4 h-4"></i>
            </button>
            <button onclick="openModalForEdit('${job.id}')" title="Sửa" class="text-slate-400 hover:text-slate-800 p-1.5 rounded-lg active:bg-slate-200 transition">
              <i data-lucide="pencil" class="w-4 h-4"></i>
            </button>
            <button onclick="deleteJob('${job.id}')" title="Xóa" class="text-slate-300 hover:text-rose-500 p-1.5 rounded-lg active:bg-rose-50 transition">
              <i data-lucide="trash-2" class="w-4 h-4"></i>
            </button>
          </div>
        </div>

        <div class="bg-slate-50 p-2.5 rounded-xl border border-slate-200/60 flex justify-between items-center text-xs">
          <div>
            <span class="text-[10px] text-slate-400 font-bold block uppercase">Tổng tiền</span>
            <span class="font-extrabold text-slate-900 text-xs">${new Intl.NumberFormat('vi-VN').format(job.income)} ₫</span>
          </div>
          <div>
            <span class="text-[10px] text-slate-400 font-bold block uppercase">Đã cọc</span>
            <span class="font-bold text-amber-600 text-xs">${new Intl.NumberFormat('vi-VN').format(deposit)} ₫</span>
          </div>
          <div class="text-right">
            <span class="text-[10px] text-slate-400 font-bold block uppercase">Còn lại</span>
            <span class="font-extrabold ${remaining > 0 ? 'text-rose-600' : 'text-emerald-600'} text-xs">
              ${remaining > 0 ? new Intl.NumberFormat('vi-VN').format(remaining) + ' ₫' : 'Thu đủ'}
            </span>
          </div>
        </div>

        <div class="flex items-center justify-between pt-1">
          <button onclick="toggleCompleted('${job.id}')" class="flex items-center gap-1.5 text-xs font-bold px-3 py-1.5 rounded-xl transition ${
            job.isCompleted ? 'bg-slate-800 text-white' : 'bg-slate-100 text-slate-600'
          }">
            <i data-lucide="${job.isCompleted ? 'check-circle-2' : 'circle'}" class="w-3.5 h-3.5"></i>
            <span>${job.isCompleted ? 'Đã hoàn thành' : 'Chưa xong'}</span>
          </button>

          <button onclick="togglePaid('${job.id}')" class="text-xs font-bold px-3 py-1.5 rounded-xl transition ${
            job.isPaid ? 'bg-emerald-600 text-white' : 'bg-rose-50 text-rose-600 border border-rose-200'
          }">
            ${job.isPaid ? '✓ Đã thu 100%' : '● Còn nợ chưa thu'}
          </button>
        </div>
      `;
      return card;
    }

    function openModalForCreate() {
      document.getElementById('modalTitle').innerText = 'Thêm Lịch Làm Việc';
      document.getElementById('modalSubmitBtn').innerText = 'Lưu Công Việc';
      document.getElementById('editingJobId').value = '';
      document.getElementById('inputTitle').value = '';
      document.getElementById('inputDate').value = selectedDate;
      document.getElementById('inputEndDate').value = '';
      document.getElementById('inputTime').value = '09:00';
      document.getElementById('inputIncome').value = '';
      document.getElementById('inputDeposit').value = '';
      document.getElementById('inputNote').value = '';
      document.getElementById('inputIsPaid').checked = false;

      calculateBalance();
      document.getElementById('jobModal').classList.remove('hidden');
    }

    function openModalForEdit(jobId) {
      const job = jobs.find(j => j.id === jobId);
      if (!job) return;

      document.getElementById('modalTitle').innerText = 'Chỉnh Sửa Công Việc';
      document.getElementById('modalSubmitBtn').innerText = 'Cập Nhật Thay Đổi';
      document.getElementById('editingJobId').value = job.id;

      document.getElementById('inputTitle').value = job.title;
      document.getElementById('inputDate').value = job.date;
      document.getElementById('inputEndDate').value = job.endDate || '';
      document.getElementById('inputTime').value = job.time;
      document.getElementById('inputIncome').value = job.income;
      document.getElementById('inputDeposit').value = job.deposit || '';
      document.getElementById('inputNote').value = job.note || '';
      document.getElementById('modalSourceSelect').value = job.source;
      document.getElementById('modalCategorySelect').value = job.category || (categories[0] ? categories[0].name : '');
      document.getElementById('inputIsPaid').checked = job.isPaid;

      calculateBalance();
      document.getElementById('jobModal').classList.remove('hidden');
    }

    function closeModal() {
      document.getElementById('jobModal').classList.add('hidden');
    }

    function saveOrUpdateJob() {
      const editingId = document.getElementById('editingJobId').value;
      const title = document.getElementById('inputTitle').value.trim();
      const date = document.getElementById('inputDate').value;
      let endDate = document.getElementById('inputEndDate').value;
      const time = document.getElementById('inputTime').value;
      const category = document.getElementById('modalCategorySelect').value;
      const source = document.getElementById('modalSourceSelect').value;
      const income = parseFloat(document.getElementById('inputIncome').value) || 0;
      const deposit = parseFloat(document.getElementById('inputDeposit').value) || 0;
      const note = document.getElementById('inputNote').value.trim();
      const isPaid = document.getElementById('inputIsPaid').checked;

      if (!title || !income || !date) {
        alert('Vui lòng nhập tên việc, ngày làm và tổng thu nhập');
        return;
      }

      if (endDate && endDate < date) {
        alert('Ngày kết thúc không được trước ngày bắt đầu!');
        return;
      }
      if (endDate === date) endDate = '';

      if (editingId) {
        const idx = jobs.findIndex(j => j.id === editingId);
        if (idx !== -1) {
          jobs[idx] = { ...jobs[idx], title, date, endDate, time, category, source, income, deposit, note, isPaid };
          persistJob(jobs[idx]);
        }
      } else {
        const newJob = {
          id: Date.now().toString(),
          title, date, endDate, time, category, source, income, deposit, note,
          isPaid,
          isCompleted: false,
          isArchived: false
        };
        jobs.unshift(newJob);
        persistJob(newJob);
      }

      closeModal();
      refreshAllViews();
      if (isAdminUnlocked) renderAdminRecordList();
    }

    function toggleCompleted(id) {
      const job = jobs.find(j => j.id === id);
      if (job) {
        job.isCompleted = !job.isCompleted;
        persistJob(job);
        refreshAllViews();
        if (isAdminUnlocked) renderAdminRecordList();
      }
    }

    function togglePaid(id) {
      const job = jobs.find(j => j.id === id);
      if (job) {
        job.isPaid = !job.isPaid;
        persistJob(job);
        refreshAllViews();
        if (isAdminUnlocked) renderAdminRecordList();
      }
    }

    function toggleArchive(id) {
      const job = jobs.find(j => j.id === id);
      if (job) {
        job.isArchived = !job.isArchived;
        persistJob(job);
        refreshAllViews();
        if (isAdminUnlocked) renderAdminRecordList();
      }
    }

    function deleteJob(id) {
      if (confirm('Xóa công việc này hoàn toàn khỏi Database?')) {
        jobs = jobs.filter(j => j.id !== id);
        removeJobFromDB(id);
        refreshAllViews();
        if (isAdminUnlocked) renderAdminRecordList();
      }
    }

    function switchListSubTab(isArchive) {
      isArchivedSubTab = isArchive;
      if (isArchive) {
        document.getElementById('subTabArchiveBtn').className = 'flex-1 py-2 rounded-xl bg-white text-slate-900 shadow-sm transition';
        document.getElementById('subTabActiveBtn').className = 'flex-1 py-2 rounded-xl text-slate-500 transition';
      } else {
        document.getElementById('subTabActiveBtn').className = 'flex-1 py-2 rounded-xl bg-white text-slate-900 shadow-sm transition';
        document.getElementById('subTabArchiveBtn').className = 'flex-1 py-2 rounded-xl text-slate-500 transition';
      }
      renderAllJobsList();
    }

    function renderAllJobsList() {
      const container = document.getElementById('allJobsContainer');
      const search = document.getElementById('searchInput').value.toLowerCase();
      const filterBox = document.getElementById('listSourceFilter');

      const activeCount = jobs.filter(j => !j.isArchived).length;
      const archiveCount = jobs.filter(j => j.isArchived).length;
      document.getElementById('activeJobCount').innerText = activeCount;
      document.getElementById('archivedJobCount').innerText = archiveCount;

      filterBox.innerHTML = '';
      ['Tất cả', ...sources.map(s => s.name)].forEach(srcName => {
        const btn = document.createElement('button');
        const active = listFilterSource === srcName;
        btn.className = `px-3 py-1.5 rounded-full text-xs font-semibold whitespace-nowrap transition border ${
          active ? 'bg-slate-900 text-white border-slate-900' : 'bg-white text-slate-600 border-slate-200'
        }`;
        btn.innerText = srcName;
        btn.onclick = () => { listFilterSource = srcName; renderAllJobsList(); };
        filterBox.appendChild(btn);
      });

      let filtered = jobs.filter(j => Boolean(j.isArchived) === isArchivedSubTab);
      filtered = filtered.filter(j => {
        const matchSearch = j.title.toLowerCase().includes(search) || (j.note && j.note.toLowerCase().includes(search));
        const matchSource = listFilterSource === 'Tất cả' || j.source === listFilterSource;
        return matchSearch && matchSource;
      });

      filtered.sort((a, b) => a.date.localeCompare(b.date));

      container.innerHTML = '';
      if (filtered.length === 0) {
        container.innerHTML = `<p class="text-center py-10 text-xs text-slate-400">${isArchivedSubTab ? 'Kho lưu trữ đang trống' : 'Không có công việc nào'}</p>`;
        return;
      }

      filtered.forEach(job => container.appendChild(createJobCard(job, true)));
      lucide.createIcons();
    }

    function changeMonth(delta) {
      viewDate.setMonth(viewDate.getMonth() + delta);
      updateHeader();
      renderMonthGrid();
    }

    /* YÊU CẦU 2: HIỂN THỊ MÀU HẠNG MỤC TƯƠNG XỨNG TRÊN LỊCH THÁNG */
    function renderMonthGrid() {
      const grid = document.getElementById('monthGrid');
      grid.innerHTML = '';
      const year = viewDate.getFullYear();
      const month = viewDate.getMonth();
      document.getElementById('calendarMonthLabel').innerText = `Tháng ${month + 1} / ${year}`;

      const firstDay = new Date(year, month, 1).getDay();
      const totalDays = new Date(year, month + 1, 0).getDate();

      for (let i = 0; i < firstDay; i++) {
        const empty = document.createElement('div');
        empty.className = 'h-10';
        grid.appendChild(empty);
      }

      for (let day = 1; day <= totalDays; day++) {
        const dStr = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
        const isSelected = dStr === selectedDate;
        const dayJobs = jobs.filter(j => isJobActiveOnDate(j, dStr) && !j.isArchived);

        const btn = document.createElement('button');
        btn.className = `h-12 rounded-2xl flex flex-col items-center justify-center transition ${
          isSelected ? 'bg-slate-900 text-white font-bold shadow-md' : 'text-slate-800 hover:bg-slate-100 font-semibold'
        }`;
        btn.onclick = () => {
          selectedDate = dStr;
          renderMonthGrid();
          renderDayJobs();
        };

        // Render chấm màu theo HẠNG MỤC tương ứng
        let dotsHtml = '<span class="h-1.5 mt-0.5"></span>';
        if (dayJobs.length > 0) {
          if (isSelected) {
            dotsHtml = `<div class="flex gap-0.5 mt-0.5"><span class="w-1.5 h-1.5 rounded-full bg-white"></span></div>`;
          } else {
            dotsHtml = `
              <div class="flex gap-0.5 mt-0.5">
                ${dayJobs.slice(0, 3).map(j => {
                  const cObj = categories.find(c => c.name === j.category) || { color: '#64748B' };
                  return `<span class="w-1.5 h-1.5 rounded-full" style="background-color: ${cObj.color};"></span>`;
                }).join('')}
              </div>
            `;
          }
        }

        btn.innerHTML = `
          <span class="text-xs">${day}</span>
          ${dotsHtml}
        `;
        grid.appendChild(btn);
      }
    }

    function renderDayJobs() {
      const container = document.getElementById('dayJobList');
      container.innerHTML = '';
      const dayJobs = jobs.filter(j => isJobActiveOnDate(j, selectedDate) && !j.isArchived);
      document.getElementById('selectedDayJobCount').innerText = `${dayJobs.length} việc`;
      document.getElementById('selectedDayLabel').innerText = `NGÀY ${selectedDate.split('-').reverse().join('/')}`;

      if (dayJobs.length === 0) {
        container.innerHTML = `
          <div class="text-center py-8 text-slate-400 bg-white rounded-2xl border border-slate-100">
            <p class="text-xs">Không có lịch làm việc trong ngày này</p>
          </div>
        `;
        return;
      }
      dayJobs.forEach(job => container.appendChild(createJobCard(job)));
      lucide.createIcons();
    }

    /* YÊU CẦU 4: BÁO CÁO DOANH THU & THEO HẠNG MỤC */
    function renderStats() {
      let totalIncome = 0;
      let totalDepositPending = 0;
      let totalPaidFull = 0;
      let totalDebt = 0;
      let completedCount = 0;

      const sourceStat = {};
      const catStat = {};

      jobs.forEach(j => {
        totalIncome += (j.income || 0);
        const dep = j.deposit || 0;

        if (j.isPaid) {
          totalPaidFull += (j.income || 0);
        } else {
          totalDepositPending += dep;
          totalDebt += Math.max(0, (j.income || 0) - dep);
        }

        if (j.isCompleted) completedCount++;

        // Thống kê Nguồn
        sourceStat[j.source] = (sourceStat[j.source] || 0) + 1;

        // Thống kê theo Hạng mục
        const catName = j.category || 'Chung';
        if (!catStat[catName]) {
          catStat[catName] = { totalJobs: 0, completed: 0, income: 0 };
        }
        catStat[catName].totalJobs += 1;
        if (j.isCompleted) catStat[catName].completed += 1;
        catStat[catName].income += (j.income || 0);
      });

      // Đưa số liệu vào giao diện đã tách biệt
      document.getElementById('statTotalJobs').innerText = jobs.length;
      document.getElementById('statCompletedJobs').innerText = completedCount;

      document.getElementById('statTotalIncome').innerText = new Intl.NumberFormat('vi-VN').format(totalIncome) + ' ₫';
      document.getElementById('statTotalDeposit').innerText = new Intl.NumberFormat('vi-VN').format(totalDepositPending) + ' ₫';
      document.getElementById('statPaidFull').innerText = new Intl.NumberFormat('vi-VN').format(totalPaidFull) + ' ₫';
      document.getElementById('statDebt').innerText = new Intl.NumberFormat('vi-VN').format(totalDebt) + ' ₫';

      // Render Báo cáo theo Hạng mục (Thể loại)
      const catBox = document.getElementById('statCategoryBreakdown');
      catBox.innerHTML = '';
      const catKeys = Object.keys(catStat);

      if (catKeys.length === 0) {
        catBox.innerHTML = `<p class="text-slate-400 text-center py-2">Chưa có dữ liệu</p>`;
      } else {
        catKeys.forEach(cat => {
          const item = catStat[cat];
          const catObj = categories.find(c => c.name === cat) || { color: '#64748B' };
          catBox.innerHTML += `
            <div class="p-3 bg-slate-50 rounded-2xl border border-slate-100 space-y-1.5">
              <div class="flex justify-between items-center">
                <span class="font-extrabold text-slate-800 flex items-center gap-1.5">
                  <span class="w-2.5 h-2.5 rounded-full" style="background-color: ${catObj.color}"></span>
                  ${cat}
                </span>
                <span class="font-extrabold text-slate-900">${new Intl.NumberFormat('vi-VN').format(item.income)} ₫</span>
              </div>
              <div class="flex justify-between items-center text-[11px] text-slate-500 pt-0.5">
                <span>Số việc: <b>${item.totalJobs}</b></span>
                <span class="text-emerald-600 font-bold">Hoàn thành: ${item.completed}/${item.totalJobs}</span>
              </div>
            </div>
          `;
        });
      }

      // Render Báo cáo theo Nguồn
      const breakdown = document.getElementById('statSourceBreakdown');
      breakdown.innerHTML = '';
      Object.keys(sourceStat).forEach(src => {
        const pct = Math.round((sourceStat[src] / (jobs.length || 1)) * 100);
        const srcObj = sources.find(s => s.name === src) || { color: '#64748B' };
        breakdown.innerHTML += `
          <div class="flex justify-between items-center py-1.5 border-b border-slate-50">
            <span class="font-medium text-slate-700 flex items-center gap-1.5">
              <span class="w-2 h-2 rounded-full" style="background-color: ${srcObj.color}"></span>
              ${src}
            </span>
            <span class="font-bold text-slate-900">${sourceStat[src]} việc (${pct}%)</span>
          </div>
        `;
      });
    }

    /* ================= YÊU CẦU 1: QUYỀN QUẢN TRỊ DATABASE (PASS: 1998) ================= */
    function loginAdminDatabase() {
      const pass = document.getElementById('adminPassInput').value.trim();
      const err = document.getElementById('adminPassError');
      if (pass === '1998') {
        err.classList.add('hidden');
        isAdminUnlocked = true;
        document.getElementById('adminLockBox').classList.add('hidden');
        document.getElementById('adminPanelBox').classList.remove('hidden');
        document.getElementById('adminStatusBadge').innerText = 'Đã mở khóa';
        document.getElementById('adminStatusBadge').className = 'text-[10px] font-bold px-2 py-0.5 rounded-full bg-emerald-100 text-emerald-700';
        document.getElementById('adminPassInput').value = '';
        renderAdminRecordList();
      } else {
        err.classList.remove('hidden');
      }
    }

    function logoutAdminDatabase() {
      isAdminUnlocked = false;
      document.getElementById('adminLockBox').classList.remove('hidden');
      document.getElementById('adminPanelBox').classList.add('hidden');
      document.getElementById('adminStatusBadge').innerText = 'Đang khóa';
      document.getElementById('adminStatusBadge').className = 'text-[10px] font-bold px-2 py-0.5 rounded-full bg-slate-100 text-slate-500';
    }

    function renderAdminRecordList() {
      const container = document.getElementById('adminRecordList');
      container.innerHTML = '';
      document.getElementById('adminRecordCount').innerText = jobs.length;

      if (jobs.length === 0) {
        container.innerHTML = `<p class="text-center py-4 text-xs text-slate-400">Database hiện không có bản ghi nào.</p>`;
        return;
      }

      jobs.forEach(job => {
        const item = document.createElement('div');
        item.className = 'flex items-center justify-between p-2.5 bg-slate-50 border border-slate-200/70 rounded-xl text-xs';
        item.innerHTML = `
          <div class="flex-1 pr-2 truncate">
            <p class="font-bold text-slate-900 truncate">${job.title}</p>
            <p class="text-[10px] text-slate-400">${job.date} • ${new Intl.NumberFormat('vi-VN').format(job.income)}₫ • ${job.category || 'Chung'}</p>
          </div>
          <div class="flex items-center gap-1">
            <button onclick="openModalForEdit('${job.id}')" class="p-1 text-slate-600 hover:text-slate-900 bg-white rounded-lg border border-slate-200">
              <i data-lucide="pencil" class="w-3.5 h-3.5"></i>
            </button>
            <button onclick="deleteJob('${job.id}')" class="p-1 text-rose-600 hover:text-rose-800 bg-white rounded-lg border border-slate-200">
              <i data-lucide="trash-2" class="w-3.5 h-3.5"></i>
            </button>
          </div>
        `;
        container.appendChild(item);
      });
      lucide.createIcons();
    }

    function adminClearEntireDB() {
      if (confirm('CẢNH BÁO: Bạn có chắc chắn muốn XÓA TRẮNG toàn bộ Database không? Hành động này không thể hoàn tác!')) {
        jobs = [];
        localStorage.removeItem('PRO_JOBS');
        if (db) {
          const tx = db.transaction(STORE_NAME, 'readwrite');
          tx.objectStore(STORE_NAME).clear();
        }
        refreshAllViews();
        renderAdminRecordList();
        alert('Đã xóa sạch cơ sở dữ liệu.');
      }
    }

    /* ================= YÊU CẦU 2: CÀI ĐẶT THỂ LOẠI & COLOR PICKER ================= */
    function renderCategoryColorPicker() {
      const picker = document.getElementById('categoryColorPicker');
      picker.innerHTML = '';
      PALETTE.forEach(c => {
        const dot = document.createElement('button');
        dot.className = `w-5 h-5 rounded-full transition ${selectedCategoryColor === c ? 'ring-2 ring-slate-800 scale-110' : ''}`;
        dot.style.backgroundColor = c;
        dot.onclick = () => { selectedCategoryColor = c; renderCategoryColorPicker(); };
        picker.appendChild(dot);
      });
    }

    function renderSettingsCategories() {
      const container = document.getElementById('settingsCategoryList');
      container.innerHTML = '';
      categories.forEach((cat, idx) => {
        const row = document.createElement('div');
        row.className = 'flex justify-between items-center py-2 px-3 bg-slate-50 rounded-xl text-xs font-semibold text-slate-700';
        row.innerHTML = `
          <div class="flex items-center gap-2">
            <span class="w-3 h-3 rounded-full" style="background-color: ${cat.color}"></span>
            <span>${cat.name}</span>
          </div>
          <button onclick="deleteCategory(${idx})" class="text-slate-400 hover:text-rose-500">
            <i data-lucide="trash" class="w-4 h-4"></i>
          </button>
        `;
        container.appendChild(row);
      });
      lucide.createIcons();
    }

    function addNewCategory() {
      const input = document.getElementById('inputNewCategory');
      const val = input.value.trim();
      if (!val) return;
      if (categories.some(c => c.name === val)) { alert('Hạng mục này đã có'); return; }
      categories.push({ name: val, color: selectedCategoryColor });
      localStorage.setItem('PRO_CATEGORIES_OBJ', JSON.stringify(categories));
      input.value = '';
      renderSettingsCategories();
      populateDropdowns();
      refreshAllViews();
    }

    function deleteCategory(idx) {
      if (confirm('Xóa hạng mục này?')) {
        categories.splice(idx, 1);
        localStorage.setItem('PRO_CATEGORIES_OBJ', JSON.stringify(categories));
        renderSettingsCategories();
        populateDropdowns();
        refreshAllViews();
      }
    }

    /* MÀU CHO NGUỒN */
    function renderColorPicker() {
      const picker = document.getElementById('sourceColorPicker');
      picker.innerHTML = '';
      PALETTE.forEach(c => {
        const dot = document.createElement('button');
        dot.className = `w-5 h-5 rounded-full transition ${selectedColor === c ? 'ring-2 ring-slate-800 scale-110' : ''}`;
        dot.style.backgroundColor = c;
        dot.onclick = () => { selectedColor = c; renderColorPicker(); };
        picker.appendChild(dot);
      });
    }

    function renderSettingsSources() {
      const list = document.getElementById('settingsSourceList');
      list.innerHTML = '';
      sources.forEach((src, idx) => {
        const row = document.createElement('div');
        row.className = "flex justify-between items-center py-2 px-3 bg-slate-50 rounded-xl text-xs font-semibold text-slate-700";
        row.innerHTML = `
          <div class="flex items-center gap-2">
            <span class="w-3 h-3 rounded-full" style="background-color: ${src.color}"></span>
            <span>${src.name}</span>
          </div>
          <button onclick="deleteSource(${idx})" class="text-slate-400 hover:text-rose-500">
            <i data-lucide="trash" class="w-4 h-4"></i>
          </button>
        `;
        list.appendChild(row);
      });
      lucide.createIcons();
    }

    function addNewSource() {
      const input = document.getElementById('inputNewSource');
      const val = input.value.trim();
      if (!val) return;
      if (sources.some(s => s.name === val)) { alert('Nguồn này đã tồn tại'); return; }
      sources.push({ name: val, color: selectedColor });
      localStorage.setItem('PRO_SOURCES_COLOR', JSON.stringify(sources));
      input.value = '';
      renderSettingsSources();
      populateDropdowns();
      refreshAllViews();
    }

    function deleteSource(idx) {
      if (confirm('Xóa nguồn này khỏi danh sách?')) {
        sources.splice(idx, 1);
        localStorage.setItem('PRO_SOURCES_COLOR', JSON.stringify(sources));
        renderSettingsSources();
        populateDropdowns();
        refreshAllViews();
      }
    }

    function populateDropdowns() {
      const srcSel = document.getElementById('modalSourceSelect');
      srcSel.innerHTML = '';
      sources.forEach(s => {
        const opt = document.createElement('option');
        opt.value = s.name;
        opt.innerText = s.name;
        srcSel.appendChild(opt);
      });

      const catSel = document.getElementById('modalCategorySelect');
      catSel.innerHTML = '';
      categories.forEach(c => {
        const opt = document.createElement('option');
        opt.value = c.name;
        opt.innerText = c.name;
        catSel.appendChild(opt);
      });
    }

    /* BACKUP / IMPORT */
    function exportDataBackup() {
      const backupData = { sources, categories, jobs, exportDate: new Date().toISOString() };
      const blob = new Blob([JSON.stringify(backupData, null, 2)], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `LichViec_Backup_${formatDate(new Date())}.json`;
      a.click();
      URL.revokeObjectURL(url);
    }

    function importDataBackup(event) {
      const file = event.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = async (e) => {
        try {
          const imported = JSON.parse(e.target.result);
          if (imported.jobs && Array.isArray(imported.jobs)) {
            jobs = imported.jobs;
            if (imported.sources) sources = imported.sources;
            if (imported.categories) categories = imported.categories;

            localStorage.setItem('PRO_SOURCES_COLOR', JSON.stringify(sources));
            localStorage.setItem('PRO_CATEGORIES_OBJ', JSON.stringify(categories));
            localStorage.setItem('PRO_JOBS', JSON.stringify(jobs));

            if (db) {
              const tx = db.transaction(STORE_NAME, 'readwrite');
              const store = tx.objectStore(STORE_NAME);
              store.clear();
              jobs.forEach(j => store.put(j));
            }
            alert('Khôi phục dữ liệu thành công!');
            location.reload();
          }
        } catch (err) { alert('Lỗi đọc file sao lưu'); }
      };
      reader.readAsText(file);
    }

    function refreshAllViews() {
      renderMonthGrid();
      renderDayJobs();
      renderAllJobsList();
      renderStats();
    }

    function navigateTab(tab) {
      currentTab = tab;
      const tabs = ['calendar', 'list', 'stats', 'settings'];
      tabs.forEach(t => {
        const el = document.getElementById('tab' + t.charAt(0).toUpperCase() + t.slice(1));
        const btn = document.getElementById('nav' + t.charAt(0).toUpperCase() + t.slice(1));
        if (t === tab) {
          el.classList.remove('hidden');
          btn.className = 'flex flex-col items-center gap-1 text-slate-900';
          btn.querySelector('span').className = 'text-[10px] font-bold';
        } else {
          el.classList.add('hidden');
          btn.className = 'flex flex-col items-center gap-1 text-slate-400';
          btn.querySelector('span').className = 'text-[10px] font-semibold';
        }
      });
      updateHeader();
      if (tab === 'list') renderAllJobsList();
      if (tab === 'stats') renderStats();
    }

    function updateHeader() {
      const topTitle = document.getElementById('topTitle');
      const topSubtitle = document.getElementById('topSubtitle');
      if (currentTab === 'calendar') {
        topSubtitle.innerText = 'LỊCH LÀM VIỆC';
        topTitle.innerText = `Tháng ${viewDate.getMonth() + 1}, ${viewDate.getFullYear()}`;
      } else if (currentTab === 'list') {
        topSubtitle.innerText = 'TRA CỨU';
        topTitle.innerText = isArchivedSubTab ? 'Kho Lưu Trữ' : 'Danh Sách Việc';
      } else if (currentTab === 'stats') {
        topSubtitle.innerText = 'TÀI CHÍNH';
        topTitle.innerText = 'Báo Cáo Doanh Thu';
      } else {
        topSubtitle.innerText = 'HỆ THỐNG';
        topTitle.innerText = 'Cài Đặt Quản Trị';
      }
    }

    window.onload = init;
  </script>
</body>
</html>
