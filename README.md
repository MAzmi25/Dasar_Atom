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


  { "en": "Apa Itu Atom?", "id": "Partikel Pembangun Dasar Seluruh Materi." },
  { "en": "Dari Bahasa Apa Kata Atom Berasal?", "id": "Berasal Dari Bahasa Yunani 'Atomos'." },
  { "en": "Apa Arti Kata 'Atomos'?", "id": "Tidak Dapat Dibagi Lagi." },
  { "en": "Sebutkan Tiga Partikel Subatomik Utama?", "id": "Proton Neutron Dan Elektron." },
  { "en": "Di Mana Pusat Atom Berada?", "id": "Di Dalam Inti Atom Atau Nukleus." },
  { "en": "Partikel Apa Yang Ada Di Inti Atom?", "id": "Proton Dan Neutron." },
  { "en": "Apa Nama Partikel Di Dalam Inti Atom?", "id": "Nukleon." },
  { "en": "Partikel Apa Yang Mengorbit Inti Atom?", "id": "Elektron." },
  { "en": "Apa Muatan Listrik Proton?", "id": "Muatan Positif (+1)." },
  { "en": "Apa Muatan Listrik Elektron?", "id": "Muatan Negatif (-1)." },
  { "en": "Apa Muatan Listrik Neutron?", "id": "Tidak Bermuatan Atau Netral." },
  { "en": "Siapa Yang Menemukan Elektron?", "id": "J.J. Thomson Pada Tahun 1897." },
  { "en": "Siapa Yang Menemukan Proton?", "id": "Ernest Rutherford Pada Tahun 1919." },
  { "en": "Siapa Yang Menemukan Neutron?", "id": "James Chadwick Pada Tahun 1932." },
  { "en": "Partikel Mana Yang Paling Ringan?", "id": "Elektron Adalah Yang Paling Ringan." },
  { "en": "Partikel Mana Yang Paling Berat?", "id": "Neutron Sedikit Lebih Berat Dari Proton." },
  { "en": "Apa Itu Nomor Atom (Z)?", "id": "Jumlah Proton Di Dalam Inti Atom." },
  { "en": "Apa Fungsi Nomor Atom?", "id": "Menentukan Identitas Suatu Unsur Kimia." },
  { "en": "Unsur Apa Yang Memiliki Nomor Atom 1?", "id": "Hidrogen." },
  { "en": "Unsur Apa Yang Memiliki Nomor Atom 6?", "id": "Karbon." },
  { "en": "Unsur Apa Yang Memiliki Nomor Atom 8?", "id": "Oksigen." },
  { "en": "Apa Itu Nomor Massa (A)?", "id": "Jumlah Total Proton Dan Neutron." },
  { "en": "Bagaimana Cara Menghitung Jumlah Neutron?", "id": "Nomor Massa Dikurangi Nomor Atom." },
  { "en": "Apa Itu Atom Netral?", "id": "Atom Dengan Jumlah Proton Dan Elektron Sama." },
  { "en": "Apa Muatan Keseluruhan Atom Netral?", "id": "Nol Atau Tidak Bermuatan." },
  { "en": "Mengapa Atom Secara Keseluruhan Netral?", "id": "Karena Muatan Positif Dan Negatif Seimbang." },
  { "en": "Apa Itu Ion?", "id": "Atom Yang Kehilangan Atau Mendapat Elektron." },
  { "en": "Apa Itu Kation?", "id": "Ion Bermuatan Positif." },
  { "en": "Bagaimana Kation Terbentuk?", "id": "Saat Atom Kehilangan Satu Atau Lebih Elektron." },
  { "en": "Apa Itu Anion?", "id": "Ion Bermuatan Negatif." },
  { "en": "Bagaimana Anion Terbentuk?", "id": "Saat Atom Menerima Satu Atau Lebih Elektron." },
  { "en": "Apa Itu Isotop?", "id": "Atom Unsur Sama Beda Jumlah Neutron." },
  { "en": "Apa Persamaan Antar Isotop?", "id": "Memiliki Jumlah Proton Yang Sama." },
  { "en": "Apa Perbedaan Antar Isotop?", "id": "Memiliki Nomor Massa Yang Berbeda." },
  { "en": "Sebutkan Isotop Hidrogen?", "id": "Protium Deuterium Tritium." },
  { "en": "Isotop Hidrogen Mana Yang Paling Umum?", "id": "Protium (Tanpa Neutron)." },
  { "en": "Apa Itu Model Atom Dalton?", "id": "Atom Adalah Bola Padat Tak Terbagi." },
  { "en": "Apa Itu Model Atom Thomson?", "id": "Model Roti Kismis." },
  { "en": "Apa Analogi Model Roti Kismis?", "id": "Elektron Tersebar Di Dalam Muatan Positif." },
  { "en": "Apa Itu Model Atom Rutherford?", "id": "Model Tata Surya Atau Model Nuklir." },
  { "en": "Apa Kesimpulan Utama Model Rutherford?", "id": "Atom Memiliki Inti Padat Bermuatan Positif." },
  { "en": "Apa Itu Model Atom Bohr?", "id": "Elektron Mengorbit Inti Pada Lintasan Tertentu." },
  { "en": "Apa Nama Lintasan Elektron Menurut Bohr?", "id": "Kulit Atom Atau Tingkat Energi." },
  { "en": "Apa Itu Model Atom Mekanika Kuantum?", "id": "Model Atom Modern Saat Ini." },
  { "en": "Bagaimana Model Kuantum Menggambarkan Elektron?", "id": "Sebagai Awan Peluang Keberadaan Elektron." },
  { "en": "Apa Nama Wilayah Peluang Elektron?", "id": "Orbital Atom." },
  { "en": "Gaya Apa Yang Menahan Proton Di Inti?", "id": "Gaya Nuklir Kuat." },
  { "en": "Gaya Apa Yang Menahan Elektron Dekat Inti?", "id": "Gaya Elektromagnetik Atau Gaya Tarik Listrik." },
  { "en": "Mengapa Proton Di Inti Tidak Saling Mendorong?", "id": "Gaya Nuklir Kuat Mengalahkan Tolakan Listrik." },
  { "en": "Apa Itu Unsur Kimia?", "id": "Zat Murni Terdiri Dari Atom Sejenis." },
  { "en": "Bagaimana Unsur Kimia Ditentukan?", "id": "Oleh Jumlah Proton Atau Nomor Atomnya." },
  { "en": "Apa Itu Tabel Periodik?", "id": "Tabel Yang Mengatur Semua Unsur Kimia." },
  { "en": "Bagaimana Unsur Diatur Dalam Tabel Periodik?", "id": "Berdasarkan Kenaikan Nomor Atom." },
  { "en": "Apa Itu Molekul?", "id": "Dua Atau Lebih Atom Terikat Bersama." },
  { "en": "Contoh Molekul Sederhana?", "id": "Molekul Air (H₂O)." },
  { "en": "Apa Itu Senyawa?", "id": "Zat Terbentuk Dari Dua Unsur Berbeda." },
  { "en": "Apakah Sebagian Besar Atom Itu Ruang Hampa?", "id": "Ya Sebagian Besar Volumenya Ruang Kosong." },
  { "en": "Di Mana Sebagian Besar Massa Atom Terkonsentrasi?", "id": "Di Dalam Inti Atom." },
  { "en": "Apa Itu Kulit Valensi?", "id": "Kulit Atom Terluar." },
  { "en": "Apa Itu Elektron Valensi?", "id": "Elektron Di Kulit Atom Terluar." },
  { "en": "Apa Peran Elektron Valensi?", "id": "Berperan Dalam Pembentukan Ikatan Kimia." },
  { "en": "Apa Itu Aturan Oktet?", "id": "Kecenderungan Atom Mencapai Delapan Elektron Valensi." },
  { "en": "Mengapa Atom Membentuk Ikatan Kimia?", "id": "Untuk Mencapai Konfigurasi Elektron Yang Stabil." },
  { "en": "Apa Itu Ikatan Kovalen?", "id": "Ikatan Berbagi Pasangan Elektron." },
  { "en": "Apa Itu Ikatan Ionik?", "id": "Ikatan Akibat Tarik Menarik Antar Ion." },
  { "en": "Satuan Massa Apa Yang Digunakan Untuk Atom?", "id": "Satuan Massa Atom (Sma Atau Dalton)." },
  { "en": "Apa Definisi Satu Sma?", "id": "Seperdua Belas Massa Atom Karbon-12." },
  { "en": "Apa Itu Massa Atom Relatif?", "id": "Massa Rata-Rata Isotop Suatu Unsur." },
  { "en": "Apa Yang Membuat Atom Stabil?", "id": "Rasio Proton Dan Neutron Yang Seimbang." },
  { "en": "Apa Yang Terjadi Jika Inti Tidak Stabil?", "id": "Akan Mengalami Peluruhan Radioaktif." },
  { "en": "Apa Itu Radioaktivitas?", "id": "Pemancaran Energi Dari Inti Tidak Stabil." },
  { "en": "Apa Itu Partikel Alfa?", "id": "Inti Helium (Dua Proton Dua Neutron)." },
  { "en": "Apa Itu Partikel Beta?", "id": "Elektron Berkecepatan Tinggi Dari Inti." },
  { "en": "Apa Itu Sinar Gamma?", "id": "Radiasi Elektromagnetik Berenergi Sangat Tinggi." },
  { "en": "Apa Itu Tingkat Energi?", "id": "Jumlah Energi Tertentu Yang Dimiliki Elektron." },
  { "en": "Bisakah Elektron Berada Di Antara Tingkat Energi?", "id": "Tidak Hanya Boleh Di Tingkat Energi Tertentu." },
  { "en": "Apa Itu Eksitasi Elektron?", "id": "Elektron Pindah Ke Tingkat Energi Lebih Tinggi." },
  { "en": "Bagaimana Elektron Bisa Tereksitasi?", "id": "Dengan Menyerap Energi (Panas Cahaya Listrik)." },
  { "en": "Apa Yang Terjadi Saat Elektron Kembali Turun?", "id": "Memancarkan Energi Dalam Bentuk Cahaya." },
  { "en": "Apa Itu Spektrum Emisi?", "id": "Pola Cahaya Unik Yang Dipancarkan Unsur." },
  { "en": "Apakah Setiap Unsur Punya Spektrum Emisi Unik?", "id": "Ya Seperti Sidik Jari Untuk Unsur." },
  { "en": "Apa Itu Konfigurasi Elektron?", "id": "Susunan Elektron Dalam Kulit Dan Orbital." },
  { "en": "Apa Prinsip Aufbau?", "id": "Elektron Mengisi Orbital Energi Terendah Dahulu." },
  { "en": "Apa Larangan Pauli?", "id": "Satu Orbital Maksimal Diisi Dua Elektron." },
  { "en": "Apa Aturan Hund?", "id": "Elektron Mengisi Orbital Satu Persatu Dahulu." },
  { "en": "Berapa Jumlah Maksimal Elektron Di Kulit Pertama?", "id": "Dua Elektron." },
  { "en": "Berapa Jumlah Maksimal Elektron Di Kulit Kedua?", "id": "Delapan Elektron." },
  { "en": "Berapa Jumlah Maksimal Elektron Di Kulit Ketiga?", "id": "Delapan Belas Elektron." },
  { "en": "Apa Itu Subkulit S P D F?", "id": "Bentuk Orbital Di Dalam Kulit Atom." },
  { "en": "Berapa Orbital Yang Ada Di Subkulit S?", "id": "Satu Orbital." },
  { "en": "Berapa Orbital Yang Ada Di Subkulit P?", "id": "Tiga Orbital." },
  { "en": "Berapa Orbital Yang Ada Di Subkulit D?", "id": "Lima Orbital." },
  { "en": "Berapa Orbital Yang Ada Di Subkulit F?", "id": "Tujuh Orbital." },
  { "en": "Apa Itu Bilangan Kuantum?", "id": "Angka Yang Menjelaskan Sifat Elektron." },
  { "en": "Apa Itu Bilangan Kuantum Utama (n)?", "id": "Menentukan Tingkat Energi Utama Atau Kulit." },
  { "en": "Apa Nilai Yang Mungkin Untuk Bilangan Kuantum Utama?", "id": "Bilangan Bulat Positif (1 2 3...)." },
  { "en": "Apa Itu Bilangan Kuantum Azimut (l)?", "id": "Menentukan Bentuk Orbital Atau Subkulit." },
  { "en": "Apa Nama Lain Bilangan Kuantum Azimut?", "id": "Bilangan Kuantum Momentum Sudut." },
  { "en": "Berapa Nilai l Untuk Subkulit S?", "id": "Nilai l Adalah Nol." },
  { "en": "Berapa Nilai l Untuk Subkulit P?", "id": "Nilai l Adalah Satu." },
  { "en": "Berapa Nilai l Untuk Subkulit D?", "id": "Nilai l Adalah Dua." },
  { "en": "Apa Itu Bilangan Kuantum Magnetik (ml)?", "id": "Menentukan Orientasi Orbital Dalam Ruang." },
  { "en": "Berapa Nilai ml Untuk Subkulit S?", "id": "Hanya Nol." },
  { "en": "Berapa Nilai ml Untuk Subkulit P?", "id": "Minus Satu Nol Dan Positif Satu." },
  { "en": "Apa Itu Bilangan Kuantum Spin (ms)?", "id": "Menentukan Arah Putaran Intrinsik Elektron." },
  { "en": "Apa Nilai Yang Mungkin Untuk Bilangan Kuantum Spin?", "id": "Positif Setengah Atau Negatif Setengah." },
  { "en": "Bagaimana Bentuk Dari Orbital S?", "id": "Berbentuk Bola Simetris." },
  { "en": "Bagaimana Bentuk Dari Orbital P?", "id": "Seperti Balon Terpilin Atau Angka Delapan." },
  { "en": "Ada Berapa Orientasi Orbital P?", "id": "Tiga Orientasi (Px Py Pz)." },
  { "en": "Bagaimana Bentuk Sebagian Besar Orbital D?", "id": "Seperti Daun Semanggi Empat." },
  { "en": "Berapa Maksimal Elektron Dalam Satu Orbital?", "id": "Maksimal Dua Elektron Dengan Spin Berlawanan." },
  { "en": "Prinsip Apa Yang Mendasari Aturan Ini?", "id": "Prinsip Pengecualian Pauli." },
  { "en": "Apa Itu Jari-Jari Atom?", "id": "Jarak Dari Inti Ke Kulit Terluar." },
  { "en": "Bagaimana Tren Jari-Jari Atom Dalam Satu Golongan?", "id": "Meningkat Dari Atas Ke Bawah." },
  { "en": "Mengapa Jari-Jari Atom Meningkat Ke Bawah?", "id": "Karena Jumlah Kulit Atom Bertambah." },
  { "en": "Bagaimana Tren Jari-Jari Atom Dalam Satu Periode?", "id": "Menurun Dari Kiri Ke Kanan." },
  { "en": "Mengapa Jari-Jari Atom Menurun Ke Kanan?", "id": "Karena Tarikan Inti Semakin Kuat." },
  { "en": "Apa Itu Energi Ionisasi?", "id": "Energi Untuk Melepaskan Satu Elektron." },
  { "en": "Bagaimana Tren Energi Ionisasi Dalam Satu Golongan?", "id": "Menurun Dari Atas Ke Bawah." },
  { "en": "Bagaimana Tren Energi Ionisasi Dalam Satu Periode?", "id": "Meningkat Dari Kiri Ke Kanan." },
  { "en": "Unsur Mana Yang Punya Energi Ionisasi Tertinggi?", "id": "Helium." },
  { "en": "Apa Itu Afinitas Elektron?", "id": "Energi Yang Dilepas Saat Menerima Elektron." },
  { "en": "Bagaimana Tren Afinitas Elektron Secara Umum?", "id": "Meningkat Dari Kiri Bawah Ke Kanan Atas." },
  { "en": "Apa Itu Keelektronegatifan?", "id": "Kemampuan Atom Menarik Elektron Dalam Ikatan." },
  { "en": "Bagaimana Tren Keelektronegatifan?", "id": "Meningkat Dari Kiri Bawah Ke Kanan Atas." },
  { "en": "Unsur Mana Yang Paling Elektronegatif?", "id": "Fluorin." },
  { "en": "Unsur Mana Yang Paling Elektropositif?", "id": "Fransium." },
  { "en": "Apa Itu Sifat Logam?", "id": "Kecenderungan Atom Melepaskan Elektron." },
  { "en": "Bagaimana Tren Sifat Logam Dalam Tabel Periodik?", "id": "Meningkat Dari Kanan Atas Ke Kiri Bawah." },
  { "en": "Apa Itu Fisi Nuklir?", "id": "Pembelahan Inti Atom Berat Menjadi Lebih Ringan." },
  { "en": "Apa Yang Dihasilkan Dari Fisi Nuklir?", "id": "Energi Besar Dan Neutron Tambahan." },
  { "en": "Apa Itu Reaksi Berantai?", "id": "Neutron Hasil Fisi Menyebabkan Fisi Berikutnya." },
  { "en": "Di Mana Fisi Nuklir Digunakan?", "id": "Pembangkit Listrik Tenaga Nuklir Dan Bom Atom." },
  { "en": "Unsur Apa Yang Umum Digunakan Untuk Fisi?", "id": "Uranium-235 Dan Plutonium-239." },
  { "en": "Apa Itu Fusi Nuklir?", "id": "Penggabungan Inti Atom Ringan Menjadi Lebih Berat." },
  { "en": "Apa Yang Dibutuhkan Untuk Terjadinya Fusi Nuklir?", "id": "Suhu Dan Tekanan Yang Sangat Tinggi." },
  { "en": "Di Mana Fusi Nuklir Terjadi Secara Alami?", "id": "Di Inti Matahari Dan Bintang-Bintang." },
  { "en": "Apa Hasil Dari Fusi Hidrogen?", "id": "Menghasilkan Inti Helium Dan Energi Sangat Besar." },
  { "en": "Manakah Yang Menghasilkan Energi Lebih Besar Fisi Atau Fusi?", "id": "Fusi Nuklir." },
  { "en": "Apa Itu Waktu Paruh?", "id": "Waktu Separuh Zat Radioaktif Untuk Meluruh." },
  { "en": "Untuk Apa Waktu Paruh Digunakan?", "id": "Menentukan Umur Fosil Atau Batuan." },
  { "en": "Apa Nama Proses Penentuan Umur Fosil?", "id": "Penanggalan Karbon Atau Carbon Dating." },
  { "en": "Isotop Apa Yang Digunakan Dalam Penanggalan Karbon?", "id": "Karbon-14." },
  { "en": "Apa Itu Antimateri?", "id": "Materi Dengan Muatan Partikel Yang Berlawanan." },
  { "en": "Apa Anti-Partikel Dari Elektron?", "id": "Positron." },
  { "en": "Apa Muatan Positron?", "id": "Muatan Positif (+1)." },
  { "en": "Apa Anti-Partikel Dari Proton?", "id": "Antiproton." },
  { "en": "Apa Muatan Antiproton?", "id": "Muatan Negatif (-1)." },
  { "en": "Apa Yang Terjadi Jika Materi Dan Antimateri Bertemu?", "id": "Saling Memusnahkan Dan Menghasilkan Energi." },
  { "en": "Apa Nama Proses Pemusnahan Ini?", "id": "Anihilasi." },
  { "en": "Bagaimana Susunan Atom Dalam Zat Padat?", "id": "Tersusun Rapat Dan Bergetar Di Tempat." },
  { "en": "Bagaimana Susunan Atom Dalam Zat Cair?", "id": "Berdekatan Tapi Dapat Bergerak Bebas." },
  { "en": "Bagaimana Susunan Atom Dalam Zat Gas?", "id": "Saling Berjauhan Dan Bergerak Sangat Cepat." },
  { "en": "Apa Itu Foton?", "id": "Partikel Kuantum Cahaya Atau Energi." },
  { "en": "Apa Itu Spektrum Absorpsi?", "id": "Pola Garis Gelap Saat Cahaya Melewati Unsur." },
  { "en": "Bagaimana Spektrum Absorpsi Terbentuk?", "id": "Elektron Menyerap Foton Dengan Energi Tertentu." },
  { "en": "Apa Beda Spektrum Emisi Dan Absorpsi?", "id": "Emisi Garis Terang Absorpsi Garis Gelap." },
  { "en": "Apakah Posisi Garisnya Sama?", "id": "Ya Posisi Garisnya Tepat Sama." },
  { "en": "Apa Itu Efek Fotolistrik?", "id": "Elektron Terlepas Dari Logam Saat Dikenai Cahaya." },
  { "en": "Siapa Yang Menjelaskan Efek Fotolistrik?", "id": "Albert Einstein." },
  { "en": "Apa Itu Kuantisasi Energi?", "id": "Energi Hanya Ada Dalam Paket Diskrit." },
  { "en": "Apa Nama Paket Energi Diskrit Itu?", "id": "Kuanta (Bentuk Jamak Dari Kuantum)." },
  { "en": "Apa Itu Orbital Hibrida?", "id": "Penggabungan Beberapa Orbital Atom." },
  { "en": "Contoh Hibridisasi?", "id": "Hibridisasi Sp³ Pada Atom Karbon." },
  { "en": "Apa Bentuk Molekul Akibat Hibridisasi Sp³?", "id": "Bentuk Tetrahedral." },
  { "en": "Apa Itu Gaya Nuklir Lemah?", "id": "Gaya Fundamental Yang Menyebabkan Peluruhan Beta." },
  { "en": "Manakah Gaya Yang Paling Lemah Di Alam Semesta?", "id": "Gaya Gravitasi." },
  { "en": "Apakah Gravitasi Berperan Penting Dalam Atom?", "id": "Tidak Perannya Dapat Diabaikan." },
  { "en": "Apa Itu Plasma?", "id": "Wujud Zat Keempat." },
  { "en": "Seperti Apa Wujud Plasma Itu?", "id": "Gas Terionisasi Yang Terdiri Dari Ion." },
  { "en": "Di Mana Plasma Ditemukan?", "id": "Bintang Petir Dan Lampu Neon." },
  { "en": "Apa Itu Alotrop?", "id": "Bentuk Berbeda Dari Unsur Yang Sama." },
  { "en": "Sebutkan Alotrop Dari Karbon?", "id": "Intan Grafit Dan Grafena." },
  { "en": "Apa Yang Menyebabkan Adanya Alotrop?", "id": "Perbedaan Susunan Atau Ikatan Atom." },
  { "en": "Apa Itu Bilangan Avogadro?", "id": "Jumlah Partikel Dalam Satu Mol Zat." },
  { "en": "Berapa Nilai Bilangan Avogadro?", "id": "Sekitar 6.022 Kali 10 Pangkat 23." },
  { "en": "Apa Itu Mol?", "id": "Satuan Jumlah Zat Dalam Kimia." },
  { "en": "Apa Itu Massa Molar?", "id": "Massa Satu Mol Suatu Zat." },
  { "en": "Apa Itu Gas Mulia?", "id": "Unsur Di Golongan 18 Tabel Periodik." },
  { "en": "Mengapa Gas Mulia Sangat Stabil?", "id": "Karena Memiliki Kulit Valensi Penuh." },
  { "en": "Contoh Gas Mulia?", "id": "Helium Neon Argon." },
  { "en": "Apa Itu Halogen?", "id": "Unsur Di Golongan 17 Tabel Periodik." },
  { "en": "Bagaimana Sifat Halogen?", "id": "Sangat Reaktif Dan Cenderung Menerima Elektron." },
  { "en": "Contoh Halogen?", "id": "Fluorin Klorin Bromin." },
  { "en": "Apa Itu Logam Alkali?", "id": "Unsur Di Golongan 1 Tabel Periodik." },
  { "en": "Bagaimana Sifat Logam Alkali?", "id": "Sangat Reaktif Dan Cenderung Melepas Elektron." },
  { "en": "Contoh Logam Alkali?", "id": "Litium Natrium Kalium." },
  { "en": "Apa Itu Ikatan Logam?", "id": "Ikatan Antar Atom Logam." },
  { "en": "Bagaimana Ikatan Logam Terbentuk?", "id": "Lautan Elektron Mengikat Kation Logam." },
  { "en": "Apa Itu Dualitas Gelombang-Partikel?", "id": "Partikel Dapat Menunjukkan Sifat Gelombang." },
  { "en": "Siapa Yang Mengusulkan Dualitas Gelombang-Partikel?", "id": "Louis De Broglie." },
  { "en": "Apa Nama Eksperimen Penemu Inti Atom?", "id": "Eksperimen Lempeng Emas Rutherford." },
  { "en": "Partikel Apa Yang Ditembakkan Dalam Eksperimen Rutherford?", "id": "Partikel Alfa Bermuatan Positif." },
  { "en": "Apa Hasil Yang Mengejutkan Dari Eksperimen Rutherford?", "id": "Sebagian Kecil Partikel Alfa Memantul Kembali." },
  { "en": "Apa Kesimpulan Dari Pantulan Partikel Alfa?", "id": "Atom Punya Inti Kecil Padat Positif." },
  { "en": "Alat Apa Yang Digunakan Thomson Untuk Menemukan Elektron?", "id": "Tabung Sinar Katoda." },
  { "en": "Apa Itu Sinar Katoda?", "id": "Aliran Elektron Yang Bergerak Cepat." },
  { "en": "Bagaimana Perilaku Sinar Katoda Dalam Medan Listrik?", "id": "Dibelokkan Menuju Lempeng Positif." },
  { "en": "Eksperimen Apa Yang Mengukur Muatan Elektron?", "id": "Eksperimen Tetes Minyak Milikan." },
  { "en": "Apa Itu Partikel Elementer?", "id": "Partikel Yang Tidak Tersusun Dari Partikel Lain." },
  { "en": "Apakah Elektron Termasuk Partikel Elementer?", "id": "Ya Elektron Adalah Partikel Elementer." },
  { "en": "Apakah Proton Dan Neutron Partikel Elementer?", "id": "Tidak Keduanya Tersusun Dari Kuark." },
  { "en": "Apa Itu Kuark (Quark)?", "id": "Partikel Elementer Pembangun Proton Dan Neutron." },
  { "en": "Sebutkan Dua Jenis Kuark Paling Umum?", "id": "Kuark Atas (Up) Dan Kuark Bawah (Down)." },
  { "en": "Apa Muatan Listrik Kuark Atas?", "id": "Positif Dua Pertiga (+2/3)." },
  { "en": "Apa Muatan Listrik Kuark Bawah?", "id": "Negatif Sepertiga (-1/3)." },
  { "en": "Apa Komposisi Kuark Sebuah Proton?", "id": "Dua Kuark Atas Satu Kuark Bawah." },
  { "en": "Apa Komposisi Kuark Sebuah Neutron?", "id": "Satu Kuark Atas Dua Kuark Bawah." },
  { "en": "Partikel Apa Yang Membawa Gaya Nuklir Kuat?", "id": "Gluon." },
  { "en": "Apa Fungsi Gluon?", "id": "Mengikat Kuark Bersama Di Dalam Nukleon." },
  { "en": "Apa Itu Kelompok Partikel Lepton?", "id": "Partikel Elementer Yang Tidak Merasakan Gaya Kuat." },
  { "en": "Partikel Apa Yang Termasuk Dalam Lepton?", "id": "Elektron Muon Tau Dan Neutrino." },
  { "en": "Bagaimana Ukuran Kation Dibandingkan Atom Netralnya?", "id": "Kation Selalu Lebih Kecil." },
  { "en": "Mengapa Kation Lebih Kecil?", "id": "Karena Kehilangan Kulit Atau Tarikan Inti Kuat." },
  { "en": "Bagaimana Ukuran Anion Dibandingkan Atom Netralnya?", "id": "Anion Selalu Lebih Besar." },
  { "en": "Mengapa Anion Lebih Besar?", "id": "Karena Tolakan Antar Elektron Meningkat." },
  { "en": "Apa Itu Deret Isoelektronik?", "id": "Atom Atau Ion Dengan Jumlah Elektron Sama." },
  { "en": "Contoh Deret Isoelektronik?", "id": "Ion Natrium Neon Dan Ion Fluorida." },
  { "en": "Apa Itu Ikatan Kovalen Tunggal?", "id": "Berbagi Satu Pasang Elektron." },
  { "en": "Apa Itu Ikatan Kovalen Rangkap Dua?", "id": "Berbagi Dua Pasang Elektron." },
  { "en": "Apa Itu Ikatan Kovalen Rangkap Tiga?", "id": "Berbagi Tiga Pasang Elektron." },
  { "en": "Manakah Ikatan Yang Paling Kuat?", "id": "Ikatan Rangkap Tiga Adalah Yang Terkuat." },
  { "en": "Manakah Ikatan Yang Paling Pendek?", "id": "Ikatan Rangkap Tiga Adalah Yang Terpendek." },
  { "en": "Apa Itu Panjang Ikatan?", "id": "Jarak Rata-Rata Antara Dua Inti Terikat." },
  { "en": "Apa Itu Energi Ikatan?", "id": "Energi Untuk Memutuskan Suatu Ikatan Kimia." },
  { "en": "Apa Itu Ikatan Kovalen Polar?", "id": "Berbagi Elektron Yang Tidak Merata." },
  { "en": "Apa Yang Menyebabkan Ikatan Polar?", "id": "Perbedaan Keelektronegatifan Antar Atom." },
  { "en": "Apa Itu Momen Dipol?", "id": "Ukuran Kepolaran Suatu Ikatan Kimia." },
  { "en": "Apa Itu Ikatan Kovalen Nonpolar?", "id": "Berbagi Elektron Yang Merata Sempurna." },
  { "en": "Kapan Ikatan Nonpolar Terjadi?", "id": "Antara Atom Yang Sama (Contoh: H-H)." },
  { "en": "Apa Itu Teori VSEPR?", "id": "Teori Untuk Memprediksi Bentuk Molekul." },
  { "en": "Apa Prinsip Dasar Teori VSEPR?", "id": "Pasangan Elektron Saling Menolak Sejauh Mungkin." },
  { "en": "Bentuk Molekul Apa Dengan Dua Pasangan Elektron?", "id": "Bentuk Linear." },
  { "en": "Berapa Sudut Ikatan Pada Bentuk Linear?", "id": "Seratus Delapan Puluh Derajat." },
  { "en": "Bentuk Molekul Apa Dengan Tiga Pasangan Elektron?", "id": "Segitiga Planar." },
  { "en": "Bentuk Molekul Apa Dengan Empat Pasangan Elektron?", "id": "Bentuk Tetrahedral." },
  { "en": "Berapa Sudut Ikatan Pada Bentuk Tetrahedral?", "id": "Sekitar 109.5 Derajat." },
  { "en": "Contoh Molekul Berbentuk Tetrahedral?", "id": "Metana (CH₄)." },
  { "en": "Apa Itu Pita Kestabilan?", "id": "Grafik Plot Neutron Versus Proton." },
  { "en": "Apa Arti Inti Di Dalam Pita Kestabilan?", "id": "Inti Tersebut Cenderung Stabil." },
  { "en": "Bagaimana Inti Di Atas Pita Kestabilan Meluruh?", "id": "Melalui Peluruhan Beta." },
  { "en": "Apa Yang Terjadi Selama Peluruhan Beta?", "id": "Neutron Berubah Menjadi Proton Dan Elektron." },
  { "en": "Bagaimana Inti Di Bawah Pita Kestabilan Meluruh?", "id": "Melalui Emisi Positron Atau Penangkapan Elektron." },
  { "en": "Apa Itu Emisi Positron?", "id": "Proton Berubah Menjadi Neutron Dan Positron." },
  { "en": "Apa Itu Penangkapan Elektron?", "id": "Inti Menangkap Elektron Dari Kulit Dalam." },
  { "en": "Apa Itu Deret Peluruhan?", "id": "Rangkaian Peluruhan Radioaktif Hingga Stabil." },
  { "en": "Unsur Berat Seperti Uranium Meluruh Menjadi Apa?", "id": "Akhirnya Menjadi Isotop Timbal Yang Stabil." },
  { "en": "Apa Itu Bilangan Ajaib (Magic Numbers)?", "id": "Jumlah Nukleon Yang Sangat Stabil." },
  { "en": "Sebutkan Beberapa Bilangan Ajaib?", "id": "Dua Delapan Dua Puluh Dua Delapan." },
  { "en": "Apa Itu Inti Genap-Genap?", "id": "Inti Dengan Jumlah Proton Dan Neutron Genap." },
  { "en": "Manakah Jenis Inti Yang Paling Stabil?", "id": "Inti Dengan Nukleon Genap-Genap." },
  { "en": "Apa Itu Transmutasi?", "id": "Perubahan Satu Unsur Menjadi Unsur Lain." },
  { "en": "Bagaimana Transmutasi Dapat Terjadi?", "id": "Melalui Reaksi Nuklir Atau Peluruhan Radioaktif." },
  { "en": "Apa Itu Reaktor Nuklir?", "id": "Tempat Reaksi Fisi Berantai Terkendali." },
  { "en": "Apa Fungsi Batang Kendali (Control Rods)?", "id": "Menyerap Neutron Untuk Mengontrol Laju Reaksi." },
  { "en": "Bahan Apa Yang Digunakan Untuk Batang Kendali?", "id": "Boron Atau Kadmium." },
  { "en": "Apa Fungsi Moderator Dalam Reaktor?", "id": "Memperlambat Neutron Agar Efisien." },
  { "en": "Bahan Apa Yang Digunakan Sebagai Moderator?", "id": "Air Atau Grafit." },
  { "en": "Apa Itu Spektroskopi?", "id": "Studi Interaksi Antara Materi Dan Radiasi." },
  { "en": "Apa Kegunaan Spektroskopi Atom?", "id": "Mengidentifikasi Komposisi Unsur Suatu Sampel." },
  { "en": "Bagaimana Cara Kerja Spektroskopi Absorpsi Atom?", "id": "Mengukur Cahaya Yang Diserap Oleh Atom." },
  { "en": "Aplikasi Spektroskopi Dalam Astronomi?", "id": "Menentukan Komposisi Bintang Dan Galaksi." },
  { "en": "Apa Itu Jam Atom?", "id": "Jam Paling Akurat Berdasarkan Getaran Atom." },
  { "en": "Atom Unsur Apa Yang Digunakan Jam Atom?", "id": "Atom Sesium-133." },
  { "en": "Apa Itu Efek Zeeman?", "id": "Pemisahan Garis Spektrum Dalam Medan Magnet." },
  { "en": "Apa Itu Efek Stark?", "id": "Pemisahan Garis Spektrum Dalam Medan Listrik." },
  { "en": "Apa Itu Keadaan Dasar (Ground State)?", "id": "Tingkat Energi Terendah Suatu Atom." },
  { "en": "Apa Itu Keadaan Tereksitasi (Excited State)?", "id": "Tingkat Energi Yang Lebih Tinggi Dari Dasar." },
  { "en": "Apakah Keadaan Tereksitasi Stabil?", "id": "Tidak Sangat Tidak Stabil." },
  { "en": "Apa Itu Gaya Van Der Waals?", "id": "Gaya Tarik Antarmolekul Yang Lemah." },
  { "en": "Apa Penyebab Gaya Van Der Waals?", "id": "Fluktuasi Sesaat Distribusi Elektron." },
  { "en": "Apa Itu Ikatan Hidrogen?", "id": "Jenis Ikatan Antarmolekul Yang Kuat." },
  { "en": "Antara Atom Apa Ikatan Hidrogen Terjadi?", "id": "Hidrogen Dengan Oksigen Nitrogen Atau Fluorin." },
  { "en": "Mengapa Air Memiliki Sifat Yang Unik?", "id": "Karena Adanya Ikatan Hidrogen Yang Kuat." },
  { "en": "Apa Itu Struktur Lewis?", "id": "Diagram Yang Menunjukkan Elektron Valensi." },
  { "en": "Apa Yang Digambarkan Titik Dalam Struktur Lewis?", "id": "Elektron Valensi." },
  { "en": "Apa Yang Digambarkan Garis Dalam Struktur Lewis?", "id": "Sepasang Elektron Ikatan." },
  { "en": "Apa Itu Pasangan Elektron Bebas?", "id": "Elektron Valensi Yang Tidak Terlibat Ikatan." },
  { "en": "Bagaimana Pasangan Bebas Mempengaruhi Bentuk Molekul?", "id": "Memiliki Gaya Tolak Yang Lebih Kuat." },
  { "en": "Apa Bentuk Molekul Air (H₂O)?", "id": "Bentuk V Atau Tekuk." },
  { "en": "Mengapa Molekul Air Berbentuk Tekuk?", "id": "Karena Dua Pasangan Elektron Bebas." },
  { "en": "Apa Itu Resonansi?", "id": "Penggunaan Beberapa Struktur Lewis Untuk Satu Molekul." },
  { "en": "Kapan Resonansi Dibutuhkan?", "id": "Saat Satu Struktur Lewis Tidak Cukup." },
  { "en": "Contoh Molekul Dengan Resonansi?", "id": "Ozon (O₃) Dan Benzena." },
  { "en": "Apa Itu Model Lautan Elektron?", "id": "Model Untuk Menjelaskan Ikatan Logam." },
  { "en": "Apa Arti Elektron Terdelokalisasi?", "id": "Elektron Tidak Terikat Pada Satu Atom." },
  { "en": "Sifat Apa Yang Disebabkan Elektron Terdelokalisasi?", "id": "Konduktivitas Listrik Dan Panas Pada Logam." },
  { "en": "Apa Itu Kelimpahan Isotop (Isotopic Abundance)?", "id": "Persentase Isotop Tertentu Di Alam." },
  { "en": "Apakah Semua Isotop Bersifat Radioaktif?", "id": "Tidak Banyak Isotop Yang Stabil." },
  { "en": "Apa Isotop Yang Digunakan Dalam Terapi Kanker?", "id": "Kobalt-60." },
  { "en": "Apa Isotop Yang Digunakan Untuk Mengobati Tiroid?", "id": "Iodin-131." },
  { "en": "Apa Isotop Yang Digunakan Sebagai Bahan Bakar Nuklir?", "id": "Uranium-235." },
  { "en": "Bagaimana Cara Memisahkan Isotop?", "id": "Melalui Proses Pengayaan (Enrichment)." },
  { "en": "Apa Itu Zat Murni?", "id": "Zat Dengan Komposisi Kimia Seragam." },
  { "en": "Sebutkan Dua Jenis Zat Murni?", "id": "Unsur Dan Senyawa." },
  { "en": "Apa Itu Campuran (Mixture)?", "id": "Gabungan Zat Tanpa Reaksi Kimia." },
  { "en": "Apa Beda Senyawa Dan Campuran?", "id": "Senyawa Terikat Kimiawi Campuran Tidak." },
  { "en": "Apa Itu Campuran Homogen?", "id": "Campuran Yang Komposisinya Seragam Sempurna." },
  { "en": "Apa Nama Lain Campuran Homogen?", "id": "Larutan." },
  { "en": "Contoh Campuran Homogen?", "id": "Air Gula Udara." },
  { "en": "Apa Itu Campuran Heterogen?", "id": "Campuran Yang Komposisinya Tidak Seragam." },
  { "en": "Contoh Campuran Heterogen?", "id": "Pasir Dan Air Minyak Dan Air." },
  { "en": "Apa Itu Reaksi Kimia?", "id": "Proses Penataan Ulang Ikatan Atom." },
  { "en": "Apa Itu Reaktan?", "id": "Zat Awal Sebelum Terjadi Reaksi." },
  { "en": "Apa Itu Produk?", "id": "Zat Baru Yang Terbentuk Setelah Reaksi." },
  { "en": "Apa Bunyi Hukum Kekekalan Massa?", "id": "Massa Sebelum Dan Sesudah Reaksi Sama." },
  { "en": "Siapa Yang Mencetuskan Hukum Kekekalan Massa?", "id": "Antoine Lavoisier." },
  { "en": "Apa Itu Rumus Empiris?", "id": "Rasio Atom Paling Sederhana Dalam Senyawa." },
  { "en": "Apa Itu Rumus Molekul?", "id": "Jumlah Atom Aktual Dalam Satu Molekul." },
  { "en": "Contoh Rumus Empiris Dan Molekul?", "id": "CH₂O (Empiris) C₆H₁₂O₆ (Molekul)." },
  { "en": "Apa Itu Efek Perisai (Shielding Effect)?", "id": "Elektron Dalam Menghalangi Tarikan Inti." },
  { "en": "Bagaimana Efek Perisai Mempengaruhi Elektron Valensi?", "id": "Membuat Elektron Valensi Kurang Terikat Erat." },
  { "en": "Apa Itu Muatan Inti Efektif (Zeff)?", "id": "Muatan Positif Aktual Yang Dirasakan Elektron." },
  { "en": "Bagaimana Muatan Inti Efektif Berubah Dalam Periode?", "id": "Meningkat Dari Kiri Ke Kanan." },
  { "en": "Bagaimana Zeff Menjelaskan Tren Jari-Jari Atom?", "id": "Zeff Tinggi Menarik Elektron Lebih Kuat." },
  { "en": "Apa Itu Unsur Blok-S?", "id": "Unsur Dengan Elektron Terluar Di Orbital S." },
  { "en": "Golongan Mana Yang Termasuk Blok-S?", "id": "Golongan 1 (Alkali) Dan 2 (Alkali Tanah)." },
  { "en": "Apa Itu Unsur Blok-P?", "id": "Unsur Dengan Elektron Terluar Di Orbital P." },
  { "en": "Golongan Mana Yang Termasuk Blok-P?", "id": "Golongan 13 Hingga 18." },
  { "en": "Apa Itu Unsur Blok-D?", "id": "Unsur Dengan Elektron Mengisi Orbital D." },
  { "en": "Apa Nama Lain Unsur Blok-D?", "id": "Logam Transisi." },
  { "en": "Apa Itu Unsur Blok-F?", "id": "Unsur Dengan Elektron Mengisi Orbital F." },
  { "en": "Apa Nama Lain Unsur Blok-F?", "id": "Logam Transisi Dalam." },
  { "en": "Unsur Blok-F Terdiri Dari Apa Saja?", "id": "Lantanida Dan Aktinida." },
  { "en": "Apa Itu Lantanida?", "id": "Deret Unsur Setelah Lantanum." },
  { "en": "Apa Itu Aktinida?", "id": "Deret Unsur Setelah Aktinium." },
  { "en": "Apakah Sebagian Besar Aktinida Radioaktif?", "id": "Ya Semua Aktinida Bersifat Radioaktif." },
  { "en": "Apa Itu Ion Poliatomik?", "id": "Molekul Bermuatan Yang Terdiri Dari Banyak Atom." },
  { "en": "Contoh Ion Poliatomik?", "id": "Ion Sulfat (SO₄²⁻) Dan Amonium (NH₄⁺)." },
  { "en": "Apa Itu Proses Ionisasi?", "id": "Proses Pembentukan Ion Dari Atom Netral." },
  { "en": "Apa Itu Energi Ionisasi Kedua?", "id": "Energi Untuk Melepas Elektron Kedua." },
  { "en": "Mengapa Energi Ionisasi Kedua Selalu Lebih Besar?", "id": "Karena Melepas Elektron Dari Ion Positif." },
  { "en": "Apa Itu Penyinaran (Irradiation)?", "id": "Proses Memaparkan Sesuatu Pada Radiasi." },
  { "en": "Apakah Benda Yang Disinari Menjadi Radioaktif?", "id": "Tidak Kecuali Terjadi Kontaminasi." },
  { "en": "Apa Itu Kontaminasi Radioaktif?", "id": "Saat Materi Radioaktif Menempel Pada Benda." },
  { "en": "Apa Itu Spektrometer Massa?", "id": "Alat Untuk Mengukur Massa Atom Dan Molekul." },
  { "en": "Bagaimana Spektrometer Massa Bekerja?", "id": "Membelokkan Ion Dengan Medan Magnet." },
  { "en": "Ion Mana Yang Paling Sedikit Dibelokkan?", "id": "Ion Dengan Massa Paling Berat." },
  { "en": "Apa Itu Fluoresensi?", "id": "Pemancaran Cahaya Sesaat Setelah Menyerap Energi." },
  { "en": "Apa Itu Fosforesensi?", "id": "Pemancaran Cahaya Tertunda Setelah Menyerap Energi." },
  { "en": "Apa Perbedaan Utama Keduanya?", "id": "Kecepatan Pemancaran Kembali Cahaya." },
  { "en": "Contoh Benda Fosforesen?", "id": "Stiker 'Glow In The Dark'." },
  { "en": "Apa Itu Model Standar Fisika Partikel?", "id": "Teori Yang Menjelaskan Partikel Elementer." },
  { "en": "Menurut Model Standar Apa Saja Partikel Fundamental?", "id": "Kuark Lepton Dan Boson." },
  { "en": "Apa Itu Boson?", "id": "Partikel Pembawa Gaya Fundamental." },
  { "en": "Foton Adalah Pembawa Gaya Apa?", "id": "Gaya Elektromagnetik." },
  { "en": "Gluon Adalah Pembawa Gaya Apa?", "id": "Gaya Nuklir Kuat." },
  { "en": "Apa Itu Boson W Dan Z?", "id": "Pembawa Gaya Nuklir Lemah." },
  { "en": "Partikel Apa Yang Diduga Membawa Gravitasi?", "id": "Graviton (Masih Hipotetis)." },
  { "en": "Apa Itu Partikel Higgs Boson?", "id": "Partikel Yang Memberikan Massa Pada Partikel Lain." },
  { "en": "Apa Itu Akselerator Partikel?", "id": "Mesin Untuk Mempercepat Partikel Subatomik." },
  { "en": "Mengapa Partikel Perlu Dipercepat?", "id": "Untuk Mempelajari Interaksi Pada Energi Tinggi." },
  { "en": "Contoh Akselerator Partikel Terkenal?", "id": "Large Hadron Collider (LHC) Di CERN." },
  { "en": "Apa Itu Hadron?", "id": "Partikel Komposit Yang Terbuat Dari Kuark." },
  { "en": "Apa Saja Jenis Hadron?", "id": "Barion (Tiga Kuark) Dan Meson (Dua Kuark)." },
  { "en": "Apakah Proton Dan Neutron Termasuk Hadron?", "id": "Ya Keduanya Termasuk Dalam Kelompok Barion." },
  { "en": "Apa Itu Neutrino?", "id": "Partikel Lepton Netral Yang Sangat Ringan." },
  { "en": "Dari Mana Neutrino Berasal?", "id": "Dari Reaksi Nuklir Seperti Di Matahari." },
  { "en": "Mengapa Neutrino Sulit Dideteksi?", "id": "Karena Sangat Jarang Berinteraksi Dengan Materi." },
  { "en": "Apa Itu Prinsip Ketidakpastian Heisenberg?", "id": "Tidak Bisa Tahu Posisi Dan Momentum Sekaligus." },
  { "en": "Apa Implikasi Prinsip Ketidakpastian?", "id": "Membatasi Pengetahuan Kita Tentang Dunia Kuantum." },
  { "en": "Apa Itu Terowongan Kuantum (Quantum Tunneling)?", "id": "Partikel Menembus Penghalang Energi." },
  { "en": "Apakah Terowongan Kuantum Mungkin Dalam Dunia Klasik?", "id": "Tidak Ini Adalah Fenomena Kuantum Murni." },
  { "en": "Apa Itu Kondensat Bose-Einstein?", "id": "Wujud Zat Kelima." },
  { "en": "Bagaimana Kondensat Bose-Einstein Terbentuk?", "id": "Saat Atom Didinginkan Mendekati Nol Mutlak." },
  { "en": "Apa Itu Suhu Nol Mutlak?", "id": "Suhu Terdingin Yang Mungkin (Nol Kelvin)." },
  { "en": "Apa Yang Terjadi Pada Atom Di Suhu Nol Mutlak?", "id": "Semua Gerakan Atomik Berhenti." },
  { "en": "Apa Itu Entanglement Kuantum?", "id": "Keterikatan Aneh Antara Dua Partikel." },
  { "en": "Bagaimana Sifat Partikel Yang Terjerat?", "id": "Mengukur Satu Partikel Mempengaruhi Yang Lainnya." },
  { "en": "Apa Istilah Einstein Untuk Entanglement?", "id": "Aksi Seram Dari Jarak Jauh." },
  { "en": "Apa Itu Superposisi Kuantum?", "id": "Partikel Bisa Ada Dalam Banyak Keadaan Sekaligus." },
  { "en": "Kapan Superposisi Berakhir?", "id": "Saat Pengukuran Dilakukan." },
  { "en": "Apa Analogi Terkenal Untuk Superposisi?", "id": "Kucing Schrödinger." },
  { "en": "Apa Itu Orbital Molekul?", "id": "Orbital Yang Mencakup Seluruh Molekul." },
  { "en": "Bagaimana Orbital Molekul Terbentuk?", "id": "Kombinasi Linear Dari Orbital Atom." },
  { "en": "Apa Itu Orbital Ikatan (Bonding)?", "id": "Orbital Molekul Yang Menstabilkan Molekul." },
  { "en": "Apa Itu Orbital Anti-Ikatan (Antibonding)?", "id": "Orbital Molekul Yang Melemahkan Molekul." },
  { "en": "Elektron Mana Yang Lebih Stabil?", "id": "Elektron Di Orbital Ikatan." },
  { "en": "Apa Itu Orde Ikatan?", "id": "Setengah Selisih Elektron Ikatan Dan Anti-Ikatan." },
  { "en": "Apa Arti Orde Ikatan Nol?", "id": "Molekul Tidak Stabil Dan Tidak Terbentuk." },
  { "en": "Apa Itu Radikal Bebas?", "id": "Atom Atau Molekul Dengan Elektron Tidak Berpasangan." },
  { "en": "Bagaimana Sifat Radikal Bebas?", "id": "Sangat Reaktif Dan Tidak Stabil." },
  { "en": "Apa Itu Bilangan Oksidasi?", "id": "Ukuran Tingkat Oksidasi Atom." },
  { "en": "Apa Nama Lain Bilangan Oksidasi?", "id": "Keadaan Oksidasi." },
  { "en": "Berapa Bilangan Oksidasi Unsur Bebas?", "id": "Selalu Nol." },
  { "en": "Berapa Bilangan Oksidasi Ion Monoatomik?", "id": "Sama Dengan Muatan Ionnya." },
  { "en": "Apa Itu Reaksi Oksidasi?", "id": "Proses Kehilangan Elektron." },
  { "en": "Apa Itu Reaksi Reduksi?", "id": "Proses Menerima Elektron." },
  { "en": "Apa Itu Reaksi Redoks?", "id": "Reaksi Oksidasi Dan Reduksi Terjadi Bersamaan." },
  { "en": "Apa Itu Oksidator?", "id": "Zat Yang Menyebabkan Oksidasi." },
  { "en": "Apa Itu Reduktor?", "id": "Zat Yang Menyebabkan Reduksi." },
  { "en": "Apa Itu Kisi Kristal (Crystal Lattice)?", "id": "Susunan Tiga Dimensi Atom Yang Berulang." },
  { "en": "Apa Itu Sel Satuan (Unit Cell)?", "id": "Bagian Terkecil Yang Berulang Dari Kisi." },
  { "en": "Apa Itu Padatan Kristal?", "id": "Padatan Dengan Susunan Atom Yang Teratur." },
  { "en": "Contoh Padatan Kristal?", "id": "Garam Dapur Logam Dan Intan." },
  { "en": "Apa Itu Padatan Amorf?", "id": "Padatan Dengan Susunan Atom Tidak Teratur." },
  { "en": "Contoh Padatan Amorf?", "id": "Kaca Plastik Dan Karet." },
  { "en": "Apa Itu Struktur Kubus Sederhana (Simple Cubic)?", "id": "Atom Hanya Di Setiap Sudut Kubus." },
  { "en": "Apa Itu Struktur Kubus Berpusat Badan (BCC)?", "id": "Atom Di Sudut Dan Di Pusat Kubus." },
  { "en": "Apa Itu Struktur Kubus Berpusat Muka (FCC)?", "id": "Atom Di Sudut Dan Di Pusat Muka." },
  { "en": "Struktur Kristal Apa Yang Paling Rapat?", "id": "FCC Dan HCP (Hexagonal Close-Packed)." },
  { "en": "Apa Itu Polimorfisme?", "id": "Kemampuan Zat Membentuk Struktur Kristal Berbeda." },
  { "en": "Apa Itu Detektor Radiasi?", "id": "Alat Untuk Mendeteksi Partikel Atau Radiasi." },
  { "en": "Bagaimana Cara Kerja Pencacah Geiger (Geiger Counter)?", "id": "Mendeteksi Ionisasi Gas Oleh Radiasi." },
  { "en": "Radiasi Apa Yang Dideteksi Pencacah Geiger?", "id": "Alfa Beta Dan Gamma." },
  { "en": "Apa Itu Detektor Sintilasi?", "id": "Mendeteksi Kilatan Cahaya Akibat Radiasi." },
  { "en": "Apa Satuan SI Untuk Aktivitas Radioaktif?", "id": "Becquerel (Bq)." },
  { "en": "Apa Definisi Satu Becquerel?", "id": "Satu Peluruhan Per Detik." },
  { "en": "Apa Satuan Lama Untuk Aktivitas Radioaktif?", "id": "Curie (Ci)." },
  { "en": "Apa Satuan SI Untuk Dosis Serap?", "id": "Gray (Gy)." },
  { "en": "Apa Definisi Satu Gray?", "id": "Satu Joule Energi Per Kilogram Massa." },
  { "en": "Apa Satuan SI Untuk Dosis Ekuivalen?", "id": "Sievert (Sv)." },
  { "en": "Mengapa Dosis Ekuivalen Dibutuhkan?", "id": "Memperhitungkan Efek Biologis Jenis Radiasi." },
  { "en": "Radiasi Mana Yang Paling Berbahaya Secara Biologis?", "id": "Partikel Alfa (Jika Di Dalam Tubuh)." },
  { "en": "Apa Itu Pereaksi Pembatas?", "id": "Reaktan Yang Habis Terlebih Dahulu." },
  { "en": "Apa Fungsi Pereaksi Pembatas?", "id": "Menentukan Jumlah Maksimal Produk Yang Terbentuk." },
  { "en": "Apa Itu Pereaksi Berlebih?", "id": "Reaktan Yang Tersisa Setelah Reaksi Selesai." },
  { "en": "Apa Itu Hasil Teoritis?", "id": "Jumlah Maksimal Produk Menurut Perhitungan." },
  { "en": "Apa Itu Hasil Aktual?", "id": "Jumlah Produk Yang Sebenarnya Diperoleh." },
  { "en": "Mengapa Hasil Aktual Sering Lebih Kecil?", "id": "Karena Reaksi Tidak Sempurna Atau Ada Kerugian." },
  { "en": "Apa Itu Persen Hasil?", "id": "Rasio Hasil Aktual Terhadap Hasil Teoritis." },
  { "en": "Apa Itu Stoikiometri?", "id": "Studi Hubungan Kuantitatif Dalam Reaksi Kimia." },
  { "en": "Apa Itu Koefisien Stoikiometri?", "id": "Angka Di Depan Rumus Kimia." },
  { "en": "Apa Fungsi Koefisien Stoikiometri?", "id": "Menyeimbangkan Jumlah Atom Dalam Reaksi." },
  { "en": "Apa Itu Katoda?", "id": "Elektroda Tempat Terjadinya Reaksi Reduksi." },
  { "en": "Apa Itu Anoda?", "id": "Elektroda Tempat Terjadinya Reaksi Oksidasi." },
  { "en": "Dalam Sel Elektrolisis Katoda Bermuatan Apa?", "id": "Bermuatan Negatif." },
  { "en": "Dalam Sel Elektrolisis Anoda Bermuatan Apa?", "id": "Bermuatan Positif." },
  { "en": "Apa Itu Elektrolisis?", "id": "Penguraian Senyawa Menggunakan Arus Listrik." },
  { "en": "Apa Hasil Elektrolisis Air?", "id": "Gas Hidrogen Dan Gas Oksigen." },
  { "en": "Apa Itu Spektrometer?", "id": "Alat Untuk Mengukur Komponen Spektrum." },
  { "en": "Apa Fungsi Prisma Dalam Spektrometer?", "id": "Memisahkan Cahaya Menjadi Panjang Gelombang." },
  { "en": "Apa Rumus Rydberg?", "id": "Menghitung Panjang Gelombang Foton." },
  { "en": "Untuk Atom Apa Rumus Rydberg Bekerja Sempurna?", "id": "Atom Hidrogen." },
  { "en": "Siapa Yang Mengusulkan Nama 'Elektron'?", "id": "George Johnstone Stoney." },
  { "en": "Siapa Yang Menemukan Sinar-X?", "id": "Wilhelm Conrad Röntgen." },
  { "en": "Apa Itu Sinar-X?", "id": "Radiasi Elektromagnetik Berenergi Tinggi." },
  { "en": "Bagaimana Sinar-X Dihasilkan?", "id": "Saat Elektron Berkecepatan Tinggi Melambat Tiba-Tiba." },
  { "en": "Apa Itu Bintik Matahari?", "id": "Area Lebih Dingin Di Permukaan Matahari." },
  { "en": "Apa Hubungan Bintik Matahari Dengan Aktivitas Magnetik?", "id": "Bintik Matahari Disebabkan Aktivitas Magnetik Kuat." },
  { "en": "Apa Itu Angin Matahari?", "id": "Aliran Partikel Bermuatan Dari Matahari." },
  { "en": "Partikel Apa Yang Dominan Dalam Angin Matahari?", "id": "Proton Dan Elektron." },
  { "en": "Apa Itu Aurora?", "id": "Cahaya Indah Akibat Partikel Matahari." },
  { "en": "Bagaimana Aurora Terbentuk?", "id": "Partikel Menabrak Atom Di Atmosfer Bumi." },
  { "en": "Warna Apa Yang Dihasilkan Atom Oksigen?", "id": "Hijau Atau Merah." },
  { "en": "Warna Apa Yang Dihasilkan Atom Nitrogen?", "id": "Biru Atau Ungu." },
  { "en": "Apa Itu Metaloid?", "id": "Unsur Dengan Sifat Antara Logam Dan Nonlogam." },
  { "en": "Contoh Metaloid?", "id": "Silikon Germanium Arsen." },
  { "en": "Apa Sifat Semikonduktor?", "id": "Dapat Menghantarkan Listrik Dalam Kondisi Tertentu." },
  { "en": "Unsur Apa Yang Jadi Dasar Semikonduktor?", "id": "Silikon." },
  { "en": "Apa Itu Doping?", "id": "Menambahkan Atom Asing Ke Semikonduktor." },
  { "en": "Apa Tujuan Doping?", "id": "Untuk Mengubah Sifat Kelistrikan Semikonduktor." },
  { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Semikonduktor Dengan Kelebihan Elektron." },
  { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Semikonduktor Dengan Kekurangan Elektron (Lubang)." },
  { "en": "Apa Itu 'Lubang' (Hole) Dalam Semikonduktor?", "id": "Kekosongan Elektron Yang Bertindak Muatan Positif." },
  { "en": "Apa Itu Sinar Kosmik?", "id": "Partikel Berenergi Tinggi Dari Luar Angkasa." },
  { "en": "Apa Komponen Utama Sinar Kosmik?", "id": "Proton Dan Inti Atom." },
  { "en": "Apa Itu Reaksi Eksotermik?", "id": "Reaksi Kimia Yang Melepaskan Panas." },
  { "en": "Apa Itu Reaksi Endotermik?", "id": "Reaksi Kimia Yang Menyerap Panas." },
  { "en": "Apakah Fisi Nuklir Termasuk Reaksi Eksotermik?", "id": "Ya Melepaskan Energi Sangat Besar." },
  { "en": "Apa Itu Elektron Auger?", "id": "Elektron Yang Dipancarkan Dari Atom." },
  { "en": "Apa Itu Spektroskopi Elektron Auger?", "id": "Teknik Menganalisis Permukaan Material." },
  { "en": "Apa Itu Molekul Diatomik?", "id": "Molekul Yang Terdiri Dari Dua Atom." },
  { "en": "Contoh Molekul Diatomik?", "id": "Oksigen (O₂) Nitrogen (N₂) Klorin (Cl₂)." },
  { "en": "Apa Itu Massa Jenis (Density)?", "id": "Massa Per Satuan Volume." },
  { "en": "Unsur Alami Manakah Yang Paling Padat?", "id": "Osmium." },
  { "en": "Unsur Alami Manakah Yang Paling Ringan?", "id": "Hidrogen." },
  { "en": "Apa Itu Emisi Termionik?", "id": "Pancaran Elektron Dari Logam Panas." },
  { "en": "Di Mana Emisi Termionik Digunakan?", "id": "Pada Tabung Vakum Dan Tabung Sinar Katoda." },
  { "en": "Apa Itu Penangkapan Neutron?", "id": "Inti Atom Menyerap Neutron Bebas." },
  { "en": "Apa Hasil Dari Penangkapan Neutron?", "id": "Membentuk Isotop Yang Lebih Berat." },
  { "en": "Apa Itu Unsur Transuranik?", "id": "Unsur Dengan Nomor Atom Di Atas 92." },
  { "en": "Bagaimana Unsur Transuranik Dibuat?", "id": "Melalui Reaksi Nuklir Di Laboratorium." },
  { "en": "Apakah Semua Unsur Transuranik Tidak Stabil?", "id": "Ya Semuanya Bersifat Radioaktif." },
  { "en": "Apa Itu Teori Kuantum?", "id": "Teori Fisika Yang Menjelaskan Alam Skala Atom." },
  { "en": "Siapa Bapak Teori Kuantum?", "id": "Max Planck." },
  { "en": "Apa Itu Konstanta Planck?", "id": "Konstanta Fundamental Dalam Mekanika Kuantum." },
  { "en": "Apa Itu Defek Massa (Mass Defect)?", "id": "Selisih Massa Inti Dengan Total Massa Nukleon." },
  { "en": "Mengapa Ada Defek Massa?", "id": "Massa Berubah Menjadi Energi Pengikat Nuklir." },
  { "en": "Rumus Apa Yang Menghubungkan Massa Dan Energi?", "id": "Rumus E = mc² Milik Einstein." },
  { "en": "Apa Itu Energi Pengikat Nuklir?", "id": "Energi Yang Menahan Inti Atom Bersama." },
  { "en": "Apa Ciri Inti Dengan Energi Pengikat Tinggi?", "id": "Inti Tersebut Sangat Stabil." },
  { "en": "Inti Unsur Mana Yang Paling Stabil?", "id": "Besi-56 (Fe-56)." },
  { "en": "Apa Yang Terjadi Pada Unsur Lebih Ringan Dari Besi?", "id": "Melepaskan Energi Melalui Fusi." },
  { "en": "Apa Yang Terjadi Pada Unsur Lebih Berat Dari Besi?", "id": "Melepaskan Energi Melalui Fisi." },
  { "en": "Apa Itu Gaya Antarmolekul?", "id": "Gaya Tarik Atau Tolak Antar Molekul." },
  { "en": "Apakah Gaya Antarmolekul Sama Dengan Ikatan Kimia?", "id": "Tidak Jauh Lebih Lemah Dari Ikatan Kimia." },
  { "en": "Sebutkan Jenis Utama Gaya Van Der Waals?", "id": "Gaya Dispersi London Dan Interaksi Dipol-Dipol." },
  { "en": "Apa Itu Gaya Dispersi London?", "id": "Gaya Akibat Fluktuasi Elektron Sesaat." },
  { "en": "Pada Molekul Apa Gaya London Terjadi?", "id": "Terjadi Pada Semua Jenis Molekul." },
  { "en": "Manakah Gaya Antarmolekul Yang Paling Lemah?", "id": "Gaya Dispersi London." },
  { "en": "Apa Itu Interaksi Dipol-Dipol?", "id": "Gaya Antara Molekul Polar Permanen." },
  { "en": "Manakah Yang Lebih Kuat Dispersi London Atau Dipol-Dipol?", "id": "Interaksi Dipol-Dipol Umumnya Lebih Kuat." },
  { "en": "Ikatan Hidrogen Adalah Jenis Interaksi Apa?", "id": "Jenis Interaksi Dipol-Dipol Yang Sangat Kuat." },
  { "en": "Apa Itu Deret Lyman?", "id": "Transisi Elektron Ke Kulit n=1." },
  { "en": "Radiasi Apa Yang Dipancarkan Deret Lyman?", "id": "Radiasi Ultraviolet." },
  { "en": "Apa Itu Deret Balmer?", "id": "Transisi Elektron Ke Kulit n=2." },
  { "en": "Radiasi Apa Yang Dipancarkan Deret Balmer?", "id": "Cahaya Tampak Dan Ultraviolet." },
  { "en": "Apa Itu Emisi Spontan?", "id": "Elektron Pindah Ke Tingkat Energi Rendah." },
  { "en": "Apa Itu Emisi Terstimulasi (Stimulated Emission)?", "id": "Foton Menyebabkan Emisi Foton Identik Lainnya." },
  { "en": "Prinsip Apa Yang Digunakan Dalam Laser?", "id": "Prinsip Emisi Terstimulasi." },
  { "en": "Apa Kepanjangan Dari Laser?", "id": "Light Amplification By Stimulated Emission Of Radiation." },
  { "en": "Apa Itu Inversi Populasi?", "id": "Kondisi Lebih Banyak Atom Dalam Keadaan Tereksitasi." },
  { "en": "Mengapa Inversi Populasi Penting Untuk Laser?", "id": "Agar Emisi Terstimulasi Dominan." },
  { "en": "Apa Itu Isomer?", "id": "Molekul Dengan Rumus Kimia Sama Bentuk Berbeda." },
  { "en": "Apa Itu Isomer Struktural?", "id": "Isomer Dengan Konektivitas Atom Yang Berbeda." },
  { "en": "Apa Itu Stereoisomer?", "id": "Isomer Dengan Susunan Ruang Yang Berbeda." },
  { "en": "Apa Itu Rumus Struktural?", "id": "Gambar Yang Menunjukkan Bagaimana Atom Terhubung." },
  { "en": "Apa Itu Katalis?", "id": "Zat Yang Mempercepat Reaksi Kimia." },
  { "en": "Apakah Katalis Ikut Bereaksi?", "id": "Tidak Katalis Tidak Dikonsumsi Dalam Reaksi." },
  { "en": "Bagaimana Cara Kerja Katalis?", "id": "Menurunkan Energi Aktivasi Reaksi." },
  { "en": "Apa Itu Energi Aktivasi?", "id": "Energi Minimum Yang Dibutuhkan Untuk Reaksi." },
  { "en": "Apa Itu Enzim?", "id": "Katalis Biologis Yang Terbuat Dari Protein." },
  { "en": "Apa Yang Terjadi Pada Atom Saat Meleleh?", "id": "Mulai Bergerak Bebas Melewati Satu Sama Lain." },
  { "en": "Apa Yang Terjadi Pada Atom Saat Mendidih?", "id": "Lepas Dari Fase Cair Menjadi Gas." },
  { "en": "Apa Itu Sublimasi?", "id": "Perubahan Langsung Dari Padat Menjadi Gas." },
  { "en": "Apa Itu Deposisi?", "id": "Perubahan Langsung Dari Gas Menjadi Padat." },
  { "en": "Energi Apa Yang Dibutuhkan Untuk Mengubah Wujud?", "id": "Energi Panas Laten." },
  { "en": "Apa Itu Hukum Perbandingan Tetap (Proust)?", "id": "Rasio Massa Unsur Dalam Senyawa Tetap." },
  { "en": "Apa Itu Hukum Perbandingan Berganda (Dalton)?", "id": "Rasio Unsur Membentuk Bilangan Bulat Sederhana." },
  { "en": "Apa Empat Elemen Klasik Yunani Kuno?", "id": "Tanah Air Udara Dan Api." },
  { "en": "Apa Itu Alkimia (Alchemy)?", "id": "Pendahulu Kimia Modern." },
  { "en": "Apa Tujuan Utama Alkimia?", "id": "Mengubah Logam Biasa Menjadi Emas." },
  { "en": "Apa Itu Batu Bertuah (Philosopher's Stone)?", "id": "Zat Legendaris Dalam Alkimia." },
  { "en": "Apa Kontribusi Alkimia Pada Sains Modern?", "id": "Pengembangan Peralatan Laboratorium Dan Eksperimen." },
  { "en": "Siapa Yang Dianggap Bapak Kimia Modern?", "id": "Antoine Lavoisier." },
  { "en": "Apa Itu Valensi?", "id": "Ukuran Kemampuan Atom Untuk Berikatan." },
  { "en": "Bagaimana Valensi Berhubungan Dengan Elektron Valensi?", "id": "Biasanya Sama Dengan Jumlah Elektron Valensi." },
  { "en": "Apa Itu Jari-Jari Ionik?", "id": "Ukuran Jari-Jari Sebuah Ion." },
  { "en": "Bagaimana Tren Jari-Jari Ionik?", "id": "Mirip Dengan Tren Jari-Jari Atom." },
  { "en": "Apa Itu Muatan Nuklir?", "id": "Total Muatan Positif Di Inti Atom." },
  { "en": "Apa Itu Struktur Titik Elektron?", "id": "Nama Lain Untuk Struktur Lewis." },
  { "en": "Apa Itu Pasangan Ikatan?", "id": "Sepasang Elektron Yang Dibagi Antar Atom." },
  { "en": "Apa Itu Pasangan Sunyi (Lone Pair)?", "id": "Nama Lain Untuk Pasangan Elektron Bebas." },
  { "en": "Apa Itu Polaritas Molekul?", "id": "Distribusi Muatan Yang Tidak Merata." },
  { "en": "Apakah Molekul Simetris Bersifat Polar?", "id": "Tidak Molekul Simetris Biasanya Nonpolar." },
  { "en": "Contoh Molekul Polar?", "id": "Air (H₂O) Dan Amonia (NH₃)." },
  { "en": "Contoh Molekul Nonpolar?", "id": "Karbon Dioksida (CO₂) Dan Metana (CH₄)." },
  { "en": "Apa Itu Kelarutan?", "id": "Kemampuan Zat Untuk Larut Dalam Pelarut." },
  { "en": "Prinsip Kelarutan Apa Yang Populer?", "id": "Suka Melarutkan Suka (Like Dissolves Like)." },
  { "en": "Apa Maksud 'Like Dissolves Like'?", "id": "Zat Polar Larut Dalam Pelarut Polar." },
  { "en": "Mengapa Minyak Dan Air Tidak Bercampur?", "id": "Karena Minyak Nonpolar Dan Air Polar." },
  { "en": "Apa Itu Titik Leleh?", "id": "Suhu Saat Padatan Berubah Menjadi Cairan." },
  { "en": "Apa Itu Titik Didih?", "id": "Suhu Saat Cairan Berubah Menjadi Gas." },
  { "en": "Faktor Apa Yang Mempengaruhi Titik Didih?", "id": "Kekuatan Gaya Antarmolekul." },
  { "en": "Manakah Yang Punya Titik Didih Lebih Tinggi?", "id": "Senyawa Dengan Ikatan Hidrogen." },
  { "en": "Apa Itu Sifat Koligatif?", "id": "Sifat Larutan Yang Bergantung Pada Konsentrasi." },
  { "en": "Contoh Sifat Koligatif?", "id": "Penurunan Titik Beku Dan Kenaikan Titik Didih." },
  { "en": "Mengapa Garam Mencairkan Es Di Jalan?", "id": "Karena Garam Menurunkan Titik Beku Air." },
  { "en": "Apa Itu Tekanan Osmotik?", "id": "Tekanan Untuk Mencegah Terjadinya Osmosis." },
  { "en": "Apa Itu Materi Gelap (Dark Matter)?", "id": "Materi Hipotetis Yang Tidak Memancarkan Cahaya." },
  { "en": "Apakah Materi Gelap Terbuat Dari Atom?", "id": "Diperkirakan Tidak Terbuat Dari Atom Biasa." },
  { "en": "Apa Itu Energi Gelap (Dark Energy)?", "id": "Bentuk Energi Yang Mempercepat Ekspansi Alam Semesta." },
  { "en": "Berapa Persen Alam Semesta Terdiri Dari Atom Biasa?", "id": "Kurang Dari Lima Persen." },
  { "en": "Apa Itu Peluruhan Gamma?", "id": "Inti Melepaskan Kelebihan Energi Melalui Foton." },
  { "en": "Apakah Peluruhan Gamma Mengubah Nomor Atom?", "id": "Tidak Nomor Atom Dan Massa Tetap Sama." },
  { "en": "Radiasi Mana Yang Paling Sulit Dihentikan?", "id": "Radiasi Gamma." },
  { "en": "Bahan Apa Yang Bisa Menghentikan Sinar Gamma?", "id": "Timbal Tebal Atau Beton." },
  { "en": "Apa Yang Bisa Menghentikan Partikel Alfa?", "id": "Selembar Kertas Atau Kulit Manusia." },
  { "en": "Apa Yang Bisa Menghentikan Partikel Beta?", "id": "Selembar Aluminium Tipis." },
  { "en": "Apa Itu Radiasi Latar Belakang (Background Radiation)?", "id": "Radiasi Alami Yang Ada Di Lingkungan." },
  { "en": "Dari Mana Sumber Radiasi Latar Belakang?", "id": "Sinar Kosmik Batuan Dan Tubuh Manusia." },
  { "en": "Apa Itu Momen Magnetik Nuklir?", "id": "Sifat Magnetik Yang Dimiliki Inti Atom." },
  { "en": "Aplikasi Apa Yang Menggunakan Momen Magnetik Nuklir?", "id": "Pencitraan Resonansi Magnetik (MRI)." },
  { "en": "Apa Itu Positron Emission Tomography (PET Scan)?", "id": "Teknik Pencitraan Medis Menggunakan Positron." },
  { "en": "Bagaimana Cara Kerja PET Scan?", "id": "Mendeteksi Anihilasi Positron Dengan Elektron." },
  { "en": "Apa Itu Teori Atomisme?", "id": "Gagasan Filosofis Bahwa Materi Terdiri Dari Atom." },
  { "en": "Filsuf Yunani Kuno Siapa Yang Mengusulkan Atomisme?", "id": "Leukippos Dan Demokritos." },
  { "en": "Apa Itu Eter (Aether) Klasik?", "id": "Elemen Kelima Hipotetis Yang Mengisi Ruang." },
  { "en": "Apakah Teori Eter Masih Berlaku?", "id": "Tidak Teori Itu Telah Ditinggalkan." },
  { "en": "Apa Itu 'Ultraviolet Catastrophe'?", "id": "Kegagalan Fisika Klasik Menjelaskan Radiasi." },
  { "en": "Bagaimana Masalah Ini Dipecahkan?", "id": "Oleh Teori Kuantum Max Planck." },
  { "en": "Apa Itu Teori Kinetik Gas?", "id": "Gas Terdiri Dari Atom Yang Bergerak Acak." },
  { "en": "Bagaimana Teori Kinetik Menjelaskan Tekanan Gas?", "id": "Sebagai Tumbukan Atom Dengan Dinding Wadah." },
  { "en": "Bagaimana Teori Kinetik Menjelaskan Suhu?", "id": "Sebagai Ukuran Rata-Rata Energi Kinetik Atom." },
  { "en": "Mengapa Benda Memiliki Warna?", "id": "Menyerap Sebagian Cahaya Dan Memantulkan Sisanya." },
  { "en": "Warna Apa Yang Kita Lihat?", "id": "Warna Cahaya Yang Dipantulkan Oleh Benda." },
  { "en": "Apa Yang Terjadi Pada Foton Saat Diserap?", "id": "Energinya Mengeksitasi Elektron." },
  { "en": "Mengapa Benda Berwarna Hitam?", "id": "Menyerap Semua Panjang Gelombang Cahaya." },
  { "en": "Mengapa Benda Berwarna Putih?", "id": "Memantulkan Semua Panjang Gelombang Cahaya." },
  { "en": "Mengapa Logam Terlihat Berkilau?", "id": "Lautan Elektron Memantulkan Foton Secara Efisien." },
  { "en": "Mengapa Kaca Bersifat Transparan?", "id": "Foton Tidak Punya Cukup Energi Mengeksitasi Elektron." },
  { "en": "Mengapa Langit Berwarna Biru?", "id": "Hamburan Rayleigh Oleh Molekul Udara." },
  { "en": "Warna Apa Yang Paling Banyak Dihamburkan?", "id": "Cahaya Biru Dan Ungu." },
  { "en": "Mengapa Matahari Terbenam Berwarna Merah?", "id": "Cahaya Biru Telah Terhamburkan Jauh." },
  { "en": "Apa Itu Senyawa Biner?", "id": "Senyawa Yang Terdiri Dari Dua Unsur." },
  { "en": "Akhiran Apa Yang Digunakan Untuk Anion Monoatomik?", "id": "Akhiran -ida (Contoh: Klorida Oksida)." },
  { "en": "Apa Arti Akhiran -at Pada Ion Poliatomik?", "id": "Ion Dengan Jumlah Oksigen Lebih Banyak." },
  { "en": "Apa Arti Akhiran -it Pada Ion Poliatomik?", "id": "Ion Dengan Jumlah Oksigen Lebih Sedikit." },
  { "en": "Contoh Ion Dengan Akhiran -at Dan -it?", "id": "Sulfat (SO₄²⁻) Dan Sulfit (SO₃²⁻)." },
  { "en": "Apa Itu Sistem Stock?", "id": "Menggunakan Angka Romawi Untuk Menunjukkan Muatan." },
  { "en": "Kapan Sistem Stock Digunakan?", "id": "Untuk Logam Yang Punya Banyak Bilangan Oksidasi." },
  { "en": "Contoh Penggunaan Sistem Stock?", "id": "Besi(II) Oksida Dan Besi(III) Oksida." },
  { "en": "Apa Itu Massa Kritis?", "id": "Massa Minimum Untuk Reaksi Berantai Berkelanjutan." },
  { "en": "Apa Itu Keadaan Subkritis?", "id": "Reaksi Berantai Berhenti Dan Mati." },
  { "en": "Apa Itu Keadaan Superkritis?", "id": "Laju Reaksi Berantai Meningkat Secara Eksponensial." },
  { "en": "Keadaan Apa Yang Dibutuhkan Bom Atom?", "id": "Keadaan Superkritis." },
  { "en": "Bagaimana Massa Superkritis Dicapai Dalam Bom?", "id": "Dua Massa Subkritis Disatukan Dengan Cepat." },
  { "en": "Apa Itu Bom Termonuklir?", "id": "Bom Yang Menggunakan Reaksi Fusi Nuklir." },
  { "en": "Apa Pemicu Bom Termonuklir?", "id": "Ledakan Fisi Kecil Untuk Menciptakan Panas." },
  { "en": "Apa Itu Fallout Radioaktif?", "id": "Materi Radioaktif Yang Jatuh Dari Atmosfer." },
  { "en": "Apa Sifat Ulet (Ductility)?", "id": "Kemampuan Ditarik Menjadi Kawat Tanpa Putus." },
  { "en": "Apa Sifat Dapat Ditempa (Malleability)?", "id": "Kemampuan Dibentuk Tanpa Pecah." },
  { "en": "Sifat Apa Yang Umum Pada Logam?", "id": "Ulet Dan Dapat Ditempa." },
  { "en": "Mengapa Logam Bisa Ulet Dan Ditempa?", "id": "Lapisan Atom Dapat Bergeser Tanpa Memutus Ikatan." },
  { "en": "Apa Itu Sifat Rapuh (Brittle)?", "id": "Cenderung Pecah Atau Patah Saat Diberi Tekanan." },
  { "en": "Mengapa Senyawa Ionik Cenderung Rapuh?", "id": "Pergeseran Lapisan Menyebabkan Tolakan Antar Ion." },
  { "en": "Apa Itu Dislokasi Dalam Kristal?", "id": "Cacat Garis Dalam Susunan Kisi Kristal." },
  { "en": "Bagaimana Dislokasi Mempengaruhi Kekuatan Logam?", "id": "Gerakan Dislokasi Memungkinkan Logam Berubah Bentuk." },
  { "en": "Apa Itu Paduan (Alloy)?", "id": "Campuran Logam Dengan Unsur Lain." },
  { "en": "Contoh Paduan Logam?", "id": "Baja Kuningan Dan Perunggu." },
  { "en": "Mengapa Paduan Seringkali Lebih Kuat?", "id": "Atom Asing Menghalangi Gerakan Dislokasi." },
  { "en": "Apa Itu Energi Titik Nol (Zero-Point Energy)?", "id": "Energi Terendah Yang Mungkin Dimiliki Sistem." },
  { "en": "Apakah Atom Benar-Benar Berhenti Di Nol Mutlak?", "id": "Tidak Masih Bergetar Karena Energi Titik Nol." },
  { "en": "Apakah Atom Bisa Dihancurkan?", "id": "Bisa Diubah Menjadi Energi (E=mc²)." },
  { "en": "Apakah Atom Bisa Diciptakan?", "id": "Bisa Diciptakan Dari Energi Murni." },
  { "en": "Apa Itu Produksi Pasangan (Pair Production)?", "id": "Foton Berenergi Tinggi Menjadi Partikel-Antipartikel." },
  { "en": "Pasangan Apa Yang Umumnya Dihasilkan?", "id": "Pasangan Elektron Dan Positron." },
  { "en": "Apa Kebalikan Dari Produksi Pasangan?", "id": "Anihilasi." },
  { "en": "Apa Itu Inti Induk (Parent Nucleus)?", "id": "Inti Awal Sebelum Mengalami Peluruhan." },
  { "en": "Apa Itu Inti Anak (Daughter Nucleus)?", "id": "Inti Baru Yang Terbentuk Setelah Peluruhan." },
  { "en": "Dalam Peluruhan Alfa Apa Hubungan Induk Dan Anak?", "id": "Nomor Massa Anak Berkurang Empat." },
  { "en": "Dalam Peluruhan Beta Apa Hubungan Induk Dan Anak?", "id": "Nomor Atom Anak Bertambah Satu." },
  { "en": "Apa Peran Konstanta Planck (h)?", "id": "Menghubungkan Energi Foton Dengan Frekuensinya." },
  { "en": "Apa Peran Konstanta Boltzmann (k)?", "id": "Menghubungkan Energi Kinetik Partikel Dengan Suhu." },
  { "en": "Apa Itu Transisi Hiperhalus (Hyperfine Transition)?", "id": "Perubahan Energi Sangat Kecil Dalam Atom." },
  { "en": "Apa Yang Menyebabkan Transisi Hiperhalus?", "id": "Interaksi Antara Spin Elektron Dan Spin Inti." },
  { "en": "Mengapa Transisi Hiperhalus Digunakan Di Jam Atom?", "id": "Karena Frekuensinya Sangat Stabil Dan Tepat." },
  { "en": "Berapa Frekuensi Transisi Atom Sesium-133?", "id": "Sekitar 9 Miliar Siklus Per Detik." },
  { "en": "Apa Itu Diagram Orbital?", "id": "Cara Menggambarkan Distribusi Elektron Dalam Orbital." },
  { "en": "Apa Yang Direpresentasikan Kotak Dalam Diagram Orbital?", "id": "Satu Orbital Atom." },
  { "en": "Apa Yang Direpresentasikan Panah Dalam Diagram Orbital?", "id": "Satu Elektron Beserta Arah Spinnya." },
  { "en": "Apa Itu Zat Paramagnetik?", "id": "Zat Yang Sedikit Tertarik Medan Magnet." },
  { "en": "Apa Penyebab Sifat Paramagnetik?", "id": "Adanya Elektron Yang Tidak Berpasangan." },
  { "en": "Apa Itu Zat Diamagnetik?", "id": "Zat Yang Sedikit Ditolak Medan Magnet." },
  { "en": "Apa Penyebab Sifat Diamagnetik?", "id": "Semua Elektron Dalam Atom Berpasangan." },
  { "en": "Apakah Sebagian Besar Zat Bersifat Diamagnetik?", "id": "Ya Sifat Ini Umum Ditemukan." },
  { "en": "Apa Itu Zat Feromagnetik?", "id": "Zat Yang Sangat Kuat Tertarik Medan Magnet." },
  { "en": "Contoh Zat Feromagnetik?", "id": "Besi Kobalt Dan Nikel." },
  { "en": "Apa Yang Terjadi Pada Skala Atomik Di Zat Feromagnetik?", "id": "Momen Magnetik Atom Sejajar Satu Sama Lain." },
  { "en": "Apa Itu Domain Magnetik?", "id": "Wilayah Di Mana Momen Magnetik Sejajar." },
  { "en": "Apa Itu Suhu Curie?", "id": "Suhu Di Atas Mana Zat Feromagnetik Lenyap." },
  { "en": "Apa Yang Terjadi Di Atas Suhu Curie?", "id": "Zat Menjadi Paramagnetik Biasa." },
  { "en": "Apa Itu Efek Kiral (Chirality)?", "id": "Molekul Yang Merupakan Bayangan Cermin." },
  { "en": "Apakah Tangan Kiri Dan Kanan Kiral?", "id": "Ya Keduanya Adalah Contoh Kiralitas." },
  { "en": "Apa Itu Enantiomer?", "id": "Sepasang Molekul Kiral Yang Merupakan Bayangan Cermin." },
  { "en": "Mengapa Kiralitas Penting Dalam Biologi?", "id": "Enzim Seringkali Hanya Bekerja Pada Satu Enantiomer." },
  { "en": "Apa Itu Viskositas?", "id": "Ukuran Ketahanan Cairan Untuk Mengalir." },
  { "en": "Bagaimana Gaya Antarmolekul Mempengaruhi Viskositas?", "id": "Gaya Kuat Menghasilkan Viskositas Tinggi." },
  { "en": "Apa Itu Tegangan Permukaan?", "id": "Kecenderungan Cairan Untuk Mengerut." },
  { "en": "Apa Penyebab Tegangan Permukaan?", "id": "Gaya Tarik Kohesif Antar Molekul Cairan." },
  { "en": "Mengapa Serangga Bisa Berjalan Di Atas Air?", "id": "Karena Adanya Tegangan Permukaan Air." },
  { "en": "Apa Itu Aksi Kapilaritas?", "id": "Gerakan Cairan Naik Melawan Gravitasi." },
  { "en": "Apa Penyebab Aksi Kapilaritas?", "id": "Kombinasi Gaya Kohesi Dan Adhesi." },
  { "en": "Apa Itu Gaya Kohesi?", "id": "Gaya Tarik Antar Molekul Sejenis." },
  { "en": "Apa Itu Gaya Adhesi?", "id": "Gaya Tarik Antar Molekul Tidak Sejenis." },
  { "en": "Bagaimana Pohon Mengangkut Air Ke Atas?", "id": "Melalui Aksi Kapilaritas Di Dalam Xilem." },
  { "en": "Apa Itu Nomor Atom Efektif?", "id": "Ukuran Muatan Inti Rata-Rata." },
  { "en": "Apa Itu Atom Rydberg?", "id": "Atom Tereksitasi Dengan Elektron Sangat Jauh." },
  { "en": "Bagaimana Sifat Atom Rydberg?", "id": "Berukuran Sangat Besar Dan Sangat Rapuh." },
  { "en": "Apa Itu Difraksi Elektron?", "id": "Penyebaran Elektron Saat Melewati Celah Kecil." },
  { "en": "Apa Yang Dibuktikan Oleh Difraksi Elektron?", "id": "Sifat Gelombang Dari Elektron." },
  { "en": "Apa Itu Mikroskop Elektron?", "id": "Mikroskop Yang Menggunakan Elektron Bukan Cahaya." },
  { "en": "Mengapa Mikroskop Elektron Sangat Kuat?", "id": "Elektron Memiliki Panjang Gelombang Jauh Lebih Pendek." },
  { "en": "Apa Itu Scanning Tunneling Microscope (STM)?", "id": "Mikroskop Untuk Melihat Atom Individual." },
  { "en": "Bagaimana Cara Kerja STM?", "id": "Menggunakan Efek Terowongan Kuantum." },
  { "en": "Apa Itu Afinitas Proton?", "id": "Ukuran Kebasaan Suatu Spesies Fasa Gas." },
  { "en": "Apa Itu Keadaan Meta stabil?", "id": "Keadaan Tereksitasi Yang Bertahan Lebih Lama." },
  { "en": "Peran Apa Yang Dimainkan Keadaan Meta stabil?", "id": "Penting Dalam Fosforesensi Dan Laser." },
  { "en": "Apa Itu Koloid?", "id": "Campuran Dengan Partikel Lebih Besar Dari Larutan." },
  { "en": "Contoh Koloid?", "id": "Susu Kabut Dan Jeli." },
  { "en": "Apa Itu Nukleosintesis Big Bang?", "id": "Pembentukan Inti Atom Pertama Setelah Big Bang." },
  { "en": "Unsur Apa Yang Terbentuk Saat Big Bang?", "id": "Hidrogen Helium Dan Sedikit Litium." },
  { "en": "Apa Itu Nukleosintesis Bintang (Stellar)?", "id": "Pembentukan Unsur Berat Di Dalam Bintang." },
  { "en": "Bagaimana Bintang Menghasilkan Unsur Berat?", "id": "Melalui Reaksi Fusi Nuklir Di Intinya." },
  { "en": "Unsur Apa Batas Produksi Dalam Bintang Biasa?", "id": "Hingga Besi Dan Nikel." },
  { "en": "Di Mana Unsur Lebih Berat Dari Besi Terbentuk?", "id": "Dalam Ledakan Supernova." },
  { "en": "Apa Itu Nukleosintesis Supernova?", "id": "Pembentukan Unsur Terberat Saat Bintang Meledak." },
  { "en": "Dari Mana Asal Emas Dan Uranium Di Bumi?", "id": "Dari Ledakan Supernova Di Masa Lalu." },
  { "en": "Apa Itu Radiasi Cherenkov?", "id": "Cahaya Biru Dari Partikel Melebihi Kecepatan Cahaya." },
  { "en": "Di Mana Partikel Bisa Melebihi Kecepatan Cahaya?", "id": "Di Dalam Medium Seperti Air." },
  { "en": "Di Mana Radiasi Cherenkov Terlihat?", "id": "Di Sekitar Reaktor Nuklir Dalam Air." },
  { "en": "Apa Itu Bremsstrahlung?", "id": "Radiasi Akibat Perlambatan Partikel Bermuatan." },
  { "en": "Apa Arti Bremsstrahlung Dalam Bahasa Jerman?", "id": "Radiasi Pengereman (Braking Radiation)." },
  { "en": "Apa Itu Hamburan Compton?", "id": "Hamburan Foton Oleh Elektron Bebas." },
  { "en": "Apa Yang Terjadi Pada Foton Setelah Hamburan Compton?", "id": "Kehilangan Sebagian Energi Dan Panjang Gelombangnya Bertambah." },
  { "en": "Apa Yang Dibuktikan Hamburan Compton?", "id": "Sifat Partikel Dari Cahaya (Foton)." },
  { "en": "Apa Itu Hamburan Raman?", "id": "Hamburan Cahaya Tak Elastis Oleh Molekul." },
  { "en": "Apa Kegunaan Spektroskopi Raman?", "id": "Mengidentifikasi Molekul Berdasarkan Getaran Vibrasinya." },
  { "en": "Apa Itu Hidrat?", "id": "Senyawa Kristal Yang Mengandung Molekul Air." },
  { "en": "Apa Itu Zat Anhidrat?", "id": "Senyawa Hidrat Yang Airnya Telah Dihilangkan." },
  { "en": "Apa Itu Deliquescence?", "id": "Zat Menyerap Uap Air Hingga Menjadi Larutan." },
  { "en": "Apa Itu Efflorescence?", "id": "Zat Hidrat Kehilangan Airnya Ke Udara." },
  { "en": "Apa Itu Higroskopis?", "id": "Kemampuan Zat Menyerap Uap Air Dari Udara." },
  { "en": "Apa Itu Gugus Fungsional?", "id": "Kelompok Atom Spesifik Penentu Sifat Molekul." },
  { "en": "Apa Contoh Gugus Fungsional?", "id": "Hidroksil (-OH) Karbonil (-C=O)." },
  { "en": "Apa Sifat Gugus Hidroksil?", "id": "Membuat Molekul Menjadi Alkohol Dan Polar." },
  { "en": "Apa Bunyi Hukum Perbandingan Volume (Gay-Lussac)?", "id": "Volume Gas Bereaksi Memiliki Rasio Bilangan Bulat." },
  { "en": "Apa Bunyi Hukum Avogadro?", "id": "Volume Gas Sama Mengandung Jumlah Molekul Sama." },
  { "en": "Bagaimana Hukum Avogadro Menjelaskan Hukum Gay-Lussac?", "id": "Rasio Volume Sama Dengan Rasio Molekul." },
  { "en": "Apa Itu Elektron Inti (Core Electrons)?", "id": "Elektron Di Kulit Bagian Dalam." },
  { "en": "Apakah Elektron Inti Berpartisipasi Dalam Ikatan?", "id": "Tidak Umumnya Tidak Berpartisipasi." },
  { "en": "Apa Itu Muon Dan Tau?", "id": "Partikel Lepton Yang Lebih Berat Dari Elektron." },
  { "en": "Apakah Muon Dan Tau Stabil?", "id": "Tidak Keduanya Meluruh Dengan Sangat Cepat." },
  { "en": "Apa Itu Keadaan Degenerasi?", "id": "Saat Beberapa Keadaan Kuantum Punya Energi Sama." },
  { "en": "Contoh Keadaan Degenerasi?", "id": "Tiga Orbital P Dalam Atom Terisolasi." },
  { "en": "Bagaimana Degenerasi Dapat Dihilangkan?", "id": "Dengan Menerapkan Medan Magnet Eksternal." },
  { "en": "Apa Itu Model Atom Thomas-Fermi?", "id": "Model Awal Atom Berdasarkan Statistik Gas Elektron." },
  { "en": "Apa Itu Metode Hartree-Fock?", "id": "Metode Komputasi Untuk Menghitung Struktur Elektron." },
  { "en": "Apa Itu Pergeseran Lamb (Lamb Shift)?", "id": "Perbedaan Energi Kecil Antara Orbital Hidrogen." },
  { "en": "Apa Itu Konstanta Struktur Halus?", "id": "Konstanta Yang Mengkarakterisasi Kekuatan Interaksi Elektromagnetik." },
  { "en": "Apa Itu Penanggalan Radiometrik?", "id": "Teknik Menentukan Umur Batuan Dan Mineral." },
  { "en": "Prinsip Apa Yang Digunakan Penanggalan Radiometrik?", "id": "Perbandingan Jumlah Isotop Induk Dan Anak." },
  { "en": "Isotop Apa Yang Digunakan Untuk Batuan Sangat Tua?", "id": "Uranium-Timbal Atau Kalium-Argon." },
  { "en": "Bagaimana Atom Menjadi Dasar Biologi?", "id": "Membentuk Molekul Kompleks Seperti DNA Dan Protein." },
  { "en": "Unsur Apa Yang Menjadi Tulang Punggung Kehidupan?", "id": "Karbon." },
  { "en": "Mengapa Karbon Begitu Istimewa?", "id": "Dapat Membentuk Empat Ikatan Stabil." },
  { "en": "Apa Itu Ikatan Peptida?", "id": "Ikatan Yang Menghubungkan Asam Amino." },
  { "en": "Apa Yang Dibentuk Rantai Asam Amino?", "id": "Protein." },
  { "en": "Apa Itu Basa Nitrogen?", "id": "Molekul Pembangun Kode Genetik DNA." },
  { "en": "Apa Itu Titik Tripel?", "id": "Suhu Dan Tekanan Di Mana Tiga Wujud Zat Ada." },
  { "en": "Apa Itu Titik Kritis?", "id": "Titik Di Mana Fase Cair Dan Gas Tak Terbedakan." },
  { "en": "Apa Itu Fluida Superkritis?", "id": "Zat Di Atas Suhu Dan Tekanan Kritis." },
  { "en": "Apa Itu Efek Meissner?", "id": "Penolakan Medan Magnet Oleh Superkonduktor." },
  { "en": "Apa Itu Superkonduktor?", "id": "Material Dengan Resistansi Listrik Nol." },
  { "en": "Apa Itu Superfluida?", "id": "Cairan Dengan Viskositas Nol." },
  { "en": "Apa Sifat Aneh Superfluida?", "id": "Dapat Mengalir Ke Atas Melawan Gravitasi." },
  { "en": "Apa Itu Teori String?", "id": "Partikel Elementer Adalah Getaran String Kecil." },
  { "en": "Apakah Teori String Sudah Terbukti?", "id": "Belum Masih Merupakan Kerangka Teoritis." },
  { "en": "Apa Itu Kromodinamika Kuantum (QCD)?", "id": "Teori Yang Menjelaskan Interaksi Gaya Kuat." },
  { "en": "Apa Itu Elektrodinamika Kuantum (QED)?", "id": "Teori Kuantum Untuk Interaksi Elektromagnetik." },
  { "en": "Apa Itu Warna (Color) Dalam QCD?", "id": "Jenis Muatan Untuk Gaya Kuat." },
  { "en": "Apa Saja Jenis Muatan Warna Itu?", "id": "Merah Hijau Dan Biru." },
  { "en": "Apa Itu Pengurungan Kuark (Quark Confinement)?", "id": "Kuark Tidak Pernah Bisa Ditemukan Sendirian." },
  { "en": "Mengapa Kuark Tidak Bisa Diisolasi?", "id": "Karena Kekuatan Gaya Kuat Meningkat Dengan Jarak." },
  { "en": "Apa Itu Kebebasan Asimtotik?", "id": "Kuark Berperilaku Seperti Partikel Bebas." },
  { "en": "Kapan Kuark Berperilaku Bebas?", "id": "Saat Sangat Berdekatan Satu Sama Lain." },
  { "en": "Apa Itu Simetri Dalam Fisika?", "id": "Sifat Sistem Yang Tidak Berubah." },
  { "en": "Apa Itu Teorema Noether?", "id": "Menghubungkan Setiap Simetri Dengan Hukum Kekekalan." },
  { "en": "Simetri Terhadap Waktu Menghasilkan Apa?", "id": "Hukum Kekekalan Energi." },
  { "en": "Simetri Terhadap Translasi Ruang Menghasilkan Apa?", "id": "Hukum Kekekalan Momentum." },
  { "en": "Apa Itu Pelanggaran Paritas?", "id": "Pelanggaran Simetri Cermin Dalam Interaksi Lemah." },
  { "en": "Apa Itu Materi Eksotis?", "id": "Materi Hipotetis Dengan Sifat Aneh." },
  { "en": "Contoh Materi Eksotis?", "id": "Materi Dengan Massa Negatif." },
  { "en": "Apa Itu Radiasi Hawking?", "id": "Radiasi Termal Yang Dipancarkan Lubang Hitam." },
  { "en": "Bagaimana Radiasi Hawking Dihasilkan?", "id": "Akibat Efek Kuantum Dekat Cakrawala Peristiwa." },
  { "en": "Apa Itu Gelembung Vakum?", "id": "Fluktuasi Kuantum Di Ruang Hampa." },
  { "en": "Apa Itu Energi Vakum?", "id": "Energi Latar Belakang Yang Ada Di Ruang Hampa." },
  { "en": "Apa Itu Efek Casimir?", "id": "Gaya Tarik Antara Dua Pelat Dekat." },
  { "en": "Apa Penyebab Efek Casimir?", "id": "Fluktuasi Energi Vakum Kuantum." },
  { "en": "Apa Itu Radius Atom Bohr?", "id": "Jari-Jari Paling Mungkin Elektron Hidrogen." },
  { "en": "Apa Itu Momen Magnetik Bohr?", "id": "Satuan Momen Magnetik Atom." },
  { "en": "Apa Itu Fisika Atom Molekul Dan Optik?", "id": "Bidang Studi Sifat-Sifat Atom." },
  { "en": "Apa Itu Kondensasi Fermionik?", "id": "Wujud Zat Mirip Kondensat Bose-Einstein." },
  { "en": "Apa Beda Fermion Dan Boson?", "id": "Fermion Mematuhi Prinsip Pengecualian Pauli." },
  { "en": "Apakah Elektron Termasuk Fermion Atau Boson?", "id": "Elektron Adalah Fermion." },
  { "en": "Apakah Foton Termasuk Fermion Atau Boson?", "id": "Foton Adalah Boson." },
  { "en": "Apa Itu Spintronik?", "id": "Elektronik Yang Memanfaatkan Spin Elektron." },
  { "en": "Apa Itu Komputer Kuantum?", "id": "Komputer Yang Memanfaatkan Prinsip Kuantum." },
  { "en": "Apa Satuan Dasar Informasi Kuantum?", "id": "Qubit." },
  { "en": "Apa Kelebihan Qubit Dibanding Bit Klasik?", "id": "Qubit Dapat Berada Dalam Superposisi." },
  { "en": "Apa Itu Dekokrensi Kuantum?", "id": "Hilangnya Sifat Kuantum Akibat Lingkungan." },
  { "en": "Mengapa Membangun Komputer Kuantum Sangat Sulit?", "id": "Karena Efek Dekoherensi Kuantum." },
  { "en": "Apa Itu Teori Medan Kuantum?", "id": "Kerangka Teori Untuk Fisika Partikel." },
  { "en": "Apa Konsep Utama Teori Medan Kuantum?", "id": "Partikel Adalah Eksitasi Dari Medan Kuantum." },
  { "en": "Apa Itu Unifikasi Besar (Grand Unification)?", "id": "Upaya Menyatukan Tiga Gaya Fundamental." },
  { "en": "Gaya Apa Yang Ingin Disatukan?", "id": "Elektromagnetik Lemah Dan Kuat." },
  { "en": "Apa Itu Teori Segalanya (Theory of Everything)?", "id": "Upaya Menyatukan Semua Gaya Termasuk Gravitasi." },
  { "en": "Apakah Teori Segalanya Sudah Ditemukan?", "id": "Belum Ini Adalah Tujuan Akhir Fisika." }



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
