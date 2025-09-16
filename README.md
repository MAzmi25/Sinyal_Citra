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


  { "en": "Apa itu sinyal?", "id": "Representasi informasi dari suatu fenomena fisik." },
  { "en": "Apa itu citra?", "id": "Representasi visual dua dimensi dari objek." },
  { "en": "Sinyal analog?", "id": "Sinyal dengan nilai kontinu setiap saat." },
  { "en": "Sinyal digital?", "id": "Sinyal dengan nilai diskrit pada waktu diskrit." },
  { "en": "Sinyal waktu kontinu?", "id": "Sinyal yang didefinisikan pada setiap waktu." },
  { "en": "Sinyal waktu diskrit?", "id": "Sinyal yang didefinisikan pada waktu tertentu." },
  { "en": "Apa itu piksel?", "id": "Elemen terkecil dari sebuah citra digital." },
  { "en": "Apa itu resolusi citra?", "id": "Ukuran detail yang dimiliki sebuah citra." },
  { "en": "Proses sampling?", "id": "Mengubah sinyal kontinu menjadi sinyal diskrit." },
  { "en": "Proses kuantisasi?", "id": "Mengubah amplitudo sinyal menjadi level diskrit." },
  { "en": "Apa itu noise?", "id": "Gangguan acak yang tidak diinginkan pada sinyal." },
  { "en": "Domain waktu?", "id": "Representasi sinyal terhadap fungsi waktu." },
  { "en": "Domain frekuensi?", "id": "Representasi sinyal terhadap komponen frekuensinya." },
  { "en": "Apa itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah sinyal analog menjadi sinyal digital." },
  { "en": "Apa itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah sinyal digital menjadi sinyal analog." },
  { "en": "Apa kepanjangan dari DSP?", "id": "Digital Signal Processing." },
  { "en": "Apa kepanjangan dari DIP?", "id": "Digital Image Processing." },
  { "en": "Citra biner?", "id": "Citra yang hanya memiliki dua level intensitas." },
  { "en": "Citra grayscale?", "id": "Citra dengan level keabuan dari hitam-putih." },
  { "en": "Citra berwarna?", "id": "Citra yang merepresentasikan informasi warna." },
  { "en": "Model warna RGB (Red, Green, Blue)?", "id": "Model warna aditif berbasis tiga warna." },
  { "en": "Model warna CMYK (Cyan, Magenta, Yellow, Key)?", "id": "Model warna subtraktif untuk pencetakan." },
  { "en": "Apa itu histogram citra?", "id": "Grafik distribusi intensitas nilai piksel." },
  { "en": "Apa itu Fourier Transform?", "id": "Mengubah sinyal dari domain waktu ke frekuensi." },
  { "en": "Apa itu sinyal periodik?", "id": "Sinyal yang berulang dalam interval waktu." },
  { "en": "Apa itu sinyal aperiodik?", "id": "Sinyal yang tidak memiliki pola berulang." },
  { "en": "Frekuensi sinyal?", "id": "Jumlah siklus per satuan waktu." },
  { "en": "Apa Itu 'Amplitudo sinyal'?", "id": "Besar simpangan maksimum dari nilai nol." },
  { "en": "Fase sinyal?", "id": "Posisi relatif sinyal pada suatu waktu." },
  { "en": "Apa itu sistem LTI (Linear Time-Invariant)?", "id": "Sistem linear yang tidak berubah oleh waktu." },
  { "en": "Definisi konvolusi?", "id": "Operasi matematika penggabungan dua sinyal." },
  { "en": "Apa itu filter?", "id": "Sistem yang memodifikasi komponen frekuensi sinyal." },
  { "en": "Jenis low-pass filter?", "id": "Melewatkan frekuensi rendah, meredam frekuensi tinggi." },
  { "en": "Jenis high-pass filter?", "id": "Melewatkan frekuensi tinggi, meredam frekuensi rendah." },
  { "en": "Jenis band-pass filter?", "id": "Melewatkan rentang frekuensi tertentu." },
  { "en": "Jenis band-stop filter?", "id": "Meredam rentang frekuensi tertentu." },
  { "en": "Apa itu Nyquist Rate?", "id": "Laju sampling minimum untuk menghindari aliasing." },
  { "en": "Apa itu aliasing?", "id": "Distorsi sinyal akibat laju sampling rendah." },
  { "en": "Apa itu kernel?", "id": "Matriks kecil untuk operasi konvolusi citra." },
  { "en": "Operasi titik pada citra?", "id": "Memodifikasi piksel tanpa melihat tetangganya." },
  { "en": "Operasi spasial pada citra?", "id": "Memodifikasi piksel berdasarkan nilai tetangganya." },
  { "en": "Apa itu DFT (Discrete Fourier Transform)?", "id": "Transformasi Fourier untuk sinyal waktu diskrit." },
  { "en": "Apa itu FFT (Fast Fourier Transform)?", "id": "Algoritma efisien untuk menghitung DFT." },
  { "en": "Apa itu sinyal deterministik?", "id": "Sinyal yang dapat diprediksi secara matematis." },
  { "en": "Apa itu sinyal acak?", "id": "Sinyal yang tidak dapat diprediksi nilainya." },
  { "en": "Apa itu bit depth?", "id": "Jumlah bit informasi warna per piksel." },
  { "en": "Apa itu kontras citra?", "id": "Perbedaan antara intensitas maksimum dan minimum." },
  { "en": "Apa itu kecerahan citra?", "id": "Tingkat pencahayaan keseluruhan dari suatu citra." },
  { "en": "Sistem kausal?", "id": "Output hanya bergantung pada input saat ini/lalu." },
  { "en": "Sistem non-kausal?", "id": "Output bergantung pada input masa depan." },
  { "en": "Sistem stabil?", "id": "Output terbatas untuk input yang terbatas." },
  { "en": "Sistem tidak stabil?", "id": "Output tidak terbatas untuk input terbatas." },
  { "en": "Apa itu sinyal energi?", "id": "Sinyal dengan total energi yang berhingga." },
  { "en": "Apa itu sinyal daya?", "id": "Sinyal dengan daya rata-rata berhingga." },
  { "en": "Apa itu Z-Transform?", "id": "Generalisasi DFT untuk analisis sistem diskrit." },
  { "en": "Apa itu region of convergence (ROC)?", "id": "Daerah konvergensi dari Z-Transform." },
  { "en": "Apa itu pole pada sistem?", "id": "Akar dari penyebut fungsi transfer." },
  { "en": "Apa itu zero pada sistem?", "id": "Akar dari pembilang fungsi transfer." },
  { "en": "Apa itu impulse response?", "id": "Output sistem saat diberi input impuls." },
  { "en": "Apa itu sinyal impuls?", "id": "Sinyal dengan durasi sangat singkat, amplitudo tak-hingga." },
  { "en": "Apa itu sinyal step?", "id": "Sinyal bernilai nol sebelum t=0, satu sesudahnya." },
  { "en": "Apa itu interpolasi?", "id": "Proses estimasi nilai di antara titik data." },
  { "en": "Apa itu decimasi?", "id": "Proses mengurangi laju sampling sinyal." },
  { "en": "Apa itu FIR (Finite Impulse Response) filter?", "id": "Filter dengan respon impuls durasi terbatas." },
  { "en": "Apa itu IIR (Infinite Impulse Response) filter?", "id": "Filter dengan respon impuls durasi tak-terbatas." },
  { "en": "Keuntungan filter FIR?", "id": "Selalu stabil dan memiliki fase linear." },
  { "en": "Keuntungan filter IIR?", "id": "Lebih efisien secara komputasi daripada FIR." },
  { "en": "Apa itu windowing?", "id": "Teknik mengurangi kebocoran spektral pada DFT." },
  { "en": "Apa itu zero-padding?", "id": "Menambahkan nol pada sinyal sebelum transformasi." },
  { "en": "Definisi korelasi?", "id": "Ukuran kemiripan antara dua sinyal." },
  { "en": "Apa itu autokorelasi?", "id": "Korelasi sinyal dengan dirinya sendiri." },
  { "en": "Apa itu cross-korelasi?", "id": "Korelasi antara dua sinyal yang berbeda." },
  { "en": "Apa itu PSNR (Peak Signal-to-Noise Ratio)?", "id": "Ukuran kualitas rekonstruksi citra atau sinyal." },
  { "en": "Apa itu MSE (Mean Squared Error)?", "id": "Rata-rata kuadrat selisih antara dua sinyal." },
  { "en": "Tujuan image enhancement?", "id": "Meningkatkan kualitas visual citra untuk manusia." },
  { "en": "Tujuan image restoration?", "id": "Memperbaiki citra yang terdegradasi menjadi asli." },
  { "en": "Tujuan image compression?", "id": "Mengurangi ukuran file citra tanpa hilang informasi." },
  { "en": "Tipe kompresi lossless?", "id": "Kompresi data tanpa kehilangan informasi apapun." },
  { "en": "Tipe kompresi lossy?", "id": "Kompresi data dengan menghilangkan beberapa informasi." },
  { "en": "Apa itu hue?", "id": "Atribut warna murni (misalnya merah, hijau)." },
  { "en": "Apa itu saturation?", "id": "Intensitas atau kemurnian dari suatu warna." },
  { "en": "Apa itu value (brightness)?", "id": "Tingkat kecerahan atau kegelapan suatu warna." },
  { "en": "Apa itu model warna HSV?", "id": "Hue, Saturation, Value." },
  { "en": "Apa itu segmentasi citra?", "id": "Mempartisi citra menjadi beberapa segmen (objek)." },
  { "en": "Apa itu deteksi tepi?", "id": "Mengidentifikasi batas objek dalam sebuah citra." },
  { "en": "Apa itu thresholding?", "id": "Mengubah citra grayscale menjadi citra biner." },
  { "en": "Apa itu morfologi matematika?", "id": "Analisis bentuk objek dalam citra." },
  { "en": "Operasi erosi morfologi?", "id": "Mengikis batas-batas objek dalam citra." },
  { "en": "Operasi dilasi morfologi?", "id": "Memperluas batas-batas objek dalam citra." },
  { "en": "Operasi opening morfologi?", "id": "Erosi diikuti oleh dilasi." },
  { "en": "Operasi closing morfologi?", "id": "Dilasi diikuti oleh erosi." },
  { "en": "Apa itu connected components labeling?", "id": "Menemukan dan melabeli semua objek terhubung." },
  { "en": "Apa itu ekstraksi fitur?", "id": "Mengambil informasi penting dari data mentah." },
  { "en": "Apa itu klasifikasi citra?", "id": "Memberi label pada citra berdasarkan isinya." },
  { "en": "Apa itu wavelet transform?", "id": "Transformasi sinyal dengan representasi waktu-frekuensi." },
  { "en": "Sinyal satu dimensi (1D)?", "id": "Sinyal yang bergantung pada satu variabel." },
  { "en": "Sinyal dua dimensi (2D)?", "id": "Sinyal yang bergantung pada dua variabel." },
  { "en": "Apa itu filter Butterworth?", "id": "Filter dengan respons frekuensi paling datar." },
  { "en": "Apa itu filter Chebyshev?", "id": "Filter dengan ripple di passband atau stopband." },
  { "en": "Apa itu filter Elliptic?", "id": "Filter dengan ripple di kedua passband/stopband." },
  { "en": "Fungsi DCT (Discrete Cosine Transform)?", "id": "Memisahkan citra menjadi bagian frekuensi berbeda." },
  { "en": "Aplikasi umum DCT?", "id": "Kompresi citra seperti pada format JPEG." },
  { "en": "Apa itu Power Spectral Density (PSD)?", "id": "Distribusi daya sinyal pada berbagai frekuensi." },
  { "en": "Apa itu white noise?", "id": "Noise dengan PSD konstan di semua frekuensi." },
  { "en": "Noise salt-and-pepper?", "id": "Noise berupa titik-titik hitam dan putih." },
  { "en": "Noise Gaussian?", "id": "Noise dengan distribusi probabilitas bentuk lonceng." },
  { "en": "Teknik histogram equalization?", "id": "Meningkatkan kontras citra dengan meratakan histogram." },
  { "en": "Teknik contrast stretching?", "id": "Memperluas rentang intensitas piksel suatu citra." },
  { "en": "Apa itu median filter?", "id": "Filter non-linear untuk mengurangi noise salt-and-pepper." },
  { "en": "Apa itu mean filter?", "id": "Filter linear untuk menghaluskan citra (blurring)." },
  { "en": "Apa itu Gaussian filter?", "id": "Filter untuk mengurangi noise dengan kernel Gaussian." },
  { "en": "Apa itu DWT (Discrete Wavelet Transform)?", "id": "Analisis sinyal menggunakan fungsi basis wavelet." },
  { "en": "Apa itu upsampling?", "id": "Proses meningkatkan laju sampling suatu sinyal." },
  { "en": "Apa itu downsampling?", "id": "Proses menurunkan laju sampling suatu sinyal." },
  { "en": "Definisi Sinyal Multirate?", "id": "Pemrosesan sinyal pada laju sampling berbeda." },
  { "en": "Apa itu image sharpening?", "id": "Teknik untuk menonjolkan tepi pada citra." },
  { "en": "Contoh operator deteksi tepi?", "id": "Operator Sobel, Prewitt, dan Canny." },
  { "en": "Tujuan operator Sobel?", "id": "Mendeteksi tepi dengan menghitung gradien intensitas." },
  { "en": "Apa itu Canny edge detection?", "id": "Metode deteksi tepi dengan performa optimal." },
  { "en": "Model warna YCbCr?", "id": "Representasi warna dengan komponen luma dan chroma." },
  { "en": "Komponen luma (Y)?", "id": "Informasi kecerahan (brightness) pada sinyal video." },
  { "en": "Komponen chroma (Cb, Cr)?", "id": "Informasi perbedaan warna (biru dan merah)." },
  { "en": "Apa itu sinyal stasioner?", "id": "Sinyal dengan properti statistik tidak berubah." },
  { "en": "Apa itu sinyal non-stasioner?", "id": "Sinyal dengan properti statistik berubah waktu." },
  { "en": "Apa itu Gibbs phenomenon?", "id": "Osilasi dekat diskontinuitas pada deret Fourier." },
  { "en": "Fungsi Jendela (Window Function)?", "id": "Mengurangi diskontinuitas di tepi sinyal." },
  { "en": "Jendela Rectangular?", "id": "Jendela paling sederhana, tidak melakukan apa-apa." },
  { "en": "Jendela Hamming/Hanning?", "id": "Mengurangi spectral leakage lebih baik dari rectangular." },
  { "en": "Apa itu spectral leakage?", "id": "Penyebaran energi sinyal ke frekuensi lain." },
  { "en": "Apa itu Laplacian of Gaussian (LoG)?", "id": "Operator untuk mendeteksi tepi dan blob." },
  { "en": "Apa itu registrasi citra?", "id": "Menyelaraskan dua atau lebih citra." },
  { "en": "Apa itu fusi citra?", "id": "Menggabungkan informasi dari beberapa citra." },
  { "en": "Apa itu color space?", "id": "Model matematika untuk merepresentasikan himpunan warna." },
  { "en": "Apa itu color gamut?", "id": "Rentang warna yang bisa ditampilkan perangkat." },
  { "en": "Apa itu interpolasi nearest-neighbor?", "id": "Menggunakan nilai piksel terdekat saat resizing." },
  { "en": "Apa itu interpolasi bilinear?", "id": "Menggunakan rata-rata 4 piksel tetangga." },
  { "en": "Apa itu interpolasi bikubik?", "id": "Menggunakan rata-rata 16 piksel tetangga." },
  { "en": "Apa itu inpainting?", "id": "Teknik merekonstruksi bagian citra yang rusak." },
  { "en": "Apa itu deblurring?", "id": "Proses untuk mengurangi blur pada citra." },
  { "en": "Penyebab motion blur?", "id": "Gerakan cepat objek atau kamera." },
  { "en": "Apa itu Point Spread Function (PSF)?", "id": "Respons sistem optik terhadap sumber titik." },
  { "en": "Apa itu dekonvolusi?", "id": "Proses membalik efek konvolusi, misal deblurring." },
  { "en": "Apa itu Wiener filter?", "id": "Filter optimal untuk restorasi sinyal/citra." },
  { "en": "Apa itu sinyal pita dasar (baseband)?", "id": "Sinyal dengan frekuensi mendekati nol." },
  { "en": "Apa itu sinyal passband?", "id": "Sinyal yang dimodulasi ke frekuensi pembawa." },
  { "en": "Apa itu modulasi?", "id": "Proses menumpangkan sinyal informasi ke sinyal pembawa." },
  { "en": "Apa itu demodulasi?", "id": "Proses mengambil sinyal informasi dari pembawa." },
  { "en": "Apa itu sinyal I/Q (In-phase/Quadrature)?", "id": "Representasi sinyal passband pada baseband." },
  { "en": "Struktur filter Direct Form I?", "id": "Implementasi filter digital secara langsung." },
  { "en": "Struktur filter Direct Form II?", "id": "Bentuk implementasi filter yang lebih efisien." },
  { "en": "Apa itu filter cascade?", "id": "Menghubungkan beberapa filter secara seri." },
  { "en": "Apa itu filter parallel?", "id": "Menghubungkan beberapa filter secara paralel." },
  { "en": "Definisi kuantisasi vektor?", "id": "Kuantisasi sekelompok data secara bersamaan." },
  { "en": "Apa itu dither?", "id": "Menambahkan noise untuk mencegah artefak kuantisasi." },
  { "en": "Apa itu image moments?", "id": "Deskriptor global bentuk objek dalam citra." },
  { "en": "Apa itu template matching?", "id": "Mencari lokasi sebuah template dalam citra." },
  { "en": "Definisi stereo vision?", "id": "Memperkirakan kedalaman dari dua citra." },
  { "en": "Apa itu epipolar geometry?", "id": "Geometri intrinsik dari dua pandangan." },
  { "en": "Apa itu optical flow?", "id": "Pola gerakan objek antara dua frame." },
  { "en": "Apa itu background subtraction?", "id": "Mendeteksi objek bergerak dengan memisahkan latar." },
  { "en": "Apa itu Hough Transform?", "id": "Teknik untuk mendeteksi bentuk seperti garis/lingkaran." },
  { "en": "Apa itu color constancy?", "id": "Kemampuan membedakan warna di bawah iluminasi berbeda." },
  { "en": "Apa itu white balancing?", "id": "Menyesuaikan warna agar objek putih terlihat putih." },
  { "en": "Apa itu gamma correction?", "id": "Mengoreksi kecerahan dan kontras pada monitor." },
  { "en": "Definisi sinyal chirp?", "id": "Sinyal dengan frekuensi yang berubah waktu." },
  { "en": "Apa itu group delay?", "id": "Rata-rata delay sinyal melewati suatu sistem." },
  { "en": "Apa itu phase delay?", "id": "Delay waktu untuk setiap komponen frekuensi." },
  { "en": "Apa itu fase linear?", "id": "Kondisi dimana group delay konstan." },
  { "en": "Mengapa fase linear penting?", "id": "Menghindari distorsi bentuk gelombang sinyal." },
  { "en": "Apa itu sistem minimum phase?", "id": "Sistem stabil dan inversnya juga stabil." },
  { "en": "Apa itu sistem all-pass?", "id": "Sistem yang hanya mengubah fasa sinyal." },
  { "en": "Apa itu companding?", "id": "Mengompres lalu mengekspansi rentang dinamis sinyal." },
  { "en": "Apa itu A-law/μ-law?", "id": "Algoritma companding standar dalam telekomunikasi." },
  { "en": "Apa itu codec?", "id": "Perangkat/program untuk encoding dan decoding sinyal." },
  { "en": "Apa itu bit rate?", "id": "Jumlah bit yang diproses per satuan waktu." },
  { "en": "Apa itu baud rate?", "id": "Jumlah perubahan simbol per satuan waktu." },
  { "en": "Apa itu aliasing spasial?", "id": "Artefak pola pada citra akibat sampling rendah." },
  { "en": "Apa itu Moiré pattern?", "id": "Pola interferensi akibat superposisi dua kisi." },
  { "en": "Apa itu aliasing temporal?", "id": "Contohnya roda tampak berputar ke belakang." },
  { "en": "Apa itu watermark digital?", "id": "Informasi yang disembunyikan dalam sinyal/citra." },
  { "en": "Apa itu steganografi?", "id": "Seni menyembunyikan pesan rahasia dalam media." },
  { "en": "Apa itu matriks kovarians?", "id": "Matriks yang merepresentasikan korelasi antar-dimensi data." },
  { "en": "Apa itu PCA (Principal Component Analysis)?", "id": "Teknik reduksi dimensi data." },
  { "en": "Apa itu SVD (Singular Value Decomposition)?", "id": "Faktorisasi matriks menjadi tiga matriks khusus." },
  { "en": "Apa itu region growing?", "id": "Metode segmentasi dengan mengelompokkan piksel serupa." },
  { "en": "Apa itu k-means clustering?", "id": "Algoritma untuk mempartisi data menjadi k cluster." },
  { "en": "Apa itu texture analysis?", "id": "Analisis karakteristik permukaan suatu objek." },
  { "en": "Apa itu super-resolution?", "id": "Meningkatkan resolusi citra melebihi sensor." },
  { "en": "Apa itu HDR (High Dynamic Range) imaging?", "id": "Menggabungkan beberapa eksposur menjadi satu citra." },
  { "en": "Apa itu FPGA (Field-Programmable Gate Array)?", "id": "Sirkuit terpadu yang dapat diprogram pengguna." },
  { "en": "Apa itu GPU (Graphics Processing Unit)?", "id": "Prosesor khusus untuk komputasi paralel." },
  { "en": "Apa itu filter adaptif?", "id": "Filter yang parameternya dapat menyesuaikan diri." },
  { "en": "Apa itu algoritma LMS (Least Mean Squares)?", "id": "Algoritma adaptif untuk memperbarui koefisien filter." },
  { "en": "Apa itu filter Kalman?", "id": "Estimator optimal untuk sistem linear dinamis." },
  { "en": "Apa itu Hidden Markov Model (HMM)?", "id": "Model statistik untuk sistem dengan state tersembunyi." },
  { "en": "Apa itu spectrogram?", "id": "Visualisasi frekuensi sinyal terhadap waktu." },
  { "en": "Apa itu cepstrum?", "id": "Hasil transformasi Fourier dari spektrum logaritmik." },
  { "en": "Tujuan analisis cepstral?", "id": "Memisahkan sinyal sumber dan filter." },
  { "en": "Apa itu MFCC (Mel-Frequency Cepstral Coefficients)?", "id": "Fitur penting untuk pengenalan suara manusia." },
  { "en": "Skala Mel?", "id": "Skala frekuensi perseptual yang meniru telinga manusia." },
  { "en": "Apa itu pitch sinyal audio?", "id": "Persepsi frekuensi fundamental dari suatu suara." },
  { "en": "Apa itu formant?", "id": "Puncak spektral dari suara vokal." },
  { "en": "Apa itu Convolutional Neural Network (CNN)?", "id": "Jaringan saraf untuk analisis data visual." },
  { "en": "Apa itu layer konvolusi?", "id": "Layer yang menerapkan filter untuk mengekstrak fitur." },
  { "en": "Apa itu feature map?", "id": "Output dari satu filter pada layer konvolusi." },
  { "en": "Apa itu pooling layer?", "id": "Layer untuk mengurangi dimensi spasial feature map." },
  { "en": "Jenis Max Pooling?", "id": "Mengambil nilai maksimum dari setiap patch." },
  { "en": "Jenis Average Pooling?", "id": "Mengambil nilai rata-rata dari setiap patch." },
  { "en": "Apa itu fungsi aktivasi?", "id": "Fungsi yang menentukan output dari sebuah neuron." },
  { "en": "Contoh fungsi aktivasi?", "id": "ReLU, Sigmoid, dan Tanh." },
  { "en": "Apa itu ReLU (Rectified Linear Unit)?", "id": "Fungsi aktivasi yang populer dalam deep learning." },
  { "en": "Apa itu fully connected layer?", "id": "Layer dimana setiap neuron terhubung ke semua neuron." },
  { "en": "Apa itu backpropagation?", "id": "Metode melatih jaringan saraf dengan menghitung gradien." },
  { "en": "Format citra JPEG?", "id": "Format kompresi lossy berbasis DCT." },
  { "en": "Format citra PNG (Portable Network Graphics)?", "id": "Format kompresi lossless dengan dukungan transparansi." },
  { "en": "Format citra GIF (Graphics Interchange Format)?", "id": "Format lossless untuk gambar animasi dan sederhana." },
  { "en": "Format citra TIFF (Tagged Image File Format)?", "id": "Format fleksibel yang populer dalam percetakan." },
  { "en": "Apa itu JPEG2000?", "id": "Standar kompresi citra berbasis wavelet transform." },
  { "en": "Apa itu MPEG (Moving Picture Experts Group)?", "id": "Standar untuk kompresi audio dan video." },
  { "en": "Apa itu I-frame (Intra-coded frame)?", "id": "Frame video yang dikodekan secara independen." },
  { "en": "Apa itu P-frame (Predicted frame)?", "id": "Frame video yang dikodekan dari frame sebelumnya." },
  { "en": "Apa itu B-frame (Bidirectional predicted frame)?", "id": "Frame yang dikodekan dari frame sebelum/sesudahnya." },
  { "en": "Apa itu HOG (Histogram of Oriented Gradients)?", "id": "Deskriptor fitur untuk deteksi objek." },
  { "en": "Apa itu LBP (Local Binary Patterns)?", "id": "Deskriptor tekstur yang efisien secara komputasi." },
  { "en": "Apa itu SIFT (Scale-Invariant Feature Transform)?", "id": "Algoritma deteksi fitur invarian terhadap skala." },
  { "en": "Apa itu SURF (Speeded Up Robust Features)?", "id": "Alternatif yang lebih cepat dari SIFT." },
  { "en": "Apa itu GLCM (Gray-Level Co-occurrence Matrix)?", "id": "Matriks untuk analisis tekstur suatu citra." },
  { "en": "Apa itu Radon Transform?", "id": "Proyeksi citra 2D ke dalam domain sinogram." },
  { "en": "Aplikasi Radon Transform?", "id": "Rekonstruksi citra pada CT (Computed Tomography) scanner." },
  { "en": "Apa itu depth map?", "id": "Citra yang berisi informasi jarak/kedalaman." },
  { "en": "Apa itu point cloud?", "id": "Kumpulan titik data dalam sistem koordinat 3D." },
  { "en": "Apa itu pencitraan medis?", "id": "Teknik memvisualisasikan bagian dalam tubuh manusia." },
  { "en": "Prinsip MRI (Magnetic Resonance Imaging)?", "id": "Menggunakan medan magnet kuat dan gelombang radio." },
  { "en": "Prinsip CT (Computed Tomography) scan?", "id": "Menggabungkan beberapa citra X-ray." },
  { "en": "Prinsip Ultrasound (USG)?", "id": "Menggunakan gelombang suara frekuensi tinggi." },
  { "en": "Prinsip PET (Positron Emission Tomography)?", "id": "Mendeteksi partikel gamma dari pelacak radioaktif." },
  { "en": "Apa itu Gabor filter?", "id": "Filter linear untuk analisis tekstur dan deteksi tepi." },
  { "en": "Apa itu Gabor Transform?", "id": "Transformasi Fourier berjendela dengan fungsi Gaussian." },
  { "en": "Apa itu homomorphic filtering?", "id": "Memperbaiki citra dengan memanipulasi komponen iluminasi." },
  { "en": "Apa itu image pyramid?", "id": "Representasi citra pada berbagai tingkat resolusi." },
  { "en": "Apa itu Gaussian pyramid?", "id": "Pyramid yang dibuat dengan filtering Gaussian." },
  { "en": "Apa itu Laplacian pyramid?", "id": "Pyramid yang menyimpan informasi perbedaan antar level." },
  { "en": "Apa itu watershed transform?", "id": "Algoritma segmentasi citra yang terinspirasi topografi." },
  { "en": "Apa itu active contours (snakes)?", "id": "Model berbasis energi untuk delineasi objek." },
  { "en": "Apa itu level-set method?", "id": "Metode numerik untuk melacak antarmuka dan bentuk." },
  { "en": "Apa itu photometric stereo?", "id": "Merekonstruksi bentuk 3D dari beberapa pencahayaan." },
  { "en": "Apa itu structure from motion (SfM)?", "id": "Rekonstruksi 3D dari urutan gambar 2D." },
  { "en": "Apa itu SLAM (Simultaneous Localization and Mapping)?", "id": "Membangun peta sambil melacak lokasi." },
  { "en": "Apa itu RANSAC (Random Sample Consensus)?", "id": "Metode iteratif untuk estimasi parameter model." },
  { "en": "Apa itu kalibrasi kamera?", "id": "Menentukan parameter intrinsik dan ekstrinsik kamera." },
  { "en": "Apa itu distorsi lensa?", "id": "Penyimpangan dari proyeksi garis lurus." },
  { "en": "Jenis distorsi radial?", "id": "Penyimpangan garis lurus dari pusat gambar." },
  { "en": "Jenis distorsi tangensial?", "id": "Lensa tidak sejajar sempurna dengan sensor." },
  { "en": "Apa itu matriks homografi?", "id": "Memetakan titik dari satu bidang ke bidang lain." },
  { "en": "Apa itu matriks esensial?", "id": "Relasi geometris antara dua citra." },
  { "en": "Apa itu matriks fundamental?", "id": "Generalisasi matriks esensial untuk kamera tidak terkalibrasi." },
  { "en": "Apa itu image retrieval?", "id": "Proses mencari citra dari database besar." },
  { "en": "Metode Content-Based Image Retrieval (CBIR)?", "id": "Pencarian citra berdasarkan konten visualnya." },
  { "en": "Apa itu Support Vector Machine (SVM)?", "id": "Model klasifikasi dengan mencari hyperplane pemisah." },
  { "en": "Apa itu Decision Tree?", "id": "Model seperti flowchart untuk pengambilan keputusan." },
  { "en": "Apa itu Random Forest?", "id": "Kumpulan decision tree untuk meningkatkan akurasi." },
  { "en": "Apa itu gradient descent?", "id": "Algoritma optimisasi untuk menemukan minimum fungsi." },
  { "en": "Apa itu learning rate?", "id": "Parameter yang mengontrol kecepatan pembelajaran." },
  { "en": "Apa itu overfitting?", "id": "Model terlalu cocok dengan data training." },
  { "en": "Apa itu underfitting?", "id": "Model terlalu sederhana, tidak bisa menangkap pola." },
  { "en": "Apa itu regularisasi?", "id": "Teknik untuk mencegah overfitting pada model." },
  { "en": "Apa itu dropout?", "id": "Teknik regularisasi dengan menonaktifkan neuron acak." },
  { "en": "Apa itu batch normalization?", "id": "Menormalkan input layer untuk menstabilkan training." },
  { "en": "Apa itu transfer learning?", "id": "Menggunakan model terlatih untuk tugas berbeda." },
  { "en": "Apa itu data augmentation?", "id": "Membuat data training baru dari data ada." },
  { "en": "Apa itu zero-crossing?", "id": "Titik dimana sinyal melewati sumbu nol." },
  { "en": "Apa itu bank filter?", "id": "Kumpulan filter band-pass untuk analisis sinyal." },
  { "en": "Apa itu polyphase decomposition?", "id": "Memecah filter menjadi beberapa sub-filter efisien." },
  { "en": "Apa itu Quadrature Mirror Filter (QMF)?", "id": "Bank filter untuk dekomposisi sinyal sempurna." },
  { "en": "Apa itu cyclostationary signal?", "id": "Sinyal dengan statistik periodik terhadap waktu." },
  { "en": "Apa itu bispectrum?", "id": "Analisis orde tinggi untuk sinyal non-Gaussian." },
  { "en": "Apa itu Independent Component Analysis (ICA)?", "id": "Memisahkan sinyal menjadi komponen statistik independen." },
  { "en": "Contoh aplikasi ICA?", "id": "Pemisahan suara dalam 'cocktail party problem'." },
  { "en": "Apa itu VQ (Vector Quantization) codebook?", "id": "Kumpulan vektor prototipe dalam kuantisasi vektor." },
  { "en": "Apa itu algoritma Linde-Buzo-Gray (LBG)?", "id": "Algoritma iteratif untuk membuat codebook VQ." },
  { "en": "Apa itu warping?", "id": "Transformasi geometris pada sebuah citra." },
  { "en": "Apa itu affine transformation?", "id": "Transformasi linear yang mempertahankan garis paralel." },
  { "en": "Apa itu projective transformation?", "id": "Transformasi yang mempertahankan garis lurus." },
  { "en": "Apa itu deteksi objek?", "id": "Melokalisasi dan mengklasifikasi objek dalam citra." },
  { "en": "Apa itu semantic segmentation?", "id": "Mengklasifikasikan setiap piksel ke dalam sebuah kelas." },
  { "en": "Apa itu instance segmentation?", "id": "Membedakan setiap instance objek dalam kelas sama." },
  { "en": "Apa itu YOLO (You Only Look Once)?", "id": "Algoritma deteksi objek real-time yang populer." },
  { "en": "Apa itu R-CNN (Region-based CNN)?", "id": "Keluarga model deteksi objek berbasis proposal." },
  { "en": "Apa itu Intersection over Union (IoU)?", "id": "Metrik untuk mengevaluasi akurasi deteksi objek." },
  { "en": "Apa itu non-maximum suppression (NMS)?", "id": "Menghapus bounding box yang tumpang tindih." },
  { "en": "Apa itu anchor boxes?", "id": "Kotak referensi dengan rasio aspek berbeda." },
  { "en": "Apa itu FCN (Fully Convolutional Network)?", "id": "Jaringan CNN tanpa fully connected layer." },
  { "en": "Aplikasi FCN?", "id": "Tugas-tugas dense prediction seperti semantic segmentation." },
  { "en": "Apa itu U-Net?", "id": "Arsitektur CNN untuk segmentasi citra biomedis." },
  { "en": "Apa itu transposed convolution?", "id": "Operasi untuk upsampling feature map (deconvolution)." },
  { "en": "Apa itu dilated convolution (atrous)?", "id": "Konvolusi dengan celah untuk memperluas receptive field." },
  { "en": "Apa itu ResNet (Residual Network)?", "id": "Arsitektur CNN yang menggunakan koneksi residual." },
  { "en": "Tujuan skip connection di ResNet?", "id": "Mengatasi masalah vanishing gradient di jaringan dalam." },
  { "en": "Apa itu Inception Module (GoogLeNet)?", "id": "Blok yang melakukan konvolusi berbagai ukuran." },
  { "en": "Apa itu VGGNet?", "id": "Arsitektur CNN dalam yang menggunakan filter 3x3." },
  { "en": "Apa itu AlexNet?", "id": "Arsitektur CNN yang memenangkan ImageNet 2012." },
  { "en": "Apa itu Generative Adversarial Network (GAN)?", "id": "Model generatif dengan dua jaringan (generator/diskriminator)." },
  { "en": "Fungsi generator pada GAN?", "id": "Menciptakan data palsu yang terlihat nyata." },
  { "en": "Fungsi diskriminator pada GAN?", "id": "Membedakan antara data asli dan palsu." },
  { "en": "Apa itu CycleGAN?", "id": "GAN untuk translasi citra tanpa data berpasangan." },
  { "en": "Apa itu Autoencoder?", "id": "Jaringan saraf untuk learning representasi data." },
  { "en": "Bagian encoder pada autoencoder?", "id": "Mengompresi input menjadi representasi latent-space." },
  { "en": "Bagian decoder pada autoencoder?", "id": "Merekonstruksi input dari representasi latent-space." },
  { "en": "Aplikasi autoencoder?", "id": "Reduksi dimensi, denoising, dan deteksi anomali." },
  { "en": "Apa itu Variational Autoencoder (VAE)?", "id": "Autoencoder generatif yang belajar distribusi probabilitas." },
  { "en": "Apa itu optimiser Adam?", "id": "Algoritma optimisasi populer untuk melatih model." },
  { "en": "Apa itu SGD (Stochastic Gradient Descent)?", "id": "Optimiser yang memperbarui bobot per sampel." },
  { "en": "Apa itu loss function?", "id": "Fungsi yang mengukur error dari sebuah model." },
  { "en": "Contoh loss untuk regresi?", "id": "Mean Squared Error (MSE)." },
  { "en": "Contoh loss untuk klasifikasi?", "id": "Cross-Entropy Loss." },
  { "en": "Apa itu sinyal EKG (Elektrokardiogram)?", "id": "Sinyal kelistrikan yang dihasilkan oleh jantung." },
  { "en": "Apa itu P wave pada EKG?", "id": "Representasi depolarisasi atrium jantung." },
  { "en": "Apa itu QRS complex pada EKG?", "id": "Representasi depolarisasi ventrikel jantung." },
  { "en": "Apa itu T wave pada EKG?", "id": "Representasi repolarisasi ventrikel jantung." },
  { "en": "Apa itu sinyal EEG (Elektroensefalogram)?", "id": "Sinyal aktivitas kelistrikan di otak." },
  { "en": "Gelombang Alpha pada EEG?", "id": "Terjadi saat rileks dengan mata tertutup." },
  { "en": "Gelombang Beta pada EEG?", "id": "Terjadi saat aktif berpikir atau konsentrasi." },
  { "en": "Apa itu sinyal EMG (Elektromiogram)?", "id": "Sinyal kelistrikan yang dihasilkan oleh otot." },
  { "en": "Apa itu Hilbert Transform?", "id": "Membuat sinyal analitik dari sinyal real." },
  { "en": "Apa itu instantaneous frequency?", "id": "Frekuensi sinyal pada satu titik waktu." },
  { "en": "Apa itu compressed sensing?", "id": "Rekonstruksi sinyal dari sampel yang lebih sedikit." },
  { "en": "Syarat compressed sensing?", "id": "Sinyal harus sparse dalam domain tertentu." },
  { "en": "Apa itu sparsity?", "id": "Sinyal memiliki banyak koefisien bernilai nol." },
  { "en": "Apa itu bilateral filter?", "id": "Filter penghalus yang mempertahankan ketajaman tepi." },
  { "en": "Apa itu non-local means?", "id": "Algoritma denoising dengan merata-ratakan patch serupa." },
  { "en": "Apa itu anisotropic diffusion?", "id": "Proses penghalusan citra yang bergantung pada arah." },
  { "en": "Apa itu optical character recognition (OCR)?", "id": "Mengenali teks di dalam gambar digital." },
  { "en": "Apa itu barcode reader?", "id": "Memindai dan menerjemahkan data dari barcode." },
  { "en": "Apa itu QR (Quick Response) code?", "id": "Barcode matriks dua dimensi." },
  { "en": "Apa itu kuantisasi uniform?", "id": "Level kuantisasi memiliki jarak yang sama." },
  { "en": "Apa itu kuantisasi non-uniform?", "id": "Level kuantisasi memiliki jarak yang bervariasi." },
  { "en": "Apa itu algoritma Lloyd-Max?", "id": " Mendesain quantizer skalar yang optimal." },
  { "en": "Apa itu sinyal radar?", "id": "Gelombang elektromagnetik untuk mendeteksi objek." },
  { "en": "Apa itu efek Doppler?", "id": "Perubahan frekuensi gelombang karena gerakan relatif." },
  { "en": "Aplikasi efek Doppler di radar?", "id": "Mengukur kecepatan target yang bergerak." },
  { "en": "Apa itu radar FMCW (Frequency-Modulated Continuous-Wave)?", "id": "Radar yang memancarkan sinyal chirp kontinu." },
  { "en": "Apa itu sinyal sonar?", "id": "Gelombang suara untuk navigasi atau deteksi." },
  { "en": "Apa itu beamforming?", "id": "Mengarahkan sinyal ke arah tertentu." },
  { "en": "Apa itu array sinyal?", "id": "Sekelompok sensor yang bekerja bersama." },
  { "en": "Apa itu motion estimation?", "id": "Memperkirakan gerakan objek dalam video." },
  { "en": "Apa itu block-matching algorithm?", "id": "Metode estimasi gerak dengan membandingkan blok." },
  { "en": "Apa itu motion vector?", "id": "Vektor yang merepresentasikan perpindahan sebuah blok." },
  { "en": "Standar kompresi video H.264/AVC?", "id": "Standar kompresi video yang sangat umum." },
  { "en": "Standar kompresi video H.265/HEVC?", "id": "Penerus H.264 dengan efisiensi lebih tinggi." },
  { "en": "Apa itu color bleeding?", "id": "Warna menyebar ke area yang berdekatan." },
  { "en": "Apa itu ringing artifact?", "id": "Osilasi palsu di sekitar tepi yang tajam." },
  { "en": "Apa itu blocking artifact?", "id": "Munculnya blok-blok pada citra terkompresi." },
  { "en": "Apa itu phase vocoder?", "id": "Algoritma untuk time-stretching dan pitch-shifting audio." },
  { "en": "Apa itu time stretching?", "id": "Mengubah durasi audio tanpa mengubah pitch." },
  { "en": "Apa itu pitch shifting?", "id": "Mengubah pitch audio tanpa mengubah durasi." },
  { "en": "Apa itu tone mapping?", "id": "Memetakan citra HDR ke perangkat LDR." },
  { "en": "Apa itu LDR (Low Dynamic Range)?", "id": "Rentang dinamis yang terbatas seperti monitor." },
  { "en": "Apa itu confusion matrix?", "id": "Tabel untuk mengevaluasi performa model klasifikasi." },
  { "en": "Definisi True Positive (TP)?", "id": "Hasil positif yang diprediksi dengan benar." },
  { "en": "Definisi True Negative (TN)?", "id": "Hasil negatif yang diprediksi dengan benar." },
  { "en": "Definisi False Positive (FP)?", "id": "Hasil negatif yang salah diprediksi positif." },
  { "en": "Definisi False Negative (FN)?", "id": "Hasil positif yang salah diprediksi negatif." },
  { "en": "Apa itu Akurasi (Accuracy)?", "id": "Fraksi prediksi yang benar dari total." },
  { "en": "Apa itu Presisi (Precision)?", "id": "Fraksi prediksi positif yang benar." },
  { "en": "Apa itu Recall (Sensitivity)?", "id": "Fraksi aktual positif yang teridentifikasi benar." },
  { "en": "Apa itu F1-Score?", "id": "Rata-rata harmonik dari presisi dan recall." },
  { "en": "Apa itu kurva ROC (Receiver Operating Characteristic)?", "id": "Grafik performa klasifikasi pada semua threshold." },
  { "en": "Apa itu AUC (Area Under the Curve)?", "id": "Ukuran kemampuan pemisahan dari suatu model." },
  { "en": "Apa itu cross-validation?", "id": "Teknik mengevaluasi model dengan membagi data." },
  { "en": "Apa itu k-fold cross-validation?", "id": "Membagi data menjadi k subset (folds)." },
  { "en": "Apa itu one-hot encoding?", "id": "Representasi variabel kategori sebagai vektor biner." },
  { "en": "Apa itu normalisasi data?", "id": "Menskalakan data ke rentang [0, 1]." },
  { "en": "Apa itu standardisasi data?", "id": "Mengubah data agar memiliki mean 0, stdev 1." },
  { "en": "Apa itu centering sinyal?", "id": "Membuat sinyal memiliki mean (rata-rata) nol." },
  { "en": "Apa itu blind source separation?", "id": "Memisahkan sinyal sumber tanpa informasi campuran." },
  { "en": "Apa itu image quilting?", "id": "Metode untuk sintesis tekstur secara non-parametrik." },
  { "en": "Apa itu seam carving?", "id": "Algoritma untuk mengubah ukuran citra (retargeting)." },
  { "en": "Apa itu matting?", "id": "Proses mengekstrak objek foreground secara presisi." },
  { "en": "Apa itu alpha channel?", "id": "Layer tambahan yang merepresentasikan tingkat transparansi." },
  { "en": "Apa itu citra hiperspektral?", "id": "Citra dengan ratusan band spektral kontinu." },
  { "en": "Apa itu citra multispektral?", "id": "Citra dengan beberapa band spektral diskrit." },
  { "en": "Apa itu spectral signature?", "id": "Reflektansi objek pada panjang gelombang berbeda." },
  { "en": "Aplikasi citra hiperspektral?", "id": "Pertanian presisi, geologi, dan pengawasan lingkungan." },
  { "en": "Apa itu unmixing spektral?", "id": "Memisahkan spektrum piksel menjadi endmember murni." },
  { "en": "Apa itu endmember?", "id": "Spektrum dari material murni dalam citra." },
  { "en": "Apa itu NDVI (Normalized Difference Vegetation Index)?", "id": "Indikator untuk mengukur kesehatan vegetasi." },
  { "en": "Apa itu light field (plenoptic)?", "id": "Representasi intensitas cahaya dari semua arah." },
  { "en": "Aplikasi light field imaging?", "id": "Refocusing citra setelah pengambilan gambar." },
  { "en": "Apa itu kamera plenoptic?", "id": "Kamera yang menangkap informasi light field." },
  { "en": "Apa itu Graph Signal Processing (GSP)?", "id": "Generalisasi DSP untuk sinyal pada graf." },
  { "en": "Apa itu graph Laplacian?", "id": "Operator yang mengukur kelancaran sinyal graf." },
  { "en": "Apa itu Graph Fourier Transform (GFT)?", "id": "Dekomposisi sinyal graf ke basis eigenvector." },
  { "en": "Apa itu Graph Convolutional Network (GCN)?", "id": "CNN yang beroperasi pada data berstruktur graf." },
  { "en": "Apa itu dictionary learning?", "id": "Mempelajari representasi basis (kamus) untuk sinyal." },
  { "en": "Apa itu K-SVD?", "id": "Algoritma untuk dictionary learning dan sparse coding." },
  { "en": "Apa itu sparse coding?", "id": "Merepresentasikan sinyal sebagai kombinasi linear jarang." },
  { "en": "Apa itu skeletonization?", "id": "Mengurangi bentuk biner menjadi kerangka 1-piksel." },
  { "en": "Apa itu thinning?", "id": "Operasi morfologi untuk erosi terkondisi." },
  { "en": "Apa itu thickening?", "id": "Operasi morfologi untuk dilasi terkondisi." },
  { "en": "Apa itu hit-or-miss transform?", "id": "Mendeteksi konfigurasi piksel tertentu dalam citra." },
  { "en": "Apa itu granulometry?", "id": "Analisis distribusi ukuran partikel dalam citra." },
  { "en": "Apa itu distance transform?", "id": "Menghitung jarak setiap piksel ke objek terdekat." },
  { "en": "Apa itu geodesic distance?", "id": "Jarak terpendek antara dua titik dalam bentuk." },
  { "en": "Apa itu computational photography?", "id": "Menggabungkan komputasi untuk meningkatkan hasil fotografi." },
  { "en": "Apa itu exposure fusion?", "id": "Menggabungkan beberapa eksposur tanpa merekonstruksi HDR." },
  { "en": "Apa itu focus stacking?", "id": "Menggabungkan beberapa fokus untuk depth of field luas." },
  { "en": "Apa itu coded aperture?", "id": "Menggunakan pola buram untuk menangkap informasi tambahan." },
  { "en": "Apa itu sinyal ECG ambulatory?", "id": "Perekaman EKG jangka panjang (Holter monitor)." },
  { "en": "Apa itu Heart Rate Variability (HRV)?", "id": "Variasi interval waktu antara detak jantung." },
  { "en": "Apa itu ballistocardiography (BCG)?", "id": "Mengukur gerakan tubuh akibat ejeksi darah." },
  { "en": "Apa itu photoplethysmography (PPG)?", "id": "Mengukur perubahan volume darah secara optik." },
  { "en": "Aplikasi sinyal PPG?", "id": "Monitor detak jantung pada smartwatch." },
  { "en": "Apa itu automatic speech recognition (ASR)?", "id": "Mengubah ucapan lisan menjadi teks tertulis." },
  { "en": "Apa itu text-to-speech (TTS)?", "id": "Mensintesis ucapan buatan dari teks." },
  { "en": "Apa itu fonem (phoneme)?", "id": "Unit perseptual terkecil dari suara." },
  { "en": "Apa itu viseme?", "id": "Gerakan visual wajah yang merepresentasikan fonem." },
  { "en": "Apa itu Recurrent Neural Network (RNN)?", "id": "Jaringan saraf untuk memproses data sekuensial." },
  { "en": "Apa itu LSTM (Long Short-Term Memory)?", "id": "Jenis RNN untuk mengatasi vanishing gradient." },
  { "en": "Apa itu GRU (Gated Recurrent Unit)?", "id": "Varian LSTM yang lebih sederhana." },
  { "en": "Apa itu attention mechanism?", "id": "Memungkinkan model fokus pada bagian input relevan." },
  { "en": "Apa itu Transformer model?", "id": "Arsitektur berbasis attention, tanpa rekurensi." },
  { "en": "Apa itu entropy dalam informasi?", "id": "Ukuran ketidakpastian atau informasi dalam sinyal." },
  { "en": "Apa itu mutual information?", "id": "Ukuran ketergantungan statistik antara dua variabel." },
  { "en": "Apa itu Kullback-Leibler (KL) divergence?", "id": "Ukuran perbedaan antara dua distribusi probabilitas." },
  { "en": "Apa itu channel capacity?", "id": "Laju data maksimum melalui kanal komunikasi." },
  { "en": "Apa itu source coding?", "id": "Mengompresi data dari sebuah sumber (kompresi)." },
  { "en": "Apa itu channel coding?", "id": "Menambahkan redundansi untuk deteksi/koreksi error." },
  { "en": "Apa itu Hamming distance?", "id": "Jumlah posisi bit yang berbeda." },
  { "en": "Apa itu Cyclic Redundancy Check (CRC)?", "id": "Kode pendeteksi error yang umum digunakan." },
  { "en": "Apa itu convolutional code?", "id": "Jenis kode koreksi error (error-correcting code)." },
  { "en": "Apa itu Viterbi algorithm?", "id": "Algoritma untuk decoding convolutional codes." },
  { "en": "Apa itu Turbo codes?", "id": "Kode koreksi error berkinerja tinggi." },
  { "en": "Apa itu LDPC (Low-Density Parity-Check) codes?", "id": "Kode koreksi error mendekati batas Shannon." },
  { "en": "Apa itu polarimetric imaging?", "id": "Pencitraan yang menangkap informasi polarisasi cahaya." },
  { "en": "Apa itu SAR (Synthetic-Aperture Radar)?", "id": "Teknik radar untuk menghasilkan citra resolusi tinggi." },
  { "en": "Apa itu InSAR (Interferometric SAR)?", "id": "Menggunakan dua citra SAR untuk membuat peta elevasi." },
  { "en": "Apa itu GPR (Ground-Penetrating Radar)?", "id":"Teknik geofisika untuk pencitraan bawah permukaan." },
  { "en": "Apa itu tomografi?", "id": "Pencitraan berdasarkan irisan dari gelombang." },
  { "en": "Apa itu back-projection?", "id": "Algoritma rekonstruksi dalam tomografi." },
  { "en": "Apa itu foveated rendering?", "id": "Teknik rendering dengan detail di pusat pandangan." },
  { "en": "Apa itu blind deconvolution?", "id": "Dekonvolusi saat Point Spread Function tidak diketahui." },
  { "en": "Apa itu manifold learning?", "id": "Teknik reduksi dimensi non-linear." },
  { "en": "Contoh manifold learning?", "id": "t-SNE (t-Distributed Stochastic Neighbor Embedding)." },
  { "en": "Apa itu Eigenface?", "id": "Pendekatan berbasis PCA untuk pengenalan wajah." },
  { "en": "Apa itu Fisherface?", "id": "Pendekatan berbasis LDA untuk pengenalan wajah." },
  { "en": "Apa itu LDA (Linear Discriminant Analysis)?", "id": "Teknik reduksi dimensi untuk memaksimalkan keterpisahan kelas." },
  { "en": "Apa itu Canonical Correlation Analysis (CCA)?", "id": "Menemukan hubungan antara dua set variabel." },
  { "en": "Apa itu sinyal cyclo-stationary?", "id": "Sinyal dengan statistik yang periodik." },
  { "en": "Apa itu Wigner-Ville distribution?", "id": "Representasi waktu-frekuensi sinyal resolusi tinggi." },
  { "en": "Apa itu fractional Fourier transform?", "id": "Generalisasi dari transformasi Fourier." },
  { "en": "Apa itu sampling acak (random sampling)?", "id": "Mengambil sampel pada interval waktu acak." },
  { "en": "Apa itu sampling non-uniform?", "id": "Interval sampling tidak konstan." },
  { "en": "Apa itu Perfect Reconstruction Filter Bank?", "id": "Memungkinkan sinyal dipecah dan digabung kembali." },
  { "en": "Apa itu Lifting Scheme?", "id": "Metode efisien untuk membangun wavelet transform." },
  { "en": "Apa itu ridgelet transform?", "id": "Transformasi untuk merepresentasikan objek dengan singularitas garis." },
  { "en": "Apa itu curvelet transform?", "id": "Transformasi multiskala untuk representasi kurva." },
  { "en": "Apa itu shearlet transform?", "id": "Transformasi yang efisien menangkap anisotropi." },
  { "en": "Apa itu Empirical Mode Decomposition (EMD)?", "id": "Memecah sinyal menjadi Intrinsic Mode Functions." },
  { "en": "Apa itu Intrinsic Mode Function (IMF)?", "id": "Fungsi yang merepresentasikan osilasi sederhana." },
  { "en": "Apa itu Hilbert-Huang Transform (HHT)?", "id": "Kombinasi EMD dan Hilbert spectral analysis." },
  { "en": "Apa itu phase congruency?", "id": "Metode deteksi fitur yang invarian kontras." },
  { "en": "Apa itu monogenic signal?", "id": "Generalisasi sinyal analitik untuk dimensi lebih tinggi." },
  { "en": "Apa itu color transfer?", "id": "Menerapkan palet warna satu citra ke citra lain." },
  { "en": "Apa itu demosaicing?", "id": "Merekonstruksi citra warna penuh dari sensor Bayer." },
  { "en": "Apa itu filter Bayer?", "id": "Pola filter warna pada sensor citra digital." },
  { "en": "Apa itu chromatic aberration?", "id": "Lensa gagal memfokuskan semua warna ke titik sama." },
  { "en": "Apa itu vignetting?", "id": "Penggelapan di sudut-sudut citra." },
  { "en": "Apa itu lens flare?", "id": "Artefak cahaya akibat pantulan dalam lensa." },
  { "en": "Apa itu bokeh?", "id": "Kualitas estetika blur pada bagian out-of-focus." },
  { "en": "Apa itu panning (kamera)?", "id": "Gerakan kamera horizontal pada sumbu vertikal." },
  { "en": "Apa itu tilting (kamera)?", "id": "Gerakan kamera vertikal pada sumbu horizontal." },
  { "en": "Apa itu rolling shutter?", "id": "Sensor merekam citra baris per baris." },
  { "en": "Apa itu global shutter?", "id": "Sensor merekam seluruh citra secara bersamaan." },
  { "en": "Apa itu forensik citra?", "id": "Analisis citra untuk pengumpulan bukti digital." },
  { "en": "Apa itu deteksi copy-move?", "id": "Menemukan bagian citra yang disalin & ditempel." },
  { "en": "Apa itu deteksi splicing?", "id": "Mendeteksi penggabungan dua citra yang berbeda." },
  { "en": "Apa itu image tampering?", "id": "Manipulasi citra digital dengan niat jahat." },
  { "en": "Apa itu Photo Response Non-Uniformity (PRNU)?", "id": "Noise pola tetap unik untuk setiap sensor kamera." },
  { "en": "Fungsi PRNU dalam forensik?", "id": "Mengidentifikasi kamera sumber dari sebuah citra." },
  { "en": "Apa itu steganalysis?", "id": "Ilmu mendeteksi adanya pesan tersembunyi." },
  { "en": "Apa itu watermark rapuh (fragile)?", "id": "Watermark yang rusak jika citra dimodifikasi." },
  { "en": "Apa itu watermark kokoh (robust)?", "id": "Watermark yang bertahan terhadap modifikasi citra." },
  { "en": "Apa itu perceptual hashing?", "id": "Membuat 'sidik jari' unik untuk konten visual." },
  { "en": "Apa itu LIDAR (Light Detection and Ranging)?", "id": "Penginderaan jauh aktif menggunakan pulsa laser." },
  { "en": "Aplikasi data LIDAR?", "id": "Pemetaan topografi, kehutanan, dan mobil otonom." },
  { "en": "Apa itu pan-sharpening?", "id": "Menggabungkan citra pankromatik dan multispektral." },
  { "en": "Tujuan pan-sharpening?", "id": "Meningkatkan resolusi spasial citra multispektral." },
  { "en": "Apa itu koreksi atmosferik?", "id": "Menghilangkan efek distorsi atmosfer dari citra." },
  { "en": "Apa itu albedo permukaan?", "id": "Ukuran daya reflektif dari permukaan bumi." },
  { "en": "Apa itu BRDF (Bidirectional Reflectance Distribution Function)?", "id": "Fungsi yang mendeskripsikan bagaimana cahaya dipantulkan." },
  { "en": "Apa itu orthorectification?", "id": "Mengoreksi distorsi geometris dari citra satelit." },
  { "en": "Apa itu Quantum Image Processing (QIP)?", "id": "Pemrosesan citra menggunakan prinsip mekanika kuantum." },
  { "en": "Apa itu qubit?", "id": "Unit dasar informasi kuantum." },
  { "en": "Apa itu superposisi kuantum?", "id": "Kemampuan qubit berada di banyak state bersamaan." },
  { "en": "Apa itu FRQI (Flexible Representation of Quantum Images)?", "id": "Metode untuk merepresentasikan citra pada komputer kuantum." },
  { "en": "Apa itu NEQR (Novel Enhanced Quantum Representation)?", "id": "Representasi kuantum lain untuk citra digital." },
  { "en": "Apa itu Just Noticeable Difference (JND)?", "id": "Perubahan stimulus terkecil yang dapat dirasakan." },
  { "en": "Apa itu model psychovisual?", "id": "Model matematis dari persepsi visual manusia." },
  { "en": "Apa itu masking visual?", "id": "Satu stimulus visual dibuat tidak terlihat." },
  { "en": "Apa itu fovea?", "id": "Bagian tengah retina untuk penglihatan tajam." },
  { "en": "Apa itu penglihatan perifer?", "id": "Penglihatan yang terjadi di luar pusat tatapan." },
  { "en": "Apa itu saccade?", "id": "Gerakan mata cepat antara titik fiksasi." },
  { "en": "Apa itu saliency map?", "id": "Peta yang menyoroti wilayah paling menonjol." },
  { "en": "Apa itu F0 estimation?", "id": "Memperkirakan frekuensi pitch dasar dalam audio." },
  { "en": "Apa itu chroma features?", "id": "Fitur audio yang merepresentasikan 12 kelas pitch." },
  { "en": "Apa itu onset detection?", "id": "Mendeteksi awal dari sebuah event musikal." },
  { "en": "Apa itu beat tracking?", "id": "Mengidentifikasi ketukan dalam sinyal musik." },
  { "en": "Apa itu time-domain reflectometry (TDR)?", "id": "Teknik mengukur refleksi sinyal untuk menemukan gangguan." },
  { "en": "Apa itu cepstral mean normalization (CMN)?", "id": "Mengurangi efek distorsi kanal dalam speech." },
  { "en": "Apa itu voice activity detection (VAD)?", "id": "Membedakan antara segmen suara dan senyap." },
  { "en": "Apa itu speaker diarization?", "id": "Menentukan 'siapa berbicara kapan' dalam audio." },
  { "en": "Apa itu music source separation?", "id": "Memisahkan vokal dan instrumen dari lagu." },
  { "en": "Apa itu Siamese Network?", "id": "Jaringan dengan dua cabang identik." },
  { "en": "Fungsi Siamese Network?", "id": "Mempelajari fungsi kemiripan antara dua input." },
  { "en": "Apa itu Triplet Loss?", "id": "Loss function untuk memaksimalkan jarak antar kelas." },
  { "en": "Struktur Triplet Loss?", "id": "Menggunakan anchor, positive (sama), dan negative (beda)." },
  { "en": "Apa itu one-shot learning?", "id": "Belajar mengenali objek dari satu contoh." },
  { "en": "Apa itu zero-shot learning?", "id": "Mengenali objek yang belum pernah dilihat." },
  { "en": "Apa itu few-shot learning?", "id": "Belajar dari jumlah sampel yang sangat sedikit." },
  { "en": "Apa itu Capsule Network (CapsNet)?", "id": "Jaringan saraf yang memodelkan hubungan spasial." },
  { "en": "Apa itu dynamic routing?", "id": "Proses komunikasi antar kapsul dalam CapsNet." },
  { "en": "Apa itu Equivariance?", "id": "Output berubah secara prediktif saat input berubah." },
  { "en": "Apa itu Invariance?", "id": "Output tidak berubah saat input berubah." },
  { "en": "Apa itu Spiking Neural Network (SNN)?", "id": "Jaringan saraf yang meniru neuron biologis." },
  { "en": "Apa itu sinyal seismik?", "id": "Gelombang energi yang berjalan melalui bumi." },
  { "en": "Apa itu deghosting (seismik)?", "id": "Menghilangkan refleksi 'hantu' dari permukaan air." },
  { "en": "Apa itu migrasi seismik?", "id": "Memfokuskan data seismik ke posisi geologis benar." },
  { "en": "Apa itu packet loss concealment (PLC)?", "id": "Teknik menutupi efek paket data yang hilang." },
  { "en": "Apa itu Quality of Service (QoS)?", "id": "Ukuran performa layanan jaringan." },
  { "en": "Apa itu jitter?", "id": "Variasi dalam penundaan paket data." },
  { "en": "Apa itu RTP (Real-time Transport Protocol)?", "id": "Protokol standar untuk streaming media." },
  { "en": "Apa itu RTCP (RTP Control Protocol)?", "id": "Menyediakan statistik dan kontrol untuk sesi RTP." },
  { "en": "Apa itu DASH (Dynamic Adaptive Streaming over HTTP)?", "id": "Streaming video dengan bitrate adaptif." },
  { "en": "Apa itu re-sampling?", "id": "Mengubah laju sampling dari sinyal diskrit." },
  { "en": "Apa itu oversampling?", "id": "Sampling pada laju jauh di atas Nyquist." },
  { "en": "Apa itu noise shaping?", "id": "Memindahkan noise kuantisasi ke frekuensi lain." },
  { "en": "Apa itu Sigma-Delta modulator?", "id": "Komponen kunci dalam ADC/DAC modern." },
  { "en": "Apa itu Nyquist plot?", "id": "Plot respons frekuensi dalam koordinat polar." },
  { "en": "Apa itu Bode plot?", "id": "Kombinasi plot magnitudo dan fasa." },
  { "en": "Apa itu Nichols plot?", "id": "Plot gain versus fasa pada koordinat Kartesius." },
  { "en": "Apa itu root locus analysis?", "id": "Metode grafis untuk menganalisis stabilitas sistem." },
  { "en": "Apa itu State-Space Representation?", "id": "Model sistem LTI menggunakan matriks state." },
  { "en": "Apa itu observability?", "id": "Kemampuan mengamati state internal dari output." },
  { "en": "Apa itu controllability?", "id": "Kemampuan mengarahkan state sistem dengan input." },
  { "en": "Apa itu Lyapunov stability?", "id": "Konsep stabilitas untuk sistem dinamis." },
  { "en": "Apa itu Gibbs sampling?", "id": "Algoritma MCMC untuk mengambil sampel probabilitas." },
  { "en": "Apa itu Metropolis-Hastings algorithm?", "id": "Algoritma MCMC lain untuk sampling probabilitas." },
  { "en": "Apa itu Bayesian inference?", "id": "Metode inferensi statistik berbasis teorema Bayes." },
  { "en": "Apa itu prior probability?", "id": "Keyakinan awal sebelum bukti baru diobservasi." },
  { "en": "Apa itu posterior probability?", "id": "Keyakinan yang diperbarui setelah observasi." },
  { "en": "Apa itu likelihood function?", "id": "Probabilitas data yang diamati untuk parameter." },
  { "en": "Apa itu Markov Chain Monte Carlo (MCMC)?", "id": "Kelas algoritma untuk sampling dari distribusi." },
  { "en": "Apa itu Poisson noise (shot noise)?", "id": "Noise yang mengikuti distribusi Poisson." },
  { "en": "Aplikasi Poisson noise?", "id": "Umum pada citra medis dan astronomi." },
  { "en": "Apa itu Maximum Likelihood Estimation (MLE)?", "id": "Metode estimasi parameter model statistik." },
  { "en": "Apa itu Maximum a Posteriori (MAP) estimation?", "id": "Estimasi MLE yang menggabungkan distribusi prior." },
  { "en": "Apa itu Expectation-Maximization (EM) algorithm?", "id": "Algoritma iteratif mencari estimasi MLE." },
  { "en": "Apa itu Procrustes analysis?", "id": "Teknik statistik untuk menganalisis bentuk." },
  { "en": "Apa itu Active Shape Model (ASM)?", "id": "Model statistik bentuk objek untuk segmentasi." },
  { "en": "Apa itu Active Appearance Model (AAM)?", "id": "Gabungan model bentuk dan tekstur objek." },
  { "en": "Apa itu image hashing?", "id": "Membuat nilai hash unik untuk sebuah citra." },
  { "en": "Perbedaan image hash & cryptographic hash?", "id": "Image hash serupa untuk citra yang mirip." },
  { "en": "Apa itu denoising autoencoder?", "id": "Autoencoder dilatih untuk merekonstruksi input bersih." },
  { "en": "Apa itu variational inference?", "id": "Metode alternatif untuk inferensi Bayesian." },
  { "en": "Apa itu message passing?", "id": "Proses pertukaran informasi dalam model grafis." },
  { "en": "Apa itu Belief Propagation?", "id": "Algoritma message passing pada model grafis." },
  { "en": "Apa itu Conditional Random Field (CRF)?", "id": "Model statistik untuk segmentasi dan pelabelan." },
  { "en": "Apa itu Explainable AI (XAI)?", "id": "Metode untuk membuat keputusan AI dapat dipahami." },
  { "en": "Apa itu Grad-CAM?", "id": "Visualisasi area citra yang mengaktivasi neuron CNN." },
  { "en": "Apa itu LIME (Local Interpretable Model-agnostic Explanations)?", "id": "Menjelaskan prediksi model dengan aproksimasi lokal." },
  { "en": "Apa itu SHAP (SHapley Additive exPlanations)?", "id": "Menjelaskan prediksi menggunakan nilai Shapley dari teori game." },
  { "en": "Apa itu serangan adversarial?", "id": "Input yang dimodifikasi sedikit untuk menipu model." },
  { "en": "Apa itu FGSM (Fast Gradient Sign Method)?", "id": "Metode satu langkah untuk menghasilkan contoh adversarial." },
  { "en": "Apa itu PGD (Projected Gradient Descent) attack?", "id": "Serangan adversarial iteratif yang lebih kuat." },
  { "en": "Apa itu adversarial training?", "id": "Melatih model dengan contoh adversarial." },
  { "en": "Apa itu defensive distillation?", "id": "Teknik untuk membuat model lebih robust." },
  { "en": "Apa itu tensor?", "id": "Generalisasi multidimensi dari vektor dan matriks." },
  { "en": "Apa itu tensor decomposition?", "id": "Memfaktorkan tensor menjadi komponen yang lebih sederhana." },
  { "en": "Apa itu CP/PARAFAC decomposition?", "id": "Dekomposisi tensor menjadi jumlah komponen rank-satu." },
  { "en": "Apa itu Tucker decomposition?", "id": "Dekomposisi tensor menjadi core tensor dan matriks faktor." },
  { "en": "Apa itu Tensor Train (TT) decomposition?", "id": "Representasi tensor sebagai produk matriks." },
  { "en": "Aplikasi tensor decomposition?", "id": "Analisis data multi-modal, kompresi, dan denoising." },
  { "en": "Apa itu Topological Data Analysis (TDA)?", "id": "Menganalisis bentuk data menggunakan konsep topologi." },
  { "en": "Apa itu persistent homology?", "id": "Metode TDA untuk mengukur fitur topologis." },
  { "en": "Apa itu Betti numbers?", "id": "Invarian topologis (jumlah komponen, lubang, rongga)." },
  { "en": "Apa itu barcode (TDA)?", "id": "Visualisasi dari persistent homology suatu data." },
  { "en": "Apa itu biometrik?", "id": "Pengenalan individu berdasarkan karakteristik unik." },
  { "en": "Apa itu minutiae sidik jari?", "id": "Fitur-fitur kecil seperti ujung punggungan dan bifurkasi." },
  { "en": "Apa itu pengenalan iris?", "id": "Menggunakan pola unik pada iris mata." },
  { "en": "Apa itu gait recognition?", "id": "Mengenali seseorang dari cara mereka berjalan." },
  { "en": "Apa itu keystroke dynamics?", "id": "Menganalisis ritme mengetik seseorang." },
  { "en": "Apa itu video surveillance?", "id": "Pemantauan aktivitas menggunakan kamera video." },
  { "en": "Apa itu re-identification (Re-ID)?", "id": "Mengenali kembali individu di berbagai kamera." },
  { "en": "Apa itu financial signal processing?", "id": "Aplikasi DSP pada data pasar keuangan." },
  { "en": "Apa itu time series data?", "id": "Urutan titik data yang diindeks waktu." },
  { "en": "Apa itu moving average (MA)?", "id": "Indikator tren dengan merata-ratakan data." },
  { "en": "Apa itu exponential moving average (EMA)?", "id": "Memberi bobot lebih pada data yang lebih baru." },
  { "en": "Apa itu MACD (Moving Average Convergence Divergence)?", "id": "Indikator momentum yang mengikuti tren." },
  { "en": "Apa itu RSI (Relative Strength Index)?", "id": "Osilator momentum yang mengukur kecepatan perubahan harga." },
  { "en": "Apa itu sinyal alpha (keuangan)?", "id": "Sinyal yang memprediksi pengembalian aset masa depan." },
  { "en": "Apa itu high-frequency trading (HFT)?", "id": "Perdagangan algoritmik dengan kecepatan eksekusi tinggi." },
  { "en": "Apa itu backtesting?", "id": "Menguji strategi trading pada data historis." },
  { "en": "Apa itu Physics-Informed Neural Network (PINN)?", "id": "Jaringan saraf yang mematuhi hukum fisika." },
  { "en": "Fungsi PINN?", "id": "Menyelesaikan persamaan diferensial parsial (PDE)." },
  { "en": "Apa itu Neural Radiance Fields (NeRF)?", "id": "Metode untuk mensintesis pandangan baru 3D." },
  { "en": "Apa itu Gaussian Splatting?", "id": "Teknik rendering real-time untuk scene 3D." },
  { "en": "Apa itu video coding 3D (3DVC)?", "id": "Standar kompresi untuk video stereoskopik." },
  { "en": "Apa itu Multiview Video Coding (MVC)?", "id": "Standar untuk video dari berbagai sudut pandang." },
  { "en": "Apa itu VMAF (Video Multimethod Assessment Fusion)?", "id": "Metrik kualitas video perseptual dari Netflix." },
  { "en": "Apa itu Digital Imaging and Communications in Medicine (DICOM)?", "id": "Standar untuk citra medis dan informasi terkait." },
  { "en": "Apa itu PACS (Picture Archiving and Communication System)?", "id": "Sistem penyimpanan dan pengambilan citra medis." },
  { "en": "Apa itu elastography?", "id": "Metode pencitraan untuk memetakan elastisitas jaringan." },
  { "en": "Apa itu functional MRI (fMRI)?", "id": "Mengukur aktivitas otak dengan mendeteksi perubahan aliran darah." },
  { "en": "Apa itu Diffusion Tensor Imaging (DTI)?", "id": "Teknik MRI untuk memetakan serabut saraf." },
  { "en": "Apa itu C-scan pada ultrasound?", "id": "Citra 2D penampang pada kedalaman tertentu." },
  { "en": "Apa itu B-mode pada ultrasound?", "id": "Mode standar untuk citra ultrasound 2D." },
  { "en": "Apa itu speckle tracking?", "id": "Teknik untuk memperkirakan gerakan dalam citra ultrasound." },
  { "en": "Apa itu eddy current testing?", "id": "Metode NDT menggunakan induksi elektromagnetik." },
  { "en": "Apa itu sinyal audio spasial?", "id": "Audio yang memberikan sensasi ruang 3D." },
  { "en": "Apa itu Head-Related Transfer Function (HRTF)?", "id": "Fungsi yang mendeskripsikan bagaimana telinga menerima suara." },
  { "en": "Apa itu Ambisonics?", "id": "Format audio full-sphere." },
  { "en": "Apa itu reverberation?", "id": "Kumpulan gema setelah suara asli berhenti." },
  { "en": "Apa itu acoustic echo cancellation (AEC)?", "id": "Menghilangkan gema dalam komunikasi full-duplex." },
  { "en": "Apa itu blind signal separation (BSS)?", "id": "Memisahkan sinyal sumber dari sinyal campuran." },
  { "en": "Apa itu 'cocktail party problem'?", "id": "Masalah memisahkan satu suara dari keramaian." },
  { "en": "Apa itu generative modeling?", "id": "Mempelajari distribusi data untuk menghasilkan sampel baru." },
  { "en": "Apa itu diffusion model?", "id": "Model generatif yang secara bertahap menghilangkan noise." },
  { "en": "Apa itu denoising diffusion probabilistic models (DDPM)?", "id": "Jenis populer dari model difusi." },
  { "en": "Apa itu score-based generative modeling?", "id": "Model generatif terkait dengan model difusi." },
  { "en": "Apa itu normalizing flow?", "id": "Model generatif yang menggunakan transformasi invertible." },
  { "en": "Apa itu meta-learning?", "id": "Belajar cara belajar (learning to learn)." },
  { "en": "Apa itu continual learning (lifelong learning)?", "id": "Belajar secara berurutan tanpa melupakan tugas lama." },
  { "en": "Apa itu catastrophic forgetting?", "id": "Kecenderungan AI melupakan tugas yang dipelajari sebelumnya." },
  { "en": "Apa itu federated learning?", "id": "Melatih model di banyak perangkat terdesentralisasi." },
  { "en": "Apa itu self-supervised learning?", "id": "Belajar dari data tanpa label buatan manusia." },
  { "en": "Contoh self-supervised learning?", "id": "Model bahasa memprediksi kata berikutnya." },
  { "en": "Apa itu contrastive learning?", "id": "Belajar dengan menarik sampel serupa, mendorong sampel berbeda." },
  { "en": "Apa itu domain adaptation?", "id": "Mengadaptasi model dari domain sumber ke target." },
  { "en": "Apa itu domain generalization?", "id": "Melatih model yang bekerja di domain tak terlihat." },
  { "en": "Apa itu zero-padding (CNN)?", "id": "Menambahkan nol di sekitar batas input." },
  { "en": "Apa itu stride (CNN)?", "id": "Langkah pergerakan filter di atas input." },
  { "en": "Apa itu receptive field?", "id": "Wilayah input yang mempengaruhi neuron tertentu." },
  { "en": "Apa itu knowledge distillation?", "id": "Mentransfer pengetahuan dari model besar ke kecil." },
  { "en": "Apa itu model pruning?", "id": "Menghapus bobot yang tidak signifikan dari model." },
  { "en": "Apa itu model quantization?", "id": "Mengurangi presisi numerik bobot model." },
  { "en": "Apa itu hyperparameter tuning?", "id": "Proses mencari hyperparameter optimal untuk model." },
  { "en": "Apa itu grid search?", "id": "Mencari hyperparameter dengan mencoba semua kombinasi." },
  { "en": "Apa itu random search?", "id": "Mencari hyperparameter dengan mencoba kombinasi acak." },
  { "en": "Apa itu Bayesian optimization?", "id": "Metode optimisasi untuk menemukan hyperparameter." },
  { "en": "Apa itu manifold?", "id": "Ruang topologis yang secara lokal menyerupai Euclidean." },
  { "en": "Apa itu geometric deep learning?", "id": "Generalisasi DL untuk data non-Euclidean." },
  { "en": "Apa itu Lie group?", "id": "Grup yang juga merupakan manifold diferensiabel." },
  { "en": "Apa itu Fourier analysis on groups?", "id": "Generalisasi analisis Fourier untuk grup." },
  { "en": "Apa itu signal processing on graphs?", "id": "Sinonim untuk Graph Signal Processing (GSP)." },
  { "en": "Apa itu spectral graph theory?", "id": "Studi properti graf melalui matriksnya." },
  { "en": "Apa itu graph drawing?", "id": "Visualisasi graf untuk tujuan estetika/informatif." },
  { "en": "Apa itu community detection?", "id": "Menemukan cluster atau komunitas dalam graf." },
  { "en": "Apa itu PageRank?", "id": "Algoritma untuk mengukur pentingnya simpul graf." },
  { "en": "Apa itu sinc filter?", "id": "Filter low-pass ideal dalam domain frekuensi." },
  { "en": "Masalah sinc filter?", "id": "Tidak kausal dan memiliki respon impuls tak-terbatas." },
  { "en": "Apa itu raised-cosine filter?", "id": "Filter untuk meminimalkan interferensi antar-simbol." },
  { "en": "Apa itu intersymbol interference (ISI)?", "id": "Distorsi sinyal dimana satu simbol mengganggu simbol berikutnya." },
  { "en": "Apa itu Vision Transformer (ViT)?", "id": "Arsitektur berbasis Transformer untuk tugas Computer Vision." },
  { "en": "Cara kerja ViT?", "id": "Membagi citra menjadi patch dan memprosesnya." },
  { "en": "Apa itu self-attention?", "id": "Mekanisme yang memungkinkan input berinteraksi satu sama lain." },
  { "en": "Apa itu multi-head attention?", "id": "Menjalankan mekanisme attention secara paralel." },
  { "en": "Apa itu positional embedding?", "id": "Menambahkan informasi posisi ke dalam input sekuensial." },
  { "en": "Apa itu Mixture of Experts (MoE)?", "id": "Model yang mengaktifkan sebagian jaringan ('expert')." },
  { "en": "Keuntungan model MoE?", "id": "Kapasitas model besar dengan biaya komputasi rendah." },
  { "en": "Apa itu SimCLR?", "id": "Framework simple untuk contrastive learning visual." },
  { "en": "Apa itu MoCo (Momentum Contrast)?", "id": "Contrastive learning menggunakan dictionary dan momentum encoder." },
  { "en": "Apa itu BYOL (Bootstrap Your Own Latent)?", "id": "Pendekatan self-supervised tanpa sampel negatif." },
  { "en": "Apa itu Deepfake?", "id": "Media sintetis dimana seseorang digantikan orang lain." },
  { "en": "Teknologi di balik Deepfake?", "id": "Biasanya menggunakan autoencoder atau GAN." },
  { "en": "Apa itu style transfer?", "id": "Menerapkan gaya artistik satu citra ke citra lain." },
  { "en": "Apa itu AdaIN (Adaptive Instance Normalization)?", "id": "Layer kunci dalam algoritma style transfer." },
  { "en": "Apa itu VSLAM (Visual SLAM)?", "id": "SLAM yang hanya menggunakan input kamera." },
  { "en": "Apa itu ORB-SLAM?", "id": "Sistem SLAM real-time berbasis fitur ORB." },
  { "en": "Apa itu visual odometry?", "id": "Memperkirakan posisi dengan menganalisis frame kamera." },
  { "en": "Apa itu fusi sensor?", "id": "Menggabungkan data dari beberapa sensor berbeda." },
  { "en": "Contoh fusi sensor?", "id": "Menggabungkan data kamera dan IMU (Inertial Measurement Unit)." },
  { "en": "Apa itu masalah invers (inverse problem)?", "id": "Menentukan sebab dari hasil yang diobservasi." },
  { "en": "Apa itu ill-posed problem?", "id": "Masalah invers yang solusinya tidak unik/stabil." },
  { "en": "Apa itu regularisasi?", "id": "Teknik untuk mengatasi masalah ill-posed." },
  { "en": "Apa itu Tikhonov regularization (ridge regression)?", "id": "Regularisasi L2 yang umum digunakan." },
  { "en": "Apa itu LASSO (Least Absolute Shrinkage and Selection Operator)?", "id": "Regularisasi L1 yang mendorong sparsity." },
  { "en": "Apa itu Elastic Net?", "id": "Kombinasi dari regularisasi L1 dan L2." },
  { "en": "Apa itu diagram mata (eye diagram)?", "id": "Visualisasi kualitas sinyal komunikasi digital." },
  { "en": "Apa itu 'eye opening'?", "id": "Area terbuka di tengah diagram mata." },
  { "en": "Makna eye opening lebar?", "id": "Sinyal berkualitas baik dengan noise rendah." },
  { "en": "Apa itu diagram konstelasi?", "id": "Plot sinyal modulasi pada bidang kompleks." },
  { "en": "Apa itu QAM (Quadrature Amplitude Modulation)?", "id": "Modulasi digital menggunakan amplitudo dan fasa." },
  { "en": "Apa itu PSK (Phase-Shift Keying)?", "id": "Modulasi digital menggunakan fasa sinyal." },
  { "en": "Apa itu channel equalization?", "id": "Memperbaiki distorsi yang disebabkan oleh kanal." },
  { "en": "Apa itu Zero-Forcing Equalizer?", "id": "Equalizer yang mencoba membalik respons kanal." },
  { "en": "Apa itu MMSE (Minimum Mean Square Error) Equalizer?", "id": "Meminimalkan error kuadrat rata-rata." },
  { "en": "Apa itu decision feedback equalizer (DFE)?", "id": "Menggunakan keputusan sebelumnya untuk menghilangkan ISI." },
  { "en": "Apa itu fixed-point arithmetic?", "id": "Representasi angka dengan jumlah desimal tetap." },
  { "en": "Apa itu floating-point arithmetic?", "id": "Representasi angka menggunakan mantissa dan eksponen." },
  { "en": "Keuntungan fixed-point?", "id": "Lebih cepat dan efisien daya pada hardware." },
  { "en": "Kekurangan fixed-point?", "id": "Rentang dinamis terbatas, potensi overflow." },
  { "en": "Apa itu ASIC (Application-Specific Integrated Circuit)?", "id": "Chip yang dirancang untuk tugas spesifik." },
  { "en": "Keuntungan ASIC untuk DSP?", "id": "Performa dan efisiensi daya sangat tinggi." },
  { "en": "Apa itu systolic array?", "id": "Jaringan unit pemrosesan untuk komputasi paralel." },
  { "en": "Apa itu CORDIC (COordinate Rotation DIgital Computer)?", "id": "Algoritma efisien untuk fungsi trigonometri." },
  { "en": "Apa itu timbre?", "id": "Kualitas suara yang membedakan instrumen musik." },
  { "en": "Apa itu attack (audio)?", "id": "Bagian awal dari sebuah suara musikal." },
  { "en": "Apa itu decay (audio)?", "id": "Penurunan amplitudo setelah attack." },
  { "en": "Apa itu sustain (audio)?", "id": "Bagian suara dengan amplitudo relatif konstan." },
  { "en": "Apa itu release (audio)?", "id": "Waktu hingga suara hilang setelah not berhenti." },
  { "en": "Apa itu ADSR envelope?", "id": "Attack, Decay, Sustain, Release." },
  { "en": "Apa itu acoustic scene classification?", "id": "Mengidentifikasi lingkungan dari rekaman audio." },
  { "en": "Apa itu sound event detection?", "id": "Mendeteksi kapan suara tertentu terjadi." },
  { "en": "Apa itu polyphonic sound event detection?", "id": "Mendeteksi beberapa suara yang terjadi bersamaan." },
  { "en": "Apa itu audio captioning?", "id": "Menghasilkan deskripsi teks dari konten audio." },
  { "en": "Apa itu fractional calculus?", "id": "Generalisasi turunan dan integral ke orde non-bilangan bulat." },
  { "en": "Aplikasi fractional calculus di DSP?", "id": "Desain filter fraksional dan pemodelan sistem." },
  { "en": "Apa itu Fejér kernel?", "id": "Rata-rata dari kernel Dirichlet." },
  { "en": "Fungsi Fejér kernel?", "id": "Menjamin konvergensi pada deret Fourier." },
  { "en": "Apa itu Gibbs ringing phenomenon?", "id": "Artefak osilasi dekat diskontinuitas tajam." },
  { "en": "Apa itu Lanczos resampling?", "id": "Metode interpolasi berbasis fungsi sinc berjendela." },
  { "en": "Apa itu aliasing frekuensi negatif?", "id": "Terjadi pada sinyal real saat sampling." },
  { "en": "Apa itu analytic signal?", "id": "Sinyal bernilai kompleks tanpa komponen frekuensi negatif." },
  { "en": "Apa itu instantaneous amplitude?", "id": "Envelope dari sinyal real." },
  { "en": "Apa itu 'wrapping' fasa?", "id": "Fasa terbatas pada rentang (-π, π)." },
  { "en": "Apa itu phase unwrapping?", "id": "Merekonstruksi fasa absolut dari fasa 'wrapped'." },
  { "en": "Apa itu group delay?", "id": "Tundaan waktu dari envelope sinyal." },
  { "en": "Apa itu phase delay?", "id": "Tundaan waktu dari fasa sinyal." },
  { "en": "Apa itu sinyal kausalitas 2D?", "id": "Konsep kausalitas yang lebih kompleks dari 1D." },
  { "en": "Apa itu filter separable?", "id": "Filter 2D yang dapat dipecah jadi dua filter 1D." },
  { "en": "Keuntungan filter separable?", "id": "Komputasi jauh lebih efisien." },
  { "en": "Apa itu steerable filter?", "id": "Filter yang dapat diorientasikan ke segala arah." },
  { "en": "Apa itu bag-of-visual-words (BoVW)?", "id": "Metode representasi citra dengan menghitung 'kata' visual." },
  { "en": "Apa itu VLAD (Vector of Locally Aggregated Descriptors)?", "id": "Peningkatan dari model BoVW." },
  { "en": "Apa itu Fisher Vector?", "id": "Representasi citra yang kuat, peningkatan dari BoVW." },
  { "en": "Apa itu hashing peka lokalitas (LSH)?", "id": "Teknik hashing untuk pencarian tetangga terdekat." },
  { "en": "Apa itu pohon kd (k-d tree)?", "id": "Struktur data untuk pencarian di ruang k-dimensi." },
  { "en": "Apa itu pencarian tetangga terdekat (Nearest Neighbor)?", "id": "Menemukan titik terdekat dalam sebuah set data." },
  { "en": "Apa itu kutukan dimensi (curse of dimensionality)?", "id": "Masalah yang timbul saat menganalisis data high-dimensional." },
  { "en": "Apa itu sub-pixel accuracy?", "id": "Estimasi posisi dengan presisi lebih tinggi dari piksel." },
  { "en": "Apa itu camera trapping?", "id": "Penggunaan kamera sensor gerak untuk memantau satwa liar." },
  { "en": "Apa itu acoustic trapping?", "id": "Menggunakan gelombang suara untuk memanipulasi objek." },
  { "en": "Apa itu Doppler beaming?", "id": "Efek dimana sumber bergerak tampak lebih terang." },
  { "en": "Apa itu inverse synthetic-aperture radar (ISAR)?", "id": "Teknik radar untuk mencitrakan target bergerak." },
  { "en": "Apa itu bistatic radar?", "id": "Sistem radar dengan pemancar dan penerima terpisah." },
  { "en": "Apa itu multistatic radar?", "id": "Sistem radar dengan banyak pemancar/penerima." },
  { "en": "Apa itu clutter (radar)?", "id": "Gema yang tidak diinginkan dari lingkungan." },
  { "en": "Apa itu MTI (Moving Target Indication)?", "id": "Teknik untuk menekan clutter stasioner." },
  { "en": "Apa itu pulse-Doppler radar?", "id": "Radar yang mengukur jangkauan dan kecepatan target." },
  { "en": "Apa itu super-resolution microscopy?", "id": "Teknik mikroskop melampaui batas difraksi." },
  { "en": "Contoh super-resolution microscopy?", "id": "STED (Stimulated Emission Depletion) dan PALM/STORM." },
  { "en": "Apa itu Shack-Hartmann wavefront sensor?", "id": "Sensor untuk mengukur aberasi optik." },
  { "en": "Apa itu optik adaptif?", "id": "Teknologi untuk mengoreksi distorsi optik." },
  { "en": "Apa itu speckle imaging?", "id": "Teknik untuk mengatasi blur akibat turbulensi atmosfer." },
  { "en": "Apa itu Lucky imaging?", "id": "Memilih frame paling tajam dari video." },
  { "en": "Apa itu image registration?", "id": "Menyelaraskan dua atau lebih citra." },
  { "en": "Apa itu rigid registration?", "id": "Penyelarasan hanya dengan translasi dan rotasi." },
  { "en": "Apa itu affine registration?", "id": "Memungkinkan scaling dan shearing selain rigid." },
  { "en": "Apa itu deformable (non-rigid) registration?", "id": "Memungkinkan transformasi lokal yang kompleks." },
  { "en": "Apa itu Affective Computing?", "id": "Komputasi yang berhubungan dengan emosi manusia." },
  { "en": "Deteksi emosi dari suara?", "id": "Menganalisis pitch, energi, dan fitur spektral." },
  { "en": "Deteksi emosi dari wajah?", "id": "Menganalisis ekspresi mikro dan unit aksi." },
  { "en": "Apa itu Facial Action Coding System (FACS)?", "id": "Sistem untuk mengklasifikasikan gerakan otot wajah." },
  { "en": "Apa itu sinyal fisiologis?", "id": "Sinyal dari tubuh (detak jantung, konduktansi kulit)." },
  { "en": "Apa itu Galvanic Skin Response (GSR)?", "id": "Mengukur perubahan konduktansi kulit akibat emosi." },
  { "en": "Apa itu Granger Causality?", "id": "Uji statistik untuk kausalitas prediktif." },
  { "en": "Fungsi Granger Causality?", "id": "Menentukan apakah satu time series berguna." },
  { "en": "Apa itu Transfer Entropy?", "id": "Ukuran aliran informasi antara dua proses." },
  { "en": "Apa itu computational imaging?", "id": "Pencitraan yang bergantung pada pemrosesan komputasional." },
  { "en": "Apa itu single-pixel camera?", "id": "Kamera yang membentuk citra dengan satu detektor." },
  { "en": "Apa itu ghost imaging?", "id": "Membentuk citra objek tanpa cahaya menyentuhnya." },
  { "en": "Apa itu diffraction tomography?", "id": "Teknik pencitraan 3D menggunakan data difraksi." },
  { "en": "Apa itu ptychography?", "id": "Teknik mikroskopi resolusi tinggi." },
  { "en": "Apa itu event-based camera?", "id": "Sensor yang hanya merekam perubahan kecerahan piksel." },
  { "en": "Keuntungan event-based camera?", "id": "Rentang dinamis tinggi dan latensi rendah." },
  { "en": "Apa itu Finite Rate of Innovation (FRI)?", "id": "Prinsip sampling sinyal non-bandlimited." },
  { "en": "Syarat sinyal FRI?", "id": "Memiliki struktur parametrik seperti aliran Dirac." },
  { "en": "Apa itu Xampling?", "id": "Framework hardware-efficient untuk sampling sinyal FRI." },
  { "en": "Apa itu Teorema Sampling Shannon?", "id": "Menetapkan laju sampling minimum untuk sinyal bandlimited." },
  { "en": "Siapa Claude Shannon?", "id": "Bapak dari teori informasi." },
  { "en": "Siapa Norbert Wiener?", "id": "Perintis sibernetika dan filter Wiener." },
  { "en": "Apa itu Kolmogorov Complexity?", "id": "Ukuran kompleksitas algoritmik dari suatu objek." },
  { "en": "Hubungan Komplesitas Kolmogorov dengan kompresi?", "id": "Batas bawah teoretis dari kompresi lossless." },
  { "en": "Apa itu The Lottery Ticket Hypothesis?", "id": "Jaringan saraf padat berisi 'subnetwork' pemenang." },
  { "en": "Apa itu Neural Tangent Kernel (NTK)?", "id": "Mendeskripsikan dinamika training jaringan saraf lebar." },
  { "en": "Apa itu infinite-width limit?", "id": "Batas teoretis saat jumlah neuron tak terbatas." },
  { "en": "Apa itu WaveNet?", "id": "Model generatif dalam untuk audio mentah." },
  { "en": "Apa itu physical modeling synthesis?", "id": "Sintesis suara dengan memodelkan instrumen fisik." },
  { "en": "Apa itu granular synthesis?", "id": "Sintesis suara dengan menggabungkan 'butiran' audio kecil." },
  { "en": "Apa itu subtractive synthesis?", "id": "Sintesis dengan memfilter bentuk gelombang kaya harmonik." },
  { "en": "Apa itu additive synthesis?", "id": "Sintesis dengan menjumlahkan gelombang sinus." },
  { "en": "Apa itu frequency modulation (FM) synthesis?", "id": "Sintesis suara dengan memodulasi frekuensi osilator." },
  { "en": "Apa itu sinyal dari LIGO?", "id": "Mendeteksi riak kecil dalam ruang-waktu." },
  { "en": "Sumber sinyal gelombang gravitasi?", "id": "Tabrakan lubang hitam atau bintang neutron." },
  { "en": "Apa itu matched filtering?", "id": "Teknik untuk mendeteksi sinyal yang diketahui." },
  { "en": "Aplikasi matched filtering di LIGO?", "id": "Mencari sinyal gelombang gravitasi dalam noise." },
  { "en": "Apa itu radio astronomy?", "id": "Mempelajari objek langit pada frekuensi radio." },
  { "en": "Apa itu interferometry (astronomi)?", "id": "Menggabungkan sinyal dari banyak teleskop." },
  { "en": "Tujuan interferometry?", "id": "Mencapai resolusi sudut yang sangat tinggi." },
  { "en": "Apa itu deconvolution (astronomi)?", "id": "Menghapus efek instrumental dari citra." },
  { "en": "Apa itu algoritma CLEAN?", "id": "Algoritma dekonvolusi populer dalam radio astronomi." },
  { "en": "Apa itu Full Waveform Inversion (FWI)?", "id": "Teknik pencitraan seismik resolusi tinggi." },
  { "en": "Apa itu Reverse Time Migration (RTM)?", "id": "Metode migrasi seismik berbasis persamaan gelombang." },
  { "en": "Apa itu Phasor Measurement Unit (PMU)?", "id": "Mengukur magnitudo dan fasa tegangan." },
  { "en": "Apa itu synchrophasor?", "id": "Pengukuran phasor yang disinkronkan dengan waktu." },
  { "en": "Aplikasi PMU?", "id": "Monitoring dan kontrol jaringan listrik." },
  { "en": "Apa itu Wide Area Measurement System (WAMS)?", "id": "Jaringan PMU untuk memantau area luas." },
  { "en": "Apa itu modal analysis?", "id": "Studi properti dinamis dari suatu struktur." },
  { "en": "Apa itu Operational Deflection Shape (ODS)?", "id": "Visualisasi getaran struktur pada frekuensi tertentu." },
  { "en": "Apa itu Occupancy Grid Mapping?", "id": "Metode robotik untuk merepresentasikan lingkungan." },
  { "en": "Apa itu particle filter?", "id": "Metode Monte Carlo untuk estimasi Bayesian." },
  { "en": "Aplikasi particle filter?", "id": "Pelacakan objek dan lokalisasi robot." },
  { "en": "Apa itu unscented Kalman filter (UKF)?", "id": "Filter Kalman non-linear menggunakan unscented transform." },
  { "en": "Apa itu extended Kalman filter (EKF)?", "id": "Filter Kalman non-linear menggunakan linearisasi." },
  { "en": "Apa itu covariance intersection?", "id": "Menggabungkan estimasi tanpa mengetahui korelasi." },
  { "en": "Apa itu fuzzy logic?", "id": "Logika yang berurusan dengan 'derajat kebenaran'." },
  { "en": "Apa itu fuzzy c-means clustering?", "id": "Versi fuzzy dari algoritma k-means." },
  { "en": "Apa itu genetic algorithm?", "id": "Algoritma optimisasi yang terinspirasi oleh evolusi." },
  { "en": "Apa itu swarm intelligence?", "id": "Perilaku kolektif dari sistem terdesentralisasi." },
  { "en": "Apa itu Ant Colony Optimization?", "id": "Algoritma optimisasi berdasarkan perilaku semut." },
  { "en": "Apa itu Particle Swarm Optimization?", "id": "Algoritma optimisasi berdasarkan perilaku kawanan." },
  { "en": "Apa itu Dempster-Shafer theory?", "id": "Kerangka kerja untuk penalaran di bawah ketidakpastian." },
  { "en": "Apa itu random field?", "id": "Generalisasi dari proses stokastik ke banyak dimensi." },
  { "en": "Apa itu Markov Random Field (MRF)?", "id": "Random field dimana setiap simpul bersifat Markovian." },
  { "en": "Aplikasi MRF dalam citra?", "id": "Segmentasi citra, denoising, dan inpainting." },
  { "en": "Apa itu Gibbs sampling?", "id": "Algoritma MCMC untuk sampling dari distribusi." },
  { "en": "Apa itu simulated annealing?", "id": "Metode optimisasi global probabilistik." },
  { "en": "Apa itu graph cuts?", "id": "Metode optimisasi untuk masalah seperti segmentasi." },
  { "en": "Apa itu dynamic time warping (DTW)?", "id": "Menyelaraskan dua sekuens temporal yang berbeda kecepatan." },
  { "en": "Aplikasi DTW?", "id": "Pengenalan ucapan, analisis deret waktu." },
  { "en": "Apa itu Latent Dirichlet Allocation (LDA)?", "id": "Model generatif untuk menemukan topik dalam teks." },
  { "en": "Apa itu word embedding?", "id": "Representasi vektor dari kata-kata." },
  { "en": "Contoh word embedding?", "id": "Word2Vec, GloVe, dan FastText." },
  { "en": "Apa itu BERT (Bidirectional Encoder Representations from Transformers)?", "id": "Model bahasa berbasis transformer yang kuat." },
  { "en": "Apa itu Generative Pre-trained Transformer (GPT)?", "id": "Keluarga model bahasa generatif dari OpenAI." },
  { "en": "Apa itu natural language processing (NLP)?", "id": "Bidang AI untuk interaksi komputer-bahasa." },
  { "en": "Apa itu Named Entity Recognition (NER)?", "id": "Mengidentifikasi entitas bernama (orang, tempat) dalam teks." },
  { "en": "Apa itu part-of-speech (POS) tagging?", "id": "Melabeli kata dengan kelas katanya (kata benda, kerja)." },
  { "en": "Apa itu sentiment analysis?", "id": "Menentukan sentimen (positif/negatif) dari teks." },
  { "en": "Apa itu reinforcement learning?", "id": "Belajar melalui interaksi dengan lingkungan (rewards)." },
  { "en": "Apa itu Q-learning?", "id": "Algoritma reinforcement learning bebas model." },
  { "en": "Apa itu Deep Q-Network (DQN)?", "id": "Menggunakan jaringan saraf untuk aproksimasi fungsi-Q." },
  { "en": "Apa itu Policy Gradient Methods?", "id": "Metode RL yang secara langsung mengoptimalkan kebijakan." },
  { "en": "Apa itu Actor-Critic methods?", "id": "Kombinasi policy gradient dan value-based methods." },
  { "en": "Apa itu multi-agent reinforcement learning?", "id": "Beberapa agen belajar secara bersamaan." },
  { "en": "Apa itu inverse reinforcement learning?", "id": "Menentukan fungsi reward dari perilaku yang diamati." },
  { "en": "Apa itu curriculum learning?", "id": "Melatih model dari contoh mudah ke sulit." },
  { "en": "Apa itu active learning?", "id": "Model secara aktif memilih data untuk dilabeli." },
  { "en": "Apa itu dimensionality reduction?", "id": "Mengurangi jumlah variabel acak." },
  { "en": "Apa itu t-SNE?", "id": "Teknik reduksi dimensi untuk visualisasi data." },
  { "en": "Apa itu UMAP (Uniform Manifold Approximation and Projection)?", "id": "Teknik reduksi dimensi modern yang efisien." },
  { "en": "Apa itu isomap?", "id": "Teknik reduksi dimensi non-linear berbasis jarak geodesic." },
  { "en": "Apa itu Locally Linear Embedding (LLE)?", "id": "Reduksi dimensi yang mempertahankan lingkungan lokal." },
  { "en": "Apa itu Information Geometry?", "id": "Studi geometri dari manifold model statistik." },
  { "en": "Apa itu Fisher Information Matrix?", "id": "Metrik Riemannian pada manifold statistik." },
  { "en": "Apa itu Amari-Chentsov tensor?", "id": "Struktur koneksi affine pada manifold statistik." },
  { "en": "Apa itu natural gradient?", "id": "Gradien yang menghormati geometri ruang parameter." },
  { "en": "Apa itu teori chaos?", "id": "Studi sistem dinamis yang sensitif terhadap kondisi awal." },
  { "en": "Apa itu strange attractor?", "id": "Set titik dimana sistem chaotic berevolusi." },
  { "en": "Apa itu Lyapunov exponent?", "id": "Ukuran laju pemisahan lintasan yang sangat dekat." },
  { "en": "Lyapunov exponent positif menandakan?", "id": "Sistem tersebut bersifat chaotic." },
  { "en": "Apa itu bifurcation diagram?", "id": "Peta yang menunjukkan kemungkinan nilai jangka panjang." },
  { "en": "Apa itu dimensi fraktal?", "id": "Ukuran 'kekasaran' atau kompleksitas dari suatu set." },
  { "en": "Contoh fraktal?", "id": "Himpunan Mandelbrot, Koch snowflake, dan Sierpinski triangle." },
  { "en": "Apa itu phase retrieval?", "id": "Merekonstruksi sinyal dari magnitudo transformasi Fouriernya." },
  { "en": "Mengapa phase retrieval sulit?", "id": "Informasi fasa hilang, masalahnya ill-posed." },
  { "en": "Aplikasi phase retrieval?", "id": "Kristalografi, astronomi, dan mikroskopi." },
  { "en": "Apa itu algoritma Gerchberg-Saxton?", "id": "Algoritma iteratif klasik untuk phase retrieval." },
  { "en": "Apa itu Total Variation (TV) denoising?", "id": "Metode denoising yang menjaga ketajaman tepi." },
  { "en": "Fungsi TV regularization?", "id": "Mendorong solusi yang piecewise-constant." },
  { "en": "Apa itu Bregman iteration?", "id": "Algoritma untuk menyelesaikan masalah optimisasi teregulasi." },
  { "en": "Apa itu algoritma MUSIC (MUltiple SIgnal Classification)?", "id": "Algoritma estimasi frekuensi super-resolusi." },
  { "en": "Apa itu algoritma ESPRIT?", "id": "Estimasi parameter sinyal via teknik rotasi." },
  { "en": "Apa itu array processing?", "id": "Pemrosesan sinyal dari sekumpulan sensor." },
  { "en": "Apa itu arah kedatangan (Direction of Arrival)?", "id": "Estimasi arah datangnya sinyal." },
  { "en": "Apa itu Functional Data Analysis (FDA)?", "id": "Analisis data dimana setiap observasi adalah fungsi." },
  { "en": "Apa itu Functional PCA?", "id": "Versi Principal Component Analysis untuk data fungsional." },
  { "en": "Apa itu blind dereverberation?", "id": "Menghilangkan gema dari audio tanpa informasi ruangan." },
  { "en": "Apa itu homomorphic encryption?", "id": "Memungkinkan komputasi pada data terenkripsi." },
  { "en": "Aplikasi homomorphic encryption di ML?", "id": "Melatih model tanpa mendekripsi data (privacy)." },
  { "en": "Apa itu secure multi-party computation?", "id": "Komputasi bersama tanpa mengungkapkan data pribadi." },
  { "en": "Apa itu differential privacy?", "id": "Jaminan privasi statistik dalam analisis data." },
  { "en": "Apa itu data non-IID (Not Independent and Identically Distributed)?", "id": "Data dengan distribusi yang tidak seragam." },
  { "en": "Masalah data non-IID di Federated Learning?", "id": "Membuat konvergensi model global menjadi sulit." },
  { "en": "Apa itu algoritma FedAvg?", "id": "Algoritma agregasi standar dalam federated learning." },
  { "en": "Apa itu Scientific Machine Learning (SciML)?", "id": "Gabungan ML dengan pemodelan ilmiah." },
  { "en": "Apa itu Universal Differential Equations?", "id": "Persamaan diferensial yang digabungkan dengan jaringan saraf." },
  { "en": "Apa itu network science?", "id": "Studi jaringan kompleks (graf)." },
  { "en": "Apa itu degree centrality?", "id": "Ukuran jumlah koneksi yang dimiliki simpul." },
  { "en": "Apa itu betweenness centrality?", "id": "Ukuran seberapa sering simpul menjadi jembatan." },
  { "en": "Apa itu closeness centrality?", "id": "Ukuran seberapa dekat simpul dengan simpul lain." },
  { "en": "Apa itu eigenvector centrality?", "id": "Ukuran pengaruh simpul dalam suatu jaringan." },
  { "en": "Apa itu modularity?", "id": "Ukuran kekuatan pembagian jaringan menjadi komunitas." },
  { "en": "Apa itu small-world network?", "id": "Jaringan dengan path pendek dan klastering tinggi." },
  { "en": "Apa itu scale-free network?", "id": "Jaringan dengan distribusi derajat power-law." },
  { "en": "Apa itu preferential attachment?", "id": "Mekanisme pertumbuhan jaringan (yang kaya makin kaya)." },
  { "en": "Apa itu tomosynthesis?", "id": "Bentuk tomografi 3D dengan sudut terbatas." },
  { "en": "Apa itu mammography?", "id": "Pencitraan X-ray payudara untuk deteksi kanker." },
  { "en": "Apa itu Computer-Aided Detection (CADe)?", "id": "Sistem yang menandai area mencurigakan untuk radiolog." },
  { "en": "Apa itu Computer-Aided Diagnosis (CADx)?", "id": "Sistem yang membantu diagnosis penyakit." },
  { "en": "Apa itu digital pathology?", "id": "Mendigitalkan slide jaringan untuk analisis." },
  { "en": "Apa itu histopathology?", "id": "Studi mikroskopis dari jaringan yang sakit." },
  { "en": "Apa itu cytology?", "id": "Studi mikroskopis dari sel tunggal." },
  { "en": "Apa itu Fourier Ptychography?", "id": "Teknik mikroskopi komputasional untuk resolusi tinggi." },
  { "en": "Apa itu structured illumination microscopy (SIM)?", "id": "Teknik super-resolusi menggunakan pola cahaya." },
  { "en": "Apa itu optical coherence tomography (OCT)?", "id": "Pencitraan 3D resolusi tinggi berbasis interferometri." },
  { "en": "Aplikasi OCT?", "id": "Oftalmologi (pencitraan retina) dan kardiologi." },
  { "en": "Apa itu optoacoustic (photoacoustic) imaging?", "id": "Pencitraan berbasis efek fotoakustik." },
  { "en": "Apa itu bolometer?", "id": "Detektor untuk radiasi elektromagnetik (panas)." },
  { "en": "Apa itu charge-coupled device (CCD)?", "id": "Sensor citra yang umum digunakan." },
  { "en": "Apa itu CMOS (Complementary Metal-Oxide-Semiconductor) sensor?", "id": "Sensor citra alternatif untuk CCD." },
  { "en": "Perbedaan utama CCD dan CMOS?", "id": "Cara pembacaan muatan (piksel) berbeda." },
  { "en": "Apa itu F-number (lensa)?", "id": "Rasio panjang fokus terhadap diameter aperture." },
  { "en": "F-number kecil berarti?", "id": "Aperture besar, lebih banyak cahaya masuk." },
  { "en": "Apa itu depth of field (DOF)?", "id": "Rentang jarak dimana objek tampak tajam." },
  { "en": "Aperture kecil menghasilkan DOF?", "id": "Depth of field yang lebih dalam (luas)." },
  { "en": "Apa itu circle of confusion?", "id": "Titik blur optik dari sumber titik." },
  { "en": "Apa itu hyperfocal distance?", "id": "Jarak fokus yang memberikan DOF maksimum." },
  { "en": "Apa itu point cloud registration?", "id": "Menyelaraskan beberapa point cloud menjadi satu." },
  { "en": "Apa itu algoritma Iterative Closest Point (ICP)?", "id": "Algoritma umum untuk registrasi point cloud." },
  { "en": "Apa itu volumetric video?", "id": "Video yang menangkap bentuk 3D secara real-time." },
  { "en": "Apa itu neural rendering?", "id": "Sintesis citra menggunakan model jaringan saraf." },
  { "en": "Apa itu implicit neural representation?", "id": "Merepresentasikan sinyal/bentuk sebagai fungsi neural." },
  { "en": "Contoh implicit neural representation?", "id": "NeRF (Neural Radiance Fields)." },
  { "en": "Apa itu diffeomorphic registration?", "id": "Registrasi non-rigid yang menjaga topologi." },
  { "en": "Apa itu atlas (citra medis)?", "id": "Citra referensi standar dari suatu anatomi." },
  { "en": "Apa itu multi-modal registration?", "id": "Menyelaraskan citra dari modalitas berbeda (CT-MRI)." },
  { "en": "Apa itu mutual information sebagai metrik registrasi?", "id": "Mengukur dependensi statistik antara intensitas citra." },
  { "en": "Apa itu sum of squared differences (SSD)?", "id": "Metrik sederhana untuk registrasi citra." },
  { "en": "Apa itu normalized cross-correlation (NCC)?", "id": "Metrik registrasi yang invarian terhadap kecerahan." },
  { "en": "Apa itu sampling Importance Resampling (SIR)?", "id": "Langkah kunci dalam algoritma particle filter." },
  { "en": "Apa itu Rao-Blackwellization?", "id": "Teknik untuk meningkatkan efisiensi estimator." },
  { "en": "Apa itu Cramér-Rao lower bound (CRLB)?", "id": "Batas bawah varians dari estimator." },
  { "en": "Apa itu estimator efisien?", "id": "Estimator yang variansnya mencapai batas Cramér-Rao." },
  { "en": "Apa itu sufficient statistic?", "id": "Statistik yang menangkap semua informasi tentang parameter." },
  { "en": "Apa itu exponential family?", "id": "Keluarga distribusi probabilitas dengan bentuk tertentu." },
  { "en": "Apa itu conjugate prior?", "id": "Prior yang menghasilkan posterior dari keluarga sama." },
  { "en": "Apa itu Bayesian network?", "id": "Model grafis probabilitas untuk variabel acak." },
  { "en": "Apa itu d-separation?", "id": "Kriteria independensi kondisional dalam Bayesian network." },
  { "en": "Apa itu junction tree algorithm?", "id": "Algoritma untuk inferensi eksak pada graf." },
  { "en": "Apa itu variational autoencoder (VAE) ELBO?", "id": "Evidence Lower Bound, objektif yang dioptimalkan." },
  { "en": "Apa itu reparameterization trick?", "id": "Memungkinkan backpropagation melalui simpul acak di VAE." },
  { "en": "Apa itu disentangled representation?", "id": "Representasi dimana faktor variasinya terpisah." },
  { "en": "Apa itu world model?", "id": "Model internal yang dipelajari agen RL." },
  { "en": "Apa itu model-based reinforcement learning?", "id": "RL yang menggunakan model lingkungan." },
  { "en": "Apa itu model-free reinforcement learning?", "id": "RL yang belajar kebijakan secara langsung." },
  { "en": "Apa itu exploration-exploitation tradeoff?", "id": "Dilema antara mengeksplorasi aksi baru/memanfaatkan yang sudah diketahui." },
  { "en": "Apa itu epsilon-greedy policy?", "id": "Kebijakan sederhana untuk menyeimbangkan eksplorasi/eksploitasi." },
  { "en": "Apa itu intrinsic motivation?", "id": "Mendorong agen untuk menjelajah karena 'rasa ingin tahu'." },
  { "en": "Apa itu Green AI?", "id": "Riset AI yang berfokus pada efisiensi komputasi." },
  { "en": "Apa itu Red AI?", "id": "Riset AI yang mengabaikan biaya komputasi." },
  { "en": "Apa itu Algorithmic Fairness?", "id": "Memastikan model tidak menghasilkan hasil diskriminatif." },
  { "en": "Apa itu bias (dalam AI)?", "id": "Kesalahan sistematis yang merugikan kelompok tertentu." },
  { "en": "Contoh bias dalam computer vision?", "id": "Model pengenalan wajah yang kurang akurat." },
  { "en": "Apa itu Disparate Impact?", "id": "Metrik fairness yang mengukur dampak pada kelompok." },
  { "en": "Apa itu Neuro-Symbolic AI?", "id": "Menggabungkan neural networks dengan penalaran simbolik." },
  { "en": "Keuntungan Neuro-Symbolic AI?", "id": "Lebih dapat diinterpretasikan dan efisien data." },
  { "en": "Apa itu Structural Causal Model (SCM)?", "id": "Model grafis yang merepresentasikan hubungan kausal." },
  { "en": "Apa itu intervensi (kausal)?", "id": "Memanipulasi variabel untuk mengamati efeknya." },
  { "en": "Apa itu counterfactual?", "id": "Penalaran tentang 'apa yang akan terjadi jika'." },
  { "en": "Apa itu Spherical Harmonics?", "id": "Basis fungsi ortogonal pada permukaan bola." },
  { "en": "Aplikasi Spherical Harmonics?", "id": "Grafik komputer, kosmologi, dan geofisika." },
  { "en": "Apa itu Cosmic Microwave Background (CMB)?", "id": "Sisa radiasi dari Big Bang." },
  { "en": "Analisis CMB menggunakan apa?", "id": "Dekomposisi sinyal pada bola (Spherical Harmonics)." },
  { "en": "Apa itu Zernike polynomials?", "id": "Fungsi ortogonal yang mendeskripsikan aberasi optik." },
  { "en": "Apa itu Bessel functions?", "id": "Solusi untuk persamaan diferensial Bessel." },
  { "en": "Aplikasi Bessel functions?", "id": "Analisis getaran membran, propagasi gelombang." },
  { "en": "Apa itu Legendre polynomials?", "id": "Solusi polinomial untuk persamaan diferensial Legendre." },
  { "en": "Apa itu Green's function?", "id": "Respon impuls dari operator diferensial linear." },
  { "en": "Apa itu optimisasi konveks?", "id": "Meminimalkan fungsi konveks pada set konveks." },
  { "en": "Mengapa optimisasi konveks penting?", "id": "Setiap minimum lokal adalah minimum global." },
  { "en": "Apa itu ADMM (Alternating Direction Method of Multipliers)?", "id": "Algoritma optimisasi untuk masalah terstruktur." },
  { "en": "Apa itu proximal algorithm?", "id": "Algoritma untuk meminimalkan fungsi non-smooth." },
  { "en": "Apa itu primal-dual method?", "id": "Metode optimisasi yang menyelesaikan masalah primal/dual." },
  { "en": "Apa itu subgradient method?", "id": "Generalisasi gradient descent untuk fungsi non-differentiable." },
  { "en": "Apa itu Frank-Wolfe algorithm?", "id": "Algoritma optimisasi konveks bebas proyeksi." },
  { "en": "Apa itu interior-point method?", "id": "Kelas algoritma untuk optimisasi linear/non-linear." },
  { "en": "Apa itu Karush-Kuhn-Tucker (KKT) conditions?", "id": "Kondisi perlu untuk optimalitas dalam optimisasi." },
  { "en": "Apa itu Lagrangian dual problem?", "id": "Masalah optimisasi terkait yang memberikan batas bawah." },
  { "en": "Apa itu Quantum Machine Learning (QML)?", "id": "Menggunakan komputasi kuantum untuk tugas machine learning." },
  { "en": "Apa itu Quantum PCA?", "id": "Algoritma kuantum untuk Principal Component Analysis." },
  { "en": "Apa itu Quantum SVM?", "id": "Algoritma kuantum untuk Support Vector Machines." },
  { "en": "Apa itu Variational Quantum Eigensolver (VQE)?", "id": "Algoritma hibrida kuantum-klasik." },
  { "en": "Apa itu Quantum Fourier Transform (QFT)?", "id": "Analog kuantum dari Discrete Fourier Transform." },
  { "en": "Apa itu algoritma Shor?", "id": "Algoritma kuantum untuk faktorisasi (berbasis QFT)." },
  { "en": "Apa itu differential geometry?", "id": "Studi geometri menggunakan kalkulus." },
  { "en": "Apa itu geodesic?", "id": "Jalur terpendek antara dua titik pada manifold." },
  { "en": "Aplikasi geodesic dalam citra?", "id": "Segmentasi menggunakan active contours." },
  { "en": "Apa itu curvature?", "id": "Ukuran seberapa banyak kurva/permukaan melengkung." },
  { "en": "Apa itu Ricci flow?", "id": "Proses yang menghaluskan metrik suatu manifold." },
  { "en": "Apa itu formal verification?", "id": "Membuktikan atau menyangkal kebenaran sistem." },
  { "en": "Aplikasi formal verification di AI?", "id": "Memverifikasi keamanan dan robustnes dari model." },
  { "en": "Apa itu model checking?", "id": "Teknik verifikasi untuk sistem finite-state." },
  { "en": "Apa itu abstract interpretation?", "id": "Teori aproksimasi semantik dari program komputer." },
  { "en": "Apa itu 'AI alignment problem'?", "id": "Masalah memastikan AI bertindak sesuai keinginan manusia." },
  { "en": "Apa itu 'value learning'?", "id": "Proses dimana AI mempelajari nilai-nilai manusia." },
  { "en": "Apa itu 'corrigibility'?", "id": "Sifat AI yang tidak menolak untuk dikoreksi." },
  { "en": "Apa itu Teorema Konvolusi?", "id": "Konvolusi di satu domain adalah perkalian." },
  { "en": "Teorema Konvolusi untuk Fourier Transform?", "id": "Konvolusi waktu menjadi perkalian frekuensi." },
  { "en": "Teorema Konvolusi untuk Laplace Transform?", "id": "Konvolusi waktu menjadi perkalian domain-s." },
  { "en": "Teorema Konvolusi untuk Z-Transform?", "id": "Konvolusi waktu diskrit menjadi perkalian domain-z." },
  { "en": "Apa itu Parseval's theorem?", "id": "Total energi sinyal sama di domain waktu/frekuensi." },
  { "en": "Apa itu Plancherel's theorem?", "id": "Generalisasi dari teorema Parseval." },
  { "en": "Apa itu Wiener-Khinchin theorem?", "id": "Menghubungkan autokorelasi dengan Power Spectral Density." },
  { "en": "Apa itu Gabor uncertainty principle?", "id": "Trade-off resolusi antara waktu dan frekuensi." },
  { "en": "Apa itu time-bandwidth product?", "id": "Ukuran kuantitatif dari prinsip ketidakpastian." },
  { "en": "Apa itu Cramer's rule?", "id": "Solusi eksplisit untuk sistem persamaan linear." },
  { "en": "Apa itu Cholesky decomposition?", "id": "Faktorisasi matriks simetris menjadi segitiga." },
  { "en": "Apa itu QR decomposition?", "id": "Faktorisasi matriks menjadi ortogonal dan segitiga." },
  { "en": "Apa itu LU decomposition?", "id": "Faktorisasi matriks menjadi lower dan upper." },
  { "en": "Apa itu Schur decomposition?", "id": "Faktorisasi matriks menjadi matriks segitiga." },
  { "en": "Apa itu Jordan normal form?", "id": "Representasi matriks dalam bentuk 'hampir' diagonal." },
  { "en": "Apa itu Cayley-Hamilton theorem?", "id": "Setiap matriks memenuhi persamaan karakteristiknya sendiri." },
  { "en": "Apa itu Perron-Frobenius theorem?", "id": "Teorema tentang eigenvector dari matriks positif." },
  { "en": "Apa itu Gershgorin circle theorem?", "id": "Memberikan batas untuk eigenvalue suatu matriks." },
  { "en": "Apa itu condition number?", "id": "Ukuran sensitivitas solusi terhadap error." },
  { "en": "Condition number besar menandakan?", "id": "Masalah tersebut 'ill-conditioned'." },
  { "en": "Apa itu 'vanishing gradient problem'?", "id": "Gradien menjadi sangat kecil saat backpropagation." },
  { "en": "Apa itu 'exploding gradient problem'?", "id": "Gradien menjadi sangat besar saat backpropagation." },
  { "en": "Apa itu gradient clipping?", "id": "Teknik untuk mengatasi exploding gradient." },
  { "en": "Apa itu skip connection?", "id": "Koneksi yang 'melompati' beberapa layer." },
  { "en": "Apa itu highway network?", "id": "Jaringan saraf dengan 'gerbang' informasi." },
  { "en": "Apa itu dense network (DenseNet)?", "id": "Setiap layer terhubung ke semua layer berikutnya." },
  { "en": "Apa itu 'no free lunch' theorem?", "id": "Tidak ada satu algoritma yang terbaik." },
  { "en": "Apa itu Occam's razor?", "id": "Prinsip memilih penjelasan yang paling sederhana." },
  { "en": "Apa itu Bias-Variance tradeoff?", "id": "Trade-off antara error dari asumsi dan data." },
  { "en": "Model dengan bias tinggi?", "id": "Terlalu sederhana, mengalami underfitting." },
  { "en": "Model dengan varians tinggi?", "id": "Terlalu kompleks, mengalami overfitting." },
  { "en": "Apa itu ensemble learning?", "id": "Menggabungkan beberapa model untuk meningkatkan performa." },
  { "en": "Apa itu bagging?", "id": "Membuat ansambel dengan training pada subset data." },
  { "en": "Apa itu boosting?", "id": "Melatih model secara sekuensial untuk memperbaiki error." },
  { "en": "Apa itu stacking?", "id": "Menggunakan model untuk menggabungkan output model lain." },
  { "en": "Apa itu dropout sebagai ansambel?", "id": "Dapat diinterpretasikan sebagai ansambel dari subnetwork." },
  { "en": "Apa itu Bayesian neural network?", "id": "Jaringan saraf yang bobotnya adalah distribusi." },
  { "en": "Apa itu Gaussian process?", "id": "Model stokastik untuk regresi non-parametrik." },
  { "en": "Apa itu Dirichlet process?", "id": "Distribusi probabilitas pada distribusi probabilitas." },
  { "en": "Apa itu Chinese Restaurant Process?", "id": "Proses stokastik untuk clustering." },
  { "en": "Apa itu multi-task learning?", "id": "Melatih satu model untuk beberapa tugas sekaligus." },
  { "en": "Apa itu zero-shot learning (definisi lain)?", "id": "Klasifikasi tanpa melihat sampel kelas tersebut." },
  { "en": "Apa itu self-distillation?", "id": "Model 'mengajar' dirinya sendiri." },
  { "en": "Apa itu knowledge graph?", "id": "Representasi pengetahuan terstruktur dalam bentuk graf." },
  { "en": "Apa itu 'common sense' reasoning?", "id": "Kemampuan AI untuk membuat asumsi seperti manusia." },
  { "en": "Apa itu Inductive Logic Programming (ILP)?", "id": "Sub-bidang AI yang menggunakan logika untuk learning." },
  { "en": "Apa itu Abductive Reasoning?", "id": "Penalaran untuk mencari penjelasan terbaik." },
  { "en": "Apa itu analogical reasoning?", "id": "Penalaran berdasarkan analogi atau kemiripan." }



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
