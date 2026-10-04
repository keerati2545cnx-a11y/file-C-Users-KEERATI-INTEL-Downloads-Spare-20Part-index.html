<!DOCTYPE html>
<html lang="th" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>MARIO PARTS HUB - ระบบบริหารจัดการสแปร์พาร์ทเรียบหรูสไตล์มาริโอ้</title>
  
  <!-- Google Fonts: Prompt & Montserrat for Luxury Executive feel, plus Inter -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700;800;900&family=Prompt:wght@300;400;500;600;700&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            mario: {
              red: '#E52521',
              darkRed: '#B81D24',
              gold: '#FFD700',
              amber: '#F59E0B',
              luigi: '#10B981',
              toad: '#3B82F6',
              obsidian: '#0A0E17',
              surface: '#101726',
              card: '#151E32',
              cardHover: '#1A2640',
              border: '#22304C'
            }
          },
          fontFamily: {
            luxury: ['"Montserrat"', 'sans-serif'],
            sans: ['Prompt', 'Inter', 'system-ui', 'sans-serif']
          }
        }
      }
    }
  </script>

  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>

  <!-- SheetJS (Excel .xlsx Export & Import) -->
  <script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>

  <!-- JsBarcode for Barcode Generation -->
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.6/dist/JsBarcode.all.min.js"></script>

  <!-- HTML5-QRCode Scanner for Webcam & Mobile -->
  <script src="https://unpkg.com/html5-qrcode@2.3.8/html5-qrcode.min.js"></script>

  <!-- Custom Mario Luxury Theme Styles -->
  <link rel="stylesheet" href="css/mario-theme.css">
</head>
<body class="bg-mario-obsidian text-slate-100 min-h-screen font-sans antialiased selection:bg-mario-gold selection:text-slate-950 flex flex-col justify-between">

  <!-- ========================================== -->
  <!-- TOP NAVIGATION BAR (Mario Luxury Header) -->
  <!-- ========================================== -->
  <header class="sticky top-0 z-40 bg-mario-surface/95 backdrop-blur-md border-b border-mario-border px-4 lg:px-8 py-3 transition-all">
    <div class="max-w-7xl mx-auto flex items-center justify-between gap-4">
      
      <!-- Brand Logo -->
      <div class="flex items-center space-x-6">
        <a href="#" onclick="App.switchTab('catalog')" class="flex items-center space-x-3 group">
          <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-red-500 to-red-700 flex items-center justify-center text-white font-luxury font-black text-xl shadow-lg border border-red-400/40 group-hover:scale-105 transition transform">
            <span class="drop-shadow">M</span>
          </div>
          <div>
            <div class="font-luxury text-xl md:text-2xl font-black tracking-tight leading-none flex items-center gap-1.5">
              <span class="text-white">MARIO</span>
              <span class="text-gold-gradient">PARTS</span>
              <span class="text-xs px-1.5 py-0.5 rounded bg-yellow-500/20 text-yellow-400 border border-yellow-500/40 font-mono font-bold">HUB</span>
            </div>
            <div class="text-[10px] text-slate-400 font-semibold tracking-wider uppercase flex items-center gap-1 mt-0.5">
              <span>⭐</span>
              <span>Epson Robot Arms & OEMCM Test Socket Pins Hub</span>
            </div>
          </div>
        </a>

        <!-- Main Nav Tabs -->
        <nav class="hidden md:flex items-center space-x-1">
          <button id="navCatalogBtn" onclick="App.switchTab('catalog')" class="px-3.5 py-1.5 text-sm font-semibold text-yellow-400 border-b-2 border-yellow-400 transition flex items-center gap-1.5">
            <span>📦</span>
            <span>คลังสแปร์พาร์ท</span>
          </button>
          <button id="navLogsBtn" onclick="App.switchTab('logs')" class="px-3.5 py-1.5 text-sm font-semibold text-slate-400 hover:text-white transition flex items-center gap-1.5">
            <span>📜</span>
            <span>ประวัติการเบิกจ่าย</span>
          </button>
        </nav>
      </div>

      <!-- Center Search & Scanner -->
      <div class="flex-1 max-w-md hidden sm:block">
        <div class="relative">
          <span class="absolute left-3.5 top-1/2 -translate-y-1/2 text-sm text-yellow-500/70">🔍</span>
          <input 
            type="text" 
            id="globalSearchInput" 
            placeholder="ค้นหาตามรหัส [ ? ], ชื่ออะไหล่, สเปก, ยี่ห้อ หรือ Rack... (Ctrl+K)" 
            class="w-full bg-slate-900/90 border border-slate-700/80 rounded-full py-2 pl-10 pr-24 text-xs md:text-sm text-white placeholder-slate-400 focus:outline-none focus:border-yellow-500 focus:ring-1 focus:ring-yellow-500 transition shadow-inner"
          />
          <div class="absolute right-2 top-1/2 -translate-y-1/2 flex items-center space-x-1">
            <button onclick="App.openCameraScannerModal()" class="p-1.5 bg-slate-800 hover:bg-slate-700 text-yellow-400 rounded-full transition" title="สแกน Barcode/QR ผ่านกล้อง">
              <i data-lucide="camera" class="w-3.5 h-3.5"></i>
            </button>
            <span class="text-[10px] font-mono bg-slate-800 text-slate-400 px-1.5 py-0.5 rounded border border-slate-700">^K</span>
          </div>
        </div>
      </div>

      <!-- Right Actions: Scanner, Admin Toggle, Requester Profile -->
      <div class="flex items-center space-x-3">
        <!-- Mobile Camera button -->
        <button onclick="App.openCameraScannerModal()" class="sm:hidden p-2 bg-slate-800 text-yellow-400 rounded-xl hover:text-white border border-slate-700" title="สแกนบาร์โค้ด">
          <i data-lucide="camera" class="w-5 h-5"></i>
        </button>

        <!-- Admin Master PIN Lock Status Button -->
        <button 
          id="adminAuthToggleBtn"
          onclick="AdminController.isAdmin ? AdminController.logoutAdmin() : AdminController.showPinModal()"
          class="flex items-center space-x-2 px-3 py-1.5 rounded-xl border border-slate-700 bg-slate-900/90 hover:border-yellow-500/60 transition active:scale-95 shadow-sm"
          title="คลิกเพื่อปลดล็อกหรือล็อกโหมดผู้ดูแลระบบ (Admin)"
        >
          <span id="adminLockIcon"><i data-lucide="lock" class="w-4 h-4 text-red-400"></i></span>
          <span id="adminStatusText" class="text-xs font-semibold text-slate-300 hidden lg:inline">Admin Protected</span>
        </button>

        <!-- Profile / Requester Switcher -->
        <button 
          id="headerProfileBtn" 
          onclick="AdminController.openRequesterModal()" 
          class="flex items-center space-x-2 p-1 pl-1.5 pr-3 bg-slate-900/90 border border-slate-700 rounded-2xl hover:border-yellow-500/60 transition shadow-sm"
          title="จัดการรายชื่อผู้เบิก & หน้าตัวละคร (Mario Character Roster)"
        >
          <div class="w-8 h-8 rounded-xl bg-red-600 flex items-center justify-center text-sm shadow">
            M
          </div>
          <span class="text-xs font-medium text-slate-200 hidden md:inline">คนเบิกพาร์ท</span>
        </button>
      </div>

    </div>
  </header>

  <!-- ========================================== -->
  <!-- MAIN CONTAINER -->
  <!-- ========================================== -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-4 lg:px-8 py-6 space-y-6">

    <!-- KPI Metric Summary Cards (Mario Luxury Style) -->
    <section class="grid grid-cols-2 md:grid-cols-5 gap-3 md:gap-4">
      
      <!-- Total Parts -->
      <div class="luxury-card p-4 rounded-2xl flex flex-col justify-between">
        <div class="text-xs font-semibold text-slate-400 flex items-center justify-between">
          <span>พาร์ททั้งหมด</span>
          <span class="mario-block-badge px-1.5 py-0.5 rounded text-[10px] font-mono">[ ? ]</span>
        </div>
        <div class="mt-2 flex items-baseline justify-between">
          <span id="metricTotalParts" class="text-2xl md:text-3xl font-extrabold text-white">0</span>
          <span class="text-xs text-slate-400 font-medium">รายการ</span>
        </div>
      </div>

      <!-- Total Units (Gold Coins) -->
      <div class="luxury-card p-4 rounded-2xl flex flex-col justify-between">
        <div class="text-xs font-semibold text-slate-400 flex items-center justify-between">
          <span>จำนวนคงคลังรวม</span>
          <span class="text-base coin-shimmer">🪙</span>
        </div>
        <div class="mt-2 flex items-baseline justify-between">
          <span id="metricTotalUnits" class="text-2xl md:text-3xl font-extrabold text-yellow-400">0</span>
          <span class="text-xs text-yellow-500/80 font-medium">ชิ้น</span>
        </div>
      </div>

      <!-- Low Stock Warning (Bowser Warning) -->
      <div class="luxury-card p-4 rounded-2xl border-yellow-500/30 flex flex-col justify-between cursor-pointer hover:border-yellow-500 transition" onclick="App.setStockFilter('LOW')">
        <div class="text-xs font-semibold text-yellow-400 flex items-center justify-between">
          <span>สต็อกใกล้หมดเกณฑ์</span>
          <span>⚠️</span>
        </div>
        <div class="mt-2 flex items-baseline justify-between">
          <span id="metricLowStock" class="text-2xl md:text-3xl font-extrabold text-yellow-400">0</span>
          <span class="text-xs text-yellow-500 font-medium">ต้องสั่งซื้อ</span>
        </div>
      </div>

      <!-- Out of Stock (Bowser Shell) -->
      <div class="luxury-card p-4 rounded-2xl border-red-500/30 flex flex-col justify-between cursor-pointer hover:border-red-500 transition" onclick="App.setStockFilter('OUT')">
        <div class="text-xs font-semibold text-red-400 flex items-center justify-between">
          <span>หมดสต็อก (Out)</span>
          <span>💀</span>
        </div>
        <div class="mt-2 flex items-baseline justify-between">
          <span id="metricOutOfStock" class="text-2xl md:text-3xl font-extrabold text-red-500">0</span>
          <span class="text-xs text-red-400/80 font-medium">วิกฤต</span>
        </div>
      </div>

      <!-- Today's Requisitions (Super Mushroom) -->
      <div class="col-span-2 md:col-span-1 luxury-card p-4 rounded-2xl flex flex-col justify-between">
        <div class="text-xs font-semibold text-slate-400 flex items-center justify-between">
          <span>เบิกจ่ายวันนี้</span>
          <span class="text-base">🍄</span>
        </div>
        <div class="mt-2 flex items-baseline justify-between">
          <span id="metricTodayRequisitions" class="text-2xl md:text-3xl font-extrabold text-emerald-400">0</span>
          <span class="text-xs text-emerald-500/80 font-medium">ชิ้น</span>
        </div>
      </div>
    </section>

    <!-- ========================================== -->
    <!-- TAB 1: CATALOG SECTION -->
    <!-- ========================================== -->
    <div id="catalogSection" class="space-y-6">

      <!-- Super Star Spotlight Billboard Banner -->
      <section id="billboardHero"></section>

      <!-- Action Toolbar & Filter Controls -->
      <div class="luxury-card rounded-2xl p-4 flex flex-col md:flex-row items-stretch md:items-center justify-between gap-4">
        
        <!-- Stock Filter Buttons -->
        <div class="flex items-center space-x-1.5 overflow-x-auto pb-1 md:pb-0">
          <button onclick="App.setStockFilter('ALL')" data-filter="ALL" class="stock-filter-btn px-3 py-1.5 rounded-xl text-xs font-bold transition border border-yellow-500/50 bg-slate-800 text-yellow-400 shadow">
            ⭐ ทั้งหมด
          </button>
          <button onclick="App.setStockFilter('NORMAL')" data-filter="NORMAL" class="stock-filter-btn px-3 py-1.5 rounded-xl text-xs font-medium text-slate-400 hover:text-white border border-slate-800 transition">
            🍄 ปกติ
          </button>
          <button onclick="App.setStockFilter('LOW')" data-filter="LOW" class="stock-filter-btn px-3 py-1.5 rounded-xl text-xs font-medium text-slate-400 hover:text-yellow-400 border border-slate-800 transition flex items-center gap-1">
            <span>⚠️</span> ใกล้หมด
          </button>
          <button onclick="App.setStockFilter('OUT')" data-filter="OUT" class="stock-filter-btn px-3 py-1.5 rounded-xl text-xs font-medium text-slate-400 hover:text-red-400 border border-slate-800 transition flex items-center gap-1">
            <span>💀</span> หมดสต็อก
          </button>
        </div>

        <!-- Right Admin & View Toggle Buttons -->
        <div class="flex items-center flex-wrap gap-2 justify-end">
          
          <!-- View Toggle (Grid vs Table) -->
          <div class="flex items-center bg-slate-950/80 p-1 rounded-xl border border-slate-800">
            <button id="viewModeGridBtn" onclick="App.setViewMode('grid')" class="p-2 rounded-lg bg-yellow-500/20 text-yellow-400 border border-yellow-500/40 shadow" title="Mario Cards View">
              <i data-lucide="layout-grid" class="w-4 h-4"></i>
            </button>
            <button id="viewModeTableBtn" onclick="App.setViewMode('table')" class="p-2 rounded-lg bg-slate-800 text-slate-400 hover:text-white" title="Enterprise Table View">
              <i data-lucide="table" class="w-4 h-4"></i>
            </button>
          </div>

          <!-- Enterprise Export Excel -->
          <button onclick="AdminController.exportInventoryToExcel()" class="px-3 py-2 bg-slate-800/90 hover:bg-slate-700 text-slate-200 border border-slate-700 rounded-xl text-xs font-semibold flex items-center gap-1.5 transition active:scale-95 shadow-sm" title="ส่งออกข้อมูลสแปร์พาร์ทเป็น Excel">
            <i data-lucide="file-spreadsheet" class="w-4 h-4 text-emerald-400"></i>
            <span>Excel</span>
          </button>

          <!-- Manage Requesters Button -->
          <button onclick="AdminController.openRequesterModal()" class="px-3 py-2 bg-slate-800/90 hover:bg-slate-700 text-slate-200 border border-slate-700 rounded-xl text-xs font-semibold flex items-center gap-1.5 transition active:scale-95 shadow-sm" title="เปิดหน้าตัวละคร & รายชื่อคนเบิก">
            <span>🍄</span>
            <span>หน้าตัวละครคนเบิก</span>
          </button>

          <!-- Add Part (Admin Protected) -->
          <button onclick="AdminController.openAddPartModal()" class="px-4 py-2 bg-gradient-to-r from-red-600 to-red-700 hover:from-red-500 hover:to-red-600 text-white rounded-xl text-xs font-bold flex items-center gap-1.5 transition active:scale-95 shadow-lg border border-red-500/40">
            <span class="mario-block-badge px-1 py-0.2 rounded text-[10px]">[?]</span>
            <span>คีย์พาร์ทใหม่</span>
          </button>

          <!-- Settings & Backup -->
          <button onclick="AdminController.openSettingsModal()" class="p-2 bg-slate-800/90 hover:bg-slate-700 text-slate-300 border border-slate-700 rounded-xl transition shadow-sm" title="ตั้งค่าระบบ & สำรองข้อมูล">
            <i data-lucide="settings" class="w-4 h-4"></i>
          </button>

        </div>
      </div>

      <!-- Category Filter Pills Bar -->
      <div id="categoryFiltersContainer" class="flex items-center space-x-2 overflow-x-auto py-1 scrollbar-none"></div>

      <!-- Result Count Info -->
      <div class="flex items-center justify-between text-xs text-slate-400 px-1">
        <span id="catalogResultCount">กำลังโหลดข้อมูล...</span>
        <span class="text-slate-500 flex items-center gap-1">
          <i data-lucide="info" class="w-3.5 h-3.5"></i> เสียบเครื่องยิงบาร์โค้ด USB เพื่อค้นหาและเบิกพาร์ททันที
        </span>
      </div>

      <!-- GRID VIEW CONTAINER -->
      <div id="catalogGridView" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-5"></div>

      <!-- TABLE VIEW CONTAINER (No Price Column) -->
      <div id="catalogTableView" class="hidden overflow-x-auto luxury-card rounded-2xl">
        <table class="w-full text-left border-collapse">
          <thead>
            <tr class="border-b border-slate-800 bg-slate-950/70 text-xs font-bold text-slate-400 uppercase tracking-wider">
              <th class="py-3 px-4">Part ID</th>
              <th class="py-3 px-4">ชื่ออะไหล่ / สเปก</th>
              <th class="py-3 px-4">หมวดหมู่</th>
              <th class="py-3 px-4">ตำแหน่ง (Location)</th>
              <th class="py-3 px-4">คงเหลือ (Stock)</th>
              <th class="py-3 px-4 text-center">จุดเตือนขั้นต่ำ</th>
              <th class="py-3 px-4 text-right">ดำเนินการ</th>
            </tr>
          </thead>
          <tbody id="catalogTableBody"></tbody>
        </table>
      </div>

      <!-- Empty State -->
      <div id="catalogEmptyState" class="hidden text-center py-16 luxury-card rounded-2xl">
        <div class="w-16 h-16 mx-auto mb-4 rounded-2xl bg-slate-800 flex items-center justify-center text-3xl">
          🍄
        </div>
        <h3 class="text-lg font-bold text-white mb-1">ไม่พบรายการสแปร์พาร์ทที่ค้นหา</h3>
        <p class="text-xs text-slate-400 mb-4">ลองเปลี่ยนคำค้นหา หรือรีเซ็ตตัวกรองหมวดหมู่</p>
        <button onclick="App.filterByCategory('ALL'); document.getElementById('globalSearchInput').value=''; App.searchTerm=''; App.renderCatalog();" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-yellow-400 rounded-xl text-xs font-semibold border border-yellow-500/30">
          ⭐ ล้างตัวกรองค้นหาทั้งหมด
        </button>
      </div>

    </div>

    <!-- ========================================== -->
    <!-- TAB 2: AUDIT LOGS SECTION -->
    <!-- ========================================== -->
    <div id="logsSection" class="hidden space-y-4">
      <div class="flex flex-col md:flex-row items-start md:items-center justify-between gap-4 luxury-card p-4 rounded-2xl">
        <div>
          <h2 class="text-lg font-bold text-white flex items-center gap-2">
            <span>📜</span>
            <span>ประวัติการเบิกจ่ายสแปร์พาร์ท (Audit Trail & Requisition History)</span>
          </h2>
          <p class="text-xs text-slate-400">บันทึกทุกรายการตัดสต็อกอัตโนมัติ ผู้เบิก และยอดคงเหลือย้อนหลัง</p>
        </div>
        <div class="flex items-center space-x-2">
          <button onclick="AdminController.exportLogsToExcel()" class="px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white rounded-xl text-xs font-bold flex items-center gap-2 shadow-lg transition active:scale-95 border border-emerald-500/40">
            <i data-lucide="download" class="w-4 h-4"></i>
            <span>ส่งออกประวัติเป็น Excel (.xlsx)</span>
          </button>
        </div>
      </div>

      <!-- Logs Table -->
      <div class="overflow-x-auto luxury-card rounded-2xl">
        <table class="w-full text-left border-collapse">
          <thead>
            <tr class="border-b border-slate-800 bg-slate-950/70 text-xs font-bold text-slate-400 uppercase tracking-wider">
              <th class="py-3 px-4">เลขที่ใบเบิก</th>
              <th class="py-3 px-4">วัน-เวลา</th>
              <th class="py-3 px-4">รหัส / รายการพาร์ท</th>
              <th class="py-3 px-4 text-center">จำนวนที่เบิก</th>
              <th class="py-3 px-4 text-center">คงเหลือหลังเบิก</th>
              <th class="py-3 px-4">ผู้เบิก (ตัวละคร)</th>
              <th class="py-3 px-4">เครื่องจักร / วัตถุประสงค์</th>
              <th class="py-3 px-4 text-right">หลักฐาน</th>
            </tr>
          </thead>
          <tbody id="logsTableBody"></tbody>
        </table>
      </div>

      <div id="logsEmptyState" class="hidden text-center py-16 luxury-card rounded-2xl">
        <div class="text-3xl mb-2">📜</div>
        <p class="text-sm text-slate-400">ยังไม่มีประวัติการเบิกพาร์ทในระบบ</p>
      </div>
    </div>

  </main>

  <!-- ========================================== -->
  <!-- FOOTER -->
  <!-- ========================================== -->
  <footer class="border-t border-slate-800 bg-mario-surface py-6 px-4 text-center text-xs text-slate-400">
    <div class="max-w-7xl mx-auto flex flex-col md:flex-row items-center justify-between gap-3">
      <div class="flex items-center space-x-2">
        <span class="font-luxury font-black text-yellow-400 tracking-wider">MARIO PARTS HUB</span>
        <span>• Enterprise Spare Part Inventory & Automated Stock Requisition System</span>
      </div>
      <div>
        <span>ธีมเรียบหรูสไตล์เกมมาริโอ้ (Mario Character Edition) • มาตรฐานความปลอดภัยระดับองค์กร</span>
      </div>
    </div>
  </footer>

  <!-- ================================================================= -->
  <!-- MODAL 1: REQUISITION / CHECKOUT MODAL (เบิกพาร์ท & ตัดสต็อกอัตโนมัติ) -->
  <!-- ================================================================= -->
  <div id="requisitionModal" class="fixed inset-0 z-50 hidden modal-backdrop items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-700/80 rounded-2xl max-w-lg w-full overflow-hidden shadow-2xl animate-in fade-in zoom-in-95 duration-200">
      
      <!-- Modal Header -->
      <div class="p-5 border-b border-slate-800 flex items-center justify-between bg-slate-950/70">
        <div class="flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-gradient-to-r from-red-600 to-red-700 flex items-center justify-center text-xl text-white shadow-lg border border-red-400/40">
            🪙
          </div>
          <div>
            <h3 class="font-bold text-white text-base">ทำรายการเบิกสแปร์พาร์ท (Coin Checkout)</h3>
            <p class="text-xs text-slate-400">ระบบจะทำการตัดยอดสต็อกในคลังทันทีอัตโนมัติ</p>
          </div>
        </div>
        <button onclick="App.closeRequisitionModal()" class="text-slate-400 hover:text-white p-1 rounded-lg">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>

      <!-- Modal Body -->
      <div class="p-6 space-y-5">
        
        <!-- Part Information Card -->
        <div class="flex items-center space-x-4 p-3 bg-slate-950/80 border border-slate-800 rounded-xl">
          <img id="reqModalPartImg" src="" alt="Part" class="w-16 h-16 rounded-xl object-cover bg-slate-800 flex-shrink-0 border border-slate-700" />
          <div class="flex-1 min-w-0">
            <div class="flex items-center space-x-2">
              <span id="reqModalPartId" class="mario-block-badge font-mono text-xs font-black px-2 py-0.5 rounded shadow"></span>
              <span class="text-xs text-slate-400">คงเหลือ:</span>
              <span id="reqModalAvailableStock" class="text-xs font-bold text-emerald-400"></span>
            </div>
            <h4 id="reqModalPartName" class="font-bold text-white text-sm truncate mt-1"></h4>
            <p id="reqModalPartSpec" class="text-xs text-slate-400 truncate"></p>
          </div>
        </div>

        <!-- Requester Selection with Character Preview -->
        <div>
          <label class="block text-xs font-bold text-slate-300 mb-1.5 flex items-center justify-between">
            <span>ผู้ขอเบิกพาร์ท (Requester Character) *</span>
            <button onclick="AdminController.openRequesterModal()" class="text-xs text-yellow-400 hover:underline flex items-center gap-1">
              <span>🍄</span>
              <span>จัดการตัวละคร</span>
            </button>
          </label>
          <select id="reqModalRequesterSelect" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2.5 text-sm text-white focus:outline-none focus:border-yellow-500 focus:ring-1 focus:ring-yellow-500"></select>
          <div id="reqModalCharPreview" class="mt-2.5"></div>
        </div>

        <!-- Quantity Picker -->
        <div>
          <label class="block text-xs font-bold text-slate-300 mb-1.5">
            จำนวนที่ต้องการเบิก (<span id="reqModalUnit">ชิ้น</span>) *
          </label>
          <div class="flex items-center space-x-3">
            <button onclick="App.stepRequisitionQty(-1)" class="w-11 h-11 bg-slate-800 hover:bg-slate-700 text-white rounded-xl font-bold text-lg flex items-center justify-center border border-slate-700 active:scale-95 transition">
              -
            </button>
            <input 
              type="number" 
              id="reqModalQtyInput" 
              min="1" 
              value="1" 
              oninput="App.calculateRemainingPreview()" 
              class="flex-1 text-center font-bold text-xl bg-slate-950 border border-slate-700 rounded-xl py-2 text-white focus:outline-none focus:border-yellow-500"
            />
            <button onclick="App.stepRequisitionQty(1)" class="w-11 h-11 bg-slate-800 hover:bg-slate-700 text-white rounded-xl font-bold text-lg flex items-center justify-center border border-slate-700 active:scale-95 transition">
              +
            </button>
          </div>
          <div id="reqModalRemainingPreview" class="text-xs text-slate-400 mt-1.5 text-right font-medium"></div>
        </div>

        <!-- Machine Reference / Station -->
        <div>
          <label class="block text-xs font-bold text-slate-300 mb-1">
            เครื่องจักร / สายการผลิตเป้าหมาย (Machine / Station Ref)
          </label>
          <input 
            type="text" 
            id="reqModalMachine" 
            placeholder="เช่น เครื่องซีล A-02, Line 3 Main Conveyor, แขนกล Robot 1" 
            class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-xs md:text-sm text-white focus:outline-none focus:border-yellow-500"
          />
        </div>

        <!-- Remarks -->
        <div>
          <label class="block text-xs font-bold text-slate-300 mb-1">
            หมายเหตุ / สาเหตุการเปลี่ยน (Remarks / PM / Breakdown)
          </label>
          <input 
            type="text" 
            id="reqModalRemarks" 
            placeholder="เช่น ตลับลูกปืนแตก, แผน PM ประจำเดือน, สายพานหมดสภาพ" 
            class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-xs md:text-sm text-white focus:outline-none focus:border-yellow-500"
          />
        </div>

      </div>

      <!-- Modal Footer -->
      <div class="p-4 bg-slate-950/80 border-t border-slate-800 flex items-center justify-end space-x-3">
        <button onclick="App.closeRequisitionModal()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold transition">
          ยกเลิก
        </button>
        <button onclick="App.submitRequisition()" class="px-6 py-2.5 bg-gradient-to-r from-red-600 to-red-700 hover:from-red-500 hover:to-red-600 text-white rounded-xl text-xs font-bold flex items-center space-x-2 shadow-lg transition active:scale-95 border border-red-500/40">
          <span>🪙</span>
          <span>ยืนยันการเบิก & ตัดสต็อก</span>
        </button>
      </div>

    </div>
  </div>

  <!-- ================================================================= -->
  <!-- MODAL 2: PRINTABLE REQUISITION SLIP / VOUCHER (ใบเบิกพาร์ท) -->
  <!-- ================================================================= -->
  <div id="printModal" class="fixed inset-0 z-50 hidden modal-backdrop items-center justify-center p-4">
    <div class="bg-white text-slate-900 rounded-2xl max-w-lg w-full overflow-hidden shadow-2xl p-6 md:p-8 space-y-5">
      
      <!-- Slip Header -->
      <div class="border-b-2 border-dashed border-slate-300 pb-4 text-center">
        <div class="text-xl font-black font-luxury text-red-600 tracking-wider">MARIO PARTS ENTERPRISE</div>
        <div class="text-xs font-bold text-slate-800 uppercase tracking-widest mt-0.5">ใบเบิกสแปร์พาร์ท / Spare Part Requisition Voucher</div>
        <div class="text-[11px] text-slate-500 mt-1">เลขที่ใบเบิก: <span id="slipVoucherNo" class="font-mono font-bold text-slate-900"></span></div>
        <div class="text-[11px] text-slate-500">วันเวลา: <span id="slipDateTime"></span></div>
      </div>

      <!-- Slip Details Table -->
      <div class="space-y-3 text-xs">
        <div class="flex justify-between border-b border-slate-200 py-1">
          <span class="text-slate-500">ผู้ขอเบิก:</span>
          <span id="slipRequester" class="font-bold text-slate-900"></span>
        </div>
        <div class="flex justify-between border-b border-slate-200 py-1">
          <span class="text-slate-500">แผนก:</span>
          <span id="slipDept" class="font-bold text-slate-900"></span>
        </div>
        <div class="flex justify-between border-b border-slate-200 py-1">
          <span class="text-slate-500">รหัสพาร์ท:</span>
          <span id="slipPartId" class="font-mono font-bold text-red-600"></span>
        </div>
        <div class="flex justify-between border-b border-slate-200 py-1">
          <span class="text-slate-500">ชื่อรายการ:</span>
          <span id="slipPartName" class="font-bold text-slate-900 text-right max-w-[240px]"></span>
        </div>
        <div class="flex justify-between border-b border-slate-200 py-1 bg-yellow-50 p-2 rounded-lg">
          <span class="font-bold text-slate-800">จำนวนที่เบิก:</span>
          <span id="slipQty" class="font-black text-red-600 text-sm"></span>
        </div>
        <div class="flex justify-between border-b border-slate-200 py-1">
          <span class="text-slate-500">สต็อกคงเหลือในคลัง:</span>
          <span id="slipRemaining" class="font-bold text-slate-800"></span>
        </div>
        <div class="flex justify-between border-b border-slate-200 py-1">
          <span class="text-slate-500">เครื่องจักรเป้าหมาย:</span>
          <span id="slipMachine" class="font-bold text-slate-900"></span>
        </div>
        <div class="flex justify-between border-b border-slate-200 py-1">
          <span class="text-slate-500">หมายเหตุ:</span>
          <span id="slipRemarks" class="text-slate-800"></span>
        </div>
      </div>

      <!-- Barcode Generation -->
      <div class="text-center pt-2 flex flex-col items-center">
        <svg id="slipBarcode" class="max-w-full"></svg>
      </div>

      <!-- Signature Lines for Enterprise Audit -->
      <div class="grid grid-cols-2 gap-6 pt-6 border-t border-slate-300 text-center text-xs text-slate-600">
        <div>
          <div class="h-10 border-b border-slate-400"></div>
          <div class="mt-1 font-medium">ลงชื่อผู้ขอเบิก</div>
        </div>
        <div>
          <div class="h-10 border-b border-slate-400"></div>
          <div class="mt-1 font-medium">ลงชื่อเจ้าหน้าที่คลังพาร์ท</div>
        </div>
      </div>

      <!-- Action Buttons (Hidden on Print) -->
      <div class="no-print pt-4 flex items-center justify-end space-x-3 border-t border-slate-200">
        <button onclick="App.closePrintModal()" class="px-4 py-2 bg-slate-200 hover:bg-slate-300 text-slate-800 rounded-xl text-xs font-semibold">
          ปิดหน้าต่าง
        </button>
        <button onclick="App.printCurrentVoucher()" class="px-5 py-2 bg-gradient-to-r from-red-600 to-red-700 hover:from-red-500 hover:to-red-600 text-white rounded-xl text-xs font-bold flex items-center space-x-1.5 shadow">
          <i data-lucide="printer" class="w-4 h-4"></i>
          <span>พิมพ์ใบเบิก (Print Slip)</span>
        </button>
      </div>

    </div>
  </div>

  <!-- ================================================================= -->
  <!-- MODAL 3: PART DETAIL & BIN RACK BARCODE LABEL (NO PRICE) -->
  <!-- ================================================================= -->
  <div id="partDetailModal" class="fixed inset-0 z-50 hidden modal-backdrop items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-700/80 rounded-2xl max-w-xl w-full overflow-hidden shadow-2xl">
      <div class="relative h-48 bg-slate-950">
        <img id="detailPartImg" src="" alt="Part Image" class="w-full h-full object-cover filter brightness-85" />
        <div class="absolute inset-0 bg-gradient-to-t from-slate-900 via-transparent to-black/60"></div>
        <button onclick="App.closePartDetailsModal()" class="absolute top-4 right-4 text-white bg-black/60 p-1.5 rounded-full hover:bg-black/90">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
        <div class="absolute bottom-4 left-6 right-6">
          <span id="detailPartId" class="mario-block-badge px-2.5 py-0.5 text-xs font-black rounded shadow"></span>
          <h3 id="detailPartName" class="text-xl font-bold text-white mt-1"></h3>
        </div>
      </div>

      <div class="p-6 space-y-4 text-xs md:text-sm">
        <div class="grid grid-cols-2 gap-4">
          <div class="bg-slate-950 p-3 rounded-xl border border-slate-800">
            <span class="text-slate-400 block text-xs">หมวดหมู่:</span>
            <span id="detailPartCategory" class="font-bold text-white"></span>
          </div>
          <div class="bg-slate-950 p-3 rounded-xl border border-slate-800">
            <span class="text-slate-400 block text-xs">ตำแหน่งจัดเก็บ (Rack/Bin):</span>
            <span id="detailPartLocation" class="font-bold text-yellow-400 font-mono"></span>
          </div>
          <div class="bg-slate-950 p-3 rounded-xl border border-slate-800 col-span-2">
            <span class="text-slate-400 block text-xs">แบรนด์ / ผู้ผลิต:</span>
            <span id="detailPartBrand" class="font-bold text-white"></span>
          </div>
        </div>

        <div class="bg-slate-950 p-3 rounded-xl border border-slate-800">
          <span class="text-slate-400 block text-xs">สเปก / รุ่น (Specification):</span>
          <span id="detailPartSpec" class="font-medium text-slate-200"></span>
        </div>

        <div class="bg-slate-950 p-3 rounded-xl border border-slate-800">
          <span class="text-slate-400 block text-xs">สถานะสต็อกปัจจุบัน:</span>
          <span id="detailPartStock" class="font-bold text-emerald-400"></span>
        </div>

        <div class="bg-slate-950 p-3 rounded-xl border border-slate-800">
          <span class="text-slate-400 block text-xs">คำอธิบายเพิ่มเติม:</span>
          <span id="detailPartDesc" class="text-slate-300"></span>
        </div>

        <!-- Bin Label Barcode -->
        <div class="bg-slate-950 p-4 rounded-xl border border-slate-800 text-center flex flex-col items-center">
          <div class="text-xs text-slate-400 mb-1">บาร์โค้ดติดหน้ากล่อง / ชั้นวาง (Warp Bin Rack Barcode)</div>
          <svg id="detailPartBarcode"></svg>
        </div>
      </div>

      <div class="p-4 bg-slate-950 border-t border-slate-800 flex justify-end space-x-3">
        <button onclick="App.closePartDetailsModal()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold">
          ปิด
        </button>
        <button id="detailModalCheckoutBtn" class="px-6 py-2 bg-gradient-to-r from-red-600 to-red-700 hover:from-red-500 hover:to-red-600 text-white rounded-xl text-xs font-bold flex items-center space-x-1.5 shadow">
          <span>🪙</span>
          <span>เบิกพาร์ทนี้</span>
        </button>
      </div>
    </div>
  </div>

  <!-- ================================================================= -->
  <!-- MODAL 4: ADMIN MASTER PIN AUTHENTICATION -->
  <!-- ================================================================= -->
  <div id="adminPinModal" class="fixed inset-0 z-50 hidden modal-backdrop items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-700/80 rounded-2xl max-w-sm w-full p-6 text-center space-y-4 shadow-2xl animate-in fade-in zoom-in-95">
      <div class="w-14 h-14 bg-red-950/80 border border-yellow-500/40 rounded-2xl flex items-center justify-center mx-auto text-2xl shadow-lg">
        ⭐
      </div>
      <div>
        <h3 class="text-lg font-bold text-white">ระบบรักษาความปลอดภัยผู้ดูแลระบบ</h3>
        <p class="text-xs text-slate-400 mt-1">กรุณากรอกรหัสผ่าน Admin Master PIN เพื่อปลดล็อก (ค่าเริ่มต้น: <span class="font-mono text-yellow-400 font-bold">8888</span>)</p>
      </div>

      <form onsubmit="event.preventDefault(); AdminController.submitPin();" class="space-y-3">
        <input 
          type="password" 
          id="adminPinInput" 
          maxlength="8" 
          placeholder="••••" 
          autocomplete="off"
          class="w-full text-center tracking-widest text-2xl font-mono bg-slate-950 border border-slate-700 rounded-xl py-3 text-white focus:outline-none focus:border-yellow-500"
        />
        <div id="adminPinError" class="text-xs text-red-400 hidden font-semibold"></div>

        <div class="grid grid-cols-2 gap-3 pt-2">
          <button type="button" onclick="AdminController.closePinModal()" class="w-full py-2.5 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-bold transition">
            ยกเลิก
          </button>
          <button type="submit" class="w-full py-2.5 bg-gradient-to-r from-red-600 to-red-700 hover:from-red-500 hover:to-red-600 text-white rounded-xl text-xs font-bold transition shadow-lg border border-red-500/30">
            ปลดล็อก ⭐
          </button>
        </div>
      </form>
    </div>
  </div>

  <!-- ================================================================= -->
  <!-- MODAL 5: ADD / EDIT SPARE PART MODAL (NO PRICE) -->
  <!-- ================================================================= -->
  <div id="partFormModal" class="fixed inset-0 z-50 hidden modal-backdrop items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-700/80 rounded-2xl max-w-2xl w-full max-h-[90vh] flex flex-col overflow-hidden shadow-2xl">
      
      <div class="p-5 border-b border-slate-800 flex items-center justify-between bg-slate-950/70">
        <div class="flex items-center space-x-2.5">
          <div class="w-8 h-8 rounded-xl bg-gradient-to-br from-red-600 to-red-700 flex items-center justify-center text-white">
            <span class="text-xs font-black">[?]</span>
          </div>
          <h3 id="partModalTitle" class="font-bold text-white text-base">เพิ่มรายการสแปร์พาร์ทใหม่</h3>
        </div>
        <button onclick="AdminController.closePartModal()" class="text-slate-400 hover:text-white p-1">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>

      <form id="partForm" onsubmit="AdminController.savePartFromForm(event)" class="overflow-y-auto p-6 space-y-4 text-xs md:text-sm flex-1">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="block font-bold text-slate-300 mb-1">รหัสพาร์ท (Part ID / SKU) *</label>
            <input type="text" id="partFormId" required placeholder="เช่น EP-MEC-011, PIN-SOC-020, FIX-GUI-015" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white uppercase font-mono focus:border-yellow-500" />
          </div>
          <div>
            <label class="block font-bold text-slate-300 mb-1">หมวดหมู่ (Category) *</label>
            <select id="partFormCategory" required class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-yellow-500">
              <option value="Epson Robot Mechanics">Epson Robot Mechanics (กลไกแขนกล & สายพาน Robot Epson)</option>
              <option value="Epson Controller & Cables">Epson Controller & Cables (คอนโทรลเลอร์ & สายเคเบิล M/C)</option>
              <option value="Test Socket Pins & Probes">Test Socket Pins & Probes (ซ็อกเก็ตพิน & โพรบเทส OEMCM)</option>
              <option value="Test Fixture & Alignment">Test Fixture & Alignment (จิ๊กเทส & พินไกด์ระบุตำแหน่ง)</option>
              <option value="Pneumatics & Vacuum">Pneumatics & Vacuum (ระบบลม & ยางดูดสุญญากาศ ESD)</option>
              <option value="Sensors & Vision">Sensors & Vision (เซ็นเซอร์ตรวจจับ & กล้อง Vision)</option>
              <option value="Consumables & Maintenance">Consumables & Maintenance (วัสดุสิ้นเปลือง & จาระบี Harmonic)</option>
            </select>
          </div>
        </div>

        <div>
          <label class="block font-bold text-slate-300 mb-1">ชื่ออะไหล่ / สแปร์พาร์ท (Part Name) *</label>
          <input type="text" id="partFormName" required placeholder="เช่น OMRON Proximity Sensor E2B" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-yellow-500" />
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="block font-bold text-slate-300 mb-1">สเปก / รุ่น (Spec & Model)</label>
            <input type="text" id="partFormSpec" placeholder="เช่น M12 PNP NO, 10-30VDC" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-yellow-500" />
          </div>
          <div>
            <label class="block font-bold text-slate-300 mb-1">ตำแหน่งจัดเก็บ (Rack / Bin Location) *</label>
            <input type="text" id="partFormLocation" required placeholder="เช่น Warp Rack E-02-B, ตู้ A ชั้น 3" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-yellow-500" />
          </div>
        </div>

        <div class="grid grid-cols-3 gap-4">
          <div>
            <label class="block font-bold text-slate-300 mb-1">จำนวนคงเหลือเริ่มต้น *</label>
            <input type="number" id="partFormQty" min="0" value="10" required class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-yellow-500" />
          </div>
          <div>
            <label class="block font-bold text-slate-300 mb-1">จุดเตือนขั้นต่ำ (Min Alert) *</label>
            <input type="number" id="partFormMin" min="0" value="5" required class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-yellow-500" />
          </div>
          <div>
            <label class="block font-bold text-slate-300 mb-1">หน่วยนับ (Unit)</label>
            <input type="text" id="partFormUnit" value="ชิ้น" placeholder="ชิ้น, ตลับ, ตัว" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-yellow-500" />
          </div>
        </div>

        <div>
          <label class="block font-bold text-slate-300 mb-1">ยี่ห้อ / ผู้จัดจำหน่าย (Brand)</label>
          <input type="text" id="partFormBrand" placeholder="เช่น SMC, OMRON, SKF, Festo" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-yellow-500" />
        </div>

        <div>
          <label class="block font-bold text-slate-300 mb-1">ลิงก์รูปภาพ (Image URL)</label>
          <input type="url" id="partFormImage" placeholder="https://..." class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-yellow-500" />
        </div>

        <div>
          <label class="block font-bold text-slate-300 mb-1">คำอธิบายเพิ่มเติม / เครื่องจักรที่ใช้</label>
          <textarea id="partFormDesc" rows="2" placeholder="รายละเอียดการใช้งาน..." class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-yellow-500"></textarea>
        </div>

        <div class="pt-4 border-t border-slate-800 flex justify-end space-x-3">
          <button type="button" onclick="AdminController.closePartModal()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold">
            ยกเลิก
          </button>
          <button type="submit" class="px-6 py-2 bg-gradient-to-r from-red-600 to-red-700 hover:from-red-500 hover:to-red-600 text-white rounded-xl text-xs font-bold transition shadow-lg border border-red-500/30">
            บันทึกข้อมูลพาร์ท
          </button>
        </div>
      </form>
    </div>
  </div>

  <!-- ================================================================= -->
  <!-- MODAL 6: QUICK STOCK ADJUSTMENT (1-UP STOCK IN) -->
  <!-- ================================================================= -->
  <div id="stockAdjustModal" class="fixed inset-0 z-50 hidden modal-backdrop items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-700/80 rounded-2xl max-w-md w-full p-6 space-y-4 shadow-2xl">
      <div class="flex items-center space-x-3 border-b border-slate-800 pb-3">
        <div class="w-10 h-10 rounded-xl bg-emerald-950/80 text-emerald-400 flex items-center justify-center text-xl border border-emerald-500/40">
          🟢
        </div>
        <div>
          <h3 class="font-bold text-white text-base">1-Up รับเข้าพาร์ท / ปรับปรุงยอดสต็อก</h3>
          <p id="adjustPartTitle" class="text-xs text-slate-400 font-mono"></p>
        </div>
      </div>

      <div class="space-y-3 text-xs md:text-sm">
        <div class="flex justify-between p-3 bg-slate-950 rounded-xl border border-slate-800">
          <span class="text-slate-400">สต็อกคงเหลือปัจจุบัน:</span>
          <span id="adjustCurrentStock" class="font-bold text-white"></span>
        </div>

        <div>
          <label class="block font-bold text-slate-300 mb-1">
            จำนวนที่ต้องการปรับ (+เพิ่ม หรือ -ลด) *
          </label>
          <input 
            type="number" 
            id="adjustDeltaInput" 
            placeholder="เช่น +20 หรือ -2" 
            class="w-full text-center text-lg font-bold bg-slate-950 border border-slate-700 rounded-xl py-2.5 text-white focus:outline-none focus:border-emerald-500"
          />
        </div>

        <div>
          <label class="block font-bold text-slate-300 mb-1">เหตุผลการปรับปรุงสต็อก *</label>
          <input 
            type="text" 
            id="adjustReasonInput" 
            value="รับของเข้าคลังตามใบสั่งซื้อ" 
            class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-emerald-500 text-xs"
          />
        </div>
      </div>

      <div class="flex justify-end space-x-3 pt-3 border-t border-slate-800">
        <button onclick="AdminController.closeStockAdjustModal()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold">
          ยกเลิก
        </button>
        <button onclick="AdminController.saveStockAdjust()" class="px-5 py-2 bg-emerald-600 hover:bg-emerald-700 text-white rounded-xl text-xs font-bold transition shadow border border-emerald-500/40">
          บันทึกการปรับปรุงสต็อก 🟢
        </button>
      </div>
    </div>
  </div>

  <!-- ================================================================= -->
  <!-- MODAL 6.5: RETURN PART TO STOCK (คืนพาร์ทเข้าที่เดิมจากการเบิกผิด) -->
  <!-- ================================================================= -->
  <div id="returnPartModal" class="fixed inset-0 z-50 hidden modal-backdrop items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-700/80 rounded-2xl max-w-lg w-full overflow-hidden shadow-2xl animate-in fade-in zoom-in-95">
      
      <!-- Modal Header -->
      <div class="p-5 border-b border-slate-800 flex items-center justify-between bg-slate-950/80">
        <div class="flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-emerald-600 to-emerald-800 flex items-center justify-center text-xl shadow-lg border border-emerald-400/40">
            ↩️
          </div>
          <div>
            <h3 class="font-bold text-white text-base flex items-center gap-2">
              <span>คืนสแปร์พาร์ทเข้าที่เดิม (Return to Stock)</span>
              <span class="mario-block-badge text-[10px] px-1.5 py-0.2 rounded font-mono font-black">ADMIN VOID</span>
            </h3>
            <p class="text-xs text-slate-400">แก้ไขกรณีเบิกพาร์ทผิด หรือคืนอะไหล่กลับเข้าชั้นวางเดิม</p>
          </div>
        </div>
        <button onclick="AdminController.closeReturnModal()" class="text-slate-400 hover:text-white p-1 rounded-lg">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>

      <!-- Modal Body -->
      <div class="p-6 space-y-4 text-xs md:text-sm">
        
        <!-- Requisition Reference Details -->
        <div class="p-4 bg-slate-950/90 rounded-2xl border border-slate-800 space-y-2">
          <div class="flex justify-between items-center text-xs">
            <span class="text-slate-400">อ้างอิงเลขที่ใบเบิก:</span>
            <span id="returnModalVoucherNo" class="font-mono font-bold text-yellow-400"></span>
          </div>
          <div class="flex justify-between items-center text-xs">
            <span class="text-slate-400">รายการสแปร์พาร์ท:</span>
            <span id="returnModalPartInfo" class="font-bold text-white text-right max-w-[240px] truncate"></span>
          </div>
          <div class="flex justify-between items-center text-xs pt-1 border-t border-slate-800/80">
            <span class="text-slate-400">ตำแหน่งจัดเก็บที่จะคืนเข้า:</span>
            <span id="returnModalLocation" class="font-mono font-bold text-emerald-400"></span>
          </div>
          <div class="text-[11px] text-slate-400 pt-1 border-t border-slate-800/80">
            <span class="block text-slate-500">ข้อมูลผู้ที่เบิกผิด:</span>
            <span id="returnModalOriginalRequester" class="text-slate-300 font-medium"></span>
          </div>
        </div>

        <!-- Quantity to Return -->
        <div>
          <label class="block font-bold text-slate-300 mb-1">
            จำนวนที่ต้องการรับคืนเข้าคลัง (<span id="returnModalUnit">ชิ้น</span>) *
          </label>
          <input 
            type="number" 
            id="returnModalQtyInput" 
            min="1" 
            class="w-full text-center font-bold text-xl bg-slate-950 border border-slate-700 rounded-xl py-2 text-white focus:outline-none focus:border-emerald-500"
          />
          <div class="text-[11px] text-slate-400 mt-1">ระบบจะเพิ่มจำนวนสต็อกชิ้นส่วนนี้กลับเข้าคลังทันที</div>
        </div>

        <!-- Return Reason -->
        <div>
          <label class="block font-bold text-slate-300 mb-1">สาเหตุการรับคืน (Return Reason) *</label>
          <select id="returnModalReasonSelect" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white focus:border-emerald-500 text-xs mb-2">
            <option value="เบิกผิดรุ่น / ผิดสเปก">เบิกผิดรุ่น / ผิดสเปก (Wrong Spec / Model)</option>
            <option value="เบิกเกินจำนวนที่ใช้จริง">เบิกเกินจำนวนที่ใช้จริง (Over-requisitioned)</option>
            <option value="ยกเลิกงานซ่อมบำรุง / ไม่ได้ใช้งาน">ยกเลิกงานซ่อมบำรุง / ไม่ได้ใช้งาน (Cancelled Maintenance)</option>
            <option value="หยิบพาร์ทผิดจากชั้นวาง">หยิบพาร์ทผิดจากชั้นวาง (Wrong Bin Pick)</option>
            <option value="คีย์จำนวนตัวเลขในระบบผิด">คีย์จำนวนตัวเลขในระบบผิด (Key-in Typing Error)</option>
            <option value="อื่นๆ">อื่นๆ (ระบุเพิ่มเติมด้านล่าง)</option>
          </select>
          <input 
            type="text" 
            id="returnModalReasonCustom" 
            placeholder="รายละเอียดเพิ่มเติม (เช่น เปลี่ยนไปใช้ PIN รุ่น SP-02 แทน)..." 
            class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white text-xs focus:border-emerald-500"
          />
        </div>

      </div>

      <!-- Modal Footer -->
      <div class="p-4 bg-slate-950 border-t border-slate-800 flex justify-end space-x-3">
        <button onclick="AdminController.closeReturnModal()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold">
          ยกเลิก
        </button>
        <button onclick="AdminController.submitReturnPart()" class="px-5 py-2.5 bg-gradient-to-r from-emerald-600 to-emerald-700 hover:from-emerald-500 hover:to-emerald-600 text-white rounded-xl text-xs font-bold shadow-lg transition active:scale-95 border border-emerald-500/40 flex items-center space-x-1.5">
          <span>🟢</span>
          <span>ยืนยันรับคืนเข้าที่เดิม & เพิ่มสต็อก</span>
        </button>
      </div>

    </div>
  </div>

  <!-- ================================================================= -->
  <!-- MODAL 7: REQUESTER MANAGEMENT (MARIO CHARACTER ROSTER SCREEN) -->
  <!-- ================================================================= -->
  <div id="requesterManagementModal" class="fixed inset-0 z-50 hidden modal-backdrop items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-700/80 rounded-2xl max-w-3xl w-full max-h-[90vh] flex flex-col overflow-hidden shadow-2xl">
      
      <!-- Modal Header -->
      <div class="p-5 border-b border-slate-800 flex items-center justify-between bg-slate-950/80">
        <div class="flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-red-600 to-red-700 flex items-center justify-center text-xl shadow-lg border border-red-400/40">
            🍄
          </div>
          <div>
            <h3 class="font-bold text-white text-base flex items-center gap-2">
              <span>หน้าตัวละคร & รายชื่อคนเบิกพาร์ท</span>
              <span class="mario-block-badge text-[10px] px-2 py-0.5 rounded font-mono font-black">CHARACTER ROSTER</span>
            </h3>
            <p class="text-xs text-slate-400">เลือกและจัดการตัวละครสำหรับทีมงานผู้มีสิทธิ์เบิกสแปร์พาร์ท</p>
          </div>
        </div>
        <button onclick="AdminController.closeRequesterModal()" class="text-slate-400 hover:text-white p-1">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>

      <!-- Modal Body -->
      <div class="overflow-y-auto p-6 space-y-6 flex-1 text-xs md:text-sm">
        
        <!-- Add New Requester Panel with Character Picker -->
        <div class="bg-slate-950 border border-slate-800 p-5 rounded-2xl space-y-4">
          <h4 class="font-bold text-white text-xs uppercase tracking-wider flex items-center gap-2">
            <span class="text-base">⭐</span>
            <span>เพิ่มรายชื่อผู้เบิกใหม่ (เลือกหน้าตัวละครที่ชอบ)</span>
          </h4>

          <!-- Visual Character Picker Grid -->
          <div>
            <label class="block text-[11px] font-semibold text-slate-400 mb-2">คลิกเพื่อเลือกหน้าตัวละคร:</label>
            <div id="characterPickerContainer" class="grid grid-cols-4 sm:grid-cols-8 gap-2"></div>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-3 pt-2">
            <input type="text" id="newReqId" placeholder="รหัสพนักงาน (เช่น EMP-009)" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-white font-mono text-xs focus:border-yellow-500" />
            <input type="text" id="newReqName" placeholder="ชื่อ-นามสกุล (ชื่อเล่น)" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-white text-xs focus:border-yellow-500" />
            <input type="text" id="newReqRole" placeholder="ตำแหน่ง (เช่น ช่างซ่อมบำรุง)" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-white text-xs focus:border-yellow-500" />
            <input type="text" id="newReqDept" placeholder="แผนก (เช่น ไฟฟ้า, ซ่อมบำรุง)" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-white text-xs focus:border-yellow-500" />
          </div>

          <div class="flex items-center justify-between gap-3 pt-1">
            <input type="text" id="newReqPhone" placeholder="เบอร์โทรติดต่อ (อุปกรณ์เสริม)" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-white text-xs flex-1 max-w-xs focus:border-yellow-500" />
            <button onclick="AdminController.addNewRequester()" class="px-5 py-2.5 bg-gradient-to-r from-red-600 to-red-700 hover:from-red-500 hover:to-red-600 text-white rounded-xl text-xs font-bold transition flex items-center justify-center space-x-1.5 border border-red-500/40 shadow-lg">
              <i data-lucide="plus" class="w-4 h-4"></i>
              <span>บันทึกตัวละครผู้เบิก ⭐</span>
            </button>
          </div>
        </div>

        <!-- Requester Character Cards Roster -->
        <div class="space-y-3">
          <div class="text-xs font-bold text-slate-300 flex items-center justify-between">
            <span>ทำเนียบหน้าตัวละครผู้เบิกพาร์ท (Current Character Roster):</span>
          </div>
          <div id="requestersListContainer" class="grid grid-cols-1 md:grid-cols-2 gap-3"></div>
        </div>
      </div>

      <div class="p-4 bg-slate-950 border-t border-slate-800 flex justify-end">
        <button onclick="AdminController.closeRequesterModal()" class="px-6 py-2 bg-slate-800 hover:bg-slate-700 text-slate-200 rounded-xl text-xs font-semibold">
          เสร็จสิ้น
        </button>
      </div>

    </div>
  </div>

  <!-- ================================================================= -->
  <!-- MODAL 8: SETTINGS & BACKUP MODAL -->
  <!-- ================================================================= -->
  <div id="settingsModal" class="fixed inset-0 z-50 hidden modal-backdrop items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-700/80 rounded-2xl max-w-lg w-full p-6 space-y-5 shadow-2xl">
      <div class="flex items-center justify-between border-b border-slate-800 pb-3">
        <h3 class="font-bold text-white text-base flex items-center gap-2">
          <i data-lucide="settings" class="w-5 h-5 text-yellow-400"></i>
          <span>ตั้งค่าระบบ & สำรองข้อมูล (Backup & Security)</span>
        </h3>
        <button onclick="AdminController.closeSettingsModal()" class="text-slate-400 hover:text-white p-1">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>

      <div class="space-y-4 text-xs md:text-sm">
        <div>
          <label class="block font-bold text-slate-300 mb-1">ชื่อองค์กร / โรงงาน</label>
          <input type="text" id="settingsCompanyName" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white" />
        </div>

        <div>
          <label class="block font-bold text-slate-300 mb-1">เปลี่ยนรหัสผ่าน Admin Master PIN ใหม่</label>
          <input type="password" id="settingsNewPin" placeholder="เว้นว่างไว้หากไม่ต้องการเปลี่ยน" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-white" />
        </div>

        <div class="pt-3 border-t border-slate-800 space-y-3">
          <div class="font-bold text-slate-300">สำรองข้อมูล & กู้คืนฐานข้อมูล (Offline Backup):</div>
          <div class="grid grid-cols-2 gap-3">
            <button onclick="AdminController.backupDatabase()" class="py-2.5 px-3 bg-slate-800 hover:bg-slate-700 text-white rounded-xl text-xs font-semibold flex items-center justify-center gap-1.5 border border-slate-700">
              <i data-lucide="download" class="w-4 h-4 text-yellow-400"></i>
              <span>ดาวน์โหลด Backup (JSON)</span>
            </button>

            <label class="py-2.5 px-3 bg-slate-800 hover:bg-slate-700 text-white rounded-xl text-xs font-semibold flex items-center justify-center gap-1.5 border border-slate-700 cursor-pointer">
              <i data-lucide="upload" class="w-4 h-4 text-emerald-400"></i>
              <span>กู้คืนข้อมูล (Restore)</span>
              <input type="file" accept=".json" onchange="AdminController.restoreDatabase(event)" class="hidden" />
            </label>
          </div>
        </div>

        <div class="pt-3 border-t border-slate-800">
          <button onclick="AdminController.resetDemoDataPrompt()" class="w-full py-2 bg-red-950/40 hover:bg-red-900/60 text-red-400 border border-red-800/50 rounded-xl text-xs font-semibold transition">
            🔄 รีเซ็ตข้อมูลกลับสู่ค่ามาตรฐานโรงงาน (Demo Data)
          </button>
        </div>
      </div>

      <div class="flex justify-end space-x-3 pt-3 border-t border-slate-800">
        <button onclick="AdminController.closeSettingsModal()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold">
          ปิด
        </button>
        <button onclick="AdminController.saveSettings()" class="px-5 py-2 bg-gradient-to-r from-red-600 to-red-700 hover:from-red-500 hover:to-red-600 text-white rounded-xl text-xs font-bold shadow border border-red-500/30">
          บันทึกการตั้งค่า
        </button>
      </div>
    </div>
  </div>

  <!-- ================================================================= -->
  <!-- MODAL 9: WEBCAM CAMERA BARCODE SCANNER (Warp Scanner) -->
  <!-- ================================================================= -->
  <div id="cameraScannerModal" class="fixed inset-0 z-50 hidden modal-backdrop items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-700/80 rounded-2xl max-w-md w-full p-6 space-y-4 shadow-2xl text-center">
      <div class="flex items-center justify-between border-b border-slate-800 pb-3">
        <h3 class="font-bold text-white text-base flex items-center gap-2">
          <i data-lucide="scan" class="w-5 h-5 text-yellow-400"></i>
          <span>Warp Scanner: สแกน Barcode / QR Code ผ่านกล้อง</span>
        </h3>
        <button onclick="App.closeCameraScannerModal()" class="text-slate-400 hover:text-white p-1">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>

      <div class="relative bg-black rounded-xl overflow-hidden aspect-square flex items-center justify-center border border-slate-800">
        <div id="cameraScannerTarget" class="w-full h-full"></div>
      </div>
      <p class="text-xs text-slate-400">หันกล้องไปที่บาร์โค้ดหรือ QR Code ของสแปร์พาร์ทเพื่อค้นหาและเปิดหน้าเบิกทันที</p>

      <button onclick="App.closeCameraScannerModal()" class="w-full py-2 bg-slate-800 hover:bg-slate-700 text-white rounded-xl text-xs font-semibold">
        ปิดกล้อง
      </button>
    </div>
  </div>

  <!-- Scripts -->
  <script src="js/sound.js"></script>
  <script src="js/characters.js"></script>
  <script src="js/storage.js"></script>
  <script src="js/scanner.js"></script>
  <script src="js/admin.js"></script>
  <script src="js/app.js"></script>
</body>
</html>
