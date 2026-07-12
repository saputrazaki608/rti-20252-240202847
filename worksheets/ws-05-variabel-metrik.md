# WS-05: Variabel & Metrik

> **Bab 5 — Metric, Measurement & Data**

---

## Ringkasan Materi

### Measurement Alignment Model

Setiap pengukuran yang valid harus bisa ditelusuri melalui rantai ini tanpa lompatan logis:

```
Problem → Concept → Variable → Metric → Data → Result
```

### Operationalization = Keputusan Desain

Menerjemahkan konsep abstrak menjadi variabel terukur bukan proses mekanis. "Code quality" yang diukur via SonarQube code smells membawa asumsi implisit. Setiap operasionalisasi harus didokumentasikan dan dijustifikasi.

### Empat Tipe Data (NOIR)

| Tipe | Ciri | Contoh | Operasi Valid |
|------|------|--------|---------------|
| **Nominal** | Kategori, tanpa urutan | Jenis algoritma (RF, SVM, CNN) | Modus, chi-square |
| **Ordinal** | Urutan, interval tidak sama | Skala Likert (1-5) | Median, Spearman |
| **Interval** | Jarak bermakna, tanpa nol absolut | Suhu Celsius | Mean, Pearson, t-test |
| **Ratio** | Jarak bermakna + nol absolut | Waktu eksekusi (ms) | Semua operasi |

Tipe data menentukan uji statistik yang valid. Kebanyakan metrik performa TI = ratio; persepsi pengguna = ordinal.

### Kriteria Pemilihan Metrik

- **Representative** — Mewakili konsep yang diteliti
- **Sensitive** — Cukup peka menangkap perbedaan bermakna (hindari ceiling effect)
- **Feasible** — Bisa dikumpulkan dalam batasan waktu dan biaya

### Pre-registration

Metrik harus ditentukan **sebelum** eksperimen. Memilih metrik setelah melihat data = **p-hacking**. Metrik tambahan yang ditemukan kemudian dilaporkan sebagai *exploratory*, bukan *confirmatory*.

### Primary vs Secondary Metric

- **Primary Metric** — Langsung terikat ke hipotesis, menentukan kesimpulan
- **Secondary Metric** — Pendukung, dilaporkan di samping primary; statusnya suplementer

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Pemilihan metrik | Berdasarkan kebiasaan/tool yang ada | Berdasarkan construct validity |
| Anomali | Dihapus untuk laporan bersih | Diinvestigasi — bisa jadi temuan |
| Kapan dipilih | Setelah sistem jadi (monitoring) | Sebelum eksperimen (by design) |

### Istilah Penting

- **Operationalization** — Transformasi konsep abstrak menjadi variabel terukur
- **Construct Validity** — Sejauh mana pengukuran benar-benar mengukur konsep yang dimaksud
- **Measurement Scale** — Klasifikasi data (NOIR) yang menentukan analisis valid
- **Multi-metric Evaluation** — Menggunakan beberapa metrik untuk menangkap konsep kompleks

---

## Template A.5 — Definisi Variabel, Metrik & Justifikasi

```
VARIABLE & METRIC DEFINITION

Research Question: ____________________

| Variabel | Tipe | Konsep | Metrik | Skala | Satuan | Cara Mengukur | Justifikasi |
|----------|------|--------|--------|-------|--------|---------------|-------------|
|          | IV   |        |        |       |        |               |             |
|          | DV   |        |        |       |        |               |             |
|          | CV   |        |        |       |        |               |             |

Alignment Check:
  RQ → Concept → Variable → Metric → Data → Result
  [ ] Setiap langkah terdokumentasi
  [ ] Tidak ada "lompatan logis"
  [ ] Metrik mengukur apa yang dimaksud (construct validity)
```

---

## Latihan 1 — Operationalization Chain

Gunakan RQ dari WS-04. Definisikan variabel dan metriknya.

**RQ:** Bagaimana performa arsitektur Lightweight CNN yang diintegrasikan dengan Temporal Attention Mechanism dalam menurunkan inference latency sekaligus mempertahankan nilai F1-score pada kelas serangan minoritas jika dibandingkan dengan arsitektur Standard 1D-CNN dan Hybrid CNN-LSTM menggunakan dataset CICIDS2017?

| Variabel | Tipe | Konsep Abstrak | Metrik Konkret | Skala (NOIR) | Satuan |
|----------|------|---------------|----------------|-------------|--------|
| *Arsitektur Model* | *IV (Independent)* | *Strategi/struktur variasi desain model deep learning yang diuji.* | *Kategori arsitektur: Usulan (Lightweight CNN + Attention) vs Baseline 1 (Standard 1D-CNN) vs Baseline 2 (CNN-LSTM).* | *Nominal* | *—* |
| *Efisiensi Komputasi* | *DV (Dependent)* | *Kecepatan respons sistem dalam memproses klasifikasi log lalu lintas jaringan.* | *Inference Latency: Rata-rata waktu eksekusi yang dibutuhkan model untuk memprediksi satu paket/baris data.* | *Rasio* | *Milidetik (ms)* |
| *Keandalan Klasifikasi* | *CV (Controlled)* | *Konsistensi infrastruktur eksekusi agar tidak memicu bias performa komputasi.* | *Spesifikasi hardware (CPU, RAM, GPU) yang statis dan penggunaan alokasi thread yang sama selama runtime.* | *Nominal* | *-* |

**Apakah ada lompatan logis dalam rantai?** [ ] Ya / [v] Tidak
> Jika ya, di mana? — (Rantai operasionalisasi sudah runtut karena konsep abstrak efisiensi dan keandalan berhasil dipetakan langsung ke metrik kuantitatif terukur di ranah komputer seperti milidetik dan skor F1).
---

## Latihan 2 — Evaluasi Metrik

Evaluasi metrik DV yang dipilih di Latihan 1 menggunakan 3 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Representative | *5* | *Inference latency mewakili secara langsung realitas beban kerja operasional edge gateway. Metrik ini secara akurat menggambarkan kelayakan model untuk skenario pertahanan jaringan real-time.* |
| Sensitive | *4* | *Cukup sensitif terhadap perubahan struktur lapisan konvolusi atau teknik kompresi model, walaupun fluktuasi minor pada OS background process terkadang bisa sedikit mendistorsi kestabilan pembacaan waktu.* |
| Feasible | *5* | *Sangat mudah dan praktis diukur dengan memanfaatkan pustaka pemrograman standar (seperti modul time atau torch.cuda.Event dalam lingkungan Python).* |

**Apakah perlu secondary metric?** [v] Ya / [ ] Tidak
> Jika ya, apa dan mengapa? 
Perlu menyertakan Jumlah Parameter Model (Size) dan Memory Footprint (RAM/VRAM usage). Alasannya, inference latency murni bisa menipu jika dipengaruhi oleh optimasi hardware-level cache. Metrik sekunder ini memastikan efisiensi model terbukti dari strukturnya yang memang ringkas.

**Contoh kasus ceiling effect untuk metrik ini:**
> Kasus ceiling effect (atau dalam konteks latensi lebih tepatnya floor effect) terjadi jika kapasitas performa perangkat keras uji laboratorium terlalu tinggi (misal menggunakan GPU server papan atas generasi terbaru). Model seberat apa pun akan mengeksekusi data dalam waktu mendekati ~0 ms, sehingga perbedaan efisiensi antar-arsitektur model tidak lagi terlihat kontras akibat dibantu oleh kekuatan perangkat keras ekstrem.
---

## Latihan 3 — Data Quality Check

Bayangkan data yang akan dikumpulkan dari eksperimen. Evaluasi 4 dimensi kualitas data.

| Dimensi | Pertanyaan | Jawaban | Strategi Mitigasi |
|---------|-----------|---------|------------------|
| Completeness | *Apakah semua data point terkumpul?* | *Ya* | *Memastikan skrip pencatatan log (logger) eksperimen mengekstrak latensi dan performa di setiap epoch dan batch pengujian tanpa ada interupsi berkas kosong.* |
| Consistency | *Apakah ada kontradiksi internal?* | *Tidak* | *Mengunci parameter acak menggunakan pembatasan benih acak (fix seed constraints seperti numpy.random.seed) sehingga hasil iterasi data prapemrosesan konsisten di setiap model.* |
| Validity | *Apakah benar-benar mengukur yang dimaksud?* | *Ya* | *Penghitungan waktu inference latency hanya mencakup fase forward pass model (prediksi), tidak mengikutsertakan durasi I/O loading dataset dari memori penyimpanan lokal.* |
| Representativeness | *Apakah sampel mewakili populasi target?* | *Ya* | *Menggunakan subset dataset komprehensif CICIDS2017 yang mencakup lalu lintas normal berbarengan dengan berbagai variasi profil serangan siber modern secara proporsional.* |

---

## Refleksi

> Mengapa memilih metrik setelah melihat data dianggap p-hacking? Apa bedanya dengan eksplorasi data yang sah?

**Jawaban:**
> P-Hacking (Manipulasi Metrik): Dianggap sebagai pelanggaran etika ilmiah karena peneliti sengaja berburu dan memilih metrik tertentu hanya setelah melihat data eksperimen keluar, tujuannya semata-mata mencari celah metrik mana yang menghasilkan nilai signifikansi bagus (p-value < 0.05) agar hipotesisnya terlihat berhasil. Ini memicu bias konfirmasi yang fatal.

> Eksplorasi Data yang Sah (Exploratory Data Analysis): Dilakukan di fase awal penelitian sebelum pengujian hipotesis final dirumuskan. Tujuannya adalah memahami distribusi data asli, memeriksa pencilan (outliers), dan mencari pola awal untuk membangun hipotesis serta metrik evaluasi yang adil, bukan untuk mencocok-cocokkan hasil eksperimen secara paksa demi kelulusan pengujian.
