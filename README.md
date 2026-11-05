# GEOSOIL — Soil Classification & Analysis

Aplikasi web mekanika tanah dan rekayasa geoteknik untuk membantu mahasiswa dan praktisi **Teknik Sipil** melakukan analisis saringan, batas Atterberg, klasifikasi tanah, serta pelaporan teknis berdasarkan tiga sistem standar:

1. **USCS (*Unified Soil Classification System*)** — Mengacu pada **SNI 6371:2015** & **ASTM D2487**.
2. **AASHTO (*American Association of State Highway and Transportation Officials*)** — Mengacu pada **AASHTO M 145**.
3. **USDA (*United States Department of Agriculture*)** — Mengacu pada **USDA Soil Survey Manual** (*12 Kelas Segitiga Tekstur Tanah*).

---

## 1. Fitur Utama Aplikasi

### A. Alur Analisis 6 Tahap Terstruktur
1. **Halaman 1 — Beranda (`HomePage.tsx`)**: Identitas visual kebumian GEOSOIL, penampang stratigrafi interaktif, dan kartu akses cepat.
2. **Halaman 2 — Pilih Sistem Klasifikasi (`SelectSystemPage.tsx`)**: Tiga kartu sistem berbeda secara visual (USCS, AASHTO, USDA) dengan perbandingan peruntukan rekayasa.
3. **Halaman 3 — Pengenalan Sistem (`SystemIntroPage.tsx`)**: Penjelasan teknis sistem, parameter wajib, standar acuan, dan pratinjau hasil.
4. **Halaman 4 — Informasi Sampel & Input Data (`SampleInputPage.tsx`)**:
   - **Metode A (Massa Tertahan / *Mass Retained*)**: Menghitung otomatis massa total tertahan, `% tertahan`, `% tertahan kumulatif`, `% lolos`, dan validasi keseimbangan massa (toleransi selisih $\le 2\%$).
   - **Metode B (Persentase Lolos Langsung / *Percent Passing*)**: Input langsung `% lolos` dengan validasi urutan monotonik ukuran saringan.
   - **15 Saringan Standar + Tambah/Hapus Saringan Kustom**: Mendukung ukuran saringan 75 mm hingga 0,075 mm (No. 200).
   - **Batas Atterberg (`LL`, `PL`, `PI = LL - PL`, dan opsi *Non-Plastic* / `NP`)** sesuai ASTM D4318.
   - **Preset Sampel Uji Cepat**: Tersedia tombol muat data sampel nyata untuk menguji berbagai skenario tanah.
5. **Halaman 5 — Proses Analisis (`AnalysisProcessPage.tsx`)**: Menampilkan tabel distribusi butir lengkap, interpolasi logaritmik $D_{10}, D_{30}, D_{60}$, perhitungan $C_u$ dan $C_c$, rumus disertai substitusi angka nyata, serta pohon keputusan (*decision tree*) langkah demi langkah.
6. **Halaman 6 — Hasil Klasifikasi & Unduhan Laporan (`AnalysisResultPage.tsx`)**:
   - **Format A — Laporan Teknis Lengkap**: Mencakup identitas sampel, tabel saringan, parameter terhitung & rumus substitusi, tahapan keputusan, grafik teknis, sifat rekayasa, catatan keterbatasan data, dan referensi standar.
   - **Format B — Ringkasan Hasil (*Executive Summary*)**: Tampilan ringkas untuk dokumentasi cepat atau presentasi.

### B. Fitur Unduhan & Ekspor Data (100% Berfungsi)
- **Unduh Laporan Teknis PDF (`Laporan_Teknis_GEOSOIL_<SISTEM>_<KODE>.pdf`)**: Dokumen PDF multi-halaman lengkap dengan logo vektor GEOSOIL, tabel saringan, rumus & substitusi, tahapan keputusan, serta grafik vektor (Kurva Semi-Log, Diagram Casagrande, Diagram AASHTO, atau Segitiga USDA).
- **Unduh Ringkasan PDF (`Ringkasan_Hasil_GEOSOIL_<SISTEM>_<KODE>.pdf`)**: Dokumen PDF ringkasan eksekutif 1–2 halaman beserta grafik vektor utama.
- **Unduh Data XLSX (`Data_GEOSOIL_<SISTEM>_<KODE>.xlsx`)**: File spreadsheet Microsoft Excel OpenXML asli dengan **3 Worksheet** (`Ringkasan_Analisis`, `Tabel_Saringan`, `Tahapan_Keputusan`).
- **Unduh Data CSV (`Data_GEOSOIL_<SISTEM>_<KODE>.csv`)**: File CSV ber-enkode UTF-8 BOM yang siap dibuka langsung di Microsoft Excel, Google Sheets, atau LibreOffice Calc.

### C. Penyimpanan Otomatis & Riwayat Analisis Lokal
- Data pekerjaan aktif disimpan otomatis di `localStorage` (`geosoil_app_state_v2`) sehingga tidak hilang saat berpindah halaman atau melakukan *refresh*.
- Fitur **Riwayat Analisis Lokal** (`geosoil_history_v1`) memungkinkan pengguna menyimpan hingga 30 hasil analisis di perangkat yang sama untuk dibuka kembali atau diunduh ulang kapan saja.
- **Catatan Keterbatasan Penyimpanan Lokal**: Data riwayat disimpan pada memori browser lokal (`localStorage`) di perangkat yang sedang digunakan dan tidak tersinkronisasi antarperangkat karena aplikasi berjalan secara *client-side* tanpa server database eksternal.

---

## 2. Daftar Dependensi (`package.json`)

Proyek ini dibuat mandiri tanpa ketergantungan pada lingkungan khusus sehingga dapat dibuka dan dijalankan di komputer mana pun menggunakan **Visual Studio Code** dan **Node.js**.

### Dependensi Utama (`dependencies`)
| Paket | Versi | Kegunaan |
| :--- | :--- | :--- |
| `react` & `react-dom` | `^19.0.0` | Library utama antarmuka pengguna (UI) berbasis komponen |
| `lucide-react` | `^1.16.0` | Ikon vektor teknis dan navigasi |
| `jspdf` | `^4.2.1` | Mesin pembuat dokumen PDF *client-side* beserta gambar grafik vektor |
| `jspdf-autotable` | `^5.0.7` | Pembuat tabel otomatis rapi & tidak terpotong di dalam dokumen PDF |

### Dependensi Pengembangan (`devDependencies`)
| Paket | Versi | Kegunaan |
| :--- | :--- | :--- |
| `vite` | `^6.2.0` | Bundler dan *development server* modern berkecepatan tinggi |
| `typescript` | `~5.7.2` | Pemeriksaan tipe statis (*type-safety*) untuk rumus dan struktur data |
| `@vitejs/plugin-react` | `^4.3.4` | Plugin kompilasi React Fast Refresh untuk Vite |
| `tailwindcss` & `@tailwindcss/vite` | `^4.2.2` | Utilitas styling CSS modern |

---

## 3. Struktur Folder Proyek

```text
GEOSOIL2/
├── index.html                        # Entry HTML utama (memuat font Manrope, DM Sans, JetBrains Mono)
├── package.json                      # Daftar dependensi dan skrip npm
├── tsconfig.json                     # Konfigurasi TypeScript
├── vite.config.ts                    # Konfigurasi bundler Vite
├── vercel.json                       # Konfigurasi siap deploy untuk Vercel
├── netlify.toml                      # Konfigurasi siap deploy untuk Netlify
├── README.md                         # Dokumentasi proyek dan panduan pengembangan
└── src/
    ├── main.tsx                      # Entry point aplikasi React
    ├── App.tsx                       # Pengatur state utama, stepper 6 halaman, dan riwayat lokal
    ├── assets/
    │   └── SoilIllustrations.tsx     # Ilustrasi SVG kustom (Stratigrafi, Tekstur Butir, Alat Lab)
    ├── components/
    │   ├── NavbarAndProgress.tsx     # Navigasi atas, indikator 6 tahapan, dan modal reset
    │   ├── SoilCharts.tsx            # Grafik SVG (Kurva Semi-Log, Casagrande, AASHTO, Segitiga USDA)
    │   └── HistoryModal.tsx          # Modal manajemen Riwayat Analisis Lokal
    ├── pages/
    │   ├── HomePage.tsx              # Halaman 1: Beranda
    │   ├── SelectSystemPage.tsx      # Halaman 2: Pilih Sistem Klasifikasi
    │   ├── SystemIntroPage.tsx       # Halaman 3: Pengenalan Sistem
    │   ├── SampleInputPage.tsx       # Halaman 4: Informasi Sampel & Input Data (Metode A & B)
    │   ├── AnalysisProcessPage.tsx   # Halaman 5: Proses Analisis & Substitusi Rumus
    │   └── AnalysisResultPage.tsx    # Halaman 6: Laporan Lengkap, Ringkasan, & Unduhan
    ├── services/
    │   ├── sieveAnalysis.ts          # Mesin perhitungan saringan (Metode A & B) & interpolasi log D10/D30/D60
    │   ├── soilClassifier.ts         # Mesin klasifikasi USCS, AASHTO (GI), dan USDA (12 kelas tekstur)
    │   ├── reportGenerator.ts        # Mesin ekspor Laporan Teknis PDF, Ringkasan PDF, CSV, & pemanggil XLSX
    │   ├── xlsxGenerator.ts          # Mesin pembuat file biner .xlsx OpenXML multi-sheet murni
    │   └── __tests__/
    │       └── soilClassifier.test.ts# 12 skenario pengujian otomatis (unit tests)
    ├── styles/
    │   └── index.css                 # Variabel warna kebumian, tipografi, dan pola kontur topografi
    ├── types/
    │   └── soil.ts                   # Definisi tipe data TypeScript
    └── utils/
        ├── systemData.ts             # Metadata sistem, 15 saringan standar, dan preset sampel uji
        └── validation.ts             # Validasi input saringan, batas Atterberg, dan komposisi USDA
```

---

## 4. Instruksi Instalasi & Menjalankan Secara Lokal di Visual Studio Code

### Prasyarat
- **Node.js** versi 18 ke atas (disarankan Node.js LTS v20 atau lebih baru).
- **Visual Studio Code (VS Code)**.

### Langkah-langkah Menjalankan Proyek
1. Buka **Visual Studio Code**.
2. Pilih menu **File -> Open Folder...** lalu pilih folder proyek `GEOSOIL2`.
3. Buka terminal bawaan VS Code dengan menekan **`Ctrl + ~`** (atau menu **Terminal -> New Terminal**).
4. Instal seluruh dependensi proyek:
   ```bash
   npm install
   ```
5. Jalankan server pengembangan lokal:
   ```bash
   npm run dev
   ```
6. Buka alamat yang muncul di terminal pada browser (umumnya **`http://localhost:5173/`**).

### Perintah Tambahan
- **Menjalankan Pengujian Otomatis (*Unit Tests*)**:
  ```bash
  npm test
  ```
- **Membuat Build Produksi (`dist/`)**:
  ```bash
  npm run build
  ```
- **Meninjau Hasil Build Produksi Secara Lokal**:
  ```bash
  npm run preview
  ```

---

## 5. Panduan Mengubah Tampilan & Fungsi Aplikasi di VS Code

Kode GEOSOIL disusun secara modular agar mudah dimodifikasi:

1. **Mengubah Palet Warna, Font, dan Pola Latar Belakang**:
   - Buka [`src/styles/index.css`](src/styles/index.css) untuk mengubah variabel warna kebumian (*Cream*, *Sand Beige*, *Terracotta*, *Deep Earthy Brown*, *Muted Olive Green*) atau mengganti font di [`index.html`](index.html).
2. **Mengubah Ilustrasi Vektor SVG**:
   - Buka [`src/assets/SoilIllustrations.tsx`](src/assets/SoilIllustrations.tsx) untuk mengedit ilustrasi penampang lapisan tanah, tekstur mikroskopis pasir/lanau/lempung, atau ikon peralatan laboratorium.
3. **Mengubah Daftar Saringan Standar atau Preset Sampel Uji**:
   - Buka [`src/utils/systemData.ts`](src/utils/systemData.ts). Anda dapat menambah ukuran saringan default pada `STANDARD_SIEVE_SIZES` atau menambahkan tombol sampel preset baru pada `SAMPLE_PRESETS`.
4. **Mengubah Rumus Analisis Saringan & Interpolasi $D_{10}, D_{30}, D_{60}$**:
   - Buka [`src/services/sieveAnalysis.ts`](src/services/sieveAnalysis.ts). Di sini terdapat fungsi `computeSieveAnalysis` dan `interpolateLogDiameter`.
5. **Mengubah Aturan Klasifikasi USCS, AASHTO, atau USDA**:
   - Buka [`src/services/soilClassifier.ts`](src/services/soilClassifier.ts). Setiap sistem dipisahkan dalam fungsi `classifyUSCS`, `classifyAASHTO`, dan `classifyUSDA`.
6. **Mengubah Tampilan Grafik Interaktif**:
   - Buka [`src/components/SoilCharts.tsx`](src/components/SoilCharts.tsx) untuk memodifikasi Kurva Distribusi Ukuran Butir Semi-Log, Diagram Plastisitas Casagrande, Grafik AASHTO, atau Segitiga Tekstur USDA.
7. **Mengubah Format Laporan PDF, CSV, dan Excel (`.xlsx`)**:
   - Buka [`src/services/reportGenerator.ts`](src/services/reportGenerator.ts) untuk mengatur tata letak PDF (`exportFullTechnicalPDF` dan `exportSummaryPDF`) serta [`src/services/xlsxGenerator.ts`](src/services/xlsxGenerator.ts) untuk mengatur isi lembar kerja Excel.

---

## 6. Panduan Publikasi (*Deployment*) ke Vercel atau Netlify

Proyek ini telah dilengkapi file konfigurasi [`vercel.json`](vercel.json) dan [`netlify.toml`](netlify.toml) serta telah diverifikasi melalui `npm run build` (menghasilkan folder statis `dist/`).

### Opsi 1 — Deploy Gratis Menggunakan Vercel (Rekomendasi)

#### Cara A: Melalui Website Vercel & GitHub (Otomatis Setiap Update)
1. Unggah (*push*) folder proyek `GEOSOIL2` ke repositori **GitHub** Anda.
2. Buka [https://vercel.com](https://vercel.com) dan masuk (*Sign In*) menggunakan akun GitHub.
3. Klik **"Add New..." -> "Project"**, lalu pilih repositori `GEOSOIL2`.
4. Vercel akan otomatis mendeteksi **Framework Preset: Vite**, **Build Command: `npm run build`**, dan **Output Directory: `dist`** dari file `vercel.json`.
5. Klik **"Deploy"**. Dalam ~1 menit, website Anda akan aktif di alamat publik `https://geosoil-anda.vercel.app` dan dapat diakses melalui HP maupun laptop.

#### Cara B: Langsung dari Terminal VS Code (Tanpa GitHub)
1. Buka terminal di VS Code pada folder proyek, lalu jalankan:
   ```bash
   npx vercel
   ```
2. Ikuti petunjuk login singkat di browser saat diminta oleh CLI Vercel, lalu tekan `Enter` untuk menyetujui pengaturan default.
3. Untuk mempublikasikan ke domain produksi utama, jalankan:
   ```bash
   npx vercel --prod
   ```

---

### Opsi 2 — Deploy Gratis Menggunakan Netlify

#### Cara A: Drag & Drop Cepat (Tanpa Akun GitHub / Tanpa CLI)
1. Pastikan Anda sudah menjalankan perintah build di terminal VS Code:
   ```bash
   npm run build
   ```
   Perintah ini akan menghasilkan folder **`dist`** di dalam proyek Anda.
2. Buka [https://app.netlify.com/drop](https://app.netlify.com/drop) dan masuk ke akun Netlify gratis Anda.
3. Tarik dan lepaskan (*drag and drop*) folder **`dist`** langsung ke halaman browser Netlify Drop.
4. Website GEOSOIL akan langsung online dalam hitungan detik dengan subdomain `.netlify.app` gratis yang dapat dibuka di HP maupun laptop.

#### Cara B: Melalui Netlify CLI dari Terminal VS Code
1. Jalankan perintah berikut di terminal VS Code:
   ```bash
   npm run build
   npx netlify-cli deploy --prod --dir=dist
   ```
2. Ikuti tautan otorisasi akun Netlify yang muncul di terminal, dan website Anda akan langsung terpublikasi.
