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


  { "en": "Apa Perbedaan Dari 'Frekuensi' Dan 'Bandwidth'?", "id": "Frekuensi Adalah Kecepatan Getaran, Bandwidth Rentang Frekuensi." },
  { "en": "Apa Bedanya 'Arus AC' Dan 'DC'?", "id": "Arus AC Bolak-Balik, Sedangkan DC Searah." },
  { "en": "Apa Perbedaan 'Rangkaian Seri' Dan 'Paralel'?", "id": "Seri Berderet, Sedangkan Paralel Bercabang." },
  { "en": "Apa Bedanya 'Konduktor' Dan 'Isolator'?", "id": "Konduktor Menghantarkan Listrik, Isolator Tidak." },
  { "en": "Bedanya 'Tegangan' Dan 'Arus'?", "id": "Tegangan Mendorong, Arus Adalah Aliran Muatan Listrik." },
  { "en": "Bedanya 'Motor Induksi' Dan 'Sinkron'?", "id": "Motor Induksi Butuh Slip, Motor Sinkron Tidak." },
  { "en": "Bedanya 'Transformator Step-Up' Dan 'Step-Down'?", "id": "Step-Up Menaikkan Tegangan, Step-Down Menurunkan Tegangan." },
  { "en": "Bedanya 'Dioda' Dan 'Transistor'?", "id": "Dioda Punya Satu Sambungan, Transistor Dua." },
  { "en": "Bedanya 'Kapasitor' Dan 'Induktor'?", "id": "Kapasitor Simpan Energi Listrik, Induktor Energi Magnet." },
  { "en": "Bedanya 'Sinyal Analog' Dan 'Digital'?", "id": "Sinyal Analog Kontinu, Sinyal Digital Diskrit." },
  { "en": "Bedanya 'Sensor' Dan 'Aktuator'?", "id": "Sensor Mendeteksi Lingkungan, Aktuator Menggerakkan Sistem." },
  { "en": "Bedanya 'Daya Aktif' Dan 'Reaktif'?", "id": "Daya Aktif Kerja Nyata, Reaktif Daya Imajiner." },
  { "en": "Bedanya 'Voltmeter' Dan 'Amperemeter'?", "id": "Voltmeter Mengukur Tegangan, Amperemeter Mengukur Arus." },
  { "en": "Bedanya 'Resistansi' Dan 'Reaktansi'?", "id": "Resistansi Hambatan Nyata, Reaktansi Hambatan Imajiner." },
  { "en": "Bedanya 'PLC' Dan 'Mikrokontroler'?", "id": "PLC Untuk Industri, Mikrokontroler Untuk Sistem Tertanam." },
  { "en": "Bedanya 'Generator' Dan 'Motor'?", "id": "Generator Hasilkan Listrik, Motor Gunakan Listrik." },
  { "en": "Bedanya 'Open Loop' Dan 'Closed Loop'?", "id": "Closed Loop Ada Umpan Balik, Open Loop Tidak." },
  { "en": "Bedanya 'Inverter' Dan 'Konverter'?", "id": "Inverter Ubah DC Ke AC, Konverter Sebaliknya." },
  { "en": "Bedanya 'MOSFET' Dan 'BJT'?", "id": "MOSFET Dikontrol Tegangan, BJT Dikontrol Arus." },
  { "en": "Bedanya 'RAM' Dan 'ROM'?", "id": "RAM Volatil, ROM Non-Volatil." },
  { "en": "Bedanya 'Filter Aktif' Dan 'Pasif'?", "id": "Filter Aktif Butuh Daya, Pasif Tidak." },
  { "en": "Bedanya 'Osilator' Dan 'Resonator'?", "id": "Osilator Hasilkan Gelombang, Resonator Bergetar Di Frekuensi Tertentu." },
  { "en": "Bedanya 'Amplifier' Dan 'Attenuator'?", "id": "Amplifier Menguatkan Sinyal, Attenuator Melemahkan Sinyal." },
  { "en": "Bedanya 'Modulasi AM' Dan 'FM'?", "id": "AM Modulasi Amplitudo, FM Modulasi Frekuensi." },
  { "en": "Bedanya 'Multiplexer' Dan 'Demultiplexer'?", "id": "Multiplexer Menggabungkan Sinyal, Demultiplexer Memisahkan Sinyal." },
  { "en": "Bedanya 'Encoder' Dan 'Decoder'?", "id": "Encoder Mengubah Data, Decoder Mengembalikan Data." },
  { "en": "Bedanya 'Half-Wave' Dan 'Full-Wave Rectifier'?", "id": "Full-Wave Lebih Efisien Dari Half-Wave." },
  { "en": "Bedanya 'Baterai Primer' Dan 'Sekunder'?", "id": "Baterai Primer Sekali Pakai, Sekunder Bisa Diisi Ulang." },
  { "en": "Bedanya 'Sekring' Dan 'MCB'?", "id": "Sekring Putus, MCB Trip Bisa Direset." },
  { "en": "Bedanya 'Grounding' Dan 'Bonding'?", "id": "Grounding Ke Bumi, Bonding Antar Peralatan." },
  { "en": "Bedanya 'Surge Arrester' Dan 'Lightning Rod'?", "id": "Surge Arrester Di Sirkuit, Lightning Rod Di Gedung." },
  { "en": "Bedanya 'Insulator Pin' Dan 'Suspension'?", "id": "Pin Untuk Tegangan Rendah, Suspension Tegangan Tinggi." },
  { "en": "Bedanya 'Kabel UTP' Dan 'STP'?", "id": "STP Punya Pelindung, UTP Tidak." },
  { "en": "Bedanya 'Biner' Dan 'Heksadesimal'?", "id": "Biner Basis 2, Heksadesimal Basis 16." },
  { "en": "Bedanya 'Latch' Dan 'Flip-Flop'?", "id": "Flip-Flop Dikontrol Clock, Latch Tidak." },
  { "en": "Bedanya 'Counter Sinkron' Dan 'Asinkron'?", "id": "Sinkron Serempak, Asinkron Berurutan." },
  { "en": "Bedanya 'Register' Dan 'Memori'?", "id": "Register Lebih Cepat Dari Memori." },
  { "en": "Bedanya 'ADC' Dan 'DAC'?", "id": "ADC Ubah Analog Ke Digital, DAC Sebaliknya." },
  { "en": "Bedanya 'OP-AMP' Dan 'Komparator'?", "id": "OP-AMP Menguatkan, Komparator Membandingkan." },
  { "en": "Bedanya 'Termokopel' Dan 'Termistor'?", "id": "Termokopel Dari Dua Logam, Termistor Semikonduktor." },
  { "en": "Bedanya 'Strain Gauge' Dan 'Load Cell'?", "id": "Strain Gauge Ukur Regangan, Load Cell Ukur Beban." },
  { "en": "Bedanya 'LED' Dan 'LCD'?", "id": "LED Pancarkan Cahaya, LCD Memblokir Cahaya." },
  { "en": "Bedanya 'Motor Stepper' Dan 'Servo'?", "id": "Stepper Bergerak Per Langkah, Servo Per Posisi." },
  { "en": "Bedanya 'Relay' Dan 'Kontaktor'?", "id": "Relay Untuk Arus Kecil, Kontaktor Arus Besar." },
  { "en": "Bedanya 'Thyristor' Dan 'Triac'?", "id": "Thyristor Searah, Triac Dua Arah." },
  { "en": "Bedanya 'IGBT' Dan 'GTO'?", "id": "IGBT Lebih Cepat Dari GTO." },
  { "en": "Bedanya 'Rectifier' Dan 'Inverter'?", "id": "Rectifier Ubah AC Ke DC, Inverter Sebaliknya." },
  { "en": "Bedanya 'PWM' Dan 'PFM'?", "id": "PWM Lebar Pulsa Bervariasi, PFM Frekuensi Bervariasi." },
  { "en": "Bedanya 'Buck' Dan 'Boost Converter'?", "id": "Buck Turunkan Tegangan, Boost Naikkan Tegangan." },
  { "en": "Bedanya 'Sistem Linier' Dan 'Non-linier'?", "id": "Sistem Linier Proporsional, Non-Linier Tidak." },
  { "en": "Bedanya 'Fourier Series' Dan 'Transform'?", "id": "Series Untuk Periodik, Transform Untuk Non-Periodik." },
  { "en": "Bedanya 'Laplace' Dan 'Z Transform'?", "id": "Laplace Untuk Kontinu, Z Untuk Diskrit." },
  { "en": "Bedanya 'Jaringan Delta' Dan 'Wye'?", "id": "Keduanya Konfigurasi Tiga Fasa." },
  { "en": "Bedanya 'Impedansi' Dan 'Admitansi'?", "id": "Admitansi Adalah Kebalikan Dari Impedansi." },
  { "en": "Bedanya 'Node' Dan 'Mesh Analysis'?", "id": "Node Berbasis KCL, Mesh Berbasis KVL." },
  { "en": "Bedanya 'Superposisi' Dan 'Thevenin'?", "id": "Superposisi Per Sumber, Thevenin Rangkaian Ekuivalen." },
  { "en": "Bedanya 'Norton' Dan 'Thevenin'?", "id": "Norton Ekuivalen Arus, Thevenin Ekuivalen Tegangan." },
  { "en": "Bedanya 'Medan Listrik' Dan 'Magnet'?", "id": "Medan Listrik Dari Muatan, Magnet Dari Arus." },
  { "en": "Bedanya 'Hukum Ohm' Dan 'Kirchhoff'?", "id": "Hukum Ohm V=IR, Kirchhoff Tentang Arus Dan Tegangan." },
  { "en": "Bedanya 'Daya' Dan 'Energi'?", "id": "Daya Laju Energi, Energi Kapasitas Kerja." },
  { "en": "Bedanya 'Efisiensi' Dan 'Faktor Daya'?", "id": "Efisiensi Output/Input, Faktor Daya Daya Nyata/Semu." },
  { "en": "Bedanya 'Arus Bocor' Dan 'Arus Sisa'?", "id": "Arus Bocor Ke Ground, Arus Sisa Selisih Fasa-Netral." },
  { "en": "Bedanya 'Short Circuit' Dan 'Open Circuit'?", "id": "Short Circuit Arus Tak Terbatas, Open Circuit Tak Ada Arus." },
  { "en": "Bedanya 'Overload' Dan 'Overcurrent'?", "id": "Overload Kelebihan Beban, Overcurrent Arus Berlebih." },
  { "en": "Bedanya 'Feromagnetik' Dan 'Paramagnetik'?", "id": "Feromagnetik Sangat Magnetis, Paramagnetik Sedikit." },
  { "en": "Bedanya 'Histeresis' Dan 'Arus Eddy'?", "id": "Histeresis Rugi Pembalikan, Arus Eddy Rugi Pusaran." },
  { "en": "Bedanya 'Konduktansi' Dan 'Susceptansi'?", "id": "Keduanya Bagian Dari Admitansi." },
  { "en": "Bedanya 'Ripple' Dan 'Noise'?", "id": "Ripple Dari Penyearahan, Noise Dari Interferensi." },
  { "en": "Bedanya 'Atenuasi' Dan 'Distorsi'?", "id": "Atenuasi Pelemahan, Distorsi Perubahan Bentuk Sinyal." },
  { "en": "Bedanya 'Simetris' Dan 'Asimetris'?", "id": "Simetris Seimbang, Asimetris Tidak Seimbang." },
  { "en": "Bedanya 'Fasa' Dan 'Netral'?", "id": "Fasa Bertegangan, Netral Titik Referensi." },
  { "en": "Bedanya 'Galvanometer' Dan 'Amperemeter'?", "id": "Galvanometer Sangat Sensitif, Amperemeter Untuk Arus Lebih Besar." },
  { "en": "Bedanya 'Osiloskop' Dan 'Multimeter'?", "id": "Osiloskop Tampilkan Gelombang, Multimeter Tampilkan Nilai." },
  { "en": "Bedanya 'Sistem Satu Fasa' Dan 'Tiga Fasa'?", "id": "Tiga Fasa Lebih Efisien Dari Satu Fasa." },
  { "en": "Bedanya 'Belitan Primer' Dan 'Sekunder'?", "id": "Primer Terima Daya, Sekunder Keluarkan Daya." },
  { "en": "Bedanya 'Inti Besi' Dan 'Inti Udara'?", "id": "Inti Besi Perkuat Medan, Inti Udara Tidak." },
  { "en": "Bedanya 'Dioda Zener' Dan 'Varactor'?", "id": "Zener Untuk Tegangan, Varactor Untuk Kapasitansi." },
  { "en": "Bedanya 'Fotodioda' Dan 'Fototransistor'?", "id": "Fototransistor Punya Penguatan, Fotodioda Tidak." },
  { "en": "Bedanya 'Optocoupler' Dan 'Relay'?", "id": "Optocoupler Isolasi Optik, Relay Isolasi Mekanik." },
  { "en": "Bedanya 'SCR' Dan 'DIAC'?", "id": "SCR Butuh Pemicu Gerbang, DIAC Tidak." },
  { "en": "Bedanya 'UJT' Dan 'PUT'?", "id": "PUT Lebih Fleksibel Dari UJT." },
  { "en": "Bedanya 'C' Dan 'C++'?", "id": "C++ Berorientasi Objek, C Prosedural." },
  { "en": "Bedanya 'Compiler' Dan 'Interpreter'?", "id": "Compiler Terjemahkan Semua, Interpreter Baris Per Baris." },
  { "en": "Bedanya 'Algoritma' Dan 'Pseudocode'?", "id": "Algoritma Langkah Logis, Pseudocode Deskripsi Informal." },
  { "en": "Bedanya 'Debugging' Dan 'Testing'?", "id": "Debugging Cari Kesalahan, Testing Verifikasi Fungsi." },
  { "en": "Bedanya 'Serial' Dan 'Paralel Port'?", "id": "Serial Kirim Data Satu Per Satu, Paralel Serentak." },
  { "en": "Bedanya 'USB' Dan 'Ethernet'?", "id": "USB Untuk Periferal, Ethernet Untuk Jaringan." },
  { "en": "Bedanya 'Bluetooth' Dan 'Wi-Fi'?", "id": "Bluetooth Jarak Pendek, Wi-Fi Jarak Lebih Jauh." },
  { "en": "Bedanya 'CPU' Dan 'GPU'?", "id": "CPU Untuk Umum, GPU Untuk Grafis." },
  { "en": "Bedanya 'Antena Dipole' Dan 'Monopole'?", "id": "Dipole Dua Kutub, Monopole Satu Kutub." },
  { "en": "Bedanya 'Propagasi Gelombang Tanah' Dan 'Langit'?", "id": "Gelombang Tanah Ikuti Bumi, Langit Pantul Ionosfer." },
  { "en": "Bedanya 'SWR' Dan 'Return Loss'?", "id": "Keduanya Mengukur Pantulan Daya." },
  { "en": "Bedanya 'Gain' Dan 'Directivity Antena'?", "id": "Gain Perhitungkan Efisiensi, Directivity Tidak." },
  { "en": "Bedanya 'Microstrip' Dan 'Stripline'?", "id": "Microstrip Di Atas Substrat, Stripline Di Dalam." },
  { "en": "Bedanya 'Waveguide' Dan 'Coaxial Cable'?", "id": "Waveguide Untuk Frekuensi Sangat Tinggi." },
  { "en": "Bedanya 'S-parameter' Dan 'T-parameter'?", "id": "Keduanya Untuk Analisis Jaringan." },
  { "en": "Bedanya 'Smith Chart' Dan 'Polar Plot'?", "id": "Smith Chart Untuk Impedansi." },
  { "en": "Bedanya 'Common Emitter' Dan 'Common Collector'?", "id": "Common Collector Penguatan Arus, Emitter Penguatan Tegangan." },
  { "en": "Apa Bedanya 'Semikonduktor Intrinsik' Dan 'Ekstrinsik'?", "id": "Intrinsik Murni, Ekstrinsik Diberi Pengotor." },
  { "en": "Apa Perbedaan 'Semikonduktor Tipe-N' Dan 'Tipe-P'?", "id": "Tipe-N Kelebihan Elektron, Tipe-P Kelebihan Lubang." },
  { "en": "Apa Bedanya 'Bias Maju' Dan 'Bias Mundur'?", "id": "Bias Maju Menghantar Arus, Bias Mundur Menghambat." },
  { "en": "Apa Perbedaan 'Daerah Deplesi' Dan 'Daerah Saturasi'?", "id": "Deplesi Kosong Muatan, Saturasi Arus Maksimal." },
  { "en": "Bedanya 'Breakdown Zener' Dan 'Avalanche'?", "id": "Zener Pada Tegangan Rendah, Avalanche Tegangan Tinggi." },
  { "en": "Bedanya 'Logika Kombinasional' Dan 'Sekuensial'?", "id": "Sekuensial Punya Memori, Kombinasional Tidak." },
  { "en": "Bedanya 'Mikroprosesor' Dan 'Mikrokontroler'?", "id": "Mikroprosesor Butuh Komponen Eksternal, Mikrokontroler Terintegrasi." },
  { "en": "Bedanya 'Arsitektur Von Neumann' Dan 'Harvard'?", "id": "Harvard Pisahkan Memori Data Dan Program." },
  { "en": "Bedanya 'Arsitektur RISC (Reduced Instruction Set Computer)' Dan 'CISC (Complex Instruction Set Computer)'?", "id": "RISC Instruksi Sederhana, CISC Instruksi Kompleks." },
  { "en": "Bedanya 'SRAM (Static Random-Access Memory)' Dan 'DRAM (Dynamic Random-Access Memory)'?", "id": "SRAM Lebih Cepat Dan Mahal Daripada DRAM." },
  { "en": "Bedanya 'EEPROM (Electrically Erasable Programmable Read-Only Memory)' Dan 'Flash Memory'?", "id": "Flash Dihapus Per Blok, EEPROM Per Byte." },
  { "en": "Bedanya 'Common Base' Dan 'Common Collector'?", "id": "Common Base Penguatan Tegangan, Collector Penguatan Arus." },
  { "en": "Bedanya 'Amplifier Kelas A' Dan 'Kelas B'?", "id": "Kelas A Bekerja Penuh, Kelas B Setengah Siklus." },
  { "en": "Bedanya 'Amplifier Kelas AB' Dan 'Kelas C'?", "id": "Kelas AB Kurangi Distorsi, Kelas C Sangat Efisien." },
  { "en": "Bedanya 'Umpan Balik Positif' Dan 'Negatif'?", "id": "Positif Untuk Osilasi, Negatif Untuk Stabilitas." },
  { "en": "Bedanya 'Low-Pass Filter' Dan 'High-Pass Filter'?", "id": "Low-Pass Loloskan Frekuensi Rendah, High-Pass Sebaliknya." },
  { "en": "Bedanya 'Band-Pass Filter' Dan 'Band-Stop Filter'?", "id": "Band-Pass Loloskan Rentang, Band-Stop Blokir Rentang." },
  { "en": "Bedanya 'Time Domain' Dan 'Frequency Domain'?", "id": "Time Domain Sinyal Terhadap Waktu, Frequency Domain Terhadap Frekuensi." },
  { "en": "Bedanya 'Sampling' Dan 'Quantizing'?", "id": "Sampling Ambil Sampel Waktu, Quantizing Ambil Level Amplitudo." },
  { "en": "Bedanya 'Aliasing' Dan 'Leakage'?", "id": "Aliasing Kesalahan Sampling, Leakage Dari Jendela Sinyal." },
  { "en": "Bedanya 'PSK (Phase-Shift Keying)' Dan 'FSK (Frequency-Shift Keying)'?", "id": "PSK Modulasi Fasa, FSK Modulasi Frekuensi." },
  { "en": "Bedanya 'ASK (Amplitude-Shift Keying)' Dan 'QAM (Quadrature Amplitude Modulation)'?", "id": "ASK Modulasi Amplitudo, QAM Amplitudo Dan Fasa." },
  { "en": "Bedanya 'TDM (Time-Division Multiplexing)' Dan 'FDM (Frequency-Division Multiplexing)'?", "id": "TDM Berbagi Waktu, FDM Berbagi Frekuensi." },
  { "en": "Bedanya 'CDMA (Code-Division Multiple Access)' Dan 'GSM (Global System for Mobile Communications)'?", "id": "CDMA Gunakan Kode Unik, GSM Gunakan Slot Waktu." },
  { "en": "Bedanya 'Protokol TCP (Transmission Control Protocol)' Dan 'UDP (User Datagram Protocol)'?", "id": "TCP Andal Tapi Lambat, UDP Cepat Tapi Tidak Andal." },
  { "en": "Bedanya 'Alamat IP (Internet Protocol)' Dan 'MAC (Media Access Control)'?", "id": "Alamat IP Logis Dan Berubah, Alamat MAC Fisik Dan Tetap." },
  { "en": "Bedanya 'Hub' Dan 'Switch'?", "id": "Hub Meneruskan Ke Semua, Switch Ke Tujuan Spesifik." },
  { "en": "Bedanya 'Router' Dan 'Bridge'?", "id": "Router Hubungkan Jaringan Berbeda, Bridge Jaringan Sejenis." },
  { "en": "Bedanya 'Permitivitas' Dan 'Permeabilitas'?", "id": "Permitivitas Medan Listrik, Permeabilitas Medan Magnet." },
  { "en": "Bedanya 'Fluks Listrik' Dan 'Fluks Magnet'?", "id": "Fluks Listrik Dari Muatan, Fluks Magnet Dari Kutub." },
  { "en": "Bedanya 'Induktansi Diri' Dan 'Induktansi Bersama'?", "id": "Induktansi Diri Internal, Bersama Antar Kumparan." },
  { "en": "Bedanya 'Medan Dekat' Dan 'Medan Jauh'?", "id": "Medan Dekat Reaktif, Medan Jauh Radiatif." },
  { "en": "Bedanya 'Antena Isotropik' Dan 'Direksional'?", "id": "Isotropik Semua Arah, Direksional Arah Tertentu." },
  { "en": "Bedanya 'Polarisasi Linier' Dan 'Sirkular'?", "id": "Linier Satu Bidang, Sirkular Berputar." },
  { "en": "Bedanya 'SCADA (Supervisory Control and Data Acquisition)' Dan 'DCS (Distributed Control System)'?", "id": "SCADA Untuk Supervisi Luas, DCS Untuk Kontrol Proses." },
  { "en": "Bedanya 'HMI (Human-Machine Interface)' Dan 'GUI (Graphical User Interface)'?", "id": "HMI Spesifik Industri, GUI Lebih Umum." },
  { "en": "Bedanya 'Kontroler PID (Proportional-Integral-Derivative)' Dan 'On-Off'?", "id": "PID Kontrol Halus, On-Off Kontrol Kasar." },
  { "en": "Bedanya 'Logika Fuzzy' Dan 'Jaringan Saraf'?", "id": "Fuzzy Berbasis Aturan, Jaringan Saraf Belajar Dari Data." },
  { "en": "Bedanya 'Motor Rotor Sangkar' Dan 'Rotor Belitan'?", "id": "Rotor Sangkar Sederhana, Rotor Belitan Torsi Awal Tinggi." },
  { "en": "Bedanya 'Trafo Tipe Inti' Dan 'Tipe Cangkang'?", "id": "Tipe Cangkang Fluks Bocor Lebih Kecil." },
  { "en": "Bedanya 'Starting Bintang' Dan 'Segitiga'?", "id": "Bintang Arus Awal Rendah, Segitiga Torsi Awal Tinggi." },
  { "en": "Bedanya 'Pembangkit Listrik Termal' Dan 'Hidro'?", "id": "Termal Dari Panas, Hidro Dari Air." },
  { "en": "Bedanya 'Pembangkit Listrik Tenaga Nuklir' Dan 'Surya'?", "id": "Nuklir Dari Fisi, Surya Dari Foton." },
  { "en": "Bedanya 'Jaringan Distribusi Radial' Dan 'Cincin'?", "id": "Cincin Lebih Andal Daripada Radial." },
  { "en": "Bedanya 'Busbar Tunggal' Dan 'Ganda'?", "id": "Busbar Ganda Lebih Fleksibel Dan Andal." },
  { "en": "Bedanya 'Pemutus Sirkuit Udara' Dan 'Vakum'?", "id": "Vakum Media Isolasi Lebih Baik." },
  { "en": "Bedanya 'Pemutus Sirkuit Minyak' Dan 'SF6 (Sulfur Hexafluoride)'?", "id": "SF6 Tidak Mudah Terbakar, Minyak Mudah." },
  { "en": "Bedanya 'Trafo Arus' Dan 'Trafo Tegangan'?", "id": "Trafo Arus Ukur Arus, Trafo Tegangan Ukur Tegangan." },
  { "en": "Bedanya 'Faktor Puncak' Dan 'Faktor Bentuk'?", "id": "Keduanya Mendeskripsikan Bentuk Gelombang AC." },
  { "en": "Bedanya 'Harmonisa' Dan 'Interharmonisa'?", "id": "Harmonisa Kelipatan Bulat, Interharmonisa Bukan." },
  { "en": "Bedanya 'Skalar' Dan 'Vektor'?", "id": "Skalar Punya Nilai, Vektor Nilai Dan Arah." },
  { "en": "Bedanya 'Kuat Arus' Dan 'Rapat Arus'?", "id": "Rapat Arus Adalah Arus Per Satuan Luas." },
  { "en": "Bedanya 'Potensial Listrik' Dan 'Tegangan'?", "id": "Tegangan Adalah Beda Potensial Listrik." },
  { "en": "Bedanya 'Dielektrik' Dan 'Isolator'?", "id": "Dielektrik Dapat Terpolarisasi, Isolator Tidak." },
  { "en": "Bedanya 'Reluktansi' Dan 'Resistansi'?", "id": "Reluktansi Hambatan Magnetik, Resistansi Hambatan Listrik." },
  { "en": "Bedanya 'Gaya Gerak Listrik (GGL)' Dan 'Tegangan Terminal'?", "id": "GGL Sumber Ideal, Tegangan Terminal Terukur." },
  { "en": "Bedanya 'Konveksi' Dan 'Konduksi'?", "id": "Konveksi Melalui Fluida, Konduksi Melalui Kontak Langsung." },
  { "en": "Bedanya 'Radiasi' Dan 'Konveksi'?", "id": "Radiasi Melalui Gelombang, Konveksi Melalui Aliran." },
  { "en": "Bedanya 'Elektron' Dan 'Proton'?", "id": "Elektron Bermuatan Negatif, Proton Bermuatan Positif." },
  { "en": "Bedanya 'Elektron' Dan 'Hole'?", "id": "Elektron Pembawa Muatan, Hole Kekosongan Elektron." },
  { "en": "Bedanya 'Pita Valensi' Dan 'Pita Konduksi'?", "id": "Pita Konduksi Energinya Lebih Tinggi Dari Valensi." },
  { "en": "Bedanya 'Doping' Dan 'Difusi'?", "id": "Doping Penambahan Impuritas, Difusi Pergerakan Partikel." },
  { "en": "Bedanya 'JFET (Junction Field-Effect Transistor)' Dan 'MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)'?", "id": "JFET Punya Gerbang PN, MOSFET Terisolasi." },
  { "en": "Bedanya 'Depletion MOSFET' Dan 'Enhancement MOSFET'?", "id": "Depletion Normal On, Enhancement Normal Off." },
  { "en": "Bedanya 'CMRR (Common-Mode Rejection Ratio)' Dan 'PSRR (Power-Supply Rejection Ratio)'?", "id": "CMRR Tolak Sinyal Common, PSRR Tolak Noise Suplai." },
  { "en": "Bedanya 'Slew Rate' Dan 'Bandwidth'?", "id": "Slew Rate Kecepatan Tegangan, Bandwidth Rentang Frekuensi." },
  { "en": "Bedanya 'Schottky Diode' Dan 'PN Junction Diode'?", "id": "Schottky Lebih Cepat, Tegangan Turun Lebih Rendah." },
  { "en": "Bedanya 'LED (Light Emitting Diode)' Dan 'Dioda Laser'?", "id": "Laser Koheren Dan Monokromatik, LED Tidak." },
  { "en": "Bedanya 'Kopling AC (Alternating Current)' Dan 'Kopling DC (Direct Current)'?", "id": "Kopling AC Blokir DC, Kopling DC Loloskan Semua." },
  { "en": "Bedanya 'Akurasi' Dan 'Presisi'?", "id": "Akurasi Kedekatan Nilai Benar, Presisi Kedekatan Nilai Ukur." },
  { "en": "Bedanya 'Resolusi' Dan 'Sensitivitas'?", "id": "Resolusi Perubahan Terkecil, Sensitivitas Rasio Output-Input." },
  { "en": "Bedanya 'Error Statis' Dan 'Error Dinamis'?", "id": "Statis Saat Diam, Dinamis Saat Berubah." },
  { "en": "Bedanya 'Transduser' Dan 'Sensor'?", "id": "Transduser Ubah Energi, Sensor Deteksi Besaran Fisik." },
  { "en": "Bedanya 'Piezoelektrik' Dan 'Piroelektrik'?", "id": "Piezoelektrik Dari Tekanan, Piroelektrik Dari Suhu." },
  { "en": "Bedanya 'Sistem Orde Satu' Dan 'Orde Dua'?", "id": "Orde Dua Dapat Berosilasi, Orde Satu Tidak." },
  { "en": "Bedanya 'Pole' Dan 'Zero'?", "id": "Pole Tentukan Stabilitas, Zero Tentukan Respon Frekuensi." },
  { "en": "Bedanya 'Bode Plot' Dan 'Nyquist Plot'?", "id": "Keduanya Menganalisis Respon Frekuensi Dan Stabilitas." },
  { "en": "Bedanya 'Root Locus' Dan 'Bode Plot'?", "id": "Root Locus Tunjukkan Lokasi Pole Dengan Gain." },
  { "en": "Bedanya 'Phase Margin' Dan 'Gain Margin'?", "id": "Keduanya Adalah Ukuran Stabilitas Relatif Sistem." },
  { "en": "Bedanya 'Lead Compensator' Dan 'Lag Compensator'?", "id": "Lead Perbaiki Respon Transien, Lag Perbaiki Error Steady-State." },
  { "en": "Bedanya 'State-Space' Dan 'Transfer Function'?", "id": "State-Space Untuk Sistem MIMO (Multiple-Input Multiple-Output), Transfer Function SISO (Single-Input Single-Output)." },
  { "en": "Bedanya 'Observabilitas' Dan 'Kontrolabilitas'?", "id": "Kontrolabilitas Kemampuan Mengontrol, Observabilitas Kemampuan Mengamati." },
  { "en": "Bedanya 'Forward Kinematics' Dan 'Inverse Kinematics'?", "id": "Forward Dari Sendi Ke Posisi, Inverse Sebaliknya." },
  { "en": "Bedanya 'Robot Serial' Dan 'Paralel'?", "id": "Serial Rantai Terbuka, Paralel Rantai Tertutup." },
  { "en": "Bedanya 'Firmware' Dan 'Software'?", "id": "Firmware Tertanam Di Hardware, Software Diinstal." },
  { "en": "Bedanya 'Interrupt' Dan 'Polling'?", "id": "Interrupt Dipicu Hardware, Polling Dicek Software." },
  { "en": "Bedanya 'DMA (Direct Memory Access)' Dan 'CPU (Central Processing Unit) Transfer'?", "id": "DMA Transfer Data Tanpa Melibatkan CPU." },
  { "en": "Bedanya 'Cache' Dan 'Register'?", "id": "Register Di Dalam CPU, Cache Di Antara CPU Dan RAM." },
  { "en": "Bedanya 'Stack' Dan 'Heap'?", "id": "Stack Statis Dikelola Otomatis, Heap Dinamis Dikelola Manual." },
  { "en": "Bedanya 'Big-Endian' Dan 'Little-Endian'?", "id": "Big-Endian Simpan Byte Terbesar Dulu, Little-Endian Sebaliknya." },
  { "en": "Bedanya 'Simulasi' Dan 'Emulasi'?", "id": "Simulasi Modelkan Perilaku, Emulasi Tiru Fungsi." },
  { "en": "Bedanya 'Verifikasi' Dan 'Validasi'?", "id": "Verifikasi Benar Dibuat, Validasi Buat Hal Benar." },
  { "en": "Bedanya 'Solenoida' Dan 'Toroida'?", "id": "Solenoida Lurus, Toroida Melingkar." },
  { "en": "Bedanya 'Hukum Ampere' Dan 'Biot-Savart'?", "id": "Ampere Untuk Simetris, Biot-Savart Untuk Umum." },
  { "en": "Bedanya 'Hukum Gauss' Dan 'Hukum Coulomb'?", "id": "Hukum Gauss Lebih Umum Dari Hukum Coulomb." },
  { "en": "Bedanya 'Arus Konduksi' Dan 'Arus Perpindahan'?", "id": "Konduksi Aliran Muatan, Perpindahan Perubahan Medan Listrik." },
  { "en": "Bedanya 'Kondisi Batas' Dan 'Nilai Awal'?", "id": "Kondisi Batas Di Ruang, Nilai Awal Di Waktu." },
  { "en": "Apa Bedanya 'Arus Drift' Dan 'Arus Difusi'?", "id": "Drift Karena Medan Listrik, Difusi Karena Konsentrasi." },
  { "en": "Apa Perbedaan 'Rekombinasi' Dan 'Generasi'?", "id": "Rekombinasi Menyatukan, Generasi Memisahkan Elektron-Hole." },
  { "en": "Bedanya 'Semikonduktor Celah Pita Langsung' Dan 'Tak Langsung'?", "id": "Langsung Memancarkan Cahaya Efisien, Tak Langsung Tidak." },
  { "en": "Bedanya 'Waktu Setup' Dan 'Waktu Hold'?", "id": "Setup Waktu Sebelum Clock, Hold Waktu Setelah Clock." },
  { "en": "Bedanya 'Mesin Mealy' Dan 'Mesin Moore'?", "id": "Output Mealy Tergantung Input, Moore Hanya State." },
  { "en": "Bedanya 'Gerbang Logika AND' Dan 'OR'?", "id": "AND Benar Jika Semua Benar, OR Cukup Satu Benar." },
  { "en": "Bedanya 'Gerbang Logika NAND' Dan 'NOR'?", "id": "NAND Inversi Dari AND, NOR Inversi Dari OR." },
  { "en": "Bedanya 'Gerbang Logika XOR (Exclusive OR)' Dan 'XNOR (Exclusive NOR)'?", "id": "XOR Benar Jika Ganjil, XNOR Benar Jika Sama." },
  { "en": "Bedanya 'Reset Sinkron' Dan 'Asinkron'?", "id": "Sinkron Aktif Sesuai Clock, Asinkron Kapan Saja." },
  { "en": "Bedanya 'Tundaan Propagasi' Dan 'Tundaan Kontaminasi'?", "id": "Propagasi Maksimal, Kontaminasi Minimal Waktu Tunda." },
  { "en": "Bedanya 'Komunikasi Simplex' Dan 'Half-Duplex'?", "id": "Simplex Satu Arah, Half-Duplex Bergantian Dua Arah." },
  { "en": "Bedanya 'Komunikasi Half-Duplex' Dan 'Full-Duplex'?", "id": "Half-Duplex Bergantian, Full-Duplex Bersamaan Dua Arah." },
  { "en": "Bedanya 'Sinyal Baseband' Dan 'Broadband'?", "id": "Baseband Satu Kanal, Broadband Banyak Kanal." },
  { "en": "Bedanya 'Bit Rate' Dan 'Baud Rate'?", "id": "Bit Rate Jumlah Bit, Baud Rate Jumlah Simbol." },
  { "en": "Bedanya 'Filter FIR (Finite Impulse Response)' Dan 'IIR (Infinite Impulse Response)'?", "id": "FIR Selalu Stabil, IIR Bisa Tidak Stabil." },
  { "en": "Bedanya 'Konvolusi' Dan 'Korelasi'?", "id": "Konvolusi Output Sistem, Korelasi Kemiripan Sinyal." },
  { "en": "Bedanya 'DFT (Discrete Fourier Transform)' Dan 'FFT (Fast Fourier Transform)'?", "id": "FFT Adalah Algoritma Cepat Untuk Menghitung DFT." },
  { "en": "Bedanya 'Tegangan Fasa' Dan 'Tegangan Antar Fasa'?", "id": "Tegangan Fasa Ke Netral, Antar Fasa Ke Fasa Lain." },
  { "en": "Bedanya 'Beban Seimbang' Dan 'Tak Seimbang'?", "id": "Beban Seimbang Impedansi Sama, Tak Seimbang Berbeda." },
  { "en": "Bedanya 'Urutan Fasa Positif' Dan 'Negatif'?", "id": "Urutan Menentukan Arah Putaran Medan Magnet." },
  { "en": "Bedanya 'Isolator' Dan 'Pemutus Sirkuit'?", "id": "Isolator Tanpa Beban, Pemutus Sirkuit Dengan Beban." },
  { "en": "Bedanya 'Relay Diferensial' Dan 'Relay Arus Lebih'?", "id": "Diferensial Sangat Cepat, Arus Lebih Ada Tundaan." },
  { "en": "Bedanya 'Tegangan Sag' Dan 'Swell'?", "id": "Sag Penurunan Tegangan Sesaat, Swell Kenaikan." },
  { "en": "Bedanya 'Blackout' Dan 'Brownout'?", "id": "Blackout Mati Total, Brownout Penurunan Tegangan." },
  { "en": "Bedanya 'Efek Kulit' Dan 'Efek Kedekatan'?", "id": "Efek Kulit Internal, Efek Kedekatan Antar Konduktor." },
  { "en": "Bedanya 'Gelombang Berdiri' Dan 'Gelombang Berjalan'?", "id": "Gelombang Berdiri Tidak Transfer Energi, Berjalan Transfer." },
  { "en": "Bedanya 'Koefisien Pantul' Dan 'Koefisien Transmisi'?", "id": "Pantul Gelombang Kembali, Transmisi Gelombang Diteruskan." },
  { "en": "Bedanya 'Mode TE (Transverse Electric)' Dan 'TM (Transverse Magnetic)'?", "id": "TE Medan Listrik Tegak Lurus, TM Medan Magnet." },
  { "en": "Bedanya 'Potensiometer' Dan 'Rheostat'?", "id": "Potensiometer Pembagi Tegangan, Rheostat Pengatur Arus." },
  { "en": "Bedanya 'LVDT (Linear Variable Differential Transformer)' Dan 'RVDT (Rotary Variable Differential Transformer)'?", "id": "LVDT Ukur Perpindahan Linier, RVDT Ukur Sudut." },
  { "en": "Bedanya 'Hard Switching' Dan 'Soft Switching'?", "id": "Soft Switching Mengurangi Rugi-Rugi Pensaklaran." },
  { "en": "Bedanya 'ZVS (Zero Voltage Switching)' Dan 'ZCS (Zero Current Switching)'?", "id": "ZVS Saklar Saat Nol Volt, ZCS Saat Nol Arus." },
  { "en": "Bedanya 'Konverter Flyback' Dan 'Forward'?", "id": "Flyback Simpan Energi Di Induktor, Forward Tidak." },
  { "en": "Bedanya 'Respon Transien' Dan 'Respon Steady-State'?", "id": "Transien Respon Awal, Steady-State Respon Akhir." },
  { "en": "Bedanya 'Rasio Redaman' Dan 'Frekuensi Alami'?", "id": "Rasio Redaman Tentukan Osilasi, Frekuensi Alami Kecepatan Osilasi." },
  { "en": "Bedanya 'Pipelining' Dan 'Pemrosesan Paralel'?", "id": "Pipelining Tumpang Tindih Instruksi, Paralel Eksekusi Serentak." },
  { "en": "Bedanya 'Siklus Instruksi' Dan 'Siklus Mesin'?", "id": "Siklus Instruksi Terdiri Dari Beberapa Siklus Mesin." },
  { "en": "Bedanya 'Bahan Diamagnetik' Dan 'Paramagnetik'?", "id": "Diamagnetik Menolak Medan, Paramagnetik Sedikit Tertarik." },
  { "en": "Bedanya 'Bahan Magnet Keras' Dan 'Lunak'?", "id": "Keras Jadi Magnet Permanen, Lunak Jadi Temporer." },
  { "en": "Bedanya 'Soldering' Dan 'Welding'?", "id": "Soldering Pakai Timah, Welding Melelehkan Logam Induk." },
  { "en": "Bedanya 'PCB (Printed Circuit Board)' Dan 'Breadboard'?", "id": "Breadboard Untuk Prototipe, PCB Untuk Produk Final." },
  { "en": "Bedanya 'Komponen Through-Hole' Dan 'Surface-Mount'?", "id": "Through-Hole Tembus Papan, Surface-Mount Di Permukaan." },
  { "en": "Bedanya 'Unipolar' Dan 'Bipolar'?", "id": "Unipolar Satu Jenis Muatan, Bipolar Dua Jenis." },
  { "en": "Bedanya 'Sinyal Deterministik' Dan 'Acak'?", "id": "Deterministik Dapat Diprediksi, Acak Tidak." },
  { "en": "Bedanya 'Sinyal Periodik' Dan 'Aperiodik'?", "id": "Periodik Berulang, Aperiodik Tidak Berulang." },
  { "en": "Bedanya 'Sinyal Energi' Dan 'Sinyal Daya'?", "id": "Sinyal Energi Terbatas, Sinyal Daya Tak Terbatas." },
  { "en": "Bedanya 'Transformasi Diskrit' Dan 'Kontinu'?", "id": "Diskrit Untuk Sinyal Diskrit, Kontinu Untuk Sinyal Kontinu." },
  { "en": "Bedanya 'Jembatan Wheatstone' Dan 'Maxwell'?", "id": "Wheatstone Ukur Resistansi, Maxwell Ukur Induktansi." },
  { "en": "Bedanya 'Error Acak' Dan 'Sistematis'?", "id": "Acak Tidak Terduga, Sistematis Konsisten." },
  { "en": "Bedanya 'Noise Termal' Dan 'Shot Noise'?", "id": "Termal Dari Agitasi, Shot Dari Fluktuasi Diskrit." },
  { "en": "Bedanya 'Flicker Noise' Dan 'Burst Noise'?", "id": "Flicker Frekuensi Rendah, Burst Tiba-Tiba." },
  { "en": "Bedanya 'Kuantisasi Uniform' Dan 'Non-Uniform'?", "id": "Uniform Level Sama, Non-Uniform Level Berbeda." },
  { "en": "Bedanya 'Kode Manchester' Dan 'NRZ (Non-Return-to-Zero)'?", "id": "Manchester Punya Clock, NRZ Tidak." },
  { "en": "Bedanya 'Parity Bit' Dan 'CRC (Cyclic Redundancy Check)'?", "id": "CRC Lebih Andal Dalam Mendeteksi Error." },
  { "en": "Bedanya 'Hamming Code' Dan 'Parity Bit'?", "id": "Hamming Code Dapat Memperbaiki Error, Parity Tidak." },
  { "en": "Bedanya 'Topologi Bus' Dan 'Bintang'?", "id": "Bus Satu Kabel Utama, Bintang Terpusat." },
  { "en": "Bedanya 'Topologi Cincin' Dan 'Mesh'?", "id": "Mesh Punya Banyak Jalur, Cincin Satu Jalur." },
  { "en": "Bedanya 'OSI Model' Dan 'TCP/IP Model'?", "id": "Model OSI Tujuh Layer, TCP/IP Empat Layer." },
  { "en": "Bedanya 'IPv4' Dan 'IPv6'?", "id": "IPv6 Punya Ruang Alamat Jauh Lebih Besar." },
  { "en": "Bedanya 'Subnetting' Dan 'Supernetting'?", "id": "Subnetting Memecah Jaringan, Supernetting Menggabungkan." },
  { "en": "Bedanya 'DNS (Domain Name System)' Dan 'DHCP (Dynamic Host Configuration Protocol)'?", "id": "DNS Terjemahkan Nama, DHCP Berikan Alamat IP." },
  { "en": "Bedanya 'HTTP (Hypertext Transfer Protocol)' Dan 'HTTPS (Hypertext Transfer Protocol Secure)'?", "id": "HTTPS Terenkripsi Dan Aman, HTTP Tidak." },
  { "en": "Bedanya 'FTP (File Transfer Protocol)' Dan 'SFTP (Secure File Transfer Protocol)'?", "id": "SFTP Versi Aman Dari FTP." },
  { "en": "Bedanya 'Email POP3 (Post Office Protocol 3)' Dan 'IMAP (Internet Message Access Protocol)'?", "id": "IMAP Sinkronisasi Antar Perangkat, POP3 Tidak." },
  { "en": "Bedanya 'Kriptografi Simetris' Dan 'Asimetris'?", "id": "Simetris Satu Kunci, Asimetris Dua Kunci." },
  { "en": "Bedanya 'Enkripsi' Dan 'Hashing'?", "id": "Enkripsi Bisa Dibalikkan, Hashing Tidak Bisa." },
  { "en": "Bedanya 'Firewall' Dan 'Antivirus'?", "id": "Firewall Lindungi Jaringan, Antivirus Lindungi Perangkat." },
  { "en": "Bedanya 'Autotransformer' Dan 'Transformator Biasa'?", "id": "Autotransformer Punya Satu Belitan, Biasa Punya Dua." },
  { "en": "Bedanya 'Arus Inrush' Dan 'Arus Starting'?", "id": "Inrush Sangat Sesaat, Starting Lebih Lama." },
  { "en": "Bedanya 'Proteksi Overload' Dan 'Short Circuit'?", "id": "Overload Termal, Short Circuit Magnetik." },
  { "en": "Bedanya 'Stator' Dan 'Rotor'?", "id": "Stator Bagian Diam, Rotor Bagian Berputar." },
  { "en": "Bedanya 'Belitan Lap' Dan 'Belitan Gelombang'?", "id": "Lap Untuk Arus Tinggi, Gelombang Untuk Tegangan Tinggi." },
  { "en": "Bedanya 'Komutator' Dan 'Slip Ring'?", "id": "Komutator Ubah AC Ke DC, Slip Ring Transfer Daya." },
  { "en": "Bedanya 'Motor DC Seri' Dan 'Shunt'?", "id": "Seri Torsi Awal Tinggi, Shunt Kecepatan Konstan." },
  { "en": "Bedanya 'Motor DC Kompon' Dan 'Seri'?", "id": "Kompon Gabungan Karakteristik Seri Dan Shunt." },
  { "en": "Bedanya 'Regulasi Tegangan' Dan 'Regulasi Kecepatan'?", "id": "Regulasi Tegangan Untuk Generator, Kecepatan Untuk Motor." },
  { "en": "Bedanya 'Faktor Distribusi' Dan 'Faktor Pitch'?", "id": "Keduanya Mengurangi GGL Induksi Pada Mesin AC." },
  { "en": "Bedanya 'Motor Sinkron' Dan 'Generator Sinkron'?", "id": "Motor Ubah Listrik Ke Mekanik, Generator Sebaliknya." },
  { "en": "Bedanya 'Kurva V' Dan 'Kurva V Terbalik'?", "id": "Kurva V Untuk Motor, Terbalik Untuk Generator." },
  { "en": "Bedanya 'Hunting' Dan 'Crawling'?", "id": "Hunting Osilasi Rotor, Crawling Putaran Lambat." },
  { "en": "Bedanya 'Motor Universal' Dan 'Motor DC'?", "id": "Motor Universal Bisa Beroperasi Di AC Dan DC." },
  { "en": "Bedanya 'Motor Histeresis' Dan 'Reluktansi'?", "id": "Histeresis Berputar Karena Rugi, Reluktansi Karena Beda Reluktansi." },
  { "en": "Bedanya 'Saklar SPST (Single Pole Single Throw)' Dan 'SPDT (Single Pole Double Throw)'?", "id": "SPST On-Off, SPDT Punya Dua Pilihan Output." },
  { "en": "Bedanya 'Saklar DPST (Double Pole Single Throw)' Dan 'DPDT (Double Pole Double Throw)'?", "id": "Double Pole Mengontrol Dua Sirkuit Sekaligus." },
  { "en": "Bedanya 'Varistor' Dan 'Termistor'?", "id": "Varistor Bergantung Tegangan, Termistor Bergantung Suhu." },
  { "en": "Bedanya 'Kapasitor Elektrolit' Dan 'Keramik'?", "id": "Elektrolit Punya Polaritas, Keramik Tidak." },
  { "en": "Bedanya 'Resistor Komposisi Karbon' Dan 'Film Logam'?", "id": "Film Logam Punya Toleransi Dan Stabilitas Lebih Baik." },
  { "en": "Bedanya 'Kode Warna Resistor' Dan 'Kode SMD (Surface-Mount Device)'?", "id": "Kode Warna Pakai Gelang, Kode SMD Pakai Angka." },
  { "en": "Bedanya 'Induktor Inti Udara' Dan 'Inti Ferit'?", "id": "Inti Ferit Punya Induktansi Lebih Tinggi." },
  { "en": "Bedanya 'Faktor Kualitas (Q Factor)' Dan 'Faktor Disipasi'?", "id": "Faktor Kualitas Adalah Kebalikan Dari Faktor Disipasi." },
  { "en": "Bedanya 'Sirkuit Tangki' Dan 'Sirkuit Penjepit'?", "id": "Sirkuit Tangki Beresonansi, Sirkuit Penjepit Geser Level DC." },
  { "en": "Bedanya 'Sirkuit Clipper' Dan 'Clamper'?", "id": "Clipper Memotong Sinyal, Clamper Menggeser Sinyal." },
  { "en": "Bedanya 'Schmitt Trigger' Dan 'Komparator'?", "id": "Schmitt Trigger Punya Histeresis Untuk Menghindari Noise." },
  { "en": "Bedanya 'Astable Multivibrator' Dan 'Monostable'?", "id": "Astable Tidak Stabil, Monostable Punya Satu Keadaan Stabil." },
  { "en": "Bedanya 'Monostable Multivibrator' Dan 'Bistable'?", "id": "Monostable Satu Keadaan Stabil, Bistable Dua Keadaan Stabil." },
  { "en": "Bedanya 'Phase-Locked Loop (PLL)' Dan 'Voltage-Controlled Oscillator (VCO)'?", "id": "VCO Adalah Bagian Dari Sirkuit PLL." },
  { "en": "Bedanya 'Virtual Ground' Dan 'Actual Ground'?", "id": "Virtual Ground Di OP-AMP, Actual Ground Referensi Nol Volt." },
  { "en": "Bedanya 'Bandwidth Sinyal Kecil' Dan 'Sinyal Besar'?", "id": "Bandwidth Sinyal Besar Dibatasi Oleh Slew Rate." },
  { "en": "Apa Bedanya 'Regulator Linier' Dan 'Switching'?", "id": "Linier Kurang Efisien, Switching Sangat Efisien." },
  { "en": "Bedanya 'Mode Konduksi Kontinu (CCM)' Dan 'Diskontinu (DCM)'?", "id": "CCM Arus Induktor Tidak Pernah Nol." },
  { "en": "Bedanya 'FPGA (Field-Programmable Gate Array)' Dan 'ASIC (Application-Specific Integrated Circuit)'?", "id": "FPGA Dapat Diprogram Ulang, ASIC Dirancang Khusus." },
  { "en": "Bedanya 'VHDL (VHSIC Hardware Description Language)' Dan 'Verilog'?", "id": "Keduanya Bahasa Deskripsi Perangkat Keras Populer." },
  { "en": "Bedanya 'Dioda Terowongan (Tunnel)' Dan 'Dioda Zener'?", "id": "Dioda Terowongan Punya Daerah Resistansi Negatif." },
  { "en": "Bedanya 'Dioda PIN' Dan 'Dioda PN'?", "id": "PIN Punya Lapisan Intrinsik Di Tengah." },
  { "en": "Bedanya 'Sel Surya' Dan 'Fotodioda'?", "id": "Sel Surya Ubah Cahaya Jadi Daya, Fotodioda Jadi Sinyal." },
  { "en": "Bedanya 'Penganalisis Spektrum' Dan 'Osiloskop'?", "id": "Spektrum Domain Frekuensi, Osiloskop Domain Waktu." },
  { "en": "Bedanya 'Penganalisis Logika' Dan 'Osiloskop'?", "id": "Penganalisis Logika Untuk Sinyal Digital, Osiloskop Analog." },
  { "en": "Bedanya 'Generator Fungsi' Dan 'Generator Gelombang Arbitrer (AWG)'?", "id": "AWG Dapat Menghasilkan Bentuk Gelombang Kustom." },
  { "en": "Bedanya 'Pengereman Regeneratif' Dan 'Dinamik'?", "id": "Regeneratif Kembalikan Energi, Dinamik Buang Jadi Panas." },
  { "en": "Bedanya 'Slip' Dan 'Kecepatan Slip'?", "id": "Slip Persentase, Kecepatan Slip Selisih Putaran." },
  { "en": "Bedanya 'Belitan Satu Lapis' Dan 'Dua Lapis'?", "id": "Dua Lapis Lebih Fleksibel Dan Umum Digunakan." },
  { "en": "Bedanya 'Efek Ferranti' Dan 'Efek Corona'?", "id": "Ferranti Kenaikan Tegangan, Corona Pelepasan Muatan." },
  { "en": "Bedanya 'Gangguan Simetris' Dan 'Asimetris'?", "id": "Simetris Melibatkan Tiga Fasa, Asimetris Tidak." },
  { "en": "Bedanya 'Bus Ayun (Swing)' Dan 'Bus Beban (Load)'?", "id": "Bus Ayun Seimbangkan Daya, Bus Beban Konsumsi Daya." },
  { "en": "Bedanya 'White Noise' Dan 'Colored Noise'?", "id": "White Noise Punya Kepadatan Spektral Datar." },
  { "en": "Bedanya 'Interferensi Antar Simbol (ISI)' Dan 'Crosstalk'?", "id": "ISI Gangguan Internal, Crosstalk Gangguan Eksternal." },
  { "en": "Bedanya 'Teorema Shannon-Hartley' Dan 'Teorema Nyquist'?", "id": "Shannon Kapasitas Kanal, Nyquist Laju Data Maksimum." },
  { "en": "Bedanya 'Windowing' Dan 'Filtering'?", "id": "Windowing Memilih Segmen, Filtering Memodifikasi Spektrum." },
  { "en": "Bedanya 'Upsampling' Dan 'Downsampling'?", "id": "Upsampling Menambah Sampel, Downsampling Mengurangi Sampel." },
  { "en": "Bedanya 'Desimasi' Dan 'Interpolasi'?", "id": "Desimasi Mengurangi Laju, Interpolasi Meningkatkan Laju." },
  { "en": "Bedanya 'Stabilitas' Dan 'Robustness'?", "id": "Stabilitas Kondisi Internal, Robustness Terhadap Gangguan Eksternal." },
  { "en": "Bedanya 'Antena Yagi-Uda' Dan 'Log-Periodik'?", "id": "Yagi Bekerja Di Frekuensi Sempit, Log-Periodik Lebar." },
  { "en": "Bedanya 'Metastabilitas' Dan 'Race Condition'?", "id": "Metastabilitas Keadaan Tak Tentu, Race Condition Hasil Tak Terduga." },
  { "en": "Bedanya 'TTL (Transistor-Transistor Logic)' Dan 'CMOS (Complementary Metal-Oxide-Semiconductor)'?", "id": "CMOS Konsumsi Daya Lebih Rendah Dari TTL." },
  { "en": "Bedanya 'ECL (Emitter-Coupled Logic)' Dan 'TTL (Transistor-Transistor Logic)'?", "id": "ECL Jauh Lebih Cepat Tetapi Boros Daya." },
  { "en": "Bedanya 'LVDS (Low-Voltage Differential Signaling)' Dan 'CML (Current-Mode Logic)'?", "id": "LVDS Gunakan Sinyal Diferensial Tegangan Rendah." },
  { "en": "Bedanya 'Sirkuit Snubber' Dan 'Crowbar'?", "id": "Snubber Redam Lonjakan, Crowbar Lindungi Dari Tegangan Lebih." },
  { "en": "Bedanya 'EMI (Electromagnetic Interference)' Dan 'EMC (Electromagnetic Compatibility)'?", "id": "EMI Adalah Gangguan, EMC Kemampuan Bertahan Dari Gangguan." },
  { "en": "Bedanya 'ESD (Electrostatic Discharge)' Dan 'Lonjakan (Surge)'?", "id": "ESD Durasi Sangat Cepat, Lonjakan Lebih Lambat." },
  { "en": "Bedanya 'Nilai RMS (Root Mean Square)' Dan 'Rata-Rata (Average)'?", "id": "RMS Berkaitan Dengan Daya, Rata-Rata Nilai DC." },
  { "en": "Bedanya 'Magnetisasi Longitudinal' Dan 'Perpendikular'?", "id": "Arah Momen Magnetik Relatif Terhadap Permukaan." },
  { "en": "Bedanya 'Koersivitas' Dan 'Retentivitas'?", "id": "Koersivitas Resistansi Demagnetisasi, Retentivitas Sisa Magnetisasi." },
  { "en": "Bedanya 'Protokol I2C' Dan 'SPI'?", "id": "SPI Lebih Cepat Dan Full-Duplex, I2C Lebih Sederhana." },
  { "en": "Bedanya 'Protokol UART' Dan 'USART'?", "id": "USART Mendukung Mode Sinkron, UART Hanya Asinkron." },
  { "en": "Bedanya 'Kalibrasi' Dan 'Verifikasi'?", "id": "Kalibrasi Menyesuaikan, Verifikasi Memeriksa." },
  { "en": "Bedanya 'Transien' Dan 'Steady State'?", "id": "Transien Kondisi Peralihan, Steady State Kondisi Stabil." },
  { "en": "Bedanya 'Resonansi Seri' Dan 'Paralel'?", "id": "Seri Impedansi Minimum, Paralel Impedansi Maksimum." },
  { "en": "Bedanya 'Desibel (dB)' Dan 'dBm'?", "id": "dB Rasio Relatif, dBm Relatif Terhadap Satu Miliwatt." },
  { "en": "Bedanya 'Sistem Kausal' Dan 'Non-Kausal'?", "id": "Kausal Tergantung Masa Lalu, Non-Kausal Tergantung Masa Depan." },
  { "en": "Bedanya 'Time Invariant' Dan 'Time Variant'?", "id": "Time Invariant Respon Tidak Berubah Dengan Waktu." },
  { "en": "Bedanya 'Konverter Single-Ended' Dan 'Push-Pull'?", "id": "Push-Pull Gunakan Dua Saklar, Single-Ended Satu." },
  { "en": "Bedanya 'Magnetoresistansi' Dan 'Magnetostriksi'?", "id": "Magnetoresistansi Ubah Resistansi, Magnetostriksi Ubah Bentuk." },
  { "en": "Bedanya 'Efek Hall' Dan 'Efek Seebeck'?", "id": "Efek Hall Dari Medan Magnet, Seebeck Dari Suhu." },
  { "en": "Bedanya 'Efek Peltier' Dan 'Efek Thomson'?", "id": "Peltier Transfer Panas, Thomson Serap/Lepas Panas." },
  { "en": "Bedanya 'Superkonduktor' Dan 'Konduktor Sempurna'?", "id": "Superkonduktor Juga Mengeluarkan Medan Magnet (Efek Meissner)." },
  { "en": "Bedanya 'Transmisi Isokron' Dan 'Asinkron'?", "id": "Isokron Jaminan Waktu Tiba, Asinkron Tidak." },
  { "en": "Bedanya 'Handshaking' Dan 'Strobing'?", "id": "Handshaking Dua Arah, Strobing Satu Arah." },
  { "en": "Bedanya 'Look-Up Table (LUT)' Dan 'Gerbang Logika'?", "id": "LUT Di FPGA Implementasikan Fungsi Logika Apapun." },
  { "en": "Bedanya 'Slicing' Dan 'Routing' Dalam FPGA?", "id": "Slicing Implementasi Logika, Routing Menghubungkan." },
  { "en": "Bedanya 'Arus Konvensional' Dan 'Aliran Elektron'?", "id": "Konvensional Dari Positif Ke Negatif, Elektron Sebaliknya." },
  { "en": "Bedanya 'Daya Semu (Apparent)' Dan 'Daya Kompleks'?", "id": "Daya Semu Magnitudo, Daya Kompleks Vektor." },
  { "en": "Bedanya 'Segitiga Daya' Dan 'Segitiga Impedansi'?", "id": "Keduanya Merepresentasikan Hubungan Fasor." },
  { "en": "Bedanya 'Faktor Keragaman (Diversity)' Dan 'Faktor Beban'?", "id": "Keragaman Terkait Beban Puncak, Beban Terkait Rata-Rata." },
  { "en": "Bedanya 'Jarak Bebas (Clearance)' Dan 'Jarak Rambat (Creepage)'?", "id": "Jarak Bebas Lewat Udara, Rambat Lewat Permukaan." },
  { "en": "Bedanya 'Pengujian Tipe (Type Test)' Dan 'Rutin (Routine Test)'?", "id": "Tipe Uji Desain, Rutin Uji Setiap Produk." },
  { "en": "Bedanya 'Kelas Isolasi' Dan 'Kenaikan Suhu'?", "id": "Kelas Isolasi Batas Maksimal, Kenaikan Suhu Batas Operasi." },
  { "en": "Bedanya 'Motor Brushless DC (BLDC)' Dan 'Brushed DC'?", "id": "BLDC Tidak Punya Sikat, Lebih Awet Dan Efisien." },
  { "en": "Bedanya 'Kontrol Vektor' Dan 'Kontrol Skalar'?", "id": "Kontrol Vektor Lebih Presisi Untuk Motor AC." },
  { "en": "Bedanya 'Encoder Absolut' Dan 'Inkremental'?", "id": "Absolut Tahu Posisi, Inkremental Tahu Perpindahan." },
  { "en": "Bedanya 'Resolver' Dan 'Encoder'?", "id": "Resolver Bersifat Analog, Encoder Bersifat Digital." },
  { "en": "Bedanya 'Sistem Waktu-Nyata Keras (Hard)' Dan 'Lunak (Soft)'?", "id": "Keras Batas Waktu Ketat, Lunak Boleh Terlambat." },
  { "en": "Bedanya 'Watchdog Timer' Dan 'Timer Biasa'?", "id": "Watchdog Mereset Sistem Jika Terjadi Kegagalan." },
  { "en": "Bedanya 'Floating Point' Dan 'Fixed Point'?", "id": "Floating Point Lebih Fleksibel, Fixed Point Lebih Cepat." },
  { "en": "Bedanya 'Sistem Operasi (OS)' Dan 'Kernel'?", "id": "Kernel Adalah Inti Dari Sistem Operasi." },
  { "en": "Bedanya 'Thread' Dan 'Process'?", "id": "Thread Berbagi Memori, Process Tidak." },
  { "en": "Bedanya 'Mutex' Dan 'Semaphore'?", "id": "Mutex Kepemilikan Tunggal, Semaphore Banyak." },
  { "en": "Bedanya 'Deadlock' Dan 'Starvation'?", "id": "Deadlock Saling Menunggu, Starvation Terus Ditunda." },
  { "en": "Bedanya 'Analisis Statis' Dan 'Dinamis'?", "id": "Statis Tanpa Eksekusi, Dinamis Saat Eksekusi." },
  { "en": "Bedanya 'Kopling (Coupling)' Dan 'Kohesi (Cohesion)'?", "id": "Kopling Antar Modul, Kohesi Dalam Modul." },
  { "en": "Bedanya 'Top-Down Design' Dan 'Bottom-Up Design'?", "id": "Top-Down Dari Umum Ke Khusus, Bottom-Up Sebaliknya." },
  { "en": "Bedanya 'Lampu Pijar' Dan 'Lampu Fluorescent'?", "id": "Pijar Dari Pemanasan, Fluorescent Dari Gas." },
  { "en": "Bedanya 'Lampu Fluorescent' Dan 'LED (Light Emitting Diode)'?", "id": "LED Jauh Lebih Efisien Dan Tahan Lama." },
  { "en": "Bedanya 'Lumen' Dan 'Lux'?", "id": "Lumen Total Cahaya, Lux Cahaya Per Area." },
  { "en": "Bedanya 'Indeks Renderasi Warna (CRI)' Dan 'Suhu Warna'?", "id": "CRI Kualitas Warna, Suhu Warna Nuansa Cahaya." },
  { "en": "Bedanya 'Ballast Elektronik' Dan 'Magnetik'?", "id": "Elektronik Lebih Efisien, Tidak Berkedip." },
  { "en": "Bedanya 'Sistem Grid-Tie' Dan 'Off-Grid'?", "id": "Grid-Tie Terhubung Ke Jaringan, Off-Grid Mandiri." },
  { "en": "Bedanya 'Panel Surya Monokristalin' Dan 'Polikristalin'?", "id": "Monokristalin Lebih Efisien, Polikristalin Lebih Murah." },
  { "en": "Bedanya 'MPPT (Maximum Power Point Tracking)' Dan 'PWM (Pulse Width Modulation) Charge Controller'?", "id": "MPPT Lebih Canggih Dan Efisien Daripada PWM." },
  { "en": "Bedanya 'Baterai Asam Timbal (Lead-Acid)' Dan 'Lithium-Ion'?", "id": "Lithium-Ion Lebih Ringan, Awet, Dan Mahal." },
  { "en": "Bedanya 'Siklus Dalam (Deep Cycle)' Dan 'Siklus Dangkal (Shallow Cycle)'?", "id": "Deep Cycle Dirancang Untuk Pelepasan Muatan Besar." },
  { "en": "Bedanya 'Kapasitas Baterai (Ah)' Dan 'Energi Baterai (Wh)'?", "id": "Energi Adalah Kapasitas Dikalikan Dengan Tegangan." },
  { "en": "Bedanya 'Keadaan Terisi (State of Charge)' Dan 'Kedalaman Pelepasan (Depth of Discharge)'?", "id": "Keduanya Saling Melengkapi, Totalnya Seratus Persen." },
  { "en": "Bedanya 'Garis Beban DC' Dan 'Garis Beban AC'?", "id": "Garis Beban AC Punya Kemiringan Lebih Curam." },
  { "en": "Bedanya 'Titik Operasi (Q-Point)' Dan 'Daerah Aktif'?", "id": "Q-Point Titik Diam Di Dalam Daerah Aktif." },
  { "en": "Bedanya 'Saturasi' Dan 'Cut-Off'?", "id": "Saturasi Kondisi On Penuh, Cut-Off Kondisi Off Penuh." },
  { "en": "Bedanya 'Model Hibrida-Pi' Dan 'Model RE'?", "id": "Keduanya Model Sinyal Kecil Untuk Transistor BJT." },
  { "en": "Bedanya 'Efek Early' Dan 'Efek Miller'?", "id": "Efek Early Modulasi Lebar Basis, Miller Kapasitansi." },
  { "en": "Bedanya 'Differential Amplifier' Dan 'Operational Amplifier'?", "id": "Differential Amplifier Adalah Tahap Masukan Dari Op-Amp." },
  { "en": "Bedanya 'Positive Feedback' Dan 'Negative Feedback'?", "id": "Negatif Menstabilkan, Positif Menyebabkan Osilasi." },
  { "en": "Bedanya 'Sirkuit Terintegrasi (IC)' Dan 'Sirkuit Diskrit'?", "id": "IC Menggabungkan Banyak Komponen, Diskrit Terpisah." },
  { "en": "Bedanya 'Fabrikasi Wafer' Dan 'Pengemasan IC'?", "id": "Fabrikasi Membuat Sirkuit, Pengemasan Melindungi." },
  { "en": "Bedanya 'Litografi' Dan 'Etching'?", "id": "Litografi Membuat Pola, Etching Menghilangkan Material." },
  { "en": "Bedanya 'Hukum Moore' Dan 'Hukum Ohm'?", "id": "Hukum Moore Tentang Kepadatan Transistor, Ohm V=IR." },
  { "en": "Bedanya 'Quantum Computing' Dan 'Classical Computing'?", "id": "Quantum Menggunakan Qubit, Klasik Menggunakan Bit." },
  { "en": "Bedanya 'Superposisi' Dan 'Keterkaitan (Entanglement)'?", "id": "Keduanya Adalah Prinsip Dasar Komputasi Kuantum." },
  { "en": "Apa Bedanya 'Rise Time' Dan 'Settling Time'?", "id": "Rise Time Waktu Naik, Settling Time Waktu Stabil." },
  { "en": "Bedanya 'Overshoot' Dan 'Undershoot'?", "id": "Overshoot Melebihi Target, Undershoot Di Bawah Target." },
  { "en": "Bedanya 'Source Coding' Dan 'Channel Coding'?", "id": "Source Coding Kompresi Data, Channel Coding Tambah Redundansi." },
  { "en": "Bedanya 'Kode Blok' Dan 'Kode Konvolusional'?", "id": "Kode Blok Bekerja Pada Blok, Konvolusional Bekerja Berkelanjutan." },
  { "en": "Bedanya 'OFDM (Orthogonal Frequency-Division Multiplexing)' Dan 'FDM (Frequency-Division Multiplexing)'?", "id": "OFDM Sub-carrier Saling Tumpang Tindih Dan Ortogonal." },
  { "en": "Bedanya 'Duplexer' Dan 'Diplexer'?", "id": "Duplexer Memisahkan Frekuensi Dekat, Diplexer Frekuensi Jauh." },
  { "en": "Bedanya 'Noise Figure' Dan 'Noise Temperature'?", "id": "Keduanya Adalah Ukuran Kualitas Noise Suatu Sistem." },
  { "en": "Bedanya 'Sum of Products (SOP)' Dan 'Product of Sums (POS)'?", "id": "SOP Implementasi Minterm, POS Implementasi Maxterm." },
  { "en": "Bedanya 'Peta Karnaugh' Dan 'Aljabar Boolean'?", "id": "Peta Karnaugh Metode Grafis, Aljabar Boolean Matematis." },
  { "en": "Bedanya 'Memori Flash' Dan 'SSD (Solid-State Drive)'?", "id": "SSD Menggunakan Memori Flash Dengan Kontroler Canggih." },
  { "en": "Bedanya 'Economic Dispatch' Dan 'Optimal Power Flow'?", "id": "Optimal Power Flow Lebih Komprehensif Dari Economic Dispatch." },
  { "en": "Bedanya 'Stabilitas Tegangan' Dan 'Stabilitas Sudut Rotor'?", "id": "Stabilitas Tegangan Jaga Tegangan, Sudut Rotor Jaga Sinkronisasi." },
  { "en": "Bedanya 'Perangkat FACTS (Flexible AC Transmission Systems)' Dan 'Kompensator Tradisional'?", "id": "FACTS Berbasis Elektronika Daya, Lebih Cepat Dan Fleksibel." },
  { "en": "Bedanya 'Kompensasi Seri' Dan 'Shunt'?", "id": "Seri Mengurangi Reaktansi, Shunt Menyediakan Daya Reaktif." },
  { "en": "Bedanya 'Media Lossy' Dan 'Lossless'?", "id": "Lossy Melemahkan Gelombang, Lossless Tidak." },
  { "en": "Bedanya 'Konduktor Baik' Dan 'Dielektrik Baik'?", "id": "Konduktor Baik Rugi Konduksi, Dielektrik Baik Rugi Disipasi." },
  { "en": "Bedanya 'Vektor Poynting' Dan 'Kerapatan Daya'?", "id": "Vektor Poynting Punya Arah, Kerapatan Daya Magnitudonya." },
  { "en": "Bedanya 'Fluks Celah Udara' Dan 'Fluks Bocor'?", "id": "Fluks Celah Udara Bermanfaat, Fluks Bocor Merugikan." },
  { "en": "Bedanya 'Reaktansi Sinkron' Dan 'Reaktansi Bocor'?", "id": "Reaktansi Sinkron Gabungan Reaktansi Bocor Dan Reaksi Jangkar." },
  { "en": "Bedanya 'Motor PMDC (Permanent Magnet DC)' Dan 'DC Eksitasi Terpisah'?", "id": "PMDC Gunakan Magnet Permanen, Eksitasi Terpisah Butuh Suplai." },
  { "en": "Bedanya 'Sensor Efek Hall' Dan 'Sensor Induktif'?", "id": "Efek Hall Deteksi Magnet, Induktif Deteksi Logam." },
  { "en": "Bedanya 'Sensor Kapasitif' Dan 'Sensor Ultrasonik'?", "id": "Kapasitif Deteksi Objek Apapun, Ultrasonik Jarak." },
  { "en": "Bedanya 'RTD (Resistance Temperature Detector)' Dan 'Termokopel'?", "id": "RTD Lebih Akurat Dan Stabil, Termokopel Rentang Luas." },
  { "en": "Bedanya 'Fase Linier' Dan 'Fase Non-Linier'?", "id": "Fase Linier Tidak Merusak Bentuk Sinyal." },
  { "en": "Bedanya 'Autokorelasi' Dan 'Kros-Korelasi'?", "id": "Autokorelasi Dengan Diri Sendiri, Kros-Korelasi Dengan Sinyal Lain." },
  { "en": "Bedanya 'Tingkat Fermi' Dan 'Celah Pita Energi'?", "id": "Tingkat Fermi Probabilitas Elektron, Celah Pita Energi Terlarang." },
  { "en": "Bedanya 'Pembawa Mayoritas' Dan 'Minoritas'?", "id": "Mayoritas Dari Doping, Minoritas Dari Termal." },
  { "en": "Bedanya 'Kapasitor Bypass' Dan 'Kopling'?", "id": "Bypass Ke Ground, Kopling Antar Tahapan." },
  { "en": "Bedanya 'Resistor Pull-Up' Dan 'Pull-Down'?", "id": "Pull-Up Ke Tegangan Tinggi, Pull-Down Ke Ground." },
  { "en": "Bedanya 'Galvanic Isolation' Dan 'Optical Isolation'?", "id": "Keduanya Mencegah Aliran Arus Langsung." },
  { "en": "Bedanya 'Simbol Skematik' Dan 'Layout PCB'?", "id": "Skematik Logika Sirkuit, Layout Penempatan Fisik." },
  { "en": "Bedanya 'Via' Dan 'Pad' Pada PCB?", "id": "Via Menghubungkan Lapis, Pad Tempat Menyolder." },
  { "en": "Bedanya 'Solder Mask' Dan 'Silkscreen'?", "id": "Solder Mask Pelindung, Silkscreen Label Komponen." },
  { "en": "Bedanya 'Jitter' Dan 'Wander'?", "id": "Jitter Deviasi Cepat, Wander Deviasi Lambat." },
  { "en": "Bedanya 'Phase Noise' Dan 'Jitter'?", "id": "Phase Noise Di Domain Frekuensi, Jitter Domain Waktu." },
  { "en": "Bedanya 'Quantization Noise' Dan 'Thermal Noise'?", "id": "Quantization Dari Konversi, Thermal Dari Gerakan Elektron." },
  { "en": "Bedanya 'Intermodulasi' Dan 'Cross-Modulasi'?", "id": "Intermodulasi Hasilkan Frekuensi Baru, Cross-Modulasi Transfer Modulasi." },
  { "en": "Bedanya 'Blocking' Dan 'Desensitisasi'?", "id": "Keduanya Efek Sinyal Pengganggu Kuat Pada Penerima." },
  { "en": "Bedanya 'Spurious Emission' Dan 'Harmonic Emission'?", "id": "Harmonik Adalah Jenis Emisi Spurious." },
  { "en": "Bedanya 'Bandwidth 3dB' Dan 'Bandwidth Noise Ekuivalen'?", "id": "Bandwidth Noise Sedikit Lebih Lebar Dari 3dB." },
  { "en": "Bedanya 'Dynamic Range' Dan 'Spurious Free Dynamic Range (SFDR)'?", "id": "SFDR Rentang Operasi Bebas Dari Distorsi." },
  { "en": "Bedanya 'Titik Kompresi 1dB' Dan 'Titik Intersep Orde Tiga (IP3)'?", "id": "Keduanya Mengukur Batas Linieritas Penguat." },
  { "en": "Bedanya 'Mixer Aktif' Dan 'Pasif'?", "id": "Mixer Aktif Memberikan Penguatan, Pasif Tidak." },
  { "en": "Bedanya 'Konversi Atas' Dan 'Konversi Bawah'?", "id": "Konversi Atas Menaikkan Frekuensi, Bawah Menurunkan." },
  { "en": "Bedanya 'Superheterodyne' Dan 'Direct Conversion Receiver'?", "id": "Superheterodyne Gunakan Frekuensi Antara (IF), Direct Conversion Tidak." },
  { "en": "Bedanya 'Image Frequency' Dan 'Intermediate Frequency (IF)'?", "id": "Image Frequency Adalah Sinyal Palsu Yang Tidak Diinginkan." },
  { "en": "Bedanya 'Automatic Gain Control (AGC)' Dan 'Manual Gain Control'?", "id": "AGC Otomatis Menyesuaikan Penguatan, Manual Diatur Pengguna." },
  { "en": "Bedanya 'Pre-Emphasis' Dan 'De-Emphasis'?", "id": "Pre-Emphasis Menaikkan Sinyal, De-Emphasis Menurunkan Kembali." },
  { "en": "Bedanya 'Squelch' Dan 'Mute'?", "id": "Squelch Membungkam Noise, Mute Membungkam Semua Sinyal." },
  { "en": "Bedanya 'Eye Diagram' Dan 'Constellation Diagram'?", "id": "Eye Diagram Kualitas Sinyal, Constellation Modulasi Digital." },
  { "en": "Bedanya 'Error Vector Magnitude (EVM)' Dan 'Bit Error Rate (BER)'?", "id": "EVM Ukuran Akurasi Modulasi, BER Ukuran Kesalahan Data." },
  { "en": "Bedanya 'Lampu Sodium Tekanan Tinggi' Dan 'Tekanan Rendah'?", "id": "Tekanan Tinggi Warna Lebih Baik, Tekanan Rendah Sangat Efisien." },
  { "en": "Bedanya 'Lampu Metal Halide' Dan 'Merkuri'?", "id": "Metal Halide Lebih Efisien Dan Warna Lebih Baik." },
  { "en": "Bedanya 'Starter' Dan 'Ignitor'?", "id": "Starter Untuk Lampu Fluorescent, Ignitor Untuk Lampu HID." },
  { "en": "Bedanya 'Sistem IT' Dan 'Sistem TN' Dalam Grounding?", "id": "Sistem IT Terisolasi, Sistem TN Netral Terhubung Langsung." },
  { "en": "Bedanya 'ELCB (Earth Leakage Circuit Breaker)' Dan 'RCCB (Residual Current Circuit Breaker)'?", "id": "RCCB Adalah Tipe ELCB Yang Lebih Modern." },
  { "en": "Bedanya 'Kabel NYA' Dan 'NYM'?", "id": "NYA Satu Inti, NYM Banyak Inti Berisolasi." },
  { "en": "Bedanya 'Kabel NYY' Dan 'NYAF'?", "id": "NYY Untuk Instalasi Tetap, NYAF Fleksibel." },
  { "en": "Bedanya 'Konduktor Tembaga' Dan 'Aluminium'?", "id": "Tembaga Konduktivitas Lebih Baik, Aluminium Lebih Ringan." },
  { "en": "Bedanya 'Kelas A' Dan 'Kelas B' Dalam Proteksi Motor?", "id": "Kelas Menentukan Batas Suhu Maksimal." },
  { "en": "Bedanya 'Variable Frequency Drive (VFD)' Dan 'Soft Starter'?", "id": "VFD Atur Kecepatan, Soft Starter Hanya Meredam Arus Awal." },
  { "en": "Bedanya 'Daya Tahan (Endurance)' Dan 'Keandalan (Reliability)'?", "id": "Daya Tahan Umur Pakai, Keandalan Probabilitas Gagal." },
  { "en": "Bedanya 'Mean Time Between Failures (MTBF)' Dan 'Mean Time To Failure (MTTF)'?", "id": "MTBF Untuk Diperbaiki, MTTF Untuk Tidak Bisa Diperbaiki." },
  { "en": "Bedanya 'Fail-Safe' Dan 'Fail-Operable'?", "id": "Fail-Safe Mati Aman, Fail-Operable Tetap Berfungsi Terbatas." },
  { "en": "Bedanya 'Redundansi Aktif' Dan 'Pasif (Standby)'?", "id": "Aktif Bekerja Bersamaan, Pasif Mengambil Alih Saat Gagal." },
  { "en": "Bedanya 'Checksum' Dan 'CRC (Cyclic Redundancy Check)'?", "id": "CRC Jauh Lebih Kuat Dalam Mendeteksi Kesalahan." },
  { "en": "Bedanya 'Firmware Over-The-Air (FOTA)' Dan 'Software Over-The-Air (SOTA)'?", "id": "FOTA Memperbarui Firmware, SOTA Memperbarui Aplikasi." },
  { "en": "Bedanya 'Bus CAN (Controller Area Network)' Dan 'LIN (Local Interconnect Network)'?", "id": "CAN Lebih Cepat Dan Andal, LIN Lebih Murah." },
  { "en": "Bedanya 'FlexRay' Dan 'MOST (Media Oriented Systems Transport)'?", "id": "FlexRay Untuk Kontrol, MOST Untuk Multimedia." },
  { "en": "Bedanya 'Resonator Kristal' Dan 'Keramik'?", "id": "Kristal Jauh Lebih Akurat Dan Stabil Daripada Keramik." },
  { "en": "Bedanya 'Osilator Pierce' Dan 'Colpitts'?", "id": "Pierce Gunakan Kristal, Colpitts Gunakan Pembagi Kapasitif." },
  { "en": "Bedanya 'Osilator Hartley' Dan 'Clapp'?", "id": "Hartley Gunakan Pembagi Induktif, Clapp Modifikasi Colpitts." },
  { "en": "Bedanya 'Catu Daya Teregulasi' Dan 'Tidak Teregulasi'?", "id": "Teregulasi Menjaga Tegangan Output Tetap Stabil." },
  { "en": "Bedanya 'Ripple' Dan 'Regulasi Garis (Line Regulation)'?", "id": "Ripple Dari Penyearahan, Regulasi Garis Dari Input." },
  { "en": "Bedanya 'Regulasi Beban (Load Regulation)' Dan 'Regulasi Garis (Line Regulation)'?", "id": "Regulasi Beban Dari Perubahan Beban, Garis Dari Input." },
  { "en": "Bedanya 'Arus Diam (Quiescent)' Dan 'Arus Shutdown'?", "id": "Arus Diam Saat Aktif, Shutdown Saat Mati." },
  { "en": "Bedanya 'Power-On Reset (POR)' Dan 'Brown-Out Detection (BOD)'?", "id": "POR Saat Dinyalakan, BOD Saat Tegangan Turun." },
  { "en": "Bedanya 'Pull-up Resistor' Dan 'Bleeder Resistor'?", "id": "Pull-up Untuk Logika, Bleeder Buang Muatan." },
  { "en": "Bedanya 'Kapasitor Decoupling' Dan 'Bulk Capacitor'?", "id": "Decoupling Frekuensi Tinggi, Bulk Penyimpan Energi Utama." },
  { "en": "Bedanya 'Ferrite Bead' Dan 'Induktor'?", "id": "Ferrite Bead Menekan Noise Frekuensi Tinggi." },
  { "en": "Bedanya 'Common Mode' Dan 'Differential Mode Noise'?", "id": "Common Mode Searah, Differential Mode Berlawanan Arah." },
  { "en": "Bedanya 'Common Mode Choke' Dan 'Differential Mode Choke'?", "id": "Common Mode Choke Menekan Noise Common Mode." },
  { "en": "Bedanya 'Ground Loop' Dan 'Ground Bounce'?", "id": "Loop Karena Multi-Ground, Bounce Karena Induktansi Ground." },
  { "en": "Bedanya 'Star Grounding' Dan 'Plane Grounding'?", "id": "Star Terpusat, Plane Area Luas." },
  { "en": "Bedanya 'Tegangan Tembus (Breakdown)' Dan 'Tegangan Kerja'?", "id": "Tegangan Tembus Batas Maksimal, Kerja Batas Aman." },
  { "en": "Bedanya 'Arus Jenuh (Saturation)' Dan 'Arus Puncak'?", "id": "Jenuh Batas Kemampuan Komponen, Puncak Batas Operasi." },
  { "en": "Bedanya 'Disipasi Daya' Dan 'Konsumsi Daya'?", "id": "Disipasi Jadi Panas, Konsumsi Daya Total Yang Digunakan." },
  { "en": "Bedanya 'Efek Termoelektrik' Dan 'Efek Termionik'?", "id": "Termoelektrik Konversi Panas-Listrik, Termionik Emisi Elektron." },
  { "en": "Bedanya 'Emisi Spontan' Dan 'Terstimulasi'?", "id": "Spontan Acak, Terstimulasi Dipicu Foton Lain." },
  { "en": "Bedanya 'Inversi Populasi' Dan 'Keadaan Dasar'?", "id": "Inversi Populasi Kondisi Untuk Aksi Laser." },
  { "en": "Bedanya 'Laser Gas' Dan 'Laser Solid-State'?", "id": "Gas Gunakan Gas, Solid-State Gunakan Kristal." },
  { "en": "Bedanya 'Modulasi Internal' Dan 'Eksternal'?", "id": "Internal Modulasi Sumber, Eksternal Modulasi Sinar." },
  { "en": "Bedanya 'Fiber Optik Single-Mode' Dan 'Multi-Mode'?", "id": "Single-Mode Inti Kecil, Multi-Mode Inti Besar." },
  { "en": "Bedanya 'Dispersi Kromatik' Dan 'Modal'?", "id": "Kromatik Karena Beda Panjang Gelombang, Modal Beda Jalur." },
  { "en": "Bedanya 'Fusion Splicing' Dan 'Mechanical Splicing'?", "id": "Fusion Meleburkan Serat, Mechanical Hanya Menjajarkan." },
  { "en": "Bedanya 'OTDR (Optical Time-Domain Reflectometer)' Dan 'Power Meter'?", "id": "OTDR Analisis Detail, Power Meter Ukur Daya Total." },
  { "en": "Apa Bedanya 'Logika NMOS (N-type Metal-Oxide-Semiconductor)' Dan 'PMOS (P-type Metal-Oxide-Semiconductor)'?", "id": "NMOS Gunakan Elektron, PMOS Gunakan Lubang (Hole)." },
  { "en": "Bedanya 'Transistor FinFET (Fin Field-Effect Transistor)' Dan 'Planar FET (Field-Effect Transistor)'?", "id": "FinFET Tiga Dimensi, Planar Dua Dimensi." },
  { "en": "Bedanya 'Dry Etching' Dan 'Wet Etching'?", "id": "Dry Etching Gunakan Gas Plasma, Wet Etching Cairan Kimia." },
  { "en": "Bedanya 'CVD (Chemical Vapor Deposition)' Dan 'PVD (Physical Vapor Deposition)'?", "id": "CVD Reaksi Kimia, PVD Proses Fisik." },
  { "en": "Bedanya 'Implantasi Ion' Dan 'Difusi'?", "id": "Implantasi Ion Lebih Terkontrol Daripada Difusi." },
  { "en": "Bedanya 'Stabilitas Transien' Dan 'Stabilitas Steady-State'?", "id": "Transien Respon Gangguan Besar, Steady-State Gangguan Kecil." },
  { "en": "Bedanya 'Sistem Per Unit' Dan 'Nilai Absolut'?", "id": "Sistem Per Unit Menyederhanakan Analisis Sistem Tenaga." },
  { "en": "Bedanya 'Komponen Simetris' Dan 'Komponen Fasa'?", "id": "Komponen Simetris Untuk Analisis Gangguan Asimetris." },
  { "en": "Bedanya 'Waktu Kliring Kritis' Dan 'Sudut Kliring Kritis'?", "id": "Keduanya Batas Maksimal Untuk Menjaga Stabilitas." },
  { "en": "Bedanya 'FFT (Fast Fourier Transform)' Dan 'DCT (Discrete Cosine Transform)'?", "id": "DCT Sangat Baik Untuk Kompresi Gambar Dan Video." },
  { "en": "Bedanya 'Transformasi Wavelet' Dan 'Transformasi Fourier'?", "id": "Wavelet Menganalisis Waktu Dan Frekuensi Secara Bersamaan." },
  { "en": "Bedanya 'Filter Median' Dan 'Filter Rata-Rata (Mean)'?", "id": "Median Lebih Baik Menghilangkan Noise Tipe Salt-and-Pepper." },
  { "en": "Bedanya 'Fenomena Gibbs' Dan 'Efek Dering (Ringing)'?", "id": "Fenomena Gibbs Adalah Penyebab Efek Dering." },
  { "en": "Bedanya 'Keuntungan Keanekaragaman (Diversity Gain)' Dan 'Keuntungan Pengkodean (Coding Gain)'?", "id": "Diversity Melawan Fading, Coding Memperbaiki Kesalahan." },
  { "en": "Bedanya 'Fading' Dan 'Shadowing'?", "id": "Fading Fluktuasi Cepat, Shadowing Fluktuasi Lambat." },
  { "en": "Bedanya 'Path Loss' Dan 'Fading'?", "id": "Path Loss Pelemahan Jarak, Fading Pelemahan Multi-Jalur." },
  { "en": "Bedanya 'Pergeseran Doppler' Dan 'Ofset Frekuensi'?", "id": "Doppler Karena Gerakan, Ofset Karena Ketidakcocokan Osilator." },
  { "en": "Bedanya 'Kontrol Proporsional (P)' Dan 'Integral (I)'?", "id": "P Respon Cepat, I Hilangkan Error Steady-State." },
  { "en": "Bedanya 'Kontrol Integral (I)' Dan 'Derivatif (D)'?", "id": "I Lihat Masa Lalu, D Prediksi Masa Depan." },
  { "en": "Bedanya 'Sudut Brewster' Dan 'Sudut Kritis'?", "id": "Brewster Untuk Polarisasi, Kritis Untuk Total Internal Reflection." },
  { "en": "Bedanya 'Antena Array Broadside' Dan 'End-fire'?", "id": "Broadside Memancar Tegak Lurus, End-fire Searah Sumbu." },
  { "en": "Bedanya 'Penjumlah Carry Look-Ahead' Dan 'Ripple-Carry'?", "id": "Carry Look-Ahead Jauh Lebih Cepat Dari Ripple-Carry." },
  { "en": "Bedanya 'Pengkodean One-Hot' Dan 'Biner'?", "id": "One-Hot Satu Bit Aktif, Biner Kombinasi Bit." },
  { "en": "Bedanya 'Probe Empat Titik' Dan 'Probe Dua Titik'?", "id": "Empat Titik Menghilangkan Resistansi Kontak." },
  { "en": "Bedanya 'Transformasi Sumber' Dan 'Analisis Simpul (Nodal)'?", "id": "Transformasi Sumber Menyederhanakan Sirkuit Sebelum Analisis." },
  { "en": "Bedanya 'Bahan Kristalin' Dan 'Amorf'?", "id": "Kristalin Punya Struktur Atom Teratur, Amorf Tidak." },
  { "en": "Bedanya 'Anisotropi' Dan 'Isotropi'?", "id": "Anisotropi Sifat Berbeda Di Arah Berbeda." },
  { "en": "Bedanya 'Heat Sink' Dan 'Heat Pipe'?", "id": "Heat Pipe Transfer Panas, Heat Sink Melepas Panas." },
  { "en": "Bedanya 'Resistansi Termal' Dan 'Konduktivitas Termal'?", "id": "Resistansi Hambatan Panas, Konduktivitas Kemampuan Hantar Panas." },
  { "en": "Bedanya 'Kurva Derating' Dan 'Safe Operating Area (SOA)'?", "id": "Derating Karena Suhu, SOA Kombinasi Arus Dan Tegangan." },
  { "en": "Bedanya 'Zero-Order Hold' Dan 'First-Order Hold'?", "id": "Zero-Order Menahan Nilai, First-Order Ekstrapolasi Linier." },
  { "en": "Bedanya 'Sinyal Waktu Kontinu' Dan 'Waktu Diskrit'?", "id": "Kontinu Didefinisikan Setiap Saat, Diskrit Hanya Pada Interval." },
  { "en": "Bedanya 'CPLD (Complex Programmable Logic Device)' Dan 'FPGA (Field-Programmable Gate Array)'?", "id": "FPGA Lebih Kompleks Dan Fleksibel Dari CPLD." },
  { "en": "Bedanya 'Hard Core' Dan 'Soft Core' Dalam FPGA?", "id": "Hard Core Fisik, Soft Core Disintesis Dari Kode." },
  { "en": "Bedanya 'Sintesis' Dan 'Implementasi'?", "id": "Sintesis Menerjemahkan Kode, Implementasi Memetakan Ke Perangkat." },
  { "en": "Bedanya 'MIPS (Million Instructions Per Second)' Dan 'FLOPS (Floating-Point Operations Per Second)'?", "id": "MIPS Untuk Integer, FLOPS Untuk Floating-Point." },
  { "en": "Bedanya 'Latency' Dan 'Throughput'?", "id": "Latency Adalah Waktu Tunda, Throughput Adalah Laju Data." },
  { "en": "Bedanya 'Cache Hit' Dan 'Cache Miss'?", "id": "Hit Data Ditemukan Di Cache, Miss Tidak Ditemukan." },
  { "en": "Bedanya 'Write-Through' Dan 'Write-Back' Cache?", "id": "Write-Through Langsung Tulis Ke Memori, Write-Back Menunda." },
  { "en": "Bedanya 'Virtual Memory' Dan 'Physical Memory'?", "id": "Virtual Memperluas Ruang Alamat, Physical Adalah RAM Sebenarnya." },
  { "en": "Bedanya 'Paging' Dan 'Segmentation'?", "id": "Paging Ukuran Tetap, Segmentation Ukuran Bervariasi." },
  { "en": "Bedanya 'Kernel Mode' Dan 'User Mode'?", "id": "Kernel Mode Punya Akses Penuh, User Mode Terbatas." },
  { "en": "Bedanya 'Synchronous' Dan 'Asynchronous I/O'?", "id": "Synchronous Memblokir, Asynchronous Tidak Memblokir." },
  { "en": "Bedanya 'SNR (Signal-to-Noise Ratio)' Dan 'SINR (Signal-to-Interference-plus-Noise Ratio)'?", "id": "SINR Memperhitungkan Interferensi Dari Sumber Lain." },
  { "en": "Bedanya 'Kuantisasi Skalar' Dan 'Vektor'?", "id": "Skalar Mengkuantisasi Satu Sampel, Vektor Sekelompok Sampel." },
  { "en": "Bedanya 'Modem' Dan 'Codec'?", "id": "Modem Modulasi Sinyal Analog, Codec Kompresi Sinyal Digital." },
  { "en": "Bedanya 'Half-Bridge' Dan 'Full-Bridge' Inverter?", "id": "Full-Bridge Dapat Menghasilkan Tegangan Output Dua Kali Lipat." },
  { "en": "Bedanya 'Single-Phase' Dan 'Three-Phase Inverter'?", "id": "Three-Phase Digunakan Untuk Aplikasi Daya Tinggi." },
  { "en": "Bedanya 'Space Vector Modulation (SVM)' Dan 'Sine PWM'?", "id": "SVM Memanfaatkan Tegangan Bus DC Lebih Baik." },
  { "en": "Bedanya 'Thyristor' Dan 'IGBT (Insulated-Gate Bipolar Transistor)'?", "id": "IGBT Dapat Dimatikan Lewat Gerbang, Thyristor Tidak." },
  { "en": "Bedanya 'Snubber Kapasitif' Dan 'RC'?", "id": "Snubber RC Lebih Baik Dalam Meredam Osilasi." },
  { "en": "Bedanya 'Efek Fotoelektrik' Dan 'Efek Compton'?", "id": "Fotoelektrik Elektron Keluar, Compton Foton Terhambur." },
  { "en": "Bedanya 'Produksi Pasangan' Dan 'Efek Fotoelektrik'?", "id": "Produksi Pasangan Hasilkan Elektron Dan Positron." },
  { "en": "Bedanya 'Radiasi Ionisasi' Dan 'Non-Ionisasi'?", "id": "Ionisasi Cukup Energi Untuk Melepas Elektron." },
  { "en": "Bedanya 'Sinar Alfa' Dan 'Sinar Beta'?", "id": "Alfa Inti Helium, Beta Adalah Elektron." },
  { "en": "Bedanya 'Sinar Gamma' Dan 'Sinar-X'?", "id": "Gamma Dari Inti Atom, Sinar-X Dari Elektron." },
  { "en": "Bedanya 'Dosis Terserap' Dan 'Dosis Ekuivalen'?", "id": "Dosis Ekuivalen Memperhitungkan Jenis Radiasi." },
  { "en": "Bedanya 'Fisi Nuklir' Dan 'Fusi Nuklir'?", "id": "Fisi Memecah Inti, Fusi Menggabungkan Inti." },
  { "en": "Bedanya 'Reaktor Air Mendidih (BWR)' Dan 'Air Bertekanan (PWR)'?", "id": "BWR Hasilkan Uap Di Reaktor, PWR Di Luar." },
  { "en": "Bedanya 'Bahan Bakar Nuklir' Dan 'Moderator'?", "id": "Bahan Bakar Sumber Energi, Moderator Perlambat Neutron." },
  { "en": "Bedanya 'Batang Kendali' Dan 'Pendingin'?", "id": "Batang Kendali Serap Neutron, Pendingin Transfer Panas." },
  { "en": "Bedanya 'DC-DC Converter Terisolasi' Dan 'Tidak Terisolasi'?", "id": "Terisolasi Punya Trafo, Memberikan Keamanan." },
  { "en": "Bedanya 'Cuk Converter' Dan 'Buck-Boost Converter'?", "id": "Cuk Punya Transfer Energi Kapasitif, Lebih Efisien." },
  { "en": "Bedanya 'SEPIC Converter' Dan 'ZETA Converter'?", "id": "Keduanya Adalah Tipe Buck-Boost Non-Inverting." },
  { "en": "Bedanya 'Active Power Factor Correction (PFC)' Dan 'Passive PFC'?", "id": "Active PFC Jauh Lebih Efisien Dan Ringan." },
  { "en": "Bedanya 'Multilevel Inverter' Dan 'Two-Level Inverter'?", "id": "Multilevel Hasilkan Gelombang Lebih Baik, Distorsi Rendah." },
  { "en": "Bedanya 'Cascaded H-Bridge' Dan 'Neutral-Point Clamped' Inverter?", "id": "Keduanya Adalah Topologi Multilevel Inverter Populer." },
  { "en": "Bedanya 'Matrix Converter' Dan 'DC-Link Converter'?", "id": "Matrix Konverter AC-AC Langsung Tanpa Penyimpanan Energi." },
  { "en": "Bedanya 'Sistem Tenaga Fleksibel' Dan 'Kaku'?", "id": "Sistem Fleksibel Dapat Beradaptasi Dengan Perubahan Beban." },
  { "en": "Bedanya 'Microgrid' Dan 'Smart Grid'?", "id": "Microgrid Bagian Kecil, Smart Grid Jaringan Cerdas Luas." },
  { "en": "Bedanya 'Demand Response' Dan 'Load Shedding'?", "id": "Demand Response Sukarela, Load Shedding Pemutusan Paksa." },
  { "en": "Bedanya 'Distributed Generation' Dan 'Centralized Generation'?", "id": "Distributed Tersebar, Centralized Terpusat." },
  { "en": "Bedanya 'Penyimpanan Energi Baterai (BESS)' Dan 'Penyimpanan Pompa Hidro'?", "id": "BESS Skala Kecil, Pompa Hidro Skala Sangat Besar." },
  { "en": "Bedanya 'Superkapasitor' Dan 'Baterai'?", "id": "Superkapasitor Kepadatan Daya Tinggi, Baterai Kepadatan Energi Tinggi." },
  { "en": "Bedanya 'Roda Gila (Flywheel)' Dan 'Superkapasitor'?", "id": "Roda Gila Simpan Energi Kinetik, Superkapasitor Elektrostatik." },
  { "en": "Bedanya 'Biomassa' Dan 'Bahan Bakar Fosil'?", "id": "Biomassa Dari Organik, Dianggap Dapat Diperbarui." },
  { "en": "Bedanya 'Energi Geotermal' Dan 'Energi Surya'?", "id": "Geotermal Dari Panas Bumi, Surya Dari Matahari." },
  { "en": "Bedanya 'Turbin Angin Sumbu Horizontal (HAWT)' Dan 'Vertikal (VAWT)'?", "id": "HAWT Lebih Umum Dan Efisien, VAWT Tidak Perlu Arah Angin." },
  { "en": "Bedanya 'Pitch Control' Dan 'Stall Control' Pada Turbin Angin?", "id": "Pitch Mengatur Sudut Bilah, Stall Mengandalkan Aerodinamika." },
  { "en": "Bedanya 'Direct Drive' Dan 'Geared' Turbin Angin?", "id": "Direct Drive Tanpa Gearbox, Perawatan Lebih Mudah." },
  { "en": "Bedanya 'Listrik Statis' Dan 'Listrik Dinamis'?", "id": "Statis Muatan Diam, Dinamis Muatan Bergerak." },
  { "en": "Bedanya 'Gaya Lorentz' Dan 'Gaya Coulomb'?", "id": "Lorentz Pada Muatan Bergerak, Coulomb Pada Muatan Diam." },
  { "en": "Bedanya 'Permitivitas Vakum' Dan 'Permitivitas Relatif'?", "id": "Relatif Adalah Rasio Terhadap Permitivitas Vakum." },
  { "en": "Bedanya 'Persamaan Maxwell' Dan 'Hukum Ohm'?", "id": "Maxwell Dasar Elektromagnetisme, Ohm Hubungan V-I-R." },
  { "en": "Bedanya 'Potensial Vektor Magnetik' Dan 'Potensial Skalar Listrik'?", "id": "Keduanya Alat Matematis Untuk Menyederhanakan Perhitungan Medan." },
  { "en": "Bedanya 'Impedansi Intrinsik' Dan 'Impedansi Karakteristik'?", "id": "Intrinsik Untuk Medium, Karakteristik Untuk Saluran Transmisi." },
  { "en": "Bedanya 'Konstanta Fasa' Dan 'Konstanta Atenuasi'?", "id": "Konstanta Fasa Pergeseran Fasa, Atenuasi Pelemahan Amplitudo." },
  { "en": "Bedanya 'Kecepatan Fasa' Dan 'Kecepatan Grup'?", "id": "Fasa Kecepatan Puncak Gelombang, Grup Kecepatan Paket Gelombang." },
  { "en": "Bedanya 'Saluran Transmisi Seimbang' Dan 'Tidak Seimbang'?", "id": "Seimbang (e.g., Twisted Pair), Tidak Seimbang (e.g., Coaxial)." },
  { "en": "Bedanya 'Balun' Dan 'Unun'?", "id": "Balun Hubungkan Seimbang Ke Tidak Seimbang." },
  { "en": "Bedanya 'Matching Impedansi' Dan 'Filtering'?", "id": "Matching Transfer Daya Maksimal, Filtering Loloskan Frekuensi." },
  { "en": "Bedanya 'Stub' Dan 'Transformator Seperempat Gelombang'?", "id": "Keduanya Adalah Teknik Untuk Matching Impedansi." },
  { "en": "Bedanya 'Noise Figure' Dan 'Signal-to-Noise Ratio (SNR)'?", "id": "Noise Figure Degradasi SNR Yang Disebabkan Sistem." },
  { "en": "Bedanya 'Gain Kompresi' Dan 'Gain Linier'?", "id": "Gain Kompresi Terjadi Saat Penguat Mulai Saturasi." },
  { "en": "Apa Bedanya 'Litografi Foto' Dan 'Litografi Sinar-Elektron'?", "id": "Sinar-Elektron Punya Resolusi Jauh Lebih Tinggi." },
  { "en": "Bedanya 'Photoresist Positif' Dan 'Negatif'?", "id": "Positif Menjadi Larut, Negatif Menjadi Keras." },
  { "en": "Bedanya 'Sel Memori DRAM (Dynamic Random-Access Memory)' Dan 'SRAM (Static Random-Access Memory)'?", "id": "DRAM Gunakan Kapasitor, SRAM Gunakan Flip-Flop." },
  { "en": "Bedanya 'Sense Amplifier' Dan 'Buffer'?", "id": "Sense Amplifier Mendeteksi Sinyal Kecil, Buffer Menguatkan Arus." },
  { "en": "Bedanya 'Komutasi Paksa' Dan 'Komutasi Alami'?", "id": "Alami Terjadi Otomatis, Paksa Butuh Sirkuit Tambahan." },
  { "en": "Bedanya 'UPS (Uninterruptible Power Supply) Offline' Dan 'Online'?", "id": "Online Memberikan Proteksi Lebih Baik Tanpa Jeda." },
  { "en": "Bedanya 'Chopper Satu Kuadran' Dan 'Empat Kuadran'?", "id": "Empat Kuadran Dapat Bekerja Di Semua Mode." },
  { "en": "Bedanya 'Derajat Kebebasan (DOF)' Dan 'Ruang Kerja (Workspace)'?", "id": "DOF Jumlah Sumbu Gerak, Workspace Area Jangkauan." },
  { "en": "Bedanya 'Aktuator' Dan 'Efektor Akhir (End Effector)'?", "id": "Aktuator Menggerakkan Sendi, Efektor Akhir Melakukan Tugas." },
  { "en": "Bedanya 'ECG (Electrocardiogram)' Dan 'EEG (Electroencephalogram)'?", "id": "ECG Aktivitas Listrik Jantung, EEG Aktivitas Otak." },
  { "en": "Bedanya 'MRI (Magnetic Resonance Imaging)' Dan 'CT Scan (Computed Tomography)'?", "id": "MRI Gunakan Magnet, CT Scan Gunakan Sinar-X." },
  { "en": "Bedanya 'Alat Pacu Jantung (Pacemaker)' Dan 'Defibrilator'?", "id": "Pacu Jantung Atur Ritme, Defibrilator Hentikan Ritme Kacau." },
  { "en": "Bedanya 'Clipping' Dan 'Distorsi'?", "id": "Clipping Adalah Bentuk Ekstrem Dari Distorsi." },
  { "en": "Bedanya 'Mikrofon Kondensor' Dan 'Dinamis'?", "id": "Kondensor Lebih Sensitif, Butuh Daya Phantom." },
  { "en": "Bedanya 'Refraksi (Pembiasan)' Dan 'Difraksi (Lenturan)'?", "id": "Refraksi Pembelokan Cahaya, Difraksi Penyebaran Cahaya." },
  { "en": "Bedanya 'Interferensi' Dan 'Difraksi'?", "id": "Interferensi Penjumlahan Gelombang, Difraksi Penyebaran Gelombang." },
  { "en": "Bedanya 'Cahaya Koheren' Dan 'Inkoheren'?", "id": "Koheren Punya Fasa Konstan, Inkoheren Acak." },
  { "en": "Bedanya 'Hukum Tegangan Kirchhoff (KVL)' Dan 'Arus (KCL)'?", "id": "KVL Tentang Tegangan Dalam Loop, KCL Arus Di Simpul." },
  { "en": "Bedanya 'Daktilitas' Dan 'Kerapuhan (Brittleness)'?", "id": "Daktilitas Kemampuan Meregang, Kerapuhan Mudah Patah." },
  { "en": "Bedanya 'Kekuatan (Strength)' Dan 'Ketangguhan (Toughness)'?", "id": "Kekuatan Tahan Beban, Ketangguhan Tahan Patahan." },
  { "en": "Bedanya 'NEXT (Near-End Crosstalk)' Dan 'FEXT (Far-End Crosstalk)'?", "id": "NEXT Di Sisi Pengirim, FEXT Di Sisi Penerima." },
  { "en": "Bedanya 'Ekualisasi' Dan 'Keanekaragaman (Diversity)'?", "id": "Ekualisasi Perbaiki Sinyal, Diversity Pilih Sinyal Terbaik." },
  { "en": "Bedanya 'Spektrum Tersebar (Spread Spectrum)' Dan 'Pita Sempit (Narrowband)'?", "id": "Spektrum Tersebar Lebih Tahan Terhadap Interferensi." },
  { "en": "Bedanya 'Cache Direct Mapped' Dan 'Fully Associative'?", "id": "Direct Mapped Sederhana, Fully Associative Fleksibel." },
  { "en": "Bedanya 'Instruction Pointer' Dan 'Stack Pointer'?", "id": "Instruction Pointer Alamat Instruksi, Stack Pointer Puncak Stack." },
  { "en": "Bedanya 'ALU (Arithmetic Logic Unit)' Dan 'FPU (Floating-Point Unit)'?", "id": "ALU Operasi Integer, FPU Operasi Floating-Point." },
  { "en": "Bedanya 'Sistem Fail-Safe' Dan 'Fault-Tolerant'?", "id": "Fail-Safe Gagal Aman, Fault-Tolerant Tetap Beroperasi." },
  { "en": "Bedanya 'Antarmuka Paralel' Dan 'Seri'?", "id": "Paralel Transfer Banyak Bit, Seri Satu Per Satu." },
  { "en": "Bedanya 'FIFO (First-In, First-Out)' Dan 'LIFO (Last-In, First-Out)'?", "id": "FIFO Seperti Antrean, LIFO Seperti Tumpukan." },
  { "en": "Bedanya 'Data Bus' Dan 'Address Bus'?", "id": "Address Bus Pilih Lokasi, Data Bus Transfer Data." },
  { "en": "Bedanya 'Chipset Northbridge' Dan 'Southbridge'?", "id": "Northbridge Komunikasi Cepat, Southbridge Komunikasi Lambat." },
  { "en": "Bedanya 'BIOS (Basic Input/Output System)' Dan 'UEFI (Unified Extensible Firmware Interface)'?", "id": "UEFI Lebih Modern Dan Cepat Dari BIOS." },
  { "en": "Bedanya 'Bootloader' Dan 'Sistem Operasi'?", "id": "Bootloader Memuat Sistem Operasi Ke Memori." },
  { "en": "Bedanya 'Real-Time Clock (RTC)' Dan 'System Clock'?", "id": "RTC Tetap Berjalan, System Clock Berhenti Saat Mati." },
  { "en": "Bedanya 'CAD (Computer-Aided Design)' Dan 'CAM (Computer-Aided Manufacturing)'?", "id": "CAD Untuk Desain, CAM Untuk Produksi." },
  { "en": "Bedanya 'Bandul Fisis' Dan 'Matematis'?", "id": "Bandul Matematis Massa Terpusat, Fisis Terdistribusi." },
  { "en": "Bedanya 'Getaran Bebas' Dan 'Paksa'?", "id": "Bebas Tanpa Gaya Luar, Paksa Dengan Gaya Luar." },
  { "en": "Bedanya 'Getaran Teredam' Dan 'Tidak Teredam'?", "id": "Teredam Amplitudo Berkurang, Tidak Teredam Amplitudo Tetap." },
  { "en": "Bedanya 'Amplitudo' Dan 'Intensitas'?", "id": "Intensitas Sebanding Dengan Kuadrat Amplitudo." },
  { "en": "Bedanya 'Frekuensi Sudut' Dan 'Frekuensi Linier'?", "id": "Frekuensi Sudut Dalam Radian, Linier Dalam Hertz." },
  { "en": "Bedanya 'Gema (Echo)' Dan 'Dengung (Reverberation)'?", "id": "Gema Pantulan Tunggal, Dengung Pantulan Berulang." },
  { "en": "Bedanya 'Overtone' Dan 'Harmonik'?", "id": "Harmonik Adalah Overtone Kelipatan Bulat." },
  { "en": "Bedanya 'Nada (Pitch)' Dan 'Kenyaringan (Loudness)'?", "id": "Nada Terkait Frekuensi, Kenyaringan Terkait Amplitudo." },
  { "en": "Bedanya 'Timbre' Dan 'Nada'?", "id": "Timbre Kualitas Suara, Nada Frekuensi Dasar." },
  { "en": "Bedanya 'Gelombang Longitudinal' Dan 'Transversal'?", "id": "Longitudinal Searah, Transversal Tegak Lurus." },
  { "en": "Bedanya 'Massa' Dan 'Berat'?", "id": "Massa Ukuran Inersia, Berat Gaya Gravitasi." },
  { "en": "Bedanya 'Kecepatan (Velocity)' Dan 'Laju (Speed)'?", "id": "Kecepatan Adalah Vektor, Laju Adalah Skalar." },
  { "en": "Bedanya 'Percepatan' Dan 'Perlambatan'?", "id": "Perlambatan Adalah Percepatan Dengan Arah Berlawanan." },
  { "en": "Bedanya 'Gaya Sentripetal' Dan 'Sentrifugal'?", "id": "Sentripetal Arah Ke Pusat, Sentrifugal Reaksi Semu." },
  { "en": "Bedanya 'Energi Kinetik' Dan 'Potensial'?", "id": "Kinetik Energi Gerak, Potensial Energi Posisi." },
  { "en": "Bedanya 'Usaha' Dan 'Daya'?", "id": "Usaha Transfer Energi, Daya Laju Melakukan Usaha." },
  { "en": "Bedanya 'Momentum' Dan 'Impuls'?", "id": "Impuls Adalah Perubahan Momentum." },
  { "en": "Bedanya 'Tumbukan Lenting Sempurna' Dan 'Tidak Sempurna'?", "id": "Lenting Sempurna Energi Kinetik Kekal." },
  { "en": "Bedanya 'Fluida Newtonian' Dan 'Non-Newtonian'?", "id": "Newtonian Viskositas Konstan, Non-Newtonian Berubah." },
  { "en": "Bedanya 'Aliran Laminer' Dan 'Turbulen'?", "id": "Laminer Aliran Teratur, Turbulen Aliran Kacau." },
  { "en": "Bedanya 'Tekanan Absolut' Dan 'Tekanan Gauge'?", "id": "Absolut Relatif Vakum, Gauge Relatif Atmosfer." },
  { "en": "Bedanya 'Kalor' Dan 'Suhu'?", "id": "Kalor Transfer Energi, Suhu Ukuran Energi Kinetik." },
  { "en": "Bedanya 'Kalor Jenis' Dan 'Kapasitas Kalor'?", "id": "Kalor Jenis Per Satuan Massa." },
  { "en": "Bedanya 'Entropi' Dan 'Entalpi'?", "id": "Entropi Ukuran Ketidakteraturan, Entalpi Total Kalor." },
  { "en": "Bedanya 'Mesin Carnot' Dan 'Mesin Otto'?", "id": "Carnot Siklus Ideal, Otto Untuk Mesin Bensin." },
  { "en": "Bedanya 'Proses Adiabatik' Dan 'Isotermal'?", "id": "Adiabatik Tanpa Transfer Kalor, Isotermal Suhu Konstan." },
  { "en": "Bedanya 'Proses Isobarik' Dan 'Isokorik'?", "id": "Isobarik Tekanan Konstan, Isokorik Volume Konstan." },
  { "en": "Bedanya 'Muatan Titik' Dan 'Distribusi Muatan'?", "id": "Muatan Titik Ideal, Distribusi Lebih Realistis." },
  { "en": "Bedanya 'Garis Medan Listrik' Dan 'Permukaan Ekuipotensial'?", "id": "Garis Medan Tegak Lurus Permukaan Ekuipotensial." },
  { "en": "Bedanya 'Kapasitor Pelat Sejajar' Dan 'Silinder'?", "id": "Beda Dalam Geometri Dan Rumus Kapasitansi." },
  { "en": "Bedanya 'Bahan Linear' Dan 'Non-Linear'?", "id": "Linear Respon Proporsional, Non-Linear Tidak." },
  { "en": "Bedanya 'Hukum Faraday' Dan 'Hukum Lenz'?", "id": "Hukum Lenz Memberikan Arah GGL Induksi Faraday." },
  { "en": "Bedanya 'Arus Bolak-Balik (AC)' Dan 'Arus Searah (DC)'?", "id": "AC Arah Berubah, DC Arah Tetap." },
  { "en": "Bedanya 'Fasor' Dan 'Vektor'?", "id": "Fasor Vektor Berputar, Representasi Gelombang Sinusoidal." },
  { "en": "Bedanya 'Kualitas (Q) Tinggi' Dan 'Kualitas Rendah'?", "id": "Q Tinggi Selektivitas Frekuensi Baik." },
  { "en": "Bedanya 'Cermin Cekung' Dan 'Cembung'?", "id": "Cekung Mengumpulkan Cahaya, Cembung Menyebarkan Cahaya." },
  { "en": "Bedanya 'Lensa Cekung' Dan 'Cembung'?", "id": "Cekung Menyebarkan Cahaya, Cembung Mengumpulkan Cahaya." },
  { "en": "Bedanya 'Bayangan Nyata' Dan 'Maya'?", "id": "Nyata Dapat Ditangkap Layar, Maya Tidak." },
  { "en": "Bedanya 'Miopi' Dan 'Hipermetropi'?", "id": "Miopi Rabun Jauh, Hipermetropi Rabun Dekat." },
  { "en": "Bedanya 'Astigmatisme' Dan 'Presbiopi'?", "id": "Astigmatisme Kornea Tidak Rata, Presbiopi Karena Usia." },
  { "en": "Bedanya 'Spektrum Emisi' Dan 'Absorpsi'?", "id": "Emisi Garis Terang, Absorpsi Garis Gelap." },
  { "en": "Bedanya 'Isotop' Dan 'Isobar'?", "id": "Isotop Proton Sama, Isobar Nomor Massa Sama." },
  { "en": "Bedanya 'Waktu Paruh' Dan 'Aktivitas Radioaktif'?", "id": "Waktu Paruh Waktu Meluruh, Aktivitas Laju Peluruhan." },
  { "en": "Bedanya 'Akselerator Partikel Linier' Dan 'Siklotron'?", "id": "Linier Lurus, Siklotron Melingkar." },
  { "en": "Bedanya 'Detektor Sintilasi' Dan 'Geiger-Muller'?", "id": "Sintilasi Hasilkan Cahaya, Geiger Hasilkan Pulsa Listrik." },
  { "en": "Bedanya 'Fisika Klasik' Dan 'Modern'?", "id": "Modern Meliputi Relativitas Dan Kuantum." },
  { "en": "Bedanya 'Mekanika Kuantum' Dan 'Relativitas Umum'?", "id": "Kuantum Skala Kecil, Relativitas Skala Sangat Besar." },
  { "en": "Bedanya 'Foton' Dan 'Fonon'?", "id": "Foton Kuantum Cahaya, Fonon Kuantum Getaran." },
  { "en": "Bedanya 'Fermion' Dan 'Boson'?", "id": "Fermion Partikel Materi, Boson Partikel Pembawa Gaya." },
  { "en": "Bedanya 'Quark' Dan 'Lepton'?", "id": "Quark Membentuk Proton, Lepton Termasuk Elektron." },
  { "en": "Bedanya 'Antimateri' Dan 'Materi Gelap'?", "id": "Antimateri Muatan Berlawanan, Materi Gelap Tak Terlihat." },
  { "en": "Bedanya 'Energi Gelap' Dan 'Materi Gelap'?", "id": "Materi Gelap Tarik Gravitasi, Energi Gelap Dorong Ekspansi." },
  { "en": "Bedanya 'Big Bang' Dan 'Big Crunch'?", "id": "Big Bang Awal Mula, Big Crunch Akhir Hipotetis." },
  { "en": "Bedanya 'Lubang Hitam' Dan 'Lubang Cacing'?", "id": "Lubang Hitam Tarikan Gravitasi, Lubang Cacing Jalan Pintas." },
  { "en": "Bedanya 'Bintang Neutron' Dan 'Katai Putih'?", "id": "Bintang Neutron Jauh Lebih Padat Dari Katai Putih." },
  { "en": "Bedanya 'Supernova' Dan 'Nova'?", "id": "Supernova Ledakan Bintang Masif, Jauh Lebih Kuat." },
  { "en": "Bedanya 'Galaksi Spiral' Dan 'Elips'?", "id": "Spiral Punya Lengan, Elips Berbentuk Bulat Lonjong." },
  { "en": "Bedanya 'Asteroid' Dan 'Komet'?", "id": "Asteroid Batuan, Komet Es Dan Debu." },
  { "en": "Bedanya 'Meteor' Dan 'Meteorit'?", "id": "Meteor Di Atmosfer, Meteorit Sampai Ke Bumi." },
  { "en": "Bedanya 'Gerhana Matahari' Dan 'Bulan'?", "id": "Gerhana Matahari Bulan Tutupi Matahari, Sebaliknya." },
  { "en": "Apa Bedanya 'Akurasi' Dan 'Repeatability'?", "id": "Akurasi Dekat Nilai Benar, Repeatability Konsistensi Pengukuran." },
  { "en": "Bedanya 'Histeresis' Dan 'Linearitas'?", "id": "Histeresis Perbedaan Output Naik-Turun, Linearitas Hubungan Lurus." },
  { "en": "Bedanya 'Meter Analog' Dan 'Meter Digital'?", "id": "Analog Gunakan Jarum, Digital Tampilkan Angka." },
  { "en": "Bedanya 'Pengkondisian Sinyal' Dan 'Akuisisi Data'?", "id": "Pengkondisian Siapkan Sinyal, Akuisisi Kumpulkan Sinyal." },
  { "en": "Bedanya 'Economic Load Dispatch' Dan 'Unit Commitment'?", "id": "Unit Commitment Pilih Generator, Economic Dispatch Bagi Beban." },
  { "en": "Bedanya 'Faktor Beban' Dan 'Faktor Permintaan'?", "id": "Faktor Beban Rata-rata/Puncak, Faktor Permintaan Puncak/Terpasang." },
  { "en": "Bedanya 'PSS (Power System Stabilizer)' Dan 'AVR (Automatic Voltage Regulator)'?", "id": "AVR Atur Tegangan, PSS Redam Osilasi Daya." },
  { "en": "Bedanya 'Tap Changer' Dan 'Regulator Tegangan'?", "id": "Tap Changer Ubah Rasio Trafo, Regulator Atur Tegangan." },
  { "en": "Bedanya 'Kekuatan Tarik' Dan 'Kekuatan Tekan'?", "id": "Tarik Kemampuan Ditarik, Tekan Kemampuan Ditekan." },
  { "en": "Bedanya 'Deformasi Elastis' Dan 'Plastis'?", "id": "Elastis Kembali Ke Bentuk Semula, Plastis Permanen." },
  { "en": "Bedanya 'Creep (Rayapan)' Dan 'Fatigue (Kelelahan)'?", "id": "Creep Karena Beban Statis, Fatigue Beban Dinamis." },
  { "en": "Bedanya 'State Observer' Dan 'State Estimator'?", "id": "Keduanya Memperkirakan Keadaan Internal Sistem." },
  { "en": "Bedanya 'Filter Kalman' Dan 'Kontroler PID'?", "id": "Kalman Estimasi Keadaan, PID Aksi Kontrol." },
  { "en": "Bedanya 'Kontrolabilitas' Dan 'Stabilisabilitas'?", "id": "Kontrolabilitas Lebih Kuat Daripada Stabilisabilitas." },
  { "en": "Bedanya 'Adaptive Filter' Dan 'Fixed Filter'?", "id": "Adaptive Dapat Berubah, Fixed Tetap." },
  { "en": "Bedanya 'White Noise' Dan 'Pink Noise'?", "id": "White Noise Daya Sama, Pink Noise Turun Per Oktaf." },
  { "en": "Bedanya 'FHSS (Frequency Hopping Spread Spectrum)' Dan 'DSSS (Direct Sequence Spread Spectrum)'?", "id": "FHSS Melompat Frekuensi, DSSS Sebar Dengan Kode." },
  { "en": "Bedanya 'OFDM (Orthogonal Frequency-Division Multiplexing)' Dan 'OFDMA (Orthogonal Frequency-Division Multiple Access)'?", "id": "OFDMA Alokasi Sub-carrier Ke Banyak Pengguna." },
  { "en": "Bedanya 'Go-Back-N' Dan 'Selective Repeat' ARQ?", "id": "Selective Repeat Lebih Efisien, Hanya Kirim Ulang Gagal." },
  { "en": "Bedanya 'Microcode' Dan 'Kode Mesin'?", "id": "Microcode Level Lebih Rendah Dari Kode Mesin." },
  { "en": "Bedanya 'Arsitektur Superscalar' Dan 'VLIW (Very Long Instruction Word)'?", "id": "Superscalar Dinamis, VLIW Statis Oleh Compiler." },
  { "en": "Bedanya 'Data Hazard' Dan 'Control Hazard'?", "id": "Data Hazard Ketergantungan Data, Control Hazard Percabangan." },
  { "en": "Bedanya 'Perencanaan Jalur (Path Planning)' Dan 'Perencanaan Trajektori'?", "id": "Trajektori Memasukkan Unsur Waktu, Path Tidak." },
  { "en": "Bedanya 'Robot Holonomik' Dan 'Non-holonomik'?", "id": "Holonomik Dapat Bergerak Ke Segala Arah." },
  { "en": "Bedanya 'Sumber Tegangan' Dan 'Sumber Arus'?", "id": "Sumber Tegangan Jaga Tegangan, Sumber Arus Jaga Arus." },
  { "en": "Bedanya 'Sumber Ideal' Dan 'Sumber Praktis'?", "id": "Sumber Praktis Punya Resistansi Internal." },
  { "en": "Bedanya 'Garis Beban' Dan 'Titik Operasi'?", "id": "Titik Operasi Perpotongan Garis Beban Dan Kurva." },
  { "en": "Bedanya 'Arus Bocor' Dan 'Arus Gelap (Dark Current)'?", "id": "Arus Gelap Arus Bocor Pada Fotodetektor." },
  { "en": "Bedanya 'Numerical Aperture' Dan 'Sudut Penerimaan'?", "id": "Numerical Aperture Berkaitan Langsung Dengan Sudut Penerimaan." },
  { "en": "Bedanya 'Serat Step-Index' Dan 'Graded-Index'?", "id": "Graded-Index Mengurangi Dispersi Modal." },
  { "en": "Bedanya 'LAN (Local Area Network)' Dan 'WAN (Wide Area Network)'?", "id": "LAN Area Lokal, WAN Area Sangat Luas." },
  { "en": "Bedanya 'MAN (Metropolitan Area Network)' Dan 'PAN (Personal Area Network)'?", "id": "MAN Skala Kota, PAN Skala Pribadi." },
  { "en": "Bedanya 'VPN (Virtual Private Network)' Dan 'Jaringan Pribadi'?", "id": "VPN Menciptakan Jaringan Pribadi Di Atas Jaringan Publik." },
  { "en": "Bedanya 'NAT (Network Address Translation)' Dan 'Proxy Server'?", "id": "NAT Ubah Alamat IP, Proxy Bertindak Sebagai Perantara." },
  { "en": "Bedanya 'Protokol ICMP (Internet Control Message Protocol)' Dan 'IP (Internet Protocol)'?", "id": "ICMP Digunakan Untuk Pesan Error Dan Kontrol IP." },
  { "en": "Bedanya 'Protokol ARP (Address Resolution Protocol)' Dan 'RARP (Reverse ARP)'?", "id": "ARP Ubah IP Ke MAC, RARP Sebaliknya." },
  { "en": "Bedanya 'Static Routing' Dan 'Dynamic Routing'?", "id": "Static Dikonfigurasi Manual, Dynamic Belajar Otomatis." },
  { "en": "Bedanya 'Distance Vector' Dan 'Link State' Routing?", "id": "Distance Vector Berbasis Jarak, Link State Berbasis Peta." },
  { "en": "Bedanya 'Circuit Switching' Dan 'Packet Switching'?", "id": "Circuit Jalur Khusus, Packet Data Dibagi-bagi." },
  { "en": "Bedanya 'Connection-Oriented' Dan 'Connectionless' Service?", "id": "Connection-Oriented Butuh Pengaturan Koneksi (e.g., TCP)." },
  { "en": "Bedanya 'Flow Control' Dan 'Congestion Control'?", "id": "Flow Control Antar Pengirim-Penerima, Congestion Dalam Jaringan." },
  { "en": "Bedanya 'Attenuation' Dan 'Dispersion'?", "id": "Attenuation Pelemahan Sinyal, Dispersion Pelebaran Sinyal." },
  { "en": "Bedanya 'Twisted Pair' Dan 'Kabel Koaksial'?", "id": "Koaksial Punya Pelindung Lebih Baik Dari Interferensi." },
  { "en": "Bedanya 'Baseband' Dan 'Passband' Transmission?", "id": "Baseband Tanpa Modulasi, Passband Dengan Modulasi." },
  { "en": "Bedanya 'Nyquist Criterion' Dan 'Shannon Capacity'?", "id": "Nyquist Laju Maksimal Ideal, Shannon Laju Praktis." },
  { "en": "Bedanya 'Phase-Locked Loop (PLL)' Dan 'Delay-Locked Loop (DLL)'?", "id": "PLL Sinkronisasi Fasa, DLL Sinkronisasi Tundaan." },
  { "en": "Bedanya 'Jangka Sorong' Dan 'Mikrometer Sekrup'?", "id": "Mikrometer Sekrup Punya Tingkat Ketelitian Lebih Tinggi." },
  { "en": "Bedanya 'Torsi' Dan 'Gaya'?", "id": "Torsi Adalah Gaya Rotasional." },
  { "en": "Bedanya 'Inersia' Dan 'Momen Inersia'?", "id": "Inersia Linier, Momen Inersia Rotasional." },
  { "en": "Bedanya 'Kerja Virtual' Dan 'Kerja Nyata'?", "id": "Kerja Virtual Konsep Teoritis Untuk Analisis." },
  { "en": "Bedanya 'Tegangan (Stress)' Dan 'Regangan (Strain)'?", "id": "Tegangan Gaya Per Area, Regangan Perubahan Bentuk Relatif." },
  { "en": "Bedanya 'Modulus Young' Dan 'Modulus Geser'?", "id": "Modulus Young Elastisitas Tarik, Modulus Geser Elastisitas Torsi." },
  { "en": "Bedanya 'Viskositas' Dan 'Kerapatan'?", "id": "Viskositas Ketahanan Alir, Kerapatan Massa Per Volume." },
  { "en": "Bedanya 'Angka Reynolds' Dan 'Angka Mach'?", "id": "Reynolds Tipe Aliran, Mach Kecepatan Suara." },
  { "en": "Bedanya 'Konduktor' Dan 'Semikonduktor'?", "id": "Semikonduktor Konduktivitasnya Dapat Diatur." },
  { "en": "Bedanya 'Isolator' Dan 'Dielektrik'?", "id": "Dielektrik Adalah Isolator Yang Dapat Terpolarisasi." },
  { "en": "Bedanya 'Forward Converter' Dan 'Push-Pull Converter'?", "id": "Push-Pull Gunakan Dua Transistor, Lebih Efisien." },
  { "en": "Bedanya 'Resonant Converter' Dan 'PWM Converter'?", "id": "Resonant Gunakan Resonansi LC, Rugi Lebih Rendah." },
  { "en": "Bedanya 'Class-D Amplifier' Dan 'Class-A Amplifier'?", "id": "Class-D Sangat Efisien, Bekerja Seperti Saklar." },
  { "en": "Bedanya 'Differential Signaling' Dan 'Single-Ended Signaling'?", "id": "Differential Lebih Tahan Terhadap Noise." },
  { "en": "Bedanya 'Slew Rate' Dan 'Rise Time'?", "id": "Slew Rate Laju Perubahan Maksimal, Rise Time Kecepatan Naik." },
  { "en": "Bedanya 'Input Bias Current' Dan 'Input Offset Current'?", "id": "Offset Adalah Selisih Antara Dua Arus Bias." },
  { "en": "Bedanya 'Input Offset Voltage' Dan 'Output Offset Voltage'?", "id": "Input Menyebabkan Output, Saling Berkaitan." },
  { "en": "Bedanya 'Gain-Bandwidth Product (GBP)' Dan 'Bandwidth'?", "id": "GBP Adalah Hasil Kali Gain Dan Bandwidth." },
  { "en": "Bedanya 'Active Load' Dan 'Passive Load'?", "id": "Active Load Gunakan Transistor, Passive Gunakan Resistor." },
  { "en": "Bedanya 'Current Mirror' Dan 'Current Source'?", "id": "Current Mirror Mereplikasi Arus, Current Source Menyediakan Arus." },
  { "en": "Bedanya 'Wilson Current Mirror' Dan 'Widlar Current Source'?", "id": "Wilson Lebih Akurat, Widlar Untuk Arus Kecil." },
  { "en": "Bedanya 'Cascode Amplifier' Dan 'Cascade Amplifier'?", "id": "Cascode Menumpuk Vertikal, Cascade Menghubungkan Seri." },
  { "en": "Bedanya 'Darlington Pair' Dan 'Sziklai Pair'?", "id": "Keduanya Pasangan Transistor, Sziklai Tipe Komplementer." },
  { "en": "Bedanya 'Latch-up' Dan 'ESD (Electrostatic Discharge)'?", "id": "ESD Dapat Memicu Latch-up Pada Sirkuit CMOS." },
  { "en": "Bedanya 'Body Effect' Dan 'Channel Length Modulation'?", "id": "Keduanya Adalah Efek Orde Kedua Pada MOSFET." },
  { "en": "Bedanya 'Velocity Saturation' Dan 'Mobility Degradation'?", "id": "Velocity Saturation Membatasi Kecepatan Pembawa Muatan." },
  { "en": "Bedanya 'Hot Carrier Injection' Dan 'Gate Oxide Breakdown'?", "id": "Keduanya Adalah Mekanisme Kegagalan Pada MOSFET." },
  { "en": "Bedanya 'Antenna Effect' Dan 'Electro-migration'?", "id": "Antenna Effect Saat Fabrikasi, Electro-migration Saat Operasi." },
  { "en": "Bedanya 'Design Rules Check (DRC)' Dan 'Layout Versus Schematic (LVS)'?", "id": "DRC Cek Aturan Geometri, LVS Cek Kesesuaian." },
  { "en": "Bedanya 'Floorplanning' Dan 'Placement'?", "id": "Floorplanning Penempatan Blok Besar, Placement Sel Standar." },
  { "en": "Bedanya 'Clock Tree Synthesis (CTS)' Dan 'Routing'?", "id": "CTS Distribusi Clock, Routing Koneksi Sinyal." },
  { "en": "Bedanya 'Timing Closure' Dan 'Power Closure'?", "id": "Timing Penuhi Batas Waktu, Power Penuhi Batas Daya." },
  { "en": "Bedanya 'Test Pattern Generation' Dan 'Fault Simulation'?", "id": "Generation Buat Pola, Simulation Cek Cakupan." },
  { "en": "Bedanya 'Boundary Scan' Dan 'Built-In Self-Test (BIST)'?", "id": "Boundary Scan Uji Koneksi, BIST Uji Internal." },
  { "en": "Bedanya 'Formal Verification' Dan 'Simulation'?", "id": "Formal Buktikan Secara Matematis, Simulation Uji Dengan Sampel." },
  { "en": "Bedanya 'Hardware Emulation' Dan 'FPGA Prototyping'?", "id": "Emulation Lebih Cepat, Prototyping Lebih Mirip Nyata." },
  { "en": "Bedanya 'System-on-Chip (SoC)' Dan 'System-in-Package (SiP)'?", "id": "SoC Satu Chip, SiP Banyak Chip Dalam Satu Paket." },
  { "en": "Bedanya 'Application Processor' Dan 'Real-time Processor'?", "id": "Real-time Punya Batas Waktu Deterministik." },
  { "en": "Bedanya 'Digital Signal Processor (DSP)' Dan 'General Purpose Processor (GPP)'?", "id": "DSP Dioptimalkan Untuk Operasi Matematika Sinyal." },
  { "en": "Bedanya 'Von Mises Stress' Dan 'Principal Stress'?", "id": "Von Mises Kriteria Kegagalan, Principal Stres Maksimum." },
  { "en": "Bedanya 'Finite Element Method (FEM)' Dan 'Finite Difference Method (FDM)'?", "id": "FEM Untuk Geometri Kompleks, FDM Untuk Grid Teratur." },
  { "en": "Bedanya 'Boundary Element Method (BEM)' Dan 'Finite Element Method (FEM)'?", "id": "BEM Hanya Diskritisasi Batas, FEM Seluruh Domain." },
  { "en": "Bedanya 'Eigenvalue' Dan 'Eigenvector'?", "id": "Eigenvalue Skalar, Eigenvector Vektor Arah." },
  { "en": "Bedanya 'Orthogonal' Dan 'Orthonormal'?", "id": "Orthonormal Adalah Orthogonal Dengan Panjang Satu." },
  { "en": "Bedanya 'Linear Independence' Dan 'Orthogonality'?", "id": "Orthogonality Mengimplikasikan Linear Independence." },
  { "en": "Bedanya 'Convolution' Dan 'Deconvolution'?", "id": "Deconvolution Adalah Proses Membalikkan Convolution." },
  { "en": "Bedanya 'Matrix Transpose' Dan 'Inverse'?", "id": "Transpose Tukar Baris-Kolom, Inverse Kebalikan Perkalian." },
  { "en": "Bedanya 'Determinant' Dan 'Trace'?", "id": "Determinant Terkait Volume, Trace Jumlah Diagonal." },
  { "en": "Bedanya 'Scalar Product' Dan 'Vector Product'?", "id": "Scalar Hasilkan Skalar, Vector Hasilkan Vektor." },
  { "en": "Apa Bedanya 'Kecepatan Drift' Dan 'Kecepatan Termal'?", "id": "Drift Karena Medan Listrik, Termal Karena Suhu." },
  { "en": "Bedanya 'Avalanche Breakdown' Dan 'Punch-through'?", "id": "Avalanche Perkalian Ion, Punch-through Pertemuan Deplesi." },
  { "en": "Bedanya 'Potensial Bawaan (Built-in)' Dan 'Bias Eksternal'?", "id": "Bawaan Internal, Eksternal Diterapkan Dari Luar." },
  { "en": "Bedanya 'Diagram Tongkat (Stick)' Dan 'Diagram Layout'?", "id": "Stick Representasi Topologi, Layout Representasi Geometri." },
  { "en": "Bedanya 'Desain Sel Standar' Dan 'Full Custom'?", "id": "Sel Standar Gunakan Blok Siap Pakai, Custom Desain Penuh." },
  { "en": "Bedanya 'Model Tundaan Elmore' Dan 'Logical Effort'?", "id": "Elmore Berbasis RC, Logical Effort Berbasis Gate." },
  { "en": "Bedanya 'Polarisasi Sirkular' Dan 'Elips'?", "id": "Sirkular Kasus Khusus Dari Polarisasi Elips." },
  { "en": "Bedanya 'Rasio Aksial' Dan 'SWR (Standing Wave Ratio)'?", "id": "Rasio Aksial Ukuran Kemurnian Polarisasi Sirkular." },
  { "en": "Bedanya 'Persamaan Transmisi Friis' Dan 'Persamaan Radar'?", "id": "Persamaan Radar Memperhitungkan Pemantulan Dari Target." },
  { "en": "Bedanya 'TCM (Trellis Coded Modulation)' Dan 'Block Coded Modulation'?", "id": "TCM Menggabungkan Pengkodean Dan Modulasi Secara Efisien." },
  { "en": "Bedanya 'Deteksi Maximum Likelihood (ML)' Dan 'Maximum A Posteriori (MAP)'?", "id": "MAP Menggunakan Informasi Probabilitas Awal, ML Tidak." },
  { "en": "Bedanya 'Microstrip' Dan 'Coplanar Waveguide (CPW)'?", "id": "CPW Punya Ground Di Sisi Yang Sama." },
  { "en": "Bedanya 'Isolator' Dan 'Sirkulator'?", "id": "Isolator Dua Port, Sirkulator Tiga Port." },
  { "en": "Bedanya 'Power Amplifier (PA)' Dan 'Low Noise Amplifier (LNA)'?", "id": "PA Di Pemancar, LNA Di Penerima." },
  { "en": "Bedanya 'Analisis Bidang Fasa' Dan 'Fungsi Deskripsi'?", "id": "Bidang Fasa Untuk Orde Dua, Fungsi Deskripsi Non-linear." },
  { "en": "Bedanya 'Stabilitas Lyapunov' Dan 'Stabilitas BIBO (Bounded-Input, Bounded-Output)'?", "id": "Lyapunov Tentang Stabilitas Internal, BIBO Eksternal." },
  { "en": "Bedanya 'Belitan Peredam (Damper)' Dan 'Belitan Medan (Field)'?", "id": "Peredam Redam Osilasi, Medan Hasilkan Fluks Utama." },
  { "en": "Bedanya 'Belitan Fractional Pitch' Dan 'Full Pitch'?", "id": "Fractional Pitch Mengurangi Harmonisa, GGL Sedikit Turun." },
  { "en": "Bedanya 'Sudut Torsi' Dan 'Sudut Beban'?", "id": "Keduanya Istilah Yang Sama Untuk Mesin Sinkron." },
  { "en": "Bedanya 'Gradien' Dan 'Divergensi'?", "id": "Gradien Operasi Vektor Pada Skalar, Divergensi Sebaliknya." },
  { "en": "Bedanya 'Curl' Dan 'Gradien'?", "id": "Curl Mengukur Rotasi, Gradien Mengukur Laju Perubahan." },
  { "en": "Bedanya 'Medan Skalar' Dan 'Medan Vektor'?", "id": "Skalar Punya Nilai, Vektor Punya Nilai Dan Arah." },
  { "en": "Bedanya 'Model Air Terjun (Waterfall)' Dan 'Model Agile'?", "id": "Waterfall Sekuensial, Agile Iteratif Dan Inkremental." },
  { "en": "Bedanya 'Git' Dan 'SVN (Subversion)'?", "id": "Git Terdistribusi, SVN Tersentralisasi." },
  { "en": "Bedanya 'Termistor NTC (Negative Temperature Coefficient)' Dan 'PTC (Positive Temperature Coefficient)'?", "id": "NTC Resistansi Turun, PTC Resistansi Naik Saat Panas." },
  { "en": "Bedanya 'Anemometer' Dan 'Manometer'?", "id": "Anemometer Ukur Kecepatan Angin, Manometer Ukur Tekanan." },
  { "en": "Bedanya 'LED (Light Emitting Diode)' Dan 'OLED (Organic LED)'?", "id": "OLED Gunakan Bahan Organik, Lebih Fleksibel." },
  { "en": "Bedanya 'Modulasi AM (Amplitude Modulation)' Dan 'SSB (Single-Sideband)'?", "id": "SSB Lebih Efisien, Hanya Kirim Satu Sisi Pita." },
  { "en": "Bedanya 'Logika Relay' Dan 'PLC (Programmable Logic Controller)'?", "id": "PLC Berbasis Software, Logika Relay Berbasis Hardware." },
  { "en": "Bedanya 'Mesin CNC (Computer Numerical Control)' Dan 'Mesin Konvensional'?", "id": "CNC Dikontrol Komputer, Jauh Lebih Presisi." },
  { "en": "Bedanya 'API (Application Programming Interface)' Dan 'SDK (Software Development Kit)'?", "id": "SDK Kumpulan Alat, API Cara Komunikasi." },
  { "en": "Bedanya 'Compiler' Dan 'Assembler'?", "id": "Compiler Bahasa Tingkat Tinggi, Assembler Bahasa Rakitan." },
  { "en": "Bedanya 'Linker' Dan 'Loader'?", "id": "Linker Gabungkan File, Loader Muat Ke Memori." },
  { "en": "Bedanya 'Multitasking' Dan 'Multithreading'?", "id": "Multithreading Bentuk Khusus Dari Multitasking." },
  { "en": "Bedanya 'Blocking Call' Dan 'Non-Blocking Call'?", "id": "Blocking Menunggu Selesai, Non-Blocking Tidak." },
  { "en": "Bedanya 'Shallow Copy' Dan 'Deep Copy'?", "id": "Deep Copy Menyalin Semua Data, Shallow Hanya Referensi." },
  { "en": "Bedanya 'Pass by Value' Dan 'Pass by Reference'?", "id": "By Value Salin Nilai, By Reference Salin Alamat." },
  { "en": "Bedanya 'Statically Typed' Dan 'Dynamically Typed' Language?", "id": "Statically Cek Tipe Saat Kompilasi, Dynamically Saat Runtime." },
  { "en": "Bedanya 'Garbage Collection' Dan 'Manual Memory Management'?", "id": "Garbage Collection Otomatis, Manual Dilakukan Programmer." },
  { "en": "Bedanya 'Object-Oriented Programming (OOP)' Dan 'Procedural Programming'?", "id": "OOP Berbasis Objek, Prosedural Berbasis Fungsi." },
  { "en": "Bedanya 'Inheritance' Dan 'Composition'?", "id": "Inheritance Hubungan 'Is-A', Composition Hubungan 'Has-A'." },
  { "en": "Bedanya 'Polymorphism' Dan 'Encapsulation'?", "id": "Polymorphism Banyak Bentuk, Encapsulation Penyembunyian Data." },
  { "en": "Bedanya 'Abstract Class' Dan 'Interface'?", "id": "Abstract Class Boleh Punya Implementasi, Interface Tidak." },
  { "en": "Bedanya 'Overloading' Dan 'Overriding'?", "id": "Overloading Nama Sama Beda Parameter, Overriding Di Kelas Turunan." },
  { "en": "Bedanya 'Unit Test' Dan 'Integration Test'?", "id": "Unit Uji Satu Komponen, Integration Uji Gabungan." },
  { "en": "Bedanya 'Black-Box Testing' Dan 'White-Box Testing'?", "id": "Black-Box Tanpa Tahu Internal, White-Box Tahu Internal." },
  { "en": "Bedanya 'Alpha Testing' Dan 'Beta Testing'?", "id": "Alpha Di Tim Internal, Beta Di Pengguna Eksternal." },
  { "en": "Bedanya 'Regression Testing' Dan 'Sanity Testing'?", "id": "Regression Cek Fitur Lama, Sanity Cek Fitur Baru." },
  { "en": "Bedanya 'SQL (Structured Query Language)' Dan 'NoSQL'?", "id": "SQL Berbasis Tabel, NoSQL Beragam Model Data." },
  { "en": "Bedanya 'Primary Key' Dan 'Foreign Key'?", "id": "Primary Key Identifikasi Unik, Foreign Key Referensi." },
  { "en": "Bedanya 'Index' Dan 'View' Dalam Database?", "id": "Index Percepat Pencarian, View Representasi Virtual Tabel." },
  { "en": "Bedanya 'ACID (Atomicity, Consistency, Isolation, Durability)' Dan 'BASE (Basically Available, Soft state, Eventually consistent)'?", "id": "ACID Untuk SQL, BASE Untuk NoSQL." },
  { "en": "Bedanya 'Sharding' Dan 'Replication'?", "id": "Sharding Memecah Data, Replication Menyalin Data." },
  { "en": "Bedanya 'Frontend' Dan 'Backend'?", "id": "Frontend Tampilan Pengguna, Backend Logika Server." },
  { "en": "Bedanya 'HTTP (Hypertext Transfer Protocol)' Dan 'WebSocket'?", "id": "WebSocket Komunikasi Dua Arah Penuh." },
  { "en": "Bedanya 'REST (Representational State Transfer)' Dan 'SOAP (Simple Object Access Protocol)'?", "id": "REST Lebih Sederhana Dan Ringan Dari SOAP." },
  { "en": "Bedanya 'JSON (JavaScript Object Notation)' Dan 'XML (eXtensible Markup Language)'?", "id": "JSON Lebih Ringan Dan Mudah Dibaca Manusia." },
  { "en": "Bedanya 'Authentication' Dan 'Authorization'?", "id": "Authentication Siapa Anda, Authorization Apa Yang Boleh Anda Lakukan." },
  { "en": "Bedanya 'Cookie' Dan 'Session'?", "id": "Cookie Disimpan Di Klien, Session Di Server." },
  { "en": "Bedanya 'RAID 0' Dan 'RAID 1'?", "id": "RAID 0 Striping (Kecepatan), RAID 1 Mirroring (Keamanan)." },
  { "en": "Bedanya 'RAID 5' Dan 'RAID 6'?", "id": "RAID 6 Punya Toleransi Kegagalan Dua Disk." },
  { "en": "Bedanya 'Monolithic Architecture' Dan 'Microservices Architecture'?", "id": "Microservices Memecah Aplikasi Menjadi Layanan Kecil." },
  { "en": "Bedanya 'Server Fisik' Dan 'Virtual Machine (VM)'?", "id": "VM Emulasi Perangkat Keras Di Atas Server Fisik." },
  { "en": "Bedanya 'Virtual Machine (VM)' Dan 'Container'?", "id": "Container Lebih Ringan, Berbagi Kernel Host." },
  { "en": "Bedanya 'IaaS (Infrastructure as a Service)' Dan 'PaaS (Platform as a Service)'?", "id": "IaaS Beri Infrastruktur, PaaS Beri Platform." },
  { "en": "Bedanya 'PaaS (Platform as a Service)' Dan 'SaaS (Software as a Service)'?", "id": "PaaS Beri Platform, SaaS Beri Aplikasi Jadi." },
  { "en": "Bedanya 'Load Balancer' Dan 'Reverse Proxy'?", "id": "Load Balancer Adalah Jenis Khusus Reverse Proxy." },
  { "en": "Bedanya 'CDN (Content Delivery Network)' Dan 'Web Hosting'?", "id": "CDN Mendistribusikan Konten, Hosting Menyimpan Konten." },
  { "en": "Bedanya 'Domain Name' Dan 'IP Address'?", "id": "Domain Name Adalah Alias Untuk IP Address." },
  { "en": "Bedanya 'TCP (Transmission Control Protocol) Handshake' Dan 'TLS (Transport Layer Security) Handshake'?", "id": "TLS Handshake Membuat Koneksi Aman Di Atas TCP." },
  { "en": "Bedanya 'Symmetric Key' Dan 'Asymmetric Key Encryption'?", "id": "Symmetric Satu Kunci, Asymmetric Dua Kunci." },
  { "en": "Bedanya 'Digital Signature' Dan 'Digital Certificate'?", "id": "Sertifikat Mengikat Kunci Publik Ke Identitas." },
  { "en": "Bedanya 'Virus' Dan 'Worm'?", "id": "Worm Dapat Menyebar Sendiri, Virus Butuh Inang." },
  { "en": "Bedanya 'Trojan Horse' Dan 'Spyware'?", "id": "Trojan Menyamar, Spyware Memata-matai." },
  { "en": "Bedanya 'Phishing' Dan 'Spoofing'?", "id": "Phishing Upaya Penipuan, Spoofing Teknik Pemalsuan." },
  { "en": "Bedanya 'Denial of Service (DoS)' Dan 'Distributed Denial of Service (DDoS)'?", "id": "DDoS Serangan Dari Banyak Sumber." },
  { "en": "Bedanya 'Firewall Stateful' Dan 'Stateless'?", "id": "Stateful Mengingat Status Koneksi, Stateless Tidak." },
  { "en": "Bedanya 'Intrusion Detection System (IDS)' Dan 'Intrusion Prevention System (IPS)'?", "id": "IDS Mendeteksi, IPS Mencegah." },
  { "en": "Bedanya 'Honeypot' Dan 'Firewall'?", "id": "Honeypot Jebakan, Firewall Tembok Pertahanan." },
  { "en": "Bedanya 'Klasifikasi' Dan 'Regresi' Dalam Machine Learning?", "id": "Klasifikasi Output Diskrit, Regresi Output Kontinu." },
  { "en": "Bedanya 'Supervised Learning' Dan 'Unsupervised Learning'?", "id": "Supervised Pakai Data Berlabel, Unsupervised Tidak." },
  { "en": "Bedanya 'Reinforcement Learning' Dan 'Supervised Learning'?", "id": "Reinforcement Belajar Dari Reward Dan Punishment." },
  { "en": "Bedanya 'Deep Learning' Dan 'Machine Learning'?", "id": "Deep Learning Adalah Sub-bidang Machine Learning." },
  { "en": "Bedanya 'Training Set' Dan 'Test Set'?", "id": "Training Untuk Melatih Model, Test Untuk Menguji." },
  { "en": "Bedanya 'Overfitting' Dan 'Underfitting'?", "id": "Overfitting Terlalu Kompleks, Underfitting Terlalu Sederhana." },
  { "en": "Bedanya 'Akurasi' Dan 'F1-Score'?", "id": "F1-Score Gabungan Presisi Dan Recall." },
  { "en": "Bedanya 'Precision' Dan 'Recall'?", "id": "Precision Akurasi Prediksi Positif, Recall Kelengkapan." },
  { "en": "Bedanya 'K-Means' Dan 'Hierarchical Clustering'?", "id": "K-Means Butuh Jumlah Cluster, Hierarchical Tidak." },
  { "en": "Bedanya 'Decision Tree' Dan 'Random Forest'?", "id": "Random Forest Kumpulan Dari Banyak Decision Tree." },
  { "en": "Bedanya 'Support Vector Machine (SVM)' Dan 'Logistic Regression'?", "id": "SVM Cari Batas Maksimal, Logistic Regression Probabilitas." },
  { "en": "Bedanya 'Convolutional Neural Network (CNN)' Dan 'Recurrent Neural Network (RNN)'?", "id": "CNN Untuk Gambar, RNN Untuk Data Urutan." },
  { "en": "Bedanya 'Natural Language Processing (NLP)' Dan 'Natural Language Understanding (NLU)'?", "id": "NLU Sub-bidang Dari NLP, Fokus Pada Pemahaman." },
  { "en": "Bedanya 'Stemming' Dan 'Lemmatization'?", "id": "Lemmatization Lebih Akurat, Menggunakan Kamus." },
  { "en": "Bedanya 'Bag of Words' Dan 'TF-IDF'?", "id": "TF-IDF Memberi Bobot Pada Kata-kata Penting." },
  { "en": "Apa Bedanya 'Koordinasi Isolasi' Dan 'Proteksi Lonjakan'?", "id": "Koordinasi Menentukan Level, Proteksi Melindungi." },
  { "en": "Bedanya 'Relay Buchholz' Dan 'Relay Diferensial'?", "id": "Buchholz Deteksi Gas Trafo, Diferensial Selisih Arus." },
  { "en": "Bedanya 'Gangguan Tanah' Dan 'Gangguan Fasa'?", "id": "Gangguan Tanah Libatkan Ground, Fasa Antar Fasa." },
  { "en": "Bedanya 'Metode Tuning Ziegler-Nichols' Dan 'Cohen-Coon'?", "id": "Keduanya Metode Tuning PID, Cohen-Coon Untuk Tundaan Besar." },
  { "en": "Bedanya 'Matriks Kontrolabilitas' Dan 'Matriks Observabilitas'?", "id": "Kontrolabilitas Kemampuan Mengontrol, Observabilitas Kemampuan Mengamati." },
  { "en": "Bedanya 'LQR (Linear Quadratic Regulator)' Dan 'Kontroler PID'?", "id": "LQR Optimal, PID Heuristik Dan Lebih Umum." },
  { "en": "Bedanya 'Fenomena Gibbs' Dan 'Kebocoran Spektral'?", "id": "Kebocoran Spektral Disebabkan Oleh Jendela (Windowing)." },
  { "en": "Bedanya 'Zero Padding' Dan 'Windowing'?", "id": "Zero Padding Tingkatkan Resolusi Frekuensi, Windowing Kurangi Kebocoran." },
  { "en": "Bedanya 'Transformasi Hilbert' Dan 'Transformasi Fourier'?", "id": "Hilbert Hasilkan Sinyal Analitik, Geser Fasa 90 Derajat." },
  { "en": "Bedanya 'DS-CDMA' Dan 'FH-CDMA'?", "id": "DS Sebar Dengan Kode, FH Lompat Frekuensi." },
  { "en": "Bedanya 'Penerima Rake' Dan 'Equalizer'?", "id": "Rake Manfaatkan Multipath, Equalizer Melawan Multipath." },
  { "en": "Bedanya 'Hard Handoff' Dan 'Soft Handoff'?", "id": "Hard Putus Dulu Sambung, Soft Sambung Dulu Putus." },
  { "en": "Bedanya 'Resonator Rongga (Cavity)' Dan 'Resonator LC'?", "id": "Rongga Untuk Frekuensi Sangat Tinggi, Kualitas Lebih Baik." },
  { "en": "Bedanya 'Dioda Gunn' Dan 'Dioda IMPATT'?", "id": "Keduanya Sumber Gelombang Mikro, Mekanisme Berbeda." },
  { "en": "Bedanya 'CMP (Chemical Mechanical Polishing)' Dan 'Etching'?", "id": "CMP Meratakan Permukaan, Etching Menghilangkan Material." },
  { "en": "Bedanya 'Hukum Moore' Dan 'Skala Dennard'?", "id": "Moore Tentang Jumlah, Dennard Tentang Kepadatan Daya." },
  { "en": "Bedanya 'Segmentasi Gambar' Dan 'Deteksi Objek'?", "id": "Segmentasi Kelompokkan Piksel, Deteksi Temukan Objek." },
  { "en": "Bedanya 'Ekstraksi Fitur' Dan 'Seleksi Fitur'?", "id": "Ekstraksi Buat Fitur Baru, Seleksi Pilih Fitur Ada." },
  { "en": "Bedanya 'Dinamika Maju' Dan 'Dinamika Mundur'?", "id": "Maju Hitung Percepatan, Mundur Hitung Torsi Sendi." },
  { "en": "Bedanya 'Kontrol Torsi Terhitung' Dan 'Kontrol PID'?", "id": "Kontrol Torsi Lebih Canggih, Berbasis Model." },
  { "en": "Bedanya 'Prinsip Linearitas' Dan 'Superposisi'?", "id": "Linearitas Terdiri Dari Homogenitas Dan Aditivitas (Superposisi)." },
  { "en": "Bedanya 'Homogenitas' Dan 'Aditivitas'?", "id": "Homogenitas Sifat Penskalaan, Aditivitas Sifat Penjumlahan." },
  { "en": "Bedanya 'Client-Side Rendering' Dan 'Server-Side Rendering'?", "id": "Server-Side Render Di Server, Client-Side Di Browser." },
  { "en": "Bedanya 'SQL Injection' Dan 'XSS (Cross-Site Scripting)'?", "id": "SQL Serang Database, XSS Serang Pengguna Lain." },
  { "en": "Bedanya 'SMP (Symmetric Multiprocessing)' Dan 'AMP (Asymmetric Multiprocessing)'?", "id": "SMP Semua Inti Sama, AMP Inti Berbeda." },
  { "en": "Bedanya 'Piezoelektrik' Dan 'Ferroelektrik'?", "id": "Ferroelektrik Punya Polarisasi Spontan, Piezoelektrik Tidak." },
  { "en": "Bedanya 'Piroelektrik' Dan 'Termoelektrik'?", "id": "Piroelektrik Hasilkan Tegangan Dari Perubahan Suhu." },
  { "en": "Bedanya 'Paket BGA (Ball Grid Array)' Dan 'LGA (Land Grid Array)'?", "id": "BGA Bola Solder Di Paket, LGA Di Soket." },
  { "en": "Bedanya 'Paket QFP (Quad Flat Package)' Dan 'DIP (Dual In-line Package)'?", "id": "QFP Punya Pin Di Empat Sisi, DIP Dua Sisi." },
  { "en": "Bedanya 'URL (Uniform Resource Locator)' Dan 'URI (Uniform Resource Identifier)'?", "id": "URL Adalah Jenis Spesifik Dari URI." },
  { "en": "Bedanya 'URI (Uniform Resource Identifier)' Dan 'URN (Uniform Resource Name)'?", "id": "URN Nama Unik, URI Bisa Nama Atau Lokasi." },
  { "en": "Bedanya 'Lambda Calculus' Dan 'Mesin Turing'?", "id": "Keduanya Model Komputasi Universal Yang Ekuivalen." },
  { "en": "Bedanya 'Kompleksitas Waktu' Dan 'Kompleksitas Ruang'?", "id": "Waktu Tentang Langkah Eksekusi, Ruang Tentang Memori." },
  { "en": "Bedanya 'Notasi Big-O' Dan 'Big-Omega'?", "id": "Big-O Batas Atas, Big-Omega Batas Bawah." },
  { "en": "Bedanya 'Struktur Data Array' Dan 'Linked List'?", "id": "Array Memori Berurutan, Linked List Pakai Pointer." },
  { "en": "Bedanya 'Stack' Dan 'Queue'?", "id": "Stack LIFO (Last-In, First-Out), Queue FIFO (First-In, First-Out)." },
  { "en": "Bedanya 'Pohon Biner (Binary Tree)' Dan 'Pohon Biner Pencarian (BST)'?", "id": "BST Punya Aturan Urutan, Kiri Lebih Kecil Kanan Lebih Besar." },
  { "en": "Bedanya 'Graph' Dan 'Tree'?", "id": "Tree Adalah Jenis Graph Tanpa Siklus." },
  { "en": "Bedanya 'Breadth-First Search (BFS)' Dan 'Depth-First Search (DFS)'?", "id": "BFS Eksplorasi Per Level, DFS Eksplorasi Mendalam." },
  { "en": "Bedanya 'Algoritma Dijkstra' Dan 'A*'?", "id": "A* Menggunakan Heuristik, Lebih Cepat Dari Dijkstra." },
  { "en": "Bedanya 'Algoritma Greedy' Dan 'Pemrograman Dinamis'?", "id": "Greedy Ambil Pilihan Lokal, Dinamis Pecah Sub-masalah." },
  { "en": "Bedanya 'Rekursi' Dan 'Iterasi'?", "id": "Rekursi Panggil Diri Sendiri, Iterasi Gunakan Perulangan." },
  { "en": "Bedanya 'Mealy Machine' Dan 'Turing Machine'?", "id": "Mesin Turing Jauh Lebih Kuat, Punya Memori Tak Terbatas." },
  { "en": "Bedanya 'Bahasa Regular' Dan 'Context-Free'?", "id": "Context-Free Lebih Ekspresif, Dapat Menghitung." },
  { "en": "Bedanya 'Finite Automata Deterministik (DFA)' Dan 'Non-deterministik (NFA)'?", "id": "NFA Bisa Punya Banyak Transisi, DFA Hanya Satu." },
  { "en": "Bedanya 'Topologi Jaringan Fisik' Dan 'Logis'?", "id": "Fisik Tata Letak Kabel, Logis Aliran Data." },
  { "en": "Bedanya 'Collision Domain' Dan 'Broadcast Domain'?", "id": "Switch Pisah Collision, Router Pisah Broadcast." },
  { "en": "Bedanya 'CSMA/CD' Dan 'CSMA/CA'?", "id": "CD Deteksi Tabrakan, CA Hindari Tabrakan." },
  { "en": "Bedanya 'Token Ring' Dan 'Ethernet'?", "id": "Token Ring Gunakan Token, Ethernet Gunakan CSMA/CD." },
  { "en": "Bedanya 'IPv4 Checksum' Dan 'TCP Checksum'?", "id": "IPv4 Cek Header, TCP Cek Header Dan Data." },
  { "en": "Bedanya 'Fragmentasi' Dan 'Segmentasi'?", "id": "Fragmentasi Di Lapisan Jaringan, Segmentasi Di Lapisan Transport." },
  { "en": "Bedanya 'Classful Addressing' Dan 'Classless Addressing (CIDR)'?", "id": "Classless Lebih Fleksibel Dalam Alokasi Alamat IP." },
  { "en": "Bedanya 'Soket' Dan 'Port'?", "id": "Soket Kombinasi Alamat IP Dan Nomor Port." },
  { "en": "Bedanya 'Epoll' Dan 'Select'?", "id": "Epoll Jauh Lebih Efisien Untuk Banyak Koneksi." },
  { "en": "Bedanya 'Process ID (PID)' Dan 'Thread ID (TID)'?", "id": "Semua Thread Dalam Proses Punya PID Yang Sama." },
  { "en": "Bedanya 'Zombie Process' Dan 'Orphan Process'?", "id": "Zombie Selesai Tapi Tabel Proses Masih Ada." },
  { "en": "Bedanya 'Spinlock' Dan 'Mutex'?", "id": "Spinlock Terus Cek, Mutex 'Tidur'." },
  { "en": "Bedanya 'Race Condition' Dan 'Deadlock'?", "id": "Race Condition Akses Bersamaan, Deadlock Saling Menunggu." },
  { "en": "Bedanya 'Atomic Operation' Dan 'Critical Section'?", "id": "Atomic Tak Bisa Diinterupsi, Critical Section Dilindungi." },
  { "en": "Bedanya 'Memory-Mapped I/O' Dan 'Port-Mapped I/O'?", "id": "Memory-Mapped Gunakan Alamat Sama, Port-Mapped Alamat Khusus." },
  { "en": "Bedanya 'Interrupt Vector' Dan 'Interrupt Service Routine (ISR)'?", "id": "Vector Alamat, ISR Adalah Kode Yang Dijalankan." },
  { "en": "Bedanya 'Maskable Interrupt' Dan 'Non-Maskable Interrupt (NMI)'?", "id": "Maskable Bisa Diabaikan, NMI Tidak Bisa." },
  { "en": "Bedanya 'Level-Triggered' Dan 'Edge-Triggered' Interrupt?", "id": "Level Sensitif Level Sinyal, Edge Sensitif Perubahan." },
  { "en": "Bedanya 'Direct Memory Access (DMA)' Dan 'PIO (Programmed I/O)'?", "id": "DMA Transfer Data Tanpa CPU, PIO Libatkan CPU." },
  { "en": "Bedanya 'Little Endian' Dan 'Big Endian'?", "id": "Little Simpan Byte Terendah, Big Simpan Byte Tertinggi." },
  { "en": "Bedanya 'Floating Point Precision Tunggal' Dan 'Ganda'?", "id": "Ganda Gunakan 64-bit, Tunggal 32-bit." },
  { "en": "Bedanya 'Normalization' Dan 'Denormalization'?", "id": "Normalization Hilangkan Redundansi, Denormalization Tingkatkan Kinerja." },
  { "en": "Bedanya 'Foreign Key' Dan 'Primary Key'?", "id": "Foreign Referensi Ke Primary, Primary Kunci Unik." },
  { "en": "Bedanya 'Transistor PMOS' Dan 'NMOS'?", "id": "PMOS Gunakan Hole, NMOS Gunakan Elektron." },
  { "en": "Bedanya 'Logika Statis' Dan 'Dinamis'?", "id": "Dinamis Gunakan Kapasitor Untuk Menyimpan, Lebih Cepat." },
  { "en": "Bedanya 'Leakage Power' Dan 'Switching Power'?", "id": "Leakage Saat Diam, Switching Saat Berubah." },
  { "en": "Bedanya 'Setup Time' Dan 'Hold Time Violation'?", "id": "Setup Data Terlambat, Hold Data Terlalu Cepat." },
  { "en": "Bedanya 'Clock Skew' Dan 'Clock Jitter'?", "id": "Skew Perbedaan Waktu Tiba, Jitter Variasi Waktu." },
  { "en": "Bedanya 'Glitches' Dan 'Hazards'?", "id": "Hazard Potensi Glitch, Glitch Transisi Tak Diinginkan." },
  { "en": "Bedanya 'Static Hazard' Dan 'Dynamic Hazard'?", "id": "Static Output Salah Sesaat, Dynamic Berubah Berkali-kali." },
  { "en": "Bedanya 'Power Gating' Dan 'Clock Gating'?", "id": "Clock Gating Hentikan Clock, Power Gating Matikan Daya." },
  { "en": "Bedanya 'Level Shifter' Dan 'Buffer'?", "id": "Level Shifter Mengubah Level Tegangan Logika." },
  { "en": "Bedanya 'Antenna Gain' Dan 'Antenna Efficiency'?", "id": "Gain Adalah Directivity Dikalikan Dengan Efisiensi." },
  { "en": "Bedanya 'Radome' Dan 'Reflector'?", "id": "Radome Pelindung Antena, Reflector Memfokuskan Sinyal." },
  { "en": "Bedanya 'Beamwidth' Dan 'Sidelobe Level'?", "id": "Beamwidth Lebar Sinar Utama, Sidelobe Level Sinar Samping." },
  { "en": "Bedanya 'Front-to-Back Ratio' Dan 'Gain'?", "id": "F/B Ratio Perbandingan Gain Depan Dan Belakang." },
  { "en": "Bedanya 'Circular Polarization' Dan 'Linear Polarization'?", "id": "Circular Vektor Medan Listrik Berputar." },
  { "en": "Bedanya 'Scattering' Dan 'Reflection'?", "id": "Scattering Pantulan Ke Banyak Arah." },
  { "en": "Bedanya 'Knife-Edge Diffraction' Dan 'Fresnel Zone'?", "id": "Knife-Edge Model Sederhana, Fresnel Zone Analisis Rinci." },
  { "en": "Bedanya 'Multipath Fading' Dan 'Doppler Spread'?", "id": "Multipath Karena Pantulan, Doppler Karena Gerakan." },
  { "en": "Bedanya 'Coherence Time' Dan 'Coherence Bandwidth'?", "id": "Ukuran Stabilitas Kanal Di Waktu Dan Frekuensi." },
  { "en": "Bedanya 'Intersymbol Interference (ISI)' Dan 'Co-Channel Interference (CCI)'?", "id": "ISI Dari Simbol Sendiri, CCI Dari Pengguna Lain." },
  { "en": "Bedanya 'Adjacent Channel Interference (ACI)' Dan 'Co-Channel Interference (CCI)'?", "id": "ACI Dari Kanal Sebelah, CCI Dari Kanal Sama." },
  { "en": "Bedanya 'Link Budget' Dan 'Power Budget'?", "id": "Link Budget Hitung Semua Gain Dan Loss." },
  { "en": "Bedanya 'Effective Isotropic Radiated Power (EIRP)' Dan 'Transmitter Power'?", "id": "EIRP Memperhitungkan Gain Antena Pemancar." },
  { "en": "Bedanya 'Figure of Merit (G/T)' Dan 'Carrier-to-Noise Ratio (C/N)'?", "id": "G/T Kualitas Penerima, C/N Kualitas Sinyal." },
  { "en": "Bedanya 'BER (Bit Error Rate)' Dan 'PER (Packet Error Rate)'?", "id": "PER Mengukur Kesalahan Per Paket, Bukan Per Bit." },
  { "en": "Bedanya 'Forward Error Correction (FEC)' Dan 'Automatic Repeat Request (ARQ)'?", "id": "FEC Perbaiki Error, ARQ Minta Kirim Ulang." },
  { "en": "Bedanya 'Convolutional Code' Dan 'Turbo Code'?", "id": "Turbo Code Lebih Kuat, Mendekati Batas Shannon." },
  { "en": "Bedanya 'Viterbi Algorithm' Dan 'BCJR Algorithm'?", "id": "BCJR Optimal Untuk Kode Turbo, Viterbi Untuk Konvolusional." },
  { "en": "Bedanya 'LDPC (Low-Density Parity-Check) Code' Dan 'Polar Code'?", "id": "Keduanya Kode Modern Dengan Kinerja Sangat Baik." },
  { "en": "Apa Bedanya 'Elektromigrasi' Dan 'Migrasi Stres'?", "id": "Elektromigrasi Karena Arus, Migrasi Stres Karena Tegangan Mekanis." },
  { "en": "Bedanya 'Latch-up' Dan 'Soft Error'?", "id": "Latch-up Fisik Dan Permanen, Soft Error Transien." },
  { "en": "Bedanya 'Transistor FinFET' Dan 'Gate-All-Around (GAA) FET'?", "id": "GAA Punya Kontrol Gerbang Yang Lebih Baik." },
  { "en": "Bedanya 'Silikon (Si)' Dan 'Galium Nitrida (GaN)'?", "id": "GaN Untuk Frekuensi Dan Daya Yang Lebih Tinggi." },
  { "en": "Bedanya 'Kontrol Droop' Dan 'Kontrol Isokron'?", "id": "Droop Bagi Beban, Isokron Jaga Frekuensi Tepat." },
  { "en": "Bedanya 'PMU (Phasor Measurement Unit)' Dan 'SCADA (Supervisory Control and Data Acquisition)'?", "id": "PMU Lebih Cepat Dan Sinkron, Untuk Analisis Dinamis." },
  { "en": "Bedanya 'Transmisi HVDC (High Voltage Direct Current)' Dan 'HVAC (High Voltage Alternating Current)'?", "id": "HVDC Lebih Efisien Untuk Jarak Sangat Jauh." },
  { "en": "Bedanya 'Kriteria Stabilitas Nyquist' Dan 'Bode'?", "id": "Keduanya Menganalisis Stabilitas, Nyquist Lebih Umum." },
  { "en": "Bedanya 'Saturasi Aktuator' Dan 'Integrator Windup'?", "id": "Saturasi Menyebabkan Fenomena Integrator Windup." },
  { "en": "Bedanya 'MIMO (Multiple-Input Multiple-Output)' Dan 'MISO (Multiple-Input Single-Output)'?", "id": "MIMO Gunakan Banyak Antena Di Kedua Sisi." },
  { "en": "Bedanya 'SIMO (Single-Input Multiple-Output)' Dan 'SISO (Single-Input Single-Output)'?", "id": "SIMO Gunakan Keanekaragaman Penerimaan (Receive Diversity)." },
  { "en": "Bedanya 'Beamforming' Dan 'Diversity'?", "id": "Beamforming Arahkan Sinyal, Diversity Lawan Fading." },
  { "en": "Bedanya 'Directional Coupler' Dan 'Power Divider'?", "id": "Coupler Ambil Sampel, Divider Bagi Daya." },
  { "en": "Bedanya 'Parameter Y' Dan 'Parameter Z'?", "id": "Y Admitansi, Z Impedansi." },
  { "en": "Bedanya 'Parameter S' Dan 'Parameter ABCD'?", "id": "S Untuk Frekuensi Tinggi, ABCD Untuk Jaringan Bertingkat." },
  { "en": "Bedanya 'Ekualisasi Histogram' Dan 'Peregangan Kontras'?", "id": "Ekualisasi Lebih Canggih, Meratakan Distribusi." },
  { "en": "Bedanya 'Filter Domain Spasial' Dan 'Domain Frekuensi'?", "id": "Spasial Operasi Pada Piksel, Frekuensi Pada Spektrum." },
  { "en": "Bedanya 'Filter Median' Dan 'Filter Gaussian'?", "id": "Median Hapus Noise, Gaussian Lakukan Pemburaman (Blur)." },
  { "en": "Bedanya 'Sample Rate' Dan 'Bit Depth'?", "id": "Sample Rate Resolusi Waktu, Bit Depth Resolusi Amplitudo." },
  { "en": "Bedanya 'Kompresi Lossy' Dan 'Lossless'?", "id": "Lossy Buang Data (MP3), Lossless Tidak (FLAC)." },
  { "en": "Bedanya 'Kontrol Jacobian Transpose' Dan 'Jacobian Inverse'?", "id": "Transpose Lebih Sederhana, Inverse Lebih Akurat." },
  { "en": "Bedanya 'Efektor Akhir' Dan 'Manipulator'?", "id": "Efektor Akhir Alat, Manipulator Lengan Robotnya." },
  { "en": "Bedanya 'Teorema CAP' Dan 'Properti ACID'?", "id": "CAP Untuk Sistem Terdistribusi, ACID Untuk Transaksi." },
  { "en": "Bedanya 'Docker' Dan 'Kubernetes'?", "id": "Docker Buat Container, Kubernetes Mengelolanya." },
  { "en": "Bedanya 'Monorepo' Dan 'Polyrepo'?", "id": "Monorepo Satu Repositori, Polyrepo Banyak Repositori." },
  { "en": "Bedanya 'Probe Aktif' Dan 'Probe Pasif'?", "id": "Aktif Punya Penguat, Untuk Sinyal Cepat." },
  { "en": "Bedanya 'Kristal Fotonik' Dan 'Metamaterial'?", "id": "Fotonik Kontrol Foton, Metamaterial Kontrol Gelombang EM." },
  { "en": "Bedanya 'Efek Elektro-Optik' Dan 'Akusto-Optik'?", "id": "Elektro Dari Listrik, Akusto Dari Suara." },
  { "en": "Bedanya 'Silicon Carbide (SiC)' Dan 'Gallium Nitride (GaN)'?", "id": "SiC Konduktivitas Termal Lebih Baik, GaN Switching Lebih Cepat." },
  { "en": "Bedanya 'Quantum Dot' Dan 'Quantum Well'?", "id": "Dot Terkurung Tiga Dimensi, Well Satu Dimensi." },
  { "en": "Bedanya 'Spintronics' Dan 'Electronics'?", "id": "Spintronics Manfaatkan Spin Elektron, Bukan Hanya Muatan." },
  { "en": "Bedanya 'MEMS (Microelectromechanical Systems)' Dan 'NEMS (Nanoelectromechanical Systems)'?", "id": "NEMS Bekerja Pada Skala Nano, Jauh Lebih Kecil." },
  { "en": "Bedanya 'Stateflow' Dan 'Simulink'?", "id": "Stateflow Untuk Logika Berbasis Event, Simulink Berbasis Waktu." },
  { "en": "Bedanya 'Hardware-in-the-Loop (HIL)' Dan 'Software-in-the-Loop (SIL)'?", "id": "HIL Uji Kontroler Dengan Hardware Nyata." },
  { "en": "Bedanya 'Model-Based Design' Dan 'Desain Tradisional'?", "id": "Model-Based Gunakan Model Untuk Simulasi Dan Verifikasi." },
  { "en": "Bedanya 'Code Generation' Dan 'Hand Coding'?", "id": "Code Generation Otomatis Dari Model." },
  { "en": "Bedanya 'Rapid Prototyping' Dan 'Production Code'?", "id": "Prototyping Cepat, Produksi Andal Dan Efisien." },
  { "en": "Bedanya 'Scattering Parameters (S-Parameters)' Dan 'Admittance Parameters (Y-Parameters)'?", "id": "S-Parameters Lebih Mudah Diukur Pada Frekuensi Tinggi." },
  { "en": "Bedanya 'Power Divider' Dan 'Hybrid Coupler'?", "id": "Hybrid Punya Port Terisolasi, Divider Tidak." },
  { "en": "Bedanya 'Return Loss' Dan 'Insertion Loss'?", "id": "Return Loss Daya Pantul, Insertion Loss Daya Terserap." },
  { "en": "Bedanya 'Voltage Standing Wave Ratio (VSWR)' Dan 'Reflection Coefficient'?", "id": "VSWR Representasi Skalar Dari Koefisien Pantul." },
  { "en": "Bedanya 'Noise Floor' Dan 'Signal-to-Noise Ratio (SNR)'?", "id": "Noise Floor Tingkat Noise Bawaan Sistem." },
  { "en": "Bedanya 'Third-Order Intercept Point (IP3)' Dan 'Second-Order Intercept Point (IP2)'?", "id": "IP3 Ukuran Distorsi Intermodulasi Ganjil, IP2 Genap." },
  { "en": "Bedanya 'Phase Noise' Dan 'Frequency Stability'?", "id": "Phase Noise Fluktuasi Jangka Pendek, Stabilitas Jangka Panjang." },
  { "en": "Bedanya 'Local Oscillator (LO)' Dan 'Reference Oscillator'?", "id": "LO Hasilkan Frekuensi Campuran, Reference Beri Stabilitas." },
  { "en": "Bedanya 'Upconverter' Dan 'Downconverter'?", "id": "Upconverter Naikkan Frekuensi (Pemancar), Downconverter Turunkan (Penerima)." },
  { "en": "Bedanya 'Image Rejection Ratio (IRR)' Dan 'Selectivity'?", "id": "IRR Kemampuan Menolak Frekuensi Bayangan (Image)." },
  { "en": "Bedanya 'Direct Conversion' Dan 'Low-IF Receiver'?", "id": "Low-IF Gunakan Frekuensi Antara Rendah, Hindari Masalah DC." },
  { "en": "Bedanya 'Time Division Duplexing (TDD)' Dan 'Frequency Division Duplexing (FDD)'?", "id": "TDD Berbagi Waktu, FDD Berbagi Frekuensi." },
  { "en": "Bedanya 'Code Division Multiple Access (CDMA)' Dan 'Time Division Multiple Access (TDMA)'?", "id": "CDMA Berbagi Kode, TDMA Berbagi Waktu." },
  { "en": "Bedanya 'Orthogonal Variable Spreading Factor (OVSF)' Dan 'Walsh Codes'?", "id": "OVSF Mempertahankan Ortogonalitas Antara Kode Berbeda Panjang." },
  { "en": "Bedanya 'Pilot Channel' Dan 'Traffic Channel'?", "id": "Pilot Bawa Sinyal Referensi, Traffic Bawa Data." },
  { "en": "Bedanya 'Paging Channel' Dan 'Access Channel'?", "id": "Paging Dari Jaringan Ke Ponsel, Access Sebaliknya." },
  { "en": "Bedanya 'Cell Breathing' Dan 'Cell Sectorization'?", "id": "Cell Breathing Perubahan Ukuran Sel, Sectorization Pembagian Sel." },
  { "en": "Bedanya 'Frequency Reuse' Dan 'Frequency Hopping'?", "id": "Reuse Gunakan Ulang, Hopping Lompat Frekuensi." },
  { "en": "Bedanya 'Trunking' Dan 'Multiplexing'?", "id": "Trunking Berbagi Kanal, Multiplexing Gabungkan Sinyal." },
  { "en": "Bedanya 'Peak Power' Dan 'Average Power'?", "id": "Peak Titik Tertinggi, Average Rata-rata Daya." },
  { "en": "Bedanya 'Root Mean Square (RMS)' Dan 'Average Absolute Value'?", "id": "RMS Berkaitan Dengan Daya, Average Dengan Nilai Rata-rata." },
  { "en": "Bedanya ' crest factor' Dan 'Form Factor'?", "id": "Crest Factor Rasio Puncak/RMS, Form Factor Rasio RMS/Average." },
  { "en": "Bedanya 'Total Harmonic Distortion (THD)' Dan 'Signal-to-Noise Ratio (SNR)'?", "id": "THD Distorsi Harmonik, SNR Gangguan Acak." },
  { "en": "Bedanya 'Spurious-Free Dynamic Range (SFDR)' Dan 'Intermodulation Distortion (IMD)'?", "id": "SFDR Rentang Dinamis Bebas Dari Produk IMD." },
  { "en": "Bedanya 'Data Warehouse' Dan 'Database'?", "id": "Warehouse Untuk Analisis, Database Untuk Transaksi." },
  { "en": "Bedanya 'ETL (Extract, Transform, Load)' Dan 'ELT (Extract, Load, Transform)'?", "id": "ELT Transformasi Dilakukan Di Dalam Data Warehouse." },
  { "en": "Bedanya 'Data Mining' Dan 'Data Analysis'?", "id": "Mining Temukan Pola, Analysis Jawab Pertanyaan." },
  { "en": "Bedanya 'Data Lake' Dan 'Data Warehouse'?", "id": "Lake Simpan Data Mentah, Warehouse Data Terstruktur." },
  { "en": "Bedanya 'Big Data' Dan 'Data Biasa'?", "id": "Big Data Punya Volume, Velocity, Dan Variety Besar." },
  { "en": "Bedanya 'Hadoop' Dan 'Spark'?", "id": "Spark Pemrosesan Di Memori, Jauh Lebih Cepat." },
  { "en": "Bedanya 'MapReduce' Dan 'SQL'?", "id": "MapReduce Model Pemrograman Paralel, SQL Bahasa Kueri." },
  { "en": "Bedanya 'Blockchain' Dan 'Database'?", "id": "Blockchain Terdesentralisasi Dan Tidak Dapat Diubah." },
  { "en": "Bedanya 'Proof of Work' Dan 'Proof of Stake'?", "id": "Keduanya Mekanisme Konsensus, Proof of Stake Lebih Efisien." },
  { "en": "Bedanya 'Smart Contract' Dan 'Kontrak Tradisional'?", "id": "Smart Contract Kode Yang Dieksekusi Otomatis." },
  { "en": "Bedanya 'Public Key' Dan 'Private Key'?", "id": "Public Untuk Enkripsi, Private Untuk Dekripsi." },
  { "en": "Bedanya 'Internet of Things (IoT)' Dan 'Machine-to-Machine (M2M)'?", "id": "IoT Lebih Luas, Libatkan Jaringan Dan Manusia." },
  { "en": "Bedanya 'MQTT (Message Queuing Telemetry Transport)' Dan 'HTTP (Hypertext Transfer Protocol)'?", "id": "MQTT Sangat Ringan, Didesain Untuk IoT." },
  { "en": "Bedanya 'Edge Computing' Dan 'Cloud Computing'?", "id": "Edge Proses Data Dekat Sumber, Cloud Jauh." },
  { "en": "Bedanya 'Augmented Reality (AR)' Dan 'Virtual Reality (VR)'?", "id": "AR Tambahkan Ke Dunia Nyata, VR Ciptakan Dunia Baru." },
  { "en": "Bedanya 'Computer Graphics' Dan 'Computer Vision'?", "id": "Graphics Membuat Gambar, Vision Memahami Gambar." },
  { "en": "Bedanya 'Ray Tracing' Dan 'Rasterization'?", "id": "Ray Tracing Lebih Realistis, Rasterization Lebih Cepat." },
  { "en": "Bedanya 'Shader' Dan 'Texture'?", "id": "Shader Program Grafis, Texture Gambar Permukaan." },
  { "en": "Bedanya 'Gimbal Lock' Dan 'Quaternion'?", "id": "Quaternion Solusi Untuk Menghindari Masalah Gimbal Lock." },
  { "en": "Bedanya 'Entropy' Dalam Termodinamika Dan Teori Informasi?", "id": "Keduanya Konsep Ketidakpastian Atau Ketidakteraturan." },
  { "en": "Bedanya 'Mutual Information' Dan 'Correlation'?", "id": "Mutual Information Lebih Umum, Deteksi Hubungan Non-Linier." },
  { "en": "Bedanya 'Kullback-Leibler (KL) Divergence' Dan 'Jarak Euklides'?", "id": "KL Divergence Ukuran Perbedaan Antara Distribusi Probabilitas." },
  { "en": "Bedanya 'Principal Component Analysis (PCA)' Dan 'Linear Discriminant Analysis (LDA)'?", "id": "PCA Unsupervised, LDA Supervised." },
  { "en": "Bedanya 'Generative Model' Dan 'Discriminative Model'?", "id": "Generative Pelajari Distribusi, Discriminative Pelajari Batas." },
  { "en": "Bedanya 'Markov Chain' Dan 'Hidden Markov Model (HMM)'?", "id": "HMM Punya Keadaan Tersembunyi Yang Tidak Teramati." },
  { "en": "Bedanya 'Naive Bayes' Dan 'Logistic Regression'?", "id": "Naive Bayes Generatif, Logistic Regression Diskriminatif." },
  { "en": "Bedanya 'L1 Regularization' Dan 'L2 Regularization'?", "id": "L1 Dapat Menghasilkan Bobot Nol (Seleksi Fitur)." },
  { "en": "Bedanya 'Gradient Descent' Dan 'Stochastic Gradient Descent (SGD)'?", "id": "SGD Gunakan Satu Sampel, Lebih Cepat Tapi Berisik." },
  { "en": "Bedanya 'Activation Function' Dan 'Loss Function'?", "id": "Aktivasi Di Neuron, Loss Ukur Kesalahan Model." },
  { "en": "Bedanya 'Backpropagation' Dan 'Forward Propagation'?", "id": "Forward Hitung Output, Backpropagation Hitung Gradien." },
  { "en": "Bedanya 'Batch Normalization' Dan 'Layer Normalization'?", "id": "Batch Normalisasi Per Batch, Layer Normalisasi Per Sampel." },
  { "en": "Bedanya 'Dropout' Dan 'Regularization'?", "id": "Dropout Teknik Regularisasi Untuk Mencegah Overfitting." },
  { "en": "Bedanya 'Recurrent Neural Network (RNN)' Dan 'Long Short-Term Memory (LSTM)'?", "id": "LSTM Jenis RNN Yang Atasi Masalah Vanishing Gradient." },
  { "en": "Bedanya 'Attention Mechanism' Dan 'Encoder-Decoder'?", "id": "Attention Memungkinkan Model Fokus Pada Bagian Penting." },
  { "en": "Apa Bedanya 'Dioda Laser' Dan 'LED (Light Emitting Diode)'?", "id": "Laser Hasilkan Cahaya Koheren, LED Inkoheren." },
  { "en": "Bedanya 'APD (Avalanche Photodiode)' Dan 'PIN Photodiode'?", "id": "APD Punya Penguatan Internal, PIN Tidak." },
  { "en": "Bedanya 'Efisiensi Kuantum' Dan 'Responsivitas'?", "id": "Keduanya Mengukur Kinerja Detektor, Saling Berkaitan." },
  { "en": "Bedanya 'Bus Ayun (Swing)' Dan 'Bus Slack'?", "id": "Keduanya Istilah Yang Sama Untuk Bus Referensi." },
  { "en": "Bedanya 'Analisis Aliran Daya' Dan 'Analisis Hubung Singkat'?", "id": "Aliran Daya Kondisi Normal, Hubung Singkat Kondisi Gangguan." },
  { "en": "Bedanya 'Metode Gauss-Seidel' Dan 'Newton-Raphson'?", "id": "Newton-Raphson Lebih Cepat Konvergen, Tapi Lebih Kompleks." },
  { "en": "Bedanya 'Wafer' Dan 'Die'?", "id": "Wafer Adalah Piringan, Die Adalah Chip Individual." },
  { "en": "Bedanya 'Wire Bonding' Dan 'Flip-Chip'?", "id": "Flip-Chip Koneksi Langsung, Wire Bonding Pakai Kawat." },
  { "en": "Bedanya 'Rasio Aspek' Dan 'Selektivitas' Dalam Etching?", "id": "Rasio Aspek Kedalaman/Lebar, Selektivitas Laju Etch." },
  { "en": "Bedanya 'RTOS (Real-Time Operating System)' Dan 'GPOS (General Purpose Operating System)'?", "id": "RTOS Punya Waktu Respon Deterministik, GPOS Tidak." },
  { "en": "Bedanya 'Tugas (Task)' Dan 'Thread' Dalam RTOS?", "id": "Tugas Punya Ruang Memori Sendiri, Thread Berbagi." },
  { "en": "Bedanya 'Semaphore Biner' Dan 'Mutex'?", "id": "Mutex Punya Konsep Kepemilikan, Semaphore Tidak." },
  { "en": "Bedanya 'Sound Pressure Level (SPL)' Dan 'Sound Power Level'?", "id": "Power Level Sumber Suara, Pressure Level Di Suatu Titik." },
  { "en": "Bedanya 'Ruang Anechoic' Dan 'Ruang Gema'?", "id": "Anechoic Serap Suara, Gema Pantulkan Suara." },
  { "en": "Bedanya 'Ultrasonik' Dan 'Sinar-X'?", "id": "Ultrasonik Gelombang Suara, Sinar-X Gelombang Elektromagnetik." },
  { "en": "Bedanya 'PET (Positron Emission Tomography)' Dan 'SPECT (Single-Photon Emission Computed Tomography)'?", "id": "PET Punya Sensitivitas Dan Resolusi Lebih Tinggi." },
  { "en": "Bedanya 'Subnet Mask' Dan 'Wildcard Mask'?", "id": "Wildcard Mask Adalah Inversi Dari Subnet Mask." },
  { "en": "Bedanya 'IGP (Interior Gateway Protocol)' Dan 'EGP (Exterior Gateway Protocol)'?", "id": "IGP Dalam Satu Jaringan, EGP Antar Jaringan." },
  { "en": "Bedanya 'Filter Fasa Linier' Dan 'Minimum'?", "id": "Linier Tundaan Konstan, Minimum Tundaan Terkecil." },
  { "en": "Bedanya 'Desimasi' Dan 'Downsampling'?", "id": "Desimasi Adalah Downsampling Setelah Proses Filtering." },
  { "en": "Bedanya 'Pola Desain Singleton' Dan 'Factory'?", "id": "Singleton Pastikan Satu Instansi, Factory Buat Instansi." },
  { "en": "Bedanya 'Arsitektur MVC (Model-View-Controller)' Dan 'MVVM (Model-View-ViewModel)'?", "id": "MVVM Gunakan Data Binding Untuk Sinkronisasi View." },
  { "en": "Bedanya 'Fonon' Dan 'Foton'?", "id": "Fonon Kuantum Getaran Kisi, Foton Kuantum Cahaya." },
  { "en": "Bedanya 'Eksiton' Dan 'Polaron'?", "id": "Eksiton Pasangan Elektron-Hole, Polaron Elektron Dengan Deformasi." },
  { "en": "Bedanya 'Kisi Kristal' Dan 'Sel Satuan (Unit Cell)'?", "id": "Kisi Susunan Titik, Sel Satuan Blok Pembangun." },
  { "en": "Bedanya 'WDM (Wavelength Division Multiplexing)' Dan 'OTDM (Optical Time Division Multiplexing)'?", "id": "WDM Berbagi Panjang Gelombang, OTDM Berbagi Waktu." },
  { "en": "Bedanya 'EDFA (Erbium-Doped Fiber Amplifier)' Dan 'SOA (Semiconductor Optical Amplifier)'?", "id": "EDFA Untuk Serat Optik, SOA Berbasis Semikonduktor." },
  { "en": "Bedanya 'DFT (Design for Testability)' Dan 'DFM (Design for Manufacturability)'?", "id": "DFT Kemudahan Pengujian, DFM Kemudahan Produksi." },
  { "en": "Bedanya 'Fault Coverage' Dan 'Test Coverage'?", "id": "Fault Coverage Persentase Kesalahan Yang Terdeteksi." },
  { "en": "Bedanya 'Stuck-at Fault' Dan 'Transition Fault'?", "id": "Stuck-at Statis, Transition Terkait Waktu." },
  { "en": "Bedanya 'Scan Chain' Dan 'JTAG (Joint Test Action Group)'?", "id": "Scan Chain Bagian Dari Implementasi JTAG." },
  { "en": "Bedanya 'Critical Path' Dan 'False Path'?", "id": "Critical Path Jalur Terpanjang, False Path Tidak Fungsional." },
  { "en": "Bedanya 'Physical Synthesis' Dan 'Logical Synthesis'?", "id": "Physical Memperhitungkan Layout, Logical Hanya Gerbang." },
  { "en": "Bedanya 'Verilog' Dan 'SystemVerilog'?", "id": "SystemVerilog Ekstensi Verilog Untuk Verifikasi Kompleks." },
  { "en": "Bedanya 'UVM (Universal Verification Methodology)' Dan 'OVM (Open Verification Methodology)'?", "id": "UVM Standar Industri Yang Berkembang Dari OVM." },
  { "en": "Bedanya 'Assertion' Dan 'Constraint'?", "id": "Assertion Cek Properti, Constraint Batasi Stimulus Acak." },
  { "en": "Bedanya 'Functional Coverage' Dan 'Code Coverage'?", "id": "Functional Cek Skenario, Code Cek Baris Kode." },
  { "en": "Bedanya 'Formal Property Verification' Dan 'Dynamic Simulation'?", "id": "Formal Buktikan Secara Menyeluruh, Simulasi Uji Kasus." },
  { "en": "Bedanya 'Power Integrity' Dan 'Signal Integrity'?", "id": "Power Integritas Suplai Daya, Signal Integritas Sinyal." },
  { "en": "Bedanya 'Decoupling Capacitor' Dan 'Bypass Capacitor'?", "id": "Keduanya Istilah Yang Sering Digunakan Bergantian." },
  { "en": "Bedanya 'Crosstalk' Dan 'Reflection'?", "id": "Crosstalk Antar Sinyal, Reflection Pantulan Di Ujung." },
  { "en": "Bedanya 'Impedance Matching' Dan 'Termination'?", "id": "Termination Adalah Cara Untuk Mencapai Impedance Matching." },
  { "en": "Bedanya 'Pre-emphasis' Dan 'De-emphasis' Dalam Sinyal?", "id": "Pre-emphasis Tingkatkan Frekuensi Tinggi, De-emphasis Kembalikan." },
  { "en": "Bedanya 'Rise Time' Dan 'Fall Time'?", "id": "Rise Time Waktu Naik, Fall Time Waktu Turun." },
  { "en": "Bedanya 'Duty Cycle' Dan 'Frequency'?", "id": "Duty Cycle Persentase Waktu Aktif, Frekuensi Jumlah Siklus." },
  { "en": "Bedanya 'H-Bridge' Dan 'L298N'?", "id": "L298N Adalah IC Yang Mengandung Dua H-Bridge." },
  { "en": "Bedanya 'Arduino Uno' Dan 'Raspberry Pi'?", "id": "Arduino Mikrokontroler, Raspberry Pi Komputer Papan Tunggal." },
  { "en": "Bedanya 'SPI (Serial Peripheral Interface)' Dan 'I2C (Inter-Integrated Circuit)'?", "id": "SPI Lebih Cepat, I2C Butuh Lebih Sedikit Kabel." },
  { "en": "Bedanya 'CAN (Controller Area Network)' Dan 'Ethernet'?", "id": "CAN Deterministik, Ethernet Berbasis Peluang." },
  { "en": "Bedanya 'Kalman Filter' Dan 'Moving Average Filter'?", "id": "Kalman Filter Optimal, Berbasis Model Sistem." },
  { "en": "Bedanya 'Particle Filter' Dan 'Kalman Filter'?", "id": "Particle Filter Untuk Sistem Non-Linier Non-Gaussian." },
  { "en": "Bedanya 'Alpha-Beta Filter' Dan 'Kalman Filter'?", "id": "Alpha-Beta Adalah Bentuk Sederhana Dari Kalman Filter." },
  { "en": "Bedanya 'Feedforward Control' Dan 'Feedback Control'?", "id": "Feedforward Atasi Gangguan, Feedback Atasi Error." },
  { "en": "Bedanya 'Cascade Control' Dan 'Ratio Control'?", "id": "Cascade Satu Kontroler Atur Kontroler Lain." },
  { "en": "Bedanya 'Smith Predictor' Dan 'PID Controller'?", "id": "Smith Predictor Untuk Mengatasi Tundaan Waktu Besar." },
  { "en": "Bedanya 'Adaptive Control' Dan 'Robust Control'?", "id": "Adaptive Belajar, Robust Kuat Terhadap Ketidakpastian." },
  { "en": "Bedanya 'Fuzzy Logic' Dan 'Boolean Logic'?", "id": "Fuzzy Punya Nilai Antara Benar Dan Salah." },
  { "en": "Bedanya 'Neural Network' Dan 'Fuzzy Logic'?", "id": "Neural Belajar Dari Data, Fuzzy Dari Aturan." },
  { "en": "Bedanya 'Genetic Algorithm' Dan 'Simulated Annealing'?", "id": "Keduanya Algoritma Optimisasi Metaheuristik." },
  { "en": "Bedanya 'Gradient Descent' Dan 'Newton's Method'?", "id": "Newton Gunakan Informasi Orde Dua, Konvergen Lebih Cepat." },
  { "en": "Bedanya 'Linear Programming' Dan 'Nonlinear Programming'?", "id": "Linear Fungsi Objektif Dan Batasan Linier." },
  { "en": "Bedanya 'Integer Programming' Dan 'Linear Programming'?", "id": "Integer Variabel Harus Bilangan Bulat." },
  { "en": "Bedanya 'Convex Optimization' Dan 'Non-Convex Optimization'?", "id": "Convex Jaminan Solusi Optimal Global." },
  { "en": "Bedanya 'Least Squares' Dan 'Least Absolute Deviations'?", "id": "Least Squares Sensitif Terhadap Outlier." },
  { "en": "Bedanya 'Eigenvalue Decomposition' Dan 'Singular Value Decomposition (SVD)'?", "id": "SVD Lebih Umum, Berlaku Untuk Semua Matriks." },
  { "en": "Bedanya 'Covariance Matrix' Dan 'Correlation Matrix'?", "id": "Korelasi Adalah Kovariansi Yang Dinormalisasi." },
  { "en": "Bedanya 'Probability Density Function (PDF)' Dan 'Cumulative Distribution Function (CDF)'?", "id": "CDF Adalah Integral Dari PDF." },
  { "en": "Bedanya 'Mean (Rata-rata)' Dan 'Median'?", "id": "Mean Sensitif Terhadap Outlier, Median Tidak." },
  { "en": "Bedanya 'Standard Deviation' Dan 'Variance'?", "id": "Standard Deviation Adalah Akar Kuadrat Dari Variance." },
  { "en": "Bedanya 'Skewness' Dan 'Kurtosis'?", "id": "Skewness Kemiringan Distribusi, Kurtosis Keruncingan." },
  { "en": "Bedanya 'Bayesian Probability' Dan 'Frequentist Probability'?", "id": "Bayesian Libatkan Kepercayaan Awal, Frequentist Frekuensi Jangka Panjang." },
  { "en": "Bedanya 'Hypothesis Testing' Dan 'Confidence Interval'?", "id": "Hypothesis Testing Menjawab Ya/Tidak, Confidence Interval Beri Rentang." },
  { "en": "Bedanya 'P-value' Dan 'Alpha Level'?", "id": "P-value Dibandingkan Dengan Alpha Untuk Menolak Hipotesis." },
  { "en": "Bedanya 'Type I Error' Dan 'Type II Error'?", "id": "Tipe I Salah Menolak, Tipe II Gagal Menolak." },
  { "en": "Bedanya 'Parametric Test' Dan 'Non-Parametric Test'?", "id": "Parametrik Asumsikan Distribusi, Non-Parametrik Tidak." },
  { "en": "Bedanya 'ANOVA' Dan 'T-test'?", "id": "T-test Bandingkan Dua Grup, ANOVA Lebih Dari Dua." },
  { "en": "Bedanya 'Chi-Squared Test' Dan 'T-test'?", "id": "Chi-Squared Untuk Data Kategorikal." },
  { "en": "Bedanya 'Linear Regression' Dan 'Polynomial Regression'?", "id": "Polynomial Dapat Menangkap Hubungan Non-Linier." },
  { "en": "Bedanya 'Simple Regression' Dan 'Multiple Regression'?", "id": "Simple Satu Variabel Prediktor, Multiple Banyak Variabel." },
  { "en": "Bedanya 'Coefficient of Determination (R-squared)' Dan 'Correlation Coefficient (r)'?", "id": "R-squared Adalah Kuadrat Dari r." },
  { "en": "Bedanya 'Autocorrelation' Dan 'Partial Autocorrelation'?", "id": "Partial Hilangkan Efek Tundaan Antara." },
  { "en": "Bedanya 'Stationary Series' Dan 'Non-Stationary Series'?", "id": "Stationary Properti Statistik Konstan Seiring Waktu." },
  { "en": "Bedanya 'White Noise' Dan 'Random Walk'?", "id": "Random Walk Adalah Integrasi Dari White Noise." },
  { "en": "Bedanya 'ARIMA Model' Dan 'Exponential Smoothing'?", "id": "ARIMA Lebih Fleksibel, Exponential Smoothing Lebih Sederhana." },
  { "en": "Bedanya 'Fourier Series' Dan 'Fourier Integral'?", "id": "Series Untuk Sinyal Periodik, Integral Untuk Aperiodik." },
  { "en": "Bedanya 'Laplace Transform' Dan 'Fourier Transform'?", "id": "Laplace Lebih Umum, Termasuk Respon Transien." },
  { "en": "Bedanya 'Z-Transform' Dan 'Discrete-Time Fourier Transform (DTFT)'?", "id": "DTFT Adalah Z-Transform Pada Lingkaran Satuan." },
  { "en": "Bedanya 'Region of Convergence (ROC)' Dan 'Frequency Response'?", "id": "ROC Terkait Stabilitas, Respon Frekuensi Perilaku Sinyal." },
  { "en": "Bedanya 'Pole' Dan 'Zero' Dalam Fungsi Transfer?", "id": "Pole Tentukan Stabilitas, Zero Tentukan Respon." },
  { "en": "Bedanya 'Bode Plot' Dan 'Pole-Zero Plot'?", "id": "Bode Respon Frekuensi, Pole-Zero Lokasi Pole Dan Zero." },
  { "en": "Bedanya 'Impulse Response' Dan 'Step Response'?", "id": "Step Response Adalah Integral Dari Impulse Response." },
  { "en": "Bedanya 'Time Constant' Dan 'Settling Time'?", "id": "Settling Time Sekitar Empat Hingga Lima Time Constant." },
  { "en": "Bedanya 'Natural Response' Dan 'Forced Response'?", "id": "Natural Respon Internal, Forced Respon Eksternal." },
  { "en": "Bedanya 'Transient Response' Dan 'Steady-State Response'?", "id": "Transien Bagian Awal, Steady-State Bagian Akhir." },
  { "en": "Apa Bedanya 'Rotasi Faraday' Dan 'Efek Kerr'?", "id": "Faraday Terkait Magnet, Kerr Terkait Medan Listrik." },
  { "en": "Bedanya 'Gelombang Evanescent' Dan 'Gelombang Merambat'?", "id": "Evanescent Meluruh Cepat, Tidak Membawa Energi." },
  { "en": "Bedanya 'SVM (Space Vector Modulation)' Dan 'Third-Harmonic Injection PWM'?", "id": "SVM Punya Pemanfaatan Tegangan Bus DC Lebih Tinggi." },
  { "en": "Bedanya 'Filter Aktif' Dan 'Pasif' Dalam Kualitas Daya?", "id": "Aktif Lebih Baik Atasi Harmonisa, Tapi Lebih Kompleks." },
  { "en": "Bedanya 'Unity Power Factor (UPF)' Dan 'Power Factor Correction (PFC)'?", "id": "PFC Adalah Proses Untuk Mencapai UPF." },
  { "en": "Bedanya 'Massa Efektif' Dan 'Massa Diam' Elektron?", "id": "Massa Efektif Dalam Kristal, Berbeda Dari Massa Diam." },
  { "en": "Bedanya 'Kepadatan Keadaan (Density of States)' Dan 'Distribusi Fermi-Dirac'?", "id": "Kepadatan Keadaan Tempat, Distribusi Probabilitas Pengisian." },
  { "en": "Bedanya 'Desibel (dB)' Dan 'Phon'?", "id": "Phon Adalah Ukuran Kenyaringan Subjektif." },
  { "en": "Bedanya 'Efek Doppler' Dan 'Gelombang Kejut'?", "id": "Gelombang Kejut Terjadi Saat Melebihi Kecepatan Suara." },
  { "en": "Bedanya 'ILP (Instruction Level Parallelism)' Dan 'TLP (Thread Level Parallelism)'?", "id": "ILP Dalam Satu Thread, TLP Antar Thread." },
  { "en": "Bedanya 'Prediksi Cabang' Dan 'Eksekusi Spekulatif'?", "id": "Prediksi Memilih Jalur, Spekulatif Mengeksekusi Jalur." },
  { "en": "Bedanya 'Quality of Service (QoS)' Dan 'Layanan Best-Effort'?", "id": "QoS Memberi Prioritas, Best-Effort Tidak Ada Jaminan." },
  { "en": "Bedanya 'Traffic Shaping' Dan 'Traffic Policing'?", "id": "Shaping Menunda Paket, Policing Membuang Paket." },
  { "en": "Bedanya 'Data I-Q' Dan 'Data Riil'?", "id": "Data I-Q Representasi Kompleks, Bawa Informasi Fasa." },
  { "en": "Bedanya 'Sinyal Analitik' Dan 'Sinyal Riil'?", "id": "Sinyal Analitik Tidak Punya Komponen Frekuensi Negatif." },
  { "en": "Bedanya 'Group Delay' Dan 'Phase Delay'?", "id": "Group Delay Tundaan Paket, Phase Delay Tundaan Fasa." },
  { "en": "Bedanya 'Lingkaran Gain' Dan 'Stabilitas' Pada Smith Chart?", "id": "Lingkaran Gain Untuk Desain Penguat, Stabilitas Untuk Osilasi." },
  { "en": "Bedanya 'Noise Figure' Dan 'Noise Factor'?", "id": "Noise Figure Adalah Noise Factor Dalam Desibel." },
  { "en": "Bedanya 'Ruang Konfigurasi' Dan 'Ruang Tugas'?", "id": "Konfigurasi Ruang Sendi, Tugas Ruang Efektor Akhir." },
  { "en": "Bedanya 'Dinamika Lagrangian' Dan 'Newton-Euler'?", "id": "Lagrangian Berbasis Energi, Newton-Euler Berbasis Gaya." },
  { "en": "Bedanya 'EMG (Electromyography)' Dan 'EEG (Electroencephalography)'?", "id": "EMG Aktivitas Listrik Otot, EEG Aktivitas Otak." },
  { "en": "Bedanya 'Penguat Biopotensial' Dan 'Penguat Instrumentasi'?", "id": "Biopotensial Didesain Khusus Untuk Sinyal Fisiologis." },
  { "en": "Bedanya 'Konkurensi' Dan 'Paralelisme'?", "id": "Konkurensi Berjalan Bergantian, Paralelisme Berjalan Bersamaan." },
  { "en": "Bedanya 'Deadlock' Dan 'Livelock'?", "id": "Deadlock Diam, Livelock Sibuk Tapi Tidak Maju." },
  { "en": "Bedanya 'SCADA' Dan 'HMI (Human-Machine Interface)'?", "id": "HMI Adalah Bagian Tampilan Dari Sistem SCADA." },
  { "en": "Bedanya 'FMEA (Failure Mode and Effects Analysis)' Dan 'FTA (Fault Tree Analysis)'?", "id": "FMEA Bottom-Up, FTA Top-Down." },
  { "en": "Bedanya 'HDL (Hardware Description Language)' Dan 'Bahasa Pemrograman'?", "id": "HDL Mendeskripsikan Hardware, Bukan Urutan Perintah." },
  { "en": "Bedanya 'Blocking Assignment' Dan 'Non-Blocking Assignment' Dalam Verilog?", "id": "Blocking Sekuensial, Non-Blocking Paralel." },
  { "en": "Bedanya 'Variabel Wire' Dan 'Reg' Dalam Verilog?", "id": "Wire Untuk Koneksi, Reg Untuk Menyimpan Nilai." },
  { "en": "Bedanya 'Parameter' Dan 'Localparam'?", "id": "Parameter Dapat Diubah, Localparam Konstanta Lokal." },
  { "en": "Bedanya 'Fungsi' Dan 'Tugas (Task)' Dalam Verilog?", "id": "Fungsi Kembalikan Nilai, Tugas Tidak." },
  { "en": "Bedanya 'Initial Block' Dan 'Always Block'?", "id": "Initial Berjalan Sekali, Always Berulang." },
  { "en": "Bedanya 'Case Statement' Dan 'If-Else Statement'?", "id": "Case Lebih Efisien Untuk Banyak Pilihan." },
  { "en": "Bedanya 'For Loop' Dan 'Generate Block'?", "id": "Generate Membuat Hardware Berulang Saat Kompilasi." },
  { "en": "Bedanya 'Sintesis' Dan 'Simulasi'?", "id": "Sintesis Membuat Hardware, Simulasi Memverifikasi Perilaku." },
  { "en": "Bedanya 'Behavioral Modeling' Dan 'Structural Modeling'?", "id": "Behavioral Deskripsi Fungsi, Structural Interkoneksi Komponen." },
  { "en": "Bedanya 'Simulasi Pre-Synthesis' Dan 'Post-Synthesis'?", "id": "Post-Synthesis Memasukkan Informasi Gerbang Logika." },
  { "en": "Bedanya 'Timing Simulation' Dan 'Functional Simulation'?", "id": "Timing Memasukkan Informasi Waktu Tunda (Delay)." },
  { "en": "Bedanya 'Testbench' Dan 'Design Under Test (DUT)'?", "id": "Testbench Memberi Stimulus Dan Memeriksa DUT." },
  { "en": "Bedanya 'Golden Model' Dan 'Behavioral Model'?", "id": "Golden Model Adalah Referensi Yang Telah Terverifikasi." },
  { "en": "Bedanya 'Floating Point Unit (FPU)' Dan 'Arithmetic Logic Unit (ALU)'?", "id": "FPU Untuk Bilangan Koma, ALU Untuk Integer." },
  { "en": "Bedanya 'Cache Koherensi' Dan 'Cache Konsistensi'?", "id": "Koherensi Antar Cache, Konsistensi Cache Dan Memori." },
  { "en": "Bedanya 'Protokol Snooping' Dan 'Berbasis Direktori'?", "id": "Keduanya Untuk Menjaga Koherensi Cache." },
  { "en": "Bedanya 'False Sharing' Dan 'True Sharing'?", "id": "False Sharing Konflik Karena Data Berbeda Di Baris Sama." },
  { "en": "Bedanya 'Prefetching' Dan 'Caching'?", "id": "Prefetching Ambil Data Sebelum Dibutuhkan." },
  { "en": "Bedanya 'Translation Lookaside Buffer (TLB)' Dan 'Cache'?", "id": "TLB Adalah Cache Untuk Terjemahan Alamat Virtual." },
  { "en": "Bedanya 'Page Fault' Dan 'Cache Miss'?", "id": "Page Fault Di Memori Virtual, Cache Miss Di Cache." },
  { "en": "Bedanya 'Demand Paging' Dan 'Prepaging'?", "id": "Demand Muat Saat Perlu, Prepaging Muat Terlebih Dahulu." },
  { "en": "Bedanya 'Working Set' Dan 'Resident Set'?", "id": "Working Set Yang Dibutuhkan, Resident Set Yang Di Memori." },
  { "en": "Bedanya 'Thrashing' Dan 'Paging'?", "id": "Thrashing Terlalu Banyak Paging, Kinerja Menurun." },
  { "en": "Bedanya 'Internal Fragmentation' Dan 'External Fragmentation'?", "id": "Internal Ruang Sisa Di Blok, Eksternal Antar Blok." },
  { "en": "Bedanya 'Compaction' Dan 'Garbage Collection'?", "id": "Compaction Mengatasi Fragmentasi Eksternal." },
  { "en": "Bedanya 'File Descriptor' Dan 'Inode'?", "id": "Inode Simpan Metadata, File Descriptor Tunjuk Ke Inode." },
  { "en": "Bedanya 'Hard Link' Dan 'Symbolic Link (Soft Link)'?", "id": "Hard Link Tunjuk Inode, Symbolic Link Tunjuk Nama File." },
  { "en": "Bedanya 'Journaling File System' Dan 'Non-Journaling'?", "id": "Journaling Lebih Tahan Terhadap Kegagalan." },
  { "en": "Bedanya 'Block Device' Dan 'Character Device'?", "id": "Block Transfer Per Blok, Character Per Karakter." },
  { "en": "Bedanya 'Device Driver' Dan 'Kernel Module'?", "id": "Driver Adalah Modul Kernel Untuk Kendalikan Perangkat." },
  { "en": "Bedanya 'System Call' Dan 'Library Call'?", "id": "Library Call Bungkus System Call Agar Lebih Mudah." },
  { "en": "Bedanya 'Context Switch' Dan 'Mode Switch'?", "id": "Context Switch Pindah Antar Proses, Mode Switch Ubah Hak." },
  { "en": "Bedanya 'Scheduler Preemptive' Dan 'Non-Preemptive'?", "id": "Preemptive Dapat Menginterupsi Proses Yang Berjalan." },
  { "en": "Bedanya 'Penjadwalan FCFS (First-Come, First-Served)' Dan 'SJF (Shortest Job First)'?", "id": "SJF Optimal Tapi Sulit Diprediksi." },
  { "en": "Bedanya 'Penjadwalan Round Robin' Dan 'Priority'?", "id": "Round Robin Beri Jatah Waktu, Priority Beri Prioritas." },
  { "en": "Bedanya 'Starvation' Dan 'Priority Inversion'?", "id": "Priority Inversion Tugas Prioritas Tinggi Tertunda." },
  { "en": "Bedanya 'Banker's Algorithm' Dan 'Deadlock Detection'?", "id": "Banker's Algorithm Mencegah Deadlock, Detection Mendeteksi." },
  { "en": "Bedanya 'Client-Server Model' Dan 'Peer-to-Peer (P2P) Model'?", "id": "P2P Tidak Ada Server Pusat, Semua Setara." },
  { "en": "Bedanya 'RPC (Remote Procedure Call)' Dan 'Socket Programming'?", "id": "RPC Abstraksi Level Tinggi Di Atas Soket." },
  { "en": "Bedanya 'Serialization' Dan 'Marshalling'?", "id": "Istilah Yang Sering Digunakan Secara Bergantian." },
  { "en": "Bedanya 'Idempotent' Dan 'Non-Idempotent' Operation?", "id": "Idempotent Dipanggil Berkali-kali, Hasilnya Sama." },
  { "en": "Bedanya 'Two-Phase Commit' Dan 'Three-Phase Commit'?", "id": "Three-Phase Commit Lebih Tahan Terhadap Kegagalan." },
  { "en": "Bedanya 'Paxos' Dan 'Raft'?", "id": "Keduanya Algoritma Konsensus, Raft Didesain Lebih Mudah." },
  { "en": "Bedanya 'CAP Theorem' Dan 'PACELC Theorem'?", "id": "PACELC Memperluas CAP Dengan Trade-off Latency-Consistency." },
  { "en": "Bedanya 'Eventual Consistency' Dan 'Strong Consistency'?", "id": "Eventual Konsisten Pada Akhirnya, Strong Seketika." },
  { "en": "Bedanya 'Gossip Protocol' Dan 'Leader Election'?", "id": "Gossip Sebarkan Info, Leader Election Pilih Koordinator." },
  { "en": "Bedanya 'Consistent Hashing' Dan 'Standard Hashing'?", "id": "Consistent Hashing Minimalkan Perubahan Saat Server Berubah." },
  { "en": "Bedanya 'Merkle Tree' Dan 'Hash Table'?", "id": "Merkle Tree Verifikasi Data, Hash Table Simpan Data." },
  { "en": "Bedanya 'Circuit Breaker Pattern' Dan 'Retry Pattern'?", "id": "Circuit Breaker Mencegah Kegagalan Berulang." },
  { "en": "Bedanya 'Sidecar Pattern' Dan 'Proxy Pattern'?", "id": "Sidecar Terkait Erat Dengan Aplikasi Utama." },
  { "en": "Bedanya 'Infrastructure as Code (IaC)' Dan 'Konfigurasi Manual'?", "id": "IaC Otomatisasi, Reproducibel, Dan Terdokumentasi." },
  { "en": "Bedanya 'CI (Continuous Integration)' Dan 'CD (Continuous Delivery)'?", "id": "CI Integrasi Kode, CD Otomatisasi Rilis." },
  { "en": "Bedanya 'Continuous Delivery' Dan 'Continuous Deployment'?", "id": "Deployment Otomatis Rilis Ke Produksi." },
  { "en": "Bedanya 'Canary Release' Dan 'Blue-Green Deployment'?", "id": "Canary Rilis Bertahap, Blue-Green Ganti Instansi." },
  { "en": "Bedanya 'Observability' Dan 'Monitoring'?", "id": "Observability Kemampuan Bertanya, Monitoring Pengumpulan Data." },
  { "en": "Bedanya 'Logs' Dan 'Metrics'?", "id": "Logs Catatan Event, Metrics Ukuran Numerik." },
  { "en": "Bedanya 'Tracing' Dan 'Logging'?", "id": "Tracing Mengikuti Alur Permintaan Antar Layanan." },
  { "en": "Bedanya 'Service Level Agreement (SLA)' Dan 'Service Level Objective (SLO)'?", "id": "SLO Target Internal, SLA Janji Ke Pelanggan." },
  { "en": "Bedanya 'Mean Time To Recovery (MTTR)' Dan 'Mean Time Between Failures (MTBF)'?", "id": "MTTR Waktu Perbaikan, MTBF Waktu Antar Kegagalan." },
  { "en": "Bedanya 'Post-mortem' Dan 'Root Cause Analysis (RCA)'?", "id": "RCA Adalah Bagian Dari Proses Post-mortem." },
  { "en": "Bedanya 'Chaos Engineering' Dan 'Disaster Recovery Testing'?", "id": "Chaos Menyuntikkan Kegagalan, Disaster Recovery Uji Rencana." },
  { "en": "Bedanya 'Feature Flag' Dan 'Environment Variable'?", "id": "Feature Flag Kontrol Fitur Saat Runtime." },
  { "en": "Bedanya 'Gitflow' Dan 'GitHub Flow'?", "id": "GitHub Flow Jauh Lebih Sederhana Dari Gitflow." },
  { "en": "Bedanya 'Technical Debt' Dan 'Bug'?", "id": "Debt Jalan Pintas, Bug Kesalahan Fungsional." },
  { "en": "Bedanya 'Refactoring' Dan 'Rewriting'?", "id": "Refactoring Ubah Internal, Rewriting Tulis Ulang." },
  { "en": "Bedanya 'SOLID Principles' Dan 'Design Patterns'?", "id": "SOLID Prinsip Desain, Patterns Solusi Umum." },
  { "en": "Bedanya 'Coupling' Dan 'Cohesion'?", "id": "Coupling Ketergantungan Antar Modul, Cohesion Keterkaitan Dalam Modul." },
  { "en": "Bedanya 'YAGNI (You Ain't Gonna Need It)' Dan 'KISS (Keep It Simple, Stupid)'?", "id": "YAGNI Hindari Fitur, KISS Hindari Kompleksitas." },
  { "en": "Bedanya 'DRY (Don't Repeat Yourself)' Dan 'WET (Write Everything Twice)'?", "id": "DRY Mendorong Reusabilitas, WET Adalah Anti-Pattern." },
  { "en": "Apa Bedanya 'Qubit' Dan 'Bit'?", "id": "Qubit Bisa Superposisi, Bit Hanya Satu Atau Nol." },
  { "en": "Bedanya 'Gerbang Kuantum' Dan 'Gerbang Klasik'?", "id": "Gerbang Kuantum Operasi Pada Qubit, Dapat Dibalikkan." },
  { "en": "Bedanya 'Algoritma Shor' Dan 'Algoritma Grover'?", "id": "Shor Untuk Faktorisasi, Grover Untuk Pencarian." },
  { "en": "Bedanya 'Plasmon Permukaan' Dan 'Fonon-Polariton'?", "id": "Plasmon Kopling Foton-Elektron, Polariton Foton-Fonon." },
  { "en": "Bedanya 'Optik Medan Dekat' Dan 'Medan Jauh'?", "id": "Medan Dekat Melampaui Batas Difraksi." },
  { "en": "Bedanya 'MPC (Model Predictive Control)' Dan 'Kontrol PID'?", "id": "MPC Gunakan Model Untuk Prediksi Masa Depan." },
  { "en": "Bedanya 'SMC (Sliding Mode Control)' Dan 'Kontrol Adaptif'?", "id": "SMC Sangat Robust, Adaptif Belajar Parameter." },
  { "en": "Bedanya 'Kontrol H-infinity' Dan 'LQR'?", "id": "H-infinity Fokus Pada Robustness, LQR Pada Kinerja Optimal." },
  { "en": "Bedanya 'Penerowongan Kuantum (Tunneling)' Dan 'Emisi Termionik'?", "id": "Tunneling Tembus Penghalang, Termionik Lewati Penghalang." },
  { "en": "Bedanya 'Rekombinasi Auger' Dan 'Rekombinasi SRH (Shockley-Read-Hall)'?", "id": "Auger Libatkan Tiga Partikel, SRH Lewat Perangkap." },
  { "en": "Bedanya 'Semikonduktor Degenerate' Dan 'Non-degenerate'?", "id": "Degenerate Berperilaku Seperti Logam." },
  { "en": "Bedanya 'Cepstrum' Dan 'Spektrum'?", "id": "Cepstrum Adalah Spektrum Dari Spektrum Logaritmik." },
  { "en": "Bedanya 'Filter Homomorfik' Dan 'Filter Linier'?", "id": "Homomorfik Memisahkan Sinyal Yang Tergabung Secara Multiplikatif." },
  { "en": "Bedanya 'Filter Kalman' Dan 'Filter Wiener'?", "id": "Kalman Rekursif Dan Untuk Waktu Nyata." },
  { "en": "Bedanya 'Teorema Source Coding' Dan 'Channel Coding'?", "id": "Source Tentang Kompresi, Channel Tentang Transmisi Andal." },
  { "en": "Bedanya 'GAN (Generative Adversarial Network)' Dan 'VAE (Variational Autoencoder)'?", "id": "GAN Hasilkan Sampel Lebih Tajam, VAE Lebih Stabil." },
  { "en": "Bedanya 'Model Transformer' Dan 'RNN/LSTM'?", "id": "Transformer Gunakan Mekanisme Atensi, Bukan Rekurensi." },
  { "en": "Bedanya 'Batch Gradient Descent' Dan 'Mini-batch'?", "id": "Mini-batch Kompromi Antara Batch Dan Stochastic." },
  { "en": "Bedanya 'MMC (Modular Multilevel Converter)' Dan 'CHB (Cascaded H-Bridge)'?", "id": "MMC Punya Lengan, CHB Rangkaian Seri H-Bridge." },
  { "en": "Bedanya 'SDN (Software-Defined Networking)' Dan 'Jaringan Tradisional'?", "id": "SDN Pisahkan Bidang Kontrol Dan Data." },
  { "en": "Bedanya 'NFV (Network Functions Virtualization)' Dan 'SDN (Software-Defined Networking)'?", "id": "NFV Virtualisasi Fungsi, SDN Sentralisasi Kontrol." },
  { "en": "Bedanya 'Cipher Blok' Dan 'Cipher Aliran (Stream)'?", "id": "Blok Enkripsi Per Blok, Aliran Enkripsi Per Bit." },
  { "en": "Bedanya 'Mode Operasi CBC (Cipher Block Chaining)' Dan 'ECB (Electronic Codebook)'?", "id": "CBC Lebih Aman, Gunakan Blok Sebelumnya." },
  { "en": "Bedanya 'BLE (Bluetooth Low Energy)' Dan 'Bluetooth Klasik'?", "id": "BLE Dirancang Untuk Konsumsi Daya Sangat Rendah." },
  { "en": "Bedanya 'RFID (Radio-Frequency Identification)' Dan 'NFC (Near-Field Communication)'?", "id": "NFC Jarak Sangat Dekat, Berbasis RFID." },
  { "en": "Bedanya 'Man-in-the-Middle (MitM) Attack' Dan 'Phishing'?", "id": "MitM Cegat Komunikasi, Phishing Tipu Pengguna." },
  { "en": "Bedanya 'Zero-Day Vulnerability' Dan 'Known Vulnerability'?", "id": "Zero-Day Belum Diketahui, Belum Ada Patch." },
  { "en": "Bedanya 'Sandboxing' Dan 'Virtualization'?", "id": "Sandbox Isolasi Aplikasi, Virtualisasi Isolasi Sistem Operasi." },
  { "en": "Bedanya 'Air Gap' Dan 'Firewall'?", "id": "Air Gap Isolasi Fisik Total, Firewall Logis." },
  { "en": "Bedanya 'Steganografi' Dan 'Kriptografi'?", "id": "Steganografi Sembunyikan Pesan, Kriptografi Acak Pesan." },
  { "en": "Bedanya 'Public Key Infrastructure (PKI)' Dan 'Web of Trust'?", "id": "PKI Tersentralisasi (CA), Web of Trust Terdesentralisasi." },
  { "en": "Bedanya 'Certificate Authority (CA)' Dan 'Registration Authority (RA)'?", "id": "RA Verifikasi Identitas, CA Menerbitkan Sertifikat." },
  { "en": "Bedanya 'Certificate Revocation List (CRL)' Dan 'OCSP (Online Certificate Status Protocol)'?", "id": "OCSP Cek Status Real-time, CRL Daftar Periodik." },
  { "en": "Bedanya 'Transport Layer Security (TLS)' Dan 'Secure Sockets Layer (SSL)'?", "id": "TLS Versi Lebih Baru Dan Aman Dari SSL." },
  { "en": "Bedanya 'Perfect Forward Secrecy (PFS)' Dan 'Enkripsi Biasa'?", "id": "PFS Lindungi Sesi Lampau Jika Kunci Bocor." },
  { "en": "Bedanya 'Salt' Dan 'Pepper' Dalam Hashing?", "id": "Salt Unik Per Pengguna, Pepper Rahasia Global." },
  { "en": "Bedanya 'Key Stretching' Dan 'Hashing'?", "id": "Key Stretching Membuat Hashing Menjadi Lambat." },
  { "en": "Bedanya 'HMAC (Hash-based Message Authentication Code)' Dan 'Checksum'?", "id": "HMAC Gunakan Kunci Rahasia, Untuk Otentikasi." },
  { "en": "Bedanya 'OAuth' Dan 'OpenID Connect'?", "id": "OAuth Untuk Otorisasi, OpenID Connect Untuk Otentikasi." },
  { "en": "Bedanya 'SAML (Security Assertion Markup Language)' Dan 'OAuth'?", "id": "SAML Umumnya Untuk SSO Perusahaan." },
  { "en": "Bedanya 'Kerberos' Dan 'SAML'?", "id": "Kerberos Andalkan Tiket, SAML Andalkan Pernyataan (Assertion)." },
  { "en": "Bedanya 'Biometrics' Dan 'Password'?", "id": "Biometrik Sesuatu Yang Anda Miliki, Password Yang Anda Tahu." },
  { "en": "Bedanya 'False Acceptance Rate (FAR)' Dan 'False Rejection Rate (FRR)'?", "id": "FAR Terima Yang Salah, FRR Tolak Yang Benar." },
  { "en": "Bedanya 'Equal Error Rate (EER)' Dan 'Akurasi'?", "id": "EER Titik Di Mana FAR Sama Dengan FRR." },
  { "en": "Bedanya 'Data at Rest' Dan 'Data in Transit'?", "id": "At Rest Tersimpan, In Transit Bergerak." },
  { "en": "Bedanya 'Full Disk Encryption' Dan 'File-Based Encryption'?", "id": "File-Based Lebih Granular, Enkripsi Per File." },
  { "en": "Bedanya 'Threat Modeling' Dan 'Penetration Testing'?", "id": "Modeling Identifikasi Ancaman, Testing Eksploitasi Kerentanan." },
  { "en": "Bedanya 'Vulnerability Assessment' Dan 'Penetration Testing'?", "id": "Assessment Temukan, Testing Coba Manfaatkan." },
  { "en": "Bedanya 'Reverse Engineering' Dan 'Software Development'?", "id": "Reverse Engineering Membongkar, Development Membangun." },
  { "en": "Bedanya 'Static Code Analysis (SAST)' Dan 'Dynamic Code Analysis (DAST)'?", "id": "SAST Analisis Kode, DAST Analisis Aplikasi Berjalan." },
  { "en": "Bedanya 'Fuzzing' Dan 'Unit Testing'?", "id": "Fuzzing Beri Input Acak, Unit Testing Input Spesifik." },
  { "en": "Bedanya 'Principle of Least Privilege' Dan 'Defense in Depth'?", "id": "Least Privilege Batasi Akses, Defense in Depth Banyak Lapis." },
  { "en": "Bedanya 'Separation of Duties' Dan 'Least Privilege'?", "id": "Separation Pisahkan Tugas, Least Privilege Batasi Hak." },
  { "en": "Bedanya 'Non-repudiation' Dan 'Integrity'?", "id": "Non-repudiation Bukti Pengiriman Dan Penerimaan." },
  { "en": "Bedanya 'Confidentiality' Dan 'Privacy'?", "id": "Confidentiality Kerahasiaan Data, Privacy Hak Individu." },
  { "en": "Bedanya 'Availability' Dan 'Uptime'?", "id": "Uptime Metrik, Availability Janji Layanan." },
  { "en": "Bedanya 'Failover' Dan 'Fallback'?", "id": "Failover Otomatis, Fallback Kembali Ke Sistem Sebelumnya." },
  { "en": "Bedanya 'Hot Site' Dan 'Cold Site' Untuk Disaster Recovery?", "id": "Hot Site Siap Pakai, Cold Site Butuh Persiapan." },
  { "en": "Bedanya 'Recovery Point Objective (RPO)' Dan 'Recovery Time Objective (RTO)'?", "id": "RPO Toleransi Kehilangan Data, RTO Toleransi Waktu Henti." },
  { "en": "Bedanya 'Full Backup' Dan 'Incremental Backup'?", "id": "Incremental Hanya Simpan Perubahan Sejak Backup Terakhir." },
  { "en": "Bedanya 'Incremental Backup' Dan 'Differential Backup'?", "id": "Differential Simpan Perubahan Sejak Full Backup Terakhir." },
  { "en": "Bedanya 'Snapshot' Dan 'Backup'?", "id": "Snapshot Instan Dan Lokal, Backup Biasanya Eksternal." },
  { "en": "Bedanya 'High Availability' Dan 'Fault Tolerance'?", "id": "Fault Tolerance Tanpa Waktu Henti, High Availability Minimalkan." },
  { "en": "Bedanya 'Active-Active' Dan 'Active-Passive' Clustering?", "id": "Active-Active Semua Node Bekerja, Active-Passive Satu Standby." },
  { "en": "Bedanya 'Stateless' Dan 'Stateful' Application?", "id": "Stateful Ingat Sesi Klien, Stateless Tidak." },
  { "en": "Bedanya 'Service Discovery' Dan 'Service Registry'?", "id": "Discovery Proses Menemukan, Registry Tempat Menyimpan." },
  { "en": "Bedanya 'Health Check' Dan 'Monitoring'?", "id": "Health Check Cek Status, Monitoring Kumpulkan Metrik." },
  { "en": "Bedanya 'Orchestration' Dan 'Choreography'?", "id": "Orchestration Terpusat, Choreography Terdesentralisasi." },
  { "en": "Bedanya 'Serverless' Dan 'Platform as a Service (PaaS)'?", "id": "Serverless Abstraksi Lebih Tinggi, Bayar Per Eksekusi." },
  { "en": "Bedanya 'Function as a Service (FaaS)' Dan 'Serverless'?", "id": "FaaS Adalah Model Komputasi Inti Dari Serverless." },
  { "en": "Bedanya 'Vendor Lock-in' Dan 'Portabilitas'?", "id": "Lock-in Sulit Pindah Vendor, Portabilitas Mudah." },
  { "en": "Bedanya 'Open Source' Dan 'Proprietary' Software?", "id": "Open Source Kode Terbuka, Proprietary Kode Tertutup." },
  { "en": "Bedanya 'Free Software' Dan 'Open Source'?", "id": "Free Software Fokus Pada Kebebasan, Open Source Pada Praktikalitas." },
  { "en": "Bedanya 'Copyleft' Dan 'Permissive' License?", "id": "Copyleft Wajibkan Turunan Berlisensi Sama." },
  { "en": "Bedanya 'GPL (General Public License)' Dan 'MIT License'?", "id": "GPL Adalah Lisensi Copyleft, MIT Permisif." },
  { "en": "Bedanya 'Static Library' Dan 'Dynamic Library (Shared)'?", "id": "Static Digabung Saat Kompilasi, Dynamic Saat Runtime." },
  { "en": "Bedanya 'Thin Provisioning' Dan 'Thick Provisioning'?", "id": "Thin Alokasi Sesuai Kebutuhan, Thick Alokasi Penuh." },
  { "en": "Bedanya 'Object Storage' Dan 'File Storage'?", "id": "Object Simpan Data Sebagai Objek, File Simpan Hierarkis." },
  { "en": "Bedanya 'Block Storage' Dan 'File Storage'?", "id": "Block Beri Akses Blok Mentah, File Abstraksi File." },
  { "en": "Bedanya 'iSCSI' Dan 'Fibre Channel'?", "id": "Keduanya Protokol Block Storage, iSCSI Lewat Jaringan IP." },
  { "en": "Bedanya 'RAID (Redundant Array of Independent Disks)' Dan 'JBOD (Just a Bunch of Disks)'?", "id": "RAID Gabungkan Disk, JBOD Tidak." },
  { "en": "Bedanya 'Storage Area Network (SAN)' Dan 'Network Attached Storage (NAS)'?", "id": "SAN Block Level, NAS File Level." },
  { "en": "Bedanya 'Data Deduplication' Dan 'Compression'?", "id": "Deduplication Hilangkan Blok Duplikat, Compression Kurangi Ukuran." },
  { "en": "Bedanya 'Firmware' Dan 'BIOS'?", "id": "BIOS Adalah Jenis Firmware Spesifik Untuk PC." },
  { "en": "Bedanya 'Assembly Language' Dan 'Machine Code'?", "id": "Assembly Representasi Teks, Machine Code Representasi Biner." },
  { "en": "Bedanya 'Just-In-Time (JIT) Compilation' Dan 'Ahead-Of-Time (AOT) Compilation'?", "id": "JIT Kompilasi Saat Runtime, AOT Sebelum Runtime." },
  { "en": "Bedanya 'Interpreted Language' Dan 'Compiled Language'?", "id": "Interpreted Dijalankan Baris-per-baris, Compiled Diterjemahkan Dulu." },
  { "en": "Bedanya 'Strongly Typed' Dan 'Weakly Typed' Language?", "id": "Strongly Tipe Data Ketat, Weakly Boleh Campur." },
  { "en": "Bedanya 'Manifest Typing' Dan 'Type Inference'?", "id": "Manifest Tipe Dideklarasikan, Inference Tipe Disimpulkan." },
  { "en": "Bedanya 'Duck Typing' Dan 'Static Typing'?", "id": "Duck Typing Fokus Pada Perilaku, Bukan Tipe." },
  { "en": "Bedanya 'Covariance' Dan 'Contravariance'?", "id": "Keduanya Terkait Pewarisan Tipe Dalam Pemrograman." },
  { "en": "Bedanya 'Pure Function' Dan 'Impure Function'?", "id": "Pure Function Tanpa Efek Samping." },
  { "en": "Bedanya 'Higher-Order Function' Dan 'First-Class Function'?", "id": "Higher-Order Menerima Fungsi, First-Class Diperlakukan Seperti Variabel." },
  { "en": "Bedanya 'Closure' Dan 'Function'?", "id": "Closure Ingat Lingkungan Saat Dibuat." },
  { "en": "Bedanya 'Memoization' Dan 'Caching'?", "id": "Memoization Bentuk Caching Untuk Hasil Fungsi." },
  { "en": "Bedanya 'Lazy Evaluation' Dan 'Eager Evaluation'?", "id": "Lazy Evaluasi Saat Dibutuhkan, Eager Seketika." },
  { "en": "Apa Bedanya 'Transformasi Wavelet Packet' Dan 'Wavelet'?", "id": "Wavelet Packet Menganalisis Komponen Aproksimasi Dan Detail." },
  { "en": "Bedanya 'EMD (Empirical Mode Decomposition)' Dan 'Transformasi Fourier'?", "id": "EMD Untuk Sinyal Non-Stasioner Dan Non-Linier." },
  { "en": "Bedanya 'Algoritma LMS (Least Mean Squares)' Dan 'RLS (Recursive Least Squares)'?", "id": "RLS Konvergen Lebih Cepat, Komputasi Lebih Berat." },
  { "en": "Bedanya 'Massive MIMO' Dan 'MIMO Konvensional'?", "id": "Massive MIMO Gunakan Ratusan Antena Di Base Station." },
  { "en": "Bedanya 'NOMA (Non-Orthogonal Multiple Access)' Dan 'OMA (Orthogonal Multiple Access)'?", "id": "NOMA Melayani Banyak Pengguna Di Sumber Daya Sama." },
  { "en": "Bedanya 'Radio Kognitif' Dan 'SDR (Software-Defined Radio)'?", "id": "Radio Kognitif Cerdas, SDR Fleksibel." },
  { "en": "Bedanya 'Bridging Fault' Dan 'Stuck-at Fault'?", "id": "Bridging Hubung Singkat Antar Jalur, Stuck-at Terjebak." },
  { "en": "Bedanya 'NBTI (Negative-Bias Temperature Instability)' Dan 'PBTI (Positive-Bias Temperature Instability)'?", "id": "NBTI Degradasi PMOS, PBTI Degradasi NMOS." },
  { "en": "Bedanya 'TDDB (Time-Dependent Dielectric Breakdown)' Dan 'Elektromigrasi'?", "id": "TDDB Kegagalan Isolator, Elektromigrasi Kegagalan Konduktor." },
  { "en": "Bedanya 'Algoritma Tomasulo' Dan 'Scoreboarding'?", "id": "Tomasulo Gunakan Register Renaming, Hindari Hazard." },
  { "en": "Bedanya 'Eksekusi Out-of-Order' Dan 'In-Order'?", "id": "Out-of-Order Eksekusi Instruksi Saat Operan Siap." },
  { "en": "Bedanya 'SIMD (Single Instruction, Multiple Data)' Dan 'MIMD (Multiple Instruction, Multiple Data)'?", "id": "SIMD Satu Instruksi Banyak Data, MIMD Banyak Instruksi Banyak Data." },
  { "en": "Bedanya 'Interferometer Fabry-Pérot' Dan 'Mach-Zehnder'?", "id": "Fabry-Pérot Gunakan Refleksi Ganda, Mach-Zehnder Pisah Sinar." },
  { "en": "Bedanya 'Resonator Cincin' Dan 'Kisi Bragg'?", "id": "Cincin Resonansi Dalam Lingkaran, Bragg Refleksi Periodik." },
  { "en": "Bedanya 'Islanding' Dan 'Black Start'?", "id": "Islanding Pemisahan Dari Grid, Black Start Mulai Tanpa Grid." },
  { "en": "Bedanya 'Estimasi Keadaan' Dan 'Aliran Daya'?", "id": "Estimasi Gunakan Pengukuran, Aliran Daya Hitung Kondisi." },
  { "en": "Bedanya 'Analisis Kontingensi' Dan 'Analisis Stabilitas'?", "id": "Kontingensi Jika Komponen Gagal, Stabilitas Jika Ada Gangguan." },
  { "en": "Bedanya 'SLAM (Simultaneous Localization and Mapping)' Dan 'Lokalisasi'?", "id": "SLAM Membangun Peta Sambil Menentukan Lokasi." },
  { "en": "Bedanya 'Robotika Probabilistik' Dan 'Deterministik'?", "id": "Probabilistik Memodelkan Ketidakpastian, Deterministik Tidak." },
  { "en": "Bedanya 'Fungsi Lyapunov' Dan 'Fungsi Biaya (Cost)'?", "id": "Lyapunov Untuk Buktikan Stabilitas, Biaya Untuk Optimisasi." },
  { "en": "Bedanya 'Kontrol Bang-Bang' Dan 'Kontrol Proporsional'?", "id": "Bang-Bang Aksi Ekstrem (On/Off), Proporsional Gradual." },
  { "en": "Bedanya 'ECC (Elliptic Curve Cryptography)' Dan 'RSA'?", "id": "ECC Butuh Kunci Lebih Pendek Untuk Keamanan Sama." },
  { "en": "Bedanya 'AES (Advanced Encryption Standard)' Dan 'DES (Data Encryption Standard)'?", "id": "AES Standar Modern, Jauh Lebih Kuat Dari DES." },
  { "en": "Bedanya 'Muatan' Dan 'Fluks'?", "id": "Muatan Sumber Medan, Fluks Ukuran Medan." },
  { "en": "Bedanya 'Vektor Poynting' Dan 'Iradiansi'?", "id": "Iradiansi Adalah Rata-rata Waktu Vektor Poynting." },
  { "en": "Bedanya 'Arsitektur ARM (Advanced RISC Machine)' Dan 'x86'?", "id": "ARM Berbasis RISC, x86 Berbasis CISC." },
  { "en": "Bedanya 'GPU (Graphics Processing Unit)' Dan 'TPU (Tensor Processing Unit)'?", "id": "TPU Dioptimalkan Khusus Untuk Operasi Tensor (AI)." },
  { "en": "Bedanya 'TensorFlow' Dan 'PyTorch'?", "id": "Keduanya Kerangka Kerja Deep Learning Populer." },
  { "en": "Bedanya 'Keras' Dan 'TensorFlow'?", "id": "Keras Adalah API Level Tinggi Di Atas TensorFlow." },
  { "en": "Bedanya 'Jupyter Notebook' Dan 'Google Colab'?", "id": "Colab Adalah Jupyter Notebook Yang Dijalankan Di Cloud." },
  { "en": "Bedanya 'Anaconda' Dan 'Pip'?", "id": "Anaconda Manajer Lingkungan Dan Paket, Pip Manajer Paket." },
  { "en": "Bedanya 'Virtual Environment' Dan 'Container'?", "id": "Container Isolasi Seluruh Sistem, Virtual Environment Hanya Python." },
  { "en": "Bedanya 'Scikit-learn' Dan 'TensorFlow'?", "id": "Scikit-learn Machine Learning Umum, TensorFlow Deep Learning." },
  { "en": "Bedanya 'Pandas' Dan 'NumPy'?", "id": "NumPy Untuk Array Numerik, Pandas Untuk Data Tabular." },
  { "en": "Bedanya 'Matplotlib' Dan 'Seaborn'?", "id": "Seaborn API Level Tinggi Di Atas Matplotlib." },
  { "en": "Bedanya 'Static Website' Dan 'Dynamic Website'?", "id": "Static Konten Tetap, Dynamic Dihasilkan Oleh Server." },
  { "en": "Bedanya 'JavaScript' Dan 'TypeScript'?", "id": "TypeScript Tambahkan Tipe Statis Ke JavaScript." },
  { "en": "Bedanya 'React' Dan 'Angular'?", "id": "React Pustaka, Angular Kerangka Kerja Lengkap." },
  { "en": "Bedanya 'Vue.js' Dan 'React'?", "id": "Vue Dianggap Lebih Mudah Dipelajari Daripada React." },
  { "en": "Bedanya 'Node.js' Dan 'JavaScript'?", "id": "Node.js Jalankan JavaScript Di Sisi Server." },
  { "en": "Bedanya 'NPM (Node Package Manager)' Dan 'Yarn'?", "id": "Yarn Alternatif NPM, Fokus Pada Kecepatan Dan Keamanan." },
  { "en": "Bedanya 'Express.js' Dan 'Node.js'?", "id": "Express Kerangka Kerja Web Di Atas Node.js." },
  { "en": "Bedanya 'SQLAlchemy' Dan 'psycopg2'?", "id": "SQLAlchemy ORM, psycopg2 Driver Database PostgreSQL." },
  { "en": "Bedanya 'Django' Dan 'Flask'?", "id": "Django Kerangka Kerja Lengkap, Flask Mikro-kerangka Kerja." },
  { "en": "Bedanya 'Web Server' Dan 'Application Server'?", "id": "Web Server Layani Konten Statis, Application Server Logika Bisnis." },
  { "en": "Bedanya 'Apache' Dan 'Nginx'?", "id": "Nginx Lebih Baik Untuk Koneksi Bersamaan Tinggi." },
  { "en": "Bedanya 'Gunicorn' Dan 'uWSGI'?", "id": "Keduanya Server Antarmuka Aplikasi Python (WSGI)." },
  { "en": "Bedanya 'Long Polling' Dan 'WebSocket'?", "id": "WebSocket Komunikasi Dua Arah Penuh, Lebih Efisien." },
  { "en": "Bedanya 'Server-Sent Events (SSE)' Dan 'WebSocket'?", "id": "SSE Komunikasi Satu Arah Dari Server Ke Klien." },
  { "en": "Bedanya 'GraphQL' Dan 'REST'?", "id": "GraphQL Klien Minta Data Yang Tepat Dibutuhkan." },
  { "en": "Bedanya 'gRPC' Dan 'REST'?", "id": "gRPC Gunakan Protobuf, Kinerja Lebih Tinggi." },
  { "en": "Bedanya 'Microkernel' Dan 'Monolithic Kernel'?", "id": "Microkernel Hanya Fungsi Dasar, Monolithic Semua Fungsi." },
  { "en": "Bedanya 'Hypervisor Tipe 1' Dan 'Tipe 2'?", "id": "Tipe 1 Langsung Di Hardware, Tipe 2 Di Atas OS." },
  { "en": "Bedanya 'Paravirtualization' Dan 'Full Virtualization'?", "id": "Paravirtualization Guest OS Sadar Divirtualisasi." },
  { "en": "Bedanya 'RAID 10' Dan 'RAID 01'?", "id": "RAID 10 (Stripe of Mirrors) Lebih Tahan Gagal." },
  { "en": "Bedanya 'Hot Spare' Dan 'Cold Spare'?", "id": "Hot Spare Otomatis Ambil Alih, Cold Spare Manual." },
  { "en": "Bedanya 'In-band' Dan 'Out-of-band' Management?", "id": "In-band Lewat Jaringan Data, Out-of-band Jalur Khusus." },
  { "en": "Bedanya 'Error Correction Code (ECC) Memory' Dan 'Non-ECC Memory'?", "id": "ECC Dapat Mendeteksi Dan Memperbaiki Kesalahan Memori." },
  { "en": "Bedanya 'Single-Rank' Dan 'Dual-Rank' RAM?", "id": "Dual-Rank Punya Dua Set Chip Memori." },
  { "en": "Bedanya 'Clock Speed' Dan 'IPC (Instructions Per Clock)'?", "id": "Kinerja Adalah Perkalian Dari Keduanya." },
  { "en": "Bedanya 'Hyper-Threading' Dan 'Multicore'?", "id": "Hyper-Threading Satu Inti Fisik, Dua Inti Logis." },
  { "en": "Bedanya 'TDP (Thermal Design Power)' Dan 'Konsumsi Daya Aktual'?", "id": "TDP Bukan Konsumsi Daya, Tapi Beban Panas." },
  { "en": "Bedanya 'Chiplet' Dan 'Monolithic Die'?", "id": "Chiplet Gabungkan Banyak Die Kecil Menjadi Satu." },
  { "en": "Bedanya 'PCIe Gen 3' Dan 'Gen 4'?", "id": "Gen 4 Punya Bandwidth Dua Kali Lipat Gen 3." },
  { "en": "Bedanya 'NVMe' Dan 'SATA'?", "id": "NVMe Protokol Penyimpanan Jauh Lebih Cepat Dari SATA." },
  { "en": "Bedanya 'M.2' Dan 'SATA SSD'?", "id": "M.2 Faktor Bentuk, Bisa Gunakan Protokol SATA Atau NVMe." },
  { "en": "Bedanya 'Optane Memory' Dan 'DRAM'?", "id": "Optane Bersifat Non-volatil, Tapi Lebih Lambat Dari DRAM." },
  { "en": "Bedanya 'DirectX' Dan 'OpenGL'?", "id": "Keduanya API Grafis, DirectX Spesifik Untuk Windows." },
  { "en": "Bedanya 'Vulkan' Dan 'OpenGL'?", "id": "Vulkan API Level Rendah, Beri Kontrol Lebih." },
  { "en": "Bedanya 'Ray Tracing' Dan 'Path Tracing'?", "id": "Path Tracing Bentuk Lebih Canggih Dari Ray Tracing." },
  { "en": "Bedanya 'Anti-aliasing' Dan 'Anisotropic Filtering'?", "id": "Anti-aliasing Haluskan Tepi, Anisotropic Tajamkan Tekstur." },
  { "en": "Bedanya 'VSync' Dan 'G-Sync/FreeSync'?", "id": "G-Sync Sinkronkan Refresh Rate Monitor Dengan GPU." },
  { "en": "Bedanya 'Refresh Rate (Hz)' Dan 'Frame Rate (FPS)'?", "id": "Refresh Rate Kemampuan Monitor, Frame Rate Output GPU." },
  { "en": "Bedanya 'Response Time' Dan 'Input Lag'?", "id": "Response Time Piksel, Input Lag Seluruh Sistem." },
  { "en": "Bedanya 'HDR (High Dynamic Range)' Dan 'SDR (Standard Dynamic Range)'?", "id": "HDR Punya Rentang Warna Dan Kecerahan Lebih Luas." },
  { "en": "Bedanya 'Color Gamut' Dan 'Color Space'?", "id": "Gamut Rentang Warna, Space Cara Mendefinisikannya." },
  { "en": "Bedanya 'sRGB' Dan 'Adobe RGB'?", "id": "Adobe RGB Punya Gamut Warna Yang Lebih Luas." },
  { "en": "Bedanya 'DisplayPort' Dan 'HDMI'?", "id": "DisplayPort Umumnya Punya Bandwidth Lebih Tinggi." },
  { "en": "Bedanya 'USB Type-C' Dan 'Thunderbolt'?", "id": "Thunderbolt Gunakan Konektor USB-C, Tapi Jauh Lebih Cepat." },
  { "en": "Bedanya 'USB Power Delivery (PD)' Dan 'Quick Charge'?", "id": "Keduanya Standar Pengisian Cepat, PD Lebih Universal." },
  { "en": "Bedanya 'Wi-Fi 6 (802.11ax)' Dan 'Wi-Fi 5 (802.11ac)'?", "id": "Wi-Fi 6 Lebih Efisien Di Lingkungan Padat." },
  { "en": "Bedanya 'WPA2' Dan 'WPA3'?", "id": "WPA3 Standar Keamanan Wi-Fi Yang Lebih Kuat." },
  { "en": "Bedanya 'Mesh Wi-Fi' Dan 'Range Extender'?", "id": "Mesh Ciptakan Satu Jaringan, Extender Ciptakan Jaringan Terpisah." },
  { "en": "Bedanya 'Powerline Adapter' Dan 'Wi-Fi Extender'?", "id": "Powerline Gunakan Jaringan Listrik, Extender Nirkabel." },
  { "en": "Bedanya 'DAC (Digital-to-Analog Converter)' Eksternal Dan Internal?", "id": "Eksternal Umumnya Punya Kualitas Suara Lebih Baik." },
  { "en": "Bedanya 'Amplifier' Dan 'Receiver'?", "id": "Receiver Adalah Amplifier Dengan Radio Tuner." },
  { "en": "Bedanya 'Impedansi Speaker' Dan 'Sensitivitas Speaker'?", "id": "Impedansi Resistansi, Sensitivitas Efisiensi Suara." },
  { "en": "Bedanya 'Crossover Aktif' Dan 'Pasif'?", "id": "Aktif Pisahkan Frekuensi Sebelum Amplifikasi." },
  { "en": "Bedanya 'Subwoofer' Dan 'Woofer'?", "id": "Subwoofer Didedikasikan Untuk Frekuensi Sangat Rendah." },
  { "en": "Bedanya 'Tweeter' Dan 'Mid-range' Speaker?", "id": "Tweeter Frekuensi Tinggi, Mid-range Frekuensi Tengah." },
  { "en": "Bedanya 'Ported (Bass Reflex)' Dan 'Sealed' Subwoofer?", "id": "Ported Lebih Keras, Sealed Lebih Akurat." },
  { "en": "Bedanya 'Dolby Atmos' Dan 'DTS:X'?", "id": "Keduanya Format Suara Berbasis Objek." },
  { "en": "Bedanya 'Noise Cancelling' Dan 'Noise Isolating'?", "id": "Cancelling Aktif Hapus Noise, Isolating Pasif Blokir." },
  { "en": "Bedanya 'Codec Bluetooth SBC' Dan 'aptX'?", "id": "aptX Punya Kualitas Suara Lebih Baik Dari SBC." },
  { "en": "Apa Bedanya 'Graphene' Dan 'Carbon Nanotubes'?", "id": "Graphene Lembaran Dua Dimensi, Nanotube Tabung Satu Dimensi." },
  { "en": "Bedanya 'Topological Insulator' Dan 'Insulator Konvensional'?", "id": "Topological Insulator Konduktif Di Permukaan, Isolator Di Dalam." },
  { "en": "Bedanya 'Fermion Weyl' Dan 'Fermion Dirac'?", "id": "Weyl Tidak Bermassa, Dirac Punya Massa." },
  { "en": "Bedanya 'GSR (Galvanic Skin Response)' Dan 'HRV (Heart Rate Variability)'?", "id": "GSR Ukur Konduktivitas Kulit, HRV Variasi Detak Jantung." },
  { "en": "Bedanya 'fMRI (Functional MRI)' Dan 'MRI (Magnetic Resonance Imaging)'?", "id": "fMRI Ukur Aktivitas Otak Dengan Aliran Darah." },
  { "en": "Bedanya 'Microelectrode Array' Dan 'Single Electrode'?", "id": "Array Merekam Dari Banyak Neuron Secara Bersamaan." },
  { "en": "Bedanya 'Quantum Annealing' Dan 'Gate-Based Quantum Computing'?", "id": "Annealing Untuk Optimisasi, Gate-Based Untuk Komputasi Universal." },
  { "en": "Bedanya 'Dekoherensi' Dan 'Relaksasi' Kuantum?", "id": "Dekoherensi Kehilangan Fasa, Relaksasi Kehilangan Energi." },
  { "en": "Bedanya 'Phased Array' Dan 'MIMO'?", "id": "Phased Array Arahkan Satu Sinar, MIMO Banyak Sinar." },
  { "en": "Bedanya 'Antena Metamaterial' Dan 'Antena Fraktal'?", "id": "Metamaterial Gunakan Struktur Buatan, Fraktal Geometri Berulang." },
  { "en": "Bedanya 'FlexRay' Dan 'CAN (Controller Area Network)'?", "id": "FlexRay Punya Bandwidth Lebih Tinggi Dan Deterministik." },
  { "en": "Bedanya 'AUTOSAR (Automotive Open System Architecture)' Dan 'Software ECU Tradisional'?", "id": "AUTOSAR Standar Terbuka Untuk Perangkat Lunak Otomotif." },
  { "en": "Bedanya 'ARINC 429' Dan 'MIL-STD-1553'?", "id": "ARINC Untuk Avionik Sipil, MIL-STD Untuk Militer." },
  { "en": "Bedanya 'IMU (Inertial Measurement Unit)' Dan 'GPS (Global Positioning System)'?", "id": "IMU Ukur Gerak Relatif, GPS Posisi Absolut." },
  { "en": "Bedanya 'PROFIBUS' Dan 'PROFINET'?", "id": "PROFIBUS Berbasis Serial, PROFINET Berbasis Ethernet." },
  { "en": "Bedanya 'Modbus RTU' Dan 'Modbus TCP'?", "id": "RTU Serial, TCP Lewat Jaringan Ethernet." },
  { "en": "Bedanya 'VNA (Vector Network Analyzer)' Dan 'Penganalisis Spektrum'?", "id": "VNA Ukur Fasa Dan Amplitudo, Spektrum Hanya Amplitudo." },
  { "en": "Bedanya 'Time-Domain Reflectometry (TDR)' Dan 'Frequency-Domain Reflectometry'?", "id": "Keduanya Untuk Cari Gangguan, Beda Di Domain Analisis." },
  { "en": "Bedanya 'Gilbert Cell' Dan 'Diode Ring Mixer'?", "id": "Gilbert Cell Adalah Mixer Aktif, Ring Diode Pasif." },
  { "en": "Bedanya 'Referensi Bandgap' Dan 'Referensi Dioda Zener'?", "id": "Bandgap Jauh Lebih Stabil Terhadap Perubahan Suhu." },
  { "en": "Bedanya 'BCD (Binary Coded Decimal)' Dan 'Biner'?", "id": "BCD Mengkodekan Setiap Digit Desimal Secara Terpisah." },
  { "en": "Bedanya 'Kode Gray' Dan 'Kode Biner'?", "id": "Kode Gray Hanya Satu Bit Berubah Antar Nilai." },
  { "en": "Bedanya 'DCS (Distributed Control System)' Dan 'PLC (Programmable Logic Controller)'?", "id": "DCS Untuk Kontrol Proses Skala Besar Dan Terdistribusi." },
  { "en": "Bedanya 'RTU (Remote Terminal Unit)' Dan 'PLC'?", "id": "RTU Lebih Tangguh, Untuk Telemetri Jarak Jauh." },
  { "en": "Bedanya 'Osilator Kristal' Dan 'MEMS Oscillator'?", "id": "Osilator MEMS Lebih Kecil Dan Tahan Guncangan." },
  { "en": "Bedanya 'SAW (Surface Acoustic Wave) Filter' Dan 'BAW (Bulk Acoustic Wave) Filter'?", "id": "BAW Untuk Frekuensi Lebih Tinggi, Kinerja Lebih Baik." },
  { "en": "Bedanya 'ADC (Analog-to-Digital Converter) Flash' Dan 'Successive Approximation (SAR)'?", "id": "Flash Sangat Cepat, SAR Kompromi Kecepatan Dan Resolusi." },
  { "en": "Bedanya 'ADC Sigma-Delta' Dan 'SAR'?", "id": "Sigma-Delta Punya Resolusi Sangat Tinggi, Kecepatan Rendah." },
  { "en": "Bedanya 'DAC (Digital-to-Analog Converter) R-2R Ladder' Dan 'String DAC'?", "id": "R-2R Lebih Cepat Dan Presisi." },
  { "en": "Bedanya 'Sample and Hold' Dan 'Track and Hold'?", "id": "Track and Hold Mengikuti Sinyal Sebelum Menahan." },
  { "en": "Bedanya 'Dither' Dan 'Noise Shaping'?", "id": "Keduanya Teknik Untuk Meningkatkan Kinerja Kuantisasi." },
  { "en": "Bedanya 'Integral Nonlinearity (INL)' Dan 'Differential Nonlinearity (DNL)'?", "id": "DNL Error Antar Langkah, INL Deviasi Dari Garis Lurus." },
  { "en": "Bedanya 'Aperture Jitter' Dan 'Clock Jitter'?", "id": "Aperture Jitter Ketidakpastian Waktu Sampling." },
  { "en": "Bedanya 'Aliasing' Dan 'Imaging'?", "id": "Aliasing Di ADC, Imaging Di DAC." },
  { "en": "Bedanya 'Nyquist Rate' Dan 'Nyquist Frequency'?", "id": "Rate Frekuensi Sampling, Frequency Setengahnya." },
  { "en": "Bedanya 'Oversampling' Dan 'Undersampling'?", "id": "Oversampling Di Atas Nyquist, Undersampling Di Bawah." },
  { "en": "Bedanya 'Root Locus' Dan 'Nyquist Plot'?", "id": "Root Locus Variasi Gain, Nyquist Variasi Frekuensi." },
  { "en": "Bedanya 'System Type' Dan 'System Order'?", "id": "Type Jumlah Integrator, Order Pangkat Tertinggi." },
  { "en": "Bedanya 'Error Koefisien Statis' Dan 'Error Steady-State'?", "id": "Koefisien Menentukan Error Steady-State." },
  { "en": "Bedanya 'Phase Lead' Dan 'Phase Lag' Compensator?", "id": "Lead Tingkatkan Stabilitas, Lag Kurangi Error." },
  { "en": "Bedanya 'Dominant Pole' Dan 'Non-Dominant Pole'?", "id": "Dominant Pole Paling Menentukan Respon Sistem." },
  { "en": "Bedanya 'Observasi Kalman' Dan 'Luenberger Observer'?", "id": "Kalman Untuk Sistem Stokastik, Luenberger Deterministik." },
  { "en": "Bedanya 'Direct Current (DC) Motor' Dan 'Alternating Current (AC) Motor'?", "id": "Motor DC Gunakan Arus Searah, AC Bolak-balik." },
  { "en": "Bedanya 'Switched Reluctance Motor' Dan 'Stepper Motor'?", "id": "Switched Reluctance Torsi Lebih Tinggi, Kontrol Kompleks." },
  { "en": "Bedanya 'Field Oriented Control (FOC)' Dan 'Direct Torque Control (DTC)'?", "id": "DTC Respon Torsi Lebih Cepat, FOC Lebih Halus." },
  { "en": "Bedanya 'Reaksi Jangkar' Dan 'Fluks Bocor'?", "id": "Reaksi Jangkar Efek Medan Stator Ke Rotor." },
  { "en": "Bedanya 'Commutation' Dan 'Armature Reaction'?", "id": "Commutation Proses Pembalikan Arus Di Kumparan." },
  { "en": "Bedanya 'Interpole' Dan 'Compensating Winding'?", "id": "Keduanya Untuk Melawan Efek Reaksi Jangkar." },
  { "en": "Bedanya 'Voltage Regulation' Dan 'Speed Regulation'?", "id": "Voltage Untuk Generator, Speed Untuk Motor." },
  { "en": "Bedanya 'Sychronous Condenser' Dan 'Capacitor Bank'?", "id": "Condenser Dapat Menyerap Dan Menyediakan Daya Reaktif." },
  { "en": "Bedanya 'Circuit Breaker Vakum' Dan 'SF6'?", "id": "Vakum Untuk Tegangan Menengah, SF6 Tegangan Tinggi." },
  { "en": "Bedanya 'Relay Elektromekanis' Dan 'Relay Statis'?", "id": "Statis Gunakan Komponen Elektronik, Tanpa Bagian Bergerak." },
  { "en": "Bedanya 'Relay Statis' Dan 'Relay Numerik'?", "id": "Numerik Gunakan Mikroprosesor, Lebih Canggih." },
  { "en": "Bedanya 'Proteksi Jarak' Dan 'Proteksi Arus Lebih'?", "id": "Jarak Ukur Impedansi, Tidak Terpengaruh Arus Beban." },
  { "en": "Bedanya 'Zona Proteksi' Dan 'Koordinasi Proteksi'?", "id": "Zona Area Lindung, Koordinasi Atur Urutan." },
  { "en": "Bedanya 'Auto-recloser' Dan 'Circuit Breaker'?", "id": "Auto-recloser Coba Tutup Kembali Circuit Breaker." },
  { "en": "Bedanya 'Sistem SCADA' Dan 'Sistem Manajemen Energi (EMS)'?", "id": "EMS Punya Fungsi Analisis Dan Optimisasi Lanjut." },
  { "en": "Bedanya 'Wide Area Measurement System (WAMS)' Dan 'SCADA'?", "id": "WAMS Gunakan PMU, Lebih Cepat Dan Sinkron." },
  { "en": "Bedanya 'Kabel Bawah Tanah' Dan 'Saluran Udara'?", "id": "Bawah Tanah Mahal, Tapi Estetis Dan Andal." },
  { "en": "Bedanya 'Konduktor ACSR' Dan 'AAAC'?", "id": "ACSR Inti Baja, AAAC Inti Paduan Aluminium." },
  { "en": "Bedanya 'Efek Kulit' Dan 'Efek Ferranti'?", "id": "Kulit Distribusi Arus, Ferranti Kenaikan Tegangan." },
  { "en": "Bedanya 'Korona' Dan 'Arc Flash'?", "id": "Korona Pelepasan Ion, Arc Flash Ledakan Listrik." },
  { "en": "Bedanya 'Sag' Dan 'Tension' Pada Saluran Transmisi?", "id": "Sag Lendutan, Tension Tegangan Tarik." },
  { "en": "Bedanya 'Isolator Tipe Pin' Dan 'Tipe Gantung'?", "id": "Pin Untuk Tegangan Rendah, Gantung Rangkaian Isolator." },
  { "en": "Bedanya 'Lightning Arrester' Dan 'Surge Arrester'?", "id": "Keduanya Istilah Yang Sering Digunakan Bergantian." },
  { "en": "Bedanya 'Ground Wire' Dan 'Neutral Wire'?", "id": "Ground Untuk Keamanan, Neutral Jalur Kembali Arus." },
  { "en": "Bedanya 'Sistem Tiga Fasa Empat Kawat' Dan 'Tiga Kawat'?", "id": "Empat Kawat Punya Jalur Netral." },
  { "en": "Bedanya 'Beban Linier' Dan 'Non-Linier'?", "id": "Non-Linier Menghasilkan Arus Harmonisa." },
  { "en": "Bedanya 'Faktor K (K-Factor)' Dan 'Faktor Daya'?", "id": "Faktor K Menunjukkan Kemampuan Trafo Tangani Harmonisa." },
  { "en": "Bedanya 'Filter Harmonisa Pasif' Dan 'Aktif'?", "id": "Aktif Dapat Beradaptasi, Pasif Ditala Frekuensi Tertentu." },
  { "en": "Bedanya 'Static VAR Compensator (SVC)' Dan 'STATCOM'?", "id": "STATCOM Berbasis Inverter, Respon Lebih Cepat." },
  { "en": "Bedanya 'Unified Power Flow Controller (UPFC)' Dan 'STATCOM'?", "id": "UPFC Dapat Mengontrol Aliran Daya Aktif Dan Reaktif." },
  { "en": "Bedanya 'Energi' Dan 'Exergy'?", "id": "Exergy Adalah Energi Yang Tersedia Untuk Digunakan." },
  { "en": "Bedanya 'Efisiensi Termal' Dan 'COP (Coefficient of Performance)'?", "id": "Efisiensi Untuk Mesin Kalor, COP Untuk Pompa Kalor." },
  { "en": "Bedanya 'Konduksi' Dan 'Difusi'?", "id": "Konduksi Transfer Panas, Difusi Transfer Massa." },
  { "en": "Bedanya 'Bilangan Prandtl' Dan 'Bilangan Nusselt'?", "id": "Prandtl Sifat Fluida, Nusselt Transfer Kalor Konveksi." },
  { "en": "Bedanya 'Radiasi Benda Hitam' Dan 'Benda Abu-abu'?", "id": "Benda Abu-abu Emisivitasnya Kurang Dari Satu." },
  { "en": "Bedanya 'Hukum Stefan-Boltzmann' Dan 'Hukum Wien'?", "id": "Stefan-Boltzmann Daya Total, Wien Puncak Panjang Gelombang." },
  { "en": "Bedanya 'Absorptivitas' Dan 'Emisivitas'?", "id": "Menurut Kirchhoff, Keduanya Sama Pada Kesetimbangan." },
  { "en": "Bedanya 'Reflektivitas' Dan 'Transmisivitas'?", "id": "Reflektivitas Dipantulkan, Transmisivitas Diteruskan." },
  { "en": "Bedanya 'Aliran Couette' Dan 'Aliran Poiseuille'?", "id": "Couette Karena Gerak Dinding, Poiseuille Beda Tekanan." },
  { "en": "Bedanya 'Lapisan Batas' Dan 'Aliran Bebas'?", "id": "Lapisan Batas Tempat Efek Viskositas Penting." },
  { "en": "Bedanya 'Gaya Apung' Dan 'Gaya Angkat'?", "id": "Apung Karena Fluida, Angkat Karena Aerodinamika." },
  { "en": "Bedanya 'Tekanan Statis' Dan 'Tekanan Dinamis'?", "id": "Tekanan Total Adalah Jumlah Keduanya." },
  { "en": "Bedanya 'Kavitasi' Dan 'Didihan (Boiling)'?", "id": "Kavitasi Pembentukan Uap Karena Penurunan Tekanan." },
  { "en": "Bedanya 'Tegangan Permukaan' Dan 'Kapilaritas'?", "id": "Tegangan Permukaan Adalah Penyebab Kapilaritas." },
  { "en": "Bedanya 'Mekanika Fluida' Dan 'Hidrolika'?", "id": "Hidrolika Aplikasi Mekanika Fluida, Terutama Air." },
  { "en": "Bedanya 'Momen' Dan 'Kopel'?", "id": "Kopel Pasangan Gaya, Hasilkan Momen Murni." },
  { "en": "Bedanya 'Keseimbangan Statis' Dan 'Dinamis'?", "id": "Statis Diam, Dinamis Bergerak Dengan Kecepatan Konstan." },
  { "en": "Bedanya 'Gesekan Statis' Dan 'Kinetis'?", "id": "Gesekan Statis Lebih Besar Dari Kinetis." },
  { "en": "Bedanya 'Pusat Massa' Dan 'Pusat Gravitasi'?", "id": "Keduanya Sama Jika Medan Gravitasi Seragam." },
  { "en": "Bedanya 'Osiloskop Penyimpanan Digital (DSO)' Dan 'Analog'?", "id": "DSO Dapat Menyimpan Dan Menganalisis Bentuk Gelombang." },
  { "en": "Bedanya 'Generator Sinyal' Dan 'Generator Fungsi'?", "id": "Generator Sinyal Lebih Presisi, Untuk Aplikasi RF." },
  { "en": "Bedanya 'Catu Daya Linier' Dan 'Switching'?", "id": "Linier Noise Rendah, Switching Efisiensi Tinggi." },
  { "en": "Bedanya 'Beban Elektronik' Dan 'Resistor Beban'?", "id": "Beban Elektronik Dapat Diprogram Dan Dinamis." },
  { "en": "Bedanya 'LCR Meter' Dan 'Multimeter'?", "id": "LCR Meter Khusus Ukur Induktansi, Kapasitansi, Resistansi." }



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
