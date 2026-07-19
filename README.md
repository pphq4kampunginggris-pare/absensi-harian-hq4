<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Absensi & Izin Santri - PP. Hamalatul Qur'an Putri 4</title>
    
    <!-- PWA Primary Customization Metas for Mobile Status Bar Styling -->
    <meta name="theme-color" content="#065f46">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    
    <!-- Favicon Integration using official institute logo -->
    <link rel="icon" type="image/png" href="ChatGPT Image 16 Jul 2026, 22.21.31.png">
    <link rel="apple-touch-icon" href="ChatGPT Image 16 Jul 2026, 22.21.31.png">
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- html2pdf Library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <!-- Supabase JS Library -->
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                    },
                    colors: {
                        emerald: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            200: '#bbf7d0',
                            300: '#86efac',
                            400: '#4ade80',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                            800: '#166534',
                            900: '#14532d',
                        }
                    }
                }
            }
        }
    </script>
    
    <style>
        /* Custom scrollbar for beautiful UI */
        ::-webkit-scrollbar {
            width: 4px;
            height: 4px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #16a34a;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #15803d;
        }
        /* Custom Shake Animation for PIN Error */
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-8px); }
            75% { transform: translateX(8px); }
        }
        .animate-shake {
            animation: shake 0.2s ease-in-out 0s 2;
        }
        /* Hide scrollbar for Chrome, Safari and Opera */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        /* Hide scrollbar for IE, Edge and Firefox */
        .no-scrollbar {
            -ms-overflow-style: none;  /* IE and Edge */
            scrollbar-width: none;  /* Firefox */
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800 font-sans min-h-screen flex flex-col relative">

    <!-- Cloud Sync Loading Overlay Indicator -->
    <div id="loading-overlay" class="fixed inset-0 bg-white/85 backdrop-blur-md z-[9995] flex flex-col items-center justify-center hidden transition-all duration-300">
        <div class="relative w-20 h-20 flex items-center justify-center">
            <div class="animate-spin rounded-full h-16 w-16 border-4 border-emerald-100 border-t-emerald-600"></div>
            <i class="fa-solid fa-cloud text-xl text-emerald-600 absolute animate-pulse"></i>
        </div>
        <p class="text-sm font-extrabold text-emerald-900 mt-4 tracking-wider">SINKRONISASI DATA AWAN...</p>
        <p class="text-[11px] text-emerald-650/80 mt-1">Menyelaraskan data multi-perangkat secara aman dan real-time</p>
    </div>

    <!-- ================= SECURITY LOCK SCREEN OVERLAY ================= -->
    <div id="pin-lock-screen" class="fixed inset-0 bg-gradient-to-br from-emerald-950 via-emerald-900 to-teal-950 text-white z-[9999] flex items-center justify-center p-4 transition-all duration-500 opacity-100">
        <div class="bg-black/25 backdrop-blur-md p-8 rounded-3xl max-w-sm w-full border border-white/10 text-center shadow-2xl relative overflow-hidden">
            <!-- Decorative Dome Accent -->
            <div class="absolute -top-10 left-1/2 -translate-x-1/2 w-40 h-20 bg-emerald-500/10 rounded-full blur-xl pointer-events-none"></div>
            
            <div class="relative z-10">
                <!-- FITUR RAHASIA PENGEMBANG: Klik ikon perisai ini 5 kali untuk bypass PIN -->
                <div onclick="triggerSecretBackdoor()" class="bg-emerald-500/20 w-16 h-16 rounded-2xl flex items-center justify-center mx-auto mb-4 border border-emerald-400/20 text-emerald-300 cursor-pointer select-none active:scale-90 transition-transform" title="Pintu Rahasia Pengembang">
                    <img src="ChatGPT Image 16 Jul 2026, 22.21.31.png" alt="HQ Shield" class="w-10 h-10 object-contain rounded-lg">
                </div>
                
                <h2 class="text-xl font-extrabold tracking-wide uppercase">PP. HAMALATUL QUR'AN</h2>
                <p class="text-xs text-emerald-300 font-medium tracking-widest uppercase mt-0.5">PUTRI 4 AL-KARIMA</p>
                <div class="h-[1px] bg-white/10 my-4 w-1/2 mx-auto"></div>
                
                <!-- Display Dynamic Lock Text -->
                <p id="lock-screen-title" class="text-sm font-semibold text-emerald-100">Memeriksa Kunci Keamanan...</p>
                <p id="lock-screen-desc" class="text-[11px] text-emerald-300/80 mt-1">Gunakan PIN untuk membuka gerbang digital</p>

                <!-- Dot Indicators for PIN Entered -->
                <div id="pin-dots" class="flex justify-center gap-3 my-6">
                    <span class="w-3.5 h-3.5 rounded-full border-2 border-emerald-400/40 transition-all duration-150"></span>
                    <span class="w-3.5 h-3.5 rounded-full border-2 border-emerald-400/40 transition-all duration-150"></span>
                    <span class="w-3.5 h-3.5 rounded-full border-2 border-emerald-400/40 transition-all duration-150"></span>
                    <span class="w-3.5 h-3.5 rounded-full border-2 border-emerald-400/40 transition-all duration-150"></span>
                </div>

                <!-- Custom Security Digital Keypad -->
                <div class="grid grid-cols-3 gap-3 max-w-[260px] mx-auto mt-4 text-sm font-bold text-emerald-50">
                    <button onclick="pressPinKey('1')" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 transition-colors active:scale-95 duration-100">1</button>
                    <button onclick="pressPinKey('2')" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 transition-colors active:scale-95 duration-100">2</button>
                    <button onclick="pressPinKey('3')" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 transition-colors active:scale-95 duration-100">3</button>
                    
                    <button onclick="pressPinKey('4')" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 transition-colors active:scale-95 duration-100">4</button>
                    <button onclick="pressPinKey('5')" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 transition-colors active:scale-95 duration-100">5</button>
                    <button onclick="pressPinKey('6')" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 transition-colors active:scale-95 duration-100">6</button>
                    
                    <button onclick="pressPinKey('7')" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 transition-colors active:scale-95 duration-100">7</button>
                    <button onclick="pressPinKey('8')" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 transition-colors active:scale-95 duration-100">8</button>
                    <button onclick="pressPinKey('9')" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 transition-colors active:scale-95 duration-100">9</button>
                    
                    <button onclick="clearPinInput()" class="bg-rose-500/10 hover:bg-rose-500/20 text-rose-300 py-3 rounded-xl transition-colors active:scale-95 duration-100 font-medium">Batal</button>
                    <button onclick="pressPinKey('0')" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 transition-colors active:scale-95 duration-100">0</button>
                    <button onclick="backspacePinInput()" class="bg-white/5 hover:bg-white/10 py-3 rounded-xl border border-white/5 flex items-center justify-center transition-colors active:scale-95 duration-100">
                        <i class="fa-solid fa-delete-left text-lg"></i>
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Custom Ganti PIN Modal Dialog -->
    <div id="pin-change-modal" class="fixed inset-0 bg-black/60 backdrop-blur-sm flex items-center justify-center p-4 z-[9990] hidden opacity-0 transition-opacity duration-300">
        <div class="bg-white rounded-3xl max-w-sm w-full p-6 shadow-2xl transform scale-95 transition-transform duration-300 border border-gray-100">
            <div class="w-12 h-12 bg-emerald-100 text-emerald-700 rounded-full flex items-center justify-center text-xl mb-4 mx-auto">
                <i class="fa-solid fa-key"></i>
            </div>
            <h4 class="text-base font-bold text-gray-800 text-center mb-2">Ganti PIN Keamanan Aplikasi</h4>
            <p class="text-[11px] text-gray-400 text-center mb-4">Pastikan PIN baru mudah diingat namun aman.</p>
            
            <div class="space-y-3">
                <div>
                    <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">PIN Baru Anda (4-6 Digit)</label>
                    <input type="password" id="input-new-pin" maxlength="6" class="w-full text-center tracking-widest text-lg font-bold border border-gray-200 rounded-xl p-2.5 focus:ring-1 focus:ring-emerald-500 focus:outline-none" placeholder="••••••" />
                </div>
            </div>

            <div class="flex gap-2.5 mt-5">
                <button onclick="closeChangePinModal()" class="bg-gray-100 hover:bg-gray-200 text-gray-600 font-bold px-4 py-2.5 rounded-xl text-xs transition-colors w-1/3">
                    Batal
                </button>
                <button onclick="processChangePin()" class="bg-emerald-600 hover:bg-emerald-700 text-white font-bold px-4 py-2.5 rounded-xl text-xs transition-colors flex-grow shadow-md shadow-emerald-600/10">
                    Simpan PIN Baru
                </button>
            </div>
        </div>
    </div>

    <!-- Header / Navbar -->
    <header class="bg-gradient-to-r from-emerald-800 via-emerald-700 to-teal-800 text-white shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3 md:py-4 flex items-center justify-between gap-3">
            <div class="flex items-center gap-2.5 md:gap-3 text-left">
                <!-- Fitur Rahasia: Klik logo instansi ini 5 kali secara cepat untuk mengganti PIN -->
                <div onclick="triggerSecretPinChange()" class="bg-white/15 p-1 rounded-2xl backdrop-blur-sm border border-white/20 shadow-inner cursor-pointer transition-all duration-300 hover:scale-105 active:scale-95 shrink-0 w-12 h-12 md:w-14 md:h-14 flex items-center justify-center overflow-hidden" title="PP. Hamalatul Qur'an (Klik 5x untuk ganti PIN)">
                    <img src="ChatGPT Image 16 Jul 2026, 22.21.31.png" alt="Logo HQ" class="w-full h-full object-contain rounded-xl" onerror="this.src='https://placehold.co/100x100/15803d/ffffff?text=HQ'">
                </div>
                <div>
                    <h1 class="text-sm md:text-2xl font-extrabold tracking-tight leading-tight">PP. HAMALATUL QUR'AN PUTRI 4</h1>
                    <p class="text-[9px] md:text-sm text-emerald-200 font-medium">Sistem Informasi Absensi Kegiatan Santri Digital</p>
                </div>
            </div>
            <!-- Live Date, Time & Quick Lock -->
            <div class="flex items-center gap-2 md:gap-3 shrink-0">
                <!-- Tombol Lonceng Notifikasi HP -->
                <button id="btn-notification-toggle" onclick="toggleNotificationPermission()" class="bg-white/10 hover:bg-emerald-600 border border-white/10 hover:border-emerald-500 p-2 md:p-2.5 rounded-xl text-white transition-all duration-300 hover:scale-105 flex items-center justify-center shrink-0 relative" title="Aktifkan Notifikasi HP">
                    <i class="fa-solid fa-bell text-xs md:text-base"></i>
                    <span id="notification-badge" class="absolute -top-1 -right-1 w-2.5 h-2.5 bg-rose-500 rounded-full hidden"></span>
                </button>
                <div class="bg-black/20 px-3 py-1.5 rounded-xl border border-white/10 text-right hidden sm:block">
                    <div id="live-date" class="font-bold text-[10px] md:text-sm text-white">Selasa, 14 Juli 2026</div>
                    <div id="live-time" class="text-[9px] md:text-[11px] text-emerald-300 font-mono tracking-wider mt-0.5">19:45:00 WIB</div>
                </div>
                <!-- Lock App Button -->
                <button onclick="lockAppInstantly()" class="bg-white/10 hover:bg-rose-600 border border-white/10 hover:border-rose-500 p-2 md:p-2.5 rounded-xl text-white transition-all duration-300 hover:scale-105 flex items-center justify-center shrink-0" title="Kunci Aplikasi Sekarang">
                    <i class="fa-solid fa-lock text-xs md:text-base"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Jadwal Shalat Strip -->
    <div class="bg-emerald-900 text-white/90 border-t border-emerald-700/30 py-2 px-4 shadow-inner">
        <div class="max-w-7xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-2">
            <div class="flex items-center gap-1.5 font-bold text-amber-300 text-xs shrink-0">
                <i class="fa-solid fa-clock-rotate-left"></i>
                <span>Jadwal Shalat Kediri (Pare):</span>
            </div>
            <div class="flex flex-wrap items-center justify-center gap-2 sm:gap-4 text-[10px] sm:text-xs font-semibold">
                <span class="bg-black/20 px-2 py-0.5 rounded-lg border border-white/5 flex items-center gap-1">Subuh <strong class="text-white font-mono" id="sholat-subuh">04:25</strong></span>
                <span class="bg-black/20 px-2 py-0.5 rounded-lg border border-white/5 flex items-center gap-1">Dzuhur <strong class="text-white font-mono" id="sholat-dzuhur">11:42</strong></span>
                <span class="bg-black/20 px-2 py-0.5 rounded-lg border border-white/5 flex items-center gap-1">Ashar <strong class="text-white font-mono" id="sholat-ashar">14:59</strong></span>
                <span class="bg-black/20 px-2 py-0.5 rounded-lg border border-white/5 flex items-center gap-1">Maghrib <strong class="text-white font-mono" id="sholat-maghrib">17:35</strong></span>
                <span class="bg-black/20 px-2 py-0.5 rounded-lg border border-white/5 flex items-center gap-1">Isya <strong class="text-white font-mono" id="sholat-isya">18:48</strong></span>
            </div>
        </div>
    </div>

    <!-- Main Workspace -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-4 md:py-6">
        
        <!-- Navigation Tabs - Mobile Scrollable Row -->
        <div class="bg-white p-1.5 rounded-2xl shadow-sm border border-gray-100 flex flex-nowrap overflow-x-auto gap-1.5 mb-5 no-scrollbar scroll-smooth">
            <button onclick="switchTab('dashboard')" id="btn-tab-dashboard" class="tab-btn shrink-0 flex items-center gap-2 px-4 py-2.5 rounded-xl font-bold text-xs md:text-sm transition-all duration-300 bg-emerald-600 text-white shadow-md shadow-emerald-600/10">
                <i class="fa-solid fa-chart-line text-xs"></i> Dashboard
            </button>
            <button onclick="switchTab('absensi')" id="btn-tab-absensi" class="tab-btn shrink-0 flex items-center gap-2 px-4 py-2.5 rounded-xl font-bold text-xs md:text-sm transition-all duration-300 text-gray-600 hover:bg-gray-100">
                <i class="fa-solid fa-clipboard-user text-xs"></i> Absensi Harian
            </button>
            <button onclick="switchTab('izin')" id="btn-tab-izin" class="tab-btn shrink-0 flex items-center gap-2 px-4 py-2.5 rounded-xl font-bold text-xs md:text-sm transition-all duration-300 text-gray-600 hover:bg-gray-100">
                <i class="fa-solid fa-id-card text-xs"></i> Izin Keluar/Pulang
            </button>
            <button onclick="switchTab('rekap')" id="btn-tab-rekap" class="tab-btn shrink-0 flex items-center gap-2 px-4 py-2.5 rounded-xl font-bold text-xs md:text-sm transition-all duration-300 text-gray-600 hover:bg-gray-100">
                <i class="fa-solid fa-calendar-check text-xs"></i> Rekap Mingguan
            </button>
            <button onclick="switchTab('santri')" id="btn-tab-santri" class="tab-btn shrink-0 flex items-center gap-2 px-4 py-2.5 rounded-xl font-bold text-xs md:text-sm transition-all duration-300 text-gray-600 hover:bg-gray-100">
                <i class="fa-solid fa-users text-xs"></i> Kelola Santri
            </button>
            <button onclick="switchTab('kegiatan')" id="btn-tab-kegiatan" class="tab-btn shrink-0 flex items-center gap-2 px-4 py-2.5 rounded-xl font-bold text-xs md:text-sm transition-all duration-300 text-gray-600 hover:bg-gray-100">
                <i class="fa-solid fa-book-open text-xs"></i> Kelola Kegiatan
            </button>
        </div>

        <!-- Alert Notification Box -->
        <div id="toast-container" class="fixed top-24 right-4 z-50 flex flex-col gap-2 max-w-sm w-full pointer-events-none"></div>

        <!-- ================= DETAIL ABSENSI MODAL OVERLAY ================= -->
        <div id="detail-absensi-modal" class="fixed inset-0 bg-black/60 backdrop-blur-sm flex items-center justify-center p-4 z-[9990] hidden opacity-0 transition-opacity duration-300">
            <div class="bg-white rounded-3xl max-w-lg w-full p-6 shadow-2xl transform scale-95 transition-transform duration-300 border border-gray-100 flex flex-col max-h-[85vh]">
                <!-- Modal Header -->
                <div class="flex items-center justify-between border-b border-gray-100 pb-3.5 mb-4">
                    <div class="text-left">
                        <h4 class="text-base font-extrabold text-gray-800 flex items-center gap-2">
                            <i class="fa-solid fa-file-invoice text-emerald-600"></i> Detail Jurnal Presensi
                        </h4>
                        <p id="detail-modal-subtitle" class="text-[11px] text-gray-450 mt-0.5 font-medium">Sesi: - | Tanggal: -</p>
                    </div>
                    <button onclick="closeDetailAbsenModal()" class="text-gray-400 hover:text-gray-600 transition-colors p-1 bg-gray-55 rounded-full hover:bg-gray-100">
                        <i class="fa-solid fa-xmark text-base"></i>
                    </button>
                </div>
                
                <!-- Quick Statistics Summary inside Modal -->
                <div class="grid grid-cols-4 gap-2 mb-4 text-center">
                    <div class="bg-emerald-50 text-emerald-800 p-2 rounded-xl border border-emerald-100/50">
                        <div class="text-[9px] font-bold uppercase tracking-wider">Hadir</div>
                        <div id="detail-stat-hadir" class="text-sm font-extrabold mt-0.5">0</div>
                    </div>
                    <div class="bg-blue-50 text-blue-800 p-2 rounded-xl border border-blue-100/50">
                        <div class="text-[9px] font-bold uppercase tracking-wider">Izin</div>
                        <div id="detail-stat-izin" class="text-sm font-extrabold mt-0.5">0</div>
                    </div>
                    <div class="bg-amber-50 text-amber-800 p-2 rounded-xl border border-amber-100/50">
                        <div class="text-[9px] font-bold uppercase tracking-wider">Sakit</div>
                        <div id="detail-stat-sakit" class="text-sm font-extrabold mt-0.5">0</div>
                    </div>
                    <div class="bg-rose-50 text-rose-800 p-2 rounded-xl border border-rose-100/50">
                        <div class="text-[9px] font-bold uppercase tracking-wider">Alpa</div>
                        <div id="detail-stat-alpa" class="text-sm font-extrabold mt-0.5">0</div>
                    </div>
                </div>

                <!-- Modal Body: Scrollable detailed list -->
                <div class="overflow-y-auto pr-1 flex-grow space-y-2.5 max-h-[45vh]" id="detail-santri-list">
                    <!-- Populated dynamically with beautiful colored rows -->
                </div>

                <!-- Modal Footer -->
                <div class="border-t border-gray-100 pt-3.5 mt-4 flex justify-end">
                    <button onclick="closeDetailAbsenModal()" class="bg-emerald-600 hover:bg-emerald-700 text-white font-bold px-5 py-2.5 rounded-xl text-xs transition-colors shadow-md shadow-emerald-600/10">
                        Tutup Detail
                    </button>
                </div>
            </div>
        </div>

        <!-- Custom Confirmation Modal Dialog -->
        <div id="confirm-modal" class="fixed inset-0 bg-black/50 flex items-center justify-center p-4 z-50 hidden opacity-0 transition-opacity duration-300">
            <div class="bg-white rounded-3xl max-w-sm w-full p-5 shadow-2xl transform scale-95 transition-transform duration-300">
                <div class="w-12 h-12 bg-rose-100 rounded-full flex items-center justify-center text-rose-600 mb-4 mx-auto text-xl">
                    <i class="fa-solid fa-circle-exclamation"></i>
                </div>
                <h4 id="confirm-modal-title" class="text-base font-bold text-gray-800 text-center mb-1.5">Konfirmasi Tindakan</h4>
                <p id="confirm-modal-message" class="text-xs text-gray-500 text-center leading-relaxed mb-5">Apakah Anda yakin ingin melakukan tindakan ini?</p>
                <div class="flex gap-2.5 justify-end">
                    <button id="confirm-modal-cancel" class="bg-gray-100 hover:bg-gray-200 text-gray-600 font-bold px-4 py-2.5 rounded-xl text-xs transition-colors w-1/2">
                        Batal
                    </button>
                    <button id="confirm-modal-ok" class="bg-rose-600 hover:bg-rose-700 text-white font-bold px-4 py-2.5 rounded-xl text-xs transition-colors w-1/2 shadow-md shadow-rose-600/10">
                        Hapus
                    </button>
                </div>
            </div>
        </div>

        <!-- ================= DASHBOARD TAB ================= -->
        <section id="tab-dashboard" class="tab-content block">
            <!-- Stats Grid - Mobile Optimized (2 Columns on Mobile, 4 on Desktop) -->
            <div class="grid grid-cols-2 lg:grid-cols-4 gap-3 md:gap-6 mb-6 md:mb-8">
                <!-- Stat 1: Total Santri -->
                <div class="bg-white p-4 md:p-6 rounded-2xl md:rounded-3xl border border-gray-100 shadow-sm flex items-center justify-between overflow-hidden relative group">
                    <div class="absolute -right-4 -bottom-4 w-16 h-16 md:w-24 md:h-24 bg-emerald-50 rounded-full group-hover:scale-110 transition-transform duration-300"></div>
                    <div class="relative">
                        <p class="text-[10px] md:text-sm font-bold text-gray-400 uppercase tracking-wider">Total Santri</p>
                        <h3 id="stat-total-santri" class="text-xl md:text-3xl font-extrabold text-gray-800 mt-1 md:mt-2">0</h3>
                        <p class="text-[9px] md:text-xs text-emerald-600 mt-1 font-medium hidden sm:block"><i class="fa-solid fa-circle-check"></i> Terdaftar aktif</p>
                    </div>
                    <div class="bg-emerald-100 text-emerald-700 p-2.5 md:p-4 rounded-xl md:rounded-2xl relative">
                        <i class="fa-solid fa-users text-lg md:text-2xl"></i>
                    </div>
                </div>

                <!-- Stat 2: Total Kegiatan -->
                <div class="bg-white p-4 md:p-6 rounded-2xl md:rounded-3xl border border-gray-100 shadow-sm flex items-center justify-between overflow-hidden relative group">
                    <div class="absolute -right-4 -bottom-4 w-16 h-16 md:w-24 md:h-24 bg-blue-50 rounded-full group-hover:scale-110 transition-transform duration-300"></div>
                    <div class="relative">
                        <p class="text-[10px] md:text-sm font-bold text-gray-400 uppercase tracking-wider">Total Kegiatan</p>
                        <h3 id="stat-total-kegiatan" class="text-xl md:text-3xl font-extrabold text-gray-800 mt-1 md:mt-2">0</h3>
                        <p class="text-[9px] md:text-xs text-blue-600 mt-1 font-medium hidden sm:block"><i class="fa-solid fa-calendar-plus"></i> Rutinitas harian</p>
                    </div>
                    <div class="bg-blue-100 text-blue-700 p-2.5 md:p-4 rounded-xl md:rounded-2xl relative">
                        <i class="fa-solid fa-mosque text-lg md:text-2xl"></i>
                    </div>
                </div>

                <!-- Stat 3: Absensi Hari Ini -->
                <div class="bg-white p-4 md:p-6 rounded-2xl md:rounded-3xl border border-gray-100 shadow-sm flex items-center justify-between overflow-hidden relative group">
                    <div class="absolute -right-4 -bottom-4 w-16 h-16 md:w-24 md:h-24 bg-amber-50 rounded-full group-hover:scale-110 transition-transform duration-300"></div>
                    <div class="relative">
                        <p class="text-[10px] md:text-sm font-bold text-gray-400 uppercase tracking-wider">Absen Hari Ini</p>
                        <h3 id="stat-absen-today" class="text-xl md:text-3xl font-extrabold text-gray-800 mt-1 md:mt-2">0</h3>
                        <p class="text-[9px] md:text-xs text-amber-600 mt-1 font-medium hidden sm:block"><i class="fa-solid fa-clock"></i> Sesi terabsen</p>
                    </div>
                    <div class="bg-amber-100 text-amber-700 p-2.5 md:p-4 rounded-xl md:rounded-2xl relative">
                        <i class="fa-solid fa-file-signature text-lg md:text-2xl"></i>
                    </div>
                </div>

                <!-- Stat 4: Persentase Kehadiran -->
                <div class="bg-white p-4 md:p-6 rounded-2xl md:rounded-3xl border border-gray-100 shadow-sm flex items-center justify-between overflow-hidden relative group">
                    <div class="absolute -right-4 -bottom-4 w-16 h-16 md:w-24 md:h-24 bg-purple-50 rounded-full group-hover:scale-110 transition-transform duration-300"></div>
                    <div class="relative">
                        <p class="text-[10px] md:text-sm font-bold text-gray-400 uppercase tracking-wider">Rata-rata Kehadiran</p>
                        <h3 id="stat-attendance-rate" class="text-xl md:text-3xl font-extrabold text-gray-800 mt-1 md:mt-2">0%</h3>
                        <p class="text-[9px] md:text-xs text-purple-600 mt-1 font-medium hidden sm:block"><i class="fa-solid fa-arrow-trend-up"></i> Tingkat disiplin</p>
                    </div>
                    <div class="bg-purple-100 text-purple-700 p-2.5 md:p-4 rounded-xl md:rounded-2xl relative">
                        <i class="fa-solid fa-chart-pie text-lg md:text-2xl"></i>
                    </div>
                </div>
            </div>

            <!-- Welcome & Quick Steps -->
            <div class="grid grid-cols-1 gap-6">
                <!-- Welcome Card -->
                <div class="bg-gradient-to-br from-emerald-900 via-emerald-800 to-emerald-700 text-white p-6 md:p-8 rounded-3xl shadow-lg shadow-emerald-900/10 flex flex-col justify-between relative overflow-hidden">
                    <div class="absolute top-0 right-0 transform translate-x-10 -translate-y-10 text-emerald-600 opacity-20 pointer-events-none">
                        <i class="fa-solid fa-book-quran text-[18rem]"></i>
                    </div>
                    <div class="relative z-10">
                        <span class="bg-amber-400 text-emerald-950 px-3.5 py-1.5 rounded-full text-[10px] md:text-xs font-extrabold tracking-widest uppercase shadow-md shadow-amber-500/10 inline-block">Ahlan Wa Sahlan</span>
                        <h2 class="text-lg md:text-3xl font-extrabold mt-3 md:mt-4 leading-snug">Portal Presensi HQ Putri 4</h2>
                        <p class="text-emerald-100 text-xs md:text-sm mt-2 max-w-3xl leading-relaxed">
                            Mulai dengan merekam kehadiran santriwati, mengelola agenda wajib harian pesantren secara dinamis, mengajukan perizinan santri oleh Pengasuh, serta konfirmasi keamanan digital.
                        </p>
                    </div>
                    <div class="flex flex-wrap gap-2 md:gap-3 mt-6 md:mt-8 relative z-10">
                        <button onclick="switchTab('absensi')" class="bg-white text-emerald-800 hover:bg-emerald-50 px-4 py-2.5 rounded-xl font-bold text-xs transition-all duration-300 shadow-md">
                            <i class="fa-solid fa-pencil-alt mr-1"></i> Mulai Absen Sekarang
                        </button>
                        <button onclick="switchTab('izin')" class="bg-amber-400 text-emerald-950 hover:bg-amber-300 px-4 py-2.5 rounded-xl font-bold text-xs transition-all duration-300 shadow-md">
                            <i class="fa-solid fa-id-card mr-1"></i> Izin Keluar/Pulang
                        </button>
                    </div>
                </div>
            </div>

            <!-- Jurnal Presensi Tersimpan (Tabel Riwayat - Folder Grouping) -->
            <div class="bg-white p-4 md:p-6 rounded-3xl border border-gray-100 shadow-sm mt-6 md:mt-8">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 mb-4">
                    <div>
                        <h4 class="text-sm md:text-lg font-bold text-gray-800 flex items-center gap-2">
                            <i class="fa-solid fa-journal-whills text-emerald-600"></i> Berkas Presensi Tersimpan (Riwayat Cloud)
                        </h4>
                        <p class="text-[11px] text-gray-400 mt-0.5">Daftar lembar presensi kegiatan harian yang dikelompokkan rapi berdasarkan tanggal.</p>
                    </div>
                </div>
                
                <!-- Grouped Folders Container -->
                <div id="container-riwayat-presensi" class="space-y-3.5">
                    <!-- Dynamic Collapsible Folders with nested tables will be injected here -->
                </div>

                <div id="riwayat-empty-state" class="hidden text-center py-10 text-gray-400">
                    <i class="fa-solid fa-folder-open text-3xl mb-2 text-gray-300"></i>
                    <p class="text-xs">Belum ada lembar presensi harian yang tersimpan.</p>
                </div>
            </div>
        </section>

        <!-- ================= ABSENSI TAB ================= -->
        <section id="tab-absensi" class="tab-content hidden">
            <!-- Setup & Filter Sheet -->
            <div class="bg-white p-5 rounded-3xl border border-gray-100 shadow-sm mb-5">
                <h3 class="text-sm md:text-lg font-bold text-gray-800 mb-3.5 flex items-center gap-2">
                    <i class="fa-solid fa-sliders text-emerald-600"></i> Parameter Lembar Absensi
                </h3>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                    <!-- Date Input -->
                    <div>
                        <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Tanggal Presensi</label>
                        <div class="relative">
                            <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                <i class="fa-solid fa-calendar text-xs"></i>
                            </span>
                            <input type="date" id="absensi-tanggal" class="pl-9 w-full rounded-xl border border-gray-200 py-2.5 px-3 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500 focus:border-emerald-500" />
                        </div>
                    </div>
                    <!-- Activity Select -->
                    <div>
                        <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Jenis Kegiatan</label>
                        <div class="relative">
                            <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                <i class="fa-solid fa-book-open text-xs"></i>
                            </span>
                            <select id="absensi-kegiatan" class="pl-9 w-full rounded-xl border border-gray-200 py-2.5 px-3 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500 appearance-none bg-white">
                                <option value="">-- Pilih Kegiatan --</option>
                            </select>
                            <span class="absolute inset-y-0 right-0 pr-3 flex items-center text-gray-400 pointer-events-none">
                                <i class="fa-solid fa-chevron-down text-[10px]"></i>
                            </span>
                        </div>
                    </div>
                    <!-- Load/Generate Form Button -->
                    <div class="flex items-end">
                        <button onclick="buatLembarAbsen()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-2.5 px-4 rounded-xl text-xs transition-all duration-300 flex items-center justify-center gap-1.5 shadow-md">
                            <i class="fa-solid fa-file-signature"></i> Buka Lembar Absen
                        </button>
                    </div>
                </div>
            </div>

            <!-- Absensi Form Sheet (Hidden until generated) -->
            <div id="lembar-absensi-container" class="hidden">
                <div class="bg-white rounded-3xl border border-gray-100 shadow-sm overflow-hidden mb-6">
                    <!-- Header of sheet -->
                    <div class="bg-gradient-to-r from-emerald-50 to-teal-50 px-4 py-4 md:px-6 md:py-5 border-b border-gray-100 flex flex-col sm:flex-row items-center justify-between gap-3">
                        <div class="text-center sm:text-left">
                            <span class="bg-emerald-100 text-emerald-800 px-2.5 py-1 rounded-full text-[10px] font-bold uppercase tracking-wider border border-emerald-200" id="badge-info-kegiatan">Tahfidz Subuh</span>
                            <h4 class="text-sm font-bold text-gray-800 mt-1.5" id="header-info-tanggal">Presensi: Selasa, 14 Juli 2026</h4>
                        </div>
                        <!-- Set All Status Shortcut -->
                        <div class="flex flex-wrap items-center justify-center gap-1.5 bg-white/80 backdrop-blur-sm p-1.5 rounded-xl border border-gray-200/50">
                            <span class="text-[10px] font-bold text-gray-500 px-1">Set Semua:</span>
                            <button onclick="setSemuaAbsensi('H')" class="bg-emerald-50 hover:bg-emerald-100 text-emerald-700 text-[10px] font-bold px-2 py-1 rounded-lg transition-colors">Hadir</button>
                            <button onclick="setSemuaAbsensi('I')" class="bg-blue-50 hover:bg-blue-100 text-blue-700 text-[10px] font-bold px-2 py-1 rounded-lg transition-colors">Izin</button>
                            <button onclick="setSemuaAbsensi('S')" class="bg-amber-50 hover:bg-amber-100 text-amber-700 text-[10px] font-bold px-2 py-1 rounded-lg transition-colors">Sakit</button>
                            <button onclick="setSemuaAbsensi('A')" class="bg-rose-50 hover:bg-rose-100 text-rose-700 text-[10px] font-bold px-2 py-1 rounded-lg transition-colors">Alpa</button>
                        </div>
                    </div>

                    <!-- Santri List for Attendance -->
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse min-w-[650px]">
                            <thead>
                                <tr class="border-b border-gray-100 text-[10px] font-bold text-gray-400 uppercase bg-gray-50/50">
                                    <th class="py-3 px-4 w-12 text-center">No</th>
                                    <th class="py-3 px-4">Nama Santriwati</th>
                                    <th class="py-3 px-4 text-center w-[300px]">Opsi Kehadiran</th>
                                    <th class="py-3 px-4 w-48">Keterangan / Status Izin Ust</th>
                                </tr>
                            </thead>
                            <tbody id="tabel-input-absen" class="divide-y divide-gray-100">
                                <!-- Generated Dynamically -->
                            </tbody>
                        </table>
                    </div>

                    <!-- Footer Action -->
                    <div class="bg-gray-50 px-4 py-4 border-t border-gray-100 flex justify-end gap-2.5">
                        <button onclick="batalAbsensi()" class="bg-white hover:bg-gray-100 text-gray-600 font-bold px-4 py-2 rounded-xl border border-gray-200 text-xs transition-colors">
                            Batal
                        </button>
                        <button onclick="simpanAbsensi()" class="bg-emerald-600 hover:bg-emerald-700 text-white font-bold px-5 py-2 rounded-xl text-xs transition-all duration-300 flex items-center gap-1.5 shadow-md">
                            <i class="fa-solid fa-circle-check"></i> Simpan Data Absensi
                        </button>
                    </div>
                </div>
            </div>

            <!-- Empty State -->
            <div id="absen-empty-state" class="bg-white rounded-3xl border border-gray-100 p-8 text-center shadow-sm">
                <div class="w-16 h-16 bg-emerald-50 rounded-full flex items-center justify-center mx-auto mb-3 text-emerald-600">
                    <i class="fa-solid fa-clipboard-user text-2xl"></i>
                </div>
                <h4 class="text-sm font-bold text-gray-800">Lembar Absensi Belum Terbuka</h4>
                <p class="text-gray-400 text-[11px] mt-1 max-w-xs mx-auto">Silakan tentukan parameter di atas lalu klik "Buka Lembar Absen" untuk memulai pencatatan.</p>
            </div>
        </section>

        <!-- ================= IZIN TAB ================= -->
        <section id="tab-izin" class="tab-content hidden">
            <!-- Grid Layout: Form Pengajuan Keamanan & Panel Persetujuan Pengasuh (Bebas Scroll) -->
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                
                <!-- Left Column: Form Keamanan (4 Cols) -->
                <div class="lg:col-span-4 bg-white p-5 rounded-3xl border border-gray-100 shadow-sm h-fit">
                    <div class="flex items-center gap-2.5 mb-4 text-emerald-800">
                        <div class="p-2 bg-emerald-100 text-emerald-700 rounded-xl">
                            <i class="fa-solid fa-shield-halved text-base"></i>
                        </div>
                        <div>
                            <h3 class="text-sm md:text-base font-extrabold text-gray-800">Form Keamanan</h3>
                            <p class="text-[10px] text-gray-400 font-medium">Pengajuan Izin Keluar/Pulang Santri</p>
                        </div>
                    </div>
                    
                    <form id="form-pengajuan-izin" onsubmit="event.preventDefault(); ajukanIzinSantri();" class="space-y-3.5">
                        <!-- Select Santri -->
                        <div>
                            <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Pilih Santriwati</label>
                            <div class="relative">
                                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                    <i class="fa-solid fa-user-graduate text-xs"></i>
                                </span>
                                <select id="izin-santri-id" required class="pl-9 w-full rounded-xl border border-gray-200 py-2.5 px-3 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500 appearance-none bg-white">
                                    <option value="">-- Pilih Santri --</option>
                                    <!-- Populated dynamically -->
                                </select>
                                <span class="absolute inset-y-0 right-0 pr-3 flex items-center text-gray-400 pointer-events-none">
                                    <i class="fa-solid fa-chevron-down text-[10px]"></i>
                                </span>
                            </div>
                        </div>

                        <!-- Type of Permit -->
                        <div>
                            <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Tipe Izin</label>
                            <div class="grid grid-cols-2 gap-2">
                                <label class="border border-gray-200 rounded-xl p-2.5 flex items-center gap-2 cursor-pointer hover:bg-gray-50 transition-colors">
                                    <input type="radio" name="izin-tipe" value="Keluar" checked class="text-emerald-600 focus:ring-emerald-500">
                                    <div class="text-left">
                                        <p class="text-[11px] font-bold text-gray-700">Izin Keluar</p>
                                        <p class="text-[9px] text-gray-400">Sementara / Hari ini</p>
                                    </div>
                                </label>
                                <label class="border border-gray-200 rounded-xl p-2.5 flex items-center gap-2 cursor-pointer hover:bg-gray-50 transition-colors">
                                    <input type="radio" name="izin-tipe" value="Pulang" class="text-emerald-600 focus:ring-emerald-500">
                                    <div class="text-left">
                                        <p class="text-[11px] font-bold text-gray-700">Izin Pulang</p>
                                        <p class="text-[9px] text-gray-400">Pulang ke rumah (Menginap)</p>
                                    </div>
                                </label>
                            </div>
                        </div>

                        <!-- Keperluan / Reason -->
                        <div>
                            <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Keperluan / Alasan</label>
                            <div class="relative">
                                <span class="absolute top-3 left-3 text-gray-400">
                                    <i class="fa-solid fa-comment-dots text-xs"></i>
                                </span>
                                <textarea id="izin-keperluan" required rows="2" placeholder="Contoh: Berobat ke dokter gigi Pare, jenguk nenek sakit..." class="pl-9 w-full rounded-xl border border-gray-200 p-2.5 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500"></textarea>
                            </div>
                        </div>

                        <!-- Estimasi Kembali -->
                        <div>
                            <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Estimasi Waktu Kembali</label>
                            <div class="relative">
                                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                    <i class="fa-solid fa-hourglass-half text-xs"></i>
                                </span>
                                <input type="text" id="izin-estimasi" required placeholder="Contoh: Pukul 15:00 WIB, atau 3 Hari" class="pl-9 w-full rounded-xl border border-gray-200 p-2.5 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500" />
                            </div>
                        </div>

                        <!-- Catatan Keamanan -->
                        <div>
                            <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Catatan Tambahan Keamanan</label>
                            <input type="text" id="izin-catatan-keamanan" placeholder="Contoh: Dijemput bapak kandung, naik motor..." class="w-full rounded-xl border border-gray-200 p-2.5 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500 text-left" />
                        </div>

                        <button type="submit" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-2.5 rounded-xl text-xs transition-all duration-300 flex items-center justify-center gap-1.5 shadow-md shadow-emerald-600/15">
                            <i class="fa-solid fa-paper-plane"></i> Ajukan Izin ke Pengasuh
                        </button>
                    </form>
                </div>

                <!-- Right Column: Panel Persetujuan Pengasuh (8 Cols - Tanpa Scroll) -->
                <div class="lg:col-span-8 space-y-6">
                    
                    <!-- KARTU 1: PERSIDANGAN / IZIN AKTIF -->
                    <div class="bg-white p-5 rounded-3xl border border-gray-100 shadow-sm text-left">
                        <div class="flex items-center gap-2.5 mb-4 text-emerald-800">
                            <div class="p-2 bg-emerald-100 text-emerald-700 rounded-xl">
                                <i class="fa-solid fa-user-shield text-base"></i>
                            </div>
                            <div>
                                <h3 class="text-sm md:text-base font-extrabold text-gray-800">Persetujuan & Izin Hack / Aktif</h3>
                                <p class="text-[10px] text-gray-400 font-medium">Santriwati yang sedang di luar pesantren / menunggu keputusan Pengasuh</p>
                            </div>
                        </div>

                        <!-- Grid Utama Izin Aktif (Bebas Scroll - Grid murni) -->
                        <div id="grid-log-izin-aktif" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <!-- Populated dynamically (Menunggu Konfirmasi & Disetujui) -->
                        </div>

                        <!-- Empty State for Active Permits -->
                        <div id="izin-aktif-empty-state" class="hidden text-center py-10 text-gray-450">
                            <i class="fa-solid fa-circle-check text-4xl mb-3 text-emerald-500/30"></i>
                            <h4 class="text-xs font-bold text-gray-700">Semua Santriwati di Dalam Pesantren</h4>
                            <p class="text-[10px] text-gray-400 mt-1">Tidak ada pengajuan izin tertunda atau santriwati di luar asrama saat ini.</p>
                        </div>
                    </div>

                    <!-- KARTU 2: LACIK ARSIP DAN REKAM JEJAK (Collapsible Accordion) -->
                    <div class="bg-white p-5 rounded-3xl border border-gray-100 shadow-sm text-left">
                        <button onclick="toggleArsipIzin()" class="w-full flex items-center justify-between focus:outline-none">
                            <div class="flex items-center gap-2.5 text-gray-800">
                                <div class="p-2 bg-gray-100 text-gray-700 rounded-xl">
                                    <i class="fa-solid fa-box-archive text-base"></i>
                                </div>
                                <div>
                                    <h3 class="text-sm md:text-base font-extrabold text-gray-800">Arsip & Rekam Jejak Perizinan</h3>
                                    <p class="text-[10px] text-gray-400 font-medium">Kumpulan riwayat keluar-masuk santriwati yang telah selesai atau ditolak</p>
                                </div>
                            </div>
                            <div class="flex items-center gap-2.5">
                                <span id="badge-total-arsip" class="bg-gray-150 text-gray-700 font-extrabold text-[9px] md:text-[10px] px-2.5 py-1 rounded-xl">
                                    0 Arsip
                                </span>
                                <i id="chevron-arsip-izin" class="fa-solid fa-chevron-down text-xs text-gray-400 transition-transform duration-250"></i>
                            </div>
                        </button>

                        <!-- Archived permits block (Collapsible, Bebas Scroll) -->
                        <div id="container-arsip-izin" class="hidden mt-4 pt-4 border-t border-gray-100 grid grid-cols-1 md:grid-cols-2 gap-4">
                            <!-- Populated dynamically (Selesai & Ditolak) -->
                        </div>

                        <!-- Empty State for Archived Permits -->
                        <div id="izin-arsip-empty-state" class="hidden text-center py-10 text-gray-450">
                            <i class="fa-solid fa-folder-open text-4xl mb-3 text-gray-300"></i>
                            <h4 class="text-xs font-bold text-gray-750">Arsip Perizinan Masih Kosong</h4>
                            <p class="text-[10px] text-gray-400 mt-1">Belum ada riwayat izin selesai atau ditolak.</p>
                        </div>
                    </div>

                </div>
            </div>
        </section>

        <!-- ================= REKAP TAB ================= -->
        <section id="tab-rekap" class="tab-content hidden">
            <!-- Filter & Action Rekap -->
            <div class="bg-white p-5 rounded-3xl border border-gray-100 shadow-sm mb-5">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
                    <div>
                        <h3 class="text-sm md:text-lg font-bold text-gray-800 flex items-center gap-2">
                            <i class="fa-solid fa-clock-rotate-left text-emerald-600"></i> Rekap Kehadiran Otomatis
                        </h3>
                        <p class="text-xs text-gray-500 mt-0.5">
                            Akumulasi kehadiran dalam <span class="font-bold text-emerald-700">7 Hari Terakhir</span>.
                        </p>
                    </div>
                    <div class="bg-emerald-50 text-emerald-800 border border-emerald-100 px-3 py-1.5 rounded-xl flex items-center gap-1.5 self-start sm:self-auto">
                        <i class="fa-solid fa-calendar-days text-emerald-600 text-xs"></i>
                        <span id="label-rentang-rekap" class="text-[10px] font-bold font-mono">Loading rentang tanggal...</span>
                    </div>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-3 gap-3 mt-4 items-end">
                    <!-- Filter Activity -->
                    <div class="md:col-span-2">
                        <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Filter Berdasarkan Kegiatan</label>
                        <div class="relative">
                            <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                <i class="fa-solid fa-book-open text-xs"></i>
                            </span>
                            <select id="rekap-filter-kegiatan" onchange="prosesRekap()" class="pl-9 w-full rounded-xl border border-gray-200 py-2.5 px-3 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500 appearance-none bg-white">
                                <option value="ALL">Semua Kegiatan</option>
                            </select>
                            <span class="absolute inset-y-0 right-0 pr-3 flex items-center text-gray-400 pointer-events-none">
                                <i class="fa-solid fa-chevron-down text-[10px]"></i>
                            </span>
                        </div>
                    </div>
                    <!-- Buttons -->
                    <div class="flex gap-2">
                        <button onclick="prosesRekap()" class="flex-grow bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-2.5 px-3 rounded-xl text-xs transition-all duration-300 flex items-center justify-center gap-1.5 shadow-md">
                            <i class="fa-solid fa-sync"></i> Refresh
                        </button>
                        <button onclick="unduhPDFRekap()" class="bg-rose-100 hover:bg-rose-200 text-rose-700 font-bold p-2.5 rounded-xl transition-all duration-300" title="Unduh PDF Portrait">
                            <i class="fa-solid fa-file-pdf"></i>
                        </button>
                        <button onclick="cetakRekap()" class="bg-gray-100 hover:bg-gray-200 text-gray-700 font-bold p-2.5 rounded-xl transition-all duration-300" title="Cetak Rekap">
                            <i class="fa-solid fa-print"></i>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Rekap Sheet Display -->
            <div id="rekap-sheet-container" class="hidden">
                <!-- Summary Metrics (Stacked/Grid) -->
                <div class="grid grid-cols-2 md:grid-cols-4 gap-3 mb-4">
                    <div class="bg-emerald-50 border border-emerald-100 p-3 rounded-xl text-center">
                        <p class="text-[9px] font-bold text-emerald-800 uppercase tracking-wider">Hadir</p>
                        <h4 id="summary-rekap-hadir" class="text-lg font-extrabold text-emerald-950 mt-0.5">0</h4>
                    </div>
                    <div class="bg-blue-50 border border-blue-100 p-3 rounded-xl text-center">
                        <p class="text-[9px] font-bold text-blue-800 uppercase tracking-wider">Izin</p>
                        <h4 id="summary-rekap-izin" class="text-lg font-extrabold text-blue-950 mt-0.5">0</h4>
                    </div>
                    <div class="bg-amber-50 border border-amber-100 p-3 rounded-xl text-center">
                        <p class="text-[9px] font-bold text-amber-800 uppercase tracking-wider">Sakit</p>
                        <h4 id="summary-rekap-sakit" class="text-lg font-extrabold text-amber-950 mt-0.5">0</h4>
                    </div>
                    <div class="bg-rose-50 border border-rose-100 p-3 rounded-xl text-center">
                        <p class="text-[9px] font-bold text-rose-800 uppercase tracking-wider">Alpa</p>
                        <h4 id="summary-rekap-alpa" class="text-lg font-extrabold text-rose-950 mt-0.5">0</h4>
                    </div>
                </div>

                <!-- Legenda Kategori Keaktifan -->
                <div class="bg-emerald-50 border border-emerald-100 px-4 py-3 rounded-2xl mb-4 flex flex-col lg:flex-row items-start lg:items-center justify-between gap-2 text-xs">
                    <div class="flex items-center gap-1.5 text-emerald-900 font-bold">
                        <i class="fa-solid fa-graduation-cap text-emerald-600"></i>
                        <span>Kriteria Keaktifan:</span>
                    </div>
                    <div class="flex flex-wrap gap-x-3 gap-y-1 text-[10px] font-semibold text-emerald-800">
                        <span>A (Sangat Aktif): 91-100%</span>
                        <span>B (Aktif): 81-90%</span>
                        <span>C (Cukup): 71-80%</span>
                        <span>D (Kurang): &le;70%</span>
                    </div>
                </div>

                <!-- Rekap Table Paper -->
                <div class="bg-white rounded-3xl border border-gray-100 shadow-sm overflow-hidden" id="print-area">
                    <div class="p-4 md:p-6 border-b border-gray-100 flex flex-col sm:flex-row sm:items-center justify-between gap-2">
                        <div>
                            <h3 class="text-sm md:text-lg font-bold text-gray-800">Tabel Rekap Kehadiran</h3>
                            <p class="text-[11px] text-gray-400 mt-0.5" id="rekap-sub-title">Periode: - s/d -</p>
                        </div>
                        <div>
                            <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-[10px] font-medium bg-emerald-100 text-emerald-800" id="badge-total-sesi">
                                Total Sesi: 0
                            </span>
                        </div>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse min-w-[650px]">
                            <thead>
                                <tr class="border-b border-gray-100 text-[10px] font-bold text-gray-400 uppercase bg-gray-50/50">
                                    <th class="py-3 px-4 w-12 text-center">No</th>
                                    <th class="py-3 px-4">Nama Santriwati</th>
                                    <th class="py-3 px-3 text-center bg-emerald-50/30 text-emerald-700">Hadir</th>
                                    <th class="py-3 px-3 text-center bg-blue-50/30 text-blue-700">Izin</th>
                                    <th class="py-3 px-3 text-center bg-amber-50/30 text-amber-700">Sakit</th>
                                    <th class="py-3 px-3 text-center bg-rose-50/30 text-rose-700">Alpa</th>
                                    <th class="py-3 px-3 text-center">Total Sesi</th>
                                    <th class="py-3 px-4 text-center">Keaktifan</th>
                                </tr>
                            </thead>
                            <tbody id="tabel-laporan-rekap" class="divide-y divide-gray-100 text-xs">
                                <!-- Generated Dynamically -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Empty Rekap State -->
            <div id="rekap-empty-state" class="bg-white rounded-3xl border border-gray-100 p-8 text-center shadow-sm">
                <div class="w-16 h-16 bg-emerald-50 rounded-full flex items-center justify-center mx-auto mb-3 text-emerald-600">
                    <i class="fa-solid fa-chart-bar text-2xl"></i>
                </div>
                <h4 class="text-sm font-bold text-gray-800">Laporan Rekap Belum Ditampilkan</h4>
                <p class="text-gray-400 text-[11px] mt-1 max-w-xs mx-auto">Silakan atur filter kegiatan, kemudian klik tombol "Cari" untuk memproses data.</p>
            </div>
        </section>

        <!-- ================= SANTRI TAB ================= -->
        <section id="tab-santri" class="tab-content hidden">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-5">
                <!-- Add & Edit Form (Shown SECOND on Mobile) -->
                <div class="bg-white p-5 rounded-3xl border border-gray-100 shadow-sm h-fit order-2 lg:order-1">
                    <h3 id="form-santri-title" class="text-sm md:text-lg font-bold text-gray-800 mb-4 flex items-center gap-2">
                        <i class="fa-solid fa-user-plus text-emerald-600"></i> Tambah Santriwati Baru
                    </h3>
                    
                    <form id="form-santri" onsubmit="event.preventDefault(); saveSantri();">
                        <input type="hidden" id="santri-edit-id" value="" />
                        <div class="space-y-3.5">
                            <!-- Name input -->
                            <div>
                                <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Nama Lengkap</label>
                                <div class="relative">
                                    <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                        <i class="fa-solid fa-signature text-xs"></i>
                                    </span>
                                    <input type="text" id="santri-nama" required placeholder="Contoh: Sinta Roudlotul J." class="pl-9 w-full rounded-xl border border-gray-200 p-2.5 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500 focus:border-emerald-500" />
                                </div>
                            </div>

                            <!-- Alamat input -->
                            <div>
                                <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Alamat Asal</label>
                                <div class="relative">
                                    <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                        <i class="fa-solid fa-location-dot text-xs"></i>
                                    </span>
                                    <input type="text" id="santri-alamat" required placeholder="Contoh: Pare, Kediri" class="pl-9 w-full rounded-xl border border-gray-200 p-2.5 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500 focus:border-emerald-500" />
                                </div>
                            </div>

                            <!-- Kelas/Asrama input -->
                            <div>
                                <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Grup / Kamar</label>
                                <div class="relative">
                                    <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                        <i class="fa-solid fa-house text-xs"></i>
                                    </span>
                                    <input type="text" id="santri-grup" placeholder="Contoh: Kamar Maryam" class="pl-9 w-full rounded-xl border border-gray-200 p-2.5 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500 focus:border-emerald-500" />
                                </div>
                            </div>
                        </div>

                        <div class="flex gap-2 mt-5">
                            <button type="button" onclick="resetFormSantri()" class="bg-gray-100 hover:bg-gray-200 text-gray-600 font-bold px-4 py-2.5 rounded-xl text-xs transition-colors w-1/3">
                                Batal
                            </button>
                            <button type="submit" class="bg-emerald-600 hover:bg-emerald-700 text-white font-bold px-4 py-2.5 rounded-xl text-xs transition-all duration-300 flex-grow shadow-md shadow-emerald-600/10">
                                <i class="fa-solid fa-floppy-disk mr-1"></i> Simpan
                            </button>
                        </div>
                    </form>
                </div>

                <!-- Santriwati Table List (Shown FIRST on Mobile) -->
                <div class="bg-white rounded-3xl border border-gray-100 shadow-sm overflow-hidden lg:col-span-2 order-1 lg:order-2">
                    <div class="p-4 md:p-6 border-b border-gray-100 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
                        <div>
                            <h3 class="text-sm md:text-lg font-bold text-gray-800">Daftar Santriwati</h3>
                            <p class="text-[11px] text-gray-400 mt-0.5">Mengelola nama and asal alamat santriwati.</p>
                        </div>
                        <div class="relative w-full sm:w-60">
                            <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                <i class="fa-solid fa-magnifying-glass text-xs"></i>
                            </span>
                            <input type="text" id="search-search-santri" oninput="renderSantriList()" placeholder="Cari nama santriwati..." class="pl-8 w-full rounded-xl border border-gray-200 py-2 px-3 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500" />
                        </div>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse min-w-[500px]">
                            <thead>
                                <tr class="border-b border-gray-100 text-[10px] font-bold text-gray-400 uppercase bg-gray-50/50">
                                    <th class="py-3.5 px-4 w-12 text-center">No</th>
                                    <th class="py-3.5 px-4">Identitas Santri</th>
                                    <th class="py-3.5 px-4">Grup/Kamar</th>
                                    <th class="py-3.5 px-4 text-center w-24">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="tabel-daftar-santri" class="divide-y divide-gray-100 text-xs">
                                <!-- Generated Dynamically -->
                            </tbody>
                        </table>
                    </div>

                    <!-- Empty State for table -->
                    <div id="santri-empty-table" class="hidden p-6 text-center text-gray-400">
                        <i class="fa-solid fa-user-slash text-2xl mb-1 text-gray-300"></i>
                        <p class="text-xs">Data santriwati tidak ditemukan.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- ================= KEGIATAN TAB ================= -->
        <section id="tab-kegiatan" class="tab-content hidden">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-5">
                <!-- Add & Edit Form (Shown SECOND on Mobile) -->
                <div class="bg-white p-5 rounded-3xl border border-gray-100 shadow-sm h-fit order-2 lg:order-1">
                    <h3 id="form-kegiatan-title" class="text-sm md:text-lg font-bold text-gray-800 mb-4 flex items-center gap-2">
                        <i class="fa-solid fa-calendar-plus text-emerald-600"></i> Tambah Kegiatan Baru
                    </h3>
                    
                    <form id="form-kegiatan" onsubmit="event.preventDefault(); saveKegiatan();">
                        <input type="hidden" id="kegiatan-edit-id" value="" />
                        <div class="space-y-3.5">
                            <!-- Name input -->
                            <div>
                                <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Nama Kegiatan</label>
                                <div class="relative">
                                    <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                        <i class="fa-solid fa-bookmark text-xs"></i>
                                    </span>
                                    <input type="text" id="kegiatan-nama" required placeholder="Contoh: Tahfidz Subuh, Kajian Kitab" class="pl-9 w-full rounded-xl border border-gray-200 p-2.5 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500 focus:border-emerald-500" />
                                </div>
                            </div>

                            <!-- Time inputs (Teks Bebas Manual) -->
                            <div>
                                <label class="block text-[10px] font-bold text-gray-500 uppercase tracking-wider mb-1.5">Waktu Pelaksanaan (Teks Bebas)</label>
                                <div class="relative">
                                    <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                                        <i class="fa-solid fa-clock text-xs"></i>
                                    </span>
                                    <input type="text" id="kegiatan-waktu" required placeholder="Contoh: Pukul 05:00 - 06:30 WIB" class="pl-9 w-full rounded-xl border border-gray-200 p-2.5 text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500 focus:border-emerald-500 bg-white" />
                                </div>
                            </div>
                        </div>

                        <div class="flex gap-2 mt-5">
                            <button type="button" onclick="resetFormKegiatan()" class="bg-gray-100 hover:bg-gray-200 text-gray-600 font-bold px-4 py-2.5 rounded-xl text-xs transition-colors w-1/3">
                                Batal
                            </button>
                            <button type="submit" class="bg-emerald-600 hover:bg-emerald-700 text-white font-bold px-4 py-2.5 rounded-xl text-xs transition-all duration-300 flex-grow shadow-md shadow-emerald-600/10">
                                <i class="fa-solid fa-floppy-disk mr-1"></i> Simpan
                            </button>
                        </div>
                    </form>
                </div>

                <!-- Kegiatan Table List (Shown FIRST on Mobile) -->
                <div class="bg-white rounded-3xl border border-gray-100 shadow-sm overflow-hidden lg:col-span-2 order-1 lg:order-2">
                    <div class="p-4 md:p-6 border-b border-gray-100">
                        <h3 class="text-sm md:text-lg font-bold text-gray-800">Daftar Kegiatan Harian</h3>
                        <p class="text-[11px] text-gray-400 mt-0.5">Mengelola daftar agenda kegiatan wajib bagi para santriwati.</p>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse min-w-[500px]">
                            <thead>
                                <tr class="border-b border-gray-100 text-[10px] font-bold text-gray-400 uppercase bg-gray-50/50">
                                    <th class="py-3.5 px-4 w-12 text-center">No</th>
                                    <th class="py-3.5 px-4">Nama Kegiatan</th>
                                    <th class="py-3.5 px-4">Waktu/Deskripsi</th>
                                    <th class="py-3.5 px-4 text-center w-24">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="tabel-daftar-kegiatan" class="divide-y divide-gray-100 text-xs">
                                <!-- Generated Dynamically -->
                            </tbody>
                        </table>
                    </div>

                    <!-- Empty State for table -->
                    <div id="kegiatan-empty-table" class="hidden p-6 text-center text-gray-400">
                        <i class="fa-solid fa-calendar-times text-2xl mb-1 text-gray-300"></i>
                        <p class="text-xs">Daftar kegiatan masih kosong.</p>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- Footer -->
    <footer class="bg-emerald-950 text-emerald-300 border-t border-emerald-800 py-6 mt-12">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center text-xs space-y-1.5">
            <p>&copy; 2026 PP. Hamalatul Qur'an Putri 4. All Rights Reserved.</p>
            <p class="text-emerald-500 font-medium">Dirancang demi ketertiban serta kelancaran pencatatan presensi santriwati berbasis digital.</p>
        </div>
    </footer>

    <!-- JavaScript App Logic -->
    <script>
        const DB_SANTRI_KEY = 'hq_putri4_santri';
        const DB_KEGIATAN_KEY = 'hq_putri4_kegiatan';
        const DB_ABSEN_KEY = 'hq_putri4_absensi';
        const DB_PIN_KEY = 'hq_putri4_secure_pin';
        const DB_IZIN_KEY = 'hq_putri4_izin';

        // DYNAMIC WEB MANIFEST GENERATION FOR PWA INSTALLATION SUPPORT
        (function() {
            try {
                const manifest = {
                    "name": "E-Presensi & Izin HQ Putri 4",
                    "short_name": "Presensi HQ4",
                    "description": "Sistem Informasi Absensi & Keamanan Santri PP. Hamalatul Qur'an Putri 4 Al-Karima Kediri",
                    "start_url": "./index.html",
                    "display": "standalone",
                    "background_color": "#064e3b",
                    "theme_color": "#065f46",
                    "orientation": "portrait",
                    "icons": [
                        {
                            "src": "ChatGPT Image 16 Jul 2026, 22.21.31.png",
                            "sizes": "192x192",
                            "type": "image/png",
                            "purpose": "any maskable"
                        },
                        {
                            "src": "ChatGPT Image 16 Jul 2026, 22.21.31.png",
                            "sizes": "512x512",
                            "type": "image/png"
                        }
                    ]
                };
                const manifestStr = JSON.stringify(manifest);
                const blob = new Blob([manifestStr], {type: 'application/json'});
                const manifestURL = URL.createObjectURL(blob);
                const link = document.createElement('link');
                link.rel = 'manifest';
                link.href = manifestURL;
                document.head.appendChild(link);
            } catch (err) {
                console.error("Gagal membuat manifest PWA dinamis:", err);
            }
        })();

        // Pendaftaran otomatis Service Worker sw.js agar ikon muncul di baris status atas HP Android
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('sw.js')
                    .then(reg => {
                        console.log('Status Bar Service Worker berhasil terdaftar:', reg);
                    })
                    .catch(err => {
                        console.warn('Gagal mendaftarkan Service Worker, menggunakan sistem fallback in-app banner:', err);
                    });
            });
        }

        // Initialize Supabase Client
        const SUPABASE_URL = 'https://ogbvyeypznbwurmsmwld.supabase.co';
        const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im9nYnZ5ZXlwem5id3VybXNtd2xkIiwicm9sZSI6ImFub24iLCJpYXQiOjE3ODE3OTM1MzgsImV4cCI6MjA5NzM2OTUzOH0.LSO8qrGBs85lkSD5mzVL7zOBO5LTHJX90v7Q-FJEYQo';
        const supabaseClient = supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

        // Preloaded official 24 santriwati names exactly as in DOC-20260516-WA0006(1).pdf
        const sampleSantri = [
            { id: "S-1", nama: "Sinta Roudlotul J.", alamat: "Pare, Kediri", grup: "Kamar Maryam" },
            { id: "S-2", nama: "Malahatul Kholilah", alamat: "Tulungrejo, Kediri", grup: "Kamar Khadijah" },
            { id: "S-3", nama: "Maghfirotul fadhilah", alamat: "Mangunrejo", grup: "Kamar Aisyah" },
            { id: "S-4", nama: "Ummu Kulsum", alamat: "Pare, Kediri", grup: "Kamar Maryam" },
            { id: "S-5", nama: "Nila Qoni'a Jazila", alamat: "Tulungrejo, Kediri", grup: "Kamar Khadijah" },
            { id: "S-6", nama: "Agni Mukarromah", alamat: "Mangunrejo", grup: "Kamar Aisyah" },
            { id: "S-7", nama: "Putri Maulidiyah", alamat: "Pare, Kediri", grup: "Kamar Maryam" },
            { id: "S-8", nama: "Lely sabna", alamat: "Tulungrejo, Kediri", grup: "Kamar Khadijah" },
            { id: "S-9", nama: "Kuswatun Khasanah", alamat: "Mangunrejo", grup: "Kamar Aisyah" },
            { id: "S-10", nama: "Naznin Rhaina Salwa", alamat: "Pare, Kediri", grup: "Kamar Maryam" },
            { id: "S-11", nama: "Kayla Anas Alwa", alamat: "Tulungrejo, Kediri", grup: "Kamar Khadijah" },
            { id: "S-12", nama: "Nur Baiti", alamat: "Mangunrejo", grup: "Kamar Aisyah" },
            { id: "S-13", nama: "Cantika Diva Anjani", alamat: "Pare, Kediri", grup: "Kamar Maryam" },
            { id: "S-14", nama: "Zahrofi Romadhon", alamat: "Tulungrejo, Kediri", grup: "Kamar Khadijah" },
            { id: "S-15", nama: "Wildatul aluf", alamat: "Mangunrejo", grup: "Kamar Aisyah" },
            { id: "S-16", nama: "Innaya salsabilla", alamat: "Pare, Kediri", grup: "Kamar Maryam" },
            { id: "S-17", nama: "Siti Romlah", alamat: "Mangunrejo", grup: "Kamar Khadijah" },
            { id: "S-18", nama: "Nurul Hidayah", alamat: "Sidoarjo", grup: "Kamar Aisyah" },
            { id: "S-19", nama: "Lina Syabaniah", alamat: "Tulungrejo, Kediri", grup: "Kamar Maryam" },
            { id: "S-20", nama: "Siti Aminah", alamat: "Mangunrejo", grup: "Kamar Khadijah" },
            { id: "S-21", nama: "Cendekia A. Najwa", alamat: "Pare, Kediri", grup: "Kamar Aisyah" },
            { id: "S-22", nama: "Nur wening Larasati", alamat: "Tulungrejo, Kediri", grup: "Kamar Maryam" },
            { id: "S-23", nama: "Fitri Nurani", alamat: "Mangunrejo", grup: "Kamar Khadijah" },
            { id: "S-24", nama: "Jenyar Silviana Majid", alamat: "Pare, Kediri", grup: "Kamar Aisyah" }
        ];

        const sampleKegiatan = [];

        // Global State Variables
        let dataSantri = [];
        let dataKegiatan = [];
        let dataAbsensi = []; 
        let dataIzin = [];

        // Tracking click counts for Secret Approval Gesture (Ust/Pengasuh ONLY)
        let approvalGestureTracker = {};

        // Tracking Variables for Real-time Multi-device Notifications
        let previousIzinLength = null;
        let previousAbsenLength = null;
        let previousIzinStates = {};

        let activeAbsensiSession = {
            date: '',
            activity_id: '',
            attendance: {}, 
            notes: {} 
        };

        // Arsip accordion toggle state variable
        let arsipIzinOpen = false;

        // PIN Security State variables
        let appPinCode = '';
        let pinConfirmStage = false;
        let pinTempSetup = '';
        let pinEnteredBuffer = '';

        // Secret Click Counter Variables (Ganti PIN)
        let logoClickCount = 0;
        let logoClickTimeout;
        
        // Backdoor Click Counter Variables (Bypass lock screen)
        let backdoorClickCount = 0;
        let backdoorClickTimeout;

        // --- SINKRONISASI SUARA NOTIFIKASI NYATA (WA-STYLE DOUBLE CHIME) ---
        function playNotificationChime() {
            try {
                const AudioCtx = window.AudioContext || window.webkitAudioContext;
                if (!AudioCtx) return;
                const ctx = new AudioCtx();
                const now = ctx.currentTime;
                
                // Nada Pertama (Tinggi)
                const osc1 = ctx.createOscillator();
                const gain1 = ctx.createGain();
                osc1.type = 'sine';
                osc1.frequency.setValueAtTime(880, now); // A5
                osc1.frequency.exponentialRampToValueAtTime(1320, now + 0.1); // E6
                gain1.gain.setValueAtTime(0.2, now);
                gain1.gain.exponentialRampToValueAtTime(0.01, now + 0.25);
                osc1.connect(gain1);
                gain1.connect(ctx.destination);
                osc1.start(now);
                osc1.stop(now + 0.25);
                
                // Nada Kedua (WA Style Pop - Jeda 120ms)
                const delay = 0.12;
                const osc2 = ctx.createOscillator();
                const gain2 = ctx.createGain();
                osc2.type = 'sine';
                osc2.frequency.setValueAtTime(1046.50, now + delay); // C6
                osc2.frequency.exponentialRampToValueAtTime(1567.98, now + delay + 0.15); // G6
                gain2.gain.setValueAtTime(0.25, now + delay);
                gain2.gain.exponentialRampToValueAtTime(0.01, now + delay + 0.3);
                osc2.connect(gain2);
                gain2.connect(ctx.destination);
                osc2.start(now + delay);
                osc2.stop(now + delay + 0.3);
                
            } catch (err) {
                console.warn("AudioContext diblokir interaksi browser:", err);
            }
        }

        // --- SISTEM NOTIFIKASI SISTEM DAN DESAIN MOBILE IN-APP SLIDEDOWN BANNER ---
        function sendSystemNotification(title, body, type = 'success') {
            // Bunyikan nada notifikasi WA-Style yang nyaring
            playNotificationChime();

            // Berikan efek getar pada HP (Getar-Jeda-Getar, khas pesan masuk)
            if (window.navigator && window.navigator.vibrate) {
                try {
                    window.navigator.vibrate([150, 100, 150]);
                } catch (e) {
                    console.debug("Izin getar diblokir sistem operasi.");
                }
            }

            // TRIGGER SYSTEM NOTIFICATION DI ATAS LAYAR HP (STATUS BAR)
            // Menggunakan Service Worker agar tampil di status bar
            if (window.Notification && Notification.permission === "granted") {
                const notificationOptions = {
                    body: body,
                    icon: "ChatGPT Image 16 Jul 2026, 22.21.31.png", // Logo pesantren sebagai foto profil notif
                    badge: "ChatGPT Image 16 Jul 2026, 22.21.31.png", // Logo pesantren muncul monochrome di status bar atas
                    tag: "hq-security-alert",
                    renotify: true, // Paksa HP bergetar kembali jika ada update baru
                    silent: true,  // Suara dihandle manual oleh playNotificationChime() agar sinkron
                    vibrate: [150, 100, 150],
                    data: { url: window.location.href }
                };

                if ('serviceWorker' in navigator && navigator.serviceWorker.controller) {
                    navigator.serviceWorker.ready.then(registration => {
                        registration.showNotification(`HQ Putri 4: ${title}`, notificationOptions);
                    }).catch(err => {
                        // Fallback jika Service Worker sibuk / bermasalah
                        new Notification(`HQ Putri 4: ${title}`, notificationOptions);
                    });
                } else {
                    // Fallback normal browser jika sw.js belum diaktifkan penuh
                    new Notification(`HQ Putri 4: ${title}`, notificationOptions);
                }
            }

            // Tampilkan juga spanduk custom cantik di dalam layar aktif aplikasi
            const banner = document.createElement('div');
            banner.className = 'fixed top-4 left-1/2 -translate-x-1/2 w-11/12 max-w-sm bg-white/95 backdrop-blur-md rounded-2xl border border-gray-100 shadow-2xl z-[9999] flex p-3.5 gap-3 transform -translate-y-40 transition-all duration-500 ease-out cursor-pointer pointer-events-auto';
            
            let themeIcon = "fa-bell text-emerald-600 bg-emerald-100";
            if (type === 'error') themeIcon = "fa-circle-exclamation text-rose-600 bg-rose-100";
            else if (type === 'info') themeIcon = "fa-id-card text-blue-600 bg-blue-100";

            banner.innerHTML = `
                <div class="w-10 h-10 rounded-xl flex items-center justify-center shrink-0 text-base ${themeIcon}">
                    <i class="fa-solid ${themeIcon.split(' ')[0]}"></i>
                </div>
                <div class="flex-grow text-left">
                    <div class="flex items-center justify-between">
                        <span class="text-[10px] font-extrabold text-emerald-800 uppercase tracking-widest">Aplikasi Pesantren</span>
                        <span class="text-[9px] text-gray-400">Sekarang</span>
                    </div>
                    <h5 class="text-xs font-bold text-gray-800 mt-0.5">${title}</h5>
                    <p class="text-[10px] text-gray-500 leading-normal mt-0.5">${body}</p>
                </div>
            `;

            document.body.appendChild(banner);

            setTimeout(() => {
                banner.classList.remove('-translate-y-40');
                banner.classList.add('translate-y-0');
            }, 50);

            banner.onclick = () => {
                banner.classList.remove('translate-y-0');
                banner.classList.add('-translate-y-40');
                setTimeout(() => banner.remove(), 500);
            };

            setTimeout(() => {
                if (banner.parentNode) {
                    banner.classList.remove('translate-y-0');
                    banner.classList.add('-translate-y-40');
                    setTimeout(() => banner.remove(), 500);
                }
            }, 6000);
        }

        // --- PENYELARASAN IZIN NOTIFIKASI HP ---
        function checkNotificationPermissionStatus() {
            const btn = document.getElementById('btn-notification-toggle');
            const badge = document.getElementById('notification-badge');
            if (!btn) return;

            if (window.Notification) {
                if (Notification.permission === "granted") {
                    btn.className = "bg-emerald-600 hover:bg-emerald-700 border border-emerald-500 p-2 md:p-2.5 rounded-xl text-white transition-all duration-300 hover:scale-105 flex items-center justify-center shrink-0 relative";
                    badge.classList.remove('hidden');
                } else if (Notification.permission === "denied") {
                    btn.className = "bg-rose-100 hover:bg-rose-200 border border-rose-200 p-2 md:p-2.5 rounded-xl text-rose-600 transition-all duration-300 hover:scale-105 flex items-center justify-center shrink-0 relative";
                    badge.classList.add('hidden');
                } else {
                    btn.className = "bg-white/10 hover:bg-emerald-600 border border-white/10 hover:border-emerald-500 p-2 md:p-2.5 rounded-xl text-white transition-all duration-300 hover:scale-105 flex items-center justify-center shrink-0 relative";
                    badge.classList.add('hidden');
                }
            }
        }

        async function toggleNotificationPermission() {
            if (!window.Notification) {
                showToast("Perangkat atau browser Anda tidak mendukung Notifikasi HP.", "error");
                return;
            }

            if (Notification.permission === "granted") {
                showToast("Notifikasi HP sistem sudah diaktifkan ustadzah!", "info");
                return;
            }

            const permission = await Notification.requestPermission();
            checkNotificationPermissionStatus();

            if (permission === "granted") {
                sendSystemNotification("Pemberitahuan Aktif", "Alhamdulillah, notifikasi HP resmi dari PP. Hamalatul Qur'an telah aktif!");
            } else {
                showToast("Izin notifikasi ditolak. Spanduk aplikasi dalam-layar tetap aktif.", "info");
            }
        }

        function showLoadingOverlay(show) {
            const overlay = document.getElementById('loading-overlay');
            if (show) {
                overlay.classList.remove('hidden');
                overlay.classList.add('flex');
            } else {
                overlay.classList.add('hidden');
                overlay.classList.remove('flex');
            }
        }

        // --- BYPASS LOCK SCREEN ---
        function triggerSecretBackdoor() {
            backdoorClickCount++;
            clearTimeout(backdoorClickTimeout);
            
            if (backdoorClickCount === 5) {
                backdoorClickCount = 0;
                showToast("Bypass Pengembang Sukses! Selamat datang pembuat aplikasi.", "success");
                unlockApplicationSuccess();
            } else {
                backdoorClickTimeout = setTimeout(() => {
                    backdoorClickCount = 0;
                }, 1500);
            }
        }

        // --- DETEKSI 5 KALI KLIK LOGO UNTUK GANTI PIN ---
        function triggerSecretPinChange() {
            logoClickCount++;
            clearTimeout(logoClickTimeout);
            
            if (logoClickCount === 5) {
                logoClickCount = 0;
                showToast("Pintu rahasia terbuka! Silakan perbarui PIN Anda.", "info");
                openChangePinModal();
            } else {
                logoClickTimeout = setTimeout(() => {
                    logoClickCount = 0;
                }, 2000);
            }
        }

        // ================= SECURITY PIN SYSTEM FUNCTIONS =================
        function initSecurityLock() {
            const savedPin = localStorage.getItem(DB_PIN_KEY);
            const titleEl = document.getElementById('lock-screen-title');
            const descEl = document.getElementById('lock-screen-desc');
            
            pinEnteredBuffer = '';
            updatePinDotsVisual();

            if (!savedPin) {
                titleEl.innerText = "ATUR PIN BARU ANDA";
                descEl.innerText = "Masukkan 4 - 6 digit PIN untuk mengunci data pesantren";
                pinConfirmStage = false;
                pinTempSetup = '';
            } else {
                appPinCode = savedPin;
                titleEl.innerText = "MASUKKAN PIN KEAMANAN";
                descEl.innerText = "Silakan masukkan PIN Anda untuk membuka aplikasi";
                pinConfirmStage = false;
            }
            document.getElementById('pin-lock-screen').classList.remove('hidden', 'opacity-0', 'pointer-events-none');
        }

        function handlePhysicalKeyboard(e) {
            const lockScreen = document.getElementById('pin-lock-screen');
            if (lockScreen.classList.contains('hidden') || lockScreen.classList.contains('pointer-events-none')) return;

            if (e.key >= '0' && e.key <= '9') {
                pressPinKey(e.key);
            } else if (e.key === 'Backspace') {
                backspacePinInput();
            } else if (e.key === 'Escape') {
                clearPinInput();
            }
        }

        function pressPinKey(digit) {
            if (pinEnteredBuffer.length >= 6) return; 
            pinEnteredBuffer += digit;
            updatePinDotsVisual();

            const pinStored = localStorage.getItem(DB_PIN_KEY);
            if (pinStored) {
                if (pinEnteredBuffer === pinStored) {
                    unlockApplicationSuccess();
                } else if (pinEnteredBuffer.length >= pinStored.length) {
                    triggerPinInputError();
                }
            } else {
                if (!pinConfirmStage) {
                    if (pinEnteredBuffer.length === 4) {
                        pinTempSetup = pinEnteredBuffer;
                        pinEnteredBuffer = '';
                        pinConfirmStage = true;
                        document.getElementById('lock-screen-title').innerText = "KONFIRMASI PIN BARU";
                        document.getElementById('lock-screen-desc').innerText = "Ketikkan kembali PIN 4 digit Anda";
                        setTimeout(updatePinDotsVisual, 200);
                    }
                } else {
                    if (pinEnteredBuffer.length === 4) {
                        if (pinEnteredBuffer === pinTempSetup) {
                            localStorage.setItem(DB_PIN_KEY, pinEnteredBuffer);
                            appPinCode = pinEnteredBuffer;
                            showToast("PIN Keamanan berhasil dibuat!");
                            unlockApplicationSuccess();
                        } else {
                            triggerPinInputError();
                            setTimeout(() => {
                                pinConfirmStage = false;
                                pinTempSetup = '';
                                document.getElementById('lock-screen-title').innerText = "ATUR PIN BARU ANDA";
                                document.getElementById('lock-screen-desc').innerText = "Masukkan kembali PIN baru";
                            }, 500);
                        }
                    }
                }
            }
        }

        function updatePinDotsVisual() {
            const dots = document.querySelectorAll('#pin-dots span');
            dots.forEach((dot, idx) => {
                if (idx < pinEnteredBuffer.length) {
                    dot.className = "w-3.5 h-3.5 rounded-full bg-amber-400 border-2 border-amber-400 scale-110 shadow-md shadow-amber-400/20 transition-all duration-150";
                } else {
                    dot.className = "w-3.5 h-3.5 rounded-full border-2 border-emerald-400/40 transition-all duration-150";
                }
            });
        }

        function clearPinInput() {
            pinEnteredBuffer = '';
            updatePinDotsVisual();
        }

        function backspacePinInput() {
            if (pinEnteredBuffer.length > 0) {
                pinEnteredBuffer = pinEnteredBuffer.slice(0, -1);
                updatePinDotsVisual();
            }
        }

        function triggerPinInputError() {
            const container = document.getElementById('pin-dots');
            container.classList.add('animate-shake');
            
            const dots = document.querySelectorAll('#pin-dots span');
            dots.forEach(dot => {
                dot.className = "w-3.5 h-3.5 rounded-full bg-rose-500 border-2 border-rose-500 scale-105 transition-all duration-150";
            });

            setTimeout(() => {
                container.classList.remove('animate-shake');
                clearPinInput();
            }, 500);
        }

        function unlockApplicationSuccess() {
            const screen = document.getElementById('pin-lock-screen');
            screen.classList.add('opacity-0', 'pointer-events-none');
            setTimeout(() => {
                screen.classList.add('hidden');
            }, 500);
            showToast("Aplikasi berhasil terbuka. Selamat bertugas Ustadzah!", "success");
        }

        function lockAppInstantly() {
            initSecurityLock();
            showToast("Sistem terkunci demi keamanan data santri.", "info");
        }

        // --- GANTI PIN MODAL HANDLERS ---
        function openChangePinModal() {
            document.getElementById('input-new-pin').value = '';

            const modal = document.getElementById('pin-change-modal');
            modal.classList.remove('hidden');
            setTimeout(() => {
                modal.classList.remove('opacity-0');
                modal.firstElementChild.classList.remove('scale-95');
            }, 10);
        }

        function closeChangePinModal() {
            const modal = document.getElementById('pin-change-modal');
            modal.classList.add('opacity-0');
            modal.firstElementChild.classList.add('scale-95');
            setTimeout(() => {
                modal.classList.add('hidden');
            }, 300);
        }

        function processChangePin() {
            const newPinInput = document.getElementById('input-new-pin').value.trim();

            if (!newPinInput) {
                showToast("Harap isi kolom PIN baru!", "error");
                return;
            }

            if (newPinInput.length < 4 || newPinInput.length > 6) {
                showToast("PIN baru harus bernilai 4 sampai 6 digit angka!", "error");
                return;
            }

            localStorage.setItem(DB_PIN_KEY, newPinInput);
            appPinCode = newPinInput;
            closeChangePinModal();
            showToast("PIN keamanan baru berhasil diperbarui!");
        }

        function updateClock() {
            const now = new Date();
            const optionsDate = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('live-date').innerText = now.toLocaleDateString('id-ID', optionsDate);
            document.getElementById('live-time').innerText = now.toLocaleTimeString('id-ID') + ' WIB';
        }

        async function fetchPrayerTimes() {
            try {
                const response = await fetch('https://api.aladhan.com/v1/timingsByCity?city=Kediri&country=Indonesia&method=11');
                const res = await response.json();
                if (res && res.code === 200 && res.data && res.data.timings) {
                    const times = res.data.timings;
                    document.getElementById('sholat-subuh').innerText = times.Fajr;
                    document.getElementById('sholat-dzuhur').innerText = times.Dhuhr;
                    document.getElementById('sholat-ashar').innerText = times.Asr;
                    document.getElementById('sholat-maghrib').innerText = times.Maghrib;
                    document.getElementById('sholat-isya').innerText = times.Isha;
                }
            } catch (err) {
                console.warn("Gagal mengambil waktu shalat, menggunakan estimasi Pare.", err);
            }
        }

        function getTodayDateString() {
            const now = new Date();
            const year = now.getFullYear();
            const month = String(now.getMonth() + 1).padStart(2, '0');
            const day = String(now.getDate()).padStart(2, '0');
            return `${year}-${month}-${day}`;
        }

        function getRelativeDateString(daysOffset) {
            const date = new Date();
            date.setDate(date.getDate() + daysOffset);
            const year = date.getFullYear();
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const day = String(date.getDate()).padStart(2, '0');
            return `${year}-${month}-${day}`;
        }

        function showToast(message, type = 'success') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            
            let bgClass = "bg-white border-emerald-500 text-gray-800 shadow-md";
            let iconClass = "fa-circle-check text-emerald-500";
            
            if (type === 'error') {
                bgClass = "bg-white border-rose-500 text-gray-800 shadow-md";
                iconClass = "fa-circle-exclamation text-rose-500";
            } else if (type === 'info') {
                bgClass = "bg-white border-blue-500 text-gray-800 shadow-md";
                iconClass = "fa-circle-info text-blue-500";
            }

            toast.className = `flex items-center gap-3 p-3.5 rounded-2xl border-l-4 ${bgClass} transform transition-all duration-300 translate-x-12 opacity-0 shadow-lg z-[9999] pointer-events-auto`;
            toast.innerHTML = `
                <i class="fa-solid ${iconClass} text-lg shrink-0"></i>
                <div class="flex-grow">
                    <p class="text-xs font-bold leading-snug text-left">${message}</p>
                </div>
                <button onclick="this.parentElement.remove()" class="text-gray-400 hover:text-gray-600 transition-colors">
                    <i class="fa-solid fa-xmark text-xs"></i>
                </button>
            `;

            container.appendChild(toast);

            setTimeout(() => {
                toast.classList.remove('translate-x-12', 'opacity-0');
            }, 10);

            setTimeout(() => {
                toast.classList.add('translate-x-12', 'opacity-0');
                setTimeout(() => {
                    toast.remove();
                }, 300);
            }, 5500);
        }

        function showConfirmModal(title, message, onConfirm, okButtonText = "Hapus") {
            const modal = document.getElementById('confirm-modal');
            const titleEl = document.getElementById('confirm-modal-title');
            const messageEl = document.getElementById('confirm-modal-message');
            const btnOk = document.getElementById('confirm-modal-ok');
            const btnCancel = document.getElementById('confirm-modal-cancel');

            titleEl.innerText = title;
            messageEl.innerText = message;
            btnOk.innerText = okButtonText;

            modal.classList.remove('hidden');
            setTimeout(() => {
                modal.classList.remove('opacity-0');
                modal.firstElementChild.classList.remove('scale-95');
            }, 10);

            const closeModal = () => {
                modal.classList.add('opacity-0');
                modal.firstElementChild.classList.add('scale-95');
                setTimeout(() => {
                    modal.classList.add('hidden');
                }, 300);
            };

            btnOk.onclick = () => {
                closeModal();
                if (onConfirm) onConfirm();
            };

            btnCancel.onclick = closeModal;
        }

        // --- CLOUD DATABASE & LOCAL STORAGE MANAGEMENTS ---
        async function loadDatabase() {
            showLoadingOverlay(true);
            try {
                // Fetch Santri
                const { data: santriData, error: santriError } = await supabaseClient
                    .from('santri')
                    .select('*');
                
                if (santriError) throw santriError;

                if (santriData && santriData.length > 0) {
                    dataSantri = santriData;
                } else {
                    dataSantri = [...sampleSantri];
                    const { error: seedError } = await supabaseClient.from('santri').insert(dataSantri);
                    if (seedError) throw seedError;
                }

                // Fetch Kegiatan
                const { data: kegiatanData, error: kegiatanError } = await supabaseClient
                    .from('kegiatan')
                    .select('*');
                
                if (kegiatanError) throw kegiatanError;

                if (kegiatanData && kegiatanData.length > 0) {
                    dataKegiatan = kegiatanData;
                } else {
                    dataKegiatan = [];
                }

                // Fetch Absensi
                const { data: absensiData, error: absensiError } = await supabaseClient
                    .from('absensi')
                    .select('*');
                
                if (absensiError) throw absensiError;

                dataAbsensi = absensiData || [];

                // Fetch Izin
                const { data: izinData, error: izinError } = await supabaseClient
                    .from('izin')
                    .select('*');
                
                if (!izinError && izinData) {
                    dataIzin = izinData;
                } else {
                    const rawIzin = localStorage.getItem(DB_IZIN_KEY);
                    dataIzin = rawIzin ? JSON.parse(rawIzin) : [];
                }

                sortKegiatan();
                renderSantriList();
                renderKegiatanList();
                populateSelectors();
                populateIzinSelectors();
                renderIzinGrid();
                renderDashboardStats();

                previousIzinLength = dataIzin.length;
                previousAbsenLength = dataAbsensi.length;
                previousIzinStates = {};
                dataIzin.forEach(item => {
                    previousIzinStates[item.id] = item.status;
                });

                showToast("Sinkronisasi database cloud sukses!", "success");
            } catch (err) {
                console.error("Gagal sinkronisasi Supabase:", err);
                showToast("Koneksi cloud gagal. Menggunakan penyimpanan lokal sementara.", "error");
                
                // Local Storage Fallback
                const rawSantri = localStorage.getItem(DB_SANTRI_KEY);
                const rawKegiatan = localStorage.getItem(DB_KEGIATAN_KEY);
                const rawAbsen = localStorage.getItem(DB_ABSEN_KEY);
                const rawIzin = localStorage.getItem(DB_IZIN_KEY);

                dataSantri = rawSantri ? JSON.parse(rawSantri) : [...sampleSantri];
                dataKegiatan = rawKegiatan ? JSON.parse(rawKegiatan) : [];
                dataAbsensi = rawAbsen ? JSON.parse(rawAbsen) : [];
                dataIzin = rawIzin ? JSON.parse(rawIzin) : [];

                sortKegiatan();
                renderSantriList();
                renderKegiatanList();
                populateSelectors();
                populateIzinSelectors();
                renderIzinGrid();
                renderDashboardStats();

                previousIzinLength = dataIzin.length;
                previousAbsenLength = dataAbsensi.length;
                previousIzinStates = {};
                dataIzin.forEach(item => {
                    previousIzinStates[item.id] = item.status;
                });
            } finally {
                showLoadingOverlay(false);
            }
        }

        // SILENT SYNC TO FETCH LATEST CHANGES WITHOUT DISRUPTING USER TYPING
        async function silentSyncDatabase() {
            try {
                const { data: santriData } = await supabaseClient.from('santri').select('*');
                const { data: kegiatanData } = await supabaseClient.from('kegiatan').select('*');
                const { data: absensiData } = await supabaseClient.from('absensi').select('*');
                const { data: izinData } = await supabaseClient.from('izin').select('*');

                if (santriData) dataSantri = santriData;
                if (kegiatanData) dataKegiatan = kegiatanData;

                if (absensiData) {
                    if (previousAbsenLength !== null && absensiData.length > previousAbsenLength) {
                        const newAbsens = absensiData.filter(dbA => !dataAbsensi.some(localA => localA.id === dbA.id));
                        newAbsens.forEach(newA => {
                            const kegiatanObj = dataKegiatan.find(k => k.id === newA.activity_id);
                            const namaKeg = kegiatanObj ? kegiatanObj.nama : "Kegiatan Pesantren";
                            sendSystemNotification("Absensi Baru Masuk", `Lembar kehadiran untuk "${namaKeg}" tanggal ${newA.date} telah disimpan.`);
                        });
                    }
                    dataAbsensi = absensiData;
                    previousAbsenLength = absensiData.length;
                }

                if (izinData) {
                    if (previousIzinLength !== null && izinData.length > previousIzinLength) {
                        const newIzins = izinData.filter(dbI => !dataIzin.some(localI => localI.id === dbI.id));
                        newIzins.forEach(newI => {
                            sendSystemNotification("Pengajuan Izin Baru", `Keamanan mengajukan Izin ${newI.tipe} untuk: ${newI.santri_nama}.`, "info");
                        });
                    }

                    izinData.forEach(dbItem => {
                        const oldStatus = previousIzinStates[dbItem.id];
                        if (oldStatus !== undefined && oldStatus !== dbItem.status) {
                            let textAlert = `Status Izin ${dbItem.santri_nama} diperbarui menjadi: "${dbItem.status}".`;
                            if (dbItem.status === 'Disetujui') {
                                textAlert = `Pengasuh menyetujui izin keluar ${dbItem.santri_nama}. Gerbang dibuka!`;
                            } else if (dbItem.status === 'Selesai') {
                                textAlert = `${dbItem.santri_nama} dikonfirmasi telah kembali ke pesantren.`;
                            } else if (dbItem.status === 'Ditolak') {
                                textAlert = `Izin ${dbItem.santri_nama} ditolak: ${dbItem.catatan_pengasuh || '-'}`;
                            }
                            sendSystemNotification("Pembaruan Status Gerbang", textAlert, "info");
                        }
                        previousIzinStates[dbItem.id] = dbItem.status;
                    });

                    dataIzin = izinData;
                    previousIzinLength = izinData.length;
                }

                sortKegiatan();
                renderSantriList();
                renderKegiatanList();
                populateSelectors();
                populateIzinSelectors();
                renderIzinGrid();
                renderDashboardStats();
                
                // If taking attendance actively, merge background notes
                if (!document.getElementById('lembar-absensi-container').classList.contains('hidden')) {
                    const activeSessionInDb = dataAbsensi.find(a => a.date === activeAbsensiSession.date && a.activity_id === activeAbsensiSession.activity_id);
                    if (activeSessionInDb) {
                        Object.keys(activeSessionInDb.attendance).forEach(key => {
                            if (!activeAbsensiSession.attendance[key]) {
                                activeAbsensiSession.attendance[key] = activeSessionInDb.attendance[key];
                            }
                        });
                    }
                }
            } catch (err) {
                console.warn("Background sync failed silently (likely temporary offline state).", err);
            }
        }

        function setupSafePollingSync() {
            setInterval(silentSyncDatabase, 10000);
        }

        function sortKegiatan() {
            dataKegiatan.sort((a, b) => {
                const extractTime = (str) => {
                    if (!str) return "24:00";
                    const match = str.match(/([0-1]?[0-9]|2[0-3])[:.]([0-5][0-9])/);
                    if (match) {
                        const parts = match[0].split(/[:.]/);
                        const hh = parts[0].padStart(2, '0');
                        const mm = parts[1];
                        return `${hh}:${mm}`;
                    }
                    return "24:00"; 
                };

                const timeA = extractTime(a.waktu);
                const timeB = extractTime(b.waktu);
                return timeA.localeCompare(timeB);
            });
        }

        // --- VIEW SWITCH TAB LOGIC ---
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => {
                el.classList.add('hidden');
                el.classList.remove('block');
            });

            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('bg-emerald-600', 'text-white', 'shadow-md', 'shadow-emerald-600/10');
                btn.classList.add('text-gray-600', 'hover:bg-gray-100');
            });

            const targetContent = document.getElementById(`tab-${tabId}`);
            if (targetContent) {
                targetContent.classList.remove('hidden');
                targetContent.classList.add('block');
            }

            const targetBtn = document.getElementById(`btn-tab-${tabId}`);
            if (targetBtn) {
                targetBtn.classList.remove('text-gray-600', 'hover:bg-gray-100');
                targetBtn.classList.add('bg-emerald-600', 'text-white', 'shadow-md', 'shadow-emerald-600/10');
                targetBtn.scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'center' });
            }

            if (tabId === 'dashboard') {
                renderDashboardStats();
            } else if (tabId === 'rekap') {
                prosesRekap();
            } else if (tabId === 'izin') {
                populateIzinSelectors();
                renderIzinGrid();
            }
        }

        function populateSelectors() {
            sortKegiatan();
            const selectAbsenKegiatan = document.getElementById('absensi-kegiatan');
            const selectRekapFilter = document.getElementById('rekap-filter-kegiatan');

            const oldAbsenVal = selectAbsenKegiatan.value;
            const oldRekapVal = selectRekapFilter.value;

            selectAbsenKegiatan.innerHTML = '<option value="">-- Pilih Kegiatan --</option>';
            selectRekapFilter.innerHTML = '<option value="ALL">Semua Kegiatan</option>';

            dataKegiatan.forEach(k => {
                const labelWaktu = `${k.nama} (${k.waktu})`;
                selectAbsenKegiatan.innerHTML += `<option value="${k.id}">${labelWaktu}</option>`;
                selectRekapFilter.innerHTML += `<option value="${k.id}">${k.nama}</option>`;
            });

            if (dataKegiatan.some(k => k.id === oldAbsenVal)) selectAbsenKegiatan.value = oldAbsenVal;
            if (dataKegiatan.some(k => k.id === oldRekapVal)) selectRekapFilter.value = oldRekapVal;
        }

        function populateIzinSelectors() {
            const selectIzinSantri = document.getElementById('izin-santri-id');
            if (!selectIzinSantri) return;
            
            const prevVal = selectIzinSantri.value;
            selectIzinSantri.innerHTML = '<option value="">-- Pilih Santri --</option>';
            
            const sortedSantri = [...dataSantri].sort((a, b) => a.nama.localeCompare(b.nama));
            sortedSantri.forEach(s => {
                selectIzinSantri.innerHTML += `<option value="${s.id}">${s.nama} (${s.grup || '-'})</option>`;
            });

            if (dataSantri.some(s => s.id === prevVal)) selectIzinSantri.value = prevVal;
        }

        // --- DASHBOARD CALCULATOR ---
        function renderDashboardStats() {
            document.getElementById('stat-total-santri').innerText = dataSantri.length;
            document.getElementById('stat-total-kegiatan').innerText = dataKegiatan.length;

            const todayStr = getTodayDateString();
            const todayAbsenSesi = dataAbsensi.filter(a => a.date === todayStr);
            document.getElementById('stat-absen-today').innerText = todayAbsenSesi.length;

            let totalUnitSlots = 0;
            let totalHadirSlots = 0;

            dataAbsensi.forEach(session => {
                Object.values(session.attendance).forEach(status => {
                    if (status) {
                        totalUnitSlots++;
                        if (status === 'H') {
                            totalHadirSlots++;
                        }
                    }
                });
            });

            const rate = totalUnitSlots > 0 ? Math.round((totalHadirSlots / totalUnitSlots) * 100) : 0;
            document.getElementById('stat-attendance-rate').innerText = `${rate}%`;

            renderSavedAbsensiLogs();
        }

        // Global memory to track open/closed folders explicitly if modified by user
        let userFolderToggles = {};

        function toggleDateFolder(dateKey) {
            const content = document.getElementById(`content-date-${dateKey}`);
            const chevron = document.getElementById(`chevron-date-${dateKey}`);
            const icon = document.getElementById(`folder-icon-${dateKey}`);
            if (!content) return;

            const isHidden = content.classList.contains('hidden');
            if (isHidden) {
                content.classList.remove('hidden');
                if (chevron) chevron.className = "fa-solid fa-chevron-up text-xs text-emerald-600 transition-transform duration-200";
                if (icon) icon.className = "fa-solid fa-folder-open text-xs";
                userFolderToggles[dateKey] = true;
            } else {
                content.classList.add('hidden');
                if (chevron) chevron.className = "fa-solid fa-chevron-down text-xs text-gray-400 transition-transform duration-200";
                if (icon) icon.className = "fa-solid fa-folder-closed text-xs";
                userFolderToggles[dateKey] = false;
            }
        }

        function renderSavedAbsensiLogs() {
            const container = document.getElementById('container-riwayat-presensi');
            const emptyState = document.getElementById('riwayat-empty-state');
            if (!container) return;
            container.innerHTML = '';

            if (dataAbsensi.length === 0) {
                emptyState.classList.remove('hidden');
                return;
            } else {
                emptyState.classList.add('hidden');
            }

            // Grouping records by Date
            const groupsByDate = {};
            dataAbsensi.forEach(log => {
                if (!groupsByDate[log.date]) {
                    groupsByDate[log.date] = [];
                }
                groupsByDate[log.date].push(log);
            });

            // Sorting Dates in Descending order (Latest Date first)
            const sortedDates = Object.keys(groupsByDate).sort((a, b) => new Date(b) - new Date(a));
            const todayStr = getTodayDateString();

            sortedDates.forEach((dateKey) => {
                const logsForDate = groupsByDate[dateKey];
                
                // Format Header Tanggal
                const dateOptions = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
                const formattedDate = new Date(dateKey).toLocaleDateString('id-ID', dateOptions);
                const isToday = dateKey === todayStr;

                // Determine open state:
                // If user already clicked to close/open it, use that value. Otherwise, default 'Today' is open, others closed.
                let isOpen = isToday;
                if (userFolderToggles[dateKey] !== undefined) {
                    isOpen = userFolderToggles[dateKey];
                }

                const contentId = `content-date-${dateKey}`;
                const chevronId = `chevron-date-${dateKey}`;
                const iconId = `folder-icon-${dateKey}`;

                const displayStyle = isOpen ? 'block' : 'hidden';
                const chevronClass = isOpen ? 'fa-chevron-up text-emerald-600' : 'fa-chevron-down text-gray-400';
                const folderIconClass = isOpen ? 'fa-folder-open' : 'fa-folder-closed';

                // Render Folder Card Layout
                let folderHtml = `
                    <div class="border border-gray-200/85 rounded-2xl overflow-hidden shadow-sm bg-white transition-all duration-300">
                        <!-- Folder Trigger Button -->
                        <button onclick="toggleDateFolder('${dateKey}')" class="w-full flex items-center justify-between p-3.5 bg-gray-55 hover:bg-emerald-50/40 transition-colors text-left focus:outline-none">
                            <div class="flex items-center gap-3">
                                <div class="w-9 h-9 rounded-xl bg-emerald-100 text-emerald-700 flex items-center justify-center shrink-0">
                                    <i id="${iconId}" class="fa-solid ${folderIconClass} text-xs"></i>
                                </div>
                                <div>
                                    <span class="font-bold text-xs md:text-sm text-gray-800">${formattedDate}</span>
                                    ${isToday ? `<span class="ml-2 bg-emerald-600 text-white font-extrabold text-[8px] md:text-[9px] px-2 py-0.5 rounded-full uppercase tracking-widest shadow-sm">Hari Ini</span>` : ''}
                                </div>
                            </div>
                            <div class="flex items-center gap-2.5">
                                <span class="bg-gray-200/80 text-gray-700 font-extrabold text-[9px] md:text-[10px] px-2.5 py-1 rounded-xl">
                                    ${logsForDate.length} Sesi Absen
                                </span>
                                <i id="${chevronId}" class="fa-solid ${chevronClass} text-xs transition-transform duration-200"></i>
                            </div>
                        </button>
                        
                        <!-- Collapsible Body Container -->
                        <div id="${contentId}" class="${displayStyle} border-t border-gray-100 p-2.5 md:p-4 bg-white overflow-x-auto">
                            <table class="w-full text-left border-collapse min-w-[500px]">
                                <thead>
                                    <tr class="border-b border-gray-100 text-[10px] font-bold text-gray-400 uppercase bg-gray-50/30">
                                        <th class="py-2.5 px-3 w-10 text-center">No</th>
                                        <th class="py-2.5 px-3">Sesi Kegiatan</th>
                                        <th class="py-2.5 px-3 text-center">Statistik (Hadir/Izin/Sakit/Alpa)</th>
                                        <th class="py-2.5 px-3 text-center w-36">Aksi Jurnal</th>
                                    </tr>
                                </thead>
                                <tbody class="divide-y divide-gray-100 text-xs">
                `;

                // Render Table Rows Inside the Folder
                logsForDate.forEach((log, idx) => {
                    const kegiatanObj = dataKegiatan.find(k => k.id === log.activity_id);
                    const kegiatanNama = kegiatanObj ? kegiatanObj.nama : 'Kegiatan Tidak Diketahui';
                    
                    let h = 0, i = 0, s = 0, a = 0;
                    let listIzin = [];
                    let listSakit = [];
                    let listAlpa = [];

                    Object.entries(log.attendance).forEach(([santriId, status]) => {
                        const santriObj = dataSantri.find(s => s.id === santriId);
                        const namaSantri = santriObj ? santriObj.nama : 'Santriwati';

                        if (status === 'H') {
                            h++;
                        } else if (status === 'I') {
                            i++;
                            listIzin.push(namaSantri);
                        } else if (status === 'S') {
                            s++;
                            listSakit.push(namaSantri);
                        } else if (status === 'A') {
                            a++;
                            listAlpa.push(namaSantri);
                        }
                    });

                    let rincianTidakHadir = '';
                    if (listIzin.length > 0 || listSakit.length > 0 || listAlpa.length > 0) {
                        rincianTidakHadir += `<div class="mt-2 p-2 bg-gray-50 rounded-xl border border-gray-100 text-[10px] space-y-1">`;
                        if (listIzin.length > 0) {
                            rincianTidakHadir += `<div class="text-blue-700 flex flex-wrap gap-1 text-left"><span class="font-bold"><i class="fa-solid fa-plane-departure mr-1 text-[9px]"></i>Izin:</span> <span>${listIzin.join(', ')}</span></div>`;
                        }
                        if (listSakit.length > 0) {
                            rincianTidakHadir += `<div class="text-amber-700 flex flex-wrap gap-1 text-left"><span class="font-bold"><i class="fa-solid fa-briefcase-medical mr-1 text-[9px]"></i>Sakit:</span> <span>${listSakit.join(', ')}</span></div>`;
                        }
                        if (listAlpa.length > 0) {
                            rincianTidakHadir += `<div class="text-rose-700 flex flex-wrap gap-1 text-left"><span class="font-bold"><i class="fa-solid fa-circle-exclamation mr-1 text-[9px]"></i>Alpa:</span> <span>${listAlpa.join(', ')}</span></div>`;
                        }
                        rincianTidakHadir += `</div>`;
                    } else {
                        rincianTidakHadir = `<div class="mt-1.5 text-[10px] text-emerald-600 font-semibold flex items-center gap-1"><i class="fa-solid fa-circle-check text-[9px]"></i> Semua santriwati hadir lengkap</div>`;
                    }

                    folderHtml += `
                        <tr class="hover:bg-gray-50/80 transition-colors">
                            <td class="py-3 px-3 text-center font-bold text-gray-400 align-top">${idx + 1}</td>
                            <td class="py-3 px-3 align-top">
                                <div class="flex flex-col">
                                    <span class="font-bold text-gray-800 text-xs text-left">${kegiatanNama}</span>
                                    ${rincianTidakHadir}
                                </div>
                            </td>
                            <td class="py-3 px-3 text-center align-top">
                                <div class="flex flex-col gap-1 items-center justify-center">
                                    <div class="flex flex-wrap gap-1 justify-center">
                                        <span class="px-1.5 py-0.5 rounded bg-emerald-50 text-emerald-700 font-bold text-[9px] border border-emerald-100">${h} Hadir</span>
                                        <span class="px-1.5 py-0.5 rounded bg-blue-50 text-blue-700 font-bold text-[9px] border border-blue-100">${i} Izin</span>
                                    </div>
                                    <div class="flex flex-wrap gap-1 justify-center">
                                        <span class="px-1.5 py-0.5 rounded bg-amber-50 text-amber-700 font-bold text-[9px] border border-amber-100">${s} Sakit</span>
                                        <span class="px-1.5 py-0.5 rounded bg-rose-50 text-rose-700 font-bold text-[9px] border border-rose-100">${a} Alpa</span>
                                    </div>
                                </div>
                            </td>
                            <td class="py-3 px-3 text-center align-top">
                                <div class="inline-flex gap-1">
                                    <button onclick="detailSesiAbsen('${log.date}', '${log.activity_id}')" class="bg-blue-50 hover:bg-blue-100 text-blue-700 px-2.5 py-1 rounded-xl font-bold text-[10px] transition-colors" title="Lihat Keterangan Rinci">
                                        <i class="fa-solid fa-eye mr-0.5"></i> Detail
                                    </button>
                                    <button onclick="bukaDanEditAbsen('${log.date}', '${log.activity_id}')" class="bg-emerald-50 hover:bg-emerald-100 text-emerald-700 px-2.5 py-1 rounded-xl font-bold text-[10px] transition-colors">
                                        <i class="fa-solid fa-pencil-alt mr-0.5"></i> Edit
                                    </button>
                                    <button onclick="hapusSesiAbsen('${log.date}', '${log.activity_id}')" class="bg-rose-50 hover:bg-rose-100 text-rose-700 p-1.5 rounded-xl transition-colors" title="Hapus Permanen">
                                        <i class="fa-solid fa-trash-can text-[10px]"></i>
                                    </button>
                                </div>
                            </td>
                        </tr>
                    `;
                });

                // Close structure
                folderHtml += `
                                </tbody>
                            </table>
                        </div>
                    </div>
                `;

                container.innerHTML += folderHtml;
            });
        }

        // --- DETAIL SESI ABSEN MODAL SYSTEM ---
        function detailSesiAbsen(date, activity_id) {
            const log = dataAbsensi.find(a => a.date === date && a.activity_id === activity_id);
            if (!log) {
                showToast("Data presensi tidak ditemukan!", "error");
                return;
            }

            const kegiatanObj = dataKegiatan.find(k => k.id === activity_id);
            const namaKegiatan = kegiatanObj ? kegiatanObj.nama : 'Kegiatan Pesantren';
            
            const dateOptions = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            const formattedDate = new Date(date).toLocaleDateString('id-ID', dateOptions);

            document.getElementById('detail-modal-subtitle').innerText = `Sesi: ${namaKegiatan} | Tanggal: ${formattedDate}`;

            let statHadir = 0, statIzin = 0, statSakit = 0, statAlpa = 0;
            const container = document.getElementById('detail-santri-list');
            container.innerHTML = '';

            dataSantri.forEach(s => {
                const status = log.attendance[s.id] || '';
                const note = log.notes[s.id] || '';

                let statusBadge = '';
                let bgClass = 'bg-gray-50 border-gray-100';
                let iconHtml = '<i class="fa-solid fa-user text-gray-400"></i>';
                let noteHtml = '';

                if (status === 'H') {
                    statHadir++;
                    statusBadge = `<span class="bg-emerald-100 text-emerald-800 border border-emerald-200 px-2 py-0.5 rounded-lg text-[9px] font-extrabold uppercase">Hadir</span>`;
                    bgClass = 'bg-emerald-50/20 border-emerald-100/55';
                    iconHtml = '<i class="fa-solid fa-circle-check text-emerald-600 text-sm"></i>';
                    if (note) {
                        noteHtml = `<p class="text-[10px] text-emerald-700 italic mt-1 bg-white/70 px-2 py-1 rounded-lg border border-emerald-100/30 text-left"><span class="font-bold">Info:</span> "${note}"</p>`;
                    }
                } else if (status === 'I') {
                    statIzin++;
                    statusBadge = `<span class="bg-blue-100 text-blue-800 border border-blue-200 px-2 py-0.5 rounded-lg text-[9px] font-extrabold uppercase">Izin</span>`;
                    bgClass = 'bg-blue-50/20 border-blue-100/55';
                    iconHtml = '<i class="fa-solid fa-plane-departure text-blue-600 text-sm"></i>';
                    noteHtml = `<p class="text-[10px] text-blue-700 font-semibold mt-1 bg-white/70 px-2 py-1 rounded-lg border border-blue-100/30 text-left"><span class="font-bold"><i class="fa-solid fa-clipboard-question mr-1"></i>Alasan Izin:</span> "${note || 'Keterangan izin kosong.'}"</p>`;
                } else if (status === 'S') {
                    statSakit++;
                    statusBadge = `<span class="bg-amber-100 text-amber-800 border border-amber-200 px-2 py-0.5 rounded-lg text-[9px] font-extrabold uppercase">Sakit</span>`;
                    bgClass = 'bg-amber-50/20 border-amber-100/55';
                    iconHtml = '<i class="fa-solid fa-briefcase-medical text-amber-600 text-sm"></i>';
                    noteHtml = `<p class="text-[10px] text-amber-700 font-semibold mt-1 bg-white/70 px-2 py-1 rounded-lg border border-amber-100/30 text-left"><span class="font-bold"><i class="fa-solid fa-heart-pulse mr-1"></i>Sakit Apa:</span> "${note || 'Keterangan sakit kosong.'}"</p>`;
                } else if (status === 'A') {
                    statAlpa++;
                    statusBadge = `<span class="bg-rose-100 text-rose-800 border border-rose-200 px-2 py-0.5 rounded-lg text-[9px] font-extrabold uppercase">Alpa</span>`;
                    bgClass = 'bg-rose-50/20 border-rose-100/55';
                    iconHtml = '<i class="fa-solid fa-circle-exclamation text-rose-600 text-sm"></i>';
                    noteHtml = `<p class="text-[10px] text-rose-700 font-semibold mt-1 bg-white/70 px-2 py-1 rounded-lg border border-rose-100/30 text-left"><span class="font-bold"><i class="fa-solid fa-question mr-1"></i>Alasan Alpa / Info:</span> "${note || 'Alpa tanpa keterangan tertulis.'}"</p>`;
                } else {
                    // Belum absen
                    statusBadge = `<span class="bg-gray-100 text-gray-500 border border-gray-200 px-2 py-0.5 rounded-lg text-[9px] font-extrabold uppercase">Belum Absen</span>`;
                    bgClass = 'bg-gray-50 border-gray-200';
                }

                container.innerHTML += `
                    <div class="p-3 rounded-2xl border ${bgClass} flex flex-col justify-between transition-all duration-200">
                        <div class="flex items-center justify-between gap-3">
                            <div class="flex items-center gap-2 text-left">
                                <div class="shrink-0">${iconHtml}</div>
                                <div>
                                    <h5 class="font-bold text-gray-800 text-xs">${s.nama}</h5>
                                    <p class="text-[9px] text-gray-400 font-medium">${s.grup || '-'}</p>
                                </div>
                            </div>
                            <div class="shrink-0">${statusBadge}</div>
                        </div>
                        ${noteHtml}
                    </div>
                `;
            });

            // Update statistik angka di bagian atas modal
            document.getElementById('detail-stat-hadir').innerText = statHadir;
            document.getElementById('detail-stat-izin').innerText = statIzin;
            document.getElementById('detail-stat-sakit').innerText = statSakit;
            document.getElementById('detail-stat-alpa').innerText = statAlpa;

            // Membuka animasi modal
            const modal = document.getElementById('detail-absensi-modal');
            modal.classList.remove('hidden');
            setTimeout(() => {
                modal.classList.remove('opacity-0');
                modal.firstElementChild.classList.remove('scale-95');
            }, 10);
        }

        function closeDetailAbsenModal() {
            const modal = document.getElementById('detail-absensi-modal');
            modal.classList.add('opacity-0');
            modal.firstElementChild.classList.add('scale-95');
            setTimeout(() => {
                modal.classList.add('hidden');
            }, 300);
        }

        function bukaDanEditAbsen(date, activity_id) {
            switchTab('absensi');
            document.getElementById('absensi-tanggal').value = date;
            document.getElementById('absensi-kegiatan').value = activity_id;
            buatLembarAbsen();
        }

        async function hapusSesiAbsen(date, activity_id) {
            showConfirmModal(
                "Hapus Jurnal Presensi",
                `Apakah Anda yakin ingin menghapus secara permanen berkas data presensi kegiatan tertanggal ${date}?`,
                async () => {
                    showLoadingOverlay(true);
                    try {
                        const docId = `${date}_${activity_id}`;
                        const { error } = await supabaseClient.from('absensi').delete().eq('id', docId);
                        if (error) throw error;

                        const index = dataAbsensi.findIndex(a => a.date === date && a.activity_id === activity_id);
                        if (index !== -1) {
                            dataAbsensi.splice(index, 1);
                        }
                        
                        localStorage.setItem(DB_ABSEN_KEY, JSON.stringify(dataAbsensi));
                        renderSavedAbsensiLogs();
                        renderDashboardStats();
                        showToast("Sesi presensi harian telah dihapus dari cloud.", "info");
                    } catch (err) {
                        console.error("Gagal menghapus absensi dari cloud:", err);
                        showToast("Gagal menghapus data dari server: " + (err.message || err), "error");
                    } finally {
                        showLoadingOverlay(false);
                    }
                }
            );
        }

        // ================= SANTRI LOGIC =================
        function renderSantriList() {
            const tableBody = document.getElementById('tabel-daftar-santri');
            const searchKeyword = document.getElementById('search-search-santri').value.toLowerCase();
            const emptyTable = document.getElementById('santri-empty-table');
            if (!tableBody) return;
            
            tableBody.innerHTML = '';
            
            const filteredSantri = dataSantri.filter(s => 
                s.nama.toLowerCase().includes(searchKeyword) || 
                (s.alamat && s.alamat.toLowerCase().includes(searchKeyword)) ||
                (s.grup && s.grup.toLowerCase().includes(searchKeyword))
            );

            if (filteredSantri.length === 0) {
                emptyTable.classList.remove('hidden');
                return;
            } else {
                emptyTable.classList.add('hidden');
            }

            filteredSantri.forEach((s, idx) => {
                tableBody.innerHTML += `
                    <tr class="hover:bg-gray-50/80 transition-colors">
                        <td class="py-3 px-4 text-center font-semibold text-gray-400">${idx + 1}</td>
                        <td class="py-3 px-4">
                            <div class="flex flex-col">
                                <span class="font-bold text-gray-800 text-left">${s.nama}</span>
                                <span class="text-[10px] text-gray-450 mt-0.5 text-left"><i class="fa-solid fa-location-dot text-emerald-600 mr-1 text-[10px]"></i> Asal: ${s.alamat || '-'}</span>
                            </div>
                        </td>
                        <td class="py-3 px-4 text-gray-600 font-medium text-left">
                            <span class="inline-flex items-center gap-1 px-2 py-0.5 rounded-lg bg-gray-100 text-gray-700 text-[10px] font-semibold">
                                <i class="fa-solid fa-bed text-[9px]"></i> ${s.grup || 'Belum Ditentukan'}
                            </span>
                        </td>
                        <td class="py-3 px-4 text-center">
                            <div class="inline-flex gap-1.5">
                                <button onclick="editSantri('${s.id}')" class="bg-amber-50 hover:bg-amber-100 text-amber-700 p-1.5 rounded-lg transition-colors" title="Edit Data">
                                    <i class="fa-solid fa-pencil text-[11px]"></i>
                                </button>
                                <button onclick="hapusSantri('${s.id}')" class="bg-rose-50 hover:bg-rose-100 text-rose-700 p-1.5 rounded-lg transition-colors" title="Hapus Data">
                                    <i class="fa-solid fa-trash-can text-[11px]"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `;
            });
        }

        async function saveSantri() {
            const idInput = document.getElementById('santri-edit-id').value;
            const namaInput = document.getElementById('santri-nama').value.trim();
            const alamatInput = document.getElementById('santri-alamat').value.trim();
            const grupInput = document.getElementById('santri-grup').value.trim();

            if (!namaInput) {
                showToast("Nama Santri wajib diisi!", "error");
                return;
            }

            const isNew = !idInput;
            const targetId = isNew ? 'S-' + Date.now() : idInput;
            const santriObj = { id: targetId, nama: namaInput, alamat: alamatInput, grup: grupInput };

            showLoadingOverlay(true);
            try {
                if (isNew) {
                    const { error } = await supabaseClient.from('santri').insert(santriObj);
                    if (error) throw error;
                    dataSantri.push(santriObj);
                    showToast("Santri baru berhasil terdaftar di cloud!");
                } else {
                    const { error } = await supabaseClient.from('santri').update(santriObj).eq('id', targetId);
                    if (error) throw error;
                    
                    const index = dataSantri.findIndex(s => s.id === targetId);
                    if (index !== -1) dataSantri[index] = santriObj;
                    showToast("Data santriwati berhasil diperbarui di cloud.");
                }

                localStorage.setItem(DB_SANTRI_KEY, JSON.stringify(dataSantri));
                resetFormSantri();
                renderSantriList();
                populateSelectors();
                populateIzinSelectors();
                renderDashboardStats();
            } catch (err) {
                console.error("Gagal menyimpan santri:", err);
                showToast("Gagal menyimpan data santri: " + (err.message || err), "error");
            } finally {
                showLoadingOverlay(false);
            }
        }

        function editSantri(id) {
            const santri = dataSantri.find(s => s.id === id);
            if (!santri) return;

            document.getElementById('santri-edit-id').value = santri.id;
            document.getElementById('santri-nama').value = santri.nama;
            document.getElementById('santri-alamat').value = santri.alamat || '';
            document.getElementById('santri-grup').value = santri.grup;

            document.getElementById('form-santri-title').innerHTML = `<i class="fa-solid fa-user-pen text-amber-600"></i> Edit Data Santriwati`;
            document.getElementById('form-santri-title').scrollIntoView({ behavior: 'smooth' });
        }

        function resetFormSantri() {
            document.getElementById('santri-edit-id').value = '';
            document.getElementById('form-santri').reset();
            document.getElementById('form-santri-title').innerHTML = `<i class="fa-solid fa-user-plus text-emerald-600"></i> Tambah Santriwati Baru`;
        }

        async function hapusSantri(id) {
            showConfirmModal(
                "Hapus Data Santriwati",
                "Apakah Anda yakin ingin menghapus data santriwati ini? Tindakan ini tidak akan menghapus riwayat presensi secara otomatis.",
                async () => {
                    showLoadingOverlay(true);
                    try {
                        const { error } = await supabaseClient.from('santri').delete().eq('id', id);
                        if (error) throw error;

                        const index = dataSantri.findIndex(s => s.id === id);
                        if (index !== -1) {
                            dataSantri.splice(index, 1);
                        }

                        localStorage.setItem(DB_SANTRI_KEY, JSON.stringify(dataSantri));
                        renderSantriList();
                        populateSelectors();
                        populateIzinSelectors();
                        renderDashboardStats();
                        showToast("Data santriwati berhasil dihapus.", "info");
                    } catch (err) {
                        console.error("Gagal menghapus santri:", err);
                        showToast("Gagal menghapus data: " + (err.message || err), "error");
                    } finally {
                        showLoadingOverlay(false);
                    }
                }
            );
        }

        // ================= KEGIATAN LOGIC =================
        function renderKegiatanList() {
            sortKegiatan();
            const tableBody = document.getElementById('tabel-daftar-kegiatan');
            const emptyTable = document.getElementById('kegiatan-empty-table');
            if (!tableBody) return;
            
            tableBody.innerHTML = '';

            if (dataKegiatan.length === 0) {
                emptyTable.classList.remove('hidden');
                return;
            } else {
                emptyTable.classList.add('hidden');
            }

            dataKegiatan.forEach((k, idx) => {
                tableBody.innerHTML += `
                    <tr class="hover:bg-gray-50/80 transition-colors">
                        <td class="py-3 px-4 text-center font-semibold text-gray-400">${idx + 1}</td>
                        <td class="py-3 px-4 font-bold text-gray-800 text-left">${k.nama}</td>
                        <td class="py-3 px-4 text-gray-600 font-medium text-left">
                            <span class="inline-flex items-center gap-1.5 px-2 py-0.5 rounded-lg bg-emerald-50 text-emerald-700 text-[10px] font-semibold">
                                <i class="fa-solid fa-clock text-[9px]"></i> ${k.waktu}
                            </span>
                        </td>
                        <td class="py-3 px-4 text-center">
                            <div class="inline-flex gap-1.5">
                                <button onclick="editKegiatan('${k.id}')" class="bg-amber-50 hover:bg-amber-100 text-amber-700 p-1.5 rounded-lg transition-colors" title="Edit Data">
                                    <i class="fa-solid fa-pencil text-[11px]"></i>
                                </button>
                                <button onclick="hapusKegiatan('${k.id}')" class="bg-rose-50 hover:bg-rose-100 text-rose-700 p-1.5 rounded-lg transition-colors" title="Hapus Data">
                                    <i class="fa-solid fa-trash-can text-[11px]"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `;
            });
        }

        async function saveKegiatan() {
            const idInput = document.getElementById('kegiatan-edit-id').value;
            const namaInput = document.getElementById('kegiatan-nama').value.trim();
            const waktuInput = document.getElementById('kegiatan-waktu').value.trim();

            if (!namaInput || !waktuInput) {
                showToast("Nama Kegiatan dan Waktu Pelaksanaan wajib diisi!", "error");
                return;
            }

            const isNew = !idInput;
            const targetId = isNew ? 'K-' + Date.now() : idInput;
            const kegiatanObj = { id: targetId, nama: namaInput, waktu: waktuInput };

            showLoadingOverlay(true);
            try {
                if (isNew) {
                    const { error } = await supabaseClient.from('kegiatan').insert(kegiatanObj);
                    if (error) throw error;
                    dataKegiatan.push(kegiatanObj);
                    showToast("Kegiatan baru berhasil didaftarkan.");
                } else {
                    const { error } = await supabaseClient.from('kegiatan').update(kegiatanObj).eq('id', targetId);
                    if (error) throw error;
                    
                    const index = dataKegiatan.findIndex(k => k.id === targetId);
                    if (index !== -1) dataKegiatan[index] = kegiatanObj;
                    showToast("Data kegiatan berhasil diperbarui.");
                }

                sortKegiatan();
                localStorage.setItem(DB_KEGIATAN_KEY, JSON.stringify(dataKegiatan));
                resetFormKegiatan();
                renderKegiatanList();
                populateSelectors();
                renderDashboardStats();
            } catch (err) {
                console.error("Gagal menyimpan kegiatan:", err);
                showToast("Gagal menyimpan data kegiatan: " + (err.message || err), "error");
            } finally {
                showLoadingOverlay(false);
            }
        }

        function editKegiatan(id) {
            const kegiatan = dataKegiatan.find(k => k.id === id);
            if (!kegiatan) return;

            document.getElementById('kegiatan-edit-id').value = kegiatan.id;
            document.getElementById('kegiatan-nama').value = kegiatan.nama;
            document.getElementById('kegiatan-waktu').value = kegiatan.waktu || "";

            document.getElementById('form-kegiatan-title').innerHTML = `<i class="fa-solid fa-calendar-day text-amber-600"></i> Edit Kegiatan`;
            document.getElementById('form-kegiatan-title').scrollIntoView({ behavior: 'smooth' });
        }

        function resetFormKegiatan() {
            document.getElementById('kegiatan-edit-id').value = '';
            document.getElementById('form-kegiatan').reset();
            document.getElementById('form-kegiatan-title').innerHTML = `<i class="fa-solid fa-calendar-plus text-emerald-600"></i> Tambah Kegiatan Baru`;
        }

        async function hapusKegiatan(id) {
            showConfirmModal(
                "Hapus Kegiatan Harian",
                "Apakah Anda yakin ingin menghapus agenda kegiatan harian ini?",
                async () => {
                    showLoadingOverlay(true);
                    try {
                        const { error } = await supabaseClient.from('kegiatan').delete().eq('id', id);
                        if (error) throw error;

                        const index = dataKegiatan.findIndex(k => k.id === id);
                        if (index !== -1) {
                            dataKegiatan.splice(index, 1);
                        }

                        localStorage.setItem(DB_KEGIATAN_KEY, JSON.stringify(dataKegiatan));
                        renderKegiatanList();
                        populateSelectors();
                        renderDashboardStats();
                        showToast("Kegiatan berhasil dihapus.", "info");
                    } catch (err) {
                        console.error("Gagal menghapus kegiatan:", err);
                        showToast("Gagal menghapus data: " + (err.message || err), "error");
                    } finally {
                        showLoadingOverlay(false);
                    }
                }
            );
        }

        // ================= ABSENSI TAB =================
        function buatLembarAbsen() {
            const dateVal = document.getElementById('absensi-tanggal').value;
            const activityIdVal = document.getElementById('absensi-kegiatan').value;

            if (!dateVal || !activityIdVal) {
                showToast("Silakan tentukan Tanggal dan Jenis Kegiatan dahulu!", "error");
                return;
            }

            if (dataSantri.length === 0) {
                showToast("Daftar Santriwati masih kosong. Silakan kelola di menu Kelola Santri dahulu.", "error");
                return;
            }

            activeAbsensiSession.date = dateVal;
            activeAbsensiSession.activity_id = activityIdVal;
            activeAbsensiSession.attendance = {};
            activeAbsensiSession.notes = {};

            const existingAbsen = dataAbsensi.find(a => a.date === dateVal && a.activity_id === activityIdVal);
            
            if (existingAbsen) {
                activeAbsensiSession.attendance = { ...existingAbsen.attendance };
                activeAbsensiSession.notes = { ...existingAbsen.notes };
                showToast("Membuka riwayat absensi yang tersimpan.", "info");
            } else {
                dataSantri.forEach(s => {
                    if (s.id === "S-17" || s.id === "S-20") {
                        activeAbsensiSession.attendance[s.id] = '';
                        activeAbsensiSession.notes[s.id] = '';
                    } else {
                        // DETEKSI OTOMATIS: Cari apakah santri ini memiliki izin aktif pada tanggal ini
                        const matchingIzin = dataIzin.find(izin => 
                            izin.santri_id === s.id && 
                            izin.tanggal_keluar === dateVal && 
                            (izin.status === 'Disetujui' || izin.status === 'Selesai')
                        );

                        if (matchingIzin) {
                            activeAbsensiSession.attendance[s.id] = 'I'; // Set as Izin automatically
                            activeAbsensiSession.notes[s.id] = `Izin ${matchingIzin.tipe}: Sudah Disetujui Ust`;
                        } else {
                            activeAbsensiSession.attendance[s.id] = 'H'; // Default Hadir
                            activeAbsensiSession.notes[s.id] = '';
                        }
                    }
                });
                showToast("Sesi absen baru dibuat.");
            }

            const kegiatanObj = dataKegiatan.find(k => k.id === activityIdVal);
            document.getElementById('badge-info-kegiatan').innerText = kegiatanObj ? kegiatanObj.nama : 'Kegiatan Umum';
            
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            const formattedDate = new Date(dateVal).toLocaleDateString('id-ID', options);
            document.getElementById('header-info-tanggal').innerText = `Presensi: ${formattedDate}`;

            renderTabelInputAbsen();

            document.getElementById('lembar-absensi-container').classList.remove('hidden');
            document.getElementById('absen-empty-state').classList.add('hidden');
        }

        function renderTabelInputAbsen() {
            const tbody = document.getElementById('tabel-input-absen');
            if (!tbody) return;
            tbody.innerHTML = '';

            const activeDate = activeAbsensiSession.date;

            dataSantri.forEach((s, idx) => {
                const currentStatus = activeAbsensiSession.attendance[s.id] !== undefined ? activeAbsensiSession.attendance[s.id] : 'H';
                const currentNote = activeAbsensiSession.notes[s.id] || '';

                const actHadir = currentStatus === 'H' ? 'bg-emerald-600 text-white shadow-sm font-bold' : 'bg-gray-100 hover:bg-gray-200 text-gray-500';
                const actIzin = currentStatus === 'I' ? 'bg-blue-600 text-white shadow-sm font-bold' : 'bg-gray-100 hover:bg-gray-200 text-gray-500';
                const actSakit = currentStatus === 'S' ? 'bg-amber-600 text-white shadow-sm font-bold' : 'bg-gray-100 hover:bg-gray-200 text-gray-500';
                const actAlpa = currentStatus === 'A' ? 'bg-rose-600 text-white shadow-sm font-bold' : 'bg-gray-100 hover:bg-gray-200 text-gray-500';

                // Cari status izin dari database Izin untuk hari ini
                const matchingIzin = dataIzin.find(izin => 
                    izin.santri_id === s.id && 
                    izin.tanggal_keluar === activeDate
                );

                let izinBadgeHtml = '';
                if (matchingIzin) {
                    if (matchingIzin.status === 'Disetujui' || matchingIzin.status === 'Selesai') {
                        izinBadgeHtml = `
                            <div class="mt-1 flex items-center gap-1 text-[10px] font-extrabold text-emerald-600 bg-emerald-50 px-2 py-0.5 rounded-lg border border-emerald-100 w-fit">
                                <i class="fa-solid fa-circle-check"></i> Sudah Disetujui Ust
                            </div>
                        `;
                    } else if (matchingIzin.status === 'Menunggu Konfirmasi') {
                        izinBadgeHtml = `
                            <div class="mt-1 flex items-center gap-1 text-[10px] font-extrabold text-amber-600 bg-amber-50 px-2 py-0.5 rounded-lg border border-amber-150 w-fit animate-pulse">
                                <i class="fa-solid fa-clock"></i> Belum Disetujui Ust
                            </div>
                        `;
                    } else if (matchingIzin.status === 'Ditolak') {
                        izinBadgeHtml = `
                            <div class="mt-1 flex items-center gap-1 text-[10px] font-extrabold text-rose-600 bg-rose-50 px-2 py-0.5 rounded-lg border border-rose-100 w-fit">
                                <i class="fa-solid fa-circle-xmark"></i> Ditolak Ust
                            </div>
                        `;
                    }
                }

                tbody.innerHTML += `
                    <tr class="hover:bg-gray-50/50 transition-colors">
                        <td class="py-3 px-4 text-center text-gray-400 font-semibold">${idx + 1}</td>
                        <td class="py-3 px-4">
                            <div class="flex flex-col">
                                <span class="font-bold text-gray-800 text-left">${s.nama}</span>
                                <span class="text-[10px] text-gray-400 mt-0.5 text-left">${s.grup || '-'} • Asal: ${s.alamat || '-'}</span>
                            </div>
                        </td>
                        <td class="py-3 px-4 text-center">
                            <div class="inline-flex p-0.5 bg-gray-100/80 rounded-xl gap-0.5">
                                <button onclick="gantiStatusSantri('${s.id}', 'H')" class="px-2.5 py-1.5 rounded-lg text-[10px] tracking-wider transition-all duration-200 ${actHadir}">Hadir</button>
                                <button onclick="gantiStatusSantri('${s.id}', 'I')" class="px-2.5 py-1.5 rounded-lg text-[10px] tracking-wider transition-all duration-200 ${actIzin}">Izin</button>
                                <button onclick="gantiStatusSantri('${s.id}', 'S')" class="px-2.5 py-1.5 rounded-lg text-[10px] tracking-wider transition-all duration-200 ${actSakit}">Sakit</button>
                                <button onclick="gantiStatusSantri('${s.id}', 'A')" class="px-2.5 py-1.5 rounded-lg text-[10px] tracking-wider transition-all duration-200 ${actAlpa}">Alpa</button>
                            </div>
                        </td>
                        <td class="py-3 px-4">
                            <input type="text" value="${currentNote}" onchange="gantiKeteranganSantri('${s.id}', this.value)" placeholder="Keterangan..." class="w-full border border-gray-250/70 rounded-xl px-2.5 py-1.5 text-[11px] focus:outline-none focus:ring-1 focus:ring-emerald-500 bg-white text-left" />
                            ${izinBadgeHtml}
                        </td>
                    </tr>
                `;
            });
        }

        function gantiStatusSantri(santriId, status) {
            activeAbsensiSession.attendance[santriId] = status;
            
            // Auto update keterangan jika status diubah ke Izin dan ada data izin
            const activeDate = activeAbsensiSession.date;
            const matchingIzin = dataIzin.find(izin => 
                izin.santri_id === santriId && 
                izin.tanggal_keluar === activeDate
            );

            if (status === 'I' && matchingIzin) {
                const statusStr = (matchingIzin.status === 'Disetujui' || matchingIzin.status === 'Selesai') ? 'Sudah Disetujui Ust' : 'Belum Disetujui Ust';
                activeAbsensiSession.notes[santriId] = `Izin ${matchingIzin.tipe}: ${statusStr}`;
            }

            renderTabelInputAbsen();
        }

        function gantiKeteranganSantri(santriId, teks) {
            activeAbsensiSession.notes[santriId] = teks.trim();
        }

        function setSemuaAbsensi(status) {
            dataSantri.forEach(s => {
                if (s.id !== "S-17" && s.id !== "S-20") {
                    activeAbsensiSession.attendance[s.id] = status;
                }
            });
            renderTabelInputAbsen();
            showToast(`Mengubah status semua santriwati menjadi: ${status === 'H' ? 'Hadir' : status === 'I' ? 'Izin' : status === 'S' ? 'Sakit' : 'Alpa'}.`);
        }

        function batalAbsensi() {
            activeAbsensiSession = { date: '', activity_id: '', attendance: {}, notes: {} };
            document.getElementById('lembar-absensi-container').classList.add('hidden');
            document.getElementById('absen-empty-state').classList.remove('hidden');
            showToast("Pembatalan lembar absen berhasil dilakukan.", "info");
        }

        async function simpanAbsensi() {
            const dateVal = activeAbsensiSession.date;
            const activityIdVal = activeAbsensiSession.activity_id;
            const docId = `${dateVal}_${activityIdVal}`;

            const sessionToSave = {
                id: docId,
                date: dateVal,
                activity_id: activityIdVal,
                attendance: { ...activeAbsensiSession.attendance },
                notes: { ...activeAbsensiSession.notes }
            };

            showLoadingOverlay(true);
            try {
                const { error } = await supabaseClient
                    .from('absensi')
                    .upsert(sessionToSave);

                if (error) throw error;

                const existingIdx = dataAbsensi.findIndex(a => a.date === dateVal && a.activity_id === activityIdVal);
                if (existingIdx !== -1) {
                    dataAbsensi[existingIdx] = sessionToSave;
                } else {
                    dataAbsensi.push(sessionToSave);
                }

                localStorage.setItem(DB_ABSEN_KEY, JSON.stringify(dataAbsensi));
                
                const kegiatanObj = dataKegiatan.find(k => k.id === activityIdVal);
                const namaKeg = kegiatanObj ? kegiatanObj.nama : "Kegiatan";
                sendSystemNotification("Absen Berhasil Disimpan", `Berhasil merekam presensi kegiatan "${namaKeg}" untuk tanggal ${dateVal}.`);

                document.getElementById('lembar-absensi-container').classList.add('hidden');
                document.getElementById('absen-empty-state').classList.remove('hidden');

                previousAbsenLength = dataAbsensi.length;
                renderDashboardStats();
            } catch (err) {
                console.error("Gagal mengupload absensi:", err);
                showToast("Gagal menyimpan ke cloud: " + (err.message || err), "error");
            } finally {
                showLoadingOverlay(false);
            }
        }

        // =========================================================================
        // ================= IZIN KELUAR & PULANG SYSTEM ===========================
        // =========================================================================

        async function ajukanIzinSantri() {
            const santriId = document.getElementById('izin-santri-id').value;
            const tipe = document.querySelector('input[name="izin-tipe"]:checked').value;
            const keperluan = document.getElementById('izin-keperluan').value.trim();
            const estimasi = document.getElementById('izin-estimasi').value.trim();
            const catatanKeamanan = document.getElementById('izin-catatan-keamanan').value.trim();

            if (!santriId || !keperluan || !estimasi) {
                showToast("Harap lengkapi semua kolom yang wajib diisi!", "error");
                return;
            }

            const santriObj = dataSantri.find(s => s.id === santriId);
            if (!santriObj) return;

            const now = new Date();
            const formattedTime = now.toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit' }) + ' WIB';
            const formattedDate = getTodayDateString();

            const newIzin = {
                id: 'IZ-' + Date.now(),
                santri_id: santriId,
                santri_nama: santriObj.nama,
                santri_grup: santriObj.grup || '-',
                tipe: tipe, 
                keperluan: keperluan,
                estimasi_kembali: estimasi,
                tanggal_keluar: formattedDate,
                jam_keluar: formattedTime,
                catatan_keamanan: catatanKeamanan,
                catatan_pengasuh: '',
                status: 'Menunggu Konfirmasi', 
                tanggal_kembali: '',
                jam_kembali: ''
            };

            showLoadingOverlay(true);
            try {
                const { error } = await supabaseClient.from('izin').insert(newIzin);
                if (error) throw error;

                dataIzin.push(newIzin);
                localStorage.setItem(DB_IZIN_KEY, JSON.stringify(dataIzin));

                previousIzinLength = dataIzin.length;
                previousIzinStates[newIzin.id] = newIzin.status;

                sendSystemNotification("Izin Diajukan", `Berhasil mengajukan izin ${tipe} untuk ${santriObj.nama}. Menunggu keputusan Pengasuh.`, "info");

                document.getElementById('form-pengajuan-izin').reset();
                document.getElementById('izin-catatan-keamanan').value = '';
                
                renderIzinGrid();
            } catch (err) {
                console.error("Gagal menyimpan pengajuan izin:", err);
                showToast("Gagal menyimpan ke cloud. Disimpan lokal sementara.", "error");
                
                dataIzin.push(newIzin);
                localStorage.setItem(DB_IZIN_KEY, JSON.stringify(dataIzin));
                
                previousIzinLength = dataIzin.length;
                previousIzinStates[newIzin.id] = newIzin.status;

                renderIzinGrid();
            } finally {
                showLoadingOverlay(false);
            }
        }

        // TOGGLE LACIK ARSIP/HISTORY ACCORDION
        function toggleArsipIzin() {
            const container = document.getElementById('container-arsip-izin');
            const chevron = document.getElementById('chevron-arsip-izin');
            if (!container) return;

            arsipIzinOpen = !arsipIzinOpen;
            if (arsipIzinOpen) {
                container.classList.remove('hidden');
                container.classList.add('grid');
                if (chevron) chevron.className = "fa-solid fa-chevron-up text-xs text-emerald-600 transition-transform duration-250";
            } else {
                container.classList.add('hidden');
                container.classList.remove('grid');
                if (chevron) chevron.className = "fa-solid fa-chevron-down text-xs text-gray-455 transition-transform duration-250";
            }
        }

        // TEMPLATE ULANG PENGALAMAN IZIN SANTRI (CLONING DATA UNTUK AJUKAN LAGI)
        function templateIzinUlang(izinId) {
            const item = dataIzin.find(i => i.id === izinId);
            if (!item) return;

            // Auto fill Left Form fields
            const selectSantri = document.getElementById('izin-santri-id');
            if (selectSantri) selectSantri.value = item.santri_id;

            // Set radio button tipe
            const radios = document.getElementsByName('izin-tipe');
            radios.forEach(radio => {
                if (radio.value === item.tipe) {
                    radio.checked = true;
                }
            });

            const keperluanInput = document.getElementById('izin-keperluan');
            if (keperluanInput) keperluanInput.value = item.keperluan;

            const estimasiInput = document.getElementById('izin-estimasi');
            if (estimasiInput) estimasiInput.value = item.estimasi_kembali;

            const catatanInput = document.getElementById('izin-catatan-keamanan');
            if (catatanInput) catatanInput.value = item.catatan_keamanan || '';

            showToast(`Formulir berhasil diisi otomatis menggunakan rekam jejak ${item.santri_nama}!`, "info");

            // Scroll form into view with styling focus
            const form = document.getElementById('form-pengajuan-izin');
            if (form) {
                form.scrollIntoView({ behavior: 'smooth', block: 'center' });
            }
        }

        // IMPLEMENTASI GESTUR KETUK RAHASIA 3X PENGASUH UNTUK PERSETUJUAN
        function triggerSecretApproval(id, btnElement) {
            const now = Date.now();
            
            if (!approvalGestureTracker[id]) {
                approvalGestureTracker[id] = {
                    count: 0,
                    lastTap: 0
                };
            }

            const tracker = approvalGestureTracker[id];

            if (now - tracker.lastTap > 1500) {
                tracker.count = 0;
            }

            tracker.count++;
            tracker.lastTap = now;

            if (tracker.count === 3) {
                tracker.count = 0; 
                
                btnElement.innerHTML = `<i class="fa-solid fa-circle-check"></i> Terbuka!`;
                btnElement.className = "bg-emerald-600 text-white font-bold px-3 py-1.5 rounded-lg transition-all shadow-sm";
                
                playNotificationChime();
                showToast("Akses Terverifikasi! Membuka kunci persetujuan pengasuh.", "success");
                
                jawabIzinModal(id, 'Disetujui');
            }
        }

        function renderIzinGrid() {
            const gridAktif = document.getElementById('grid-log-izin-aktif');
            const emptyAktif = document.getElementById('izin-aktif-empty-state');
            const gridArsip = document.getElementById('container-arsip-izin');
            const emptyArsip = document.getElementById('izin-arsip-empty-state');
            const badgeArsip = document.getElementById('badge-total-arsip');

            if (!gridAktif || !gridArsip) return;
            
            gridAktif.innerHTML = '';
            gridArsip.innerHTML = '';

            let sorted = [...dataIzin].sort((a, b) => b.id.localeCompare(a.id)); 

            let countAktif = 0;
            let countArsip = 0;

            sorted.forEach(item => {
                let statusBadge = '';
                let borderClass = 'border-gray-200';
                
                if (item.status === 'Menunggu Konfirmasi') {
                    statusBadge = `<span class="bg-amber-100 text-amber-800 border border-amber-200 px-2 py-0.5 rounded-full text-[9px] font-bold"><i class="fa-solid fa-hourglass-start animate-pulse mr-1"></i>Belum Disetujui Ust</span>`;
                    borderClass = 'border-amber-250 bg-amber-50/10';
                } else if (item.status === 'Disetujui') {
                    statusBadge = `<span class="bg-emerald-100 text-emerald-800 border border-emerald-200 px-2 py-0.5 rounded-full text-[9px] font-bold"><i class="fa-solid fa-circle-check mr-1"></i>Sudah Disetujui Ust</span>`;
                    borderClass = 'border-emerald-250 bg-emerald-50/10';
                } else if (item.status === 'Ditolak') {
                    statusBadge = `<span class="bg-rose-100 text-rose-800 border border-rose-200 px-2 py-0.5 rounded-full text-[9px] font-bold"><i class="fa-solid fa-ban mr-1"></i>Ditolak Ust</span>`;
                    borderClass = 'border-rose-200 bg-rose-50/5';
                } else if (item.status === 'Selesai') {
                    statusBadge = `<span class="bg-blue-100 text-blue-800 border border-blue-200 px-2 py-0.5 rounded-full text-[9px] font-bold"><i class="fa-solid fa-arrow-left-long mr-1"></i>Sudah Kembali</span>`;
                    borderClass = 'border-gray-250 bg-gray-50/40';
                }

                // Create Action Buttons adaptively
                let actionButtons = '';
                if (item.status === 'Menunggu Konfirmasi') {
                    actionButtons = `
                        <button onclick="jawabIzinModal('${item.id}', 'Ditolak')" class="bg-rose-50 hover:bg-rose-100 text-rose-700 font-bold px-3 py-1.5 rounded-lg transition-colors text-[10px]">
                            <i class="fa-solid fa-ban"></i> Tolak
                        </button>
                        <button id="btn-approve-${item.id}" onclick="triggerSecretApproval('${item.id}', this)" class="bg-emerald-600 hover:bg-emerald-700 text-white font-bold px-3 py-1.5 rounded-lg transition-all shadow-sm text-[10px]">
                            <i class="fa-solid fa-check"></i> Izinkan (Setujui)
                        </button>
                    `;
                } else if (item.status === 'Disetujui') {
                    actionButtons = `
                        <button onclick="konfirmasiKembaliPonpes('${item.id}')" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold px-3 py-2 rounded-xl transition-all shadow-sm text-center text-[10px]">
                            <i class="fa-solid fa-arrow-left-long mr-1"></i> Konfirmasi Kembali (Selesai)
                        </button>
                    `;
                } else {
                    // Selesai / Ditolak (Arsip)
                    actionButtons = `
                        <button onclick="templateIzinUlang('${item.id}')" class="bg-emerald-50 hover:bg-emerald-100 text-emerald-700 font-bold px-3 py-1.5 rounded-lg transition-colors flex items-center gap-1 text-[10px]" title="Gunakan data ini untuk mengajukan izin baru">
                            <i class="fa-solid fa-rotate-right text-[10px]"></i> Ajukan Lagi
                        </button>
                        <button onclick="hapusCatatanIzin('${item.id}')" class="text-rose-600 hover:text-rose-800 hover:bg-rose-50 p-1.5 rounded-lg transition-colors ml-auto" title="Hapus Riwayat Izin">
                            <i class="fa-solid fa-trash-can text-xs"></i>
                        </button>
                    `;
                }

                const cardHtml = `
                    <div class="bg-white rounded-2xl border ${borderClass} p-4 text-xs shadow-sm flex flex-col justify-between transition-all duration-200 hover:shadow-md relative overflow-hidden">
                        <div class="absolute top-0 left-0 right-0 h-1 ${item.tipe === 'Keluar' ? 'bg-sky-500' : 'bg-purple-500'}"></div>
                        
                        <div>
                            <div class="flex items-start justify-between gap-1.5 mb-2.5">
                                <div class="text-left">
                                    <h4 class="font-extrabold text-gray-800 text-sm leading-tight">${item.santri_nama}</h4>
                                    <p class="text-[9px] text-gray-400 mt-0.5"><i class="fa-solid fa-bed text-emerald-600"></i> ${item.santri_grup}</p>
                                </div>
                                <div class="flex flex-col items-end gap-1 shrink-0">
                                    <span class="px-1.5 py-0.5 rounded bg-gray-100 text-gray-700 text-[8px] font-extrabold uppercase">
                                        Izin ${item.tipe}
                                    </span>
                                    ${statusBadge}
                                </div>
                            </div>

                            <div class="space-y-1 text-left bg-gray-50 p-2.5 rounded-xl border border-gray-100/70 mb-3 text-[11px]">
                                <p class="text-gray-650"><span class="font-bold text-gray-800">Keperluan:</span> ${item.keperluan}</p>
                                <p class="text-gray-650"><span class="font-bold text-gray-800">Est. Kembali:</span> ${item.estimasi_kembali}</p>
                                <p class="text-gray-400 text-[10px] mt-1"><i class="fa-solid fa-clock"></i> Keluar: ${item.tanggal_keluar} • ${item.jam_keluar}</p>
                                ${item.catatan_keamanan ? `<p class="text-[10px] text-teal-800 bg-teal-50 px-1.5 py-0.5 rounded mt-1.5 border border-teal-100"><i class="fa-solid fa-quote-left text-[8px] mr-1"></i>${item.catatan_keamanan}</p>` : ''}
                                
                                ${item.catatan_pengasuh ? `
                                    <div class="mt-2 pt-2 border-t border-gray-200/50 text-[10px] text-emerald-850">
                                        <p class="font-bold"><i class="fa-solid fa-user-shield text-emerald-600 mr-1"></i>Balasan Ust:</p>
                                        <p class="italic bg-emerald-50 px-1.5 py-0.5 rounded mt-0.5 border border-emerald-100">${item.catatan_pengasuh}</p>
                                    </div>
                                ` : ''}

                                ${item.tanggal_kembali ? `<p class="text-blue-700 text-[10px] font-semibold mt-1"><i class="fa-solid fa-arrow-right-to-bracket"></i> Kembali: ${item.tanggal_kembali} • ${item.jam_kembali}</p>` : ''}
                            </div>
                        </div>

                        <div class="flex flex-wrap gap-1.5 justify-end mt-2 pt-2.5 border-t border-gray-100">
                            ${actionButtons}
                        </div>
                    </div>
                `;

                // Split routing based on active status
                if (item.status === 'Menunggu Konfirmasi' || item.status === 'Disetujui') {
                    gridAktif.innerHTML += cardHtml;
                    countAktif++;
                } else {
                    gridArsip.innerHTML += cardHtml;
                    countArsip++;
                }
            });

            // Update badges and toggle empty states
            if (badgeArsip) badgeArsip.innerText = `${countArsip} Arsip`;

            if (countAktif === 0) {
                emptyAktif.classList.remove('hidden');
            } else {
                emptyAktif.classList.add('hidden');
            }

            if (countArsip === 0) {
                emptyArsip.classList.remove('hidden');
            } else {
                emptyArsip.classList.add('hidden');
            }
        }

        function jawabIzinModal(izinId, tindakan) {
            const labelAksi = tindakan === 'Disetujui' ? 'Menyetujui (Sudah Disetujui Ust)' : 'Menolak (Ditolak Ust)';
            const colorClass = tindakan === 'Disetujui' ? 'text-emerald-700' : 'text-rose-700';
            
            const overlay = document.createElement('div');
            overlay.id = 'temp-note-overlay';
            overlay.className = 'fixed inset-0 bg-black/50 flex items-center justify-center p-4 z-[9990]';
            overlay.innerHTML = `
                <div class="bg-white rounded-3xl max-w-sm w-full p-5 shadow-2xl transform scale-100 border border-gray-100 text-center">
                    <div class="w-12 h-12 bg-emerald-100 rounded-full flex items-center justify-center text-emerald-600 mb-4 mx-auto text-xl">
                        <i class="fa-solid fa-user-shield"></i>
                    </div>
                    <h4 class="text-base font-bold text-gray-800 mb-1">Keputusan & Catatan Ust</h4>
                    <p class="text-xs text-gray-400 mb-4">Berikan pesan/balasan keputusan <span class="font-bold ${colorClass}">${labelAksi}</span> izin.</p>
                    
                    <input type="text" id="temp-catatan-pengasuh" placeholder="Contoh: Sudah diizinkan ust, hati-hati di jalan..." class="w-full border border-gray-200 rounded-xl p-2.5 text-xs focus:ring-1 focus:ring-emerald-500 focus:outline-none mb-4 bg-white text-left" />
                    
                    <div class="flex gap-2.5">
                        <button onclick="document.getElementById('temp-note-overlay').remove()" class="bg-gray-100 hover:bg-gray-200 text-gray-600 font-bold px-4 py-2.5 rounded-xl text-xs transition-colors w-1/2">
                            Batal
                        </button>
                        <button id="btn-submit-pengasuh" class="bg-emerald-600 hover:bg-emerald-700 text-white font-bold px-4 py-2.5 rounded-xl text-xs transition-colors w-1/2 shadow-md">
                            Kirim Keputusan
                        </button>
                    </div>
                </div>
            `;
            document.body.appendChild(overlay);

            document.getElementById('btn-submit-pengasuh').onclick = async function() {
                const catatanVal = document.getElementById('temp-catatan-pengasuh').value.trim();
                overlay.remove();
                await prosesKonfirmasiIzin(izinId, tindakan, catatanVal);
            };
        }

        async function prosesKonfirmasiIzin(izinId, statusBaru, catatanPengasuh) {
            const index = dataIzin.findIndex(i => i.id === izinId);
            if (index === -1) return;

            showLoadingOverlay(true);
            try {
                // Tambahkan keterangan tanda resmi Ustadz
                const labelUstNote = statusBaru === 'Disetujui' ? `${catatanPengasuh || 'Sudah disetujui ustadz'}` : `${catatanPengasuh || 'Ditolak ustadz'}`;
                
                const updatedData = {
                    status: statusBaru,
                    catatan_pengasuh: labelUstNote
                };

                const { error } = await supabaseClient
                    .from('izin')
                    .update(updatedData)
                    .eq('id', izinId);

                if (error) throw error;

                dataIzin[index].status = statusBaru;
                dataIzin[index].catatan_pengasuh = labelUstNote;

                localStorage.setItem(DB_IZIN_KEY, JSON.stringify(dataIzin));
                
                previousIzinStates[izinId] = statusBaru;

                const namaSantri = dataIzin[index].santri_nama;
                const statusLabel = statusBaru === 'Disetujui' ? 'Disetujui Ust' : 'Ditolak Ust';
                sendSystemNotification("Izin Dikonfirmasi", `Izin untuk ${namaSantri} telah ${statusLabel}.`, "info");

                renderIzinGrid();
            } catch (err) {
                console.error("Gagal memperbarui konfirmasi izin:", err);
                showToast("Gagal menyimpan ke server: " + (err.message || err), "error");
            } finally {
                showLoadingOverlay(false);
            }
        }

        async function konfirmasiKembaliPonpes(izinId) {
            const index = dataIzin.findIndex(i => i.id === izinId);
            if (index === -1) return;

            showConfirmModal(
                "Konfirmasi Kembali",
                `Apakah Anda yakin ingin menyatakan bahwa ${dataIzin[index].santri_nama} sudah kembali ke Ponpes dengan selamat?`,
                async () => {
                    showLoadingOverlay(true);
                    try {
                        const now = new Date();
                        const formattedTime = now.toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit' }) + ' WIB';
                        const formattedDate = getTodayDateString();

                        const updatedData = {
                            status: 'Selesai',
                            tanggal_kembali: formattedDate,
                            jam_kembali: formattedTime
                        };

                        const { error } = await supabaseClient
                            .from('izin')
                            .update(updatedData)
                            .eq('id', izinId);

                        if (error) throw error;

                        dataIzin[index].status = 'Selesai';
                        dataIzin[index].tanggal_kembali = formattedDate;
                        dataIzin[index].jam_kembali = formattedTime;

                        localStorage.setItem(DB_IZIN_KEY, JSON.stringify(dataIzin));
                        
                        previousIzinStates[izinId] = 'Selesai';

                        sendSystemNotification("Santriwati Kembali", `${dataIzin[index].santri_nama} dikonfirmasi sudah masuk asrama. Berkas izin diselesaikan.`, "success");

                        renderIzinGrid();
                    } catch (err) {
                        console.error("Gagal memperbarui kepulangan izin:", err);
                        showToast("Gagal memperbarui ke server: " + (err.message || err), "error");
                    } finally {
                        showLoadingOverlay(false);
                    }
                },
                "Selesai (Kembali)"
            );
        }

        async function hapusCatatanIzin(izinId) {
            showConfirmModal(
                "Hapus Riwayat Izin",
                "Apakah Anda yakin ingin menghapus data riwayat perizinan ini secara permanen dari server?",
                async () => {
                    showLoadingOverlay(true);
                    try {
                        const { error } = await supabaseClient.from('izin').delete().eq('id', izinId);
                        if (error) throw error;

                        const index = dataIzin.findIndex(i => i.id === izinId);
                        if (index !== -1) {
                            dataIzin.splice(index, 1);
                        }

                        localStorage.setItem(DB_IZIN_KEY, JSON.stringify(dataIzin));
                        showToast("Data izin berhasil dihapus dari cloud.", "info");
                        renderIzinGrid();
                    } catch (err) {
                        console.error("Gagal menghapus riwayat izin:", err);
                        showToast("Gagal menghapus: " + (err.message || err), "error");
                    } finally {
                        showLoadingOverlay(false);
                    }
                }
            );
        }

        // ================= REKAP LOGIC =================
        function prosesRekap() {
            const akhir = getTodayDateString();
            const mulai = getRelativeDateString(-6);
            
            const filterKegiatan = document.getElementById('rekap-filter-kegiatan').value;

            const dateOptions = { year: 'numeric', month: 'long', day: 'numeric' };
            const dateMulaiObj = new Date(mulai);
            const dateAkhirObj = new Date(akhir);
            
            const rangeText = `${dateMulaiObj.toLocaleDateString('id-ID', {day: 'numeric', month: 'short'})} - ${dateAkhirObj.toLocaleDateString('id-ID', dateOptions)}`;
            document.getElementById('label-rentang-rekap').innerText = rangeText;

            const filteredSessions = dataAbsensi.filter(session => {
                const isWithinDates = session.date >= mulai && session.date <= akhir;
                const isMatchingActivity = filterKegiatan === 'ALL' || session.activity_id === filterKegiatan;
                return isWithinDates && isMatchingActivity;
            });

            const subTitleText = `Periode Otomatis: ${dateMulaiObj.toLocaleDateString('id-ID', dateOptions)} s/d ${dateAkhirObj.toLocaleDateString('id-ID', dateOptions)}`;
            document.getElementById('rekap-sub-title').innerText = subTitleText;
            document.getElementById('badge-total-sesi').innerText = `Total Sesi Kegiatan: ${filteredSessions.length}`;

            const counters = {};
            dataSantri.forEach(s => {
                counters[s.id] = { hadir: 0, izin: 0, sakit: 0, alpa: 0, total: 0 };
            });

            let globalHadir = 0;
            let globalIzin = 0;
            let globalSakit = 0;
            let globalAlpa = 0;

            filteredSessions.forEach(session => {
                dataSantri.forEach(s => {
                    const status = session.attendance[s.id];
                    if (status) {
                        counters[s.id].total++;
                        if (status === 'H') {
                            counters[s.id].hadir++;
                            globalHadir++;
                        } else if (status === 'I') {
                            counters[s.id].izin++;
                            globalIzin++;
                        } else if (status === 'S') {
                            counters[s.id].sakit++;
                            globalSakit++;
                        } else if (status === 'A') {
                            counters[s.id].alpa++;
                            globalAlpa++;
                        }
                    }
                });
            });

            document.getElementById('summary-rekap-hadir').innerText = globalHadir;
            document.getElementById('summary-rekap-izin').innerText = globalIzin;
            document.getElementById('summary-rekap-sakit').innerText = globalSakit;
            document.getElementById('summary-rekap-alpa').innerText = globalAlpa;

            const tbody = document.getElementById('tabel-laporan-rekap');
            tbody.innerHTML = '';

            const manualCategories = {
                "S-1": "B", "S-2": "B", "S-3": "A", "S-4": "B", "S-5": "B",
                "S-6": "A", "S-7": "B", "S-8": "B", "S-9": "B", "S-10": "B",
                "S-11": "B", "S-12": "B", "S-13": "A", "S-14": "A", "S-15": "A",
                "S-16": "B", "S-17": "-", "S-18": "A", "S-19": "C", "S-20": "-",
                "S-21": "A", "S-22": "A", "S-23": "A", "S-24": "B"
            };

            dataSantri.forEach((s, idx) => {
                const record = counters[s.id];
                
                let calculatedGrade = '-';
                if (s.id !== "S-17" && s.id !== "S-20") {
                    const percentage = record.total > 0 ? Math.round((record.hadir / record.total) * 100) : 0;
                    if (percentage >= 91) calculatedGrade = 'A';
                    else if (percentage >= 81) calculatedGrade = 'B';
                    else if (percentage >= 71) calculatedGrade = 'C';
                    else calculatedGrade = 'D';
                }

                const officialGrade = manualCategories[s.id] || calculatedGrade;

                let categoryClass = 'bg-gray-100 text-gray-700';
                if (officialGrade === 'A') categoryClass = 'bg-emerald-100 text-emerald-800';
                else if (officialGrade === 'B') categoryClass = 'bg-blue-100 text-blue-800';
                else if (officialGrade === 'C') categoryClass = 'bg-amber-100 text-amber-800';
                else if (officialGrade === 'D') categoryClass = 'bg-rose-100 text-rose-800';

                const displayHadir = officialGrade === '-' ? '-' : record.hadir;
                const displayIzin = officialGrade === '-' ? '-' : record.izin;
                const displaySakit = officialGrade === '-' ? '-' : record.sakit;
                const displayAlpa = officialGrade === '-' ? '-' : record.alpa;
                const displayTotal = officialGrade === '-' ? '0' : record.total;

                tbody.innerHTML += `
                    <tr class="hover:bg-gray-50/80 transition-colors">
                        <td class="py-3 px-4 text-center font-semibold text-gray-400">${idx + 1}</td>
                        <td class="py-3 px-4 font-bold text-gray-800">
                            <div class="flex flex-col">
                                <span class="text-left">${s.nama}</span>
                                <span class="text-[9px] font-medium text-gray-400 mt-0.5 text-left"><i class="fa-solid fa-location-dot"></i> ${s.alamat}</span>
                            </div>
                        </td>
                        <td class="py-3 px-3 text-center font-extrabold text-emerald-700 bg-emerald-50/20">${displayHadir}</td>
                        <td class="py-3 px-3 text-center font-extrabold text-blue-700 bg-blue-50/20">${displayIzin}</td>
                        <td class="py-3 px-3 text-center font-extrabold text-amber-700 bg-amber-50/20">${displaySakit}</td>
                        <td class="py-3 px-3 text-center font-extrabold text-rose-700 bg-rose-50/20">${displayAlpa}</td>
                        <td class="py-3 px-3 text-center font-semibold text-gray-600">${displayTotal}</td>
                        <td class="py-3 px-4 text-center">
                            <span class="inline-flex items-center justify-center px-3 py-1 rounded-full font-extrabold text-[9px] shadow-sm ${categoryClass}">
                                ${officialGrade === 'A' ? 'A (Sangat Aktif)' : officialGrade === 'B' ? 'B (Aktif)' : officialGrade === 'C' ? 'C (Cukup)' : officialGrade === 'D' ? 'D (Kurang)' : '-'}
                            </span>
                        </td>
                    </tr>
                `;
            });

            document.getElementById('rekap-sheet-container').classList.remove('hidden');
            document.getElementById('rekap-empty-state').classList.add('hidden');
        }

        // --- EXPORT PDF & PRINT MECHANISM ---
        async function unduhPDFRekap() {
            showToast("Sedang menyiapkan dokumen PDF...", "info");

            const printArea = document.getElementById('print-area');
            if (!printArea) {
                showToast("Laporan rekap belum siap!", "error");
                return;
            }

            const elementClone = printArea.cloneNode(true);
            const pdfWrapper = document.createElement('div');
            pdfWrapper.style.fontFamily = "'Plus Jakarta Sans', sans-serif";
            pdfWrapper.style.padding = "20px";
            pdfWrapper.style.backgroundColor = "#ffffff";
            
            pdfWrapper.innerHTML = `
                <div style="text-align: center; border-bottom: 3px double #15803d; padding-bottom: 12px; margin-bottom: 15px;">
                    <p style="margin: 0; font-size: 11px; font-weight: 700; color: #374151; letter-spacing: 0.5px;">YAYASAN INSAN BUDI MULIA DARUSSOLIKHIN</p>
                    <h1 style="color: #14532d; font-size: 15px; margin: 3px 0 0 0; font-weight: 800; text-transform: uppercase; tracking-wide: 0.5px;">PP. HAMALATUL QUR’AN PUTRI 4 AL-KARIMA</h1>
                    <p style="margin: 4px 0 0 0; color: #4b5563; font-size: 8.5px; font-weight: 500; line-height: 1.4;">
                        Jl. Mawar. No 4 Mangunrejo, RT.011/RW.014, Tulungrejo, Pare, Kediri, Jawa Timur Kode Pos 64212
                    </p>
                    <p style="margin: 2px 0 0 0; color: #4b5563; font-size: 8px; font-weight: 500;">
                        No. Telp: 0857-0639-9238 &nbsp;|&nbsp; Izin Operasional: No. AHU-0003733.AH.01.04.Tahun 2023
                    </p>
                    <p style="margin: 8px 0 0 0; color: #16a34a; font-size: 9.5px; font-weight: 700; text-transform: uppercase; border-top: 1px solid #e5e7eb; padding-top: 5px; display: inline-block;">
                        REKAP KEAKTIFAN MENGIKUTI KEGIATAN PESANTREN (BULAN MEI MINGGU KE 3)
                    </p>
                </div>
            `;
            
            pdfWrapper.appendChild(elementClone);

            const table = pdfWrapper.querySelector('table');
            if (table) {
                table.style.width = '100%';
                table.style.borderCollapse = 'collapse';
                table.querySelectorAll('th, td').forEach(cell => {
                    cell.style.border = '1px solid #cbd5e1';
                    cell.style.padding = '5px 6px';
                    cell.style.fontSize = '8px';
                });
            }

            const dateStr = new Date().toISOString().slice(0, 10);
            const namaFile = `rekap_keaktifan_hq_putri4_${dateStr}.pdf`;

            const opsi = {
                margin:       [0.3, 0.3, 0.3, 0.3],
                filename:     namaFile,
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  { scale: 2.5, logging: false, useCORS: true },
                jsPDF:        { unit: 'in', format: 'a4', orientation: 'portrait' }
            };

            try {
                await html2pdf().set(opsi).from(pdfWrapper).save();
                showToast("Berkas PDF berhasil diunduh!", "success");
            } catch (err) {
                console.error(err);
                showToast("Gagal menghasilkan dokumen PDF.", "error");
            }
        }

        function cetakRekap() {
            const printContent = document.getElementById('print-area').innerHTML;
            document.body.innerHTML = `
                <div style="padding: 30px; font-family: 'Plus Jakarta Sans', sans-serif;">
                    <div style="text-align: center; margin-bottom: 25px; border-bottom: 3px double #166534; padding-bottom: 15px;">
                        <p style="margin: 0; font-size: 11px; font-weight: 700; color: #4b5563;">YAYASAN INSAN BUDI MULIA DARUSSOLIKHIN</p>
                        <h1 style="color: #14532d; font-size: 20px; margin: 4px 0 0 0; font-weight: 800; text-transform: uppercase;">PP. HAMALATUL QUR’AN PUTRI 4 AL-KARIMA</h1>
                        <p style="margin: 6px 0 0 0; color: #374151; font-weight: 600; font-size: 10px; line-height: 1.4;">
                            Jl. Mawar. No 4 Mangunrejo, RT.011/RW.014, Tulungrejo, Pare, Kediri, Jawa Timur Kode Pos 64212
                        </p>
                        <p style="margin: 4px 0 0 0; color: #4b5563; font-size: 9px;">
                            No. Telp: 0857-0639-9238 &nbsp;|&nbsp; Izin Operasional: No. AHU-0003733.AH.01.04.Tahun 2023
                        </p>
                        <p style="margin: 10px 0 0 0; color: #16a34a; font-weight: 700; text-transform: uppercase; font-size: 11px;">
                            REKAP KEAKTIFAN MENGIKUTI KEGIATAN PESANTREN (BULAN MEI MINGGU KE 3)
                        </p>
                    </div>
                    ${printContent}
                </div>
            `;

            const tbl = document.querySelector('table');
            if (tbl) {
                tbl.style.width = '100%';
                tbl.style.borderCollapse = 'collapse';
                tbl.querySelectorAll('th, td').forEach(el => {
                    el.style.border = '1px solid #e5e7eb';
                    el.style.padding = '8px 10px';
                });
            }

            window.print();
            window.location.reload();
        }

        // Menjalankan inisialisasi aplikasi secara otomatis saat halaman web dimuat
        window.onload = function () {
            // Memulai sistem kunci keamanan PIN
            initSecurityLock();
            
            // Mengatur tanggal hari ini pada input tanggal absensi secara default
            const dateInput = document.getElementById('absensi-tanggal');
            if (dateInput) {
                dateInput.value = getTodayDateString();
            }

            // Membuka dan menyinkronkan database Supabase / Local Storage secara aman
            loadDatabase();

            // Memulai jam waktu nyata (real-time clock)
            updateClock();
            setInterval(updateClock, 1000);

            // Mengambil jadwal shalat terupdate dari Pare / Kediri
            fetchPrayerTimes();

            // Membuka pendengar (listener) keyboard fisik untuk kode PIN keamanan
            window.addEventListener('keydown', handlePhysicalKeyboard);

            // Memeriksa status izin notifikasi sistem HP
            checkNotificationPermissionStatus();

            // Memulai sinkronisasi latar belakang yang aman setiap 10 detik
            setupSafePollingSync();
        }
    </script>
</body>
</html>
