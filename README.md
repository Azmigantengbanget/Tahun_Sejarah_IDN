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


    { "en": "Kerajaan Kutai berdiri?", "id": "400" },
    { "en": "Prasasti Yupa dibuat?", "id": "400" },
    { "en": "Kerajaan Tarumanegara berdiri?", "id": "358" },
    { "en": "Prasasti Ciaruteun ditulis?", "id": "450" },
    { "en": "Kerajaan Sriwijaya berdiri?", "id": "650" },
    { "en": "Prasasti Kedukan Bukit ditulis?", "id": "683" },
    { "en": "Kerajaan Mataram Kuno (Hindu-Buddha) berdiri?", "id": "732" },
    { "en": "Candi Borobudur mulai dibangun?", "id": "750" },
    { "en": "Candi Prambanan dibangun?", "id": "850" },
    { "en": "Wangsa Syailendra berkuasa di Mataram?", "id": "752" },
    { "en": "Wangsa Sanjaya berkuasa di Mataram?", "id": "732" },
    { "en": "Kerajaan Medang (Mataram Kuno) pindah ke Jawa Timur?", "id": "929" },
    { "en": "Mpu Sindok memerintah?", "id": "929" },
    { "en": "Kerajaan Kahuripan berdiri?", "id": "1019" },
    { "en": "Airlangga membagi Kerajaan Kahuripan?", "id": "1045" },
    { "en": "Kerajaan Kediri berdiri?", "id": "1045" },
    { "en": "Kerajaan Janggala berdiri?", "id": "1045" },
    { "en": "Jayabaya memerintah Kediri?", "id": "1135" },
    { "en": "Kerajaan Singhasari berdiri?", "id": "1222" },
    { "en": "Ken Arok mendirikan Singhasari?", "id": "1222" },
    { "en": "Pertempuran Ganter terjadi?", "id": "1222" },
    { "en": "Kertanegara memerintah Singhasari?", "id": "1268" },
    { "en": "Ekspedisi Pamalayu diluncurkan?", "id": "1275" },
    { "en": "Serangan Mongol ke Jawa?", "id": "1293" },
    { "en": "Kerajaan Majapahit berdiri?", "id": "1293" },
    { "en": "Raden Wijaya menjadi raja Majapahit pertama?", "id": "1293" },
    { "en": "Hayam Wuruk lahir?", "id": "1334" },
    { "en": "Sumpah Palapa diucapkan Gajah Mada?", "id": "1336" },
    { "en": "Hayam Wuruk naik takhta Majapahit?", "id": "1350" },
    { "en": "Perang Bubat terjadi?", "id": "1357" },
    { "en": "Gajah Mada wafat?", "id": "1364" },
    { "en": "Hayam Wuruk wafat?", "id": "1389" },
    { "en": "Perang Paregreg di Majapahit?", "id": "1404" },
    { "en": "Majapahit runtuh?", "id": "1478" },
    { "en": "Kerajaan Samudera Pasai menjadi kesultanan Islam?", "id": "1267" },
    { "en": "Marco Polo mengunjungi Sumatera?", "id": "1292" },
    { "en": "Kesultanan Malaka didirikan?", "id": "1400" },
    { "en": "Wali Songo menyebarkan Islam di Jawa?", "id": "1404" },
    { "en": "Kesultanan Demak berdiri?", "id": "1475" },
    { "en": "Raden Patah menjadi Sultan Demak pertama?", "id": "1475" },
    { "en": "Portugis menaklukkan Malaka?", "id": "1511" },
    { "en": "Pati Unus menyerang Portugis di Malaka?", "id": "1513" },
    { "en": "Sultan Trenggana memerintah Demak?", "id": "1521" },
    { "en": "Fatahillah merebut Sunda Kelapa?", "id": "1527" },
    { "en": "Nama Jayakarta digunakan?", "id": "1527" },
    { "en": "Kesultanan Banten berdiri?", "id": "1527" },
    { "en": "Kesultanan Cirebon berdiri?", "id": "1430" },
    { "en": "Sunan Gunung Jati wafat?", "id": "1568" },
    { "en": "Kesultanan Pajang berdiri?", "id": "1568" },
    { "en": "Joko Tingkir (Sultan Hadiwijaya) memerintah?", "id": "1568" },
    { "en": "Kesultanan Mataram Islam berdiri?", "id": "1586" },
    { "en": "Panembahan Senopati menjadi raja Mataram?", "id": "1586" },
    { "en": "Sultan Agung memerintah Mataram?", "id": "1613" },
    { "en": "Sultan Agung menyerang Batavia pertama kali?", "id": "1628" },
    { "en": "Sultan Agung menyerang Batavia kedua kali?", "id": "1629" },
    { "en": "Perjanjian Giyanti ditandatangani?", "id": "1755" },
    { "en": "Kesultanan Yogyakarta berdiri?", "id": "1755" },
    { "en": "Kasunanan Surakarta berdiri?", "id": "1755" },
    { "en": "Perjanjian Salatiga ditandatangani?", "id": "1757" },
    { "en": "Kadipaten Mangkunegaran didirikan?", "id": "1757" },
    { "en": "Cornelis de Houtman tiba di Banten?", "id": "1596" },
    { "en": "VOC (Vereenigde Oostindische Compagnie) didirikan?", "id": "1602" },
    { "en": "Jan Pieterszoon Coen menjadi Gubernur Jenderal VOC?", "id": "1619" },
    { "en": "VOC membangun Batavia?", "id": "1619" },
    { "en": "VOC dibubarkan?", "id": "1799" },
    { "en": "Pemerintahan Hindia Belanda dimulai?", "id": "1800" },
    { "en": "Herman Willem Daendels menjadi Gubernur Jenderal?", "id": "1808" },
    { "en": "Jalan Raya Pos (Anyer-Panarukan) dibangun?", "id": "1808" },
    { "en": "Inggris menguasai Jawa?", "id": "1811" },
    { "en": "Thomas Stamford Raffles menjadi Letnan Gubernur Jawa?", "id": "1811" },
    { "en": "Buku \"The History of Java\" terbit?", "id": "1817" },
    { "en": "Perjanjian London (Konvensi London) ditandatangani?", "id": "1814" },
    { "en": "Belanda kembali menguasai Hindia Belanda?", "id": "1816" },
    { "en": "Perang Padri dimulai?", "id": "1803" },
    { "en": "Perang Diponegoro dimulai?", "id": "1825" },
    { "en": "Pangeran Diponegoro ditangkap?", "id": "1830" },
    { "en": "Cultuurstelsel (Tanam Paksa) diterapkan?", "id": "1830" },
    { "en": "Perang Padri berakhir?", "id": "1838" },
    { "en": "Tuanku Imam Bonjol ditangkap?", "id": "1837" },
    { "en": "Perang Aceh dimulai?", "id": "1873" },
    { "en": "Snouck Hurgronje tiba di Aceh?", "id": "1891" },
    { "en": "Cut Nyak Dien ditangkap?", "id": "1905" },
    { "en": "Perang Aceh berakhir?", "id": "1904" },
    { "en": "Puputan Badung terjadi?", "id": "1906" },
    { "en": "Sisingamangaraja XII wafat?", "id": "1907" },
    { "en": "Boedi Oetomo didirikan?", "id": "1908" },
    { "en": "Sarekat Dagang Islam didirikan?", "id": "1905" },
    { "en": "Sarekat Islam didirikan?", "id": "1912" },
    { "en": "Indische Partij didirikan?", "id": "1912" },
    { "en": "Muhammadiyah didirikan?", "id": "1912" },
    { "en": "Pahlawan Tiga Serangkai diasingkan?", "id": "1913" },
    { "en": "ISDV (Indische Sociaal-Democratische Vereeniging) berdiri?", "id": "1914" },
    { "en": "Volksraad (Dewan Rakyat) dibentuk?", "id": "1918" },
    { "en": "Partai Komunis Indonesia (PKI) berdiri?", "id": "1920" },
    { "en": "Taman Siswa didirikan?", "id": "1922" },
    { "en": "Perhimpoenan Indonesia didirikan?", "id": "1922" },
    { "en": "Pemberontakan PKI di Banten?", "id": "1926" },
    { "en": "Partai Nasional Indonesia (PNI) didirikan?", "id": "1927" },
    { "en": "Soekarno memimpin PNI?", "id": "1927" },
    { "en": "Kongres Pemuda I diadakan?", "id": "1926" },
    { "en": "Kongres Pemuda II diadakan?", "id": "1928" },
    { "en": "Sumpah Pemuda diikrarkan?", "id": "1928" },
    { "en": "Lagu Indonesia Raya pertama kali diperdengarkan?", "id": "1928" },
    { "en": "Soekarno ditangkap dan dipenjara di Banceuy?", "id": "1929" },
    { "en": "Pledoi \"Indonesia Menggugat\" dibacakan?", "id": "1930" },
    { "en": "Soekarno diasingkan ke Ende?", "id": "1934" },
    { "en": "Soekarno dipindahkan ke Bengkulu?", "id": "1938" },
    { "en": "Nahdlatul Ulama (NU) didirikan?", "id": "1926" },
    { "en": "GAPI (Gabungan Politik Indonesia) dibentuk?", "id": "1939" },
    { "en": "Jepang mendarat di Tarakan?", "id": "1942" },
    { "en": "Belanda menyerah kepada Jepang di Kalijati?", "id": "1942" },
    { "en": "Pemerintahan pendudukan Jepang dimulai?", "id": "1942" },
    { "en": "Putera (Pusat Tenaga Rakyat) dibentuk?", "id": "1943" },
    { "en": "PETA (Pembela Tanah Air) dibentuk?", "id": "1943" },
    { "en": "Romusha (kerja paksa) diterapkan?", "id": "1942" },
    { "en": "Janji Koiso tentang kemerdekaan Indonesia?", "id": "1944" },
    { "en": "BPUPKI (Badan Penyelidik Usaha-usaha Persiapan Kemerdekaan Indonesia) dibentuk?", "id": "1945" },
    { "en": "Sidang pertama BPUPKI?", "id": "1945" },
    { "en": "Soekarno menyampaikan pidato Lahirnya Pancasila?", "id": "1945" },
    { "en": "Piagam Jakarta dirumuskan?", "id": "1945" },
    { "en": "PPKI (Panitia Persiapan Kemerdekaan Indonesia) dibentuk?", "id": "1945" },
    { "en": "Bom atom di Hiroshima?", "id": "1945" },
    { "en": "Bom atom di Nagasaki?", "id": "1945" },
    { "en": "Jepang menyerah kepada Sekutu?", "id": "1945" },
    { "en": "Peristiwa Rengasdengklok terjadi?", "id": "1945" },
    { "en": "Proklamasi Kemerdekaan Indonesia?", "id": "1945" },
    { "en": "UUD 1945 disahkan?", "id": "1945" },
    { "en": "Soekarno dan Hatta diangkat menjadi Presiden dan Wakil Presiden?", "id": "1945" },
    { "en": "Komite Nasional Indonesia Pusat (KNIP) dibentuk?", "id": "1945" },
    { "en": "Insiden Hotel Yamato di Surabaya?", "id": "1945" },
    { "en": "Pertempuran Lima Hari di Semarang?", "id": "1945" },
    { "en": "Pertempuran Surabaya meletus?", "id": "1945" },
    { "en": "Hari Pahlawan diperingati?", "id": "1945" },
    { "en": "Ibu kota pindah ke Yogyakarta?", "id": "1946" },
    { "en": "Perjanjian Linggarjati ditandatangani?", "id": "1946" },
    { "en": "Agresi Militer Belanda I?", "id": "1947" },
    { "en": "Komisi Tiga Negara (KTN) dibentuk?", "id": "1947" },
    { "en": "Perjanjian Renville ditandatangani?", "id": "1948" },
    { "en": "Pemberontakan PKI Madiun?", "id": "1948" },
    { "en": "Agresi Militer Belanda II?", "id": "1948" },
    { "en": "Pemerintahan Darurat Republik Indonesia (PDRI) dibentuk?", "id": "1948" },
    { "en": "Serangan Umum 1 Maret?", "id": "1949" },
    { "en": "Perjanjian Roem-Royen?", "id": "1949" },
    { "en": "Konferensi Meja Bundar (KMB) di Den Haag?", "id": "1949" },
    { "en": "Pengakuan kedaulatan Indonesia oleh Belanda?", "id": "1949" },
    { "en": "Republik Indonesia Serikat (RIS) dibentuk?", "id": "1949" },
    { "en": "Indonesia kembali menjadi Negara Kesatuan Republik Indonesia (NKRI)?", "id": "1950" },
    { "en": "Pemberontakan Andi Azis di Makassar?", "id": "1950" },
    { "en": "Pemberontakan Republik Maluku Selatan (RMS)?", "id": "1950" },
    { "en": "Pemberontakan DI/TII di Jawa Barat dimulai?", "id": "1949" },
    { "en": "Pemilu pertama di Indonesia?", "id": "1955" },
    { "en": "Konferensi Asia-Afrika di Bandung?", "id": "1955" },
    { "en": "Dekrit Presiden dikeluarkan?", "id": "1959" },
    { "en": "Demokrasi Terpimpin dimulai?", "id": "1959" },
    { "en": "Manipol-USDEK dijadikan haluan negara?", "id": "1959" },
    { "en": "Irian Barat (Papua) menjadi bagian dari Indonesia?", "id": "1963" },
    { "en": "Operasi Trikora dilancarkan?", "id": "1961" },
    { "en": "Perjanjian New York ditandatangani?", "id": "1962" },
    { "en": "Konfrontasi dengan Malaysia dimulai?", "id": "1963" },
    { "en": "Gerakan 30 September (G30S) terjadi?", "id": "1965" },
    { "en": "Surat Perintah Sebelas Maret (Supersemar) dikeluarkan?", "id": "1966" },
    { "en": "Orde Baru dimulai?", "id": "1966" },
    { "en": "Soeharto menjadi Pejabat Presiden?", "id": "1967" },
    { "en": "Soeharto dilantik menjadi Presiden?", "id": "1968" },
    { "en": "Indonesia kembali menjadi anggota PBB?", "id": "1966" },
    { "en": "ASEAN didirikan?", "id": "1967" },
    { "en": "Penentuan Pendapat Rakyat (Pepera) di Irian Barat?", "id": "1969" },
    { "en": "Pemilu pertama Orde Baru?", "id": "1971" },
    { "en": "Malari (Malapetaka Lima Belas Januari) terjadi?", "id": "1974" },
    { "en": "Indonesia menginvasi Timor Timur?", "id": "1975" },
    { "en": "Timor Timur menjadi provinsi ke-27 Indonesia?", "id": "1976" },
    { "en": "Sistem satelit Palapa diluncurkan?", "id": "1976" },
    { "en": "Swasembada beras tercapai?", "id": "1984" },
    { "en": "Program Keluarga Berencana (KB) digalakkan?", "id": "1970" },
    { "en": "Krisis moneter Asia melanda Indonesia?", "id": "1997" },
    { "en": "Tragedi Trisakti terjadi?", "id": "1998" },
    { "en": "Kerusuhan Mei terjadi?", "id": "1998" },
    { "en": "Soeharto mengundurkan diri?", "id": "1998" },
    { "en": "Era Reformasi dimulai?", "id": "1998" },
    { "en": "B.J. Habibie menjadi Presiden?", "id": "1998" },
    { "en": "Referendum Timor Timur diadakan?", "id": "1999" },
    { "en": "Pemilu era Reformasi pertama?", "id": "1999" },
    { "en": "Abdurrahman Wahid (Gus Dur) menjadi Presiden?", "id": "1999" },
    { "en": "Megawati Soekarnoputri menjadi Presiden?", "id": "2001" },
    { "en": "Bom Bali I terjadi?", "id": "2002" },
    { "en": "Pemilu Presiden langsung pertama?", "id": "2004" },
    { "en": "Susilo Bambang Yudhoyono (SBY) menjadi Presiden?", "id": "2004" },
    { "en": "Tsunami Aceh terjadi?", "id": "2004" },
    { "en": "Perjanjian damai Helsinki (RI-GAM) ditandatangani?", "id": "2005" },
    { "en": "Joko Widodo menjadi Presiden?", "id": "2014" },
    { "en": "Prasasti Talang Tuo dibuat?", "id": "684" },
    { "en": "Prasasti Kota Kapur ditemukan?", "id": "686" },
    { "en": "Prasasti Karang Brahi dibuat?", "id": "686" },
    { "en": "Serangan Sriwijaya ke Jawa?", "id": "990" },
    { "en": "Prasasti Canggal dibuat?", "id": "732" },
    { "en": "Candi Kalasan dibangun?", "id": "778" },
    { "en": "Candi Sewu dibangun?", "id": "782" },
    { "en": "Balaputradewa menjadi raja Sriwijaya?", "id": "856" },
    { "en": "Kitab Sutasoma ditulis Mpu Tantular?", "id": "1365" },
    { "en": "Kitab Negarakertagama ditulis Mpu Prapanca?", "id": "1365" },
    { "en": "Armada Cheng Ho mengunjungi Jawa?", "id": "1405" },
    { "en": "Kerajaan Gowa-Tallo mencapai puncak kejayaan?", "id": "1631" },
    { "en": "Sultan Hasanuddin memerintah Gowa?", "id": "1653" },
    { "en": "Perjanjian Bongaya ditandatangani?", "id": "1667" },
    { "en": "Kerajaan Ternate mencapai puncak kejayaan?", "id": "1570" },
    { "en": "Sultan Baabullah mengusir Portugis dari Ternate?", "id": "1575" },
    { "en": "Amangkurat I memerintah Mataram?", "id": "1646" },
    { "en": "Pemberontakan Trunajaya?", "id": "1674" },
    { "en": "Amangkurat I wafat?", "id": "1677" },
    { "en": "VOC mengintervensi Mataram?", "id": "1677" },
    { "en": "Untung Suropati memberontak?", "id": "1686" },
    { "en": "Perang Geger Pacinan?", "id": "1740" },
    { "en": "Raffles menemukan bunga Rafflesia arnoldii?", "id": "1818" },
    { "en": "Pattimura memimpin perlawanan di Saparua?", "id": "1817" },
    { "en": "Perang Banjar dimulai?", "id": "1859" },
    { "en": "Pangeran Antasari wafat?", "id": "1862" },
    { "en": "Max Havelaar karya Multatuli terbit?", "id": "1860" },
    { "en": "Politik Etis (Politik Balas Budi) dicanangkan?", "id": "1901" },
    { "en": "Kartini wafat?", "id": "1904" },
    { "en": "Buku \"Habis Gelap Terbitlah Terang\" terbit?", "id": "1911" },
    { "en": "HOS Tjokroaminoto memimpin Sarekat Islam?", "id": "1914" },
    { "en": "Pemberontakan PKI di Sumatera Barat?", "id": "1927" },
    { "en": "Wage Rudolf Supratman lahir?", "id": "1903" },
    { "en": "Mohammad Hatta menjadi ketua Perhimpoenan Indonesia?", "id": "1926" },
    { "en": "Mohammad Hatta ditangkap di Belanda?", "id": "1927" },
    { "en": "Pledoi \"Indonesia Vrij\" dibacakan Hatta?", "id": "1928" },
    { "en": "Partindo (Partai Indonesia) didirikan?", "id": "1931" },
    { "en": "PNI Baru didirikan Hatta dan Sjahrir?", "id": "1931" },
    { "en": "Hatta dan Sjahrir diasingkan ke Boven Digoel?", "id": "1935" },
    { "en": "Petisi Soetardjo diajukan?", "id": "1936" },
    { "en": "Heiho (prajurit pembantu Jepang) dibentuk?", "id": "1943" },
    { "en": "Pemberontakan PETA di Blitar oleh Supriyadi?", "id": "1945" },
    { "en": "Badan Keamanan Rakyat (BKR) dibentuk?", "id": "1945" },
    { "en": "Tentara Keamanan Rakyat (TKR) dibentuk?", "id": "1945" },
    { "en": "Pertempuran Ambarawa terjadi?", "id": "1945" },
    { "en": "Bandung Lautan Api terjadi?", "id": "1946" },
    { "en": "Puputan Margarana di Bali?", "id": "1946" },
    { "en": "I Gusti Ngurah Rai gugur?", "id": "1946" },
    { "en": "Sutan Sjahrir menjadi Perdana Menteri pertama?", "id": "1945" },
    { "en": "Indonesia menjadi anggota PBB ke-60?", "id": "1950" },
    { "en": "Uang Republik Indonesia (ORI) pertama kali beredar?", "id": "1946" },
    { "en": "Amir Sjarifuddin menjadi Perdana Menteri?", "id": "1947" },
    { "en": "Kabinet Hatta I dibentuk?", "id": "1948" },
    { "en": "Pemberontakan DI/TII di Aceh dimulai?", "id": "1953" },
    { "en": "Pemberontakan PRRI (Pemerintahan Revolusioner Republik Indonesia)?", "id": "1958" },
    { "en": "Pemberontakan Permesta (Perjuangan Rakyat Semesta)?", "id": "1957" },
    { "en": "Djuanda Kartawidjaja menjadi Perdana Menteri?", "id": "1957" },
    { "en": "Deklarasi Djuanda tentang batas laut teritorial?", "id": "1957" },
    { "en": "Nasionalisasi perusahaan-perusahaan Belanda?", "id": "1957" },
    { "en": "Asian Games IV diadakan di Jakarta?", "id": "1962" },
    { "en": "Monumen Nasional (Monas) mulai dibangun?", "id": "1961" },
    { "en": "GANEFO (Games of the New Emerging Forces) diadakan?", "id": "1963" },
    { "en": "D.N. Aidit memimpin PKI?", "id": "1951" },
    { "en": "Indonesia keluar dari PBB?", "id": "1965" },
    { "en": "Tritura (Tiga Tuntutan Rakyat) diserukan?", "id": "1966" },
    { "en": "PKI dibubarkan?", "id": "1966" },
    { "en": "Kabinet Ampera dibentuk?", "id": "1966" },
    { "en": "Pelita (Pembangunan Lima Tahun) I dimulai?", "id": "1969" },
    { "en": "Adam Malik menjadi Wakil Presiden?", "id": "1978" },
    { "en": "Insiden Woyla (pembajakan pesawat Garuda)?", "id": "1981" },
    { "en": "Peristiwa Tanjung Priok?", "id": "1984" },
    { "en": "Peristiwa Talangsari Lampung?", "id": "1989" },
    { "en": "ICMI (Ikatan Cendekiawan Muslim Indonesia) didirikan?", "id": "1990" },
    { "en": "Peristiwa 27 Juli (Kudatuli)?", "id": "1996" },
    { "en": "UU Antisubversi dicabut?", "id": "1999" },
    { "en": "Otonomi Daerah mulai diterapkan?", "id": "2001" },
    { "en": "Amandemen UUD 1945 pertama?", "id": "1999" },
    { "en": "Amandemen UUD 1945 kedua?", "id": "2000" },
    { "en": "Amandemen UUD 1945 ketiga?", "id": "2001" },
    { "en": "Amandemen UUD 1945 keempat?", "id": "2002" },
    { "en": "Mahkamah Konstitusi dibentuk?", "id": "2003" },
    { "en": "Komisi Pemberantasan Korupsi (KPK) dibentuk?", "id": "2002" },
    { "en": "Bom JW Marriott Jakarta?", "id": "2003" },
    { "en": "Bom Kedutaan Besar Australia?", "id": "2004" },
    { "en": "Bom Bali II terjadi?", "id": "2005" },
    { "en": "Gempa bumi Yogyakarta?", "id": "2006" },
    { "en": "Indonesia menjadi tuan rumah KTT APEC?", "id": "2013" },
    { "en": "Kabinet Kerja dibentuk Jokowi?", "id": "2014" },
    { "en": "Ibu kota negara direncanakan pindah ke Kalimantan Timur?", "id": "2019" },
    { "en": "Prasasti Tugu dibuat?", "id": "450" },
    { "en": "Prasasti Kebon Kopi I dibuat?", "id": "500" },
    { "en": "Prasasti Ligor dibuat?", "id": "775" },
    { "en": "Perang saudara Majapahit dimulai?", "id": "1404" },
    { "en": "Syekh Yusuf Al-Makassari berjuang?", "id": "1683" },
    { "en": "Perang Jawa berakhir?", "id": "1830" },
    { "en": "Eduard Douwes Dekker lahir?", "id": "1820" },
    { "en": "Van Deventer menulis 'Een Eereschuld'?", "id": "1899" },
    { "en": "STOVIA (School tot Opleiding van Inlandsche Artsen) didirikan?", "id": "1898" },
    { "en": "Jong Java didirikan?", "id": "1915" },
    { "en": "Jong Sumatranen Bond didirikan?", "id": "1917" },
    { "en": "Jong Ambon didirikan?", "id": "1917" },
    { "en": "Jong Minahasa didirikan?", "id": "1918" },
    { "en": "Jong Celebes didirikan?", "id": "1919" },
    { "en": "Sumpah Pemuda dibacakan?", "id": "1928" },
    { "en": "Lahirnya Pancasila?", "id": "1945" },
    { "en": "Sidang PPKI pertama?", "id": "1945" },
    { "en": "Sidang PPKI kedua?", "id": "1945" },
    { "en": "Sidang PPKI ketiga?", "id": "1945" },
    { "en": "Maklumat Pemerintah 3 November?", "id": "1945" },
    { "en": "Kabinet Sjahrir I dibentuk?", "id": "1945" },
    { "en": "Tentara Republik Indonesia (TRI) dibentuk?", "id": "1946" },
    { "en": "Tentara Nasional Indonesia (TNI) dibentuk?", "id": "1947" },
    { "en": "Perundingan Hooge Veluwe?", "id": "1946" },
    { "en": "Perundingan Kaliurang?", "id": "1948" },
    { "en": "Negara Indonesia Timur dibentuk?", "id": "1946" },
    { "en": "Konferensi Inter-Indonesia diadakan?", "id": "1949" },
    { "en": "Kabinet Natsir dibentuk?", "id": "1950" },
    { "en": "Kabinet Sukiman dibentuk?", "id": "1951" },
    { "en": "Kabinet Wilopo dibentuk?", "id": "1952" },
    { "en": "Kabinet Ali Sastroamidjojo I dibentuk?", "id": "1953" },
    { "en": "Kabinet Burhanuddin Harahap dibentuk?", "id": "1955" },
    { "en": "Kabinet Ali Sastroamidjojo II dibentuk?", "id": "1956" },
    { "en": "Kabinet Djuanda dibentuk?", "id": "1957" },
    { "en": "Pembentukan Dewan Nasional?", "id": "1957" },
    { "en": "Pembubaran Konstituante?", "id": "1959" },
    { "en": "Penetapan Manifesto Politik (Manipol)?", "id": "1959" },
    { "en": "Pidato \"Penemuan Kembali Revolusi Kita\"?", "id": "1959" },
    { "en": "Pembentukan MPRS?", "id": "1960" },
    { "en": "Pembentukan DPAS?", "id": "1959" },
    { "en": "Soekarno diangkat sebagai Presiden seumur hidup?", "id": "1963" },
    { "en": "Proyek Mercusuar dicanangkan?", "id": "1961" },
    { "en": "Pembangunan Gelora Bung Karno selesai?", "id": "1962" },
    { "en": "Hotel Indonesia diresmikan?", "id": "1962" },
    { "en": "Tugu Selamat Datang diresmikan?", "id": "1962" },
    { "en": "Dwikora (Dwi Komando Rakyat) diumumkan?", "id": "1964" },
    { "en": "Mahkamah Militer Luar Biasa (Mahmillub) dibentuk?", "id": "1966" },
    { "en": "TAP MPRS XXV/1966 tentang pembubaran PKI?", "id": "1966" },
    { "en": "UU Penanaman Modal Asing (PMA) disahkan?", "id": "1967" },
    { "en": "UU Penanaman Modal Dalam Negeri (PMDN) disahkan?", "id": "1968" },
    { "en": "Repelita (Rencana Pembangunan Lima Tahun) I dimulai?", "id": "1969" },
    { "en": "Normalisasi hubungan dengan Malaysia?", "id": "1966" },
    { "en": "Fusi partai politik?", "id": "1973" },
    { "en": "Pedoman Penghayatan dan Pengamalan Pancasila (P4)?", "id": "1978" },
    { "en": "Azas tunggal Pancasila diberlakukan?", "id": "1985" },
    { "en": "Peristiwa Santa Cruz, Dili?", "id": "1991" },
    { "en": "Indonesia menjadi ketua Gerakan Non-Blok?", "id": "1992" },
    { "en": "Sidang Umum MPR melantik kembali Soeharto?", "id": "1998" },
    { "en": "Surat Soeharto kepada IMF ditandatangani?", "id": "1998" },
    { "en": "UU Kebebasan Menyampaikan Pendapat di Muka Umum?", "id": "1998" },
    { "en": "UU Pers disahkan?", "id": "1999" },
    { "en": "Sidang Istimewa MPR diselenggarakan?", "id": "1998" },
    { "en": "Presiden Gus Dur membubarkan Departemen Penerangan?", "id": "1999" },
    { "en": "Presiden Gus Dur membubarkan Departemen Sosial?", "id": "1999" },
    { "en": "Dekrit Presiden Gus Dur?", "id": "2001" },
    { "en": "Gus Dur dimakzulkan MPR?", "id": "2001" },
    { "en": "UU Terorisme disahkan?", "id": "2003" },
    { "en": "UU Partai Politik disahkan?", "id": "2008" },
    { "en": "Badan Penyelenggara Jaminan Sosial (BPJS) beroperasi?", "id": "2014" },
    { "en": "Pandemi COVID-19 masuk Indonesia?", "id": "2020" },
    { "en": "UU Cipta Kerja disahkan?", "id": "2020" },
    { "en": "Prasasti Pasir Awi ditemukan?", "id": "1864" },
    { "en": "Prasasti Muara Cianten ditemukan?", "id": "1864" },
    { "en": "Kerajaan Kalingga berdiri?", "id": "600" },
    { "en": "Kerajaan Sunda berdiri?", "id": "669" },
    { "en": "Perjanjian Sunda-Portugis ditandatangani?", "id": "1522" },
    { "en": "Kerajaan Pajajaran runtuh?", "id": "1579" },
    { "en": "Kesultanan Aceh didirikan?", "id": "1496" },
    { "en": "Sultan Iskandar Muda memerintah Aceh?", "id": "1607" },
    { "en": "Sultan Iskandar Thani memerintah Aceh?", "id": "1636" },
    { "en": "Perang Jagaraga di Bali?", "id": "1848" },
    { "en": "Go Tik Swan (Panembahan Hardjonagoro) menciptakan Batik Indonesia?", "id": "1950" },
    { "en": "Pemberontakan Kahar Muzakkar di Sulawesi Selatan?", "id": "1950" },
    { "en": "Pemberontakan Ibnu Hadjar di Kalimantan Selatan?", "id": "1950" },
    { "en": "Pemberontakan DI/TII Daud Beureu'eh di Aceh?", "id": "1953" },
    { "en": "Pemberontakan DI/TII Kartosuwiryo berakhir?", "id": "1962" },
    { "en": "Pembentukan Komando Cadangan Strategis Angkatan Darat (Kostrad)?", "id": "1961" },
    { "en": "Pembentukan Komando Pasukan Khusus (Kopassus)?", "id": "1952" },
    { "en": "Monumen Pancasila Sakti dibangun?", "id": "1967" },
    { "en": "Taman Mini Indonesia Indah (TMII) diresmikan?", "id": "1975" },
    { "en": "Sastrawan Pramoedya Ananta Toer dibebaskan dari Pulau Buru?", "id": "1979" },
    { "en": "Insiden Priok berdarah?", "id": "1984" },
    { "en": "Nurcholish Madjid (Cak Nur) menggagas Islam Inklusif?", "id": "1970" },
    { "en": "Bank Indonesia didirikan?", "id": "1953" },
    { "en": "Bursa Efek Jakarta (sekarang BEI) didirikan?", "id": "1912" },
    { "en": "Pindad didirikan?", "id": "1808" },
    { "en": "PT PAL Indonesia didirikan?", "id": "1980" },
    { "en": "PT Dirgantara Indonesia (dulu IPTN) didirikan?", "id": "1976" },
    { "en": "Pesawat N-250 Gatotkaca terbang perdana?", "id": "1995" },
    { "en": "Krisis finansial Asia dimulai?", "id": "1997" },
    { "en": "IMF memberikan bantuan kepada Indonesia?", "id": "1997" },
    { "en": "Gerakan mahasiswa menuntut reformasi?", "id": "1998" },
    { "en": "Jenderal Wiranto menjadi Panglima ABRI?", "id": "1998" },
    { "en": "Kabinet Reformasi Pembangunan dibentuk?", "id": "1998" },
    { "en": "TAP MPR tentang pencabutan mandat Presiden Habibie?", "id": "1999" },
    { "en": "Konflik komunal di Ambon meletus?", "id": "1999" },
    { "en": "Konflik Sampit terjadi?", "id": "2001" },
    { "en": "Otonomi Khusus Papua disahkan?", "id": "2001" },
    { "en": "Pemilu Legislatif serentak pertama?", "id": "2019" },
    { "en": "Pembangunan MRT Jakarta dimulai?", "id": "2013" },
    { "en": "MRT Jakarta beroperasi?", "id": "2019" },
    { "en": "Asian Games XVIII diadakan di Jakarta-Palembang?", "id": "2018" },
    { "en": "Situs Manusia Purba Sangiran diakui UNESCO?", "id": "1996" },
    { "en": "Subak di Bali diakui UNESCO?", "id": "2012" },
    { "en": "Candi Borobudur diakui UNESCO?", "id": "1991" },
    { "en": "Candi Prambanan diakui UNESCO?", "id": "1991" },
    { "en": "Keris Indonesia diakui UNESCO?", "id": "2005" },
    { "en": "Batik Indonesia diakui UNESCO?", "id": "2009" },
    { "en": "Angklung diakui UNESCO?", "id": "2010" },
    { "en": "Tari Saman diakui UNESCO?", "id": "2011" },
    { "en": "Noken Papua diakui UNESCO?", "id": "2012" },
    { "en": "Tiga Genre Tari Tradisional Bali diakui UNESCO?", "id": "2015" },
    { "en": "Pencak Silat diakui UNESCO?", "id": "2019" },
    { "en": "Pantun diakui UNESCO?", "id": "2020" },
    { "en": "Gamelan diakui UNESCO?", "id": "2021" },
    { "en": "Prasasti Kalasan ditulis?", "id": "778" },
    { "en": "Prasasti Mantyasih (Prasasti Balitung) dibuat?", "id": "907" },
    { "en": "Prasasti Anjuk Ladang dibuat?", "id": "937" },
    { "en": "Pararaton ditulis?", "id": "1600" },
    { "en": "Serat Centhini ditulis?", "id": "1814" },
    { "en": "Babad Diponegoro ditulis?", "id": "1831" },
    { "en": "Babad Tanah Jawi ditulis?", "id": "1647" },
    { "en": "Perlawanan Si Singamangaraja IX?", "id": "1877" },
    { "en": "Vereeniging voor Vrouwenkiesrecht (Perkumpulan untuk Hak Pilih Wanita) berdiri?", "id": "1917" },
    { "en": "Kongres Perempuan Indonesia I?", "id": "1928" },
    { "en": "Persatuan Bangsa Indonesia (PBI) didirikan dr. Soetomo?", "id": "1930" },
    { "en": "Fraksi Nasional di Volksraad dibentuk?", "id": "1930" },
    { "en": "Majelis Islam A'la Indonesia (MIAI) dibentuk?", "id": "1937" },
    { "en": "Jawa Hokokai dibentuk?", "id": "1944" },
    { "en": "Masyumi dibentuk (era Jepang)?", "id": "1943" },
    { "en": "Naskah proklamasi dirumuskan?", "id": "1945" },
    { "en": "Penyebaran berita proklamasi?", "id": "1945" },
    { "en": "Pembentukan 12 kementerian pertama?", "id": "1945" },
    { "en": "Pembentukan 8 provinsi pertama?", "id": "1945" },
    { "en": "Pertempuran Medan Area?", "id": "1945" },
    { "en": "Peristiwa Merah Putih di Manado?", "id": "1946" },
    { "en": "Operasi Produk (Agresi Militer Belanda I)?", "id": "1947" },
    { "en": "Operasi Kraai (Agresi Militer Belanda II)?", "id": "1948" },
    { "en": "Jenderal Soedirman memimpin perang gerilya?", "id": "1948" },
    { "en": "Jenderal Soedirman wafat?", "id": "1950" },
    { "en": "Mohammad Hatta mengundurkan diri sebagai Wapres?", "id": "1956" },
    { "en": "Konsepsi Presiden Soekarno tentang Demokrasi Terpimpin?", "id": "1957" },
    { "en": "Penpres No. 2/1959 tentang Pembentukan MPRS?", "id": "1959" },
    { "en": "Penpres No. 3/1959 tentang Pembubaran Konstituante?", "id": "1959" },
    { "en": "Pidato \"To Build the World Anew\" di PBB?", "id": "1960" },
    { "en": "TVRI mengudara pertama kali?", "id": "1962" },
    { "en": "Maphilindo (Malaysia, Filipina, Indonesia) dibentuk?", "id": "1963" },
    { "en": "Peristiwa Lubang Buaya?", "id": "1965" },
    { "en": "Sidang Istimewa MPRS mencabut kekuasaan Soekarno?", "id": "1967" },
    { "en": "Bung Hatta wafat?", "id": "1980" },
    { "en": "Sutan Sjahrir wafat?", "id": "1966" },
    { "en": "Tan Malaka wafat?", "id": "1949" },
    { "en": "Pemilu 1982?", "id": "1982" },
    { "en": "Pemilu 1987?", "id": "1987" },
    { "en": "Pemilu 1992?", "id": "1992" },
    { "en": "Pemilu 1997?", "id": "1997" },
    { "en": "TAP MPR tentang pembatasan masa jabatan presiden?", "id": "1998" },
    { "en": "Pemisahan TNI dan Polri?", "id": "1999" },
    { "en": "Bank Indonesia menjadi lembaga independen?", "id": "1999" },
    { "en": "Perdamaian Malino untuk Poso?", "id": "2001" },
    { "en": "Perdamaian Malino untuk Ambon?", "id": "2002" },
    { "en": "UU Sistem Pendidikan Nasional disahkan?", "id": "2003" },
    { "en": "SBY dilantik untuk periode kedua?", "id": "2009" },
    { "en": "Joko Widodo dilantik untuk periode kedua?", "id": "2019" },
    { "en": "Letusan Gunung Merapi dahsyat?", "id": "2010" },
    { "en": "Gempa dan tsunami Palu?", "id": "2018" },
    { "en": "Prasasti Gandasuli ditemukan?", "id": "832" },
    { "en": "Kerajaan Bali Kuno berdiri?", "id": "914" },
    { "en": "Penyerangan Dharmasraya oleh Majapahit?", "id": "1347" },
    { "en": "Pemberontakan Kuti?", "id": "1319" },
    { "en": "Pemberontakan Sadeng?", "id": "1331" },
    { "en": "Sultan Ageng Tirtayasa memerintah Banten?", "id": "1651" },
    { "en": "Perang Saudara Banten?", "id": "1680" },
    { "en": "Perang Makassar dimulai?", "id": "1660" },
    { "en": "Cornelis Speelman memimpin VOC?", "id": "1681" },
    { "en": "Tanam Paksa Kopi di Priangan?", "id": "1720" },
    { "en": "Gubernur Jenderal van der Capellen memerintah?", "id": "1816" },
    { "en": "Gubernur Jenderal van den Bosch menerapkan Cultuurstelsel?", "id": "1830" },
    { "en": "UU Agraria (Agrarische Wet) disahkan?", "id": "1870" },
    { "en": "UU Gula (Suikerwet) disahkan?", "id": "1870" },
    { "en": "Krakatau meletus dahsyat?", "id": "1883" },
    { "en": "KNIL (Koninklijk Nederlandsch-Indisch Leger) dibentuk?", "id": "1830" },
    { "en": "Tirto Adhi Soerjo mendirikan Medan Prijaji?", "id": "1907" },
    { "en": "Organisasi PNI dibubarkan?", "id": "1931" },
    { "en": "Ki Hadjar Dewantara wafat?", "id": "1959" },
    { "en": "Soekarno membacakan Pancasila?", "id": "1945" },
    { "en": "Kabinet Presidensial pertama dibentuk?", "id": "1945" },
    { "en": "Insiden Bendera di Tunjungan?", "id": "1945" },
    { "en": "Perundingan Linggarjati dimulai?", "id": "1946" },
    { "en": "Penandatanganan Perjanjian Renville?", "id": "1948" },
    { "en": "Sjafruddin Prawiranegara memimpin PDRI?", "id": "1948" },
    { "en": "Penyerahan kedaulatan di Amsterdam?", "id": "1949" },
    { "en": "Penyerahan kedaulatan di Jakarta?", "id": "1949" },
    { "en": "Pemberontakan APRA (Angkatan Perang Ratu Adil)?", "id": "1950" },
    { "en": "Westerling memimpin APRA?", "id": "1950" },
    { "en": "Mosi Integral Natsir?", "id": "1950" },
    { "en": "Peristiwa 17 Oktober?", "id": "1952" },
    { "en": "Pemilu untuk Konstituante?", "id": "1955" },
    { "en": "UU Darurat No. 7 Tahun 1956?", "id": "1956" },
    { "en": "Pembangunan Masjid Istiqlal dimulai?", "id": "1961" },
    { "en": "Masjid Istiqlal diresmikan?", "id": "1978" },
    { "en": "Gerakan Usaha Pembaharuan Pendidikan Islam (GUPPI) berdiri?", "id": "1950" },
    { "en": "Tragedi Gerbong Maut?", "id": "1947" },
    { "en": "Peristiwa Situjuah?", "id": "1949" },
    { "en": "TAP MPRS XXXIII/1967?", "id": "1967" },
    { "en": "Pelita II dimulai?", "id": "1974" },
    { "en": "Pelita III dimulai?", "id": "1979" },
    { "en": "Pelita IV dimulai?", "id": "1984" },
    { "en": "Pelita V dimulai?", "id": "1989" },
    { "en": "Pelita VI dimulai?", "id": "1994" },
    { "en": "Program transmigrasi digalakkan?", "id": "1950" },
    { "en": "Wafatnya Ibu Tien Soeharto?", "id": "1996" },
    { "en": "Penembakan mahasiswa Semanggi I?", "id": "1998" },
    { "en": "Penembakan mahasiswa Semanggi II?", "id": "1999" },
    { "en": "UU Otonomi Daerah No. 22/1999?", "id": "1999" },
    { "en": "UU Perimbangan Keuangan Pusat dan Daerah?", "id": "1999" },
    { "en": "Gus Dur berkunjung ke Israel?", "id": "1994" },
    { "en": "Pelepasan Timor Timur dari Indonesia?", "id": "1999" },
    { "en": "Kabinet Persatuan Nasional dibentuk Gus Dur?", "id": "1999" },
    { "en": "Kabinet Gotong Royong dibentuk Megawati?", "id": "2001" },
    { "en": "UU KPK disahkan?", "id": "2002" },
    { "en": "UU Pemilu Presiden dan Wapres disahkan?", "id": "2003" },
    { "en": "Kabinet Indonesia Bersatu I dibentuk SBY?", "id": "2004" },
    { "en": "Kabinet Indonesia Bersatu II dibentuk SBY?", "id": "2009" },
    { "en": "Kasus korupsi Bank Century?", "id": "2008" },
    { "en": "Pembangunan Jembatan Suramadu selesai?", "id": "2009" },
    { "en": "Pencanangan Visi Indonesia 2045?", "id": "2019" },
    { "en": "Kerajaan Kutai Martadipura?", "id": "350" },
    { "en": "Prasasti Jambu (Tarumanegara)?", "id": "450" },
    { "en": "Prasasti Pasir Koleangkak?", "id": "450" },
    { "en": "Prasasti Cidanghiang?", "id": "450" },
    { "en": "Prasasti Palas Pasemah?", "id": "680" },
    { "en": "I-Tsing mengunjungi Sriwijaya?", "id": "671" },
    { "en": "Rakai Pikatan memerintah?", "id": "840" },
    { "en": "Pramodhawardhani memerintah?", "id": "833" },
    { "en": "Kakawin Ramayana ditulis?", "id": "870" },
    { "en": "Kakawin Arjunawiwaha oleh Mpu Kanwa?", "id": "1030" },
    { "en": "Kakawin Bharatayuddha oleh Mpu Sedah dan Mpu Panuluh?", "id": "1157" },
    { "en": "Tohjaya memerintah Singhasari?", "id": "1248" },
    { "en": "Pemberontakan Ranggalawe?", "id": "1295" },
    { "en": "Pemberontakan Sora?", "id": "1300" },
    { "en": "Pemberontakan Nambi?", "id": "1316" },
    { "en": "Pemberontakan Tanca?", "id": "1328" },
    { "en": "Tribhuwana Wijayatunggadewi memerintah Majapahit?", "id": "1328" },
    { "en": "Invasi Majapahit ke Bali?", "id": "1343" },
    { "en": "Perjalanan Adityawarman ke Sumatera?", "id": "1347" },
    { "en": "Wafatnya Wikramawardhana?", "id": "1429" },
    { "en": "Girindrawardhana memerintah Majapahit?", "id": "1478" },
    { "en": "Sultan Malik as-Salih wafat?", "id": "1297" },
    { "en": "Ibnu Batutah mengunjungi Samudera Pasai?", "id": "1345" },
    { "en": "Sunan Ampel menyebarkan Islam?", "id": "1443" },
    { "en": "Sunan Giri mendirikan Giri Kedaton?", "id": "1487" },
    { "en": "Sunan Kalijaga berdakwah?", "id": "1450" },
    { "en": "Demak menyerang Majapahit?", "id": "1527" },
    { "en": "Sultan Maulana Hasanuddin memerintah Banten?", "id": "1552" },
    { "en": "Sutawijaya mengalahkan Arya Penangsang?", "id": "1549" },
    { "en": "VOC mendirikan loji di Jepara?", "id": "1651" },
    { "en": "Amangkurat II naik takhta?", "id": "1677" },
    { "en": "Pakubuwana I memerintah?", "id": "1704" },
    { "en": "Perang Takhta Jawa Pertama?", "id": "1703" },
    { "en": "Perang Takhta Jawa Kedua?", "id": "1719" },
    { "en": "Perang Takhta Jawa Ketiga?", "id": "1749" },
    { "en": "Mangkubumi (Sultan Hamengkubuwono I) melawan VOC?", "id": "1749" },
    { "en": "Pangeran Sambernyawa (Mangkunegara I) berjuang?", "id": "1741" },
    { "en": "Kontrak Dagang VOC dengan Mataram?", "id": "1677" },
    { "en": "Kapitulasi Tuntang?", "id": "1811" },
    { "en": "Sistem Sewa Tanah Raffles?", "id": "1811" },
    { "en": "Kebun Raya Bogor didirikan?", "id": "1817" },
    { "en": "Perang Bone melawan Belanda?", "id": "1824" },
    { "en": "Perang Jambi melawan Belanda?", "id": "1858" },
    { "en": "Sentot Alibasyah bergabung dengan Diponegoro?", "id": "1828" },
    { "en": "Cultuurprocenten dihapuskan?", "id": "1864" },
    { "en": "Pembukaan Terusan Suez?", "id": "1869" },
    { "en": "Koelie Ordonnantie (Ordonansi Kuli) dikeluarkan?", "id": "1880" },
    { "en": "Penerapan Plakat Panjang di Aceh?", "id": "1873" },
    { "en": "Teuku Umar gugur?", "id": "1899" },
    { "en": "Gerakan Ratu Adil muncul?", "id": "1888" },
    { "en": "Raden Ajeng Kartini lahir?", "id": "1879" },
    { "en": "Sekolah Kartini pertama didirikan?", "id": "1913" },
    { "en": "Perkumpulan Sutardjo didirikan?", "id": "1920" },
    { "en": "STUDIEFONDS (Dana Pelajar) didirikan?", "id": "1913" },
    { "en": "Pemogokan buruh kereta api dan trem?", "id": "1923" },
    { "en": "Indonesia Raya diciptakan?", "id": "1924" },
    { "en": "W.R. Supratman wafat?", "id": "1938" },
    { "en": "Tuntutan \"Indonesia Berparlemen\" oleh GAPI?", "id": "1939" },
    { "en": "Jepang menyerang Pearl Harbor?", "id": "1941" },
    { "en": "Gerakan 3A (Nippon Pemimpin Asia, Pelindung Asia, Cahaya Asia)?", "id": "1942" },
    { "en": "Barisan Pelopor dibentuk?", "id": "1944" },
    { "en": "Penyusunan teks Proklamasi di rumah Laksamana Maeda?", "id": "1945" },
    { "en": "Pengibaran bendera Merah Putih pertama?", "id": "1945" },
    { "en": "Maklumat Wakil Presiden No. X?", "id": "1945" },
    { "en": "Pertempuran Puputan Margarana?", "id": "1946" },
    { "en": "Resolusi Dewan Keamanan PBB tentang Indonesia?", "id": "1947" },
    { "en": "Pemberontakan Batalyon 426 di Kudus?", "id": "1951" },
    { "en": "Olimpiade pertama Indonesia di Helsinki?", "id": "1952" },
    { "en": "Pidato \"Tahun Vivere Pericoloso\"?", "id": "1964" },
    { "en": "TAP MPRS XI/1966?", "id": "1966" },
    { "en": "Perjanjian Bangkok (Normalisasi Hubungan Indonesia-Malaysia)?", "id": "1966" },
    { "en": "Bulog (Badan Urusan Logistik) didirikan?", "id": "1967" },
    { "en": "RRI (Radio Republik Indonesia) didirikan?", "id": "1945" },
    { "en": "Kunjungan Paus Paulus VI ke Indonesia?", "id": "1970" },
    { "en": "Penerbangan haji pertama oleh Garuda Indonesia?", "id": "1955" },
    { "en": "Operasi Seroja di Timor Timur?", "id": "1975" },
    { "en": "Gerakan Jumat Bersih dicanangkan?", "id": "1995" },
    { "en": "Reformasi moneter (redenominasi) direncanakan?", "id": "2013" },
    { "en": "Erupsi Gunung Agung, Bali?", "id": "2017" },
    { "en": "Tsunami Selat Sunda?", "id": "2018" },
    { "en": "UU Ibu Kota Negara disahkan?", "id": "2022" },
    { "en": "Prasasti Tukmas ditemukan?", "id": "500" },
    { "en": "Dapunta Hyang Sri Jayanasa melakukan perjalanan suci?", "id": "683" },
    { "en": "Kerajaan Kanjuruhan berdiri?", "id": "760" },
    { "en": "Pembangunan Candi Plaosan?", "id": "825" },
    { "en": "Serangan Rajendra Chola ke Sriwijaya?", "id": "1025" },
    { "en": "Kertajaya memerintah Kediri?", "id": "1194" },
    { "en": "Perang Ganter antara Ken Arok dan Kertajaya?", "id": "1222" },
    { "en": "Anusapati memerintah Singhasari?", "id": "1227" },
    { "en": "Ranggawuni (Wisnuwardhana) memerintah Singhasari?", "id": "1248" },
    { "en": "Jayanegara memerintah Majapahit?", "id": "1309" },
    { "en": "Pemberontakan Semi?", "id": "1318" },
    { "en": "Perang Paregreg berakhir?", "id": "1406" },
    { "en": "Kesultanan Tidore berdiri?", "id": "1450" },
    { "en": "Armada Spanyol tiba di Maluku?", "id": "1521" },
    { "en": "Pangeran Dipati Ukur memberontak?", "id": "1628" },
    { "en": "Perang Batak dimulai?", "id": "1878" },
    { "en": "Sarekat Islam Merah (PKI) terbentuk?", "id": "1921" },
    { "en": "Radio Bataviaasche Radio Vereeniging (BRV) berdiri?", "id": "1925" },
    { "en": "Soekarno bebas dari Sukamiskin?", "id": "1931" },
    { "en": "Gerindo (Gerakan Rakyat Indonesia) didirikan?", "id": "1937" },
    { "en": "Hizbullah (tentara sukarela) dibentuk?", "id": "1944" },
    { "en": "Laskar Rakyat dibentuk?", "id": "1945" },
    { "en": "S.K. Trimurti menjadi Menteri Tenaga Kerja?", "id": "1947" },
    { "en": "Perundingan di atas kapal USS Renville?", "id": "1947" },
    { "en": "Pembentukan BFO (Bijeenkomst voor Federaal Overleg)?", "id": "1948" },
    { "en": "UNCI (United Nations Commission for Indonesia) dibentuk?", "id": "1949" },
    { "en": "UUDS 1950 diberlakukan?", "id": "1950" },
    { "en": "Program Benteng dicanangkan?", "id": "1950" },
    { "en": "Sistem Ekonomi Ali-Baba?", "id": "1954" },
    { "en": "Pembatalan hasil KMB secara sepihak?", "id": "1956" },
    { "en": "Gerakan Anti-Imperialisme dan Kolonialisme (GANEFO)?", "id": "1963" },
    { "en": "Konferensi Tingkat Tinggi (KTT) Non-Blok I di Beograd?", "id": "1961" },
    { "en": "Komando Mandala Pembebasan Irian Barat?", "id": "1962" },
    { "en": "UNTEA (United Nations Temporary Executive Authority) di Irian Barat?", "id": "1962" },
    { "en": "Pidato Nawaksara oleh Presiden Soekarno?", "id": "1966" },
    { "en": "Penandatanganan Deklarasi ASEAN (Deklarasi Bangkok)?", "id": "1967" },
    { "en": "KTT ASEAN pertama di Bali?", "id": "1976" },
    { "en": "Petisi 50 diterbitkan?", "id": "1980" },
    { "en": "Kunjungan Presiden AS Ronald Reagan ke Bali?", "id": "1986" },
    { "en": "Aksi mahasiswa menduduki Gedung DPR/MPR?", "id": "1998" },
    { "en": "UU Partai Politik No. 2/1999?", "id": "1999" },
    { "en": "UU Pemilu No. 3/1999?", "id": "1999" },
    { "en": "UU Susunan dan Kedudukan MPR, DPR, dan DPRD?", "id": "1999" },
    { "en": "Pemberlakuan Darurat Militer di Aceh?", "id": "2003" },
    { "en": "Konferensi Tingkat Tinggi Dunia tentang Pembangunan Berkelanjutan di Johannesburg?", "id": "2002" },
    { "en": "Banjir bandang di Bahorok, Sumatera Utara?", "id": "2003" },
    { "en": "Kasus lumpur Lapindo Sidoarjo dimulai?", "id": "2006" },
    { "en": "Indonesia menjadi tuan rumah Konferensi Perubahan Iklim PBB di Bali?", "id": "2007" },
    { "en": "UU Informasi dan Transaksi Elektronik (ITE) disahkan?", "id": "2008" },
    { "en": "Program Kartu Indonesia Sehat (KIS) diluncurkan?", "id": "2014" },
    { "en": "Program Kartu Indonesia Pintar (KIP) diluncurkan?", "id": "2014" },
    { "en": "Aksi 212 di Jakarta?", "id": "2016" },
    { "en": "Kunjungan Raja Salman dari Arab Saudi?", "id": "2017" },
    { "en": "Kerusuhan pasca-pemilu di Jakarta?", "id": "2019" },
    { "en": "Vaksinasi COVID-19 pertama di Indonesia?", "id": "2021" },
    { "en": "Prasasti Sojomerto ditulis?", "id": "650" },
    { "en": "Prasasti Dinoyo ditulis?", "id": "760" },
    { "en": "Pembangunan Candi Sari?", "id": "778" },
    { "en": "Prasasti Kelurak ditulis?", "id": "782" },
    { "en": "Dyah Balitung memerintah Mataram Kuno?", "id": "899" },
    { "en": "Dharmawangsa Teguh memerintah Medang?", "id": "991" },
    { "en": "Peristiwa Pralaya Medang?", "id": "1016" },
    { "en": "Kakawin Sutasoma Mpu Tantular?", "id": "1365" },
    { "en": "Perang Regreg berakhir?", "id": "1406" },
    { "en": "Penyebaran Islam oleh Syekh Maulana Malik Ibrahim?", "id": "1390" },
    { "en": "Raden Patah wafat?", "id": "1518" },
    { "en": "Fatahillah mengganti nama Sunda Kelapa menjadi Jayakarta?", "id": "1527" },
    { "en": "Sultan Baabullah memerintah Ternate?", "id": "1570" },
    { "en": "Pieter Both menjadi Gubernur Jenderal VOC pertama?", "id": "1610" },
    { "en": "Pembangunan Benteng Batavia?", "id": "1619" },
    { "en": "Amangkurat I memindahkan keraton ke Plered?", "id": "1647" },
    { "en": "Sultan Haji merebut takhta Banten?", "id": "1683" },
    { "en": "Perlawanan Pangeran Mangkubumi dimulai?", "id": "1749" },
    { "en": "Raffles menghapuskan Kesultanan Banten?", "id": "1813" },
    { "en": "Pattimura dihukum mati?", "id": "1817" },
    { "en": "Benteng Fort de Kock dibangun?", "id": "1825" },
    { "en": "Perang Bali I?", "id": "1846" },
    { "en": "Perang Bali II?", "id": "1848" },
    { "en": "Perang Bali III?", "id": "1849" },
    { "en": "Sistem Tanam Paksa dihapuskan secara resmi?", "id": "1870" },
    { "en": "K.H. Ahmad Dahlan lahir?", "id": "1868" },
    { "en": "K.H. Hasyim Asy'ari lahir?", "id": "1871" },
    { "en": "Pemogokan Sarekat Islam di Semarang?", "id": "1918" },
    { "en": "Technische Hoogeschool te Bandoeng (sekarang ITB) didirikan?", "id": "1920" },
    { "en": "Manifesto Politik Perhimpoenan Indonesia?", "id": "1925" },
    { "en": "Soekarno lahir?", "id": "1901" },
    { "en": "Mohammad Hatta lahir?", "id": "1902" },
    { "en": "Sutan Sjahrir lahir?", "id": "1909" },
    { "en": "Jepang membentuk Chuo Sangi-in (Dewan Pertimbangan Pusat)?", "id": "1943" },
    { "en": "Perumusan Dasar Negara oleh BPUPKI?", "id": "1945" },
    { "en": "Pembacaan teks Proklamasi oleh Soekarno?", "id": "1945" },
    { "en": "Maklumat Pemerintah 14 November (perubahan sistem kabinet)?", "id": "1945" },
    { "en": "Pertempuran Lima Hari Lima Malam di Palembang?", "id": "1947" },
    { "en": "Yogyakarta menjadi ibu kota?", "id": "1946" },
    { "en": "Yogyakarta diduduki Belanda?", "id": "1948" },
    { "en": "Serangan Oemoem di Surakarta?", "id": "1949" },
    { "en": "Pemerintah RI kembali ke Yogyakarta?", "id": "1949" },
    { "en": "Kabinet Parlementer pertama?", "id": "1945" },
    { "en": "Pemilu memilih anggota DPR?", "id": "1955" },
    { "en": "Konstituante memulai sidang?", "id": "1956" },
    { "en": "Berlakunya kembali UUD 1945?", "id": "1959" },
    { "en": "Pemutusan hubungan diplomatik dengan Belanda?", "id": "1960" },
    { "en": "Misi Garuda I dikirim ke Mesir?", "id": "1957" },
    { "en": "Indonesia menjadi Juara Umum Asian Games IV?", "id": "1962" },
    { "en": "Gerakan mahasiswa KAMI dan KAPPI?", "id": "1966" },
    { "en": "Soekarno wafat?", "id": "1970" },
    { "en": "UU Perkawinan No. 1/1974?", "id": "1974" },
    { "en": "Deklarasi Integrasi Balibo?", "id": "1975" },
    { "en": "Rusuh 27 Juli (Kudatuli)?", "id": "1996" },
    { "en": "Habibie mengumumkan kebebasan pers?", "id": "1998" },
    { "en": "Opsi referendum untuk Timor Timur ditawarkan?", "id": "1999" },
    { "en": "INTERFET (pasukan perdamaian) masuk Timor Timur?", "id": "1999" },
    { "en": "Pilkada langsung pertama?", "id": "2005" },
    { "en": "Visit Indonesia Year dicanangkan?", "id": "2008" },
    { "en": "Banjir besar Jakarta?", "id": "2013" },
    { "en": "Prasasti Wurare dibuat?", "id": "1289" },
    { "en": "Prasasti Singhasari (Gajah Mada) dibuat?", "id": "1351" },
    { "en": "Ma Huan mengunjungi Majapahit?", "id": "1413" },
    { "en": "Serangan Demak ke Sunda Kelapa?", "id": "1527" },
    { "en": "Penyerangan Sultan Agung ke Surabaya?", "id": "1625" },
    { "en": "Perlawanan Pangeran Raden Mas Said (Mangkunegara I)?", "id": "1741" },
    { "en": "Perang Diponegoro berakhir?", "id": "1830" },
    { "en": "Perlawanan Tuanku Tambusai?", "id": "1834" },
    { "en": "Pemberontakan petani Banten oleh K.H. Wasyid?", "id": "1888" },
    { "en": "Indische Vereeniging didirikan?", "id": "1908" },
    { "en": "Kongres Al-Islam Hindia?", "id": "1922" },
    { "en": "Berdirinya Partai Fasis Indonesia?", "id": "1933" },
    { "en": "Pertempuran Laut Jawa?", "id": "1942" },
    { "en": "Pertempuran Selat Badung?", "id": "1942" },
    { "en": "Rapat Raksasa di Lapangan Ikada?", "id": "1945" },
    { "en": "Peristiwa Merah Putih di Biak?", "id": "1948" },
    { "en": "Pemberontakan Westerling di Bandung?", "id": "1950" },
    { "en": "Operasi 17 Agustus menumpas PRRI?", "id": "1958" },
    { "en": "Pembentukan Front Nasional?", "id": "1960" },
    { "en": "Manipol menjadi GBHN?", "id": "1960" },
    { "en": "Soeharto mengambil alih pimpinan Angkatan Darat?", "id": "1965" },
    { "en": "UU Pokok Pers No. 21/1982?", "id": "1982" },
    { "en": "IMF Letter of Intent ditandatangani?", "id": "1998" },
    { "en": "Pemilu multipartai pertama setelah Orde Baru?", "id": "1999" },
    { "en": "Kabinet Indonesia Maju dibentuk Jokowi?", "id": "2019" },
    { "en": "Airlangga menyatukan kembali kerajaan?", "id": "1037" },
    { "en": "Pembangunan Candi Jago?", "id": "1268" },
    { "en": "Pembangunan Candi Kidal?", "id": "1248" },
    { "en": "Pembangunan Candi Singasari?", "id": "1300" },
    { "en": "Ekspedisi Wringin Pitu?", "id": "1447" },
    { "en": "Serangan Demak ke Panarukan?", "id": "1546" },
    { "en": "Sultan Nuku dari Tidore melawan VOC?", "id": "1780" },
    { "en": "Kompeni (VOC) menguasai Banten?", "id": "1684" },
    { "en": "Raffles menulis 'History of the East Indian Archipelago'?", "id": "1820" },
    { "en": "Perang Menteng (Perang Batak)?", "id": "1878" },
    { "en": "Perserikatan Guru Hindia Belanda (PGHB) didirikan?", "id": "1912" },
    { "en": "Soekamiskin menjadi penjara politik?", "id": "1924" },
    { "en": "Gerakan Seinendan dibentuk Jepang?", "id": "1943" },
    { "en": "Gerakan Keibodan dibentuk Jepang?", "id": "1943" },
    { "en": "Peristiwa 10 November di Surabaya?", "id": "1945" },
    { "en": "Negara Pasundan didirikan?", "id": "1948" },
    { "en": "Negara Jawa Timur didirikan?", "id": "1948" },
    { "en": "Negara Madura didirikan?", "id": "1948" },
    { "en": "Negara Sumatera Timur didirikan?", "id": "1947" },
    { "en": "Negara Sumatera Selatan didirikan?", "id": "1948" },
    { "en": "Serangan Fajar di Yogyakarta?", "id": "1949" },
    { "en": "Kabinet Natsir memerintah?", "id": "1950" },
    { "en": "Rencana Pembangunan Lima Tahun (RPLT) pertama?", "id": "1956" },
    { "en": "Gagasan NASAKOM (Nasionalis, Agama, Komunis) dicetuskan?", "id": "1956" },
    { "en": "De-Soekarnoisasi dimulai?", "id": "1966" },
    { "en": "Pemilu 1977?", "id": "1977" },
    { "en": "Pembangunan Jalan Tol Jagorawi?", "id": "1978" },
    { "en": "Penembakan Misterius (Petrus) terjadi?", "id": "1983" },
    { "en": "Kerusuhan Situbondo?", "id": "1996" },
    { "en": "Kerusuhan Tasikmalaya?", "id": "1996" },
    { "en": "Kerusuhan Rengasdengklok (Jawa Barat)?", "id": "1997" },
    { "en": "TAP MPR tentang Penyelenggaraan Negara yang Bersih dari KKN?", "id": "1998" },
    { "en": "Pemilihan Presiden dan Wapres oleh MPR terakhir?", "id": "1999" },
    { "en": "Konflik Poso meletus?", "id": "1998" },
    { "en": "UU Penanggulangan Bencana disahkan?", "id": "2007" },
    { "en": "Indonesia menjadi tuan rumah SEA Games?", "id": "2011" },
    { "en": "Kerajaan Sriwijaya runtuh?", "id": "1377" },
    { "en": "Pembangunan Candi Penataran dimulai?", "id": "1197" },
    { "en": "Jayanegara wafat?", "id": "1328" },
    { "en": "Pemberontakan Ra Semi?", "id": "1318" },
    { "en": "Sunda Kelapa diserang Demak?", "id": "1527" },
    { "en": "Herman Willem Daendels memerintah?", "id": "1808" },
    { "en": "Perang Puputan Klungkung?", "id": "1908" },
    { "en": "Perhimpunan Pelajar-Pelajar Indonesia (PPPI) didirikan?", "id": "1926" },
    { "en": "Sidang Kedua BPUPKI?", "id": "1945" },
    { "en": "Peristiwa Lapangan Ikada?", "id": "1945" },
    { "en": "Kabinet Sjahrir II dibentuk?", "id": "1946" },
    { "en": "Kabinet Sjahrir III dibentuk?", "id": "1946" },
    { "en": "Kabinet Amir Sjarifuddin II dibentuk?", "id": "1947" },
    { "en": "Pembentukan Uni Indonesia-Belanda?", "id": "1949" },
    { "en": "Pemberontakan DI/TII Ibnu Hadjar?", "id": "1950" },
    { "en": "Rudi Hartono menjuarai All England pertama kali?", "id": "1968" },
    { "en": "Susi Susanti meraih emas Olimpiade?", "id": "1992" },
    { "en": "Alan Budikusuma meraih emas Olimpiade?", "id": "1992" },
    { "en": "KTT APEC di Bogor?", "id": "1994" },
    { "en": "Kerusuhan Ketapang, Jakarta?", "id": "1998" },
    { "en": "UU Anti KKN disahkan?", "id": "1999" },
    { "en": "Kerajaan Galuh berdiri?", "id": "612" },
    { "en": "Perang Bubat meletus?", "id": "1357" },
    { "en": "Sultan Trenggono wafat?", "id": "1546" },
    { "en": "Pajang menaklukkan Jipang?", "id": "1554" },
    { "en": "Senopati menjadi Raja Mataram?", "id": "1586" },
    { "en": "Penyerbuan Trunajaya ke Mataram?", "id": "1677" },
    { "en": "Perang Banjar berakhir?", "id": "1905" },
    { "en": "Perlawanan Patani di Maluku?", "id": "1914" },
    { "en": "Kongres Pemuda Indonesia III?", "id": "1938" },
    { "en": "Pembentukan Seinendan?", "id": "1943" },
    { "en": "Pertempuran Bojongkokosan?", "id": "1945" },
    { "en": "Peristiwa Tiga Daerah (Brebes, Tegal, Pemalang)?", "id": "1945" },
    { "en": "Konferensi Malino?", "id": "1946" },
    { "en": "Konferensi Denpasar?", "id": "1946" },
    { "en": "Kabinet RIS dibentuk?", "id": "1949" },
    { "en": "Operasi Merdeka menumpas Permesta?", "id": "1958" },
    { "en": "TAP MPRS tentang Pengangkatan Presiden Seumur Hidup?", "id": "1963" },
    { "en": "Mahasiswa menduduki gedung DPR/MPR?", "id": "1998" },
    { "en": "Tragedi Gejayan?", "id": "1998" },
    { "en": "Penculikan aktivis pro-demokrasi?", "id": "1997" },
    { "en": "Pembentukan Komisi Nasional Hak Asasi Manusia (Komnas HAM)?", "id": "1993" },
    { "en": "Banjir bandang Wasior, Papua Barat?", "id": "2010" },
    { "en": "Letusan Gunung Kelud?", "id": "2014" },
    { "en": "Situs Homo floresiensis ditemukan?", "id": "2003" },
    { "en": "Kerajaan Tulang Bawang?", "id": "600" },
    { "en": "Prasasti Hujung Langit?", "id": "997" },
    { "en": "Prasasti Butak (Kertarajasa)?", "id": "1294" },
    { "en": "Sultan Alaudin Riayat Syah al-Kahar memerintah Aceh?", "id": "1539" },
    { "en": "Sultan Nuku wafat?", "id": "1805" },
    { "en": "Benteng Vredeburg dibangun?", "id": "1760" },
    { "en": "Pendirian ELS (Europeesche Lagere School)?", "id": "1817" },
    { "en": "Pendirian HIS (Hollandsch-Inlandsche School)?", "id": "1914" },
    { "en": "Pendirian MULO (Meer Uitgebreid Lager Onderwijs)?", "id": "1914" },
    { "en": "Pendirian AMS (Algemeene Middelbare School)?", "id": "1919" },
    { "en": "Rechts Hogeschool (Sekolah Tinggi Hukum) didirikan?", "id": "1924" },
    { "en": "Geneeskundige Hogeschool (Sekolah Tinggi Kedokteran) didirikan?", "id": "1927" },
    { "en": "Fakultas Sastra didirikan?", "id": "1940" },
    { "en": "Fujinkai (perkumpulan wanita) dibentuk Jepang?", "id": "1943" },
    { "en": "KNIL dibubarkan?", "id": "1950" },
    { "en": "Gerakan Sanering (pemotongan nilai uang)?", "id": "1959" },
    { "en": "Bank Dagang Negara didirikan?", "id": "1960" },
    { "en": "Bank Bumi Daya didirikan?", "id": "1959" },
    { "en": "Bank Ekspor Impor Indonesia didirikan?", "id": "1960" },
    { "en": "Bank Pembangunan Indonesia (Bapindo) didirikan?", "id": "1960" },
    { "en": "Bank Mandiri terbentuk dari merger?", "id": "1998" },
    { "en": "UU Penyiaran disahkan?", "id": "2002" },
    { "en": "UU Kelistrikan disahkan?", "id": "2002" },
    { "en": "UU Sumber Daya Air disahkan?", "id": "2004" },
    { "en": "UU Guru dan Dosen disahkan?", "id": "2005" },
    { "en": "UU Kewarganegaraan disahkan?", "id": "2006" },
    { "en": "UU Pornografi disahkan?", "id": "2008" },
    { "en": "UU Keterbukaan Informasi Publik disahkan?", "id": "2008" },
    { "en": "UU Lalu Lintas dan Angkutan Jalan disahkan?", "id": "2009" },
    { "en": "UU Badan Hukum Pendidikan dibatalkan MK?", "id": "2010" },
    { "en": "UU Desa disahkan?", "id": "2014" },
    { "en": "Kebakaran hutan dan lahan hebat?", "id": "2015" },
    { "en": "Indonesia menjadi anggota tidak tetap Dewan Keamanan PBB?", "id": "2019" },
    { "en": "Prasasti Telaga Batu ditemukan?", "id": "680" },
    { "en": "Candi Ratu Boko dibangun?", "id": "800" },
    { "en": "Pemberontakan Gajah Biru?", "id": "1400" },
    { "en": "VOC menguasai Jayakarta?", "id": "1619" },
    { "en": "Gubernur Jenderal van Imhoff memerintah?", "id": "1743" },
    { "en": "Daendels membubarkan Kesultanan Cirebon?", "id": "1809" },
    { "en": "Liga Demokrasi dibentuk?", "id": "1960" },
    { "en": "Badan Pendukung Soekarnoisme dibentuk?", "id": "1964" },
    { "en": "Operasi Trisula menumpas sisa PKI di Blitar Selatan?", "id": "1968" },
    { "en": "Sistem Normalisasi Kehidupan Kampus/Badan Koordinasi Kemahasiswaan (NKK/BKK)?", "id": "1978" },
    { "en": "UU Perfilman disahkan?", "id": "1992" },
    { "en": "Pembentukan Komisi Pemilihan Umum (KPU)?", "id": "1999" },
    { "en": "Pembentukan Komisi Pengawas Persaingan Usaha (KPPU)?", "id": "1999" },
    { "en": "Pemberlakuan kembali Darurat Sipil di Maluku?", "id": "2000" },
    { "en": "Prasasti Rukam?", "id": "907" },
    { "en": "Prasasti Wanua Tengah III?", "id": "908" },
    { "en": "Prasasti Sangguran (Prasasti Minto)?", "id": "928" },
    { "en": "Prasasti Pucangan (Prasasti Calcutta)?", "id": "1041" },
    { "en": "Prasasti Hantang (Prasasti Ngantang)?", "id": "1135" },
    { "en": "Prasasti Jaring?", "id": "1181" },
    { "en": "Prasasti Kudadu?", "id": "1294" },
    { "en": "Prasasti Sukamerta?", "id": "1296" },
    { "en": "Kakawin Nagarakretagama ditemukan kembali?", "id": "1894" },
    { "en": "Pemberontakan Patih Nambi?", "id": "1316" },
    { "en": "Armada Portugis tiba di Sunda Kelapa?", "id": "1522" },
    { "en": "Perang antara Ternate dan Tidore?", "id": "1524" },
    { "en": "VOC memonopoli cengkih di Ambon?", "id": "1605" },
    { "en": "Tragedi Pembantaian Amboyna?", "id": "1623" },
    { "en": "Pembangunan Istana Bogor dimulai?", "id": "1744" },
    { "en": "Perlawanan Nuku di Tidore?", "id": "1780" },
    { "en": "Perang Padri fase kedua?", "id": "1821" },
    { "en": "Penangkapan Tuanku Imam Bonjol?", "id": "1837" },
    { "en": "Pangeran Antasari menjadi Sultan Banjar?", "id": "1862" },
    { "en": "Perang Lombok?", "id": "1894" },
    { "en": "Jong Islamieten Bond didirikan?", "id": "1925" },
    { "en": "Lembaga Kantor Berita Nasional Antara didirikan?", "id": "1937" },
    { "en": "Pembubaran GAPI oleh Jepang?", "id": "1942" },
    { "en": "Pertempuran Selat Sunda?", "id": "1942" },
    { "en": "Pembentukan BKR Laut?", "id": "1945" },
    { "en": "Peristiwa Day of Infamy (Pertempuran Surabaya)?", "id": "1945" },
    { "en": "Peristiwa Westerling di Sulawesi Selatan?", "id": "1946" },
    { "en": "Pemberontakan Angkatan Umat Islam (AUI) di Kebumen?", "id": "1950" },
    { "en": "Gunting Syafruddin (kebijakan moneter)?", "id": "1950" },
    { "en": "Komando Operasi Tertinggi (KOTI) dibentuk?", "id": "1963" },
    { "en": "Komando Ganyang Malaysia (Kogam) dibentuk?", "id": "1964" },
    { "en": "Kunjungan Soeharto ke beberapa negara ASEAN?", "id": "1970" },
    { "en": "Pemberlakuan Ejaan Yang Disempurnakan (EYD)?", "id": "1972" },
    { "en": "Indonesia menjadi tuan rumah World Food Conference?", "id": "1974" },
    { "en": "UU Pemerintahan Daerah No. 5/1974?", "id": "1974" },
    { "en": "Kunjungan Paus Yohanes Paulus II ke Indonesia?", "id": "1989" },
    { "en": "UU Sistem Jaminan Sosial Nasional (SJSN) disahkan?", "id": "2004" },
    { "en": "Rupiah anjlok terhadap dolar?", "id": "1997" },
    { "en": "Gerakan Beli Indonesia diluncurkan?", "id": "1998" },
    { "en": "Kerusuhan Sambas?", "id": "1999" },
    { "en": "Indonesia keluar dari OPEC?", "id": "2008" },
    { "en": "Prasasti Mulak?", "id": "500" },
    { "en": "Prasasti Batutulis di Bogor?", "id": "1533" },
    { "en": "Serangan Mataram ke Blambangan?", "id": "1639" },
    { "en": "Pemberontakan Raden Kajoran?", "id": "1677" },
    { "en": "Perlawanan Martha Christina Tiahahu?", "id": "1817" },
    { "en": "Perang Batak berakhir?", "id": "1907" },
    { "en": "Perang Bayu di Blambangan?", "id": "1771" },
    { "en": "Pendirian Sekolah Dokter Jawa?", "id": "1851" },
    { "en": "Pemuda Indonesia didirikan?", "id": "1927" },
    { "en": "Partai Rakyat Indonesia (PRI) didirikan?", "id": "1930" },
    { "en": "Federasi Keristen di Indonesia?", "id": "1937" },
    { "en": "Pendaratan Sekutu di Sabang?", "id": "1945" },
    { "en": "Pertempuran Margonda di Depok?", "id": "1945" },
    { "en": "Pemberontakan DI/TII Amir Fatah di Jawa Tengah?", "id": "1949" },
    { "en": "Peristiwa Tanjung Morawa?", "id": "1953" },
    { "en": "Rencana Sumitro (Program Benteng)?", "id": "1950" },
    { "en": "Musyawarah Nasional (Munas) Pembangunan?", "id": "1957" },
    { "en": "Deklarasi Ekonomi (Dekon) diumumkan?", "id": "1963" },
    { "en": "Panglima Kostrad Soeharto mengambil alih komando?", "id": "1965" },
    { "en": "Penyerahan Pel-Nawaksara oleh Soekarno?", "id": "1967" },
    { "en": "Golkar menjadi partai politik?", "id": "1971" },
    { "en": "Gerakan Mahasiswa 1978 (menolak NKK/BKK)?", "id": "1978" },
    { "en": "SD Inpres (Sekolah Dasar Instruksi Presiden) mulai dibangun?", "id": "1973" },
    { "en": "Indonesia menjadi anggota G20?", "id": "1999" },
    { "en": "Pemberlakuan kurikulum 2013?", "id": "2013" },
    { "en": "Prasasti Amoghapasa?", "id": "1286" },
    { "en": "Prasasti Manjusri?", "id": "1343" },
    { "en": "Ekspedisi penaklukan Bali oleh Gajah Mada?", "id": "1343" },
    { "en": "Perang Takhta Banten?", "id": "1680" },
    { "en": "Serangan VOC ke Gowa?", "id": "1667" },
    { "en": "Pemberontakan Untung Suropati di Pasuruan?", "id": "1686" },
    { "en": "Pembantaian orang Tionghoa di Batavia?", "id": "1740" },
    { "en": "Janssens menyerah kepada Inggris?", "id": "1811" },
    { "en": "Perlawanan Pangeran Antasari di Banjar?", "id": "1859" },
    { "en": "Sarekat Islam 'Afdeeling B'?", "id": "1917" },
    { "en": "Revolusi Sosial di Sumatera Timur?", "id": "1946" },
    { "en": "Agresi Militer I dilancarkan?", "id": "1947" },
    { "en": "Kabinet Darurat PDRI?", "id": "1948" },
    { "en": "Pemberontakan Ibnu Hadjar berakhir?", "id": "1963" },
    { "en": "Repelita I dimulai?", "id": "1969" },
    { "en": "UU Desa No. 6/2014?", "id": "2014" },
    { "en": "Penemuan Manusia Purba di Trinil?", "id": "1891" },
    { "en": "Perang Padri fase pertama?", "id": "1803" },
    { "en": "Perang Padri fase ketiga?", "id": "1830" },
    { "en": "Perjanjian Masang?", "id": "1824" },
    { "en": "Perlawanan Ida Anak Agung Gde Ngurah Agung?", "id": "1849" },
    { "en": "Budi Utomo mengadakan kongres pertama?", "id": "1908" },
    { "en": "Partai Indonesia Raya (Parindra) didirikan?", "id": "1935" },
    { "en": "Perjanjian Kalijati ditandatangani?", "id": "1942" },
    { "en": "Semboyan 'Sekali Merdeka Tetap Merdeka'?", "id": "1945" },
    { "en": "Konferensi Ekonomi di Yogyakarta?", "id": "1946" },
    { "en": "Pemberontakan Kahar Muzakkar berakhir?", "id": "1965" },
    { "en": "Konfrontasi dengan Malaysia berakhir?", "id": "1966" },
    { "en": "UU Perbankan No. 14/1967?", "id": "1967" },
    { "en": "UU Anti Korupsi pertama?", "id": "1971" },
    { "en": "UU Pemilu No. 12/2003?", "id": "2003" },
    { "en": "Prasasti Kawali?", "id": "1350" },
    { "en": "Pangeran Sambernyawa mendirikan Mangkunegaran?", "id": "1757" },
    { "en": "Pemerintah Kolonial Belanda dibentuk?", "id": "1816" },
    { "en": "Perang Puputan di Kusamba?", "id": "1849" },
    { "en": "Serangan Umum Empat Hari di Solo?", "id": "1949" },
    { "en": "Operasi Jayawijaya?", "id": "1962" },
    { "en": "Supersemar dikeluarkan?", "id": "1966" },
    { "en": "UU Otonomi Khusus bagi Aceh disahkan?", "id": "2001" },
    { "en": "Prasasti Kuburajo I?", "id": "1347" },
    { "en": "Kematian Sultan Ageng Tirtayasa?", "id": "1692" },
    { "en": "Perang Bone?", "id": "1905" },
    { "en": "Organisasi Jong Batak didirikan?", "id": "1925" },
    { "en": "Penandatanganan Perjanjian Linggarjati?", "id": "1947" },
    { "en": "Pembentukan Negara Republik Indonesia Serikat (RIS)?", "id": "1949" },
    { "en": "Operasi Tegas menumpas PRRI?", "id": "1958" },
    { "en": "Indonesia menjadi anggota OPEC?", "id": "1962" },
    { "en": "UU Bank Sentral No. 13/1968?", "id": "1968" },
    { "en": "Kerajaan Ho-ling (Kalingga) dicatat I-Tsing?", "id": "664" },
    { "en": "Perang Paregreg?", "id": "1404" },
    { "en": "Perang saudara Demak?", "id": "1546" },
    { "en": "Amangkurat II menandatangani perjanjian dengan VOC?", "id": "1677" },
    { "en": "Gubernur Jenderal G.A.G.Ph. Baron van der Capellen?", "id": "1816" },
    { "en": "Sisingamangaraja XII memulai perlawanan?", "id": "1878" },
    { "en": "Komite Bumiputera didirikan?", "id": "1913" },
    { "en": "Kongres Bahasa Indonesia pertama di Solo?", "id": "1938" },
    { "en": "Misi Diplomasi Beras ke India?", "id": "1946" },
    { "en": "Pemberontakan Darul Islam di Aceh?", "id": "1953" },
    { "en": "Doktrin 'Poros Jakarta-Peking'?", "id": "1965" },
    { "en": "Gerakan 'Kembali ke Desa'?", "id": "1968" },
    { "en": "Kunjungan Ratu Elizabeth II ke Indonesia?", "id": "1974" },
    { "en": "Peresmian Satelit Palapa A1?", "id": "1976" },
    { "en": "Program Wajib Belajar 9 Tahun?", "id": "1994" },
    { "en": "Prasasti Wurukudu?", "id": "922" },
    { "en": "Prasasti Kalkuta?", "id": "1041" },
    { "en": "Pemberontakan Kertajaya?", "id": "1222" },
    { "en": "Sultan Ali Mughayat Syah mendirikan Kesultanan Aceh?", "id": "1514" },
    { "en": "VOC mengalahkan Sultan Hasanuddin?", "id": "1669" },
    { "en": "Perang Kuning?", "id": "1741" },
    { "en": "Perlawanan Pangeran Antasari?", "id": "1859" },
    { "en": "Puputan Margarana?", "id": "1946" },
    { "en": "Kabinet Ali Sastroamidjojo memerintah?", "id": "1953" },
    { "en": "Pembubaran DPR hasil pemilu 1955?", "id": "1960" },
    { "en": "Pembentukan DPR-GR?", "id": "1960" },
    { "en": "TAP MPRS tentang pembubaran PKI?", "id": "1966" },
    { "en": "Kabinet Pembangunan I?", "id": "1968" },
    { "en": "Pesta Olahraga Asia Tenggara (SEA Games) pertama di Jakarta?", "id": "1979" },
    { "en": "Masa Kerajaan Hindu-Buddha berakhir?", "id": "1500" },
    { "en": "Pendaratan VOC pertama kali?", "id": "1596" },
    { "en": "Awal mula penjajahan Belanda?", "id": "1800" },
    { "en": "Hari Kebangkitan Nasional?", "id": "1908" },
    { "en": "Awal pendudukan Jepang?", "id": "1942" },
    { "en": "Masa Demokrasi Liberal?", "id": "1950" },
    { "en": "Akhir Demokrasi Terpimpin?", "id": "1965" },
    { "en": "Awal Orde Baru?", "id": "1966" },
    { "en": "Akhir Orde Baru?", "id": "1998" },
    { "en": "Masa Reformasi dimulai?", "id": "1998" },
    { "en": "B.J. Habibie menjadi presiden?", "id": "1998" },
    { "en": "Abdurrahman Wahid menjadi presiden?", "id": "1999" },
    { "en": "Megawati Soekarnoputri menjadi presiden?", "id": "2001" },
    { "en": "Susilo Bambang Yudhoyono menjadi presiden?", "id": "2004" },
    { "en": "Joko Widodo menjadi presiden?", "id": "2014" },
    { "en": "Prasasti Sankhara ditulis?", "id": "900" },
    { "en": "Kerajaan Dharmasraya berdiri?", "id": "1183" },
    { "en": "Sunda sebagai kerajaan merdeka?", "id": "669" },
    { "en": "Pembangunan Candi Jawi?", "id": "1292" },
    { "en": "Pemberontakan Lembu Sora?", "id": "1300" },
    { "en": "Ratu Suhita memerintah Majapahit?", "id": "1429" },
    { "en": "Kesultanan Banjar didirikan?", "id": "1520" },
    { "en": "Kesultanan Buton didirikan?", "id": "1332" },
    { "en": "Perang Saudara di Kesultanan Banten?", "id": "1680" },
    { "en": "VOC mendirikan pos dagang di Ambon?", "id": "1605" },
    { "en": "Sultan Agung menaklukkan Surabaya?", "id": "1625" },
    { "en": "Serangan Mataram ke Giri Kedaton?", "id": "1636" },
    { "en": "Amangkurat I membangun Keraton Plered?", "id": "1647" },
    { "en": "Pemberontakan Raden Mas Said (Sambernyawa) dimulai?", "id": "1741" },
    { "en": "Mangkunegara I wafat?", "id": "1795" },
    { "en": "Hamengkubuwono I wafat?", "id": "1792" },
    { "en": "Gubernur Jenderal van der Parra memerintah?", "id": "1761" },
    { "en": "Perang Puputan Bayu di Blambangan?", "id": "1771" },
    { "en": "Pendirian Museum Radya Pustaka?", "id": "1890" },
    { "en": "Janssens menjadi Gubernur Jenderal?", "id": "1811" },
    { "en": "Perlawanan Tuanku Nan Renceh?", "id": "1803" },
    { "en": "Benteng Duurstede direbut Pattimura?", "id": "1817" },
    { "en": "Pangeran Diponegoro membangun markas di Selarong?", "id": "1825" },
    { "en": "Sistem Benteng Stelsel diterapkan di Perang Diponegoro?", "id": "1827" },
    { "en": "Kyai Mojo ditangkap Belanda?", "id": "1828" },
    { "en": "Poenale Sanctie diberlakukan?", "id": "1872" },
    { "en": "Perang Lampung dimulai?", "id": "1817" },
    { "en": "Ekspedisi militer Belanda ke Bali?", "id": "1846" },
    { "en": "Agresi KNIL ke Lombok?", "id": "1894" },
    { "en": "Dewi Sartika mendirikan Sakola Istri?", "id": "1904" },
    { "en": "Kongres pertama Boedi Oetomo di Yogyakarta?", "id": "1908" },
    { "en": "Douwes Dekker mendirikan Indische Partij?", "id": "1912" },
    { "en": "Kongres Sarekat Islam pertama di Surabaya?", "id": "1913" },
    { "en": "Berdirinya Algemeene Studieclub di Bandung?", "id": "1926" },
    { "en": "Tan Malaka menulis 'Naar de Republiek Indonesia'?", "id": "1925" },
    { "en": "Lagu Indonesia Raya pertama kali dipublikasikan?", "id": "1928" },
    { "en": "Soewardi Soerjaningrat menulis 'Als Ik Eens Nederlander Was'?", "id": "1913" },
    { "en": "Kongres Bahasa Indonesia I di Solo?", "id": "1938" },
    { "en": "Komisi Visman dibentuk?", "id": "1940" },
    { "en": "Sayuti Melik mengetik naskah proklamasi?", "id": "1945" },
    { "en": "Latief Hendraningrat mengibarkan bendera pusaka?", "id": "1945" },
    { "en": "Kabinet Presidensial dibubarkan?", "id": "1945" },
    { "en": "Oerip Soemohardjo menjadi Kepala Staf TKR?", "id": "1945" },
    { "en": "Pertempuran Lengkong terjadi?", "id": "1946" },
    { "en": "Sutan Sjahrir berpidato di PBB?", "id": "1947" },
    { "en": "Resolusi DK PBB pertama untuk Indonesia?", "id": "1947" },
    { "en": "Pemberontakan DI/TII Kartosuwiryo diproklamasikan?", "id": "1949" },
    { "en": "Serangan Umum Surakarta?", "id": "1949" },
    { "en": "Hasil KMB disahkan KNIP?", "id": "1949" },
    { "en": "Soekarno kembali ke Jakarta dari Yogyakarta?", "id": "1949" },
    { "en": "Kabinet Halim dibentuk di RI (Yogyakarta)?", "id": "1950" },
    { "en": "Universitas Indonesia diresmikan?", "id": "1950" },
    { "en": "Universitas Gadjah Mada diresmikan?", "id": "1949" },
    { "en": "Pemberontakan Batalyon 426?", "id": "1951" },
    { "en": "Gunting Syafruddin diberlakukan?", "id": "1950" },
    { "en": "Kabinet Wilopo jatuh?", "id": "1953" },
    { "en": "Konstituante dibubarkan?", "id": "1959" },
    { "en": "Bank Negara Indonesia (BNI) didirikan?", "id": "1946" },
    { "en": "Pembangunan Patung Dirgantara (Pancoran)?", "id": "1964" },
    { "en": "Aksi sepihak PKI di Boyolali?", "id": "1964" },
    { "en": "Isu Dokumen Gilchrist muncul?", "id": "1965" },
    { "en": "Jenderal Ahmad Yani terbunuh?", "id": "1965" },
    { "en": "Soeharto membubarkan PKI?", "id": "1966" },
    { "en": "Kabinet Dwikora disempurnakan (Kabinet 100 Menteri)?", "id": "1966" },
    { "en": "Perjanjian Persahabatan Indonesia-Malaysia?", "id": "1966" },
    { "en": "Freeport menandatangani Kontrak Karya pertama?", "id": "1967" },
    { "en": "Pertamina didirikan?", "id": "1957" },
    { "en": "Kabinet Pembangunan I dibentuk?", "id": "1968" },
    { "en": "Kabinet Pembangunan II dibentuk?", "id": "1973" },
    { "en": "Operasi Woyla pembebasan sandera?", "id": "1981" },
    { "en": "UU Pokok Pers No. 21 Tahun 1982?", "id": "1982" },
    { "en": "Bank Muamalat Indonesia berdiri?", "id": "1991" },
    { "en": "KTT Gerakan Non-Blok ke-10 di Jakarta?", "id": "1992" },
    { "en": "Penculikan aktivis 1997/1998 dimulai?", "id": "1997" },
    { "en": "Surat Pernyataan Berhenti Presiden Soeharto?", "id": "1998" },
    { "en": "Pembentukan UKP-PIP (cikal bakal BPIP)?", "id": "2017" },
    { "en": "Kerajaan Salakanagara berdiri?", "id": "130" },
    { "en": "Prasasti Munjul ditemukan?", "id": "400" },
    { "en": "Prasasti Nalanda dibuat?", "id": "860" },
    { "en": "Pembangunan Candi Sambisari?", "id": "812" },
    { "en": "Pembangunan Candi Ijo?", "id": "900" },
    { "en": "Prasasti Luitan?", "id": "901" },
    { "en": "Kerajaan Sriwijaya menguasai Selat Malaka?", "id": "700" },
    { "en": "Airlangga wafat?", "id": "1049" },
    { "en": "Perang saudara antara Janggala dan Panjalu?", "id": "1052" },
    { "en": "Jayakatwang memberontak melawan Kertanegara?", "id": "1292" },
    { "en": "Pemberontakan Ra Pangsa?", "id": "1300" },
    { "en": "Ekspedisi ke Dompo?", "id": "1340" },
    { "en": "Invasi Demak ke Blambangan?", "id": "1546" },
    { "en": "Arya Penangsang terbunuh?", "id": "1549" },
    { "en": "VOC merebut Benteng Somba Opu?", "id": "1669" },
    { "en": "Perang Suksesi Jawa I berakhir?", "id": "1708" },
    { "en": "Untung Suropati tewas?", "id": "1706" },
    { "en": "Perang Buleleng?", "id": "1846" },
    { "en": "Sentot Prawirodirdjo diasingkan?", "id": "1829" },
    { "en": "Perlawanan Radin Inten II di Lampung?", "id": "1850" },
    { "en": "Pangeran Hidayatullah dari Banjar menyerah?", "id": "1862" },
    { "en": "Perang Batak dimulai secara terbuka?", "id": "1878" },
    { "en": "Snouck Hurgronje menyamar di Aceh?", "id": "1884" },
    { "en": "Pendirian Sekolah Kehutanan di Bogor?", "id": "1876" },
    { "en": "Pendirian Perusahaan Kereta Api Negara (Staatsspoorwegen)?", "id": "1875" },
    { "en": "Penerbangan pertama di Hindia Belanda?", "id": "1911" },
    { "en": "Semaun mendirikan PKI?", "id": "1920" },
    { "en": "Pemogokan buruh pegadaian?", "id": "1922" },
    { "en": "Soekarno mendirikan PNI?", "id": "1927" },
    { "en": "PNI dibubarkan oleh Soekarno?", "id": "1931" },
    { "en": "Kongres Perempuan Indonesia II?", "id": "1935" },
    { "en": "Kongres Perempuan Indonesia III?", "id": "1938" },
    { "en": "Latihan militer Gyugun?", "id": "1944" },
    { "en": "Golongan muda menculik Soekarno-Hatta?", "id": "1945" },
    { "en": "Tentara Pelajar (TP) dibentuk?", "id": "1945" },
    { "en": "Syarifuddin Prawiranegara memimpin PDRI?", "id": "1948" },
    { "en": "Kabinet Hatta II dibentuk?", "id": "1949" },
    { "en": "Kedaulatan Indonesia diakui Belanda?", "id": "1949" },
    { "en": "RIS dibubarkan?", "id": "1950" },
    { "en": "Kabinet Sukiman-Suwirjo dibentuk?", "id": "1951" },
    { "en": "Film 'Darah dan Doa' (film nasional pertama) diproduksi?", "id": "1950" },
    { "en": "Indonesia menasionalisasi De Javasche Bank?", "id": "1951" },
    { "en": "Perundingan Djuanda-Subandrio di Tokyo?", "id": "1958" },
    { "en": "Peraturan Pemerintah Pengganti Undang-Undang (Perppu) tentang Keadaan Bahaya?", "id": "1959" },
    { "en": "Proyek Jembatan Ampera dimulai?", "id": "1962" },
    { "en": "Penetapan Soekarno sebagai Pemimpin Besar Revolusi?", "id": "1962" },
    { "en": "Radio Republik Indonesia (RRI) di bawah pengawasan militer?", "id": "1965" },
    { "en": "Jenderal Soeharto menerima Supersemar?", "id": "1966" },
    { "en": "Sidang Umum MPRS ke-4?", "id": "1966" },
    { "en": "UU Perbankan disahkan?", "id": "1967" },
    { "en": "Krisis Ibnu Sutowo (Pertamina)?", "id": "1975" },
    { "en": "UU Narkotika pertama disahkan?", "id": "1976" },
    { "en": "Wajib Belajar 6 Tahun dicanangkan?", "id": "1984" },
    { "en": "Kasus Kedung Ombo?", "id": "1989" },
    { "en": "Indonesia menjadi ketua APEC?", "id": "1994" },
    { "en": "Partai Demokrasi Indonesia (PDI) terpecah?", "id": "1996" },
    { "en": "TAP MPR tentang Otonomi Daerah?", "id": "1998" },
    { "en": "Pembentukan Badan Penyehatan Perbankan Nasional (BPPN)?", "id": "1998" },
    { "en": "UU Penghapusan Diskriminasi Ras dan Etnis disahkan?", "id": "2008" },
    { "en": "Tax Amnesty (Pengampunan Pajak) Jilid I?", "id": "2016" },
    { "en": "Pembangunan Sirkuit Mandalika dimulai?", "id": "2019" },
    { "en": "Prasasti Upit?", "id": "900" },
    { "en": "Candi Kidal dibangun untuk Anusapati?", "id": "1248" },
    { "en": "Pemberontakan Warok?", "id": "1400" },
    { "en": "Kesultanan Palembang didirikan?", "id": "1659" },
    { "en": "Perjanjian Jepara antara Mataram dan VOC?", "id": "1677" },
    { "en": "Pemberontakan Pangeran Puger?", "id": "1703" },
    { "en": "Raffles memberlakukan sistem peradilan baru?", "id": "1812" },
    { "en": "Perang Diponegoro secara resmi dimulai?", "id": "1825" },
    { "en": "Perlawanan Cut Meutia di Aceh?", "id": "1901" },
    { "en": "Jalur kereta api pertama (Semarang-Tanggung) dibuka?", "id": "1867" },
    { "en": "Jaringan telegraf pertama dipasang?", "id": "1855" },
    { "en": "Haji Samanhudi mendirikan Sarekat Dagang Islam?", "id": "1911" },
    { "en": "Kongres Pemuda di Batavia?", "id": "1928" },
    { "en": "Soekarno dipenjara di Sukamiskin?", "id": "1930" },
    { "en": "Fatmawati menjahit bendera pusaka?", "id": "1944" },
    { "en": "Peristiwa Tiga Daerah di Tegal, Brebes, Pemalang?", "id": "1945" },
    { "en": "Perundingan Salatiga (RI-Belanda)?", "id": "1949" },
    { "en": "Pemindahan ibu kota RIS dari Yogyakarta ke Jakarta?", "id": "1949" },
    { "en": "Pemberontakan DI/TII Daud Beureueh berakhir?", "id": "1962" },
    { "en": "Penumpasan PRRI/Permesta selesai?", "id": "1961" },
    { "en": "Pembentukan Resimen Tjakrabirawa?", "id": "1962" },
    { "en": "Aksi Tritura pertama kali?", "id": "1966" },
    { "en": "Penetapan Soeharto sebagai Presiden oleh MPRS?", "id": "1968" },
    { "en": "Badan Koordinasi Intelijen Negara (BAKIN) dibentuk?", "id": "1968" },
    { "en": "Program ABRI Masuk Desa (AMD) dimulai?", "id": "1980" },
    { "en": "Pemilu terakhir Orde Baru?", "id": "1997" },
    { "en": "Empat tuntutan mahasiswa reformasi?", "id": "1998" },
    { "en": "Partai Amanat Nasional (PAN) didirikan?", "id": "1998" },
    { "en": "Partai Kebangkitan Bangsa (PKB) didirikan?", "id": "1998" },
    { "en": "Partai Keadilan (sekarang PKS) didirikan?", "id": "1998" },
    { "en": "Gus Dur dilengserkan?", "id": "2001" },
    { "en": "Lembaga Penjamin Simpanan (LPS) didirikan?", "id": "2004" },
    { "en": "Gempa dan tsunami Pangandaran?", "id": "2006" },
    { "en": "Presidensi G20 Indonesia?", "id": "2022" },
    { "en": "Prasasti Padang Roco?", "id": "1286" },
    { "en": "Tome Pires menulis Suma Oriental?", "id": "1515" },
    { "en": "Perang Aceh-Portugis di Malaka?", "id": "1537" },
    { "en": "Pemberontakan Pangeran Alit di Mataram?", "id": "1647" },
    { "en": "Daendels membangun Rumah Sakit Militer di Batavia?", "id": "1809" },
    { "en": "Perlawanan Thomas Matulessy?", "id": "1817" },
    { "en": "Perang Mentawai?", "id": "1864" },
    { "en": "Telepon pertama kali digunakan di Hindia Belanda?", "id": "1882" },
    { "en": "Nederlandsch-Indische Spoorweg Maatschappij didirikan?", "id": "1863" },
    { "en": "Kongres PNI pertama di Surabaya?", "id": "1928" },
    { "en": "Majalah 'Pikiran Ra'jat' diterbitkan?", "id": "1932" },
    { "en": "Jepang melarang pengibaran Merah Putih?", "id": "1942" },
    { "en": "Naskah proklamasi ditulis tangan Soekarno?", "id": "1945" },
    { "en": "Insiden bendera di Makassar?", "id": "1945" },
    { "en": "Perundingan gencatan senjata pertama?", "id": "1946" },
    { "en": "Blokade ekonomi oleh Belanda?", "id": "1946" },
    { "en": "Amir Sjarifuddin dieksekusi?", "id": "1948" },
    { "en": "PBB membentuk UNCI?", "id": "1949" },
    { "en": "Kabinet Susanto Tirtoprodjo (peralihan)?", "id": "1949" },
    { "en": "Rencana Kesejahteraan Kasimo (Kasimo Plan)?", "id": "1947" },
    { "en": "Badan Perancang Ekonomi dibentuk?", "id": "1947" },
    { "en": "Pembentukan Dewan Perancang Nasional (Depernas)?", "id": "1959" },
    { "en": "Pembangunan Tugu Tani?", "id": "1963" },
    { "en": "Jenderal A.H. Nasution selamat dari G30S?", "id": "1965" },
    { "en": "Tap MPRS tentang Supersemar?", "id": "1966" },
    { "en": "Kabinet Pembangunan III dibentuk?", "id": "1978" },
    { "en": "Kabinet Pembangunan IV dibentuk?", "id": "1983" },
    { "en": "Kabinet Pembangunan V dibentuk?", "id": "1988" },
    { "en": "Kabinet Pembangunan VI dibentuk?", "id": "1993" },
    { "en": "Kabinet Pembangunan VII dibentuk?", "id": "1998" },
    { "en": "Badan Koordinasi Penanaman Modal (BKPM) didirikan?", "id": "1973" },
    { "en": "Pemberlakuan Asas Tunggal Pancasila?", "id": "1985" },
    { "en": "Amien Rais memimpin reformasi?", "id": "1998" },
    { "en": "UU Bank Indonesia yang baru (independensi BI)?", "id": "1999" },
    { "en": "Pembentukan Komisi Yudisial (KY)?", "id": "2004" },
    { "en": "Gerhana Matahari Total disaksikan luas?", "id": "1983" },
    { "en": "KTT APEC di Bali?", "id": "2013" },
    { "en": "Prasasti Astana Gede?", "id": "1350" },
    { "en": "Perang melawan Sawerigading?", "id": "1450" },
    { "en": "Portugis membangun benteng di Ternate?", "id": "1522" },
    { "en": "Pemberontakan Patrona Halil?", "id": "1700" },
    { "en": "Pemerintahan Inggris di Bengkulu berakhir?", "id": "1824" },
    { "en": "Pemberontakan Gedangan di Sidoarjo?", "id": "1904" },
    { "en": "Insulinde didirikan?", "id": "1907" },
    { "en": "Majalah 'Soeloeh Indonesia Moeda' terbit?", "id": "1928" },
    { "en": "Pembentukan Gerakan 3A?", "id": "1942" },
    { "en": "Jepang membubarkan Putera?", "id": "1944" },
    { "en": "BPUPKI dibubarkan?", "id": "1945" },
    { "en": "PPKI dibentuk?", "id": "1945" },
    { "en": "Agresi Militer II dilancarkan?", "id": "1948" },
    { "en": "Perundingan Roem-Royen dimulai?", "id": "1949" },
    { "en": "Pemberontakan DI/TII Kahar Muzakkar dimulai?", "id": "1952" },
    { "en": "Rencana Pembangunan Lima Tahun gagal?", "id": "1958" },
    { "en": "Penpres No. 1 tahun 1961?", "id": "1961" },
    { "en": "Peristiwa Cikini (percobaan pembunuhan Soekarno)?", "id": "1957" },
    { "en": "Operasi Sapta Marga menumpas Permesta?", "id": "1958" },
    { "en": "Aksi pembakaran Kedubes Inggris di Jakarta?", "id": "1963" },
    { "en": "Komando Aksi Pengganyangan Malaysia?", "id": "1964" },
    { "en": "Pengadilan Mahmillub untuk para petinggi G30S?", "id": "1966" },
    { "en": "Kelompencapir (Kelompok Pendengar, Pembaca, dan Pirsawan) populer?", "id": "1980" },
    { "en": "Peringatan 50 tahun Kemerdekaan Indonesia?", "id": "1995" },
    { "en": "Likuidasi 16 bank swasta?", "id": "1997" },
    { "en": "Pendirian Dewan Perwakilan Daerah (DPD)?", "id": "2004" },
    { "en": "Letusan Gunung Sinabung?", "id": "2010" },
    { "en": "Prasasti Horren?", "id": "885" },
    { "en": "Candi Tikus ditemukan?", "id": "1914" },
    { "en": "Babad Giyanti ditulis?", "id": "1755" },
    { "en": "Penyerangan VOC ke Kesultanan Palembang?", "id": "1659" },
    { "en": "Perang Batak (Perang Tapanuli) berakhir?", "id": "1907" },
    { "en": "Pabrik Gula pertama didirikan?", "id": "1830" },
    { "en": "Hoofdenschool (Sekolah Raja) didirikan?", "id": "1878" },
    { "en": "Persatuan Wanita Republik Indonesia (Perwari) didirikan?", "id": "1945" },
    { "en": "Resolusi Jihad NU dikeluarkan?", "id": "1945" },
    { "en": "Pemberontakan Merapi-Merbabu Complex (MMC)?", "id": "1948" },
    { "en": "Sistem Ekonomi Gerakan Benteng?", "id": "1950" },
    { "en": "Konferensi Colombo?", "id": "1954" },
    { "en": "Badan Perencanaan Pembangunan Nasional (Bappenas) didirikan?", "id": "1963" },
    { "en": "Konferensi Islam Asia Afrika?", "id": "1965" },
    { "en": "Lembaga Ilmu Pengetahuan Indonesia (LIPI) didirikan?", "id": "1967" },
    { "en": "Wisma Nusantara, gedung pencakar langit pertama, diresmikan?", "id": "1967" },
    { "en": "UU Pokok Kepegawaian disahkan?", "id": "1974" },
    { "en": "Indonesia mengecam invasi Vietnam ke Kamboja?", "id": "1978" },
    { "en": "Kasus korupsi 'Buloggate'?", "id": "2000" },
    { "en": "Kasus korupsi 'Bruneigate'?", "id": "2000" },
    { "en": "Pembangunan Jembatan Selat Sunda direncanakan?", "id": "2011" },
    { "en": "Penemuan situs Gunung Padang?", "id": "1914" },
    { "en": "Prasasti Cikapundung?", "id": "1341" },
    { "en": "Taman Sari Yogyakarta dibangun?", "id": "1758" },
    { "en": "Perlawanan Sultan Mahmud Badaruddin II?", "id": "1811" },
    { "en": "Perang Bone I?", "id": "1824" },
    { "en": "Perang Bone II?", "id": "1825" },
    { "en": "Perang Bone III?", "id": "1859" },
    { "en": "Pemberontakan Petani Ciomas?", "id": "1886" },
    { "en": "Perkumpulan 'Tirtajasa' didirikan?", "id": "1901" },
    { "en": "Sarekat Islam Putih terbentuk?", "id": "1923" },
    { "en": "Perang Dunia II dimulai di Eropa?", "id": "1939" },
    { "en": "Pemberontakan petani di Indramayu (Cimanuk)?", "id": "1944" },
    { "en": "Kabinet Darurat dipimpin Sjafruddin Prawiranegara?", "id": "1948" },
    { "en": "Perundingan Kaliurang gagal?", "id": "1948" },
    { "en": "Pemberontakan DI/TII di Kalimantan Selatan dimulai?", "id": "1950" },
    { "en": "Deklarasi Pembentukan Gerakan Non-Blok?", "id": "1961" },
    { "en": "Pidato 'Genta Suara Revolusi Indonesia' (Gesuri)?", "id": "1961" },
    { "en": "UU Penanaman Modal Dalam Negeri (PMDN)?", "id": "1968" },
    { "en": "Indonesia memulai program luar angkasa (LAPAN)?", "id": "1963" },
    { "en": "UU Peradilan Agama disahkan?", "id": "1989" },
    { "en": "Megawati menjadi ketua umum PDI?", "id": "1993" },
    { "en": "UNTAET (Pemerintahan Transisi PBB di Timor Timur) dibentuk?", "id": "1999" },
    { "en": "UU Keistimewaan Yogyakarta disahkan?", "id": "2012" },
    { "en": "Prasasti Air Mata?", "id": "1048" },
    { "en": "Perang Pamalayu oleh Kertanegara?", "id": "1275" },
    { "en": "Perang Gowa?", "id": "1666" },
    { "en": "Raffles menghapus perbudakan di Jawa?", "id": "1812" },
    { "en": "Pemberontakan Nuku di Tidore?", "id": "1780" },
    { "en": "Perkumpulan 'Setia Budi' didirikan?", "id": "1913" },
    { "en": "Pemberontakan PKI di Silungkang, Sumatera Barat?", "id": "1927" },
    { "en": "Persatuan Pemuda Pelajar Indonesia (PPPI) didirikan?", "id": "1927" },
    { "en": "Angkatan Laut Republik Indonesia (ALRI) dibentuk?", "id": "1945" },
    { "en": "Angkatan Udara Republik Indonesia (AURI) dibentuk?", "id": "1946" },
    { "en": "Komite van Aksi Menteng 31?", "id": "1945" },
    { "en": "Konferensi Meja Bundar berakhir?", "id": "1949" },
    { "en": "Penandatanganan Piagam Persetujuan RI-RIS?", "id": "1950" },
    { "en": "Piagam Bandung (Konferensi Asia Afrika) disepakati?", "id": "1955" },
    { "en": "UUDS 1950 mulai berlaku?", "id": "1950" },
    { "en": "UU tentang Pembentukan Provinsi Irian Barat?", "id": "1969" },
    { "en": "UU Migas disahkan?", "id": "2001" },
    { "en": "Tragedi bom buku?", "id": "2011" },
    { "en": "Perlawanan rakyat Singaparna?", "id": "1944" },
    { "en": "Pembentukan Kejaksaan Agung?", "id": "1945" },
    { "en": "Pembentukan Mahkamah Agung?", "id": "1945" },
    { "en": "Badan Pengawas Keuangan dan Pembangunan (BPKP) dibentuk?", "id": "1983" },
    { "en": "Pemberantasan Buta Huruf (PBH) digalakkan?", "id": "1951" },
    { "en": "Peringatan Hari Ibu ditetapkan?", "id": "1938" },
    { "en": "Film 'Tiga Dara' Usmar Ismail dirilis?", "id": "1956" },
    { "en": "Festival Film Indonesia (FFI) pertama?", "id": "1955" },
    { "en": "UU Perfilman Nasional disahkan?", "id": "2009" },
    { "en": "Komisi Kebenaran dan Rekonsiliasi (KKR) dibentuk?", "id": "2004" },
    { "en": "Peresmian Masjid Raya Baiturrahman pasca-tsunami?", "id": "2017" },
    { "en": "Perang Paregreg antara Wikramawardhana dan Bhre Wirabhumi?", "id": "1404" },
    { "en": "Pemberontakan Ki Ageng Mangir?", "id": "1588" },
    { "en": "VOC menguasai Malaka dari Portugis?", "id": "1641" },
    { "en": "Perjanjian Painan?", "id": "1663" },
    { "en": "Perang Kediri (melawan Kertajaya)?", "id": "1222" },
    { "en": "Perlawanan Depati Parbo di Jambi?", "id": "1901" },
    { "en": "Pemberontakan Sarekat Rakyat?", "id": "1926" },
    { "en": "Berdirinya 'Studieclub' Surabaya?", "id": "1924" },
    { 'en': 'Rapat Ikada dibubarkan tentara Jepang?', 'id': '1945' },
    { 'en': 'Pemberontakan laskar di Tangerang (Peristiwa Lengkong)?', 'id': '1946' },
    { 'en': 'Program Re-Ra (Rekonstruksi dan Rasionalisasi) angkatan perang?', 'id': '1948' },
    { 'en': 'Pemilu pertama untuk memilih anggota Konstituante?', 'id': '1955' },
    { 'en': 'Rencana Pembangunan Semesta Berencana?', 'id': '1961' },
    { 'en': 'Operasi Sadar menumpas Permesta?', 'id': '1958' },
    { 'en': 'Lembaga Ketahanan Nasional (Lemhannas) didirikan?', 'id': '1965' },
    { 'en': 'UU Penanaman Modal Asing (PMA)?', 'id': '1967' },
    { 'en': 'Gerakan Mahasiswa menolak kenaikan harga BBM?', 'id': '1978' },
    { 'en': 'Kunjungan Presiden AS Bill Clinton ke Indonesia?', 'id': '1994' },
    { 'en': 'Kunjungan Presiden AS Barack Obama ke Indonesia?', 'id': '2010' },
    { 'en': 'Sensus Penduduk Online pertama kali diadakan?', 'id': '2020' },
    { 'en': 'Perang Padri berakhir dengan jatuhnya Benteng Bonjol?', 'id': '1837' },
    { 'en': 'Perlawanan Haji Wasid di Cilegon?', 'id': '1888' },
    { 'en': 'Pemogokan umum buruh terbesar di Hindia Belanda?', 'id': '1923' },
    { 'en': 'Peristiwa Tiga Maret di Sumatera Barat?', 'id': '1947' },
    { 'en': 'Perundingan di kapal Renville dimulai?', 'id': '1947' },
    { 'en': 'Pemberontakan Andi Azis di Makassar?', 'id': '1950' },
    { 'en': 'Indonesia mengakui kedaulatan RRC?', 'id': '1950' },
    { 'en': 'Penumpasan DI/TII di Sulawesi Selatan dan Tenggara?', 'id': '1965' },
    { 'en': 'Misi diplomatik Lambertus Nicodemus Palar di PBB?', 'id': '1947' },
    { 'en': 'Kabinet Pembangunan Nasional di era Gus Dur dibubarkan?', 'id': '2000' },
    { 'en': 'Peresmian Bursa Efek Indonesia (hasil merger)?', 'id': '2007' },
    { 'en': 'Prasasti Pagaruyung VII?', 'id': '1369' },
    { 'en': 'Pemberontakan Bhre Kertabhumi?', 'id': '1468' },
    { 'en': 'Perlawanan Kapitan Kakiali di Maluku?', 'id': '1643' },
    { 'en': 'Hindia Belanda menjadi eksportir gula terbesar kedua?', 'id': '1930' },
    { 'en': 'Perdebatan Soekarno-Natsir tentang dasar negara?', 'id': '1940' },
    { 'en': 'Pemberontakan Teuku Daud Beureueh di Aceh?', 'id': '1953' },
    { 'en': 'Indonesia menjadi tuan rumah Thomas Cup pertama kali?', 'id': '1958' },
    { 'en': 'Tragedi Situjuah Batur?', 'id': '1949' },
    { "en": "Prasasti Wulakan dibuat?", "id": "927" },
    { "en": "Pembangunan Candi Banyunibo?", "id": "800" },
    { "en": "Kerajaan Pagaruyung didirikan Adityawarman?", "id": "1347" },
    { "en": "Pemberontakan Pangeran Dipati Anom di Mataram?", "id": "1661" },
    { "en": "Cornelis Speelman menjadi Gubernur Jenderal VOC?", "id": "1681" },
    { "en": "Pendirian Tiong Hoa Hwee Koan?", "id": "1900" },
    { "en": "Wafatnya Teuku Cik di Tiro?", "id": "1891" },
    { "en": "Koran 'Bataviasche Nouvelles' terbit pertama kali?", "id": "1744" },
    { "en": "Perkumpulan 'Roekoen Peladjar Indonesia' (Roepi) berdiri?", "id": "1914" },
    { "en": "Lembaga Eijkman pertama kali didirikan?", "id": "1888" },
    { "en": "Persatuan Arab Indonesia (PAI) didirikan?", "id": "1934" },
    { "en": "Balai Pustaka didirikan?", "id": "1917" },
    { "en": "Pujangga Baru sebagai gerakan sastra dimulai?", "id": "1933" },
    { "en": "Film 'Loetoeng Kasaroeng' (film pertama) dirilis?", "id": "1926" },
    { "en": "Jepang membentuk 'Barisan Pelopor Istimewa'?", "id": "1944" },
    { "en": "Maklumat Pemerintah tentang pembentukan partai politik?", "id": "1945" },
    { "en": "Peristiwa perobekan bendera di Hotel Yamato?", "id": "1945" },
    { "en": "Laskar Hizbullah dan Sabilillah dibentuk?", "id": "1945" },
    { "en": "Peristiwa 3 Maret di Sumatra?", "id": "1947" },
    { "en": "Yogyakarta diserang dalam Agresi Militer Belanda II?", "id": "1948" },
    { "en": "Kabinet Darurat (PDRI) berakhir?", "id": "1949" },
    { "en": "Westerling melakukan kudeta APRA?", "id": "1950" },
    { "en": "Universitas Airlangga didirikan?", "id": "1954" },
    { "en": "Gerakan 'Asaat' (anti-Tionghoa) muncul?", "id": "1956" },
    { "en": "Piagam Perjuangan Semesta (Permesta) diumumkan?", "id": "1957" },
    { "en": "Soekarno membubarkan Masyumi dan PSI?", "id": "1960" },
    { "en": "Lembaga Pertahanan Nasional (Lemhannas) dibentuk?", "id": "1965" },
    { "en": "Peresmian Jembatan Semanggi?", "id": "1962" },
    { "en": "Operasi Kalong untuk menyusup ke Irian Barat?", "id": "1962" },
    { "en": "Soeharto diangkat menjadi Panglima Kopkamtib?", "id": "1965" },
    { "en": "Demonstrasi menentang 'Kabinet 100 Menteri'?", "id": "1966" },
    { "en": "Repelita I berakhir?", "id": "1974" },
    { "en": "Institut Teknologi Sepuluh Nopember (ITS) diresmikan?", "id": "1960" },
    { "en": "Peristiwa 'Minggu Palma Berdarah' di Dili?", "id": "1975" },
    { "en": "UU tentang Desa disahkan Orde Baru?", "id": "1979" },
    { "en": "Pabrik pesawat IPTN (sekarang PTDI) diresmikan?", "id": "1976" },
    { "en": "Badan Pengkajian dan Penerapan Teknologi (BPPT) didirikan?", "id": "1978" },
    { "en": "Kasus korupsi Eddy Tansil?", "id": "1994" },
    { "en": "UU Telekomunikasi disahkan?", "id": "1999" },
    { "en": "Otoritas Jasa Keuangan (OJK) mulai beroperasi?", "id": "2012" },
    { "en": "Prasasti Harinjing B?", "id": "921" },
    { "en": "Perang Ganter?", "id": "1222" },
    { "en": "Sultan Tahlilullah memerintah Banjar?", "id": "1700" },
    { "en": "Perang Suksesi Jawa II dimulai?", "id": "1719" },
    { "en": "Perjanjian Jepara ditandatangani?", "id": "1677" },
    { "en": "Ekspedisi Belanda ke Kerinci?", "id": "1903" },
    { "en": "Koran 'Slompret Melajoe' terbit di Semarang?", "id": "1860" },
    { "en": "Asosiasi Pers Indonesia (API) didirikan?", "id": "1933" },
    { "en": "Kongres Bahasa Indonesia II di Medan?", "id": "1954" },
    { "en": "Penerbitan novel 'Siti Nurbaya'?", "id": "1922" },
    { "en": "Penerbitan novel 'Layar Terkembang'?", "id": "1936" },
    { "en": "Gerakan Fujinkai dibentuk?", "id": "1943" },
    { "en": "Piagam Jakarta ditandatangani?", "id": "1945" },
    { "en": "Peristiwa 'Tale' di Sulawesi Selatan?", "id": "1946" },
    { "en": "Kabinet Hatta I dibentuk?", "id": "1948" },
    { "en": "Perjanjian Linggarjati diratifikasi KNIP?", "id": "1947" },
    { "en": "Negara Indonesia Timur (NIT) dibubarkan?", "id": "1950" },
    { "en": "Pemberontakan 'Batalyon 501'?", "id": "1952" },
    { "en": "Pemberian status otonomi luas untuk Aceh?", "id": "1959" },
    { "en": "Penpres No.1/1965 (UU Penodaan Agama)?", "id": "1965" },
    { "en": "Misi Garuda II ke Kongo?", "id": "1960" },
    { "en": "Misi Garuda III ke Kongo?", "id": "1962" },
    { "en": "Gerakan Mahasiswa Sosialis (Gemsos) dibentuk?", "id": "1954" },
    { "en": "Konsentrasi Gerakan Mahasiswa Indonesia (CGMI) dibentuk?", "id": "1956" },
    { "en": "Aksi Pengganyangan 'Tujuh Setan Desa'?", "id": "1964" },
    { "en": "Pembersihan unsur PKI di pemerintahan?", "id": "1966" },
    { "en": "BAPPENAS dibentuk menggantikan Depernas?", "id": "1963" },
    { "en": "UU Peradilan Anak disahkan?", "id": "1997" },
    { "en": "UU Kesejahteraan Lanjut Usia disahkan?", "id": "1998" },
    { "en": "Badan Intelijen Negara (BIN) dibentuk (menggantikan BAKIN)?", "id": "2000" },
    { "en": "UU Pemberantasan Tindak Pidana Terorisme?", "id": "2003" },
    { "en": "UU Perlindungan Saksi dan Korban (LPSK)?", "id": "2006" },
    { "en": "Sensus Ekonomi pertama kali diadakan?", "id": "1986" },
    { "en": "Prasasti Garaman?", "id": "1053" },
    { "en": "Candi Gebang ditemukan?", "id": "1936" },
    { "en": "Perang Paregreg berakhir dengan kematian Bhre Wirabhumi?", "id": "1406" },
    { "en": "Perang Suksesi Jawa III dimulai?", "id": "1749" },
    { "en": "Perang Makassar berakhir?", "id": "1669" },
    { "en": "Herman Daendels memerintah sebagai Gubernur Jenderal?", "id": "1808" },
    { "en": "Tan Malaka mendirikan PARI?", "id": "1929" },
    { "en": "Chairil Anwar menulis 'Aku'?", "id": "1943" },
    { "en": "Pembentukan Badan Pekerja Komite Nasional Indonesia Pusat (BP-KNIP)?", "id": "1945" },
    { "en": "Angkatan Kepolisian Republik Indonesia (AKRI) dibentuk?", "id": "1946" },
    { "en": "Perjanjian Gencatan Senjata pertama RI-Belanda?", "id": "1946" },
    { "en": "Pemberontakan PKI di Cirebon?", "id": "1946" },
    { "en": "Pemberontakan komunis di Madiun?", "id": "1948" },
    { "en": "Kabinet Natsir jatuh?", "id": "1951" },
    { "en": "Perlombaan layar Ancol-Amsterdam?", "id": "1936" },
    { "en": "Gagasan 'Jalan Tengah' A.H. Nasution?", "id": "1958" },
    { "en": "Penentuan Pendapat Rakyat (Pepera) dilaksanakan?", "id": "1969" },
    { "en": "KOPKAMTIB dibubarkan?", "id": "1988" },
    { "en": "Badan Koordinasi Stabilitas Nasional (Bakorstanas) dibentuk?", "id": "1988" },
    { "en": "Pemilu pertama era Reformasi?", "id": "1999" },
    { "en": "UU Ketenagakerjaan disahkan?", "id": "2003" },
    { "en": "Badan Nasional Penanggulangan Bencana (BNPB) dibentuk?", "id": "2008" },
    { "en": "UU Pengadaan Tanah bagi Pembangunan disahkan?", "id": "2012" },
    { "en": "Peresmian Tol Trans-Jawa?", "id": "2018" },
    { "en": "Prasasti Gajah Mada?", "id": "1351" },
    { "en": "Pembangunan Istana Maimun di Medan?", "id": "1888" },
    { "en": "H.O.S. Tjokroaminoto wafat?", "id": "1934" },
    { "en": "K.H. Ahmad Dahlan wafat?", "id": "1923" },
    { "en": "K.H. Hasyim Asy'ari wafat?", "id": "1947" },
    { "en": "Partai Katolik Republik Indonesia didirikan?", "id": "1945" },
    { "en": "Partai Kristen Indonesia (Parkindo) didirikan?", "id": "1945" },
    { "en": "Peristiwa Serangan Fajar di Solo?", "id": "1949" },
    { "en": "Pemberontakan Soumokil (RMS)?", "id": "1950" },
    { "en": "Kabinet Dwikora I dibentuk?", "id": "1965" },
    { "en": "Sidang pertama Kabinet Ampera?", "id": "1966" },
    { "en": "Misi Garuda IV ke Vietnam?", "id": "1973" },
    { "en": "Misi Garuda V ke Vietnam?", "id": "1973" },
    { "en": "Misi Garuda VI ke Timur Tengah?", "id": "1973" },
    { "en": "UU Perubahan Atas UU Pemilu?", "id": "1980" },
    { "en": "UU tentang Wajib Daftar Perusahaan?", "id": "1982" },
    { "en": "Sidang Istimewa MPR melengserkan Gus Dur?", "id": "2001" },
    { "en": "UU Pemberantasan Tindak Pidana Perdagangan Orang?", "id": "2007" },
    { "en": "UU Administrasi Kependudukan?", "id": "2006" },
    { "en": "Ibu Kota Nusantara ditetapkan secara hukum?", "id": "2022" },
    { "en": "Prasasti Anjuk Ladang (Jayastambha)?", "id": "937" },
    { "en": "Sultan Agung menetapkan kalender Jawa?", "id": "1633" },
    { "en": "Perlawanan Pangeran Trunajaya dimulai?", "id": "1674" },
    { "en": "Ekspedisi Pamalayu dilancarkan?", "id": "1275" },
    { "en": "Perlawanan Untung Suropati berakhir?", "id": "1706" },
    { "en": "Jong Sumatranen Bond dibubarkan?", "id": "1929" },
    { "en": "Penerbitan surat kabar 'Medan Prijaji'?", "id": "1907" },
    { "en": "Pembentukan 'Volksraad' (Dewan Rakyat)?", "id": "1918" },
    { "en": "Misi Sjahrir mencari dukungan ke India dan Mesir?", "id": "1947" },
    { "en": "Musso kembali ke Indonesia dari Uni Soviet?", "id": "1948" },
    { "en": "Pemberontakan Ibnu Hadjar di Kalsel dimulai?", "id": "1950" },
    { "en": "Pemberian amnesti kepada sisa-sisa DI/TII?", "id": "1962" },
    { "en": "Rencana Pembangunan Lima Tahun (RPLT) 1956-1960?", "id": "1956" },
    { "en": "Pelaksanaan Asian Games di Jakarta?", "id": "1962" },
    { "en": "Penculikan dan pembunuhan jenderal Angkatan Darat?", "id": "1965" },
    { "en": "Dewan Mahasiswa di berbagai universitas dibekukan?", "id": "1978" },
    { "en": "Paket Kebijakan Deregulasi Perbankan (Pakto 88)?", "id": "1988" },
    { "en": "KTT APEC di Bogor melahirkan 'Bogor Goals'?", "id": "1994" },
    { "en": "Tragedi Semanggi I?", "id": "1998" },
    { "en": "Tragedi Semanggi II?", "id": "1999" },
    { "en": "Prasasti Suruaso?", "id": "1375" },
    { "en": "Trunajaya merebut Keraton Plered?", "id": "1677" },
    { "en": "Pemberontakan Cina di Batavia (Geger Pacinan)?", "id": "1740" },
    { "en": "Pemberontakan petani di Condet?", "id": "1916" },
    { "en": "Kongres Jong Java di Solo?", "id": "1922" },
    { "en": "Penyelenggaraan Pekan Olahraga Nasional (PON) I di Solo?", "id": "1948" },
    { "en": "Peristiwa 'Zwarte Sinterklaas' di Surabaya?", "id": "1948" },
    { "en": "Pembentukan Komisi Tiga Negara (KTN)?", "id": "1947" },
    { "en": "Pengambilalihan perusahaan-perusahaan Belanda?", "id": "1957" },
    { "en": "Kabinet Kerja I dibentuk?", "id": "1959" },
    { "en": "Indonesia menjadi tuan rumah Ganefo?", "id": "1963" },
    { "en": "Penyerbuan Gedung Sekretariat ASEAN oleh demonstran?", "id": "1963" },
    { "en": "Deklarasi Bogor (KTT APEC)?", "id": "1994" },
    { "en": "Pilkada langsung serentak pertama?", "id": "2015" },
    { "en": "Prasasti Blanjong di Bali?", "id": "914" },
    { "en": "Sultan Baabullah mengusir Portugis?", "id": "1575" },
    { "en": "Pembangunan Observatorium Bosscha?", "id": "1923" },
    { "en": "Peristiwa Kapal Tujuh Provinsi (De Zeven Provinciën)?", "id": "1933" },
    { "en": "Pemberontakan PETA di Gumilir, Cilacap?", "id": "1945" },
    { "en": "Pembentukan Kabinet RIS?", "id": "1949" },
    { "en": "Misi pembelian senjata A.H. Nasution ke luar negeri?", "id": "1950" },
    { "en": "Program Pagar Betis menumpas DI/TII?", "id": "1959" },
    { "en": "Doktrin 'Resopim' (Revolusi, Sosialisme Indonesia, Pimpinan Nasional)?", "id": "1961" },
    { "en": "UU Perguruan Tinggi disahkan?", "id": "1961" },
    { "en": "Peristiwa Idul Fitri Berdarah di Dili?", "id": "1983" },
    { "en": "UU Lalu Lintas pertama kali disahkan?", "id": "1965" },
    { "en": "UU Penyiaran disahkan era Reformasi?", "id": "2002" },
    { "en": "Indonesia menjadi tuan rumah World Ocean Conference?", "id": "2009" },
    { "en": "Prasasti Sdak?", "id": "1296" },
    { "en": "J.P. Coen menghancurkan Jayakarta?", "id": "1619" },
    { "en": "Koninklijke Paketvaart-Maatschappij (KPM) didirikan?", "id": "1888" },
    { "en": "Fatwa Resolusi Jihad NU?", "id": "1945" },
    { "en": "Perundingan gencatan senjata dihentikan sepihak oleh Belanda?", "id": "1947" },
    { "en": "Perjanjian Keamanan RI-Australia (Perjanjian Lombok)?", "id": "2006" },
    { "en": "Kebijakan redenominasi rupiah diumumkan?", "id": "2013" },
    { "en": "Prasasti Ambetra?", "id": "900" },
    { "en": "Pemberontakan Pangeran Puger berakhir?", "id": "1708" },
    { "en": "Pembukaan tambang timah di Bangka?", "id": "1710" },
    { "en": "Gerakan Samin (Saminisme) muncul?", "id": "1890" },
    { "en": "Komite Penyelamat Diri dari Kelaparan di Pulau Jawa?", "id": "1944" },
    { "en": "Aksi Polisionil pertama Belanda?", "id": "1947" },
    { "en": "Aksi Polisionil kedua Belanda?", "id": "1948" },
    { "en": "Misi Sudarsono ke India?", "id": "1947" },
    { "en": "Kabinet Karya pimpinan Djuanda?", "id": "1957" },
    { "en": "UU Pokok Agraria disahkan?", "id": "1960" },
    { "en": "Program Pembangunan Nasional Semesta Berencana?", "id": "1961" },
    { "en": "UU tentang Karantina Hewan, Ikan, dan Tumbuhan disahkan?", "id": "1992" },
    { "en": "Banjir bandang dan longsor di Jember?", "id": "2006" },
    { "en": "Pengesahan UU Ibu Kota Negara (IKN)?", "id": "2022" },
    { "en": "Situs manusia purba Liang Bua ditemukan?", "id": "2003" }




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
