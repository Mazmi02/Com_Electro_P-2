<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Computer Vision (Visi Komputer)?", "id": "Bagaimana Komputer Menginterpretasi Gambar." },
  { "en": "Apa Itu Machine Learning (Pembelajaran Mesin)?", "id": "Algoritma Yang Belajar Dari Data." },
  { "en": "Apa Itu Neural Network (Jaringan Saraf)?", "id": "Model Pembelajaran Terinspirasi Otak Manusia." },
  { "en": "Apa Itu Deep Learning (Pembelajaran Mendalam)?", "id": "Jaringan Saraf Dengan Banyak Lapisan." },
  { "en": "Apa Itu TensorFlow?", "id": "Library Open Source Untuk Machine Learning." },
  { "en": "Apa Itu PyTorch?", "id": "Library Machine Learning Populer Lainnya." },
  { "en": "Apa Itu OpenCV (Open Computer Vision)?", "id": "Library Open Source Untuk Computer Vision." },
  { "en": "Apa Itu ROS (Robot Operating System)?", "id": "Kerangka Kerja (Framework) Untuk Robotika." },
  { "en": "Apa Itu Motor DC (Direct Current) Tanam (Coreless)?", "id": "Motor DC Tanpa Inti Besi (Cepat)." },
  { "en": "Apa Itu Motor DC (Direct Current) Pancake?", "id": "Motor DC Dengan Bentuk Datar (Tipis)." },
  { "en": "Apa Itu Generator Tacho (Tachogenerator)?", "id": "Sensor Kecepatan Putar (Menghasilkan Tegangan)." },
  { "en": "Apa Itu Sensor Quadrature Encoder?", "id": "Encoder Inkremental (Mendeteksi Arah Putaran)." },
  { "en": "Apa Itu Sinyal A Dan B Encoder?", "id": "Dua Sinyal Bergeser Fasa 90 Derajat." },
  { "en": "Apa Itu Indeks Z (Index) Encoder?", "id": "Satu Pulsa Per Putaran (Referensi Nol)." },
  { "en": "Apa Itu Resolusi Encoder?", "id": "Jumlah Pulsa Per Putaran (PPR)." },
  { "en": "Apa Itu H-Bridge (Jembatan H)?", "id": "Rangkaian Pengontrol Arah Putaran Motor DC." },
  { "en": "Kenapa Disebut H-Bridge (Jembatan H)?", "id": "Skema Rangkaian Mirip Huruf H." },
  { "en": "Apa Itu IC (Integrated Circuit) H-Bridge?", "id": "IC Driver Motor (Contoh: L298N)." },
  { "en": "Apa Itu Shoot-Through (H-Bridge)?", "id": "Kondisi Dua Transistor Vertikal Menyala." },
  { "en": "Kenapa Shoot-Through (H-Bridge) Berbahaya?", "id": "Menyebabkan Hubung Singkat Catu Daya." },
  { "en": "Apa Itu Dead Time (Waktu Mati)?", "id": "Jeda Mencegah Shoot-Through." },
  { "en": "Apa Itu Kontrol Motor Bipolar Stepper?", "id": "Mengontrol Dua Kumparan Penuh (H-Bridge)." },
  { "en": "Apa Itu Kontrol Motor Unipolar Stepper?", "id": "Mengontrol Kumparan Center-Tap (Lebih Sederhana)." },
  { "en": "Apa Itu Microstepping (Motor Stepper)?", "id": "Membagi Langkah (Gerakan Lebih Halus)." },
  { "en": "Bagaimana Cara Microstepping Bekerja?", "id": "Menggunakan Sinyal Mirip Sinusoidal." },
  { "en": "Apa Itu Torsi Tahan (Holding Torque) Stepper?", "id": "Kekuatan Torsi Saat Motor Diam." },
  { "en": "Apa Itu Torsi Tarik (Pull-In Torque) Stepper?", "id": "Torsi Saat Motor Mulai Berputar." },
  { "en": "Apa Itu Linear Actuator (Aktuator Linear)?", "id": "Aktuator Penggerak Gerakan Lurus." },
  { "en": "Contoh Linear Actuator (Aktuator Linear)?", "id": "Motor Stepper Linear, Solenoid." },
  { "en": "Apa Itu Voice Coil (Kumparan Suara)?", "id": "Aktuator Linear Cepat (Hard Disk)." },
  { "en": "Apa Itu Sistem Suspensi (Hard Disk)?", "id": "Lengan Mekanis Penahan Kepala Baca." },
  { "en": "Apa Itu Platter (Hard Disk)?", "id": "Piringan Magnetik Penyimpan Data." },
  { "en": "Apa Itu Head (Hard Disk)?", "id": "Kepala Pembaca Penulis Data Magnetik." },
  { "en": "Apa Itu SSD (Solid State Drive)?", "id": "Penyimpanan Data Berbasis Memori Flash." },
  { "en": "Apa Kelebihan SSD (Solid State Drive) Dari HDD?", "id": "Jauh Lebih Cepat, Tahan Guncangan." },
  { "en": "Apa Itu NVMe (Non-Volatile Memory Express)?", "id": "Protokol SSD Super Cepat (Lewat PCIe)." },
  { "en": "Apa Itu SATA (Serial Advanced Technology Attachment)?", "id": "Antarmuka Konektor Hard Drive/SSD." },
  { "en": "Apa Itu M.2 (Slot)?", "id": "Bentuk Konektor SSD Kecil (NVMe/SATA)." },
  { "en": "Apa Itu Wear Leveling (SSD)?", "id": "Teknik Meratakan Penggunaan Sel Memori Flash." },
  { "en": "Kenapa Wear Leveling (SSD) Penting?", "id": "MemperpanjAng Umur Pakai SSD." },
  { "en": "Apa Itu Sel Memori Flash NAND?", "id": "Unit Dasar Penyimpanan Data SSD." },
  { "en": "Apa Itu SLC (Single-Level Cell)?", "id": "Satu Bit Per Sel (Cepat, Mahal)." },
  { "en": "Apa Itu MLC (Multi-Level Cell)?", "id": "Dua Bit Per Sel." },
  { "en": "Apa Itu TLC (Triple-Level Cell)?", "id": "Tiga Bit Per Sel (Umum, Murah)." },
  { "en": "Apa Itu QLC (Quad-Level Cell)?", "id": "Empat Bit Per Sel (Paling Murah)." },
  { "en": "Apa Itu DRAM Cache (SSD)?", "id": "Memori Cepat (Buffer) Di SSD." },
  { "en": "Apa Itu HMB (Host Memory Buffer) SSD?", "id": "SSD Meminjam RAM Komputer (Tanpa DRAM)." },
  { "en": "Apa Itu TRIM (Command)?", "id": "Perintah Sistem Operasi (Membersihkan Blok SSD)." },
  { "en": "Apa Itu Garbage Collection (SSD)?", "id": "Proses Latar Belakang Pembersihan SSD." },
  { "en": "Apa Itu Thermal Throttling (CPU/SSD)?", "id": "Menurunkan Kecepatan Karena Terlalu Panas." },
  { "en": "Apa Itu Pendingin Pasif (Passive Cooling)?", "id": "Pendinginan Tanpa Kipas (Hanya Heatsink)." },
  { "en": "Apa Itu Pendingin Aktif (Active Cooling)?", "id": "Pendinginan Menggunakan Kipas Atau Cairan." },
  { "en": "Apa Itu Pendingin Cairan (Water Cooling)?", "id": "Pendinginan Menggunakan Sirkulasi Cairan." },
  { "en": "Apa Itu Radiator (Pendingin Cairan)?", "id": "Komponen Pembuang Panas Cairan Ke Udara." },
  { "en": "Apa Itu Blok Air (Water Block)?", "id": "Komponen Penempel Di CPU (Pendingin Cairan)." },
  { "en": "Apa Itu Pompa (Pump) Pendingin Cairan?", "id": "Alat Pemompa Sirkulasi Cairan Pendingin." },
  { "en": "Apa Itu Heat Pipe (Pipa Panas)?", "id": "Pipa Tembaga Pemindah Panas Efisien." },
  { "en": "Bagaimana Heat Pipe (Pipa Panas) Bekerja?", "id": "Menggunakan Perubahan Fasa Cairan Internal." },
  { "en": "Apa Itu Laptop?", "id": "Komputer Portabel Lipat." },
  { "en": "Apa Itu Motherboard (Mobo)?", "id": "Papan Sirkuit Utama Komputer." },
  { "en": "Apa Itu CPU (Central Processing Unit)?", "id": "Otak Pemroses Utama Komputer." },
  { "en": "Apa Itu GPU (Graphics Processing Unit)?", "id": "Unit Pemroses Khusus Grafis." },
  { "en": "Apa Itu GPU (Graphics Processing Unit) Terintegrasi?", "id": "GPU Yang Menjadi Satu Dengan CPU." },
  { "en": "Apa Itu GPU (Graphics Processing Unit) Diskrit?", "id": "Kartu Grafis Terpisah (Performa Tinggi)." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Kerja Utama Komputer (Volatile)." },
  { "en": "Apa Itu PSU (Power Supply Unit)?", "id": "Catu Daya Komputer (Pengubah AC Ke DC)." },
  { "en": "Apa Itu Casing (Komputer)?", "id": "Kotak Pelindung Komponen Komputer." },
  { "en": "Apa Itu ATX (Advanced Technology eXtended)?", "id": "Standar Ukuran Motherboard PSU." },
  { "en": "Apa Itu Micro-ATX (mATX)?", "id": "Standar Ukuran Motherboard (Lebih Kecil)." },
  { "en": "Apa Itu Mini-ITX?", "id": "Standar Ukuran Motherboard (Sangat Kecil)." },
  { "en": "Apa Itu Chipset (Motherboard)?", "id": "Pengatur Komunikasi Antar Komponen." },
  { "en": "Apa Itu BIOS (Basic Input Output System)?", "id": "Firmware Motherboard Saat Awal Menyala." },
  { "en": "Apa Itu UEFI (Unified Extensible Firmware Interface)?", "id": "Pengganti BIOS Modern (Antarmuka Grafis)." },
  { "en": "Apa Itu CMOS (Baterai)?", "id": "Baterai Koin Penyimpan Pengaturan BIOS." },
  { "en": "Apa Itu POST (Power-On Self-Test)?", "id": "Proses Pengecekan Hardware Saat Menyala." },
  { "en": "Apa Itu Kode Beep (Beep Code)?", "id": "Sinyal Suara Indikator Error POST." },
  { "en": "Apa Itu Overclocking (CPU/GPU)?", "id": "Menjalankan Komponen Di Atas Kecepatan Standar." },
  { "en": "Apa Itu Undervolting?", "id": "Menurunkan Tegangan Operasi (Mengurangi Panas)." },
  { "en": "Apa Itu Bus Sistem (System Bus)?", "id": "Jalur Komunikasi Utama Motherboard." },
  { "en": "Apa Itu FSB (Front Side Bus)?", "id": "Bus Komunikasi CPU Ke Chipset (Lama)." },
  { "en": "Apa Itu PCIe (Peripheral Component Interconnect Express)?", "id": "Slot Ekspansi Kecepatan Tinggi (GPU)." },
  { "en": "Apa Itu Slot PCI (Peripheral Component Interconnect)?", "id": "Slot Ekspansi Standar (Lama)." },
  { "en": "Apa Itu Slot RAM (Random Access Memory) DIMM?", "id": "Slot Pemasangan Memori RAM (PC)." },
  { "en": "Apa Itu Slot RAM (Random Access Memory) SO-DIMM?", "id": "Slot Pemasangan Memori RAM (Laptop)." },
  { "en": "Apa Itu DDR (Double Data Rate) RAM?", "id": "Standar Teknologi Memori RAM." },
  { "en": "Apa Itu DDR4 (Double Data Rate 4)?", "id": "Generasi Keempat Standar Memori DDR." },
  { "en": "Apa Itu DDR5 (Double Data Rate 5)?", "id": "Generasi Kelima Standar Memori DDR." },
  { "en": "Apa Itu XMP (Extreme Memory Profile)?", "id": "Profil Overclocking RAM Otomatis (Intel)." },
  { "en": "Apa Itu EXPO (Extended Profiles for Overclocking)?", "id": "Profil Overclocking RAM Otomatis (AMD)." },
  { "en": "Apa Itu Latency (Latensi) RAM?", "id": "Waktu Tunda Akses Memori (CAS Latency)." },
  { "en": "Apakah Latency (Latensi) RAM Lebih Rendah Lebih Baik?", "id": "Ya, Latensi Rendah Lebih Cepat." },
  { "en": "Apa Itu Dual Channel (RAM)?", "id": "Memasang Dua Keping RAM (Bandwidth Ganda)." },
  { "en": "Apa Itu Quad Channel (RAM)?", "id": "Memasang Empat Keping RAM (Platform High-End)." },
  { "en": "Apa Itu ECC (Error-Correcting Code) RAM?", "id": "RAM Yang Dapat Mendeteksi Memperbaiki Error." },
  { "en": "Di Mana ECC (Error-Correcting Code) RAM Digunakan?", "id": "Server, Workstation (Kestabilan Kritis)." },
  { "en": "Apa Itu Heatsink (Pendingin) RAM?", "id": "Logam Penyebar Panas Di Modul RAM." }, 
  { "en": "Apa Itu Port USB (Universal Serial Bus)?", "id": "Konektor Standar Komunikasi Data Dan Daya." },
  { "en": "Apa Itu USB (Universal Serial Bus) Tipe A?", "id": "Konektor USB Persegi Panjang Standar." },
  { "en": "Apa Itu USB (Universal Serial Bus) Tipe B?", "id": "Konektor USB Kotak (Printer, Arduino Uno)." },
  { "en": "Apa Itu USB (Universal Serial Bus) Tipe C?", "id": "Konektor USB Oval Reversibel (Baru)." },
  { "en": "Apa Itu USB (Universal Serial Bus) Mini?", "id": "Konektor USB Versi Kecil (Lama)." },
  { "en": "Apa Itu USB (Universal Serial Bus) Mikro?", "id": "Konektor USB (Umum Di HP Lama)." },
  { "en": "Apa Itu USB (Universal Serial Bus) 2.0?", "id": "Standar Kecepatan USB (480 Mbps)." },
  { "en": "Apa Itu USB (Universal Serial Bus) 3.0?", "id": "Standar Kecepatan USB (5 Gbps, Biru)." },
  { "en": "Apa Itu USB (Universal Serial Bus) OTG (On-The-Go)?", "id": "Fitur USB (HP Membaca Flashdisk)." },
  { "en": "Apa Itu USB (Universal Serial Bus) Power Delivery (PD)?", "id": "Standar Pengisian Daya Cepat USB-C." },
  { "en": "Apa Itu Thunderbolt (Antarmuka)?", "id": "Antarmuka Kecepatan Sangat Tinggi (Intel)." },
  { "en": "Apa Itu Audio Jack 3.5mm?", "id": "Konektor Standar Audio (Headphone)." },
  { "en": "Apa Itu Bluetooth?", "id": "Standar Komunikasi Nirkabel Jarak Pendek." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Standar Jaringan Nirkabel Lokal (WLAN)." },
  { "en": "Apa Itu Standar Wi-Fi 802.11n?", "id": "Standar Wi-Fi (Generasi Wi-Fi 4)." },
  { "en": "Apa Itu Standar Wi-Fi 802.11ac?", "id": "Standar Wi-Fi (Generasi Wi-Fi 5)." },
  { "en": "Apa Itu Standar Wi-Fi 802.11ax?", "id": "Standar Wi-Fi (Generasi Wi-Fi 6)." },
  { "en": "Apa Itu Frekuensi Wi-Fi 2.4 GHz?", "id": "Pita Frekuensi Wi-Fi (Jangkauan Luas)." },
  { "en": "Apa Itu Frekuensi Wi-Fi 5 GHz?", "id": "Pita Frekuensi Wi-Fi (Kecepatan Tinggi)." },
  { "en": "Apa Itu SSID (Service Set Identifier)?", "id": "Nama Jaringan Wi-Fi Anda." },
  { "en": "Apa Itu Enkripsi WPA2 (Wi-Fi Protected Access 2)?", "id": "Standar Keamanan Wi-Fi (Umum)." },
  { "en": "Apa Itu Enkripsi WPA3 (Wi-Fi Protected Access 3)?", "id": "Standar Keamanan Wi-Fi (Terbaru)." },
  { "en": "Apa Itu WPS (Wi-Fi Protected Setup)?", "id": "Metode Koneksi Wi-Fi Mudah (Tombol)." },
  { "en": "Apa Itu Router Nirkabel (Wireless Router)?", "id": "Perangkat Pemancar Sinyal Wi-Fi." },
  { "en": "Apa Itu Repeater Wi-Fi (Pengulang)?", "id": "Perangkat Perluasan Jangkauan Sinyal Wi-Fi." },
  { "en": "Apa Itu Sistem Mesh Wi-Fi?", "id": "Sistem Wi-Fi Banyak Titik (Cakupan Luas)." },
  { "en": "Apa Itu Jaringan Seluler 4G LTE?", "id": "Generasi Keempat Jaringan Seluler." },
  { "en": "Apa Itu Jaringan Seluler 5G?", "id": "Generasi Kelima Jaringan Seluler." },
  { "en": "Apa Itu Kartu SIM (Subscriber Identity Module)?", "id": "Kartu Identitas Pelanggan Seluler." },
  { "en": "Apa Itu IMEI (International Mobile Equipment Identity)?", "id": "Nomor Identitas Unik Perangkat Seluler." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem Penentuan Posisi Global (Satelit)." },
  { "en": "Bagaimana GPS (Global Positioning System) Bekerja?", "id": "Menerima Sinyal Dari Beberapa Satelit." },
  { "en": "Apa Itu Trilaterasi (GPS)?", "id": "Metode Penentuan Lokasi GPS." },
  { "en": "Apa Itu GLONASS?", "id": "Sistem Navigasi Satelit Milik Rusia." },
  { "en": "Apa Itu Galileo (Navigasi)?", "id": "Sistem Navigasi Satelit Milik Eropa." },
  { "en": "Apa Itu BeiDou (Navigasi)?", "id": "Sistem Navigasi Satelit Milik Tiongkok." },
  { "en": "Apa Itu GNSS (Global Navigation Satellite System)?", "id": "Istilah Umum Sistem Navigasi Satelit." },
  { "en": "Apa Itu A-GPS (Assisted GPS)?", "id": "GPS Dibantu Data Jaringan (Cepat)." },
  { "en": "Apa Itu Sensor Sidik Jari (Fingerprint)?", "id": "Sensor Biometrik Pembaca Sidik Jari." },
  { "en": "Apa Itu Sensor Sidik Jari Kapasitif?", "id": "Sensor Sidik Jari Paling Umum (HP)." },
  { "en": "Apa Itu Sensor Sidik Jari Optik?", "id": "Sensor Sidik Jari (Di Bawah Layar)." },
  { "en": "Apa Itu Sensor Sidik Jari Ultrasonik?", "id": "Sensor Sidik Jari (Suara 3D)." },
  { "en": "Apa Itu Pengenalan Wajah (Face Recognition)?", "id": "Biometrik Pengenal Struktur Wajah." },
  { "en": "Apa Itu Pemindai Iris (Iris Scanner)?", "id": "Biometrik Pemindai Pola Iris Mata." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Komunikasi Nirkabel Jarak Sangat Dekat." },
  { "en": "Contoh Penggunaan NFC (Near Field Communication)?", "id": "Pembayaran Nirkontak, Cek Saldo E-Toll." },
  { "en": "Apa Itu Pembayaran Nirkontak (Contactless)?", "id": "Pembayaran Tanpa Sentuh (Tap)." },
  { "en": "Apa Itu Kode QR (Quick Response)?", "id": "Kode Matriks Dua Dimensi (Kamera)." },
  { "en": "Apa Itu Barcode (Kode Batang)?", "id": "Kode Garis Satu Dimensi (Produk)." },
  { "en": "Apa Itu Layar LCD (Liquid Crystal Display)?", "id": "Layar Panel Kristal Cair." },
  { "en": "Bagaimana LCD (Liquid Crystal Display) Bekerja?", "id": "Kristal Cair Memblokir Cahaya Latar." },
  { "en": "Apa Itu Lampu Latar (Backlight) LCD?", "id": "Sumber Cahaya Di Belakang Panel LCD." },
  { "en": "Apa Itu Layar LED (Light Emitting Diode) (TV)?", "id": "Layar LCD Dengan Lampu Latar LED." },
  { "en": "Apa Itu Layar OLED (Organic Light Emitting Diode)?", "id": "Layar Dengan Piksel Pemancar Cahaya." },
  { "en": "Apa Kelebihan Layar OLED?", "id": "Hitam Sempurna, Kontras Tinggi, Tipis." },
  { "en": "Apa Itu AMOLED (Active Matrix OLED)?", "id": "Tipe Layar OLED (Samsung)." },
  { "en": "Apa Itu Layar Micro-LED?", "id": "Teknologi Layar Masa Depan (Gabungan)." },
  { "en": "Apa Itu Layar E-Ink (Tinta Elektronik)?", "id": "Layar Tampilan (E-Reader, Hemat Daya)." },
  { "en": "Apa Kelebihan Layar E-Ink?", "id": "Sangat Hemat Daya, Terbaca Di Matahari." },
  { "en": "Apa Itu Layar Sentuh (Touchscreen)?", "id": "Layar Yang Berfungsi Sebagai Input Sentuh." },
  { "en": "Apa Itu Layar Sentuh Resistif?", "id": "Layar Sentuh Berbasis Tekanan (Lama)." },
  { "en": "Apa Itu Layar Sentuh Kapasitif?", "id": "Layar Sentuh Berbasis Listrik Tubuh (HP)." },
  { "en": "Apa Itu Multi-Sentuh (Multi-Touch)?", "id": "Layar Sentuh Mendeteksi Banyak Jari." },
  { "en": "Apa Itu Haptic Feedback (Umpan Balik)?", "id": "Getaran Simulasi Sentuhan (HP, Stik)." },
  { "en": "Apa Itu Aktuator Haptic Linear?", "id": "Motor Getar Presisi Tinggi (iPhone)." },
  { "en": "Apa Itu Piksel (Pixel)?", "id": "Titik Terkecil Pembentuk Gambar Digital." },
  { "en": "Apa Itu Sub-Piksel (Sub-Pixel)?", "id": "Komponen Warna Piksel (Merah, Hijau, Biru)." },
  { "en": "Apa Itu Resolusi Layar?", "id": "Jumlah Total Piksel (Lebar x Tinggi)." },
  { "en": "Apa Itu Resolusi HD (High Definition) (720p)?", "id": "Resolusi 1280 x 720 Piksel." },
  { "en": "Apa Itu Resolusi FHD (Full HD) (1080p)?", "id": "Resolusi 1920 x 1080 Piksel." },
  { "en": "Apa Itu Resolusi QHD (Quad HD) (1440p)?", "id": "Resolusi 2560 x 1440 Piksel." },
  { "en": "Apa Itu Resolusi 4K (Ultra HD)?", "id": "Resolusi 3840 x 2160 Piksel." },
  { "en": "Apa Itu Rasio Aspek (Aspect Ratio)?", "id": "Perbandingan Lebar Dan Tinggi Layar." },
  { "en": "Contoh Rasio Aspek (Aspect Ratio)?", "id": "16:9 (TV), 4:3 (Layar Lama)." },
  { "en": "Apa Itu PPI (Pixels Per Inch)?", "id": "Kepadatan Piksel Layar (Ketajaman)." },
  { "en": "Apa Itu Refresh Rate (Layar)?", "id": "Seberapa Cepat Gambar Diperbarui (Hertz)." },
  { "en": "Apa Itu Refresh Rate 60 Hz?", "id": "Layar Memperbarui Gambar 60 Kali Detik." },
  { "en": "Apa Itu Refresh Rate 120 Hz?", "id": "Layar Memperbarui Gambar 120 Kali Detik." },
  { "en": "Apa Itu Response Time (Waktu Respon) Layar?", "id": "Kecepatan Piksel Berubah Warna (Milidetik)." },
  { "en": "Apa Itu Motion Blur (Layar)?", "id": "Efek Buram Gerakan (Waktu Respon Lambat)." },
  { "en": "Apa Itu Panel Layar TN (Twisted Nematic)?", "id": "Panel LCD (Respon Cepat, Warna Buruk)." },
  { "en": "Apa Itu Panel Layar VA (Vertical Alignment)?", "id": "Panel LCD (Kontras Tinggi, Respon Lambat)." },
  { "en": "Apa Itu Panel Layar IPS (In-Plane Switching)?", "id": "Panel LCD (Warna Akurat, Sudut Pandang Luas)." },
  { "en": "Apa Itu Sudut Pandang (Viewing Angle)?", "id": "Sudut Maksimal Layar Terlihat Jelas." },
  { "en": "Apa Itu HDR (High Dynamic Range)?", "id": "Rentang Kecerahan Dan Warna Lebih Luas." },
  { "en": "Apa Itu Color Gamut (Cakupan Warna)?", "id": "Rentang Warna Yang Dapat Ditampilkan Layar." },
  { "en": "Apa Itu sRGB (Standar RGB)?", "id": "Standar Cakupan Warna Umum (Web, PC)." },
  { "en": "Apa Itu Adobe RGB?", "id": "Cakupan Warna Lebih Luas (Fotografi Profesional)." },
  { "en": "Apa Itu DCI-P3?", "id": "Cakupan Warna Luas (Industri Film Digital)." },
  { "en": "Apa Itu NTSC (National Television System Committee)?", "id": "Standar Televisi Analog Amerika (Lama)." },
  { "en": "Apa Itu PAL (Phase Alternating Line)?", "id": "Standar Televisi Analog Eropa (Lama)." },
  { "en": "Apa Itu SECAM (Séquentiel Couleur à Mémoire)?", "id": "Standar Televisi Analog Prancis Rusia (Lama)." },
  { "en": "Apa Itu Sinyal Video Komposit (Composite)?", "id": "Sinyal Video Analog Satu Kabel (RCA Kuning)." },
  { "en": "Apa Itu Sinyal Video S-Video?", "id": "Sinyal Video Analog Terpisah (Luma, Chroma)." },
  { "en": "Apa Itu Sinyal Video Komponen (Component)?", "id": "Sinyal Video Analog Tiga Kabel (YPbPr)." },
  { "en": "Apa Itu Sinyal RGB (Red Green Blue)?", "id": "Sinyal Video Analog (Kualitas Terbaik)." },
  { "en": "Apa Itu HDMI (High-Definition Multimedia Interface)?", "id": "Standar Audio Video Digital Modern." },
  { "en": "Apa Itu HDCP (High-bandwidth Digital Content Protection)?", "id": "Proteksi Salinan Konten Digital (HDMI)." },
  { "en": "Apa Itu HDMI (High-Definition Multimedia Interface) ARC?", "id": "Audio Return Channel (Audio TV Ke Soundbar)." },
  { "en": "Apa Itu HDMI (High-Definition Multimedia Interface) eARC?", "id": "Versi ARC Yang Ditingkatkan (Audio HD)." },
  { "en": "Apa Itu HDMI (High-Definition Multimedia Interface) CEC?", "id": "Kontrol Elektronik Konsumen (Satu Remote)." },
  { "en": "Apa Itu DisplayPort?", "id": "Standar Video Digital (Umum Di PC)." },
  { "en": "Apa Itu DVI (Digital Visual Interface)?", "id": "Standar Video Digital/Analog (Sebelum HDMI)." },
  { "en": "Apa Itu VGA (Video Graphics Array)?", "id": "Standar Video Analog 15-Pin (Lama)." },
  { "en": "Apa Itu Kamera Digital?", "id": "Kamera Perekam Gambar (Sensor Elektronik)." },
  { "en": "Apa Itu Sensor Gambar (Image Sensor)?", "id": "Komponen Pengubah Cahaya Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Sensor CCD (Charge-Coupled Device)?", "id": "Tipe Sensor Gambar (Kualitas Tinggi, Lama)." },
  { "en": "Apa Itu Sensor CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Tipe Sensor Gambar (Paling Umum Saat Ini)." },
  { "en": "Apa Itu Megapiksel (Megapixel)?", "id": "Satuan Resolusi Kamera (Satu Juta Piksel)." },
  { "en": "Apa Itu Lensa (Kamera)?", "id": "Optik Kaca Pembias Cahaya Ke Sensor." },
  { "en": "Apa Itu Aperture (Bukaan) Lensa?", "id": "Bukaan Lensa Pengontrol Jumlah Cahaya." },
  { "en": "Apa Itu F-Stop (F-Number)?", "id": "Ukuran Bukaan Aperture (f/1.8)." },
  { "en": "Apa Arti F-Stop (F-Number) Kecil (f/1.8)?", "id": "Bukaan Lensa Besar (Banyak Cahaya)." },
  { "en": "Apa Arti F-Stop (F-Number) Besar (f/16)?", "id": "Bukaan Lensa Kecil (Sedikit Cahaya)." },
  { "en": "Apa Itu Depth of Field (DOF)?", "id": "Rentang Area Fokus Yang Tajam." },
  { "en": "Apa Efek Aperture Besar (f/1.8) Ke DOF?", "id": "DOF Sempit (Latar Belakang Blur/Bokeh)." },
  { "en": "Apa Efek Aperture Kecil (f/16) Ke DOF?", "id": "DOF Luas (Semua Area Tajam)." },
  { "en": "Apa Itu Bokeh?", "id": "Kualitas Estetika Area Blur Latar Belakang." },
  { "en": "Apa Itu Shutter Speed (Kecepatan Rana)?", "id": "Lama Waktu Sensor Merekam Cahaya." },
  { "en": "Apa Efek Shutter Speed Cepat (1/1000s)?", "id": "Membekukan Gerakan (Foto Tajam)." },
  { "en": "Apa Efek Shutter Speed Lambat (1s)?", "id": "Efek Gerakan Blur (Air Terjun Halus)." },
  { "en": "Apa Itu ISO (Kamera)?", "id": "Tingkat Sensitivitas Sensor Terhadap Cahaya." },
  { "en": "Apa Efek ISO (Kamera) Tinggi?", "id": "Foto Terang Di Gelap (Tapi Noise)." },
  { "en": "Apa Efek ISO (Kamera) Rendah?", "id": "Foto Paling Bersih (Butuh Banyak Cahaya)." },
  { "en": "Apa Itu Segitiga Eksposur (Exposure Triangle)?", "id": "Hubungan Antara Aperture, Shutter, ISO." },
  { "en": "Apa Itu White Balance (WB)?", "id": "Penyesuaian Warna Putih (Koreksi Warna)." },
  { "en": "Apa Itu Format Gambar JPEG (Joint Photographic Experts Group)?", "id": "Format Gambar Terkompresi (Umum)." },
  { "en": "Apa Itu Format Gambar RAW?", "id": "Format Gambar Mentah (Data Sensor Penuh)." },
  { "en": "Apa Kelebihan Format RAW?", "id": "Fleksibilitas Edit Sangat Tinggi." },
  { "en": "Apa Itu Format Gambar PNG (Portable Network Graphics)?", "id": "Format Gambar (Mendukung Transparansi)." },
  { "en": "Apa Itu Format Gambar GIF (Graphics Interchange Format)?", "id": "Format Gambar (Animasi Sederhana)." },
  { "en": "Apa Itu Format Gambar BMP (Bitmap)?", "id": "Format Gambar Tidak Terkompresi (Windows)." },
  { "en": "Apa Itu Metadata EXIF (Exchangeable Image File)?", "id": "Informasi Data Di Dalam File Foto." },
  { "en": "Apa Itu Lensa Prime (Tetap)?", "id": "Lensa Dengan Satu Focal Length (Tidak Zoom)." },
  { "en": "Apa Itu Lensa Zoom?", "id": "Lensa Dengan Focal Length Variabel." },
  { "en": "Apa Itu Focal Length (Panjang Fokus)?", "id": "Jarak Lensa Ke Sensor (Mengatur Sudut Pandang)." },
  { "en": "Apa Itu Lensa Wide-Angle (Sudut Lebar)?", "id": "Lensa Focal Length Pendek (Cakupan Luas)." },
  { "en": "Apa Itu Lensa Telephoto (Tele)?", "id": "Lensa Focal Length Panjang (Zoom Jauh)." },
  { "en": "Apa Itu Lensa Makro (Macro)?", "id": "Lensa Khusus Foto Jarak Sangat Dekat." },
  { "en": "Apa Itu Lensa Fish-Eye (Mata Ikan)?", "id": "Lensa Sudut Sangat Lebar (Efek Cembung)." },
  { "en": "Apa Itu Kamera DSLR (Digital Single-Lens Reflex)?", "id": "Kamera Digital Dengan Cermin Refleks." },
  { "en": "Apa Itu Viewfinder (Jendela Bidik) Optik?", "id": "Jendela Bidik Melihat Langsung Lewat Lensa." },
  { "en": "Apa Itu Kamera Mirrorless?", "id": "Kamera Digital Tanpa Cermin Refleks." },
  { "en": "Apa Itu Viewfinder (Jendela Bidik) Elektronik (EVF)?", "id": "Layar Kecil Di Dalam Jendela Bidik." },
  { "en": "Apa Itu Kamera Saku (Point-and-Shoot)?", "id": "Kamera Kompak Sederhana." },
  { "en": "Apa Itu Kamera Aksi (Action Camera)?", "id": "Kamera Kecil Tangguh (GoPro)." },
  { "en": "Apa Itu Stabilisasi Gambar (Image Stabilization)?", "id": "Fitur Pengurang Guncangan (Blur)." },
  { "en": "Apa Itu OIS (Optical Image Stabilization)?", "id": "Stabilisasi Optik (Elemen Lensa Bergerak)." },
  { "en": "Apa Itu EIS (Electronic Image Stabilization)?", "id": "Stabilisasi Elektronik (Sensor/Software)." },
  { "en": "Apa Itu IBIS (In-Body Image Stabilization)?", "id": "Stabilisasi Di Dalam Bodi (Sensor Bergerak)." },
  { "en": "Apa Itu Autofokus (Autofocus)?", "id": "Fitur Penyesuaian Fokus Otomatis." },
  { "en": "Apa Itu Autofokus Deteksi Fasa (Phase Detect)?", "id": "Metode Autofokus Cepat (DSLR, Mirrorless)." },
  { "en": "Apa Itu Autofokus Deteksi Kontras (Contrast Detect)?", "id": "Metode Autofokus (Mencari Kontras Tertinggi)." },
  { "en": "Apa Itu Audio?", "id": "Getaran Suara Yang Terdengar." },
  { "en": "Apa Itu Frekuensi (Audio)?", "id": "Menentukan Nada Suara (Tinggi/Rendah)." },
  { "en": "Apa Itu Amplitudo (Audio)?", "id": "Menentukan Volume Suara (Keras/Pelan)." },
  { "en": "Apa Rentang Pendengaran Manusia?", "id": "20 Hertz Hingga 20 KiloHertz." },
  { "en": "Apa Itu Suara Infrasonik?", "id": "Suara Dibawah 20 Hertz." },
  { "en": "Apa Itu Suara Ultrasonik?", "id": "Suara Diatas 20 KiloHertz." },
  { "en": "Apa Itu Desibel (dB) Audio?", "id": "Satuan Logaritmik Pengukuran Volume." },
  { "en": "Apa Itu Audio Mono (Monophonic)?", "id": "Suara Satu Saluran (Channel)." },
  { "en": "Apa Itu Audio Stereo (Stereophonic)?", "id": "Suara Dua Saluran (Kiri Kanan)." },
  { "en": "Apa Itu Audio Surround Sound?", "id": "Suara Multi-Saluran (Depan, Belakang)." },
  { "en": "Contoh Audio Surround Sound?", "id": "5.1 (Lima Speaker, Satu Subwoofer)." },
  { "en": "Apa Itu Mikrofon (Microphone)?", "id": "Transduser Pengubah Suara Menjadi Listrik." },
  { "en": "Apa Itu Mikrofon Dinamis (Dynamic)?", "id": "Mikrofon Berbasis Kumparan Bergerak (Tangguh)." },
  { "en": "Di Mana Mikrofon Dinamis Digunakan?", "id": "Panggung Live, Vokal." },
  { "en": "Apa Itu Mikrofon Kondenser (Condenser)?", "id": "Mikrofon Berbasis Kapasitor (Sensitif)." },
  { "en": "Di Mana Mikrofon Kondenser Digunakan?", "id": "Studio Rekaman, Podcast." },
  { "en": "Apakah Mikrofon Kondenser Butuh Daya?", "id": "Ya, Membutuhkan Phantom Power." },
  { "en": "Apa Itu Phantom Power (+48V)?", "id": "Daya Listrik DC Untuk Mikrofon Kondenser." },
  { "en": "Apa Itu Mikrofon Lavalier (Clip-On)?", "id": "Mikrofon Kecil Yang Dijepitkan Di Baju." },
  { "en": "Apa Itu Mikrofon Shotgun?", "id": "Mikrofon Sangat Arah (Merekam Jauh)." },
  { "en": "Apa Itu Pola Polar (Mikrofon)?", "id": "Sensitivitas Arah Tangkapan Suara Mikrofon." },
  { "en": "Apa Itu Pola Polar Kardioid (Cardioid)?", "id": "Pola Menangkap Dari Depan (Bentuk Hati)." },
  { "en": "Apa Itu Pola Polar Omnidirectional?", "id": "Pola Menangkap Dari Segala Arah." },
  { "en": "Apa Itu Pola Polar Bidirectional (Angka 8)?", "id": "Pola Menangkap Dari Depan Belakang." },
  { "en": "Apa Itu Pop Filter (Mikrofon)?", "id": "Filter Pencegah Suara Letupan (Plosive)." },
  { "en": "Apa Itu Windshield (Mikrofon)?", "id": "Busa Pelindung Suara Angin." },
  { "en": "Apa Itu Shock Mount (Mikrofon)?", "id": "Dudukan Elastis Pencegah Getaran." },
  { "en": "Apa Itu Antarmuka Audio (Audio Interface)?", "id": "Perangkat Penghubung Mikrofon Ke Komputer." },
  { "en": "Apa Itu Preamp (Preamplifier) Mikrofon?", "id": "Penguat Sinyal Mikrofon Sangat Lemah." },
  { "en": "Apa Itu Sinyal Level Mikrofon (Mic Level)?", "id": "Sinyal Output Mikrofon (Sangat Lemah)." },
  { "en": "Apa Itu Sinyal Level Saluran (Line Level)?", "id": "Sinyal Audio Standar (Lebih Kuat)." },
  { "en": "Apa Itu Sinyal Level Instrumen (Instrument Level)?", "id": "Sinyal Output Gitar Listrik (Lemah)." },
  { "en": "Apa Itu DI Box (Direct Input)?", "id": "Konverter Sinyal Instrumen Ke Mic Level." },
  { "en": "Apa Itu Speaker (Loudspeaker)?", "id": "Transduser Pengubah Listrik Menjadi Suara." },
  { "en": "Apa Komponen Utama Speaker Dinamis?", "id": "Magnet, Kumparan Suara, Dan Kerucut (Cone)." },
  { "en": "Apa Itu Kerucut (Cone) Speaker?", "id": "Membran Pendorong Udara (Menghasilkan Suara)." },
  { "en": "Apa Itu Kumparan Suara (Voice Coil) Speaker?", "id": "Kumparan Bergerak Dalam Medan Magnet." },
  { "en": "Apa Itu Tweeter (Speaker)?", "id": "Speaker Khusus Frekuensi Tinggi (Treble)." },
  { "en": "Apa Itu Mid-range (Speaker)?", "id": "Speaker Khusus Frekuensi Tengah (Vokal)." },
  { "en": "Apa Itu Woofer (Speaker)?", "id": "Speaker Khusus Frekuensi Rendah (Bass)." },
  { "en": "Apa Itu Subwoofer (Speaker)?", "id": "Speaker Khusus Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Crossover Audio?", "id": "Rangkaian Pemisah Frekuensi Untuk Speaker." },
  { "en": "Apa Itu Crossover Pasif?", "id": "Crossover Menggunakan Resistor, Kapasitor, Induktor." },
  { "en": "Apa Itu Crossover Aktif?", "id": "Crossover Elektronik (Sebelum Amplifier)." },
  { "en": "Apa Itu Bi-Amping (Audio)?", "id": "Menggunakan Amplifier Terpisah (Woofer Tweeter)." },
  { "en": "Apa Itu Impedansi (Impedance) Speaker?", "id": "Hambatan Total Speaker (Ohm)." },
  { "en": "Berapa Impedansi (Impedance) Speaker Umum?", "id": "Empat, Enam, Atau Delapan Ohm." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Rentang Frekuensi Yang Dapat Diproduksi." },
  { "en": "Apa Itu Sensitivitas (Sensitivity) Speaker?", "id": "Seberapa Keras Speaker (dB Per Watt)." },
  { "en": "Apa Itu Audio Digital?", "id": "Representasi Digital Sinyal Suara Analog." },
  { "en": "Apa Itu Sample Rate (Audio Digital)?", "id": "Seberapa Sering Sinyal Analog Diukur." },
  { "en": "Berapa Sample Rate (Audio) CD?", "id": "44.1 KiloHertz (44100 Kali Detik)." },
  { "en": "Apa Itu Bit Depth (Audio Digital)?", "id": "Resolusi Amplitudo Setiap Sampel." },
  { "en": "Berapa Bit Depth (Audio) CD?", "id": "Enam Belas Bit (16-Bit)." },
  { "en": "Apa Itu Bitrate (Audio)?", "id": "Jumlah Data Audio Per Detik (Kbps)." },
  { "en": "Apa Itu Kompresi Audio Lossy?", "id": "Kompresi Membuang Data (Ukuran Kecil)." },
  { "en": "Contoh Kompresi Audio Lossy?", "id": "MP3, AAC." },
  { "en": "Apa Itu Kompresi Audio Lossless?", "id": "Kompresi Tanpa Membuang Data (Bisa Kembali)." },
  { "en": "Contoh Kompresi Audio Lossless?", "id": "FLAC, ALAC." },
  { "en": "Apa Itu Format Audio WAV (Waveform)?", "id": "Format Audio Digital Tanpa Kompresi." },
  { "en": "Apa Itu Format Audio MP3 (MPEG Layer 3)?", "id": "Format Audio Lossy Paling Populer." },
  { "en": "Apa Itu Format Audio AAC (Advanced Audio Coding)?", "id": "Format Audio Lossy (Lebih Efisien)." },
  { "en": "Apa Itu Format Audio FLAC (Free Lossless Audio Codec)?", "id": "Format Audio Lossless Populer." },
  { "en": "Apa Itu Format Audio ALAC (Apple Lossless Audio Codec)?", "id": "Format Audio Lossless Milik Apple." },
  { "en": "Apa Itu Format Audio Ogg Vorbis?", "id": "Format Audio Lossy Open Source." },
  { "en": "Apa Itu Format Audio DSD (Direct Stream Digital)?", "id": "Format Audio Resolusi Sangat Tinggi." },
  { "en": "Apa Itu DAW (Digital Audio Workstation)?", "id": "Perangkat Lunak Perekaman Produksi Musik." },
  { "en": "Contoh DAW (Digital Audio Workstation)?", "id": "Pro Tools, Logic Pro, Ableton Live." },
  { "en": "Apa Itu VST (Virtual Studio Technology)?", "id": "Plugin Efek Audio Atau Instrumen Digital." },
  { "en": "Apa Itu MIDI (Musical Instrument Digital Interface)?", "id": "Protokol Komunikasi Data Musik." },
  { "en": "Apa Isi Data MIDI (Musical Instrument Digital Interface)?", "id": "Informasi Nada, Volume, Tempo (Bukan Suara)." },
  { "en": "Apa Itu MIDI (Musical Instrument Digital Interface) Controller?", "id": "Perangkat Input MIDI (Keyboard)." },
  { "en": "Apa Itu Konektor MIDI (Musical Instrument Digital Interface)?", "id": "Konektor DIN 5-Pin (Lama)." },
  { "en": "Apa Itu USB (Universal Serial Bus) MIDI?", "id": "MIDI Yang Ditransmisikan Lewat Kabel USB." },
  { "en": "Apa Itu Synthesizer (Penyintesis)?", "id": "Alat Musik Elektronik Penghasil Suara." },
  { "en": "Apa Itu Osilator (Synthesizer)?", "id": "Penghasil Bentuk Gelombang Dasar (Suara)." },
  { "en": "Apa Itu Filter (Synthesizer)?", "id": "Mengubah Karakter Suara (Memotong Frekuensi)." },
  { "en": "Apa Itu Amplifier (Synthesizer)?", "id": "Mengontrol Volume Suara (Amplop ADSR)." },
  { "en": "Apa Itu LFO (Low Frequency Oscillator)?", "id": "Osilator Lambat Untuk Modulasi (Efek)." },
  { "en": "Apa Itu Amplop ADSR?", "id": "Attack, Decay, Sustain, Release." },
  { "en": "Apa Itu Attack (Amplop ADSR)?", "id": "Waktu Awal Suara Mencapai Puncak." },
  { "en": "Apa Itu Decay (Amplop ADSR)?", "id": "Waktu Suara Turun Ke Level Sustain." },
  { "en": "Apa Itu Sustain (Amplop ADSR)?", "id": "Level Volume Saat Tombol Ditahan." },
  { "en": "Apa Itu Release (Amplop ADSR)?", "id": "Waktu Suara Hilang Setelah Tombol Dilepas." },
  { "en": "Apa Itu Arpeggiator?", "id": "Fitur Pemain Akord Menjadi Nada Terpisah." },
  { "en": "Apa Itu Sequencer (Musik)?", "id": "Alat Perekam Pemutar Pola Nada (MIDI)." },
  { "en": "Apa Itu Drum Machine?", "id": "Alat Elektronik Pembuat Pola Drum." },
  { "en": "Apa Itu Vocoder?", "id": "Efek Vokal Robotik (Suara Sintesis)." },
  { "en": "Apa Itu Auto-Tune (Auto-Tune)?", "id": "Perangkat Lunak Koreksi Nada Vokal." },
  { "en": "Apa Itu Efek Reverb (Gema)?", "id": "Efek Simulasi Pantulan Suara Ruangan." },
  { "en": "Apa Itu Efek Delay (Tunda)?", "id": "Efek Pengulangan Suara (Echo)." },
  { "en": "Apa Itu Efek Chorus?", "id": "Efek Penebal Suara (Suara Ganda)." },
  { "en": "Apa Itu Efek Flanger?", "id": "Efek Suara Jet (Geser Fasa)." },
  { "en": "Apa Itu Efek Phaser?", "id": "Efek Suara Berputar (Geser Fasa)." },
  { "en": "Apa Itu Efek Distortion (Distorsi)?", "id": "Efek Suara Pecah (Gitar Rock)." },
  { "en": "Apa Itu Efek Overdrive?", "id": "Efek Distorsi Ringan (Blues)." },
  { "en": "Apa Itu Efek Fuzz?", "id": "Efek Distorsi Ekstrem (Vintage)." },
  { "en": "Apa Itu Pedal Efek (Stompbox)?", "id": "Kotak Efek Gitar (Ditekan Kaki)." },
  { "en": "Apa Itu Amplifier Gitar?", "id": "Penguat Suara Khusus Gitar Listrik." },
  { "en": "Apa Itu Amplifier Gitar Tabung (Tube)?", "id": "Penguat Menggunakan Tabung Vakum." },
  { "en": "Apa Itu Amplifier Gitar Solid-State?", "id": "Penguat Menggunakan Transistor (Lebih Bersih)." },
  { "en": "Apa Itu Amplifier Gitar Modeling?", "id": "Penguat Digital Peniru Suara Amplifier Lain." },
  { "en": "Apa Itu Kabinet (Cabinet) Gitar?", "id": "Kotak Speaker Untuk Amplifier Gitar." },
  { "en": "Apa Itu Head (Kepala) Amplifier?", "id": "Bagian Amplifier (Tanpa Speaker)." },
  { "en": "Apa Itu Combo (Kombo) Amplifier?", "id": "Amplifier Dan Speaker Dalam Satu Unit." },
  { "en": "Apa Itu Pickup (Gitar Listrik)?", "id": "Transduser Magnetik Penangkap Getaran Senar." },
  { "en": "Apa Itu Pickup Single-Coil?", "id": "Pickup Satu Kumparan (Suara Cerah)." },
  { "en": "Apa Itu Pickup Humbucker?", "id": "Pickup Dua Kumparan (Senyap Noise)." },
  { "en": "Apa Itu Pickup Aktif?", "id": "Pickup Dengan Preamp Internal (Butuh Baterai)." },
  { "en": "Apa Itu Pickup Pasif?", "id": "Pickup Standar Tanpa Preamp Internal." },
  { "en": "Apa Itu Gitar Akustik Elektrik?", "id": "Gitar Akustik Dengan Pickup (Piezo)." },
  { "en": "Apa Itu Pickup Piezo?", "id": "Pickup Berbasis Tekanan (Kristal)." },
  { "en": "Apa Itu Feedback (Audio)?", "id": "Suara Mendenging (Mikrofon Menangkap Suara Speaker)." },
  { "en": "Bagaimana Mencegah Feedback (Audio)?", "id": "Menjauhkan Mikrofon Dari Speaker." },
  { "en": "Apa Itu Noise Gate (Gerbang Kebisingan)?", "id": "Mematikan Audio Otomatis Saat Sinyal Diam." },
  { "en": "Apa Itu Kompresor (Compressor) Audio?", "id": "Meratakan Volume (Mengecilkan Yang Keras)." },
  { "en": "Apa Itu Limiter (Audio)?", "id": "Kompresor Ekstrem (Mencegah Clipping)." },
  { "en": "Apa Itu Clipping (Audio)?", "id": "Sinyal Terpotong (Level Terlalu Keras)." },
  { "en": "Apa Itu Headroom (Audio)?", "id": "Jarak Antara Level Sinyal Dan Clipping." },
  { "en": "Apa Itu Normalisasi (Normalization) Audio?", "id": "Mengatur Puncak Amplitudo Ke Level Maksimal." },
  { "en": "Apa Itu Keseimbangan Stereo (Stereo Balance)?", "id": "Volume Relatif Saluran Kiri Dan Kanan." },
  { "en": "Apa Itu Panning (Audio)?", "id": "Mengatur Posisi Suara (Kiri Atau Kanan)." },
  { "en": "Apa Itu Mixer (Audio)?", "id": "Alat Pencampur Berbagai Sumber Suara." },
  { "en": "Apa Itu Fader (Mixer)?", "id": "Tuas Geser Pengatur Volume Saluran." },
  { "en": "Apa Itu Equalizer (EQ) (Mixer)?", "id": "Kontrol Nada (Bass, Mid, Treble)." },
  { "en": "Apa Itu Tombol PFL (Pre-Fader Listen)?", "id": "Mendengar Sinyal Sebelum Fader (Headphone)." },
  { "en": "Apa Itu Tombol Mute (Mixer)?", "id": "Tombol Untuk Membisukan Saluran." },
  { "en": "Apa Itu Tombol Solo (Mixer)?", "id": "Mendengar Satu Saluran Saja." },
  { "en": "Apa Itu Aux Send (Mixer)?", "id": "Keluaran Sinyal Tambahan (Monitor, Efek)." },
  { "en": "Apa Itu Monitor Panggung (Stage Monitor)?", "id": "Speaker Di Panggung (Untuk Musisi)." },
  { "en": "Apa Itu Audio Mobil (Car Audio)?", "id": "Sistem Suara Di Dalam Kendaraan." },
  { "en": "Apa Itu Head Unit (Audio Mobil)?", "id": "Pusat Kontrol Sistem Audio Mobil." },
  { "en": "Apa Itu Amplifier (Audio Mobil)?", "id": "Penguat Daya Tambahan Audio Mobil." },
  { "en": "Apa Itu Kapasitor Bank (Audio Mobil)?", "id": "Penyimpan Energi (Mencegah Lampu Redup)." },
  { "en": "Apa Itu Ground Loop (Audio Mobil)?", "id": "Noise Mendenging Akibat Grounding Buruk." },
  { "en": "Apa Itu Noise Isolator (Audio)?", "id": "Alat Penghilang Noise Ground Loop." },
  { "en": "Apa Itu Headphone (Headphone)?", "id": "Speaker Kecil Di Telinga (Menutup Telinga)." },
  { "en": "Apa Itu Earphone (Earphone)?", "id": "Speaker Kecil (Masuk Lubang Telinga)." },
  { "en": "Apa Itu Headphone Over-Ear?", "id": "Headphone Menutup Seluruh Daun Telinga." },
  { "en": "Apa Itu Headphone On-Ear?", "id": "Headphone Menempel Di Atas Daun Telinga." },
  { "en": "Apa Itu In-Ear Monitor (IEM)?", "id": "Earphone Profesional (Monitor Panggung)." },
  { "en": "Apa Itu Headphone Open-Back?", "id": "Headphone Dengan Bagian Belakang Terbuka." },
  { "en": "Apa Keuntungan Headphone Open-Back?", "id": "Suara Lebih Luas (Soundstage Lebar)." },
  { "en": "Apa Kerugian Headphone Open-Back?", "id": "Suara Bocor, Tidak Meredam Suara Luar." },
  { "en": "Apa Itu Headphone Closed-Back?", "id": "Headphone Dengan Bagian Belakang Tertutup." },
  { "en": "Apa Keuntungan Headphone Closed-Back?", "id": "Isolasi Suara Baik (Suara Tidak Bocor)." },
  { "en": "Apa Itu Driver (Headphone)?", "id": "Komponen Speaker Di Dalam Headphone." },
  { "en": "Apa Itu Driver Dinamis (Dynamic)?", "id": "Tipe Driver Headphone Paling Umum." },
  { "en": "Apa Itu Driver Balanced Armature (BA)?", "id": "Tipe Driver Kecil (Umum Di IEM)." },
  { "en": "Apa Itu Driver Planar Magnetic?", "id": "Tipe Driver (Detail Tinggi, Mahal)." },
  { "en": "Apa Itu Headphone Nirkabel (Wireless)?", "id": "Headphone Menggunakan Koneksi Bluetooth." },
  { "en": "Apa Itu Headphone True Wireless (TWS)?", "id": "Earphone Nirkabel (Tanpa Kabel Sama Sekali)." },
  { "en": "Apa Itu Kodek (Codec) Bluetooth?", "id": "Algoritma Kompresi Audio Bluetooth." },
  { "en": "Contoh Kodek (Codec) Bluetooth?", "id": "SBC, AAC, aptX, LDAC." },
  { "en": "Apa Itu SBC (Subband Coding)?", "id": "Kodek Bluetooth Dasar (Standar Wajib)." },
  { "en": "Apa Itu AAC (Advanced Audio Coding)?", "id": "Kodek Bluetooth (Umum Di Perangkat Apple)." },
  { "en": "Apa Itu aptX (Audio Processing Technology)?", "id": "Kodek Bluetooth Kualitas Tinggi (Qualcomm)." },
  { "en": "Apa Itu LDAC?", "id": "Kodek Bluetooth Resolusi Tinggi (Sony)." },
  { "en": "Apa Itu Active Noise Cancellation (ANC)?", "id": "Fitur Peredam Suara Luar Aktif." },
  { "en": "Bagaimana ANC (Active Noise Cancellation) Bekerja?", "id": "Menciptakan Gelombang Suara Terbalik." },
  { "en": "Apa Itu Mode Transparansi (Transparency Mode)?", "id": "Fitur Mendengar Suara Luar (Mikrofon)." },
  { "en": "Apa Itu Impedansi (Impedance) Headphone?", "id": "Hambatan Listrik Headphone (Ohm)." },
  { "en": "Apa Itu Headphone Impedansi Rendah?", "id": "Mudah Digerakkan (Langsung Ke HP)." },
  { "en": "Apa Itu Headphone Impedansi Tinggi?", "id": "Butuh Amplifier Khusus (Kualitas Studio)." },
  { "en": "Apa Itu Amplifier Headphone (Headphone Amp)?", "id": "Penguat Khusus Untuk Headphone." },
  { "en": "Kapan Perlu Amplifier Headphone?", "id": "Saat Menggunakan Headphone Impedansi Tinggi." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) (Audio)?", "id": "Perangkat Pengubah Audio Digital Ke Analog." },
  { "en": "Kenapa Perlu DAC (Digital-to-Analog Converter) Eksternal?", "id": "Kualitas Suara Lebih Baik Dari Internal." },
  { "en": "Apa Itu DAC/Amp Combo?", "id": "Gabungan DAC Dan Amplifier Headphone." },
  { "en": "Apa Itu Radio?", "id": "Transmisi Sinyal Suara Lewat Gelombang Radio." },
  { "en": "Apa Itu Gelombang Radio AM (Amplitude Modulation)?", "id": "Radio Gelombang Amplitudo (Jarak Jauh)." },
  { "en": "Apa Itu Gelombang Radio FM (Frequency Modulation)?", "id": "Radio Gelombang Frekuensi (Suara Jernih)." },
  { "en": "Apa Itu Pita Frekuensi (Frequency Band)?", "id": "Rentang Frekuensi (Contoh: FM 88-108 MHz)." },
  { "en": "Apa Itu SW (Shortwave) Radio?", "id": "Radio Gelombang Pendek (Jarak Sangat Jauh)." },
  { "en": "Apa Itu MW (Mediumwave) Radio?", "id": "Gelombang Menengah (Pita AM)." },
  { "en": "Apa Itu LW (Longwave) Radio?", "id": "Radio Gelombang Panjang." },
  { "en": "Apa Itu Radio Satelit?", "id": "Radio Siaran Berbasis Satelit (Berlangganan)." },
  { "en": "Apa Itu Radio Digital (DAB)?", "id": "Radio Siaran Berbasis Sinyal Digital." },
  { "en": "Apa Itu Radio Internet?", "id": "Radio Siaran Melalui Jaringan Internet." },
  { "en": "Apa Itu Podcast?", "id": "Siaran Audio Digital (Bisa Diunduh)." },
  { "en": "Apa Itu RDS (Radio Data System)?", "id": "Sistem Data Teks Di Sinyal Radio FM." },
  { "en": "Apa Itu Pemancar (Transmitter)?", "id": "Alat Pemancar Gelombang Radio." },
  { "en": "Apa Itu Penerima (Receiver)?", "id": "Alat Penerima Gelombang Radio." },
  { "en": "Apa Itu Transceiver?", "id": "Gabungan Pemancar Dan Penerima." },
  { "en": "Contoh Transceiver?", "id": "Walkie-Talkie, Radio Amatir." },
  { "en": "Apa Itu Radio Amatir (Ham Radio)?", "id": "Hobi Komunikasi Radio (Perlu Izin)." },
  { "en": "Apa Itu Tanda Panggil (Call Sign)?", "id": "Kode Identifikasi Unik Stasiun Radio." },
  { "en": "Apa Itu Repeater (Radio Amatir)?", "id": "Stasiun Pengulang (Memperluas Jangkauan)." },
  { "en": "Apa Itu Kode Morse?", "id": "Sistem Komunikasi (Titik Dan Garis)." },
  { "en": "Apa Itu CW (Continuous Wave)?", "id": "Sebutan Transmisi Kode Morse." },
  { "en": "Apa Itu Radio SSB (Single Sideband)?", "id": "Mode Transmisi Suara Efisien (Radio HF)." },
  { "en": "Apa Itu Squelch (Radio)?", "id": "Fitur Peredam Desis Saat Hening." },
  { "en": "Apa Itu Antena Dipole?", "id": "Tipe Antena Sederhana (Dua Elemen)." },
  { "en": "Apa Itu Antena Yagi?", "id": "Tipe Antena Arah (Antena TV Lama)." },
  { "en": "Apa Itu Antena Parabola (Dish)?", "id": "Antena Reflektor Mangkuk (Satelit)." },
  { "en": "Apa Itu LNB (Low-Noise Block Downconverter)?", "id": "Komponen Di Fokus Antena Parabola." },
  { "en": "Apa Itu TV Satelit?", "id": "Siaran Televisi Melalui Satelit." },
  { "en": "Apa Itu TV Kabel?", "id": "Siaran Televisi Lewat Kabel Koaksial." },
  { "en": "Apa Itu IPTV (Internet Protocol Television)?", "id": "Siaran Televisi Lewat Jaringan Internet." },
  { "en": "Apa Itu TV Digital Terestrial?", "id": "Siaran TV Digital (Antena UHF Biasa)." },
  { "en": "Apa Itu DVB-T2 (Digital Video Broadcasting)?", "id": "Standar Siaran TV Digital Di Indonesia." },
  { "en": "Apa Itu Set Top Box (STB)?", "id": "Alat Konverter Sinyal Digital Ke TV Analog." },
  { "en": "Apa Itu TV Analog?", "id": "Siaran TV Sinyal Analog (Sudah Dimatikan)." },
  { "en": "Apa Itu ASO (Analog Switch-Off)?", "id": "Proses Penghentian Siaran TV Analog." },
  { "en": "Apa Itu Smart TV?", "id": "Televisi Yang Terhubung Internet (Aplikasi)." },
  { "en": "Apa Itu TV Box (Android TV Box)?", "id": "Alat Pengubah TV Biasa Menjadi Smart TV." },
  { "en": "Apa Itu Chromecast?", "id": "Dongle Streaming Google (Casting Layar)." },
  { "en": "Apa Itu Miracast?", "id": "Standar Nirkabel Pencerminan Layar (Screen Mirroring)." },
  { "en": "Apa Itu DLNA (Digital Living Network Alliance)?", "id": "Standar Berbagi Media Di Jaringan Rumah." },
  { "en": "Apa Itu Home Theater (Teater Rumah)?", "id": "Sistem Audio Video Bioskop Versi Rumah." },
  { "en": "Apa Itu Penerima AV (AV Receiver)?", "id": "Pusat Kontrol Audio Video Home Theater." },
  { "en": "Apa Itu Soundbar?", "id": "Speaker Audio Memanjang (Pengganti Speaker TV)." },
  { "en": "Apa Itu Proyektor (Projector)?", "id": "Alat Proyeksi Gambar Ke Layar Besar." },
  { "en": "Apa Itu Proyektor DLP (Digital Light Processing)?", "id": "Proyektor Menggunakan Cermin Mikro Digital." },
  { "en": "Apa Itu Proyektor LCD (Liquid Crystal Display)?", "id": "Proyektor Menggunakan Panel LCD." },
  { "en": "Apa Itu Proyektor LCoS (Liquid Crystal on Silicon)?", "id": "Proyektor Hibrida (DLP Dan LCD)." },
  { "en": "Apa Itu Proyektor Laser?", "id": "Proyektor Menggunakan Sumber Cahaya Laser." },
  { "en": "Apa Itu ANSI Lumen?", "id": "Satuan Standar Kecerahan Proyektor." },
  { "en": "Apa Itu Rasio Kontras (Contrast Ratio)?", "id": "Perbandingan Antara Putih Terang Hitam Gelap." },
  { "en": "Apa Itu Keystone Correction (Proyektor)?", "id": "Fitur Koreksi Gambar Trapesium." },
  { "en": "Apa Itu Throw Ratio (Proyektor)?", "id": "Menentukan Jarak Proyektor Ke Layar." },
  { "en": "Apa Itu Proyektor Short Throw?", "id": "Proyektor Untuk Jarak Sangat Dekat." },
  { "en": "Apa Itu Layar Proyektor (Projector Screen)?", "id": "Layar Khusus Penerima Proyeksi." },
  { "en": "Apa Itu OHP (Overhead Projector)?", "id": "Proyektor Menggunakan Plastik Transparan (Lama)." },
  { "en": "Apa Itu Elektronika Medis?", "id": "Elektronika Untuk Aplikasi Kesehatan." },
  { "en": "Apa Itu EKG (Elektrokardiogram)?", "id": "Alat Perekam Aktivitas Listrik Jantung." },
  { "en": "Apa Itu EEG (Elektroensefalogram)?", "id": "Alat Perekam Aktivitas Listrik Otak." },
  { "en": "Apa Itu EMG (Elektromiogram)?", "id": "Alat Perekam Aktivitas Listrik Otot." },
  { "en": "Apa Itu Tensi Meter Digital?", "id": "Alat Ukur Tekanan Darah Otomatis." },
  { "en": "Apa Itu Oksimeter Denyut (Pulse Oximeter)?", "id": "Alat Ukur Saturasi Oksigen Darah." },
  { "en": "Bagaimana Oksimeter Denyut Bekerja?", "id": "Menggunakan Cahaya Merah Inframerah (Jari)." },
  { "en": "Apa Itu Termometer Digital?", "id": "Alat Ukur Suhu Tubuh Elektronik." },
  { "en": "Apa Itu Termometer Inframerah (Tembak)?", "id": "Termometer Pengukur Suhu (Tanpa Sentuh)." },
  { "en": "Apa Itu Alat Pacu Jantung (Pacemaker)?", "id": "Perangkat Implan Stimulator Listrik Jantung." },
  { "en": "Apa Itu Defibrilator?", "id": "Alat Kejut Listrik Jantung (Henti Jantung)." },
  { "en": "Apa Itu AED (Automated External Defibrillator)?", "id": "Defibrilator Otomatis Untuk Publik." },
  { "en": "Apa Itu USG (Ultrasonografi)?", "id": "Pencitraan Medis Menggunakan Gelombang Suara." },
  { "en": "Apa Itu Sinar-X (X-Ray)?", "id": "Pencitraan Medis Menggunakan Radiasi Elektromagnetik." },
  { "en": "Apa Itu CT Scan (Computed Tomography)?", "id": "Pencitraan Sinar-X Tiga Dimensi." },
  { "en": "Apa Itu MRI (Magnetic Resonance Imaging)?", "id": "Pencitraan Medis Menggunakan Medan Magnet." },
  { "en": "Apa Itu Mesin Dialisis (Cuci Darah)?", "id": "Mesin Pembersih Darah (Gagal Ginjal)." },
  { "en": "Apa Itu Elektronika Otomotif?", "id": "Sistem Elektronik Di Dalam Kendaraan." },
  { "en": "Apa Itu ECU (Engine Control Unit)?", "id": "Komputer Kontrol Utama Mesin Mobil." },
  { "en": "Apa Itu Injeksi Bahan Bakar Elektronik (EFI)?", "id": "Sistem Injeksi Bensin Terkontrol Komputer." },
  { "en": "Apa Itu Sistem ABS (Anti-lock Braking System)?", "id": "Sistem Pengereman Anti Terkunci." },
  { "en": "Fungsi Sistem ABS (Anti-lock Braking System)?", "id": "Mencegah Roda Terkunci Saat Rem Mendadak." },
  { "en": "Apa Itu EBD (Electronic Brakeforce Distribution)?", "id": "Distribusi Kekuatan Rem Elektronik." },
  { "en": "Apa Itu Kontrol Traksi (Traction Control)?", "id": "Mencegah Roda Selip Saat Akselerasi." },
  { "en": "Apa Itu ESP (Electronic Stability Program)?", "id": "Program Stabilitas Elektronik (Mencegah Tergelincir)." },
  { "en": "Apa Itu Kantung Udara (Airbag)?", "id": "Sistem Keselamatan Bantalan Udara." },
  { "en": "Apa Itu Sensor Tabrakan (Airbag)?", "id": "Sensor Pendeteksi Tabrakan (Mengaktifkan Airbag)." },
  { "en": "Apa Itu Power Steering Elektronik (EPS)?", "id": "Sistem Kemudi Ringan Berbasis Motor Listrik." },
  { "en": "Apa Itu Transmisi Otomatis Elektronik?", "id": "Transmisi Otomatis Dikontrol Komputer." },
  { "en": "Apa Itu Drive-By-Wire (Throttle)?", "id": "Sistem Gas Tanpa Kabel (Sensor Pedal)." },
  { "en": "Apa Itu Cruise Control (Kontrol Jelajah)?", "id": "Fitur Penjaga Kecepatan Mobil Otomatis." },
  { "en": "Apa Itu Cruise Control (Kontrol Jelajah) Adaptif?", "id": "Cruise Control Dengan Sensor Jarak (Radar)." },
  { "en": "Apa Itu Sensor Parkir (Mundur)?", "id": "Sensor Ultrasonik Pendeteksi Halangan Belakang." },
  { "en": "Apa Itu Kamera Parkir (Mundur)?", "id": "Kamera Bantuan Visual Saat Mundur." },
  { "en": "Apa Itu Sistem Peringatan Titik Buta (Blind Spot)?", "id": "Sensor Peringatan Kendaraan Di Titik Buta." },
  { "en": "Apa Itu Immobilizer (Kunci Mobil)?", "id": "Sistem Keamanan Anti-Pencurian Kunci." },
  { "en": "Bagaimana Immobilizer Bekerja?", "id": "Chip Transponder Di Kunci (RFID)." },
  { "en": "Apa Itu Keyless Entry (Masuk Tanpa Kunci)?", "id": "Membuka Kunci Mobil Dengan Remote." },
  { "en": "Apa Itu Start-Stop Engine (Tombol)?", "id": "Menyalakan Mesin Menggunakan Tombol." },
  { "en": "Apa Itu OBD-II (On-Board Diagnostics II)?", "id": "Port Diagnostik Standar Mobil." },
  { "en": "Fungsi Port OBD-II (On-Board Diagnostics II)?", "id": "Membaca Kode Error Komputer Mobil." },
  { "en": "Apa Itu Busi (Spark Plug)?", "id": "Komponen Pemantik Api Ruang Bakar." },
  { "en": "Apa Itu Koil Pengapian (Ignition Coil)?", "id": "Trafo Step-Up Pembangkit Tegangan Busi." },
  { "en": "Apa Itu Alternator (Mobil)?", "id": "Generator Pengisi Aki Saat Mesin Nyala." },
  { "en": "Apa Itu Aki (Accumulator) Mobil?", "id": "Baterai Penyimpan Listrik (Starter)." },
  { "en": "Apa Itu Sistem Kelistrikan Mobil?", "id": "Sistem Tegangan Rendah (Biasanya 12 Volt DC)." },
  { "en": "Apa Itu Mobil Listrik (Electric Vehicle)?", "id": "Mobil Yang Bergerak Dengan Motor Listrik." },
  { "en": "Apa Itu Mobil Hibrida (Hybrid Vehicle)?", "id": "Mobil Gabungan Mesin Bensin Dan Motor Listrik." },
  { "en": "Apa Itu Baterai Traksi (Traction Battery)?", "id": "Baterai Besar Sumber Tenaga Mobil Listrik." },
  { "en": "Apa Itu Stasiun Pengisian (Charging Station) EV?", "id": "Tempat Pengisian Ulang Baterai Mobil Listrik." },
  { "en": "Apa Itu Avionik (Avionics)?", "id": "Sistem Elektronik Pesawat Terbang." },
  { "en": "Apa Itu Radar (Radio Detection and Ranging)?", "id": "Sistem Deteksi Objek Gelombang Radio." },
  { "en": "Apa Itu Sonar (Sound Navigation and Ranging)?", "id": "Sistem Deteksi Objek Gelombang Suara (Air)." },
  { "en": "Di Mana Sonar Digunakan?", "id": "Kapal Selam, Pendeteksi Ikan (Fish Finder)." },
  { "en": "Apa Itu Militer Elektronika?", "id": "Aplikasi Elektronika Untuk Pertahanan." },
  { "en": "Apa Itu Peperangan Elektronik (Electronic Warfare)?", "id": "Penggunaan Spektrum Elektromagnetik (Perang)." },
  { "en": "Apa Itu Jamming (Perang Elektronik)?", "id": "Mengganggu Sinyal Komunikasi Musuh." },
  { "en": "Apa Itu Rudal (Peluru Kendali)?", "id": "Senjata Dengan Sistem Pemandu Elektronik." },
  { "en": "Apa Itu Pemandu Inframerah (Rudal)?", "id": "Pemandu Rudal Mencari Sumber Panas." },
  { "en": "Apa Itu Pemandu Radar (Rudal)?", "id": "Pemandu Rudal Menggunakan Gelombang Radar." },
  { "en": "Apa Itu Pemandu Laser (Rudal)?", "id": "Pemandu Rudal Mengikuti Pantulan Sinar Laser." },
  { "en": "Apa Itu Pemandu GPS (Rudal)?", "id": "Pemandu Rudal Menggunakan Koordinat Satelit." },
  { "en": "Apa Itu Kacamata Malam (Night Vision)?", "id": "Alat Penglihatan Di Kondisi Gelap." },
  { "en": "Bagaimana Kacamata Malam Bekerja?", "id": "Menguatkan Sisa Cahaya (Bintang, Bulan)." },
  { "en": "Apa Itu Pencitraan Termal (Thermal Imaging)?", "id": "Melihat Perbedaan Suhu (Panas Tubuh)." },
  { "en": "Apa Beda Night Vision Dan Termal?", "id": "Night Vision (Butuh Cahaya), Termal (Deteksi Panas)." },
  { "en": "Apa Itu Enkripsi (Cryptography)?", "id": "Proses Pengacakan Data (Keamanan)." },
  { "en": "Apa Itu Dekripsi (Cryptography)?", "id": "Proses Pengembalian Data Acak (Kunci)." },
  { "en": "Apa Itu Enkripsi Simetris?", "id": "Kunci Enkripsi Dan Dekripsi Sama." },
  { "en": "Apa Itu Enkripsi Asimetris (Kunci Publik)?", "id": "Kunci Enkripsi Dan Dekripsi Berbeda." },
  { "en": "Apa Itu AES (Advanced Encryption Standard)?", "id": "Standar Enkripsi Simetris Kuat." },
  { "en": "Apa Itu RSA (Rivest-Shamir-Adleman)?", "id": "Standar Enkripsi Asimetris Populer." },
  { "en": "Apa Itu Kunci Publik (Public Key)?", "id": "Kunci Untuk Enkripsi (Boleh Disebar)." },
  { "en": "Apa Itu Kunci Privat (Private Key)?", "id": "Kunci Untuk Dekripsi (Harus Rahasia)." },
  { "en": "Apa Itu Tanda Tangan Digital (Digital Signature)?", "id": "Validasi Keaslian Pengirim Data." },
  { "en": "Apa Itu Hash (Fungsi Hash)?", "id": "Algoritma Sidik Jari Digital Unik." },
  { "en": "Apa Sifat Fungsi Hash?", "id": "Satu Arah, Ukuran Tetap, Unik." },
  { "en": "Contoh Algoritma Hash?", "id": "MD5 (Message Digest 5), SHA-256 (Secure Hash Algorithm)." },
  { "en": "Apa Itu MD5 (Message Digest 5)?", "id": "Algoritma Hash Lama (Tidak Aman)." },
  { "en": "Apa Itu SHA-256 (Secure Hash Algorithm 256)?", "id": "Algoritma Hash Aman (Umum Dipakai)." },
  { "en": "Apa Itu Sertifikat Digital?", "id": "Bukti Identitas Digital (Website)." },
  { "en": "Apa Itu CA (Certificate Authority)?", "id": "Lembaga Penerbit Sertifikat Digital." },
  { "en": "Apa Itu SSL (Secure Sockets Layer)?", "id": "Protokol Keamanan Jaringan (Lama)." },
  { "en": "Apa Itu TLS (Transport Layer Security)?", "id": "Protokol Keamanan (Pengganti SSL)." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Jaringan Privat Virtual (Koneksi Aman)." },
  { "en": "Bagaimana VPN (Virtual Private Network) Bekerja?", "id": "Membuat Terowongan (Tunnel) Terenkripsi." },
  { "en": "Apa Itu Blockchain (Rantai Blok)?", "id": "Buku Besar Digital Terdistribusi." },
  { "en": "Apa Itu Cryptocurrency (Mata Uang Kripto)?", "id": "Mata Uang Digital Terenkripsi." },
  { "en": "Contoh Cryptocurrency (Mata Uang Kripto)?", "id": "Bitcoin, Ethereum." },
  { "en": "Apa Itu Penambangan (Mining) Kripto?", "id": "Proses Validasi Transaksi Kripto." },
  { "en": "Apa Itu GPU (Graphics Processing Unit) Mining?", "id": "Penambangan Kripto Menggunakan Kartu Grafis." },
  { "en": "Apa Itu ASIC (Application-Specific Integrated Circuit) Miner?", "id": "Mesin Penambang Kripto Khusus." },
  { "en": "Apa Itu Bukti Kerja (Proof of Work)?", "id": "Algoritma Konsensus Penambangan (Bitcoin)." },
  { "en": "Apa Itu Bukti Kepemilikan (Proof of Stake)?", "id": "Algoritma Konsensus (Hemat Energi)." },
  { "en": "Apa Itu Kontrak Pintar (Smart Contract)?", "id": "Program Otomatis Di Atas Blockchain." },
  { "en": "Di Mana Kontrak Pintar Populer?", "id": "Jaringan Ethereum." },
  { "en": "Apa Itu NFT (Non-Fungible Token)?", "id": "Token Digital Unik (Karya Seni)." },
  { "en": "Apa Itu Kuantum Komputer (Quantum Computing)?", "id": "Komputer Berbasis Mekanika Kuantum." },
  { "en": "Apa Itu Qubit (Quantum Bit)?", "id": "Unit Dasar Komputasi Kuantum." },
  { "en": "Apa Beda Bit Dan Qubit?", "id": "Bit (0/1), Qubit (Superposisi)." },
  { "en": "Apa Itu Superposisi (Kuantum)?", "id": "Kondisi Bisa 0 Dan 1 Bersamaan." },
  { "en": "Apa Itu Keterkaitan (Entanglement) Kuantum?", "id": "Koneksi Antar Qubit (Meskipun Jauh)." },
  { "en": "Apa Ancaman Komputer Kuantum?", "id": "Dapat Memecahkan Enkripsi RSA." },
  { "en": "Apa Itu Printer 3D (3D Printing)?", "id": "Mesin Pencetak Objek Tiga Dimensi." },
  { "en": "Apa Itu Manufaktur Aditif (Additive)?", "id": "Metode Pencetakan Lapis Demi Lapis." },
  { "en": "Apa Itu Printer 3D FDM (Fused Deposition Modeling)?", "id": "Printer 3D Menggunakan Filamen Plastik." },
  { "en": "Apa Itu Filamen (Printer 3D)?", "id": "Bahan Plastik Gulungan (PLA, ABS)." },
  { "en": "Apa Itu PLA (Polylactic Acid)?", "id": "Filamen 3D Populer (Ramah Lingkungan)." },
  { "en": "Apa Itu ABS (Acrylonitrile Butadiene Styrene)?", "id": "Filamen 3D Kuat (Butuh Panas)." },
  { "en": "Apa Itu Extruder (Printer 3D)?", "id": "Bagian Pendorong Peleleh Filamen." },
  { "en": "Apa Itu Hotend (Printer 3D)?", "id": "Bagian Pemanas Ujung Extruder." },
  { "en": "Apa Itu Nozzle (Printer 3D)?", "id": "Ujung Lubang Kecil Keluarnya Filamen." },
  { "en": "Apa Itu Heated Bed (Printer 3D)?", "id": "Alas Cetak Yang Dipanaskan (Mencegah Melengkung)." },
  { "en": "Apa Itu Warping (Printer 3D)?", "id": "Sudut Cetakan Terangkat (Melengkung)." },
  { "en": "Apa Itu Printer 3D SLA (Stereolithography)?", "id": "Printer 3D Menggunakan Resin Cair (Laser)." },
  { "en": "Apa Itu Printer 3D DLP (Digital Light Processing)?", "id": "Printer 3D Resin (Proyektor UV)." },
  { "en": "Apa Itu Printer 3D SLS (Selective Laser Sintering)?", "id": "Printer 3D Menggunakan Bubuk (Laser)." },
  { "en": "Apa Itu File STL (Stereolithography)?", "id": "Format File Model 3D (Umum)." },
  { "en": "Apa Itu Slicer (Perangkat Lunak)?", "id": "Pemotong Model 3D Menjadi Lapisan (G-Code)." },
  { "en": "Apa Itu G-Code?", "id": "Bahasa Perintah Mesin CNC Printer 3D." },
  { "en": "Apa Itu Mesin CNC (Computer Numerical Control)?", "id": "Mesin Pabrikasi Terkontrol Komputer." },
  { "en": "Apa Itu Manufaktur Subtraktif (Subtractive)?", "id": "Metode Membuang Material (Bor, Potong)." },
  { "en": "Contoh Mesin CNC (Computer Numerical Control)?", "id": "Mesin Bubut, Mesin Fraise (Milling)." },
  { "en": "Apa Itu Mesin Pemotong Laser (Laser Cutter)?", "id": "Mesin CNC Pemotong Bahan Laser." },
  { "en": "Apa Itu CAD (Computer-Aided Design)?", "id": "Perangkat Lunak Desain Model (2D/3D)." },
  { "en": "Contoh Perangkat Lunak CAD?", "id": "AutoCAD, SolidWorks, Fusion 360." },
  { "en": "Apa Itu CAM (Computer-Aided Manufacturing)?", "id": "Perangkat Lunak Pembuat G-Code (CNC)." },
  { "en": "Apa Itu Mechatronics (Mekatronika)?", "id": "Gabungan Teknik Mesin, Elektro, Informatika." },
  { "en": "Apa Itu Bionics (Bionik)?", "id": "Ilmu Terapan Sistem Biologi Ke Teknik." },
  { "en": "Contoh Bionics (Bionik)?", "id": "Tangan Prostetik Robotik." },
  { "en": "Apa Itu Prostetik (Prosthetic)?", "id": "Alat Pengganti Anggota Tubuh Buatan." },
  { "en": "Apa Itu Cybernetics (Sibernetika)?", "id": "Ilmu Sistem Kontrol Umpan Balik." },
  { "en": "Apa Itu AI (Artificial Intelligence)?", "id": "Kecerdasan Buatan (Simulasi Manusia)." },
  { "en": "Apa Itu Kecerdasan Kuat (Strong AI)?", "id": "AI Selevel Manusia (Hipotetis)." },
  { "en": "Apa Itu Kecerdasan Lemah (Weak AI)?", "id": "AI Tugas Spesifik (Saat Ini)." },
  { "en": "Contoh Kecerdasan Lemah (Weak AI)?", "id": "Asisten Virtual, Rekomendasi Produk." },
  { "en": "Apa Itu Tes Turing (Turing Test)?", "id": "Tes Kecerdasan Mesin (Menipu Manusia)." },
  { "en": "Apa Itu NLP (Natural Language Processing)?", "id": "Pemrosesan Bahasa Manusia Oleh Komputer." },
  { "en": "Apa Itu LLM (Large Language Model)?", "id": "Model AI Bahasa Skala Besar." },
  { "en": "Apa Itu Generative AI (AI Generatif)?", "id": "AI Yang Dapat Menciptakan Konten Baru." },
  { "en": "Apa Itu Big Data (Data Raya)?", "id": "Kumpulan Data Sangat Besar Kompleks." },
  { "en": "Apa Itu Cloud Computing (Komputasi Awan)?", "id": "Layanan Komputasi Melalui Internet." },
  { "en": "Apa Itu IaaS (Infrastructure as a Service)?", "id": "Layanan Awan (Infrastruktur Virtual)." },
  { "en": "Apa Itu PaaS (Platform as a Service)?", "id": "Layanan Awan (Platform Pengembangan)." },
  { "en": "Apa Itu SaaS (Software as a Service)?", "id": "Layanan Awan (Perangkat Lunak Jadi)." },
  { "en": "Contoh SaaS (Software as a Service)?", "id": "Gmail, Google Docs, Netflix." },
  { "en": "Apa Itu Server?", "id": "Komputer Penyedia Layanan Jaringan." },
  { "en": "Apa Itu Klien (Client)?", "id": "Komputer Pengakses Layanan Server." },
  { "en": "Apa Itu Data Center (Pusat Data)?", "id": "Fasilitas Penyimpanan Ribuan Server." },
  { "en": "Apa Itu Virtualisasi (Virtualization)?", "id": "Membuat Versi Virtual (Mesin, Jaringan)." },
  { "en": "Apa Itu Virtual Machine (VM)?", "id": "Mesin Virtual (Komputer Di Dalam Komputer)." },
  { "en": "Apa Itu Hypervisor?", "id": "Perangkat Lunak Pembuat Mesin Virtual." },
  { "en": "Apa Itu Kontainer (Containerization)?", "id": "Virtualisasi Level Aplikasi (Ringan)." },
  { "en": "Contoh Kontainer (Containerization)?", "id": "Docker." },
  { "en": "Apa Itu Kubernetes?", "id": "Orkestrasi Platform Manajemen Kontainer." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Penghubung Antar Perangkat Lunak." },
  { "en": "Apa Itu REST (Representational State Transfer) API?", "id": "Arsitektur API Populer (Basis HTTP)." },
  { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Teks Pertukaran Data Ringan." },
  { "en": "Apa Itu XML (Extensible Markup Language)?", "id": "Bahasa Markup (Struktur Data)." },
  { "en": "Apa Beda JSON (JavaScript Object Notation) Dan XML?", "id": "JSON Lebih Ringan Dan Sederhana." },
  { "en": "Apa Itu SDK (Software Development Kit)?", "id": "Kumpulan Alat Bantu Pengembangan Software." },
  { "en": "Apa Itu Open Source (Sumber Terbuka)?", "id": "Kode Program Terbuka Untuk Publik." },
  { "en": "Contoh Open Source (Sumber Terbuka)?", "id": "Linux, Android, Firefox." },
  { "en": "Apa Itu Proprietary Software (Tertutup)?", "id": "Perangkat Lunak Berbayar (Kode Tertutup)." },
  { "en": "Contoh Proprietary Software (Tertutup)?", "id": "Windows, MacOS, Microsoft Office." },
  { "en": "Apa Itu Git?", "id": "Sistem Kontrol Versi Terdistribusi." },
  { "en": "Apa Itu GitHub?", "id": "Layanan Hosting Proyek Git Populer." },
  { "en": "Apa Itu Kontrol Versi (Version Control)?", "id": "Sistem Perekam Perubahan Kode Program." },
  { "en": "Apa Itu Repository (Repositori)?", "id": "Tempat Penyimpanan Proyek Git." },
  { "en": "Apa Itu Commit (Git)?", "id": "Proses Menyimpan Perubahan Ke Repositori." },
  { "en": "Apa Itu Branch (Cabang Git)?", "id": "Cabang Pengembangan Paralel (Fitur Baru)." },
  { "en": "Apa Itu Merge (Menggabungkan Git)?", "id": "Proses Menggabungkan Dua Cabang (Branch)." },
  { "en": "Apa Itu Pull Request (Permintaan Tarik)?", "id": "Permintaan Menggabungkan Kode (Branch Ke Master)." },
  { "en": "Apa Itu Agile (Metodologi)?", "id": "Metodologi Pengembangan Perangkat Lunak Iteratif." },
  { "en": "Apa Itu Scrum (Agile)?", "id": "Kerangka Kerja Agile (Sprint, Stand-up)." },
  { "en": "Apa Itu Sprint (Scrum)?", "id": "Siklus Kerja Jangka Pendek (Contoh: 2 Minggu)." },
  { "en": "Apa Itu Waterfall (Metodologi)?", "id": "Metodologi Pengembangan Sekuensial (Lama)." },
  { "en": "Apa Itu DevOps (Development Operations)?", "id": "Integrasi Pengembangan Perangkat Lunak Operasi TI." },
  { "en": "Apa Itu CI/CD (Continuous Integration/Delivery)?", "id": "Otomasi Proses Rilis Perangkat Lunak." },
  { "en": "Apa Itu Unit Testing (Tes Unit)?", "id": "Pengujian Fungsi Individual Kode." },
  { "en": "Apa Itu Integration Testing (Tes Integrasi)?", "id": "Pengujian Interaksi Antar Modul." },
  { "en": "Apa Itu System Testing (Tes Sistem)?", "id": "Pengujian Keseluruhan Sistem Lengkap." },
  { "en": "Apa Itu Acceptance Testing (Tes Penerimaan)?", "id": "Pengujian Oleh Pengguna (User)." },
  { "en": "Apa Itu Regression Testing (Tes Regresi)?", "id": "Memastikan Fitur Baru Tidak Merusak Lama." },
  { "en": "Apa Itu Bug (Kutu)?", "id": "Kesalahan Atau Cacat Dalam Program." },
  { "en": "Apa Itu Debugging (Pengawakutuan)?", "id": "Proses Mencari Dan Memperbaiki Bug." },
  { "en": "Apa Itu Kode Biner (Binary Code)?", "id": "Bahasa Mesin (Hanya 0 Dan 1)." },
  { "en": "Apa Itu Bahasa Assembly (Rakitan)?", "id": "Bahasa Pemrograman Level Rendah (Mnemonik)." },
  { "en": "Apa Itu Bahasa Pemrograman Level Tinggi?", "id": "Bahasa Pemrograman (Mudah Dibaca Manusia)." },
  { "en": "Contoh Bahasa Level Tinggi?", "id": "Python, Java, C++, JavaScript." },
  { "en": "Apa Itu Bahasa Kompilasi (Compiled)?", "id": "Bahasa Diterjemahkan Sekaligus (Contoh: C++)." },
  { "en": "Apa Itu Bahasa Interpretasi (Interpreted)?", "id": "Bahasa Diterjemahkan Baris Per Baris (Python)." },
  { "en": "Apa Itu JavaScript?", "id": "Bahasa Pemrograman Untuk Interaktivitas Web." },
  { "en": "Apa Itu HTML (Hypertext Markup Language)?", "id": "Bahasa Markup Struktur Halaman Web." },
  { "en": "Apa Itu CSS (Cascading Style Sheets)?", "id": "Bahasa Penataan Gaya Halaman Web (Warna)." },
  { "en": "Apa Itu Front-End (Web)?", "id": "Pengembangan Sisi Klien (Yang Terlihat)." },
  { "en": "Apa Itu Back-End (Web)?", "id": "Pengembangan Sisi Server (Logika, Database)." },
  { "en": "Apa Itu Database (Basis Data)?", "id": "Sistem Penyimpanan Data Terstruktur." },
  { "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Kueri Database Relasional." },
  { "en": "Contoh Database SQL?", "id": "MySQL, PostgreSQL, SQL Server." },
  { "en": "Apa Itu Database NoSQL?", "id": "Database Non-Relasional (Fleksibel)." },
  { "en": "Contoh Database NoSQL?", "id": "MongoDB, Redis." },
  { "en": "Apa Itu Sistem Operasi (Operating System)?", "id": "Perangkat Lunak Pengelola Perangkat Keras." },
  { "en": "Contoh Sistem Operasi (Operating System)?", "id": "Windows, MacOS, Linux, Android, iOS." },
  { "en": "Apa Itu Linux?", "id": "Sistem Operasi Open Source (Berbasis Unix)." },
  { "en": "Apa Itu Kernel (Sistem Operasi)?", "id": "Inti Utama Sistem Operasi." },
  { "en": "Apa Itu Distro (Distribusi) Linux?", "id": "Varian Linux (Contoh: Ubuntu, Debian)." },
  { "en": "Apa Itu Terminal (Command Line)?", "id": "Antarmuka Teks Perintah Sistem Operasi." },
  { "en": "Apa Itu GUI (Graphical User Interface)?", "id": "Antarmuka Grafis Pengguna (Visual)." },
  { "en": "Apa Itu Virtual Reality (VR)?", "id": "Simulasi Imersif Tiga Dimensi." },
  { "en": "Alat Virtual Reality (VR)?", "id": "Headset VR (Contoh: Oculus Rift)." },
  { "en": "Apa Itu Augmented Reality (AR)?", "id": "Menampilkan Objek Digital Di Dunia Nyata." },
  { "en": "Contoh Augmented Reality (AR)?", "id": "Game Pokemon Go, Filter Instagram." },
  { "en": "Apa Itu Mixed Reality (MR)?", "id": "Gabungan VR Dan AR (Interaktif)." },
  { "en": "Apa Itu Internet?", "id": "Jaringan Komputer Global." },
  { "en": "Apa Itu World Wide Web (WWW)?", "id": "Layanan Informasi Di Atas Internet." },
  { "en": "Apa Itu Browser (Peramban) Web?", "id": "Perangkat Lunak Pengakses Halaman Web." },
  { "en": "Apa Itu URL (Uniform Resource Locator)?", "id": "Alamat Unik Halaman Web." },
  { "en": "Apa Itu Domain Name (Nama Domain)?", "id": "Nama Unik Website (Contoh: google.com)." },
  { "en": "Apa Itu Hosting (Web Hosting)?", "id": "Layanan Penyimpanan File Website." },
  { "en": "Apa Itu Server Web?", "id": "Perangkat Lunak Penyaji Halaman Web." },
  { "en": "Contoh Server Web?", "id": "Apache, Nginx." },
  { "en": "Apa Itu Cookie (Web)?", "id": "File Kecil Penyimpan Data Sesi Browser." },
  { "en": "Apa Itu Cache (Web)?", "id": "Penyimpanan Sementara (Mempercepat Loading)." },
  { "en": "Apa Itu Malware?", "id": "Perangkat Lunak Berbahaya (Virus, Ransomware)." },
  { "en": "Apa Itu Virus (Komputer)?", "id": "Program Malware Pengganda Diri." },
  { "en": "Apa Itu Worm (Cacing Komputer)?", "id": "Virus Yang Menyebar Lewat Jaringan." },
  { "en": "Apa Itu Trojan (Kuda Troya)?", "id": "Malware Menyamar Sebagai Program Legal." },
  { "en": "Apa Itu Ransomware (Perangkat Pemeras)?", "id": "Malware Pengenkripsi Data (Minta Tebusan)." },
  { "en": "Apa Itu Spyware (Perangkat Pengintai)?", "id": "Malware Pencuri Data Diam-Diam." },
  { "en": "Apa Itu Adware (Perangkat Iklan)?", "id": "Malware Penampil Iklan Paksa." },
  { "en": "Apa Itu Phishing (Pengelabuan)?", "id": "Upaya Penipuan (Mencuri Password)." },
  { "en": "Apa Itu Antivirus?", "id": "Perangkat Lunak Pelindung Dari Malware." },
  { "en": "Apa Itu Firewall (Tembok Api)?", "id": "Penyaring Lalu Lintas Jaringan (Keamanan)." },
  { "en": "Apa Itu Otentikasi Dua Faktor (2FA)?", "id": "Metode Keamanan Dua Lapis (Password + Kode)." },
  { "en": "Apa Itu Biometrik?", "id": "Otentikasi Menggunakan Ciri Fisik (Sidik Jari)." },
  { "en": "Apa Itu Kapasitor Film?", "id": "Kapasitor Dielektrik Plastik (Milar, Polipropilena)." },
  { "en": "Apa Itu Kapasitor Silver Mica?", "id": "Kapasitor Presisi Frekuensi Tinggi (Stabil)." },
  { "en": "Apa Itu Kapasitor Feed-Through?", "id": "Kapasitor Filter Noise (Dipasang Di Sasis)." },
  { "en": "Apa Itu Kapasitor Safety (Pengaman)?", "id": "Kapasitor Khusus Proteksi Listrik AC." },
  { "en": "Apa Itu Kapasitor Kelas X?", "id": "Kapasitor Safety (Antara Fasa Netral)." },
  { "en": "Apa Itu Kapasitor Kelas Y?", "id": "Kapasitor Safety (Antara Fasa Ground)." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Dengan Tegangan Maju Rendah." },
  { "en": "Kelebihan Dioda Schottky?", "id": "Switching Cepat, Efisiensi Tinggi." },
  { "en": "Di Mana Dioda Schottky Digunakan?", "id": "SMPS, Rangkaian Frekuensi Tinggi." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda Penstabil Tegangan." },
  { "en": "Bagaimana Dioda Zener Bekerja?", "id": "Menjaga Tegangan Tetap Di Daerah Breakdown." },
  { "en": "Apa Itu Tegangan Zener (VZ)?", "id": "Tegangan Stabil Yang Dihasilkan Dioda Zener." },
  { "en": "Apa Itu Rangkaian Regulator Zener?", "id": "Regulator Sederhana (Zener Resistor)." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya." },
  { "en": "Apa Itu Tegangan Maju (Forward Voltage) LED?", "id": "Tegangan Minimal Agar LED Menyala." },
  { "en": "Kenapa LED (Light Emitting Diode) Perlu Resistor?", "id": "Untuk Membatasi Arus Listrik." },
  { "en": "Apa Itu LED (Light Emitting Diode) RGB?", "id": "Satu LED Dengan Tiga Warna (Merah, Hijau, Biru)." },
  { "en": "Apa Itu LED (Light Emitting Diode) RGB Common Cathode?", "id": "Semua Katoda Negatif Terhubung." },
  { "en": "Apa Itu LED (Light Emitting Diode) RGB Common Anode?", "id": "Semua Anoda Positif Terhubung." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Cahaya." },
  { "en": "Bagaimana Sifat LDR (Light Dependent Resistor)?", "id": "Cahaya Terang Hambatan Kecil." },
  { "en": "Apa Itu Photodiode (Dioda Foto)?", "id": "Dioda Peka Cahaya (Respon Cepat)." },
  { "en": "Apa Itu Phototransistor (Transistor Foto)?", "id": "Transistor Peka Cahaya (Lebih Sensitif)." },
  { "en": "Apa Itu Optocoupler (Optoisolator)?", "id": "Isolator Rangkaian (LED Phototransistor)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay Elektronik (Tanpa Bagian Bergerak)." },
  { "en": "Komponen Internal Solid State Relay (SSR)?", "id": "Optocoupler Dan TRIAC (Atau MOSFET)." },
  { "en": "Kelebihan Solid State Relay (SSR) Dari Relay Mekanis?", "id": "Lebih Cepat, Awet, Tanpa Suara." },
  { "en": "Kekurangan Solid State Relay (SSR)?", "id": "Lebih Panas, Harga Lebih Mahal." },
  { "en": "Apa Itu Relay (Elektromekanis)?", "id": "Saklar Dikontrol Elektromagnet." },
  { "en": "Apa Itu Kumparan (Coil) Relay?", "id": "Bagian Elektromagnet Pemicu Relay." },
  { "en": "Apa Itu Kontak (Contact) Relay?", "id": "Bagian Saklar Yang Terhubung Terputus." },
  { "en": "Apa Itu Kontak NO (Normally Open)?", "id": "Kontak Normal Terbuka (Menutup Saat Aktif)." },
  { "en": "Apa Itu Kontak NC (Normally Closed)?", "id": "Kontak Normal Tertutup (Membuka Saat Aktif)." },
  { "en": "Apa Itu Kontak COM (Common)?", "id": "Kaki Pusat Saklar Relay." },
  { "en": "Apa Itu Relay SPDT (Single Pole Double Throw)?", "id": "Relay Satu Kutub Dua Arah (NO/NC)." },
  { "en": "Apa Itu Relay DPDT (Double Pole Double Throw)?", "id": "Relay Dua Kutub Dua Arah." },
  { "en": "Apa Itu Dioda Flyback (Relay)?", "id": "Dioda Pelindung Paralel Kumparan Relay." },
  { "en": "Fungsi Dioda Flyback?", "id": "Meredam Lonjakan Tegangan Induktif." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Dioda Terkontrol Berfungsi Saklar DC." },
  { "en": "Kaki SCR (Silicon Controlled Rectifier)?", "id": "Anoda, Katoda, Dan Gerbang (Gate)." },
  { "en": "Bagaimana Cara Mengaktifkan SCR?", "id": "Memberi Pemicu Singkat Ke Gerbang." },
  { "en": "Bagaimana Cara Mematikan SCR?", "id": "Arus Anoda-Katoda Harus Diputus." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Saklar Elektronik Pengendali Arus AC." },
  { "en": "Kaki TRIAC (Triode for AC)?", "id": "Main Terminal 1, Main Terminal 2, Gate." },
  { "en": "Apa Beda SCR Dan TRIAC?", "id": "SCR (Satu Arah DC), TRIAC (Dua Arah AC)." },
  { "en": "Apa Itu DIAC (Diode for AC)?", "id": "Komponen Pemicu Gerbang TRIAC." },
  { "en": "Bagaimana Cara Kerja DIAC?", "id": "Menghantar Setelah Mencapai Tegangan Tembus." },
  { "en": "Di Mana Rangkaian DIAC-TRIAC Digunakan?", "id": "Peredup Lampu (Dimmer), Kontrol Kipas." },
  { "en": "Apa Itu UJT (Unijunction Transistor)?", "id": "Transistor Pemicu Osilator Relaksasi." },
  { "en": "Apa Itu PUT (Programmable UJT)?", "id": "UJT Yang Tegangan Pemicunya Dapat Diatur." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Gabungan Keuntungan MOSFET Dan BJT." },
  { "en": "Apa Keuntungan IGBT?", "id": "Input MOSFET (Impedansi Tinggi), Output BJT (Arus Kuat)." },
  { "en": "Di Mana IGBT (Insulated Gate Bipolar Transistor) Digunakan?", "id": "Inverter Daya Tinggi, Kontrol Motor AC." },
  { "en": "Apa Itu GTO (Gate Turn-Off Thyristor)?", "id": "Thyristor Yang Bisa Dimatikan Lewat Gerbang." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip IC." },
  { "en": "Apa Bagian Utama Mikrokontroler?", "id": "CPU, Memori (RAM/ROM), Port I/O." },
  { "en": "Apa Itu Port I/O (Input/Output)?", "id": "Pin Mikrokontroler Untuk Interaksi Luar." },
  { "en": "Apa Itu GPIO (General Purpose Input Output)?", "id": "Pin I/O Yang Fungsinya Fleksibel." },
  { "en": "Apa Itu CPU (Central Processing Unit)?", "id": "Otak Pemroses Instruksi Utama." },
  { "en": "Apa Itu RAM (Random Access Memory) (Mikrokontroler)?", "id": "Penyimpan Data Variabel Sementara (Volatile)." },
  { "en": "Apa Itu ROM (Read Only Memory) (Mikrokontroler)?", "id": "Penyimpan Program (Non-Volatile)." },
  { "en": "Apa Itu Memori Flash (Mikrokontroler)?", "id": "Tipe ROM/EEPROM Penyimpan Program." },
  { "en": "Apa Itu EEPROM (Electrically Erasable PROM)?", "id": "Memori Penyimpan Data Non-Volatile (Awet)." },
  { "en": "Apa Itu Arsitektur 8-Bit?", "id": "CPU Pemroses Data 8-Bit Sekaligus." },
  { "en": "Contoh Mikrokontroler 8-Bit?", "id": "ATmega328P (Arduino Uno), PIC16F." },
  { "en": "Apa Itu Arsitektur 32-Bit?", "id": "CPU Pemroses Data 32-Bit Sekaligus." },
  { "en": "Contoh Mikrokontroler 32-Bit?", "id": "ESP32, STM32, Raspberry Pi Pico." },
  { "en": "Apa Itu ARM (Advanced RISC Machine)?", "id": "Arsitektur Prosesor Populer (32/64-Bit)." },
  { "en": "Apa Itu Clock (Sistem Digital)?", "id": "Sinyal Denyut Sinkronisasi Operasi." },
  { "en": "Apa Satuan Kecepatan Clock?", "id": "Hertz (Hz), MegaHertz (MHz)." },
  { "en": "Apa Itu Osilator Internal?", "id": "Sumber Clock Internal Mikrokontroler." },
  { "en": "Apa Itu Osilator Eksternal (Kristal)?", "id": "Sumber Clock Eksternal (Lebih Akurat)." },
  { "en": "Kenapa Perlu Kristal Eksternal?", "id": "Akurasi Waktu Tinggi (Komunikasi Serial)." },
  { "en": "Apa Itu Sistem Reset?", "id": "Mengembalikan Mikrokontroler Ke Kondisi Awal." },
  { "en": "Apa Itu Reset Aktif Low?", "id": "Reset Terjadi Jika Pin Diberi Logika Low." },
  { "en": "Apa Itu Peripheral (Mikrokontroler)?", "id": "Modul Perangkat Keras Internal (Timer, ADC)." },
  { "en": "Apa Itu Modul Timer/Counter?", "id": "Peripheral Penghitung Waktu Atau Pulsa." },
  { "en": "Apa Itu Modul PWM (Pulse Width Modulation)?", "id": "Peripheral Penghasil Sinyal PWM." },
  { "en": "Apa Itu Modul ADC (Analog-to-Digital Converter)?", "id": "Peripheral Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu Modul Komunikasi Serial?", "id": "Peripheral Untuk Komunikasi (UART, I2C, SPI)." },
  { "en": "Apa Itu UART (Universal Asynchronous Receiver/Transmitter)?", "id": "Komunikasi Serial Dua Kabel (TX, RX)." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Komunikasi Serial Dua Kabel (SDA, SCL)." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Komunikasi Serial Empat Kabel (Cepat)." },
  { "en": "Apa Itu CAN (Controller Area Network) Bus?", "id": "Protokol Komunikasi Kuat (Otomotif)." },
  { "en": "Apa Itu USB (Universal Serial Bus) (Mikrokontroler)?", "id": "Peripheral Komunikasi USB Langsung." },
  { "en": "Apa Itu Ethernet (Mikrokontroler)?", "id": "Peripheral Komunikasi Jaringan Kabel." },
  { "en": "Apa Itu Register (Mikrokontroler)?", "id": "Lokasi Memori Internal Pengaturan Peripheral." },
  { "en": "Apa Itu Pemrograman Level Register?", "id": "Pemrograman Mengatur Bit Register Langsung." },
  { "en": "Apa Itu HAL (Hardware Abstraction Layer)?", "id": "Lapisan Abstraksi Perangkat Keras (Library)." },
  { "en": "Apa Itu Papan Pengembangan (Development Board)?", "id": "Papan Siap Pakai (Arduino, NodeMCU)." },
  { "en": "Apa Itu Programmer (Alat)?", "id": "Alat Pemasuk Program Ke Mikrokontroler." },
  { "en": "Contoh Programmer (Alat)?", "id": "USBasp (AVR), ST-Link (STM32)." },
  { "en": "Apa Itu ICSP (In-Circuit Serial Programming)?", "id": "Metode Pemrograman AVR Di Dalam Sirkuit." },
  { "en": "Apa Itu IDE (Integrated Development Environment)?", "id": "Perangkat Lunak Terpadu (Editor, Compiler)." },
  { "en": "Contoh IDE (Integrated Development Environment) Mikrokontroler?", "id": "Arduino IDE, PlatformIO, MPLAB." },
  { "en": "Apa Itu Compiler (Kompilator)?", "id": "Penerjemah Kode Manusia Ke Kode Mesin." },
  { "en": "Apa Itu Linker?", "id": "Penggabung File Objek Menjadi Program." },
  { "en": "Apa Itu File HEX (.hex)?", "id": "Format File Biner Kode Mesin (Intel HEX)." },
  { "en": "Apa Itu Assembler?", "id": "Penerjemah Kode Assembly Ke Kode Mesin." },
  { "en": "Apa Itu Simulator (Mikrokontroler)?", "id": "Simulasi Eksekusi Kode Di Komputer." },
  { "en": "Apa Itu Emulator (Mikrokontroler)?", "id": "Simulasi Hardware Mikrokontroler Di Komputer." },
  { "en": "Apa Itu Debugger (In-Circuit)?", "id": "Alat Debugging Kode Langsung Di Hardware." },
  { "en": "Apa Itu Breakpoint (Debugging)?", "id": "Titik Pemberhentian Paksa Eksekusi Program." },
  { "en": "Apa Itu Watch (Debugging)?", "id": "Fitur Pemantau Nilai Variabel." },
  { "en": "Apa Itu Step Over (Debugging)?", "id": "Eksekusi Satu Baris (Melompati Fungsi)." },
  { "en": "Apa Itu Step Into (Debugging)?", "id": "Eksekusi Satu Baris (Masuk Ke Fungsi)." },
  { "en": "Apa Itu Step Out (Debugging)?", "id": "Keluar Dari Fungsi (Kembali Ke Pemanggil)." },
  { "en": "Apa Itu Stack Overflow (Kesalahan)?", "id": "Memori Stack Penuh (Rekursi Berlebih)." },
  { "en": "Apa Itu Rekursi (Recursion)?", "id": "Fungsi Yang Memanggil Dirinya Sendiri." },
  { "en": "Apa Itu Null Pointer Exception?", "id": "Error Mengakses Alamat Memori Nol." },
  { "en": "Apa Itu Memory Leak (Kebocoran Memori)?", "id": "Alokasi Memori Tanpa Dealokasi." },
  { "en": "Apa Itu Alokasi Memori Dinamis?", "id": "Memesan Memori Saat Program Berjalan." },
  { "en": "Apa Itu Garbage Collector (Pengumpul Sampah)?", "id": "Otomatis Membersihkan Memori (Bukan Di C++)." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "Sistem Operasi Respon Waktu Nyata." },
  { "en": "Apa Itu Task (RTOS)?", "id": "Unit Pekerjaan Independen (Thread)." },
  { "en": "Apa Itu Scheduler (RTOS)?", "id": "Pengatur Jadwal Eksekusi Task." },
  { "en": "Apa Itu Preemptive Scheduling?", "id": "Task Prioritas Tinggi Bisa Menyela Task Rendah." },
  { "en": "Apa Itu Cooperative Scheduling?", "id": "Task Harus Sukarela Menyerahkan Kontrol." },
  { "en": "Apa Itu Semaphore (RTOS)?", "id": "Alat Sinkronisasi (Mengatur Akses Sumber Daya)." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Semaphore (Mencegah Akses Bersamaan)." },
  { "en": "Apa Itu Queue (Antrian RTOS)?", "id": "Mekanisme Pengiriman Data Antar Task." },
  { "en": "Apa Itu Inter-Task Communication (ITC)?", "id": "Komunikasi Antar Task (Antrian, Semaphore)." },
  { "en": "Apa Itu Priority Inversion (Inversi Prioritas)?", "id": "Masalah Task Prioritas Rendah Menghalangi Tinggi." },
  { "en": "Apa Itu Solusi Priority Inversion?", "id": "Priority Inheritance (Pewarisan Prioritas)." },
  { "en": "Apa Itu Deadlock (Kebuntuan)?", "id": "Kondisi Dua Task Saling Menunggu." },
  { "en": "Apa Itu FreeRTOS?", "id": "Sistem Operasi RTOS Populer (Gratis)." },
  { "en": "Apa Itu Arduino IDE?", "id": "IDE Sederhana Untuk Papan Arduino." },
  { "en": "Apa Itu PlatformIO (PIO)?", "id": "IDE Lintas Platform Untuk Mikrokontroler." },
  { "en": "Apa Itu VS (Visual Studio) Code?", "id": "Editor Kode Populer (Digunakan PlatformIO)." },
  { "en": "Apa Itu Arduino Core?", "id": "Kumpulan Library (HAL) Untuk Arduino IDE." },
  { "en": "Apa Itu Board Manager (Arduino)?", "id": "Fitur Penambah Dukungan Papan Baru." },
  { "en": "Apa Itu Library Manager (Arduino)?", "id": "Fitur Penambah Library Pihak Ketiga." },
  { "en": "Apa Itu Fritzing?", "id": "Perangkat Lunak Desain Breadboard Skematik." },
  { "en": "Apa Itu Eagle CAD?", "id": "Perangkat Lunak Desain PCB (Autodesk)." },
  { "en": "Apa Itu KiCad?", "id": "Perangkat Lunak Desain PCB (Open Source)." },
  { "en": "Apa Itu Altium Designer?", "id": "Perangkat Lunak Desain PCB Profesional." },
  { "en": "Apa Itu LTspice?", "id": "Simulator Rangkaian Analog (Gratis)." },
  { "en": "Apa Itu Proteus (Software)?", "id": "Simulator Rangkaian (Mikrokontroler Dan Analog)." },
  { "en": "Apa Itu Multisim (Software)?", "id": "Simulator Rangkaian Elektronika (NI)." },
  { "en": "Apa Itu Tinkercad Circuits?", "id": "Simulator Rangkaian Online Sederhana (Autodesk)." },
  { "en": "Apa Itu EasyEDA?", "id": "Software Desain PCB Berbasis Web." },
  { "en": "Apa Itu MATLAB (Matrix Laboratory)?", "id": "Software Komputasi Teknis Dan Simulasi." },
  { "en": "Apa Itu Simulink?", "id": "Lingkungan Simulasi Grafis (Bagian MATLAB)." },
  { "en": "Apa Itu LabVIEW (Laboratory Virtual Instrument)?", "id": "Bahasa Pemrograman Grafis (Instrumentasi)." },
  { "en": "Apa Itu OrCAD?", "id": "Perangkat Lunak Desain Rangkaian (Cadence)." },
  { "en": "Apa Itu PSpice (Personal Simulation Program)?", "id": "Simulator Rangkaian Analog (Bagian OrCAD)." },
  { "en": "Apa Itu Mathematica?", "id": "Perangkat Lunak Komputasi Simbolik." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "IC Logika Digital Yang Dapat Diprogram." },
  { "en": "Apa Beda FPGA (Field-Programmable Gate Array) Dan CPLD?", "id": "FPGA Lebih Kompleks Dan Fleksibel." },
  { "en": "Apa Itu CPLD (Complex Programmable Logic Device)?", "id": "IC Logika Terprogram (Lebih Sederhana)." },
  { "en": "Apa Itu LUT (Look-Up Table) FPGA?", "id": "Blok Logika Dasar Dalam FPGA." },
  { "en": "Apa Itu HDL (Hardware Description Language)?", "id": "Bahasa Pemrograman Perangkat Keras." },
  { "en": "Contoh HDL (Hardware Description Language)?", "id": "VHDL Dan Verilog." },
  { "en": "Apa Itu VHDL?", "id": "Bahasa Deskripsi Perangkat Keras (Standar)." },
  { "en": "Apa Itu Verilog?", "id": "Bahasa Deskripsi Perangkat Keras (Mirip C)." },
  { "en": "Apa Itu Sintesis (Synthesis) FPGA?", "id": "Proses Menerjemahkan Kode HDL Ke Logika." },
  { "en": "Apa Itu Place And Route (FPGA)?", "id": "Proses Penempatan Rute Logika Di FPGA." },
  { "en": "Apa Itu Bitstream (FPGA)?", "id": "File Konfigurasi Final Untuk FPGA." },
  { "en": "Produsen FPGA (Field-Programmable Gate Array) Utama?", "id": "Xilinx (AMD) Dan Altera (Intel)." },
  { "en": "Apa Itu SoC (System on a Chip) FPGA?", "id": "FPGA Yang Digabung Prosesor ARM." },
  { "en": "Apa Itu Papan Pengembangan FPGA?", "id": "Papan Siap Pakai Untuk Belajar FPGA." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Antarmuka Debug Dan Pemrograman (FPGA)." },
  { "en": "Apa Itu ASICs (Application-Specific Integrated Circuits)?", "id": "IC Kustom Didesain Untuk Tugas Spesifik." },
  { "en": "Apa Beda FPGA (Field-Programmable Gate Array) Dan ASIC?", "id": "FPGA (Diprogram Ulang), ASIC (Permanen)." },
  { "en": "Kapan Menggunakan ASIC (Application-Specific Integrated Circuits)?", "id": "Produksi Massal Volume Sangat Tinggi." },
  { "en": "Apa Itu FPAA (Field-Programmable Analog Array)?", "id": "IC Analog Yang Dapat Diprogram." },
  { "en": "Apa Itu Solder Mask (Masker Solder)?", "id": "Lapisan Pelindung PCB (Biasanya Hijau)." },
  { "en": "Fungsi Solder Mask (Masker Solder)?", "id": "Mencegah Jembatan Solder (Short)." },
  { "en": "Apa Itu Silkscreen (Sablon Sutra)?", "id": "Teks Putih Penanda Komponen Di PCB." },
  { "en": "Apa Itu Pad (PCB)?", "id": "Titik Solder Kaki Komponen." },
  { "en": "Apa Itu Trace (Jalur PCB)?", "id": "Garis Tembaga Penghubung Antar Pad." },
  { "en": "Apa Itu Via (PCB)?", "id": "Lubang Penghubung Jalur Antar Lapisan PCB." },
  { "en": "Apa Itu Annular Ring (Cincin Via)?", "id": "Area Tembaga Sekeliling Lubang Via." },
  { "en": "Apa Itu DRC (Design Rule Check)?", "id": "Pengecekan Aturan Desain PCB Otomatis." },
  { "en": "Apa Itu File Gerber?", "id": "Format File Standar Manufaktur PCB." },
  { "en": "Apa Itu File Bor (Drill File)?", "id": "File Data Lokasi Lubang Bor PCB." },
  { "en": "Apa Itu Bill Of Materials (BOM)?", "id": "Daftar Lengkap Komponen Rangkaian." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Fleksibel (Flex)?", "id": "PCB Yang Dapat Ditekuk." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Kaku-Fleksibel (Rigid-Flex)?", "id": "Gabungan PCB Kaku Dan Fleksibel." },
  { "en": "Apa Itu Bahan Substrat PCB?", "id": "Bahan Dasar Isolator PCB." },
  { "en": "Apa Itu FR-4?", "id": "Bahan Substrat PCB Paling Umum (Epoksi Kaca)." },
  { "en": "Apa Itu CEM-1?", "id": "Bahan Substrat PCB Murah (Kertas Epoksi)." },
  { "en": "Apa Itu Ketebalan Tembaga PCB?", "id": "Diukur Dalam Ounce (oz) Per Kaki Persegi." },
  { "en": "Apa Itu Ounce (oz) Tembaga (PCB)?", "id": "Standar Ketebalan Lapisan Tembaga." },
  { "en": "Apa Itu Etching (Etsa) PCB?", "id": "Proses Pembuangan Tembaga Yang Tidak Diinginkan." },
  { "en": "Bahan Etsa (Etching) PCB?", "id": "Ferri Klorida (Ferric Chloride)." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Buatan Rumah?", "id": "Metode Transfer Toner (Setrika)." },
  { "en": "Apa Itu Photolithography (PCB)?", "id": "Metode Pembuatan PCB (Cahaya UV)." },
  { "en": "Apa Itu Solder Resist (Tahan Solder)?", "id": "Nama Lain Solder Mask." },
  { "en": "Apa Itu HASL (Hot Air Solder Leveling)?", "id": "Finishing Permukaan PCB (Lapisan Timah)." },
  { "en": "Apa Itu ENIG (Electroless Nickel Immersion Gold)?", "id": "Finishing Permukaan PCB (Lapisan Emas)." },
  { "en": "Keuntungan ENIG (Electroless Nickel Immersion Gold)?", "id": "Permukaan Rata (Bagus Untuk SMD)." },
  { "en": "Apa Itu Panelisasi (Panelization)?", "id": "Menggabungkan Beberapa PCB Dalam Satu Papan." },
  { "en": "Apa Itu V-Scoring (V-Cut)?", "id": "Alur Potongan V Pemisah Panel PCB." },
  { "en": "Apa Itu Tab Routing (PCB)?", "id": "Metode Pemisah Panel (Lubang Kecil)." },
  { "en": "Apa Itu Uji Jarum Terbang (Flying Probe Test)?", "id": "Pengujian Elektrikal PCB Otomatis." },
  { "en": "Apa Itu Uji Ranjang Paku (Bed of Nails)?", "id": "Pengujian Elektrikal PCB (Fixture Khusus)." },
  { "en": "Apa Itu IPC (Institute for Printed Circuits)?", "id": "Organisasi Standar Industri PCB." },
  { "en": "Apa Itu RoHS (Restriction of Hazardous Substances)?", "id": "Direktif Pembatasan Bahan Berbahaya (Timbal)." },
  { "en": "Apa Itu WEEE (Waste Electrical Electronic Equipment)?", "id": "Direktif Penanganan Limbah Elektronik." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Bekerja Tanpa Gangguan." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Yang Dihasilkan Perangkat Elektronik." },
  { "en": "Apa Itu RFI (Radio Frequency Interference)?", "id": "EMI Dalam Spektrum Frekuensi Radio." },
  { "en": "Apa Itu Kandang Faraday (Faraday Cage)?", "id": "Pelindung Penghalang Medan Elektromagnetik." },
  { "en": "Apa Itu Pelindung (Shielding) Kabel?", "id": "Lapisan Logam (Foil) Pelindung Kabel." },
  { "en": "Apa Itu Ferit Bead (Ferrite Bead)?", "id": "Komponen Peredam Noise Frekuensi Tinggi." },
  { "en": "Apa Itu Kapasitor Bypass (Decoupling)?", "id": "Kapasitor Peredam Noise Di Jalur Daya." },
  { "en": "Di Mana Kapasitor Bypass Dipasang?", "id": "Sangat Dekat Dengan Pin Daya IC." },
  { "en": "Nilai Umum Kapasitor Bypass?", "id": "0.1 MikroFarad (100 NanoFarad)." },
  { "en": "Apa Itu Ground Plane Stitching?", "id": "Menghubungkan Area Ground Plane Dengan Via." },
  { "en": "Apa Itu Loop Area (Area Loop)?", "id": "Area Tertutup Jalur Sinyal (Rentan Noise)." },
  { "en": "Bagaimana Mengurangi Loop Area?", "id": "Menjaga Jalur Sinyal Ground Tetap Dekat." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak IC)." },
  { "en": "Bagaimana Mencegah ESD (Electrostatic Discharge)?", "id": "Gelang Anti-Statis, Alas Anti-Statis." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Gelang Penghubung Tubuh Ke Ground." },
  { "en": "Apa Itu Alas Anti-Statis (ESD Mat)?", "id": "Alas Meja Kerja Penghantar Listrik Statis." },
  { "en": "Apa Itu Kantong Anti-Statis?", "id": "Kantong Penyimpan Komponen Sensitif ESD." },
  { "en": "Apa Itu Ionizer (Anti-Statis)?", "id": "Alat Peniup Udara Terionisasi." },
  { "en": "Apa Itu Komponen Sensitif ESD?", "id": "MOSFET, CMOS IC, CPU." },
  { "en": "Apa Itu Kelas Kebersihan (Cleanroom)?", "id": "Standar Jumlah Partikel Debu Di Udara." },
  { "en": "Apa Itu Standar ISO 14644-1?", "id": "Standar Klasifikasi Kebersihan Udara." },
  { "en": "Apa Itu Pakaian Bunny Suit?", "id": "Pakaian Khusus Ruang Bersih (Cleanroom)." },
  { "en": "Apa Itu Filter HEPA (High-Efficiency Particulate Air)?", "id": "Filter Udara Efisiensi Sangat Tinggi." },
  { "en": "Apa Itu Filter ULPA (Ultra-Low Particulate Air)?", "id": "Filter Udara (Lebih Baik Dari HEPA)." },
  { "en": "Apa Itu Laminar Flow (Aliran Laminar)?", "id": "Aliran Udara Searah Tanpa Turbulensi." },
  { "en": "Apa Itu Air Shower (Mandi Udara)?", "id": "Ruang Peniup Udara (Masuk Cleanroom)." },
  { "en": "Apa Itu Wafer (Semikonduktor)?", "id": "Cakram Tipis Bahan Semikonduktor (Silikon)." },
  { "en": "Apa Itu Die (Semikonduktor)?", "id": "Satu Unit Chip IC Di Atas Wafer." },
  { "en": "Apa Itu Fab (Fabrikasi Semikonduktor)?", "id": "Pabrik Pembuatan Chip Semikonduktor." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit (Cahaya)." },
  { "en": "Apa Itu Photoresist (Resis Foto)?", "id": "Bahan Peka Cahaya Untuk Litografi." },
  { "en": "Apa Itu Mask (Litografi)?", "id": "Cetakan Pola Sirkuit (Kaca Kuarsa)." },
  { "en": "Apa Itu Stepper (Litografi)?", "id": "Mesin Proyeksi Pola Mask Ke Wafer." },
  { "en": "Apa Itu Doping (Semikonduktor)?", "id": "Proses Penambahan Unsur Asing Ke Silikon." },
  { "en": "Apa Itu Semikonduktor Intrinsik?", "id": "Semikonduktor Murni (Tanpa Doping)." },
  { "en": "Apa Itu Semikonduktor Ekstrinsik?", "id": "Semikonduktor Dengan Doping (Tipe N/P)." },
  { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Doping Kelebihan Elektron (Donor)." },
  { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Doping Kekurangan Elektron (Akseptor, Hole)." },
  { "en": "Apa Itu Hole (Lubang)?", "id": "Pembawa Muatan Positif (Kekosongan Elektron)." },
  { "en": "Apa Itu Persimpangan P-N (P-N Junction)?", "id": "Gabungan Semikonduktor Tipe P Dan N." },
  { "en": "Apa Komponen Dasar Persimpangan P-N?", "id": "Dioda Adalah Persimpangan P-N." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Area Kosong Muatan Di Persimpangan P-N." },
  { "en": "Apa Itu Forward Bias (Panjar Maju)?", "id": "Dioda Menghantar (Daerah Deplesi Sempit)." },
  { "en": "Apa Itu Reverse Bias (Panjar Mundur)?", "id": "Dioda Menghambat (Daerah Deplesi Lebar)." },
  { "en": "Apa Itu Tegangan Tembus (Breakdown)?", "id": "Tegangan Mundur Saat Dioda Bocor (Rusak)." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Komponen Penguat Berbasis Arus." },
  { "en": "Struktur Transistor BJT (Bipolar Junction Transistor)?", "id": "NPN Atau PNP." },
  { "en": "Kaki Transistor BJT (Bipolar Junction Transistor)?", "id": "Emitor, Basis, Kolektor." },
  { "en": "Fungsi Basis (BJT)?", "id": "Terminal Kontrol Arus Kolektor-Emitor." },
  { "en": "Apa Itu Beta (β) Atau HFE Transistor?", "id": "Faktor Penguatan Arus DC BJT." },
  { "en": "Apa Itu Transistor FET (Field Effect Transistor)?", "id": "Komponen Penguat Berbasis Tegangan." },
  { "en": "Kaki TransMasistor FET (Field Effect Transistor)?", "id": "Source, Gate, Drain." },
  { "en": "Apa Itu JFET (Junction Field Effect Transistor)?", "id": "FET Dengan Gerbang Persimpangan P-N." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET Dengan Gerbang Terisolasi Oksida." },
  { "en": "Kenapa MOSFET (Metal-Oxide-Semiconductor FET) Sensitif ESD?", "id": "Lapisan Oksida Gerbang Sangat Tipis." },
  { "en": "Apa Itu Efek Medan (Field Effect)?", "id": "Prinsip Kontrol Arus Menggunakan Medan Listrik." },
  { "en": "Apa Itu Baterai?", "id": "Sumber Energi Listrik Kimia (DC)." },
  { "en": "Apa Itu Sel Primer (Baterai)?", "id": "Baterai Sekali Pakai (Tidak Bisa Diisi)." },
  { "en": "Apa Itu Sel Sekunder (Baterai)?", "id": "Baterai Isi Ulang (Rechargeable)." },
  { "en": "Contoh Sel Primer?", "id": "Baterai Alkaline, Zinc-Carbon." },
  { "en": "Contoh Sel Sekunder?", "id": "Lithium-Ion, NiMH, Aki." },
  { "en": "Apa Itu Baterai Alkaline?", "id": "Baterai Primer (Tegangan 1.5 Volt)." },
  { "en": "Apa Itu Baterai Lithium-Ion (Li-Ion)?", "id": "Baterai Isi Ulang Populer (Kepadatan Tinggi)." },
  { "en": "Apa Tegangan Nominal Sel Li-Ion?", "id": "Sekitar 3.7 Volt." },
  { "en": "Apa Itu Baterai LiPo (Lithium-Polymer)?", "id": "Baterai Li-Ion (Elektrolit Polimer, Fleksibel)." },
  { "en": "Apa Itu Aki (Baterai) Asam Timbal?", "id": "Baterai Isi Ulang (Mobil, Motor, UPS)." },
  { "en": "Apa Itu Aki (Baterai) Basah?", "id": "Aki Asam Timbal (Perlu Isi Air)." },
  { "en": "Apa Itu Aki (Baterai) Kering (VRLA)?", "id": "Aki Asam Timbal (Bebas Perawatan)." },
  { "en": "Apa Itu Siklus Pengisian (Charge Cycle)?", "id": "Satu Siklus Penuh Pengisian Pengosongan." },
  { "en": "Apa Itu Kapasitas Baterai (Ah)?", "id": "Ukuran Penyimpanan Energi (Ampere-Hour)." },
  { "en": "Apa Itu Pengisian Cepat (Fast Charging)?", "id": "Mengisi Baterai Dengan Arus Tinggi." },
  { "en": "Apa Itu Pengisian Tetes (Trickle Charging)?", "id": "Pengisian Lambat (Menjaga Baterai Penuh)." },
  { "en": "Apa Itu Pengontrol Pengisian (Charge Controller)?", "id": "Sirkuit Pengatur Proses Pengisian Baterai." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem Perlindungan Baterai Lithium." },
  { "en": "Apa Itu Efek Memori (Baterai)?", "id": "Penurunan Kapasitas (Hanya Baterai NiCd)." },
  { "en": "Apa Itu Self-Discharge (Baterai)?", "id": "Baterai Kehilangan Daya Saat Disimpan." },
  { "en": "Apa Itu Pembangkit Listrik?", "id": "Instalasi Pembangkit Energi Listrik." },
  { "en": "Apa Itu PLTA (Pembangkit Listrik Tenaga Air)?", "id": "Pembangkit Listrik Energi Air (Turbin)." },
  { "en": "Apa Itu PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Pembangkit Listrik Energi Uap (Batu Bara)." },
  { "en": "Apa Itu PLTG (Pembangkit Listrik Tenaga Gas)?", "id": "Pembangkit Listrik Energi Gas Alam." },
  { "en": "Apa Itu PLTGU (Pembangkit Listrik Tenaga Gas Uap)?", "id": "Gabungan PLTG Dan PLTU (Efisiensi)." },
  { "en": "Apa Itu PLTD (Pembangkit Listrik Tenaga Diesel)?", "id": "Pembangkit Listrik Mesin Diesel (Generator)." },
  { "en": "Apa Itu PLTN (Pembangkit Listrik Tenaga Nuklir)?", "id": "Pembangkit Listrik Reaksi Fisi Nuklir." },
  { "en": "Apa Itu PLTS (Pembangkit Listrik Tenaga Surya)?", "id": "Pembangkit Listrik Energi Matahari (Fotovoltaik)." },
  { "en": "Apa Itu PLTB (Pembangkit Listrik Tenaga Bayu)?", "id": "Pembangkit Listrik Energi Angin (Kincir)." },
  { "en": "Apa Itu Energi Terbarukan?", "id": "Energi Dari Sumber Alam (Air, Angin)." },
  { "en": "Apa Itu Energi Tak Terbarukan?", "id": "Energi Dari Sumber Fosil (Batu Bara)." },
  { "en": "Apa Itu Transmisi (Listrik)?", "id": "Proses Penyaluran Listrik Jarak Jauh." },
  { "en": "Kenapa Transmisi (Listrik) Menggunakan Tegangan Tinggi?", "id": "Mengurangi Kerugian Daya (Rugi-Rugi)." },
  { "en": "Apa Itu SUTET (Saluran Udara Tegangan Ekstra Tinggi)?", "id": "Menara Transmisi Listrik Tegangan Tinggi." },
  { "en": "Apa Itu Trafo Step-Up (Transmisi)?", "id": "Menaikkan Tegangan Di Pembangkit." },
  { "en": "Apa Itu Trafo Step-Down (Distribusi)?", "id": "Menurunkan Tegangan Di Gardu Induk." },
  { "en": "Apa Itu Distribusi (Listrik)?", "id": "Proses Penyaluran Listrik Ke Pelanggan." },
  { "en": "Apa Itu Gardu Induk?", "id": "Pusat Penurunan Tegangan Distribusi." },
  { "en": "Apa Itu Jaringan Tegangan Menengah (JTM)?", "id": "Jaringan Distribusi (Contoh: 20 KV)." },
  { "en": "Apa Itu Jaringan Tegangan Rendah (JTR)?", "id": "Jaringan Ke Rumah (220V / 380V)." },
  { "en": "Apa Itu Trafo Distribusi?", "id": "Trafo Penurun Tegangan (20KV Ke 220V)." },
  { "en": "Apa Itu Sistem Tiga Fasa?", "id": "Sistem Listrik Tiga Kabel Fasa (Industri)." },
  { "en": "Apa Itu Sistem Satu Fasa?", "id": "Sistem Listrik Dua Kabel (Rumah Tangga)." },
  { "en": "Berapa Tegangan Satu Fasa Indonesia?", "id": "Dua Ratus Dua Puluh Volt (220V)." },
  { "en": "Berapa Tegangan Tiga Fasa Indonesia?", "id": "Tiga Ratus Delapan Puluh Volt (380V)." },
  { "en": "Apa Itu Koneksi Bintang (Star/Y)?", "id": "Koneksi Tiga Fasa (Ada Titik Netral)." },
  { "en": "Apa Itu Koneksi Delta (Segitiga)?", "id": "Koneksi Tiga Fasa (Tanpa Netral)." },
  { "en": "Apa Itu Tegangan Fasa-Netral?", "id": "Tegangan Antara Kabel Fasa Dan Netral." },
  { "en": "Apa Itu Tegangan Fasa-Fasa?", "id": "Tegangan Antara Dua Kabel Fasa." },
  { "en": "Apa Itu KWh Meter (Meteran Listrik)?", "id": "Alat Pengukur Konsumsi Energi Listrik." },
  { "en": "Apa Itu Listrik Prabayar (Token)?", "id": "Sistem Pembelian Listrik Di Muka." },
  { "en": "Apa Itu Listrik Pascabayar?", "id": "Sistem Pembayaran Listrik Setelah Pemakaian." },
  { "en": "Apa Itu MCB (Miniature Circuit Breaker)?", "id": "Pengaman Listrik Otomatis (Beban Lebih)." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pengaman Listrik (Putus Saat Arus Lebih)." },
  { "en": "Apa Beda MCB (Miniature Circuit Breaker) Dan Sekring?", "id": "MCB (Bisa Reset), Sekring (Sekali Pakai)." },
  { "en": "Apa Itu ELCB (Earth Leakage Circuit Breaker)?", "id": "Pengaman Arus Bocor (Anti Kejut Listrik)." },
  { "en": "Apa Itu Grounding (Pentanahan) Rumah?", "id": "Kabel Pengaman (Ditanam Ke Bumi)." },
  { "en": "Fungsi Grounding (Pentanahan) Rumah?", "id": "Mencegah Kejut Listrik (Arus Bocor)." },
  { "en": "Apa Itu Instalasi Listrik?", "id": "Sistem Pengkabelan Listrik Di Bangunan." },
  { "en": "Apa Itu Stop Kontak (Outlet)?", "id": "Titik Sambungan Listrik Di Dinding." },
  { "en": "Apa Itu Steker (Plug)?", "id": "Colokan Listrik (Kaki Dua Atau Tiga)." },
  { "en": "Apa Itu Saklar (Switch)?", "id": "Pemutus Penyambung Aliran Listrik (Lampu)." },
  { "en": "Apa Itu Saklar Tunggal?", "id": "Saklar Untuk Satu Lampu." },
  { "en": "Apa Itu Saklar Ganda (Seri)?", "id": "Dua Saklar Dalam Satu Plat." },
  { "en": "Apa Itu Saklar Tukar (Hotel)?", "id": "Saklar (Menyalakan Dari Dua Tempat)." },
  { "en": "Apa Itu Fitting (Lampu)?", "id": "Rumah Dudukan Bohlam Lampu." },
  { "en": "Apa Itu Kabel NYM?", "id": "Kabel Instalasi Rumah (Putih, Isi Kaku)." },
  { "en": "Apa Itu Kabel NYY?", "id": "Kabel Instalasi Luar Ruangan (Hitam, Tahan Air)." },
  { "en": "Apa Itu Kabel NYA?", "id": "Kabel Instalasi (Satu Inti, Perlu Pipa)." },
  { "en": "Apa Itu Pipa Konduit?", "id": "Pipa Pelindung Kabel Instalasi Listrik." },
  { "en": "Apa Itu T-Dus (Tee Dus)?", "id": "Kotak Sambungan Kabel Di Plafon." },
  { "en": "Apa Itu Lasdop (Penutup Sambungan)?", "id": "Penutup Isolasi Ujung Sambungan Kabel." },
  { "en": "Apa Itu Tang Kombinasi?", "id": "Tang Serbaguna (Jepit, Potong, Putar)." },
  { "en": "Apa Itu Tang Potong?", "id": "Tang Khusus Untuk Memotong Kawat." },
  { "en": "Apa Itu Tang Lancip (Long Nose)?", "id": "Tang Penjepit Ujung Runcing." },
  { "en": "Apa Itu Tang Kupas Kabel?", "id": "Tang Pengupas Isolasi Kabel Otomatis." },
  { "en": "Apa Itu Tespen (Test Pen)?", "id": "Alat Cek Fasa Listrik AC." },
  { "en": "Apa Itu Multimeter (Avometer)?", "id": "Alat Ukur Listrik (Volt, Ampere, Ohm)." },
  { "en": "Apa Itu Clamp Meter (Tang Ampere)?", "id": "Alat Ukur Arus Tanpa Memutus Kabel." },
  { "en": "Bagaimana Clamp Meter (Tang Ampere) Bekerja?", "id": "Mengukur Medan Magnet Sekitar Kabel." },
  { "en": "Apa Itu Insulation Tester (Megger)?", "id": "Alat Ukur Resistansi Isolasi Kabel." },
  { "en": "Apa Itu Earth Tester (Tester Bumi)?", "id": "Alat Ukur Resistansi Pentanahan." },
  { "en": "Apa Itu Phase Sequence Tester?", "id": "Alat Pengecek Urutan Fasa (R-S-T)." },
  { "en": "Apa Itu Lux Meter?", "id": "Alat Ukur Intensitas Cahaya (Penerangan)." },
  { "en": "Apa Itu Sound Level Meter?", "id": "Alat Ukur Tingkat Kebisingan Suara (dB)." },
  { "en": "Apa Itu Anemometer?", "id": "Alat Ukur Kecepatan Angin." },
  { "en": "Apa Itu Hygrometer (Higrometer)?", "id": "Alat Ukur Kelembaban Udara (Persen)." },
  { "en": "Apa Itu Termometer?", "id": "Alat Ukur Suhu (Derajat Celcius)." },
  { "en": "Apa Itu Termometer Inframerah?", "id": "Pengukur Suhu Jarak Jauh (Tanpa Sentuh)." },
  { "en": "Apa Itu Kamera Termal?", "id": "Kamera Penglihat Suhu (Distribusi Panas)." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Ukur Visual Bentuk Sinyal Listrik." },
  { "en": "Apa Itu Sumbu Horizontal Osiloskop?", "id": "Menunjukkan Waktu (Time/Division)." },
  { "en": "Apa Itu Sumbu Vertikal Osiloskop?", "id": "Menunjukkan Tegangan (Volt/Division)." },
  { "en": "Apa Itu Probe (Osiloskop)?", "id": "Kabel Input Penghubung Rangkaian." },
  { "en": "Apa Itu Generator Sinyal (Signal Generator)?", "id": "Alat Penghasil Sinyal Uji (Sinus, Kotak)." },
  { "en": "Apa Itu LCR Meter?", "id": "Alat Ukur Induktansi, Kapasitansi, Resistansi." },
  { "en": "Apa Itu Penganalisa Spektrum (Spectrum Analyzer)?", "id": "Alat Ukur Sinyal Di Domain Frekuensi." },
  { "en": "Apa Itu Logic Analyzer (Penganalisa Logika)?", "id": "Alat Ukur Sinyal Digital (Banyak Kanal)." },
  { "en": "Apa Itu Catu Daya DC (DC Power Supply)?", "id": "Sumber Tegangan DC Variabel (Laboratorium)." },
  { "en": "Apa Itu Beban Elektronik (Electronic Load)?", "id": "Alat Simulasi Beban Listrik (Tes)." },
  { "en": "Apa Itu Fisika Dasar?", "id": "Ilmu Tentang Materi Dan Energi." },
  { "en": "Apa Itu Materi (Matter)?", "id": "Segala Sesuatu Yang Memiliki Massa Ruang." },
  { "en": "Apa Itu Atom?", "id": "Bagian Terkecil Suatu Unsur." },
  { "en": "Apa Saja Bagian Atom?", "id": "Proton, Neutron, Dan Elektron." },
  { "en": "Apa Itu Proton?", "id": "Partikel Subatomik Muatan Positif (Di Inti)." },
  { "en": "Apa Itu Neutron?", "id": "Partikel Subatomik Muatan Netral (Di Inti)." },
  { "en": "Apa Itu Elektron?", "id": "Partikel Subatomik Muatan Negatif (Mengorbit)." },
  { "en": "Apa Itu Inti Atom (Nukleus)?", "id": "Pusat Atom (Proton Dan Neutron)." },
  { "en": "Kenapa Atom Netral Secara Listrik?", "id": "Jumlah Proton Sama Dengan Elektron." },
  { "en": "Apa Itu Ion?", "id": "Atom Yang Kehilangan Mendapat Elektron." },
  { "en": "Apa Itu Ion Positif (Kation)?", "id": "Atom Yang Kehilangan Elektron." },
  { "en": "Apa Itu Ion Negatif (Anion)?", "id": "Atom Yang Mendapat Elektron." },
  { "en": "Apa Itu Muatan Listrik?", "id": "Sifat Dasar Partikel (Positif/Negatif)." },
  { "en": "Apa Satuan Muatan Listrik?", "id": "Coulomb (C)." },
  { "en": "Apa Itu Hukum Coulomb?", "id": "Gaya Tarik Menarik Tolak Menolak Muatan." },
  { "en": "Apa Itu Medan Listrik?", "id": "Area Sekitar Muatan (Memengaruhi Muatan Lain)." },
  { "en": "Apa Itu Potensial Listrik?", "id": "Energi Potensial Per Satuan Muatan." },
  { "en": "Apa Satuan Potensial Listrik?", "id": "Volt (V)." },
  { "en": "Apa Itu Tegangan (Beda Potensial)?", "id": "Perbedaan Potensial Listrik Dua Titik." },
  { "en": "Apa Itu Arus Listrik?", "id": "Aliran Muatan Listrik (Elektron)." },
  { "en": "Apa Satuan Arus Listrik?", "id": "Ampere (A)." },
  { "en": "Apa Arah Arus Konvensional?", "id": "Aliran Muatan Positif (Berlawanan Elektron)." },
  { "en": "Apa Arah Aliran Elektron?", "id": "Dari Negatif Ke Positif." },
  { "en": "Apa Itu Hambatan (Resistansi)?", "id": "Penghalang Aliran Arus Listrik." },
  { "en": "Apa Satuan Hambatan (Resistansi)?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Hukum Ohm?", "id": "Hubungan Tegangan, Arus, Hambatan (V=IR)." },
  { "en": "Apa Itu Konduktor?", "id": "Bahan Mudah Menghantarkan Listrik (Logam)." },
  { "en": "Kenapa Konduktor Mudah Menghantar?", "id": "Memiliki Banyak Elektron Bebas." },
  { "en": "Apa Itu Isolator?", "id": "Bahan Sulit Menghantarkan Listrik (Karet)." },
  { "en": "Kenapa Isolator Sulit Menghantar?", "id": "Elektron Terikat Kuat Di Atom." },
  { "en": "Apa Itu Semikonduktor?", "id": "Bahan Antara Konduktor Dan Isolator." },
  { "en": "Contoh Bahan Semikonduktor?", "id": "Silikon (Si), Germanium (Ge)." },
  { "en": "Apa Itu Konduktivitas?", "id": "Kemampuan Bahan Menghantarkan Listrik." },
  { "en": "Apa Itu Resistivitas?", "id": "Kemampuan Bahan Menghambat Listrik." },
  { "en": "Apa Itu Daya Listrik?", "id": "Laju Energi Listrik (P = V * I)." },
  { "en": "Apa Satuan Daya Listrik?", "id": "Watt (W)." },
  { "en": "Apa Itu Energi Listrik?", "id": "Daya Listrik Dikali Waktu (P * t)." },
  { "en": "Apa Satuan Energi Listrik?", "id": "Joule (J) Atau Watt-Hour (Wh)." },
  { "en": "Apa Itu KiloWatt-Hour (kWh)?", "id": "Satuan Energi Listrik (PLN)." },
  { "en": "Apa Itu Rangkaian Seri?", "id": "Komponen Terhubung Satu Jalur." },
  { "en": "Sifat Rangkaian Seri?", "id": "Arus Sama, Tegangan Terbagi." },
  { "en": "Apa Itu Rangkaian Paralel?", "id": "Komponen Terhubung Bercabang." },
  { "en": "Sifat Rangkaian Paralel?", "id": "Tegangan Sama, Arus Terbagi." },
  { "en": "Apa Itu Hukum Kirchhoff I (Arus)?", "id": "Jumlah Arus Masuk Sama Dengan Keluar." },
  { "en": "Apa Itu Hukum Kirchhoff II (Tegangan)?", "id": "Jumlah Tegangan Dalam Loop Tertutup Nol." },
  { "en": "Apa Itu Magnet?", "id": "Benda Penghasil Medan Magnet." },
  { "en": "Apa Itu Kutub Magnet?", "id": "Kutub Utara Dan Kutub Selatan." },
  { "en": "Apa Itu Medan Magnet?", "id": "Area Sekitar Magnet (Memengaruhi Magnet Lain)." },
  { "en": "Apa Itu Elektromagnet?", "id": "Magnet Yang Dihasilkan Arus Listrik." },
  { "en": "Bagaimana Membuat Elektromagnet?", "id": "Melilitkan Kawat (Kumparan) Pada Inti Besi." },
  { "en": "Apa Itu Induksi Elektromagnetik?", "id": "Medan Magnet Berubah Menghasilkan Listrik." },
  { "en": "Siapa Penemu Induksi Elektromagnetik?", "id": "Michael Faraday." },
  { "en": "Apa Itu Hukum Faraday?", "id": "GGL Induksi Sebanding Perubahan Fluks Magnet." },
  { "en": "Apa Itu Hukum Lenz?", "id": "Arah Arus Induksi Melawan Perubahan." },
  { "en": "Apa Prinsip Kerja Generator?", "id": "Induksi Elektromagnetik (Gerak Jadi Listrik)." },
  { "en": "Apa Prinsip Kerja Motor?", "id": "Gaya Lorentz (Listrik Jadi Gerak)." },
  { "en": "Apa Itu Gaya Lorentz?", "id": "Gaya Pada Kawat Berarus Di Medan Magnet." },
  { "en": "Apa Itu Fluks Magnet?", "id": "Jumlah Garis Gaya Magnet." },
  { "en": "Apa Itu Gelombang?", "id": "Getaran Yang Merambat." },
  { "en": "Apa Itu Gelombang Mekanik?", "id": "Gelombang Butuh Medium (Suara, Air)." },
  { "en": "Apa Itu Gelombang Elektromagnetik?", "id": "Gelombang Tak Butuh Medium (Cahaya)." },
  { "en": "Apa Itu Spektrum Elektromagnetik?", "id": "Rentang Semua Gelombang Elektromagnetik." },
  { "en": "Urutan Spektrum Elektromagnetik?", "id": "Radio, Mikro, Inframerah, Cahaya, UV, X, Gamma." },
  { "en": "Apa Itu Frekuensi (Gelombang)?", "id": "Jumlah Getaran Per Detik (Hertz)." },
  { "en": "Apa Itu Panjang Gelombang (Lambda)?", "id": "Jarak Satu Siklus Gelombang." },
  { "en": "Apa Itu Kecepatan Cahaya (c)?", "id": "Kecepatan Gelombang Elektromagnetik Di Vakum." },
  { "en": "Apa Itu Amplitudo (Gelombang)?", "id": "Simpangan Maksimum Getaran Gelombang." },
  { "en": "Apa Itu Cahaya Tampak?", "id": "Bagian Spektrum (Terlihat Mata Manusia)." },
  { "en": "Urutan Warna Cahaya Tampak?", "id": "Merah, Jingga, Kuning, Hijau, Biru, Nila, Ungu." },
  { "en": "Apa Itu Inframerah (Infrared)?", "id": "Gelombang Elektromagnetik (Radiasi Panas)." },
  { "en": "Apa Itu Ultraviolet (UV)?", "id": "Gelombang Elektromagnetik (Sinar Matahari)." },
  { "en": "Apa Itu Sinar-X (X-Ray)?", "id": "Gelombang Elektromagnetik (Tembus Benda)." },
  { "en": "Apa Itu Sinar Gamma?", "id": "Gelombang Elektromagnetik (Energi Tertinggi, Radioaktif)." },
  { "en": "Apa Itu Optik (Optics)?", "id": "Cabang Fisika Tentang Cahaya." },
  { "en": "Apa Itu Refleksi (Pemantulan) Cahaya?", "id": "Cahaya Memantul Kembali (Cermin)." },
  { "en": "Apa Itu Refraksi (Pembiasan) Cahaya?", "id": "Cahaya Berbelok Saat Pindah Medium." },
  { "en": "Apa Itu Lensa (Lens)?", "id": "Benda Bening Pembias Cahaya." },
  { "en": "Apa Itu Lensa Cembung (Konveks)?", "id": "Lensa Pengumpul Cahaya (Positif)." },
  { "en": "Apa Itu Lensa Cekung (Konkav)?", "id": "Lensa Penyebar Cahaya (Negatif)." },
  { "en": "Apa Itu Difraksi (Lenturan) Cahaya?", "id": "Cahaya Melengkung Di Celah Sempit." },
  { "en": "Apa Itu Interferensi (Cahaya)?", "id": "Gabungan Dua Gelombang Cahaya." },
  { "en": "Apa Itu Polarisasi (Cahaya)?", "id": "Penyaringan Arah Getar Gelombang Cahaya." },
  { "en": "Apa Itu Serat Optik?", "id": "Kabel Transmisi Cahaya (Refleksi Internal)." },
  { "en": "Apa Itu Akustik (Acoustics)?", "id": "Cabang Fisika Tentang Suara." },
  { "en": "Apa Itu Suara (Sound)?", "id": "Gelombang Mekanik (Getaran Medium)." },
  { "en": "Apa Itu Nada (Pitch)?", "id": "Persepsi Frekuensi Suara (Tinggi/Rendah)." },
  { "en": "Apa Itu Intensitas (Volume) Suara?", "id": "Persepsi Amplitudo Suara (Keras/Pelan)." },
  { "en": "Apa Satuan Intensitas Suara?", "id": "Desibel (dB)." },
  { "en": "Apa Itu Efek Doppler?", "id": "Perubahan Frekuensi Akibat Gerak Sumber." },
  { "en": "Contoh Efek Doppler?", "id": "Suara Sirine Ambulans Berubah." },
  { "en": "Apa Itu Termodinamika?", "id": "Cabang Fisika Tentang Panas Energi." },
  { "en": "Apa Itu Suhu (Temperatur)?", "id": "Ukuran Derajat Panas Dingin Benda." },
  { "en": "Apa Satuan Suhu (Temperatur)?", "id": "Celcius (°C), Kelvin (K), Fahrenheit (°F)." },
  { "en": "Apa Itu Nol Absolut?", "id": "Suhu Terendah Mungkin (Nol Kelvin)." },
  { "en": "Apa Itu Kalor (Panas)?", "id": "Energi Yang Berpindah Akibat Beda Suhu." },
  { "en": "Apa Satuan Kalor (Panas)?", "id": "Joule (J) Atau Kalori (cal)." },
  { "en": "Apa Itu Konduksi (Panas)?", "id": "Perpindahan Panas Lewat Zat Padat." },
  { "en": "Apa Itu Konveksi (Panas)?", "id": "Perpindahan Panas Lewat Aliran Fluida." },
  { "en": "Apa Itu Radiasi (Panas)?", "id": "Perpindahan Panas Lewat Gelombang Inframerah." },
  { "en": "Apa Itu Hukum Pertama Termodinamika?", "id": "Hukum Kekekalan Energi (Energi Internal)." },
  { "en": "Apa Itu Hukum Kedua Termodinamika?", "id": "Panas Mengalir Dari Panas Ke Dingin." },
  { "en": "Apa Itu Entropi?", "id": "Ukuran Ketidakteraturan Sistem." },
  { "en": "Apa Itu Mekanika?", "id": "Cabang Fisika Tentang Gerak." },
  { "en": "Apa Itu Kinematika?", "id": "Ilmu Gerak (Tanpa Penyebab)." },
  { "en": "Apa Itu Dinamika?", "id": "Ilmu Gerak (Dengan Penyebab Gaya)." },
  { "en": "Apa Itu Jarak (Distance)?", "id": "Total Lintasan Yang Ditempuh." },
  { "en": "Apa Itu Perpindahan (Displacement)?", "id": "Perubahan Posisi (Besaran Vektor)." },
  { "en": "Apa Itu Kecepatan (Velocity)?", "id": "Perpindahan Per Satuan Waktu (Vektor)." },
  { "en": "Apa Itu Kelajuan (Speed)?", "id": "Jarak Per Satuan Waktu (Skalar)." },
  { "en": "Apa Itu Percepatan (Acceleration)?", "id": "Perubahan Kecepatan Per Satuan Waktu." },
  { "en": "Apa Itu Hukum Pertama Newton?", "id": "Hukum Kelembaman (Inersia)." },
  { "en": "Apa Itu Hukum Kedua Newton?", "id": "Gaya Sama Dengan Massa Kali Percepatan." },
  { "en": "Apa Rumus Hukum Kedua Newton?", "id": "F = m x a." },
  { "en": "Apa Itu Hukum Ketiga Newton?", "id": "Hukum Aksi Reaksi." },
  { "en": "Apa Itu Gaya (Force)?", "id": "Tarikan Atau Dorongan." },
  { "en": "Apa Satuan Gaya (Force)?", "id": "Newton (N)." },
  { "en": "Apa Itu Massa (Mass)?", "id": "Ukuran Kelembaman Benda (Kilogram)." },
  { "en": "Apa Itu Berat (Weight)?", "id": "Gaya Gravitasi Pada Benda (Newton)." },
  { "en": "Apa Beda Massa Dan Berat?", "id": "Massa Tetap, Berat Tergantung Gravitasi." },
  { "en": "Apa Itu Gaya Gravitasi?", "id": "Gaya Tarik Antar Benda Bermassa." },
  { "en": "Apa Itu Gaya Gesek?", "id": "Gaya Penghambat Gerak Antar Permukaan." },
  { "en": "Apa Itu Gaya Sentripetal?", "id": "Gaya Menuju Pusat (Gerak Melingkar)." },
  { "en": "Apa Itu Momentum?", "id": "Ukuran Kesukaran Menghentikan Benda." },
  { "en": "Apa Rumus Momentum?", "id": "Momentum = Massa x Kecepatan." },
  { "en": "Apa Itu Impuls?", "id": "Perubahan Momentum (Gaya Kali Waktu)." },
  { "en": "Apa Itu Hukum Kekekalan Momentum?", "id": "Momentum Total Sistem Tetap (Jika Terisolasi)." },
  { "en": "Apa Itu Energi Kinetik?", "id": "Energi Benda Karena Geraknya." },
  { "en": "Apa Rumus Energi Kinetik?", "id": "EK = ½ x m x v²." },
  { "en": "Apa Itu Energi Potensial?", "id": "Energi Benda Karena Posisinya (Ketinggian)." },
  { "en": "Apa Rumus Energi Potensial Gravitasi?", "id": "EP = m x g x h." },
  { "en": "Apa Itu Energi Mekanik?", "id": "Jumlah Energi Kinetik Dan Potensial." },
  { "en": "Apa Itu Hukum Kekekalan Energi Mekanik?", "id": "Energi Mekanik Total Sistem Tetap." },
  { "en": "Apa Itu Usaha (Work)?", "id": "Gaya Kali Perpindahan." },
  { "en": "Apa Satuan Usaha (Work)?", "id": "Joule (J)." },
  { "en": "Apa Itu Daya (Power)?", "id": "Usaha Per Satuan Waktu." },
  { "en": "Apa Satuan Daya (Power)?", "id": "Watt (W)." },
  { "en": "Apa Itu Fluida?", "id": "Zat Yang Dapat Mengalir (Cair, Gas)." },
  { "en": "Apa Itu Tekanan (Pressure)?", "id": "Gaya Per Satuan Luas." },
  { "en": "Apa Satuan Tekanan (Pressure)?", "id": "Pascal (Pa) Atau Atmosfer (atm)." },
  { "en": "Apa Itu Hukum Pascal?", "id": "Tekanan Cairan Dirambatkan Sama Rata." },
  { "en": "Apa Itu Hukum Archimedes?", "id": "Gaya Apung Benda Tercelup." },
  { "en": "Apa Itu Massa Jenis (Density)?", "id": "Massa Per Satuan Volume." },
  { "en": "Apa Itu Tegangan Permukaan?", "id": "Kecenderungan Permukaan Cairan Menegang." },
  { "en": "Apa Itu Kapilaritas?", "id": "Naik Turunnya Cairan Di Pipa Sempit." },
  { "en": "Apa Itu Viskositas?", "id": "Ukuran Kekentalan Fluida." },
  { "en": "Apa Itu Aliran Laminar?", "id": "Aliran Fluida Mulus Teratur." },
  { "en": "Apa Itu Aliran Turbulen?", "id": "Aliran Fluida Kacau Berputar." },
  { "en": "Apa Itu Persamaan Bernoulli?", "id": "Hubungan Tekanan Kecepatan Ketinggian Fluida." },
  { "en": "Apa Itu Fisika Modern?", "id": "Fisika Abad 20 (Relativitas, Kuantum)." },
  { "en": "Apa Itu Teori Relativitas Khusus?", "id": "Teori Einstein (Kecepatan Cahaya Konstan)." },
  { "en": "Apa Itu Dilatasi Waktu?", "id": "Waktu Bergerak Lebih Lambat (Relativitas)." },
  { "en": "Apa Itu Kontraksi Panjang?", "id": "Benda Memendek Searah Gerak (Relativitas)." },
  { "en": "Apa Rumus Kesetaraan Massa-Energi?", "id": "E = mc²." },
  { "en": "Apa Itu Teori Relativitas Umum?", "id": "Teori Einstein Tentang Gravitasi." },
  { "en": "Apa Itu Gravitasi (Relativitas Umum)?", "id": "Kelengkungan Ruang-Waktu Akibat Massa." },
  { "en": "Apa Itu Lubang Hitam (Black Hole)?", "id": "Objek Gravitasi Sangat Kuat." },
  { "en": "Apa Itu Fisika Kuantum?", "id": "Fisika Benda Sangat Kecil (Atom)." },
  { "en": "Apa Itu Kuantisasi Energi?", "id": "Energi Hanya Ada Dalam Paket Diskrit." },
  { "en": "Apa Itu Foton (Photon)?", "id": "Partikel Kuantum Cahaya." },
  { "en": "Apa Itu Efek Fotolistrik?", "id": "Cahaya Melepas Elektron Dari Logam." },
  { "en": "Apa Itu Dualitas Gelombang-Partikel?", "id": "Materi Bersifat Gelombang Dan Partikel." },
  { "en": "Apa Itu Prinsip Ketidakpastian Heisenberg?", "id": "Posisi Momentum Tak Bisa Diukur Pasti." },
  { "en": "Apa Itu Model Atom Bohr?", "id": "Elektron Mengorbit Inti Di Lintasan Tetap." },
  { "en": "Apa Itu Model Atom Kuantum?", "id": "Elektron Di Awan Probabilitas (Orbital)." },
  { "en": "Apa Itu Radioaktivitas?", "id": "Peluruhan Inti Atom Tidak Stabil." },
  { "en": "Apa Itu Sinar Alfa?", "id": "Radiasi Partikel Helium (Positif)." },
  { "en": "Apa Itu Sinar Beta?", "id": "Radiasi Partikel Elektron (Negatif)." },
  { "en": "Apa Itu Sinar Gamma?", "id": "Radiasi Elektromagnetik Energi Tinggi." },
  { "en": "Apa Itu Waktu Paruh (Half-Life)?", "id": "Waktu Separuh Zat Meluruh." },
  { "en": "Apa Itu Reaksi Fisi (Nuklir)?", "id": "Pembelahan Inti Atom Berat." },
  { "en": "Apa Itu Reaksi Fusi (Nuklir)?", "id": "Penggabungan Inti Atom Ringan." },
  { "en": "Di Mana Reaksi Fusi Terjadi?", "id": "Di Inti Matahari." },
  { "en": "Apa Itu Bom Atom?", "id": "Senjata Peledak Berbasis Reaksi Fisi." },
  { "en": "Apa Itu Bom Hidrogen?", "id": "Senjata Peledak Berbasis Reaksi Fusi." },
  { "en": "Apa Itu Reaktor Nuklir?", "id": "Tempat Reaksi Fisi Nuklir Terkendali." },
  { "en": "Apa Itu Batang Kendali (Reaktor)?", "id": "Penyerap Neutron (Pengendali Reaksi)." },
  { "en": "Apa Itu Moderator (Reaktor)?", "id": "Bahan Perlambat Neutron (Contoh: Air)." },
  { "en": "Apa Itu Pendingin (Reaktor)?", "id": "Cairan Pengambil Panas Dari Reaktor." },
  { "en": "Apa Itu Uranium Diperkaya?", "id": "Uranium Dengan Konsentrasi U-235 Tinggi." },
  { "en": "Apa Itu Limbah Nuklir?", "id": "Sisa Bahan Bakar Radioaktif Berbahaya." },
  { "en": "Apa Itu CERN (Organisasi Eropa Riset Nuklir)?", "id": "Laboratorium Fisika Partikel Terbesar." },
  { "en": "Apa Itu LHC (Large Hadron Collider)?", "id": "Penumbuk Partikel Raksasa Di CERN." },
  { "en": "Apa Itu Model Standar (Fisika)?", "id": "Teori Partikel Elementer Gaya Dasar." },
  { "en": "Apa Itu Kuark (Quark)?", "id": "Partikel Elementer Pembentuk Proton Neutron." },
  { "en": "Apa Itu Lepton?", "id": "Partikel Elementer (Contoh: Elektron)." },
  { "en": "Apa Itu Boson Higgs?", "id": "Partikel Yang Memberi Massa Partikel Lain." },
  { "en": "Apa Itu Antimateri?", "id": "Materi Dengan Muatan Berlawanan." },
  { "en": "Apa Itu Positron?", "id": "Antimateri Dari Elektron (Muatan Positif)." },
  { "en": "Apa Itu Anihilasi (Pemusnahan)?", "id": "Materi Antimateri Bertemu (Jadi Energi)." },
  { "en": "Apa Itu Teori String (String Theory)?", "id": "Teori Partikel Adalah Getaran String." },
  { "en": "Apa Itu Teori Big Bang (Ledakan Dahsyat)?", "id": "Teori Awal Mula Alam Semesta." },
  { "en": "Apa Itu Latar Belakang Gelombang Mikro Kosmik?", "id": "Sisa Radiasi Panas Big Bang." },
  { "en": "Apa Itu Energi Gelap (Dark Energy)?", "id": "Energi Misterius (Percepatan Ekspansi Alam)." },
  { "en": "Apa Itu Materi Gelap (Dark Matter)?", "id": "Materi Tak Terlihat (Efek Gravitasi)." },
  { "en": "Apa Itu Astrofisika?", "id": "Penerapan Fisika Pada Benda Langit." },
  { "en": "Apa Itu Astronomi?", "id": "Ilmu Observasi Benda Langit." },
  { "en": "Apa Itu Teleskop?", "id": "Alat Optik Pengamat Benda Jauh." },
  { "en": "Apa Itu Teleskop Refraktor (Pembias)?", "id": "Teleskop Menggunakan Lensa." },
  { "en": "Apa Itu Teleskop Reflektor (Pemantul)?", "id": "Teleskop Menggunakan Cermin." },
  { "en": "Apa Itu Teleskop Radio?", "id": "Teleskop Penangkap Gelombang Radio Kosmik." },
  { "en": "Apa Itu Teleskop Luar Angkasa?", "id": "Teleskop Di Orbit (Contoh: Hubble)." },
  { "en": "Apa Itu Planet?", "id": "Benda Langit Pengorbit Bintang." },
  { "en": "Apa Itu Bintang (Star)?", "id": "Benda Langit Penghasil Cahaya (Fusi)." },
  { "en": "Apa Itu Tata Surya (Solar System)?", "id": "Matahari Dan Sistem Planetnya." },
  { "en": "Apa Itu Galaksi?", "id": "Kumpulan Miliaran Bintang (Contoh: Bima Sakti)." },
  { "en": "Apa Itu Satelit (Alami)?", "id": "Benda Langit Pengorbit Planet (Bulan)." },
  { "en": "Apa Itu Satelit (Buatan)?", "id": "Mesin Pengorbit Bumi (Komunikasi, GPS)." },
  { "en": "Apa Itu Orbit (Orbit)?", "id": "Jalur Lintasan Benda Mengelilingi Benda Lain." },
  { "en": "Apa Itu Orbit LEO (Low Earth Orbit)?", "id": "Orbit Rendah Bumi (Stasiun Luar Angkasa)." },
  { "en": "Apa Itu Orbit MEO (Medium Earth Orbit)?", "id": "Orbit Menengah Bumi (Satelit GPS)." },
  { "en": "Apa Itu Orbit GEO (Geostationary Orbit)?", "id": "Orbit Geostasioner (Satelit Komunikasi)." },
  { "en": "Kenapa Satelit GEO (Geostationary Orbit) Terlihat Diam?", "id": "Kecepatan Orbit Sama Dengan Rotasi Bumi." },
  { "en": "Apa Itu Komet (Bintang Berekor)?", "id": "Benda Es Gas Debu (Ekor)." },
  { "en": "Apa Itu Asteroid (Planetoid)?", "id": "Benda Batuan Kecil Pengorbit Matahari." },
  { "en": "Apa Itu Meteoroid?", "id": "Batuan Kecil Di Luar Angkasa." },
  { "en": "Apa Itu Meteor (Bintang Jatuh)?", "id": "Meteoroid Yang Terbakar Di Atmosfer." },
  { "en": "Apa Itu Meteorit?", "id": "Sisa Meteor Yang Sampai Ke Bumi." },
  { "en": "Apa Itu Roket?", "id": "Kendaraan Peluncur Muatan Ke Luar Angkasa." },
  { "en": "Apa Itu Propulsi (Propulsion)?", "id": "Sistem Pendorong (Mesin Roket)." },
  { "en": "Apa Itu Bahan Bakar Roket?", "id": "Bahan Kimia (Bahan Bakar Oksidator)." },
  { "en": "Apa Itu Oksidator (Roket)?", "id": "Penyuplai Oksigen Pembakaran (Contoh: Oksigen Cair)." },
  { "en": "Apa Itu Roket Bahan Bakar Padat?", "id": "Bahan Bakar Oksidator Tercampur Padat." },
  { "en": "Apa Itu Roket Bahan Bakar Cair?", "id": "Bahan Bakar Oksidator Cair Terpisah." },
  { "en": "Apa Itu ISS (International Space Station)?", "id": "Stasiun Luar Angkasa Internasional." },
  { "en": "Apa Itu Astronaut (Kosmonot)?", "id": "Orang Yang Pergi Ke Luar Angkasa." },
  { "en": "Apa Itu Baju Luar Angkasa?", "id": "Pakaian Pelindung Astronaut Di Luar Angkasa." },
  { "en": "Apa Itu Kondisi Tanpa Bobot (Weightlessness)?", "id": "Kondisi Jatuh Bebas (Di Orbit)." },
  { "en": "Apa Itu Gravitasi Mikro (Microgravity)?", "id": "Nama Lain Kondisi Tanpa Bobot." },
  { "en": "Apa Itu Bahan Material?", "id": "Ilmu Tentang Sifat Struktur Material." },
  { "en": "Apa Itu Logam (Metal)?", "id": "Material Konduktor Listrik Panas (Padat)." },
  { "en": "Apa Itu Logam Mulia?", "id": "Logam Tahan Korosi (Emas, Perak, Platina)." },
  { "en": "Apa Itu Logam Paduan (Alloy)?", "id": "Campuran Dua Atau Lebih Logam." },
  { "en": "Contoh Logam Paduan (Alloy)?", "id": "Baja (Besi Karbon), Kuningan (Tembaga Seng)." },
  { "en": "Apa Itu Keramik (Ceramic)?", "id": "Material Anorganik Non-Logam (Isolator)." },
  { "en": "Contoh Keramik (Ceramic)?", "id": "Kaca, Porselen, Semen." },
  { "en": "Apa Itu Polimer (Polymer)?", "id": "Material Rantai Molekul Panjang (Plastik)." },
  { "en": "Contoh Polimer (Polymer)?", "id": "Polietilena (PE), PVC, Nilon." },
  { "en": "Apa Itu Termoplastik?", "id": "Polimer Melunak Saat Panas (Bisa Didaur)." },
  { "en": "Apa Itu Termoset (Thermoset)?", "id": "Polimer Mengeras Permanen Saat Panas." },
  { "en": "Apa Itu Elastomer?", "id": "Polimer Elastis (Contoh: Karet)." },
  { "en": "Apa Itu Komposit (Composite)?", "id": "Gabungan Dua Material Berbeda Sifat." },
  { "en": "Contoh Komposit (Composite)?", "id": "Fiberglass (Serat Kaca Resin)." },
  { "en": "Apa Itu Serat Karbon (Carbon Fiber)?", "id": "Material Komposit Sangat Kuat Ringan." },
  { "en": "Apa Itu Nanoteknologi?", "id": "Teknologi Skala Sangat Kecil (Nanometer)." },
  { "en": "Apa Itu Grafena (Graphene)?", "id": "Satu Lapisan Atom Karbon (Sangat Kuat)." },
  { "en": "Apa Itu Tabung Nano Karbon (CNT)?", "id": "Struktur Karbon Tabung Skala Nano." },
  { "en": "Apa Itu Sifat Mekanik Material?", "id": "Respon Material Terhadap Gaya (Kekuatan)." },
  { "en": "Apa Itu Kekuatan (Strength)?", "id": "Kemampuan Material Menahan Beban." },
  { "en": "Apa Itu Kekerasan (Hardness)?", "id": "Kemampuan Material Tahan Goresan." },
  { "en": "Apa Itu Keuletan (Ductility)?", "id": "Kemampuan Material Meregang (Jadi Kawat)." },
  { "en": "Apa Itu Kerapuhan (Brittleness)?", "id": "Sifat Material Mudah Patah (Getas)." },
  { "en": "Apa Itu Elastisitas (Elasticity)?", "id": "Kemampuan Kembali Ke Bentuk Semula." },
  { "en": "Apa Itu Plastisitas (Plasticity)?", "id": "Perubahan Bentuk Permanen (Tidak Kembali)." },
  { "en": "Apa Itu Kelelahan (Fatigue) Material?", "id": "Kerusakan Akibat Beban Berulang." },
  { "en": "Apa Itu Rayapan (Creep) Material?", "id": "Deformasi Lambat Akibat Beban Suhu Tinggi." },
  { "en": "Apa Itu Korosi (Corrosion)?", "id": "Kerusakan Material Akibat Reaksi Kimia." },
  { "en": "Contoh Korosi (Corrosion)?", "id": "Perkaratan Besi (Oksidasi)." },
  { "en": "Bagaimana Mencegah Korosi?", "id": "Pengecatan, Pelapisan Logam (Galvanis)." },
  { "en": "Apa Itu Galvanisasi (Galvanizing)?", "id": "Pelapisan Besi Dengan Seng (Zinc)." },
  { "en": "Apa Itu Anodisasi (Anodizing)?", "id": "Proses Pelapisan Oksida (Aluminium)." },
  { "en": "Apa Itu Konduktivitas Termal?", "id": "Kemampuan Material Menghantarkan Panas." },
  { "en": "Apa Itu Ekspansi Termal?", "id": "Material Memuai Saat Dipanaskan." },
  { "en": "Apa Itu Superkonduktor?", "id": "Material Hambatan Listrik Nol (Suhu Dingin)." },
  { "en": "Apa Itu Biomaterial?", "id": "Material Yang Berinteraksi Dengan Sistem Biologi." },
  { "en": "Contoh Biomaterial?", "id": "Implan Tulang, Lensa Kontak." },
  { "en": "Apa Itu Piezoelektrik (Material)?", "id": "Material Penghasil Listrik Saat Ditekan." },
  { "en": "Apa Itu Material Cerdas (Smart Material)?", "id": "Material Berubah Sifat Akibat Rangsangan." },
  { "en": "Apa Itu Paduan Memori Bentuk (Shape Memory)?", "id": "Material Kembali Ke Bentuk Asli (Dipanaskan)." },
  { "en": "Apa Itu Material Fotokromik?", "id": "Material Berubah Warna Akibat Cahaya." },
  { "en": "Apa Itu Material Termokromik?", "id": "Material Berubah Warna Akibat Suhu." },
  { "en": "Apa Itu Material Magnetik?", "id": "Material Yang Bereaksi Terhadap Medan Magnet." },
  { "en": "Apa Itu Feromagnetik?", "id": "Material Yang Dapat Ditarik Kuat Magnet." },
  { "en": "Contoh Material Feromagnetik?", "id": "Besi, Nikel, Kobalt." },
  { "en": "Apa Itu Paramagnetik?", "id": "Material Ditarik Lemah Medan Magnet." },
  { "en": "Apa Itu Diamagnetik?", "id": "Material Ditolak Lemah Medan Magnet." },
  { "en": "Contoh Material Diamagnetik?", "id": "Air, Tembaga, Bismut." },
  { "en": "Apa Itu Histeresis (Magnetik)?", "id": "Keterlambatan Reaksi Magnetisasi Material." },
  { "en": "Apa Itu Magnet Keras (Hard Magnet)?", "id": "Material Sulit Dimagnetisasi Sulit Dihilangkan." },
  { "en": "Contoh Magnet Keras?", "id": "Magnet Permanen (Neodymium)." },
  { "en": "Apa Itu Magnet Lunak (Soft Magnet)?", "id": "Material Mudah Dimagnetisasi Mudah Hilang." },
  { "en": "Contoh Magnet Lunak?", "id": "Inti Besi Transformator." },
  { "en": "Apa Itu Titik Curie (Curie Point)?", "id": "Suhu Saat Material Kehilangan Sifat Magnet." },
  { "en": "Apa Itu Magnet Neodymium?", "id": "Magnet Permanen Paling Kuat (NdFeB)." },
  { "en": "Apa Itu Magnet Ferit (Keramik)?", "id": "Magnet Permanen Umum (Hitam, Rapuh)." },
  { "en": "Apa Itu Kimia?", "id": "Ilmu Tentang Komposisi Struktur Sifat Materi." },
  { "en": "Apa Itu Unsur (Element)?", "id": "Zat Murni (Tidak Bisa Diurai Lagi)." },
  { "en": "Contoh Unsur (Element)?", "id": "Hidrogen (H), Oksigen (O), Karbon (C)." },
  { "en": "Apa Itu Senyawa (Compound)?", "id": "Gabungan Dua Unsur Atau Lebih (Kimia)." },
  { "en": "Contoh Senyawa (Compound)?", "id": "Air (H2O), Garam (NaCl)." },
  { "en": "Apa Itu Campuran (Mixture)?", "id": "Gabungan Zat (Tanpa Reaksi Kimia)." },
  { "en": "Apa Itu Molekul?", "id": "Gabungan Dua Atom Atau Lebih (Ikatan Kimia)." },
  { "en": "Apa Itu Ikatan Kimia?", "id": "Gaya Yang Mengikat Atom Bersama." },
  { "en": "Apa Itu Ikatan Ionik?", "id": "Ikatan (Transfer Elektron, Logam Non-Logam)." },
  { "en": "Apa Itu Ikatan Kovalen?", "id": "Ikatan (Berbagi Elektron, Non-Logam)." },
  { "en": "Apa Itu Ikatan Logam?", "id": "Ikatan (Lautan Elektron, Logam)." },
  { "en": "Apa Itu Tabel Periodik?", "id": "Tabel Susunan Unsur Kimia." },
  { "en": "Apa Itu Nomor Atom?", "id": "Jumlah Proton Dalam Inti Atom." },
  { "en": "Apa Itu Nomor Massa?", "id": "Jumlah Proton Ditambah Neutron." },
  { "en": "Apa Itu Isotop?", "id": "Atom Unsur Sama (Jumlah Neutron Beda)." },
  { "en": "Apa Itu Elektron Valensi?", "id": "Elektron Di Kulit Atom Terluar." },
  { "en": "Apa Itu Aturan Oktet?", "id": "Kecenderungan Atom (Mencapai 8 Elektron Valensi)." },
  { "en": "Apa Itu Reaksi Kimia?", "id": "Proses Perubahan Zat (Pembentukan Zat Baru)." },
  { "en": "Apa Itu Reaktan?", "id": "Zat Awal Sebelum Reaksi Kimia." },
  { "en": "Apa Itu Produk (Kimia)?", "id": "Zat Hasil Reaksi Kimia." },
  { "en": "Apa Itu Reaksi Endotermik?", "id": "Reaksi Yang Membutuhkan Menyerap Panas." },
  { "en": "Apa Itu Reaksi Eksotermik?", "id": "Reaksi Yang Melepaskan Menghasilkan Panas." },
  { "en": "Apa Itu Katalis (Catalyst)?", "id": "Zat Peningkat Laju Reaksi (Tidak Habis)." },
  { "en": "Apa Itu Enzim?", "id": "Katalis Biologis (Protein)." },
  { "en": "Apa Itu pH?", "id": "Ukuran Tingkat Keasaman Atau Kebasaan." },
  { "en": "Berapa Rentang Skala pH?", "id": "Nol Hingga Empat Belas (0-14)." },
  { "en": "Apa Itu pH Asam?", "id": "Nilai pH Kurang Dari Tujuh (< 7)." },
  { "en": "Apa Itu pH Basa (Alkali)?", "id": "Nilai pH Lebih Dari Tujuh (> 7)." },
  { "en": "Apa Itu pH Netral?", "id": "Nilai pH Tepat Tujuh (= 7)." },
  { "en": "Contoh pH Netral?", "id": "Air Murni." },
  { "en": "Apa Itu Larutan Asam?", "id": "Larutan Menghasilkan Ion Hidrogen (H+)." },
  { "en": "Apa Itu Larutan Basa?", "id": "Larutan Menghasilkan Ion Hidroksida (OH-)." },
  { "en": "Apa Itu Reaksi Netralisasi?", "id": "Reaksi Asam Dan Basa Menjadi Garam Air." },
  { "en": "Apa Itu Indikator pH?", "id": "Zat Berubah Warna Sesuai pH." },
  { "en": "Contoh Indikator pH?", "id": "Kertas Lakmus." },
  { "en": "Apa Warna Lakmus Merah Di Basa?", "id": "Berubah Menjadi Warna Biru." },
  { "en": "Apa Warna Lakmus Biru Di Asam?", "id": "Berubah Menjadi Warna Merah." },
  { "en": "Apa Itu Reaksi Oksidasi?", "id": "Reaksi Pelepasan Elektron (Biloks Naik)." },
  { "en": "Apa Itu Reaksi Reduksi?", "id": "Reaksi Penerimaan Elektron (Biloks Turun)." },
  { "en": "Apa Itu Reaksi Redoks (Redox)?", "id": "Gabungan Reaksi Reduksi Dan Oksidasi." },
  { "en": "Contoh Reaksi Redoks?", "id": "Perkaratan Besi, Pembakaran, Baterai." },
  { "en": "Apa Itu Oksidator?", "id": "Zat Penyebab Oksidasi (Mereduksi)." },
  { "en": "Apa Itu Reduktor?", "id": "Zat Penyebab Reduksi (Mengoksidasi)." },
  { "en": "Apa Itu Elektrokimia?", "id": "Ilmu Hubungan Listrik Reaksi Kimia." },
  { "en": "Apa Itu Sel Volta (Galvani)?", "id": "Sel Penghasil Listrik Dari Reaksi Kimia." },
  { "en": "Prinsip Kerja Sel Volta?", "id": "Reaksi Redoks Spontan (Contoh: Baterai)." },
  { "en": "Apa Itu Sel Elektrolisis?", "id": "Sel Menggunakan Listrik Untuk Reaksi Kimia." },
  { "en": "Prinsip Kerja Sel Elektrolisis?", "id": "Reaksi Redoks Tidak Spontan." },
  { "en": "Contoh Sel Elektrolisis?", "id": "Penyepuhan Logam (Electroplating)." },
  { "en": "Apa Itu Anoda (Sel Volta)?", "id": "Elektroda Negatif (Tempat Oksidasi)." },
  { "en": "Apa Itu Katoda (Sel Volta)?", "id": "Elektroda Positif (Tempat Reduksi)." },
  { "en": "Apa Itu Anoda (Sel Elektrolisis)?", "id": "Elektroda Positif (Tempat Oksidasi)." },
  { "en": "Apa Itu Katoda (Sel Elektrolisis)?", "id": "Elektroda Negatif (Tempat Reduksi)." },
  { "en": "Apa Itu Jembatan Garam?", "id": "Penyeimbang Muatan Ion Dalam Sel Volta." },
  { "en": "Apa Itu Potensial Reduksi Standar?", "id": "Ukuran Kecenderungan Unsur Menerima Elektron." },
  { "en": "Apa Itu Kimia Organik?", "id": "Ilmu Senyawa Berbasis Karbon." },
  { "en": "Apa Itu Hidrokarbon?", "id": "Senyawa Hanya Karbon Dan Hidrogen." },
  { "en": "Contoh Hidrokarbon?", "id": "Metana (Gas Alam), Propana (LPG)." },
  { "en": "Apa Itu Alkana?", "id": "Hidrokarbon Ikatan Tunggal." },
  { "en": "Apa Itu Alkena?", "id": "Hidrokarbon Ikatan Rangkap Dua." },
  { "en": "Apa Itu Alkuna?", "id": "Hidrokarbon Ikatan Rangkap Tiga." },
  { "en": "Apa Itu Alkohol?", "id": "Senyawa Organik Dengan Gugus Hidroksil (-OH)." },
  { "en": "Contoh Alkohol?", "id": "Etanol (Minuman Keras), Metanol (Spiritus)." },
  { "en": "Apa Itu Asam Karboksilat?", "id": "Senyawa Organik Gugus Karboksil (-COOH)." },
  { "en": "Contoh Asam Karboksilat?", "id": "Asam Asetat (Cuka)." },
  { "en": "Apa Itu Polimerisasi?", "id": "Proses Pembentukan Polimer Dari Monomer." },
  { "en": "Apa Itu Monomer?", "id": "Unit Molekul Kecil Pembentuk Polimer." },
  { "en": "Apa Itu Plastik?", "id": "Material Sintetis Berbasis Polimer." },
  { "en": "Apa Itu Bensin (Gasoline)?", "id": "Bahan Bakar (Campuran Hidrokarbon)." },
  { "en": "Apa Itu Bilangan Oktan?", "id": "Ukuran Ketahanan Bensin Terhadap Ketukan." },
  { "en": "Apa Itu Ketukan (Knocking) Mesin?", "id": "Pembakaran Bensin Tidak Sempurna (Dini)." },
  { "en": "Apa Itu Pembakaran Sempurna?", "id": "Pembakaran Menghasilkan Karbon Dioksida Air." },
  { "en": "Apa Itu Pembakaran Tidak Sempurna?", "id": "Pembakaran Menghasilkan Karbon Monoksida." },
  { "en": "Apa Bahaya Karbon Monoksida (CO)?", "id": "Gas Beracun (Mengikat Hemoglobin)." },
  { "en": "Apa Itu Gas Rumah Kaca?", "id": "Gas Penyebab Pemanasan Global." },
  { "en": "Contoh Gas Rumah Kaca?", "id": "Karbon Dioksida (CO2), Metana (CH4)." },
  { "en": "Apa Itu Pemanasan Global?", "id": "Peningkatan Suhu Rata-Rata Atmosfer Bumi." },
  { "en": "Apa Itu Efek Rumah Kaca?", "id": "Proses Panas Terperangkap Atmosfer." },
  { "en": "Apa Itu Lapisan Ozon (O3)?", "id": "Lapisan Pelindung Bumi Dari Sinar UV." },
  { "en": "Apa Itu CFC (Chloro Fluoro Carbon)?", "id": "Zat Perusak Lapisan Ozon." },
  { "en": "Apa Itu Hujan Asam?", "id": "Hujan Dengan pH Rendah (Asam)." },
  { "en": "Penyebab Hujan Asam?", "id": "Polusi Oksida Sulfur Nitrogen." },
  { "en": "Apa Itu Biologi?", "id": "Ilmu Tentang Kehidupan Makhluk Hidup." },
  { "en": "Apa Itu Sel (Biologi)?", "id": "Unit Dasar Kehidupan Terkecil." },
  { "en": "Apa Itu Sel Prokariotik?", "id": "Sel Tanpa Membran Inti (Bakteri)." },
  { "en": "Apa Itu Sel Eukariotik?", "id": "Sel Dengan Membran Inti (Hewan, Tumbuhan)." },
  { "en": "Apa Itu Membran Sel?", "id": "Lapisan Luar Pelindung Sel." },
  { "en": "Apa Itu Sitoplasma?", "id": "Cairan Di Dalam Sel." },
  { "en": "Apa Itu Inti Sel (Nukleus)?", "id": "Pusat Kontrol Sel (Mengandung DNA)." },
  { "en": "Apa Itu DNA (Deoxyribonucleic Acid)?", "id": "Materi Genetik Penyimpan Informasi." },
  { "en": "Apa Itu Gen?", "id": "Segmen DNA (Penentu Sifat)." },
  { "en": "Apa Itu Kromosom?", "id": "Struktur DNA Yang Terpadatkan." },
  { "en": "Apa Itu Ribosom?", "id": "Organel Tempat Sintesis Protein." },
  { "en": "Apa Itu Mitokondria?", "id": "Organel Pembangkit Energi Sel (Respirasi)." },
  { "en": "Apa Itu Dinding Sel?", "id": "Lapisan Kaku Luar Sel Tumbuhan." },
  { "en": "Apa Itu Kloroplas?", "id": "Organel Fotosintesis (Hanya Tumbuhan)." },
  { "en": "Apa Itu Fotosintesis?", "id": "Proses Pembuatan Makanan Tumbuhan (Cahaya)." },
  { "en": "Apa Itu Respirasi Seluler?", "id": "Proses Pembongkaran Makanan Menjadi Energi." },
  { "en": "Apa Itu ATP (Adenosine Triphosphate)?", "id": "Molekul Energi Utama Sel." },
  { "en": "Apa Itu Jaringan (Biologi)?", "id": "Kumpulan Sel Sejenis (Fungsi Sama)." },
  { "en": "Apa Itu Organ (Biologi)?", "id": "Kumpulan Jaringan (Fungsi Khusus)." },
  { "en": "Apa Itu Sistem Organ?", "id": "Kumpulan Organ (Sistem Pencernaan)." },
  { "en": "Apa Itu Organisme?", "id": "Makhluk Hidup Individual." },
  { "en": "Apa Itu Populasi?", "id": "Kumpulan Organisme Sejenis Di Satu Tempat." },
  { "en": "Apa Itu Komunitas (Biologi)?", "id": "Kumpulan Berbagai Populasi." },
  { "en": "Apa Itu Ekosistem?", "id": "Interaksi Komunitas Dengan Lingkungan." },
  { "en": "Apa Itu Biosfer?", "id": "Bagian Bumi Tempat Adanya Kehidupan." },
  { "en": "Apa Itu Taksonomi (Biologi)?", "id": "Ilmu Klasifikasi Makhluk Hidup." },
  { "en": "Siapa Bapak Taksonomi?", "id": "Carolus Linnaeus." },
  { "en": "Apa Itu Sistem Binomial Nomenklatur?", "id": "Sistem Nama Latin (Genus Spesies)." },
  { "en": "Contoh Binomial Nomenklatur?", "id": "Homo Sapiens (Manusia)." },
  { "en": "Apa Urutan Taksonomi?", "id": "Kingdom, Filum, Kelas, Ordo, Famili, Genus, Spesies." },
  { "en": "Apa Itu Lima Kingdom (Kerajaan)?", "id": "Monera, Protista, Fungi, Plantae, Animalia." },
  { "en": "Apa Itu Kingdom Monera?", "id": "Kerajaan Bakteri (Prokariotik)." },
  { "en": "Apa Itu Kingdom Protista?", "id": "Eukariotik Sederhana (Amoeba, Alga)." },
  { "en": "Apa Itu Kingdom Fungi (Jamur)?", "id": "Jamur (Menyerap Makanan)." },
  { "en": "Apa Itu Kingdom Plantae (Tumbuhan)?", "id": "Tumbuhan (Berfotosintesis)." },
  { "en": "Apa Itu Kingdom Animalia (Hewan)?", "id": "Hewan (Bergerak, Memakan)." },
  { "en": "Apa Itu Vertebrata?", "id": "Hewan Bertulang Belakang." },
  { "en": "Apa Itu Invertebrata?", "id": "Hewan Tidak Bertulang Belakang." },
  { "en": "Apa Itu Mamalia?", "id": "Hewan Menyusui (Berambut, Berdarah Panas)." },
  { "en": "Apa Itu Burung (Aves)?", "id": "Hewan Berbulu (Bisa Terbang)." },
  { "en": "Apa Itu Reptil?", "id": "Hewan Melata Bersisik (Berdarah Dingin)." },
  { "en": "Apa Itu Amfibi?", "id": "Hewan Hidup Dua Alam (Katak)." },
  { "en": "Apa Itu Ikan (Pisces)?", "id": "Hewan Hidup Di Air (Bernapas Insang)." },
  { "en": "Apa Itu Serangga (Insecta)?", "id": "Invertebrata (Enam Kaki, Tiga Bagian Tubuh)." },
  { "en": "Apa Itu Virus?", "id": "Agen Infeksi (Bukan Sel Hidup)." },
  { "en": "Kenapa Virus Bukan Makhluk Hidup?", "id": "Tidak Bisa Bereproduksi Sendiri." },
  { "en": "Apa Itu Bakteri?", "id": "Organisme Prokariotik Bersel Satu." },
  { "en": "Apa Itu Bakteri Baik (Menguntungkan)?", "id": "Bakteri (Membantu Pencernaan)." },
  { "en": "Apa Itu Bakteri Jahat (Patogen)?", "id": "Bakteri Penyebab Penyakit." },
  { "en": "Apa Itu Antibiotik?", "id": "Obat Pembunuh Bakteri." },
  { "en": "Apakah Antibiotik Membunuh Virus?", "id": "Tidak, Antibiotik Hanya Untuk Bakteri." },
  { "en": "Apa Itu Vaksin (Vaksinasi)?", "id": "Melatih Sistem Kekebalan Tubuh." },
  { "en": "Apa Itu Sistem Kekebalan Tubuh (Imun)?", "id": "Sistem Pertahanan Tubuh Melawan Penyakit." },
  { "en": "Apa Itu Antibodi?", "id": "Protein Sistem Imun (Melawan Kuman)." },
  { "en": "Apa Itu Alergi?", "id": "Reaksi Berlebih Sistem Imun." },
  { "en": "Apa Itu Penyakit Autoimun?", "id": "Sistem Imun Menyerang Tubuh Sendiri." },
  { "en": "Apa Itu Evolusi (Biologi)?", "id": "Perubahan Sifat Warisan Seiring Waktu." },
  { "en": "Siapa Penggagas Teori Evolusi?", "id": "Charles Darwin." },
  { "en": "Apa Itu Seleksi Alam?", "id": "Proses Alam (Yang Kuat Bertahan)." },
  { "en": "Apa Itu Mutasi (Genetika)?", "id": "Perubahan Acak Pada Urutan DNA." },
  { "en": "Apa Itu Adaptasi (Biologi)?", "id": "Penyesuaian Makhluk Hidup Lingkungan." },
  { "en": "Apa Itu Ekologi?", "id": "Ilmu Interaksi Makhluk Hidup Lingkungan." },
  { "en": "Apa Itu Habitat?", "id": "Tempat Tinggal Alami Makhluk Hidup." },
  { "en": "Apa Itu Niche (Relung)?", "id": "Peran Makhluk Hidup Di Ekosistem." },
  { "en": "Apa Itu Rantai Makanan?", "id": "Urutan Transfer Energi (Makan Dimakan)." },
  { "en": "Apa Itu Produsen (Ekosistem)?", "id": "Pembuat Makanan Sendiri (Tumbuhan)." },
  { "en": "Apa Itu Konsumen (Ekosistem)?", "id": "Pemakan Organisme Lain (Hewan)." },
  { "en": "Apa Itu Dekomposer (Pengurai)?", "id": "Pengurai Sisa Organik (Jamur, Bakteri)." },
  { "en": "Apa Itu Herbivora?", "id": "Hewan Pemakan Tumbuhan." },
  { "en": "Apa Itu Karnivora?", "id": "Hewan Pemakan Daging." },
  { "en": "Apa Itu Omnivora?", "id": "Hewan Pemakan Segala." },
  { "en": "Apa Itu Simbiosis?", "id": "Hubungan Dekat Dua Spesies Berbeda." },
  { "en": "Apa Itu Simbiosis Mutualisme?", "id": "Hubungan Saling Menguntungkan." },
  { "en": "Apa Itu Simbiosis Komensalisme?", "id": "Satu Untung, Satu Tidak Rugi." },
  { "en": "Apa Itu Simbiosis Parasitisme?", "id": "Satu Untung, Satu Rugi (Parasit)." },
  { "en": "Apa Itu Predator?", "id": "Hewan Pemburu Mangsa." },
  { "en": "Apa Itu Mangsa (Prey)?", "id": "Hewan Yang Diburu Predator." },
  { "en": "Apa Itu Kamuflase (Penyamaran)?", "id": "Adaptasi Menyamar Dengan Lingkungan." },
  { "en": "Apa Itu Mimikri?", "id": "Adaptasi Meniru Penampilan Makhluk Lain." },
  { "en": "Apa Itu Metamorfosis?", "id": "Perubahan Bentuk Hewan (Kupu-Kupu)." },
  { "en": "Apa Itu Metamorfosis Sempurna?", "id": "Telur, Larva, Pupa, Dewasa." },
  { "en": "Apa Itu Metamorfosis Tidak Sempurna?", "id": "Telur, Nimfa, Dewasa." },
  { "en": "Apa Itu Hormon?", "id": "Zat Kimia Pembawa Pesan Tubuh." },
  { "en": "Apa Itu Sistem Endokrin?", "id": "Sistem Kelenjar Penghasil Hormon." },
  { "en": "Apa Itu Sistem Saraf?", "id": "Sistem Kontrol Tubuh (Sinyal Listrik)." },
  { "en": "Apa Itu Neuron (Sel Saraf)?", "id": "Sel Dasar Sistem Saraf." },
  { "en": "Apa Itu Otak (Brain)?", "id": "Pusat Kontrol Sistem Saraf." },
  { "en": "Apa Itu Sistem Pencernaan?", "id": "Sistem Pemroses Makanan Tubuh." },
  { "en": "Apa Itu Enzim (Pencernaan)?", "id": "Protein Pemecah Makanan." },
  { "en": "Apa Itu Sistem Pernapasan?", "id": "Sistem Pengambilan Oksigen (Paru-Paru)." },
  { "en": "Apa Itu Sistem Peredaran Darah?", "id": "Sistem Pengedar Darah (Jantung, Pembuluh)." },
  { "en": "Apa Itu Jantung (Heart)?", "id": "Organ Pemompa Darah." },
  { "en": "Apa Itu Darah (Blood)?", "id": "Cairan Transportasi Tubuh." },
  { "en": "Apa Itu Hemoglobin?", "id": "Protein Pengikat Oksigen Di Darah Merah." },
  { "en": "Apa Itu Sistem Ekskresi?", "id": "Sistem Pembuangan Sisa Metabolisme." },
  { "en": "Apa Itu Ginjal (Kidney)?", "id": "Organ Penyaring Darah (Ekskresi)." },
  { "en": "Apa Itu Anatomi?", "id": "Ilmu Struktur Tubuh." },
  { "en": "Apa Itu Fisiologi?", "id": "Ilmu Fungsi Tubuh." },
  { "en": "Apa Itu Matematika Teknik?", "id": "Aplikasi Matematika Untuk Menyelesaikan Masalah Teknik." },
  { "en": "Apa Itu Kalkulus?", "id": "Cabang Matematika (Diferensial Dan Integral)." },
  { "en": "Apa Itu Diferensial (Turunan)?", "id": "Mengukur Laju Perubahan Sesaat." },
  { "en": "Apa Aplikasi Diferensial Di Elektro?", "id": "Analisis Sinyal (Slope), Rangkaian RLC." },
  { "en": "Apa Itu Integral?", "id": "Kebalikan Diferensial (Luas Di Bawah Kurva)." },
  { "en": "Apa Aplikasi Integral Di Elektro?", "id": "Menghitung Muatan Kapasitor, Fluks Magnet." },
  { "en": "Apa Itu Persamaan Diferensial?", "id": "Persamaan Yang Melibatkan Turunan." },
  { "en": "Di Mana Persamaan Diferensial Digunakan?", "id": "Analisis Rangkaian RLC, Sistem Kontrol." },
  { "en": "Apa Itu Persamaan Diferensial Orde Pertama?", "id": "Persamaan Dengan Turunan Tertinggi Satu." },
  { "en": "Contoh Rangkaian Orde Pertama?", "id": "Rangkaian RC Atau RL Sederhana." },
  { "en": "Apa Itu Persamaan Diferensial Orde Kedua?", "id": "Persamaan Dengan Turunan Tertinggi Dua." },
  { "en": "Contoh Rangkaian Orde Kedua?", "id": "Rangkaian RLC Seri Atau Paralel." },
  { "en": "Apa Itu Respon Transien?", "id": "Respon Awal Rangkaian Saat Diberi Input." },
  { "en": "Apa Itu Respon Steady-State (Mapan)?", "id": "Respon Rangkaian Setelah Waktu Lama." },
  { "en": "Apa Itu Konstanta Waktu (Tau) RC?", "id": "Waktu Pengisian Kapasitor (R Kali C)." },
  { "en": "Apa Itu Konstanta Waktu (Tau) RL?", "id": "Waktu Pengisian Induktor (L Bagi R)." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Metode Matematika (Ubah Domain Waktu Ke S)." },
  { "en": "Kenapa Menggunakan Transformasi Laplace?", "id": "Menyederhanakan Solusi Persamaan Diferensial." },
  { "en": "Apa Itu Domain-S (Domain Laplace)?", "id": "Domain Frekuensi Kompleks." },
  { "en": "Apa Itu Fungsi Transfer (Sistem Kontrol)?", "id": "Rasio Output Terhadap Input (Domain-S)." },
  { "en": "Apa Itu Transformasi Fourier?", "id": "Metode Matematika (Ubah Domain Waktu Ke Frekuensi)." },
  { "en": "Apa Beda Laplace Dan Fourier?", "id": "Laplace (Umum), Fourier (Analisis Steady-State AC)." },
  { "en": "Apa Itu Deret Fourier?", "id": "Mengurai Sinyal Periodik Menjadi Sinusoidal." },
  { "en": "Contoh Aplikasi Deret Fourier?", "id": "Analisis Harmonik Sinyal Kotak." },
  { "en": "Apa Itu Bilangan Kompleks?", "id": "Bilangan Dengan Bagian Riil Dan Imajiner." },
  { "en": "Kenapa Bilangan Kompleks Dipakai Di Elektro?", "id": "Analisis Rangkaian AC (Fasor, Impedansi)." },
  { "en": "Apa Itu Bilangan Imajiner (j)?", "id": "Akar Kuadrat Dari Minus Satu (√-1)." },
  { "en": "Kenapa Elektro Menggunakan 'j' Bukan 'i'?", "id": "Agar Tidak Bingung Dengan Simbol Arus (i)." },
  { "en": "Apa Itu Fasor (Phasor)?", "id": "Representasi Bilangan Kompleks Sinyal AC." },
  { "en": "Apa Itu Impedansi (Z)?", "id": "Hambatan Total Rangkaian AC (Kompleks)." },
  { "en": "Apa Rumus Impedansi (Z)?", "id": "Z = R + jX (Resistansi + Reaktansi)." },
  { "en": "Apa Itu Reaktansi (X)?", "id": "Hambatan Non-Resistif (Kapasitor, Induktor)." },
  { "en": "Apa Itu Reaktansi Induktif (XL)?", "id": "Reaktansi Induktor (jωL)." },
  { "en": "Apa Itu Reaktansi Kapasitif (XC)?", "id": "Reaktansi Kapasitor (1 / jωC)." },
  { "en": "Apa Itu 'ω' (Omega)?", "id": "Frekuensi Sudut (2 * pi * f)." },
  { "en": "Apa Itu Admitansi (Y)?", "id": "Kebalikan Dari Impedansi (Y = 1/Z)." },
  { "en": "Apa Itu Konduktansi (G)?", "id": "Kebalikan Dari Resistansi (Bagian Riil Y)." },
  { "en": "Apa Itu Suseptansi (B)?", "id": "Kebalikan Dari Reaktansi (Bagian Imajiner Y)." },
  { "en": "Apa Itu Aljabar Linear?", "id": "Matematika Vektor Dan Matriks." },
  { "en": "Aplikasi Aljabar Linear Di Elektro?", "id": "Analisis Rangkaian (Loop/Node), Teori Kontrol." },
  { "en": "Apa Itu Matriks?", "id": "Susunan Bilangan (Baris Dan Kolom)." },
  { "en": "Apa Itu Determinan Matriks?", "id": "Nilai Skalar Dari Matriks Persegi." },
  { "en": "Apa Itu Invers Matriks?", "id": "Kebalikan Matriks (Penyelesaian Persamaan)." },
  { "en": "Apa Itu Metode Analisis Node (Simpul)?", "id": "Analisis Rangkaian Berbasis Tegangan Node." },
  { "en": "Apa Itu Metode Analisis Mesh (Loop)?", "id": "Analisis Rangkaian Berbasis Arus Loop." },
  { "en": "Apa Itu Teorema Superposisi?", "id": "Analisis Rangkaian (Satu Sumber Aktif)." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Penyederhanaan Rangkaian (Sumber Tegangan Seri)." },
  { "en": "Apa Itu Rangkaian Ekuivalen Thevenin?", "id": "Satu Sumber Tegangan (Vth) Seri Resistor (Rth)." },
  { "en": "Apa Itu Teorema Norton?", "id": "Penyederhanaan Rangkaian (Sumber Arus Paralel)." },
  { "en": "Apa Itu Rangkaian Ekuivalen Norton?", "id": "Satu Sumber Arus (In) Paralel Resistor (Rn)." },
  { "en": "Apa Hubungan Thevenin Dan Norton?", "id": "Rth = Rn, Vth = In * Rn." },
  { "en": "Apa Itu Teorema Transfer Daya Maksimum?", "id": "Daya Maksimal Saat Hambatan Beban = Hambatan Sumber." },
  { "en": "Apa Itu Transformasi Delta-Wye (Pi-Tee)?", "id": "Konversi Konfigurasi Resistor Segitiga-Bintang." },
  { "en": "Apa Itu Rangkaian Dua Gerbang (Two-Port)?", "id": "Model Rangkaian (Input, Output)." },
  { "en": "Apa Itu Parameter Z (Impedansi)?", "id": "Parameter Rangkaian Dua Gerbang." },
  { "en": "Apa Itu Parameter Y (Admitansi)?", "id": "Parameter Rangkaian Dua Gerbang." },
  { "en": "Apa Itu Parameter H (Hibrida)?", "id": "Parameter (Model Transistor)." },
  { "en": "Apa Itu Probabilitas Dan Statistik?", "id": "Matematika Ketidakpastian Dan Data." },
  { "en": "Aplikasi Statistik Di Elektro?", "id": "Teori Sinyal, Kontrol Kualitas, Noise." },
  { "en": "Apa Itu Noise (Deras) Acak?", "id": "Sinyal Acak Yang Tidak Diinginkan." },
  { "en": "Apa Itu Distribusi Normal (Gaussian)?", "id": "Distribusi Probabilitas (Bentuk Lonceng)." },
  { "en": "Apa Itu White Noise (Deras Putih)?", "id": "Noise Dengan Kepadatan Spektral Datar." },
  { "en": "Apa Itu Pink Noise (Deras Merah Muda)?", "id": "Noise (Daya Turun Per Oktaf)." },
  { "en": "Apa Itu Rata-Rata (Mean)?", "id": "Nilai Rata-Rata Statistik." },
  { "en": "Apa Itu Variansi (Variance)?", "id": "Ukuran Sebaran Data (Kuadrat)." },
  { "en": "Apa Itu Standar Deviasi?", "id": "Ukuran Sebaran Data (Akar Variansi)." },
  { "en": "Apa Itu SNR (Signal-to-Noise Ratio)?", "id": "Rasio Kekuatan Sinyal Terhadap Noise." },
  { "en": "Apa Itu Matematika Diskrit?", "id": "Matematika Objek Diskrit (Terhitung)." },
  { "en": "Aplikasi Matematika Diskrit Di Elektro?", "id": "Logika Digital, Teori Informasi, Kriptografi." },
  { "en": "Apa Itu Aljabar Boolean?", "id": "Aljabar Logika Biner (True/False)." },
  { "en": "Siapa Penemu Aljabar Boolean?", "id": "George Boole." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Bangunan Dasar Logika Digital." },
  { "en": "Apa Itu Teorema De Morgan?", "id": "Aturan Ekuivalensi Gerbang Logika." },
  { "en": "Apa Itu Peta Karnaugh (K-Map)?", "id": "Metode Penyederhanaan Fungsi Logika." },
  { "en": "Apa Itu Teori Graf (Graph Theory)?", "id": "Studi Jaringan Titik Dan Garis." },
  { "en": "Aplikasi Teori Graf Di Elektro?", "id": "Desain Rangkaian, Jaringan Komputer." },
  { "en": "Apa Itu Metode Numerik?", "id": "Metode Penyelesaian Masalah (Aproksimasi)." },
  { "en": "Kenapa Perlu Metode Numerik?", "id": "Solusi Analitik Terlalu Sulit." },
  { "en": "Contoh Metode Numerik?", "id": "Metode Newton-Raphson, Aturan Simpson." },
  { "en": "Apa Itu Analisis Vektor?", "id": "Matematika Besaran Vektor (Arah Nilai)." },
  { "en": "Aplikasi Analisis Vektor Di Elektro?", "id": "Elektromagnetisme (Medan Listrik/Magnet)." },
  { "en": "Apa Itu Gradien (Vektor)?", "id": "Turunan Vektor (Laju Perubahan Maks)." },
  { "en": "Apa Itu Divergensi (Vektor)?", "id": "Ukuran Sumber Medan Vektor." },
  { "en": "Apa Itu Curl (Vektor)?", "id": "Ukuran Rotasi Medan Vektor." },
  { "en": "Apa Itu Persamaan Maxwell?", "id": "Empat Persamaan Dasar Elektromagnetisme." },
  { "en": "Apa Itu Vektor Satuan (Unit Vector)?", "id": "Vektor Dengan Panjang Satu." },
  { "en": "Apa Itu Perkalian Titik (Dot Product)?", "id": "Perkalian Vektor (Hasil Skalar)." },
  { "en": "Apa Itu Perkalian Silang (Cross Product)?", "id": "Perkalian Vektor (Hasil Vektor)." },
  { "en": "Apa Itu Sistem Koordinat Kartesius?", "id": "Sistem Koordinat (x, y, z)." },
  { "en": "Apa Itu Sistem Koordinat Silinder?", "id": "Sistem Koordinat (Radius, Sudut, Tinggi)." },
  { "en": "Apa Itu Sistem Koordinat Bola?", "id": "Sistem Koordinat (Radius, Dua Sudut)." },
  { "en": "Apa Itu Elektromagnetisme?", "id": "Ilmu Interaksi Medan Listrik Magnet." },
  { "en": "Apa Itu Hukum Gauss (Listrik)?", "id": "Persamaan Maxwell (Fluks Listrik Muatan)." },
  { "en": "Apa Itu Hukum Gauss (Magnet)?", "id": "Persamaan Maxwell (Tidak Ada Monopol Magnet)." },
  { "en": "Apa Itu Hukum Induksi Faraday?", "id": "Persamaan Maxwell (Perubahan Medan Magnet)." },
  { "en": "Apa Itu Hukum Ampere-Maxwell?", "id": "Persamaan Maxwell (Arus Medan Magnet)." },
  { "en": "Apa Itu Arus Perpindahan (Displacement Current)?", "id": "Konsep Maxwell (Perubahan Medan Listrik)." },
  { "en": "Apa Itu Gelombang Elektromagnetik?", "id": "Getaran Medan Listrik Magnet (Cahaya)." },
  { "en": "Apa Itu Konstanta Dielektrik (Permitivitas)?", "id": "Ukuran Polarisasi Material Listrik." },
  { "en": "Apa Itu Permeabilitas (Magnetik)?", "id": "Ukuran Kemampuan Material Dimagnetisasi." },
  { "en": "Apa Itu Reluktansi (Reluctance)?", "id": "Hambatan Terhadap Fluks Magnet (Seperti Resistansi)." },
  { "en": "Apa Itu Rangkaian Magnetik?", "id": "Analogi Rangkaian Listrik Untuk Magnet." },
  { "en": "Apa Itu Hukum Ohm (Magnetik)?", "id": "MMF = Fluks x Reluktansi." },
  { "en": "Apa Itu MMF (Magnetomotive Force)?", "id": "Gaya Gerak Magnet (Seperti Tegangan)." },
  { "en": "Apa Satuan MMF (Magnetomotive Force)?", "id": "Ampere-Lilit (Ampere-Turn)." },
  { "en": "Apa Itu Kurva B-H (Kurva Histeresis)?", "id": "Grafik Hubungan Medan Magnet Dan Magnetisasi." },
  { "en": "Apa Itu Retentivitas (Remanence)?", "id": "Sisa Magnet Saat Medan Luar Nol." },
  { "en": "Apa Itu Koersivitas (Coercivity)?", "id": "Kekuatan Medan Balik (Menghilangkan Magnet)." },
  { "en": "Apa Itu Arus Eddy (Eddy Current)?", "id": "Arus Pusar Induksi Di Dalam Konduktor." },
  { "en": "Di Mana Arus Eddy (Eddy Current) Merugikan?", "id": "Inti Transformator (Menimbulkan Panas)." },
  { "en": "Bagaimana Mengurangi Arus Eddy?", "id": "Menggunakan Inti Berlaminasi (Berlapis)." },
  { "en": "Di Mana Arus Eddy (Eddy Current) Bermanfaat?", "id": "Rem Magnetik, Kompor Induksi." },
  { "en": "Apa Itu Kompor Induksi?", "id": "Kompor Pemanas Panci (Arus Eddy)." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus AC Mengalir Di Permukaan Konduktor." },
  { "en": "Kapan Efek Kulit (Skin Effect) Terjadi?", "id": "Pada Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Kabel Litz?", "id": "Kabel Serabut Terisolasi (Mengurangi Efek Kulit)." },
  { "en": "Apa Itu Antena?", "id": "Pengubah Sinyal Listrik Ke Gelombang EM." },
  { "en": "Apa Itu Antena Resonan?", "id": "Antena Efisien Di Frekuensi Tertentu." },
  { "en": "Apa Itu Antena Dipole Setengah Gelombang?", "id": "Antena Resonan Paling Dasar." },
  { "en": "Apa Itu Pola Radiasi (Antena)?", "id": "Grafik Arah Pancaran Sinyal Antena." },
  { "en": "Apa Itu Antena Omnidirectional?", "id": "Antena Memancar Ke Segala Arah (Horizontal)." },
  { "en": "Apa Itu Antena Directional (Terarah)?", "id": "Antena Memancar Ke Satu Arah Tertentu." },
  { "en": "Apa Itu Gain (Penguatan) Antena?", "id": "Ukuran Keterarahan Pancaran Antena (dBi)." },
  { "en": "Apa Itu Impedansi (Impedansi) Antena?", "id": "Impedansi Di Titik Umpan Antena." },
  { "en": "Apa Itu Impedansi (Impedansi) Kabel Koaksial Umum?", "id": "Lima Puluh Ohm (50Ω) Atau 75 Ohm." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Ukuran Ketidakcocokan Impedansi (Antena Kabel)." },
  { "en": "Berapa Nilai SWR (Standing Wave Ratio) Ideal?", "id": "Satu Banding Satu (1:1)." },
  { "en": "Apa Itu Antenna Tuner (Penyelaras)?", "id": "Alat Pencocok Impedansi (Transmitter Antena)." },
  { "en": "Apa Itu Balun (Balanced-Unbalanced)?", "id": "Trafo Pencocok (Antena Simetris Ke Koaksial)." },
  { "en": "Apa Itu Garis Transmisi (Transmission Line)?", "id": "Media Penyalur Sinyal RF (Kabel Koaksial)." },
  { "en": "Apa Itu Waveguide (Pemandu Gelombang)?", "id": "Pipa Logam Penyalur Gelombang Mikro." },
  { "en": "Apa Itu Modulasi?", "id": "Proses Menumpangkan Sinyal Informasi." },
  { "en": "Apa Itu Sinyal Pembawa (Carrier)?", "id": "Sinyal Frekuensi Tinggi (Pembawa Info)." },
  { "en": "Apa Itu Sinyal Modulasi (Informasi)?", "id": "Sinyal Asli (Suara, Data)." },
  { "en": "Apa Itu Modulasi Analog?", "id": "Modulasi Sinyal Analog (AM, FM)." },
  { "en": "Apa Itu AM (Amplitude Modulation)?", "id": "Modulasi Amplitudo Sinyal Pembawa." },
  { "en": "Apa Itu FM (Frequency Modulation)?", "id": "Modulasi Frekuensi Sinyal Pembawa." },
  { "en": "Apa Kelebihan FM (Frequency Modulation) Dari AM?", "id": "Lebih Tahan Noise, Kualitas Suara Baik." },
  { "en": "Apa Itu PM (Phase Modulation)?", "id": "Modulasi Fasa Sinyal Pembawa." },
  { "en": "Apa Itu Modulasi Digital?", "id": "Modulasi Sinyal Digital (Bit 0 Dan 1)." },
  { "en": "Apa Itu ASK (Amplitude Shift Keying)?", "id": "Modulasi Digital (Variasi Amplitudo)." },
  { "en": "Apa Itu FSK (Frequency Shift Keying)?", "id": "Modulasi Digital (Variasi Frekuensi)." },
  { "en": "Apa Itu PSK (Phase Shift Keying)?", "id": "Modulasi Digital (Variasi Fasa)." },
  { "en": "Apa Itu QAM (Quadrature Amplitude Modulation)?", "id": "Modulasi Digital (Gabungan Amplitudo Fasa)." },
  { "en": "Apa Itu Demodulasi (Deteksi)?", "id": "Proses Pemisahan Sinyal Informasi." },
  { "en": "Apa Itu Detektor Selubung (Envelope)?", "id": "Rangkaian Demodulator AM Sederhana." },
  { "en": "Apa Itu Penerima Superheterodyne?", "id": "Arsitektur Penerima Radio Paling Umum." },
  { "en": "Apa Itu Frekuensi Antara (IF)?", "id": "Frekuensi Hasil Pencampuran (Mixer)." },
  { "en": "Apa Itu Mixer (Radio)?", "id": "Rangkaian Pengali Dua Sinyal Frekuensi." },
  { "en": "Apa Itu Osilator Lokal (Local Oscillator)?", "id": "Osilator Internal Penerima Radio." },
  { "en": "Apa Itu AGC (Automatic Gain Control)?", "id": "Kontrol Penguatan Otomatis (Stabilkan Volume)." },
  { "en": "Apa Itu SDR (Software Defined Radio)?", "id": "Radio (Fungsi Demodulasi Didefinisikan Software)." },
  { "en": "Apa Itu Teori Kontrol?", "id": "Ilmu Analisis Kontrol Sistem Dinamis." },
  { "en": "Apa Itu Sistem Loop Terbuka?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Loop Tertutup?", "id": "Sistem Kontrol Dengan Umpan Balik (Feedback)." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Menggunakan Sinyal Output Untuk Koreksi Input." },
  { "en": "Apa Itu Umpan Balik Negatif?", "id": "Umpan Balik (Mengurangi Error, Stabil)." },
  { "en": "Apa Itu Umpan Balik Positif?", "id": "Umpan Balik (Menambah Error, Osilator)." },
  { "en": "Apa Itu Diagram Blok (Sistem Kontrol)?", "id": "Representasi Grafis Sistem Kontrol." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Output Input (Domain Laplace)." },
  { "en": "Apa Itu Respon Waktu (Time Response)?", "id": "Perilaku Sistem Terhadap Waktu." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Perilaku Sistem Terhadap Frekuensi." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Grafik Respon Frekuensi (Magnitude Fasa)." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem?", "id": "Kemampuan Sistem Kembali Seimbang." },
  { "en": "Apa Itu Kriteria Kestabilan Routh-Hurwitz?", "id": "Metode Cek Kestabilan (Matematis)." },
  { "en": "Apa Itu Diagram Nyquist?", "id": "Metode Cek Kestabilan (Grafis)." },
  { "en": "Apa Itu Gain Margin (Batas Penguatan)?", "id": "Ukuran Kestabilan (Seberapa Jauh Dari Osilasi)." },
  { "en": "Apa Itu Phase Margin (Batas Fasa)?", "id": "Ukuran Kestabilan (Seberapa Jauh Dari Osilasi)." },
  { "en": "Apa Itu Kontroler PID?", "id": "Kontroler Umpan Balik (Proporsional, Integral, Derivatif)." },
  { "en": "Apa Itu Aksi Proporsional (P)?", "id": "Kontrol Berdasarkan Error Saat Ini." },
  { "en": "Apa Itu Aksi Integral (I)?", "id": "Kontrol Berdasarkan Akumulasi Error Lalu." },
  { "en": "Apa Itu Aksi Derivatif (D)?", "id": "Kontrol Berdasarkan Laju Perubahan Error." },
  { "en": "Fungsi Aksi Integral (I)?", "id": "Menghilangkan Error Steady-State." },
  { "en": "Fungsi Aksi Derivatif (D)?", "id": "Meredam Osilasi (Antisipasi)." },
  { "en": "Apa Itu Root Locus (Lokasi Akar)?", "id": "Metode Grafis Analisis Kestabilan." },
  { "en": "Apa Itu State-Space (Ruang Keadaan)?", "id": "Model Sistem (Persamaan Diferensial)." },
  { "en": "Apa Itu Kontrol Digital?", "id": "Sistem Kontrol Diimplementasi Digital." },
  { "en": "Apa Itu Transformasi-Z?", "id": "Metode Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu Kontrol Adaptif?", "id": "Kontroler Yang Menyesuaikan Parameter." },
  { "en": "Apa Itu Kontrol Fuzzy Logic?", "id": "Kontrol Berbasis Logika Kabur (Linguistik)." },
  { "en": "Apa Itu Kontrol Jaringan Saraf?", "id": "Kontrol Berbasis Model Jaringan Saraf." },
  { "en": "Apa Itu Robotika?", "id": "Ilmu Teknologi Desain Robot." },
  { "en": "Apa Itu Sensor (Robot)?", "id": "Pendeteksi Lingkungan (Mata, Telinga)." },
  { "en": "Apa Itu Aktuator (Robot)?", "id": "Penggerak Robot (Motor, Otot)." },
  { "en": "Apa Itu Kontroler (Robot)?", "id": "Otak Pemroses Pengambil Keputusan." },
  { "en": "Apa Itu Lengan Robot Industri?", "id": "Robot Manipulator Di Pabrik." },
  { "en": "Apa Itu Derajat Kebebasan (DOF)?", "id": "Jumlah Sumbu Gerakan Independen." },
  { "en": "Apa Itu Kinematika (Robot)?", "id": "Ilmu Gerakan Geometris Robot." },
  { "en": "Apa Itu Kinematika Maju (Forward)?", "id": "Menghitung Posisi Ujung (Dari Sendi)." },
  { "en": "Apa Itu Kinematika Mundur (Inverse)?", "id": "Menghitung Sendi (Dari Posisi Ujung)." },
  { "en": "Apa Itu Dinamika (Robot)?", "id": "Ilmu Gerakan Robot (Melibatkan Gaya)." },
  { "en": "Apa Itu Perencanaan Jalur (Path Planning)?", "id": "Menentukan Jalur Gerak Robot (A Ke B)." },
  { "en": "Apa Itu Penghindaran Rintangan?", "id": "Kemampuan Robot Menghindari Tabrakan." },
  { "en": "Apa Itu Robot Bergerak (Mobile Robot)?", "id": "Robot Beroda (Bisa Berpindah)." },
  { "en": "Apa Itu AGV (Automated Guided Vehicle)?", "id": "Robot Bergerak Pengikut Jalur (Gudang)." },
  { "en": "Apa Itu SLAM (Simultaneous Localization Mapping)?", "id": "Robot Memetakan Sambil Menentukan Lokasi." },
  { "en": "Apa Itu UAV (Unmanned Aerial Vehicle)?", "id": "Pesawat Tanpa Awak (Drone)." },
  { "en": "Apa Itu Humanoid (Robot)?", "id": "Robot Berbentuk Seperti Manusia." },
  { "en": "Apa Itu HMI (Human-Machine Interface)?", "id": "Antarmuka Interaksi Manusia Mesin." },
  { "en": "Apa Itu Komunikasi Data?", "id": "Proses Pengiriman Penerimaan Data Digital." },
  { "en": "Apa Itu Sinyal (Signal)?", "id": "Besaran Fisik Pembawa Informasi." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu (Nilai Berkelanjutan)." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit (Nilai 0 Dan 1)." },
  { "en": "Apa Itu Bit (Binary Digit)?", "id": "Satuan Informasi Digital Terkecil." },
  { "en": "Apa Itu Byte?", "id": "Kumpulan Delapan Bit (8 Bit)." },
  { "en": "Apa Itu Bit Rate (Laju Bit)?", "id": "Jumlah Bit Yang Dikirim Per Detik." },
  { "en": "Apa Satuan Bit Rate?", "id": "Bit Per Detik (Bps)." },
  { "en": "Apa Itu Baud Rate (Laju Baud)?", "id": "Jumlah Perubahan Simbol Per Detik." },
  { "en": "Apa Itu Bandwidth (Lebar Pita)?", "id": "Rentang Frekuensi Yang Dapat Dilewatkan." },
  { "en": "Apa Satuan Bandwidth (Lebar Pita)?", "id": "Hertz (Hz)." },
  { "en": "Apa Itu Kanal (Channel) Komunikasi?", "id": "Media Fisik Transmisi Sinyal." },
  { "en": "Contoh Kanal (Channel) Komunikasi?", "id": "Kabel Tembaga, Serat Optik, Udara." },
  { "en": "Apa Itu Transmisi Simplex?", "id": "Komunikasi Satu Arah Saja (Radio)." },
  { "en": "Apa Itu Transmisi Half-Duplex?", "id": "Komunikasi Dua Arah (Bergantian)." },
  { "en": "Apa Itu Transmisi Full-Duplex?", "id": "Komunikasi Dua Arah (Bersamaan)." },
  { "en": "Contoh Transmisi Half-Duplex?", "id": "Walkie-Talkie." },
  { "en": "Contoh Transmisi Full-Duplex?", "id": "Telepon." },
  { "en": "Apa Itu Komunikasi Serial?", "id": "Pengiriman Data Bit Per Bit (Satu Jalur)." },
  { "en": "Apa Itu Komunikasi Paralel?", "id": "Pengiriman Data Banyak Bit (Banyak Jalur)." },
  { "en": "Keuntungan Komunikasi Serial?", "id": "Lebih Sedikit Kabel, Jarak Jauh." },
  { "en": "Keuntungan Komunikasi Paralel?", "id": "Kecepatan Transfer Data Lebih Tinggi." },
  { "en": "Apa Itu Komunikasi Sinkron (Synchronous)?", "id": "Transmisi Data Menggunakan Sinyal Clock." },
  { "en": "Apa Itu Komunikasi Asinkron (Asynchronous)?", "id": "Transmisi Data Tanpa Sinyal Clock." },
  { "en": "Bagaimana Sinkronisasi Komunikasi Asinkron?", "id": "Menggunakan Start Bit Dan Stop Bit." },
  { "en": "Contoh Komunikasi Asinkron?", "id": "Protokol Serial UART (RS-232)." },
  { "en": "Apa Itu Protokol (Protokol Komunikasi)?", "id": "Aturan Standar Dalam Komunikasi Data." },
  { "en": "Apa Itu Noise (Deras) Komunikasi?", "id": "Gangguan Sinyal Yang Merusak Data." },
  { "en": "Apa Itu Atenuasi (Attenuation)?", "id": "Pelemahan Sinyal Seiring Jarak." },
  { "en": "Apa Itu Distorsi (Distortion)?", "id": "Perubahan Bentuk Sinyal Asli." },
  { "en": "Apa Itu Pengecekan Error (Error Checking)?", "id": "Metode Deteksi Kesalahan Data." },
  { "en": "Apa Itu Parity Bit (Bit Paritas)?", "id": "Bit Tambahan Untuk Deteksi Error." },
  { "en": "Apa Itu Paritas Ganjil (Odd Parity)?", "id": "Jumlah Total Bit 1 Harus Ganjil." },
  { "en": "Apa Itu Paritas Genap (Even Parity)?", "id": "Jumlah Total Bit 1 Harus Genap." },
  { "en": "Apa Itu Checksum?", "id": "Metode Deteksi Error (Penjumlahan Data)." },
  { "en": "Apa Itu CRC (Cyclic Redundancy Check)?", "id": "Metode Deteksi Error (Lebih Kuat)." },
  { "en": "Apa Itu Koreksi Error (Error Correction)?", "id": "Metode Memperbaiki Data Yang Rusak." },
  { "en": "Apa Itu Kode Hamming?", "id": "Contoh Kode Koreksi Error." },
  { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Sinyal Data Ke Sinyal Pembawa." },
  { "en": "Kenapa Perlu Modulasi?", "id": "Transmisi Efisien, Frekuensi Berbeda." },
  { "en": "Apa Itu Demodulasi?", "id": "Memisahkan Sinyal Data Dari Pembawa." },
  { "en": "Apa Itu Modem (Modulator-Demodulator)?", "id": "Perangkat Modulasi Dan Demodulasi." },
  { "en": "Apa Itu Modulasi Digital?", "id": "Modulasi Sinyal Digital (ASK, FSK, PSK)." },
  { "en": "Apa Itu ASK (Amplitude Shift Keying)?", "id": "Modulasi Digital (Beda Amplitudo)." },
  { "en": "Apa Itu FSK (Frequency Shift Keying)?", "id": "Modulasi Digital (Beda Frekuensi)." },
  { "en": "Apa Itu PSK (Phase Shift Keying)?", "id": "Modulasi Digital (Beda Fasa)." },
  { "en": "Apa Itu BPSK (Binary Phase Shift Keying)?", "id": "PSK Dengan Dua Fasa (0, 180 Derajat)." },
  { "en": "Apa Itu QPSK (Quadrature Phase Shift Keying)?", "id": "PSK Dengan Empat Fasa (2 Bit)." },
  { "en": "Apa Itu QAM (Quadrature Amplitude Modulation)?", "id": "Gabungan Modulasi Amplitudo Dan Fasa." },
  { "en": "Apa Itu Diagram Konstelasi?", "id": "Grafik Titik Simbol Modulasi Digital." },
  { "en": "Apa Itu Multiplexing (Multipleksi)?", "id": "Berbagi Satu Saluran Untuk Banyak Sinyal." },
  { "en": "Apa Itu FDM (Frequency Division Multiplexing)?", "id": "Multipleksi Berbasis Pembagian Pita Frekuensi." },
  { "en": "Contoh FDM (Frequency Division Multiplexing)?", "id": "Siaran Radio FM, Siaran TV." },
  { "en": "Apa Itu TDM (Time Division Multiplexing)?", "id": "Multipleksi Berbasis Pembagian Slot Waktu." },
  { "en": "Contoh TDM (Time Division Multiplexing)?", "id": "Jaringan Telepon Digital (PCM)." },
  { "en": "Apa Itu PCM (Pulse Code Modulation)?", "id": "Metode Konversi Analog Ke Digital (Telepon)." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Multipleksi FDM Untuk Serat Optik (Warna)." },
  { "en": "Apa Itu CDM (Code Division Multiplexing)?", "id": "Multipleksi Berbasis Kode Unik (CDMA)." },
  { "en": "Apa Itu CDMA (Code Division Multiple Access)?", "id": "Metode Akses Jaringan Seluler (Lama)." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Kabel Pasangan Berpilin (Ethernet, Telepon)." },
  { "en": "Kenapa Kabel Dipilin (Twisted)?", "id": "Untuk Mengurangi Interferensi (Crosstalk)." },
  { "en": "Apa Itu UTP (Unshielded Twisted Pair)?", "id": "Kabel Pilin Tanpa Pelindung Logam." },
  { "en": "Apa Itu STP (Shielded Twisted Pair)?", "id": "Kabel Pilin Dengan Pelindung Logam." },
  { "en": "Apa Itu Kabel Koaksial (Coaxial)?", "id": "Kabel (Inti Tengah, Isolator, Serabut)." },
  { "en": "Contoh Penggunaan Kabel Koaksial?", "id": "Antena TV, Jaringan Kabel (Lama)." },
  { "en": "Apa Itu Kabel Serat Optik?", "id": "Kabel Transmisi Data Menggunakan Cahaya." },
  { "en": "Apa Inti (Core) Serat Optik?", "id": "Bagian Tengah Kaca (Tempat Cahaya Lewat)." },
  { "en": "Apa Itu Cladding (Serat Optik)?", "id": "Lapisan Kaca Sekeliling Inti." },
  { "en": "Prinsip Kerja Serat Optik?", "id": "Pemantulan Internal Total Cahaya." },
  { "en": "Kelebihan Serat Optik?", "id": "Bandwidth Besar, Tahan Interferensi." },
  { "en": "Apa Itu Serat Optik Single-Mode?", "id": "Serat Optik (Inti Kecil, Jarak Jauh)." },
  { "en": "Apa Itu Serat Optik Multi-Mode?", "id": "Serat Optik (Inti Besar, Jarak Pendek)." },
  { "en": "Apa Itu Splicing (Serat Optik)?", "id": "Teknik Penyambungan Inti Serat Optik." },
  { "en": "Apa Itu OTDR (Optical Time Domain Reflectometer)?", "id": "Alat Tes Serat Optik (Deteksi Putus)." },
  { "en": "Apa Itu Jaringan Komputer?", "id": "Kumpulan Komputer Saling Terhubung." },
  { "en": "Apa Itu LAN (Local Area Network)?", "id": "Jaringan Area Lokal (Satu Gedung)." },
  { "en": "Apa Itu WAN (Wide Area Network)?", "id": "Jaringan Area Luas (Antar Kota, Internet)." },
  { "en": "Apa Itu MAN (Metropolitan Area Network)?", "id": "Jaringan Area Metropolitan (Satu Kota)." },
  { "en": "Apa Itu PAN (Personal Area Network)?", "id": "Jaringan Area Personal (Bluetooth)." },
  { "en": "Apa Itu Internet?", "id": "Jaringan Global (Kumpulan WAN)." },
  { "en": "Apa Itu Intranet?", "id": "Jaringan Internal Perusahaan (Privat)." },
  { "en": "Apa Itu Extranet?", "id": "Intranet Yang Dibuka Untuk Pihak Luar." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Tata Letak Fisik Jaringan." },
  { "en": "Apa Itu Topologi Bus?", "id": "Semua Terhubung Ke Satu Kabel Utama." },
  { "en": "Apa Itu Topologi Ring (Cincin)?", "id": "Semua Terhubung Membentuk Lingkaran." },
  { "en": "Apa Itu Topologi Star (Bintang)?", "id": "Semua Terhubung Ke Hub/Switch Pusat." },
  { "en": "Topologi Apa Paling Umum Saat Ini?", "id": "Topologi Star (Bintang)." },
  { "en": "Apa Itu Topologi Mesh?", "id": "Semua Perangkat Saling Terhubung (Redundan)." },
  { "en": "Apa Itu Model Referensi OSI?", "id": "Model Konseptual Jaringan Tujuh Lapis." },
  { "en": "Apa Itu Model TCP/IP?", "id": "Model Protokol Jaringan (Empat Lapis)." },
  { "en": "Apa Itu Lapis Fisik (Physical)?", "id": "Lapis OSI (Kabel, Sinyal Listrik)." },
  { "en": "Apa Itu Lapis Tautan Data (Data Link)?", "id": "Lapis OSI (MAC Address, Error Control)." },
  { "en": "Apa Itu Lapis Jaringan (Network)?", "id": "Lapis OSI (IP Address, Routing)." },
  { "en": "Apa Itu Lapis Transport (Transport)?", "id": "Lapis OSI (TCP, UDP, Port)." },
  { "en": "Apa Itu Lapis Aplikasi (Application)?", "id": "Lapis OSI (HTTP, FTP, DNS)." },
  { "en": "Apa Itu MAC (Media Access Control) Address?", "id": "Alamat Fisik Unik Perangkat Jaringan." },
  { "en": "Apa Itu IP (Internet Protocol) Address?", "id": "Alamat Logika Perangkat Di Jaringan." },
  { "en": "Apa Itu IPv4 (Internet Protocol Version 4)?", "id": "Alamat IP 32-Bit (Format Lama)." },
  { "en": "Apa Itu IPv6 (Internet Protocol Version 6)?", "id": "Alamat IP 128-Bit (Format Baru)." },
  { "en": "Apa Itu Subnet Mask?", "id": "Penentu Bagian Jaringan Host Alamat IP." },
  { "en": "Apa Itu Gateway (Gerbang Jaringan)?", "id": "Perangkat Penghubung Jaringan Lokal Internet." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
