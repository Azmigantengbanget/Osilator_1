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


    { "en": "Apa Rangkaian Pembangkit Sinyal Periodik?", "id": "Osilator (Oscillator)." },
    { "en": "Apa Fungsi Utama Osilator?", "id": "Menghasilkan Gelombang Tanpa Sinyal Input." },
    { "en": "Apa Dua Jenis Utama Osilator?", "id": "Sinusoidal Dan Non-Sinusoidal." },
    { "en": "Apa Bentuk Gelombang Osilator Sinusoidal?", "id": "Gelombang Sinus Murni." },
    { "en": "Apa Contoh Osilator Sinusoidal?", "id": "Osilator LC (Inductor-Capacitor), RC (Resistor-Capacitor), Kristal." },
    { "en": "Apa Bentuk Gelombang Osilator Non-Sinusoidal?", "id": "Persegi, Segitiga, Gigi Gergaji." },
    { "en": "Apa Nama Lain Osilator Non-Sinusoidal?", "id": "Osilator Relaksasi (Relaxation Oscillator)." },
    { "en": "Apa Syarat Terjadinya Osilasi?", "id": "Kriteria Stabilitas Barkhausen." },
    { "en": "Apa Dua Syarat Kriteria Barkhausen?", "id": "Penguatan Loop Satu, Pergeseran Fasa Nol." },
    { "en": "Apa Tipe Umpan Balik Yang Dibutuhkan Osilator?", "id": "Umpan Balik Positif (Positive Feedback)." },
    { "en": "Apa Komponen Yang Menentukan Frekuensi Osilator?", "id": "Jaringan Resonansi (Tangki)." },
    { "en": "Apa Jaringan Resonansi Paling Umum?", "id": "Sirkuit LC (Inductor-Capacitor)." },
    { "en": "Apa Osilator Berbasis Sirkuit Tangki LC?", "id": "Osilator LC (Inductor-Capacitor)." },
    { "en": "Osilator LC Mana Yang Menggunakan Pembagi Kapasitif?", "id": "Osilator Colpitts." },
    { "en": "Osilator LC Mana Yang Menggunakan Pembagi Induktif?", "id": "Osilator Hartley." },
    { "en": "Osilator LC Mana Yang Menggunakan Transformator?", "id": "Osilator Armstrong." },
    { "en": "Apa Variasi Osilator Colpitts Untuk Stabilitas?", "id": "Osilator Clapp." },
    { "en": "Apa Keuntungan Osilator LC (Inductor-Capacitor)?", "id": "Baik Untuk Frekuensi Tinggi (RF)." },
    { "en": "Apa Kelemahan Osilator LC (Inductor-Capacitor)?", "id": "Stabilitas Kurang Baik." },
    { "en": "Apa Osilator Berbasis Jaringan Pergeseran Fasa?", "id": "Osilator RC (Resistor-Capacitor)." },
    { "en": "Osilator RC Mana Yang Menggunakan Jaringan Lead-Lag?", "id": "Osilator Jembatan Wien." },
    { "en": "Berapa Pergeseran Fasa Jaringan Wien Pada Resonansi?", "id": "Nol Derajat." },
    { "en": "Apa Osilator RC Yang Menggunakan Tiga Jaringan RC?", "id": "Osilator Geser Fasa (Phase-Shift)." },
    { "en": "Berapa Pergeseran Fasa Setiap Jaringan RC?", "id": "60 Derajat." },
    { "en": "Total Pergeseran Fasa Jaringan RC?", "id": "180 Derajat." },
    { "en": "Apa Keuntungan Osilator RC (Resistor-Capacitor)?", "id": "Baik Untuk Frekuensi Audio." },
    { "en": "Apa Osilator Paling Stabil?", "id": "Osilator Kristal." },
    { "en": "Apa Prinsip Kerja Osilator Kristal?", "id": "Efek Piezoelektrik Terbalik." },
    { "en": "Apa Material Yang Digunakan Osilator Kristal?", "id": "Kristal Kuarsa (Quartz)." },
    { "en": "Apa Rangkaian Ekuivalen Kristal Kuarsa?", "id": "Sirkuit RLC (Resistor-Inductor-Capacitor) Seri-Paralel." },
    { "en": "Apa Itu Frekuensi Resonansi Seri?", "id": "Impedansi Minimum Kristal." },
    { "en": "Apa Itu Frekuensi Resonansi Paralel?", "id": "Impedansi Maksimum Kristal." },
    { "en": "Apa Faktor Kualitas (Q Factor) Kristal?", "id": "Sangat Tinggi." },
    { "en": "Apa Osilator Kristal Paling Umum?", "id": "Osilator Pierce." },
    { "en": "Apa Osilator Non-Sinusoidal Paling Umum?", "id": "Multivibrator." },
    { "en": "Apa Multivibrator Tanpa Keadaan Stabil?", "id": "Multivibrator Astable." },
    { "en": "Apa Fungsi Multivibrator Astable?", "id": "Menghasilkan Gelombang Persegi Kontinu." },
    { "en": "Apa Multivibrator Dengan Satu Keadaan Stabil?", "id": "Multivibrator Monostable." },
    { "en": "Apa Fungsi Multivibrator Monostable?", "id": "Menghasilkan Satu Pulsa (One-Shot)." },
    { "en": "Apa Multivibrator Dengan Dua Keadaan Stabil?", "id": "Multivibrator Bistable." },
    { "en": "Apa Nama Lain Multivibrator Bistable?", "id": "Flip-Flop." },
    { "en": "Apa IC (Integrated Circuit) Pewaktu Yang Populer?", "id": "Timer 555." },
    { "en": "Bagaimana Mengkonfigurasi Timer 555 Sebagai Osilator?", "id": "Mode Astable." },
    { "en": "Apa Yang Menentukan Frekuensi Osilator 555?", "id": "Dua Resistor Dan Satu Kapasitor." },
    { "en": "Apa Osilator Yang Frekuensinya Dikontrol Tegangan?", "id": "VCO (Voltage-Controlled Oscillator)." },
    { "en": "Apa Aplikasi VCO (Voltage-Controlled Oscillator)?", "id": "PLL (Phase-Locked Loop), Synthesizer." },
    { "en": "Apa Osilator Yang Frekuensinya Dikontrol Digital?", "id": "DCO (Digitally-Controlled Oscillator)." },
    { "en": "Apa Osilator Kristal Terkontrol Tegangan?", "id": "VCXO (Voltage-Controlled Crystal Oscillator)." },
    { "en": "Apa Osilator Kristal Terkompensasi Suhu?", "id": "TCXO (Temperature-Compensated Crystal Oscillator)." },
    { "en": "Apa Osilator Kristal Terkontrol Oven?", "id": "OCXO (Oven-Controlled Crystal Oscillator)." },
    { "en": "Osilator Mana Yang Paling Stabil Terhadap Suhu?", "id": "OCXO (Oven-Controlled Crystal Oscillator)." },
    { "en": "Apa Itu Stabilitas Frekuensi?", "id": "Kemampuan Osilator Menjaga Frekuensi." },
    { "en": "Apa Ukuran Stabilitas Jangka Pendek?", "id": "Derau Fasa (Phase Noise)." },
    { "en": "Apa Ukuran Stabilitas Jangka Panjang?", "id": "Penuaan (Aging)." },
    { "en": "Apa Itu Derau Fasa (Phase Noise)?", "id": "Fluktuasi Acak Fasa Sinyal." },
    { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Pada Tepi Sinyal." },
    { "en": "Apa Hubungan Jitter Dan Derau Fasa?", "id": "Jitter Adalah Manifestasi Derau Fasa." },
    { "en": "Apa Itu Penuaan (Aging) Osilator?", "id": "Pergeseran Frekuensi Lambat Seiring Waktu." },
    { "en": "Apa Itu Osilator Relaksasi?", "id": "Bekerja Berdasarkan Pengisian/Pengosongan Kapasitor." },
    { "en": "Contoh Osilator Relaksasi?", "id": "Multivibrator Astable, Generator Gigi Gergaji." },
    { "en": "Apa Osilator Yang Menggunakan Jaringan Kembar-T?", "id": "Osilator Notch Twin-T." },
    { "en": "Apa Itu Osilator Cincin (Ring Oscillator)?", "id": "Osilator Berbasis Inverter Ganjil." },
    { "en": "Apa Osilator Untuk Gelombang Mikro?", "id": "Osilator Gunn, Klystron, Magnetron." },
    { "en": "Apa Osilator Berbasis Dioda Efek Terowongan?", "id": "Osilator Tunnel Diode." },
    { "en": "Apa Osilator Untuk Frekuensi Sangat Rendah?", "id": "Generator Fungsi." },
    { "en": "Apa Itu Osilator Kuadratur?", "id": "Menghasilkan Output Sinus Dan Cosinus." },
    { "en": "Apa Itu Sintesis Frekuensi (Frequency Synthesis)?", "id": "Menghasilkan Banyak Frekuensi Dari Satu Referensi." },
    { "en": "Apa Itu Sintesis Langsung Digital?", "id": "DDS (Direct Digital Synthesis)." },
    { "en": "Apa Komponen Utama DDS (Direct Digital Synthesis)?", "id": "Akumulator Fasa, ROM, DAC." },
    { "en": "Apa Itu Loop Terkunci Fasa?", "id": "PLL (Phase-Locked Loop)." },
    { "en": "Apa Komponen Utama PLL (Phase-Locked Loop)?", "id": "Detektor Fasa, Filter Loop, VCO." },
    { "en": "Apa Fungsi Detektor Fasa?", "id": "Membandingkan Fasa Dua Sinyal." },
    { "en": "Apa Fungsi Filter Loop?", "id": "Menyaring Output Detektor Fasa." },
    { "en": "Apa Itu Rentang Kunci (Lock Range)?", "id": "Rentang Frekuensi PLL Dapat Tetap Terkunci." },
    { "en": "Apa Itu Rentang Tangkap (Capture Range)?", "id": "Rentang Frekuensi PLL Dapat Mulai Mengunci." },
    { "en": "Apa Itu Osilator Lokal?", "id": "LO (Local Oscillator) Di Penerima Radio." },
    { "en": "Apa Itu Frekuensi Fundamental?", "id": "Frekuensi Terendah Yang Dihasilkan Osilator." },
    { "en": "Apa Itu Harmonik?", "id": "Frekuensi Kelipatan Dari Fundamental." },
    { "en": "Bagaimana Cara Mendapatkan Gelombang Persegi?", "id": "Memotong Puncak Gelombang Sinus." },
    { "en": "Bagaimana Cara Mendapatkan Gelombang Segitiga?", "id": "Mengintegrasikan Gelombang Persegi." },
    { "en": "Apa Itu Osilator Pengikut Sumber?", "id": "Osilator Berbasis FET (Field-Effect Transistor)." },
    { "en": "Apa Itu Osilator Butler?", "id": "Osilator Kristal Dengan Penguat Emitor." },
    { "en": "Apa Itu Osilator Franklin?", "id": "Osilator LC (Inductor-Capacitor) Yang Sangat Stabil." },
    { "en": "Apa Itu Osilator Vackar?", "id": "Varian Dari Osilator Colpitts/Clapp." },
    { "en": "Apa Itu Osilator Seiler?", "id": "Varian Lain Dari Osilator Colpitts." },
    { "en": "Apa Itu Osilator Opto-elektronik?", "id": "OEO (Opto-Electronic Oscillator)." },
    { "en": "Apa Itu Osilator MEMS (Micro-Electro-Mechanical Systems)?", "id": "Resonator Mekanis Mikro." },
    { "en": "Apa Itu Distorsi Harmonik?", "id": "Kehadiran Harmonik Yang Tidak Diinginkan." },
    { "en": "Apa Itu THD (Total Harmonic Distortion)?", "id": "Ukuran Total Distorsi Harmonik." },
    { "en": "Bagaimana Cara Menstabilkan Amplitudo Osilator?", "id": "Menggunakan Sirkuit AGC (Automatic Gain Control) Atau Dioda." },
    { "en": "Apa Itu Pulling Frekuensi?", "id": "Perubahan Frekuensi Akibat Variasi Beban." },
    { "en": "Apa Itu Pushing Frekuensi?", "id": "Perubahan Frekuensi Akibat Variasi Catu Daya." },
    { "en": "Apa Itu Start-up Time?", "id": "Waktu Osilator Mulai Berosilasi Stabil." },
    { "en": "Apa Itu Osilasi Parasitik?", "id": "Osilasi Tak Diinginkan Pada Frekuensi Lain." },
    { "en": "Apa Itu Pengunci Injeksi (Injection Locking)?", "id": "Sinkronisasi Osilator Dengan Sinyal Eksternal." },
    { "en": "Apa Itu Osilator Terkopel?", "id": "Beberapa Osilator Yang Saling Mempengaruhi." },
    { "en": "Apa Itu Osilator Chaos?", "id": "Osilator Dengan Perilaku Non-Periodik." },
    { "en": "Apa Itu Sirkuit Chua?", "id": "Sirkuit Elektronik Sederhana Penghasil Chaos." },
    { "en": "Apa Itu Penguat Umpan Balik?", "id": "Rangkaian Penguat Dalam Osilator." },
    { "en": "Apa Itu Jaringan Umpan Balik?", "id": "Jaringan Penentu Frekuensi (LC/RC)." },
    { "en": "Apa Penguatan Loop Terbuka?", "id": "A (Penguatan Penguat)." },
    { "en": "Apa Faktor Umpan Balik?", "id": "β (Beta)." },
    { "en": "Apa Penguatan Loop Tertutup?", "id": "Aβ (Penguatan Total Loop)." },
    { "en": "Berapa Nilai Aβ Agar Osilasi Dimulai?", "id": "Sedikit Lebih Besar Dari Satu." },
    { "en": "Berapa Nilai Aβ Agar Osilasi Stabil?", "id": "Tepat Sama Dengan Satu." },
    { "en": "Berapa Pergeseran Fasa Total Loop?", "id": "360° Atau 0°." },
    { "en": "Berapa Pergeseran Fasa Dari Penguat Inverting?", "id": "180 Derajat." },
    { "en": "Berapa Pergeseran Fasa Jaringan Feedback?", "id": "180 Derajat (Untuk Penguat Inverting)." },
    { "en": "Apa Itu Amplitudo Osilasi?", "id": "Tegangan Puncak Gelombang Yang Dihasilkan." },
    { "en": "Apa Yang Membatasi Amplitudo Osilasi?", "id": "Saturasi Penguat Atau Sirkuit AGC." },
    { "en": "Apa Itu Automatic Gain Control (AGC)?", "id": "Sirkuit Penstabil Amplitudo Otomatis." },
    { "en": "Apa Osilator Berbasis Jembatan?", "id": "Osilator Jembatan Wien." },
    { "en": "Apa Kondisi Keseimbangan Jembatan Wien?", "id": "Saat Jembatan Seimbang." },
    { "en": "Apa Itu Osilator Dengan Resistansi Negatif?", "id": "Osilator Yang Menggunakan Perangkat Resistansi Negatif." },
    { "en": "Contoh Perangkat Resistansi Negatif?", "id": "Tunnel Diode, Gunn Diode." },
    { "en": "Apa Itu Frekuensi Resonansi Sirkuit Tangki?", "id": "Frekuensi Osilasi Alami." },
    { "en": "Apa Itu Redaman (Damping)?", "id": "Hilangnya Energi Dalam Sirkuit Tangki." },
    { "en": "Bagaimana Osilator Mengatasi Redaman?", "id": "Penguat Memberi Energi Kembali." },
    { "en": "Apa Itu Efek Pemuatan (Loading)?", "id": "Beban Mempengaruhi Frekuensi Osilator." },
    { "en": "Bagaimana Mengurangi Efek Pemuatan?", "id": "Menggunakan Sirkuit Penyangga (Buffer)." },
    { "en": "Apa Itu Penyangga (Buffer)?", "id": "Penguat Dengan Gain Satu." },
    { "en": "Apa Itu Osilator Butler?", "id": "Osilator Kristal Dengan Pengikut Emitor." },
    { "en": "Apa Itu Osilator Colpitts FET?", "id": "Osilator Colpitts Menggunakan Transistor FET." },
    { "en": "Apa Itu Osilator Hartley Seri?", "id": "Variasi Dari Osilator Hartley." },
    { "en": "Apa Itu Osilator Hartley Paralel?", "id": "Bentuk Umum Osilator Hartley." },
    { "en": "Apa Itu Osilator Meissner?", "id": "Nama Lain Untuk Osilator Armstrong." },
    { "en": "Apa Itu Osilator Blok?", "id": "Blocking Oscillator." },
    { "en": "Apa Fungsi Blocking Oscillator?", "id": "Menghasilkan Pulsa Sempit." },
    { "en": "Apa Itu Osilator RC Aktif?", "id": "Osilator RC Menggunakan Op-Amp." },
    { "en": "Apa Itu Osilator RC Pasif?", "id": "Tidak Ada, Osilator RC Butuh Penguat." },
    { "en": "Apa Itu Resonator Keramik?", "id": "Alternatif Murah Untuk Kristal Kuarsa." },
    { "en": "Apa Itu Resonator SAW (Surface Acoustic Wave)?", "id": "Resonator Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu Osilator Dielektrik Resonator?", "id": "DRO (Dielectric Resonator Oscillator)." },
    { "en": "Di Mana DRO Digunakan?", "id": "Aplikasi Microwave Frekuensi Stabil." },
    { "en": "Apa Itu Osilator YIG (Yttrium Iron Garnet)?", "id": "Osilator Microwave Yang Dapat Disetel." },
    { "en": "Bagaimana Cara Menyetel Osilator YIG?", "id": "Dengan Mengubah Medan Magnet." },
    { "en": "Apa Itu Osilator Frekuensi Sapu (Sweep)?", "id": "Osilator Yang Frekuensinya Berubah Linear." },
    { "en": "Apa Itu Generator Wobble?", "id": "Nama Lama Untuk Sweep Oscillator." },
    { "en": "Apa Itu Osilator Ketukan (Beat Oscillator)?", "id": "Mencampur Dua Frekuensi Untuk Menghasilkan Selisih." },
    { "en": "Apa Itu Beat Frequency?", "id": "Frekuensi Selisih Hasil Pencampuran." },
    { "en": "Apa Itu BFO (Beat Frequency Oscillator)?", "id": "Digunakan Untuk Demodulasi CW/SSB." },
    { "en": "Apa Itu Clock?", "id": "Osilator Untuk Sirkuit Digital." },
    { "en": "Apa Itu Siklus Clock?", "id": "Satu Periode Lengkap Sinyal Clock." },
    { "en": "Apa Itu Tepi Naik Clock?", "id": "Transisi Dari Logika 0 Ke 1." },
    { "en": "Apa Itu Tepi Turun Clock?", "id": "Transisi Dari Logika 1 Ke 0." },
    { "en": "Apa Itu Osilator Terkendali Gerbang?", "id": "GCO (Gate-Controlled Oscillator)." },
    { "en": "Apa Itu Osilator Terkendali Arus?", "id": "CCO (Current-Controlled Oscillator)." },
    { "en": "Apa Itu Osilator Koaksial?", "id": "Osilator Menggunakan Jalur Koaksial." },
    { "en": "Apa Itu Osilator Garis-Lecher?", "id": "Osilator Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu Osilator Dynatron?", "id": "Osilator Resistansi Negatif Menggunakan Tabung Vakum." },
    { "en": "Apa Itu Osilator Pearson-Anson?", "id": "Osilator Relaksasi Menggunakan Lampu Neon." },
    { "en": "Apa Itu Generator Fungsi?", "id": "Menghasilkan Gelombang Sinus, Persegi, Segitiga." },
    { "en": "Apa Inti Dari Generator Fungsi?", "id": "Osilator Gelombang Persegi Dan Segitiga." },
    { "en": "Bagaimana Gelombang Sinus Dibuat Di Generator Fungsi?", "id": "Membentuk Gelombang Segitiga." },
    { "en": "Apa Itu Osilator Fasa Terkunci Optik?", "id": "OPLL (Optical Phase-Locked Loop)." },
    { "en": "Apa Itu Osilator Kuantum?", "id": "Osilator Berbasis Prinsip Mekanika Kuantum." },
    { "en": "Apa Itu Maser?", "id": "Osilator Atau Penguat Microwave." },
    { "en": "Apa Itu Laser?", "id": "Osilator Atau Penguat Cahaya." },
    { "en": "Apa Itu Sinyal Referensi Frekuensi?", "id": "Osilator Sangat Stabil Sebagai Standar." },
    { "en": "Contoh Referensi Frekuensi?", "id": "Jam Atom." },
    { "en": "Apa Itu Allan Deviation/Variance?", "id": "Ukuran Stabilitas Frekuensi Osilator." },
    { "en": "Apa Itu Spurs (Spurious Signals)?", "id": "Frekuensi Palsu Yang Tidak Diinginkan." },
    { "en": "Apa Itu Resolusi Frekuensi?", "id": "Langkah Perubahan Frekuensi Terkecil." },
    { "en": "Apa Itu Waktu Settling?", "id": "Waktu Frekuensi Stabil Setelah Perubahan." },
    { "en": "Apa Itu Bandwidth Loop?", "id": "Parameter Penting Dalam Desain PLL." },
    { "en": "Apa Itu Faktor Redaman (Damping Factor) PLL?", "id": "Menentukan Respon Transien PLL." },
    { "en": "Apa Itu Referensi Spurious?", "id": "Spurs Pada Kelipatan Frekuensi Referensi." },
    { "en": "Apa Itu Integer-N PLL?", "id": "Pembagi Frekuensi Menggunakan Bilangan Bulat." },
    { "en": "Apa Itu Fractional-N PLL?", "id": "Pembagi Frekuensi Menggunakan Bilangan Pecahan." },
    { "en": "Apa Keuntungan Fractional-N PLL?", "id": "Resolusi Frekuensi Lebih Baik." },
    { "en": "Apa Itu Modulator Sigma-Delta?", "id": "Digunakan Dalam Fractional-N PLL." },
    { "en": "Apa Itu Derau Kuantisasi?", "id": "Derau Akibat Pembulatan Digital." },
    { "en": "Apa Itu Frekuensi Gambar (Image Frequency)?", "id": "Respon Palsu Dalam Mixer." },
    { "en": "Apa Itu Osilasi Parasitik?", "id": "Osilasi Tak Diinginkan Pada Penguat." },
    { "en": "Apa Itu Umpan Balik?", "id": "Mengembalikan Sebagian Output Ke Input." },
    { "en": "Apa Itu Penguatan?", "id": "Kemampuan Untuk Memperkuat Sinyal." },
    { "en": "Apa Itu Atenuasi?", "id": "Pelemahan Sinyal." },
    { "en": "Apa Itu Pergeseran Fasa?", "id": "Perubahan Fasa Sinyal." },
    { "en": "Apa Itu Frekuensi?", "id": "Jumlah Siklus Per Detik." },
    { "en": "Apa Itu Periode?", "id": "Waktu Untuk Satu Siklus." },
    { "en": "Apa Itu Amplitudo?", "id": "Nilai Puncak Gelombang." },
    { "en": "Apa Itu Gelombang Sinus?", "id": "Bentuk Gelombang Periodik Dasar." },
    { "en": "Apa Itu Gelombang Persegi?", "id": "Gelombang Dengan Transisi Cepat." },
    { "en": "Apa Itu Gelombang Segitiga?", "id": "Gelombang Dengan Kemiringan Linear." },
    { "en": "Apa Itu Gelombang Gigi Gergaji?", "id": "Varian Dari Gelombang Segitiga." },
    { "en": "Apa Itu Resonansi?", "id": "Getaran Amplitudo Maksimum." },
    { "en": "Apa Itu Sirkuit Resonansi?", "id": "Sirkuit LC (Inductor-Capacitor)." },
    { "en": "Apa Itu Frekuensi Cutoff?", "id": "Frekuensi Batas Operasi." },
    { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Operasi." },
    { "en": "Apa Itu Spektrum?", "id": "Komponen Frekuensi Sinyal." },
    { "en": "Apa Itu Transformasi Fourier?", "id": "Analisis Spektrum Sinyal." },
    { "en": "Apa Itu Osilator Armstrong?", "id": "Osilator LC Menggunakan Kopling Trafo." },
    { "en": "Apa Itu Osilator Clapp?", "id": "Osilator Colpitts Dengan Kapasitor Tambahan." },
    { "en": "Apa Itu Osilator Pierce?", "id": "Osilator Kristal Dengan Inverter Logika." },
    { "en": "Apa Itu Osilator Relaksasi?", "id": "Contoh: Multivibrator Astable." },
    { "en": "Apa Osilator Yang Menggunakan Jaringan RC Notch?", "id": "Osilator Notch Twin-T." },
    { "en": "Apa Osilator Yang Menggunakan Resistansi Negatif?", "id": "Osilator Tunnel Diode." },
    { "en": "Apa Osilator Yang Menghasilkan Dua Sinyal Beda Fasa?", "id": "Osilator Kuadratur." },
    { "en": "Apa Osilator Terstabilkan Oven?", "id": "OCXO (Oven-Controlled Crystal Oscillator)." },
    { "en": "Apa Osilator Dengan Kompensasi Suhu?", "id": "TCXO (Temperature-Compensated Crystal Oscillator)." },
    { "en": "Apa Osilator Yang Frekuensinya Dikontrol Arus?", "id": "CCO (Current-Controlled Oscillator)." },
    { "en": "Apa Osilator Yang Berosilasi Karena Blocking?", "id": "Blocking Oscillator." },
    { "en": "Apa Osilator Yang Menghasilkan Pulsa Sangat Sempit?", "id": "Blocking Oscillator." },
    { "en": "Apa Osilator Berbasis Gerbang Logika Inverter?", "id": "Osilator Cincin (Ring Oscillator)." },
    { "en": "Apa Syarat Agar Osilator Cincin Berosilasi?", "id": "Jumlah Inverter Harus Ganjil." },
    { "en": "Apa Osilator Yang Frekuensinya Disapu?", "id": "Sweep Oscillator." },
    { "en": "Apa Itu Osilasi Parasitik?", "id": "Osilasi Liar Yang Tidak Diinginkan." },
    { "en": "Apa Itu Penarikan Frekuensi (Frequency Pulling)?", "id": "Perubahan Frekuensi Akibat Beban." },
    { "en": "Apa Itu Pendorongan Frekuensi (Frequency Pushing)?", "id": "Perubahan Frekuensi Akibat Catu Daya." },
    { "en": "Apa Sirkuit Untuk Menstabilkan Amplitudo?", "id": "AGC (Automatic Gain Control)." },
    { "en": "Apa Itu Start-up Time Osilator?", "id": "Waktu Mulai Hingga Osilasi Stabil." },
    { "en": "Apa Itu Kemurnian Spektral?", "id": "Seberapa Bersih Sinyal Dari Harmonik." },
    { "en": "Apa Itu Derau Jitter?", "id": "Variasi Waktu Sinyal." },
    { "en": "Apa Itu Akurasi Frekuensi?", "id": "Kedekatan Frekuensi Dengan Nilai Nominal." },
    { "en": "Apa Itu Penuaan (Aging)?", "id": "Pergeseran Frekuensi Jangka Panjang." },
    { "en": "Apa Itu Stabilitas Jangka Pendek?", "id": "Diukur Dengan Derau Fasa." },
    { "en": "Apa Itu Stabilitas Jangka Panjang?", "id": "Diukur Dengan Penuaan." },
    { "en": "Apa Itu Akumulator Fasa?", "id": "Bagian Dari DDS (Direct Digital Synthesis)." },
    { "en": "Apa Itu DAC (Digital-to-Analog Converter) Dalam DDS?", "id": "Mengubah Nilai Digital Menjadi Analog." },
    { "en": "Apa Itu Resolusi Penyetelan?", "id": "Langkah Perubahan Frekuensi Terkecil." },
    { "en": "Apa Keuntungan DDS (Direct Digital Synthesis)?", "id": "Pergantian Frekuensi Sangat Cepat." },
    { "en": "Apa Itu Sinyal Palsu (Spurs)?", "id": "Frekuensi Yang Tidak Diinginkan." },
    { "en": "Apa Penyebab Spurs Dalam DDS?", "id": "Kuantisasi DAC Dan Amplitudo." },
    { "en": "Apa Itu PLL (Phase-Locked Loop) Integer-N?", "id": "Pembagi Frekuensi Menggunakan Bilangan Bulat." },
    { "en": "Apa Itu PLL (Phase-Locked Loop) Fractional-N?", "id": "Pembagi Frekuensi Menggunakan Bilangan Pecahan." },
    { "en": "Apa Itu Detektor Fasa Frekuensi?", "id": "PFD (Phase-Frequency Detector)." },
    { "en": "Apa Itu Pompa Muatan (Charge Pump)?", "id": "Mengubah Sinyal Error Menjadi Arus." },
    { "en": "Apa Itu Overshoot Dalam Respon PLL?", "id": "Melebihi Frekuensi Target Sesaat." },
    { "en": "Apa Itu Waktu Penguncian (Lock Time)?", "id": "Waktu PLL Mencapai Frekuensi Target." },
    { "en": "Apa Itu Slipping Siklus (Cycle Slips)?", "id": "Kehilangan Kunci Fasa Sesaat." },
    { "en": "Apa Itu Bandwidth Loop?", "id": "Mempengaruhi Kecepatan Dan Kebersihan Sinyal." },
    { "en": "Bandwidth Loop Sempit Menghasilkan Apa?", "id": "Derau Fasa Rendah, Respon Lambat." },
    { "en": "Bandwidth Loop Lebar Menghasilkan Apa?", "id": "Respon Cepat, Derau Fasa Buruk." },
    { "en": "Apa Itu Osilasi Barkhausen?", "id": "Osilasi Yang Terjadi Secara Alami." },
    { "en": "Apa Itu Osilasi Terpaksa?", "id": "Osilasi Akibat Sinyal Pemicu Eksternal." },
    { "en": "Apa Itu Resonator?", "id": "Elemen Penentu Frekuensi." },
    { "en": "Contoh Resonator?", "id": "Kristal Kuarsa, Sirkuit LC." },
    { "en": "Apa Itu Mode Overtone?", "id": "Osilasi Kristal Pada Harmonik Ganjil." },
    { "en": "Apa Itu Osilator Transfer?", "id": "Mengukur Frekuensi Tinggi Dengan Frekuensi Rendah." },
    { "en": "Apa Itu Osilator Kalibrasi?", "id": "Osilator Referensi Untuk Kalibrasi." },
    { "en": "Apa Itu Osilator Dua Nada?", "id": "Menghasilkan Dua Frekuensi Untuk Pengujian." },
    { "en": "Apa Itu Osilator Tipe Jembatan?", "id": "Jembatan Wien, Jembatan Maxwell." },
    { "en": "Apa Itu Osilator Tipe Jaringan Tangga?", "id": "Osilator Geser Fasa." },
    { "en": "Apa Itu Osilator Optik Parametrik?", "id": "OPO (Optical Parametric Oscillator)." },
    { "en": "Apa Itu Osilator Kuantum Cascade Laser?", "id": "QCL (Quantum Cascade Laser)." },
    { "en": "Apa Itu Osilator Spin-Torque?", "id": "STO (Spin-Torque Oscillator)." },
    { "en": "Apa Itu Osilator Berbasis MEMS?", "id": "Menggantikan Osilator Kristal." },
    { "en": "Apa Itu Osilasi Parametrik?", "id": "Osilasi Karena Perubahan Parameter." },
    { "en": "Apa Itu Derau Termal?", "id": "Derau Dari Gerakan Acak Elektron." },
    { "en": "Apa Itu Derau Shot?", "id": "Derau Dari Aliran Diskret Elektron." },
    { "en": "Apa Itu Derau Flicker?", "id": "Derau Frekuensi Rendah (1/f)." },
    { "en": "Apa Itu Derau Putih?", "id": "Derau Dengan Spektrum Datar." },
    { "en": "Apa Itu Derau Merah Jambu?", "id": "Derau Dengan Energi Sama Per Oktaf." },
    { "en": "Apa Itu Q-factor Osilator?", "id": "Rasio Energi Tersimpan Per Energi Hilang." },
    { "en": "Q-factor Tinggi Berarti Apa?", "id": "Stabilitas Frekuensi Lebih Baik." },
    { "en": "Apa Itu Osilator Linier?", "id": "Menghasilkan Gelombang Sinus." },
    { "en": "Apa Itu Osilasi Terhambat?", "id": "Osilasi Yang Amplitudonya Mereda." },
    { "en": "Apa Itu Osilasi Terpaksa?", "id": "Osilasi Karena Gaya Eksternal." },
    { "en": "Apa Itu Frekuensi Alami?", "id": "Frekuensi Osilasi Tanpa Redaman." },
    { "en": "Apa Itu Frekuensi Teredam?", "id": "Frekuensi Osilasi Dengan Adanya Redaman." },
    { "en": "Apa Itu Redaman Kritis?", "id": "Kembali Ke Keseimbangan Paling Cepat." },
    { "en": "Apa Itu Redaman Berlebih?", "id": "Kembali Ke Keseimbangan Secara Lambat." },
    { "en": "Apa Itu Redaman Kurang?", "id": "Berisolasi Sebelum Kembali Ke Keseimbangan." },
    { "en": "Apa Itu Osilator Harmonik?", "id": "Osilator Dengan Gaya Pemulih Linear." },
    { "en": "Apa Itu Osilator Anharmonik?", "id": "Osilator Dengan Gaya Pemulih Non-linear." },
    { "en": "Apa Itu Mode Normal?", "id": "Pola Getaran Sistem Terkopel." },
    { "en": "Apa Itu Osilator Van der Pol?", "id": "Osilator Non-linear Dengan Redaman." },
    { "en": "Apa Itu Osilator Lorentz?", "id": "Sistem Dinamis Penghasil Chaos." },
    { "en": "Apa Itu Sinkronisasi?", "id": "Penyesuaian Ritme Osilator." },
    { "en": "Apa Itu Entrainment?", "id": "Osilator Mengunci Frekuensi Sinyal Eksternal." },
    { "en": "Apa Itu Osilasi Plasma?", "id": "Osilasi Elektron Dalam Plasma." },
    { "en": "Apa Itu Osilasi Neutrino?", "id": "Fenomena Neutrino Berubah Jenis." },
    { "en": "Apa Itu Osilasi Quasi-Periodik?", "id": "Osilasi Dengan Beberapa Frekuensi Dominan." },
    { "en": "Apa Itu Osilator Saraf?", "id": "Neuron Yang Menghasilkan Sinyal Berirama." },
    { "en": "Apa Itu Osilator Biologis?", "id": "Ritme Sirkadian, Detak Jantung." },
    { "en": "Apa Itu Model Kuramoto?", "id": "Model Matematis Untuk Osilator Terkopel." },
    { "en": "Apa Itu Koherensi Fasa?", "id": "Hubungan Fasa Yang Stabil." },
    { "en": "Apa Itu Inkoheren?", "id": "Hubungan Fasa Yang Acak." },
    { "en": "Apa Itu Osilator Gelombang Mundur?", "id": "BWO (Backward Wave Oscillator)." },
    { "en": "Apa Itu Osilator Terdistribusi?", "id": "Menggunakan Jalur Transmisi Sebagai Resonator." },
    { "en": "Apa Itu Osilator Kunci Injeksi?", "id": "ILO (Injection-Locked Oscillator)." },
    { "en": "Apa Itu Osilator Penstabil Rongga?", "id": "CSO (Cavity-Stabilized Oscillator)." },
    { "en": "Apa Itu Resonator Dielektrik?", "id": "DR (Dielectric Resonator)." },
    { "en": "Apa Itu Osilator Pengganda Frekuensi?", "id": "Menghasilkan Harmonik Dan Menyaringnya." },
    { "en": "Apa Itu Pembagi Frekuensi?", "id": "Sirkuit Yang Membagi Frekuensi Osilator." },
    { "en": "Apa Itu Prescaler?", "id": "Pembagi Frekuensi Kecepatan Tinggi." },
    { "en": "Apa Itu Osilator Magnetik?", "id": "Osilator Berbasis Sifat Magnetik." },
    { "en": "Apa Itu Osilator Spintronik?", "id": "Osilator Berbasis Spin Elektron." },
    { "en": "Apa Osilator Yang Menghasilkan Dua Sinyal Beda Fasa?", "id": "Osilator Kuadratur." },
    { "en": "Apa Osilator Terstabilkan Oven?", "id": "OCXO (Oven-Controlled Crystal Oscillator)." },
    { "en": "Apa Osilator Dengan Kompensasi Suhu?", "id": "TCXO (Temperature-Compensated Crystal Oscillator)." },
    { "en": "Apa Osilator Yang Frekuensinya Dikontrol Arus?", "id": "CCO (Current-Controlled Oscillator)." },
    { "en": "Apa Osilator Yang Berosilasi Karena Blocking?", "id": "Blocking Oscillator." },
    { "en": "Apa Osilator Yang Menghasilkan Pulsa Sangat Sempit?", "id": "Blocking Oscillator." },
    { "en": "Apa Osilator Berbasis Gerbang Logika Inverter?", "id": "Osilator Cincin (Ring Oscillator)." },
    { "en": "Apa Syarat Agar Osilator Cincin Berosilasi?", "id": "Jumlah Inverter Harus Ganjil." },
    { "en": "Apa Osilator Yang Frekuensinya Disapu?", "id": "Sweep Oscillator." },
    { "en": "Apa Itu Osilasi Parasitik?", "id": "Osilasi Liar Yang Tidak Diinginkan." },
    { "en": "Apa Itu Penarikan Frekuensi (Frequency Pulling)?", "id": "Perubahan Frekuensi Akibat Beban." },
    { "en": "Apa Itu Pendorongan Frekuensi (Frequency Pushing)?", "id": "Perubahan Frekuensi Akibat Catu Daya." },
    { "en": "Apa Sirkuit Untuk Menstabilkan Amplitudo?", "id": "AGC (Automatic Gain Control)." },
    { "en": "Apa Itu Start-up Time Osilator?", "id": "Waktu Mulai Hingga Osilasi Stabil." },
    { "en": "Apa Itu Kemurnian Spektral?", "id": "Seberapa Bersih Sinyal Dari Harmonik." },
    { "en": "Apa Itu Derau Jitter?", "id": "Variasi Waktu Sinyal." },
    { "en": "Apa Itu Akurasi Frekuensi?", "id": "Kedekatan Frekuensi Dengan Nilai Nominal." },
    { "en": "Apa Itu Penuaan (Aging)?", "id": "Pergeseran Frekuensi Jangka Panjang." },
    { "en": "Apa Itu Stabilitas Jangka Pendek?", "id": "Diukur Dengan Derau Fasa." },
    { "en": "Apa Itu Stabilitas Jangka Panjang?", "id": "Diukur Dengan Penuaan." },
    { "en": "Apa Itu Akumulator Fasa?", "id": "Bagian Dari DDS (Direct Digital Synthesis)." },
    { "en": "Apa Itu DAC (Digital-to-Analog Converter) Dalam DDS?", "id": "Mengubah Nilai Digital Menjadi Analog." },
    { "en": "Apa Itu Resolusi Penyetelan?", "id": "Langkah Perubahan Frekuensi Terkecil." },
    { "en": "Apa Keuntungan DDS (Direct Digital Synthesis)?", "id": "Pergantian Frekuensi Sangat Cepat." },
    { "en": "Apa Itu Sinyal Palsu (Spurs)?", "id": "Frekuensi Yang Tidak Diinginkan." },
    { "en": "Apa Penyebab Spurs Dalam DDS?", "id": "Kuantisasi DAC (Digital-to-Analog Converter) Dan Amplitudo." },
    { "en": "Apa Itu PLL (Phase-Locked Loop) Integer-N?", "id": "Pembagi Frekuensi Menggunakan Bilangan Bulat." },
    { "en": "Apa Itu PLL (Phase-Locked Loop) Fractional-N?", "id": "Pembagi Frekuensi Menggunakan Bilangan Pecahan." },
    { "en": "Apa Itu Detektor Fasa Frekuensi?", "id": "PFD (Phase-Frequency Detector)." },
    { "en": "Apa Itu Pompa Muatan (Charge Pump)?", "id": "Mengubah Sinyal Error Menjadi Arus." },
    { "en": "Apa Itu Overshoot Dalam Respon PLL?", "id": "Melebihi Frekuensi Target Sesaat." },
    { "en": "Apa Itu Waktu Penguncian (Lock Time)?", "id": "Waktu PLL (Phase-Locked Loop) Mencapai Frekuensi Target." },
    { "en": "Apa Itu Slipping Siklus (Cycle Slips)?", "id": "Kehilangan Kunci Fasa Sesaat." },
    { "en": "Apa Itu Bandwidth Loop?", "id": "Mempengaruhi Kecepatan Dan Kebersihan Sinyal." },
    { "en": "Bandwidth Loop Sempit Menghasilkan Apa?", "id": "Derau Fasa Rendah, Respon Lambat." },
    { "en": "Bandwidth Loop Lebar Menghasilkan Apa?", "id": "Respon Cepat, Derau Fasa Buruk." },
    { "en": "Apa Itu Osilasi Barkhausen?", "id": "Osilasi Yang Terjadi Secara Alami." },
    { "en": "Apa Itu Osilasi Terpaksa?", "id": "Osilasi Akibat Sinyal Pemicu Eksternal." },
    { "en": "Apa Itu Resonator?", "id": "Elemen Penentu Frekuensi." },
    { "en": "Contoh Resonator?", "id": "Kristal Kuarsa, Sirkuit LC." },
    { "en": "Apa Itu Mode Overtone?", "id": "Osilasi Kristal Pada Harmonik Ganjil." },
    { "en": "Apa Itu Osilator Transfer?", "id": "Mengukur Frekuensi Tinggi Dengan Frekuensi Rendah." },
    { "en": "Apa Itu Osilator Kalibrasi?", "id": "Osilator Referensi Untuk Kalibrasi." },
    { "en": "Apa Itu Osilator Dua Nada?", "id": "Menghasilkan Dua Frekuensi Untuk Pengujian." },
    { "en": "Apa Itu Osilator Tipe Jembatan?", "id": "Jembatan Wien, Jembatan Maxwell." },
    { "en": "Apa Itu Osilator Tipe Jaringan Tangga?", "id": "Osilator Geser Fasa." },
    { "en": "Apa Itu Osilator Optik Parametrik?", "id": "OPO (Optical Parametric Oscillator)." },
    { "en": "Apa Itu Osilator Kuantum Cascade Laser?", "id": "QCL (Quantum Cascade Laser)." },
    { "en": "Apa Itu Osilator Spin-Torque?", "id": "STO (Spin-Torque Oscillator)." },
    { "en": "Apa Itu Osilator Berbasis MEMS?", "id": "Menggantikan Osilator Kristal." },
    { "en": "Apa Itu Osilasi Parametrik?", "id": "Osilasi Karena Perubahan Parameter." },
    { "en": "Apa Itu Derau Termal?", "id": "Derau Dari Gerakan Acak Elektron." },
    { "en": "Apa Itu Derau Shot?", "id": "Derau Dari Aliran Diskret Elektron." },
    { "en": "Apa Itu Derau Flicker?", "id": "Derau Frekuensi Rendah (1/f)." },
    { "en": "Apa Itu Derau Putih?", "id": "Derau Dengan Spektrum Datar." },
    { "en": "Apa Itu Derau Merah Jambu?", "id": "Derau Dengan Energi Sama Per Oktaf." },
    { "en": "Apa Itu Q-factor Osilator?", "id": "Rasio Energi Tersimpan Per Energi Hilang." },
    { "en": "Q-factor Tinggi Berarti Apa?", "id": "Stabilitas Frekuensi Lebih Baik." },
    { "en": "Apa Itu Osilator Linier?", "id": "Menghasilkan Gelombang Sinus." },
    { "en": "Apa Itu Osilasi Terhambat?", "id": "Osilasi Yang Amplitudonya Mereda." },
    { "en": "Apa Itu Osilasi Terpaksa?", "id": "Osilasi Karena Gaya Eksternal." },
    { "en": "Apa Itu Frekuensi Alami?", "id": "Frekuensi Osilasi Tanpa Redaman." },
    { "en": "Apa Itu Frekuensi Teredam?", "id": "Frekuensi Osilasi Dengan Adanya Redaman." },
    { "en": "Apa Itu Redaman Kritis?", "id": "Kembali Ke Keseimbangan Paling Cepat." },
    { "en": "Apa Itu Redaman Berlebih?", "id": "Kembali Ke Keseimbangan Secara Lambat." },
    { "en": "Apa Itu Redaman Kurang?", "id": "Berisolasi Sebelum Kembali Ke Keseimbangan." },
    { "en": "Apa Itu Osilator Harmonik?", "id": "Osilator Dengan Gaya Pemulih Linear." },
    { "en": "Apa Itu Osilator Anharmonik?", "id": "Osilator Dengan Gaya Pemulih Non-linear." },
    { "en": "Apa Itu Mode Normal?", "id": "Pola Getaran Sistem Terkopel." },
    { "en": "Apa Itu Osilator Van der Pol?", "id": "Osilator Non-linear Dengan Redaman." },
    { "en": "Apa Itu Osilator Lorentz?", "id": "Sistem Dinamis Penghasil Chaos." },
    { "en": "Apa Itu Sinkronisasi?", "id": "Penyesuaian Ritme Osilator." },
    { "en": "Apa Itu Entrainment?", "id": "Osilator Mengunci Frekuensi Sinyal Eksternal." },
    { "en": "Apa Itu Osilasi Plasma?", "id": "Osilasi Elektron Dalam Plasma." },
    { "en": "Apa Itu Osilasi Neutrino?", "id": "Fenomena Neutrino Berubah Jenis." },
    { "en": "Apa Itu Osilasi Quasi-Periodik?", "id": "Osilasi Dengan Beberapa Frekuensi Dominan." },
    { "en": "Apa Itu Osilator Saraf?", "id": "Neuron Yang Menghasilkan Sinyal Berirama." },
    { "en": "Apa Itu Osilator Biologis?", "id": "Ritme Sirkadian, Detak Jantung." },
    { "en": "Apa Itu Model Kuramoto?", "id": "Model Matematis Untuk Osilator Terkopel." },
    { "en": "Apa Itu Koherensi Fasa?", "id": "Hubungan Fasa Yang Stabil." },
    { "en": "Apa Itu Inkoheren?", "id": "Hubungan Fasa Yang Acak." },
    { "en": "Apa Itu Osilator Gelombang Mundur?", "id": "BWO (Backward Wave Oscillator)." },
    { "en": "Apa Itu Osilator Terdistribusi?", "id": "Menggunakan Jalur Transmisi Sebagai Resonator." },
    { "en": "Apa Itu Osilator Kunci Injeksi?", "id": "ILO (Injection-Locked Oscillator)." },
    { "en": "Apa Itu Osilator Penstabil Rongga?", "id": "CSO (Cavity-Stabilized Oscillator)." },
    { "en": "Apa Itu Resonator Dielektrik?", "id": "DR (Dielectric Resonator)." },
    { "en": "Apa Itu Osilator Pengganda Frekuensi?", "id": "Menghasilkan Harmonik Dan Menyaringnya." },
    { "en": "Apa Itu Pembagi Frekuensi?", "id": "Sirkuit Yang Membagi Frekuensi Osilator." },
    { "en": "Apa Itu Prescaler?", "id": "Pembagi Frekuensi Kecepatan Tinggi." },
    { "en": "Apa Itu Osilator Magnetik?", "id": "Osilator Berbasis Sifat Magnetik." },
    { "en": "Apa Itu Osilator Spintronik?", "id": "Osilator Berbasis Spin Elektron." },
    { "en": "Apa Itu Penguat Umpan Balik?", "id": "Rangkaian Penguat Dalam Osilator." },
    { "en": "Apa Itu Jaringan Umpan Balik?", "id": "Jaringan Penentu Frekuensi (LC/RC)." },
    { "en": "Apa Penguatan Loop Terbuka?", "id": "A (Penguatan Penguat)." },
    { "en": "Apa Faktor Umpan Balik?", "id": "β (Beta)." },
    { "en": "Apa Penguatan Loop Tertutup?", "id": "Aβ (Penguatan Total Loop)." },
    { "en": "Berapa Nilai Aβ Agar Osilasi Dimulai?", "id": "Sedikit Lebih Besar Dari Satu." },
    { "en": "Berapa Nilai Aβ Agar Osilasi Stabil?", "id": "Tepat Sama Dengan Satu." },
    { "en": "Berapa Pergeseran Fasa Total Loop?", "id": "360° Atau 0°." },
    { "en": "Berapa Pergeseran Fasa Dari Penguat Inverting?", "id": "180 Derajat." },
    { "en": "Berapa Pergeseran Fasa Jaringan Feedback?", "id": "180 Derajat (Untuk Penguat Inverting)." },
    { "en": "Apa Itu Amplitudo Osilasi?", "id": "Tegangan Puncak Gelombang Yang Dihasilkan." },
    { "en": "Apa Yang Membatasi Amplitudo Osilasi?", "id": "Saturasi Penguat Atau Sirkuit AGC." },
    { "en": "Apa Itu Automatic Gain Control (AGC)?", "id": "Sirkuit Penstabil Amplitudo Otomatis." },
    { "en": "Apa Osilator Berbasis Jembatan?", "id": "Osilator Jembatan Wien." },
    { "en": "Apa Kondisi Keseimbangan Jembatan Wien?", "id": "Saat Jembatan Seimbang." },
    { "en": "Apa Itu Osilator Dengan Resistansi Negatif?", "id": "Osilator Yang Menggunakan Perangkat Resistansi Negatif." },
    { "en": "Contoh Perangkat Resistansi Negatif?", "id": "Tunnel Diode, Gunn Diode." },
    { "en": "Apa Itu Frekuensi Resonansi Sirkuit Tangki?", "id": "Frekuensi Osilasi Alami." },
    { "en": "Apa Itu Redaman (Damping)?", "id": "Hilangnya Energi Dalam Sirkuit Tangki." },
    { "en": "Bagaimana Osilator Mengatasi Redaman?", "id": "Penguat Memberi Energi Kembali." },
    { "en": "Apa Itu Efek Pemuatan (Loading)?", "id": "Beban Mempengaruhi Frekuensi Osilator." },
    { "en": "Bagaimana Mengurangi Efek Pemuatan?", "id": "Menggunakan Sirkuit Penyangga (Buffer)." },
    { "en": "Apa Itu Penyangga (Buffer)?", "id": "Penguat Dengan Gain Satu." },
    { "en": "Apa Itu Osilator Butler?", "id": "Osilator Kristal Dengan Pengikut Emitor." },
    { "en": "Apa Itu Osilator Colpitts FET?", "id": "Osilator Colpitts Menggunakan Transistor FET." },
    { "en": "Apa Itu Osilator Hartley Seri?", "id": "Variasi Dari Osilator Hartley." },
    { "en": "Apa Itu Osilator Hartley Paralel?", "id": "Bentuk Umum Osilator Hartley." },
    { "en": "Apa Itu Osilator Meissner?", "id": "Nama Lain Untuk Osilator Armstrong." },
    { "en": "Apa Itu Osilator Blok?", "id": "Blocking Oscillator." },
    { "en": "Apa Fungsi Blocking Oscillator?", "id": "Menghasilkan Pulsa Sempit." },
    { "en": "Apa Itu Osilator RC Aktif?", "id": "Osilator RC Menggunakan Op-Amp." },
    { "en": "Apa Itu Osilator RC Pasif?", "id": "Tidak Ada, Osilator RC Butuh Penguat." },
    { "en": "Apa Itu Resonator Keramik?", "id": "Alternatif Murah Untuk Kristal Kuarsa." },
    { "en": "Apa Itu Resonator SAW (Surface Acoustic Wave)?", "id": "Resonator Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu Osilator Dielektrik Resonator?", "id": "DRO (Dielectric Resonator Oscillator)." },
    { "en": "Di Mana DRO Digunakan?", "id": "Aplikasi Microwave Frekuensi Stabil." },
    { "en": "Apa Itu Osilator YIG (Yttrium Iron Garnet)?", "id": "Osilator Microwave Yang Dapat Disetel." },
    { "en": "Bagaimana Cara Menyetel Osilator YIG?", "id": "Dengan Mengubah Medan Magnet." },
    { "en": "Apa Itu Osilator Frekuensi Sapu (Sweep)?", "id": "Osilator Yang Frekuensinya Berubah Linear." },
    { "en": "Apa Itu Generator Wobble?", "id": "Nama Lama Untuk Sweep Oscillator." },
    { "en": "Apa Itu Osilator Ketukan (Beat Oscillator)?", "id": "Mencampur Dua Frekuensi Untuk Menghasilkan Selisih." },
    { "en": "Apa Itu Beat Frequency?", "id": "Frekuensi Selisih Hasil Pencampuran." },
    { "en": "Apa Itu BFO (Beat Frequency Oscillator)?", "id": "Digunakan Untuk Demodulasi CW/SSB." },
    { "en": "Apa Itu Clock?", "id": "Osilator Untuk Sirkuit Digital." },
    { "en": "Apa Itu Siklus Clock?", "id": "Satu Periode Lengkap Sinyal Clock." },
    { "en": "Apa Itu Tepi Naik Clock?", "id": "Transisi Dari Logika 0 Ke 1." },
    { "en": "Apa Itu Tepi Turun Clock?", "id": "Transisi Dari Logika 1 Ke 0." },
    { "en": "Apa Itu Osilator Terkendali Gerbang?", "id": "GCO (Gate-Controlled Oscillator)." },
    { "en": "Apa Itu Osilator Terkendali Arus?", "id": "CCO (Current-Controlled Oscillator)." },
    { "en": "Apa Itu Osilator Koaksial?", "id": "Osilator Menggunakan Jalur Koaksial." },
    { "en": "Apa Itu Osilator Garis-Lecher?", "id": "Osilator Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu Osilator Dynatron?", "id": "Osilator Resistansi Negatif Menggunakan Tabung Vakum." },
    { "en": "Apa Itu Osilator Pearson-Anson?", "id": "Osilator Relaksasi Menggunakan Lampu Neon." },
    { "en": "Apa Itu Generator Fungsi?", "id": "Menghasilkan Gelombang Sinus, Persegi, Segitiga." },
    { "en": "Apa Inti Dari Generator Fungsi?", "id": "Osilator Gelombang Persegi Dan Segitiga." },
    { "en": "Bagaimana Gelombang Sinus Dibuat Di Generator Fungsi?", "id": "Membentuk Gelombang Segitiga." },
    { "en": "Apa Itu Osilator Fasa Terkunci Optik?", "id": "OPLL (Optical Phase-Locked Loop)." },
    { "en": "Apa Itu Osilator Kuantum?", "id": "Osilator Berbasis Prinsip Mekanika Kuantum." },
    { "en": "Apa Itu Maser?", "id": "Osilator Atau Penguat Microwave." },
    { "en": "Apa Itu Laser?", "id": "Osilator Atau Penguat Cahaya." },
    { "en": "Apa Itu Sinyal Referensi Frekuensi?", "id": "Osilator Sangat Stabil Sebagai Standar." },
    { "en": "Contoh Referensi Frekuensi?", "id": "Jam Atom." },
    { "en": "Apa Itu Allan Deviation/Variance?", "id": "Ukuran Stabilitas Frekuensi Osilator." },
    { "en": "Apa Itu Spurs (Spurious Signals)?", "id": "Frekuensi Palsu Yang Tidak Diinginkan." },
    { "en": "Apa Itu Resolusi Frekuensi?", "id": "Langkah Perubahan Frekuensi Terkecil." },
    { "en": "Apa Itu Waktu Settling?", "id": "Waktu Frekuensi Stabil Setelah Perubahan." },
    { "en": "Apa Itu Bandwidth Loop?", "id": "Parameter Penting Dalam Desain PLL." },
    { "en": "Apa Itu Faktor Redaman (Damping Factor) PLL?", "id": "Menentukan Respon Transien PLL." },
    { "en": "Apa Itu Referensi Spurious?", "id": "Spurs Pada Kelipatan Frekuensi Referensi." },
    { "en": "Apa Itu Integer-N PLL?", "id": "Pembagi Frekuensi Menggunakan Bilangan Bulat." },
    { "en": "Apa Itu Fractional-N PLL?", "id": "Pembagi Frekuensi Menggunakan Bilangan Pecahan." },
    { "en": "Apa Keuntungan Fractional-N PLL?", "id": "Resolusi Frekuensi Lebih Baik." },
    { "en": "Apa Itu Modulator Sigma-Delta?", "id": "Digunakan Dalam Fractional-N PLL." },
    { "en": "Apa Itu Derau Kuantisasi?", "id": "Derau Akibat Pembulatan Digital." },
    { "en": "Apa Itu Frekuensi Gambar (Image Frequency)?", "id": "Respon Palsu Dalam Mixer." },
    { "en": "Apa Itu Osilasi Parasitik?", "id": "Osilasi Tak Diinginkan Pada Penguat." },
    { "en": "Apa Itu Umpan Balik?", "id": "Mengembalikan Sebagian Output Ke Input." },
    { "en": "Apa Itu Penguatan?", "id": "Kemampuan Untuk Memperkuat Sinyal." },
    { "en": "Apa Itu Atenuasi?", "id": "Pelemahan Sinyal." },
    { "en": "Apa Itu Pergeseran Fasa?", "id": "Perubahan Fasa Sinyal." },
    { "en": "Apa Itu Frekuensi?", "id": "Jumlah Siklus Per Detik." },
    { "en": "Apa Itu Periode?", "id": "Waktu Untuk Satu Siklus." },
    { "en": "Apa Itu Amplitudo?", "id": "Nilai Puncak Gelombang." },
    { "en": "Apa Itu Gelombang Sinus?", "id": "Bentuk Gelombang Periodik Dasar." },
    { "en": "Apa Itu Gelombang Persegi?", "id": "Gelombang Dengan Transisi Cepat." },
    { "en": "Apa Itu Gelombang Segitiga?", "id": "Gelombang Dengan Kemiringan Linear." },
    { "en": "Apa Itu Gelombang Gigi Gergaji?", "id": "Varian Dari Gelombang Segitiga." },
    { "en": "Apa Itu Resonansi?", "id": "Getaran Amplitudo Maksimum." },
    { "en": "Apa Itu Sirkuit Resonansi?", "id": "Sirkuit LC (Inductor-Capacitor)." },
    { "en": "Apa Itu Frekuensi Cutoff?", "id": "Frekuensi Batas Operasi." },
    { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Operasi." },
    { "en": "Apa Itu Spektrum?", "id": "Komponen Frekuensi Sinyal." },
    { "en": "Apa Itu Transformasi Fourier?", "id": "Analisis Spektrum Sinyal." },
    { "en": "Apa Itu Osilator Armstrong?", "id": "Osilator LC Menggunakan Kopling Trafo." },
    { "en": "Apa Itu Osilator Clapp?", "id": "Osilator Colpitts Dengan Kapasitor Tambahan." },
    { "en": "Apa Itu Osilator Pierce?", "id": "Osilator Kristal Dengan Inverter Logika." },
    { "en": "Apa Itu Osilator Relaksasi?", "id": "Contoh: Multivibrator Astable." },
    { "en": "Apa Osilator Untuk Menghasilkan Sinyal RF Lokal?", "id": "LO (Local Oscillator)." },
    { "en": "Apa Fungsi LO (Local Oscillator) Dalam Superheterodyne?", "id": "Dicampur Dengan Sinyal RF Masuk." },
    { "en": "Apa Osilator Yang Menghasilkan Clock Sistem?", "id": "Clock Generator." },
    { "en": "Apa Osilator Referensi Dalam Alat Ukur?", "id": "Timebase Oscillator." },
    { "en": "Apa Osilator Yang Dideteksi Untuk Demodulasi CW?", "id": "BFO (Beat Frequency Oscillator)." },
    { "en": "Apa Dioda Yang Digunakan Untuk Tuning VCO?", "id": "Dioda Varaktor (Varactor Diode)." },
    { "en": "Apa Osilator Yang Menggunakan Resonator Keramik?", "id": "Ceramic Resonator Oscillator." },
    { "en": "Apa Osilator Berbasis Rongga Logam?", "id": "Cavity Oscillator." },
    { "en": "Apa Ukuran Stabilitas Frekuensi Dalam Domain Waktu?", "id": "Allan Deviation." },
    { "en": "Apa Harmonik Di Bawah Frekuensi Fundamental?", "id": "Sub-harmonik." },
    { "en": "Apa Ukuran Total Derau Dalam Lebar Pita Tertentu?", "id": "Integrated Phase Noise." },
    { "en": "Apa Osilator Berbasis Umpan Balik Optik?", "id": "OEO (Opto-Electronic Oscillator)." },
    { "en": "Apa Osilator Microwave Yang Disetel Medan Magnet?", "id": "YIG (Yttrium Iron Garnet) Oscillator." },
    { "en": "Apa Osilator Frekuensi Tinggi Berbasis Dielektrik?", "id": "DRO (Dielectric Resonator Oscillator)." },
    { "en": "Apa Osilator Berbasis Tabung Vakum?", "id": "BWO (Backward-Wave Oscillator)." },
    { "en": "Apa Osilator Dengan Konfigurasi Master-Slave?", "id": "Osilator Kunci Injeksi." },
    { "en": "Bagaimana Mengontrol Siklus Kerja Multivibrator Astable?", "id": "Mengubah Rasio Resistor Pengisi." },
    { "en": "Bagaimana Mengontrol Lebar Pulsa Monostable?", "id": "Mengubah Nilai RC (Resistor-Capacitor)." },
    { "en": "Apa Osilator Relaksasi Berbasis Komparator?", "id": "Schmitt Trigger Oscillator." },
    { "en": "Apa Yang Menentukan Ukuran Langkah Frekuensi DDS?", "id": "Panjang Kata Akumulator Fasa." },
    { "en": "Bagaimana Cara Mengurangi Ukuran ROM Dalam DDS?", "id": "Kompresi ROM Atau Algoritma." },
    { "en": "Apa Ketidaksempurnaan Pompa Muatan (Charge Pump) PLL?", "id": "Arus Bocor Dan Mismatch." },
    { "en": "Apa Sirkuit Yang Mendeteksi Kondisi Terkunci PLL?", "id": "Detektor Kunci (Lock Detector)." },
    { "en": "Apa Osilator Variasi Colpitts Stabilitas Tinggi?", "id": "Osilator Vackar." },
    { "en": "Apa Osilator Yang Mirip Armstrong Tapi Berbasis FET?", "id": "Osilator Meissner." },
    { "en": "Apa Osilator Yang Menggunakan Resonator Mekanis Mikro?", "id": "Osilator MEMS (Micro-Electro-Mechanical Systems)." },
    { "en": "Apa Sinyal Yang Dihasilkan Osilator Chaos?", "id": "Sinyal Aperiodik Deterministik." },
    { "en": "Apa Osilator Berbasis Reaksi Kimia?", "id": "Osilator Belousov-Zhabotinsky." },
    { "en": "Apa Itu Osilasi Terkopel?", "id": "Interaksi Antara Beberapa Osilator." },
    { "en": "Apa Kondisi Awal Osilasi?", "id": "Derau Termal Yang Diperkuat." },
    { "en": "Apa Yang Terjadi Jika Penguatan Loop Terlalu Besar?", "id": "Amplitudo Terpotong (Clipping)." },
    { "en": "Apa Yang Terjadi Jika Penguatan Loop Terlalu Kecil?", "id": "Osilasi Akan Mereda Dan Berhenti." },
    { "en": "Apa Osilator Yang Menghasilkan Gelombang Sinus?", "id": "Osilator Harmonik Atau Linier." },
    { "en": "Apa Osilator Yang Menghasilkan Gelombang Persegi/Segitiga?", "id": "Osilator Relaksasi Atau Non-Linier." },
    { "en": "Apa Itu Isolator Osilator?", "id": "Mencegah Penarikan Frekuensi (Pulling)." },
    { "en": "Apa Osilator Yang Menghasilkan Frekuensi Sangat Akurat?", "id": "Jam Atom." },
    { "en": "Elemen Apa Yang Digunakan Di Jam Atom?", "id": "Atom Cesium Atau Rubidium." },
    { "en": "Apa Osilator Yang Digunakan Sebagai Sumber Pembangkit Panas?", "id": "Magnetron Di Oven Microwave." },
    { "en": "Apa Osilator Yang Digunakan Di Jam Tangan Kuarsa?", "id": "Osilator Kristal 32.768 kHz." },
    { "en": "Kenapa Frekuensi 32.768 kHz Dipilih?", "id": "Mudah Dibagi Menjadi 1 Hz." },
    { "en": "Apa Osilator Di Dalam CPU (Central Processing Unit) Komputer?", "id": "PLL (Phase-Locked Loop) Yang Dikunci Kristal." },
    { "en": "Apa Osilator Internal Mikrokontroler?", "id": "Osilator RC (Resistor-Capacitor) Internal." },
    { "en": "Apa Kelemahan Osilator RC Internal?", "id": "Kurang Akurat Dibanding Kristal." },
    { "en": "Apa Itu 'Pullability' Kristal?", "id": "Seberapa Jauh Frekuensi Dapat Ditarik." },
    { "en": "Apa Itu 'Trim Sensitivity' Kristal?", "id": "Perubahan Frekuensi Per Perubahan Kapasitansi." },
    { "en": "Apa Itu Drive Level Kristal?", "id": "Daya Maksimum Yang Diberikan Ke Kristal." },
    { "en": "Apa Itu ESR (Equivalent Series Resistance) Kristal?", "id": "Resistansi Seri Ekuivalen." },
    { "en": "Apa Osilasi Terganjal (Squegging)?", "id": "Osilasi Yang Hidup-Mati Secara Periodik." },
    { "en": "Apa Itu Osilator Jembatan Meacham?", "id": "Osilator Kristal Stabilitas Sangat Tinggi." },
    { "en": "Apa Itu Osilator Robinson?", "id": "Osilator Logika Sederhana." },
    { "en": "Apa Itu Osilator Gerbang?", "id": "Osilator Pierce Menggunakan Gerbang Logika." },
    { "en": "Apa Itu Pematrian Ulang (Reflow) Kristal?", "id": "Dapat Merusak Kristal Jika Terlalu Panas." },
    { "en": "Apa Itu Osilasi Spurious?", "id": "Osilasi Pada Frekuensi Yang Tidak Diinginkan." },
    { "en": "Apa Itu Mode Getaran Kristal?", "id": "Fundamental, Overtone Ketiga, Kelima." },
    { "en": "Bagaimana Memilih Mode Overtone?", "id": "Dengan Menambahkan Sirkuit Tangki LC." },
    { "en": "Apa Itu Jitter Periode?", "id": "Variasi Antara Periode Clock." },
    { "en": "Apa Itu Jitter Siklus-ke-Siklus?", "id": "Variasi Periode Antara Dua Siklus Berurutan." },
    { "en": "Apa Itu Jitter Akumulasi?", "id": "Total Jitter Selama Beberapa Siklus." },
    { "en": "Apa Itu Derau Fasa SSB (Single-Sideband)?", "id": "Ukuran Derau Fasa Dalam dBc/Hz." },
    { "en": "Apa Itu dBc/Hz?", "id": "Desibel Relatif Terhadap Pembawa Per Hertz." },
    { "en": "Apa Itu Lantai Derau Fasa (Phase Noise Floor)?", "id": "Tingkat Derau Fasa Jauh Dari Pembawa." },
    { "en": "Apa Osilator Dengan Derau Fasa Terbaik?", "id": "Osilator Kristal SAPPHIRE." },
    { "en": "Apa Itu Osilator Parametrik?", "id": "Osilasi Dihasilkan Dengan Memompa Parameter." },
    { "en": "Apa Itu Osilator Kunci Mode (Mode-locked)?", "id": "Menghasilkan Pulsa Cahaya Sangat Pendek." },
    { "en": "Apa Itu Osilator Laser Serat?", "id": "Laser Yang Resonatornya Adalah Serat Optik." },
    { "en": "Apa Itu Osilator Laser Dioda?", "id": "Laser Berbasis Semikonduktor." },
    { "en": "Apa Itu Osilator Relaksasi UJT (Unijunction Transistor)?", "id": "Osilator Sederhana Menggunakan UJT." },
    { "en": "Apa Itu Osilator Flyback?", "id": "Osilator Konverter Flyback." },
    { "en": "Apa Itu Osilator Royer?", "id": "Osilator Push-pull Resonansi." },
    { "en": "Apa Itu Osilator Tipe Jembatan?", "id": "Menggunakan Jembatan (Bridge) Sebagai Jaringan Feedback." },
    { "en": "Apa Itu Osilator Bubba?", "id": "Osilator Geser Fasa Kuadratur." },
    { "en": "Apa Itu Osilator Tiga Titik?", "id": "Kategori Umum Untuk Colpitts Dan Hartley." },
    { "en": "Apa Osilasi Barkhausen-Kurz?", "id": "Osilasi Elektron Dalam Tabung Vakum." },
    { "en": "Apa Itu Osilator Transit-Time?", "id": "Osilasi Berbasis Waktu Transit Elektron." },
    { "en": "Contoh Osilator Transit-Time?", "id": "Klystron, Magnetron." },
    { "en": "Apa Itu Osilator Harmonik?", "id": "Menghasilkan Harmonik Yang Diinginkan." },
    { "en": "Apa Itu Subharmonik?", "id": "Frekuensi Pecahan Dari Frekuensi Pemicu." },
    { "en": "Apa Itu Pengunci Subharmonik?", "id": "Osilator Mengunci Ke Subharmonik." },
    { "en": "Apa Itu Osilator Optik?", "id": "Nama Lain Untuk Laser." },
    { "en": "Apa Itu Osilator Akustik?", "id": "Contoh: Peluit, Alat Musik." },
    { "en": "Apa Itu Osilator Mekanis?", "id": "Contoh: Pendulum, Roda Keseimbangan Jam." },
    { "en": "Apa Itu Osilasi Terkopel?", "id": "Dua Pendulum Terhubung Dengan Pegas." },
    { "en": "Apa Itu Sintesis Frekuensi?", "id": "Membangun Frekuensi Dari Referensi." },
    { "en": "Apa Itu Prescaler Dua Modulus?", "id": "Pembagi Dengan Dua Rasio Pembagian." },
    { "en": "Apa Itu Akumulator?", "id": "Bagian Dari Sirkuit Digital." },
    { "en": "Apa Itu Register Geser?", "id": "Menyimpan Dan Menggeser Bit." },
    { "en": "Apa Itu Memori Hanya Baca?", "id": "ROM (Read-Only Memory)." },
    { "en": "Apa Itu Pengubah Digital Ke Analog?", "id": "DAC (Digital-to-Analog Converter)." },
    { "en": "Apa Itu Komparator?", "id": "Membandingkan Dua Tegangan." },
    { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator Dengan Histeresis." },
    { "en": "Apa Itu Penguat Operasional?", "id": "Op-Amp (Operational Amplifier)." },
    { "en": "Apa Itu Transistor Persimpangan Bipolar?", "id": "BJT (Bipolar Junction Transistor)." },
    { "en": "Apa Itu Transistor Efek Medan?", "id": "FET (Field-Effect Transistor)." },
    { "en": "Apa Itu Resonator LC Paralel?", "id": "Sering Disebut Sirkuit Tangki." },
    { "en": "Apa Itu Osilator Jembatan Ganda-T?", "id": "Osilator Notch Twin-T." },
    { "en": "Apa Osilator Berbasis Schmitt Trigger?", "id": "Schmitt Trigger Relaxation Oscillator." },
    { "en": "Apa Fungsi Timer 555 Dalam Mode Monostable?", "id": "Menghasilkan Pulsa Tunggal (One-Shot)." },
    { "en": "Apa Fungsi Timer 555 Dalam Mode Bistable?", "id": "Berfungsi Sebagai Flip-Flop RS." },
    { "en": "Apa Osilator Multivibrator Berbasis Transistor?", "id": "Emitter-Coupled Astable Multivibrator." },
    { "en": "Apa Detektor Fasa Berbasis Gerbang Logika?", "id": "XOR Phase Detector." },
    { "en": "Apa Detektor Fasa Berbasis Flip-Flop?", "id": "JK Flip-Flop Phase Detector." },
    { "en": "Apa Filter Loop PLL Yang Menggunakan Op-Amp?", "id": "Filter Loop Aktif." },
    { "en": "Apa Filter Loop PLL Yang Hanya RC?", "id": "Filter Loop Pasif." },
    { "en": "Apa Pembagi Frekuensi Dengan Rasio Ganda?", "id": "Dual-Modulus Prescaler." },
    { "en": "Apa Masalah Osilator Gagal Berosilasi?", "id": "Kegagalan Start-up." },
    { "en": "Apa Fenomena Osilator Melompat Antar Mode?", "id": "Mode Hopping." },
    { "en": "Apa Osilator Geser Fasa Menggunakan FET?", "id": "FET Phase-Shift Oscillator." },
    { "en": "Apa Osilator Colpitts Dengan Basis Ditanahkan?", "id": "Grounded-Base Colpitts Oscillator." },
    { "en": "Apa Osilator Yang Menghasilkan Pembawa TV Analog?", "id": "Carrier Oscillator." },
    { "en": "Apa Osilator Yang Menghasilkan Sinyal Warna TV?", "id": "Subcarrier Oscillator." },
    { "en": "Bagaimana Cara Mengubah Segitiga Menjadi Sinus?", "id": "Sirkuit Pembentuk Dioda (Diode Shaper)." },
    { "en": "Bagaimana Menghasilkan Gelombang Ramp Linear?", "id": "Kapasitor Diisi Sumber Arus Konstan." },
    { "en": "Apa Osilator Tabung Vakum Resistansi Negatif?", "id": "Osilator Dynatron." },
    { "en": "Apa Osilator Kristal Dua-Tahap?", "id": "Osilator Franklin." },
    { "en": "Bagaimana Cara Mengukur Derau Fasa?", "id": "Menggunakan Spectrum Analyzer." },
    { "en": "Bagaimana Cara Mengukur Jitter?", "id": "Menggunakan Osiloskop (Histogram)." },
    { "en": "Apa Tipe Penghitungan Frekuensi Paling Dasar?", "id": "Direct Counting." },
    { "en": "Apa Tipe Penghitungan Untuk Frekuensi Rendah?", "id": "Reciprocal Counting." },
    { "en": "Apa Model Teoritis Untuk Derau Fasa?", "id": "Model Leeson." },
    { "en": "Bagaimana Derau 1/f Mempengaruhi Derau Fasa?", "id": "Up-conversion Ke Dekat Frekuensi Pembawa." },
    { "en": "Apa Akibat Non-Linearitas Dioda Varaktor?", "id": "Perubahan Sensitivitas Penyetelan." },
    { "en": "Apa Akibat Drive Level Kristal Terlalu Tinggi?", "id": "Pergeseran Frekuensi Dan Kerusakan." },
    { "en": "Apa Itu Osilator Goyah (Jittery)?", "id": "Osilator Dengan Jitter Tinggi." },
    { "en": "Apa Itu Osilator Koheren?", "id": "Mempertahankan Fasa Stabil Seiring Waktu." },
    { "en": "Apa Itu Toleransi Frekuensi?", "id": "Penyimpangan Maksimum Dari Nominal." },
    { "en": "Apa Itu Stabilitas Vs Suhu?", "id": "Seberapa Besar Frekuensi Berubah Dengan Suhu." },
    { "en": "Apa Itu Koefisien Suhu Kristal?", "id": "Diukur Dalam PPM (Parts Per Million) Per Derajat Celcius." },
    { "en": "Apa Itu Titik Balik Kurva Suhu Kristal?", "id": "Titik Dengan Koefisien Suhu Nol." },
    { "en": "Apa Itu Resonator Pemandu Gelombang Akustik?", "id": "BAW (Bulk Acoustic Wave) Resonator." },
    { "en": "Apa Itu Osilator Diferensial?", "id": "Menghasilkan Output Diferensial (Ganda)." },
    { "en": "Apa Keuntungan Osilator Diferensial?", "id": "Penolakan Derau Common-mode Lebih Baik." },
    { "en": "Apa Itu Osilator Injeksi Regeneratif?", "id": "Osilator Yang Mengunci Ke Harmonik." },
    { "en": "Apa Itu Osilator Push-Pull?", "id": "Dua Transistor Bekerja Berlawanan Fasa." },
    { "en": "Apa Keuntungan Osilator Push-Pull?", "id": "Penekanan Harmonik Genap." },
    { "en": "Apa Itu Osilator Optik Parametrik?", "id": "Mengubah Satu Foton Menjadi Dua." },
    { "en": "Apa Itu Osilator Abadi?", "id": "Osilator Ideal Tanpa Kehilangan Energi." },
    { "en": "Apa Itu Kualitas Resonator?", "id": "Faktor Q (Quality Factor)." },
    { "en": "Apa Itu Faktor Muat (Loaded Q)?", "id": "Faktor Q Setelah Terhubung Ke Sirkuit." },
    { "en": "Apa Itu Faktor Tanpa Muat (Unloaded Q)?", "id": "Faktor Q Intrinsik Resonator." },
    { "en": "Apa Itu Bandwidth Osilasi?", "id": "Rentang Frekuensi Osilasi." },
    { "en": "Apa Itu Penarikan (Pulling Figure)?", "id": "Ukuran Kuantitatif Penarikan Frekuensi." },
    { "en": "Apa Itu Isolasi Balik?", "id": "Kemampuan Mencegah Sinyal Balik." },
    { "en": "Apa Itu Osilasi Terkopel Lemah?", "id": "Interaksi Minimal Antar Osilator." },
    { "en": "Apa Itu Osilasi Terkopel Kuat?", "id": "Interaksi Signifikan Antar Osilator." },
    { "en": "Apa Itu Kematian Amplitudo (Amplitude Death)?", "id": "Osilasi Berhenti Karena Kopling." },
    { "en": "Apa Itu Penarikan Fasa (Phase Pulling)?", "id": "Pergeseran Fasa Osilator Akibat Kopling." },
    { "en": "Apa Itu Osilator Sebagai Detektor?", "id": "Contoh: Regenerative Receiver." },
    { "en": "Apa Itu Efek Mikrofonik?", "id": "Perubahan Frekuensi Akibat Getaran Mekanis." },
    { "en": "Apa Itu Osilator Dengan Kebisingan Rendah?", "id": "Dirancang Khusus Untuk Derau Fasa Rendah." },
    { "en": "Apa Itu Osilator Daya Tinggi?", "id": "Dirancang Untuk Menghasilkan Daya Output Besar." },
    { "en": "Apa Itu Efisiensi Osilator?", "id": "Rasio Daya Output RF Per Daya DC." },
    { "en": "Apa Itu Penekanan Harmonik?", "id": "Ukuran Seberapa Rendah Level Harmonik." },
    { "en": "Apa Itu Filter Output Osilator?", "id": "Menghilangkan Harmonik Yang Tidak Diinginkan." },
    { "en": "Apa Itu Osilator Dengan Frekuensi Dapat Disetel?", "id": "Tunable Oscillator." },
    { "en": "Apa Itu Rentang Penyetelan (Tuning Range)?", "id": "Rentang Frekuensi Yang Dapat Dicapai." },
    { "en": "Apa Itu Linearitas Penyetelan?", "id": "Seberapa Linear Hubungan Tegangan-Frekuensi." },
    { "en": "Apa Itu Kecepatan Penyetelan?", "id": "Seberapa Cepat Frekuensi Dapat Berubah." },
    { "en": "Apa Itu Jitter Deterministik?", "id": "Jitter Dengan Pola Yang Dapat Diprediksi." },
    { "en": "Apa Itu Jitter Acak (Random Jitter)?", "id": "Jitter Tanpa Pola (Gaussian)." },
    { "en": "Apa Itu Jitter Total?", "id": "Kombinasi Jitter Deterministik Dan Acak." },
    { "en": "Apa Itu Distribusi Jitter?", "id": "Histogram Dari Variasi Waktu." },
    { "en": "Apa Itu Toleransi Jitter?", "id": "Jumlah Jitter Maksimum Yang Diterima Sistem." },
    { "en": "Apa Itu Transfer Jitter?", "id": "Bagaimana Jitter Merambat Melalui Sistem." },
    { "en": "Apa Itu Pembersihan Jitter (Jitter Cleaning)?", "id": "Mengurangi Jitter Menggunakan PLL." },
    { "en": "Apa Itu Attenuator Jitter?", "id": "Sirkuit PLL Untuk Mengurangi Jitter." },
    { "en": "Apa Itu Osilator Relaksasi Thyristor?", "id": "Osilator Menggunakan SCR Atau UJT." },
    { "en": "Apa Itu Osilator Royer?", "id": "Osilator Push-pull Resonansi." },
    { "en": "Di Mana Osilator Royer Digunakan?", "id": "Inverter Untuk Lampu CCFL." },
    { "en": "Apa Itu Osilator Pierce?", "id": "Osilator Kristal Paling Umum." },
    { "en": "Apa Itu Osilator Colpitts?", "id": "Osilator LC Dengan Pembagi Kapasitif." },
    { "en": "Apa Itu Osilator Hartley?", "id": "Osilator LC Dengan Pembagi Induktif." },
    { "en": "Apa Itu Osilator Jembatan Wien?", "id": "Osilator RC Untuk Gelombang Sinus." },
    { "en": "Apa Itu Osilator Geser Fasa?", "id": "Osilator RC Dengan Jaringan Geser Fasa." },
    { "en": "Apa Itu Multivibrator Astable?", "id": "Osilator Relaksasi Gelombang Persegi." },
    { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Frekuensi Tergantung Pada Tegangan Input." },
    { "en": "Apa Itu DDS (Direct Digital Synthesis)?", "id": "Menghasilkan Gelombang Secara Digital." },
    { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Umpan Balik Untuk Sinkronisasi Frekuensi." },
    { "en": "Apa Itu OCXO (Oven-Controlled Crystal Oscillator)?", "id": "Kristal Dipanaskan Untuk Stabilitas Maksimal." },
    { "en": "Apa Itu TCXO (Temperature-Compensated Crystal Oscillator)?", "id": "Menggunakan Sirkuit Untuk Kompensasi Suhu." },
    { "en": "Apa Itu VCXO (Voltage-Controlled Crystal Oscillator)?", "id": "Frekuensi Kristal Dapat Sedikit Diubah." },
    { "en": "Apa Itu Resonator?", "id": "Komponen Yang Bergetar Pada Frekuensi Tertentu." },
    { "en": "Apa Itu Faktor Kualitas?", "id": "Q Factor." },
    { "en": "Apa Itu Harmonik?", "id": "Kelipatan Ganzzahl Dari Frekuensi Fundamental." },
    { "en": "Apa Itu Derau Fasa?", "id": "Ketidakstabilan Frekuensi Jangka Pendek." },
    { "en": "Apa Itu Jitter?", "id": "Ketidakstabilan Waktu Jangka Pendek." },
    { "en": "Apa Itu Penuaan?", "id": "Pergeseran Frekuensi Jangka Panjang." },
    { "en": "Apa Itu Sinkronisasi?", "id": "Mengunci Beberapa Osilator Bersama." },
    { "en": "Apa Itu Kriteria Barkhausen?", "id": "Syarat Matematis Untuk Osilasi." },
    { "en": "Apa Itu Umpan Balik Positif?", "id": "Umpan Balik Yang Memperkuat Perubahan." },
    { "en": "Apa Itu Sirkuit Tangki?", "id": "Jaringan Resonansi LC Paralel." },
    { "en": "Apa Itu Osilator Armstrong?", "id": "Osilator LC Menggunakan Kopling Magnetik." },
    { "en": "Apa Itu Osilator Clapp?", "id": "Modifikasi Colpitts Untuk Stabilitas Lebih Baik." },
    { "en": "Apa Itu Osilator Cincin?", "id": "Osilator Berbasis Rantai Inverter." },
    { "en": "Apa Itu Multivibrator?", "id": "Nama Umum Osilator Relaksasi." },
    { "en": "Apa Itu Timer 555?", "id": "IC (Integrated Circuit) Serbaguna Untuk Osilator." },
    { "en": "Apa Osilator Berbasis Schmitt Trigger?", "id": "Schmitt Trigger Relaxation Oscillator." },
    { "en": "Apa Fungsi Timer 555 Dalam Mode Monostable?", "id": "Menghasilkan Pulsa Tunggal (One-Shot)." },
    { "en": "Apa Fungsi Timer 555 Dalam Mode Bistable?", "id": "Berfungsi Sebagai Flip-Flop RS." },
    { "en": "Apa Osilator Multivibrator Berbasis Transistor?", "id": "Emitter-Coupled Astable Multivibrator." },
    { "en": "Apa Detektor Fasa Berbasis Gerbang Logika?", "id": "XOR Phase Detector." },
    { "en": "Apa Detektor Fasa Berbasis Flip-Flop?", "id": "JK Flip-Flop Phase Detector." },
    { "en": "Apa Filter Loop PLL Yang Menggunakan Op-Amp?", "id": "Filter Loop Aktif." },
    { "en": "Apa Filter Loop PLL Yang Hanya RC?", "id": "Filter Loop Pasif." },
    { "en": "Apa Pembagi Frekuensi Dengan Rasio Ganda?", "id": "Dual-Modulus Prescaler." },
    { "en": "Apa Masalah Osilator Gagal Berosilasi?", "id": "Kegagalan Start-up." },
    { "en": "Apa Fenomena Osilator Melompat Antar Mode?", "id": "Mode Hopping." },
    { "en": "Apa Osilator Geser Fasa Menggunakan FET?", "id": "FET Phase-Shift Oscillator." },
    { "en": "Apa Osilator Colpitts Dengan Basis Ditanahkan?", "id": "Grounded-Base Colpitts Oscillator." },
    { "en": "Apa Osilator Yang Menghasilkan Pembawa TV Analog?", "id": "Carrier Oscillator." },
    { "en": "Apa Osilator Yang Menghasilkan Sinyal Warna TV?", "id": "Subcarrier Oscillator." },
    { "en": "Bagaimana Cara Mengubah Segitiga Menjadi Sinus?", "id": "Sirkuit Pembentuk Dioda (Diode Shaper)." },
    { "en": "Bagaimana Menghasilkan Gelombang Ramp Linear?", "id": "Kapasitor Diisi Sumber Arus Konstan." },
    { "en": "Apa Osilator Tabung Vakum Resistansi Negatif?", "id": "Osilator Dynatron." },
    { "en": "Apa Osilator Kristal Dua-Tahap?", "id": "Osilator Franklin." },
    { "en": "Bagaimana Cara Mengukur Derau Fasa?", "id": "Menggunakan Spectrum Analyzer." },
    { "en": "Bagaimana Cara Mengukur Jitter?", "id": "Menggunakan Osiloskop (Histogram)." },
    { "en": "Apa Tipe Penghitungan Frekuensi Paling Dasar?", "id": "Direct Counting." },
    { "en": "Apa Tipe Penghitungan Untuk Frekuensi Rendah?", "id": "Reciprocal Counting." },
    { "en": "Apa Model Teoritis Untuk Derau Fasa?", "id": "Model Leeson." },
    { "en": "Bagaimana Derau 1/f Mempengaruhi Derau Fasa?", "id": "Up-conversion Ke Dekat Frekuensi Pembawa." },
    { "en": "Apa Akibat Non-Linearitas Dioda Varaktor?", "id": "Perubahan Sensitivitas Penyetelan." },
    { "en": "Apa Akibat Drive Level Kristal Terlalu Tinggi?", "id": "Pergeseran Frekuensi Dan Kerusakan." },
    { "en": "Apa Itu Osilator Goyah (Jittery)?", "id": "Osilator Dengan Jitter Tinggi." },
    { "en": "Apa Itu Osilator Koheren?", "id": "Mempertahankan Fasa Stabil Seiring Waktu." },
    { "en": "Apa Itu Toleransi Frekuensi?", "id": "Penyimpangan Maksimum Dari Nominal." },
    { "en": "Apa Itu Stabilitas Vs Suhu?", "id": "Seberapa Besar Frekuensi Berubah Dengan Suhu." },
    { "en": "Apa Itu Koefisien Suhu Kristal?", "id": "Diukur Dalam PPM (Parts Per Million) Per Derajat Celcius." },
    { "en": "Apa Itu Titik Balik Kurva Suhu Kristal?", "id": "Titik Dengan Koefisien Suhu Nol." },
    { "en": "Apa Itu Resonator Pemandu Gelombang Akustik?", "id": "BAW (Bulk Acoustic Wave) Resonator." },
    { "en": "Apa Itu Osilator Diferensial?", "id": "Menghasilkan Output Diferensial (Ganda)." },
    { "en": "Apa Keuntungan Osilator Diferensial?", "id": "Penolakan Derau Common-mode Lebih Baik." },
    { "en": "Apa Itu Osilator Injeksi Regeneratif?", "id": "Osilator Yang Mengunci Ke Harmonik." },
    { "en": "Apa Itu Osilator Push-Pull?", "id": "Dua Transistor Bekerja Berlawanan Fasa." },
    { "en": "Apa Keuntungan Osilator Push-Pull?", "id": "Penekanan Harmonik Genap." },
    { "en": "Apa Itu Osilator Optik Parametrik?", "id": "Mengubah Satu Foton Menjadi Dua." },
    { "en": "Apa Itu Osilator Abadi?", "id": "Osilator Ideal Tanpa Kehilangan Energi." },
    { "en": "Apa Itu Kualitas Resonator?", "id": "Faktor Q (Quality Factor)." },
    { "en": "Apa Itu Faktor Muat (Loaded Q)?", "id": "Faktor Q Setelah Terhubung Ke Sirkuit." },
    { "en": "Apa Itu Faktor Tanpa Muat (Unloaded Q)?", "id": "Faktor Q Intrinsik Resonator." },
    { "en": "Apa Itu Bandwidth Osilasi?", "id": "Rentang Frekuensi Osilasi." },
    { "en": "Apa Itu Penarikan (Pulling Figure)?", "id": "Ukuran Kuantitatif Penarikan Frekuensi." },
    { "en": "Apa Itu Isolasi Balik?", "id": "Kemampuan Mencegah Sinyal Balik." },
    { "en": "Apa Itu Osilasi Terkopel Lemah?", "id": "Interaksi Minimal Antar Osilator." },
    { "en": "Apa Itu Osilasi Terkopel Kuat?", "id": "Interaksi Signifikan Antar Osilator." },
    { "en": "Apa Itu Kematian Amplitudo (Amplitude Death)?", "id": "Osilasi Berhenti Karena Kopling." },
    { "en": "Apa Itu Penarikan Fasa (Phase Pulling)?", "id": "Pergeseran Fasa Osilator Akibat Kopling." },
    { "en": "Apa Itu Osilator Sebagai Detektor?", "id": "Contoh: Regenerative Receiver." },
    { "en": "Apa Itu Efek Mikrofonik?", "id": "Perubahan Frekuensi Akibat Getaran Mekanis." },
    { "en": "Apa Itu Osilator Dengan Kebisingan Rendah?", "id": "Dirancang Khusus Untuk Derau Fasa Rendah." },
    { "en": "Apa Itu Osilator Daya Tinggi?", "id": "Dirancang Untuk Menghasilkan Daya Output Besar." },
    { "en": "Apa Itu Efisiensi Osilator?", "id": "Rasio Daya Output RF Per Daya DC." },
    { "en": "Apa Itu Penekanan Harmonik?", "id": "Ukuran Seberapa Rendah Level Harmonik." },
    { "en": "Apa Itu Filter Output Osilator?", "id": "Menghilangkan Harmonik Yang Tidak Diinginkan." },
    { "en": "Apa Itu Osilator Dengan Frekuensi Dapat Disetel?", "id": "Tunable Oscillator." },
    { "en": "Apa Itu Rentang Penyetelan (Tuning Range)?", "id": "Rentang Frekuensi Yang Dapat Dicapai." },
    { "en": "Apa Itu Linearitas Penyetelan?", "id": "Seberapa Linear Hubungan Tegangan-Frekuensi." },
    { "en": "Apa Itu Kecepatan Penyetelan?", "id": "Seberapa Cepat Frekuensi Dapat Berubah." },
    { "en": "Apa Itu Jitter Deterministik?", "id": "Jitter Dengan Pola Yang Dapat Diprediksi." },
    { "en": "Apa Itu Jitter Acak (Random Jitter)?", "id": "Jitter Tanpa Pola (Gaussian)." },
    { "en": "Apa Itu Jitter Total?", "id": "Kombinasi Jitter Deterministik Dan Acak." },
    { "en": "Apa Itu Distribusi Jitter?", "id": "Histogram Dari Variasi Waktu." },
    { "en": "Apa Itu Toleransi Jitter?", "id": "Jumlah Jitter Maksimum Yang Diterima Sistem." },
    { "en": "Apa Itu Transfer Jitter?", "id": "Bagaimana Jitter Merambat Melalui Sistem." },
    { "en": "Apa Itu Pembersihan Jitter (Jitter Cleaning)?", "id": "Mengurangi Jitter Menggunakan PLL." },
    { "en": "Apa Itu Attenuator Jitter?", "id": "Sirkuit PLL Untuk Mengurangi Jitter." },
    { "en": "Apa Itu Osilator Relaksasi Thyristor?", "id": "Osilator Menggunakan SCR Atau UJT." },
    { "en": "Apa Itu Osilator Royer?", "id": "Osilator Push-pull Resonansi." },
    { "en": "Di Mana Osilator Royer Digunakan?", "id": "Inverter Untuk Lampu CCFL." },
    { "en": "Apa Itu Osilator Pierce?", "id": "Osilator Kristal Paling Umum." },
    { "en": "Apa Itu Osilator Colpitts?", "id": "Osilator LC Dengan Pembagi Kapasitif." },
    { "en": "Apa Itu Osilator Hartley?", "id": "Osilator LC Dengan Pembagi Induktif." },
    { "en": "Apa Itu Osilator Jembatan Wien?", "id": "Osilator RC Untuk Gelombang Sinus." },
    { "en": "Apa Itu Osilator Geser Fasa?", "id": "Osilator RC Dengan Jaringan Geser Fasa." },
    { "en": "Apa Itu Multivibrator Astable?", "id": "Osilator Relaksasi Gelombang Persegi." },
    { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Frekuensi Tergantung Pada Tegangan Input." },
    { "en": "Apa Itu DDS (Direct Digital Synthesis)?", "id": "Menghasilkan Gelombang Secara Digital." },
    { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Umpan Balik Untuk Sinkronisasi Frekuensi." },
    { "en": "Apa Itu OCXO (Oven-Controlled Crystal Oscillator)?", "id": "Kristal Dipanaskan Untuk Stabilitas Maksimal." },
    { "en": "Apa Itu TCXO (Temperature-Compensated Crystal Oscillator)?", "id": "Menggunakan Sirkuit Untuk Kompensasi Suhu." },
    { "en": "Apa Itu VCXO (Voltage-Controlled Crystal Oscillator)?", "id": "Frekuensi Kristal Dapat Sedikit Diubah." },
    { "en": "Apa Itu Resonator?", "id": "Komponen Yang Bergetar Pada Frekuensi Tertentu." },
    { "en": "Apa Itu Faktor Kualitas?", "id": "Q Factor." },
    { "en": "Apa Itu Harmonik?", "id": "Kelipatan Ganzzahl Dari Frekuensi Fundamental." },
    { "en": "Apa Itu Derau Fasa?", "id": "Ketidakstabilan Frekuensi Jangka Pendek." },
    { "en": "Apa Itu Jitter?", "id": "Ketidakstabilan Waktu Jangka Pendek." },
    { "en": "Apa Itu Penuaan?", "id": "Pergeseran Frekuensi Jangka Panjang." },
    { "en": "Apa Itu Sinkronisasi?", "id": "Mengunci Beberapa Osilator Bersama." },
    { "en": "Apa Itu Kriteria Barkhausen?", "id": "Syarat Matematis Untuk Osilasi." },
    { "en": "Apa Itu Umpan Balik Positif?", "id": "Umpan Balik Yang Memperkuat Perubahan." },
    { "en": "Apa Itu Sirkuit Tangki?", "id": "Jaringan Resonansi LC Paralel." },
    { "en": "Apa Itu Osilator Armstrong?", "id": "Osilator LC Menggunakan Kopling Magnetik." },
    { "en": "Apa Itu Osilator Clapp?", "id": "Modifikasi Colpitts Untuk Stabilitas Lebih Baik." },
    { "en": "Apa Itu Osilator Cincin?", "id": "Osilator Berbasis Rantai Inverter." }



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
