# WS-01: Distorsi & Paradigma

> **Bab 1 — Research Mindset in IT**

---

## Ringkasan Materi

### Research Trust Model

Pengetahuan ilmiah tidak muncul langsung dari kenyataan. Ia melewati **6 tahap transformasi** yang masing-masing rawan distorsi:

```
Reality → Data → Processing → Analysis → Inference → Knowledge
```

Etika mencegah distorsi yang disengaja (fabrikasi, cherry-picking). Validitas mendeteksi distorsi yang tidak disengaja (confounding variable, sampling bias).

### Tiga Jenis Validitas

| Jenis | Pertanyaan | Contoh Ancaman |
|-------|-----------|----------------|
| **Internal Validity** | Apakah hubungan kausal benar ada? | Confounding variable |
| **External Validity** | Apakah bisa digeneralisasi? | Dataset terlalu homogen |
| **Construct Validity** | Apakah mengukur hal yang benar? | Metrik tidak sesuai klaim |

### Paradigma Riset

Mata kuliah ini menggunakan pendekatan **Positivist** (fenomena TI bisa diukur objektif melalui eksperimen terkontrol) diperkuat **Design Science Research** (DSR). Penting untuk membedakan keduanya:

| Paradigma | Cara Kerja | Contoh di TI |
|-----------|-----------|---------------|
| **Positivis** | Uji hipotesis dengan eksperimen terkontrol | Apakah CNN lebih akurat dari RF pada dataset X? |
| **Design Science Research** | Bangun artefak (sistem/model/framework) untuk menguji proposisi | Dapatkah arsitektur hybrid CNN+LSTM membuktikan peningkatan recall ≥5%? |
| **Interpretivis** | Pahami makna melalui konteks & kualitatif | Bagaimana peneliti manafsirkan anomali data sensor IoT? |

Dalam DSR, artefak **bukan tujuan akhir** — ia adalah instrumen untuk menghasilkan pengetahuan. Pertanyaan riset tetap harus difalsifikasi.

### Mode Berpikir Peneliti

**Curious** (mempertanyakan fenomena) → **Critical** (mengevaluasi klaim berdasarkan bukti) → **Systematic** (merancang investigasi terstruktur dan reproducible).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Membuat sistem yang bekerja | Menghasilkan pengetahuan yang valid |
| Pertanyaan khas | "Bagaimana membuatnya jalan?" | "Apakah klaim ini benar?" |
| Ukuran sukses | Sistem berfungsi, client puas | Hipotesis terjawab, temuan tervalidasi |
| Kegagalan | Harus dihindari | Harus dilaporkan (negative result = kontribusi) |

### Istilah Penting

- **Research Mindset** — Pola pikir yang menuntut bukti dan mempertanyakan asumsi
- **Research Ethics** — Prinsip perilaku: kejujuran, objektivitas, keterbukaan, akuntabilitas
- **HARKing** — Hypothesizing After Results are Known — merumuskan hipotesis setelah melihat data
- **Falsifiability** — Hipotesis harus bisa dibuktikan salah

---

## Template A.1 — Research Mindset Self-Assessment

```
Nama Peneliti    : ____________________
Tanggal          : ____________________

1. Ketika membaca klaim "metode X 95% akurat":
   - Pertanyaan pertama saya: ____________________
   - Data yang dibutuhkan untuk verifikasi: ____________________

2. Posisi paradigma:
   - Pendekatan: [ ] Positivis  [ ] Interpretivis  [ ] Design Science  [ ] Mixed
   - Alasan: ____________________

3. Identifikasi distorsi:
   - Asumsi tersembunyi: ____________________
   - Sumber bias potensial: ____________________
   - Langkah mitigasi: ____________________

4. Komitmen etika:
   - Data yang tidak akan dimanipulasi: ____________________
   - Batasan yang diakui sejak awal: ____________________
```

---

## Latihan 1 — Identifikasi Distorsi

Pilih satu paper riset di bidang TI yang mengklaim "metode X meningkatkan performa." Telusuri setiap tahap Research Trust Model.

> **Panduan pencarian paper:** Gunakan [IEEE Xplore](https://ieeexplore.ieee.org), [ACM Digital Library](https://dl.acm.org), atau Google Scholar. Pilih paper **tahun 2020 ke atas**, di topik yang Anda minati: deteksi anomali, klasifikasi citra, NLP, keamanan siber, IoT, dsb.
>
> **Contoh domain TI:** "Deteksi anomali lalu-lintas jaringan menggunakan CNN — akurasi meningkat 94% vs baseline SVM 87%." Distorsi potensial: apakah dataset normal/anomali seimbang? Apakah hanya diuji pada satu vendor traffic?

**Paper yang dipilih:**
> Judul: Deep Learning for Network Intrusion Detection: An Efficient CNN Model for Anomaly Detection
> Penulis (Tahun): M. A. Khan, mdan kawan-kawan (2021)
> Sumber/Link DOI: https://doi.org/10.1109/ACCESS.2021.3050412 (IEEE Xplore / IEEE Access)

| Tahap | Apa yang Dilakukan | Potensi Distorsi |
|-------|-------------------|-----------------|
| Reality → Data |*Menggunakan dataset publik standar industri seperti NSL-KDD atau CICIDS2017 untuk mensimulasikan lalu lintas jaringan realitas.*| *Distorsi Representasi: Dataset publik sering kali bersifat usang (pola serangan lama) dan tidak sepenuhnya mencerminkan lalu lintas jaringan dunia nyata yang dinamis serta terus berubah.*|
| Data → Processing |*Melakukan min-max normalization, menangani missing values, dan menggunakan teknik One-Hot Encoding untuk mengubah fitur kategorikal menjadi numerik.*|*Distorsi Informasi: Prapemrosesan yang terlalu agresif (seperti membuang outlier ekstrem secara sepihak) dapat menghilangkan karakteristik krusial dari serangan siber tipe baru (zero-day attack).*|
| Processing → Analysis |*Melatih model Convolutional Neural Network (CNN) dan membandingkan akurasinya terhadap algoritma tradisional seperti Support Vector Machine (SVM) dan Random Forest.*|*Distorsi Evaluasi: Perbandingan bisa pincang (biased) jika arsitektur SVM baseline tidak dioptimalkan parameter hper-nya (hyperparameter tuning) secara adil, sehingga membuat model CNN terkesan jauh lebih unggul.*|
| Analysis → Inference |*Mengklaim keberhasilan model karena mencapai akurasi 98,5% pada data pengujian (test set).*|*Distorsi Generalisasi (Overfitting): Akurasi yang sangat tinggi kerap terjadi karena data leakage (kebocoran data) atau karakteristik data training dan testing yang terlalu mirip, membuat model gagal ketika diuji pada vendor traffic yang berbeda.*|
| Inference → Knowledge |*Menarik kesimpulan bahwa metode CNN ini siap diimplementasikan sebagai sistem pertahanan utama pada Intrusion Detection System (IDS) industri.*|*Distorsi Kontekstual: Mengabaikan faktor real-time throughput dan latensi komputasi deep learning yang tinggi, yang pada realitasnya bisa membuat traffic jaringan menjadi bottleneck.*|

**Distorsi paling besar di tahap:** *Analysis --> Inference*

**Dua distorsi spesifik yang teridentifikasi:**
1. *Ketidakseimbangan Kelas (Class Imbalance Bias): Klaim akurasi tinggi (98%) sering kali mengecoh jika jumlah data normal jauh mendominasi data serangan (imbalanced dataset), sehingga model yang mendeteksi semua data sebagai 'normal' pun akan tetap mendapat skor akurasi tinggi.*
2. *Keterbatasan Pengujian Vendor (Environment Lock-in): Model hanya diuji pada satu repositori dataset statis, sehingga keandalannya di lingkungan operasional infrastruktur jaringan yang sesungguhnya belum terbukti secara empiris.*

---

## Latihan 2 — Analisis Kasus Etika

Skenario: Seorang peneliti menemukan bahwa jika 3 data point outlier dihapus, hasil eksperimennya menjadi signifikan. Dengan outlier, hasilnya tidak signifikan.

| Perspektif | Analisis |
|------------|---------|
| Kejujuran ilmiah | *Peneliti wajib melaporkan performa model dalam dua kondisi tersebut. Menyembunyikan outlier hanya agar model deep learning terlihat "superior" atau "paling akurat" merupakan bentuk kecurangan ilmiah (falsification). Dalam ranah siber, 3 data outlier itu bisa jadi adalah serangan krusial yang gagal dideteksi.* |
| Transparansi |*Peneliti harus secara eksplisit menjelaskan kriteria pengidentifikasian outlier tersebut pada bab metodologi. Harus disebutkan metode statistik yang digunakan (misalnya Isolation Forest atau Z-score) serta menyediakan akses ke source code prapemrosesan data (misalnya file Jupyter Notebook atau skrip Python) di repositori publik seperti GitHub agar alur eliminasi data dapat diaudit.*|
| Peer review |*Melalui transparansi data, para mitra bestari (reviewers) dapat mengevaluasi apakah penghapusan 3 data tersebut valid secara teknis keamanan jaringan atau sekadar manipulasi (p-hacking). Hal ini memastikan bahwa kesimpulan yang ditarik oleh komunitas ilmiah benar-benar objektif dan tidak menyesatkan peneliti lain yang ingin mereplikasi model tersebut.*|

**Keputusan akhir dan justifikasi:**
> Keputusan Akhir:
*Tetap mempertahankan ketiga data outlier tersebut di dalam dataset utama dan melaporkan performa riil model apa adanya. Jika outlier terpaksa harus dihapus, penghapusan tersebut wajib didasari oleh alasan kesalahan teknis yang klir (seperti corrupted packet log atau kegagalan sistem pencatatan sensor), dan kedua hasil eksperimen (sebelum dan sesudah penghapusan) harus ditampilkan berdampingan di dalam paper.*
> Justifikasi:
*Berdasarkan pedoman etika publikasi ilmiah internasional dari COPE (Committee on Publication Ethics) serta standar pelaporan eksperimen IEEE, manipulasi sampel data demi memuluskan klaim performa metode baru adalah pelanggaran serius terhadap integritas akademik. Terlebih dalam domain keamanan siber, mengabaikan data pencilan secara sengaja sangat berbahaya karena outlier dalam lalu lintas jaringan sering kali merepresentasikan pola serangan baru (zero-day attacks) atau anomali kritis yang justru merupakan target utama dari sistem pertahanan jaringan itu sendiri.*
---

## Latihan 3 — Posisi Paradigma

**Topik riset:** *Deteksi Anomali Lalu Lintas Jaringan Menggunakan Deep Learning (CNN) untuk Mitigasi Serangan Siber*

> **Skala 1–5:** 1 = tidak sesuai sama sekali dengan topik ini, 5 = sangat sesuai dan dominan digunakan pada riset bertopik serupa.

| Kriteria | Positivis | Interpretivis | Design Science |
|----------|-----------|---------------|----------------|
| Kesesuaian dengan topik (1–5) | *4 — Sangat sesuai karena riset memerlukan pengujian hipotesis objektif dan komparasi statistik performa antar-algoritma.* | *1 — Tidak sesuai sama sekali karena fokus riset ini bukan menggali makna sosial, pengalaman kultural, atau konteks subjektif manusia.* | *5 — Sangat sesuai dan dominan karena riset komputer ini bertujuan utama membangun sebuah artefak teknologi baru (model arsitektur CNN).* |
| Jenis data yang dikumpulkan | *Metrik numerik kuantitatif seperti tingkat akurasi, Precision, Recall, F1-Score, False Alarm Rate, dan log waktu latensi eksperimen.* | *Rekaman wawancara atau transkrip persepsi tim Network Security Administrator mengenai ancaman siber.* | *Hasil pengujian fungsionalitas artefak, arsitektur layer CNN, serta tabel komparasi kinerja model usulan terhadap model baseline.* |
| Limitasi paradigma |*Cenderung mereduksi masalah keamanan jaringan yang dinamis hanya ke dalam angka statis, serta mengabaikan aspek humanis di lapangan.*|*Hasil analisis bersifat lokal/spesifik dan tidak dapat digeneralisasikan untuk mendeteksi variasi serangan berskala global.*|*Evaluasi performa sering kali terbatas pada lingkungan laboratorium (controlled environment) dan belum menjamin skalabilitas saat diterapkan langsung pada jaringan industri nyata.*|

**Paradigma yang dipilih:** *Design Science Research (DSR) yang didukung oleh pendekatan Positivis.*
**Alasan:** *Riset ini berfokus pada penyelesaian masalah praktis di dunia nyata keamanan siber dengan merancang dan menguji sebuah artefak komputasi baru (model CNN). Penilaian keberhasilan dari artefak tersebut kemudian diukur secara empiris dan terukur menggunakan pembuktian kuantitatif numerik (positivis) untuk menunjukkan keunggulannya secara valid.*
---

## Refleksi

> Sebelum membaca materi ini, apakah pernah mempertanyakan klaim "95% akurat"? Setelah memahami rantai distorsi, pertanyaan apa yang sekarang akan diajukan saat membaca paper?

**Jawaban:*Sebelumnya, klaim metrik performa tinggi seperti "95% akurat" sering kali diterima secara mentah-mentah sebagai bukti mutlak kesuksesan sebuah sistem. Namun, setelah memahami rantai distorsi, pertanyaan kritis yang kini wajib diajukan saat membaca paper deteksi anomali adalah:*
> *Aspek Representasi Data: Apakah dataset yang digunakan untuk melatih model tersebut seimbang? Jika data serangan hanya berjumlah 5% dari total keseluruhan data jaringan, maka model yang menebak semua lalu lintas sebagai 'normal' secara keliru pun akan tetap menghasilkan klaim "Akurasi 95%".*
> *Aspek Generalisasi Lingkungan: Apakah model tersebut diuji pada variasi traffic dari vendor/perangkat keras yang berbeda, atau performa tinggi itu hanya terjadi karena overfitting dan data leakage pada satu dataset spesifik saja?*
