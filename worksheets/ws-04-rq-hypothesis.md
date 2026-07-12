# WS-04: Research Question & Hypothesis

> **Bab 4 — Research Question, Contribution & Hypothesis**

---

## Ringkasan Materi

### RQ Bukan Pertanyaan Biasa

Research Question yang baik secara implisit mengandung cetak biru eksperimen: subjek, baseline, metrik, domain, dataset.

| Kualitas | Contoh |
|----------|--------|
| **Buruk** | "Bagaimana pengaruh deep learning terhadap deteksi malware?" |
| **Baik** | "Apakah CNN menghasilkan F1-Score lebih tinggi dari RF pada CIC-MalMem-2022?" |

Perbedaan: RQ yang baik menyebutkan **metode spesifik**, **metrik terukur**, **baseline**, dan **dataset**.

### Tiga Jenis RQ

| Jenis | Pola | Kebutuhan |
|-------|------|-----------|
| **Comparison** | A vs B → mana lebih baik? | ≥ 2 metode, metrik sama |
| **Improvement** | A' vs A → modifikasi lebih baik? | Pre/post, bukti perbaikan |
| **Exploratory** | Faktor X₁...Xₙ → pengaruh terhadap Y? | Multi-variabel, korelasi/regresi |

### Contribution Statement

Tiga jenis kontribusi: **Improvement** (metode terbukti lebih baik), **Comparison** (perbandingan sistematis yang belum ada), **Novel Approach** (pendekatan baru). Kontribusi harus terhubung langsung dengan gap — kontribusi tanpa gap = klaim tanpa justifikasi.

### Hypothesis H₀ / H₁

- **H₀** (Null) = Tidak ada perbedaan signifikan — asumsi default, harus dibuktikan salah
- **H₁** (Alternative) = Ada perbedaan signifikan — diterima hanya jika H₀ ditolak
- Harus **falsifiable**, mengandung **metrik terukur**, dirumuskan **SEBELUM eksperimen**

### Rantai Operasionalisasi

```
RQ → Variable → Metric → Data → Analysis
```

Jika rantai ini tidak lengkap, RQ belum mature. Bi-directional: RQ yang tidak bisa jadi hipotesis testable harus direvisi mundur.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan pertanyaan | Apa yang harus dibangun? | Apa yang harus dibuktikan? |
| Bentuk jawaban | Sistem yang berfungsi | Bukti empiris terukur |
| Sukses diukur oleh | User satisfaction, uptime | Signifikansi statistik, effect size |
| Jika gagal | Debug dan perbaiki | Laporkan, analisis mengapa |

### Istilah Penting

- **Research Question (RQ)** — Pertanyaan spesifik: variabel terukur + metrik + konteks
- **Contribution Statement** — Apa yang diketahui setelah riset selesai yang sebelumnya belum ada
- **H₀ / H₁** — Null vs Alternative Hypothesis
- **Falsifiability** — Kondisi hipotesis ditolak harus bisa didefinisikan sebelum eksperimen
- **Operationalization** — Proses mewujudkan konsep abstrak menjadi variabel terukur

---

## Template A.4 — RQ-Contribution-Hypothesis

```
RQ-CONTRIBUTION-HYPOTHESIS

Gap Statement  : ____________________

Research Question:
  Tipe         : [ ] Comparison  [ ] Improvement  [ ] Exploratory
  Formulasi    : ____________________
  Variabel IV  : ____________________
  Variabel DV  : ____________________
  Metrik       : ____________________
  Dataset      : ____________________
  Baseline     : ____________________

Quality Check RQ:
  [ ] Variabel spesifik
  [ ] Metrik jelas
  [ ] Baseline ada
  [ ] Konteks disebutkan
  [ ] Memerlukan eksperimen (bukan hanya survei literatur)

Contribution Statement:
  Apa yang baru diketahui : ____________________
  Jenis kontribusi        : [ ] Improvement  [ ] Comparison  [ ] Novel approach
  Gap yang diisi          : ____________________

Hypothesis Pair:
  H₀ : ____________________
  H₁ : ____________________
  Threshold              : ____________________
  Justifikasi threshold  : ____________________
```

---

## Latihan 1 — Dari Gap ke RQ

Gunakan gap yang ditemukan di WS-03. Transformasikan menjadi Research Question.

**Gap dari WS-03:** Model CNN ringan (lightweight) berhasil memangkas waktu komputasi, namun mengalami penurunan akurasi (F1-score) yang signifikan pada pendeteksian kategori serangan minoritas akibat gagal menangkap dependensi fitur temporal.

**RQ versi pertama (tulis bebas):** Bagaimana cara membuat model CNN yang cepat dan efisien tapi tetap akurat untuk mendeteksi semua jenis serangan jaringan?
> ___________________________________________________

**Evaluasi RQ:**

| Komponen | Ada? | Isi |
|----------|------|-----|
| Metode spesifik | *Ya* | *Optimasi arsitektur CNN Ringan (Lightweight CNN) dengan integrasi modul perhatian temporal (Temporal Attention Mechanism).* |
| Metrik terukur | *Ya* | *inference Latency (kecepatan eksekusi) dan F1-Score (khususnya pada kelas serangan minoritas).* |
| Baseline | *Ya* | *Standard 1D-CNN dan Hybrid CNN-LSTM.* |
| Dataset/konteks | *Ya* | *Dataset lalu lintas jaringan publik (CICIDS2017 atau NSL-KDD).* |

**Tipe RQ:** [ ] Comparison / [v] Improvement / [ ] Exploratory

**RQ versi revisi (setelah evaluasi):**
> Bagaimana performa arsitektur Lightweight CNN yang diintegrasikan dengan Temporal Attention Mechanism dalam menurunkan inference latency sekaligus mempertahankan nilai F1-score pada kelas serangan minoritas jika dibandingkan dengan arsitektur Standard 1D-CNN dan Hybrid CNN-LSTM menggunakan dataset CICIDS2017? 

## Latihan 2 — Hypothesis Pair

Rumuskan pasangan hipotesis dari RQ di Latihan 1.

| Komponen | Isi |
|----------|-----|
| H₀ | *Tidak ada penurunan inference latency yang signifikan dan/atau tidak ada peningkatan nilai F1-score pada kelas serangan minoritas yang dihasilkan oleh model Lightweight CNN + Temporal Attention Mechanism dibandingkan dengan Standard 1D-CNN dan Hybrid CNN-LSTM pada dataset CICIDS2017.* |
| H₁ | *Model Lightweight CNN + Temporal Attention Mechanism menghasilkan penurunan inference latency yang signifikan sekaligus mempertahankan atau meningkatkan nilai F1-score pada kelas serangan minoritas secara signifikan dibandingkan dengan Standard 1D-CNN dan Hybrid CNN-LSTM pada dataset CICIDS2017.* |
| Metrik | *Inference Latency (diukur dalam satuan milidetik per paket data / ms).Macam-macam nilai F1-Score (skala 0.0 - 1.0) spesifik untuk kategori serangan minoritas.* |
| Threshold | *Significance level ($\alpha$) = 0.05 ($5\%$) untuk uji p-value statistik (seperti t-test atau ANOVA).* Batas toleransi penurunan akurasi: Nilai F1-score model usulan tidak boleh turun lebih dari $2\%$ ($\Delta \le 0.02$) dibandingkan model CNN-LSTM (SOTA).* |
| Justifikasi threshold | *Nilai $\alpha = 0.05$ merupakan standar baku (common practice) dalam pengujian hipotesis riset kuantitatif ilmu komputer untuk menolak $H_0$.* Batas toleransi $2\%$ untuk F1-score ditetapkan karena efisiensi kecepatan (latency) yang tinggi dinilai tidak membawa dampak positif yang berarti jika keandalan sistem dalam mendeteksi serangan siber minoritas turun terlalu drastis di lapangan.* |

**Apakah hipotesis ini falsifiable?** [v] Ya / [ ] Tidak
> Bagaimana cara membuktikannya salah? 
 Hipotesis $H_1$ akan terbukti salah jika setelah dilakukan eksperimen dan uji statistik, didapatkan bahwa p-value > 0.05 (menunjukkan tidak ada perbedaan performa yang signifikan), atau jika waktu eksekusi (inference latency) model usulan ternyata lebih lambat daripada Standard 1D-CNN, atau jika nilai F1-score pada serangan minoritas anjlok melampaui batas toleransi $2\%$ dibanding model baseline.
---

## Latihan 3 — Rantai Operasionalisasi

Lengkapi rantai dari RQ hingga metode analisis.

| Tahap | Isi |
|-------|-----|
| RQ | *Bagaimana performa arsitektur Lightweight CNN + Temporal Attention Mechanism dalam menurunkan inference latency sekaligus mempertahankan nilai F1-score pada kelas serangan minoritas jika dibandingkan dengan Standard 1D-CNN dan Hybrid CNN-LSTM menggunakan dataset CICIDS2017?* |
| Variable (IV) | *Jenis arsitektur model deep learning yang diuji (Standard 1D-CNN vs Hybrid CNN-LSTM vs Lightweight CNN + Temporal Attention Mechanism).Jenis arsitektur model deep learning yang diuji (Standard 1D-CNN vs Hybrid CNN-LSTM vs Lightweight CNN + Temporal Attention Mechanism).* |
| Variable (DV) | *Kecepatan eksekusi deteksi (Inference Latency) dan keandalan deteksi klasifikasi anomali (F1-Score).* |
| Metric | *Milidetik per paket data ($ms/packet$) untuk mengukur Inference Latency.* *Nilai indeks skor ($0.0 - 1.0$) untuk metrik F1-Score pada kelas serangan minoritas.* |
| Data source | *Dataset lalu lintas data jaringan publik CICIDS2017 (PCAP logs / berkas ekstraksi fitur CSV).* |
| Analysis method | *Pengujian komparatif empiris kuantitatif yang dilanjutkan dengan uji signifikansi statistik ANOVA (Analysis of Variance) dan Post-Hoc Test ($\alpha = 0.05$) untuk memvalidasi perbedaan performa antar-model.* |

**Apakah rantai lengkap?** [v] Ya / [ ] Tidak
> Jika tidak, tahap mana yang perlu direvisi? - (Sudah lengkap dan selaras dari RQ hingga metode analisis)

---

## Refleksi

> Ambil satu judul skripsi/paper yang pernah dibaca. Coba ekstrak RQ-nya. Apakah RQ tersebut memenuhi semua komponen (metode, metrik, baseline, konteks)? Jika tidak, apa yang hilang?
Bagian ini mengekstrak contoh riil dari salah satu literatur dasar yang kita gunakan di Latihan 1 sebelumnya (Khan et al., 2021) untuk mengevaluasi kelengkapan komponen pertanyaan risetnya:

**Judul:** Efficient Lightweight CNN Architecture for Network Intrusion Detection System
**RQ yang diekstrak:** Bagaimana merancang arsitektur CNN yang ringan (lightweight) untuk mendeteksi intrusi jaringan dengan waktu komputasi yang lebih cepat dan efisien?
**Komponen yang hilang:** RQ asli dalam paper tersebut cenderung langsung berfokus pada tujuan efisiensi tanpa menyebutkan secara eksplisit di dalam kalimat tanyanya mengenai model pembanding (baseline) apa yang akan digunakan serta metrik kuantitatif mutlak apa (seperti F1-score atau Inference Latency) yang dijadikan acuan uji validitasnya.
