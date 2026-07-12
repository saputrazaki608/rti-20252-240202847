# WS-10: Experiment Execution & Data Collection

> **Bab 10 — Eksekusi Eksperimen & Pengumpulan Data**

---

## Ringkasan Materi

### Experiment Execution Pipeline

```
Design → Execution Plan → Controlled Execution → Data Collection → Data Logging → Dataset for Analysis
```

### Multiple Run = Non-Negotiable

Single run **tidak pernah cukup** untuk klaim ilmiah. Minimum 5-10 run per skenario dengan seed berbeda. Multiple run menghasilkan:
- Mean, std, confidence interval
- Distribusi hasil → uji statistik
- Variabilitas → error bar di grafik

### Execution Plan

Setiap eksperimen harus memiliki plan sebelum eksekusi:
- Daftar skenario
- Jumlah run per skenario
- Random seed per run (pre-determined!)
- Urutan eksekusi (randomisasi/counterbalancing)
- Pre-execution checklist

### Data Logging Komprehensif

Setiap run menghasilkan log terstruktur:
1. **Identitas** — Run ID, timestamp, skenario
2. **Konfigurasi** — Semua parameter, seed, code version
3. **Hasil** — Semua metrik, output detail
4. **Metadata** — Waktu eksekusi, resource usage, warning/error

Format: CSV/JSON/database — **bukan stdout yang di-copy-paste**.

### Engineering vs Research Execution

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Run | Sekali (deploy) | Multiple (min 5-10, seed berbeda) |
| Logging | Error log, access log | Semua parameter, metrik, metadata |
| Anomali | Bug → fix → redeploy | Investigasi → dokumentasi → analisis |
| Urutan | Tidak penting | Bisa bias — perlu randomisasi |

### Anomali = Dokumentasi, Bukan Hapus

Run gagal/anomali tidak boleh dihapus tanpa dokumentasi. Bisa jadi:
- **Bug** → fix & re-run (dokumentasikan!)
- **Batas kemampuan metode** → DNF = temuan
- **Data yang bias** jika hanya simpan run "berhasil"

### Jebakan Kognitif

1. "Satu angka cukup" → tanpa distribusi, tidak bisa diuji
2. "Seed tidak penting" → bahkan algoritma deterministik bisa dipengaruhi library stokastik
3. "Run gagal langsung hapus" → kehilangan temuan potensial
4. "Semua run harus hari ini" → thermal throttling, fatigue

---

## Template A.10 — Execution Plan & Data Log

```
EXECUTION PLAN

| Run # | Skenario | Seed | Parameter | Status | Waktu | Output File |
|-------|----------|------|-----------|--------|-------|-------------|
| 1     |          |      |           |        |       |             |
| 2     |          |      |           |        |       |             |
| 3     |          |      |           |        |       |             |
| ...   |          |      |           |        |       |             |

Jumlah runs per skenario : ____
Total runs               : ____

DATA LOG (per run):
  Run ID    : ____________________
  Timestamp : ____________________
  Skenario  : ____________________
  Input     : ____________________
  Output    : ____________________
  Anomali   : ____________________
  Catatan   : ____________________
```

---

## Latihan 1 — Execution Plan

Susun execution plan untuk eksperimen Anda. Tentukan skenario, jumlah run, dan seed sebelum eksekusi.

| Run # | Skenario | Seed | Parameter Kunci | Status |
|-------|----------|------|----------------|--------|
| *1* | *Baseline: Standard 1D-CNN, CICIDS2017* | *42* | *$lr=0.001, batch=256, epoch=50$* | *Planned* |
| *2* | *Baseline: Hybrid CNN-LSTM, CICIDS2017* | *42* | *$lr=0.001, batch=256, epoch=50$* | *Planned* |
| 3 | *Proposed: Lightweight CNN + Attention, DS-1* | *42* | *$lr=0.001, batch=256, epoch=50$* | *Planned* |
| 4 | *Proposed: Lightweight CNN + Attention, DS-1* | *123* | *$lr=0.001, batch=256, epoch=50$* | *Planned* |
| 5 | *Proposed: Lightweight CNN + Attention, DS-1* | *999* | *$lr=0.001, batch=256, epoch=50$* | *Planned* |

**Total skenario:** 3 (Standard 1D-CNN, Hybrid CNN-LSTM, dan Lightweight CNN+Attention)
**Run per skenario:** Bervariasi (1 run untuk masing-masing baseline sebagai perbandingan acuan, dan 3 run dengan variasi seed berbeda untuk skenario model usulan guna menguji stabilitas)
**Total run keseluruhan:** 5

---

## Latihan 2 — Data Log Terstruktur

Desain format data log untuk eksperimen Anda. Tentukan field apa saja yang akan dicatat.

**Identitas:**
| Field | Contoh |
|-------|--------|
| Run ID | *run-lcnn-att-003* |
| Timestamp | *2026-07-05T09:40:00* |
| | |

**Konfigurasi:**
| Field | Contoh |
|-------|--------|
| Seed | *42* |
| Code version | *commit b10c7a8ef9* |
| Model Architecture | *Lightweight_CNN_Attention* |
| Learning Rate | *0.001* |
**Hasil:**
| Metrik | Tipe Data | Range Valid |
|--------|----------|-------------|
| *Inference Latency* | *float* | *$0.1 - 10.0$ (ms per paket)* |
| *F1-Score Minoritas* | *float* | *$0.0 - 1.0$* |
| *Total Trainable Params* | *int* | *>0* |

**Format output:** [ ] CSV / [v] JSON / [ ] Database / [ ] Lainnya: ____

---

## Latihan 3 — Anomaly Protocol

Rencanakan bagaimana menangani anomali. Untuk setiap jenis, tentukan langkah yang diambil.

| Jenis Anomali | Contoh | Tindakan |
|---------------|--------|----------|
| Run gagal (crash) | *CUDA out of memory saat memproses evaluasi batch besar.* | *Dokumentasikan sisa memori, perkecil batch_size evaluasi ke 128 (pertahankan ukuran batch latihan), jalankan ulang (re-run), dan tandai perubahan konfigurasi di catatan log.* |
| Hasil ekstrem | *Nilai F1-score minoritas jatuh ke angka 0.00 tiba-tiba.* | *Hentikan alur eksekusi, periksa fungsi kalkulasi bobot loss (Class Weights), investigasi apakah terjadi vanishing gradient, simpan grafik loss trajectory, lalu dokumentasikan fenomena tersebut sebelum mengatur ulang inisialisasi bobot.* |
| Waktu eksekusi anomali | *Inference latency melonjak drastis hingga > 15 ms pada iterasi tertentu.* | *Periksa beban utilisasi CPU/GPU host. Jika ada proses latar belakang sistem operasi (background OS update atau throttling), bersihkan memori perangkat keras, dinginkan komponen, lalu ulangi run pengujian latensi secara terisolasi.* |
| Inkonsistensi dengan run lain | *Skor F1 menyimpang > 0.15 antar seed yang berbeda.* | *Investigasi distribusi sampel di fungsi pencacah data (Data Loader). Pastikan Stratified K-Fold atau pembagian data berjalan konstan di setiap variasi seed. Dokumentasikan varians performa tersebut.* |

**Prinsip:** Detect → Investigate → Document → Decide

---

## Refleksi

> Pernahkah Anda melaporkan hasil riset/tugas dari single run? Apa risikonya? Bagaimana multiple run mengubah kepercayaan terhadap hasil?

**Pengalaman sebelumnya:**
> Ya, pada tugas-tugas kuliah pemrograman atau eksperimen awal, saya sering kali terburu-buru langsung melaporkan hasil akurasi model hanya dari single run pengujian.
> Risiko single run: Hasil tersebut sangat rentan terhadap bias keberuntungan (cherry-picking). Bisa jadi model mendapatkan skor tinggi hanya karena inisialisasi bobot acak (lucky seed) sedang berada pada kondisi optimal untuk data uji tersebut, sementara performa aslinya di lapangan sangat tidak stabil atau lambat.
**Yang akan dilakukan berbeda:**
> Menerapkan Kunci Eksplisit Variasi Seed: Saya tidak akan lagi mengandalkan satu kali eksekusi acak. Saya akan menguji model usulan menggunakan minimal 3 hingga 5 benih acak (random seed) berbeda yang ditentukan di awal untuk melihat konsistensi performa aslinya.
> Mencatat Varians dan Deviasi Standar: Alih-alih hanya menyajikan satu angka performa terbaik, saya akan melaporkan performa model dalam bentuk nilai rata-rata (mean) disertai deviasi standar ( standard deviation) untuk metrik F1-score dan inference latency.
> Melakukan Isolasi Perangkat Keras saat Uji Latensi: Untuk pengujian metrik fisik seperti kecepatan inferensi (latency), saya akan memastikan seluruh proses latar belakang (background processes) dinonaktifkan secara ketat agar fluktuasi data murni dipengaruhi oleh efisiensi arsitektur Lightweight CNN, bukan karena intervensi beban komputasi luar.
> Menggunakan Format Log Terstruktur Otomatis: Saya akan mengintegrasikan skrip pengecekan otomatis yang langsung mengekspor seluruh konfigurasi hyperparameter dan hasil metrik ke dalam satu berkas berkode JSON setiap kali run selesai, guna menghindari kesalahan pencatatan manual (human error).
