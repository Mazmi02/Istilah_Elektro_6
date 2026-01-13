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


  { "en": "Apa Itu 'RS-232'?", "id": "Komunikasi Serial Kabel Jadul Jarak Dekat." },
  { "en": "Apa Itu 'RS-485'?", "id": "Komunikasi Serial Industri Jarak Jauh Kuat." },
  { "en": "Apa Itu 'Modbus RTU'?", "id": "Bahasa Komunikasi Data Standar Mesin Pabrik." },
  { "en": "Apa Itu 'Modbus TCP'?", "id": "Bahasa Komunikasi Pabrik Lewat Kabel LAN." },
  { "en": "Apa Itu 'Profibus'?", "id": "Jaringan Komunikasi Cepat Mesin Industri Siemens." },
  { "en": "Apa Itu 'Profinet'?", "id": "Jaringan Ethernet Cepat Khusus Industri Otomatis." },
  { "en": "Apa Itu 'CAN Bus'?", "id": "Jaringan Komunikasi Kabel Data Mobil Truk." },
  { "en": "Apa Itu 'LIN Bus'?", "id": "Jaringan Komunikasi Mobil Sederhana Murah." },
  { "en": "Apa Itu 'Ethernet/IP'?", "id": "Protokol Industri Lewat Jaringan Kabel Biasa." },
  { "en": "Apa Itu 'HART Protocol'?", "id": "Sinyal Digital Tumpang Di Kabel Analog." },
  { "en": "Apa Itu 'Induction Motor 1-Phase'?", "id": "Motor Listrik Pompa Air Rumah Tangga." },
  { "en": "Apa Itu 'Induction Motor 3-Phase'?", "id": "Motor Listrik Tenaga Besar Mesin Pabrik." },
  { "en": "Apa Itu 'Squirrel Cage Rotor'?", "id": "Bagian Putar Motor Bentuk Sangkar Tupai." },
  { "en": "Apa Itu 'Slip Ring Rotor'?", "id": "Bagian Putar Motor Pakai Cincin Gesek." },
  { "en": "Apa Itu 'Synchronous Speed'?", "id": "Kecepatan Putar Medan Magnet Stator Motor." },
  { "en": "Apa Itu 'Motor Slip'?", "id": "Selisih Kecepatan Medan Magnet Dan Rotor." },
  { "en": "Apa Itu 'Star-Delta Starter'?", "id": "Metode Nyala Motor Kurangi Arus Awal." },
  { "en": "Apa Itu 'Soft Starter'?", "id": "Alat Elektronik Haluskan Start Motor Besar." },
  { "en": "Apa Itu 'VFD' (Variable Frequency Drive)?", "id": "Pengatur Kecepatan Motor Paling Canggih." },
  { "en": "Apa Itu 'Motor Nameplate'?", "id": "Plat Label Spesifikasi Teknis Di Motor." },
  { "en": "Apa Itu 'Copper Pour'?", "id": "Area Tembaga Luas Mengisi Kosong PCB." },
  { "en": "Apa Itu 'Ground Plane'?", "id": "Lapisan Tembaga Khusus Jalur Negatif." },
  { "en": "Apa Itu 'Thermal Relief'?", "id": "Jalur Kaki Komponen Agar Mudah Disolder." },
  { "en": "Apa Itu 'Via Stitching'?", "id": "Jahitan Lubang Tembus Sambung Ground Plane." },
  { "en": "Apa Itu 'Blind Via'?", "id": "Lubang PCB Tidak Tembus Sampai Bawah." },
  { "en": "Apa Itu 'Buried Via'?", "id": "Lubang PCB Terkubur Di Lapisan Dalam." },
  { "en": "Apa Itu 'Track Width'?", "id": "Lebar Jalur Tembaga Sesuai Arus Listrik." },
  { "en": "Apa Itu 'Clearance'?", "id": "Jarak Aman Antara Dua Jalur Listrik." },
  { "en": "Apa Itu 'Mil' (Satuan)?", "id": "Satuan Ukuran Kecil Desain Papan PCB." },
  { "en": "Apa Itu 'Drc' (Design Rule Check)?", "id": "Pengecekan Otomatis Kesalahan Desain PCB." },
  { "en": "Apa Itu 'NEMA Enclosure'?", "id": "Standar Kotak Panel Listrik Amerika." },
  { "en": "Apa Itu 'IP Rating' (Ingress Protection)?", "id": "Kode Ketahanan Alat Terhadap Air Debu." },
  { "en": "Apa Itu 'IP20'?", "id": "Tidak Tahan Air Cuma Tahan Sentuh." },
  { "en": "Apa Itu 'IP54'?", "id": "Tahan Debu Dan Cipratan Air Hujan." },
  { "en": "Apa Itu 'IP65'?", "id": "Tahan Semprotan Air Tekanan Rendah." },
  { "en": "Apa Itu 'IP67'?", "id": "Tahan Rendam Air Kedalaman Satu Meter." },
  { "en": "Apa Itu 'IP68'?", "id": "Tahan Rendam Air Dalam Jangka Lama." },
  { "en": "Apa Itu 'Explosion Proof'?", "id": "Alat Tahan Ledakan Di Area Gas." },
  { "en": "Apa Itu 'Intrinsically Safe'?", "id": "Alat Energi Rendah Tidak Picu Ledakan." },
  { "en": "Apa Itu 'Class 1 Div 1'?", "id": "Area Sangat Berbahaya Gas Mudah Meledak." },
  { "en": "Apa Itu 'Resistor Color Code'?", "id": "Gelang Warna Penanda Nilai Hambatan Resistor." },
  { "en": "Apa Itu 'Tolerance Band'?", "id": "Gelang Emas Perak Batas Meleset Nilai." },
  { "en": "Apa Itu 'Ohm Meter'?", "id": "Alat Ukur Nilai Hambatan Resistor." },
  { "en": "Apa Itu 'Potentiometer Linear'?", "id": "Putaran Pengatur Nilai Hambatan Secara Rata." },
  { "en": "Apa Itu 'Potentiometer Logarithmic'?", "id": "Putaran Pengatur Volume Audio Telinga Manusia." },
  { "en": "Apa Itu 'Multi-Turn Potentiometer'?", "id": "Putaran Sangat Banyak Agar Nilai Presisi." },
  { "en": "Apa Itu 'Rheostat'?", "id": "Resistor Geser Besar Atur Arus Kuat." },
  { "en": "Apa Itu 'Shunt Resistor'?", "id": "Resistor Nilai Kecil Untuk Ukur Arus." },
  { "en": "Apa Itu 'Bleeder Resistor'?", "id": "Resistor Penguras Muatan Kapasitor Demi Keamanan." },
  { "en": "Apa Itu 'Terminator Resistor'?", "id": "Resistor Penutup Ujung Kabel Data RS485." },
  { "en": "Apa Itu 'Capacitor Polarity'?", "id": "Tanda Kutub Positif Negatif Kaki Kapasitor." },
  { "en": "Apa Itu 'Dielectric Material'?", "id": "Bahan Penyekat Di Dalam Kapasitor." },
  { "en": "Apa Itu 'Ripple Current'?", "id": "Arus Gangguan Naik Turun Masuk Kapasitor." },
  { "en": "Apa Itu 'ESR' (Equivalent Series Resistance)?", "id": "Hambatan Parasit Di Dalam Kapasitor Elco." },
  { "en": "Apa Itu 'Supercapacitor'?", "id": "Kapasitor Raksasa Pengganti Baterai Sementara." },
  { "en": "Apa Itu 'Filter Capacitor'?", "id": "Penyaring Tegangan DC Agar Rata Halus." },
  { "en": "Apa Itu 'Coupling Capacitor'?", "id": "Penyalur Sinyal AC Tahan Arus DC." },
  { "en": "Apa Itu 'Decoupling Capacitor'?", "id": "Penyedia Arus Cepat Dekat Kaki Chip." },
  { "en": "Apa Itu 'Snubber Capacitor'?", "id": "Peredam Lonjakan Tegangan Sakelar Listrik." },
  { "en": "Apa Itu 'Inductor Saturation'?", "id": "Inti Magnet Penuh Tidak Bisa Simpan." },
  { "en": "Apa Itu 'Back EMF'?", "id": "Tegangan Balik Saat Induktor Diputus Arus." },
  { "en": "Apa Itu 'Choke Coil'?", "id": "Induktor Penahan Frekuensi Tinggi Lewat." },
  { "en": "Apa Itu 'Transformer Ratio'?", "id": "Perbandingan Jumlah Lilitan Primer Dan Sekunder." },
  { "en": "Apa Itu 'Eddy Current Loss'?", "id": "Rugi Daya Panas Akibat Arus Liar." },
  { "en": "Apa Itu 'Hysteresis Loss'?", "id": "Rugi Daya Akibat Gesekan Magnet Inti." },
  { "en": "Apa Itu 'Lamination Steel'?", "id": "Plat Besi Tipis Inti Trafo Listrik." },
  { "en": "Apa Itu 'Toroidal Core'?", "id": "Inti Trafo Bentuk Donat Hemat Energi." },
  { "en": "Apa Itu 'Flux Density'?", "id": "Kepadatan Garis Gaya Magnet Per Area." },
  { "en": "Apa Itu 'Breadboard Bus Strip'?", "id": "Jalur Panjang Pinggir Papan Untuk Power." },
  { "en": "Apa Itu 'Breadboard Terminal Strip'?", "id": "Jalur Tengah Papan Untuk Pasang Komponen." },
  { "en": "Apa Itu 'Perfboard'?", "id": "Papan PCB Lubang Biasa Siap Solder." },
  { "en": "Apa Itu 'Stripboard' (Veroboard)?", "id": "Papan PCB Lubang Jalur Tembaga Panjang." },
  { "en": "Apa Itu 'Copper Clad'?", "id": "Papan Polos Lapis Tembaga Belum Dicetak." },
  { "en": "Apa Itu 'Ferric Chloride'?", "id": "Cairan Kimia Pelarut Tembaga Papan PCB." },
  { "en": "Apa Itu 'Toner Transfer'?", "id": "Cara Sablon PCB Pakai Setrika Panas." },
  { "en": "Apa Itu 'Drill Bit PCB'?", "id": "Mata Bor Sangat Kecil Lubangi PCB." },
  { "en": "Apa Itu 'Enclosure Box'?", "id": "Kotak Wadah Pelindung Rangkaian Elektronik." },
  { "en": "Apa Itu 'Potting Compound'?", "id": "Cairan Cor Penutup Elektronik Anti Air." },
  { "en": "Apa Itu 'Multimeter Continuity'?", "id": "Fitur Cek Kabel Nyambung Bunyi Beep." },
  { "en": "Apa Itu 'Multimeter HFE'?", "id": "Fitur Cek Penguatan Arus Transistor." },
  { "en": "Apa Itu 'Oscilloscope Channel'?", "id": "Saluran Masukan Sinyal Ke Layar Osiloskop." },
  { "en": "Apa Itu 'Oscilloscope Trigger'?", "id": "Pengunci Gambar Gelombang Agar Tidak Lari." },
  { "en": "Apa Itu 'Time Base'?", "id": "Pengatur Skala Waktu Horizontal Layar." },
  { "en": "Apa Itu 'Volt Per Division'?", "id": "Pengatur Skala Tegangan Vertikal Layar." },
  { "en": "Apa Itu 'Probe Calibration'?", "id": "Menyetel Kabel Osiloskop Agar Garis Lurus." },
  { "en": "Apa Itu 'Logic Probe'?", "id": "Pena Cek Sinyal Digital Nyala Mati." },
  { "en": "Apa Itu 'Frequency Generator'?", "id": "Alat Pembuat Sinyal Uji Rangkaian." },
  { "en": "Apa Itu 'Bench Power Supply'?", "id": "Sumber Listrik Meja Kerja Teknisi." },
  { "en": "Apa Itu 'Battery Capacity Tester'?", "id": "Alat Ukur Kapasitas Asli Baterai." },
  { "en": "Apa Itu 'Solar Irradiance'?", "id": "Kekuatan Sinar Matahari Jatuh Ke Panel." },
  { "en": "Apa Itu 'Panel Efficiency'?", "id": "Kemampuan Panel Ubah Sinar Jadi Listrik." },
  { "en": "Apa Itu 'Monocrystalline Panel'?", "id": "Panel Surya Hitam Paling Efisien Mahal." },
  { "en": "Apa Itu 'Polycrystalline Panel'?", "id": "Panel Surya Biru Lebih Murah Biasa." },
  { "en": "Apa Itu 'Thin Film Panel'?", "id": "Panel Surya Tipis Bisa Ditekuk Lentur." },
  { "en": "Apa Itu 'Solar Inverter'?", "id": "Pengubah Listrik Panel DC Jadi AC." },
  { "en": "Apa Itu 'Grid Tie'?", "id": "Sistem Panel Surya Gabung Jaringan PLN." },
  { "en": "Apa Itu 'Net Metering'?", "id": "Meteran Listrik Bisa Putar Balik Ekspor." },
  { "en": "Apa Itu 'Active Component'?", "id": "Komponen Yang Butuh Sumber Listrik." },
  { "en": "Apa Itu 'Passive Component'?", "id": "Komponen Yang Tidak Butuh Sumber Listrik." },
  { "en": "Apa Itu 'Surface Mount Device' (SMD)?", "id": "Komponen Tempel Di Atas Papan PCB." },
  { "en": "Apa Itu 'Through-Hole'?", "id": "Komponen Kaki Tusuk Tembus Papan PCB." },
  { "en": "Apa Itu 'Dual In-line Package' (DIP)?", "id": "Chip Kaki Dua Baris Sejajar Lurus." },
  { "en": "Apa Itu 'Quad Flat Package' (QFP)?", "id": "Chip Kotak Kaki Di Empat Sisi." },
  { "en": "Apa Itu 'Ball Grid Array' (BGA)?", "id": "Chip Kaki Bola Timah Di Bawah." },
  { "en": "Apa Itu 'Small Outline IC' (SOIC)?", "id": "Chip Tempel Kaki Kecil Rapat." },
  { "en": "Apa Itu 'Pinout'?", "id": "Peta Fungsi Tiap Kaki Komponen Chip." },
  { "en": "Apa Itu 'Datasheet Parameter'?", "id": "Nilai Batas Kemampuan Alat Di Buku." },
  { "en": "Apa Itu 'Nominal Voltage'?", "id": "Tegangan Rata Rata Normal Suatu Baterai." },
  { "en": "Apa Itu 'Cut-off Voltage'?", "id": "Batas Tegangan Baterai Habis Yang Aman." },
  { "en": "Apa Itu 'Charging Voltage'?", "id": "Tegangan Yang Dibutuhkan Saat Mengisi Baterai." },
  { "en": "Apa Itu 'Trickle Charge'?", "id": "Pengisian Arus Kecil Saat Baterai Penuh." },
  { "en": "Apa Itu 'Fast Charging'?", "id": "Pengisian Baterai Cepat Pakai Arus Besar." },
  { "en": "Apa Itu 'Battery Capacity' (mAh)?", "id": "Ukuran Isi Tangki Listrik Baterai Kecil." },
  { "en": "Apa Itu 'Battery Capacity' (Wh)?", "id": "Total Energi Yang Disimpan Dalam Baterai." },
  { "en": "Apa Itu 'Internal Resistance'?", "id": "Hambatan Dalam Baterai Menghambat Arus Keluar." },
  { "en": "Apa Itu 'Self-Discharge'?", "id": "Baterai Berkurang Sendiri Saat Tidak Dipakai." },
  { "en": "Apa Itu 'Cycle Life'?", "id": "Jumlah Kali Baterai Bisa Dicas Ulang." },
  { "en": "Apa Itu 'Memory Effect'?", "id": "Baterai Jadi Cepat Habis Jika Salah Cas." },
  { "en": "Apa Itu 'Deep Discharge'?", "id": "Menguras Isi Baterai Sampai Benar Habis." },
  { "en": "Apa Itu 'Overcharge'?", "id": "Mengisi Baterai Terlalu Penuh Bahaya Meledak." },
  { "en": "Apa Itu 'Overdischarge'?", "id": "Memakai Baterai Sampai Rusak Tidak Bisa Cas." },
  { "en": "Apa Itu 'Battery Pack'?", "id": "Gabungan Banyak Baterai Jadi Satu Kesatuan." },
  { "en": "Apa Itu 'Series Connection'?", "id": "Sambungan Baterai Menambah Tegangan Volt." },
  { "en": "Apa Itu 'Parallel Connection'?", "id": "Sambungan Baterai Menambah Kapasitas Ampere." },
  { "en": "Apa Itu 'Spot Welder'?", "id": "Alat Las Titik Sambung Plat Baterai." },
  { "en": "Apa Itu 'Nickel Strip'?", "id": "Plat Logam Tipis Penyambung Kutub Baterai." },
  { "en": "Apa Itu 'Kapton Tape'?", "id": "Solasi Kuning Tahan Panas Suhu Tinggi." },
  { "en": "Apa Itu 'Fish Paper'?", "id": "Kertas Isolator Hijau Pelindung Kutub Baterai." },
  { "en": "Apa Itu 'Heat Shrink Wrap'?", "id": "Plastik Pembungkus Paket Baterai Biar Rapi." },
  { "en": "Apa Itu 'XT60 Connector'?", "id": "Colokan Kuning Baterai Drone Arus Besar." },
  { "en": "Apa Itu 'Deans Connector' (T-Plug)?", "id": "Colokan Merah Baterai Mobil Remote Control." },
  { "en": "Apa Itu 'Balance Connector'?", "id": "Kabel Kecil Putih Untuk Cas Per Sel." },
  { "en": "Apa Itu 'JST-XH Connector'?", "id": "Jenis Colokan Kabel Balance Baterai Lipo." },
  { "en": "Apa Itu 'LiPo Safe Bag'?", "id": "Tas Tahan Api Untuk Simpan Baterai." },
  { "en": "Apa Itu 'Voltage Sag'?", "id": "Tegangan Anjlok Saat Beban Berat Nyala." },
  { "en": "Apa Itu 'Inrush Current'?", "id": "Lonjakan Arus Besar Saat Alat Dinyalakan." },
  { "en": "Apa Itu 'Short Circuit Current'?", "id": "Arus Maksimal Saat Kabel Korslet Langsung." },
  { "en": "Apa Itu 'No Load Current'?", "id": "Arus Yang Dipakai Saat Mesin Santai." },
  { "en": "Apa Itu 'Stall Current'?", "id": "Arus Maksimal Saat Motor Ditahan Paksa." },
  { "en": "Apa Itu 'Rated Current'?", "id": "Batas Arus Aman Pemakaian Terus Menerus." },
  { "en": "Apa Itu 'Fuse Holder'?", "id": "Rumah Tempat Memasang Sekring Pengaman." },
  { "en": "Apa Itu 'Circuit Breaker' (MCB)?", "id": "Sekring Otomatis Bisa Dinyalakan Lagi." },
  { "en": "Apa Itu 'Relay Normally Open'?", "id": "Sakelar Mati Sebelum Dialiri Listrik." },
  { "en": "Apa Itu 'Relay Normally Closed'?", "id": "Sakelar Nyala Sebelum Dialiri Listrik." },
  { "en": "Apa Itu 'Common Pin' (Relay)?", "id": "Kaki Pusat Sakelar Relay." },
  { "en": "Apa Itu 'Coil Pin' (Relay)?", "id": "Kaki Pemicu Magnet Relay." },
  { "en": "Apa Itu 'Flyback Diode'?", "id": "Dioda Pelindung Arus Balik Kumparan Relay." },
  { "en": "Apa Itu 'Transistor Base'?", "id": "Kaki Pemicu Arus Pada Transistor Bipolar." },
  { "en": "Apa Itu 'Transistor Collector'?", "id": "Kaki Masuk Arus Utama Transistor Bipolar." },
  { "en": "Apa Itu 'Transistor Emitter'?", "id": "Kaki Keluar Arus Utama Transistor Bipolar." },
  { "en": "Apa Itu 'Mosfet Gate'?", "id": "Kaki Pemicu Tegangan Pada Transistor Mosfet." },
  { "en": "Apa Itu 'Mosfet Drain'?", "id": "Kaki Masuk Arus Utama Transistor Mosfet." },
  { "en": "Apa Itu 'Mosfet Source'?", "id": "Kaki Keluar Arus Utama Transistor Mosfet." },
  { "en": "Apa Itu 'Heatsink'?", "id": "Sirip Logam Pendingin Komponen Panas." },
  { "en": "Apa Itu 'Thermal Paste'?", "id": "Pasta Abu Abu Penghantar Panas Prosesor." },
  { "en": "Apa Itu 'Fan Cooling'?", "id": "Kipas Angin Pendingin Alat Elektronik." },
  { "en": "Apa Itu 'Passive Cooling'?", "id": "Pendinginan Alami Tanpa Kipas Angin." },
  { "en": "Apa Itu 'Active Cooling'?", "id": "Pendinginan Paksa Pakai Kipas Atau Cairan." },
  { "en": "Apa Itu 'Water Cooling'?", "id": "Pendinginan Pakai Sirkulasi Air Radiator." },
  { "en": "Apa Itu 'Peltier Cooler'?", "id": "Pendingin Elektrik Sisi Dingin Dan Panas." },
  { "en": "Apa Itu 'Thermocouple'?", "id": "Sensor Suhu Dari Dua Kawat Logam." },
  { "en": "Apa Itu 'RTD Sensor'?", "id": "Sensor Suhu Logam Platina Presisi Tinggi." },
  { "en": "Apa Itu 'NTC Thermistor'?", "id": "Sensor Suhu Murah Hambatan Turun Panas." },
  { "en": "Apa Itu 'Infrared Thermometer'?", "id": "Alat Ukur Suhu Tembak Jarak Jauh." },
  { "en": "Apa Itu 'Thermal Camera'?", "id": "Kamera Melihat Panas Benda Berwarna Warni." },
  { "en": "Apa Itu 'Soldering Iron'?", "id": "Alat Pemanas Untuk Melelehkan Timah." },
  { "en": "Apa Itu 'Solder Wire'?", "id": "Kawat Timah Paduan Untuk Menyambung Komponen." },
  { "en": "Apa Itu 'Soldering Flux'?", "id": "Minyak Solder Agar Timah Nempel Sempurna." },
  { "en": "Apa Itu 'Desoldering Pump'?", "id": "Alat Sedot Timah Cair Saat Bongkar." },
  { "en": "Apa Itu 'Solder Wick'?", "id": "Pita Tembaga Penyerap Sisa Timah." },
  { "en": "Apa Itu 'Helping Hands'?", "id": "Alat Penjepit PCB Saat Menyolder." },
  { "en": "Apa Itu 'Magnifying Lamp'?", "id": "Lampu Meja Dengan Kaca Pembesar." },
  { "en": "Apa Itu 'Fume Extractor'?", "id": "Kipas Penyedot Asap Solder Beracun." },
  { "en": "Apa Itu 'Multimeter Probe'?", "id": "Kabel Tusuk Merah Hitam Alat Ukur." },
  { "en": "Apa Itu 'Alligator Clip'?", "id": "Kabel Jepit Buaya Untuk Sambungan Cepat." },
  { "en": "Apa Itu 'Banana Plug'?", "id": "Colokan Kabel Tester Bentuk Pisang." },
  { "en": "Apa Itu 'Breadboard'?", "id": "Papan Tusuk Merakit Rangkaian Tanpa Solder." },
  { "en": "Apa Itu 'Jumper Wire'?", "id": "Kabel Warni Warni Penghubung Rangkaian." },
  { "en": "Apa Itu 'Power Supply Bench'?", "id": "Sumber Listrik Meja Kerja Teknisi." },
  { "en": "Apa Itu 'Oscilloscope'?", "id": "Alat Lihat Bentuk Gelombang Sinyal Listrik." },
  { "en": "Apa Itu 'Signal Generator'?", "id": "Alat Pembuat Sinyal Gelombang Uji Coba." },
  { "en": "Apa Itu 'Logic Analyzer'?", "id": "Alat Baca Sinyal Data Digital Komputer." },
  { "en": "Apa Itu 'Frequency Counter'?", "id": "Alat Hitung Jumlah Getaran Sinyal." },
  { "en": "Apa Itu 'LCR Meter'?", "id": "Alat Ukur Komponen Pasif Paling Akurat." },
  { "en": "Apa Itu 'ESR Meter'?", "id": "Alat Cek Kesehatan Kapasitor Elco Tua." },
  { "en": "Apa Itu 'IC Tester'?", "id": "Alat Cek Kondisi Chip Rusak Tidak." },
  { "en": "Apa Itu 'Transistor Tester'?", "id": "Alat Cek Kaki Dan Jenis Transistor." },
  { "en": "Apa Itu 'Cable Tester'?", "id": "Alat Cek Kabel Putus Atau Nyambung." },
  { "en": "Apa Itu 'Wire Stripper'?", "id": "Tang Pengupas Kabel Agar Tidak Putus." },
  { "en": "Apa Itu 'Crimping Tool'?", "id": "Tang Press Kepala Kabel Sekun." },
  { "en": "Apa Itu 'Flush Cutter'?", "id": "Tang Potong Kaki Komponen Rata PCB." },
  { "en": "Apa Itu 'Needle Nose Pliers'?", "id": "Tang Lancip Penjepit Benda Kecil." },
  { "en": "Apa Itu 'Precision Screwdriver'?", "id": "Obeng Kecil Untuk Baut Jam HP." },
  { "en": "Apa Itu 'Anti-Static Mat'?", "id": "Alas Meja Karet Tahan Listrik Statis." },
  { "en": "Apa Itu 'Grounding Strap'?", "id": "Gelang Kabel Ground Tangan Teknisi." },
  { "en": "Apa Itu 'Bootloader'?", "id": "Program Awal Penerima Kode Baru." },
  { "en": "Apa Itu 'Firmware'?", "id": "Software Permanen Di Dalam Alat Elektronik." },
  { "en": "Apa Itu 'Interrupt'?", "id": "Selaan Tugas Mendadak Prioritas Tinggi." },
  { "en": "Apa Itu 'Watchdog Timer'?", "id": "Sistem Reset Otomatis Jika Alat Macet." },
  { "en": "Apa Itu 'Brown-out'?", "id": "Tegangan Turun Sesaat Bikin Error." },
  { "en": "Apa Itu 'UART Protocol'?", "id": "Komunikasi Data Serial Dua Kabel Sederhana." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Kecepatan Kirim Data Serial Per Detik." },
  { "en": "Apa Itu 'Parity Bit'?", "id": "Bit Tambahan Untuk Cek Kesalahan Data." },
  { "en": "Apa Itu 'I2C Pull-up'?", "id": "Resistor Wajib Pada Jalur Komunikasi I2C." },
  { "en": "Apa Itu 'SPI Clock'?", "id": "Sinyal Detak Pengatur Kecepatan Data SPI." },
  { "en": "Apa Itu 'MISO Line'?", "id": "Jalur Data Masuk Ke Otak Utama." },
  { "en": "Apa Itu 'MOSI Line'?", "id": "Jalur Data Keluar Dari Otak Utama." },
  { "en": "Apa Itu 'ADC Resolution'?", "id": "Tingkat Ketelitian Ubah Sinyal Jadi Angka." },
  { "en": "Apa Itu 'Sampling Rate'?", "id": "Kecepatan Ambil Sampel Sinyal Per Detik." },
  { "en": "Apa Itu 'Aliasing'?", "id": "Sinyal Palsu Akibat Sampling Terlalu Lambat." },
  { "en": "Apa Itu 'PWM Frequency'?", "id": "Kecepatan Kedip Sinyal Pengatur Daya." },
  { "en": "Apa Itu 'Duty Cycle'?", "id": "Persentase Waktu Sinyal Dalam Kondisi Nyala." },
  { "en": "Apa Itu 'H-Bridge'?", "id": "Rangkaian Jembatan Pembalik Arah Motor DC." },
  { "en": "Apa Itu 'Dead Time'?", "id": "Jeda Aman Agar Transistor Tidak Korslet." },
  { "en": "Apa Itu 'Snubber Circuit'?", "id": "Peredam Lonjakan Tegangan Liar Sakelar." },
  { "en": "Apa Itu 'Flyback Diode'?", "id": "Dioda Pengaman Arus Balik Kumparan Magnet." },
  { "en": "Apa Itu 'Decoupling Capacitor'?", "id": "Kapasitor Penstabil Tegangan Dekat Kaki Chip." },
  { "en": "Apa Itu 'Bypass Capacitor'?", "id": "Jalan Pintas Buang Gangguan Ke Tanah." },
  { "en": "Apa Itu 'Ground Loop'?", "id": "Arus Putar Tanah Penyebab Dengung Audio." },
  { "en": "Apa Itu 'Star Ground'?", "id": "Semua Kabel Ground Kumpul Satu Titik." },
  { "en": "Apa Itu 'Differential Pair'?", "id": "Dua Kabel Sinyal Saling Berlawanan Fasa." },
  { "en": "Apa Itu 'Twisted Pair'?", "id": "Kabel Dipelintir Untuk Tolak Gangguan Luar." },
  { "en": "Apa Itu 'Shielding'?", "id": "Bungkus Logam Pelindung Kabel Dari Gangguan." },
  { "en": "Apa Itu 'Impedance Matching'?", "id": "Menyamakan Hambatan Agar Sinyal Maksimal." },
  { "en": "Apa Itu 'VSWR' (Antenna)?", "id": "Ukuran Banyaknya Sinyal Balik Ke Pemancar." },
  { "en": "Apa Itu 'Smith Chart'?", "id": "Grafik Bulat Alat Bantu Desain Antena." },
  { "en": "Apa Itu 'Balun'?", "id": "Penyesuai Kabel Antena Seimbang Dan Tidak." },
  { "en": "Apa Itu 'Dipole Antenna'?", "id": "Antena Kawat Lurus Dua Kutub Sederhana." },
  { "en": "Apa Itu 'Monopole Antenna'?", "id": "Antena Kawat Tunggal Tegak Lurus Ground." },
  { "en": "Apa Itu 'Patch Antenna'?", "id": "Antena Pipih Tipis Di Atas PCB." },
  { "en": "Apa Itu 'Active Filter'?", "id": "Penyaring Frekuensi Menggunakan Penguat Listrik." },
  { "en": "Apa Itu 'Passive Filter'?", "id": "Penyaring Frekuensi Tanpa Sumber Listrik." },
  { "en": "Apa Itu 'Low Pass Filter'?", "id": "Penyaring Yang Meloloskan Frekuensi Rendah Saja." },
  { "en": "Apa Itu 'High Pass Filter'?", "id": "Penyaring Yang Meloloskan Frekuensi Tinggi Saja." },
  { "en": "Apa Itu 'Band Pass Filter'?", "id": "Penyaring Yang Meloloskan Frekuensi Tengah Saja." },
  { "en": "Apa Itu 'Band Stop Filter'?", "id": "Penyaring Yang Membuang Frekuensi Tengah Saja." },
  { "en": "Apa Itu 'Notch Filter'?", "id": "Penyaring Membuang Satu Frekuensi Gangguan." },
  { "en": "Apa Itu 'Oscillator RC'?", "id": "Pembuat Gelombang Pakai Resistor Kapasitor." },
  { "en": "Apa Itu 'Oscillator Crystal'?", "id": "Pembuat Gelombang Stabil Dari Kristal Kuarsa." },
  { "en": "Apa Itu 'Voltage Divider'?", "id": "Penurun Tegangan Pakai Dua Resistor Seri." },
  { "en": "Apa Itu 'Current Mirror'?", "id": "Rangkaian Penyalin Arus Listrik Yang Sama." },
  { "en": "Apa Itu 'Darlington Pair'?", "id": "Dua Transistor Gabung Jadi Penguat Ganda." },
  { "en": "Apa Itu 'Sziklai Pair'?", "id": "Pasangan Transistor Beda Jenis Penguat Arus." },
  { "en": "Apa Itu 'Bootstrap Circuit'?", "id": "Rangkaian Pendorong Tegangan Sisi Atas." },
  { "en": "Apa Itu 'Level Shifter'?", "id": "Penyesuai Tegangan Sinyal Data Beda Level." },
  { "en": "Apa Itu 'Op-Amp Comparator'?", "id": "Pembanding Nilai Dua Tegangan Masukan." },
  { "en": "Apa Itu 'Op-Amp Buffer'?", "id": "Penyangga Arus Dengan Tegangan Tetap." },
  { "en": "Apa Itu 'Schmitt Trigger'?", "id": "Pembersih Sinyal Masukan Yang Berisik." },
  { "en": "Apa Itu 'PCB Via'?", "id": "Lubang Tembus Penghubung Lapisan Papan PCB." },
  { "en": "Apa Itu 'PCB Trace'?", "id": "Jalur Tembaga Penghantar Listrik Di Papan." },
  { "en": "Apa Itu 'PCB Pad'?", "id": "Area Tembaga Tempat Kaki Komponen Disolder." },
  { "en": "Apa Itu 'Solder Mask'?", "id": "Cat Pelindung Jalur Tembaga Agar Awet." },
  { "en": "Apa Itu 'Silkscreen'?", "id": "Tulisan Putih Penanda Letak Komponen." },
  { "en": "Apa Itu 'Gerber File'?", "id": "Data Gambar Untuk Mesin Cetak PCB." },
  { "en": "Apa Itu 'Multimeter Continuity'?", "id": "Cek Sambungan Kabel Bunyi Beep." },
  { "en": "Apa Itu 'Oscilloscope Probe'?", "id": "Kabel Tusuk Alat Melihat Gelombang Listrik." },
  { "en": "Apa Itu 'ESD Strap'?", "id": "Gelang Pengaman Listrik Statis Tubuh Teknisi." },
  { "en": "Apa Itu 'Solder Wick'?", "id": "Pita Tembaga Penyerap Sisa Timah Cair." },
  { "en": "Apa Itu 'Flux Pen'?", "id": "Spidol Cairan Pembersih Agar Timah Nempel." },
  { "en": "Apa Itu 'Breadboard'?", "id": "Papan Rakit Rangkaian Tanpa Perlu Solder." },
  { "en": "Apa Itu 'Jumper Wire'?", "id": "Kabel Tusuk Penghubung Rangkaian Sementara." },
  { "en": "Apa Itu 'Battery Holder'?", "id": "Kotak Wadah Tempat Memasang Baterai." },
  { "en": "Apa Itu 'DC Jack'?", "id": "Lubang Colokan Masuk Daya Adaptor Listrik." },
  { "en": "Apa Itu 'Pin Header'?", "id": "Jarum Logam Konektor Pada Papan Sirkuit." },
  { "en": "Apa Itu 'Terminal Block'?", "id": "Konektor Sambungan Kabel Jepit Pakai Obeng." },
  { "en": "Apa Itu 'Fuse Holder'?", "id": "Rumah Tempat Memasang Sekring Pengaman Kaca." },
  { "en": "Apa Itu 'Heat Shrink'?", "id": "Selang Plastik Bakar Pelindung Sambungan Kabel." },
  { "en": "Apa Itu 'Cable Tie'?", "id": "Tali Plastik Pengikat Kerapian Kabel." },
  { "en": "Apa Itu 'Thermal Paste'?", "id": "Krim Penghantar Panas Ke Pendingin Logam." },
  { "en": "Apa Itu 'Heatsink'?", "id": "Logam Sirip Pembuang Panas Komponen." },
  { "en": "Apa Itu 'Fan 12V'?", "id": "Kipas Angin Pendingin Alat Elektronik." },
  { "en": "Apa Itu 'Switch Toggle'?", "id": "Sakelar Batang Besi Cetek Cetok Manual." },
  { "en": "Apa Itu 'Switch Rocker'?", "id": "Sakelar Lampu Kotak Besar On Off." },
  { "en": "Apa Itu 'Push Button'?", "id": "Tombol Tekan Yang Balik Sendiri." },
  { "en": "Apa Itu 'Potentiometer'?", "id": "Komponen Putar Pengatur Nilai Hambatan." },
  { "en": "Apa Itu 'Trimpot'?", "id": "Potensiometer Kecil Disetel Pakai Obeng." },
  { "en": "Apa Itu 'LDR Sensor'?", "id": "Sensor Cahaya Hambatan Berubah Kena Sinar." },
  { "en": "Apa Itu 'Relay 5V'?", "id": "Sakelar Listrik Kendali Tegangan Rendah." },
  { "en": "Apa Itu 'Buzzer 5V'?", "id": "Komponen Penghasil Suara Beep Nyaring." },
  { "en": "Apa Itu 'Microphone Electret'?", "id": "Komponen Kecil Perekam Suara Jadi Listrik." },
  { "en": "Apa Itu 'Solar Panel 6V'?", "id": "Pembangkit Listrik Cahaya Matahari Skala Kecil." },
  { "en": "Apa Itu 'Dynamo Motor'?", "id": "Motor Listrik DC Kecil Mainan Mobil." },
  { "en": "Apa Itu 'Stepper Motor'?", "id": "Motor Bergerak Langkah Demi Langkah Presisi." },
  { "en": "Apa Itu 'Servo Motor'?", "id": "Motor Pintar Penggerak Sudut Lengan Robot." },
  { "en": "Apa Itu 'Battery 9V'?", "id": "Baterai Kotak Untuk Multimeter Atau Mainan." },
  { "en": "Apa Itu 'Battery 18650'?", "id": "Baterai Silinder Besar Tenaga Laptop Senter." },
  { "en": "Apa Itu 'Battery AA'?", "id": "Baterai Ukuran Sedang Untuk Jam Dinding." },
  { "en": "Apa Itu 'Battery AAA'?", "id": "Baterai Ukuran Kecil Untuk Remote TV." },
  { "en": "Apa Itu 'Coin Battery'?", "id": "Baterai Pipih Bulat Untuk Jam Tangan." },
  { "en": "Apa Itu 'Surface Mount' (SMD)?", "id": "Komponen Tempel Di Atas Permukaan Papan." },
  { "en": "Apa Itu 'Soldering Iron'?", "id": "Alat Pemanas Untuk Melelehkan Timah Solder." },
  { "en": "Apa Itu 'Continuity Test'?", "id": "Fitur Cek Kabel Nyambung Bunyi Beep." },
  { "en": "Apa Itu 'Resistor Tolerance'?", "id": "Batas Nilai Meleset Yang Dianggap Wajar." },
  { "en": "Apa Itu 'Capacitor Polarity'?", "id": "Tanda Kutub Positif Negatif Pada Kapasitor." },
  { "en": "Apa Itu 'Inductor Coil'?", "id": "Gulungan Kawat Tembaga Penyimpan Medan Magnet." },
  { "en": "Apa Itu 'Step-Up Transformer'?", "id": "Alat Penaik Tegangan Listrik Arus AC." },
  { "en": "Apa Itu 'Diode Bridge'?", "id": "Empat Dioda Pengubah AC Jadi DC." },
  { "en": "Apa Itu 'Relay NC'?", "id": "Sakelar Nyala Sebelum Dialiri Arus Listrik." },
  { "en": "Apa Itu 'Heat Dissipation'?", "id": "Cara Komponen Membuang Panas Berlebih." },
  { "en": "Apa Itu 'Short Circuit'?", "id": "Kabel Positif Negatif Bersentuhan Langsung Korslet." },
  { "en": "Apa Itu 'Open Circuit'?", "id": "Jalur Kabel Putus Arus Tidak Mengalir." },
  { "en": "Apa Itu 'Grounding'?", "id": "Pembuangan Arus Bocor Ke Dalam Tanah." },
  { "en": "Apa Itu 'Cable Shielding'?", "id": "Lapisan Logam Pelindung Kabel Dari Gangguan." },
  { "en": "Apa Itu 'Twisted Pair'?", "id": "Kabel Dipelintir Untuk Kurangi Gangguan Sinyal." },
  { "en": "Apa Itu 'Datasheet'?", "id": "Buku Spesifikasi Teknis Komponen Dari Pabrik." },
  { "en": "Apa Itu 'Schematic Diagram'?", "id": "Gambar Peta Jalur Rangkaian Elektronik Lengkap." },
  { "en": "Apa Itu 'PCB Layout'?", "id": "Desain Tata Letak Komponen Pada Papan." },
  { "en": "Apa Itu 'Gerber File'?", "id": "File Desain Untuk Mesin Cetak PCB." },
  { "en": "Apa Itu 'Bill of Materials' (BOM)?", "id": "Daftar Belanja Seluruh Komponen Proyek." },
  { "en": "Apa Itu 'Firmware'?", "id": "Program Tetap Yang Ditanam Di Chip." },
  { "en": "Apa Itu 'Bootloader'?", "id": "Program Awal Untuk Masukkan Kode Baru." },
  { "en": "Apa Itu 'Debugging'?", "id": "Proses Mencari Dan Memperbaiki Kesalahan Program." },
  { "en": "Apa Itu 'Compiler'?", "id": "Penerjemah Kode Tulis Jadi Bahasa Mesin." },
  { "en": "Apa Itu 'Upload Code'?", "id": "Mengirim Program Masuk Ke Mikrokontroler." },
  { "en": "Apa Itu 'Serial Monitor'?", "id": "Layar Pesan Komunikasi Chip Dan Komputer." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Kecepatan Kirim Data Bit Per Detik." },
  { "en": "Apa Itu 'Library Code'?", "id": "Kumpulan Kode Siap Pakai Memudahkan Program." },
  { "en": "Apa Itu 'Variable'?", "id": "Tempat Menyimpan Data Sementara Dalam Program." },
  { "en": "Apa Itu 'Loop Function'?", "id": "Perintah Mengulang Program Terus Menerus." },
  { "en": "Apa Itu 'If-Else'?", "id": "Logika Keputusan Jika Maka Dalam Program." },
  { "en": "Apa Itu 'Void Function'?", "id": "Blok Kode Untuk Tugas Khusus Tertentu." },
  { "en": "Apa Itu 'Array'?", "id": "Daftar Banyak Data Dalam Satu Wadah." },
  { "en": "Apa Itu 'String Data'?", "id": "Tipe Data Berupa Teks Atau Huruf." },
  { "en": "Apa Itu 'Integer Data'?", "id": "Tipe Data Berupa Angka Bulat Utuh." },
  { "en": "Apa Itu 'Float Data'?", "id": "Tipe Data Berupa Angka Pecahan Desimal." },
  { "en": "Apa Itu 'Boolean Data'?", "id": "Tipe Data Benar Salah Atau 1-0." },
  { "en": "Apa Itu 'Header Pin'?", "id": "Jarum Konektor Logam Pada Papan Sirkuit." },
  { "en": "Apa Itu 'Breadboard'?", "id": "Papan Coba Rangkaian Tanpa Perlu Solder." },
  { "en": "Apa Itu 'Battery Holder'?", "id": "Wadah Kotak Tempat Memasang Baterai." },
  { "en": "Apa Itu 'Terminal Block'?", "id": "Konektor Kabel Jepit Pakai Baut Obeng." },
  { "en": "Apa Itu 'DC Jack'?", "id": "Lubang Colokan Masuk Daya Listrik Adaptor." },
  { "en": "Apa Itu 'USB Port'?", "id": "Colokan Data Standar Ke Komputer." },
  { "en": "Apa Itu 'Toggle Switch'?", "id": "Sakelar Batang Besi Cetek Cetok Manual." },
  { "en": "Apa Itu 'Push Button'?", "id": "Tombol Tekan Yang Kembali Sendiri." },
  { "en": "Apa Itu 'Potentiometer'?", "id": "Putaran Pengatur Nilai Hambatan Sinyal." },
  { "en": "Apa Itu 'LDR Sensor'?", "id": "Sensor Cahaya Yang Berubah Hambatannya." },
  { "en": "Apa Itu 'Relay Module'?", "id": "Modul Sakelar Listrik Kendali Mikrokontroler." },
  { "en": "Apa Itu 'Buzzer'?", "id": "Komponen Penghasil Suara Beep Peringatan." },
  { "en": "Apa Itu 'Microphone'?", "id": "Pengubah Suara Menjadi Sinyal Listrik." },
  { "en": "Apa Itu 'Speaker'?", "id": "Pengubah Sinyal Listrik Menjadi Suara." },
  { "en": "Apa Itu 'Servo Motor'?", "id": "Motor Pintar Pengatur Sudut Gerak Robot." },
  { "en": "Apa Itu 'Stepper Motor'?", "id": "Motor Gerak Langkah Demi Langkah Presisi." },
  { "en": "Apa Itu 'DC Motor'?", "id": "Motor Putar Biasa Tenaga Baterai." },
  { "en": "Apa Itu 'Solar Panel'?", "id": "Papan Pengubah Cahaya Matahari Jadi Listrik." },
  { "en": "Apa Itu 'Li-Ion Battery'?", "id": "Baterai Isi Ulang Tenaga Gadget Modern." },
  { "en": "Apa Itu 'Alkaline Battery'?", "id": "Baterai Sekali Pakai Tahan Lama." },
  { "en": "Apa Itu 'Thermal Fuse'?", "id": "Kawat Pengaman Putus Jika Suhu Panas." },
  { "en": "Apa Itu 'Heat Shrink'?", "id": "Selang Bakar Pelindung Sambungan Kabel." },
  { "en": "Apa Itu 'Heatsink'?", "id": "Logam Bersirip Pembuang Panas Komponen." },
  { "en": "Apa Itu 'Cooling Fan'?", "id": "Kipas Pendingin Alat Elektronik Panas." },
  { "en": "Apa Itu 'PCB Spacer'?", "id": "Kaki Baut Penyangga Papan PCB." },
  { "en": "Apa Itu 'Project Box'?", "id": "Kotak Wadah Pelindung Alat Elektronik." },
  { "en": "Apa Itu 'Machine Screw'?", "id": "Baut Pengencang Komponen Elektronik." },
  { "en": "Apa Itu 'Hex Nut'?", "id": "Mur Pasangan Baut Pengencang Komponen." },
  { "en": "Apa Itu 'Flat Washer'?", "id": "Ring Pelapis Baut Agar Tidak Lepas." },
  { "en": "Apa Itu 'Flux Pen'?", "id": "Spidol Pembersih Agar Timah Menempel." },
  { "en": "Apa Itu 'Desoldering Pump'?", "id": "Alat Sedot Timah Cair Manual." },
  { "en": "Apa Itu 'Wire Stripper'?", "id": "Tang Pengupas Kulit Kabel Otomatis." },
  { "en": "Apa Itu 'Flush Cutter'?", "id": "Tang Potong Kaki Komponen Rata Papan." },
  { "en": "Apa Itu 'Tweezers'?", "id": "Pinset Penjepit Komponen Chip Kecil." },
  { "en": "Apa Itu 'Magnifying Glass'?", "id": "Kaca Pembesar Melihat Jalur PCB Kecil." },
  { "en": "Apa Itu 'Third Hand Tool'?", "id": "Alat Penjepit Papan Saat Menyolder." },
  { "en": "Apa Itu 'Solder Mat'?", "id": "Alas Karet Tahan Panas Meja Kerja." },
  { "en": "Apa Itu 'Multimeter Probe'?", "id": "Kabel Jarum Alat Ukur Listrik." },
  { "en": "Apa Itu 'Alligator Clip'?", "id": "Kabel Jepit Buaya Sambungan Cepat." },
  { "en": "Apa Itu 'Power Supply'?", "id": "Sumber Listrik Meja Kerja Teknisi." },
  { "en": "Apa Itu 'Signal Generator'?", "id": "Alat Pembuat Gelombang Sinyal Uji." },
  { "en": "Apa Itu 'Oscilloscope Probe'?", "id": "Kabel Tusuk Alat Lihat Gelombang." },
  { "en": "Apa Itu 'ESD Strap'?", "id": "Gelang Pengaman Listrik Statis Tubuh." },
  { "en": "Apa Itu 'Safety Glasses'?", "id": "Kacamata Pelindung Mata Dari Timah." },
  { "en": "Apa Itu 'Tool Kit'?", "id": "Kotak Peralatan Lengkap Teknisi Elektronika." },
  { "en": "Apa Itu 'Glue Gun'?", "id": "Pistol Lem Bakar Perekat Komponen." },
  { "en": "Apa Itu 'Cutter Knife'?", "id": "Pisau Potong Jalur Atau Kabel." },
  { "en": "Apa Itu 'Ruler'?", "id": "Penggaris Ukur Jarak Kaki Komponen." },
  { "en": "Apa Itu 'Caliper'?", "id": "Jangka Sorong Ukur Diameter Komponen." },
  { "en": "Apa Itu 'Mini Drill'?", "id": "Bor Kecil Pembuat Lubang PCB." },
  { "en": "Apa Itu 'Voltage' (Tegangan)?", "id": "Tekanan Dorong Listrik Agar Mengalir." },
  { "en": "Apa Itu 'Current' (Arus)?", "id": "Aliran Listrik Yang Sedang Lewat." },
  { "en": "Apa Itu 'Resistance' (Hambatan)?", "id": "Penghambat Aliran Arus Listrik Lewat." },
  { "en": "Apa Itu 'Power' (Daya)?", "id": "Tenaga Listrik Yang Dipakai Alat." },
  { "en": "Apa Itu 'DC' (Direct Current)?", "id": "Arus Searah Seperti Baterai Aki." },
  { "en": "Apa Itu 'AC' (Alternating Current)?", "id": "Arus Bolak Balik Listrik PLN." },
  { "en": "Apa Itu 'Short Circuit' (Korslet)?", "id": "Kabel Positif Negatif Nempel Langsung." },
  { "en": "Apa Itu 'Open Circuit'?", "id": "Kabel Putus Arus Tidak Mengalir." },
  { "en": "Apa Itu 'Ground' (GND)?", "id": "Titik Nol Atau Tanah Negatif." },
  { "en": "Apa Itu 'VCC'?", "id": "Sumber Tegangan Positif Utama Rangkaian." },
  { "en": "Apa Itu 'Resistor'?", "id": "Komponen Penahan Arus Listrik." },
  { "en": "Apa Itu 'Capacitor'?", "id": "Komponen Penyimpan Setrum Sementara." },
  { "en": "Apa Itu 'Inductor'?", "id": "Gulungan Kawat Simpan Medan Magnet." },
  { "en": "Apa Itu 'Diode'?", "id": "Katup Listrik Cuma Satu Arah." },
  { "en": "Apa Itu 'LED'?", "id": "Lampu Indikator Hemat Energi Kecil." },
  { "en": "Apa Itu 'Transistor'?", "id": "Sakelar Elektronik Dan Penguat Sinyal." },
  { "en": "Apa Itu 'Relay'?", "id": "Sakelar Mekanik Digerakkan Magnet Listrik." },
  { "en": "Apa Itu 'Fuse' (Sekring)?", "id": "Kawat Putus Jika Arus Berlebih." },
  { "en": "Apa Itu 'Transformer' (Trafo)?", "id": "Alat Naik Turun Tegangan Listrik." },
  { "en": "Apa Itu 'IC' (Integrated Circuit)?", "id": "Chip Hitam Isi Ribuan Komponen." },
  { "en": "Apa Itu 'PCB'?", "id": "Papan Hijau Tempat Komponen Nempel." },
  { "en": "Apa Itu 'Solder'?", "id": "Timah Cair Penyambung Komponen Elektronik." },
  { "en": "Apa Itu 'Flux'?", "id": "Minyak Pembersih Agar Timah Lengket." },
  { "en": "Apa Itu 'Multimeter'?", "id": "Alat Ukur Listrik Serba Bisa." },
  { "en": "Apa Itu 'Oscilloscope'?", "id": "Layar Melihat Bentuk Gelombang Listrik." },
  { "en": "Apa Itu 'Power Supply'?", "id": "Kotak Sumber Listrik Meja Kerja." },
  { "en": "Apa Itu 'Microcontroller'?", "id": "Otak Komputer Kecil Pengendali Alat." },
  { "en": "Apa Itu 'Sensor'?", "id": "Indra Perasa Untuk Alat Elektronik." },
  { "en": "Apa Itu 'Actuator'?", "id": "Penggerak Fisik Hasil Perintah Listrik." },
  { "en": "Apa Itu 'Input'?", "id": "Data Masuk Ke Dalam Sistem." },
  { "en": "Apa Itu 'Output'?", "id": "Hasil Keluar Dari Dalam Sistem." },
  { "en": "Apa Itu 'Analog'?", "id": "Sinyal Gelombang Halus Tanpa Putus." },
  { "en": "Apa Itu 'Digital'?", "id": "Sinyal Kotak Putus Putus Biner." },
  { "en": "Apa Itu 'PWM'?", "id": "Atur Terang Lampu Pakai Denyut." },
  { "en": "Apa Itu 'ADC'?", "id": "Ubah Sinyal Sensor Jadi Angka." },
  { "en": "Apa Itu 'Serial Communication'?", "id": "Kirim Data Satu Per Satu." },
  { "en": "Apa Itu 'I2C'?", "id": "Komunikasi Banyak Chip Dua Kabel." },
  { "en": "Apa Itu 'SPI'?", "id": "Komunikasi Chip Cepat Empat Kabel." },
  { "en": "Apa Itu 'Wi-Fi'?", "id": "Jaringan Internet Tanpa Kabel." },
  { "en": "Apa Itu 'Bluetooth'?", "id": "Koneksi Jarak Dekat Hemat Baterai." },
  { "en": "Apa Itu 'Battery Li-Ion'?", "id": "Baterai Isi Ulang Gadget Modern." },
  { "en": "Apa Itu 'Solar Panel'?", "id": "Papan Ubah Matahari Jadi Listrik." },
  { "en": "Apa Itu 'Inverter'?", "id": "Pengubah Aki Jadi Listrik PLN." },
  { "en": "Apa Itu 'Motor DC'?", "id": "Dinamo Putar Biasa Tenaga Baterai." },
  { "en": "Apa Itu 'Servo Motor'?", "id": "Motor Pintar Bisa Diatur Sudutnya." },
  { "en": "Apa Itu 'Stepper Motor'?", "id": "Motor Gerak Langkah Demi Langkah." },
  { "en": "Apa Itu 'Display LCD'?", "id": "Layar Tampilan Huruf Dan Angka." },
  { "en": "Apa Itu 'OLED Screen'?", "id": "Layar Tajam Hemat Daya Kecil." },
  { "en": "Apa Itu 'Seven Segment'?", "id": "Layar Angka Merah Jadul Pom." },
  { "en": "Apa Itu 'Matrix LED'?", "id": "Layar Titik Titik Lampu Jalan." },
  { "en": "Apa Itu 'Breadboard'?", "id": "Papan Rakit Tanpa Solder Praktis." },
  { "en": "Apa Itu 'Jumper Wire'?", "id": "Kabel Tusuk Warni Warni Penghubung." },
  { "en": "Apa Itu 'Terminal Block'?", "id": "Sambungan Kabel Jepit Pakai Obeng." },
  { "en": "Apa Itu 'Push Button'?", "id": "Tombol Tekan Balik Posisi Awal." },
  { "en": "Apa Itu 'Switch'?", "id": "Pemutus Penyambung Aliran Listrik Manual." },
  { "en": "Apa Itu 'Potentiometer'?", "id": "Putaran Pengatur Volume Atau Nilai." },
  { "en": "Apa Itu 'LDR'?", "id": "Sensor Cahaya Gelap Jadi Terang." },
  { "en": "Apa Itu 'Buzzer'?", "id": "Komponen Penghasil Suara Beep Alarm." },
  { "en": "Apa Itu 'Cable Tie'?", "id": "Tali Plastik Pengikat Kabel Rapi." },
  { "en": "Apa Itu 'Heatsink'?", "id": "Logam Sirip Pembuang Panas Chip." },
  { "en": "Apa Itu 'Fan'?", "id": "Kipas Angin Pendingin Alat Elektronik." },
  { "en": "Apa Itu 'Thermal Paste'?", "id": "Krim Penghantar Panas Ke Pendingin." },
  { "en": "Apa Itu 'Box Project'?", "id": "Kotak Wadah Pelindung Alat Elektronik." },
  { "en": "Apa Itu 'Screw'?", "id": "Baut Pengencang Komponen Elektronik Mekanik." },
  { "en": "Apa Itu 'Nut'?", "id": "Mur Pasangan Baut Pengencang Komponen." },
  { "en": "Apa Itu 'Flux Pen'?", "id": "Spidol Pembersih Kaki Komponen Solder." },
  { "en": "Apa Itu 'Solder Wick'?", "id": "Pita Tembaga Isap Timah Cair." },
  { "en": "Apa Itu 'Solder Mat'?", "id": "Alas Meja Tahan Panas Solder." },
  { "en": "Apa Itu 'Magnifying Glass'?", "id": "Kaca Pembesar Lihat Jalur Kecil." },
  { "en": "Apa Itu 'Third Hand'?", "id": "Alat Jepit PCB Saat Nyolder." },
  { "en": "Apa Itu 'Multimeter Probe'?", "id": "Kabel Jarum Merah Hitam Tester." },
  { "en": "Apa Itu 'Signal Generator'?", "id": "Pembuat Gelombang Sinyal Uji Coba." },
  { "en": "Apa Itu 'Cutter'?", "id": "Pisau Potong Jalur Atau Kabel." },
  { "en": "Apa Itu 'Caliper'?", "id": "Jangka Sorong Ukur Tebal Komponen." },
  { "en": "Apa Itu 'Datasheet'?", "id": "Buku Manual Spesifikasi Komponen Pabrik." },
  { "en": "Apa Itu 'Schematic'?", "id": "Peta Gambar Jalur Rangkaian Listrik." },
  { "en": "Apa Itu 'Firmware'?", "id": "Program Tetap Di Dalam Chip." },
  { "en": "Apa Itu 'Bootloader'?", "id": "Program Awal Untuk Menerima Kode." },
  { "en": "Apa Itu 'Debugging'?", "id": "Proses Cari Dan Perbaiki Error." },
  { "en": "Apa Itu 'Compile'?", "id": "Ubah Kode Tulisan Jadi Mesin." },
  { "en": "Apa Itu 'Upload'?", "id": "Kirim Program Ke Dalam Chip." },
  { "en": "Apa Itu 'Library'?", "id": "Kumpulan Kode Siap Pakai Praktis." },
  { "en": "Apa Itu 'Variable'?", "id": "Wadah Simpan Data Dalam Program." },
  { "en": "Apa Itu 'Loop'?", "id": "Perintah Ulang Terus Tanpa Henti." },
  { "en": "Apa Itu 'If-Else'?", "id": "Logika Keputusan Jika Maka Terjadi." },
  { "en": "Apa Itu 'IoT' (Internet of Things)?", "id": "Benda Benda Terhubung Ke Internet." },
  { "en": "Apa Itu 'Smart Home'?", "id": "Rumah Pintar Kendali Lewat HP." },
  { "en": "Apa Itu 'Robot'?", "id": "Mesin Otomatis Yang Bisa Bergerak." },
  { "en": "Apa Itu 'Drone'?", "id": "Pesawat Terbang Tanpa Pilot Didalamnya." },
  { "en": "Apa Itu '3D Printer'?", "id": "Mesin Cetak Benda Tiga Dimensi." },
  { "en": "Apa Itu 'Laser Cutter'?", "id": "Mesin Potong Benda Pakai Cahaya." },
  { "en": "Apa Itu 'CNC Machine'?", "id": "Mesin Potong Otomatis Kendali Komputer." },
  { "en": "Apa Itu 'Parity Bit'?", "id": "Bit Tambahan Cek Error Pengiriman Data." },
  { "en": "Apa Itu 'Stop Bit'?", "id": "Tanda Akhir Paket Data Komunikasi Serial." },
  { "en": "Apa Itu 'Flow Control'?", "id": "Pengatur Lalu Lintas Data Agar Lancar." },
  { "en": "Apa Itu 'Half Duplex'?", "id": "Komunikasi Dua Arah Tapi Gantian Bicara." },
  { "en": "Apa Itu 'Full Duplex'?", "id": "Komunikasi Dua Arah Bisa Bicara Bersamaan." },
  { "en": "Apa Itu 'Simplex'?", "id": "Komunikasi Satu Arah Saja Tanpa Balasan." },
  { "en": "Apa Itu 'Interrupt Vector'?", "id": "Alamat Memori Tujuan Saat Ada Interupsi." },
  { "en": "Apa Itu 'Stack Pointer'?", "id": "Penunjuk Alamat Memori Tumpukan Data Sementara." },
  { "en": "Apa Itu 'Program Counter'?", "id": "Penunjuk Alamat Perintah Selanjutnya Yang Dijalankan." },
  { "en": "Apa Itu 'Register'?", "id": "Memori Kecil Sangat Cepat Di Prosesor." },
  { "en": "Apa Itu 'Accumulator'?", "id": "Register Utama Penyimpan Hasil Hitungan Prosesor." },
  { "en": "Apa Itu 'Buffer'?", "id": "Tempat Parkir Data Sementara Sebelum Diproses." },
  { "en": "Apa Itu 'Cache'?", "id": "Memori Kilat Penyimpan Data Sering Dipakai." },
  { "en": "Apa Itu 'Latency'?", "id": "Waktu Tunda Pengiriman Data Sampai Tujuan." },
  { "en": "Apa Itu 'Jitter'?", "id": "Ketidakstabilan Waktu Kedatangan Paket Data Digital." },
  { "en": "Apa Itu 'Bandwidth'?", "id": "Lebar Jalan Raya Untuk Lalu Lintas Data." },
  { "en": "Apa Itu 'Throughput'?", "id": "Jumlah Data Sukses Terkirim Per Detik." },
  { "en": "Apa Itu 'EMI' (Electromagnetic Interference)?", "id": "Gangguan Sinyal Akibat Medan Magnet Luar." },
  { "en": "Apa Itu 'RFI' (Radio Frequency Interference)?", "id": "Gangguan Sinyal Akibat Gelombang Radio Liar." },
  { "en": "Apa Itu 'Faraday Cage'?", "id": "Kurungan Logam Penahan Sinyal Elektromagnetik." },
  { "en": "Apa Itu 'Galvanic Isolation'?", "id": "Pemisahan Jalur Listrik Demi Keamanan Alat." },
  { "en": "Apa Itu 'Ground Loop'?", "id": "Arus Putar Tanah Bikin Dengung Audio." },
  { "en": "Apa Itu 'Common Mode Noise'?", "id": "Gangguan Sinyal Muncul Di Kedua Kabel." },
  { "en": "Apa Itu 'Differential Mode Noise'?", "id": "Gangguan Sinyal Beda Arah Di Kabel." },
  { "en": "Apa Itu 'Signal Integrity'?", "id": "Kualitas Sinyal Digital Tetap Bagus Utuh." },
  { "en": "Apa Itu 'Crosstalk'?", "id": "Kebocoran Sinyal Antar Kabel Yang Berdekatan." },
  { "en": "Apa Itu 'Attenuation'?", "id": "Pelemahan Sinyal Karena Jarak Kabel Jauh." },
  { "en": "Apa Itu 'Impedance Mismatch'?", "id": "Hambatan Tidak Cocok Bikin Sinyal Memantul." },
  { "en": "Apa Itu 'Reflection Coefficient'?", "id": "Ukuran Pantulan Sinyal Balik Ke Sumber." },
  { "en": "Apa Itu 'Standing Wave'?", "id": "Gelombang Diam Akibat Pantulan Sinyal Buruk." },
  { "en": "Apa Itu 'Skin Effect'?", "id": "Arus Listrik AC Lewat Kulit Kabel." },
  { "en": "Apa Itu 'Proximity Effect'?", "id": "Arus Terdesak Akibat Kabel Lain Dekat." },
  { "en": "Apa Itu 'Dielectric Loss'?", "id": "Energi Hilang Jadi Panas Di Isolator." },
  { "en": "Apa Itu 'Hysteresis Loss'?", "id": "Energi Hilang Akibat Gesekan Magnetik Inti." },
  { "en": "Apa Itu 'Eddy Current'?", "id": "Arus Liar Berputar Di Inti Besi." },
  { "en": "Apa Itu 'Transient Voltage'?", "id": "Lonjakan Tegangan Sangat Cepat Dan Singkat." },
  { "en": "Apa Itu 'Surge Current'?", "id": "Lonjakan Arus Besar Tiba Tiba Masuk." },
  { "en": "Apa Itu 'Voltage Sag'?", "id": "Tegangan Turun Sesaat Ganggu Kerja Mesin." },
  { "en": "Apa Itu 'Voltage Swell'?", "id": "Tegangan Naik Sesaat Bahayakan Alat Elektronik." },
  { "en": "Apa Itu 'Brownout'?", "id": "Tegangan Listrik Turun Redup Cukup Lama." },
  { "en": "Apa Itu 'Blackout'?", "id": "Mati Listrik Total Padam Satu Wilayah." },
  { "en": "Apa Itu 'Phase Unbalance'?", "id": "Beban Tiga Fasa Tidak Rata Seimbang." },
  { "en": "Apa Itu 'Neutral Current'?", "id": "Arus Mengalir Di Netral Akibat Ketidakseimbangan." },
  { "en": "Apa Itu 'Ground Fault'?", "id": "Kabel Fasa Bocor Kena Body Logam." },
  { "en": "Apa Itu 'Arc Flash'?", "id": "Ledakan Bunga Api Listrik Melalui Udara." },
  { "en": "Apa Itu 'Corona Discharge'?", "id": "Desis Listrik Bocor Di Kabel Tegangan Tinggi." },
  { "en": "Apa Itu 'Breakdown Voltage'?", "id": "Batas Tegangan Tembus Bahan Isolator Listrik." },
  { "en": "Apa Itu 'Leakage Current'?", "id": "Arus Kecil Bocor Lewat Isolator Kapasitor." },
  { "en": "Apa Itu 'Thermal Runaway'?", "id": "Panas Menghasilkan Panas Lebih Hingga Rusak." },
  { "en": "Apa Itu 'De-rating'?", "id": "Menurunkan Batas Kerja Agar Alat Awet." },
  { "en": "Apa Itu 'FPGA'?", "id": "Chip Yang Bisa Diprogram Ulang Hardwarenya." },
  { "en": "Apa Itu 'DSP' (Digital Signal Processor)?", "id": "Chip Khusus Olah Suara Dan Video." },
  { "en": "Apa Itu 'ASIC'?", "id": "Chip Khusus Dibuat Untuk Satu Tugas." },
  { "en": "Apa Itu 'SoC' (System on Chip)?", "id": "Satu Chip Isinya Komputer Lengkap." },
  { "en": "Apa Itu 'GPU'?", "id": "Chip Khusus Olah Gambar Grafis Berat." },
  { "en": "Apa Itu 'CPU Clock Speed'?", "id": "Kecepatan Pikir Chip Prosesor Per Detik." },
  { "en": "Apa Itu 'Overclocking'?", "id": "Memaksa Chip Kerja Lebih Cepat." },
  { "en": "Apa Itu 'Thermal Throttling'?", "id": "Chip Melambat Otomatis Karena Kepanasan." },
  { "en": "Apa Itu 'Solder Bridge'?", "id": "Timah Nyambung Tidak Sengaja Bikin Korslet." },
  { "en": "Apa Itu 'Tombstoning'?", "id": "Komponen Chip Berdiri Saat Disolder Oven." },
  { "en": "Apa Itu 'Cold Solder Joint'?", "id": "Solderan Mentah Kusam Gampang Lepas." },
  { "en": "Apa Itu 'Dry Joint'?", "id": "Solderan Kering Tidak Nempel Kaki Komponen." },
  { "en": "Apa Itu 'Pad Lifting'?", "id": "Jalur Tembaga Copot Dari Papan PCB." },
  { "en": "Apa Itu 'Trace Broken'?", "id": "Jalur Tembaga Putus Arus Tidak Mengalir." },
  { "en": "Apa Itu 'Delamination'?", "id": "Lapisan PCB Mengelupas Karena Terlalu Panas." },
  { "en": "Apa Itu 'Conformal Coating'?", "id": "Cat Pelindung PCB Dari Karat Air." },
  { "en": "Apa Itu 'Potting'?", "id": "Mengecor Alat Elektronik Dengan Resin Keras." },
  { "en": "Apa Itu 'Wafer Silicon'?", "id": "Piringan Bahan Baku Pembuat Chip Prosesor." },
  { "en": "Apa Itu 'Die' (Semikonduktor)?", "id": "Potongan Kecil Inti Silikon Chip." },
  { "en": "Apa Itu 'Wire Bonding'?", "id": "Kabel Emas Tipis Di Dalam Chip." },
  { "en": "Apa Itu 'Lead Frame'?", "id": "Kaki Logam Penghubung Inti Chip Keluar." },
  { "en": "Apa Itu 'Magnet Wire'?", "id": "Kawat Tembaga Lapis Email Untuk Trafo." },
  { "en": "Apa Itu 'Litz Wire'?", "id": "Kawat Serabut Anyam Tahan Frekuensi Tinggi." },
  { "en": "Apa Itu 'Bus Wire'?", "id": "Kawat Tembaga Telanjang Tanpa Kulit Isolasi." },
  { "en": "Apa Itu 'Fusion Splicer'?", "id": "Mesin Las Penyambung Kabel Serat Optik." },
  { "en": "Apa Itu 'Fiber Cleaver'?", "id": "Alat Potong Kaca Optik Sangat Rata." },
  { "en": "Apa Itu 'Optical Power Meter'?", "id": "Alat Ukur Kekuatan Cahaya Sinyal Optik." },
  { "en": "Apa Itu 'Visual Fault Locator'?", "id": "Laser Merah Cek Kabel Optik Putus." },
  { "en": "Apa Itu 'Media Converter'?", "id": "Pengubah Sinyal Listrik Jadi Sinyal Cahaya." },
  { "en": "Apa Itu 'SFP Module'?", "id": "Modul Colokan Fiber Optik Di Router." },
  { "en": "Apa Itu 'Patch Cord'?", "id": "Kabel Optik Pendek Penghubung Perangkat Jaringan." },
  { "en": "Apa Itu 'Pigtail'?", "id": "Kabel Optik Buntung Satu Konektor Saja." },
  { "en": "Apa Itu 'Rectifier Bridge'?", "id": "Empat Dioda Penyearah Dalam Satu Kemasan." },
  { "en": "Apa Itu 'Voltage Doubler'?", "id": "Rangkaian Pengganda Tegangan Pakai Kapasitor." },
  { "en": "Apa Itu 'Clipper Circuit'?", "id": "Rangkaian Pemotong Puncak Sinyal Gelombang." },
  { "en": "Apa Itu 'Clamper Circuit'?", "id": "Rangkaian Penggeser Posisi Tegangan Sinyal." },
  { "en": "Apa Itu 'Peak Detector'?", "id": "Rangkaian Penyimpan Nilai Puncak Sinyal Tertinggi." },
  { "en": "Apa Itu 'Zero Crossing Detector'?", "id": "Pendeteksi Sinyal Saat Lewat Titik Nol." },
  { "en": "Apa Itu 'Sample and Hold'?", "id": "Rangkaian Penahan Nilai Sinyal Sesaat." },
  { "en": "Apa Itu 'Phase Locked Loop' (PLL)?", "id": "Sistem Pengunci Frekuensi Agar Sangat Stabil." },
  { "en": "Apa Itu 'Voltage Controlled Oscillator'?", "id": "Pembangkit Frekuensi Diatur Oleh Tegangan Masuk." },
  { "en": "Apa Itu 'Mixer RF'?", "id": "Pencampur Dua Frekuensi Sinyal Radio." },
  { "en": "Apa Itu 'Dummy Load'?", "id": "Beban Palsu Untuk Tes Pemancar Radio." },
  { "en": "Apa Itu 'Balun' (Balance-Unbalance)?", "id": "Penyesuai Antena Kabel Seimbang Dan Tidak." },
  { "en": "Apa Itu 'Impedance 50 Ohm'?", "id": "Standar Hambatan Kabel Radio Dan Antena." },
  { "en": "Apa Itu 'Impedance 75 Ohm'?", "id": "Standar Hambatan Kabel Antena TV." },
  { "en": "Apa Itu 'Gain Antenna' (dBi)?", "id": "Kekuatan Antena Memfokuskan Arah Sinyal." },
  { "en": "Apa Itu 'Radiation Pattern'?", "id": "Gambar Pola Pancaran Sinyal Antena." },
  { "en": "Apa Itu 'Beamwidth'?", "id": "Lebar Sudut Pancaran Utama Antena." },
  { "en": "Apa Itu 'Front-to-Back Ratio'?", "id": "Perbandingan Sinyal Depan Dan Belakang Antena." },
  { "en": "Apa Itu 'Circuit Symbol'?", "id": "Gambar Lambang Komponen Di Peta Rangkaian." },
  { "en": "Apa Itu 'Symbol Resistor'?", "id": "Gambar Garis Zigzag Atau Kotak Panjang." },
  { "en": "Apa Itu 'Symbol Capacitor'?", "id": "Gambar Dua Garis Sejajar Terpisah Celah." },
  { "en": "Apa Itu 'Symbol Diode'?", "id": "Gambar Panah Nabrak Garis Satu Arah." },
  { "en": "Apa Itu 'Symbol Ground'?", "id": "Gambar Tiga Garis Makin Kecil Bawah." },
  { "en": "Apa Itu 'Symbol Battery'?", "id": "Gambar Garis Panjang Dan Pendek Berjejer." },
  { "en": "Apa Itu 'Symbol Fuse'?", "id": "Gambar Gelombang Di Dalam Kotak Persegi." },
  { "en": "Apa Itu 'Symbol Switch'?", "id": "Gambar Dua Titik Dengan Garis Terbuka." },
  { "en": "Apa Itu 'Symbol Lamp'?", "id": "Gambar Lingkaran Ada Silang Di Tengah." },
  { "en": "Apa Itu 'Symbol Inductor'?", "id": "Gambar Garis Melengkung Lengkung Seperti Per." },
  { "en": "Apa Itu 'Node' (Rangkaian)?", "id": "Titik Pertemuan Tiga Kabel Atau Lebih." },
  { "en": "Apa Itu 'Loop' (Rangkaian)?", "id": "Lintasan Arus Tertutup Kembali Ke Awal." },
  { "en": "Apa Itu 'Branch' (Cabang)?", "id": "Jalur Kabel Di Antara Dua Titik." },
  { "en": "Apa Itu 'Mesh'?", "id": "Loop Paling Kecil Tanpa Loop Lain." },
  { "en": "Apa Itu 'Series Circuit'?", "id": "Rangkaian Satu Jalur Arus Tidak Terbagi." },
  { "en": "Apa Itu 'Parallel Circuit'?", "id": "Rangkaian Banyak Jalur Tegangan Sama Rata." },
  { "en": "Apa Itu 'Short to Ground'?", "id": "Kabel Positif Tak Sengaja Kena Tanah." },
  { "en": "Apa Itu 'Voltage Source'?", "id": "Sumber Tegangan Seperti Baterai Atau Adaptor." },
  { "en": "Apa Itu 'Current Source'?", "id": "Sumber Arus Stabil Tidak Peduli Tegangan." },
  { "en": "Apa Itu 'Dependent Source'?", "id": "Sumber Listrik Dikendalikan Oleh Sinyal Lain." },
  { "en": "Apa Itu 'Independent Source'?", "id": "Sumber Listrik Tetap Tidak Terpengaruh Lain." },
  { "en": "Apa Itu 'Ideal Source'?", "id": "Sumber Sempurna Tanpa Hambatan Di Dalam." },
  { "en": "Apa Itu 'Internal Resistance'?", "id": "Hambatan Alami Di Dalam Baterai Nyata." },
  { "en": "Apa Itu 'Voltage Drop'?", "id": "Tegangan Berkurang Setelah Lewat Komponen." },
  { "en": "Apa Itu 'Power Formula' (P=VI)?", "id": "Rumus Daya Adalah Tegangan Kali Arus." },
  { "en": "Apa Itu 'Ohm's Law' (V=IR)?", "id": "Tegangan Adalah Arus Kali Hambatan." },
  { "en": "Apa Itu 'Loading Effect'?", "id": "Tegangan Turun Saat Beban Dipasang." },
  { "en": "Apa Itu 'High Impedance' (Hi-Z)?", "id": "Kondisi Kaki Chip Seolah Olah Putus." },
  { "en": "Apa Itu 'Logic High'?", "id": "Kondisi Sinyal Satu Atau Tegangan Positif." },
  { "en": "Apa Itu 'Logic Low'?", "id": "Kondisi Sinyal Nol Atau Tegangan Tanah." },
  { "en": "Apa Itu 'Logic Threshold'?", "id": "Batas Tegangan Penentu Logika Satu Nol." },
  { "en": "Apa Itu 'Floating Input'?", "id": "Kaki Masukan Tidak Terhubung Kemana Mana." },
  { "en": "Apa Itu 'Pull-up Resistor'?", "id": "Resistor Penjaga Logika Agar Tetap Tinggi." },
  { "en": "Apa Itu 'Pull-down Resistor'?", "id": "Resistor Penjaga Logika Agar Tetap Rendah." },
  { "en": "Apa Itu 'Rise Time'?", "id": "Waktu Sinyal Naik Dari Mati Nyala." },
  { "en": "Apa Itu 'Fall Time'?", "id": "Waktu Sinyal Turun Dari Nyala Mati." },
  { "en": "Apa Itu 'Propagation Delay'?", "id": "Waktu Tunda Sinyal Lewati Gerbang Logika." },
  { "en": "Apa Itu 'Glitch'?", "id": "Gangguan Sinyal Tajam Sangat Singkat." },
  { "en": "Apa Itu 'Debounce'?", "id": "Teknik Hilangkan Sinyal Getar Tombol Mekanik." },
  { "en": "Apa Itu 'Latching'?", "id": "Sifat Menahan Keadaan Terakhir Walau Dilepas." },
  { "en": "Apa Itu 'Momentary'?", "id": "Hanya Aktif Saat Tombol Sedang Ditekan." },
  { "en": "Apa Itu 'Toggle'?", "id": "Berubah Kondisi Sebaliknya Tiap Kali Ditekan." },
  { "en": "Apa Itu 'Enable Pin'?", "id": "Kaki Mengizinkan Chip Untuk Mulai Bekerja." },
  { "en": "Apa Itu 'Disable Pin'?", "id": "Kaki Mematikan Fungsi Chip Sementara Waktu." },
  { "en": "Apa Itu 'Power Good Signal'?", "id": "Sinyal Tanda Tegangan Listrik Sudah Stabil." },
  { "en": "Apa Itu 'Soft Start'?", "id": "Mencegah Lonjakan Arus Saat Awal Nyala." },
  { "en": "Apa Itu 'Hard Reset'?", "id": "Mematikan Paksa Alat Lalu Nyalakan Lagi." },
  { "en": "Apa Itu 'Soft Reset'?", "id": "Mulai Ulang Program Tanpa Matikan Listrik." },
  { "en": "Apa Itu 'Factory Reset'?", "id": "Kembali Ke Pengaturan Awal Dari Pabrik." },
  { "en": "Apa Itu 'Calibration'?", "id": "Menyesuaikan Alat Ukur Agar Tepat Akurat." },
  { "en": "Apa Itu 'Voltage Divider'?", "id": "Rangkaian Penurun Tegangan Pakai Dua Resistor." },
  { "en": "Apa Itu 'Current Limiter'?", "id": "Rangkaian Pembatas Arus Agar Alat Aman." },
  { "en": "Apa Itu 'Bleeder Resistor'?", "id": "Resistor Penguras Sisa Listrik Kapasitor." },
  { "en": "Apa Itu 'Pull-Up Resistor'?", "id": "Resistor Penjaga Sinyal Tetap Positif." },
  { "en": "Apa Itu 'Pull-Down Resistor'?", "id": "Resistor Penjaga Sinyal Tetap Negatif." },
  { "en": "Apa Itu 'Decoupling Capacitor'?", "id": "Kapasitor Penstabil Tegangan Dekat Chip." },
  { "en": "Apa Itu 'Filter Capacitor'?", "id": "Kapasitor Perata Tegangan DC Bergelombang." },
  { "en": "Apa Itu 'Bypass Capacitor'?", "id": "Kapasitor Pembuang Gangguan Ke Tanah." },
  { "en": "Apa Itu 'Flyback Diode'?", "id": "Dioda Pengaman Arus Balik Kumparan." },
  { "en": "Apa Itu 'Zener Diode'?", "id": "Dioda Penstabil Tegangan Balik Tertentu." },
  { "en": "Apa Itu 'Schottky Diode'?", "id": "Dioda Sakelar Cepat Tegangan Jatuh Rendah." },
  { "en": "Apa Itu 'Bridge Rectifier'?", "id": "Empat Dioda Penyearah Gelombang Penuh." },
  { "en": "Apa Itu 'Transistor Base'?", "id": "Kaki Pemicu Arus Pada Transistor." },
  { "en": "Apa Itu 'Transistor Emitter'?", "id": "Kaki Pembuang Arus Pada Transistor." },
  { "en": "Apa Itu 'Transistor Collector'?", "id": "Kaki Penampung Arus Pada Transistor." },
  { "en": "Apa Itu 'Mosfet Gate'?", "id": "Kaki Pemicu Tegangan Pada Mosfet." },
  { "en": "Apa Itu 'Mosfet Drain'?", "id": "Kaki Masuk Arus Utama Mosfet." },
  { "en": "Apa Itu 'Mosfet Source'?", "id": "Kaki Keluar Arus Utama Mosfet." },
  { "en": "Apa Itu 'Insulator'?", "id": "Bahan Yang Tidak Bisa Alirkan Listrik." },
  { "en": "Apa Itu 'Conductor'?", "id": "Bahan Yang Mudah Alirkan Listrik." },
  { "en": "Apa Itu 'Semiconductor'?", "id": "Bahan Setengah Penghantar Pembuat Chip." },
  { "en": "Apa Itu 'Superconductor'?", "id": "Bahan Tanpa Hambatan Listrik Sama Sekali." },
  { "en": "Apa Itu 'Resistivity'?", "id": "Tingkat Sulitnya Bahan Alirkan Listrik." },
  { "en": "Apa Itu 'Conductivity'?", "id": "Tingkat Mudahnya Bahan Alirkan Listrik." },
  { "en": "Apa Itu 'Soldering Iron'?", "id": "Alat Pemanas Peleleh Timah Solder." },
  { "en": "Apa Itu 'Solder Wire'?", "id": "Kawat Timah Untuk Menyambung Komponen." },
  { "en": "Apa Itu 'Flux'?", "id": "Cairan Pembersih Agar Timah Menempel." },
  { "en": "Apa Itu 'Desoldering Pump'?", "id": "Alat Sedot Timah Solder Cair." },
  { "en": "Apa Itu 'Solder Wick'?", "id": "Pita Tembaga Isap Sisa Timah." },
  { "en": "Apa Itu 'Multimeter'?", "id": "Alat Ukur Tegangan Arus Hambatan." },
  { "en": "Apa Itu 'Oscilloscope'?", "id": "Alat Lihat Bentuk Gelombang Listrik." },
  { "en": "Apa Itu 'Power Supply'?", "id": "Sumber Listrik Untuk Menyalakan Alat." },
  { "en": "Apa Itu 'Function Generator'?", "id": "Alat Pembuat Sinyal Gelombang Uji." },
  { "en": "Apa Itu 'Breadboard'?", "id": "Papan Rakit Tanpa Perlu Solder." },
  { "en": "Apa Itu 'PCB'?", "id": "Papan Hijau Tempat Komponen Disolder." },
  { "en": "Apa Itu 'Datasheet'?", "id": "Buku Spesifikasi Teknis Komponen Elektronik." },
  { "en": "Apa Itu 'Schematic'?", "id": "Gambar Peta Jalur Rangkaian Listrik." },
  { "en": "Apa Itu 'Microcontroller'?", "id": "Komputer Kecil Pengendali Alat Elektronik." },
  { "en": "Apa Itu 'Sensor'?", "id": "Alat Pengubah Fisik Jadi Listrik." },
  { "en": "Apa Itu 'Actuator'?", "id": "Alat Pengubah Listrik Jadi Gerakan." },
  { "en": "Apa Itu 'Transducer'?", "id": "Pengubah Satu Energi Ke Energi Lain." },
  { "en": "Apa Itu 'Analog Signal'?", "id": "Sinyal Gelombang Sambung Tanpa Putus." },
  { "en": "Apa Itu 'Digital Signal'?", "id": "Sinyal Putus Putus Angka Biner." },
  { "en": "Apa Itu 'PWM Signal'?", "id": "Sinyal Kotak Pengatur Daya Lampu." },
  { "en": "Apa Itu 'SPST Switch'?", "id": "Sakelar Satu Jalur Satu Arah." },
  { "en": "Apa Itu 'SPDT Switch'?", "id": "Sakelar Satu Jalur Dua Arah." },
  { "en": "Apa Itu 'DPDT Switch'?", "id": "Sakelar Dua Jalur Dua Arah." },
  { "en": "Apa Itu 'Push Button NO'?", "id": "Tombol Tekan Nyala Saat Ditekan." },
  { "en": "Apa Itu 'Push Button NC'?", "id": "Tombol Tekan Mati Saat Ditekan." },
  { "en": "Apa Itu 'Limit Switch'?", "id": "Sakelar Pembatas Gerakan Mesin Mekanik." },
  { "en": "Apa Itu 'Reed Switch'?", "id": "Sakelar Kaca Aktif Kena Magnet." },
  { "en": "Apa Itu 'DIP Switch'?", "id": "Barisan Sakelar Kecil Pada PCB." },
  { "en": "Apa Itu 'Seven Segment'?", "id": "Layar Angka Delapan Garis Merah." },
  { "en": "Apa Itu 'Dot Matrix'?", "id": "Layar Titik Titik Lampu Led." },
  { "en": "Apa Itu 'LCD 16x2'?", "id": "Layar Teks Dua Baris Hijau." },
  { "en": "Apa Itu 'OLED Display'?", "id": "Layar Grafis Tajam Hemat Listrik." },
  { "en": "Apa Itu 'Active Buzzer'?", "id": "Penghasil Suara Beep Langsung Nyala." },
  { "en": "Apa Itu 'Passive Buzzer'?", "id": "Penghasil Suara Butuh Sinyal Nada." },
  { "en": "Apa Itu 'Condenser Mic'?", "id": "Mic Peka Butuh Daya Tambahan." },
  { "en": "Apa Itu 'Dynamic Mic'?", "id": "Mic Panggung Tanpa Daya Tambahan." },
  { "en": "Apa Itu 'Speaker Impedance'?", "id": "Hambatan Koil Speaker Dalam Ohm." },
  { "en": "Apa Itu 'Speaker Wattage'?", "id": "Kekuatan Maksimal Speaker Menahan Daya." },
  { "en": "Apa Itu 'Amplifier Gain'?", "id": "Nilai Penguatan Sinyal Suara Masuk." },
  { "en": "Apa Itu 'Tone Control'?", "id": "Pengatur Nada Bass Dan Treble." },
  { "en": "Apa Itu 'Equalizer'?", "id": "Pengatur Detail Banyak Frekuensi Suara." },
  { "en": "Apa Itu 'Mono Audio'?", "id": "Suara Satu Saluran Kiri Kanan." },
  { "en": "Apa Itu 'Stereo Audio'?", "id": "Suara Dua Saluran Kiri Kanan." },
  { "en": "Apa Itu 'Surround Sound'?", "id": "Suara Keliling Seperti Di Bioskop." },
  { "en": "Apa Itu 'RCA Cable'?", "id": "Kabel Merah Putih Kuning Audio." },
  { "en": "Apa Itu 'Jack 3.5mm'?", "id": "Colokan Headset HP Lubang Kecil." },
  { "en": "Apa Itu 'Jack 6.5mm'?", "id": "Colokan Gitar Listrik Lubang Besar." },
  { "en": "Apa Itu 'XLR Connector'?", "id": "Colokan Mic Profesional Tiga Kaki." },
  { "en": "Apa Itu 'Binding Post'?", "id": "Terminal Jepit Kabel Speaker Belakang." },
  { "en": "Apa Itu 'Banana Plug'?", "id": "Colokan Kabel Speaker Praktis Cabut." },
  { "en": "Apa Itu 'Spade Lug'?", "id": "Sepatu Kabel Bentuk Garpu U." },
  { "en": "Apa Itu 'Ring Terminal'?", "id": "Sepatu Kabel Bentuk Cincin Bulat." },
  { "en": "Apa Itu 'Ferrule Terminal'?", "id": "Selongsong Besi Ujung Kabel Serabut." },
  { "en": "Apa Itu 'Crimping Tool'?", "id": "Tang Press Perekat Sepatu Kabel." },
  { "en": "Apa Itu 'Wire Stripper'?", "id": "Tang Kupas Kulit Kabel Otomatis." },
  { "en": "Apa Itu 'Heat Gun'?", "id": "Pemanas Selang Bakar Pelindung Kabel." },
  { "en": "Apa Itu 'Cable Gland'?", "id": "Pengunci Kabel Masuk Box Panel." },
  { "en": "Apa Itu 'Spiral Wrap'?", "id": "Plastik Pelindung Kabel Agar Rapi." },
  { "en": "Apa Itu 'Cable Tray'?", "id": "Rak Besi Penyangga Jalur Kabel." },
  { "en": "Apa Itu 'Conduit Pipe'?", "id": "Pipa Pelindung Kabel Di Tembok." },
  { "en": "Apa Itu 'Junction Box'?", "id": "Kotak Cabang Sambungan Kabel Listrik." },
  { "en": "Apa Itu 'Distribution Board'?", "id": "Panel Pembagi Listrik Ke Ruangan." },
  { "en": "Apa Itu 'MCB 10A'?", "id": "Sekring Pembatas Arus Rumah Biasa." },
  { "en": "Apa Itu 'ELCB 30mA'?", "id": "Pengaman Anti Setrum Manusia Sensitif." },
  { "en": "Apa Itu 'Ground Rod'?", "id": "Batang Tembaga Tanam Ke Tanah." },
  { "en": "Apa Itu 'Lightning Arrester'?", "id": "Penangkal Petir Lindungi Alat Listrik." },
  { "en": "Apa Itu 'Surge Protector'?", "id": "Pelindung Dari Lonjakan Listrik Tiba." },
  { "en": "Apa Itu 'Phase Wire'?", "id": "Kabel Fasa Yang Ada Setrumnya." },
  { "en": "Apa Itu 'Neutral Wire'?", "id": "Kabel Netral Tidak Ada Setrumnya." },
  { "en": "Apa Itu 'Earth Wire'?", "id": "Kabel Grounding Pembuang Arus Bocor." },
  { "en": "Apa Itu 'AM Radio'?", "id": "Radio Modulasi Tinggi Rendah Gelombang." },
  { "en": "Apa Itu 'FM Radio'?", "id": "Radio Modulasi Rapat Renggang Gelombang." },
  { "en": "Apa Itu 'Carrier Wave'?", "id": "Gelombang Penumpang Sinyal Informasi Radio." },
  { "en": "Apa Itu 'Bandwidth'?", "id": "Lebar Jalur Frekuensi Yang Digunakan." },
  { "en": "Apa Itu 'Resonance'?", "id": "Getaran Paling Kuat Pada Frekuensi Pas." },
  { "en": "Apa Itu 'High Pass Filter'?", "id": "Penyaring Loloskan Nada Tinggi Saja." },
  { "en": "Apa Itu 'Low Pass Filter'?", "id": "Penyaring Loloskan Nada Rendah Saja." },
  { "en": "Apa Itu 'Band Pass Filter'?", "id": "Penyaring Loloskan Nada Tengah Saja." },
  { "en": "Apa Itu 'Op-Amp Inverting'?", "id": "Penguat Sinyal Dengan Fasa Terbalik." },
  { "en": "Apa Itu 'Op-Amp Non-Inverting'?", "id": "Penguat Sinyal Tanpa Membalik Fasa." },
  { "en": "Apa Itu 'Comparator'?", "id": "Pembanding Dua Nilai Tegangan Masukan." },
  { "en": "Apa Itu 'Voltage Follower'?", "id": "Penyangga Tegangan Agar Arus Kuat." },
  { "en": "Apa Itu 'ADC' (Analog to Digital)?", "id": "Pengubah Sinyal Gelombang Jadi Angka." },
  { "en": "Apa Itu 'DAC' (Digital to Analog)?", "id": "Pengubah Data Angka Jadi Gelombang." },
  { "en": "Apa Itu 'Sampling Rate'?", "id": "Kecepatan Ambil Sampel Data Sinyal." },
  { "en": "Apa Itu 'Resolution Bit'?", "id": "Tingkat Ketelitian Nilai Data Digital." },
  { "en": "Apa Itu 'Bluetooth'?", "id": "Koneksi Nirkabel Jarak Dekat Hemat." },
  { "en": "Apa Itu 'Wi-Fi'?", "id": "Jaringan Internet Cepat Tanpa Kabel." },
  { "en": "Apa Itu 'Zigbee'?", "id": "Jaringan Radio Pintar Hemat Energi." },
  { "en": "Apa Itu 'LoRa'?", "id": "Radio Data Jarak Jauh Daya Rendah." },
  { "en": "Apa Itu 'RFID'?", "id": "Identifikasi Barang Pakai Gelombang Radio." },
  { "en": "Apa Itu 'NFC'?", "id": "Komunikasi Tempel Jarak Sangat Dekat." },
  { "en": "Apa Itu 'GPS'?", "id": "Penentu Lokasi Lewat Sinyal Satelit." },
  { "en": "Apa Itu 'GSM'?", "id": "Jaringan Seluler Untuk Telepon HP." },
  { "en": "Apa Itu 'Li-Ion Battery'?", "id": "Baterai Tabung Umum Laptop Senter." },
  { "en": "Apa Itu 'Li-Po Battery'?", "id": "Baterai Gepeng Ringan Drone HP." },
  { "en": "Apa Itu 'NiMH Battery'?", "id": "Baterai Isi Ulang Pengganti Biasa." },
  { "en": "Apa Itu 'Lead Acid Battery'?", "id": "Baterai Basah Berat Untuk Mobil." },
  { "en": "Apa Itu 'BMS' (Battery Management)?", "id": "Papan Pengaman Baterai Biar Awet." },
  { "en": "Apa Itu 'PWM Solar Controller'?", "id": "Pengisi Aki Surya Cara Denyut." },
  { "en": "Apa Itu 'MPPT Solar Controller'?", "id": "Pengisi Aki Surya Paling Efisien." },
  { "en": "Apa Itu 'Pure Sine Inverter'?", "id": "Pengubah Listrik Halus Seperti PLN." },
  { "en": "Apa Itu 'Modified Sine Inverter'?", "id": "Pengubah Listrik Kasar Kotak Kotak." },
  { "en": "Apa Itu 'Relay SPST'?", "id": "Sakelar Satu Jalur Satu Arah." },
  { "en": "Apa Itu 'Relay SPDT'?", "id": "Sakelar Satu Jalur Dua Arah." },
  { "en": "Apa Itu 'Solid State Relay'?", "id": "Sakelar Elektronik Awet Tanpa Bunyi." },
  { "en": "Apa Itu 'Contactor'?", "id": "Sakelar Magnet Besar Listrik Industri." },
  { "en": "Apa Itu 'Circuit Breaker'?", "id": "Pemutus Arus Otomatis Saat Lebih." },
  { "en": "Apa Itu 'ELCB / RCBO'?", "id": "Pemutus Arus Bocor Anti Setrum." },
  { "en": "Apa Itu 'Surge Arrester'?", "id": "Pelindung Alat Dari Petir Lewat." },
  { "en": "Apa Itu 'Inductive Sensor'?", "id": "Sensor Deteksi Logam Tanpa Sentuh." },
  { "en": "Apa Itu 'Capacitive Sensor'?", "id": "Sensor Deteksi Benda Padat Cair." },
  { "en": "Apa Itu 'Photoelectric Sensor'?", "id": "Sensor Deteksi Potong Sinar Cahaya." },
  { "en": "Apa Itu 'Ultrasonic Sensor'?", "id": "Sensor Jarak Pakai Pantulan Suara." },
  { "en": "Apa Itu 'Thermocouple'?", "id": "Sensor Suhu Tinggi Dua Logam." },
  { "en": "Apa Itu 'RTD PT100'?", "id": "Sensor Suhu Platina Sangat Akurat." },
  { "en": "Apa Itu 'Load Cell'?", "id": "Batang Besi Sensor Timbangan Digital." },
  { "en": "Apa Itu 'Strain Gauge'?", "id": "Sensor Regangan Benda Akibat Tekanan." },
  { "en": "Apa Itu 'Rotary Encoder'?", "id": "Tombol Putar Digital Tanpa Batas." },
  { "en": "Apa Itu 'Limit Switch'?", "id": "Sakelar Pembatas Gerak Mekanik Mesin." },
  { "en": "Apa Itu 'Voltage Divider'?", "id": "Pembagi Tegangan Pakai Dua Resistor Seri." },
  { "en": "Apa Itu 'Current Divider'?", "id": "Pembagi Arus Pakai Resistor Paralel." },
  { "en": "Apa Itu 'Loading Effect'?", "id": "Tegangan Turun Saat Beban Terpasang." },
  { "en": "Apa Itu 'Impedance Matching'?", "id": "Menyamakan Hambatan Agar Transfer Daya Maksimal." },
  { "en": "Apa Itu 'Thevenin Theorem'?", "id": "Menyederhanakan Rangkaian Jadi Satu Sumber Tegangan." },
  { "en": "Apa Itu 'Norton Theorem'?", "id": "Menyederhanakan Rangkaian Jadi Satu Sumber Arus." },
  { "en": "Apa Itu 'Superposition Theorem'?", "id": "Analisis Rangkaian Dengan Satu Sumber Bergantian." },
  { "en": "Apa Itu 'Kirchhoff Voltage Law'?", "id": "Jumlah Tegangan Dalam Loop Adalah Nol." },
  { "en": "Apa Itu 'Kirchhoff Current Law'?", "id": "Arus Masuk Sama Dengan Arus Keluar." },
  { "en": "Apa Itu 'Wheatstone Bridge'?", "id": "Jembatan Resistor Untuk Ukur Hambatan Presisi." },
  { "en": "Apa Itu 'RC Time Constant'?", "id": "Waktu Pengisian Kapasitor Hingga 63 Persen." },
  { "en": "Apa Itu 'RL Time Constant'?", "id": "Waktu Arus Induktor Hingga 63 Persen." },
  { "en": "Apa Itu 'Resonance Frequency'?", "id": "Frekuensi Dimana Reaktansi Saling Meniadakan." },
  { "en": "Apa Itu 'Bandwidth'?", "id": "Rentang Frekuensi Yang Dilewatkan Filter." },
  { "en": "Apa Itu 'Cut-off Frequency'?", "id": "Batas Frekuensi Sinyal Mulai Diredam." },
  { "en": "Apa Itu 'Quality Factor' (Q)?", "id": "Ukuran Kualitas Selektivitas Filter Resonansi." },
  { "en": "Apa Itu 'Power Factor'?", "id": "Efisiensi Penggunaan Daya Listrik AC." },
  { "en": "Apa Itu 'Real Power' (Watt)?", "id": "Daya Nyata Yang Dipakai Alat Kerja." },
  { "en": "Apa Itu 'Apparent Power' (VA)?", "id": "Total Daya Gabungan Nyata Dan Reaktif." },
  { "en": "Apa Itu 'Reactive Power' (VAR)?", "id": "Daya Hilang Akibat Induktor Dan Kapasitor." },
  { "en": "Apa Itu 'Phase Angle'?", "id": "Beda Sudut Antara Tegangan Dan Arus." },
  { "en": "Apa Itu 'Lagging Power Factor'?", "id": "Arus Tertinggal Di Belakang Tegangan." },
  { "en": "Apa Itu 'Leading Power Factor'?", "id": "Arus Mendahului Di Depan Tegangan." },
  { "en": "Apa Itu 'Harmonics'?", "id": "Gelombang Frekuensi Kelipatan Yang Mengganggu." },
  { "en": "Apa Itu 'Total Harmonic Distortion'?", "id": "Persentase Cacat Sinyal Akibat Harmonisa." },
  { "en": "Apa Itu 'Skin Effect'?", "id": "Arus AC Cenderung Lewat Kulit Kabel." },
  { "en": "Apa Itu 'Hysteresis Loss'?", "id": "Rugi Energi Akibat Gesekan Magnetik." },
  { "en": "Apa Itu 'Corona Discharge'?", "id": "Kebocoran Listrik Udara Tegangan Tinggi." },
  { "en": "Apa Itu 'Ground Fault'?", "id": "Kabel Fasa Bocor Menyentuh Tanah." },
  { "en": "Apa Itu 'Short Circuit'?", "id": "Hubungan Langsung Fasa Dan Netral." },
  { "en": "Apa Itu 'Overload'?", "id": "Beban Lebih Besar Dari Kapasitas Alat." },
  { "en": "Apa Itu 'Voltage Sag'?", "id": "Tegangan Turun Sesaat Ganggu Mesin." },
  { "en": "Apa Itu 'Voltage Swell'?", "id": "Tegangan Naik Sesaat Bahayakan Elektronik." },
  { "en": "Apa Itu 'Surge'?", "id": "Lonjakan Tegangan Tinggi Sangat Singkat." },
  { "en": "Apa Itu 'Transient'?", "id": "Gangguan Sinyal Cepat Tiba Tiba." },
  { "en": "Apa Itu 'Noise'?", "id": "Sinyal Acak Yang Mengganggu Informasi." },
  { "en": "Apa Itu 'Signal-to-Noise Ratio'?", "id": "Perbandingan Kekuatan Sinyal Lawan Gangguan." },
  { "en": "Apa Itu 'Decibel' (dB)?", "id": "Satuan Logaritmik Pengukuran Penguatan Sinyal." },
  { "en": "Apa Itu 'Gain'?", "id": "Faktor Penguatan Sinyal Amplifier." },
  { "en": "Apa Itu 'Attenuation'?", "id": "Faktor Pelemahan Sinyal Kabel Panjang." },
  { "en": "Apa Itu 'Analog Signal'?", "id": "Sinyal Kontinyu Berubah Secara Halus." },
  { "en": "Apa Itu 'Digital Signal'?", "id": "Sinyal Diskrit Hanya Nol Dan Satu." },
  { "en": "Apa Itu 'Sampling'?", "id": "Mengambil Cuplikan Sinyal Analog Berkala." },
  { "en": "Apa Itu 'Quantization'?", "id": "Membulatkan Nilai Sampel Ke Angka Digital." },
  { "en": "Apa Itu 'Aliasing'?", "id": "Kesalahan Sinyal Akibat Sampling Lambat." },
  { "en": "Apa Itu 'Nyquist Rate'?", "id": "Kecepatan Minimal Sampling Agar Akurat." },
  { "en": "Apa Itu 'Bit Rate'?", "id": "Jumlah Bit Terkirim Per Detik." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Jumlah Perubahan Sinyal Per Detik." },
  { "en": "Apa Itu 'Resistor Carbon Film'?", "id": "Resistor Murah Umum Warna Krem." },
  { "en": "Apa Itu 'Resistor Metal Film'?", "id": "Resistor Presisi Warna Biru Toleransi Kecil." },
  { "en": "Apa Itu 'Resistor Wirewound'?", "id": "Resistor Kawat Untuk Daya Besar." },
  { "en": "Apa Itu 'Potentiometer Logarithmic'?", "id": "Potensiometer Khusus Pengatur Volume Audio." },
  { "en": "Apa Itu 'Trimmer Capacitor'?", "id": "Kapasitor Kecil Bisa Diputar Obeng." },
  { "en": "Apa Itu 'Varistor' (MOV)?", "id": "Resistor Pelindung Lonjakan Tegangan Tinggi." },
  { "en": "Apa Itu 'Thermistor NTC'?", "id": "Resistor Peka Suhu Untuk Sensor." },
  { "en": "Apa Itu 'Thermistor PTC'?", "id": "Resistor Peka Suhu Untuk Sekring." },
  { "en": "Apa Itu 'LDR' (Light Dependent Resistor)?", "id": "Resistor Berubah Nilai Kena Cahaya." },
  { "en": "Apa Itu 'Capacitor Ceramic'?", "id": "Kapasitor Non-Polar Kecil Coklat." },
  { "en": "Apa Itu 'Capacitor Polyester' (Mylar)?", "id": "Kapasitor Hijau Stabil Untuk Audio." },
  { "en": "Apa Itu 'Capacitor Tantalum'?", "id": "Kapasitor Polar Kecil Stabil Mahal." },
  { "en": "Apa Itu 'Capacitor Supercap'?", "id": "Kapasitor Raksasa Pengganti Baterai Memori." },
  { "en": "Apa Itu 'Inductor Toroid'?", "id": "Induktor Inti Cincin Efisiensi Tinggi." },
  { "en": "Apa Itu 'Choke Coil'?", "id": "Induktor Penahan Frekuensi Tinggi." },
  { "en": "Apa Itu 'Ferrite Bead'?", "id": "Manik Magnet Peredam Noise Kabel." },
  { "en": "Apa Itu 'Relay SPDT'?", "id": "Sakelar Magnet Satu Induk Dua Cabang." },
  { "en": "Apa Itu 'Relay Solid State' (SSR)?", "id": "Sakelar Elektronik Tanpa Gerak Mekanik." },
  { "en": "Apa Itu 'Reed Switch'?", "id": "Sakelar Kaca Aktif Dekat Magnet." },
  { "en": "Apa Itu 'Limit Switch'?", "id": "Sakelar Pembatas Gerakan Mekanik Robot." },
  { "en": "Apa Itu 'Dip Switch'?", "id": "Sakelar Baris Kecil Setting PCB." },
  { "en": "Apa Itu 'Tactile Switch'?", "id": "Tombol Tekan Kecil Bunyi Klik." },
  { "en": "Apa Itu 'Toggle Switch'?", "id": "Sakelar Tuas Batang Cetek Cetok." },
  { "en": "Apa Itu 'Rocker Switch'?", "id": "Sakelar Lampu Kotak On Off." },
  { "en": "Apa Itu 'Selector Switch'?", "id": "Sakelar Putar Pilih Mode Mesin." },
  { "en": "Apa Itu 'Emergency Stop'?", "id": "Tombol Jamur Merah Stop Darurat." },
  { "en": "Apa Itu 'Diode Rectifier'?", "id": "Dioda Penyearah Arus Listrik AC." },
  { "en": "Apa Itu 'Diode Zener'?", "id": "Dioda Penstabil Tegangan Mundur." },
  { "en": "Apa Itu 'Diode Schottky'?", "id": "Dioda Switching Cepat Tegangan Rendah." },
  { "en": "Apa Itu 'Diode Bridge'?", "id": "Empat Dioda Dalam Satu Kemasan." },
  { "en": "Apa Itu 'LED RGB'?", "id": "Lampu LED Tiga Warna Dasar." },
  { "en": "Apa Itu 'Photodiode'?", "id": "Sensor Cahaya Jadi Arus Listrik." },
  { "en": "Apa Itu 'Optocoupler'?", "id": "Pemisah Sinyal Pakai Cahaya Aman." },
  { "en": "Apa Itu 'Transistor NPN'?", "id": "Sakelar Aktif Sinyal Positif." },
  { "en": "Apa Itu 'Transistor PNP'?", "id": "Sakelar Aktif Sinyal Negatif." },
  { "en": "Apa Itu 'Darlington Pair'?", "id": "Dua Transistor Gabung Penguatan Ganda." },
  { "en": "Apa Itu 'MOSFET N-Channel'?", "id": "Sakelar Tegangan Efisien Sinyal Positif." },
  { "en": "Apa Itu 'MOSFET P-Channel'?", "id": "Sakelar Tegangan Efisien Sinyal Negatif." },
  { "en": "Apa Itu 'IGBT'?", "id": "Transistor Daya Tinggi Mesin Industri." },
  { "en": "Apa Itu 'Triac'?", "id": "Sakelar Pengendali Arus AC Bolak-Balik." },
  { "en": "Apa Itu 'SCR' (Silicon Controlled Rectifier)?", "id": "Sakelar Pengendali Arus DC Besar." },
  { "en": "Apa Itu 'Heat Sink'?", "id": "Aluminium Sirip Pembuang Panas Komponen." },
  { "en": "Apa Itu 'Thermal Paste'?", "id": "Pasta Penghantar Panas Ke Heatsink." },
  { "en": "Apa Itu 'Insulating Mica'?", "id": "Plastik Penyekat Listrik Transistor Bodi." },
  { "en": "Apa Itu 'Soldering Iron'?", "id": "Alat Pemanas Timah Solder Manual." },
  { "en": "Apa Itu 'Solder Sucker'?", "id": "Pompa Sedot Timah Solder Cair." },
  { "en": "Apa Itu 'Flux Pen'?", "id": "Spidol Cairan Pembersih Papan PCB." },
  { "en": "Apa Itu 'Multimeter Digital'?", "id": "Alat Ukur Listrik Layar Angka." },
  { "en": "Apa Itu 'Clamp Meter'?", "id": "Tang Ukur Arus Tanpa Putus." },
  { "en": "Apa Itu 'Carry Flag'?", "id": "Penanda Sisa Hitungan Matematika Prosesor." },
  { "en": "Apa Itu 'Zero Flag'?", "id": "Penanda Hasil Hitungan Adalah Nol." },
  { "en": "Apa Itu 'Overflow Flag'?", "id": "Penanda Hasil Hitungan Melebihi Batas." },
  { "en": "Apa Itu 'Signed Integer'?", "id": "Angka Biner Bisa Positif Negatif." },
  { "en": "Apa Itu 'Unsigned Integer'?", "id": "Angka Biner Hanya Bernilai Positif." },
  { "en": "Apa Itu 'Boolean Logic'?", "id": "Logika Matematika Hanya Benar Salah." },
  { "en": "Apa Itu 'Karnaugh Map'?", "id": "Cara Menyederhanakan Persamaan Logika Rumit." },
  { "en": "Apa Itu 'State Machine'?", "id": "Sistem Logika Berubah Sesuai Kondisi." },
  { "en": "Apa Itu 'Input Pull-up'?", "id": "Resistor Internal Tarik Sinyal Keatas." },
  { "en": "Apa Itu 'Input Pull-down'?", "id": "Resistor Internal Tarik Sinyal Kebawah." },
  { "en": "Apa Itu 'External Interrupt'?", "id": "Selaan Program Dari Tombol Luar." },
  { "en": "Apa Itu 'Timer Overflow'?", "id": "Waktu Penghitung Penuh Kembali Nol." },
  { "en": "Apa Itu 'Prescaler'?", "id": "Pembagi Frekuensi Detak Agar Lambat." },
  { "en": "Apa Itu 'Watchdog Reset'?", "id": "Restart Otomatis Karena Program Macet." },
  { "en": "Apa Itu 'Brown-out Reset'?", "id": "Restart Otomatis Karena Tegangan Anjlok." },
  { "en": "Apa Itu 'EEPROM Write'?", "id": "Menyimpan Data Permanen Ke Memori." },
  { "en": "Apa Itu 'Flash Memory'?", "id": "Memori Penyimpan Kode Program Utama." },
  { "en": "Apa Itu 'SRAM Memory'?", "id": "Memori Kerja Cepat Hilang Listrik." },
  { "en": "Apa Itu 'Solder Mask'?", "id": "Cat Pelindung Tembaga Anti Karat." },
  { "en": "Apa Itu 'Drill Hit'?", "id": "Lubang Bor Tembus Papan PCB." },
  { "en": "Apa Itu 'Annular Ring'?", "id": "Cincin Tembaga Mengelilingi Lubang Bor." },
  { "en": "Apa Itu 'Trace Width'?", "id": "Lebar Jalur Tembaga Penghantar Arus." },
  { "en": "Apa Itu 'Clearance'?", "id": "Jarak Aman Antara Dua Jalur." },
  { "en": "Apa Itu 'Ratnest'?", "id": "Garis Panduan Sambungan Belum Dirute." },
  { "en": "Apa Itu 'Autorouter'?", "id": "Software Pembuat Jalur PCB Otomatis." },
  { "en": "Apa Itu 'Design Rule Check'?", "id": "Pemeriksaan Otomatis Kesalahan Desain PCB." },
  { "en": "Apa Itu 'Gerber Viewer'?", "id": "Alat Melihat Hasil Desain Produksi." },
  { "en": "Apa Itu 'Flyback Converter'?", "id": "Power Supply Switching Isolasi Trafo." },
  { "en": "Apa Itu 'Boost Converter'?", "id": "Rangkaian Penaik Tegangan DC Efisien." },
  { "en": "Apa Itu 'Buck Converter'?", "id": "Rangkaian Penurun Tegangan DC Efisien." },
  { "en": "Apa Itu 'Linear Regulator'?", "id": "Penurun Tegangan Buang Panas Banyak." },
  { "en": "Apa Itu 'LDO Regulator'?", "id": "Penurun Tegangan Selisih Input Dikit." },
  { "en": "Apa Itu 'H-Bridge'?", "id": "Rangkaian Pengendali Putaran Motor DC." },
  { "en": "Apa Itu 'Inverter Pure Sine'?", "id": "Pengubah DC Ke AC Gelombang Halus." },
  { "en": "Apa Itu 'Inverter Square Wave'?", "id": "Pengubah DC Ke AC Gelombang Kotak." },
  { "en": "Apa Itu 'Soft Start'?", "id": "Penahan Lonjakan Arus Saat Nyala." },
  { "en": "Apa Itu 'Crowbar Circuit'?", "id": "Pengaman Korslet Paksa Saat Overvoltage." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Kecepatan Kirim Data Serial Perdetik." },
  { "en": "Apa Itu 'Parity Bit'?", "id": "Bit Cek Error Pengiriman Data." },
  { "en": "Apa Itu 'Start Bit'?", "id": "Tanda Awal Pengiriman Data Serial." },
  { "en": "Apa Itu 'Stop Bit'?", "id": "Tanda Akhir Pengiriman Data Serial." },
  { "en": "Apa Itu 'Handshaking'?", "id": "Saling Sapa Sebelum Kirim Data." },
  { "en": "Apa Itu 'Checksum'?", "id": "Nilai Total Cek Keaslian Data." },
  { "en": "Apa Itu 'Packet Loss'?", "id": "Data Hilang Di Tengah Jalan." },
  { "en": "Apa Itu 'Latency'?", "id": "Waktu Tunda Data Sampai Tujuan." },
  { "en": "Apa Itu 'Simplex'?", "id": "Komunikasi Satu Arah Saja Terus." },
  { "en": "Apa Itu 'Half Duplex'?", "id": "Komunikasi Dua Arah Gantian Bicara." },
  { "en": "Apa Itu 'Full Duplex'?", "id": "Komunikasi Dua Arah Bicara Bersamaan." },
  { "en": "Apa Itu 'Thermocouple Type K'?", "id": "Sensor Suhu Industri Paling Umum." },
  { "en": "Apa Itu 'RTD PT100'?", "id": "Sensor Suhu Platina Sangat Presisi." },
  { "en": "Apa Itu 'Strain Gauge'?", "id": "Sensor Regangan Logam Akibat Tekanan." },
  { "en": "Apa Itu 'Load Cell'?", "id": "Sensor Berat Untuk Timbangan Digital." },
  { "en": "Apa Itu 'Hall Effect Sensor'?", "id": "Sensor Deteksi Medan Magnet Tanpa Sentuh." },
  { "en": "Apa Itu 'Proximity Sensor'?", "id": "Sensor Deteksi Benda Jarak Dekat." },
  { "en": "Apa Itu 'Rotary Encoder'?", "id": "Sensor Posisi Putaran As Motor." },
  { "en": "Apa Itu 'Accelerometer'?", "id": "Sensor Percepatan Dan Kemiringan Benda." },
  { "en": "Apa Itu 'Gyroscope'?", "id": "Sensor Kecepatan Sudut Putar Benda." },
  { "en": "Apa Itu 'Magnetometer'?", "id": "Sensor Arah Mata Angin Kompas." },
  { "en": "Apa Itu 'Logic Analyzer'?", "id": "Alat Cek Sinyal Digital Banyak Kanal." },
  { "en": "Apa Itu 'Spectrum Analyzer'?", "id": "Alat Cek Kekuatan Sinyal Frekuensi." },
  { "en": "Apa Itu 'Network Analyzer'?", "id": "Alat Ukur Parameter Jaringan Kabel." },
  { "en": "Apa Itu 'Function Generator'?", "id": "Pembuat Gelombang Sinus Kotak Segitiga." },
  { "en": "Apa Itu 'Frequency Counter'?", "id": "Penghitung Jumlah Getaran Per Detik." },
  { "en": "Apa Itu 'LCR Meter'?", "id": "Pengukur Induktor Kapasitor Resistor Akurat." },
  { "en": "Apa Itu 'ESR Meter'?", "id": "Pengukur Kesehatan Kapasitor Elco Tua." },
  { "en": "Apa Itu 'Isolation Transformer'?", "id": "Trafo Pemisah Listrik Demi Keamanan." },
  { "en": "Apa Itu 'Variac'?", "id": "Trafo Putar Pengatur Tegangan AC." },
  { "en": "Apa Itu 'Dummy Load'?", "id": "Beban Buatan Untuk Tes Daya." },
  { "en": "Apa Itu 'Clamp Meter'?", "id": "Tang Ampere Ukur Arus Kabel." },
  { "en": "Apa Itu 'Gain Amplifier'?", "id": "Tingkat Penguatan Sinyal Suara Masuk." },
  { "en": "Apa Itu 'Feedback Loop'?", "id": "Sinyal Keluar Masuk Lagi Kedalam." },
  { "en": "Apa Itu 'Clipping'?", "id": "Sinyal Terpotong Karena Terlalu Keras." },
  { "en": "Apa Itu 'Distortion'?", "id": "Cacat Suara Tidak Sesuai Aslinya." },
  { "en": "Apa Itu 'Noise Floor'?", "id": "Batas Suara Desis Paling Rendah." },
  { "en": "Apa Itu 'Dynamic Range'?", "id": "Rentang Suara Terpelan Hingga Terkeras." },
  { "en": "Apa Itu 'Impedance Matching'?", "id": "Menyamakan Hambatan Speaker Dan Amply." },
  { "en": "Apa Itu 'Crossover Passive'?", "id": "Pemisah Nada Speaker Tanpa Listrik." },
  { "en": "Apa Itu 'Crossover Active'?", "id": "Pemisah Nada Speaker Pakai Listrik." },
  { "en": "Apa Itu 'Pre-Amp'?", "id": "Penguat Awal Sinyal Mic Lemah." },
  { "en": "Apa Itu 'Resistor Array'?", "id": "Gabungan Banyak Resistor Satu Kemasan." },
  { "en": "Apa Itu 'Capacitor Bank'?", "id": "Kumpulan Banyak Kapasitor Disusun Paralel." },
  { "en": "Apa Itu 'Inductor Core'?", "id": "Inti Besi Di Dalam Gulungan." },
  { "en": "Apa Itu 'Air Core'?", "id": "Gulungan Kawat Tanpa Inti Besi." },
  { "en": "Apa Itu 'Rectifier Diode'?", "id": "Dioda Penyearah Arus Listrik Besar." },
  { "en": "Apa Itu 'Schottky Diode'?", "id": "Dioda Sakelar Cepat Jatuh Rendah." },
  { "en": "Apa Itu 'Tunnel Diode'?", "id": "Dioda Efek Kuantum Sakelar Cepat." },
  { "en": "Apa Itu 'Varactor Diode'?", "id": "Dioda Kapasitas Berubah Sesuai Tegangan." },
  { "en": "Apa Itu 'Photodiode'?", "id": "Dioda Pengubah Cahaya Jadi Arus." },
  { "en": "Apa Itu 'Lock Out Tag Out'?", "id": "Prosedur Gembok Pengaman Perbaikan Mesin." },
  { "en": "Apa Itu 'Grounding Rod'?", "id": "Batang Tembaga Tanam Pembuang Petir." },
  { "en": "Apa Itu 'Lightning Arrester'?", "id": "Alat Penahan Petir Masuk Rumah." },
  { "en": "Apa Itu 'Surge Protector'?", "id": "Alat Tahan Lonjakan Listrik Tiba." },
  { "en": "Apa Itu 'Fuse Link'?", "id": "Kawat Sekring Putus Saat Lebih." },
  { "en": "Apa Itu 'Circuit Breaker'?", "id": "Sakelar Otomatis Putus Saat Korslet." },
  { "en": "Apa Itu 'Residual Current'?", "id": "Arus Sisa Bocor Ke Tanah." },
  { "en": "Apa Itu 'Phase Sequence'?", "id": "Urutan Fasa Listrik Tiga Fasa." },
  { "en": "Apa Itu 'Neutral Point'?", "id": "Titik Tengah Sambungan Bintang Trafo." },
  { "en": "Apa Itu 'Earth Pit'?", "id": "Lubang Kontrol Sambungan Kabel Tanah." },
  { "en": "Apa Itu 'PLC'?", "id": "Komputer Industri Pengendali Mesin Otomatis Kuat." },
  { "en": "Apa Itu 'Ladder Diagram'?", "id": "Bahasa Pemrograman PLC Bentuk Tangga Logika." },
  { "en": "Apa Itu 'HMI' (Human Machine Interface)?", "id": "Layar Sentuh Pengendali Mesin Industri." },
  { "en": "Apa Itu 'SCADA'?", "id": "Sistem Pengawas Dan Pengendali Jarak Jauh." },
  { "en": "Apa Itu 'NO' (Normally Open)?", "id": "Kondisi Awal Sakelar Terbuka Mati." },
  { "en": "Apa Itu 'NC' (Normally Closed)?", "id": "Kondisi Awal Sakelar Tertutup Nyala." },
  { "en": "Apa Itu 'Contactor'?", "id": "Sakelar Magnetik Besar Untuk Motor Industri." },
  { "en": "Apa Itu 'Thermal Overload Relay'?", "id": "Pelindung Motor Dari Panas Arus Berlebih." },
  { "en": "Apa Itu 'Push Button'?", "id": "Tombol Tekan Kendali Mesin Manual." },
  { "en": "Apa Itu 'Emergency Stop'?", "id": "Tombol Merah Darurat Matikan Semua Mesin." },
  { "en": "Apa Itu 'Stator'?", "id": "Bagian Motor Yang Diam Tidak Bergerak." },
  { "en": "Apa Itu 'Rotor'?", "id": "Bagian Motor Yang Berputar Di Tengah." },
  { "en": "Apa Itu 'Commutator'?", "id": "Cincin Tembaga Pembalik Arus Motor DC." },
  { "en": "Apa Itu 'Carbon Brush'?", "id": "Sikat Arang Penghantar Listrik Ke Rotor." },
  { "en": "Apa Itu 'Armature'?", "id": "Gulungan Kawat Pada Rotor Motor DC." },
  { "en": "Apa Itu 'Induction Motor'?", "id": "Motor Listrik AC Paling Umum Pabrik." },
  { "en": "Apa Itu 'Synchronous Motor'?", "id": "Motor Kecepatan Konstan Ikut Frekuensi Listrik." },
  { "en": "Apa Itu 'Stepper Motor'?", "id": "Motor Gerak Patah Patah Sangat Presisi." },
  { "en": "Apa Itu 'Servo Motor'?", "id": "Motor Pintar Pengatur Sudut Posisi." },
  { "en": "Apa Itu 'RPM'?", "id": "Satuan Kecepatan Putaran Mesin Per Menit." },
  { "en": "Apa Itu 'Node'?", "id": "Titik Pertemuan Tiga Kabel Atau Lebih." },
  { "en": "Apa Itu 'Branch'?", "id": "Jalur Kabel Antara Dua Titik Node." },
  { "en": "Apa Itu 'Loop'?", "id": "Lintasan Arus Tertutup Kembali Ke Awal." },
  { "en": "Apa Itu 'Mesh'?", "id": "Loop Kecil Tanpa Loop Lain Didalamnya." },
  { "en": "Apa Itu 'Superposition Theorem'?", "id": "Analisis Rangkaian Banyak Sumber Satu Persatu." },
  { "en": "Apa Itu 'Thevenin Theorem'?", "id": "Penyederhanaan Rangkaian Jadi Satu Sumber Tegangan." },
  { "en": "Apa Itu 'Norton Theorem'?", "id": "Penyederhanaan Rangkaian Jadi Satu Sumber Arus." },
  { "en": "Apa Itu 'Voltage Drop'?", "id": "Tegangan Berkurang Setelah Lewat Beban." },
  { "en": "Apa Itu 'Leakage Current'?", "id": "Arus Bocor Kecil Lewat Isolator." },
  { "en": "Apa Itu 'Short Circuit'?", "id": "Hubungan Pendek Arus Besar Berbahaya." },
  { "en": "Apa Itu 'Analog Signal'?", "id": "Sinyal Sambung Terus Tanpa Putus." },
  { "en": "Apa Itu 'Digital Signal'?", "id": "Sinyal Putus Putus Angka Satu Nol." },
  { "en": "Apa Itu 'Bitrate'?", "id": "Kecepatan Transfer Data Digital Per Detik." },
  { "en": "Apa Itu 'Bandwidth'?", "id": "Lebar Jalan Raya Data Informasi." },
  { "en": "Apa Itu 'Frequency'?", "id": "Banyaknya Getaran Gelombang Dalam Satu Detik." },
  { "en": "Apa Itu 'Period'?", "id": "Waktu Yang Dibutuhkan Satu Gelombang Penuh." },
  { "en": "Apa Itu 'Wavelength'?", "id": "Jarak Antara Dua Puncak Gelombang." },
  { "en": "Apa Itu 'Amplitude'?", "id": "Tinggi Puncak Gelombang Dari Titik Tengah." },
  { "en": "Apa Itu 'Phase'?", "id": "Posisi Sudut Awal Gelombang Sinus." },
  { "en": "Apa Itu 'Noise'?", "id": "Sinyal Gangguan Acak Merusak Data." },
  { "en": "Apa Itu 'Varistor'?", "id": "Resistor Pelindung Lonjakan Tegangan Tinggi." },
  { "en": "Apa Itu 'Thermistor'?", "id": "Resistor Peka Perubahan Suhu Lingkungan." },
  { "en": "Apa Itu 'Optocoupler'?", "id": "Pemisah Sinyal Listrik Pakai Cahaya." },
  { "en": "Apa Itu 'Triac'?", "id": "Sakelar Elektronik Arus Bolak Balik." },
  { "en": "Apa Itu 'SCR'?", "id": "Sakelar Elektronik Arus Searah Besar." },
  { "en": "Apa Itu 'IGBT'?", "id": "Transistor Daya Besar Gabungan Mosfet Bipolar." },
  { "en": "Apa Itu 'MOSFET'?", "id": "Transistor Sakelar Cepat Efisiensi Tinggi." },
  { "en": "Apa Itu 'Relay'?", "id": "Sakelar Mekanik Digerakkan Medan Magnet." },
  { "en": "Apa Itu 'Solenoid'?", "id": "Kumparan Kawat Pendorong Batang Besi." },
  { "en": "Apa Itu 'Rectifier'?", "id": "Pengubah Arus Bolak Balik Jadi Searah." },
  { "en": "Apa Itu 'Inverter'?", "id": "Pengubah Arus Searah Jadi Bolak Balik." },
  { "en": "Apa Itu 'Transformer'?", "id": "Alat Naik Turun Tegangan Listrik AC." },
  { "en": "Apa Itu 'SMPS'?", "id": "Adaptor Modern Ringan Efisiensi Tinggi." },
  { "en": "Apa Itu 'Linear Power Supply'?", "id": "Adaptor Berat Pakai Trafo Besi Besar." },
  { "en": "Apa Itu 'Battery Capacity'?", "id": "Ukuran Daya Simpan Energi Baterai." },
  { "en": "Apa Itu 'Depth of Discharge'?", "id": "Batas Aman Pengurasan Isi Baterai." },
  { "en": "Apa Itu 'State of Charge'?", "id": "Persentase Sisa Isi Energi Baterai." },
  { "en": "Apa Itu 'BMS'?", "id": "Otak Pengaman Baterai Lithium Pack." },
  { "en": "Apa Itu 'Charging Cycle'?", "id": "Satu Kali Isi Penuh Dan Habis." },
  { "en": "Apa Itu 'Register'?", "id": "Memori Kecil Cepat Dalam Prosesor." },
  { "en": "Apa Itu 'Accumulator'?", "id": "Register Utama Penampung Hasil Hitungan." },
  { "en": "Apa Itu 'Stack Pointer'?", "id": "Penunjuk Tumpukan Memori Sementara." },
  { "en": "Apa Itu 'Program Counter'?", "id": "Penunjuk Alamat Perintah Selanjutnya." },
  { "en": "Apa Itu 'Timer Counter'?", "id": "Penghitung Waktu Atau Pulsa Internal." },
  { "en": "Apa Itu 'ADC'?", "id": "Pengubah Tegangan Sensor Jadi Angka." },
  { "en": "Apa Itu 'DAC'?", "id": "Pengubah Angka Digital Jadi Tegangan Keluar." },
  { "en": "Apa Itu 'PWM'?", "id": "Teknik Atur Daya Pakai Lebar Pulsa." },
  { "en": "Apa Itu 'GPIO'?", "id": "Pin Serba Guna Mikrokontroler." },
  { "en": "Apa Itu 'Multimeter'?", "id": "Alat Ukur Listrik Paling Dasar." },
  { "en": "Apa Itu 'Function Generator'?", "id": "Pembuat Sinyal Gelombang Uji Coba." },
  { "en": "Apa Itu 'Logic Analyzer'?", "id": "Perekam Sinyal Digital Banyak Kanal." },
  { "en": "Apa Itu 'Spectrum Analyzer'?", "id": "Alat Analisa Kekuatan Frekuensi Radio." },
  { "en": "Apa Itu 'Clamp Meter'?", "id": "Tang Ampere Ukur Arus Tanpa Putus." },
  { "en": "Apa Itu 'LCR Meter'?", "id": "Pengukur Komponen Pasif Sangat Akurat." },
  { "en": "Apa Itu 'Soldering Station'?", "id": "Alat Solder Dengan Pengatur Suhu." },
  { "en": "Apa Itu 'Desoldering Pump'?", "id": "Penyedot Timah Solder Cair Manual." },
  { "en": "Apa Itu 'Breadboard'?", "id": "Papan Rakit Rangkaian Tanpa Solder." },
  { "en": "Apa Itu 'Coaxial Cable'?", "id": "Kabel Antena TV Inti Tembaga Tunggal." },
  { "en": "Apa Itu 'Twisted Pair'?", "id": "Kabel Telepon Dipelintir Kurangi Noise." },
  { "en": "Apa Itu 'Fiber Optic'?", "id": "Kabel Serat Kaca Hantar Cahaya." },
  { "en": "Apa Itu 'Ribbon Cable'?", "id": "Kabel Pipih Lebar Banyak Jalur." },
  { "en": "Apa Itu 'Jumper Wire'?", "id": "Kabel Pendek Penghubung Rangkaian Sementara." },
  { "en": "Apa Itu 'Terminal Block'?", "id": "Konektor Kabel Jepit Pakai Baut." },
  { "en": "Apa Itu 'Male Header'?", "id": "Jarum Konektor Logam Menonjol." },
  { "en": "Apa Itu 'Female Header'?", "id": "Lubang Konektor Plastik Penerima Jarum." },
  { "en": "Apa Itu 'Banana Plug'?", "id": "Colokan Kabel Laboratorium Bentuk Pisang." },
  { "en": "Apa Itu 'Alligator Clip'?", "id": "Jepit Buaya Untuk Sambungan Cepat." },
  { "en": "Apa Itu 'PCB'?", "id": "Papan Hijau Tempat Rangkaian Elektronik." },
  { "en": "Apa Itu 'PCB Layout'?", "id": "Desain Tata Letak Komponen Di PCB." },
  { "en": "Apa Itu 'Footprint'?", "id": "Pola Kaki Komponen Di Papan PCB." },
  { "en": "Apa Itu 'Via'?", "id": "Lubang Tembus Penghubung Lapisan PCB." },
  { "en": "Apa Itu 'Solder Mask'?", "id": "Lapisan Pelindung Tembaga Anti Karat." },
  { "en": "Apa Itu 'Silkscreen'?", "id": "Tulisan Putih Label Komponen PCB." },
  { "en": "Apa Itu 'Etching'?", "id": "Proses Kikis Tembaga Bikin Jalur." },
  { "en": "Apa Itu 'Flux'?", "id": "Cairan Pembersih Bantu Timah Nempel." },
  { "en": "Apa Itu 'Root Locus'?", "id": "Grafik Jejak Akar Kestabilan Sistem Kendali." },
  { "en": "Apa Itu 'Bode Plot'?", "id": "Grafik Respon Frekuensi Pada Sistem Kendali." },
  { "en": "Apa Itu 'Transfer Function'?", "id": "Rumus Matematika Hubungan Input Dan Output." },
  { "en": "Apa Itu 'Feedback Control'?", "id": "Sistem Kendali Dengan Umpan Balik Koreksi." },
  { "en": "Apa Itu 'Feedforward Control'?", "id": "Sistem Kendali Antisipasi Gangguan Sejak Awal." },
  { "en": "Apa Itu 'PID Controller'?", "id": "Algoritma Kendali Paling Populer Di Industri." },
  { "en": "Apa Itu 'Proportional Gain'?", "id": "Penguat Respon Sesuai Besar Kesalahan Saat Ini." },
  { "en": "Apa Itu 'Integral Time'?", "id": "Waktu Koreksi Akumulasi Kesalahan Masa Lalu." },
  { "en": "Apa Itu 'Derivative Time'?", "id": "Waktu Prediksi Kesalahan Di Masa Depan." },
  { "en": "Apa Itu 'Steady State Error'?", "id": "Selisih Target Saat Sistem Sudah Stabil." },
  { "en": "Apa Itu 'Overshoot'?", "id": "Lonjakan Nilai Melebihi Target Yang Diinginkan." },
  { "en": "Apa Itu 'Settling Time'?", "id": "Waktu Yang Dibutuhkan Hingga Sinyal Tenang." },
  { "en": "Apa Itu 'Rise Time'?", "id": "Waktu Sinyal Naik Dari Nol Ke Target." },
  { "en": "Apa Itu 'Stability Margin'?", "id": "Batas Aman Sebelum Sistem Menjadi Tidak Stabil." },
  { "en": "Apa Itu 'Nyquist Criterion'?", "id": "Syarat Kestabilan Sistem Kendali Loop Tertutup." },
  { "en": "Apa Itu 'Laplace Transform'?", "id": "Alat Matematika Ubah Waktu Jadi Frekuensi." },
  { "en": "Apa Itu 'Fourier Transform'?", "id": "Alat Urai Sinyal Jadi Komponen Frekuensi." },
  { "en": "Apa Itu 'Z-Transform'?", "id": "Alat Matematika Untuk Sinyal Digital Diskrit." },
  { "en": "Apa Itu 'Convolution'?", "id": "Operasi Gabung Dua Sinyal Jadi Satu." },
  { "en": "Apa Itu 'Impulse Response'?", "id": "Reaksi Sistem Terhadap Kejutan Sesaat." },
  { "en": "Apa Itu 'Step Response'?", "id": "Reaksi Sistem Terhadap Perubahan Input Tiba-Tiba." },
  { "en": "Apa Itu 'Frequency Response'?", "id": "Reaksi Sistem Terhadap Berbagai Frekuensi Input." },
  { "en": "Apa Itu 'Sampling Theorem'?", "id": "Aturan Ambil Sampel Agar Data Akurat." },
  { "en": "Apa Itu 'Aliasing Effect'?", "id": "Sinyal Palsu Akibat Sampling Terlalu Lambat." },
  { "en": "Apa Itu 'Quantization Noise'?", "id": "Kesalahan Nilai Akibat Pembulatan Angka Digital." },
  { "en": "Apa Itu 'Digital Filter'?", "id": "Program Matematika Penyaring Sinyal Angka." },
  { "en": "Apa Itu 'FIR Filter'?", "id": "Filter Digital Respon Impuls Terbatas Stabil." },
  { "en": "Apa Itu 'IIR Filter'?", "id": "Filter Digital Respon Impuls Tak Terbatas." },
  { "en": "Apa Itu 'DSP Processor'?", "id": "Chip Khusus Hitung Matematika Sinyal Cepat." },
  { "en": "Apa Itu 'AM Modulation'?", "id": "Tumpangkan Sinyal Ubah Tinggi Gelombang." },
  { "en": "Apa Itu 'FM Modulation'?", "id": "Tumpangkan Sinyal Ubah Rapat Gelombang." },
  { "en": "Apa Itu 'PM Modulation'?", "id": "Tumpangkan Sinyal Ubah Fasa Gelombang." },
  { "en": "Apa Itu 'ASK Modulation'?", "id": "Modulasi Digital Ubah Amplitudo Sinyal." },
  { "en": "Apa Itu 'FSK Modulation'?", "id": "Modulasi Digital Ubah Frekuensi Sinyal." },
  { "en": "Apa Itu 'PSK Modulation'?", "id": "Modulasi Digital Ubah Fasa Sinyal." },
  { "en": "Apa Itu 'QAM Modulation'?", "id": "Modulasi Gabungan Amplitudo Dan Fasa Digital." },
  { "en": "Apa Itu 'Demodulation'?", "id": "Proses Ambil Kembali Informasi Dari Sinyal." },
  { "en": "Apa Itu 'Carrier Signal'?", "id": "Gelombang Pembawa Informasi Jarak Jauh." },
  { "en": "Apa Itu 'Baseband Signal'?", "id": "Sinyal Asli Informasi Sebelum Dimodulasi." },
  { "en": "Apa Itu 'Bandwidth Efficiency'?", "id": "Hematnya Pakai Pita Frekuensi Kirim Data." },
  { "en": "Apa Itu 'Bit Error Rate'?", "id": "Persentase Kesalahan Bit Yang Diterima." },
  { "en": "Apa Itu 'Signal to Noise'?", "id": "Perbandingan Kekuatan Sinyal Lawan Gangguan." },
  { "en": "Apa Itu 'Channel Capacity'?", "id": "Batas Maksimal Data Lewat Suatu Saluran." },
  { "en": "Apa Itu 'Multiplexing'?", "id": "Gabung Banyak Sinyal Lewat Satu Kabel." },
  { "en": "Apa Itu 'TDM' (Time Division)?", "id": "Multiplexing Berdasarkan Giliran Waktu." },
  { "en": "Apa Itu 'FDM' (Frequency Division)?", "id": "Multiplexing Berdasarkan Pembagian Frekuensi." },
  { "en": "Apa Itu 'CDM' (Code Division)?", "id": "Multiplexing Berdasarkan Kode Unik Pengguna." },
  { "en": "Apa Itu 'Antenna Gain'?", "id": "Kekuatan Antena Fokuskan Arah Sinyal." },
  { "en": "Apa Itu 'Radiation Pattern'?", "id": "Bentuk Pancaran Sinyal Keluar Antena." },
  { "en": "Apa Itu 'Isotropic Antenna'?", "id": "Antena Teori Pancar Segala Arah Sempurna." },
  { "en": "Apa Itu 'Dipole Antenna'?", "id": "Antena Dua Kawat Lurus Sederhana." },
  { "en": "Apa Itu 'Yagi Antenna'?", "id": "Antena Pengarah Mirip Tulang Ikan." },
  { "en": "Apa Itu 'Parabolic Antenna'?", "id": "Antena Piringan Fokus Sinyal Satelit." },
  { "en": "Apa Itu 'Waveguide'?", "id": "Pipa Logam Pemandu Gelombang Mikro." },
  { "en": "Apa Itu 'Transmission Line'?", "id": "Kabel Penghantar Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu 'Characteristic Impedance'?", "id": "Hambatan Khas Kabel Transmisi Panjang." },
  { "en": "Apa Itu 'SWR' (Standing Wave Ratio)?", "id": "Ukuran Pantulan Sinyal Balik Antena." },
  { "en": "Apa Itu 'Smith Chart'?", "id": "Grafik Bantu Desain Impedansi RF." },
  { "en": "Apa Itu 'Scattering Parameters' (S-Param)?", "id": "Parameter Perilaku Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu 'Op-Amp Slew Rate'?", "id": "Kecepatan Op-Amp Ubah Tegangan Output." },
  { "en": "Apa Itu 'CMRR' (Common Mode)?", "id": "Kemampuan Op-Amp Tolak Sinyal Gangguan." },
  { "en": "Apa Itu 'Input Bias Current'?", "id": "Arus Masuk Basis Transistor Op-Amp." },
  { "en": "Apa Itu 'Input Offset Voltage'?", "id": "Tegangan Error Kecil Input Op-Amp." },
  { "en": "Apa Itu 'Gain Bandwidth Product'?", "id": "Batas Frekuensi Kerja Penguat Op-Amp." },
  { "en": "Apa Itu 'Virtual Ground'?", "id": "Titik Tegangan Nol Semu Op-Amp." },
  { "en": "Apa Itu 'Schmitt Trigger'?", "id": "Komparator Histeresis Bersihkan Sinyal Berisik." },
  { "en": "Apa Itu 'Oscillator Colpitts'?", "id": "Pembangkit Frekuensi Pakai Dua Kapasitor." },
  { "en": "Apa Itu 'Oscillator Hartley'?", "id": "Pembangkit Frekuensi Pakai Dua Induktor." },
  { "en": "Apa Itu 'Crystal Oscillator'?", "id": "Pembangkit Frekuensi Stabil Piezoelektrik." },
  { "en": "Apa Itu 'VCO' (Voltage Controlled)?", "id": "Osilator Frekuensi Diatur Tegangan Masuk." },
  { "en": "Apa Itu 'PLL' (Phase Locked Loop)?", "id": "Rangkaian Pengunci Fasa Frekuensi Stabil." },
  { "en": "Apa Itu 'Mixer RF'?", "id": "Pencampur Sinyal Ubah Frekuensi Radio." },
  { "en": "Apa Itu 'Thyristor'?", "id": "Sakelar Semikonduktor Daya Kunci Arus." },
  { "en": "Apa Itu 'Triac'?", "id": "Thyristor Dua Arah Pengendali AC." },
  { "en": "Apa Itu 'Diac'?", "id": "Pemicu Triac Agar Nyala Stabil." },
  { "en": "Apa Itu 'GTO' (Gate Turn-off)?", "id": "Thyristor Bisa Dimatikan Lewat Gate." },
  { "en": "Apa Itu 'IGBT'?", "id": "Transistor Tegangan Tinggi Sakelar Cepat." },
  { "en": "Apa Itu 'Power MOSFET'?", "id": "Mosfet Khusus Arus Besar Efisien." },
  { "en": "Apa Itu 'Snubber Circuit'?", "id": "Peredam Lonjakan Tegangan Lindungi Sakelar." },
  { "en": "Apa Itu 'Freewheeling Diode'?", "id": "Dioda Buang Arus Balik Induktor." },
  { "en": "Apa Itu 'Heat Sink Calculation'?", "id": "Hitung Ukuran Pendingin Agar Awet." },
  { "en": "Apa Itu 'Thermal Resistance'?", "id": "Hambatan Panas Mengalir Keluar Komponen." },
  { "en": "Apa Itu 'Junction Temperature'?", "id": "Suhu Inti Chip Silikon Saat Kerja." },
  { "en": "Apa Itu 'Safe Operating Area'?", "id": "Grafik Batas Aman Kerja Transistor." },
  { "en": "Apa Itu 'Derating Factor'?", "id": "Penurunan Batas Kemampuan Sesuai Suhu." },
  { "en": "Apa Itu 'MTBF' (Mean Time)?", "id": "Rata Rata Waktu Sebelum Alat Rusak." },
  { "en": "Apa Itu 'Failure Rate'?", "id": "Laju Kerusakan Komponen Per Waktu." },
  { "en": "Apa Itu 'Burn-in Test'?", "id": "Uji Nyala Lama Deteksi Cacat Awal." },
  { "en": "Apa Itu 'Environmental Test'?", "id": "Uji Tahan Suhu Lembab Dan Getar." },
  { "en": "Apa Itu 'EMC' (Compatibility)?", "id": "Kemampuan Alat Kerja Tanpa Ganggu Lain." },
  { "en": "Apa Itu 'EMI' (Interference)?", "id": "Gangguan Gelombang Elektromagnetik Dari Luar." },
  { "en": "Apa Itu 'Shielding Effectiveness'?", "id": "Kemampuan Bungkus Logam Tahan Gangguan." },
  { "en": "Apa Itu 'Ground Loop'?", "id": "Arus Putar Tanah Bikin Dengung." },
  { "en": "Apa Itu 'Decoupling Network'?", "id": "Kapasitor Peredam Gangguan Power Supply." },
  { "en": "Apa Itu 'Transmission Loss'?", "id": "Daya Hilang Di Kabel Panjang." },
  { "en": "Apa Itu 'Insertion Loss'?", "id": "Daya Hilang Saat Alat Disisipkan." },
  { "en": "Apa Itu 'Return Loss'?", "id": "Daya Pantul Akibat Ketidakcocokan Impedansi." },
  { "en": "Apa Itu 'VHDL'?", "id": "Bahasa Pemrograman Untuk Desain Chip FPGA." },
  { "en": "Apa Itu 'Verilog'?", "id": "Bahasa Desain Hardware Mirip Bahasa C." },
  { "en": "Apa Itu 'FPGA'?", "id": "Chip Logika Bisa Diprogram Ulang Berkali-kali." },
  { "en": "Apa Itu 'Look-Up Table' (LUT)?", "id": "Tabel Kebenaran Kecil Penyusun Logika FPGA." },
  { "en": "Apa Itu 'Synthesis' (FPGA)?", "id": "Mengubah Kode Tulis Jadi Gerbang Logika." },
  { "en": "Apa Itu 'Place and Route'?", "id": "Menata Letak Gerbang Logika Dalam Chip." },
  { "en": "Apa Itu 'RTL' (Register Transfer Level)?", "id": "Desain Aliran Data Antar Register Digital." },
  { "en": "Apa Itu 'Clock Skew'?", "id": "Beda Waktu Datang Sinyal Detak." },
  { "en": "Apa Itu 'Jitter'?", "id": "Ketidakstabilan Waktu Sinyal Detak Digital." },
  { "en": "Apa Itu 'Setup Time'?", "id": "Waktu Data Siap Sebelum Detak Datang." },
  { "en": "Apa Itu 'Hold Time'?", "id": "Waktu Data Tahan Setelah Detak Lewat." },
  { "en": "Apa Itu 'Metastability'?", "id": "Kondisi Sinyal Digital Bingung Antara 0-1." },
  { "en": "Apa Itu 'Debouncing'?", "id": "Menghilangkan Sinyal Getar Tombol Mekanik." },
  { "en": "Apa Itu 'Shift Register'?", "id": "Rangkaian Geser Data Bit Demi Bit." },
  { "en": "Apa Itu 'Barrel Shifter'?", "id": "Rangkaian Geser Banyak Bit Sekaligus Cepat." },
  { "en": "Apa Itu 'Multiplexer' (Mux)?", "id": "Pemilih Satu Data Dari Banyak Sumber." },
  { "en": "Apa Itu 'Demultiplexer' (Demux)?", "id": "Penyebar Satu Data Ke Banyak Tujuan." },
  { "en": "Apa Itu 'Decoder'?", "id": "Penerjemah Kode Biner Ke Sinyal Aktif." },
  { "en": "Apa Itu 'Encoder'?", "id": "Pengubah Sinyal Aktif Jadi Kode Biner." },
  { "en": "Apa Itu 'Priority Encoder'?", "id": "Encoder Yang Mendahulukan Sinyal Paling Penting." },
  { "en": "Apa Itu 'DMA' (Direct Memory Access)?", "id": "Transfer Data Memori Tanpa Ganggu Prosesor." },
  { "en": "Apa Itu 'Interrupt Vector'?", "id": "Alamat Kode Penanganan Sinyal Selaan." },
  { "en": "Apa Itu 'Stack Pointer'?", "id": "Penunjuk Tumpukan Memori Sementara Prosesor." },
  { "en": "Apa Itu 'Heap Memory'?", "id": "Memori Dinamis Yang Diatur Oleh Programmer." },
  { "en": "Apa Itu 'Stack Memory'?", "id": "Memori Otomatis Untuk Fungsi Dan Variabel." },
  { "en": "Apa Itu 'Volatile Memory'?", "id": "Data Hilang Jika Listrik Dimatikan." },
  { "en": "Apa Itu 'Non-Volatile Memory'?", "id": "Data Tetap Ada Walau Listrik Mati." },
  { "en": "Apa Itu 'Big Endian'?", "id": "Simpan Byte Terbesar Di Alamat Awal." },
  { "en": "Apa Itu 'Little Endian'?", "id": "Simpan Byte Terkecil Di Alamat Awal." },
  { "en": "Apa Itu 'Bitwise Operation'?", "id": "Operasi Logika Tingkat Bit Satu Persatu." },
  { "en": "Apa Itu 'Masking'?", "id": "Menutupi Bit Tertentu Agar Tidak Berubah." },
  { "en": "Apa Itu 'Polling'?", "id": "Cek Status Berulang Ulang Terus Menerus." },
  { "en": "Apa Itu 'Rectifier Half Wave'?", "id": "Penyearah Setengah Gelombang Satu Dioda." },
  { "en": "Apa Itu 'Rectifier Full Wave'?", "id": "Penyearah Gelombang Penuh Empat Dioda." },
  { "en": "Apa Itu 'Center Tap Rectifier'?", "id": "Penyearah Gelombang Penuh Dua Dioda Trafo." },
  { "en": "Apa Itu 'Smoothing Capacitor'?", "id": "Kapasitor Perata Tegangan Hasil Penyearah." },
  { "en": "Apa Itu 'Ripple Voltage'?", "id": "Sisa Gelombang AC Pada Tegangan DC." },
  { "en": "Apa Itu 'Clamp Circuit'?", "id": "Penggeser Posisi Sinyal Ke Atas Bawah." },
  { "en": "Apa Itu 'Clipper Circuit'?", "id": "Pemotong Puncak Sinyal Yang Berlebih." },
  { "en": "Apa Itu 'Switching Power Supply'?", "id": "Adaptor Modern Ringan Efisiensi Tinggi." },
  { "en": "Apa Itu 'Flyback Topology'?", "id": "Jenis Adaptor Switching Paling Umum Murah." },
  { "en": "Apa Itu 'Forward Topology'?", "id": "Jenis Adaptor Switching Daya Lebih Besar." },
  { "en": "Apa Itu 'Push-Pull Converter'?", "id": "Konverter Daya Pakai Dua Sakelar Bergantian." },
  { "en": "Apa Itu 'Full Bridge Converter'?", "id": "Konverter Daya Empat Sakelar Efisiensi Tinggi." },
  { "en": "Apa Itu 'Gate Driver'?", "id": "Chip Pemicu Sakelar Mosfet Daya Besar." },
  { "en": "Apa Itu 'Dead Time'?", "id": "Jeda Aman Antar Sakelar Agar Tidak Korslet." },
  { "en": "Apa Itu 'Galvanic Isolation'?", "id": "Pemisah Jalur Listrik Pakai Trafo." },
  { "en": "Apa Itu 'Opto-Isolator'?", "id": "Pemisah Sinyal Pakai Cahaya LED." },
  { "en": "Apa Itu 'Snubber'?", "id": "Peredam Lonjakan Tegangan Pada Sakelar." },
  { "en": "Apa Itu 'Heat Sink'?", "id": "Logam Sirip Pembuang Panas Komponen." },
  { "en": "Apa Itu 'Thermal Paste'?", "id": "Krim Penghantar Panas Ke Heatsink." },
  { "en": "Apa Itu 'Convection Cooling'?", "id": "Pendinginan Alami Lewat Udara Mengalir." },
  { "en": "Apa Itu 'Forced Air Cooling'?", "id": "Pendinginan Paksa Pakai Kipas Angin." },
  { "en": "Apa Itu 'Liquid Cooling'?", "id": "Pendinginan Pakai Cairan Yang Bersirkulasi." },
  { "en": "Apa Itu 'Peltier Effect'?", "id": "Efek Dingin Saat Arus Lewat Logam Beda." },
  { "en": "Apa Itu 'Seebeck Effect'?", "id": "Beda Panas Hasilkan Listrik Pada Logam." },
  { "en": "Apa Itu 'Hall Effect'?", "id": "Medan Magnet Belokkan Arus Dalam Konduktor." },
  { "en": "Apa Itu 'Piezoelectric Effect'?", "id": "Tekanan Mekanik Hasilkan Listrik Pada Kristal." },
  { "en": "Apa Itu 'Photoelectric Effect'?", "id": "Cahaya Hasilkan Elektron Pada Permukaan Logam." },
  { "en": "Apa Itu 'Mesh Topology'?", "id": "Jaringan Dimana Semua Alat Saling Terhubung." },
  { "en": "Apa Itu 'Star Topology'?", "id": "Jaringan Terpusat Satu Hub Banyak Cabang." },
  { "en": "Apa Itu 'Bus Topology'?", "id": "Jaringan Satu Kabel Utama Dipakai Bersama." },
  { "en": "Apa Itu 'Ring Topology'?", "id": "Jaringan Kabel Melingkar Tanpa Ujung." },
  { "en": "Apa Itu 'Repeater'?", "id": "Penguat Sinyal Untuk Jarak Jauh." },
  { "en": "Apa Itu 'Hub'?", "id": "Penyambung Jaringan Sederhana Sebar Data." },
  { "en": "Apa Itu 'Switch' (Network)?", "id": "Penyambung Jaringan Pintar Kirim Tepat Sasaran." },
  { "en": "Apa Itu 'Router'?", "id": "Pengatur Lalu Lintas Data Antar Jaringan." },
  { "en": "Apa Itu 'Gateway'?", "id": "Pintu Gerbang Penghubung Beda Protokol." },
  { "en": "Apa Itu 'MAC Address'?", "id": "Alamat Fisik Unik Kartu Jaringan." },
  { "en": "Apa Itu 'IP Address'?", "id": "Alamat Logika Alat Di Jaringan Internet." },
  { "en": "Apa Itu 'Subnet Mask'?", "id": "Pemisah Antara Alamat Jaringan Dan Alat." },
  { "en": "Apa Itu 'DHCP'?", "id": "Pemberi Alamat IP Secara Otomatis." },
  { "en": "Apa Itu 'DNS'?", "id": "Penerjemah Nama Website Jadi Alamat IP." },
  { "en": "Apa Itu 'TCP'?", "id": "Protokol Kirim Data Terjamin Sampai." },
  { "en": "Apa Itu 'UDP'?", "id": "Protokol Kirim Data Cepat Tanpa Garansi." },
  { "en": "Apa Itu 'HTTP'?", "id": "Protokol Web Browser Minta Halaman." },
  { "en": "Apa Itu 'MQTT'?", "id": "Protokol Ringan Khusus Alat IoT." },
  { "en": "Apa Itu 'CoAP'?", "id": "Protokol Web Untuk Alat Sangat Kecil." },
  { "en": "Apa Itu 'Payload'?", "id": "Data Inti Yang Dikirim Dalam Paket." },
  { "en": "Apa Itu 'Header' (Data)?", "id": "Informasi Awal Paket Data Pengiriman." },
  { "en": "Apa Itu 'Footer' (Data)?", "id": "Tanda Akhir Dan Cek Error Paket." },
  { "en": "Apa Itu 'Checksum'?", "id": "Nilai Hitungan Untuk Cek Keaslian Data." },
  { "en": "Apa Itu 'Parity Bit'?", "id": "Bit Tambahan Pendeteksi Error Sederhana." },
  { "en": "Apa Itu 'CRC' (Cyclic Redundancy Check)?", "id": "Metode Cek Error Data Paling Umum." },
  { "en": "Apa Itu 'Forward Error Correction'?", "id": "Data Tambahan Untuk Perbaiki Error Otomatis." },
  { "en": "Apa Itu 'Hamming Code'?", "id": "Kode Koreksi Kesalahan Bit Data." },
  { "en": "Apa Itu 'Noise Margin'?", "id": "Batas Toleransi Sinyal Terhadap Gangguan." },
  { "en": "Apa Itu 'Signal Integrity'?", "id": "Kualitas Sinyal Digital Tetap Bagus." },
  { "en": "Apa Itu 'Crosstalk'?", "id": "Bocoran Sinyal Antar Kabel Berdekatan." },
  { "en": "Apa Itu 'Impedance Mismatch'?", "id": "Hambatan Tidak Pas Bikin Sinyal Mantul." },
  { "en": "Apa Itu 'Transmission Line'?", "id": "Kabel Khusus Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu 'Characteristic Impedance'?", "id": "Hambatan Alami Kabel Coaxial Panjang." },
  { "en": "Apa Itu 'Skin Depth'?", "id": "Kedalaman Arus Mengalir Di Permukaan Kabel." },
  { "en": "Apa Itu 'Dielectric Constant'?", "id": "Nilai Isolasi Bahan Penyekat Kapasitor." },
  { "en": "Apa Itu 'Permeability'?", "id": "Kemampuan Bahan Menyerap Medan Magnet." },
  { "en": "Apa Itu 'Permittivity'?", "id": "Kemampuan Bahan Menyimpan Medan Listrik." },
  { "en": "Apa Itu 'Magnetic Saturation'?", "id": "Inti Magnet Penuh Tak Bisa Simpan Lagi." },
  { "en": "Apa Itu 'Doppler Effect'?", "id": "Perubahan Frekuensi Akibat Gerak Sumber Suara." },
  { "en": "Apa Itu 'Radar Cross Section'?", "id": "Ukuran Pantulan Sinyal Radar Suatu Benda." },
  { "en": "Apa Itu 'Phased Array Antenna'?", "id": "Antena Radar Canggih Tanpa Gerak Fisik." },
  { "en": "Apa Itu 'LNB' (Parabola)?", "id": "Penangkap Sinyal Parabola Paling Depan." },
  { "en": "Apa Itu 'Transponder'?", "id": "Alat Terima Dan Kirim Ulang Sinyal." },
  { "en": "Apa Itu 'Uplink'?", "id": "Kirim Sinyal Dari Bumi Ke Satelit." },
  { "en": "Apa Itu 'Downlink'?", "id": "Terima Sinyal Dari Satelit Ke Bumi." },
  { "en": "Apa Itu 'Geostationary Orbit'?", "id": "Satelit Diam Di Atas Titik Bumi." },
  { "en": "Apa Itu 'Low Earth Orbit'?", "id": "Satelit Terbang Rendah Kelilingi Bumi Cepat." },
  { "en": "Apa Itu 'VSAT'?", "id": "Stasiun Bumi Kecil Penerima Sinyal Satelit." },
  { "en": "Apa Itu 'Supernode'?", "id": "Gabungan Dua Node Ada Sumber Tegangan." },
  { "en": "Apa Itu 'Supermesh'?", "id": "Gabungan Dua Loop Ada Sumber Arus." },
  { "en": "Apa Itu 'Source Transformation'?", "id": "Ubah Sumber Tegangan Jadi Sumber Arus." },
  { "en": "Apa Itu 'Maximum Power Transfer'?", "id": "Syarat Daya Terkirim Paling Besar Efisien." },
  { "en": "Apa Itu 'Reciprocity Theorem'?", "id": "Arus Dan Tegangan Bisa Tukar Posisi." },
  { "en": "Apa Itu 'Millman Theorem'?", "id": "Cara Hitung Tegangan Paralel Banyak Sumber." },
  { "en": "Apa Itu 'Delta-Wye Transform'?", "id": "Ubah Rangkaian Segitiga Jadi Bintang." },
  { "en": "Apa Itu 'Complex Power'?", "id": "Daya Total Dalam Bilangan Imajiner." },
  { "en": "Apa Itu 'Phasor Diagram'?", "id": "Gambar Panah Sudut Tegangan Dan Arus." },
  { "en": "Apa Itu 'RMS Voltage'?", "id": "Nilai Efektif Tegangan Arus Bolak Balik." },
  { "en": "Apa Itu 'CRT Tube'?", "id": "Tabung Layar Kaca TV Model Lama." },
  { "en": "Apa Itu 'Electron Gun'?", "id": "Penembak Sinar Elektron Di Dalam Tabung." },
  { "en": "Apa Itu 'Deflection Coil'?", "id": "Kumparan Pembelok Sinar Elektron TV Tabung." },
  { "en": "Apa Itu 'Vacuum Tube'?", "id": "Komponen Penguat Sinyal Kuno Sebelum Transistor." },
  { "en": "Apa Itu 'Nixie Tube'?", "id": "Tabung Kaca Tampilan Angka Neon Oranye." },
  { "en": "Apa Itu 'VFD Display'?", "id": "Layar Hijau Terang Khas Audio Mobil." },
  { "en": "Apa Itu 'E-Ink Display'?", "id": "Layar Hemat Daya Mirip Kertas Koran." },
  { "en": "Apa Itu 'TFT LCD'?", "id": "Layar Warna Canggih Tiap Piksel Transistor." },
  { "en": "Apa Itu 'IPS Panel'?", "id": "Layar LCD Dilihat Dari Samping Jelas." },
  { "en": "Apa Itu 'Backlight Unit'?", "id": "Sumber Cahaya Di Belakang Layar LCD." },
  { "en": "Apa Itu 'Variable Frequency Drive'?", "id": "Pengatur Kecepatan Motor Listrik Industri." },
  { "en": "Apa Itu 'Soft Starter'?", "id": "Alat Haluskan Arus Awal Motor Besar." },
  { "en": "Apa Itu 'Servo Drive'?", "id": "Pengendali Motor Servo Presisi Tinggi." },
  { "en": "Apa Itu 'Encoder Incremental'?", "id": "Sensor Putaran Hitung Pulsa Relatif." },
  { "en": "Apa Itu 'Encoder Absolute'?", "id": "Sensor Putaran Tahu Posisi Pasti." },
  { "en": "Apa Itu 'Resolver'?", "id": "Sensor Sudut Motor Tahan Lingkungan Keras." },
  { "en": "Apa Itu 'Tacho Generator'?", "id": "Generator Kecil Penghasil Sinyal Kecepatan Putar." },
  { "en": "Apa Itu 'Proximity Sensor'?", "id": "Sensor Deteksi Benda Tanpa Sentuhan Fisik." },
  { "en": "Apa Itu 'Solenoid Valve'?", "id": "Kran Buka Tutup Kendali Listrik Magnet." },
  { "en": "Apa Itu 'Wheatstone Bridge'?", "id": "Rangkaian Ukur Hambatan Sangat Presisi." },
  { "en": "Apa Itu 'Kelvin Clip'?", "id": "Jepitan Kabel Ukur Empat Kawat Akurat." },
  { "en": "Apa Itu 'Megger Test'?", "id": "Uji Tahanan Isolasi Kabel Tegangan Tinggi." },
  { "en": "Apa Itu 'Earth Tester'?", "id": "Alat Ukur Kualitas Grounding Tanah." },
  { "en": "Apa Itu 'Lux Meter'?", "id": "Alat Ukur Intensitas Cahaya Ruangan." },
  { "en": "Apa Itu 'Anemometer'?", "id": "Alat Ukur Kecepatan Aliran Angin." },
  { "en": "Apa Itu 'Tachometer'?", "id": "Alat Ukur Kecepatan Putaran Mesin." },
  { "en": "Apa Itu 'Sound Level Meter'?", "id": "Alat Ukur Tingkat Kebisingan Suara." },
  { "en": "Apa Itu 'Thermal Camera'?", "id": "Kamera Pendeteksi Panas Kabel Korslet." },
  { "en": "Apa Itu 'Cable Tracer'?", "id": "Alat Lacak Kabel Putus Dalam Dinding." },
  { "en": "Apa Itu 'AWG Cable'?", "id": "Standar Ukuran Diameter Kabel Amerika." },
  { "en": "Apa Itu 'NYA Cable'?", "id": "Kabel Tembaga Tunggal Isolasi Satu Lapis." },
  { "en": "Apa Itu 'NYM Cable'?", "id": "Kabel Putih Isi Banyak Untuk Rumah." },
  { "en": "Apa Itu 'NYY Cable'?", "id": "Kabel Hitam Kuat Tanam Dalam Tanah." },
  { "en": "Apa Itu 'Fiber Optic Cable'?", "id": "Kabel Serat Kaca Hantar Sinyal Cahaya." },
  { "en": "Apa Itu 'Twisted Pair Cable'?", "id": "Kabel Telepon Plintir Kurangi Gangguan." },
  { "en": "Apa Itu 'Shielded Cable'?", "id": "Kabel Bungkus Logam Anti Gangguan." },
  { "en": "Apa Itu 'Busbar'?", "id": "Batang Tembaga Penyalur Arus Panel Listrik." },
  { "en": "Apa Itu 'NiCd Battery'?", "id": "Baterai Isi Ulang Jadul Beracun." },
  { "en": "Apa Itu 'NiMH Battery'?", "id": "Baterai Isi Ulang Ramah Lingkungan." },
  { "en": "Apa Itu 'LiFePO4 Battery'?", "id": "Baterai Lithium Besi Fosfat Sangat Awet." },
  { "en": "Apa Itu 'Lead Acid Battery'?", "id": "Baterai Aki Mobil Isi Air Zuur." },
  { "en": "Apa Itu 'Deep Cycle Battery'?", "id": "Aki Tahan Kuras Sampai Hampir Habis." },
  { "en": "Apa Itu 'Gel Battery'?", "id": "Aki Kering Isi Gel Bebas Perawatan." },
  { "en": "Apa Itu 'Button Cell'?", "id": "Baterai Kancing Pipih Untuk Jam Tangan." },
  { "en": "Apa Itu 'C-Rate Battery'?", "id": "Ukuran Kecepatan Cas Atau Kuras Baterai." },
  { "en": "Apa Itu 'Self Discharge'?", "id": "Baterai Berkurang Sendiri Saat Disimpan." },
  { "en": "Apa Itu 'Battery Balancing'?", "id": "Menyamakan Tegangan Tiap Sel Baterai." },
  { "en": "Apa Itu 'Impedance Audio'?", "id": "Hambatan Speaker Terhadap Arus Musik." },
  { "en": "Apa Itu 'Mono Audio'?", "id": "Suara Satu Saluran Kiri Kanan Sama." },
  { "en": "Apa Itu 'Stereo Audio'?", "id": "Suara Dua Saluran Kiri Kanan Beda." },
  { "en": "Apa Itu 'Surround Sound'?", "id": "Suara Keliling Penonton Seperti Bioskop." },
  { "en": "Apa Itu 'Subwoofer'?", "id": "Speaker Khusus Suara Bass Rendah." },
  { "en": "Apa Itu 'Tweeter'?", "id": "Speaker Khusus Suara Treble Tinggi." },
  { "en": "Apa Itu 'Woofer'?", "id": "Speaker Untuk Suara Nada Menengah." },
  { "en": "Apa Itu 'Crossover'?", "id": "Pemisah Nada Ke Speaker Yang Cocok." },
  { "en": "Apa Itu 'Decibel' (dB)?", "id": "Satuan Ukuran Kekerasan Suara Audio." },
  { "en": "Apa Itu 'Phantom Power'?", "id": "Daya Listrik Tambahan Untuk Mic Kondensor." },
  { "en": "Apa Itu 'SR Latch'?", "id": "Rangkaian Pengunci Data Paling Dasar." },
  { "en": "Apa Itu 'D Flip-Flop'?", "id": "Penyimpan Satu Bit Data Ikut Detak." },
  { "en": "Apa Itu 'JK Flip-Flop'?", "id": "Penyimpan Data Serbaguna Bisa Toggle." },
  { "en": "Apa Itu 'T Flip-Flop'?", "id": "Penyimpan Data Berubah Tiap Detak." },
  { "en": "Apa Itu 'Counter Up'?", "id": "Rangkaian Penghitung Naik Nol Ke Sembilan." },
  { "en": "Apa Itu 'Counter Down'?", "id": "Rangkaian Penghitung Mundur Sembilan Ke Nol." },
  { "en": "Apa Itu 'Shift Register'?", "id": "Rangkaian Geser Data Samping Kiri Kanan." },
  { "en": "Apa Itu 'Ring Counter'?", "id": "Penghitung Data Berputar Melingkar." },
  { "en": "Apa Itu 'Johnson Counter'?", "id": "Penghitung Putar Dengan Kode Khusus." },
  { "en": "Apa Itu 'Frequency Divider'?", "id": "Pembagi Frekuensi Detak Jadi Lebih Lambat." },
  { "en": "Apa Itu 'Crimping Tool'?", "id": "Tang Press Pasang Kepala Kabel." },
  { "en": "Apa Itu 'Solder Sucker'?", "id": "Pompa Sedot Timah Saat Bongkar Komponen." },
  { "en": "Apa Itu 'Helping Hand'?", "id": "Penjepit PCB Tangan Ketiga Saat Solder." },
  { "en": "Apa Itu 'Flux Pen'?", "id": "Spidol Cairan Bantu Timah Nempel." },
  { "en": "Apa Itu 'Heat Gun'?", "id": "Pemanas Udara Untuk Selang Bakar." },
  { "en": "Apa Itu 'Alligator Clip'?", "id": "Jepit Buaya Untuk Sambungan Sementara." },
  { "en": "Apa Itu 'Jumper Wire'?", "id": "Kabel Tusuk Penghubung Rangkaian Uji." },
  { "en": "Apa Itu 'Antenna Polarization'?", "id": "Arah Getaran Gelombang Listrik Sinyal Radio." },
  { "en": "Apa Itu 'Vertical Polarization'?", "id": "Gelombang Sinyal Tegak Lurus Terhadap Tanah." },
  { "en": "Apa Itu 'Horizontal Polarization'?", "id": "Gelombang Sinyal Sejajar Rata Dengan Tanah." },
  { "en": "Apa Itu 'Circular Polarization'?", "id": "Gelombang Sinyal Berputar Spiral Saat Merambat." },
  { "en": "Apa Itu 'Fresnel Zone'?", "id": "Area Lonjong Ruang Bebas Antar Antena." },
  { "en": "Apa Itu 'Line of Sight'?", "id": "Jalur Pandang Lurus Tanpa Halangan Fisik." },
  { "en": "Apa Itu 'Path Loss'?", "id": "Sinyal Melemah Karena Jarak Tempuh Udara." },
  { "en": "Apa Itu 'Multipath Propagation'?", "id": "Sinyal Sampai Lewat Banyak Jalur Pantulan." },
  { "en": "Apa Itu 'Fading'?", "id": "Sinyal Naik Turun Akibat Pantulan Ganda." },
  { "en": "Apa Itu 'Link Budget'?", "id": "Hitungan Total Daya Sinyal Kirim Terima." },
  { "en": "Apa Itu 'Repeater'?", "id": "Alat Terima Dan Pancar Ulang Sinyal." },
  { "en": "Apa Itu 'Duplexer'?", "id": "Pemisah Sinyal Kirim Terima Satu Antena." },
  { "en": "Apa Itu 'Diplexer'?", "id": "Pemisah Dua Frekuensi Beda Satu Kabel." },
  { "en": "Apa Itu 'Splitter RF'?", "id": "Pembagi Sinyal Antena Ke Banyak TV." },
  { "en": "Apa Itu 'Coupler RF'?", "id": "Pencuplik Sedikit Sinyal Untuk Pengukuran." },
  { "en": "Apa Itu 'Attenuator'?", "id": "Peredam Kekuatan Sinyal Yang Terlalu Besar." },
  { "en": "Apa Itu 'Terminator 50 Ohm'?", "id": "Penutup Ujung Kabel Agar Sinyal Balik." },
  { "en": "Apa Itu 'LNA' (Low Noise Amplifier)?", "id": "Penguat Sinyal Lemah Dengan Sedikit Desis." },
  { "en": "Apa Itu 'PA' (Power Amplifier)?", "id": "Penguat Akhir Daya Besar Ke Antena." },
  { "en": "Apa Itu 'VCO' (Voltage Controlled Oscillator)?", "id": "Pembangkit Frekuensi Diatur Oleh Tegangan Masuk." },
  { "en": "Apa Itu 'TCXO'?", "id": "Osilator Kristal Stabil Tahan Perubahan Suhu." },
  { "en": "Apa Itu 'OCXO'?", "id": "Osilator Kristal Dalam Oven Sangat Stabil." },
  { "en": "Apa Itu 'Saw Filter'?", "id": "Penyaring Frekuensi Gelombang Akustik Permukaan." },
  { "en": "Apa Itu 'Ceramic Filter'?", "id": "Penyaring Frekuensi Radio Bahan Keramik Murah." },
  { "en": "Apa Itu 'Resistor Shunt'?", "id": "Resistor Nilai Rendah Untuk Ukur Arus." },
  { "en": "Apa Itu 'Resistor Network'?", "id": "Banyak Resistor Dalam Satu Kemasan Sisir." },
  { "en": "Apa Itu 'Resistor Wirewound'?", "id": "Resistor Lilitan Kawat Tahan Daya Besar." },
  { "en": "Apa Itu 'Resistor Metal Film'?", "id": "Resistor Presisi Tinggi Desis Rendah Biru." },
  { "en": "Apa Itu 'Potentiometer Multi-turn'?", "id": "Putaran Banyak Kali Agar Nilai Presisi." },
  { "en": "Apa Itu 'Trimmer Potentiometer'?", "id": "Resistor Variabel Kecil Setel Pakai Obeng." },
  { "en": "Apa Itu 'Capacitor Electrolytic'?", "id": "Kapasitor Tabung Polaritas Plus Minus Cairan." },
  { "en": "Apa Itu 'Capacitor Tantalum'?", "id": "Kapasitor Kotak Kecil Stabil Tapi Mahal." },
  { "en": "Apa Itu 'Capacitor Ceramic'?", "id": "Kapasitor Piringan Coklat Untuk Frekuensi Tinggi." },
  { "en": "Apa Itu 'Capacitor Mylar'?", "id": "Kapasitor Plastik Hijau Umum Rangkaian Audio." },
  { "en": "Apa Itu 'Capacitor Variable'?", "id": "Kapasitor Putar Untuk Tuning Gelombang Radio." },
  { "en": "Apa Itu 'Supercapacitor'?", "id": "Kapasitor Raksasa Penyimpan Energi Mirip Baterai." },
  { "en": "Apa Itu 'Inductor Air Core'?", "id": "Gulungan Kawat Kosong Tanpa Inti Besi." },
  { "en": "Apa Itu 'Inductor Ferrite Core'?", "id": "Gulungan Kawat Inti Hitam Frekuensi Tinggi." },
  { "en": "Apa Itu 'Inductor Toroid'?", "id": "Gulungan Inti Cincin Efisiensi Medan Magnet." },
  { "en": "Apa Itu 'Choke Coil'?", "id": "Inductor Penahan Frekuensi Tinggi Lewat Kabel." },
  { "en": "Apa Itu 'Transformer Step-Up'?", "id": "Trafo Penaik Tegangan Listrik Arus AC." },
  { "en": "Apa Itu 'Transformer Step-Down'?", "id": "Trafo Penurun Tegangan Listrik Arus AC." },
  { "en": "Apa Itu 'Isolation Transformer'?", "id": "Trafo Pemisah Keamanan Antara Dua Rangkaian." },
  { "en": "Apa Itu 'Autotransformer'?", "id": "Trafo Satu Gulungan Hemat Tempat Tembaga." },
  { "en": "Apa Itu 'Current Transformer' (CT)?", "id": "Trafo Sensor Untuk Mengukur Arus Besar." },
  { "en": "Apa Itu 'Potential Transformer' (PT)?", "id": "Trafo Sensor Untuk Mengukur Tegangan Tinggi." },
  { "en": "Apa Itu 'Diode Rectifier'?", "id": "Dioda Penyearah Listrik AC Jadi DC." },
  { "en": "Apa Itu 'Diode Schottky'?", "id": "Dioda Sakelar Cepat Tegangan Jatuh Rendah." },
  { "en": "Apa Itu 'Diode Zener'?", "id": "Dioda Penstabil Tegangan Pada Nilai Tertentu." },
  { "en": "Apa Itu 'Diode Varactor'?", "id": "Dioda Kapasitas Berubah Sesuai Tegangan Bias." },
  { "en": "Apa Itu 'Diode Tunnel'?", "id": "Dioda Efek Kuantum Untuk Osilator Cepat." },
  { "en": "Apa Itu 'LED SMD'?", "id": "Lampu Indikator Tempel Ukuran Sangat Kecil." },
  { "en": "Apa Itu 'LED COB'?", "id": "Lampu Chip Besar Terang Banyak Titik." },
  { "en": "Apa Itu 'Transistor BJT'?", "id": "Transistor Biasa Dikendalikan Oleh Arus Basis." },
  { "en": "Apa Itu 'Transistor FET'?", "id": "Transistor Efisien Dikendalikan Oleh Tegangan Gate." },
  { "en": "Apa Itu 'MOSFET Power'?", "id": "Transistor Sakelar Kuat Untuk Arus Besar." },
  { "en": "Apa Itu 'IGBT'?", "id": "Transistor Daya Tinggi Gabungan BJT MOSFET." },
  { "en": "Apa Itu 'Thyristor'?", "id": "Sakelar Semikonduktor Kunci Arus Satu Arah." },
  { "en": "Apa Itu 'Triac'?", "id": "Sakelar Semikonduktor Kunci Arus Dua Arah." },
  { "en": "Apa Itu 'Darlington Pair'?", "id": "Dua Transistor Seri Untuk Penguatan Ganda." },
  { "en": "Apa Itu 'Current Mirror'?", "id": "Rangkaian Penyalin Arus Agar Tetap Sama." },
  { "en": "Apa Itu 'Differential Pair'?", "id": "Pasangan Transistor Penguat Selisih Sinyal Input." },
  { "en": "Apa Itu 'Push-Pull Amplifier'?", "id": "Sepasang Transistor Kerja Bergantian Tarik Dorong." },
  { "en": "Apa Itu 'Open Collector'?", "id": "Output Transistor Mengambang Butuh Resistor Pull-up." },
  { "en": "Apa Itu 'Tri-State Logic'?", "id": "Logika Digital Tiga Kondisi Termasuk Putus." },
  { "en": "Apa Itu 'Schmitt Trigger'?", "id": "Gerbang Logika Histeresis Bersihkan Sinyal Berisik." },
  { "en": "Apa Itu 'Flip-Flop JK'?", "id": "Penyimpan Data Satu Bit Paling Fleksibel." },
  { "en": "Apa Itu 'Shift Register'?", "id": "Chip Geser Data Serial Jadi Paralel." },
  { "en": "Apa Itu 'Decade Counter'?", "id": "Penghitung Pulsa Sepuluh Langkah Lalu Reset." },
  { "en": "Apa Itu 'Multiplexer'?", "id": "Sakelar Pemilih Satu Data Dari Banyak." },
  { "en": "Apa Itu 'Demultiplexer'?", "id": "Sakelar Pembagi Satu Data Ke Banyak." },
  { "en": "Apa Itu 'Encoder'?", "id": "Pengubah Tombol Jadi Kode Biner Digital." },
  { "en": "Apa Itu 'Decoder'?", "id": "Penerjemah Kode Biner Ke Tampilan Layar." },
  { "en": "Apa Itu 'Grid Tie Inverter'?", "id": "Inverter Surya Sinkron Dengan Listrik PLN." },
  { "en": "Apa Itu 'Off Grid Inverter'?", "id": "Inverter Surya Mandiri Pakai Baterai Sendiri." },
  { "en": "Apa Itu 'Hybrid Inverter'?", "id": "Inverter Surya Bisa Baterai Dan PLN." },
  { "en": "Apa Itu 'MPPT Controller'?", "id": "Pengisi Baterai Surya Paling Efisien Canggih." },
  { "en": "Apa Itu 'PWM Controller'?", "id": "Pengisi Baterai Surya Murah Sistem Denyut." },
  { "en": "Apa Itu 'Battery Balancing'?", "id": "Menyamakan Tegangan Sel Baterai Agar Awet." },
  { "en": "Apa Itu 'Depth of Discharge'?", "id": "Batas Aman Energi Baterai Boleh Dipakai." },
  { "en": "Apa Itu 'Cycle Life'?", "id": "Umur Baterai Dihitung Dari Jumlah Cas." },
  { "en": "Apa Itu 'Busbar Tembaga'?", "id": "Plat Logam Penyalur Arus Panel Listrik." },
  { "en": "Apa Itu 'Cable Lug' (Skun)?", "id": "Sepatu Kabel Logam Ujung Sambungan Baut." },
  { "en": "Apa Itu 'Heat Shrink Tube'?", "id": "Selang Bakar Isolasi Sambungan Kabel Rapi." },
  { "en": "Apa Itu 'Cable Gland'?", "id": "Pengunci Kabel Masuk Box Tahan Air." },
  { "en": "Apa Itu 'Din Rail'?", "id": "Rel Besi Standar Dudukan Komponen Panel." },
  { "en": "Apa Itu 'Wire Duct'?", "id": "Jalur Kabel Berlubang Dalam Panel Listrik." },
  { "en": "Apa Itu 'Pilot Lamp'?", "id": "Lampu Indikator Warna Warni Status Mesin." },
  { "en": "Apa Itu 'Selector Switch'?", "id": "Sakelar Putar Untuk Memilih Mode Operasi." },
  { "en": "Apa Itu 'Emergency Stop'?", "id": "Tombol Jamur Merah Matikan Mesin Darurat." },
  { "en": "Apa Itu 'Limit Switch'?", "id": "Sakelar Mekanik Pembatas Gerakan Mesin Robot." },
  { "en": "Apa Itu 'Proximity Inductive'?", "id": "Sensor Deteksi Logam Tanpa Sentuhan Fisik." },
  { "en": "Apa Itu 'Proximity Capacitive'?", "id": "Sensor Deteksi Benda Non-Logam Tanpa Sentuh." },
  { "en": "Apa Itu 'Photoelectric Sensor'?", "id": "Sensor Deteksi Benda Memotong Sinar Cahaya." },
  { "en": "Apa Itu 'Load Cell'?", "id": "Sensor Berat Batang Besi Timbangan Digital." },
  { "en": "Apa Itu 'Rotary Encoder'?", "id": "Sensor Putaran Digital 360 Derajat Penuh." },
  { "en": "Apa Itu 'Solenoid Valve'?", "id": "Kran Buka Tutup Air Kendali Listrik." },
  { "en": "Apa Itu 'Linear Actuator'?", "id": "Motor Pendorong Batang Gerak Maju Mundur." },
  { "en": "Apa Itu 'Stepper Motor'?", "id": "Motor Gerak Sudut Langkah Demi Langkah." },
  { "en": "Apa Itu 'Servo Motor'?", "id": "Motor Pintar Bisa Diatur Posisi Sudutnya." },
  { "en": "Apa Itu 'Brushless DC Motor'?", "id": "Motor Cepat Efisien Tanpa Sikat Arang." },
  { "en": "Apa Itu 'VFD' (Inverter Motor)?", "id": "Alat Pengatur Kecepatan Motor Listrik Industri." },
  { "en": "Apa Itu 'Stator Winding'?", "id": "Gulungan Kawat Tembaga Bagian Diam Motor." },
  { "en": "Apa Itu 'Rotor Winding'?", "id": "Gulungan Kawat Tembaga Bagian Putar Motor." },
  { "en": "Apa Itu 'Commutator Segment'?", "id": "Lempeng Tembaga Pembalik Arus Motor DC." },
  { "en": "Apa Itu 'Carbon Brush'?", "id": "Sikat Arang Gesek Penyalur Arus Listrik." },
  { "en": "Apa Itu 'Slip Ring'?", "id": "Cincin Logam Penyalur Arus Rotor Berputar." },
  { "en": "Apa Itu 'Armature Reaction'?", "id": "Gangguan Medan Magnet Akibat Beban Motor." },
  { "en": "Apa Itu 'Back EMF'?", "id": "Tegangan Lawan Yang Dihasilkan Putaran Motor." },
  { "en": "Apa Itu 'Eddy Current Loss'?", "id": "Energi Hilang Jadi Panas Di Inti Besi." },
  { "en": "Apa Itu 'Hysteresis Loop'?", "id": "Grafik Rugi Magnetik Pada Inti Trafo." },
  { "en": "Apa Itu 'Copper Loss'?", "id": "Daya Hilang Akibat Hambatan Kawat Tembaga." },
  { "en": "Apa Itu 'Iron Loss'?", "id": "Daya Hilang Pada Inti Besi Magnetik." },
  { "en": "Apa Itu 'Efficiency' (Eta)?", "id": "Perbandingan Daya Keluar Dan Daya Masuk." },
  { "en": "Apa Itu 'Power Factor Correction'?", "id": "Perbaikan Efisiensi Daya Pakai Kapasitor Bank." },
  { "en": "Apa Itu 'Capacitor Bank'?", "id": "Kumpulan Kapasitor Besar Perbaiki Faktor Daya." },
  { "en": "Apa Itu 'Harmonic Distortion'?", "id": "Cacat Gelombang Listrik Akibat Beban Elektronik." },
  { "en": "Apa Itu 'Total Harmonic Distortion'?", "id": "Persentase Total Cacat Gelombang Listrik." },
  { "en": "Apa Itu 'Skin Effect'?", "id": "Arus AC Lewat Kulit Luar Kabel." },
  { "en": "Apa Itu 'Proximity Effect'?", "id": "Arus Terdesak Kabel Lain Yang Berdekatan." },
  { "en": "Apa Itu 'Busbar Trunking'?", "id": "Sistem Distribusi Daya Pakai Batang Tembaga." },
  { "en": "Apa Itu 'Cable Tray'?", "id": "Rak Besi Penyangga Jalur Kabel Banyak." },
  { "en": "Apa Itu 'Conduit Pipe'?", "id": "Pipa Pelindung Kabel Di Dinding Tembok." },
  { "en": "Apa Itu 'Cable Gland'?", "id": "Pengunci Kabel Masuk Panel Tahan Air." },
  { "en": "Apa Itu 'Distribution Board'?", "id": "Panel Pembagi Listrik Ke Beban Beban." },
  { "en": "Apa Itu 'Main Switchboard'?", "id": "Panel Induk Pusat Pembagi Listrik Gedung." },
  { "en": "Apa Itu 'Circuit Breaker'?", "id": "Sakelar Otomatis Putus Saat Arus Lebih." },
  { "en": "Apa Itu 'MCB 1 Phase'?", "id": "Sekring Otomatis Listrik Rumah Tangga Biasa." },
  { "en": "Apa Itu 'MCB 3 Phase'?", "id": "Sekring Otomatis Listrik Industri Tiga Fasa." },
  { "en": "Apa Itu 'MCCB'?", "id": "Sekring Kotak Besar Untuk Daya Tinggi." },
  { "en": "Apa Itu 'ACB' (Air Breaker)?", "id": "Pemutus Arus Utama Udara Gardu Listrik." },
  { "en": "Apa Itu 'VCB' (Vacuum Breaker)?", "id": "Pemutus Arus Ruang Hampa Tegangan Menengah." },
  { "en": "Apa Itu 'Earth Leakage'?", "id": "Arus Bocor Ke Tanah Yang Berbahaya." },
  { "en": "Apa Itu 'RCBO'?", "id": "Gabungan MCB Dan Pengaman Anti Setrum." },
  { "en": "Apa Itu 'Surge Arrester'?", "id": "Alat Pembuang Lonjakan Petir Ke Tanah." },
  { "en": "Apa Itu 'Ground Rod'?", "id": "Batang Tembaga Tanam Pembuangan Arus Tanah." },
  { "en": "Apa Itu 'Star Connection'?", "id": "Sambungan Tiga Fasa Titik Pusat Netral." },
  { "en": "Apa Itu 'Delta Connection'?", "id": "Sambungan Tiga Fasa Segitiga Tanpa Netral." },
  { "en": "Apa Itu 'Phase Voltage'?", "id": "Tegangan Antara Kabel Fasa Dan Netral." },
  { "en": "Apa Itu 'Line Voltage'?", "id": "Tegangan Antara Kabel Fasa Dan Fasa." },
  { "en": "Apa Itu 'Phase Sequence'?", "id": "Urutan Fasa R-S-T Yang Benar." },
  { "en": "Apa Itu 'Neutral Current'?", "id": "Arus Di Kabel Netral Akibat Ketidakseimbangan." },
  { "en": "Apa Itu 'Unbalanced Load'?", "id": "Beban Tiga Fasa Tidak Rata Besarnya." },
  { "en": "Apa Itu 'Logic Gate AND'?", "id": "Output Nyala Jika Semua Input Nyala." },
  { "en": "Apa Itu 'Logic Gate OR'?", "id": "Output Nyala Jika Salah Satu Nyala." },
  { "en": "Apa Itu 'Logic Gate NOT'?", "id": "Pembalik Kondisi Nyala Jadi Mati." },
  { "en": "Apa Itu 'Logic Gate XOR'?", "id": "Nyala Jika Input Saling Berbeda Kondisi." },
  { "en": "Apa Itu 'Flip-Flop'?", "id": "Rangkaian Dasar Penyimpan Memori Satu Bit." },
  { "en": "Apa Itu 'Multiplexer'?", "id": "Pemilih Satu Data Dari Banyak Jalur." },
  { "en": "Apa Itu 'Demultiplexer'?", "id": "Penyebar Satu Data Ke Banyak Jalur." },
  { "en": "Apa Itu 'Encoder'?", "id": "Pengubah Tombol Jadi Kode Biner." },
  { "en": "Apa Itu 'Decoder'?", "id": "Penerjemah Kode Biner Ke Tampilan." },
  { "en": "Apa Itu 'Half Adder'?", "id": "Rangkaian Penjumlah Dua Bit Biner Sederhana." },
  { "en": "Apa Itu 'Full Adder'?", "id": "Rangkaian Penjumlah Biner Dengan Sisa Simpan." },
  { "en": "Apa Itu 'Clock Signal'?", "id": "Detak Jantung Pengatur Kecepatan Digital." },
  { "en": "Apa Itu 'Rising Edge'?", "id": "Transisi Sinyal Dari Rendah Ke Tinggi." },
  { "en": "Apa Itu 'Falling Edge'?", "id": "Transisi Sinyal Dari Tinggi Ke Rendah." },
  { "en": "Apa Itu 'PWM Frequency'?", "id": "Kecepatan Kedip Sinyal Lebar Pulsa." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Kecepatan Transfer Data Serial Per Detik." },
  { "en": "Apa Itu 'Parity Bit'?", "id": "Bit Tambahan Untuk Cek Error Data." },
  { "en": "Apa Itu 'Start Bit'?", "id": "Tanda Awal Paket Data Mulai Dikirim." },
  { "en": "Apa Itu 'Stop Bit'?", "id": "Tanda Akhir Paket Data Selesai Dikirim." },
  { "en": "Apa Itu 'I2C Bus'?", "id": "Jalur Komunikasi Dua Kabel Banyak Chip." },
  { "en": "Apa Itu 'SPI Bus'?", "id": "Jalur Komunikasi Empat Kabel Sangat Cepat." },
  { "en": "Apa Itu 'UART'?", "id": "Komunikasi Serial Asinkron Paling Sederhana." },
  { "en": "Apa Itu 'CAN Bus'?", "id": "Jaringan Komunikasi Handal Standar Mobil." },
  { "en": "Apa Itu 'Modbus'?", "id": "Protokol Bahasa Komunikasi Mesin Industri." },
  { "en": "Apa Itu 'Ethernet Shield'?", "id": "Modul Kabel LAN Untuk Mikrokontroler." },
  { "en": "Apa Itu 'WiFi Module'?", "id": "Alat Sambung Internet Tanpa Kabel." },
  { "en": "Apa Itu 'Bluetooth Module'?", "id": "Alat Sambung HP Jarak Dekat Hemat." },
  { "en": "Apa Itu 'Resistor Color Code'?", "id": "Gelang Warna Penanda Nilai Hambatan." },
  { "en": "Apa Itu 'Tolerance Band'?", "id": "Gelang Emas Perak Batas Akurasi." },
  { "en": "Apa Itu 'Potentiometer'?", "id": "Resistor Putar Pengatur Nilai Hambatan." },
  { "en": "Apa Itu 'Trimpot'?", "id": "Potensiometer Kecil Setel Pakai Obeng." },
  { "en": "Apa Itu 'LDR'?", "id": "Resistor Berubah Nilai Kena Cahaya." },
  { "en": "Apa Itu 'Thermistor NTC'?", "id": "Hambatan Turun Jika Suhu Makin Panas." },
  { "en": "Apa Itu 'Ceramic Capacitor'?", "id": "Kapasitor Coklat Kecil Non Polar." },
  { "en": "Apa Itu 'Electrolytic Capacitor'?", "id": "Kapasitor Tabung Punya Kutub Plus Minus." },
  { "en": "Apa Itu 'Tantalum Capacitor'?", "id": "Kapasitor Kotak Kecil Stabil Dan Mahal." },
  { "en": "Apa Itu 'Inductor Coil'?", "id": "Gulungan Kawat Tembaga Simpan Magnet." },
  { "en": "Apa Itu 'Toroid Core'?", "id": "Inti Besi Bentuk Donat Efisiensi Tinggi." },
  { "en": "Apa Itu 'Diode Zener'?", "id": "Dioda Penstabil Tegangan Balik Tertentu." },
  { "en": "Apa Itu 'LED'?", "id": "Dioda Pemancar Cahaya Hemat Energi." },
  { "en": "Apa Itu 'Photodiode'?", "id": "Dioda Pengubah Cahaya Jadi Arus Listrik." },
  { "en": "Apa Itu 'Transistor NPN'?", "id": "Sakelar Aktif Sinyal Positif Masuk Basis." },
  { "en": "Apa Itu 'Transistor PNP'?", "id": "Sakelar Aktif Sinyal Negatif Masuk Basis." },
  { "en": "Apa Itu 'Heat Sink'?", "id": "Sirip Aluminium Pembuang Panas Komponen." },
  { "en": "Apa Itu 'Solder Wire'?", "id": "Kawat Timah Paduan Untuk Menyambung." },
  { "en": "Apa Itu 'Flux'?", "id": "Cairan Pembersih Agar Timah Nempel." },
  { "en": "Apa Itu 'Arduino Uno'?", "id": "Papan Mikrokontroler Pemula Paling Populer." },
  { "en": "Apa Itu 'Arduino Nano'?", "id": "Versi Kecil Uno Pas Di Breadboard." },
  { "en": "Apa Itu 'Arduino Mega'?", "id": "Papan Besar Banyak Pin I/O." },
  { "en": "Apa Itu 'ESP8266 NodeMCU'?", "id": "Mikrokontroler Murah Sudah Ada Wifi." },
  { "en": "Apa Itu 'ESP32'?", "id": "Chip Canggih Wifi Dan Bluetooth Gabung." },
  { "en": "Apa Itu 'Raspberry Pi'?", "id": "Komputer Mini Seukuran Kartu Kredit." },
  { "en": "Apa Itu 'STM32 Blue Pill'?", "id": "Mikrokontroler Murah Kinerja Lebih Cepat." },
  { "en": "Apa Itu 'Pin Digital'?", "id": "Kaki Chip Baca Tulis Sinyal 0-1." },
  { "en": "Apa Itu 'Pin Analog'?", "id": "Kaki Chip Baca Tegangan Naik Turun." },
  { "en": "Apa Itu 'Pin PWM'?", "id": "Kaki Chip Atur Terang Lampu LED." },
  { "en": "Apa Itu 'Bootloader'?", "id": "Program Awal Supaya Bisa Diisi Kode." },
  { "en": "Apa Itu 'Flash Memory'?", "id": "Tempat Simpan Program Agar Tidak Hilang." },
  { "en": "Apa Itu 'SRAM'?", "id": "Memori Kerja Sementara Saat Alat Nyala." },
  { "en": "Apa Itu 'EEPROM'?", "id": "Memori Data Kecil Awet Tanpa Listrik." },
  { "en": "Apa Itu 'Clock Speed'?", "id": "Kecepatan Mikrokontroler Menjalankan Perintah." },
  { "en": "Apa Itu 'Reset Button'?", "id": "Tombol Mulai Ulang Program Dari Awal." },
  { "en": "Apa Itu 'USB to TTL'?", "id": "Alat Sambung Chip Ke Laptop." },
  { "en": "Apa Itu 'Sensor DHT11'?", "id": "Sensor Suhu Dan Kelembaban Udara Murah." },
  { "en": "Apa Itu 'Sensor DHT22'?", "id": "Sensor Suhu Kelembaban Lebih Akurat." },
  { "en": "Apa Itu 'Sensor Ultrasonic'?", "id": "Pengukur Jarak Pakai Pantulan Suara." },
  { "en": "Apa Itu 'Sensor PIR'?", "id": "Pendeteksi Gerakan Tubuh Manusia Hangat." },
  { "en": "Apa Itu 'Sensor LDR'?", "id": "Pendeteksi Cahaya Gelap Jadi Terang." },
  { "en": "Apa Itu 'Sensor Soil Moisture'?", "id": "Pendeteksi Kelembaban Tanah Untuk Tanaman." },
  { "en": "Apa Itu 'Sensor Rain Drop'?", "id": "Pendeteksi Tetesan Air Hujan Turun." },
  { "en": "Apa Itu 'Sensor Gas MQ'?", "id": "Pendeteksi Kebocoran Gas Asap Udara." },
  { "en": "Apa Itu 'Sensor Flame'?", "id": "Pendeteksi Nyala Api Kebakaran." },
  { "en": "Apa Itu 'Sensor Vibration'?", "id": "Pendeteksi Getaran Atau Guncangan Mesin." },
  { "en": "Apa Itu 'Sensor Sound'?", "id": "Pendeteksi Suara Tepukan Tangan." },
  { "en": "Apa Itu 'Sensor Infrared'?", "id": "Pendeteksi Halangan Jarak Dekat." },
  { "en": "Apa Itu 'Sensor Color'?", "id": "Pendeteksi Warna Benda Di Depannya." },
  { "en": "Apa Itu 'Module Relay'?", "id": "Sakelar Listrik Kendali Mikrokontroler." },
  { "en": "Apa Itu 'Module Buzzer'?", "id": "Pembuat Suara Beep Alarm Sederhana." },
  { "en": "Apa Itu 'Module RTC'?", "id": "Jam Digital Akurat Simpan Waktu." },
  { "en": "Apa Itu 'Module SD Card'?", "id": "Tempat Baca Tulis Kartu Memori." },
  { "en": "Apa Itu 'Module Bluetooth HC-05'?", "id": "Alat Komunikasi Nirkabel Ke HP." },
  { "en": "Apa Itu 'Module WiFi ESP-01'?", "id": "Alat Tambah Koneksi Wifi Sederhana." },
  { "en": "Apa Itu 'Module GPS Neo'?", "id": "Penerima Lokasi Koordinat Dari Satelit." },
  { "en": "Apa Itu 'Module GSM SIM800'?", "id": "Alat Kirim SMS Dan Telepon Seluler." },
  { "en": "Apa Itu 'Module RFID RC522'?", "id": "Pembaca Kartu Tempel Identitas." },
  { "en": "Apa Itu 'Module Fingerprint'?", "id": "Pembaca Sidik Jari Untuk Kunci." },
  { "en": "Apa Itu 'LCD 16x2 I2C'?", "id": "Layar Teks Praktis Hemat Kabel." },
  { "en": "Apa Itu 'OLED 0.96 inch'?", "id": "Layar Grafis Kecil Tajam Biru." },
  { "en": "Apa Itu 'Seven Segment TM1637'?", "id": "Modul Layar Angka Jam Digital." },
  { "en": "Apa Itu 'Dot Matrix MAX7219'?", "id": "Layar Teks Berjalan Lampu Merah." },
  { "en": "Apa Itu 'Keypad 4x4'?", "id": "Tombol Angka Seperti Telepon Umum." },
  { "en": "Apa Itu 'Joystick Module'?", "id": "Tuas Pengendali Arah Robot Analog." },
  { "en": "Apa Itu 'Rotary Encoder'?", "id": "Tombol Putar Volume Digital." },
  { "en": "Apa Itu 'Servo SG90'?", "id": "Motor Penggerak Sudut Kecil Biru." },
  { "en": "Apa Itu 'Stepper 28BYJ-48'?", "id": "Motor Langkah Kecil Pelan Tapi Pasti." },
  { "en": "Apa Itu 'Driver L298N'?", "id": "Pengendali Motor DC Maju Mundur." },
  { "en": "Apa Itu 'Multimeter Analog'?", "id": "Alat Ukur Jarum Penunjuk Klasik." },
  { "en": "Apa Itu 'Multimeter Digital'?", "id": "Alat Ukur Layar Angka Akurat." },
  { "en": "Apa Itu 'Logic Analyzer'?", "id": "Alat Cek Sinyal Data Digital." },
  { "en": "Apa Itu 'LCR Meter'?", "id": "Pengukur Komponen Pasif Paling Detail." },
  { "en": "Apa Itu 'ESR Meter'?", "id": "Pendeteksi Elco Rusak Tanpa Copot." },
  { "en": "Apa Itu 'Transistor Tester'?", "id": "Alat Cek Kaki Komponen Otomatis." },
  { "en": "Apa Itu 'Clamp Meter'?", "id": "Tang Ukur Arus Tanpa Putus Kabel." },
  { "en": "Apa Itu 'Thermometer Infrared'?", "id": "Pengukur Suhu Tembak Tanpa Sentuh." },
  { "en": "Apa Itu 'Tachometer'?", "id": "Pengukur Kecepatan Putaran Mesin." },
  { "en": "Apa Itu 'Lux Meter'?", "id": "Pengukur Kekuatan Cahaya Lampu." },
  { "en": "Apa Itu 'Sound Level Meter'?", "id": "Pengukur Tingkat Kebisingan Suara." },
  { "en": "Apa Itu 'Anemometer'?", "id": "Pengukur Kecepatan Angin Kipas." },
  { "en": "Apa Itu 'Soldering Station'?", "id": "Solder Dengan Pengatur Suhu Stabil." },
  { "en": "Apa Itu 'Hot Air Gun'?", "id": "Pemanas Udara Lepas Chip SMD." },
  { "en": "Apa Itu 'Desoldering Pump'?", "id": "Penyedot Timah Cair Manual." },
  { "en": "Apa Itu 'Flux Pen'?", "id": "Spidol Cairan Bantu Solder." },
  { "en": "Apa Itu 'Wire Stripper'?", "id": "Tang Kupas Kabel Otomatis." },
  { "en": "Apa Itu 'Crimping Tool'?", "id": "Tang Press Skun Kabel." },
  { "en": "Apa Itu 'Breadboard'?", "id": "Papan Rakit Tanpa Solder." },
  { "en": "Apa Itu 'Jumper Male-Male'?", "id": "Kabel Tusuk Ke Tusuk." },
  { "en": "Apa Itu 'Jumper Male-Female'?", "id": "Kabel Tusuk Ke Lubang." },
  { "en": "Apa Itu 'Jumper Female-Female'?", "id": "Kabel Lubang Ke Lubang." },
  { "en": "Apa Itu 'Battery 18650'?", "id": "Baterai Laptop Umum Dipakai Robot." },
  { "en": "Apa Itu 'Battery Holder'?", "id": "Kotak Tempat Pasang Baterai." },
  { "en": "Apa Itu 'BMS Module'?", "id": "Papan Pengaman Baterai Lithium." },
  { "en": "Apa Itu 'Buck Converter'?", "id": "Modul Penurun Tegangan DC Efisien." },
  { "en": "Apa Itu 'Boost Converter'?", "id": "Modul Penaik Tegangan DC Efisien." },
  { "en": "Apa Itu 'Solar Panel'?", "id": "Papan Pengubah Cahaya Jadi Listrik." },
  { "en": "Apa Itu 'Solar Controller'?", "id": "Pengatur Pengisian Aki Tenaga Surya." },
  { "en": "Apa Itu 'Inverter DC-AC'?", "id": "Pengubah Aki Jadi Listrik PLN." },
  { "en": "Apa Itu 'Solid State Relay'?", "id": "Sakelar Elektronik Tanpa Suara." },
  { "en": "Apa Itu 'Limit Switch'?", "id": "Sakelar Batas Gerak Mekanik." },
  { "en": "Apa Itu 'Push Button'?", "id": "Tombol Tekan Balik Sendiri." },
  { "en": "Apa Itu 'Toggle Switch'?", "id": "Sakelar Batang Cetek Cetok." },
  { "en": "Apa Itu 'Rocker Switch'?", "id": "Sakelar Lampu Besar On Off." },
  { "en": "Apa Itu 'Dip Switch'?", "id": "Sakelar Baris Kecil Setting." },
  { "en": "Apa Itu 'Potentiometer'?", "id": "Resistor Putar Pengatur Nilai." },
  { "en": "Apa Itu 'Trimpot'?", "id": "Potensiometer Kecil Setel Obeng." },
  { "en": "Apa Itu 'Heatsink'?", "id": "Logam Pendingin Komponen Panas." },
  { "en": "Apa Itu 'Thermal Paste'?", "id": "Krim Penghantar Panas Pendingin." },
  { "en": "Apa Itu 'Fan 12V'?", "id": "Kipas Pendingin Alat Elektronik." },
  { "en": "Apa Itu 'Fuse Holder'?", "id": "Rumah Sekring Pengaman Kaca." },
  { "en": "Apa Itu 'Terminal Block'?", "id": "Konektor Kabel Jepit Baut." },
  { "en": "Apa Itu 'DC Jack'?", "id": "Colokan Listrik Adaptor Bulat." },
  { "en": "Apa Itu 'USB Connector'?", "id": "Colokan Data Standar Komputer." },
  { "en": "Apa Itu 'P-Type Semiconductor'?", "id": "Semikonduktor Kelebihan Lubang Muatan Positif." },
  { "en": "Apa Itu 'N-Type Semiconductor'?", "id": "Semikonduktor Kelebihan Elektron Muatan Negatif." },
  { "en": "Apa Itu 'PN Junction'?", "id": "Pertemuan Dua Jenis Semikonduktor Dioda." },
  { "en": "Apa Itu 'Depletion Region'?", "id": "Area Kosong Muatan Di Sambungan Dioda." },
  { "en": "Apa Itu 'Forward Bias'?", "id": "Arus Mengalir Maju Lewat Dioda." },
  { "en": "Apa Itu 'Reverse Bias'?", "id": "Arus Ditahan Mundur Oleh Dioda." },
  { "en": "Apa Itu 'Breakdown Voltage'?", "id": "Batas Tegangan Tembus Pertahanan Dioda." },
  { "en": "Apa Itu 'Avalanche Effect'?", "id": "Arus Bocor Besar Tiba Tiba." },
  { "en": "Apa Itu 'Transistor Saturation'?", "id": "Kondisi Transistor Sakelar Nyala Penuh." },
  { "en": "Apa Itu 'Transistor Cutoff'?", "id": "Kondisi Transistor Sakelar Mati Total." },
  { "en": "Apa Itu 'Transistor Active Region'?", "id": "Kondisi Transistor Sebagai Penguat Sinyal." },
  { "en": "Apa Itu 'Common Emitter'?", "id": "Rangkaian Penguat Transistor Paling Umum." },
  { "en": "Apa Itu 'Common Collector'?", "id": "Rangkaian Penyangga Arus Transistor." },
  { "en": "Apa Itu 'Common Base'?", "id": "Rangkaian Penguat Frekuensi Tinggi Transistor." },
  { "en": "Apa Itu 'TTL Logic'?", "id": "Chip Logika Standar Tegangan Lima Volt." },
  { "en": "Apa Itu 'CMOS Logic'?", "id": "Chip Logika Hemat Daya Modern." },
  { "en": "Apa Itu 'Fan-Out'?", "id": "Kemampuan Output Menggerakkan Banyak Input." },
  { "en": "Apa Itu 'Propagation Delay'?", "id": "Waktu Tunda Sinyal Lewat Gerbang." },
  { "en": "Apa Itu 'Tri-State Buffer'?", "id": "Gerbang Logika Dengan Kondisi Putus." },
  { "en": "Apa Itu 'Multiplexer 8-to-1'?", "id": "Pemilih Satu Data Dari Delapan." },
  { "en": "Apa Itu 'Demultiplexer 1-to-8'?", "id": "Penyebar Satu Data Ke Delapan." },
  { "en": "Apa Itu 'BCD Encoder'?", "id": "Pengubah Angka Biasa Jadi Biner." },
  { "en": "Apa Itu 'Seven Segment Decoder'?", "id": "Pengubah Biner Jadi Tampilan Angka." },
  { "en": "Apa Itu 'Flip-Flop RS'?", "id": "Penyimpan Memori Sederhana Dua Tombol." },
  { "en": "Apa Itu 'Flip-Flop D'?", "id": "Penyimpan Data Satu Bit Detak." },
  { "en": "Apa Itu 'Flip-Flop JK'?", "id": "Penyimpan Data Fleksibel Tanpa Error." },
  { "en": "Apa Itu 'Toggle Mode'?", "id": "Mode Sakelar Berubah Tiap Ditekan." },
  { "en": "Apa Itu 'Asynchronous Counter'?", "id": "Penghitung Pulsa Berurutan Tidak Serentak." },
  { "en": "Apa Itu 'Synchronous Counter'?", "id": "Penghitung Pulsa Serentak Bersamaan Detak." },
  { "en": "Apa Itu 'Shift Register SIPO'?", "id": "Geser Masuk Serial Keluar Paralel." },
  { "en": "Apa Itu 'Shift Register PISO'?", "id": "Geser Masuk Paralel Keluar Serial." },
  { "en": "Apa Itu 'Frequency Modulation'?", "id": "Ubah Frekuensi Sesuai Sinyal Informasi." },
  { "en": "Apa Itu 'Amplitude Modulation'?", "id": "Ubah Tinggi Sinyal Sesuai Informasi." },
  { "en": "Apa Itu 'Phase Modulation'?", "id": "Ubah Fasa Sinyal Sesuai Informasi." },
  { "en": "Apa Itu 'Pulse Width Modulation'?", "id": "Atur Lebar Pulsa Kendali Daya." },
  { "en": "Apa Itu 'Pulse Code Modulation'?", "id": "Ubah Sinyal Analog Jadi Kode Digital." },
  { "en": "Apa Itu 'Sampling Rate'?", "id": "Kecepatan Ambil Contoh Sinyal Audio." },
  { "en": "Apa Itu 'Nyquist Theorem'?", "id": "Syarat Minimal Sampling Dua Kali Lipat." },
  { "en": "Apa Itu 'Aliasing Noise'?", "id": "Gangguan Sinyal Akibat Sampling Lambat." },
  { "en": "Apa Itu 'Quantization Error'?", "id": "Kesalahan Pembulatan Nilai Digital Sinyal." },
  { "en": "Apa Itu 'Bit Depth'?", "id": "Jumlah Bit Penentu Kualitas Suara." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Kecepatan Kirim Simbol Data Serial." },
  { "en": "Apa Itu 'Bit Error Rate'?", "id": "Persentase Data Salah Saat Diterima." },
  { "en": "Apa Itu 'Signal to Noise Ratio'?", "id": "Perbandingan Sinyal Bersih Lawan Desis." },
  { "en": "Apa Itu 'Bandwidth'?", "id": "Lebar Pita Frekuensi Yang Dipakai." },
  { "en": "Apa Itu 'Spectrum Analyzer'?", "id": "Alat Lihat Kekuatan Tiap Frekuensi." },
  { "en": "Apa Itu 'Network Analyzer'?", "id": "Alat Ukur Karakteristik Kabel Antena." },
  { "en": "Apa Itu 'Impedance Matching'?", "id": "Penyesuaian Hambatan Agar Sinyal Maksimal." },
  { "en": "Apa Itu 'VSWR'?", "id": "Rasio Pantulan Sinyal Di Kabel." },
  { "en": "Apa Itu 'Return Loss'?", "id": "Daya Yang Memantul Kembali Ke Sumber." },
  { "en": "Apa Itu 'Insertion Loss'?", "id": "Daya Hilang Saat Alat Disambung." },
  { "en": "Apa Itu 'Antenna Gain'?", "id": "Penguatan Sinyal Fokus Satu Arah." },
  { "en": "Apa Itu 'Beamwidth'?", "id": "Lebar Sudut Pancaran Sinyal Antena." },
  { "en": "Apa Itu 'Polarization'?", "id": "Arah Getar Gelombang Elektromagnetik Antena." },
  { "en": "Apa Itu 'Resistor Tolerance'?", "id": "Batas Penyimpangan Nilai Hambatan Asli." },
  { "en": "Apa Itu 'Resistor Power Rating'?", "id": "Batas Daya Panas Resistor Terbakar." },
  { "en": "Apa Itu 'Capacitor Voltage Rating'?", "id": "Batas Tegangan Maksimal Kapasitor Meledak." },
  { "en": "Apa Itu 'Capacitor ESR'?", "id": "Hambatan Parasit Dalam Kapasitor Elco." },
  { "en": "Apa Itu 'Inductor Saturation'?", "id": "Inti Magnet Penuh Arus Tertahan." },
  { "en": "Apa Itu 'Quality Factor' (Q)?", "id": "Efisiensi Penyimpanan Energi Komponen Pasif." },
  { "en": "Apa Itu 'Transformer Efficiency'?", "id": "Perbandingan Daya Keluar Lawan Masuk." },
  { "en": "Apa Itu 'Copper Loss'?", "id": "Rugi Daya Panas Di Gulungan Kawat." },
  { "en": "Apa Itu 'Core Loss'?", "id": "Rugi Daya Panas Di Inti Besi." },
  { "en": "Apa Itu 'Eddy Current'?", "id": "Arus Liar Berputar Di Besi Trafo." },
  { "en": "Apa Itu 'Hysteresis Loss'?", "id": "Rugi Magnetik Akibat Gesekan Molekul." },
  { "en": "Apa Itu 'Skin Effect'?", "id": "Arus AC Lari Ke Pinggir Kabel." },
  { "en": "Apa Itu 'Proximity Effect'?", "id": "Arus Terdesak Kabel Tetangga Dekat." },
  { "en": "Apa Itu 'Dielectric Strength'?", "id": "Kekuatan Isolator Tahan Tegangan Tinggi." },
  { "en": "Apa Itu 'Permittivity'?", "id": "Kemampuan Bahan Simpan Medan Listrik." },
  { "en": "Apa Itu 'Permeability'?", "id": "Kemampuan Bahan Simpan Medan Magnet." },
  { "en": "Apa Itu 'Relay Contact'?", "id": "Titik Sambung Logam Sakelar Relay." },
  { "en": "Apa Itu 'Coil Voltage'?", "id": "Tegangan Kerja Kumparan Pemicu Relay." },
  { "en": "Apa Itu 'Contact Rating'?", "id": "Batas Arus Maksimal Sakelar Relay." },
  { "en": "Apa Itu 'Solid State Relay'?", "id": "Relay Tanpa Gerak Awet Cepat." },
  { "en": "Apa Itu 'Hall Effect Sensor'?", "id": "Sensor Deteksi Magnet Tanpa Sentuh." },
  { "en": "Apa Itu 'Proximity Sensor'?", "id": "Sensor Deteksi Logam Jarak Dekat." },
  { "en": "Apa Itu 'Thermocouple'?", "id": "Sensor Suhu Tinggi Industri Logam." },
  { "en": "Apa Itu 'RTD PT100'?", "id": "Sensor Suhu Presisi Berubah Hambatan." },
  { "en": "Apa Itu 'Load Cell'?", "id": "Sensor Berat Timbangan Digital Besi." },
  { "en": "Apa Itu 'Operational Amplifier'?", "id": "Penguat Sinyal Analog Serba Guna." },
  { "en": "Apa Itu 'Inverting Amplifier'?", "id": "Penguat Sinyal Dengan Keluaran Terbalik." },
  { "en": "Apa Itu 'Non-Inverting Amplifier'?", "id": "Penguat Sinyal Tanpa Membalik Fasa." },
  { "en": "Apa Itu 'Substation'?", "id": "Gardu Induk Pusat Distribusi Listrik." },
  { "en": "Apa Itu 'Busbar'?", "id": "Batang Tembaga Penyalur Arus Besar." },
  { "en": "Apa Itu 'Circuit Breaker'?", "id": "Sakelar Otomatis Pemutus Arus Lebih." },
  { "en": "Apa Itu 'Isolator Switch'?", "id": "Sakelar Pemisah Tanpa Beban Arus." },
  { "en": "Apa Itu 'Earthing System'?", "id": "Sistem Pembumian Pengaman Arus Bocor." },
  { "en": "Apa Itu 'Step-Up Transformer'?", "id": "Trafo Penaik Tegangan Listrik AC." },
  { "en": "Apa Itu 'Step-Down Transformer'?", "id": "Trafo Penurun Tegangan Listrik AC." },
  { "en": "Apa Itu 'Transmission Line'?", "id": "Kabel Penyalur Listrik Jarak Jauh." },
  { "en": "Apa Itu 'Distribution Line'?", "id": "Kabel Pembagi Listrik Ke Pelanggan." },
  { "en": "Apa Itu 'Insulator'?", "id": "Bahan Penyekat Isolasi Listrik Keramik." },
  { "en": "Apa Itu 'Conductor'?", "id": "Bahan Penghantar Listrik Seperti Tembaga." },
  { "en": "Apa Itu 'Regenerative Braking'?", "id": "Pengereman Yang Mengisi Ulang Baterai Mobil." },
  { "en": "Apa Itu 'EV Charger Level 1'?", "id": "Cas Mobil Listrik Pakai Colokan Rumah." },
  { "en": "Apa Itu 'EV Charger Level 2'?", "id": "Cas Mobil Listrik Lebih Cepat Rumahan." },
  { "en": "Apa Itu 'DC Fast Charging'?", "id": "Cas Super Cepat Langsung Ke Baterai." },
  { "en": "Apa Itu 'State of Health' (SOH)?", "id": "Kondisi Kesehatan Baterai Dibanding Saat Baru." },
  { "en": "Apa Itu 'State of Charge' (SOC)?", "id": "Sisa Persentase Energi Di Dalam Baterai." },
  { "en": "Apa Itu 'Battery Pack'?", "id": "Gabungan Modul Baterai Jadi Satu Kesatuan." },
  { "en": "Apa Itu 'Cell Balancing'?", "id": "Menyamakan Tegangan Tiap Sel Baterai." },
  { "en": "Apa Itu 'On-Board Charger'?", "id": "Alat Di Mobil Ubah AC Jadi DC." },
  { "en": "Apa Itu 'Silicon Wafer'?", "id": "Piringan Tipis Bahan Dasar Pembuat Chip." },
  { "en": "Apa Itu 'Photolithography'?", "id": "Proses Cetak Jalur Chip Pakai Cahaya." },
  { "en": "Apa Itu 'Etching'?", "id": "Proses Kikis Bahan Buat Pola Chip." },
  { "en": "Apa Itu 'Doping'?", "id": "Menambah Zat Asing Ubah Sifat Listrik." },
  { "en": "Apa Itu 'Moore's Law'?", "id": "Prediksi Jumlah Transistor Chip Naik Ganda." },
  { "en": "Apa Itu 'VLSI'?", "id": "Teknologi Jutaan Transistor Dalam Satu Chip." },
  { "en": "Apa Itu 'Clean Room'?", "id": "Ruangan Bebas Debu Untuk Buat Chip." },
  { "en": "Apa Itu 'Yield Rate'?", "id": "Persentase Chip Bagus Dari Satu Wafer." },
  { "en": "Apa Itu 'Buchholz Relay'?", "id": "Pengaman Trafo Minyak Dari Gelembung Gas." },
  { "en": "Apa Itu 'Tap Changer'?", "id": "Pengubah Lilitan Trafo Stabilkan Tegangan Output." },
  { "en": "Apa Itu 'Silica Gel Breather'?", "id": "Penyerap Uap Air Udara Masuk Trafo." },
  { "en": "Apa Itu 'Conservator Tank'?", "id": "Tangki Cadangan Minyak Di Atas Trafo." },
  { "en": "Apa Itu 'Bushing Trafo'?", "id": "Isolator Keramik Tempat Masuk Kabel Trafo." },
  { "en": "Apa Itu 'Differential Protection'?", "id": "Relay Bandingkan Arus Masuk Dan Keluar." },
  { "en": "Apa Itu 'Overcurrent Relay'?", "id": "Relay Kerja Saat Arus Melebihi Batas." },
  { "en": "Apa Itu 'Distance Relay'?", "id": "Relay Ukur Jarak Gangguan Lewat Impedansi." },
  { "en": "Apa Itu 'Scada System'?", "id": "Sistem Monitor Kendali Listrik Jarak Jauh." },
  { "en": "Apa Itu 'ALU' (Arithmetic Logic Unit)?", "id": "Bagian Prosesor Hitung Matematika Dan Logika." },
  { "en": "Apa Itu 'Control Unit'?", "id": "Pengatur Lalu Lintas Data Dalam Prosesor." },
  { "en": "Apa Itu 'Register File'?", "id": "Kumpulan Memori Cepat Di Dalam Prosesor." },
  { "en": "Apa Itu 'Cache Memory L1'?", "id": "Memori Paling Cepat Nempel Di Inti." },
  { "en": "Apa Itu 'Cache Memory L2'?", "id": "Memori Cepat Kedua Pendukung Prosesor." },
  { "en": "Apa Itu 'Instruction Set'?", "id": "Daftar Perintah Yang Dimengerti Oleh Prosesor." },
  { "en": "Apa Itu 'RISC Architecture'?", "id": "Desain Prosesor Perintah Sederhana Cepat." },
  { "en": "Apa Itu 'CISC Architecture'?", "id": "Desain Prosesor Perintah Kompleks Serba Bisa." },
  { "en": "Apa Itu 'Pipelining'?", "id": "Teknik Prosesor Kerjakan Banyak Tugas Beruntun." },
  { "en": "Apa Itu 'Clock Cycle'?", "id": "Satu Ketukan Detak Kerja Prosesor." },
  { "en": "Apa Itu 'Bus Width'?", "id": "Lebar Jalur Data Sekali Kirim." },
  { "en": "Apa Itu 'Address Bus'?", "id": "Jalur Penunjuk Alamat Memori Yang Tuju." },
  { "en": "Apa Itu 'Data Bus'?", "id": "Jalur Lewatnya Data Keluar Masuk." },
  { "en": "Apa Itu 'Heatsink Fan'?", "id": "Kipas Pendingin Logam Sirip Prosesor." },
  { "en": "Apa Itu 'Thermal Throttling'?", "id": "Prosesor Melambat Otomatis Saat Kepanasan." },
  { "en": "Apa Itu 'Overclocking'?", "id": "Memaksa Alat Kerja Melebihi Kecepatan Pabrik." },
  { "en": "Apa Itu 'Blue Screen'?", "id": "Layar Error Windows Akibat Kerusakan Hardware." },
  { "en": "Apa Itu 'Reflow Oven'?", "id": "Oven Pemanas Untuk Solder Komponen SMD." },
  { "en": "Apa Itu 'Solder Paste'?", "id": "Timah Bentuk Krim Untuk Komponen Tempel." },
  { "en": "Apa Itu 'Stencil PCB'?", "id": "Cetakan Plat Logam Untuk Oles Timah." },
  { "en": "Apa Itu 'Pick and Place'?", "id": "Mesin Robot Pasang Komponen Ke PCB." },
  { "en": "Apa Itu 'Conformal Coating'?", "id": "Cat Pelindung PCB Dari Lembab Korosi." },
  { "en": "Apa Itu 'Test Point'?", "id": "Titik Logam Khusus Untuk Probe Tester." },
  { "en": "Apa Itu 'Fiducial Mark'?", "id": "Tanda Titik Acuan Kamera Mesin Robot." },
  { "en": "Apa Itu 'Panelized PCB'?", "id": "Banyak PCB Gabung Satu Papan Besar." },
  { "en": "Apa Itu 'V-Cut'?", "id": "Goresan Alur Untuk Mematahkan PCB Panel." },
  { "en": "Apa Itu 'Mouse Bites'?", "id": "Lubang Kecil Berjejer Untuk Patahkan PCB." },
  { "en": "Apa Itu 'Gerber File'?", "id": "File Gambar Desain Layul PCB Pabrik." },
  { "en": "Apa Itu 'BOM' (Bill of Materials)?", "id": "Daftar Lengkap Belanja Komponen Proyek." },
  { "en": "Apa Itu 'Fourier Series'?", "id": "Urai Gelombang Kompleks Jadi Sinus Sederhana." },
  { "en": "Apa Itu 'Laplace Transform'?", "id": "Alat Matematika Analisis Sistem Kendali Analog." },
  { "en": "Apa Itu 'Z-Transform'?", "id": "Alat Matematika Analisis Sistem Kendali Digital." },
  { "en": "Apa Itu 'Convolution'?", "id": "Operasi Matematika Gabung Dua Sinyal." },
  { "en": "Apa Itu 'Transfer Function'?", "id": "Rumus Hubungan Input Dan Output Sistem." },
  { "en": "Apa Itu 'Impulse Response'?", "id": "Reaksi Sistem Saat Diberi Kejut Sesaat." },
  { "en": "Apa Itu 'Step Response'?", "id": "Reaksi Sistem Saat Input Naik Mendadak." },
  { "en": "Apa Itu 'Frequency Response'?", "id": "Respon Sistem Terhadap Berbagai Frekuensi." },
  { "en": "Apa Itu 'Feedback Loop'?", "id": "Sinyal Keluar Masuk Kembali Untuk Koreksi." },
  { "en": "Apa Itu 'Open Loop System'?", "id": "Sistem Kendali Tanpa Umpan Balik Koreksi." },
  { "en": "Apa Itu 'Closed Loop System'?", "id": "Sistem Kendali Dengan Umpan Balik Otomatis." },
  { "en": "Apa Itu 'Sensor Calibration'?", "id": "Menyetel Sensor Agar Sesuai Standar." },
  { "en": "Apa Itu 'Measurement Error'?", "id": "Selisih Nilai Ukur Dengan Nilai Asli." },
  { "en": "Apa Itu 'Precision'?", "id": "Nilai Ukur Selalu Sama Berulang Ulang." },
  { "en": "Apa Itu 'Accuracy'?", "id": "Nilai Ukur Dekat Dengan Nilai Sebenarnya." },
  { "en": "Apa Itu 'Resolution'?", "id": "Perubahan Terkecil Yang Bisa Dibaca Alat." },
  { "en": "Apa Itu 'Sensitivity'?", "id": "Respon Alat Terhadap Perubahan Kecil Input." },
  { "en": "Apa Itu 'Hysteresis'?", "id": "Perbedaan Respon Saat Naik Dan Turun." },
  { "en": "Apa Itu 'Drift'?", "id": "Perubahan Nilai Ukur Perlahan Seiring Waktu." },
  { "en": "Apa Itu 'Zero Error'?", "id": "Jarum Tidak Pas Nol Saat Mati." },
  { "en": "Apa Itu 'Parallax Error'?", "id": "Salah Baca Jarum Karena Sudut Pandang." },
  { "en": "Apa Itu 'Grounding Mat'?", "id": "Karpet Karet Anti Listrik Statis." },
  { "en": "Apa Itu 'Wrist Strap'?", "id": "Gelang Kabel Ground Tangan Teknisi." },
  { "en": "Apa Itu 'Safety Gloves'?", "id": "Sarung Tangan Karet Tahan Listrik." },
  { "en": "Apa Itu 'Safety Shoes'?", "id": "Sepatu Karet Isolator Listrik Tebal." },
  { "en": "Apa Itu 'Test Pen'?", "id": "Obeng Cek Kabel Ada Setrum Tidak." },
  { "en": "Apa Itu 'Phase Detector'?", "id": "Alat Cek Urutan Kabel Tiga Fasa." },
  { "en": "Apa Itu 'Cable Tracer'?", "id": "Alat Cari Kabel Putus Dalam Tembok." },
  { "en": "Apa Itu 'Laser Distance Meter'?", "id": "Meteran Digital Ukur Jarak Pakai Laser." },
  { "en": "Apa Itu 'Thermal Camera'?", "id": "Kamera Melihat Panas Kabel Korslet." },
  { "en": "Apa Itu 'Power Quality Analyzer'?", "id": "Alat Cek Kesehatan Listrik Secara Lengkap." },
  { "en": "Apa Itu 'Oscilloscope Probe 10x'?", "id": "Probe Peredam Tegangan Sepuluh Kali Lipat." },
  { "en": "Apa Itu 'Differential Probe'?", "id": "Probe Ukur Beda Tegangan Dua Titik." },
  { "en": "Apa Itu 'Current Clamp'?", "id": "Capit Ukur Arus Tanpa Putus Kabel." },
  { "en": "Apa Itu 'Banana Plug Stackable'?", "id": "Colokan Kabel Bisa Tumpuk Cabang." },
  { "en": "Apa Itu 'Breadboard Power Module'?", "id": "Adaptor Khusus Tancap Papan Roti." },
  { "en": "Apa Itu 'Logic Level Converter'?", "id": "Penyesuai Tegangan Sinyal 5V Ke 3V." },
  { "en": "Apa Itu 'Buck-Boost Converter'?", "id": "Modul Stabilkan Tegangan Naik Turun." },
  { "en": "Apa Itu 'USB Isolator'?", "id": "Pelindung Port USB Laptop Dari Korslet." },
  { "en": "Apa Itu 'Dummy Load'?", "id": "Beban Buatan Tes Kekuatan Power Supply." },
  { "en": "Apa Itu 'Solar Irradiance Meter'?", "id": "Alat Ukur Kekuatan Cahaya Matahari." },
  { "en": "Apa Itu 'Wind Turbine'?", "id": "Kincir Angin Pembangkit Listrik." },
  { "en": "Apa Itu 'Hydro Power'?", "id": "Pembangkit Listrik Tenaga Air Terjun." },
  { "en": "Apa Itu 'Geothermal Energy'?", "id": "Energi Listrik Dari Panas Bumi." },
  { "en": "Apa Itu 'Biomass Energy'?", "id": "Energi Listrik Dari Sisa Bahan Organik." },
  { "en": "Apa Itu 'Fuel Cell'?", "id": "Penghasil Listrik Dari Reaksi Kimia Hidrogen." }



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
